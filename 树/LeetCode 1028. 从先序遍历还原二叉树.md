# LeetCode 1028. 从先序遍历还原二叉树

## 1. 题目描述

我们从二叉树的根节点 root 开始进行深度优先搜索。

在遍历中的每个节点处，我们输出 D 条短划线（其中 D 是该节点的深度），然后输出该节点的值。（如果节点的深度为 D，则其直接子节点的深度为 D + 1。根节点的深度为 0）。

如果节点只有一个子节点，那么保证该子节点为左子节点。

给出遍历输出 S，还原树并返回其根节点 root。

![image-20200618094903247](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200618094903247.png)

```
输入："1-2--3--4-5--6--7"
输出：[1,2,5,3,4,6,7]
```

![image-20200618094946125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200618094946125.png)

```
输入："1-2--3---4-5--6---7"
输出：[1,2,5,3,null,6,null,4,null,7]
```

![image-20200618095001834](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200618095001834.png)

```
输入："1-401--349---90--88"
输出：[1,401,null,349,88,90]
```

**提示：**

- 原始树中的节点数介于 `1` 和 `1000` 之间。
- 每个节点的值介于 `1` 和 `10 ^ 9` 之间。

## 2. 题解

思路: 

使用栈维护当前遍历的层数,

遍历字符串, 获取到当前节点p的层数、值, 构建节点

- 如果是当前节点层数等于栈的长度，也就是总遍历层数，那么当前节点为最后一个节点的左节点
- 如果小于站的长度，那么当前节点是节点层数位置的节点的右节点，同时缩小栈的长度， 维护总层数

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def recoverFromPreorder(self, S: str) -> TreeNode:
        # 模拟栈, 栈的长度代表当前遍历的层数
        stack = []
        size = len(S)
        idx = 0
        # 遍历字符串
        while idx < size:
            # 获取当前节点层数
            level = 0
            while S[idx] == '-':
                idx += 1
                level += 1
            # 获取当前节点值
            value = 0
            while idx < size and S[idx].isdigit():
                value = value * 10 + int(S[idx])
                idx += 1
            # 构建节点
            node = TreeNode(value)
            # 如果当前节点层数等于总遍历层数, 那么即为最后一个节点的左节点
            if level == len(stack):
                if stack:
                    stack[-1].left = node
            # 否则, 为当前节点之前一个节点的右节点
            # 同时需要改变列表长度, 因为总层数已经改变
            else:
                stack = stack[:level]
                stack[-1].right = node
            # 将当前列表追加到列表中, 维护层数
            stack.append(node)
        # 返回头结点
        return stack[0]
```

