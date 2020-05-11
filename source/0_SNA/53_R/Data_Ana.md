# R语言_数据处理

运用R语言进行数据处理。

数据导入、数据结构、数据格式、数值处理、表格处理、日期处理、数字处理、缺失值与唯一值；
## 数据导入与导出

### 导入
* 要导入的数据必须存放在getwd()目录下
* Windows下路径要用斜杠/或者双反斜杠`\\`

```R
library(readr)

read_csv("filename.csv", col_names=c(), col_types=col( colname=col_  ))
## read_csv导入 读取分隔符为逗号
library(readr)
data <- read_csv("filename.csv", col_names =c("",""), col_types=col( cost=col_numeric(),  ))
library(dplyr)
data <- tbl_df(data) # 将表格转化为tbl_df的格式
View(data)  # 视图形式查看

# 设置列名 col_names = TRUE/FASLE/c("name1","name2")  # 第一行作为列名/不作为列名/重命名[数量必须相同]
# 排除某个列、变更列的格式 col_types = cols( X6=col_skip(),X1=col_character() ) # 不选中X6列 （X6为列名）
# 设置编码 locale = locale(encoding = "UTF-8")

## read.table导入
data <- read.table("filename.csv", header=TRUE,sep="",na.strings=c("x"))  
# header=T,即将第一行作为列名；默认为FASLE;
# sep=""，分隔符；默认为（空格、换行、回车、制表符），可设置为逗号(,)、制表符(\t)
# na.strings=c("x","y")  将等号之后的内容会被转换成NA；即该表中x/y的值会被转化为NA

## 直接用RStudio导入
分别设置第一行为列名、更改列名、列格式、跳过列
```

```R
## 加载包中的数据
data(filename, package="")  # 装在包中的特定数据
## 列出当前已加载包中所含的所有可用示例数据集
data() / data(package="")
```

### 导出
```R
setwd("D:\\")
write.table(y,"sample.csv",sep=",")
# y是R中的数据，sample.csv是存到本地的文件名
write.csv(data, file="D:/bearf2.csv")  # 若保存至其他位置，需要完整路径
```

## 数据结构
### 数据整体结构
```R
## 返回数据结构
str(object)
## 返回数据结构/统计摘要
summary()  
# 区别对待不同类型的数据变量- (1)数值型：相关极值等信息；(2)名义型/有序型：显示的是各水平的频数值
## 返回对象格式/类型
class()  # 返回 numeric / character / factor / ts …
## 返回对象维度
dim()
## 返回对象模式
mode()
```
### 行列名称&重命名

