^^^^^^^^^^^^^^^^^^^^^^^
Regular Expression
^^^^^^^^^^^^^^^^^^^^^^^

#############

#############

在VSCode 中使用正则替换
========================
::

    Expression
    ##(\s?\d*.*\n?) \s 空格 \d数字 .任意字符 \n换行
    替换为 $1=   $1 替换内容

    \s+·(\d+)
    $1
    替换 ·59 为 59 

`30minReg`_  、`精通正则表达式`_ 、`测试工具`_
http://www.zjmainstay.cn/

.. [测试工具] regexbuddy
.. [精通正则表达式] : https://book.douban.com/subject/2154713/  
.. [30minReg] : https://deerchao.cn/tutorials/regex/regex.htm