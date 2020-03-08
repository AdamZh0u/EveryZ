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

    碰到一个名词，想想自己知不知道，真的知道了吗？

    论文那么多 focus最重要

2016 Liu,Xingjian
#######################
::

    How polycentric is urban China and why? A case study of 318 cities
    Xingjian Liu, 刘行健 Mingshu Wang
    Landscape and urban planning

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
    2014 Yu,Li 各种行政城市是如何定义的
    Chinese city and regional planning systems
    于立

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
We employ a straightforward and morphological definition of polycentricity (Halbert et al., 2006). The polycentricity of individual cities is measured based on the size and distribution of intra-city centers; a city is deemed more polycentric if it is characterized by a more balanced distribution of centers (Meijers & Burger, 2010; Musterd & Kloosterman, 2001; Parr, 2004). In other words, a polycentric city would be consist of a group of urban centers with a relatively even distribution of importance. Various urban characteristics have been employed to approximate the ‘importance’ of individual centers, such as population size, employment counts, and GDP (Batty, 2013; Meijers & Burger, 2010). In addition, we are aware of the functional, relational, and political dimensions of polycentricity (Halbert et al., 2006; Liu, Derudder et al., 2015). **Although these perspectives generate more nuanced understandings of polycentric cities, they usually have higher data requirements.** For example, functional and relational approaches to polycentricity require information about the flows of people, information and goods among individual centers. However, measuring intra-city flows is difficult, especially for a large number of cities (Liu, Derudder et al., 2015). Still, there is usually a positive association between morphological and other forms of polycentricity (Burger & Meijers, 2012; Vasanen, 2012). Therefore, our analysis adopts a morphological definition of polycentricity.

Polycentricity can also be measured in a number of different ways. A first rank-size based method characterizes polycentricity by either the slope of the regression line that characterizes the rank-size distribution of centers within a city (Batty, 2008; Meijers & Burger, 2010), and/or the degree to which the largest center deviates from the rank-size distribution (Meijers, 2008). A number of modifications and improvements have been proposed to rank-size based methods, such as estimating polycentricity based on the second, third and fourth largest (sub) centers within a given city (Meijers & Burger, 2010); and transforming the actual rank to reduce small sample bias (Gabaix & Ibragimov, 2011). However, it is difficult to normalize polycentricity measures produced by rank-size based methods, making the interpretation less straightforward (Green, 2007). 

A second type of polycentricity indices draws upon (in) equality measures, such as the Gini Coefficient (Combes & Overman, 2004). More recently, Pereira, Nadalin, Monasterio, and Albuquerque (2013) have reviewed the shortcomings of existing (in) equality measures and propose a new urban centrality index (UCI) to gauge the degree of centrality/polycentricity. Nevertheless, the calculation of UCI is computationally intensive for gridded data as the process involves computing pairwise distances among all grids.

Therefore, we adopt a third polycentricity indicator which is defined by examining the standard deviation of (the size of) (sub) centers (Green, 2007; Liu, Derudder, et al., 2015)

.. math:: P = 1-\frac{\sigma_{obs}}{\sigma_{max}}

.. note::
    度量多中心性的方法

    #. 使用rank-size的指数，但是这种方法难以标准化，解释不直接。
    #. 使用不平衡指数测度，但是这种方法computationally intensive,要计算格网间的两两距离。
    #. 第三种方法

    Nick green 

.. seealso::
    #. 2010 Meijers,EJ 
    #. 2008 Batty,Micheal science  The Size, Scale, and Shape of Cities
    #. UCI Pereira, Nadalin, Monasterio, and Albuquerque (2013)
    #. green's P 

    Spatial structure and productivity in US metropolitan areas    EJ Meijers, MJ Burger     Environment and planning A.
    Liu, X., Derudder, B., Wu, K., Measuring polycentric urban development in China: an intercity transportation network perspective, Regional Studies, 2015, http://dx.doi.org/10.1080/00343404.2015.1004535 (ahead-of-print):1–14. 
    Green, N. (2007). Functional polycentricity: a formal definition in terms of social network analysis. Urban Studies, 44(11), 2077–2103. @:273

