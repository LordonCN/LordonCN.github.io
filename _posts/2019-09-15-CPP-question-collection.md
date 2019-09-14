---
layout: post
title: "CPP-面试题积累"
subtitle: 'C++ '
author: "xudongdong"
header-img: img/jassica/jessica-jung-celebrity.jpg
catalog: true
tags:
  - C++11
---

# 0、前言
> 将学习过程中遇到的知识点做一个总结归纳。

# 1、[C++中的struct与class有什么区别](https://blog.51cto.com/genwoxuec/503334)
事实上，C++中保留struct的关键字是为了使C++编译器能够兼容C开发的程序。<br>

分以下所示两种情况:<br>
1、C的struct与C++的class的区别：struct在C语言中只是作为一种复杂数据类型定义，不能用于面向对象编程(struct中只能定义成员变量，不能定义成员函数)。<br>
2、C++中的struct和class的区别：对于成员访问权限以及继承方式，class中默认的是private的，而struct中则是public的。特别的class还可以用于表示模板类型，struct则不行。


# loading...