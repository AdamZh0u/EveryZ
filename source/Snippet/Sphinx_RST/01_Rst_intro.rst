
Sphinx 默认使用 reStructureText(rst) 标记语言, 要能够处理 Markdown 还需要额外的渲染器, 而且在了解一番过后, 发现 rst 支持的内容比 Markdown 更丰富, 于是决定学习一下. 建议克隆该库, 自己使用 ``make html`` 编译结果, 再对照源码学习 reStructureText 的语法.

rst 和 Python 一样, 很多样式的表达都依赖缩进. (所以你的编辑器上最好有📏2333)

标题
====

x 级标题分别对应 ``<hx>...</hx>``.

rst 中各级标题使用符号衬在文字下一行, 并且, 符号的数目应不少于文字数目. 对于中文等宽字符, 一个字符对应两个普通符号.

注意, rst 并不在意使用的符号类型, 只需要是 "相同符号衬托文字" 就会被解析为标题, 并根据符号的出现顺序与嵌套结构划分标题层级.

一般来讲, 会用以下符号来标注标题层级.

::

    一级标题
    ########

    二级标题
    ********

    三级标题
    ========

    四级标题
    --------

    五级标题
    ^^^^^^^^

    六级标题
    """"""""

实际上, 只有下方衬有字符与上下包裹字符都是一样的. 下面的说法是错误的:

.. error::

    以上是章节标题, 还有一种标题是 "文档标题", 对应 html 标签 ``<title>`` 或 ``<subtitle>``. 和章节标题类似, 文档标题只是用两行相同符号包裹文字. 这个貌似和主题有关, ``sphinx_rst_theme`` 把多余的标题解析成 ``<h7>`` ``<h8>`` 了.

    ::

        ======
        主标题
        ======

        ------
        副标题
        ------

段落
====

这一点 rst 几乎和 md 一样, 都是由空行划分的段落. 只不过, rst 中, 缩进也是控制段落的一个因素, 相同层级的段落, 缩进应当是一样的. 段落的缩进, 会影响渲染后文字的缩进.

这是一个 reStructureText 段落.

这是第二个 reStructureText 段落.

    这个段落被缩进了一下.

段落是被空行分割的文字片段，左侧必须对齐（没有空格，或者有相同多的空格）。
缩进的段落被视为引文。

    这是引文
        demo
            demodemo

line
-------

------------

列表
====

无序列表与有序列表
-----------------------
和 Markdown 的列表标记差不多. 无序列表可以使用 ``*`` ``-`` 等符号, 有序列表则是枚举编号后跟一个点.

* 无序列表第一位
* 无序列表第二位
  也可以换行写, 只需要保持相同的缩进

  * 也可以嵌套, 但是需要空一行, 并且增加一级缩进.

0. 有序列表
1. 有序列表第二项
2. 编号乱跳是不行的, 只能按顺序来. (如果把前面的序号从 2 变成 3 或其他任何不是 2 的数字, 就会报错, 并且不会被解析为列表的下一项, 而是直接解在上一项的后面.)

阿拉伯数字: 1, 2, 3, ... (无上限)。
大写字母: A-Z。
小写字母: a-z。
大写罗马数字: I, II, III, IV, ..., MMMMCMXCIX (4999)。
小写罗马数字: i, ii, iii, iv, ..., mmmmcmxcix (4999)。


可以为序号添加前缀和后缀，下面的是被允许的。前后缀区分层级
#. 自动编号会接在同一缩进的有序列表下, 除非有其他段落隔断.

比如我这里就随便输了一个段落进行隔断.

#. 自动编号

另外, 列表前缀有多种形式可以使用, 例如 拉丁字母(a,b,c...) 罗马字母, 用括号代替点号等.

定义列表
----------------
条目占一行，解释文本要有缩进；多层可根据缩进实现。

定义1
    demo
定义2
    demodemo
    demodemo

选项列表
---------------

选项列表看起来就是为了方便命令行参数帮助的展示而定义的样式.

