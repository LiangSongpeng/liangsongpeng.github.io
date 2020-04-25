---
layout: post
title:  "栈与队列的相互实现"
---

* content
{:toc}

## 用两个栈实现队列

栈：最后被压入（push）栈的元素会第一个被弹出（pop），即后进先出。队列：先进先出。

用两个栈实现队列，首先队列的声明如下：

```c++
template <typename T> class CQueue
{
public:
    CQueue(void);
    ~CQueue(void);   
    void appendTail(const T& node);
    T deleteHead();
private:
    stack<T> stack1;
    stack<T> stack2;
};
```

函数 appendTail 和 deleteHead，分别完成在队列尾部插入结点和在队列头部删除结点的功能。

<!-- more --> <!-- 摘要预览与正文的分隔符 -->

这两个函数的具体实现思路如下：

可以从上述代码看出，一个队列中包含了两个栈 stack1 和 stack2，这个队列是 CQueue。

首先插入一个元素 a，插入 stack1 中，此时 stack1 中的元素有 {a}，stack2 为空。再压入两个元素 b 和 c，还是插入 stack1，此时 stack1 中的元素有 {a, b, c}，其中 c 位于栈顶，而 stack2 仍然为空。

这时从队列中删除一个元素。此时  {a, b, c} 中应该被删除的是 a，但 a 并不在栈顶上，因此不能直接删除。此时应把 stack1 中的元素逐个弹出并压入到 stack2 中，则 stack1 为空，而 stack2 中的元素是 {c, b, a}，这时就可以顺利弹出 stack2 的栈顶 a 了。若还想再删除一个元素，则直接从 stack2 中直接弹出 b 就行。

此时再插入一个元素 d ，还是应将其压入到 stack1 中。接下来想进行插入或删除，就都还是用上述的方法就行。

用两个栈实现队列的功能也就此实现。

具体实现代码如下：

```c++
template<typename T> void CQueue<T>::appendTail(const T& element)
{
    stack1.push(element);
} 

template<typename T> T CQueue<T>::deleteHead()
{
    if(stack2.size()<= 0)
    {
        while(stack1.size()>0)
        {
            T& data = stack1.top();
            stack1.pop();
            stack2.push(data);
        }
    }
    if(stack2.size() == 0)
        throw new exception("queue is empty");   
    T head = stack2.top();
    stack2.pop();    
    return head;
}
```

---

## 用两个队列实现栈

看了一些资料，有些地方说只可以用两个栈实现队列，不可以用两个队列实现栈。但其实仔细想想用两个队列实现栈也是可行的。

用两个队列实现栈。

具体实现思路如下：

一个栈中包含两个队列 queue1 和 queue2。

首先往栈内压入一个元素 a，将其插入 queue1 中，接下来继续往栈内压入 b、c 两个元素，都插入 queue1 中。此时 queue1 中包含三个元素 a、b、c，其中 a 位于队列的头部，c 位于队列的尾部。此时 queue1 中含有元素 a、b、c，queue2 为空。

现在从栈内弹出一个元素，应该被弹出的应该是 c，但c位于 queue1 的尾部，所以应先从 queue1 中依次删除 a、b，并将其插入 queue2 中，再从 queue1 中删除元素 c。这就相当于从栈中弹出元素 c 了。此时 queue1 为空， queue2 中含有元素 a、b。（当然也可以用同样的方法弹出元素 b，即先将 a 从 queue2 转到  queue1 中，再从 queue2 中删除 b。）

此时再插入一个元素 d ，还是应将其压入到 queue2 中，因为此时 queue2 中有元素，queue1 中无元素。若想再从栈内弹出一个元素，那么被弹出的应该是 d，则应先从头删除 queue2 中的元素并插入 queue1，直到在 queue2 中遇到 d 再直接把它删除。

用两个队列实现栈的功能也就此实现。具体代码就不在此处列出了。