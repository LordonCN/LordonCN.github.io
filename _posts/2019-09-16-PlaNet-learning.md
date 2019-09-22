---
layout: post
title: "google开源的深度规划网络，仅从图像输入中学习世界模型，并成功利用它进行规划。"
subtitle: 'planet相关'
author: "xudongdong"
header-img: img/jassica/jessica-jung-celebrity.jpg
catalog: true
tags:
  - PlaNet
---

# 0、前言
> 将学习过程中遇到的知识点做一个总结归纳。
- ctrl+shift+v 本地预览.md文件   唉... 本来想复制点东西..偶然发现了这么个组合快捷键。

# 1、服务器命令行运行之诡怪
很有意思，下午跟师姐了解了一下课题内容具体细节，因为暑假实习掌握了一些基础内容，现在学可能多少入门会快一点，晚上ssh服务器之后顺利找到程序所在位置，run demo应该是最爽的了吧<br>
还行，上来就报错pygame找不到了，我：？？？
```
$pip3 install pygame --user
$... success ...
$python3
Python 3.6.8 (default, Jan 14 2019, 11:02:34)巴拉巴拉
>> import pygame
wrong!
```
`$python3` 这一条指令软连接到系统安装默认的python版本，反复查验后pygame安装给了python3.7版本？？查看了一下/usr/bin/python* 并没有找到python3.7 这时候奇怪了，从清华源下载怎么就这样了...期间尝试了改默认python版本的软链接、官网下载pygame包本地安装、甚至删除python3.7版本，均无效..

```
jiafan@TJDL4:~/uct$ python3.7                                        Python 3.7.2 (default, Mar  2 2019, 10:56:35)[GCC 5.4.0 20160609] on linux Type "help", "copyright", "credits" or "license" for more information.    
>>> import pygame                                                    pygame 1.9.6 Hello from the pygame community. https://www.pygame.org/>>>                  
```
所以突发奇想直接指定python版本即可...  

# 2、初探流程
忙完手头的活补充了基本知识之后开始尝试大致了解代码。
> 大致流程如下所示
```
1、train_episode和test_episode目录先各收集到５个episode， /planet/control/random_episodes.py def random_episodes() |初始化def _initial_collection() | /planet/training/utility.py  def collect_initial_episodes()中调用
2、然后开始５万步，每一步从train_episode取数据，      	 /planet/scripts/configs.py ----> def _training_schedule(config, params):
3、计算loss。						 /planet/training/define_model.py ---->   train_loss = tf.cond()
4、去求gradient，再planning出一个episode。
5、5万步后是100步的test phase，会从test_episode得数据 	/planet/scripts/configs.py ----> def _training_schedule(config, params):
6、不求loss 不求gradient，                           	/planet/training/define_model.py ---->   train_loss = tf.cond()
7、之后planning出一个episode.	/planet/training/test_running.py   --->   with tf.variable_scope('simulation')
```