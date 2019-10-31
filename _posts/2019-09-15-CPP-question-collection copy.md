---
layout: post
title: "CPP-面试题积累"
subtitle: 'C++ '
author: "Lordon"
header-img: img/jassica/jessica-jung-celebrity.jpg
catalog: true
tags:
  - C++11
---

# 0、前言
> 将学习过程中遇到的知识点做一个总结归纳。

# 题型
### (1)-70-LeetCode
```
/*30+ 出现结果错误  45数值爆炸
想法：根据排列组合原理，将数字2逐步增加时的排列组合结果相加即可
方式：进行递归运算求阶乘
结果：效果一般,不过会出现数值错误的问题。
*/

#include <iostream>

using namespace std;

int fac(int n) {//递归
    if(n == 1) 
        return 1;
    else
        return (long int)(n * fac(n-1));
}

int fac_stop(int n,int stop) {//递归
    if(n == stop) 
        return 1;
        
    else
        // cout<<n<<endl;
        return (long int)(n * fac_stop(n-1,stop));
}

int main()
{
    while(1)
    {
        unsigned long int ThereAreWaysToclimb=0;
        int n=0 ; //HowManyStairs
        int i;
        int step_1=1;
        int step_2=2;
        int max_step_2_is=0;
        cout<< "pls input stairs"<<endl;
        cin  >> n ;//input how many stairs
        /*
        分析： 通过计算2的个数可以得出有几种方案
        */
        if (n==1)
            ThereAreWaysToclimb=1;
        else if (n%2==0)
            {
                max_step_2_is = n/2;
                for (int i=1;i<max_step_2_is;i++) //有i步是2的时候
                {
                    ThereAreWaysToclimb=ThereAreWaysToclimb+fac_stop((n-i),n-2*i)/fac_stop(i,1);
                    // ThereAreWaysToclimb=ThereAreWaysToclimb+(fac(n-i))/(fac(i)*fac(n-2*i));
                }
                // cout<<"偶数楼梯"<<endl;
                ThereAreWaysToclimb= ThereAreWaysToclimb+2 ;//加上两端
            }
        else 
            {
                max_step_2_is = n/2;
                for (int i=1;i<=max_step_2_is;i++)
                {
                    // cout<<"------------"<< i<<"------------"<<endl;
                    // fac_stop((n-i),n-2*i);
                    ThereAreWaysToclimb=ThereAreWaysToclimb+fac_stop((n-i),n-2*i)/fac_stop(i,1);
                }
                // cout<<"奇数楼梯"<<endl;
                ThereAreWaysToclimb= ThereAreWaysToclimb+1 ;
            }
        
        cout  << ThereAreWaysToclimb << endl;
    }
}

```
### (2)-70-LeetCode
```
/*  
想法：动态规划 
方式：递归运算
结果：到44层计算超时
*/
#include <iostream>
using namespace std;

# define N  200
int pb[N];

int dynamic_program(int n) {
    if((n == 1)||(n == 2)) //递归到最开始两级
        return n;

    pb[n-1] = dynamic_program(n-1);//上到n-1阶的步数
    pb[n-2] = dynamic_program(n-2);//上到n-2阶的步数
    pb[n] = pb[n-1]+pb[n-2]; //从 n-1 与 n-2 一步上来之和
    return pb[n];
}

int main()
{
    while(1)
    {
        int n;
        cout<<"pls input n:"<<endl;
        cin>> n;

        pb[N-1] = dynamic_program(n);
        cout<< pb[N-1]<<endl;
    }
}

```
# 1、[C++中的struct与class有什么区别](https://blog.51cto.com/genwoxuec/503334)
事实上，C++中保留struct的关键字是为了使C++编译器能够兼容C开发的程序。<br>

分以下所示两种情况:<br>
1、C的struct与C++的class的区别：struct在C语言中只是作为一种复杂数据类型定义，不能用于面向对象编程(struct中只能定义成员变量，不能定义成员函数)。<br>
2、C++中的struct和class的区别：对于成员访问权限以及继承方式，class中默认的是private的，而struct中则是public的。特别的class还可以用于表示模板类型，struct则不行。

-------------------
# 2、字符串匹配问题
关于调用string头文件后进行字符串的读取与匹配的问题。<br>
起因是早上看的一个统计输入长字符串中符合条件的字符串的个数问题，具体点：<br>
输入： abs but cut hello bye nihao    //6个字符拼接成的长字符串<br>
输出： 字符串中含有一个hello<br>

说明原因：
首先需要了解cin的用法。程序的输入都有一个缓冲区，当一次键盘输入结束时会将输入的数据存入输入缓冲区，而cin函数直接从输入缓冲区中读取数据。这种缓冲机制规定，只有收到回车键时，才会将所有输入的数据一次性提交到cin函数。回车标志一次输入的完成，如果数据不够，则会等待用户继续输入；如果数据有多余，则将多余的数据存储在输入流缓冲区中，供下次使用。举个栗子：
```coq
while（cin>>num）
cout<<num<<endl;
```
这条语句，如果输入一次1   2   3   4(用空格隔开),执行结果是：
```
1
2
3
4
```
<br>
执行过程如下：
第一次运行cin>>num的时候，输入缓冲区为空，所以会显示下划线让用户输入。用户输入1  2  3  4  ctrl+z，回车，这时候cin读入第一个整数1，然后输出1和换行。
下一次执行 cin>>num的时候，缓冲区不为空，所以不再要求用户输入，直接读取第二个整数2，然后输出2和换行。以此类推，依次输出3 ，4。
然后cin检查到结束标志ctrl+z，cin>>num返回false，循环退出。
需要特别注意的一点是：当缓冲区中有残留数据时，cin函数会直接去读取这些残留数据而不会请求键盘输入。而且，回车符也会被存入输入缓冲区中。

直接使用cin从命令行获取长字符串的原理：
<br>
开头的空格自动忽略，读取第一个字符串之后后面的内容均存储在输入缓冲区，通过反复读取缓冲区内容进行字符串的逐个判断。
- 这种方法相比使用char数组进行字符串匹配判断优势明显，那么实现一个输入密码并判断的程序就简单多了，奈何在测试的时候发现了不少问题。<br>
- 1、输入为 : true false 后无法控制只读取第一个字符(我太菜)。<br>
- 2、开头输入空格后当然也要进行判断。<br>
研究了下无论是cin.clear() cin.syns使用下面替换cin即可：
```coq
getline(cin,password);  //`非阻塞`输入字符串  
```

Code?<br>
```coq
#include <iostream>
#include <string>

using namespace std;

bool IsPasswordRightorNot()
{
   string password;
   int num_now=3;
   while(num_now--)                          //有限次机会   3次
   {
      cout<<"Pls input password"<<endl;
      getline(cin,password);                 //非阻塞输入字符串  
      if(password=="password_is_me")
      {
         cout<<"welcome"<<endl; 
         return true;
      }
      else cout<<"Pls try again,only"<<num_now<<"times left"<<endl;
   }
   return false;
}

int main()
{
   bool PassOrNot=false;

   PassOrNot=IsPasswordRightorNot();
   //if pass then next step
   cout<<"print right_wrong:"<<PassOrNot<<endl;
   return 0;
}
```
