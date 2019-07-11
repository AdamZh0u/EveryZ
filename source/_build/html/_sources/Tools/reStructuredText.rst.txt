=================
reStructuredText
=================
这种标记语言有点难懂

Structure
==============

head title
-----------

章节头部由下线(也可有上线)和包含标点的标题 组合创建, 其中下线要至少。
等于标准文本的长度。
可以表示标题的符号有" =、-、`、:、'、"、~、^、_ 、* 、+、 #、<、> "。
对于相同的符号，有上标是一级标题，没有上标是二级标题。
标题最多分六级，可以自由组合使用。
全加上上标或者是全不加上标，使用不同的 6 个符号的标题依次排列，则会依次生成的标题为H1-H6。

paragraph
----------
段落是被空行分割的文字片段，左侧必须对齐（没有空格，或者有相同多的空格）。
缩进的段落被视为引文。

    这是引文
        demo
            demodemo


line
-------

------------


list
========

Bullet Lists
----------------

符号列表可以使用 -、 *、+ 来表示,不同的符号结尾需要加上空行，
下级列表需要有空格缩进。

- list 1

    + list1.1

    - list1.1
- list 2
    Note that a blank line is required before the first item
    and after the last, but is optional between items.

Enumerated Lists
-----------------
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
----------------------
条目占一行，解释文本要有缩进；多层可根据缩进实现。

定义1
    demo
定义2
    demodemo
    demodemo

Option Lists
---------------
-a              command-line option "a"
-b file         options can have arguments and long descriptions
--long          options can be long also
--input=file    long options can also have
                arguments
/V              DOS/VMS-style options too

Field Lists
-------------
:Authors:
    Tony J. (Tibs) Ibbs,
    David Goodger
    (and sundry other good-natured folks)

:Version: 1.0 of 2001/08/08
:Dedication: To my father.

Blocks
========

Literal Blocks
-------------------
文字块就是一段文字信息，在需要插入文本块的段落后面加上 ::，接着一个空行，然后就是文字块了。
文字块不能定顶头写，要有缩进，结束标志是，新的一段文本贴开头，即没有缩进。
A paragraph containing only two colons indicates that the following
indented or quoted text is a literal block.

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
-------------
行块对于地址、诗句以及无装饰列表是非常有用的。行块是以 | 开头，
每一个行块可以是多段文本。

| 这是一段行块内容
| 这同样也是行块内容
    还是行块内容

Doctest Blocks
---------------
Doctest blocks are interactive
Python sessions. They begin with
"``>>>``" and end with a blank line.

>>> print "This is a doctest block."
This is a doctest block.

tables
========

Grid Tables
-----------
网格表中使用的符号有：-、=、|、+。- 用来分隔行， = 用来分隔表头和表体行，
| 用来分隔列，+ 用来表示行和列相交的节点。

Grid table:

+------------+------------+-----------+
| Header 1   | Header 2   | Header 3  |
+============+============+===========+
| body row 1 | column 2   | column 3  |
+------------+------------+-----------+
| body row 2 | Cells may span columns.|
+------------+------------+-----------+
| body row 3 | Cells may  | - Cells   |
+------------+ span rows. | - contain |
| body row 4 |            | - blocks. |
+------------+------------+-----------+

Simple Tables
--------------
=====  =====  ======
    Inputs     Output
------------  ------
A      B      A or B
=====  =====  ======
False  False  False
True   False  True
False  True   True
True   True   True
=====  =====  ======


links
======

External Hyperlink
--------------------
引用/参考(reference)，是简单的形式，只能是一个词语，引用的文字不能带有空格。
这篇文章来自我的Github,请参考 reference_。

.. _reference: https://github.com/SeayXu/

引用/参考(reference)，行内形式，引用的文字可以带有空格或者符号。
这篇文章来自我的Github,请参考 `SeayXu <https://github.com/SeayXu/>`_。

内部超链接|锚点(Internal Hyperlink)
-----------------------------------
更多信息参考 引用文档_

这里是其他内容

.. _引用文档:

这是引用部分的内容

匿名超链接(Anonymous hyperlink)
-------------------------------

词组(短语)引用/参考(phrase reference)，引用的文字可以带有空格或者符号，需要使用反引号引起来。

这篇文章参考的是：`Quick reStructuredText`__。

.. __: http://docutils.sourceforge.net/docs/user/rst/quickref.html

间接超链接(Indirect Hyperlink)
-----------------------------------

间接超链接是基于匿名链接的基础上的，就是将匿名链接地址换成了外部引用名_。

.. _SeayXu: https://github.com/SeayXu/

__ SeayXu_

隐式超链接(Implicit Hyperlink)
------------------------------
小节标题、脚注和引用参考会自动生成超链接地址，使用小节标题、脚注或引用参考名称作为超链接名称就可以生成隐式链接。
其他内容...

隐式链接到 `list`_，即可生成超链接。

替换引用(Substitution Reference)
--------------------------------
替换引用就是用定义的指令替换对应的文字或图片，和内置指令(inline directives)类似。
这是 |logo| github的Logo，我的github用户名是:|name|。

.. |logo| image:: https://help.github.com/assets/images/site/favicon.ico
.. |name| replace:: SeayXu

脚注引用(Footnote Reference)
------------------------------

脚注引用，有这几个方式：有手工序号(标记序号123之类)、自动序号(填入#号会自动填充序号)、自动符号(填入*会自动生成符号)。
手工序号可以和#结合使用，会自动延续手工的序号。
# 表示的方法可以在后面加上一个名称，这个名称就会生成一个链接。

脚注引用一 [1]_
脚注引用二 [#]_
脚注引用三 [#链接]_
脚注引用四 [*]_
脚注引用五 [*]_
脚注引用六 [*]_

.. [1] 脚注内容一
.. [2] 脚注内容二
.. [#] 脚注内容三
.. [#链接] 脚注内容四 链接_
.. [*] 脚注内容五
.. [*] 脚注内容六
.. [*] 脚注内容七

引用参考(Citation Reference)
----------------------------
引用参考与上面的脚注有点类似。

引用参考的内容通常放在页面结尾处，比如 [One]_，Two_

.. [One] 参考引用一
.. [Two] 参考引用二

注释(Comments)
---------------
注释以 .. 开头，后面接注释内容即可，可以是多行内容，多行时每行开头要加一个空格。

..  我是注释内容
    你们看不到我

.. This text will not be shown
    (but, for instance, in HTML might be
    rendered as an HTML comment)

Reference:

http://docutils.sourceforge.net/docs/user/rst/quickref.html#literal-blocks
https://www.jianshu.com/p/1885d5570b37
