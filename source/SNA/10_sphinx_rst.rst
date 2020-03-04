^^^^^^^^^^^^^^^^^^^^^^^
Sphinx
^^^^^^^^^^^^^^^^^^^^^^^
################
Sphinx 简单入门
################ 

Sphinx 是一个用于生成 Python 文档的工具, 但是也可以用来制作电子书. 本文记录使用该工具的一些经验.

首先使用 Sphinx 的自动配置工具
==============================

.. highlight:: none

在准备好的工作目录下, 运行 ``sphinx-quickstart`` 将会弹出一堆文本, 并让你在其中选择要使用的配置, 一般情况下, 只需要手动修改两项, 其他保持默认即可. 让我们来看看 Sphinx 询问了我们哪些问题吧.

::

    Welcome to the Sphinx 1.8.1 quickstart utility.
    Please enter values for the following settings (just press Enter to
    accept a default value, if one is given in brackets).

    Selected root path: .

选择当前工作目录为项目根目录.

::

    You have two options for placing the build directory for Sphinx output.
    Either, you use a directory "_build" within the root path, or you separate
    "source" and "build" directories within the root path.
    > Separate source and build directories (y/n) [n]: y

是否将源文件(使用 .rst 或 .md 标记语言写的文档)和生成文件(html 或 epub, pdf 等)分开放置在不同的目录

::

    Inside the root directory, two more directories will be created; "_templates"
    for custom HTML templates and "_static" for custom stylesheets and other static
    files. You can enter another prefix (such as ".") to replace the underscore.
    > Name prefix for templates and static dir [_]:

对于模板或静态目录(文件不被解析渲染), 设它们的前缀为 `_`.

::

    The project name will occur in several places in the built documentation.
    > Project name: Learn-Sphinx
    > Author name(s): Zombie110year
    > Project release []:

分别是 项目名, 作者名, Project release 是指项目发布版本, 根据实际项目来填.

::

    If the documents are to be written in a language other than English,
    you can select a language here by its language code. Sphinx will then
    translate text that it generates into that language.

    For a list of supported codes, see
    http://sphinx-doc.org/config.html#confval-language.
    > Project language [en]: zh_CN

选择项目语言, 默认是英语, 用 zh_CN 表示简体中文, 可以在上面那个链接看支持的语言以及其表示代码. 这个选项会影响搜索功能, 英文情况下, 会以英语存储搜索索引, 这样就无法使用中文搜索. 搜索功能是用 JavaScript + 字典实现的.

::

    The file name suffix for source files. Commonly, this is either ".txt"
    or ".rst".  Only files with this suffix are considered documents.
    > Source file suffix [.rst]:

文档文件后缀, 只有拥有这些后缀的文件才会被解析, 在当前使用的版本(v1.8.2)中只能接受 .rst 与 .txt 后缀(但都是以 reStructuredText 的语法进行解析). 要解析 Markdown, 需要安装额外的插件 recommonmark, 这个稍后再讲.

::

    One document is special in that it is considered the top node of the
    "contents tree", that is, it is the root of the hierarchical structure
    of the documents. Normally, this is "index", but if your "index"
    document is a custom template, you can also set this to another filename.
    > Name of your master document (without suffix) [index]:

这个是主文件, 对于 html, 就是指 index.html 等能够被浏览器直接默认显示的. 建议保持默认.

接下来就是插件配置. 这里的都是默认插件, 其中 imgmath 和 mathjax 不能同时选.

::

    Indicate which of the following Sphinx extensions should be enabled:
    > autodoc: automatically insert docstrings from modules (y/n) [n]: y
    > doctest: automatically test code snippets in doctest blocks (y/n) [n]: n
    > intersphinx: link between Sphinx documentation of different projects (y/n) [n]: y
    > todo: write "todo" entries that can be shown or hidden on build (y/n) [n]: y
    > coverage: checks for documentation coverage (y/n) [n]: n
    > imgmath: include math, rendered as PNG or SVG images (y/n) [n]: n
    > mathjax: include math, rendered in the browser by MathJax (y/n) [n]: y
    > ifconfig: conditional inclusion of content based on config values (y/n) [n]: y
    > viewcode: include links to the source code of documented Python objects (y/n) [n]: y
    > githubpages: create .nojekyll file to publish the document on GitHub pages (y/n) [n]: y

