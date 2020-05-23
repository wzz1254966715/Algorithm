# LeetCode 1. 两数之和

[TOC]



## 1. 题目详细: 

​	给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

```python
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

## 2. 解题思路:

### 2.1. 暴力解法

循环嵌套遍历数组,外层循环选择第一个数,内层循环把剩下的数一个个匹配,得到答案直接返回

时间复杂度O(n^2)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
       
        for i in range(len(nums)):
            # 从I后面的一个数开始
            for j in range(i+1, len(nums)):
                if target - nums[j] == nums[i]:
                    return [i, j]
        return [] 
```



### 2.2. 字典

遍历一遍数组,每次都判断target-当前数是否在字典中

如果不在,就将当前数的值作为key,下标作为value,存放在字典中

如果在,就直接返回key为(target-当前值)的value, 和当前值的下标返回

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
    	dic = {}
        for i in range(len(nums)):
            # 判断是否为列表的key
            # 也可以写为: if (target - nums[i]) in dic.keys():
            if dic.get(target - nums[i]) is not None:
                return [dic.get(target - nums[i]), i]
            else:
                dic[nums[i]] = i
        return []
```

