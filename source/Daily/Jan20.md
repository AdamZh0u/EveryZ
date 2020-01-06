## 1.5.2019
* qgis +google engine

* latex workshop

## 1.6.2019

* win中安装gee
https://blog.csdn.net/RS_cj/article/details/85008690
os.environ['HTTP_PROXY'] = 'http://127.0.0.1:10809'
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:10809'

* Rstudio中使用Python
https://rstudio.github.io/reticulate/articles/versions.html

* Gee中的领域统计
https://developers.google.com/earth-engine/reducers_reduce_neighborhood

* 在qgis中加载gee
认证
proxy参数设置

* 现有的工具总结
    * 大尺度的矢量计算与景观格局计算
        景观格局必须对于整体进行计算。
    * 景观格局的尺度效应
    * 开源软件低效

* 实现思路与步骤
    * 投影对面积与距离计算的影响
    * 不同距离，不同密度，不同年限
    * 多线程计算的方法
    * 算一幅图需要多久

### 实验一： 一次性能计算多大范围？

### 实验二： 