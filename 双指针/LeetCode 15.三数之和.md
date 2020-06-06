# LeetCode 15.三数之和

## 1. 题目描述

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## 2. 题解

思路: 双指针

首先对数组进行升序排序

遍历数组,最多遍历到等于0的位置,因为遍历的元素,是三元组中的最小值,如果大于0,那么一定不能求和为0

构建左右指针,左指针指向当前元素的下一位元素,右指针指向数组最后一位元素

循环收缩指针,并在收缩过程中去掉重复项

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # 获取长度
        size = len(nums)
        # 长度小于3的直接返回空列表
        if size<3:
            return []
        # 创建返回列表
        l = []
        # 对列表升序排序
        nums.sort()
        # 遍历列表
        for i in range(size):
            # 最小元素大于0一定不能求和为0
            if nums[i]>0:
                break
            # 去重,前一个元素已经把当前元素的所有可能都加入了列表
            if i>0 and nums[i] == nums[i-1]:
                continue
            # 左指针,指向最小元素后面的第一个元素
            left = i+1
            # 右指针,指向列表的最后一个元素,也就是最大的元素
            right = size-1
            # 循环遍历
            while left < right:
                if not (nums[i] + nums[left] + nums[right]):
                    # 如果求和为0,则追加到返回列表中
                    l.append([nums[i], nums[left], nums[right]])
                    
                    # 去重 
                    while left<right and nums[left] == nums[left+1]:
                        left += 1
                    while left<right and nums[right] == nums[right-1]:
                        right -= 1
                    # 循环结束后,还是重复元素,所以要继续移动一位
                    left += 1
                    right -= 1
                elif nums[i] + nums[left] < -nums[right]:
                    # 如果前两个元素相加小于第三个元素的负值, 那么就左移左指针,增大前两项和
                    left += 1
                else:
                    # 如果前两个元素相加大于第三个元素的负值, 那么就右移右指针,缩小第三项
                    right -= 1
        return l
```

