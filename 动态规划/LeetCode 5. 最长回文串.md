# LeetCode 5. 最长回文串

## 1. 题目描述

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

输入: "cbbd"
输出: "bb"
```

## 2. 题解

### 2.1 暴力解法_滑动窗口

设置窗口大小从1到长度+1,因为切片取不到right,所以+1

移动窗口,判断是否是回文串,并根据长度保留回文子串

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        s1 = ''
        length = len(s)
        # 设置窗口大小
        for i in range(1, length + 1):
            # 从最左侧开始
            left = 0
            # 窗口大小
            right = left + i
            # 移动窗口
            while right <= length:
                s2 = s[left:right]
                # 判断子串是否是回文串
                if s2 == s2[::-1] and len(s2) > len(s1):
                    s1 = s2
                left += 1
                right += 1
        return s1
```

### 2.2. 动态规划

构建dp表,遍历字符串,记录每个位置的状态

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        length = len(s)
        dp = [[False]*length for i in range(length)]
        for i in range(length):
            dp[i][i] = True
        s1 = ""
        # 子串长度
        for l in range(length):
           for i in range(length):
                j = i + l
                if j >= length:
                    break
                # 长度为0,代表只有一个元素,一定是回文
                if l == 0:
                    dp[i][j] = True
                # 两个元素,判断是否相等
                elif l == 1:
                    dp[i][j] = (s[i] == s[j])
                # 多个元素,判断最外层另个元素是否相同,并且内层是否是回文串
                else:
                    dp[i][j] = dp[i + 1][j - 1] and (s[i] == s[j])
                # 如果是回文串并且长度比当前长就替换保存的回文串
                if dp[i][j] and ((l + 1) > len(s1)):
                    s1 = s[i: j + 1]

        return s1
```