Identifying centers
======================
Our measurement of polycentricity is based on the identification of centers. In the study of urban form, **a working definition of centers is a cluster of analytic units (be they census tracts or grids) that have significantly higher density of population, employment and/or business than surrounding areas** (Leslie, 2010). As implied above, centers identified in our approach would include both core urban districts (such as CBDs) and subcenters (such as satellite towns and edge cities). There is a rich literature in identifying urban centers, using different criteria (e.g., population size, employment size, and land use mix) and based on different spatial units (e.g., census tract and regular grids; Cervero, 1989; Lee, 2007). Conventional meth- ods apply single cut-offs on different attributes, such as population and employment counts (Giuliano & Small, 1991). The key shortcoming of this single (absolute) minimum cut-off approach is that there are no one-size-fits-all criteria for all cities. As a corollary, diverse urban development patterns in China make it difficult to adopt this single cut-off approach.** More recent studies use relative criteria of varied types, including adapted criteria (Lee, 2007), spatial statistical (McMillen, 2001), Kernel density (Leslie, 2010), and spatial clustering (Vasanen, 2012) methods.**

.. note::
    如何定义城市中心

    #. 传统方法采用简单的一刀切
    #. criteria (Lee, 2007), spatial statistical (McMillen, 2001), Kernel density (Leslie, 2010), and spatial clustering (Vasanen, 2012) methods.

.. seealso::
    #. 2007 lee Edge or edgeless cities? Urban spatial structure in us metropolitan areas, 1980–2000. Journal of Regional Science
    #. McMillen, D. P. (2001). Nonparametric employment subcenter identification. Journal of Urban Economics, 50(3), 448–473.
    #. Leslie, T. (2010). Identification and differentiation of urban centers in Phoenix throught a multi-criteria kernel-density approach. International Regional Science Review, 33, 205–235
    #. Vasanen, A. (2012). Functional polycentricity: examining metropolitan spatial structure through the connectivity of urban sub-centres. Urban Studies, 49(16), 3627–3644.

Following Lee (2007), we adopt a relative minimum cut-off approach. For each city, we identify densely populated LandScan grids by set a minimum density cutoffs at the level of its 95- percentile population density (i.e., selecting the top 5% of most densely populated LandScan grids within a city). This threshold is set to 90-percentile for Beijing, Shanghai, Guangzhou, Shenzhen and Tianjin. The rationale for applying such a ``hybrid criteria`` is based on an analysis of density profiles. A city’s density profile is created based on the distribution of population density in individual grids within the corresponding city. Kolmogorov-Smirnov tests :meth:`kolmogorov_smirnov_tests` are employed to assess whether two cities’ density profiles :meth:`profile` are significantly different from each other. Results suggest that the density profiles of the five cities mentioned above (i.e., Beijing, Shanghai, Guangzhou, Shenzhen and Tianjin) are less similar to those of other cities. In addition, a visual inspection suggests that the density profiles of these five cities follow a bimodal distribution :meth:`biomodel_distribution`, while other Chinese cities usually have one ‘peak’ in their density profiles. Consequently, having a lower minimum cutoff (90-percentile instead of 95-percentile) would help us to characterize the bimodal density profiles of these five cities. Furthermore, a sensitivity analysis :meth:`sensitivity_analysis` is reported in section 2.6 to assess whether employing separate thresholds for the five cities would significantly affect our results. Once identified, significantly dense population grids that are adjacent to each other (i.e., rook contiguity) are combined to form clusters. We then define ‘centers’ as those clusters of grids that contain more than 100,000 inhabitants and include at least three grids. The population size of individual centers in a city is inserted into Equitation 1 to calculate polycentricity. 

