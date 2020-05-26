# LeetCode 9. 回文数

## 1. 题目描述

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

```
输入: 121
输出: true

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。

进阶:
你能不将整数转为字符串来解决这个问题吗？
```

## 2. 题解

### 2.1. 字符串处理

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        # 转换为字符串
        x = str(x)
        # 翻转字符串
        y = x[::-1]
        if x == y:
            return True
        else:
            return False
      # 不满足进阶
```

### 2.2. 取余

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
    	if x < 0:
            return False
       	if x < 10:
            return True
        
        m = x
        n = 0
        while m:
            n = n * 10 + m % 10
            m //= 10
        return True if x == n else False
```

### 2.3. 取余进阶

2.2.是将整个数字翻转,但如果是回文数,只翻转一半就够了

如何判断翻转了一半?  假设是回文数,当x不大于翻转数字的时候就是翻转了一半了

有什么问题: 需要提前排除可以被10整除的数

翻转后,有两种情况:

 1. 奇数个数: 则x等于翻转数整除10,吧最中间的数排除

 2. 偶数个数: 则x直接与翻转数相等

    ```python
    class Solution:
        def isPalindrome(self, x: int) -> bool:
            if x < 0 or (x % 10 == 0 and x != 0):
                return False
            if x < 10:
                return True
            n = 0
            while x > n:
                n = n * 10 + x % 10
                x //= 10
    
            return True if x==n or x == n//10 else False
    ```

    