-a              command-line option "a"
-b file         options can have arguments and long descriptions
--long          options can be long also
--input=file    long options can also have
                arguments
/V              DOS/VMS-style options too

Field 列表
----------

应当用在代码的文档字符串中.

:param arg1: 第一个参数
:param arg2: 第二个参数
:returns: 返回值
:Authors:
    Tony J. (Tibs) Ibbs,
    David Goodger
    (and sundry other good-natured folks)

:Version: 1.0 of 2001/08/08
:Dedication: To my father.
::

    def function(arg1, arg2)
        """
        :param arg1: 第一个参数
        :param arg2: 第二个参数
        :returns: 返回值
        """

块
========

文字块
-------------------
文字块就是一段文字信息，在需要插入文本块的段落后面加上 ::，接着一个空行，然后就是文字块了。文字块不能定顶头写，要有缩进，结束标志是，新的一段文本贴开头，即没有缩进。

::

    空白、换行、空行和各种标记(比如*this* or \this)由文字块保存。

    只包含“::”的段落将从结果中省略。

'':: ''可以附加在任何段落的末尾。
如果''::''前面有空格，就会被省略。'':: ''将被转换成一个冒号，如果前面有文本，就像这样::

    It's very convenient to use this form.

当文本返回到前一段的缩进时，文字块结束。
这意味着像这样的事情是可能的::

    We start here and continue here and end here.

引用也可以用于无缩进的文字块::

> Useful for quotes from email and
> for Haskell literate programming.

行块
-------------
行块对于地址、诗句以及无装饰列表是非常有用的。行块是以 | 开头，每一个行块可以是多段文本。
`| `后加一个空格，可缩进。

| 这是一段行块内容
| 这同样也是行块内容
|   还是行块内容

测试块
---------------
Doctest块是交互式Python会话。它们以` >>> `开头，以空行结束。

>>> print "This is a doctest block."
This is a doctest block.

代码块
-----------

这下面是一个 C 语言的代码块. 只需要一个 ``::`` 符号, 在之后空一行, 并缩进一级后编辑代码. 当缩进结束时, 代表代码块结束. 可以指定代码高亮模式, 默认是  代码的高亮模式.

要指定高亮模式, 应使用 ``code-block`` 指令. code-block 可以指定其他属性, 例如 ``:linenos:`` 显示行号等.

.. code-block:: c

    #include <stdio.h>
    int main()
    {
        printf("Hello\n");
        return 0;
    }

自定义代码高亮
--------------

Sphinx 是调用 pygments 进行语法高亮的.


表格
====

在 VsCode 上编辑表格, 最好下载一个 `Table Formatter <https://marketplace.visualstudio.com/items?itemName=shuworks.vscode-table-formatter>`_ 否则就会被打格式符烦死.

普通表格
--------
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
::

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

网格表格
--------

+------------+----------+
| 网格1      | 网格2    |
+------------+----------+
| 无等宽字体 | 就特别烦 |
+------------+----------+

::

    +------------+----------+
    | 网格1      | 网格2    |
    +------------+----------+
    | 无等宽字体 | 就特别烦 |
    +------------+----------+

超链接
======

参考式
------

参考式链接是在文本中使用链接文本, 将链接地址放在文档其他地方. **链接的地址需要指定协议, 否则会被当做相对路径.**

例如本文档参考了 `从 Markdown 到 reStructureText`_.

引用处, 下划线在后面, 参考处, 下划线在前面。 如果文本中含有空格, 可以使用反引号 ``\``` 将本文包括住。

如果一个链接对应多个文本, 可以这么表示::

    _文本表示1:
    _文本表示2:
    _文本表示最后: https://python.org

这样, ``文本表示1``, ``文本表示2``, ``文本表示最后`` 都对应一个链接.

内联式
------
行内形式，引用的文字可以带有空格或者符号。
这篇文章来自我的Github,请参考 `demo <https://github.com/demo/>`_。

内联式, 是将文本和链接写在一块. 相比参考式, 这更难以管理, 如果有多处引用了该链接, 需要多次输入链接. 但是, 对于那些临时使用的跳转链接, 这种方式还是很合适的.

