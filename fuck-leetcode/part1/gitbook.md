---
title: 如何计算最大公约数
date: 2020.3.13
tags:
 - 最大公约数

categories: 
 - 数学
 - 编程

keywords: [辗转相除法, 更相减损术]
description: #如果不写，自动选取文章中的部分作为描述
top_img: 
comments: true  #是否顯示評論（除非設置false,可以不寫）
cover: 
toc: true #是否顯示toc （除非特定文章設置，可以不寫）
toc_number: true #是否顯示toc數字 （除非特定文章設置，可以不寫）
copyright: #是否顯示版權 （除非特定文章設置，可以不寫）
mathjax: false
katex: true
hide:
---

# 如何求最大公约数

最大公约数指能够**整除多个整数**的最大正整数。而多个整数不能都为零。

本文将从暴力法开始一步步地求解最大公约数，从理论推导到计算机代码实现的的一个递进过程。

## 暴力法

没有什么是暴力法解决不了的，如果有，那就是我们不能把有限的时间内投入到无限的找bug中。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcud1ue072j30im0j21bc.jpg" style="zoom:50%;" />



### 暴力法的原理

暴力法的原理很简答，就是从2开始，枚举每一个数字，直到最小的数字。

### 暴力法的代码实现

```cpp
int gcd(int a, int b)
{
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    int res = 1;
    for(int i = 2; i <= smaller; i++)
    {
        if((bigger % i == 0) && (smaller % i == 0))
            res = i;
    }
    return res;
}
```

## 辗转相除法

### 辗转相除法的历史

辗转相除法， 又名欧几里得算法（Euclidean algorithm），其历史可以追溯到公元前300多年。
> “欧几里得算法是所有算法的鼻祖，因为它是现存最古老的非凡算法。”- 高德纳


<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcsmhlv6cqj30n40n6b29.jpg" alt="image-20200313213018498" style="zoom:50%;" />

### 辗转相除法原理

**两个正整数$a$和$b$，$(a > b)$，它们的最大公约数等于$a$除以$b$的余数$c$和$b$之间的最大公约数。**

**翻译成人话，举个例子**

利用辗转相除法计算 $a = 25$和$b = 10$的最大公约数的过程如下：
> 1. 从$25$中不断减去$10$，直到小于10，(可以减去2次，商为2)，余数为5
> 2. 从10中不断的5，直到结果小于5，(可以减去2次，商为2)，余数为0
> 3. 当余数为0的时候，算法终止，否则继续回到1.2。

| 步骤数 |        算式         |            商和余数             |
| :----: | :-----------------: | :-----------------------------: |
|   0    | 25 = 10 *q*0 + *r*0 |       *q*0 = 2、*r*0 = 5        |
|   1    | 10 = 5 *q*1 + *r*1  | *q*1 = 2、*r*1 = 0 （算法终止） |

写成连续的等式形式如下：

$$gcd(25, 10) = gcd(10, 5) = gcd(5, 0) = 5$$



将例子升华一下，形成通用的方程：

$$gcd(a, 0) = a$$
- 对应递归或者循环退出条件

$$ gcd(a, b) = gcd(b, a\;mod\; b)$$
- 对应递归关系，层与层之间的关系。

对于验证辗转相除法的正确性的证明，这里就不讲了。

### 辗转相除法的代码实现

```cpp
int gcd(int a, int b)
{
    if(a == 0 || b == 0)
        return a ? a : b;
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    return gcd(smaller, bigger % smaller);
}
```

### 辗转相除法存在的问题

**当两个正整数**$a$和$b$都特别大的时候，做$a\;mod\; b$的性能开销比较大。所以要将取模的操作进行替换。

## 更相减损术

### 更相减损术的历史

古希腊人很聪明，但是我们的祖先也不差，更相减损术，出自于中国古代的《九章算术》。

![](https://tva1.sinaimg.cn/large/00831rSTgy1gct8xlddmmg30bn0f6q8f.gif)

### 更相减损术原理

**两个正整数$a$和$b$（$a>b$），它们的最大公约数等于$a-b$的差值$c$和较小数$b$的最大公约数。**

这个就不用翻译成人话了，直接给出理论推倒。

$$gcd(a, 0) = a$$
- 对应递归或者循环退出条件

$$ gcd(a, b) = gcd(b, \;a\;-\; b)$$
- 对应递归关系，层与层之间的关系。

最后直接上代码。

### 更相减损术的代码实现

```cpp
int gcd(int a, int b)
{
    if(a == 0 || b == 0)
        return a ? a : b;
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    return gcd(bigger - smaller, smaller);
}
```

### 更相减损术存在的问题

虽然这里用减法代替了取模操作，但是由于每次都做的是$a - b$操作，所以收敛速度是一个问题。举个极端的例子，当$a = 9999, b = 1$时，这个收敛速度就很慢，在计算机中就要$9998$次递归。时间和空间性能都存在很大问题。

## 中西结合

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcud5di2vdj315w0q61kx.jpg" alt="image-20200315093823406" style="zoom:50%;" />

求最大公约数的问题，可以看成纯粹的数学计算。在计算机中有关有关于计算优化的问题，总结了以下几点：

> 1. 能用加减解决的，尽量不要用乘除法搞定。
> 2. 能用移位运算搞定的，不要用乘除法搞定。
>    1. 比如`a /= 2` 写成 `a >>= 1`
>    2. `a *= 6` 写成 `a = a << 2 + a << 1`
> 3. 尽量避免一些取模操作，一些特殊的取模操作可以用`&`运算符代替。
>    1. 判断奇偶`a % 2` 写成 `a & 0x1` 形式。
>    2. 对`a % (2^n)`这种数字求模写成`a & (2^n - 1)`。
> 4. 优化计算方法，减少计算迭代的次数。

在这里，对暴力法的改进成辗转相除法或更相减损术对应最后一点，是最难的一点，性能提升的很高，数学家一般干这个。

而大多数人还是通过1、2、3、4条进行改进就可以了。

最后对辗转相除法和更相减损术进行一些计算上的优化，首先应该明确以下一些原理：

> 1. 当正整数a和b（a>b）都是偶数时，`gcd(a, b) = gcd(a/2, b/2) * 2`
> 2. 当正整数a为奇数时，b（a>b）是偶数时，`gcd(a, b) = gcd(a, b/2)`
> 3. 当正整数a为偶数时，b（a>b）是奇数时，`gcd(a, b) = gcd(a/2, b)`
> 4. 当正整数a为奇数时，b（a>b）是奇数时，`gcd(a, b) = gcd(b, a-b)`，此时的`a-b`是一个整数，又回到了一奇一偶的情况。

注意到其中存在除以2这种特殊的数字，所以直接使用移位运算代替。

### 结合位运算的辗转相除法和更相减损术的代码实现

```cpp
int gcd(int a, int b)
{
    if(a == 0 || b == 0)
        return a ? a : b;
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    /*a 偶数  b 偶数*/
    if(!(smaller & 0x1) && !(bigger & 0x1))
        return gcd(bigger >> 1, smaller >> 1) << 1;
    
    /*a 偶数  b 奇数*/
    else if(!(smaller & 0x1) && (bigger & 0x1))
        return gcd(bigger, smaller >> 1);

    /*a 奇数  b 偶数*/
    else if((smaller & 0x1) && !(bigger & 0x1))
        return gcd(bigger >> 1, smaller);

    /*a 奇数  b 奇数*/
    else
        return gcd(bigger - smaller, smaller);
}
```



## 总结

其实，有很多东西都是原理或者理论简单，但是实际实现起来还是会有很多细节需要注意的，比如甚至是一个看起来不成问题的求最大公约数的问题，还有这么多门道。

为了尽快给出一个能用可行的方案，使用了暴力法，但是对于效率和性能的优化是我们必须追求的目标。所以引入了两个求最大公约数的算法。辗转相除法要求两个数的模，但是在计算机中的计算又要尽量避免大数的取模操作。采用更相减损术将取模操作改成了加减法，但是收敛速度又成为了一个大问题。继而将两种方案结合起来，形成一个相对高效的求两个数的最大公约数算法和代码。

>这里不得不说，数学才是真正的生产力，是优化的正道。
>
>而对于一些位运算优化或者代码层面的优化，相对于数学方法的优化，只能说是旁门左道。


## 测试代码

```cpp
#include <iostream>
#include <vector>
#include <ctime>
#include <cmath>
#include <sys/time.h>
using namespace std;

namespace bf{
int gcd(int a, int b)
{
    if(a == 0 || b == 0)
        return a ? a : b;
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    int res = 1;
    for(int i = 2; i <= smaller; i++)
    {
        if((bigger % i == 0) && (smaller % i == 0))
            res = i;
    }
    return res;
}
};

namespace div{
int gcd(int a, int b)
{
    if(a == 0 || b == 0)
        return a ? a : b;
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    return gcd(smaller, bigger % smaller);
}
};

namespace gx{
int gcd(int a, int b)
{
    if(a == 0 || b == 0)
        return a ? a : b;
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    return gcd(bigger - smaller, smaller);
}
};

namespace divandgx{
int gcd(int a, int b)
{
    if(a == 0 || b == 0)
        return a ? a : b;
    int smaller = a < b ? a : b;
    int bigger = a > b ? a : b;
    /*a 偶数  b 偶数*/
    if(!(smaller & 0x1) && !(bigger & 0x1))
        return gcd(bigger >> 1, smaller >> 1) << 1;
    
    /*a 偶数  b 奇数*/
    else if(!(smaller & 0x1) && (bigger & 0x1))
        return gcd(bigger, smaller >> 1);

    /*a 奇数  b 偶数*/
    else if((smaller & 0x1) && !(bigger & 0x1))
        return gcd(bigger >> 1, smaller);

    /*a 奇数  b 奇数*/
    else
        return gcd(bigger - smaller, smaller);
}
};



vector<int> testgcd(string testname, int(*gcd)(int, int), vector<int>& a, vector<int>& b)
{
    struct timeval start, end;
    gettimeofday(&start, NULL);
    vector<int> res;
    int len = a.size();
    for(int i = 0; i < len; ++i)
    {
        res.push_back(gcd(a[i], b[i]));
    }
    gettimeofday(&end, NULL);
    double duration = ((end.tv_sec - start.tv_sec) * pow(10, 6) + (end.tv_usec - start.tv_usec));
    printf("test %s has findished, it spent %f us\n", testname.c_str(), duration);
    return res;
}

int main(void)
{
    int testNum = 1000000;
    vector<int> avec, bvec;
    int left = 0, right = 10000;
    srand(time(NULL));
    for(int i = 0; i < testNum; i++)
    {
        int a  = random() % (right - left + 1) + left;
        avec.push_back(a);
        int b  = random() % (right - left + 1) + left;
        bvec.push_back(b);
    }

    auto res1 = testgcd("暴力求解法", bf::gcd, avec, bvec);
    auto res2 = testgcd("辗转相除法", div::gcd, avec, bvec);
    auto res3 = testgcd("更相减损术", gx::gcd, avec, bvec);
    auto res4 = testgcd("位运算优化", divandgx::gcd, avec, bvec);    

    if(res1 != res2 || res1 != res3 || res1 != res4)
        cout << "Error " <<endl;
}
```

- 运行结果
> test 暴力求解法 has findished, it spent 7127775.000000 us
> test 辗转相除法 has findished, it spent 75551.000000 us
> test 更相减损术 has findished, it spent 66527.000000 us
> test 位运算优化 has findished, it spent 112157.000000 us