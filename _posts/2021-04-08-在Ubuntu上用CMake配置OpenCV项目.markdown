---
layout: post
title:  "在Ubuntu上用CMake配置OpenCV项目"
date:   2021-04-08 22:19:01 +0100
categories: OpenCV
---

在Ubuntu上用CMake配置OpenCV项目需要用到**VScode**以及这个IDE的插件。

## 下载并安装Visual Studio Code
在VS Code官网[下载程序包](https://code.visualstudio.com/Download),注意下载的是**deb**文件，然后在所在文件夹中运行：
```
$sudo dpkg -i code_1.26.0-1534177765_amd64.deb
```

安装好后，创建一个文件夹用来存放源码:
```
$cd Documents
$mkdir opencvtest& cd opencvtest
$touch main.cpp
```
将源代码复制到`main.cpp`中：

```cpp
#include <opencv2/core.hpp>
#include <opencv2/imgcodecs.hpp>
#include <opencv2/highgui.hpp>
#include <iostream>
using namespace cv;
int main()
{
    std::string image_path = samples::findFile("starry_night.jpg");
    Mat img = imread(image_path, IMREAD_COLOR);
    if (img.empty())
    {
        std::cout << "Could not read the image: " << image_path << std::endl;
        return 1;
    }
    imshow("Display window", img);
    int k = waitKey(0); // Wait for a keystroke in the window
    if (k == 's')
    {
        imwrite("starry_night.png", img);
    }
    return 0;
}
```
## 配置CMakeLists.txt并安装插件

首先安装**CMake Tools**插件，然后在`main.cpp`文件夹中编写`CmakeLists.txt`文件：

```
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(opencvtest)

find_package(OpenCV REQUIRED)    // 这里使用命名查找OpenCV库
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIRS})
message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

add_executable(opencvtest main.cpp)
target_link_libraries(opencvtest "${OpenCV_LIBS}")
set_property(TARGET opencvtest PROPERTY CXX_STANDARD 11)
```

然后调用命令台工具(Ctrl+Shift+P),选择运行`Cmake config`,运行没有报错，则点击IDE最下方的**build**按钮，就会进行编译。

编译成功后CMake会在**opencvtest**文件夹中创建一个**build**文件夹，在其中就有编译好的可执行文件`opencvtest`,通过命令行运行即可。
