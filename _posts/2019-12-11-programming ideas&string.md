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

## String类

STL：

> STL的含义：标准模板库
> <br/> STL的内容：
> <br/> &#8195;容器：数据的仓库
> <br/> &#8195;算法：与数据结构相关的算法、通用的算法（和数据结构无关）
> <br/> 注：熟悉常用的算法sort reverse
> <br/> &#8195;迭代器：算法和容器的连接
> <br/> &#8195;适配器：类似于转接线，苹果线要连接安卓线

容器：

> 序列式容器（线性结构）
> <br/> string：
> <br/> array：C11静态顺序表
> <br/> vector：动态顺序表
> <br/> list：带头节点的双向循环链表
> <br/> deque：动态二维数组
> <br/> forward_list：带头结点的循环单链表
> <br/> stack：栈
> <br/> queue：队列

    String类：按照类的方式进行动态管理字符串

    底层：是一种顺序表的结构，元素是char类型的字符

    string类的常用构造函数：

        string  str——构造空的string类对象，即空字符串
        string str(str1)——str1 和 str 一样
        string  str("ABC")——等价于 str="ABC"
        string  str("ABC",strlen)——等价于 "ABC" 存入 str 中，最多存储 strlen 个字节
        string  str("ABC",stridx,strlen)——等价于 "ABC" 的stridx 位置，作为字符串开头，存到str中，最多存储 strlen 个字节
        string  str(srelen,'A')——存储 strlen 个 'A' 到 str 中

        //用法小实例
        #include<iostream>
        using namespace std;
         
        int main() {
        	string s1;   //空字符串
        	string s2(10,'$');  //十个 $
        	string s3("hello world");  //hello word
         
        	cout << s3.size << endl;
        	cout << s3.length << endl; //都是求字符串长度  11
        	cout << s3.capacity << endl; //求s3的容量
         
        	s3.clear();  //清空
        	string s4(s3); //s4 和 s3一样
        }

    注：使用string类时，必须包含头文件以及using namespace std。
    string常用成员函数：

    assign函数：

        str.assign("ABC")——清空字符串，并设置为 "ABC"
        str.assign("ABC",2)——清空字符串，并设置为"AB"，保留两个字符
        str.assign("ABC",1,1)——清空字符串，设置为 "ABC" 中的从 位置1 开始，保留 1个 字符
        str.assign(5，'A')——清空字符串，然后字符串设置为 5个 'A'

        str.length()——求字符串长度
        str.size()——和 length() 一样
        str.capacity()——获取容量，包含了不用增加内存就能使用的字符数
        str.reasize(10)——设置当前 str 的大小为10，若大小大与当前串的长度，\0 来填充
        str.reasize(10,char c)——设置当前 str 的大小为10，若大小大与当前串的长度，字0符c 来填充
        str.reserve(10)——设置str的容量 10，不会填充数据
        str.swap(str1)——交换 str1 和 str 的字符串
        str.push_back('A')——在str末尾添加一个字符  'A' ，参数必须是字符形式
        str.append("ABC")——在str末尾添加一个字符串 "ABC"，参数必须是字符串形式

    insert函数方法： 

        str.insert(2,3,'A')——在str下标为2的位置添加 3个 字符'A'
        str.insert(2,"ABC")——在str下标为2的位置添加 字符串 "ABC"
        str.insert(2,"ABC",1)——在str下标为2的位置添加 字符串 "ABC" 中 1个 字符
        str.insert(2,"ABC",1,1)——在str下标为2的位置添加 字符串 "ABC" 中从位置 1 开始的 1 个字符

    注：上个函数参数中加粗的 1 ，可以是 string::npos，这时候最大值，从 位置1 开始后面的全部字符

        str.insert( iterator pos, size_type count, CharT ch )——在 str 中，迭代器指向的 pos位置 插入 count个 字符 ch

    s4.insert(++str1.begin(),2,'a');        结果：s4：ABCD    ->    AaaBCD

        str.insert( iterator pos, InputIt first, InputIt last )——在 str 中，pos位置 插入 str1 的 开始位置 到 结束为止

    s4.insert(s4.begin(),str1.begin(),str1.end());        结果：s4：ABCD  str1：abc   ->    abcABCD

        str.erase(2)——删除 下标2 的位置开始，之后的全删除
        str.erase(2,1)——删除 下标2 的位置开始，之后的 1个 删除
        str.clear()——删除 str 所有
        str.replace(2,4,"abcd")——从 下标2 的位置，替换 4个字节 ，为"abcd"
        str.empty()——判空

    反转相关：

    (位于头文件<algorithm>)

        reverse(str.begin(),str.end())——str的开始 到 结束字符反转 

    reverse(s4.begin,s4.end);        结果：s4：ABCD   ->    DCBA
    查找相关：

    查找成功返回位置 ，查找失败，返回-1

    find函数：从头查找

        str.find('A')——查找 'A'
        str.find("ABC")——查找 "ABC"

    int n=s4.find("ABC");   s4：ABCD    ->   n = 0

        str.find('B',1)——从 位置1 处，查找'B'
        str.find("ABC",1,2)——从 位置1 处，开始查找 'ABC' 的前 2个 字符

    rfind函数：从尾部查找

        str.rfind('A')——查找 'A'
        str.rfind("ABC")——查找 "ABC"

    int n=s4.rfind("ABC");   s4：AAAABCD    ->   n = 3

        str.rfind('B',1)——从 位置1 处，向前查找'B'
        str.rfind("ABC",1,2)——从 位置1 处，开始向前查找 'ABC' 的前 2个 字符

    find_first_of()函数：

    查找是否包含有子串中任何一个字符

        str.find_first_of("abBc")——查找 "abBc" 和str 相等的任何字符，"abBc" 中有就返回位置
        str.find_first_of("abBc",1)——查找 "abBc" 和str 相等的任何字符，从 位置1 处，开始查找"abBc" 中的字符，"abBc" 中有的就返回位置
        str.find_first_of("abBc",1,2)——查找 "abBc" 和str 相等的任何字符，从 位置1 处，开始查找"abBc" 的前 2个 字符，"abBc" 中有的就返回位置

    find_last_of()函数：

    find_first_not_of ()末尾查找, 从末尾处开始,向前查找是否包含有子串中任何一个字符

        str.find_last_of("abBc")——查找 "abBc" 和str 相等的任何字符，向前查找，"abBc" 中有的返回位置
        str.find_last_of("abBc",1)——查找 "abBc" 和str 相等的任何字符，从 位置1 处，开始向前查找"abBc" 中的字符，"abBc" 中有的就返回位置
        str.find_last_of("abBc",10,2)——查找 "abBc" 和str 相等的任何字符，从 位置10 处，开始向前查找"abBc" 的前 2个 字符，"abBc" 中有的就返回位置

    拷贝相关的：

        str1=str.substr(2)——提取子串，提取出 str 的 下标为2 到末尾，给 str1
        str1=str.substr(2,3)——提取子串，提取出 str 的 下标为2 开始，提取三个字节，给 str1
        const char* s1=str.data()——将string类转为字符串数组，返回给s1

    char* s=new char[10]

        str.copy(s,count,pos)——将 str 里的 pos 位置开始，拷贝 count个 字符,存到 s 里

    比较相关的函数：

    compare函数：完全相等返回0；完全不等返回小于0；部分相等返回大于0

    示例对象：string str(“abcd”)

        str.compare(“abcd”)——完全相等，返回0
        str.compare(“dcba”)——返回一个小于0的值
        str.compare(“ab”)——返回大于0的值
        str.compare(str)——相等
        str.compare(0,2,str,2,2)——用str的 下标0 开始的 2个字符 和 str的 下标2 开始的 2个 字符比较——就是用 "ab" 和 "cd”" 比较，结果小于零
        str.compare(1,2,”bcx”,2)——用str的 下标1 开始的 2个字符 和 "bcx"的 前 2个 字符比较——就是用 "bc" 和 "bc”" 比较，结果是零
