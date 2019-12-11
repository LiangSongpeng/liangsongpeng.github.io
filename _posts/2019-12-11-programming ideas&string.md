---
layout: post
title:  "编程时应多思考 & String 类"
---

* content
{:toc}

## 编程时应多思考

最近在练习算法题时，碰到一个很简单的题，便马上开始狂敲键盘，三下五除二就搞定了。而后才想到更为高效的方法。

例如，给定一个仅包含大小写字母和空格 ' ' 的字符串，返回其最后一个单词的长度。如果不存在最后一个单词，请返回 0 。

这个题，乍一看，想都没想，就是直接从头开始遍历字符串？？？（*我的脑子是被门夹了吗？*）

明明从尾部开始遍历会快很多啊！

实现代码如下：

``` c++
int lengthOfLastWord(string s)
{
    int res = 0;
    if (s.length() == 0)
        return 0;
    for (int i = s.length()-1; i >= 0; i--)
    {
        if (s[i] != ' ')
            res++;
        else
            if (res)
                break;
    }
    return res;
}
```

十分简单的一道题，说明了写代码的时候不要着急，别想都不想就开始写！这样是会吃很多亏的。

---

## String 类

既然上面是用的 String，那么我就再记录一下 String 的相关内容吧。

STL：

> STL的含义：标准模板库
> <br/> 
> <br/> STL 的内容：
> <br/> 
> <br/> &#8195;&#8195; 容器：数据的仓库
> <br/> &#8195;&#8195; 算法：与数据结构相关的算法、通用的算法（和数据结构无关）
> <br/> 注：熟悉常用的算法 sort reverse
> <br/> &#8195;&#8195; 迭代器：算法和容器的连接
> <br/> &#8195;&#8195; 适配器：类似于转接线

容器：

> 序列式容器（线性结构）
> <br/> 
> <br/> &#8195;&#8195; string：字符串
> <br/> &#8195;&#8195; array：C11 静态顺序表
> <br/> &#8195;&#8195; vector：动态顺序表
> <br/> &#8195;&#8195; list：带头节点的双向循环链表
> <br/> &#8195;&#8195; deque：动态二维数组
> <br/> &#8195;&#8195; forward_list：带头结点的循环单链表
> <br/> &#8195;&#8195; stack：栈
> <br/> &#8195;&#8195; queue：队列

String 类：按照类的方式进行动态管理字符串

String 类底层是一种顺序表的结构，元素是 char 类型的字符

> string类的常用构造函数：
> <br/> 
> <br/> &#8195;&#8195; string str——构造空的 string 类对象，即空字符串
> <br/> &#8195;&#8195; string str(str1)——str1 和 str 一样
> <br/> &#8195;&#8195; string str("ABC")——等价于 str="ABC"
> <br/> &#8195;&#8195; string str("ABC",strlen)——等价于 "ABC" 存入 str 中，最多存储 strlen 个字节
> <br/> &#8195;&#8195; string str("ABC",stridx,strlen)——等价于 "ABC" 的 stridx 位置，作为字符串开头，存到 str 中，最多存储 strlen 个字节
> <br/> &#8195;&#8195; string str(srelen,'A')——存储 strlen 个 'A' 到 str 中
> <br/> 
> <br/> 注：使用 string 类时，必须包含头文件以及 using namespace std。
    
string 常用成员函数：

> assign 函数：
> <br/> 
> <br/> &#8195;&#8195; str.assign("ABC")——清空字符串，并设置为 "ABC"
> <br/> &#8195;&#8195; str.assign("ABC",2)——清空字符串，并设置为 "AB"，保留两个字符
> <br/> &#8195;&#8195; str.assign("ABC",1,1)——清空字符串，设置为 "ABC" 中的从 位置1 开始，保留一个字符
> <br/> &#8195;&#8195; str.assign(5，'A')——清空字符串，然后字符串设置为 5 个 'A'
> <br/> 
> <br/> 其他函数：
> <br/> 
> <br/> &#8195;&#8195; str.length()——求字符串长度
> <br/> &#8195;&#8195; str.size()——和 length() 一样
> <br/> &#8195;&#8195; str.capacity()——获取容量，包含了不用增加内存就能使用的字符数
> <br/> &#8195;&#8195; str.reasize(10)——设置当前 str 的大小为10，若大小大于当前串的长度，\0 来填充
> <br/> &#8195;&#8195; str.reasize(10,char c)——设置当前 str 的大小为 10，若大小大于当前串的长度，字符 c 来填充
> <br/> &#8195;&#8195; str.reserve(10)——设置 str 的容量 10，不会填充数据
> <br/> &#8195;&#8195; str.swap(str1)——交换 str1 和 str 的字符串
> <br/> &#8195;&#8195; str.push_back('A')——在 str 末尾添加一个字符  'A' ，参数必须是字符形式
> <br/> &#8195;&#8195; str.append("ABC")——在 str 末尾添加一个字符串 "ABC"，参数必须是字符串形式
> <br/> 
> <br/> insert 函数方法：
> <br/> 
> <br/> &#8195;&#8195; str.insert(2,3,'A')——在 str 下标为 2 的位置添加 3 个字符'A'
> <br/> &#8195;&#8195; str.insert(2,"ABC")——在 str 下标为 2 的位置添加字符串 "ABC"
> <br/> &#8195;&#8195; str.insert(2,"ABC",1)——在 str 下标为 2 的位置添加字符串 "ABC" 中 1 个字符
> <br/> &#8195;&#8195; str.insert(2,"ABC",1,1)——在 str 下标为 2 的位置添加字符串 "ABC" 中从位置 1 开始的 1 个字符
> <br/> &#8195;&#8195; str.insert( iterator pos, size_type count, CharT ch )——在 str 中，迭代器指向的 pos 位置插入 count 个字符 ch
> <br/> &#8195;&#8195; str.insert( iterator pos, InputIt first, InputIt last )——在 str 中，pos 位置插入 str1 的开始位置到结束为止
> <br/> 
> <br/> 其他函数：
> <br/> 
> <br/> &#8195;&#8195; str.erase(2)——删除下标 2 的位置开始，之后的全删除
> <br/> &#8195;&#8195; str.erase(2,1)——删除下标 2 的位置开始，之后的 1 个删除
> <br/> &#8195;&#8195; str.clear()——删除 str 所有
> <br/> &#8195;&#8195; str.replace(2,4,"abcd")——从下标 2 的位置，替换 4 个字节，为"abcd"
> <br/> &#8195;&#8195; str.empty()——判空

