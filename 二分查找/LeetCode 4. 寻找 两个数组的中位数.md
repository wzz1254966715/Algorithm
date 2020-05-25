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

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        # 自定义嵌套函数
        def func(k):
            #创建两个指针,分别控制两个列表
            left1, left2 = 0, 0
            while True:
                # 如果nums1列表结束,返回nums2的第k-1个元素(注意左边界为左指针的位置)
                if left1 == len1:
                    return nums2[left1 + k -1]
                # 如果nums2列表结束,返回nums1的第k-1个元素(注意左边界为左指针的位置)
                elif left2 == len2:
                    return nums1[left2 + k -1]
                # 当k=1时,代表已经找到第k个元素了
                elif k == 1:
                    return min(nums1[left1], nums2[left2])

                # 设置新指针,并防止越界
                new_left1 = min(k // 2 - 1, len1 - 1)
                new_left2 = min(k // 2 - 1, len2 - 1)
                # 获取指针位置的值
                num1, num2 = nums1[new_left1], nums2[new_left2]
                if num1 <= num2:
                    k -= new_left1 - left1 + 1
                    left1 = new_left1 + 1
                else:
                    k -= new_left2 - left2 + 1
                    left2 = new_left2 + 1

        # 思路: 寻找第k个数
        len1, len2 = len(nums1), len(nums2)
        len_total = len1 + len2
        # 判断奇数还是偶数
        if len_total % 2 == 0:
            # 长度为偶数,则寻找第k个和第k+1个数
            return (func(len_total // 2) + fun(len_total // 2 + 1)) / 2
        else:
            # 长度为奇数,寻找第k个数
            return func(len_total // 2)
```

