## javascript
* 函数传参
* 改变样式
    * 操作属性的方法
        * .指代：oDiv1.style.background=col
        * []: 要修改的属性不固定的时候
    * .与[]通用
    * 字符串与变量：`value ="400px" a='400px' value =a`
* style 与 className
    * 样式 
        * 行间样式与样式表
        * style加样式的时候是加在行间
        * style取样式从行间取，无法从样式表取
    * 样式优先级
        * 通配符*< 标签< class < ID < 行间
        * 内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式
```html
<head>
    <!-- 外部样式 style.css -->
    <link rel="stylesheet" type="text/css" href="style.css"/>
    <!-- 设置：h3{color:blue;} -->
    <style type="text/css">
      /* 内部样式 */
      h3{color:green;}
    </style>
</head>
<body>
    <h3>测试！</h3>
</body>
```
    * style 与class 关系
        * `元素.style.background = "Red"` 是修改行间样式
        * 之后再修改className 不会有效果
* 行间事件
    * 逐个定义行间事件
        * 比较危险
        * 特别麻烦 没个图标点击都得定义
    * 提取行间事件
        * 是一个属性 同样也可以改
        * 必须接受函数 `btn.onclick=function alert(){ alert("abc")}`
    * 匿名函数
        * `function(){}`
    * window.onload的意义
        * script一般放在head里 读一行执行一行
        * 当这个页面加载完成的时候 发生
    * 行为、样式、结构三者分离
        * 行为 JS 样式 CSS 结构 html
        * 别加行内样式
        * 别加行间js事件
* 获取一组元素
    * getElementsByTagName
        * 获取元素的方法二
        * 一次获取一组
* 选项卡
    * `aBtn[i].className="active" `为什么不行？
    * this 当前发生事件的元素
* 日历
    * innerHTML div内的东西
        * 可以往里放html
    * 类似选项卡 只是下面只有一个div
    * 数组的使用
        * 定义 arr=[1,12,3]
        * 切片 arr[0]
    * 字符串连接
        * `"123"+1+2+"3" = "123123"`
        * `"123"+(1+2)+"3" = "12333"`
    * 加入index
* css入门
    * `#tab <div id="tab">` 根据id获取元素
    * `.active <div class="active">` 根据类获取元素
    * `#tab ul` 获取tab中的 ul元素
    * `#tab .active` 获取tab中的active类

###  Debug
* 变量名错误
    * div dic
    * document documnet
    * checked checeked

## Javascript 基础
* javascript 组成
    * ECMAScript:翻译代码--解释器:包括加减乘除
    * DOM:Document object mode 文档对象模型
        * 文档：HTML
    * BOM:Browser 浏览器对象模型
        * window
    * 三者兼容性
        * ECMA:几乎没有
        * DOM:一些操作不兼容
        * BOM:没有兼容问题(完全不兼容)
* 变量类型
    * 类型：`var a=12; typeof a`
    * 数值、字符串、boolen、function、object、undefined
        * `var b; typeof b`变量本身没有类型，存什么就是什么类型
        * 一个变量应该只存一种类型
    * 显式类型转换
        * `parseInt("12px34")=12`识别到不是数字的跳出`parseInt("px")=NaN` not a number 
        * `NaN!=NaN` 判断相等结果为负
        * `isNaN(parseInt("adc"))` isNaN判断是否为NaN
        * `parseFloat` 不知道小数整数时
    * 隐式类型转换
        * `5=="5"` true 先转换类型 再比较
        * `5==="5"` fasle 不转换类型 直接比较 
        * `"7"-"3"=5` 减法会隐式类型转换
 
* 变量作用域和闭包
    * 局部变量：只能在定义的函数中使用
    * 全局变量：任何地方可用
    * 闭包：子函数可以使用父函数的局部变量
* 命名规范
    * 必要性：可读性 规范性
    * 匈牙利
        * 类型前缀
            * a 数组
            * b 布尔
            * f 浮点
            * fn 函数
            * i 整数
            * o 对象
            * re 正则
            * s 字符串
            * v 变体变量
        * 首字母大写
* 运算符
    * % 取模 求余数   parsrInr(s/60) 取整
        * 隔行变色
    * += *= 
    * `!= !== == ===`
    * 逻辑: 与或否
        * && 并且
        * ||
        * !
* 程序流程控制
    * if else if 
    * ``` javascript
            switch(变量){ 
                case 值1:
                    break
                case 值2:
                    break
                default:
                    alert("default")
            }
        ```
    * 三目运算符 条件?语句1：语句2 `a%2==0?alert("double"):alert("single")`
    * break  中断整个循环，不在执行后面 continue 中断本次循环，继续执行
    * 真假
        * 真：true、非零数字、非空字符串、非空对象（document）、
        * 假：false 、0 、空字符串、空对象、undefined
* JSON
    * Json Javascript Object Notation 
    * ```javascript
        var json ={a:1,b:4,c:"abd"}
        alert(json.a)
        alert(json["a"])

        for (var i in arr ){ alert(arr[i])}
        ```
    * javascript 中 var i in arr 返回的i是索引
    * javascript 中 var i in json 返回的是键
    * json 没有length   
## 深入javascript
* 函数返回值
    * 一个函数应该只返回一种类型值
* 函数传参
    * 不定参 arguments
    * eg:求和
    * eg:CSS函数
* 获取非行间样式
    * 兼容性问题
    * 复合样式：background、border 不能取复合样式
    * 简单样式：width、height、position
* 数组基础
    * 数组的使用
        * var arr=[1,2]
        * var arr =new Array(1,2,3) 
        * 无区别
    * 数组的属性
        * length
            * 既可以获取又可以设置
            * 例子：快速清空数组 length=0
    * 数组使用原则
        * 数组中应该只存一种类型变量
    * 数组的方法
        * 添加
            * arr.push(4) 往末尾添加
            * arr.unshift(3) 头部添加
        * 删除
            * arr.pop() 尾部删除
            * arr.shift() 头部删除
        * splice
            * 删除 arr.splice(起点 长度)
            * 插入 arr.splice(起点，0，元素) 
            * 先删除在插入 arr.splice(2,2,"a","b")
        * 连接
            * [1,2,3].concat([2,4,5])
            * join([1,3,4],"-")
        * 排序
            * arr.sort()
            * 排列一个字符串数组
            * 排列一个数字数组
## 定时器的应用
* 定时器的作用
* 数码时钟
* 延时提示框
* 无缝滚动