A ``preliminary experiment`` suggests that individual centers identified by our cut-off based approach are mostly large and spatially separated urbanized areas within a prefectural level city, such as the core urban districts, county-level cities, county seas, and new towns/districts (See for example, Figs. 3 and 4 ). In other words, our polycentricity measure tends to characterize the spatial structure of urbanized areas (i.e., intra-city urban systems) within a prefecture-level city rather than individual clusters of population within the urban core.(我们的多中心测度倾向于表征在地级市内部的城市化区域的空间结构(即城市内空间结构)，而不是城市核心内的单个人口集群。) **Nevertheless, this focus on intra-city urban systems coincides with the geographical scale of many plans and policies that aim to foster polycentricity**(尽管如此，这种对城市内部系统的关注与许多旨在促进多中心的计划和政策的地理范围是一致的). For example, polycentric urban patterns are oftentimes pursued through establishing/developing new towns and districts outside the old city centers in Beijing, Tianjin, and Shanghai (e.g., Sun & Wei, 2015).

Regression analysis
==========================
We conduct an exploratory regression analysis :class:`Regression` to assess the association between Green’s polycentricity index (P) and physical (Curvature) and socioeconomic factors (GDP per capita;GDP). This regression analysis would help us generate preliminary but important hypotheses regarding the formation of polycentric cities. As we have grouped Chinese cities into three regions, a categorical variable (Region) is introduced to capture regional differences. Total land area for each city (Area) is also included as a control variable. Our curvature measure reflects a second order derivative of DEM, indicating the marginal change of landscape slope. Therefore, within a given city, the standard deviation of the curvature of landform represents the dispersion of the marginal slope change, depicting the overall topographical characteristics of that city. We take logarithm of total area and GDP per capita to scale all variables in a similar magnitude. We note that our regression analysis is largely exploratory and future analysis could incorporate additional theoretically informed variables. We have employed a forward model selection strategy. Results for both intermediate and full models are reported in Table 3, with the full model taking the following form:

.. math:: P=\beta_0+\beta_1*\log{(GDP)}+\beta_2*\log{(Area)}+\beta_3*Curvature+\beta_4*Region+\beta_5*\log{(GDP)}*Region

We also run two additional full models with the number of centers (N) and the proportion of the largest center (Prop) as the dependent variable (Table 4).

Sensitivity analysis
========================
As discussed previously, we adopt hybrid criteria for center identification. The minimum density cutoff for the five selected cities (Beijing, Shanghai, Guangzhou, Shenzhen, and Tianjin) is set at 90-percentile, while the cutoff for all other cities is set at 95- percentile. A sensitivity analysis :meth:`sensitivity_analysis` is performed to check if this special treatment of the five cities would significantly affect our empirical results. We test three scenarios where relative minimum density cutoffs at top 10%, 5%, and 2% (i.e. 90-percentile, 95-percentile and 98-percentile) are adopt for all cities. Polycentricity measures calculated from these three scenarios are then compared with those reported in Table 2. Pearson product-moment correlation coefficients :meth:`pearson_correlation` suggest that polycentricity rankings generated with our hybrid criteria are largely consistent with those from the three scenarios (Table 1)

Results
******************
Our urban center identification method is firstly applied to all 364 Chinese at the prefecture level and above. Our method fails to identify any ‘significant’ centers in 46 cities, most of which are small cities in Western China (Fig. 1). These 46 cities are removed from subsequently analyses and we report here the results of 318 cities. In addition to Green’s (2007) polycentricity measure (P) defined by Eq. (1), for each city, we report the number of centers (N) and the proportion of the largest center among all centers (Prop) as supplementary measurements

.. note::
    三种测度

    #. Green's Index P
    #. number of centers N
    #. proportion of the largest center Prop

