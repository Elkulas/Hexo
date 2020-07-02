---
title: 多版本OpenCV安装与共存
date: 2020-07-01 15:14:55
tags:
description: 安装多版本OpenCV并且多版本共存
---

# Ubuntu多版本OpenCV共存与切换

## 前言

这里主要是参考[这个博客](https://blog.csdn.net/learning_tortosie/article/details/80594399)，为了防止博客挂掉所以这边自己再备份一个。

## 多版本OpenCV共存

假设我们已经安装好一版OpenCV，一般都安装在`/usr/local`下。
如果需要安装另一个版本的OpenCV，就不能再安装到`/usr/local`，而是选择其他路径，否则会覆盖掉之前的版本。

### 下载OpenCV

首先去https://opencv.org/releases.html下载所需版本的Sources版，也可去https://github.com/opencv/opencv/tree/3.4.1下载。
假设我们安装的第二个OpenCV版本为3.4.1。

### 安装依赖包

```
[compiler] sudo apt-get install build-essential
[required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
[optional] sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```

### 编译安装OpenCV

详见官方文档https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html，参考步骤如下。

```
$ cd opencv-3.4.1
$ mkdir build
$ cd build
$ mkdir installed
$ cmake \
-DCMAKE_BUILD_TYPE=RELEASE \
-DCMAKE_INSTALL_PREFIX=~/opencv-3.4.1/build/installed \
\
-DWITH_CUDA=OFF \
\
-DBUILD_DOCS=OFF \
-DBUILD_EXAMPLES=OFF \
-DBUILD_TESTS=OFF \
-DBUILD_PERF_TESTS=OFF \ 
..
$ make -j4
$ sudo make install
```

#### NOTICE

1. 其中`~/opencv-3.4.1/build/installed`为安装OpenCV3.4.1的路径，这个十分关键。
2. 设置OFF的理由如下，可大大加快编译速度，当然还要根据需求进行设置。

It is useful also to unset BUILD_EXAMPLES, BUILD_TESTS, BUILD_PERF_TESTS - as they all will be statically linked with OpenCV and can take a lot of memory.
此外，还可以取消设置BUILD_EXAMPLES，BUILD_TESTS和BUILD_PERF_TESTS，因为它们都将与OpenCV静态链接，并且会占用大量内存。

## 多版本OpenCV切换

打开`~/.bashrc` 或者我自己用的zsh，就是打开`~/.zshrc`

然后在文件末尾添加以下内容

```
export PKG_CONFIG_PATH=～/opencv-3.4.1/build/installed/lib/pkgconfig
export LD_LIBRARY_PATH=～/opencv-3.4.1/build/installed/lib
```

更新配置文件

```
$ source ~/.zshrc 
```

查询OpenCV版本

```
$ pkg-config --modversion opencv
```

如果输出`3.4.1`，就表明配置成功。
如果想使用之前的版本，在`~/.bashrc` or`~/.zshrc`  中注释掉增加的内容，然后`source ~/.bashrc` or `source ~/.zshrc`  即可。

### CMakeLists的更改

如果只有一个版本的OpenCV，在CMakeList.txt中使用以下语句即可。

```
FIND_PACKAGE(OpenCV REQUIRED)
```

在OpenCV编译好后，所在目录中会生成OpenCVConfig.cmake文件，这个文件中指定了CMake要去哪里找OpenCV，其.h文件在哪里等。
存在多版本OpenCV时，需要找到所需版本对应的OpenCVConfig.cmake文件，并将其路径添加到工程的CMakeLists.txt中。

```
cmake_minimum_required(VERSION 2.8)  
set(OpenCV_DIR "~/opencv-3.4.1/build")   
project(test)  
find_package(OpenCV REQUIRED) 
```

