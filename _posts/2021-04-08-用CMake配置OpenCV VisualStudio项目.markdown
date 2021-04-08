---
layout: post
title:  "通过CMake配置OpenCV Visual Studio项目"
date:   2021-04-08 10:30:01 +0100
categories: OpenCV
---
OpenCV 在Visual Studio中的配置需要在项目属性中多次手动添加openCV的库等，每次建一个新的
OpenCV项目都要重新添加一次，非常麻烦。

通过CMake就可以一次性配置好CMake文件，每次创建新的OpenCV项目时，使用同一个CMakeLists就
好啦，非常简便。

### 第一步：下载CMake

在CMake官网[下载](https://cmake.org/download/)，记得勾选“加入PATH”。

### 第二步：配置CMake

配置CMake前首先创建一个文件夹用来存放你的源码，比如`OpenCVCmake\main.cpp`,然后在这个
文件夹中创建CMakeLists.txt用来配置CMake，具体目录如下：
```
OpenCVCmake:
|-main.cpp
|-CMakeLists.txt
```
CMakeLists.txt中将项目名称，所需包含的库等设置好：
```
cmake_minimum_required(VERSION 3.10)

#设定项目名称
project(OpencvCmakeVS2019)

#设定路径变量
set(opencv_root "D:/OpenCV4/opencv")
set(opencv_include ${opencv_root}/build/include)
set(opencv_lib_dir ${opencv_root}/build/x64/vc15/lib)
set(opencv_libs opencv_world451d.lib)

#设定include
include_directories(${opencv_include})
#设定lib位置
link_directories(${opencv_lib_dir})
#设定引用的lib名称
link_libraries(${opencv_libs})

#设定输出名称"OpencvTest"
add_executable(OpencvTest main.cpp)
```

这样配置好后，我们在`OpenCVCmake`文件夹下创建一个`build`文件夹，结构如下：
```
OpenCVCmake:
|-main.cpp
|-CMakeLists.txt
|-build
```
`build`文件夹是用来存放生成好的项目文件的。

### 第三步：CMake配置和生成
这部是最后一步，先通过命令行进入`build`文件夹：
```
cd build
```
然后运行`cmake ..`。`cmake .`是指当前目录，但我们写好的`CMakeLists`在上层目录中，所以
用`cmake ../`或`cmake ..`。

运行完后在`build`文件夹中就可以看到**.sln**文件了，打开它，并将**OpencvTest**设置为“Set as Startup project”。

恭喜你，学会了用CMake配置OpenCV项目！