然后询问是否创建 Makefile 或者 Windows 的批处理脚本, 这是为了方便使用 ``make xxx`` 来构建文档.

::

    A Makefile and a Windows command file can be generated for you so that you
    only have to run e.g. `make html' instead of invoking sphinx-build
    directly.
    > Create Makefile? (y/n) [y]: y
    > Create Windows command file? (y/n) [y]: y

就算在 quickstart 中有选项不满意, 也可以在接下来的 `conf.py` 中修改.

如何规划目录结构
================

在运行了如上的 sphinx-quickstart 程序后, 目录下出现了以下文件/目录:

::

    ├─build
    └─source
        ├─_static
        ├─_templates
        |  conf.py
        |  index.rst
      Makefile

在根目录下设置了 ``Makefile`` 便于使用 make 工具自动构建, 而配置文件和索引则放在了 source 目录下.
如果需要修改文件规划, 那么, 可以在 Makefile 中修改 ``BUILDDIR`` 和 ``SOURCEDIR`` 两项目.

插件介绍
========

官方插件
--------

- autodoc: 自动从模块中抽取 docstring 插入文档
- doctest: 自动测试 doctest
- intersphinx: 链接多个 Sphinx 文档. 需要启用它才能使用 :mod:`os` 这样的语法链接到官方文档
- todo: 写下 todo 在文件头部时, 将不会渲染该文件
- coverage: 检查封面
- imgmath: 将数学公式渲染为 png 或 svg 图像
- mathjax: 使用 Mathjax 渲染数学公式
- ifconfig: 通过配置的条件判断决定文档包含
- viewcode: 将源代码包含进文档项目, 并在 api 文档中创建指向源代码的链接
- githubpages: create .nojekyll file to publish the document on GitHub pages

第三方插件
----------

- :doc:`graphviz`, 可在文档中嵌入 graphviz 代码, 在构建时生成图片
- :doc:`matplotlib`, 在文档中嵌入 matplotlib 代码, 在构建时生成图片

toctree
========

在 source 目录下添加 .rst 文件, 但是如果要在编译项目后从首页 (index.html) 进行访问, 还需要在 index.rst 中将这个文件添加到 ``toctree`` 中. 在原始的 index.rst 中, 应当有如下 toctree.

::

    .. toctree::
       :maxdepth: 2
       :caption: Contents:

要在 toctree 中添加一个文件, 应当在上面那个 toctree 结构下空一行, 添加文件名(不需要扩展)

例如, 有一个 example.rst 就将 toctree 编辑为

::

    .. toctree::
       :maxdepth: 2
       :caption: Contents:

       example

如果, 在 source 目录中, 添加了子目录, 将文档放在子目录里了, 那么, 只需要在原来 example 里面按相对于 index.rst 的路径填就可以了, 例如 /source/text/example.rst 就填:

::

    .. toctree::
       :maxdepth: 2

       text/example

toctree 参数
------------

toctree 下的 ``:maxdepth: 2``, ``:caption: Contents:`` 等就是它的参数, 可以选用的参数有:

- ``:maxdepth: n`` 将目录的标题深度设为 n. 意思是 example 文件为目录的根标题, 在这个标题下, 会建立文件中的 1, 2, ..., n 级标题的索引.
- ``:numbered:`` 给标题自动编号.
- ``:caption: xxx``

更改 html 页面主题
==================

默认的 html 页面看起来并不是很好看, 可以使用 pip 安装 ``sphinx_*_theme`` 等包, 然后在 ``conf.py`` 中引用, 就可以使用更多的主题.

例如 `sphinx_rtd_theme <https://sphinx-rtd-theme.readthedocs.io/en/latest/` 这个受很多人欢迎的主题.

.. code-block:: sh

    # 下载
    pip install sphinx_rtd_theme

::

    # conf.py 中配置
    import sphinx_rtd_theme
    html_theme = 'sphinx_rtd_theme'

在 GitHub Page 上展示文档
=========================

