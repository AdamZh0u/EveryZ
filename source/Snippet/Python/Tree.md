# 树

* 每个节点都只有有限个子节点或无子节点；
* 没有父节点的节点称为根节点；
* 每一个非根节点有且只有一个父节点；
* 除了根节点外，每个子节点可以分为多个不相交的子树；
* 树里面没有环路(cycle)

* 无序树：树中任意节点的子节点之间没有顺序关系
* 有序树：树中任意节点的子节点之间有顺序关系
    * 二叉树：每个节点最多含有两个子树的树称为二叉树；
    * 霍夫曼树：带权路径最短的二叉树称为哈夫曼树或最优二叉树；
    * B树：一种对读写操作进行优化的自平衡的二叉查找树，能够保持数据有序，拥有多于两个子树。

## 定义与实现


```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
```

## 二叉树的高度

### 递归

![](https://pic.leetcode-cn.com/Figures/104/104_slide_9.png)

* DFS
* 时间复杂度：我们每个结点只访问一次，因此时间复杂度为$O(N)$，其中$N$ 是结点的数量。
* 空间复杂度：在最糟糕的情况下，树是完全不平衡的，例如每个结点只剩下左子结点，递归将会被调用 $N$ 次（树的高度），因此保持调用栈的存储将是 $O(N)$。但在最好的情况下（树是完全平衡的），树的高度将是 $\log(N)$。因此，在这种情况下的空间复杂度将是 $O(\log(N))$。



```python
class Solution:
    def maxDepth(self, root):
        if root is None: return 0
        else:
            left_height = self.maxDepth(root.left)
            right_height = self.maxDepth(root.right)
            return max(left_height,right_height)+1
```

### 迭代

* 使用 DFS 策略访问每个结点，同时在每次访问时更新最大深度。
* 时间复杂度：$O(N)$。
* 空间复杂度：$O(N)$。


```python
class Solution:
    def maxDepth(self, root):
        stack = []
        if root is not None:
            stack.append((1,root))
        depth = 0
        while stack !=[]:
            cur_depth,root = stack.pop()
            if root is not None:
                depth = max(depth, cur_depth)
                stack.append((cur_depth+1,root.left))
                stack.append((cur_depth+1,root.right))                
        return depth
```

* BFS，层次遍历最后得到的深度就是最大的深度
* 这个写得好！！！


```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        # BFS
        if root is None:
            return 0
        queue = [(1, root)]
        while queue:
            depth, node = queue.pop(0)
            if node.left:
                queue.append((depth+1,node.left))
            if node.right:
                queue.append((depth+1,node.right))
        return depth
```

* DFS与BFS有两点不同：
    * 最后得到的深度不一定是最大深度，所以要用max判断
    * DFS（先序遍历）节点右孩子先入栈，左孩子再入栈`


```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:    
        # DFS
        if root is None:
            return 0
        stack = [(1, root)]
        depth = 0
        while stack:
            cur_dep, node = stack.pop()
            depth = max(depth, cur_dep)
            if node.right:
                stack.append((cur_dep+1,node.right))
            if node.left:
                stack.append((cur_dep+1,node.left))
        return depth
```


```python

```
