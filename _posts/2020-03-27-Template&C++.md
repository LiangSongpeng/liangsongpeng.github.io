---
layout: post
title:  "设计模式：Template 模式 & C++ 实现"
---

* content
{:toc}

## 模板方法模式

设计模式主要分为三大类：

> - 创建型
> - 行为型
> - 结构型

设计模式的具体介绍在之前的那篇“设计模式：Singleton 模式 & C++ 实现”中有提到过，在这里就不再说了。上次所提到的 Singleton（单例）模式是属于创建型的设计模式，而本次的 Template（模板方法）模式则是属于结构型的行为模式。

 在面向对象系统的分析与设计过程中经常会遇到这样一种情况：对于某一个逻辑（算法实现）在不同的对象中有不同的细节实现，但是逻辑（算法）的框架（或通用的应用算法）是相同的。对于这种问题，可采用 Template 模式进行解决，Template 模式提供了这种情况的一个实现框架。Template 模式是采用继承的方式实现这一点：将逻辑（算法）框架放在抽象基类中，并定义好细节的接口，子类中实现细节。

Template 模式适用于：每一个类中大部分都一样，只有少部分不一样。则可以把那少部分不一样的放到父类的纯虚函数中，在子类中重写不一样的部分，一样的部分直接继承父类的。

<!-- more --> <!-- 摘要预览与正文的分隔符 -->

---

## C++ 实现

Template 模式实现代码如下：

```c++
#include <iostream>
#include <string>
using namespace std;

class CPerson
{
public:
	virtual void EatStyle() = 0;  //纯虚函数
public:
	void Eat()                    //模板
	{
		cout << "Ordinary" << endl;
		this->EatStyle(); 
		cout << endl;
	}
};

class CPerson_A : public CPerson
{
public:
	virtual void EatStyle()        //重写
	{
		cout << "Unusual" << endl;
	}
};

class CPerson_B : public CPerson
{
public:
	virtual void EatStyle()        //重写
	{
		cout << "Abnormal" << endl;
	}
};

int main()
{
	CPerson* A = new CPerson_A;
	A->Eat();

	CPerson* B = new CPerson_B;
	B->Eat();

	system("pause");
	return 0;
}
```

