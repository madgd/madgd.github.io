---
title: 学习Markdown语法
date: 2016-05-24 11:18:09
tags:
---
## Markdown学习笔记

### 基本语法

    >Markdown是一种轻量级的标记语言，它允许人们用易读易写的纯文本格式编写文档，然后转换成丰富的HTML页面。 ——[维基百科](http://zh.wikipedia.org/wiki/Markdown)

>Markdown是一种轻量级的标记语言，它允许人们用易读易写的纯文本格式编写文档，然后转换成丰富的HTML页面。 ——[维基百科](http://zh.wikipedia.org/wiki/Markdown)

    标记文字为**粗体**或者*斜体*。创建[链接](http:\\madgd.github.io)或者一个[跳转](#jump)。段内`高亮`。
标记文字为**粗体**或者*斜体*。创建[链接](http:\\madgd.github.io)或者一个[跳转](#jump)。段内`高亮`。

    **段落缩进**：

    单个回车
    视为空格。

    连续回车

    才能分段。

    行尾加两个空格，这里->  
    即可段内换行。

    全角空格可以缩进

    　缩进一个空格

    　　缩进两个空格

    　　　缩进三个空格

**段落缩进**：

单个回车
视为空格。

连续回车

才能分段。

行尾加两个空格，这里->  
即可段内换行。

全角空格可以缩进

　缩进一个空格

　　缩进两个空格

　　　缩进三个空格

<span id = "jump">跳到这里
**:)**
</span>

    **删除线**：

    ~~删除线~~

    **分割线**：

    ---

**删除线**：

~~删除线~~

**分割线**：

---

### 常用功能

    图像和链接非常类似，区别在开头加一个惊叹号： ![这是一个Logo图像](http://www.turingbook.com/Content/img/Turing.Gif)
图像和链接非常类似，区别在开头加一个惊叹号： ![这是一个Logo图像](http://www.turingbook.com/Content/img/Turing.Gif)



**单行长文字**：

类似代码块，用4个空格开始

    长文字长文字长文字长文字长文字长文字长文字长文字长文字长文字长文字长文字文字长文字长文字长文字长文字长文字长文字长文字长文字长文字文字长文字长文字长文字长文字长文字长文字长文字长文字长文字文字长文字长文字长文字长文字长文字长文字长文字长文字长文字文字长文字长文字长文字长文字长文字长文字长文字长文字长文字文字长文字长文字长文字长文字长文字长文字长文字长文字长文字

***
    **代码块**：
    ```python
    def mark():
        if a in set:
            print a
    ```
**代码块**：
```python
def mark():
    if a in set:
        print a
```     

***
    **表格**：

    | Item      |    Value | Qty  |
    | :-------- | --------:| :--: |
    | Computer  | 1600 USD |  5   |
    | Phone     |   12 USD |  12  |
    | Pipe      |    1 USD | 234  |

**表格**：

| Item      |    Value | Qty  |
| :-------- | --------:| :--: |
| Computer  | 1600 USD |  5   |
| Phone     |   12 USD |  12  |
| Pipe      |    1 USD | 234  |

    **有序序列**：

    1. 第一
    2. 第二
    3. 第三

    **无序序列**：
    + 路飞
      * 娜美
      - 罗宾
        + 山治
        + 索隆

    **复选框**：
    - [x] 已完成事项
    - [ ] 待办事项1
    - [ ] 待办事项2

**有序序列**：

1. 第一
2. 第二
3. 第三

**无序序列**：
+ 路飞
  * 娜美
  - 罗宾
    + 山治
    + 索隆

**复选框**：
- [x] 已完成事项
- [ ] 待办事项1
- [ ] 待办事项2

***

### 参考：

1. [Markdown入门学习小结](http://www.jianshu.com/p/21d355525bdf)
2. [Markdown，你只需要掌握这几个](https://www.zybuluo.com/AntLog/note/63228)
3. [Markdown语法说明(简体中文版)](http://wowubuntu.com/markdown/basic.html)
4. [图灵社区:阅读：怎样使用Markdown](http://www.ituring.com.cn/article/23)
