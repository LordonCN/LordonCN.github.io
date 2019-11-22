---
layout: post
title: "Quickly build a personal Blog by yourself"
subtitle: 'Dont u want to have a try ? Come on !'
author: "xudongdong"
header-img: img/wallpapers/wall-e-and-eve.jpg
catalog: true
tags:
  - Atom
  - Github pages
--- 

> 毕业后实习过程中遇到技术问题总要在 `google`、`stackoverflow`、`CSDN`寻找解决办法，看着各位大佬在自己的Blog上一篇一篇地记录着自己成长的点滴感觉很有意义，我寻思自己好歹也撸点代码怎么不也得自己搞个Blog玩玩？<br>

> 正好赶上周末，跟着教程自己折腾了一下，这里讲个简单快速实用的小办法，一个小时搞定没问题~这可是多次实践的结果，简要地说其实就分为三步
1. Github上创建自己的`github pages`
2. Clone博客模板工程
3. 把clone的模板覆盖掉自己的并上传

# 0、准备工作
- Atom 用来编写代码
- Github客户端 提交代码到仓库
- github down下来的blog模板（你自己钟意的那一款）

# 1、基础知识
`Github pages` 与 `jekyll` 是搭建博客的主角，接下来的内容就围绕这两个来展开。
1. Github pages：在 Github上提供了一个一站式平台，只需要创建一个repository就能获得域名挂载到Github上，快速还免费。
2. jekyll：Simple, blog-aware, static sites google吧。
jekyll是blog整体设计的框架，框架嘛理解起来就得费点功夫，不过说一小时搭建就不管这些东西了，知道用得到就可以了~

#### 个人Github page 创建
- 我个人不喜欢重复造轮子，建议认真阅读👇内容。当然，把`HTTPS`之前的内容都做好就可以了~~<br>
- [参考Brick713的GitHub Pages 搭建教程](https://sspai.com/post/54608)

#### Down 一个你钟意的 repository 模板下来
- 什么？不知道怎么下载？？ 👇看第一幅图，点download zip保存到桌面等会儿用啦~<br>
- [GitHub 上下载指定的文件夹的方法](https://blog.csdn.net/qq_35860352/article/details/80313078)

## 2、Github客户端
- 默认你已经登陆上了...<br>
- clone你的仓库到本地<br>
好，等进度条结束就clone完毕


  <img src="/img/190726image/2.jpg">

## 3、Atom客户端
- 使用 Atom软件打开工程<br>
  <img src="/img/190726image/3.png">

  <img src="/img/190726image/4.png">

把众多无关页面关闭后只保留我们创建好的项目，左侧应该只会有`index`和 `_config.yml`文件。
- 找到之前下载的模板  <br>
  <img src="/img/190726image/5.png">


然后把里面内容一股脑复制到你clone下来的那个文件夹中~
- copy到clone文件夹下 <br>
  <img src="/img/190726image/6.png">

#### 怎么运行的先不管，赶时间赶时间~记得保存一下啊，然后atom上提交一下~

## 4、提交项目
- Atom 提交  <br>

  <img src="/img/190726image/7.png">

- Github 提交 <br>
  <img src="/img/190726image/8.png">

#### 等进度条完毕，输入网址就可以输入网址看到一个别人的blog出现了😄   PS: __网址在之前创建github page的时候就出现过__

  <img src="/img/190726image/success.png">

- 刷新网页看到黄色小圆点不要急，稍等一下就好

  <img src="/img/190726image/building.png">

- 整个流程下来你的blog框架基本搭完，剩下的就是把工程里面的别人东西挨个改成你的喽~ 毕竟我也没学过 HTML、Markdown、js= =

- 行，开心😊

---------------------------------------------

## 5、Ubuntu实现对Github项目的管理
ubuntu在日常工作中逐渐占据了大量的时间,windows上面的应用虽然在 ubuntu已经有了一定的替代品但是在某些方面 Ubuntu更加好用,特别是在本地管理 Github项目。

- 国际惯例
[How to `Git`](https://www.geeksforgeeks.org/how-to-install-configure-and-use-git-on-ubuntu/)

> 当然出现什么可以单独问我啦～下面的邮件可以直接给我飞鸽传书哦

## 参考文档

- [参考minimal-mistacksGithub](https://github.com/mmistakes/minimal-mistakes)
- [参考abekthink网站](https://abekthink.github.io/test/robot-framework-tutorial-installation/)
- [图片显示出错 CSDN寻找解决办法](https://blog.csdn.net/simple_the_best/article/details/53403787)
- [Ubuntu git](https://www.cnblogs.com/sawyer22/p/9265784.html)

隐藏关：
常用Ubuntu指令：	
- pip3 -19.2 version 之后需要使用 --user安装 ：pip3 install --user (whatever)
- pip3更换国内源：	pip3 install -i https://mirrors.aliyun.com/pypi/simple numpy
- 指定python运行的GPU： $CUDA_VISIBLE_DEVICES=O/1 python3 main.py           //程序只能使用GPU 在指令开头说明使用哪一个
- 下载视频：	you-get 'https://live.bilibili.com/**'   //保存在/home
- Github维护：|git add .|git commit -m 'update'|git push|git remote set-url origin XXX.git|git clone github@**.git|
- 终端获取网页内容：wget URL
- 压缩A.txt：  	tar -cvf  A.gz   A.txt
- 解压缩： 	tar -xvf B.gz
- [ssh]远程控制：|1、ifconfig	|2、ssh dooncloud@192.168.130.37 输入帐号密码即可连接|
- [ln]：		快捷方式(符号链接) ln -s Cpp-learning/ Cpp_practice 为文件夹设置快捷方式  
- [tmux]：		tmux attach (远程连接可直接关闭对话框，attach后恢复原对话)
- [scp]拿文件：	scp -r dooncloud@192.168.130.37:/home/dooncloud/桌面/practice/xuluodong/    (本地存放位置--->) /home/dong/remote_get_ssh/
- 文件夹重命名：	mv a/ b/   (同文件夹下的改名)
- 文件夹移动：	mv (当前文件夹下的徐洛冬文件夹)xuluodong/  /home/dong/remote_get_ssh(移动到remote_get_ssh文件夹下)
- [at]定时指令:	at -f practice.sh now+1 minutes
- utorrent启动指令：|1、$utserver -settingspath /opt/utorrent-server-alpha-v3_3/|2、浏览器访问http://your-ip-:8080/gui   (ip=ifconfig查看)|3、用户名admin，密码为空：
- cat\less ：	都是阅读内容  管道用法  " ls -a /use/bin | less "
- [less]：		|按"q"退出浏览模式 |G翻到最后|g翻到开头
- [history]:	查看之前使用过的指令
- [ctrl+r]:		查找相似指令 能够快速补全长指令
- /usr/share/dict/words:  匹配单词 grep -i '^..j.r$' /usr/share/dict/words   grep -i '^predic*$' /usr/share/dict/words
- 创建.txt文本：	|重定向方法 $ > hello.txt       | ls -a /uer/bin > /home/dong/Desktop/hello.txt|或者vim text.txt|
使用>>：	|重定向输出内容 可以用来查看程序运行情况 相当于log.txt |ls -a /uer/bin > /home/dong/Desktop/hello.txt 2>&1 |如果发生错误 输出错误信息|
