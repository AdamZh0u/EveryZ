# 链表

* 是一种线性表，在每一个节点里存到下一个节点的[指针](https://zh.wikipedia.org/wiki/指標_(電腦科學))(Pointer)
* 链表与顺序表
    * 由于不必须按顺序存储，链表在插入的时候可以达到O(1)的[复杂度](https://zh.wikipedia.org/wiki/複雜度)，比另一种线性表[顺序表](https://zh.wikipedia.org/wiki/顺序表)快得多
    * 但是查找一个节点或者访问特定编号的节点则需要O(n)的时间，而顺序表相应的时间复杂度分别是O(logn)和O(1)
* 适用
    * 克服数组链表需要预先知道数据大小的缺点
    * 可以充分利用计算机内存空间，实现灵活的内存动态管理
    * 失去了数组随机读取的优点
    * 增加了结点的指针域，空间开销比较大。
    * 链表用来==构建许多其它数据结构==，如堆栈，队列和他们的派生。
* 单向链表
    * 在每个节点保存数据和到下一个节点的地址
    * 在最后一个节点保存一个特殊的结束标记
    * 只会按顺序遍历这个链表的时候
* 双向链表
    * 每个节点有两个连接：一个指向前一个节点；而另一个指向下一个节点

## 实现

### 创建一个新链表


```python
class Node:
    def __init__(self,cargo = None, next = None):
        self.cargo = cargo
        self.next = next
    def __str__(self):
        return str(self.cargo)
```

next关键字有什么用？


```python
x = Node("demo")
print(str(x))
```

    demo
    


```python
node1 = Node("Mon")
node2 = Node("Tus")
node3 = Node("Wen")
node1.next = node2
node2.next = node3
```


```python
def printList(node):
    while node:
        print(node)
        node = node.next
```


```python
printList(node1)
```

    Mon
    Tus
    Wen
    

### next 关键字


```python
class Node:
    def __init__(self,cargo = None, nonext = None):
        self.cargo = cargo
        self.nonext = nonext
    def __str__(self):
        return str(self.cargo)
```


```python
x = Node("demo")
print(str(x))
```

    demo
    


```python
print(x.nonext)
```

    None
    


```python
node1 = Node("Mon")
node2 = Node("Tus")
node3 = Node("Wen")
node1.nonext = node2
node2.nonext = node3
```


```python
def printList(node):
    while node:
        print(node)
        node = node.nonext
```


```python
printList(node1)
```

    Mon
    Tus
    Wen
    

+ 无差别

### 递归打印**

#### solution1


```python
def printBackward1(lists):
    if lists == None:
        return
    head = lists
    tail= lists.next
    print(head,tail)
    printBackward(tail)
    print(head,tail)
```


```python
printBackward(node1)
```

    Mon Tus
    Tus Wen
    Wen None
    Wen None
    Tus Wen
    Mon Tus
    

```python
printBackward(node1)
    head = node1
    tail = node2
    print(node1,node2)
    printBackward(node2)
        head = node2
        tail = node3
        print(node2,node3)
        printBackward(node3)
            head = node3
            tail = none
            print(node3,none)
            printBackward(none)
                return 
            print(node3,none)            
        print(node2,node3)
    print(node1,node2)
```

#### solution2


```python
def printBackward2(lists):
    if lists == None:return
    print(lists)
    printBackward2(lists.next)

printBackward2(node1)
```

    Mon
    Tus
    Wen
    

## 基本操作

针对非循环链表的，从头节点开始操作，而且不能插入 None值到链表中。

### 计算链表长度，从前后插入，查找，删除


```python
class Node(object):
#节点类
    #功能：输入一个值data，将值变为一个节点
    def __init__(self, data, next = None):
        self.data = data
        self.next = next

    def __str__(self):
        return self.data

class LinkedList(object):

    def __init__(self, head = None):
        self.head = head
        
    def __len__(self):
        #返回链表长度
        curr = self.head
        counter = 0
        while curr is not None:
            counter += 1
            curr = curr.next
        return counter
    
    def insertToFront(self,data):
        #功能：输入data，插入到头节点前，并更改为头节点
        #输出：当前头节点
        if data is None:
            return None
        node = Node(data,self.head)
        self.head = node
        return node
    
    def append(self,data):
        #功能：输入data,作为节点插入到末尾
        if data is None:
            return None
        node = Node(data)
        if self.head is None:
            self.head = node
            return node
        curr_node = self.head
        while curr_node.next is not None:
            curr_node = curr_node.next
        curr_node.next = node
        return node
    
    def find(self,data):
        #功能：查找链表的节点data与data相同的节点
        if data is None:
            return None
        # 设置头节点为当前节点，若当前节点不为None,遍历整个链表
        curr_node = self.head
        while curr_node is not None:
            #若当前节点的data与输入的data相同，但会当前节点，否则轮到下一个节点
            if curr_node.data == data:
                return curr_node
            curr_node = curr_node.next
    
    def delete(self,data):
        #删除节点：直接将匹配节点的前一节点指向匹配节点的下一节点
        #若输入数据为None,返回
        if data is None:
            return None
        if self.head is None:
            return None
        ## 若头结点匹配，将头设为下一个
        if self.head.data == data:
            self.head = self.head.next
            return
        #将头节点设置为前节点，头节点的下一个节点设置为当前节点
        curr_node = self.head
        # 遍历整个链表，若当前节点与输入数据匹配，将前节点的指针指向当前节点的下一个节点，否则，移到下一个节点
        while curr_node.next is not None:
            if curr_node.next.data == data:
                curr_node.next = curr_node.next.next
                return
            curr_node = curr_node.next
```

### 实践


```python
nodea1 = Node("A1")
nodea2 = Node("A2")
nodea3 = Node("A3")
nodeb1 = Node("B1")
nodeb2 = Node("B2")
nodec1 = Node("C1")
nodec2 = Node("C2")
nodea1.next = nodea2
nodea2.next = nodea3
nodea3.next = nodec1
nodec1.next = nodec2
nodeb1.next = nodeb2
nodeb2.next = nodec1
```


```python
ll1 = LinkedList(nodea1)
ll2 = LinkedList(nodeb1)
```


```python
## 长度
len(ll1),len(ll2)
```




    (5, 4)




```python
## 前插
nodea0 = Node("A0")
ll1.insertToFront(nodea0)
len(ll1)
```




    6




```python
## 后插
nodec3 = Node("C3")
ll1.append(nodec3)
len(ll1)
```




    7




```python
## 查找 返回node
ll1.find("C2")
```




    <__main__.Node at 0x268bb4bf8c8>




```python
## 删除
ll1.delete("C1")
str(nodea3.next)
```




    'C2'




```python
ll3= LinkedList(nodec3)
ll3.delete("C3")
len(ll3)
```




    0



## Q160 找出链表的交点\*\*\*\*\*

* 如果两个链表没有交点，返回 null.
* 在返回结果后，两个链表仍须保持原有的结构。
* 可假定整个链表结构中没有循环。
* 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。



### 暴力法

* 对链表A中的每一个结点 $a_i$，遍历整个链表 B 并检查链表 B 中是否存在结点和$a_i$ 相同。
* 复杂度分析
    * 时间复杂度 : $(mn)$。
    * 空间复杂度 : $O(1)$。


```python
def findIntersectionNode1(ll1,ll2):
    if ll1.head is None or ll2.head is None:
        return None
    node1 = ll1.head

    while node1 is not None:
        node2 = ll2.head ## 循环内赋值
        ##print(str(node1))
        while node2 is not None:
            if node2.data == node1.data:
                return node1
            else:
                node2=node2.next
        node1 = node1.next
```


```python
findIntersectionNode1(ll1,ll2).data
```




    'C1'



### 哈希表法

* 遍历链表 A 并将每个结点的地址/引用存储在哈希表中。然后检查链表 B 中的每一个结点 $b_i$是否在哈希表中。若在，则$b_i$为相交结点。
* 复杂度分析
    * 时间复杂度 : O(m+n)O(m+n)。
    * 空间复杂度 : O(m)或 O(n)。



```python
nodec2.next
```


```python
def findIntersectionNode2(ll1,ll2):
    node1 = ll1.head
    dic = {}
    while node1 is not None:
        dic[node1.data]=node1
        node1 = node1.next
    node2 = ll2.head
    while node2 is not None:
        if node2.data in list(dic.keys()):
            return node2 ##如果改为print 跳不出循环 需加一行
        else:
            node2 = node2.next
```


```python
findIntersectionNode2(ll1,ll2).data
```




    'C1'



### 双指针法

![将链表变成环](https://pic.leetcode-cn.com/396526c47e043feb977e59f98d8df9165ae249d5042ca60ee4d3121c05fea067-%E5%8A%A8%E6%80%81%E5%9B%BE.gif)

![长度差](https://pic.leetcode-cn.com/5651993ddb76ae6a42f0b338aec9382206f567041113f49d6ca670832ac75791-Picture1.png)


```python
def findIntersectionNode3(ll1,ll2):
    a,b = ll1.head,ll2.head
    while a!=b:
        a = a.next if a else ll2.head
        b = b.next if b else ll1.head
    return a
```


```python
findIntersectionNode3(ll1,ll2).data
```




    'C1'



#### Tips: If Else 简写


```python
#值1 if 条件 else 值2
a, b, c =1, 2, 3
c = a if a>b else b
c
```




    2




```python
# [list][ condition]
a, b, c =1, 2, 3
c = [a, b][a > b]
```

    1 2 1
    


```python
# (c and [a] or [b])[0]  # 当c为真 走a 
c = ''
a = ''
b = "demo"
d = c and a or b  # 当a =0时有bug
d
```




    'demo'




```python
c = 1
(c and [a] or [b])[0] 
```




    ''



## Q206 链表反转****

https://leetcode-cn.com/problems/reverse-linked-list/


```python
nodea1 = Node("A1")
nodea2 = Node("A2")
nodea3 = Node("A3")
nodec1 = Node("C1")
nodec2 = Node("C2")
nodea1.next = nodea2
nodea2.next = nodea3
nodea3.next = nodec1
nodec1.next = nodec2
```


```python
ll = LinkedList(nodea1)
```

### 双指针迭代

![](https://pic.leetcode-cn.com/7d8712af4fbb870537607b1dd95d66c248eb178db4319919c32d9304ee85b602-%E8%BF%AD%E4%BB%A3.gif)

* 时间复杂度：$O(n)$，假设 $n$ 是列表的长度
* 空间复杂度：$O(1)$。


```python
def reverseLinkedList1(ll):
    pre = None
    cur = ll.head
    while cur:
        tmp = cur.next
        cur.next = pre
        pre = cur
        cur = tmp
    return LinkedList(pre)
```


```python
ll1 = reverseLinkedList1(ll)
ll1.head.data
```




    'C2'




```python
def reverseLinkedList2(ll):
    cur, pre = ll.head, None
    while cur:
        pre, pre.next,cur = cur, pre, cur.next
    return LinkedList(pre)
```


```python
ll2 =reverseLinkedList2(ll)
ll2.head.data
```




    'C2'



#### Tips: 同行等式与分行等式


```python
## 同行等式左右分别对应，同时赋值,左右分别当做数组
cur1,pre = ll.head,cur1.next
cur1,pre
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-47-03a5fe5f240e> in <module>
          1 ## 同行等式左右分别对应，同时赋值,左右分别当做数组
    ----> 2 cur1,pre = ll.head,cur1.next
          3 cur1,pre
    

    NameError: name 'cur1' is not defined



```python
cur = ll.head
pre = cur.next
cur,pre
```




    (<__main__.Node at 0x1aa2ce09148>, None)




```python
a,b,c = 1,2,3
a,b,c = c,b,a
a,b,c
```




    (3, 2, 1)



### 递归

![](https://pic.leetcode-cn.com/dacd1bf55dec5c8b38d0904f26e472e2024fc8bee4ea46e3aa676f340ba1eb9d-%E9%80%92%E5%BD%92.gif)

* 时间复杂度：$O(n)$，假设 n 是列表的长度，那么时间复杂度为 O(n)。
* 空间复杂度：$O(n)$，由于使用递归，将会使用隐式栈空间。递归深度可能会达到 n 层。



```python
def reverseList3(head):
    if (head==None or head.next==None):
        return head
    cur = reverseList3(head.next)
    head.next.next = head
    head.next = None
    return cur
```


```python
newhead = reverseList3(ll.head)
newhead.
```

A1-->A2-->A3-->None
```python
def reverseList3(head= A1):
    if (head==None or head.next==None):
        return head
    cur = reverseList3(head.next=A2)=A3
        if (head==None or head.next==None):
            return head  
        cur = reverseList3(head.next=A3)=A3
            if (head==None or head.next==None):
                return head = A3              
        head.next.next(A3.next) = head =A2 # 调整A3的指针
        head.next = None# A2 指针为none，删除A2指向A3
        return cur=A3
    head.next.next(A2.next) = head(A1) 
    head.next = None
    return cur
```

## Q21 合并两个有序链表 ***

https://leetcode-cn.com/problems/merge-two-sorted-lists/


```python
class Node:
    def __init__(self, x):
        self.data = x
        self.next = None
nodea1 = Node("A1")
nodea2 = Node("A2")
nodea3 = Node("A3")
nodeb1 = Node("B1")
nodeb2 = Node("B2")
nodeb3 = Node("B3")
nodea1.next = nodea2
nodea2.next = nodea3

nodeb1.next = nodeb2
nodeb2.next = nodeb3
```

### 递归法

* 时间复杂度：$O(n + m)$。 因为每次调用递归都会去掉 l1 或者 l2 的头元素（直到至少有一个链表为空），函数 mergeTwoList 中只会遍历每个元素一次。所以，时间复杂度与合并后的链表长度为线性关系。

* 空间复杂度：$O(n + m)$。调用 mergeTwoLists 退出时 l1 和 l2 中每个元素都一定已经被遍历过了，所以 n + m个栈帧会消耗$O(n+m)$ 的空间。



```python
class Node:
    def __init__(self, x):
        self.val = x
        self.next = None
class Solution:
    def mergeTwoLists1(self, l1, l2):
        if l1 is None:
            return l2
        elif l2 is None:
            return l1
        elif l1.val<l2.val:
            l1.next = self.mergeTwoLists1(l1.next, l2)
            return l1
        else:
            l2.next = self.mergeTwoLists1(l1, l2.next)
            return l2
```


```python
nodea1 = Node("A1")
nodea2 = Node("A2")
nodea3 = Node("A3")
nodeb1 = Node("B1")
nodeb2 = Node("B2")
nodeb3 = Node("B3")
nodea1.next = nodea2
nodea2.next = nodea3

nodeb1.next = nodeb2
nodeb2.next = nodeb3
```


```python
demo = Solution()
node = demo.mergeTwoLists1(nodea1,nodeb1)
```


```python
node.next.next.next.next.val
```




    'B2'



### 迭代


```python
class Node:
    def __init__(self, x):
        self.val = x
        self.next = None
class Solution:
    def mergeTwoLists2(self, l1, l2):
        prehead = ListNode(-1)
        prev = prehead
        while l1 and l2:
            if l1.val<l2.val:
                prev.next = l1
                l1 = l1.next
            else:
                prev.next = l2
                l2 = l2.next
            prev = prev.next
        prev.next = l1 if l1 is not None else l2
        return prehead.next
```


```python
demo = Solution()
node = demo.mergeTwoLists2(nodea1,nodeb1)
```


```python
node.next.next.next.next.val
```




    'B2'



#### Tips: self 是干什么的  @@@

https://blog.csdn.net/CLHugh/article/details/75000104

* \_\_init__方法的第一参数永远是self，表示创建的类实例本身
* 有了\_\_init__方法，创建实例，不能传入空的参数
* self.name = name 把外部传来的参数name的值赋值给Student类自己的属性变量self.name
* __开头表示私有变量，外部不可访问；_开头表示特殊变量，不要随意访问


```python
## self 代表类的实例 而非类
class Test:
    def ppr(self):
        print(self)
        print(self.__class__)

t = Test()
t.ppr()
```

    <__main__.Test object at 0x000002EFA50F5908>
    <class '__main__.Test'>
    


```python
## self可以不写吗-- 类方法与对象方法
class Test:
    def ppr():
        print(__class__)

Test.ppr()
```

    <class '__main__.Test'>
    


```python
## 在继承时，传入的是哪个实例，就是那个传入的实例，而不是指定义了self的类的实例
class Parent:
    def pprt(self):
        print(self)

class Child(Parent):
    def cprt(self):
        print(self)
c = Child()
c.cprt()
c.pprt()
p = Parent()
p.pprt()
```

    <__main__.Child object at 0x000002EFA50F58C8>
    <__main__.Child object at 0x000002EFA50F58C8>
    <__main__.Parent object at 0x000002EFA50F5F08>
    


```python
## 在描述符类中，self指的是描述符类的实例
class Desc:
    def __get__(self, ins, cls):
        print('self in Desc: %s ' % self )
        print(self, ins, cls)
class Test:
    x = Desc()
    def prt(self):
        print('self in Test: %s' % self)
t = Test()
t.prt()
t.x
```

    self in Test: <__main__.Test object at 0x000002EFA5103D08>
    self in Desc: <__main__.Desc object at 0x000002EFA5103F88> 
    <__main__.Desc object at 0x000002EFA5103F88> <__main__.Test object at 0x000002EFA5103D08> <class '__main__.Test'>
    

* 因为这里调用的是t.x，也就是说是Test类的实例t的属性x，由于实例t中并没有定义属性x，所以找到了类属性x，而该属性是描述符属性，为Desc类的实例而已，所以此处并没有调用Test的任何方法。

* 那么我们如果直接通过类来调用属性x也可以得到相同的结果。
* python中存在的几种方法：对象方法、静态方法、类方法


```python
Test.x
```

    self in Desc: <__main__.Desc object at 0x000002EFA5103F88> 
    <__main__.Desc object at 0x000002EFA5103F88> None <class '__main__.Test'>
    

https://blog.csdn.net/lilong117194/article/details/80090951

## Q83 从有序链表中删除重复节点 **


```python
class Node:
    def __init__(self, x):
        self.data = x
        self.next = None
nodea1 = Node("A1")
nodea2 = Node("A2")
nodea3 = Node("A3")
nodeb1 = Node("B1")
nodeb2 = Node("B2")
nodeb3 = Node("B3")
nodea1.next = nodea2
nodea2.next = nodea3

nodeb1.next = nodeb2
nodeb2.next = nodeb3
```

### wrong1


```python
class Solution:
    def deleteDuplicates(self,head):
        des = head
        while des:
            pre = des
            cur = des.next
            while cur:
                if cur.data == des.data:
                    pre.next = cur.next 
                    pre = cur.next ## pre = none ;cur error
                    cur = pre.next 
                else:
                    pre = pre.next
                    cur = cur.next
            des = des.next
        return head 
```


```python
nodea1 = Node("A1")
nodea2 = Node("A1")
nodea3 = Node("A3")
nodea1.next = nodea2
nodea2.next = nodea3
```


```python
demo = Solution()
node = demo.deleteDuplicates(nodea1)
```


```python
node.next.data
```




    'A3'



### demo2


```python
class Solution:
    def deleteDuplicates2(self,head):
        des = head
        while des:
            cur = des.next
            while cur:
                if cur.data == des.data:
                    des.next = cur.next
                cur = cur.next              
            des = des.next
        return head 
```


```python
demo = Solution()
node = demo.deleteDuplicates2(nodea1)
```

### 直接法


```python
class Solution:
    def deleteDuplicates3(self,head):
        cur = head
        while (cur is not None) & (cur.next is not None):
            if cur.data == cur.next.data:
                cur.next = cur.next.next
            else:
                cur = cur.next
        return head 
```

* 这里是有序链表


```python
demo = Solution()
node = demo.deleteDuplicates3(nodea1)
```


```python
node.next.data
```




    'A3'



### 递归***


```python
class Solution:
    def deleteDuplicates(self, head):
        if head is None or head.next is None:
            return head
        
        child = self.deleteDuplicates(head.next)
        if child and head.val == child.val:
            head.next = child.next
            child.next = None
            
        return head
```

```python
    def deleteDuplicates(self, head=a1):
        if head is None or head.next is None:return head
        child=a2 = self.deleteDuplicates(head.next=a2)
            if head is None or head.next is None:return head
            child=a3 = self.deleteDuplicates(head.next=a3) 
                if head is None or head.next is None:return head=a3
            if child and head.val == child.val:
                head.next = child.next
                child.next = None
            return head=a2
        if child and head.val == child.val:
            head.next = child.next
            child.next = None
        return head
```


```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if head:
            head.next = self.deleteDuplicates(head.next)
            return head.next if head.next and head.val == head.next.val else head
```

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if head:
            head.next=a3 = self.deleteDuplicates(head.next)
                if head=a2:
                    head.next=a3 = self.deleteDuplicates(head.next=a3)
                        if head=a3:
                            head.next=None = self.deleteDuplicates(head.next)
                            return head.next if head.next and head.val == head.next.val else head=a3
                    return head.next=a3 if head.next and head.val == head.next.val else head
            return head.next if head.next and head.val == head.next.val else head
```

* 如此精妙的递归是如何构建的
* 递归，可计算性，尾调用
    * https://www.zhihu.com/question/271081962

#### Tips: 递归fib

* 缓存装饰器


```python
def fibo(n):
    if n==1:
        return 1
    elif n==2:
        return 2
    else:
        return fibo(n-1)+fibo(n-2)
```


```python
%%time
i=1
even=[] # 所有不超过4百万的偶数
while fibo(i) <= 4000000:
    # 若该项是偶数，则添加进even[]
    if fibo(i) % 2==0:
        even.append(fibo(i))
    i+=1
print('所有4百万以内的偶数之和=%d'%sum(even))
```

    所有4百万以内的偶数之和=4613732
    Wall time: 6.99 s
    


```python
import functools

# 方法一，自定义缓存装饰器
def cache(func):
    memo = dict()
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        if args not in memo:
            memo[args] = func(*args, **kwargs)
        return memo[args]
    return wrapper

@cache
def fib(n):
    if n <= 2:
        return 1
    return fib(n-2) + fib(n-1)

# 方法二，使用内置缓存装饰器
@functools.lru_cache(8)
def fib(n):
    if n <= 2:
        return 1
    return fib(n-2) + fib(n-1)
```


```python
%%time
i = 0 
even = []
while fib(i)<=40000:
    if fib(i)%1==0:
        even.append(fib(i))
    i+=1
```

    Wall time: 999 µs
    


```python
even
```




    [1,
     1,
     1,
     2,
     3,
     5,
     8,
     13,
     21,
     34,
     55,
     89,
     144,
     233,
     377,
     610,
     987,
     1597,
     2584,
     4181,
     6765,
     10946,
     17711,
     28657]



## Q19 删除链表的倒数第N个节点


```python
class Node:
    def __init__(self, x):
        self.val = x
        self.next = None

nodea1 = Node("A1")
nodea2 = Node("A1")
nodea3 = Node("A3")
nodea1.next = nodea2
nodea2.next = nodea3
```

* 只有一个值，两个值删除头部


```python
class Solution:
    def removeNthFromEnd(self, head, n):
        counter = 0
        head1 = head
        head2 = head
        while head1.next:
            head1 = head1.next
            counter+=1
        if counter == n-1:
            return head.next
        else:
            i = counter-n
            while i:
                head2=head2.next
                i = i-1
            head2.next = head2.next.next
            return head
```


```python
demo = Solution()
demo.removeNthFromEnd1(nodea1,1)
```




    <__main__.Node at 0x1c03d98a888>



### 两次遍历方法

* 添加哑结点，简化极端情况
* 时间复杂度：$O(L)$，该算法对列表进行了两次遍历，首先计算了列表的长度 L 其次找到第 $(L - n)$个结点。 操作执行了 $2L-n$。
* 空间复杂度：$O(1)$，我们只用了常量级的额外空间


```python
class Solution:
    def removeNthFromEnd(self, head, n):
        dummy = Node(0)
        dummy.next = head
        counter = 0
        first = head
        while first:
            counter+=1
            first = first.next
        i = counter-n
        first = dummy
        while i:
            i -=1
            first = first.next
        first.next = first.next.next
        return dummy.next
```

### 一次遍历方法

* 快慢指针间隔n
* 时间复杂度：$O(L)$，该算法对含有 $L$ 个结点的列表进行了一次遍历。
* 空间复杂度：$O(1)$，我们只用了常量级的额外空间


```python
class Solution:
    def removeNthFromEnd(self, head, n):
        dummy = Node(0)
        dummy.next = head
        pre = dummy
        cur = dummy
        for i in range(n+1):
            cur = cur.next
        while cur:
            pre = pre.next
            cur = cur.next
        pre.next = pre.next.next
        return dummy.next
```

### 递归


```python
class Solution:
    i = 0 
    def removeNthFromEnd(self, head, n):
        global i 
        if head is None:
            return None
        head.next = self.removeNthFromEnd(head.next,n)
        i+=1
        return head.next if i==n else head
```

A1-->A2-->A3
```python
    def removeNthFromEnd(self, head=A1, n=2):
        head.next = A3 = removeNthFromEnd(head.next=A2,n)
            head.next = A3 = removeNthFromEnd(head.next=A3,n)
                head.next=None = removeNthFromEnd(head.next=None,n)
                    if head is None:
                        i=0
                        return None
                i+=1 =1
                return head.next if i==n else head =A3              
            i+=1 =2
            return head.next = A3 if i==n else head    
        i+=1 =3 
        return head.next if i==n else head  =A1  
```


```python
demo = Solution()
demo.removeNthFromEnd(nodea1,1)
```




    <__main__.Node at 0x1c03d98a888>




```python
nodea1.val
```




    'A1'



#### Tips: 全局变量 局部变量与 非局部变量


```python
global i
nonlocal i
```


      File "<ipython-input-33-fdfe75b6846f>", line 4
    SyntaxError: name 'i' is nonlocal and global
    



```python

```
