=================
reStructuredText
=================
这种标记语言有点难懂

Structure
==============

head title
>>>>>>>>>>>
章节头部由下线(也可有上线)和包含标点的标题 组合创建, 其中下线要至少等于
标准文本的长度。
可以表示标题的符号有 =、-、`、:、'、"、~、^、_ 、* 、+、 #、<、> 。
对于相同的符号，有上标是一级标题，没有上标是二级标题。
标题最多分六级，可以自由组合使用。
全加上上标或者是全不加上标，使用不同的 6 个符号的标题依次排列，则会依次生成的标题为H1-H6。

paragraph
>>>>>>>>>>>
段落是被空行分割的文字片段，左侧必须对齐（没有空格，或者有相同多的空格）。
缩进的段落被视为引文。

    这是引文
        demo
            demodemo

list
========

Bullet Lists
^^^^^^^^^^^^^^
符号列表可以使用 -、 *、+ 来表示,不同的符号结尾需要加上空行，
下级列表需要有空格缩进。

- list 1

    + list1.1

    - list1.1
- list 2
    Note that a blank line is required before the first item
    and after the last, but is optional between items.

Enumerated Lists
^^^^^^^^^^^^^^^^^
阿拉伯数字: 1, 2, 3, ... (无上限)。
大写字母: A-Z。
小写字母: a-z。
大写罗马数字: I, II, III, IV, ..., MMMMCMXCIX (4999)。
小写罗马数字: i, ii, iii, iv, ..., mmmmcmxcix (4999)。


可以为序号添加前缀和后缀，下面的是被允许的。前后缀区分层级

. 后缀: "1.", "A.", "a.", "I.", "i."。
() 包起来: "(1)", "(A)", "(a)", "(I)", "(i)"。
) 后缀: "1)", "A)", "a)", "I)", "i)"


枚举列表可以结合 # 自动生成枚举序号。

1. list1
#. list2
    (a) list1.1
    (#) list1.2
#. list3
    a) list3.1
        (a) list3.2

Definition Lists
^^^^^^^^^^^^^^^^^
条目占一行，解释文本要有缩进；多层可根据缩进实现。
定义1
    demo
定义2
    demodemo
    demodemo

Option Lists
^^^^^^^^^^^^^
-a              command-line option "a" 
-b file         options can have arguments and long descriptions
--long          options can be long also 
--input=file    long options can also have
                arguments
/V              DOS/VMS-style options too

Field Lists
^^^^^^^^^^^^
:Authors:
    Tony J. (Tibs) Ibbs,
    David Goodger
    (and sundry other good-natured folks)

:Version: 1.0 of 2001/08/08
:Dedication: To my father.

Blocks
========

Literal Blocks
^^^^^^^^^^^^^^^^^
文字块就是一段文字信息，在需要插入文本块的段落后面加上 ::，接着一个空行，然后就是文字块了。
文字块不能定顶头写，要有缩进，结束标志是，新的一段文本贴开头，即没有缩进。
A paragraph containing only two colons indicates that the following indented or quoted
text is a literal block.

::

    Whitespace, newlines, blank lines, and
    all kinds of markup (like *this* or
    \this) is preserved by literal blocks.

    The paragraph containing only '::'
    will be omitted from the result.

The ``::`` may be tacked onto the very end of any paragraph. The ``::`` will be
omitted if it is preceded by whitespace.The ``::`` will be converted to a single
colon if preceded by text, like this::
    It's very convenient to use this form.

Literal blocks end when text returns to the preceding paragraph's indentation.
This means that something like this is possible::

    We start here and continue here and end here.

Per-line quoting can also be used on unindented literal blocks::

> Useful for quotes from email and
> for Haskell literate programming.

Line Blocks
^^^^^^^^^^^^
行块对于地址、诗句以及无装饰列表是非常有用的。行块是以 | 开头，
每一个行块可以是多段文本。

| 这是一段行块内容
| 这同样也是行块内容
    还是行块内容

Doctest Blocks
^^^^^^^^^^^^^^^^
Doctest blocks are interactive
Python sessions. They begin with
"``>>>``" and end with a blank line.

>>> print "This is a doctest block."
This is a doctest block.



------------

Reference:

http://docutils.sourceforge.net/docs/user/rst/quickref.html#literal-blocks
https://www.jianshu.com/p/1885d5570b37
