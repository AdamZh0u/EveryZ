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

.. image:: ../Plots/img/loglogplot.png
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

.. figure:: ../Plots/img/loglogplot.png

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