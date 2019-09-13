---
layout: post
title: "cpp-array-of-pointers"
subtitle: '最近预习C++ 看到指针这里总结的挺棒 分享一波'
author: "xudongdong"
header-img: img/jassica/jasscia2.gif
catalog: true
tags:
  - C++
---

# 0、前言
> `指针数组`与`数组指针` 这两个概念下午给搞晕了

# 1、指针数组
- 想要让数组存储指向 int 或 char 或其他数据类型的指针。下面是一个指向整数的指针数组的声明：
```coq
int *ptr[MAX];
```
`ptr`声明为一个数组，由 MAX 个整数指针组成。因此，`ptr`中的每个元素都是一个指向 int 值的指针。

### 实例 -int
```coq
#include <iostream>
 
using namespace std;
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
   int *ptr[MAX];
 
   for (int i = 0; i < MAX; i++)
   {
      ptr[i] = &var[i]; // 赋值为整数的地址
   }
   for (int i = 0; i < MAX; i++)
   {
      cout << "Value of var[" << i << "] = ";
      cout << *ptr[i] << endl;
   }
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：

```coq
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```
### 实例 -char
`char *names[MAX]` 这种字符型的指针数组是存储指针的数组，但是在理解字符型指针数组的时候，可以将它理解为一个二维数组。
- 如 const char *names[4] = {"Zara Ali","Hina Ali","Nuha Ali","Sara Ali",} <br>

可以理解为一个 4 行 8 列的数组，可以用 `cout << *(names[i] + j)<< endl` 取出数组中的每个元素。
```coq
#include <iostream>

using namespace std;

const int MAX = 4;
int main ()
{
    const char *names[MAX] = {
        "Zara Ali",
        "Hina Ali",
        "Nuha Ali",
        "Sara Ali",
    };

    for (int i = 0; i < MAX; i++)
        for (int j = 0; j < 8; j++)
        {
            cout << "Value of names[" << i << "] = ";
            cout << *(names[i] + j)<< endl;
        }
    return 0;
}
```
输出结果为：
```coq
Value of names[0] = Z
Value of names[0] = a
Value of names[0] = r
Value of names[0] = a
Value of names[0] =  
Value of names[0] = A
Value of names[0] = l
Value of names[0] = i
Value of names[1] = H
Value of names[1] = i
Value of names[1] = n
Value of names[1] = a
Value of names[1] =  
Value of names[1] = A
Value of names[1] = l
Value of names[1] = i
Value of names[2] = N
Value of names[2] = u
Value of names[2] = h
Value of names[2] = a
Value of names[2] =  
Value of names[2] = A
Value of names[2] = l
Value of names[2] = i
Value of names[3] = S
Value of names[3] = a
Value of names[3] = r
Value of names[3] = a
Value of names[3] =  
Value of names[3] = A
Value of names[3] = l
Value of names[3] = i
```

# 2、数组指针
```
int (*ptr)[3];
```
C++中优先级顺序是`*`小于`()`，`()`等于`[]`，`()`和`[]`的优先级一样，但是结合顺序是从左到右，所以先是`()`里的`*`和`ptr`结合成为一个指针，然后是`(*ptr)`和`[]`相结合成为一个数组，最后叫一个指针`ptr`指向一个数组，简称数组指针。

```
int main()
{
  int v[3] = {10, 100, 200}; 
  int (*ptr)[3] = &v;
  cout<<*(ptr)[0]<<endl;
}

OUTPUT:
10
```




