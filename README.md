# CMake构建执行程序

- 只需要记住四条
- cmake_minimum_required(VERSION 2.6)
- project(testeventengine)
- SET(CMAKE_INSTALL_PREFIX ./JSTrader_HFT_Bin)
- INSTALL(TARGETS testeventengine DESTINATION "testbin")

第一行必须有的，第二行名字随便起，三行是安装总目录位置，四行是安装子目录


```
cmake_minimum_required(VERSION 2.6)
project(testeventengine)
add_definitions(-std=c++11)
set(test_source main.cpp)
add_executable(testeventengine ${test_source})   #这行第一个才是文件名
INSTALL(TARGETS testeventengine DESTINATION "testbin")
```
# CMake构建链接库

```
cmake_minimum_required(VERSION 2.6)
project(JSTrader)
add_definitions(-std=c++11)
include_directories(../../include/safecontainer)
include_directories(../../include/jsstructs)

set(eventengine_source ../../include/jsstructs/eventengine.h eventengine.cpp ../../include/jsstructs/jsstruct.hpp ../../include/jsstructs/jsmacrodef.hpp)
add_library(eventengine SHARED ${eventengine_source})#这行第一个才是文件名
INSTALL(TARGETS eventengine DESTINATION "lib")
```
# CMake构建执行文件和库文件的步骤和区别
都要引用源文件，set(xxx_source main.cpp 各种cpp)。
不同点 执行文件是add_executabe(文件名，${xxx_source})
链接库是 add_library(文件名 SHARED或者填STATIC ${xxx_source})

# 特殊编译项

1. 引用头文件搜索目录

```
include_directories(../../include/safecontainer)
```
2. 引用库文件搜索目录


```
link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib)
```

3. 链接库文件方式

```
target_link_libraries(ctpgateway thostmduserapi thosttraderapi)
```

4. 使用宏定义

```
add_definitions(-std=c++11)
```

5. 编译附加选项

```
set(CMAKE_CXX_FLAG "-Wall")
```

6. 链接附加选项

```
1.执行程序的链接
set(CMAKE_EXE_LINKER_FLAGS "-Wl,-rpath=../lib")
2.动态链接库的链接
set(CMAKE_SKIP_BUILD_RPATH TRUE)
# rpath来自../lib或者是.
set(CMAKE_SHARED_LINKER_FLAGS "-Wl,-rpath,./:../lib")
```
7. 拷贝三方库文件

```
INSTALL(FILES ${CMAKE_SOURCE_DIR}/lib/libthostmduserapi.so DESTINATION "lib")
INSTALL(FILES ${CMAKE_SOURCE_DIR}/lib/libthosttraderapi.so DESTINATION "lib")
```
8. 添加子工程和打开编译信息显示

```
set(CMAKE_VERBOSE_MAKEFILE ON)
add_subdirectory(src/eventengine)
```
# 一大项目底下一个链接库，一个可执行文件，可执行文件不需要知道链接库的目录，直接可以自动编译，自己能搜索到。CMake还是很智能的，只要外部一个大的CMakeLists.txt 同时包含两个子工程就行了
