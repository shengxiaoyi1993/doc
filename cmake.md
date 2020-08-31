#  cmake
## 目的
## 执行过程
```cpp
//配置编译选项，一般不变，出现错误时需要配置
$> ccmake directory
//根据CMakeLists.txt生成Makefile文件
$> cmake directory
//执行Makefile文件，编译程序，生成可执行文件
$> make
```
>directory为CMakeList.txt所在目录
##  规则