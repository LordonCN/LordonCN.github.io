---
layout: post
title: "google开源RNN网络PlaNet。"
subtitle: 'planet相关'
author: "xudongdong"
header-img: img/jassica/jessica-jung-celebrity.jpg
catalog: true
tags:
  - PlaNet
  - RNN LSTM GRU
  - Tensorflow
  - Cross Entropy Error Func
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

>为了学习准确的潜在动力学模型，google团队引入了：

`循环状态空间模型`：具有确定性和随机性成分的潜在动力学模型，允许根据稳健的计划预测各种可能的未来，同时记住多个时间步骤的信息。实验表明，这两个组件对于高规划性能至关重要。<br>
`潜在的超调目标`：将潜动力模型的标准训练目标推广到训练多步预测，通过在潜空间中加强一步预测和多步预测的一致性。这产生了一个快速和有效的目标，提高了长期预测，并与任何潜在序列模型兼容。
总结：
虽然预测未来图像允许教授模型，但`编码和解码`图像（上图中的梯形）需要大量计算，这会减慢计划。然而，在紧凑的潜在状态空间中的规划是快速的，因为我们仅需要预测未来的奖励而不是图像来评估动作序列。


例如:智能体可以想象球的位置及其与目标的距离将如何针对某些动作而改变，而不必使场景可视化。
这允许在智能体每次选择动作时能够比较1万个想象的`动作序列`(之后的好多步动作，这里是12步)和并对回报大小进行计算，最后执行找到的`最佳序列`的`第一个`动作，执行结束后并对之后的动作`重新进行计划`。

# 3、解决ubuntu运行不友好的问题
实验室的师兄们使用`Mobaxterm`远程连接服务器之后文件移动以及pycharm调试特别方便，那么ubuntu是不是就特别不方便了？问题不大。<br>
期初因为命令行run报错太多，pygame运行缺少video报错的问题，就把问题归咎于命令行的问题...于是搞了个软件来模拟win链接服务器，恩，是这个[`Asbru`](https://www.asbru-cm.net/)，  相对不太友好的是文件的传输还是需要`scp`指令，效果见下图：<br>
<img src="/img/190916post/asbru.png" >

不过问题还好是解决了，命令行报video的错，asbru报audio的错[stackoverflow解决办法 添加pygame的初始化](https://stackoverflow.com/questions/15933493/pygame-error-no-available-video-device/53623914)。
asbru客户端下问题确实解决了，出于好奇便想彻底解决命令行下运行报错的问题，嘿嘿，试了试把文件里面不调用音频，不过只有同时打开asbru的情况下才能正常运行...下面这个图看起来还是可以的吧。
<img src="/img/190916post/success.png" >

### 4、代码中关于使用GPU的 RuntimeError: to is not supported on TracedModules
问题很简单 就小小贴一下
[pytorch报错 使用不当报错 不知道源代码怎么错误写成了net.to(device=args.device)](https://discuss.pytorch.org/t/cannot-move-scriptmodule-to-gpu-with-to/35939)

# 5、系统地学习Tensorflow
在开始学习之前，最好对机器学习有一个大致的了解。相比于市面上的很多书籍，吴恩达的网课应该是最好不过的了，特别是CNN网络的讲解非常透彻，当初看了那么多博客回过头来看他的[Slides](https://docs.google.com/presentation/d/15E7NlyMkG8dAMa70i2OluprBDoz3UPyAk5ZpOiCkEqw/edit)才发现好多坑都填平了。
>[吴恩达机器学习](http://cs231n.github.io/)

斯坦福大学推出的CS20对于我个人来说是非常合适的，首先从基本的Tensorflow语句用法入手，大致对“图”有了解，进而讲解了`CNN`、`GANS`([这里推荐B站7月在线的网课](https://www.bilibili.com/video/av38768354/?spm_id_from=333.788.videocard.13))、`VAE`([自动化所黄怀波](https://www.bilibili.com/video/av37380954?from=search&seid=11989753959255513148))、`RNN`，课后作业是填空类型实操。
>[Stanford-Tensorflow教程与实现](http://web.stanford.edu/class/cs20si/syllabus.html)

# 6、十一结束 RNN深入学习
十一的前四天过得很充实，看了两天网课跟pdf仔细打了下基础知识，临开学前刷知乎发现了[`YJango`](https://www.zhihu.com/people/YJango/activities)大佬之前发过的RNN、LSTM详解，抓紧[码住](https://zhuanlan.zhihu.com/p/25518711)！

> RNN与LSTM关系:

如果把`RNN`比作是手机屏幕，而`LSTM-RNN`与`GRU`则可以说是手机膜。<br>
在多次迭代过程中，大量非线性累积历史信息会造成梯度消失(梯度爆炸)，就像是手机裸机使用时间长了把屏幕刮花。
而`LSTM`将信息的积累建立在线性自连接的memory cell之上，并靠其作为中间物来计算,好比是用手机屏幕膜作为中间物来观察手机屏幕;
输入门、遗忘门、输出门的过滤作用好比是手机屏幕膜的反射率、吸收率、透射率三种性质
> LSTM公式：

<img src="/img/190916post/agrizme.png" >
经典rnn过程：
<img src="/img/190916post/LSTM3-SimpleRNN.png" >
LSTM：
<img src="/img/190916post/LSTM3-chain-add-handwriting-1.png" >
添加了一下标注 上面公式中的几个变量标出来：
<img src="/img/190916post/tuidao.jpg" >
根据公式推导一遍确实理解的更加深刻，大佬666

# 6、再进一步 交叉熵损失函数(CEEF)的计算
计算分类问题的时候使用交叉熵算法能够快速有效地得到较为准确的判断条件，[文章·](https://zhuanlan.zhihu.com/p/35709485)从常见的分类误差与均方误差计算结果来进行比较，通过已知概率对所需分类事件进行对数计算求和后得到预测的结果。至于计算方法与具体细节文章中已经做过了详细的介绍。<br>
> 交叉熵算法计算的损失经过描点画图后得到了一个类似于反比例的凸函数,所以理论上存在最优解：

<img src="/img/190916post/CEEF-loss.jpg" >
注意：文章中所提到的log均以10为底。