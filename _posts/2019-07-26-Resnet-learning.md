---
layout: post
title: "Its ResNet interesting ?"
subtitle: 'Yes！'
author: "xudongdong"
header-img: img/jassica/jessica-jung.jpg
catalog: true
tags:
  - ResNet
  - Deeplearning
---

> `ResNet`个人理解~
- [2015年ResNet论文](http://arxiv.org/pdf/1512.03385.pdf )

- ResNet神经网络在生活中得到了广泛应用，无论是在研究生研究阶段就还是公司日常项目，选用它都是不错的。`ReaNet相比VGG网络具有更加显著的优良效果，文末ResNet50流程图左侧可以近似为VGG，是一种低频处理;右侧为残差项，这更像是高频信号，两者相加更显完整性` 之前在分析源代码的时候没有能够很好的理解残差和欠采样流程图的关系，走了很多弯路，自己给自己绕晕了，今天同事一语点通，幸好幸好。

- 关于这个网络的详细介绍有不少，这里贴一下使用的ResNet18网络我个人写的注释和理解,本文中所设计流程(未画欠采样)如下图所示：

  <img src="/img/190728image/resnet18.JPG">

# 卷积

>19.10.3更新，吴恩达的cs231还是建议系统学一下，如果时间真的没有那么充足，可以参考他们整理过的doc[cs20-class6](http://cs231n.github.io/convolutional-networks/)

- 卷积是第一个需要重点了解的概念，VGG、ResNet以及以后会学习到的不少网络都属于CNN网络。有网友提供的关于计算卷积操作下输入与输出图像大小变化的公式不一定适合所有的情况，所以说在定制相关网络的时候还是最好手动计算一下,这样在自行设计网络的时候修改卷`kernal_size`、`stride`和`padding`的时候更加得心应手。注意：画图的时候 `使用stride参数来画图！** **使用stride参数来画图！** **使用stride参数来画图！`

  <img src="/img/190728image/convjisuan.jpg">

- 因为自己也一直模棱两可，所以做了下实验列个表，大家可以使用上面方法手算，卷积不到的地方就去掉，会出来第三个`23×23`的结果~

| planes | kernel_size | stride | input | padding | output |
| :----: | :----: |:----:|:----:|:----:|:----:|
| 64 | 3 | 2 | 48 48 3 | 1 | 24 24 64 |
| 64 | 3 | 1 | 48×48×3 | 1 | 48×48×64 |
| 64 | 3 | 2 | 48×48×3 | 0 | 23×23×64 |    
| 64 | 2 | 1 | 48×48×3 | 0 | 47×47×64 |
| 64 | 2 | 1 | 48×48×3 | 1 | 49×49×64 |

- ResNet网络原始输入图像一般使用`224×224`大小，其实这个网络对输入图像的大小限制并不严格，我认为是Github给的源码他们设计的网络所涉及到的conv和pool操作是针对`224×224`的图像。这里我所使用的这个例子，输入为`48×48`的图像(Fer2013数据集)经过随机剪裁(Pytorch transforms.RandomCrop(cut_size)),实际进入网络的图像变成了`44×44`大小的图片，最后通过`ResNet18`进行卷积与全连接。
- 注意：这个网络因为输入图片的大小问题适当去掉了`pooling`与改变了`conv`过程中`stride`的大小，源代码链接如下：
- [WuJie1010/Facial-Expression-Recognition.Pytorch](https://github.com/WuJie1010/Facial-Expression-Recognition.Pytorch)

- 说起卷积之后图像的变化情况，这里我用一个简单的程序实现了一下，类名称其实不应该写成resnet😂，stride分别为1和2时效果图如下~
- [My image conv sourse code](https://github.com/Tcloser/resnet_conv_show) 详细内容可参考代码注释和README


  <img src="/img/190728image/stride=1.jpg">

  <img src="/img/190728image/stride=2.jpg">

## BasicBlock
适用于`ResNet18`与`ResNet34`

```coq
class BasicBlock(nn.Module):
    expansion = 1
    '''含有两个残差块 第一次过第一个残差块时由外部传入stride 之后stride=1'''
    def __init__(self, in_planes, planes, stride=1):
        super(BasicBlock, self).__init__()
        self.conv1 = nn.Conv2d(in_planes, planes, kernel_size=3, stride=stride, padding=1, bias=False)
        self.bn1 = nn.BatchNorm2d(planes)
        self.conv2 = nn.Conv2d(planes, planes, kernel_size=3, stride=1, padding=1, bias=False)
        self.bn2 = nn.BatchNorm2d(planes)
        '''
        1、如果残差计算输出与原输出大小不一致则在shortcut一路多加一次conv+BN 使得out += self.shortcut(x)公式得以成立
        2、 一般shortcut支路上无东西
        '''
        self.shortcut = nn.Sequential()
        if stride != 1 or in_planes != self.expansion*planes: #如果残差计算输出与原输出大小不一致
            self.shortcut = nn.Sequential(                    #原结果多加一个卷积 具体见图片
                nn.Conv2d(in_planes, self.expansion*planes, kernel_size=1, stride=stride, bias=False),
                nn.BatchNorm2d(self.expansion*planes)
            )
    def forward(self, x):
        out = F.relu(self.bn1(self.conv1(x)))               #每一层的第一个残差块的stride 由self.layer(n)传入
        out = self.bn2(self.conv2(out))                     #之后的 stride=1 不影响图像大小
        out += self.shortcut(x)                             #残差相加
        out = F.relu(out)
        return out

```

## Bottleneck
适用于`ResNet50`以上的网络

```coq
class Bottleneck(nn.Module):
    expansion = 4                #最后输出512*4 通道
                                 #含有三个残差块 第一次过第二个残差块时由外部传入stride 之后stride=1
    def __init__(self, in_planes, planes, stride=1):
        super(Bottleneck, self).__init__()
        self.conv1 = nn.Conv2d(in_planes, planes, kernel_size=1, bias=False)
        '''norm 参考AlexNet 归一化处理'''
        self.bn1 = nn.BatchNorm2d(planes)
        self.conv2 = nn.Conv2d(planes, planes, kernel_size=3, stride=stride, padding=1, bias=False)
        self.bn2 = nn.BatchNorm2d(planes)
        '''第三个卷积影响着输出的channel*4 具体参考ResNet50流程图'''
        self.conv3 = nn.Conv2d(planes, self.expansion*planes, kernel_size=1, bias=False)#(1)64->256  (2)->64->256
        self.bn3 = nn.BatchNorm2d(self.expansion*planes)

        self.shortcut = nn.Sequential()
        if stride != 1 or in_planes != self.expansion*planes:
            self.shortcut = nn.Sequential(
                nn.Conv2d(in_planes, self.expansion*planes, kernel_size=1, stride=stride, bias=False),
                nn.BatchNorm2d(self.expansion*planes)
            )
    def forward(self, x):
        out = F.relu(self.bn1(self.conv1(x)))
        out = F.relu(self.bn2(self.conv2(out)))
        out = self.bn3(self.conv3(out))
        out += self.shortcut(x)
        out = F.relu(out)
        return out

```
## ResNet网络
### 初始化
```coq
def __init__(self, block, num_blocks, num_classes=7):   #ResNet使用的basic类型和卷积层每一层运算次数ResNet18为[2，2，2，2]
        super(ResNet, self).__init__()
        self.in_planes = 64         #输入通道数为64
        self.conv1 = nn.Conv2d(3, 64, kernel_size=3, stride=1, padding=1, bias=False)   #二维卷积 输入3通道 输出64通道 卷积核3个 步长为1 padding为1补一圈 无偏置项
        self.bn1 = nn.BatchNorm2d(64)#卷积输出为64通道 经过BN----->查BN是啥
        '''
        1、由num_blocks[]+stride 到def _make_layer()函数给每一层layer的每一次卷积赋stride值 第二个卷积开始stride=1
        注意：这里padding隐藏在BasicBlock中  如果计算中出现小数说明有隐藏padding未找到！！
        2、返回的nn.Sequential()由所选择的block类型回到上面basicblock（bottleblock）类调整输出图像的通道大小
        '''
        self.layer1 = self._make_layer(block, 64, num_blocks[0], stride=1)
        self.layer2 = self._make_layer(block, 128, num_blocks[1], stride=2)
        self.layer3 = self._make_layer(block, 256, num_blocks[2], stride=2)
        self.layer4 = self._make_layer(block, 512, num_blocks[3], stride=2)#这个 layer4 维度可以输出 6*6*512
        self.avgpool = nn.AvgPool2d(6,stride=1)           #平均池化要把最后输出 out = self.avgpool(out) out维度变为1*1*512           
        self.fc = nn.Linear(512,num_classes)              #将512种类全连接到7种类型上 num_classes是最后目标要分成的7类
        self.output = 1                                   #下面备用

```
###  make_layer
这个源码中将原来论文中的`makelayer`进行了修改，去掉了`downsample`：
```coq
                                          #def _make_layer()。1、给不同深度的卷积层的每一层stride赋值。2、stride！=1 时 维度的变化在同一卷积层只改变一次
def _make_layer(self, block, planes, num_blocks, stride):
        strides = [stride] + [1]*(num_blocks-1)  #(num_blocks-1)给这一层卷积的第一层之外的所有层stride置1 只有第一层接收传过来的stride
        layers = []
        for stride in strides:
            layers.append(block(self.in_planes, planes, stride))
            self.in_planes = planes * block.expansion
        return nn.Sequential(*layers)

```

反观原论文，downsample 还是有必要分析一下的，参考
- [Basicblock和bottleneck的区别](https://www.cnblogs.com/wzyuan/p/9880342.html)

```coq
def _make_layer(self, block, planes, blocks, stride=1):
        downsample = None                                             
                                          #这部分是降采样 stride！=1 或者 使用resnet50这种调用bottleneck模块 第一次经过layer[n]的时候在shortcut求和之前加入
        if stride != 1 or self.inplanes != planes * block.expansion:            
            downsample = nn.Sequential(               
                conv1x1(self.inplanes, planes * block.expansion, stride),                
                nn.BatchNorm2d(planes * block.expansion),
            )                                                             
                                          #stride！=1 时 只有第一次经过此laye[n]]时stride=stride 整个layer图像大小只变化一次        
        layers = []        
        layers.append(block(self.inplanes, planes, stride, downsample))        
        self.inplanes = planes * block.expansion       
        for _ in range(1, blocks):            
            layers.append(block(self.inplanes, planes))
        return nn.Sequential(*layers)

```

加入`downsample`的流程图，如下所示

  <img src="/img/190728image/renet18downsample.JPG">

### forward
```coq
def forward(self, x):#x是输入的图片数据 数据集改变的是x
        out = F.relu(self.bn1(self.conv1(x)))# size/1
        out = self.layer1(out)            #'''torch.Size([90, 64, 44, 44])..'''
        # size/2 只在这一层的第一次运算改变图片大小
        out = self.layer2(out)            #'''torch.Size([90, 128, 22, 22])..'''
        #print(net.output.shape) 可返回layer(n)卷积后维度的大小
        self.output = self.layer3(out)    #'''torch.Size([200, 256, 11, 11])..'''
        out = self.output                                                   #查看运行时候torch.Size很有必要
        out = self.layer4(out)            #'''torch.Size([90, 512, 6, 6])..'''
        #这里大小6
        #out = F.avg_pool2d(out, 4)# size-4)/6+1
        # size-6+1     跟卷积运算图像变化算法相同
        out = self.avgpool(out)           #'''torch.Size([90, 512, 1, 1])'''
        out = out.view(out.size(0), -1)   #'''torch.Size([90, 512])'''
        out = F.dropout(out, p=0.5, training=self.training)  #重新随机删除一些神经元 防止过拟合
        out = self.fc(out)                #basic block中是1*1*512--->1*1*7 || bottle中是 1*1*512*expansion-->1*1*7
        return out                        #7种表情相似度的一维数据  [0.2,0.4,0.2,0.6...]

```
### 传入每层数量
```coq
def ResNet18():                           #指17个卷积层+1个fc   conv1+2*（2*conv2+2*conv3+2*conv4+2*conv5）+fc
    return ResNet(BasicBlock, [2,2,2,2])  #basicblock中每个layer都有2个残差块且resnet18这里每一个layer都要过两遍
def ResNet50():
    return ResNet(Bottleneck, [3,4,6,5])  #Bottleneck中每个layer都有3个残差块且resnet50这里每一个layer要分别过3、4、6、5次

```
---------
## 参考文档
- [ResNet 详解](https://blog.csdn.net/lanran2/article/details/79057994)
- [ResNet 解读](https://blog.csdn.net/csdnldp/article/details/78313087) 降采样并没有说明白  
- [源代码解析](https://zhuanlan.zhihu.com/p/31502877)
- [CUDA CUDNN TORCH安装](https://github.com/floydhub/dl-setup)
- [torch 学习](https://zhuanlan.zhihu.com/p/25572330)
- [VGG Net](https://blog.csdn.net/dcrmg/article/details/79254654)
- [CUDA 并行计算](https://www.pytorchtutorial.com/pytorch-large-batches-multi-gpu-and-distributed-training/)
- [torch.loss 原理](https://blog.csdn.net/qq_22210253/article/details/85229988)
- [torch 主函数详解](https://blog.csdn.net/u014380165/article/details/78525273/)
- [optim 多种优化器](https://zhuanlan.zhihu.com/p/22252270)

---------
最后贴一个网上的ResNet50神图(侵删)：(小声：右半部分才是精华~) 

  <img src="/img/190728image/ReaNet50.png">
