^^^^^^^^^
Methods
^^^^^^^^^

.. class:: Regression()

    .. method:: gamma
        
        文本

        :return: gamma
        :rtype: float

        :param float v: 速度, 单位 m/s

    .. attribute:: position

        

^^^^^^^^
Data
^^^^^^^^

.. data:: GAIA

    ``0``


^^^^^^^^
Notes
^^^^^^^^

2010_Seto,Karen
##############################
文章，方法，语言，数据
文章中的问题
逻辑
思路

概念：使用样式引用
主题和关键词不需要标明，在文章中可以找到
加粗主要点
||
没有第二级就不会有第三级
类用大写开头
方法用小写开头 会带括号
属性小写开头 

文章 `2010 Seto,Karen`_

替换引用 适用于常更新的东西
|logo|

|example| 替代需要更新的内容
[#demo]_
`demo2`_

.. [#demo] as you wish 
.. _`demo2`: 隐藏


2016 Liu,Xingjian
#######################
::

    How polycentric is urban China and why? A case study of 318 cities
    Xingjian Liu, Mingshu Wang

danger,tip,caution,note,warning,important,seealso

.. important::
    * Over 90% of cities have four or fewer intra-city centers. 
    * Higher degree of polycentricity is found in mountainous cities. 
    * Polycentricity is positively associated with GDP per capita in Eastern China. 
    * Identified patterns of centers in a number of cities are largely consistent with corresponding master plans.


Abstract
**************

* Despite much insightful work on polycentric urban development in China, there is a lack of systematic comparison at the intra-city level. 
* Therefore, this paper explores polycentric urban development in 318 cities of China using detailed gridded population data. 
* Our analysis examines the spatial structure of urbanized area within individual cities and identifies population centers within cities that are at the prefectural level and above. Our empirical results suggest that over 90% of Chinese cities have four or fewer ‘centers’, and approximately 40% only have one `‘dominating’ center`_. 
* Regression models :class:`Regression` reveal :attr:`position` that higher :meth:`gamma` degrees of ``polycentricity`` are associated with cities in *fragmented landscapes*. 
* Conditioning on topographic characteristics and total land area, **Gross Domestic Product** (GDP) per capita is associated positively with high polycentricity in Eastern China. :data:`GAIA`
* Furthermore, our analysis suggests that the devel- opment of multiple (sub)centers in a number of cities (e.g., Shanghai and Tianjin) is relatively consistent with their master plans.


Intro
***************
`2010_Seto,Karen`_

.. math:: P = 1-\frac{\sigma_{obs}}{\sigma_{max}}

>>> print demo

::

    流程