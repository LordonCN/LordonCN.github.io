---
layout: post
title: "Kalman_filter"
subtitle: 'Kalman_filter '
author: "Lordon"
header-img: img/jassica/jessica-jung.jpg
catalog: true
tags:
  - Algorithm
  - Kalman_filter
---
# 0、关于
> 最大后验推导和贝叶斯推导之间选一个.

WSY之前问我当初卡尔曼C语言的实现问题,据说网传基本都是错误的,所以翻了翻书顺便对比了下代码,这里总结一下.

# 1、对于 <概率机器人> 中提到的 Algorithm-Kalman_filter
卡尔曼滤波器是用于从`间接`和`不确定测量`估计系统状态的最佳估计算法.卡尔曼滤波器仅针对线性系统定义。如果有非线性系统并且想要估计系统状态，则需要使用非线性状态估计器。
书中31页首先列出了`线性高斯状态转移`和对`测量值的KF算法`,公式中参数很多,在日常线性问题使 `A` `B` `C` `I` 四个参数默认取1即可.有关基础知识看一下知乎[傻瓜也能看懂的卡尔曼](https://zhuanlan.zhihu.com/p/64539108)或者简书[卡尔曼C代码实现](https://www.jianshu.com/p/e9f3c8eba689)<br>
<img src="/img/191122image/1-1.png" >

# 2、思考
之前调车的时候常常会说注意看滤波后曲线对滤波前曲线的跟随性和准确性,仔细思考一下,这里所说的跟随性该怎么认为呢?<br>
- 卡尔曼可是由计算速度快并且效果优良著称的好算法,按照我之前的理解就是`传感器采集数据Z(t)`发生突变的时候`滤波后结果∑(t)与μ(t)`应该紧跟着发生变化,而滤除部分则是数值变化尖锐点.由此看来如果公式用的对,在一般线性问题计算过程中,滤波后数值的跟随性在响应速度上不会存在问题,跟随哪一个对象一起发生变化则是重点,所以跟随是指是否是跟对了我们想要滤波的数值对象,当然这个变量传对了就没啥问题.<br>

那么准确性又怎么说?如果我想滤传感器传回来的采集值,是不是最后结果显示滤波后与滤波前所画出来的曲线近似完全重合才是最理想的?<br>
<!-- 就像下面这样(我只是看这个效果太好了,没别的意思)<br><img src="/img/191122image/2-1lvbo.jpg" > -->
- 卡尔曼算法中提到的`Q-过程误差`、`R-测量误差`作为实际情况中很关键部分,使得`真实值`既`不等于``测量值`,也不会等于`预测值`.课本中给出的例证也说明了这一点.<br>
<img src="/img/191122image/2-2kalman_example.jpg" ><br>

卡尔曼增益怎么起作用?
- Kt分子越大,先验预测的`方差`越大,那么说明先验值准确定很低,置信度则`不高`.那么计算求解`最优估计值`的时候`加大测量值Zt置信度`.

> 概率机器人学是摒弃了可能出现的情况的单一的"最好预测",转而使用概率的算法来表示`模糊性`和`置信度`.    <br>
      
我深以为然.<br>

# 3、C语言实现
下面是伴随十二十三十四届智能车走过的简单一阶卡尔曼函数实现,也见证了两个国一的诞生.
```
float Kalman_Filter(float AccZ_Angle,float Gyropitch)
{
  //以下基本无需再调
  static float x=0;          //最优角度初值
  static float p=0.000001;   //最优角度对应协方差初值
  static float Q=0.000001;   //过程噪声
  static float R=0.35;       //测量噪声
  static float k=0;          //kalman 系数
  
  x = x + Gyropitch*0.00348f;
  p = p + Q;
  k = p / (p+R);
  x = x + k*(AccZ_Angle-x);
  p = (1-k)*p;
  return x;
}
```
针对MPU6050传感器,通过对读进来的角加速度以及角速度进行计算,得到卡尔曼滤波后的概率预测角度.<br>
说实话,封装好的卡尔曼确实挺好用.至于卡尔曼滤波后的效果以及卡尔曼参数调整后效果我会找个直立小车进行测试,用蓝牙传输回数据再用matlab搞出曲线对比一下.

# 4、实验部分
### 卡尔曼与互补滤波的对决
小车直立控制都是通过对角加速度和角速度进行融合后得到滤波后角度进行控制,效果见下图:

- 互补滤波

<img src="/img/191122image/hubulvbo.jpg" >

- 卡尔曼<br>

建议先看一下[线性卡尔曼公式以及滤波效果](https://simondlevy.academic.wlu.edu/kalman-tutorial/the-extended-kalman-filter-an-interactive-tutorial-for-non-experts-part-9/)
<br>小车姿态平稳变化效果:
<img src="/img/191122image/step_1-copy.jpg" >
外力频繁干预小车使姿态变化:
<img src="/img/191122image/step2-copy.jpg" >

直接移植到某淘宝库上简单调试一下改改直立零点设置即可.

<img src="/img/191122image/3-gif.gif" >

# 后记 
挖个坑 十二月推会卡尔曼.



# 扩展卡尔曼EKF(One way to handle the non-linearities)
扩展卡尔曼滤波器(EKF)放宽了卡尔曼的线性化假设,这的假设`状态转移概率`和`测量概率`由非线性函数g,h控制[书中3.2(b)].可以看出左上角的高斯分布出现了多个波峰,右上角函数也不再是线性.书中后面还给出了几种高斯分布情况下的泰勒近似,通过局部线性化可以有效的解决机器人中的非线性问题.

<img src="/img/191122image/ekf.png" >

<img src="/img/191122image/EKF.jpeg" >

扩展卡尔曼高斯分布的不确定性由非线性函数变换前的高斯分布决定,高斯分布不确定性越高(概率密度越分散),EKF的随机变量密度扭曲越大




[EKF](https://baijiahao.baidu.com/s?id=1618886253180427801&wfr=spider&for=pc)