用尖括号括住之后添加下划线, 或者直接书写链接. Sphinx 会自动将链接文本显示为 url::

    <https://python.org>_

或者使用反引号括住, 在前半部分书写显示文本 `Python 官网 <python.org>`_ ::

    `Python 官网 <https://python.org>`_

自动标题链接
------------

每一个标题, 都会自动生成一个锚点, 可以直接使用标题文本进行链接, 例如 `自动标题链接`_::

    `自动标题链接`_


替换引用(Substitution Reference)
--------------------------------
替换引用就是用定义的指令替换对应的文字或图片，和内置指令(inline directives)类似。
这是 |logo| github的Logo，我的github用户名是:|name|。

.. |logo| image:: https://help.github.com/assets/images/site/favicon.ico
.. |name| replace:: adamCh0u

::

    这是 |logo| github的Logo，我的github用户名是:|name|。
    .. |logo| image:: https://help.github.com/assets/images/site/favicon.ico
    .. |name| replace:: adamCh0u


脚注引用(Footnote Reference)
------------------------------

脚注引用，有这几个方式：有手工序号(标记序号123之类)、自动序号(填入#号会自动填充序号)、自动符号(填入*会自动生成符号)。
手工序号可以和#结合使用，会自动延续手工的序号。
# 表示的方法可以在后面加上一个名称，这个名称就会生成一个链接。
::

    脚注引用一 [1]_
    脚注引用二 [#]_
    脚注引用三 [#链接]_
    脚注引用四 [*]_
    脚注引用五 [*]_

    .. [1] 脚注内容一
    .. [#] 脚注内容三
    .. [#链接] 脚注内容四 链接_
    .. [*] 脚注内容五

尾注 [#f1]_ 和链接用法类似. 源代码中尾注内容可以放在任何位置, 但是引用尾注处必须使用空格与其他文本分开.

使用 ``[#]`` 自动编号. 或者使用 ``[#name]`` 为特定尾注命名::

    尾注 [#fn]_
    
    .. [#fn] 或者叫脚注, footnote.

尾注 [#fn]_

.. [#fn] 或者叫脚注, footnote.

引用参考(Citation Reference)
----------------------------
引用参考与上面的脚注有点类似。

引用参考的内容通常放在页面结尾处，比如 One_，Two_

.. [One] 参考引用一
.. [Two] 参考引用二
::

    One_，Two_
    .. [One] 参考引用一
    .. [Two] 参考引用二



注释(Comments)
---------------
注释以 .. 开头，后面接注释内容即可，可以是多行内容，多行时每行开头要加一个空格。
..  
 我是注释内容
 你们看不到我

:: 

    ..  
     我是注释内容
     你们看不到我

    .. This text will not be shown
        (but, for instance, in HTML might be
        rendered as an HTML comment)

替换语法
========

替换语法中的文本, 会在渲染时自动被定义好的语句替换.

|yufa|::

    |yufa|

    .. |yufa| replace:: 语法

.. |yufa| replace:: 语法


图片
========

Sphinx 使用指令来作为 reStructureText 的扩展. 指令的一大作用, 就是快速添加文档结构, 而无需对底层代码进行修改.

使用 ``image`` 指令. 开头两个点, 空一格, 输入 ``image``, 然后连用两个冒号 ``::`` 再空一格, 输入到图片的路径, 可以使用相对路径或绝对路径, 相对路径是相对于文档文件的. 可以在下面添加属性, 所有属性和 HTML 中的图片属性是一样的.

::

    .. image:: img/59498721_p0.jpg
    :alt: 示例图片


内联样式
========

*斜体* **粗体** ``代码``

::

    *斜体* **粗体** ``代码``


.. _`Docutils 中文文档`: https://docutils-zh-cn.readthedocs.io/zh_CN/latest/
.. _`从 Markdown 到 reStructureText`: https://macplay.github.io/posts/cong-markdown-dao-restructuredtext/#id21
.. [#f1] 尾注的文本最好放在源代码末端, 便于管理