# LeetCode 128.最长连续序列

## 1. 题目描述

给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 *O(n)*。

```
输入: [100, 4, 200, 1, 3, 2]
输出: 4
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

## 2. 题解

思路:

循环遍历数组

题目是寻找最长的连续序列,选择寻找x,查找x+1....x+n是否在数组中,统计长度

由于是向后查找,所以如果一个数n在数组中存在一个小于n的数m,那么m的序列长度一定大于n的序列长度,所以每次查找只查找序列最小的那个数,也就是n-1不在数组中的值

最后是将列表转化为集合:

	1. 去除重复值,减少不必要的查找
 	2. 集合的查询速度远高于列表

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        # 转换为set,去重并且提高查找复杂度
        nums = set(nums)
        # 初始化最长序列长度为0
        max_count = 0

        for i in nums:
            # 只从每个序列最小的元素开始
            if i-1 not in nums:
                # 拿到序列最小元素值
                num = i
                # 初始化序列长度为1
                length_count = 1

                # 查找序列,num+1....num+n则n为序列长度
                while num + 1 in nums:
                    # 最小元素+1
                    num += 1
                    # 序列长度+1
                    length_count += 1
                # 判断最长序列长度
                max_count = max(max_count, length_count)
        
        return max_count
```