在使用 Sphinx 构建完毕后, 生成的 html 项目可以直接拿来用.

GitHub Page 可以将 master, gh-pages 分支下的根目录或 master 分支的 /doc 目录渲染成页面.

为了方便管理, 可以在 build/html 目录下新建一个 git 仓库, 并重命名为 gh-pages 分支. 将这个分支 push 到 github 的 gh-pages 上, 充当 GitHub Page 的资源. (注意, build 目录应当在根目录下的 .gitignore 中被忽略)

这样, 在项目根目录只需要一个 master 分支, 在这个分支编辑源文件, 然后 ``make html``, ``git add *``, ``git commit``, ``git push``, 之后就进入 ``build/html`` 目录, 再 ``git`` 一通即可. 非常舒服. [#RTD]_

.. [#RTD] https://docs.readthedocs.io/en/stable/config-file/v2.html

---------------------------

#############
rst 基本语法
#############

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

------------------------------------------

########
rst 指令
########

指令语法如下::

    +-------+-------------------------------+
    |  ..   | 指令  ::  主参数              |
    +-------+    :额外参数:                 |
            |                               |
            |    内容                       |
            +-------------------------------+

目录
====

::

    .. toctree::
        :maxdepth: para
        :caption: para
        :numbered:
        :titlesonly:
        :glob:
        :reversed:
        :hidden:
        :includehidden:

- ``:maxdepth: para`` 接受一个参数, 应该为数字, 设置目录树展开的深度.
- ``:caption: para`` 接受一个参数, 为任意字符串, 设置该目录树的标题.
- ``:numbered:`` 为目录自动编号
- ``:titlesonly:`` 只生成文件的一级标题, 不展开子标题. 会覆盖 ``:maxdepth:``
- ``:glob:`` 启用通配符
- ``:reversed:`` 启用通配符时, 反转目录排序.
- ``:hidden:`` 只显示标题, 而不创建超链接.
- ``:includehidden:`` 只创建一级标题的超链接.

警告
====

警告将会显示为特殊的样式. 在 主参数和内容 位置处, 可以编写段落. 这个指令的所有参数都是被显示的内容.

.. danger:: 危险!

    这是一个危险操作.

::

    .. danger:: 危险!

        这是一个危险操作.

.. tip:: 提示

    提示条目

::

    .. tip:: 提示

        提示条目

.. caution:: 小心

    小心, 注意安全

::

    .. caution:: 小心

        小心, 注意安全

.. note:: 注意

    集中注意力

::

    .. note:: 注意

        集中注意力

.. warning:: 警告

    警告条目

::

    .. warning:: 警告

        警告条目

.. important:: 重要

    重要内容

::

    .. important:: 重要

        重要内容

.. seealso::

    参见某某某

::

    .. seealso::

        参见某某某

版本更新
========

.. versionadded:: 0.0.1
    添加了一些内容

::

    .. versionadded:: 0.0.1
        添加了一些内容

.. versionchanged:: 0.0.1
    修改了一些内容

::

    .. versionchanged:: 0.0.1
        修改了一些内容

.. deprecated:: 0.0.1
    某些功能被删除, 使用 某某 代替

::

    .. deprecated:: 0.0.1
        某某 被删除, 使用 某某 代替

文本样式
========

.. rubric:: 一个标题, 但是不计入 toctree

::

    .. rubric:: 一个标题, 但是不计入 toctree

.. centered:: demo

::

    .. centered:: 居中的文本

.. hlist::
    :columns: 4

    - 1
    - 2
    - 3
    - 4
    - 5
    - 6
    - 7

::

    .. hlist::
        :columns: 4

        - 1
        - 2
        - 3
        - 4
        - 5
        - 6
        - 7

图片
====

处理图片可能用到两个指令: ``image`` 和 ``figure``.

image
-----

.. image:: ./00_img/loglogplot.png
    :alt: 响爷
    :height: 100px

::

    .. image:: path/to/image
        :alt: xxx
        :height: xxx
        :width: xxx
        :scale: xxx
        :align: top | middle | bottom | left | center | right
        :target: path/to/target

- ``alt`` : 文本. 替换文本: 当应用无法显示图片时, 会显示图片的一个简短的描述或由应用为视觉受损的用户读出.
- ``height`` : 高度. 当高度与宽度只指定一个时, 会按照比例不变的原则进行缩放.
- ``width`` : 宽度.
- ``scale`` : 缩放率.
- ``align`` : "top | middle | bottom | left | center | right" 6 选 1.    图片的对齐方式, 与 CSS 一致.
- ``target`` : 超链接(URI或引用名称)    将图片变为超链接引用(可点击), 可选参数是一个URI(相对或绝对), 或一个包含下划线前缀的 "引用名称".

figure
------

一个 ``figure`` 可以理解为 "画布", 在其上可以嵌入其他 rst 结构, 包括 ``image``.

.. figure:: ./00_img/loglogplot.png

    这是 figure 的标题, 嵌入其他结构时需保证缩进.

    +-----------------------------------------+
    | 这里随便嵌入了一个列表                  |
    +-----------------------------------------+

::

    .. figure:: img/59498721_p0.jpg

        这是 figure 的标题, 嵌入其他结构时需保证缩进.

        +-----------------------------------------+
        | 这里随便嵌入了一个列表                  |
        +-----------------------------------------+

``figure`` 接受的参数和 image 相同.

代码
====

代码块
------

.. code-block:: c
    :linenos:

    int main()
    {
        return 0;
    }

::

    .. code-block:: c
        :linenos:

        int main()
        {
            return 0;
        }

接受的参数

- ``:linenos:`` 为代码块生成行号.
- ``:linenothreshold: n`` 超过 n 行的代码块才会标注行号.
- ``:lineno-start: n`` 为代码块生成行号, 并且从 n 开始.
- ``:emphasize-lines: m,n,...`` 着重显示 m,n 等行. 行选择可以使用 ``m-n`` 来选择连续的行.
- ``:caption:`` 为该代码块命名.
- ``:dedent: n`` 调整代码缩进, 减少 n 个空格.

引用外部代码
------------

"引用外部代码" 衍生自 ``include`` 指令, 将外部的代码文件内容嵌入文本.

::

    .. literalinclude:: path/to/file
        :language: codelanguage

接受的参数

允许使用 ``code-block`` 的参数, 除此之外可能需要指定文件字符编码. 并且, ``code-block`` 中高亮模式在主参数指定, 而 ``literalinclude`` 需要 ``:language:`` 参数.

- ``:language: example`` 指定高亮模式.
- ``:encoding: gbk`` 指定 gbk 文本编码.
- ``:lines: m,n,a-b,...`` 只嵌入指定行.
- ``:linenos:`` 为代码块生成行号.
- ``:lineno-start: n`` 为代码块生成行号, 并且从 n 开始.
- ``:emphasize-lines: m,n,...`` 着重显示 m,n 等行. 行选择可以使用 ``m-n`` 来选择连续的行.
- ``:caption:`` 为该行代码块命名.
- ``:dedent: n`` 调整代码缩进, 减少 n 个空格.

如果 目标文件是一个 Python 模块, 还可以从 Python 语义结构上引入指定结构::

    .. literalinclude:: code/example.py
        :pyobject: add

.. literalinclude:: code/example.py
    :pyobject: add

还可以与另一个文件做对比::

    .. literalinclude:: code/example.py
        :diff: code/example_diff.py

.. literalinclude:: code/example.py
    :diff: code/example_diff.py

highlight
---------

``highlight`` 指令影响的是段落中使用 ``::`` 之后的默认渲染语言.

它的影响范围一直持续到下一个 ``highlight`` 指令. 每一个 rst 文件, 段落后 ``::`` 缩进一个单位会被认为一个一个 code-block, 其渲染模式为 Python. 如果 使用了 ``.. highlight:: cpp``, 那么默认渲染模式会变为 C++. 以此类推.

.. highlight:: cpp

这里做一个例子::

    std::cout << "Hello World!" << std::endl;

.. highlight:: none

::

    .. highlight:: cpp

    这里做一个例子::

        std::cout << "Hello World!" << std::endl;

.. highlight:: rst

数学环境
========

使用 LaTeX 语法. ``math`` 指令将创建一个段落级别的数学环境, 要在行内使用, 需要用 ``math`` 角色. math 指令唯一的参数就是 LaTeX 语句, 不管它是在主参数位置还是在内容位置, 并且, 没有其他参数.

::

    .. math:: \frac{\partial y}{\partial x} = x

    .. math::

        \begin{bmatrix}
            1 & 2 \\
            3 & 4 \\
        \end{bmatrix}

.. math:: \frac{\partial y}{\partial x} = x

.. math::

    \begin{bmatrix}
        1 & 2 \\
        3 & 4 \\
    \end{bmatrix}

table
-----

table 指令用于生成表格. 实际上, 在用格式符编辑列表时就隐式地使用了该指令. 而显式地使用 table 指令, 可以附加额外的属性.

::

    .. table:: 列表的标题
        :widths: auto
        :align: center

- ``:width:`` 各列的宽度, 用逗号分隔, 或者使用 "auto", "grid" 参数.
- ``:align:`` 整个列表在页面中的对齐方式, 可选 "left", "center", "right".

注意, 编辑的表格仍然需要遵守语法, 而且, 和 ``.. table`` 指令需要有一个单位的缩进.

list-table
----------

可以通过 list 来创建表格, 这比标准的表格语法输入要简单一点::

  .. list-table::
    :header-rows: int, 表头的行数, 默认为 0
    :stub-columns: int, 从左开始计数, 将被合并为一格的列

在 content 中 需要用二维列表来编辑表格中的单元::

  .. list-table::

    * - (0, 0)
      - (1, 0)
      - (2, 0)
    * - (0, 1)
      - (1, 1)
      - (2, 1)

.. list-table::

  * - (0, 0)
    - (1, 0)
    - (2, 0)
  * - (0, 1)
    - (1, 1)
    - (2, 1)


其他指令
========

include
-------

``include`` 将会把另一个文件嵌入当前文本. 和 ``literalinclude`` 不同, ``include`` 嵌入的不一定是纯文本. 如果嵌入 rst 文件, 那么对应的文字也会被渲染.

::

    .. include:: path/to/file
        :start-line: a  # 从第 a 行开始
        :end-line: b    # 到第 b 行结束
        :start-after: string    # 从这个 string 在目标文本中第一次出现时开始.
        :end-before: string     # 到这个 string 在目标文本中第一次出现时结束.
        :literal:       # 作为纯文本插入, 等同于 literalinclude
        :code: type     # 作为源代码插入, 等同于 literalinclude 设置相应语言模式
        :number-lines: n        # 从 n 开始编号, 默认从 1 开始
        :encoding: utf-8        # 设置字符编码
        :tab-width: 4           # 设置制表符宽度为 4

----------------------------------------


########
rst 角色
########

"角色" 在 rst 中, 就是给一个文本加上特定的身份, 基于这个身份, 实现一系列效果. 可以类比为 CSS 中的 ``class``

语法如下::

    :rolename:`content`

- ``rolename`` 的效果与行为可以使用 Sphinx 预定义的, 也可以自定义.
- ``content`` 是指文本中的对象.

.. _ROLE-REF:

ref
===

``ref`` 可以在整个项目的文档中进行交叉引用. 它使用这样的语法::

    :ref:`Label`

可以在正文的任意位置使用它来引用 Label 所指的内容. 要定义 Label, 需要在一个标题前使用指令::

    .. _示例标签:

    该标签对应段落的标题
    --------------------

    这是一个示例段落, 这里有一个引用了它自己的 REF ---- :ref:`示例标签`

立刻尝试! :ref:`ROLE-REF`.

引用角色还可以用于 图片, 表格 等对象. 只需要在他们前面使用 ``.. _标签名:`` 指令即可 也可以为对象指定 ``:name:`` 属性.

下面的两种语法是等效的::

    .. _图片:

    .. image:: path

    --------------------------

    .. image:: path
        :name: 图片

.. note::
    与隐式链接不同, ref 角色可以跨文件,
    而隐式链接只是链接到当前页面的标题.

doc
===

doc 角色是指向项目内的某篇文档的链接.
链接目标可以用命名或路径方式指定. 不需要扩展名.

对于命名方式指定的文档, 需要其被包含在某个 toctree 当中,
例如 :doc:`latex` 将会链接到本项目中的 /latex.rst 文档,
因为它被包含在 /index.rst 的 toctree 当中.

如果要以路径方式指定, 那么可以用根路径 ``/`` 开头,
或者用 ``.`` 或 ``..`` 开头.
从根路径指定的 :doc:`/matplotlib` 将会指向 /matplotlib.rst,
而从 ``.`` 或 ``..`` 开头的, 则会以当前文档的位置为基准,
指向相对路径上的文档.

如果不指定链接命名的话, 则显示名为对应文档的标题.
:doc:`雷太赫 <latex>`

::

    :doc:`latex`

    :doc:`/matplotlib`

    # 设定命名
    :doc:`雷太赫 <latex>`

download
========

download 角色是指向项目中非 rst 文档, 而是可下载的文件的链接.

::

    :download:`example.zip`

指定的文件路径可以是相对路径或绝对路径.
相对路径以当前文档为基准,
绝对路径以项目根目录为根.

被引用的文件将会在构建时被复制到 ``_download`` 目录里,
重复的文件名将会被处理.

----------------

#########################
使用 Sphinx 书写 API 文档
#########################

程序中有哪些结构? 变量,函数,类 ...... 等等. 在 Sphinx 中定义了相应的指令或角色来描述它们, 并且, 也可以写进源代码的 docstring 中, 让 ``sphinx-apidoc`` 自动生成.

此文参考官方文档 http://www.sphinx-doc.org/en/master/usage/restructuredtext/domains.html .

.. highlight:: rst

函数
====

.. function:: getDate(time, mode="YYYY-MM-DD hh:mm:ss")

    解析传入的时间, 得到一个可读的时间字符串.

    :param int time: 从 1970 至今的秒数
    :param mode: 解析模式
    :type mode: str

    :return: 表示时间的字符串 ``YYYY-MM-DD hh:mm:ss``
    :rtype: str

    :raise ValueError: 不能传入一个负值
    :var test: 一个无关的测试量

使用 ``function`` 描述一个函数::

    .. function:: getDate(time, mode)

        解析传入的时间, 得到一个可读的时间字符串.

        :param int time: 从 1970 至今的秒数
        :param mode: 解析模式
        :type mode: str

        :return: 表示时间的字符串 ``YYYY-MM-DD hh:mm:ss``
        :rtype: str

        :raise ValueError: 不能传入一个负值
        :var test: 一个无关的测试量

- ``function`` 指令后书写函数原型, 应当处于同一行中.
- ``:param xxx:`` 描述一个参数的名称 ``xxx``.
- ``:type xxx:`` 描述参数 ``xxx`` 的类型.
- ``:param type name:`` 同时描述一个参数的类型与名称.
- ``:return:`` 描述返回值.
- ``:rtype:`` 描述返回值的类型.
- ``:raise xxx:`` 描述抛出的异常.
- ``:var yyy:`` 描述用到的一个变量.

并且可以通过 :func:`getDate` 来创建一个指向该函数的链接::

    并且可以用过 :func:`getDate` 来创建一个指向该函数的链接

类
====

.. class:: Clock(speed=0.0)

    .. method:: gamma()

        求解 :math:`\gamma` 因子

        .. math:: \gamma = \frac{1}{ \sqrt{ 1 - \frac{v^2}{c^2} } }

        :return: gamma
        :rtype: float

    .. method:: speed(v)

        设置该钟表相对观察者的速度.

        :param float v: 速度, 单位 m/s

    .. attribute:: position

        该物体相对观察者的位置 ``(float x, float y)``.

- 方法使用 ``method`` , 可接受的修饰和 `函数`_ 一致.
- 类/方法/属性, 可以使用 :class:`Clock`, :meth:`gamma`, :attr:`position` 来创建链接::

    .. class:: Clock(speed=0.0)

        .. method:: gamma()

            求解 :math:`\gamma` 因子

            .. math:: \gamma = \frac{1}{ \sqrt{ 1 - \frac{v^2}{c^2} } }

            :return: gamma
            :rtype: float

        .. method:: speed(v)

            设置该钟表相对观察者的速度.

            :param float v: 速度, 单位 m/s

        .. attribute:: position

            该物体相对观察者的位置 ``(float x, float y)``.

    类/方法/属性, 可以使用 :class:`Clock`, :meth:`gamma`, :attr:`position` 来创建链接

数据
====

用于解释程序中出现的一些重要数据, 比如全局变量/常量.

.. data:: NULL

    ``0``

并且, 使用 :data:`NULL` 来创建一个指向该块的链接::

    .. data:: NULL

        ``0``

    并且, 使用 :data:`NULL` 来创建一个指向该块的链接


使用 Sphinx 生成 LaTeX 文件 (最终得到 PDF)
##########################################

在 LaTeX 设置中, 设置以下参数::

    latex_engine = "xelatex"
    latex_elements = {
        'papersize': 'a4paper',
        'utf8extra': '',
        'inputenc': '',
        'cmappkg': '',
        'fontenc': '',
        'preamble': r'''
            \usepackage{xeCJK}
            \parindent 2em
            \setcounter{tocdepth}{3}
            \renewcommand\familydefault{\ttdefault}
            \renewcommand\CJKfamilydefault{\CJKrmdefault}
        ''',
    }

就能获得良好的 TeX 代码输出, 进入到 ``build/latex`` 目录下 ``make``, 就能自动调用 xelatex 编译 PDF 了

注意, make 文件中, 查找的 LaTeX 文件与这个设置有关::

    # Grouping the document tree into LaTeX files. List of tuples
    # (source start file, target name, title,
    #  author, documentclass [howto, manual, or own class]).
    latex_documents = [
        (master_doc, 文件名, 封面标题,
        author, 'manual'),
    ]

在文件名中, 最好不要有非 ASCII 字符, xelatex 恐怕无法找到含还有中文字符的文件名.

默认情况下, 生成的 PDF 是双页打印模式的, 在电脑上浏览会发现有很多空白, 这是那些左侧有文字, 右侧没有内容, 且下面的内容在下一个章节的情况下, 会留空.

要设置这一点, 在 ``latex_elements`` 中添加一项

.. code-block:: python
    :emphasize-lines: 2

    latex_elements = {
        'extraclassoptions': 'openany,oneside',
    }

那么, 就会按照单页样式打印.


------------------

########
graphviz
########

见 https://www.sphinx-doc.org/en/master/usage/extensions/graphviz.html

.. highlight:: rst

语法
====

可以使用指令::

    .. graphviz:: code/example.gv

来包含一个用 graphviz 语法编辑的文件, 将在构建时渲染为图片.

.. graphviz:: code/example.gv

或者用同样的指令::

    .. graphviz::

        digraph foo {
            "bar" -> "baz";
        }

.. graphviz::

    digraph foo {
        "bar" -> "baz";
    }

或者用子指令, 分别生成有向图与无向图::

    .. digraph:: 名字

        foo -> bar;

    .. graph:: 另一个名字

        foo -- bar;

.. digraph:: 名字

    foo -> bar;

.. graph:: 另一个名字

    foo -- bar;

配置
====

在 ``conf.py`` 中的 ``extensions`` 列表中添加项目 ``"sphinx.ext.graphviz"`` 以启用本插件.

- ``graphviz_dot`` 设置渲染器路径, 默认为 ``dot``, 如果下载安装的 graphviz 套件未添加进 PATH, 那么需要完整的绝对路径.
- ``graphviz_dot_args`` 传递给渲染器的命令行参数, 应该为一个列表, 类似于 :data:`sys.argv`, 或者说 :mod:`argparse` 所解析的格式. 默认为空列表 ``[]``.
- ``graphviz_output_format`` 设置构建 HTML 时的输出格式, 默认为 ``'png'``, 必须在 ``'png'`` 或 ``'svg'`` 中二选一. 如果选择了 svg, 那么为了使图片超链接正常工作, 需要在代码中指定相关的 HTML 属性::

    .. graphviz::

        digraph example {
            a [label="sphinx", href="http://sphinx-doc.org", target="_top"];
            b [label="other"];
            a -> b;
        }

##########
matplotlib
##########

语法
====

提供了 ``plot`` 等指令.

plot
----

见 https://matplotlib.org/devel/plot_directive.html

``plot`` 可以包含一个编写 matplotlib 作图的 Python 代码, 并将其渲染为图形. 同样也可以在下方一个缩进单位的区块中直接编写代码::

    .. plot:: _code/sinx.py
        :include-source:

        添加一些描述(可选的)

    .. plot::

        import matplotlib.pyplot as plt
        import numpy as np

        x = np.linspace(-6, 6, 1000)
        y = np.sin(x)
        plt.plot(x, y)
        plt.title("sin(x)")

        # 最后必须要调用 show 方法, 才能显示
        plt.show()

.. plot:: code/cosx.py
    :include-source:

    添加一些描述(可选的)

.. plot::

    import matplotlib.pyplot as plt
    import numpy as np

    x = np.linspace(-6, 6, 1000)
    y = np.sin(x)
    plt.plot(x, y)
    plt.title("sin(x)")

    # 最后必须要调用 show 方法, 才能显示
    plt.show()

默认会生成 png, big png, pdf 三种格式的图片.

- 可以给 ``plot`` 指令使用参数 ``:include-source:`` 将源代码插入到图片上方.

配置
====

需要在conf.py 文件的 extension 列表中添加项目 ``'matplotlib.sphinxext.plot_directive'`` 项目, 以启用 ``plot`` 指令.

其他可设置项:

``plot_pre_code``
-----------------

.. highlight:: python

在每幅图的代码中都会首先执行的代码, 设置后将不需要在代码中重复书写::

    plot_pre_code = """
    import numpy as np
    import matplotlib.pyplot as plt
    """

``plot_include_source``
-----------------------

设置每幅图的 ``:include-source:`` 选项的默认值::

    plot_include_source = False

``plot_basedir``
----------------

生成图像的默认储存位置, 默认为代码文件所在目录::

    plot_basedir = ''

---------------

##########
自定义扩展
##########

一个 reStructuredText 扩展就是一个 Python 模块,
首先, 需要在文档的 conf.py 中,
将扩展模块文件所在的目录添加到 :data:`sys.path` 之中.

然后, 根据扩展中定义的指令, 角色编写 ``setup`` 函数::

    def setup(app):
        app.add_directive("name", DirectiveClass)
        app.add_role("name", RoleClass)

        #....

参数 app 是由 sphinx 在调用时传递的.

.. warning::

    以下内容未完成.
    代码可能无效或出错.

自定义指令
==========

HelloWorld 扩展
---------------

定义一个指令, 需要继承 :class:`docutils.parser.rst.Directive`::

    from docutils.parser.rst import Directive

    class HelloWorld(Directive):
        pass

对于子类, 需要定义一个 ``run`` 方法::

    class HelloWorld(Directive):
        def run(self):
            pass

在 run 方法中, 返回一个 :mod:`docutils.nodes` 实例列表::

    class HelloWorld(Directive):
        def run(self):
            return [nodes.paragraph(text="Hello World!")]

以下为完整代码::

    from docutils.parser.rst import Directive
    from docutils import nodes

    class Hello(Directive):
        def run(self):
            main = nodes.paragraph(text="Hello World!")
            return [main]

    def setup(app):
        app.add_directive("hello", Hello)

然后在 rst 文档中::

    .. hello::

编译后该指令被替换为::

    Hello World!

接受参数的指令
==============

一个指令如下使用参数::

    .. 指令名:: 指令的 content
        :指令的 option:

        指令的 content

指令的 content 是除了包裹在 ``:option:`` 之外的一切内容,
包括双冒号后的输入, 以及次级缩进块中的普通文本.


##
域
##

所谓的域其实就是用来描述代码中结构的指令.

sphinx 直接支持的代码域有 Python, C, C++, JavaScript.
并且还支持 reStructuredText 与 Math 域.

其他可用的域以插件方式提供, 参见
`More Domains <http://www.sphinx-doc.org/en/master/usage/restructuredtext/domains.html#more-domains>`_


RST的标题
=============
不同文件下相同的级别的标题如何叠加？
能否在一个页面上显示


----------------

