---
layout: post
title:  "链表类的实现"
---

* content
{:toc}

## 链表类的实现

链表在实际项目应用中相当重要，而将链表嵌入类中则可以较为方便地对链表进行一系列操作。所以记录一下链表类的实现，以防自己以后忘了。

主要是对于链表类的定义与实现，以及相关的单元测试。

废话不多说，直接上代码好了。

```c++
/*链表类*/
/*将单链表嵌入类中*/

#include <iostream>
using namespace std;

/***************************************类**********************************************/
class CList
{
private:
	/*****************链表********************/
	struct Node
	{
		int nValue;    //1、整型成员
		Node* pNext;   //2、下一节点的地址
	};
	/****************************************/
	Node* m_pHead;  //链表头
	Node* m_pEnd;   //链表尾
	int m_nSize;    //链表长度
public:
	CList()  //构造函数
	{
		m_pHead = 0;
		m_pEnd = 0;
		m_nSize = 0;
	}
public:
	~CList()  //析构函数
	{
		//删除链表的所有节点
		Node* pDel = 0;
		while (m_pHead)  //链表不为空
		{
			pDel = m_pHead; //标记要删除的节点
                                        //将链表头地址变为链表头后的一个地址      
                                        //因为pNext是链表内的参数，不能直接调用，所以要通过链表的指针来调用
			m_pHead = m_pHead->pNext;   
			delete pDel;   //删除标记   //删除此指针所指地址的内容
			pDel = 0;
		}
	}
public:
	void Push_Back(int nV)  //添加节点（尾部）
	{
		//创建一个节点
		Node* node = new Node;
		node->nValue = nV;
		node->pNext = 0;

		//放到尾部  
		if (m_pHead == 0)  //若链表是空的，则头就是尾
		{
			m_pHead = node;
			m_pEnd = node;
		}
		else    //添加链表节点，最好从尾开始添                   
		{
			m_pEnd->pNext = node;   //旧的尾的下一节点的地址设为node，这样便产生新的尾
			m_pEnd = node;          //新的尾的地址设为node
		}
		m_nSize++;
	}
	void Pop_Front()   //删除节点（头部）
	{
		if (m_pHead == 0)  //若链表是空的，则没有节点
			return;
		else if (m_pHead == m_pEnd)   //只有一个节点
		{
			delete m_pHead;
			m_pHead = 0;
			m_pEnd = 0;
			m_nSize = 0;
		}
		else //删除链表节点，最好从头开始删。
                     //因为从头开始可以依次往下读到下一节点的地址，但无法从尾开始依次读到上一节点的地址
                     //所以此处若要删除一个节点，则会选择删除链表头的节点
		{         
			Node* pDel = m_pHead;          
			m_pHead = m_pHead->pNext;
			delete pDel;
			pDel = 0;
			m_nSize--;
		}
	}
	void Show()  //打印
	{
		Node* pTemp = m_pHead;
		while (pTemp)
		{
			cout << pTemp->nValue << " ";
			pTemp = pTemp->pNext;  //其地址从头开始，依次往后替换成下一节点的地址
		}
		cout << "Size:" << m_nSize << endl;
	}
};
/***************************************************************************************/

/**************************************单元测试******************************************/
int main()
{
	CList aa;
	aa.Push_Back(1);
	aa.Push_Back(2);
	aa.Push_Back(3);
	aa.Push_Back(4);
	aa.Show();

	aa.Pop_Front();
	aa.Show();

	aa.Push_Back(5);
	aa.Show();

	system("pause");   
	return 0;
}
/***************************************************************************************/
```

上述对链表的操作，只涉及到在尾部添加节点，以及在头部删除节点。

当然也可以在任意位置添加节点。在链表中第 num 个位置之**后**插入新的数据元素 nV（不能在头节点之前插入），代码如下：

```c++
void Push(int nV, int num)   
	{
		int n = 1;
		Node* node_flag = m_pHead; //用来寻找位置的节点 

		while (node_flag && n < num)   //寻找第num个节点
		{
			node_flag = node_flag->pNext;
			++n;
		}
		if (!node_flag || n > num)  //第num个元素不存在
			return;
		else if (node_flag == m_pEnd)  //第num个元素为末尾元素
		{
			Node* node = new Node;     //要插入的节点 
			node->nValue = nV;
            		node->pNext = 0;
			m_pEnd->pNext = node;
			m_pEnd = node;
			m_nSize++;
		}
		else  //第num个元素为中间元素及起始元素
		{
			Node* node = new Node;     //要插入的节点 
			node->nValue = nV;
            		node->pNext = node_flag->pNext;
			node_flag->pNext = node;
			m_nSize++;
		}
	}
```

当然也可以在任意位置删除节点。删除链表中第 num 个位置的数据元素，代码如下：

```c++
void Pop(int num)
	{
		int n = 1;
		Node* node_flag = m_pHead; //用来寻找位置的节点

		if (num == 1)  //第num个元素为起始元素
		{
			Node* pDel = m_pHead;
			m_pHead = m_pHead->pNext;
			delete pDel;
			pDel = 0;
			m_nSize--;
		}
		while (node_flag->pNext && n < num - 1)   //寻找第num个节点
		{
			node_flag = node_flag->pNext;
			++n;
		}
		if (!(node_flag->pNext) || n > num - 1)  //第num个元素不存在
			return;
		else if (node_flag->pNext == m_pEnd)   //第num个元素为末尾元素
		{
			Node* pDel = node_flag->pNext;
			node_flag->pNext = 0;
			m_pEnd = node_flag;
			delete pDel;
			pDel = 0;
			m_nSize--;
		}
		else   //第num个元素为中间元素
		{
			Node* pDel = node_flag->pNext;
			node_flag->pNext = pDel->pNext;
			delete pDel;
			pDel = 0;
			m_nSize--;
		}
	}
```

注意，以上所有的写法，链表头 m_pHead 中也存有数据，并且将链表头 m_pHead 作为了第一个元素。
