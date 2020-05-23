# LeetCode 2. 两数相加

[TOC]

## 1. 题目描述

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

```
输入：(2 -> 4 -> 5) + (5 -> 6 -> 4)
输出：7 -> 0 -> 0 -> 1
原因：542 + 465 = 1007
```

## 2. 解题思路

### 2.1. 暴力解法

​	直接将两个链表遍历产生两个整数,

​	然后两个整数相加,

​	再循环分解为新的链表

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        # 创建一个新链表对象
        p1 = l1
        p2 = l2
        p3 = ListNode(0)
        p4 = p3
        num1 = 0
        num2 = 0
        n = 1
        # 将两个链表合并成两个整数
        while p1 or p2:
            n1 = p1.val if p1 else 0
            n2 = p2.val if p2 else 0
            num1 += n1*n
            num2 += n2*n
            # 位数
            n *= 10
            if p1:
                p1 = p1.next
            if p2:
                p2 = p2.next
        # 整数相加
        num = num1 + num2
		# 分解num为链表,当num等于零的时候停止循环
        # 特例: 两个链表都为0,那么需要在添加p3.next == None,代表没有元素
        # 也可以在最开始的时候直接将p3.next的值设置为0
        while num or (p3.next == None):
            p4.next = ListNode(num % 10)
            p4 = p4.next
            num = num //10
        return p3.next
```

### 2.2. 中间变量记十位 

循环遍历列表,由于链表长度不一样,当短的链表结束后,停止后移,但另一个链表继续后移,知道两个链表都结束

设置一个中间变量,用于存放当前位置相加后的值

设置一个中间变量作为十位数的值,也就是下一次循环应该加上的值

```python
# 链表类
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        p1 = l1
        p2 = l2
        # 创建一个新链表对象,初始值为0
        p3 = ListNode(0)
        p4 = p3
        # 中间变量,作为十位数,当前应该为0
        num = 0
        # p1 都 p2为0时代表完成
        while p1 or p2:
            # 如果链表为空,则赋值为0,不影响加法
            n1 = p1.val if p1 else 0
            n2 = p2.val if p2 else 0
			# 求和
            s = n1 + n2 + num
            # 十位数
            num = s // 10
            # 将个位数加到下一个元素中
            p4. next = ListNode(s % 10)
            p4 = p4.next
            if p1:
                p1 = p1.next
            if p2:
                p2 = p2.next
        # 如果不为零代表还有一个十位数,加在最后面
        if num != 0:
            p4.next = ListNode(num)
        # 列表第一个元素是0, 在加的过程中,第一个数放在了第二个链表元素中
        return p3.next
```

