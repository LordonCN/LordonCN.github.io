---
layout: post
title: "Homework-ODB_SLAM"
subtitle: '附加题 '
author: "Lordon"
header-img: img/jassica/jessica-jung-celebrity.jpg
catalog: true
tags:
  - Probabilistic Robotics
  - ODB_SLAM
---
# 0、关于
使用摄像头或视频运行 ORB-SLAM2,记录填坑以及实现过程.

<img src="/img/191124image/succeed.png" >


# 1、安装Pangolin (耗时1h)
> "对于 pangolin(一个 GUI 库),你需要下载并安装它,它同样是个 cmake 工程"
这个东西安装没那么难,根据书上网上教程cmake.. 之后 make是一直报错的,`cmake --build .`解决

```
$ git clone https://github.com/stevenlovegrove/Pangolin.git
$ cd Pangolin
$ mkdir build
$ cd build
$ cmake ..
$ cmake --build .
```

# 2、ORB_SLAM2编译 (耗时2h)
- 1.注意安装`libsuitesparse-dev-qt4`需要指定版本,在ORB_SLAM2编译过程中出现了无数warning跟`error`,不过error(unsleep)都是因为缺少`#include <unistd.h>`头文件,添加过后编译通过

```
$ sudo apt-get install libopencv-dev libeigen3-dev libqt4-dev qt4-qmake libqglviewer-dev libsuitesparse-dev-qt4 libcxsparse3.1.2 libcholmod-dev

$ git clone https://github.com/raulmur/ORB_SLAM2.git ORB_SLAM2
$ cd ORB_SLAM2
$ chmod +x build.sh
$ ./build.sh
```

- 2.移植高翔作业中所给程序myslam.cpp以及标定myslam.yaml,首先需要将上述两个文件放到个demo程序里.此处我放到`../ORB_SLAM2-master/Examples/Monocular/myslam.cpp`(当然是因为Monocular为单目相机)并且在`../ORB_SLAM2-master/CMakeLists.txt`中添加:

```
add_executable(myslam
Examples/Monocular/myslam.cpp)
target_link_libraries(myslam ${PROJECT_NAME})
```

- 3.更改myslam.cpp中变量路径`string parameterFile` 以及 `string vocFile`.
- 4.重新编译
<img src="/img/191124image/bianyi.png" >
- 5.运行
<img src="/img/191124image/run.png" >
```
dong@Inspiron-7447:../ORB_SLAM2-master$ ./myslam Vocabulary/ORBvoc.txt Examples/Monocular/myslam.yaml
```

# 结语
跟着课程来做,督促自己一下.

# 挖坑
12月源码解析: