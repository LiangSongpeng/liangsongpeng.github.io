---
layout: post
title:  "编程中的数学思维 & Fibonacci"
---

* content
{:toc}

## 编程中的数学

最近因为新型冠状病毒感染肺炎疫情的影响，即使是假期，也哪都不能去，只有窝在家里。

窝在家里，有大把的时间，那就做一做算法题好了。其实作为一个需要编程的人员，数学功底扎实是很重要的。

有一个题目，例如：假设你正在爬楼梯。需要 n 阶你才能到达楼顶。每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

<!-- more --> <!-- 摘要预览与正文的分隔符 -->

---

## 暴力法

对于这个题呢，首先看看暴力法解题。

将所有可能爬的阶数进行组合，也就是 1 和 2 。而在每一步中我们都会继续调用 cs 这个函数模拟爬 1 阶和 2 阶的情形，并返回两个函数的返回值之和。

cs(i, n) = cs(i + 1, n) + cs(i + 2, n)

其中 i 定义了当前阶数，而 n 定义了目标阶数。

代码实现：

```c++
int climbStairs(int n) 
{
    return cs(0, n);
}
int cs(int i, int n)
{
    if (i > n)
        return 0;
    else if (i == n)
        return 1;
    return cs(i + 1, n) + cs(i + 2, n);
}
```

时间复杂度：O(2<sup>n</sup>) 。

空间复杂度：O(n) 。

---

## Fibonacci法

再来看看斐波那契（Fibonacci）法。

斐波那契公式：

Fib(n) = Fib(n − 1) + Fib(n − 2)

对于此题，也就是找出以 1 和 2 作为第一项和第二项的斐波那契数列中的第 n 个数，即 Fib(1) = 1 且 Fib(2) = 2。

代码实现：

```c++
int climbStairs(int n) 
{
    if (n == 1) 
        return 1;
    int first = 1;
    int second = 2;
    for (int i = 3; i <= n; i++) 
    {
        int third = first + second;
        first = second;
        second = third;
    }
    return second;
}
```

时间复杂度：O(n) 。

空间复杂度：O(1) 。

用斐波那契法，无论从时间复杂度还是从空间复杂度来说，都远远优于暴力法。

这其实也就是**数学**在编程之中的作用。能够为其提供更为简便、优美的算法。