Three polycentricity measures Polycentric
=============================================
Polycentric urban development at the intra-city level in China is rather uneven. Fig. 2 shows the distribution of number of (sub) centers in 318 cities. **We note that our analysis focuses on the spatial structure of urbanized area within a city rather than the distribution of population within core urban districts**. In other words, we examine the distribution of population across the core urban districts and outlying urbanized areas such new towns, development zones, seats of counties, and county-level cities. Roughly 93% of Chinese cities have 4 or fewer intra-city centers, and 135 or 41% out of 318 cities in our analysis only has one center. Green’s (2007) polycentricity indicator (P) ranges from 0 to 0.965, with an average polycentricity of 0.218; the number of centers (N) ranges from 1 to 10, with an average number of 2.16; the proportion of the largest center (Prop) ranges from 21.49% to 100%, with an average proportion of 67.19%. We have listed the top 25 cities in terms of Green’s measure of polycentricity, the number of centers as well as the proportion of the largest center (Table 2), from which the physical, socioeconomic, and political factors affecting polycentric urban development start to emerge. For example, most cities with the largest number of centers (N) are either highly developed mega-cities (such as Shanghai, Shenzhen, and Tianjin) or cities in mountainous areas (such as Quanzhou, Chongqing and Zhanjiang). In addition, cities with master plans featuring ‘polycentricity’ also rank high in terms of number of centers (e.g., Tianjin, Shanghai, and Quanzhou). Similarly, cities with highest scores in terms of Green’s polycentricity measure (P) are predominantly smaller and from peripheral and mountainous regions. Based on the ranking of the proportion of the largest center, sixteen out of the twenty-five cities listed in this category are either provincial capital cities or municipalities under the direct control of central government. A joint interpretation of the three measures could paint a fuller picture of polycentric urban development in individual cities. For example, with ten identified (sub) centers, Shanghai ranks high in the number of centers (N). However, the largest of these ten centers, which is formed by contiguous core urban districts (Fig. 3), accounts for more than 86% of population in all centers (Prop), thus leading to a relative low overall score in Green’s polycentricity indicator (P).


Physical and socioeconomic factors of polycentricity Tables
===================================================================
Tables 3 and 4 summarize the effects of physical characteristics (standard deviation of the curvature of the landform (Curvature)) and total area (log (Area))) and socioeconomic status (log(GDP) and log (GDP) *Region). Polycentricity (P) is positively associated with curvature std. More specifically, conditioning on total area and GDP per capita, one unit increase in the standard deviation of curvature of the landform (Curvature) is associated with 7.079 unit increase of the degree of polycentricity (P). Cities in the mountainous areas typically have greater standard deviation of curvature. This is also consistent with the observation that cities with more varied land- scape tend to have more centers (N; Table 4). A positive coefficient of log (Area) suggests that larger cities are oftentimes more polycentric. One unit difference in log (Area) is associated with 0.041 unit difference in the degree of polycentricity (P), ceteris paribus. Similarly, as suggested by Table 4, cities with larger land area tend to have more centers (N) and their largest centers tend to be less dominant (Prop) Conditioning on topographical characteristics and total area, for cities in Eastern China, one unit increase in logarithm of GDP per capita is associated with 0.054 unit increase of the degree of poly- centricity (P, p < 0.05). This is consistent with regressions of the number of centers (N) and the proportion of the largest center (Prop), where cities with higher GDP per capita tend to have more centers and less dominant leading centers.

Discussion
******************
We have identified intra-city centers and calculated the degree of polycentricity for 318 Chinese cities at the prefecture level and above. An exploratory regression analysis suggests the association between polycentricity and selected physical and socioeconomic factors. A large literature has attempted to explained intra-city structures and more specifically the polycentric development in Chinese cities, coining terms such as ‘decentralization’ (Lin, 1999), ‘suburbanization’ (Wu & Phelps, 2011; Zhou & Ma, 2000), ‘intra- metropolitan clusters’ (Fragkias & Seto, 2009), and ‘dispersion of urban areas’ (Sun, Wu, Lv, Yao, & Wei, 2013). Although we do not attempt to produce ‘a theory of polycentricity’, we try to link observed polycentric patterns to three broader sets of physical, socioeconomic, and political factors.

