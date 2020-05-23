# LeetCode 3. 无重复字符最长子串

## 1. 题目描述

​	给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

## 2. 题目解析

### 2.1. 暴力解法

双层循环遍历字符串

外层循环记住位置

内层循环从当前位置向后,直到遇到相同字符停止

中间变量: 最长长度, 子串

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        # 长度
        max_size = 0
        for i in range(len(s)):
            # 临时子串,每次大循环清空子串
            s1 = ''
            for j in s[i : ]:
                # 如果当前字符不在子串中,就添加到子串中
                if j not in s1:
                    s1 += j
                else:
                    # 如果当前字符存在,结束循环
                    break
            # 判断最长子串长度
            max_size = max(max_size, len(s1))
        return max_size
```

### 2.2. 滑动窗口

遍历字符串

构建列表或者字符串,用于保存子串

持续向列表中追加元素

当遇到重复字符时,计算列表长度,对比当前最大长度,将重复字符之前的包括重复字符在列表中删除,并将当前字符追加在列表中

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        max_size = 0
        l = []
        for i in s:
            if i  in l:
                max_size = max(max_size, len(l))
                left = l.index(i)
                l = l[left+1: ]
            l.append(i)
                
        max_size = max(max_size, len(l))
        return max_size
```

