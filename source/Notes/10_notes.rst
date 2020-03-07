^^^^^^^^
Notes
^^^^^^^^
.. tip::
    文章，方法，语言，数据
    文章中的问题
    逻辑
    思路

    概念：使用样式引用
    主题和关键词不需要标明，在文章中可以找到
    加粗主要点
    没有第二级就不会有第三级
    类用大写开头
    方法用小写开头 会带括号
    属性小写开头 

    danger,tip,caution,note,warning,important,seealso

    alt+<-

2010_Seto,Karen
##############################


文章 `2010_Seto,Karen`_




2016 Liu,Xingjian
#######################
::

    How polycentric is urban China and why? A case study of 318 cities
    Xingjian Liu, Mingshu Wang

.. important::
    * Over 90% of cities have four or fewer intra-city centers. 
    * Higher degree of polycentricity is found in mountainous cities. 
    * Polycentricity is positively associated with GDP per capita in Eastern China. 
    * Identified patterns of centers in a number of cities are largely consistent with corresponding master plans.

Abstract
**************

* Despite much insightful work on polycentric urban development in China, there is a lack of systematic comparison at the `intra-city`_ level. 
* Therefore, this paper explores polycentric urban development in 318 cities of China using detailed gridded population data :class:`Pop`. 
* Our analysis examines the spatial structure of urbanized area within individual cities and identifies population centers within cities that are at the prefectural level and above. Our empirical results suggest that over 90% of Chinese cities have four or fewer ‘centers’, and **approximately 40% only have one ‘dominating’ center**. 
* Regression models :class:`Regression` reveal that higher degrees of polycentricity are associated with cities in fragmented landscapes. 
* Conditioning on topographic characteristics and total land area, Gross Domestic Product (GDP) per capita is associated positively with high polycentricity in Eastern China.
* Furthermore, our analysis suggests that the development of multiple (sub)centers in a number of cities (e.g., Shanghai and Tianjin) is relatively consistent with their master plans.

.. caution::
    大约百分之四十的城市是单中心的，这直接取决于城市的定义
    它是如何定义城市的，或者如何回避这个问题的


Intro
***************
The emergence of polycentric urban development has been highlighted in recent literature (Anas, Arnott, & Small, 1998; Audirac, 2005; Musterd & Kloosterman, 2001; Vasanen, 2012). **Polycentric cities are formed when previously close-by but independent urban settlements form a larger and more integrated city-system**. Polycentricity is oftentimes deemed a desirable urban form, generating greater agglomeration externalities as well as facilitating the achievement of social, economic, and environmen- tal goals (Parr, 2004). More importantly, polycentricity has been observed and analyzed at various geographical scales, including the `intra-city`_ scale, the `inter-city`_ scale, and the `trans-regional`_ scale.

This paper focuses on polycentric urban development at the `intra-city`_ level in China, based on the identification of intra-city population centers. As the world’s most populous country and largest developing economy, China’s urban transformation has significant global socioeconomic and environmental impacts (Bai, Shi, & Liu, 2014). A recent World Bank report suggests that China is home to approximately 70% cities with more than 100 thousands people in East Asia and the Pacific region (World Bank, 2015). 

Among all strategies and measures to tackle the challenges of China’s urban transformation, we witness an increase in normative plans and policies that target at polycentric urban development (Liu, Derudder, & Wu, 2015). For example, (polycentric) urban regions are identified as the cornerstone in the recently released `national plan for ‘new form of urbanization’`_ and also referred to in `China’s Central Urban Work Conference`_ in December 2015. In addition, ‘polycentric urban patterns’ are deliberately sought after in many cities’ strategic plans (Qin & Han, 2013; Yue, Fan, Wei, & Qi, 2014). For example, in a study conducted by the `National Reform and Development Commission of China`_, 133 out of the 144 `prefectural level cities`_ included in the survey **are planning to develop or developing new districts/towns outside their old urban cores** (Sun & Wei, 2015). More recently, the drive for polycentricity is evidenced by the plan in which Beijing’s `municipal government`_, along with tens of thousands of civil servants and other supporting functions, will be moved out of the crowded old city to a satellite town. **This trend resonates with an international quest for polycentric urban development**. For example, `European spatial and social development policies`_ highlight the benefits of polycentric urban development (Burger & Meijers, 2012; Hall & Pain, 2006), and discussions about ‘megaregion’ as a planning and governance tool (re)surface in the US (Nelson & Lang, 2011).

.. _`municipal government`: 市政府
.. _`prefectural level cities`: 地级市
.. _`National Reform and Development Commission of China`: 发改委
.. _`European spatial and social development policies`: 欧洲空间与社会发展政策
.. _`China’s Central Urban Work Conference`: 中国中心城市工作会
.. _`national plan for ‘new form of urbanization’`: 新型城镇化

