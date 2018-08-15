# makefile
## 参考
- https://www.cnblogs.com/wang_yb/p/3990952.html
## 目的
- 组织大型项目的编译，整理依赖关系及操作
## make 的工作方式
1. 读入主Makefile (主Makefile中可以引用其他Makefile)
2. 读入被include的其他Makefile
3. 初始化文件中的变量
4. 推导隐晦规则, 并分析所有规则
5. 为所有的目标文件创建依赖关系链
6. 根据依赖关系, 决定哪些目标要重新生成
7. 执行生成命令
## 组成
- 显示规则 :: 说明如何生成一个或多个目标文件(包括 生成的文件, 文件的依赖文件, 生成的命令)
- 隐晦规则 :: make的自动推导功能所执行的规则
- 变量定义 :: Makefile中定义的变量
- 文件指示 :: Makefile中引用其他Makefile; 指定Makefile中有效部分; 定义一个多行命令
- 注释     :: Makefile只有行注释 "#", 如果要使用或者输出"#"字符, 需要进行转义, "\#"

## 规则
### 基本语法
```cpp
target ... : prerequisites ...（; command）
    command
    ...
    ...
```
>target        - 目标文件, 可以是 Object File, 也可以是可执行文件
 prerequisites - 生成 target 所需要的文件或者目标
 command       - make需要执行的命令 (任意的shell命令), Makefile中的命令必须以 [tab] 开头

### 通配符
- *  :: 表示任意一个或多个字符
- ?     :: 表示任意一个字符
- [...] :: ex. [abcd] 表示a,b,c,d中任意一个字符, [^abcd]表示除a,b,c,d以外的字符, [0-9]表示 0~9中- 任意一个数字
- ~     :: 表示用户的home目录

### 路径搜索
- VPATH ：, 如果当前目录中没有找到相应文件或依赖的文件, Makefile 回到 VPATH 指定的路径中再去查找
用法
```cpp
vpath <directories>            // 当前目录中找不到文件时, 就从<directories>中搜索
vpath <pattern> <directories>  // 符合<pattern>格式的文件, 就从<directories>中搜索
vpath <pattern>                // 清除符合<pattern>格式的文件搜索路径
vpath                          // 清除所有已经设置好的文件路径
```
> <directories>间用‘:’分隔
> <pattern>如‘%.h‘

## 范例
```cpp
　　　　helloworld : main.o print.o #helloword 就是我们要生成的目标
　　　　　　　　　　　　　　　　　# main.o print.o是生成此目标的先决条件
　　　　　　gcc -o helloworld main.o print.o#shell命令，最前面的一定是一个tab键

　　　　mian.o : mian.c print.h
　　　　　　gcc -c main.c
　　　　print.o : print.c print.h
　　　　　　gcc -c print.c
　　　　
　　　　clean :　　　　　　　　　　
　　　　　　　　rm helloworld main.o print.o
```