Physical factors
=====================
Topographical features exert influences on urban form (Herold,Goldstein, & Clarke, 2003; Ratti & Richens, 2004). Our analysis sug- gests that cities in mountainous areas tend to have more centers as well as higher values in terms of Green’s (2007) polycentric- ity indicator (Table 3). Indeed, topographical constraints such as mountains and waterways play a significant role in shaping land use patterns (Douglas, 2011; Schneider & Woodcock, 2008), which in turn affect the development of polycentric urban areas. For example, by comparing 77 world metropolitan areas, Huang, Lu, & Sellers, 2007 confirm the significant effects of mountainous topog- raphy and coastal lines on urban morphology. The polycentric urban form in several cities can be largely attributed to their rugged, mountainous, and fragmented land- scape. For example, among one of cities with the highest number of centers, Quanzhou in the Fujian Province is largely mountainous and the city is cut through by a number of major rivers (e.g., Jin- jiang and Luojiang rivers). Similar mountainous landscape could be found in Taizhou, Zhejiang (with 7 (sub) centers), which is surrounded by three mountains (i.e., Yandangshan, Kuocang, and Tiantai mountains). Moreover, the highest point in east Zhejiang (Mishailang, 1382.4 m) is located in Taizhou. Still, Chongqing, the only municipality level city in Western China, is dubbed “the moun- tainous city”. Built along the Yangtze river valley, the landscape of Chongqing is characterized by sharp rises and falls (Long, Wu, Wang, & Dong, 2008). Many cities rank high in terms of Green’s polycentricity measure also locate in mountainous area. For exam- ple, surrounded by Tianshan, Kunlun and Pamirs, Kizilsu Kirghiz is more than 90% mountainous. Similarly, in Yunfu, Guangdong, mountainous take up 60% of the entire land area, while hills account for another 30%.


Socioeconomic factors
========================
Regional disparities in socioeconomic development also help to explain differences in polycentric urban development. Prior to the opening and reform policies in 1978, as part of the socialist planning, resources were concentrated in large cities. Since 1978,the relaxation of state control over development has allowed a large number of small cities and towns to emerge on the basis of bottom-up rural transformative development (Lin, 2002). Thanks to a combination of political decentralization, marketization, glob- alization, and convenient locations (Lin, 1999, 2002; Peck & Zhang, 2013), coastal cities along China’s eastern seaboard have achieved substantive economic progress and along the way set the stage for more polycentric urban patterns. More specifically, economic policies that aim to foster ‘Town and Village Enterprises’ (TVEs) and attract Foreign Direct Investment (FDI) have contributed to polycentric urban development (Chang & Wang, 1994; Yusuf & Saich, 2008). For example, as the slogan ‘li tu bu li xiang, jin chang bu jin cheng’ – ‘leave the land but not the village, enter factories but not cities’ – entails, policies that targeted at TVEs encouraged the development of small towns. As many TVEs locate in suburbs and/or erstwhile rural counties, the prosperity of TVEs has contributed to an overall polycentric development pattern (Putterman, 1997). In addition to intra-city polycentricity, cities in the Pearl River and Yangtze River Deltas even coalesce into larger regional polycentric entities (Liu, Derudder et al., 2015). Moreover, featured by decentralization and marketization, changes in Chinese political economy have to certain extent facilitated polycentric urban development. Governments increas- ingly employ market tools to coordinate urban land supply and attract enterprises and foreign investors, leaving some room for polycentric development (Wu & Yeh, 1999). Polycentric urban development has featured in two economically most advanced regions in China: the Pearl River Delta (PRD) and Yangtze River Delta (YRD). For example, the polycentric development of the PRD has been well documented at both intra-city (Wu, 1998; Wu & Yeh, 1999) and intercity levels (Liu, Derudder et al., 2015). Similarly, polycentric development in leading cities across the YRD, such as Shanghai, Hangzhou and Suzhou has been extensively studied (Sun, Shi, & Ning, 2010; Wen & Tao, 2015; Xie, Batty, & Zhao, 2007; Yao, Chen, Wu, Zhang, & Chen, 2009; Yue et al., 2010). The YRD was also among the first regions in China to develop centrally coordi- nated and locally initiated region plans (Luo & Wei, 2009). Apart from cities in the YRD and PRD, other coastal cities are also asso- ciated with more centers (such as Quanzhou, Shantou, and Dalian in Table 2). On the contrary, in economically less developed areas, investment, infrastructure, and growth tend to cluster in core urban districts.

