---
layout: post
title:  "设计模式：Singleton 模式 & C++ 实现"
---

* content
{:toc}

## 设计模式

**设计模式**是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性、程序的重用性。

设计模式，针对于面向对象的语言，解决其中某一类问题的一些固定方法。即类如何架构，类中应该装什么东西。

设计模式是对面向对象设计中反复出现的问题的解决方案。算法不是设计模式，因为算法致力于解决问题而非设计问题。设计模式通常描述了一组相互紧密作用的类与对象。设计模式提供一种讨论软件设计的公共语言，使得熟练设计者的设计经验可以被初学者和其他设计者掌握。

<!-- more --> <!-- 摘要预览与正文的分隔符 -->

设计模式之于面向对象系统的设计和开发的作用就有如数据结构之于面向过程开发的作用一般。

面向对象系统的分析和设计实际上追求的就是两点，一是**高内聚**，二是**低耦合**。这也是软件设计所准求的，因此无论是封装、继承、多态，还是设计模式的原则和实例都是在为了这两个目标努力着、贡献着。

高内聚低耦合是判断软件设计好坏的标准，主要用于程序的面向对象的设计，主要看类的内聚性是否高，耦合度是否低。目的是使程序模块的可重用性、移植性大大增强。通常程序结构中各模块的内聚程度越高，模块间的耦合程度就越低。内聚是从功能角度来度量模块内的联系，一个好的内聚模块应当恰好做一件事，它描述的是模块内的功能联系；耦合是软件结构中各模块之间相互连接的一种度量，耦合强弱取决于模块间接口的复杂程度、进入或访问一个模块的点以及通过接口的数据多少，降低模块间的耦合度能减少模块间的影响，防止对某一模块修改所引起的“牵一发动全身”的水波效应。

《设计模式》一书由 GoF（Gang of Four，四人组）完成，这本书的最大部分是一个目录，该目录列举并描述了 23 种设计模式。另外，近来这一清单又增加了一些类别，最重要的是使涵盖范围扩展到更具体的问题类型。而设计模式入门，应从 AbstractFactory 模式、Adapater 模式、Composite 模式、Decorator 模式、Factory 模式、Observer 模式、Strategy 模式、Template 模式等 GoF 列出的模式，以及 Singleton 模式、Facade 模式和 Bridge 模式等模式开始入手。

---

## 单例模式 & C++ 实现

Singleton 模式可能是设计模式中最简单的一个模式，也就是如何去创建一个唯一的对象。

实现代码如下：

```c++
#include <iostream>
using namespace std;

class CPerson   
{
private:
	static bool bflag;   // 标志位
private:
	CPerson()  
	{

	}
	CPerson(const CPerson& ps) 
	{
	
	}
	~CPerson() 
	{
		bflag = false;
	}

public:
	static CPerson* GetObject()  // 获取对象
	{
		if (bflag == false)
		{
			bflag = true;
			return new CPerson;
		}
		else
			return NULL;
	}
	static void DestroyObject(CPerson* &ps)  // 销毁对象
	{
		delete ps;
		ps = 0;
	}
};
bool CPerson::bflag = false;

int main()
{
	CPerson* pp1 = CPerson::GetObject(); 

	CPerson* pp2 = CPerson::GetObject();   // 定义对象失败
	
	CPerson::DestroyObject(pp1); 
	
	CPerson* pp3 = CPerson::GetObject(); 
	
	system("pause");
	return 0;
}
```

