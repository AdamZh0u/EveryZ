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