Political factors
===================
Political factors have long been affecting the (polycentric) urban development in China. For example, one of the most ‘polycentric’ cities in our analysis, Wuhan is forged by three formerly indepen- dent cities (Jiao, 2015). However, the focus of this session will be comparing identified polycentric development with planned urban spatial structures, as many Chinese cities develop new towns and districts outside the core urban districts (Sun et al., 2010). For example, Shanghai is undergoing a major process of replanning for polycentric metropolitan areas (Lehmann, 2013). The new plan aims to relocate its heavy industry from the inner city to the city rim. More specifically, Shanghai municipal government has designed a comprehensive polycentric master plan, featuring one central city, 9 satellite cities, 60 new towns, and 600 key settle- ments (Yue et al., 2014). In this plan, the core urban districts or the central city will be surrounded by the nine satellite cities, namely, Baoshan, Jiading-Anting, Qingpu, Songjiang, Minhang, Fengxian, Jinshan, Lingang and Chongming (Fig. 3). Centers identified by our method are largely consistent with this ‘polycentric’ master plan. In our analysis, the central city and the planned Baoshan satel- lite city (where BaoSteel, the fourth largest steel producer in the world, locates) have already merged together. Other planned satellite cities, such as Jiading-Anting, Songjiang, Huinan and Chuansha (near Konggang, Pudong International Airport) also emerge in our results. The uneven distribution of municipal investments in urban infrastructure facilitates the development of polycentric- ity as investment tends to be channeled to areas that are more favorable for enterprises and residential projects. For example, the Shanghai Metro System, launched in 1995, is the longest urban rapid transit network in Asia and the third longest in the world (Yue et al., 2014). The expansion of metro lines to into Pudong New Dis- trict and Songjiang new town facilitates the development of these areas.
Similar polycentric movement could be observed the Beijing and Tianjin (Fig. 4) (Sun, Li, & Lu, 2009). Beijing started to pursue polycentric urban development in its 1958 plan as well as subse- quent revisions in 1983 and 1993 (Chen et al., 2002; Zhao et al., 2011), relocating central-city residents to suburbs so as to make room for commercial development (Feng, Wang, & Zhou, 2009). Although Beijing failed to meet the ‘disperse polycentric urban development plan’ during the period of 1975–1997 (Chen et al., 2002), the effects of urban planning increase over time (Long, Gu, & Han, 2012) and polycentric urban patterns of Beijing started to emerge during 2001–2005 (Qin & Han, 2013). In Fig. 4A, clusters of grids near Yanshan Petrochemical Park and the seat of Changping District can be identified clearly. Founded in 1970, Yanshan Petro- chemical processes more than 10 million tons of crude oil each year, making it one of the largest petrochemical industry bases in China. Meanwhile, Changping, covering an area of 1430 km2, is the most populated suburban district of Beijing. Furthermore, several planned subcenters are already connected with the central city in our results, such as Tongzhou, Yizhuang, Nanyuan, Daxing and Mentougou. By comparing five urban master plans of Beijing drafted in 1958, 1973, 1982, 1992 and 2004, Long et al. (2012) sug- gest that the planned construction area is increasing over time and much of this increase takes place in outlying urbanized areas, i.e., new districts/towns and seats of counties. In Tianjin, polycentric urban patterns emerged in the early 2000s (Liu, Zou, & Liu, 2015), and our analysis identifies the core urban districts and Binhai New District (Fig. 4B). In addition, seats of counties such as Baodi and Wuqing have been identified as subcenters, making Tianjin one of the cities with most (sub) centers (Table 2). According to Liu, Zou, et al. (2015), although the city is still in the early stage of suburbanization, the population distribution of Tianjin is already characterized by a layered, centrifugal and polycentric pattern. However, we note that, without detailed longitudinal data, it is difficult to evaluate the causality between planned and observed polycentric patterns.

