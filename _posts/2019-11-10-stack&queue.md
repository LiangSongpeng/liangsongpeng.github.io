---
layout: post
title:  "栈的特性与应用场景&队列"
---

* content
{:toc}

## 栈 stack

栈的定义: stack<int> s;

在栈中应包含头文件: #include<stack> 

> 栈：先进后出。
> <br/>栈：只能在栈顶进行添加或删除。

栈的标准库函数：

``` c++
s.empty();         //如果栈为空则返回true, 否则返回false;
s.size();          //返回栈中元素的个数
s.top();           //返回栈顶元素, 但不删除该元素
s.pop();           //弹出栈顶元素, 但不返回其值
s.push();          //将元素压入栈顶
```

* 关联性：std::unorederd_map 是一个关联容器，其中的元素根据键来引用，而不是根据索引来引用。

* 无序性：在内部，std::unordered_map 中的元素不会根据其键值或映射值按任何特定顺序排序，而是根据其哈希值组织到桶中，以允许通过键值直接快速访问各个元素（常量的平均时间复杂度）。

* 唯一性：std::unorederd_map 中的元素的键是唯一的。

类型成员|定义
:--:|:--:
key_type|第一个模板参数（Key）
mapped_type|第二个模板参数（T）
value_type|pair<const key_type,mapped_type>
hasher|第三个模板参数（Hash）
key_equal|第四个模板参数（Pred）

---

## 栈 stack 的应用场景

例如，给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

&nbsp;&nbsp;&nbsp;&nbsp;1、左括号必须用相同类型的右括号闭合。
&nbsp;&nbsp;&nbsp;&nbsp;<br/>2、左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

对于这种问题，可以使用栈，先进后出。代码如下：

``` c++
bool isValid(string s) 
{
    if (s.empty()) 
        return true;
    map<char, char> sMap = { {')', '('},
                             {'}', '{'},
                             {']', '['}
                           };
    stack<char> st;
    for (int i = 0; i < s.size(); i++) 
    {
        if (st.empty()) 
            st.push(s[i]);
        else 
        {
            if (sMap[s[i]] != st.top())    //st.top()栈顶，即进行插入或删除操作的一段
                st.push(s[i]);
            else 
                st.pop();    //先进后出
        }
        if (st.size() > s.size() / 2) 
            return false;
    }
    return st.empty();
};
```

还有就是网站的浏览记录“转到上一页”便是用的栈来实现的，想一想感觉还是很有道理，用栈来实现十分方便了。

---

## 队列 queue

既然都讲了栈了，那就顺带讲讲队列吧。

栈的定义: queue<int> q;

包含头文件: #include<queue>

> 队列：先进先出。
> <br/>队列：在队尾进行添加，在队头进行删除，在队中间不能进行操作。

队列的标准库函数：

``` c++
q.empty();         //如果队列为空返回true, 否则返回false     
q.size();          //返回队列中元素的个数
q.front();         //返回队首元素但不删除该元素
q.pop();           //弹出队首元素但不返回其值
q.push();          //将元素压入队列
q.back();          //返回队尾元素的值但不删除该元素
```

队列的用法与栈其实差不多，而比如电子排队买票的系统便是用队列来实现，毕竟先到先得嘛，用队列实现还是十分有道理的。

#### 栈和队列的知识比较基础，还是要记牢啊。
#### <br/>基础不牢，地动山摇！
