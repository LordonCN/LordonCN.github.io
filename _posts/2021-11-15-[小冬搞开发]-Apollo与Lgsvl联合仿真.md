---
layout: post
title: "[小冬搞开发]-Apollo与Lgvsl联合仿真"
subtitle: '小冬搞科研 为工作做准备'
author: "Lordon"
header-img: img/jassica/jessica-jung-celebrity.jpg
catalog: true
tags:
  - [小冬搞开发]
---

## 项目运行环境

硬件环境：8核Intel(R) Core(TM) i7-6700K CPU @ 4.00GHz处理器，单卡NVIDIA GTX 1060显卡

操作系统：ubuntu 18.04

Apollo: v6.0.0

lgsvl: svlsimulator-linux64-2021.3

官方相关文档：https://www.svlsimulator.com/docs/

相关配置视频：https://www.bilibili.com/video/av414824262/


# 起因

大噶伙应该都知道，现在随着apollo生态做的越来越好，不少公司开始直接使用开源apollo的程序来进行商业开发.这个趋势确实也不错，像物流车现在发展的如火如荼
可真是多亏了Apollo程序的开源(各个大厂都是哦~).软件方面实现了基本框架的搭建以及开箱即跑的自由之后，硬件小20w的成本也就不在话下了。但是对于我这种习惯
了白嫖的应届差学生来说这么好的仿真环境如果只是Dreamview跑跑routing、planning可就太可惜了，没有硬件就玩不了了吗？


> Lgsvl仿真
`
王方浩：这里主要介绍下Lgsvl这款仿真器，它是由LG电子美国研发中心基于Unity开发的适用于自动驾驶开发者的多机器人模拟器。可以直接和Autoware和Apollo进行对接，
同时还提供生成高精度地图。只需要做很小的集成就可以用来测试和验证整个自动驾驶系统。
`

说实话想折腾这两个联调真是一时兴起，网上的多是apollo装虚拟机后win装lgsvl联调，困难在于ip通信；鉴于实验室台式机性能还可以我就直接都在ubuntu上开搞了。
一开始apollo版本是branch:master,调试了一下午愣是无法实现完全的routing、planning以及control的联合调试效果，无奈之下只好考虑降低apollo版本(master->v5.0.0)


# 配置过程及采坑


1. apollo正常安装通过，可进行dreamview可视化等操作。
2. LGSVL仿真器的安装下载网址为：https://www.lgsvlsimulator.com/ 下载linux版本(svlsimulator-linux64-2021.3)
3. 解压后启动 ./SVLSimulator即可打开软件，此处需要注册账号等操作。注册完成后需要添加Simulations任务，如图所示选择车辆与地图信息

<p align="center">
<img src="/img/211115image/lgsvl0.png" >
</p>

4. 点击启动仿真即可在SVLSimulator中出现仿真画面,首先会自动下载地图信息.
5. 进入apollo dock容器下，执行下列命令：

```bash
	bash scripts/bootstrap_lgsvl.sh
	bash scripts/bridge.sh  //执行完毕后保持卡住，没有任何输出状态 完成与仿真器的连接
```
6. 启动  http://localhost:8888/ DV界面，选择地图，车辆，切换到Module Controller上，打开定位localization模块,这里可以参考B站视频.

<p align="center">
<img src="/img/211115image/lgsvl1.png" >
</p>


7. 点击SVLSimulator左下角启动即可使用方向盘进行控制。F12复位     w s拉伸视角


8. 目前来看已经能够获取到车辆底盘与预测数据，正好欠缺 决策规划跟控制 (狗头)
```
  ├── /apollo/canbus/chassis
  |
  ├── /apollo/sensor/gnss系列定位数据
  |
  ├── /apollo/perception/obstacles
  |
  ├── /apollo/perception/traffic_light
  .
  . 包含lidar126以及radar等数据
  .
```

# tips：

<1> routing线出现之后遇到planning等模块报红、车辆不动没现象的时候要先点Reset All，再点Setup，甚至多次，等一会儿出规划线之后就能自己跑了.

<2> 整个过程不用开Simcontrol，直接给routing点即可


# QA环节

Q:能够订阅apollo/control/数据 ，但是得想办法发出去，让这个仿真软件订阅到.
- A:`Modole controller` 这个选项中启动control模块即可.


Q:目前存在问题：/apollo/sensor/lidar128/compensator/PointCloud2话题接收不到数据. 
- A:Lgsvl中6.0的林肯模型还没有加入Lidar等传感器，只能从话题拿到`/apollo/perception/obstacles` 数据直接来用.


Q:LGSVL里面林肯车型怎么选择？？
- A:(1) Apollo5.0(full analysis)这辆车有Radar跟Lidar传感器，使用的时候效果跟B站视频是一样的，适合感知预测的同学。(2) Apollo6.0(modular testing)这辆车使用的3D Ground Truth，省去了感知并带有预测，适合直接搞决策规划控制的同学。
