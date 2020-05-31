# LeetCode 101. 对称二叉树

## 1. 题目描述

给定一个二叉树，检查它是否是镜像对称的。

```
1. 二叉树 `[1,2,2,3,4,4,3]` 是对称的。

	1
   / \
  2   2
 / \ / \
3  4 4  3

2. [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3

进阶：
你可以运用递归和迭代两种方法解决这个问题吗？
```

## 2. 题解

```python
# 二叉树类
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
```



### 2.1. 递归

镜像树的条件:

	1. 当前相等
 	2. 左子树值等于另一棵树的右子树值
 	3. 右子树值等于另一棵树的左子树值

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        # 判空
        if not root: 
            return true 
        # 传递左右子树
        return self.func(root.left, root.right)
    
    def func(self, l, r):
        # 左右都为空
        if (not l and not r):
            return True
        # 有一个为空,另一个不为空,或者两个节点的值不相等, 就不是镜像
        elif (not l or not r) or (l.val != r.val):
            return False
        # 分别传递左右子树
        return self.func(l.left, r.right) and self.func(l.right, r.left)
```

### 2.2. 迭代

​	使用一个列表或者队列,用来保证镜像节点顺序

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        # 创建列表,记录镜像节点, 将左右镜像节点传递
        q = [root.left, root.right]
        while len(q):
            # 弹出第0个元素,是左节点
            l = q.pop(0)
            # 弹出第0个元素,是右节点
            r = q.pop(0)
            # 都为空则跳出本次
            if not l and not r:
                continue
            # 两个节点不一样则直接返回不是镜像
            elif (not l or not r) or (l.val != r.val):
                return False
            # 将镜像节点按顺序追加到列表后
            q.append(l.left)
            q.append(r.right)
            q.append(l.right)
            q.append(r.left)
        # 当循环结束,代表所有节点满足镜像条件
        return True
```