Conclusion
********************
This exploratory study contributes to the literature on polycentric urban development by providing a systematic assessment of urban China at the intra-city level and generating preliminary but important hypotheses regarding the relationship between polycentric cities and physical, economic, and political factors. We have identified intra-city centers for 318 Chinese prefectural level cities and revealed multi-faceted polycentricity with three indicators. The empirical results suggest that over 90% of Chinese cities have four or fewer ‘centers’, and approximately 40% are monocentric. Regression models reveal that higher degrees of polycentricity are associated with cities in fragmented landscapes. Conditioning on topographic characteristics and total land area, GDP per capita is associated positively with high degree of polycentricity in Eastern China. Furthermore, our analysis suggests that the development of multiple centers in a number of cities (e.g., Shanghai and Tian- jin) is relatively consistent with and reflecting the effectuation of their master plans. Cities in mountainous area tend to rank high in terms of Green’s (2007) polycentricity indicator while cities with more advanced economy tend to have larger number of centers. Altough our analysis does not put forward a normative suggestion for polycentric cities, these preliminary hypotheses regarding the relationship between polycentricity and related economic, politi- cal, and physical factors could inform the planning of polycentric cities. Furthermore, our technical procedures could form the base for tracking changes in polycentric urban patterns and evaluating relevant plans and policies. Our analysis suffers from several limitations, pointing to potential directions for future research. First, as mentioned previously, the current paper focuses exclusively on the morphological aspects of polycentric urban development in China. Future studies, when detailed network data at the intra-city level are available, would render a fuller picture of polycentric urban China by shedding lights on relational and functional polycentricity (Burger & Meijers, 2012; Liu, Derudder, et al., 2015; Wang, Zhou, et al., 2016). Second, our results and discussions pertain only to population centers, as the analysis depends on intra-city centers identified from population distribution. Analysis of urban centers based on employment dis- tribution may or may not generate the same results. Therefore, our results should be benchmarked against centers derived from other urban attributes, especially employment statistics. Third, our anal- ysis only reveals urban spatial structure within prefectural cities and further studies are required to ‘open the box’ and examine subcenters within core urban districts. Fourth, a longitudinal study would help us to explore the causal relationships among (poly- centric) urban form, socioeconomic development, and urban plans, thus confirming or rejecting some hypotheses generated from the current study (e.g., Meijers & Burger, 2010).


Definition
*************
.. [intra-city] 市内尺度 e.g., Central Business Districts (CBDs), edge cities, and satellite towns within a city

.. [inter-city] 市际尺度 e.g., the ‘PearlRiver Delta’ mega-city region

.. [trans-regional] 跨区尺度 e.g. continental ‘development poles’ identified in European Union’s territorial development policies; Halbert, Convery, & Thierstein, 2006










^^^^^^^^^
Methods
^^^^^^^^^
.. class:: Regression()

    .. method:: 


.. class:: Stat()

    .. method:: profile

        `通过分布判断`

    .. method:: biomodel_distribution

        双峰分布
    
    .. method:: sensitivity_analysis

        敏感性分析 评价是否有影响

    .. method:: pearson_correlation

        皮尔逊

    .. method:: kolmogorov_smirnov_tests

        KS检验 检验是否符合某种分布或者两个分布是否相同
        t-检验的假设是检验的数据满足正态分布，否则对于小样本不满足正态分布的数据用t-检验就会造成较大的偏差，虽然对于大样本不满足正态分布的数据而言t-检验还是相当精确有效的手段。

.. note::
    https://www.cnblogs.com/arkenstone/p/5496761.html
    经常使用的拟合优度检验和Kolmogorov-Smirnov检验的检验功效较低，在许多计算机软件的Kolmogorov-Smirnov检验无论是大小样本都用大样本近似的公式，很不精准，一般使用Shapiro-Wilk检验和Lilliefor检验。
    Kolmogorov-Smirnov检验只能检验是否一个样本来自于一个已知样本，而Lilliefor检验可以检验是否来自未知总体。
    Shapiro-Wilk检验和Lilliefor检验都是进行大小排序后得到的，所以易受异常值的影响。
    Shapiro-Wilk检验只适用于小样本场合（3≤n≤50）,其他方法的检验功效一般随样本容量的增大而增大。
    拟合优度检验和Kolmogorov-Smirnov检验都采用实际频数和期望频数进行检验，前者既可用于连续总体，又可用于离散总体，而Kolmogorov-Smirnov检验只适用于连续和定量数据。
    拟合优度检验的检验结果依赖于分组，而其他方法的检验结果与区间划分无关。

.. class:: demo()

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
