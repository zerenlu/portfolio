---
layout: post
title:  "在Ubuntu上安装OpenCV"
date:   2021-04-08 14:30:01 +0100
categories: OpenCV
---
在Ubuntu上可以直接通过源码安装OpenCV,简洁方便

**安装python版的OpenCV相当简单：**

### 更新系统后直接安装
```
$sudo apt update
$sudo apt install libopencv-dev python3-opencv
```

### 验证安装结果
```
python3 -c "import cv2; print(cv2.__version__)"
```

**安装C++版的opencv需要下载源码并在本地编译**

### 安装配置工具和依赖项

```
$sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev libopenexr-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
```

### 从git库中克隆最新的opencv源码

```
$mkdir ~/opencv_build && cd ~/opencv_build
$git clone https://github.com/opencv/opencv.git
$git clone https://github.com/opencv/opencv_contrib.git
```

如果需要以前版本的opencv，则使用`git checkout <opencv-version>`而不是`git clone`。

### 创建build文件夹并配置CMake

```
$cd ~/opencv_build/opencv
$mkdir -p build && cd build
```

### 使用CMake配置OpenCV:

```
$cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..
```

### 编译

```
$make -j16
```
根据处理器的核数来配置`j`后面的数字，处理器的核数可以通过`nproc`来获得

### 安装opencv
在**build**文件夹中运行：
```
$sudo make install
```

### 验证安装结果

```
$pkg-config --modversion opencv4
```
