# LeetCode 25. K个一组翻转链表

## 1. 题目描述

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

```
给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5
```

**说明：**

- 你的算法只能使用常数的额外空间。
- **你不能只是单纯的改变节点内部的值**，而是需要实际进行节点交换。



## 2. 题解

构建反转链表函数

遍历链表,提取子链表进行翻转

合并到链表中

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None



class Solution:
    # 反转链表
    def reverse(self, head, tail):
        p = tail.next
        t = head
        while p!=tail:
            h = t.next
            t.next = p
            p = t
            t = h
        return tail, head

    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        # 新建头结点
        l = ListNode(0)
        l.next = head
        # 子链表头结点
        h = l

        while head:
            # 从子链表头结点开始向后走
            tail = h
            # 记录k值作为循环变量
            for i in range(k):
                tail = tail.next
                if tail == None:
                    return l.next
            # 记录子链表的下一个节点,用于合并子链表
            p = tail.next
            # 翻转子链表
            head, tail = self.reverse(head, tail)
            # 将子链表合并到链表中
            tail.next = p
            h.next = head
            head = p
            h = tail

            
        return l.next
```

