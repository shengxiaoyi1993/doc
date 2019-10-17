# Qt动态库编译与使用

### VS可以调用Qt工具链为msvc编译的库
- VS中下载相应版本的Qt vs  tool， 然后设置相应Qt版本
- 编译Qt项目与MinGW有出入。用MinGW能正常编译的项目用MSVC会报错
```
//若有 `__declspec(dllexport)`,`__declspec(dllexport)`则会有.lib文件
//动态调用不成功
```


### VS使用Makefile进行编译 
```
//打开VS命令行
cd build
mkdir build
cd build
cmake ../ -G "NMake Makefiles"
nmake
```

### 使用Qt msvc工具链进行编译
- Qt上设置msvc的工具链

```
//若有 `__declspec(dllexport)`,`__declspec(dllexport)`则会有.lib文件
//动态调用不成功
```


### VS加载动态库

#### 若有.lib文件，可采用隐式调用的方法

```
//path set

当需要使用别人的库时，需添加.h,.lib,.dll文件到工程中。可以有两种方法：a）将.h,.lib,.dll都放在工程目录下（），需要在使用dll的main函数中加载上头文件，再链接lib（“链接器”->“输入”->-“附加依赖项”），就可直接使用；b）单独建立include,bin,lib文件夹，然后将它们链接上；这样如果要添加的库比较多时，会更加整齐可读，推荐采用这种方法。下面介绍这种方法的实现步骤：

1）建立include文件夹，里面存放.h文件，然后右击工程项目，“属性”->“C/C++”->“常规”->“附加包含目录”，添加.h路径即可（为了防止工程迁移，建议添加相对路径）。

2）建立lib文件夹，里面存放.lib文件，然后右击工程项目，“属性”->“链接器”->“常规”->“附加库目录”，添加.lib路径即可（为了防止工程迁移，建议添加相对路径）。

右击工程项目，“属性”->“链接器”->“输入”->“附加依赖项”，添加对应的.lib文件名称。

3）建立bin文件夹 ，里面存放.dll文件，然后右击工程项目，“属性”->“配置属性”->“调试”->“工作目录”，添加.dll路径即可（为了防止工程迁移，建议添加相对路径）。

```


#### 若只有头文件和.DLL则需采用动态调用
- Qt下可以用隐式调用

```
//example
#include <iostream>
#include"windows.h"
#include"windef.h"

typedef void (WINAPI CLIENTAPI_INIT)(void);
typedef int (WINAPI CLIENTAPI_GETSERVERTIMECODE)(char* , unsigned int* );
typedef void (WINAPI CLIENTAPI_CLEANUP)(void);


int main()
{
	CLIENTAPI_INIT *clientapi_init;
	CLIENTAPI_GETSERVERTIMECODE* clientapi_getservertimecode;
	CLIENTAPI_CLEANUP* clientapi_cleanup;

	HINSTANCE hDLLDrv_api_client = LoadLibraryEx(TEXT("api_client.dll"),
		NULL, LOAD_WITH_ALTERED_SEARCH_PATH);
	if (hDLLDrv_api_client) {
		std::cout << "hDLLDrv_api_client:succeed to load lib" << std::endl;
	}
	else {
		std::cout << "hDLLDrv_api_client:fail to load lib" << std::endl;

	}

	DWORD nError = GetLastError();
	std::cout << "nError:" << nError << std::endl;


		if (hDLLDrv_api_client)
	{
			clientapi_init = (CLIENTAPI_INIT*)GetProcAddress(hDLLDrv_api_client, "capi_init");
			clientapi_getservertimecode =(CLIENTAPI_GETSERVERTIMECODE*)GetProcAddress(hDLLDrv_api_client, "capi_GetServerTimeCode");
			clientapi_cleanup = (CLIENTAPI_CLEANUP*)GetProcAddress(hDLLDrv_api_client, "capi_disabled");
			
			clientapi_init();
			unsigned int timecode;
			char ip_tmp[] = "192.168.1.188";
			char* ip = ip_tmp;
			int flag = clientapi_getservertimecode(ip,&timecode);
			if (flag == 0) {
				std::cout << "timecode:" << timecode << std::endl;

			}
			clientapi_cleanup();

	}


	if (hDLLDrv_api_client)
		FreeLibrary(hDLLDrv_api_client);
	else {
		std::cout << "fail to load lib" << std::endl;
	}
	

}

```