Despite many insightful studies on polycentric urban development in China, there lacks a systematic comparison at the intra-city level. On the one hand, most analyses have focused on inter-city or regional polycentricity, i.e., polycentric urban regions or megaregions (Yang, 2005; Liu, Derudder, et al., 2015; Yang, Song, & Lin, 2015; Zhang & Wu, 2006). On the other hand, case studies of urban polycentricity at the intra-city scale are restricted to a few larger cities along the Eastern coast, such as Beijing (Qin & Han, 2013; Zhao, Lu, & de Roo, 2011), Shanghai (Lehmann, 2013; Yue et al., 2014), Guangzhou (Wu, 1998), and Hangzhou (Yue, Liu, & Fan, 2010). As ideas about urban polycentricity are increasingly put into normative plans, systematic analyses of intra-city polycentricity would help generate a better understanding of polycentric urban development, as well as evaluating relevant development policies and plans. (现有研究缺陷)

Aiming to help fill this gap, this paper presents an exploratory analysis of intra-city polycentricity across China using detailed gridded population data. More specifically, our paper examines the spatial structure of urban area within individual cities and is organized as follows. The next section introduces the definition of polycentricity, identification of sub-centers as well as measurement of morphological polycentricity for individual cities. We then discuss our data and the calculation of different polycentric measures. Polycentric development patterns in China are subsequently reviewed, and the association with socioeconomic, physical, and political factors is presented. The paper concludes with a summary of major findings, methodological limitations and avenues for further research.

Data and Methods
********************
data and study area
==========================
Our analysis starts with 364 Mainland Chinese cities at the prefectural level and above. **To ease comprehension for international readers, the nature of a prefecture-level city needs some clarification** (see Li, 2014 for a comprehensive review of the Chinese planning and administrative system): Within the Chinese administrative division system, a prefectural-level city (di ji shi) ranks below a provincial-level unit but above county-level units. A prefectural level city usually comprises of core urban districts and their surrounding region which in turn contains districts, county-level cities, counties, towns, and/or other sub-divisions. In other words, a Chinese prefectural city resembles an intra-city urban system, consisting of a central urban area and outlying urbanized areas such as the seats of counties. In addition to prefecture-level cities, our analysis includes four `municipalities`_ under direct control of the central government (i.e., Beijing, Tianjing, Shanghai, and Chongqing), which have the same administrative ranks as provinces. 

.. _`municipalities`: 直辖市
.. seealso::
    `2014 Yu,Li`_
.. warning::
    于立 应该是Yu,Li 2014
    prefecture-level cities，prefectural level city，prefectural-level city
.. tip::
    To ease comprehension for international readers

The measurement of polycentricity is based on the LandScanTM High Resolution Global Population Dataset :data:`LandScanTM` (Dobson, Bright, Coleman, Durfee, & Worley, 2000), which estimates global population distributions in approximately 1 km by 1 km grids. Errors in population estimation ``notwithstanding``, the LandScan dataset offers two advantages in understanding urban spatial structures: First, LandScan characterizes population distributions at a fine spatial resolution, while official census data are usually aggregated at the city level and do not reflect intra-city population distribution. Second, existing attempts at downscaling aggregated census data (e.g., Wu, Long, Mao, & Liu, 2015) are affected by political/administrative boundary changes, thus becoming less useful for longitudinal analyses. In addition, LandScan averages population distribution over a 24-h period, partially taking commuting and population migration into consideration. LandScan data are gathered for the year of 2012.

.. note::
    人口空间数据的优势

    #. 反映城市内部的空间人口分布
    #. 可以进行纵向比较，消除行政边界变化的影响
    #. 考虑了24h的活动平均与人口迁徙

As LandScan data only characterize the spatial distribution of population, individual urban centers (including both core urban districts and subcenters) identified by our approach correspond to population rather than employment centers. While the analysis of employment centers is key to understanding the urban spatial economy (Giuliano & Small, 1991), there is usually a mismatch between employment and population centers (as characterized by job-housing (im)balance). A close examination of population centers is still ``policy-relevant`` in the Chinese context (Shen & Wang, 2012; Wang & Meng, 1999). For example, the redistribution/control of population is highlighted in many master plans and per-capita standards are widely employed in determining the supply of public services and infrastructure (Li, 2014). 

.. note::
    识别的是哪种中心？

    #. 职业中心？是理解城市空间经济的关键
    #. 两种中心往往是不匹配的，体现为职住不平衡
    #. 人口中心
        #. 许多总体规划都强调人口的重新分配/控制
        #. 广泛采用人均标准来确定公共服务和基础设施的供应

City-level Gross Domestic Product (GDP) and population statistics for 2012 are collected from China City Statistical Yearbook :data:`Statistics` . Our analysis also includes the standard deviation of landform curvature for individual cities, which is derived from the digital elevation model (DEM) of China. :class:`DEM` The DEM data are acquired from the United States Geological Survey. Following the definition of Chinese macro-regions in Fan and Sun (2008), cities are grouped into three regions: the eastern, the western, and the central.