```R
## 返回所有列的名称
names(object) 
colnames(object)

## 返回所有行的名称
row.names(object) 
rownames(object) --二维以上的任何对象
```
```R
## 直接修改
fix(object)  # 可以改变格式 numeric / character

## 重命名列名
library(dplyr)
rename(data, newname=oldname)  # 新的列名在前

## 选择变量时进行重命名
select(data, oldname = newname)  # 新的列名在后
```
### 列重新排序
```R
mtcars <- mtacrs[, c(12,1:11)]  # 将第12列置于第一行
```
### 长宽格式转换
```R
## 转为长格式形式
library(reshape2)
melt(data, id.vars, measure.vars,variable.name = "variable", ..., na.rm = FALSE, value.name = "value",  factorsAsStrings = TRUE)
# varaiable.name ，表示将 各个变量的列名 放在这个列下面；
# value.name，表示对应观测值的具体数值
new <- melt(data, id="var", measure="var")
# id="var"/ c("var1","var2")  以该变量为基准进行重构
# measure="var"/ c("var1","var2")   需要将哪些变量组合进id列的变量；若measure缺失，表示所有字段
`new <- melt(economics, id="data", measure=c("unemploy","uempmed")`
`new <- melt(economics, id="data", variable.name="unemploy",value.name="uempmed")`

---------------------------------------------------------------------------------             
## 重铸为宽格式[excel统计表单的形式]
dcast(data, formula, fun.aggregate = NULL, ..., margins = NULL,subset = NULL, fill = NULL, drop = TRUE, value.var = guess_value(data)
# formula中出现的变量，原本为变量的列名，融合后是不参与计算的；参与的是对应的value列

new <- dcast(data, formula, FUN)
# formula，rowvar1+rowvar2 ~ colvar1+colvar2的格式； rowvar-以此为基准的id列；colvar-需要重构的变量列
# FUN，按照任意函数来重构
dcast(data, ID~variable, mean)
```

### 数据行数、唯一值数
```R
## 返回总行数/列数
ncol() / nrow()

length - 计算元素的长度
## 返回对象的个数 或者 某个列的的观测值行数
length(object/data$col)

# 返回对象的唯一值的行数
length(unique(data$col/object))
# 返回非空置的行数
length(na.omit(object/data$col))
```

### tidyr包
1. gather — 宽数据转为长数据。类似于reshape2包中的melt函数
2. spread — 长数据转为宽数据。类似于reshape2包中的cast函数
3. unit — 多列合并为一列
4. separate — 将一列分离为多列
```R
宽数据转为长数据
gather(data, key, value, ..., na.rm = FALSE, convert = FALSE)  
# key, value 输出列的名字
# 这里，...表示需要聚合的指定列 

mtcarsNew <- mtcars %>% gather(attribute, value, -car)
nrow(mtcarsNew)  # 返回333
# 它把出了car以外的所有列都聚集起来，然后各自地把它们名字都放在各自的属性和相应的值的列中。
```

```R
多列合并为一列
unite()
unite(data, col, ..., sep = "_", remove = TRUE)

-----------------------------------------------------------------------------------
一列拆解为多列
separate(data, col, into, sep = "[^[:alnum:]]+", remove = TRUE, convert = FALSE, extra = "warn", fill = "warn",...)
```
##  数据格式
### 因子化
```R
## 简单因子化
data$col <- factor(data$col)

## 有序因子化：按当前顺序来指定顺序
data$col <- factor(data$col, order=TRUE, levels=data$col)
## 有序因子化：自定义顺序
data$col <- factor(data$col, order=TRUE, levels=c("col1","col2"))  # 因子顺序，从低到高；左侧为最低

## 简单无序
data$col <- factor(data$col, order=FALSE)
## 无序因子化/名义变量
data$col <- factor(data$col, levels=c("col1","col2"), labels=levels / c("new_col1","new_col2"))
```
* factor与as.factor的区别
* as.factor(x) – 只能对整个数据/列进行转换，其中无法插入其他语法

```R
## 根据第二列的值，重新对第一列的值进行排序
data$col <- reorder(data$col,data$col2,[FUN],[order=T/F])
# FUN，表示对第二列进行的变换，以此为排序依据
# order= T/F ,逻辑值，返回一个有序因子 or 一个因素
```

### 数据格式索引
格式判断|格式转换|含义
:--:|:--:|:--:
is.numeric( )  |as.numeric( )|数值格式
is.integer( )  |as.integer( )|整数
is.character( )|as.character( )	|字符串格式
is.factor( )   |as.factor( )	|因子化
is.logical( )  |as.logical( )	|逻辑值
 null   | as.Date(object, “format”)	|日期格式
is.list( )	|as.list( )	|列表
is.data.frame( )	|as.data.frame	|数据框格式
is.matrix( )	|as.matrix( )	|矩阵格式
is.array( )	|as.array( )	|数据组
is.vector( )	|as.vector( )	|向量格式

* double：数值型格式（双精度向量：保存更多的有效位数）
```R
data$col <- as.numeric(data$col)

------------------------------------------------------------------------------------
多列变更
gb[,c("net_activation","total_income")] <- lapply(gb[,c("net_activation","total_income")], as.numeric)
data[,col:col] <- lappy(gb[,col:col], as.numeric)
```
### 有效位数&小数位数
```R
## options(digigs=7) # 默认值 有效位数
options(digits=n)  # 限定最小值的有效位数，并使其他数字舍入后与其小数点后的位数相同；
--xx<-c(98,263.5, 2.43, 1.5531)
--options(digits=2)    [1]98 263.6 2.4 1.6
```

# 数据处理
## 绑定数据框
```R
## with绑定数据框
<- with(data, {
  stats <- summary(mpg)  
})
# 花括号{}之间的语句都对数据框table执行，赋值仅在此函数的括号内生效；若使用<<则为全局变量
# 若在with符号左侧（new<-with()）出现赋值的对象，则在with符号内产生的赋值依然在其外有效

# attach() / detach()  # 必须成对出现
attach(data)
...
detach(...)
```
## 基础函数
```R
funs(...)
funs_(dots, args = list(), env = baseenv())
# dots, ...	
## A list of functions specified by:
## Their name, "mean"
## The function itself, mean
## 调用的函数 点号(.) 作为一个虚拟参数  mean(., na.rm = TRUE)
# args	指定要添加到所有函数调用的附加参数的列表。(A named list of additional arguments to be added to all function calls.)
# env	The environment in which functions should be evaluated.
-----------------------------------------------------------------------------------
Examples
# funs(mean, "mean", mean(., na.rm = TRUE))
# Overide default names
funs(m1 = mean, m2 = "mean", m3 = mean(., na.rm = TRUE))
# If you have function names in a vector, use funs_
fs <- c("min", "max")
funs_(fs)
```

## plyr包
* what ：用来切割、计算、合并数据的包
* why ：在一个函数内同时解决spilt-apply-combine的三个步骤
    * Spilt：把要处理的数据分割成小的片段
    * Apply：对每个小片段进行操作
    * Combine:把片段重新组合
* how ：
```R
1. a*ply(.data, .margins, .fun, ..., .progress = "none") 
2. d*ply(.data, .variables, .fun, ..., .progress = "none") 
3. l*ply(.data, .fun, ..., .progress = "none")
# 
ddply(.data, .variables, .fun = NULL, ..., .progress = "none",  .inform = FALSE, .drop = TRUE, .parallel = FALSE, .paropts = NULL)
# 第一个参数是要操作的原始数据集，比如baby_name
# 第二个参数是按照某个（也可以几个）变量，对数据集分割，比如按照year对数据集分割，可以写成.(year)的形式
# 第三个参数是具体执行操作的函数，对分割后的每一个子数据集，调用该函数
# 第四个参数可选，表示第三个参数对应函数所需的额外参数
## 其他参数，可以暂时不用考虑。ddply()函数会自动的将分割后的每一小部分的计算结果汇总，以data.frame的格式保存。<span style="color:red">分割后的数据，是fun的第一个参数。</span>
```

```R
# 对原始数据集做一些操作，并把结果存储在原始数据中
transform()
ddply(baby_names, 
      .(year, sex), 
      transform, 
      rank = rank(-percent, ties.method = "first")
)
# 第二个参数有点变化，除了year，还有sex，这表示对baby_name数据集，对year和sex分类（类似于SQL中的group by year, sex）。第四个参数是transform的额外参数，如果查看transform的帮助文档，其函数调用方式如下：
# 不追加结果到原始数据，而是产生新的数据集
summarize()
summarize(baby_names_2008_boy, trend = max(percent) - min(percent))
# 0.010266
```
## 数值相关
### 创建
```R
## 创建序列 - seq()
seq(from_num, to_num, by_num) 

## 创建重复值 - rep()
rep(x, n)
```
### 替换与返回
```R
根据值来替换
## object[object condition] <- XX <- ""  # 前后变量名必须一致
leadership$age[leadership$age>75]<-"Elder"

## ifelse(test, yes, no)
temp6$budget <- with(temp6,{
     ifelse(budget<(qnt[1]-h),NA,budget)
     ifelse(budget>(qnt[2]+h),NA,budget)})
temp6 <- na.omit(temp6)

## sub()
sub(patter, replacement, object, ignore.case=FALSE, fixed=FALSE) 
--# patter表达式/文本字符串， replacement要替换的值，在object中搜索pattern；fixed=F(默认),pattern为正则表达式；fixed=T,pattern为字符串文本 (相似于vlookup函数)
```

```R
根据位置来替换
## substr() - 返回中间的部分；即替换
substr(object,star_num,stop_num) 
substr(object,star_num,stop_num) <- "xx"   #用xx替换其之间的值；若数量少于则循环，若大于则不会覆盖超出部分
```
### 拆分
```R
## strspilt
x <- strspilt(object, "sep", fixed=FALSE) 
-- # 对object按照”sep"进行分割，若fixed=F(默认),sep为正则表达式；否则为文本字符串； 
strspilt("abc", "") --返回含1个成分，3个元素的列表"a" "b" "c"
```
### 合并
```R
## cat合并；将所有的对象合并为一个单元格的值
object <- "name"
x <- cat("x1","x2",object, num, [sep=""] )

## paste合并；若对象为不同“列”，则一一对应合并；若对象有3列，合并后仍为3列
paste("x1","x2",4\n, …, [sep=""])
```
cat( ) 与 paste( ) 相同与区别

* 区别：
    * paste( ) 对应的列单独合并
    * cat( ) 合并为一个单元格
```R
> paste(c("X","Y"),1:10,sep="")
> # [1] "X1"  "Y2"  "X3"  "Y4"  "X5"  "Y6"  "X7"  "Y8"  "X9"  "Y10"
>
> cat(c("X","Y"),1:10,sep="")
> # XY12345678910
>
```
### 长度
```R
## 计算字符数量 - nchar
## 计算元素数量 - length
x1<-c("ab","cde","fghij")
nchar(x1)----2,3,5
length(x1)----3
```
## 表格相关
### 返回指定子集 - 行
```R
library(dplyr)
filter()
## 根据条件选取 filter(tbl_df, cond)
filter(hflights_df, Month == 1, DayofMonth == 1)
<- filter(tbl_df, x %in% c("a","b")) 
# 集合运算：并且(&)，或者(|)
# 条件判断1： %in% - 表示x中含"a"或者"b"的值，返回为逻辑为真
# 条件判断2：否定(!=)、大于(>)、大于等于(>=)、恒等于(==)
## 排除多条件的观测值 / 或者用 & 联接
<- filter(iris,!Species %in% c("setosa"))
<- filter(iris,Species !="setosa" & Species != "kaggle")
## 选中子集中的特定列
filter() %>% select(., var)
## 通过行数的位置进行选取
filter(tbl_df, n:n)  # 等价于data[n:n, ]
--------------------------------------------------------------------------------------
## 前/后/任意选取
head(data, n) 等价于 first(data, n)
tail(data, n) 等价于 last(data, n) 
nth(x) # 返回第x个观测  (dplyr包)
## 随机选取
sample_frac(iris, 0.5, replace=TRUE)  # 按比例
sample_n(iris, 10, replace=TRUE)  # 按数量
## 选取并排列前n个数
top_n(tbl_df, n)
top_n(tbl_df,-n)  # 从底部开始选择n个数据
## 删除重复值
distinct(hflights_df, Month, .keep_all = TRUE)
#.keep_all = TRUE指保留除Month以外的其它列的内容。默认的情况是不保存其他列的。
```

```R
## subset选取
subset(data, condition, select=c(col1,col2) / col1:col2 )
```

## 返回指定子集 - 列
```R
library(dplyr)
select()
## 通过列名(无需引号)来选取 
select(tbl_df, var1,var2)
#连续多变量 select(tbl_df, var1:var4)
#排除某变量 select(tbl_df, -var)  / select(tbl_df, -(var1:var4))
select(hflights_df, -(Year:DayOfWeek))
## 不同条件列选择 - select_if()
hflights %>% select_if(is.factor)
hflights %>% select_if(function(col) is.numeric(col) && mean(col) > 3.5)
-------------------------------------------------------------------------------------------
## 通过选项函数进行选择
#列名中以元素x为首的列 - starts_with("x")
select(iris_df, starts_with("Petal"))
  
#列名中以元素x结尾的列 -  ends_with("x")
select(iris, ends_with("Width")) 
  
#列名中包含元素x的列 - contains("x")
select(iris_df, contains("etal"))
#排除对应的列，函数前加负号 -
select(iris_df, - starts_with("Petal"))
 
#所有变量 - everything() 一般调整数据集中变量顺序时使用
select(df2tbl,y,everything())  #将变量y放到最前
 
#选择包含在声明变量中的 - one_of("")
select(iris_df, one_of("Species","Petal.Width"))  # 等价于 select(tbl_df, var,var)
  
#选择名称符合指定匹配正则表达式的列 - matches("")
select(iris, matches(".t."))
  
#选择x01到x05的变量 
num_range('x', 1:5, width = 2)
```
```R
1. vector-向量
x <- x[num:num] / <- x[c(num1,num2,num3)]
2.矩阵/数组/数据框--[]方括号中无逗号出现，表示选取列
(1)z <- z[i,j] / z[i, ] / z[, j] / z[i:j, ] / z[ ,i:j] / z[i:j] 
(2)z <- z[c("row","row") , ] 有逗号/ z[, c("col","col")] / [c("col", "col")] 
3.列表
x <- mylist[[ ]] --其中规则与2中相同
```
### 更改数据/创建新变量
```R
## 在原始数据上做修改
library(dplyr)
transfrom()
transform(df, var3=var1+var2)  # 此时将创建新列； 若为 var2 = var2 * 1.5 则将替换为2倍的var2的值
transform(gb, round(select(gb, 消费金额,激活净值,总收入_含点差),2))  # 因为在源数据上做修改，所以无需赋值      
## 创建新列
data$new_col <- c( , )
----------------------------------------------------
mutate(tbl_df, var3=var1+var2, var4=var3+..)
# 优势在于可对刚添加的列进行变换
`mutate(hflights_df,  gain = ArrDelay - DepDelay, gain_per_hour = gain / (AirTime / 60)）`

## 对每一列运行窗口函数 - mutate_each()
mutate_each(iris, funs(min_rank), [var1,var2])
# 窗口函数
--between()  # 数据在a、b之间
--lag  # 把除最后一位以外的所有数据延后，第一个元素为NA
--ntile  # 把数据分为n分
--lead  # 把除第一个值以外的所有元素提前，最后一位为NA
--percent_rank  # 把数据在[0,1]中重组，并排序
--row_number # 排序。并列时将并列数在前的序号在前
--dense_rank  #无缝排序
--min_rank  # 排序，并列时，其他序号延号
```
### 概述函数
```R
# 创建新的对象
library(dplry)
summarize()
## 对数据进行概述，并创建新的子集
summarize(tbl_df,FUN,na.rm=T) # 常伴有na.rm=T
`summarize(hflights_df,  delay = mean(DepDelay, na.rm = TRUE))`
# 概述函数
--first() / last() / nth()
--n() / n_distinct()
--min() / max() / mean() / sd() / median() / IQR() / sum() 
## 分组后求数据聚合
summarize(group_by(df2tbl,x), sum(y))
group_by(tbl_df,var) %>% summarize(., sum(Y))
## 对每一列运行概述函数
summarise_each_(tbl, funs(mean(., na.rm=T)), vars)
# vars, 与Select用法相同。如果确实，则选择所有未分组的变量.
# funs(sum(., na.rm=T))
--------------------------------------------------------------------------------------
count()
## 计算变量中每一个特定值的行数
count(tbl_df, var, [wt=])
# wt=""  若缺失，则统计数量；分类统计观测值行数
count(iris, Species)  # 分组计算Species列中各类别的频量；类似于基本函数包中的table函数
# wt="",若指定某一列，则会通过计算非缺失值的总和来比对权重(weighted)；
# wt = var2 ， 表示按var中的类别来分组计算var2中未缺失值的对应的求和
count(iris, Species, wt=Sepal.Length)  # 即按Species分组后，求对应Sepal.Length中的值的总和
--等价于  iris %>% group_by(., Species) %>% summarize(., sum(Sepal.Length))
```

### 分组与排序
```R
library(dplyr)
gropu_by
## 分组
groub_by(tbl_df, var)  # var为分组变量
iris %>% group_by(., Species) %>% summarize(., sum(Sepal.Length))
# 为每一个分组分别进行概述
iris %>% group_by(., Species) %>% mutate(., ...))
# 按组计算新变量
ungroup(iris) 
## 移出数据框的分组信息
```
```R
library(dplyr)
排序 - arrange()
arrange(tbl_df,var,desc(var))
# 默认为升序排序；降序为desc
arrange(flights, desc(dep_delay - arr_delay))
# 可以在排序里面使用计算 
--------------------------------------------------------------------------------------
## 排序 reorder
# 以对col2列进行函数FUN处理后为排序标准，对col1进行排序；默认为升序，且默认转为有序因子order=T 
reorder(data$col1, data$col2,FUN,[order=T])  
with(InsectSprays, reorder(spray, count, median)  # 以count列的中位数为排序标准对spray进行升序排列
     
--------------------------------------------------------------------------------------
## 排序 - order
data[order(data$col1, -data$col2)， ]  # 负号，表示降序；
```
### 管道函数
```R
%>%  
# 将对象传递给下一个函数的第一个参数
```
### 数据合并
```R
bind_rows(y,z)  # 将数据集z作为新的行插入到y中
bind_cols(y,z)  # 将数据集z作为新的列插入到y中
## 但必须调整好顺序
left_join(a,b,by="x1")  # 向数据集a中加入匹配的数据集b记录
inner_join(a,b,by="x1")
outer_join(a,b, by="x1")  # 保留所记录，所有行
---------------------------------------------------------------------------------
## 合并列
cbind(object1, object2)  # object1在前；但每个对象必须有相同的行数，且有相同的顺序；
cbind(object1,object[,-1]) / cbind(object1, data$col)
#1.当合并的对象中有要丢弃的向量时，可一步完成；
#2.若不是对于丢弃的向量，data[,j]表示的是跟data的第j列合并
---------------------------------------------------------------------------------
## 合并行
rbind(object1, object2)
## merge合并
merge(object1, object2, by="col_name"/=c("x1","x2"))  # 按照by的内容来合并列
```
### 数据拆分
```R
对整个表格数据作用
## 将连续型变量x分给为n个区间；用于创建美观的分割点
pretty(x, n)  
## 将连续型变量x分割成有n个水平的因子
cut(x, n,[order_result=TRUE])
```
### 数据划分
```R
## 划分训练集、测试集
library(caret)
set.seed(1234)  # 必须要有，因为划分是随机划分的；
createDataPartition(y, p=0.x , time=1, list=TRUE)
# time=num, (默认为1），要创建的分区的数目
# p=0.x , 划分为p%的训练数据的百分比，故(1-p)为检验样本量的百分比
# list=FALSE/TRUE，逻辑值； 一般为FALSE
# y, target variable，目标变量
inTraining <- createDataPartition(hr_model$left, p = .75, list = FALSE)  # 将数据进行划分成75%的训练样本和25%检验样本
training <- hr_model[ inTraining,] -将75%的训练样本数据添加到hr_model
testing  <- hr_model[-inTraining,] -将余下25%的检验样本数据添加到hr_model
print(table(hr_model$left)) - 输出分割后的数据行数
```
## 日期处理
```R
## 当前日期
Sys.Date() / date()
## 日期间隔 - difftme
difftime(object1, object2, units="") 
--"auto”,"secs”,"mins”,"hours”,"days”,"weeks”其中的一个，默认为天
## 日期格式
as.Data(object, "input_format")
as.Date(x, "01/02/1956")
## 输出指定格式的日期值，并可以提取日期中的某些部分
format(x, format="output_foramt")
format(today, format="%B %d %Y")
# [1] "November 27 2014"
format(today, format="%A")
# [1] "Thursday"
```

## 数字处理
```R
options(digigs=7) # 默认值
## 指定小数位数（舍入） - round
round(x, [digits=num])  # 将x舍入为指定位数n的小数（默认值为0）
## 指定有效位数（舍入） - sigif()
sigif(x, [digits=num])  # 指定最小值的有效位数
--sigif(3.531,digits=2)  # 返回 3.5
## 取整 - trunc()
trunc(3.531)  # 返回3
## 取整 - 向上/向下
floor()  # 向下取整；等同于Int
ceiling()  # 向上取整(大于等于x最小整数)
-------------------------------------
abs(x)  # 绝对值
x %% y  # 余数  
exp(x)  # 指数
ln(x) / log(x,[y])  # 对数
sqrt(x)  # 平方根
x^n  # 幂次方
```
```R
## 列/行求和
colSums(x)
rowSums(x)
## 行/列均值
colMeans(x) 
rowMeans(x)
range(object)  # 值域
quantile(x,c(0.n,0.n))  # 分位数
sd(x)  # 标准差
var(x)  # 方差
```
## 缺失值与唯一值
```R
缺失值与不可能值
## 检查缺失值
is.na(x)  # 缺失值
colSums(is.na(x))  # 求该列缺失值的数量
mean(is.na(x))  # 若比例小，可直接移除 na.omit(x)
is.nan(x)  # 不可能值
is.infinite(x)  # 无穷值
---------------------------------------------
## 移除缺失值
na.rm = T  # 在计算之前将缺失值移除，可用在函数内部
## 整行删除
na.omit(x)  # 移除所有含缺失值所在的行【删除整行】
newdata <- na.omit(mydata)  # 用来存储没有缺失值的数据
```
```R
唯一值
## 只对向量可用；或对 各行中各变量完全相同的行取一行
unique(x) 
## 可对数据框使用
!duplicated(x)  # 返回逻辑值；若完全相同则为TRUE
逻辑：返回data中所有不相同的值，然后在进行行选取data[x, ]
# 删除各行中变量完全相同的值 = unique(x)
data <- data[!duplicated(data), ]    --# 返回各列所有相同的值 data[duplicated(test),]
# 删除某变量中相同的值
data <- data[!duplicated(test[, "var"]), ]   
-- # 返回单列所有相同的值  data[duplicate(test[,var]),]
# 删除某两个变量完全相同的行
 data <- data[!duplicated(test[, c("var1","var2")], ] 
-- # 返回多列相同的值 data[dulpicated(test[,c("var1","var2")],]
```
## 逻辑判断
```R
which()  # 返回为真的逻辑对象，允许对数组array使用
```
## R符号
### 常见符号
名称|作用
--|--
<-	|赋值符号	
/	|转义符	
[ ]	|给定元素所处位置的数值	a[c(2,4)]
：	|用于表示一个数值序列	a[2:6]
[i,j]	|选择指定的行与列	[i,] [,j] [i,j] [,]
“”	|用于目录名、文件名、包	
‘ ‘	|引用双引号的文字为文本时出现	labs(title=’ positon=”fill” ‘)
#	|用于注释。#之后出现的任何文本都会被R解释器忽视； 并且R只能对单行进行注释，故当出现多条命令符，需在每行前面加上#	
$	|选取一个给定数据框中的某个特定变量	patientIDdata$age
<<-	|特殊赋值符	
[[ ]]	|用于列表中选取对象	mylist[[“ages”]] mylist[[2]]
^或**	|求幂	
x% %y	|求余数（x mod y) 5%%2=1	
x% / %y	|整数除法。5%/2%=2	
==	|严格等于（在浮点型数值时慎用==）	2+2==4
!=	|不等于	
!x	|非x	
x∣y	|x或y	
x&y	|x和y	
isTRUE(x)	|测试x是否为TRUE	
[,-1] [-1,] [,c(-2,-3)]/[,-c(2,3)]	|删除第一列 删除第一行 删除多列，两种表达方式均可	mydata<-mydata[,-1] mydata<-mydata[,-c(2,3)]
“[“	|提取谋而对象一部分的函数，后跟序列数n；1表示该对象的第一部分； 2表示该对象的第二部分；

### R中常见表达式
符号|	作用	|	解释
--|--|--
～	|分隔符号	|y～x+z+w	左边-因变量/响应变量 右边-自变量/解释变量
+	|分隔预测变量		
：	|预测变量的交互项|	y～x+z+x:z	
*	|所有可能交互项的表达方式	|y～xzw—y～x+z+w+x:z+x:w+z:w	
^	|交互项的某个次数|	y～(x+z+w)^2—y～x+z+w+x:z+x:w+z:w	交互项最高次为2次
.	|包含除因变量之外的所有变量|	y～.—y～x+z+w	当一个数据框包含y,x,z,w这四个变量时
-	|减号，从等式中移除某个变量|	y～(x+z+w)^2-x:w— y～x+z+w+x:z+z:w	
-1	|删除截距项	|y～x-1	拟合y在x上的回归，并强制直线通过原点
I() |【大写的i】	从算术（而非表示式）的角度来解释括号中的元素	|y～x+I((z+w)^2)	表示的是x+(z+w)²，而非x+z+w+z:w
function	|可以在表达式中运用的数学函数	|log(y)～x+z+w	
mpg ~ wt \	|cyl	|表示按条件（cyl）绘图

### 研究设计表达式
表达式	|作用	|解释
--|--|--
y～A	|单因素ANOVA	|1.小写字母，定量变量 2.大写字母，组别因子（若不转换为factor，则默认为定量协变量） 3.Subject，被试者独有的标志变量 4.Error(Subject/A)，表示组内因子
y～x+A	|含单个协变量的单因素ANCOVA|	
y～A * B	|双因素ANOVA	|展开为 ~A+B+A:B
y～x1+x2+A*B	|含两个协变量的双因素ANCOVA|	
y～B+A（B是区组因子）	|随机化区组|	
y～A + Error(Subject/A)	|单因素组内ANOVA|	
y～B*W+Error(Subject/W)	|含单个组内因子(W)和单个组间因子(B)的重复测量ANOVA	|展开为 ~B+W+B:W