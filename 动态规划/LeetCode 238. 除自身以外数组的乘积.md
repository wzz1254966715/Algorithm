# LeetCode 238. 除自身以外数组的乘积

## 1. 题目描述

给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

```
输入: [1,2,3,4]
输出: [24,12,8,6]
```

**提示：**题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。

说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

**进阶：**
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）

## 2. 题解

### 2.1.  左右计算

思路: 

创建两个列表

列表一: 统计每个数字左侧的所有数字乘积

列表二: 统计每个数字右侧的所有数字乘积

将两个列表相乘

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
    	size = len(nums)
    	r = [1]*size
    	l = [1]*size
    	# 计算所有数字左侧乘积
    	for i in range(1, size):
    		l[i] = l[i-1] * nums[i-1]
    	# 计算所有数字右侧乘积
    	for i in range(size-2, -1, -1):
    		r[i] = r[i+1] * nums[i+1]
    	# 左右乘积相乘
    	for i in range(len(l)):
    		l[i] = l[i] * r[i]
    	return l
```

### 2.2 . 左右计算进阶

思路:

整体思路不变

创建一个列表,用于统计左侧数字乘积

创建变量统计右侧所有数字乘积

动态的将变量乘到左右乘积列表中

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        # 创建列表,第一个元素为1
        l = [1]
        # 用于计算每个数字左侧所有数字的乘积
        n = 1
        # 长度
        size = len(nums)
        # 自左至右遍历数组,计算各数字左侧所有数字的乘积,追加到列表中
        for i in range(1, size):
            n *=  nums[i-1]
            l.append(n)
        # 用于计算每个数字右侧所有数字的乘积
        n = 1
        # 自右至左遍历列表,计算各数字右侧所有数字的乘积,并乘到所有左侧乘积中 
        for i in range(size-1, -1, -1):
            l[i] *= n
            n *= nums[i]     
        return l
```