.. tip::
    Following the definition of Chinese macro-regions in Fan and Sun



Defining and measuring polycentricity
========================================
We employ a straightforward and morphological definition of polycentricity (Halbert et al., 2006). The polycentricity of individual cities is measured based on the size and distribution of intra-city centers; a city is deemed more polycentric if it is characterized by a more balanced distribution of centers (Meijers & Burger, 2010; Musterd & Kloosterman, 2001; Parr, 2004). In other words, a polycentric city would be consist of a group of urban centers with a relatively even distribution of importance. Various urban characteristics have been employed to approximate the ‘importance’ of individual centers, such as population size, employment counts, and GDP (Batty, 2013; Meijers & Burger, 2010). In addition, we are aware of the functional, relational, and political dimensions of polycentricity (Halbert et al., 2006; Liu, Derudder et al., 2015). Although these perspectives generate more nuanced understandings of polycentric cities, they usually have higher data requirements. For example, functional and relational approaches to polycentricity require information about the flows of people, information and goods among individual centers. However, measuring intra-city flows is difficult, especially for a large number of cities (Liu, Derudder et al., 2015). Still, there is usually a positive association between morphological and other forms of polycentricity (Burger & Meijers, 2012; Vasanen, 2012). Therefore, our analysis adopts a morphological definition of polycentricity.

Polycentricity can also be measured in a number of different ways. A first rank-size based method characterizes polycentricity by either the slope of the regression line that characterizes the rank-size distribution of centers within a city (Batty, 2008; Meijers & Burger, 2010), and/or the degree to which the largest center deviates from the rank-size distribution (Meijers, 2008). A number of modifications and improvements have been proposed to rank-size based methods, such as estimating polycentricity based on the second, third and fourth largest (sub) centers within a given city (Meijers & Burger, 2010); and transforming the actual rank to reduce small sample bias (Gabaix & Ibragimov, 2011). However, it is difficult to normalize polycentricity measures produced by rank- size based methods, making the interpretation less straightforward (Green, 2007). 

A second type of polycentricity indices draws upon (in) equality measures, such as the Gini Coefficient (Combes & Overman, 2004). More recently, Pereira, Nadalin, Monasterio, and Albuquerque (2013) have reviewed the shortcomings of existing (in) equality measures and propose a new urban centrality index (UCI) to gauge the degree of centrality/polycentricity. Nevertheless, the calcula- tion of UCI is computationally intensive for gridded data as the process involves computing pairwise distances among all grids.

Therefore, we adopt a third polycentricity indicator which is defined by examining the standard deviation of (the size of) (sub) centers (Green, 2007; Liu, Derudder, et al., 2015)

.. math:: P = 1-\frac{\sigma_{obs}}{\sigma_{max}}

where P represents the morphological polycentricity of a city; ?obs represents the standard deviation of ‘importance’ (for example, measured in population and GDP) of individual centers within the city; and ?max is the standard deviation of ‘importance’ in a two- center city where one center has ‘no importance’ and the other has the maximum observed ‘importance’. This method generates a normalized polycentricity measure; the theoretical value of P ranges from 0 to 1, with a value of zero denoting a total lack of polycentricity within the corresponding city and one suggesting a city consisted of several centers of the same size. According to Eq. (1), cities with only one center would have a polycentricity score of zero. Furthermore, indices such as the number of centers and the proportion of population in the largest center have been employed in previous studies (e.g., Lee & Lee, 2014) to gauge polycentricity and will be reported as supplementary measurements.


Identifying centers
======================

Regression analysis
==========================

Sensitivity analysis
========================

Results
******************
Three polycentricity measures Polycentric
=============================================

Physical and socioeconomic factors of polycentricity Tables
===================================================================

Discussion
******************
Physical factors
=====================

Socioeconomic factors
========================
Political factors
===================
Conclusion
********************

Definition
*************
.. [intra-city] 市内尺度 e.g., Central Business Districts (CBDs), edge cities, and satellite towns within a city

.. [inter-city] 市际尺度 e.g., the ‘PearlRiver Delta’ mega-city region

.. [trans-regional] 跨区尺度 e.g. continental ‘development poles’ identified in European Union’s territorial development policies; Halbert, Convery, & Thierstein, 2006




2014 Yu,Li
====================
::
    Chinese city and regional planning systems
    于立



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

.. data:: Gaia

    ``0``

.. class:: POP

    `Gridded Population Data`

    .. data:: LandScanTM
        
        `LandScanTM High Resolution Global Population Dataset`


.. class:: DEM

    .. data:: DEM

        `USGS`

.. data:: Statistics

    `statistics from yearbook`
