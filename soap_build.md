# soap测试程序 及项目建立过程图解
## 环境: ubuntu ,编译的支持ssl的gsoap
## 参考博客 
https://blog.csdn.net/dgyanyong/article/details/39925523
## 项目文件列表（完成编译的完整项目）
```cpp
sxy@sxy-pc:~/project/test_soap_add$ ls
add.add.req.xml  add.h        addserver.o  Makefile         soapClient.o soapServerLib.c  stdsoap2.h
add.add.res.xml  add.nsmap    addtest      soapC.c          soapC.o       soapServer.o     stdsoap2.o
addclient.c      addserver    addtest.c    soapClient.c     soapH.h       soapStub.h
addclient.o      addserver.c  addtest.o    soapClientLib.c  soapServer.c  stdsoap2.c
```
## 项目建立过程
- 1. add.h
```cpp
//add.h 文件
//gsoapopt cw  
//gsoap ns2 schema namespace: urn:add  
//gsoap ns2 schema form: unqualified  
//gsoap ns2 service name: add  
//gsoap ns2 service type: addPortType  
//gsoap ns2 service port:http://websrv.cs.fsu.edu/~engelen/addserver.cgi  
//gsoap ns2 service namespace: urn:add  
//gsoap ns2 service transport: http://schemas.xmlsoap.org/soap/http  
//gsoap ns2  service method-style:      add rpc  
//gsoap ns2  service method-encoding:   add http://schemas.xmlsoap.org/soap/encoding/  
//gsoap ns2  service method-action:     add ""  
int ns2__add( int num1, int num2, int* sum );  
```
- 2. 执行命令行
```
soapcpp2 -c add.h
```
执行后文件目录多了许多文件
```cpp
sxy@sxy-pc:~/project/test_soap$ ls
add.add.req.xml  add.h      soapC.c       soapClientLib.c  soapServer.c     soapStub.h
add.add.res.xml  add.nsmap  soapClient.c  soapH.h          soapServerLib.c
```
- 3. addserver.c

```cpp
#include "soapH.h"  
#include "add.nsmap"  
int main(int argc, char **argv)  
{  
    int m, s;  
    struct soap add_soap;  
    soap_init(&add_soap);  
    soap_set_namespaces(&add_soap, namespaces);  
  
    if (argc < 2) {  
        printf("usage: %s <server_port> \n", argv[0]);  
        exit(1);  
    } else {  
        m = soap_bind(&add_soap, NULL, atoi(argv[1]), 100);  
        if (m < 0) {  
            soap_print_fault(&add_soap, stderr);  
            exit(-1);  
        }  
        fprintf(stderr, "Socket connection successful: master socket = %d\n", m);  
        for (;;) {  
            s = soap_accept(&add_soap);  
            if (s < 0) {  
                soap_print_fault(&add_soap, stderr);  
                exit(-1);  
            }  
            fprintf(stderr, "Socket connection successful: slave socket = %d\n", s);  
            soap_serve(&add_soap);  
            soap_end(&add_soap);  
        }  
    }  
    return 0;  
}  
#if 1  
int ns2__add(struct soap *add_soap, int num1, int num2, int *sum)  
{  
    *sum = num1 + num2;  
    return 0;  
}  
#endif
```
- 4. addclient.c

```cpp

#include "soapStub.h"  
#include "add.nsmap"  
  
int add(const char *server, int num1, int num2, int *sum)  
{  
    struct soap add_soap;  
    int result = 0;  
    soap_init(&add_soap);  
    soap_set_namespaces(&add_soap, namespaces);  
    soap_call_ns2__add(&add_soap, server, NULL, num1, num2, sum);  
    printf("server is %s, num1 is %d, num2 is %d/n", server, num1, num2);  
  
    if (add_soap.error) {  
        printf("soap error: %d, %s, %s\n", add_soap.error, *soap_faultcode(&add_soap), *soap_faultstring(&add_soap));  
        result = add_soap.error;  
    }  
    soap_end(&add_soap);  
    soap_done(&add_soap);  
    return result;  
}  
```
- 5. addtest.c

```cpp
#include <stdio.h>  
#include <stdlib.h>  
#include <string.h>  
  
int add(const char *server, int num1, int num2, int *sum);  
int main(int argc, char **argv)  
{  
    int result = -1;  
    char server[128] = {0};  
    int num1;  
    int num2;  
    int sum;  
  
    if (argc < 4) {  
        printf("usage: %s <ip:port> num1 num2 \n", argv[0]);  
        exit(1);  
    }  
  
    strcpy(server,argv[1]);  
    num1 = atoi(argv[2]);  
    num2 = atoi(argv[3]);  
    result = add(server, num1, num2,&sum);  
  
    if (result != 0) {  
        printf("soap error, errcode=%d\n", result);  
    } else {  
        printf("%d + %d = %d\n", num1, num2, sum);  
    }  
    return 0;  
}  
```
- 6. Makefile
注意将makefile中的gsoap目录更改为自己方的位置
将gsoap-2.8\gsoap目录下的stdsoap2.c和stdsoap2.h复制到项目文件夹下
执行make指令时，可能会出现错误，把$(CC)...行前的空格删除，再使用TAB就能顺利编译
```cpp
GSOAP_ROOT = /root/onvif/gsoap-2.8/gsoap  
CC = gcc -g -DWITH_NONAMESPACES  
INCLUDE = -I$(GSOAP_ROOT)  
SERVER_OBJS = soapC.o stdsoap2.o soapServer.o addserver.o   
CLIENT_OBJS = soapC.o stdsoap2.o soapClient.o addclient.o addtest.o  
  
all: server  
server: $(SERVER_OBJS)   
    $(CC) $(INCLUDE) -o addserver $(SERVER_OBJS)   
  
client: $(CLIENT_OBJS)   
    $(CC) $(INCLUDE) -o addtest $(CLIENT_OBJS)  
  
clean:  
    rm -f *.o addtest 
```
- 7. make
分别编译服务器和客户端
```cpp
make server
```
```cpp
make client
```
- 8. 测试程序
在一个窗口输入
```cpp
./addserver 1234
```
在另一个窗口输入,注意网址需要根据自己的电脑进行设置
```cpp
./addtest 192.168.0.101:1234 12 43
```
测试结果
服务器
```cpp
sxy@sxy-pc:~/project/test_soap$ ./addserver 1234
Socket connection successful: master socket = 3
Socket connection successful: slave socket = 4
```
客户端
```cpp
sxy@sxy-pc:~/project/test_soap$ ./addtest 192.168.0.101:1234 12 43
server is 192.168.0.101:1234, num1 is 12, num2 is 43/n12 + 43 = 55
```