反转相关函数：

> (位于头文件<algorithm>)
    > <br/> 
> <br/> &#8195;&#8195; reverse(str.begin(),str.end())——str 的开始到结束字符反转 
    
查找相关函数：

> 查找成功返回位置 ，查找失败，返回 -1
> <br/> 
> <br/> find函数：从头查找
> <br/> 
> <br/> &#8195;&#8195; str.find('A')——查找 'A'
> <br/> &#8195;&#8195; str.find("ABC")——查找 "ABC"
> <br/> &#8195;&#8195; str.find('B',1)——从位置 1 处，查找 'B'
> <br/> &#8195;&#8195; str.find("ABC",1,2)——从位置 1 处，开始查找 'ABC' 的前 2 个字符
> <br/> 
> <br/> rfind函数：从尾部查找
> <br/> 
> <br/> &#8195;&#8195; str.rfind('A')——查找 'A'
> <br/> &#8195;&#8195; str.rfind("ABC")——查找 "ABC"
> <br/> &#8195;&#8195; str.rfind('B',1)——从位置 1 处，向前查找 'B'
> <br/> &#8195;&#8195; str.rfind("ABC",1,2)——从位置 1 处，开始向前查找 'ABC' 的前 2 个字符
> <br/> 
> <br/> find_first_of() 函数：
> <br/> 查找是否包含有子串中任何一个字符
> <br/> 
> <br/> &#8195;&#8195; str.find_first_of("abBc")——查找 "abBc" 和 str 相等的任何字符，"abBc" 中有就返回位置
> <br/> &#8195;&#8195; str.find_first_of("abBc",1)——查找 "abBc" 和 str 相等的任何字符，从位置 1 处，开始查找 "abBc" 中的字符，"abBc" 中有的就返回位置
> <br/> &#8195;&#8195; str.find_first_of("abBc",1,2)——查找 "abBc" 和 str 相等的任何字符，从位置 1 处，开始查找 "abBc" 的前 2 个字符，"abBc" 中有的就返回位置
> <br/> 
> <br/> find_last_of() 函数：
> <br/> find_first_not_of() 末尾查找，从末尾处开始，向前查找是否包含有子串中任何一个字符
> <br/> 
> <br/> &#8195;&#8195; str.find_last_of("abBc")——查找 "abBc" 和 str 相等的任何字符，向前查找，"abBc" 中有的返回位置
> <br/> &#8195;&#8195; str.find_last_of("abBc",1)——查找 "abBc" 和 str 相等的任何字符，从位置 1 处，开始向前查找 "abBc" 中的字符，"abBc" 中有的就返回位置
> <br/> &#8195;&#8195; str.find_last_of("abBc",10,2)——查找 "abBc" 和 str 相等的任何字符，从位置 10 处，开始向前查找 "abBc" 的前 2 个字符，"abBc" 中有的就返回位置

拷贝相关的函数：

> &#8195;&#8195; str1=str.substr(2)——提取子串，提取出 str 的下标为 2 到末尾，给 str1
> <br/> &#8195;&#8195; str1=str.substr(2,3)——提取子串，提取出 str 的下标为 2 开始，提取三个字节，给 str1
> <br/> &#8195;&#8195; const char* s1=str.data()——将 string 类转为字符串数组，返回给 s1
> <br/> char* s = new char[10]
> <br/> &#8195;&#8195; str.copy(s,count,pos)——将 str 里的 pos 位置开始，拷贝 count 个字符,存到 s 里

比较相关的函数：

> compare函数：完全相等返回 0；完全不等返回小于 0；部分相等返回大于 0
> <br/> 
> <br/> &#8195;&#8195; str.compare(“abcd”)——完全相等，返回 0
> <br/> &#8195;&#8195; str.compare(“dcba”)——返回一个小于 0 的值
> <br/> &#8195;&#8195; str.compare(“ab”)——返回大于 0 的值
> <br/> &#8195;&#8195; str.compare(str)——相等
> <br/> &#8195;&#8195; str.compare(0,2,str,2,2)——用 str 的下标 0 开始的 2 个字符和 str 的 下标 2 开始的 2 个字符比较——就是用 "ab" 和 "cd" 比较，结果小于零
> <br/> &#8195;&#8195; str.compare(1,2,”bcx”,2)——用 str 的下标 1 开始的 2 个字符和 "bcx" 的前 2 个字符比较——就是用 "bc" 和 "bc" 比较，结果是零
