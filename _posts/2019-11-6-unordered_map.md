---
layout: post
title:  "unordered_map的使用方法与场景"
categories: Cpp
tags: 效率
---

* content
{:toc}

## std::unordered_map的定义与特性

* 关联性：std::unorederd_map是一个关联容器，其中的元素根据键来引用，而不是根据索引来引用。

* 无序性：在内部，std::unordered_map中的元素不会根据其键值或映射值按任何特定顺序排序，而是根据其哈希值组织到桶中，以允许通过键值直接快速访问各个元素（常量的平均时间复杂度）。

* 唯一性：std::unorederd_map中的元素的键是唯一的。

关联容器的特点：元素之间有映射关系，读取、查找效率很高。
序列容器有：链表、数组、栈、队列等等（元素一个一个往里放，没关系）。

类型成员|定义
--|:--:
key_type|第一个模板参数（Key）
mapped_type|第二个模板参数（T）
value_type|pair<const key_type,mapped_type>
hasher|第三个模板参数（Hash）
key_equal|第四个模板参数（Pred）

## 无序表的应用

例如，罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

字符|数值
--|:--:
I|1
V|5
X|10
L|50
C|100
D|500
M|1000

罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

```markdown 
    I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
    X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
    C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
```

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

代码如下：

```markdown
    int romanToInt(string s) 
    {      
        unordered_map<string, int> m = {{"I", 1}, {"IV", 3}, {"IX", 8}, {"V", 5}, {"X", 10}, {"XL", 30}, {"XC", 80}, {"L", 50}, {"C", 100}, {"CD", 300}, {"CM", 800}, {"D", 500}, {"M", 1000}};
        int r = m[s.substr(0, 1)];
        for(int i=1; i<s.size(); ++i)
        {
            string two = s.substr(i-1, 2);
            string one = s.substr(i, 1);
            r += m[two] ? m[two] : m[one];
        }
        return r;
```

对于这种问题，应该及时想到使用无序表，如哈希表等类型容器。可以将键与值一一对应，查找效率极高！
