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



## Q160 找出链表的交点

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



##### Tips: If Else 简写


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



## Q206 链表反转

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

## Q21 合并两个有序链表

https://leetcode-cn.com/problems/merge-two-sorted-lists/


```python

```
