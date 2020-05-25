# LeetCode 4. 寻找 两个数组的中位数

## 1. 题目描述

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。

请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

## 2. 题目解析

### 2.1. 暴力解法

直接合并两个数组,排序,根据长度奇偶,计算中位数

时间复杂度为O(nlogn),超出O(log(m+n))

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        nums3 = nums1
        nums3.extend(nums2)
        nums3 = sorted(nums3)
        length = len(nums3)
        n = length // 2

        if length % 2 == 0:
            n = (nums3[n - 1]+ nums3[n]) / 2
        else:
            n = nums3[n]

        return n
```

### 2.2.  二分查找

当看到时间复杂度为log(n), 那么想到的就是二分查找

