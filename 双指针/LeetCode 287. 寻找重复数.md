# LeetCode 287. 寻找重复数

## 1. 题目描述

给定一个包含 n + 1 个整数的数组 nums，可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

```
输入: [1,3,4,2,2]
输出: 2

输入: [3,1,3,4,2]
输出: 3

要求：
1. 不能更改原数组（假设数组是只读的）。
2. 只能使用额外的 O(1) 的空间。
3. 时间复杂度小于 O(n^2) 。

说明
1. 数组中只有一个重复的数字，但它可能不止重复出现一次。
2. 其数字都在 1 到 n 之间（包括 1 和 n）
```

## 2. 题解

### 2.1. 排序查找

直接对数组进行排序,找到与前一个元素相等的数

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
    	nums = sorted(nums)
        for i in range(1, len(nums)):
            if nums[i] == nums[i-1]:
                return nums[i]
        return 0
  	# 不满足要求1, 不能改变列表
```

### 2.2. 哈希表

通过字典保存遍历元素,找到重复的直接返回

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
    	dic = {}
        for i in nums:
            if i not in dic:
                dic[i] = 1
            else:
                return i
        return 0
    # 不满足要求2, 只能使用额外的O(1)空间
```

### 2.3. 暴力遍历

双循环查找相等元素,

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        size = len(nums)
        for i in range(size):
            for j in (i+1, size):
                if nums[i] == nums[j]:
                    return nums[j]
        return 0
    # 不满足要求3,时间复杂度小于O(n^2)
    # 此方法没有提交,不知道能不能过
```

### 2.4. 快慢指针

快指针一次走两步,慢指针一次走一步,当相遇时,到达环入口

此时把慢指针调至初始位置,快指针调至一次一步,当两指针值一致,即为重复值

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # 设置快慢指针
        slow, fast = 0, 0
        # 快指针每次两步
        # 慢指针每次一步
        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            # 相遇则退出,到达环入口
            if slow == fast:
                break
        # 调整慢指针到初始位置
        slow = 0
        # 快慢指针每次移动一步
        while True:
            slow = nums[slow]
            fast = nums[fast]
            if slow == fast:
                return slow
```

