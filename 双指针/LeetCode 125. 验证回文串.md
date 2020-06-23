# LeetCode 125. 验证回文串

## 1. 题目描述

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

示例 1:

```
输入: "A man, a plan, a canal: Panama"
输出: true
```


示例 2:

```
输入: "race a car"
输出: false
```

## 2. 题解

### 2.1. 清洗字符串

构建一个新的字符串, 只保留原字符串中的字母和数字, 然后判断是否是回文串

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
    	s = s.upper()
    	s1 = ''
    	for i in s:
            # 判断ascll码
    		if 47 < ord(i) < 58 or 64 < ord(i) < 91:
                s1 += i
        return s1 == s1[::-1]
```

### 2.2. 双指针

构建左右指针, 遇到非字母数字直接跳过, 遇到字母数字判断, 不相同返回False

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
    	s = s.upper()
        left, right = 0, len(s)-1
        while left < right:
            # 过滤非字母数字
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1
            if s[left] != s[right]:
                return False
           	left += 1
            right -= 1
        return True
```

