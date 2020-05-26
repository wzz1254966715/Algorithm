# LeetCode 7. 整数翻转

## 1. 题目描述

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

```
输入: 123
输出: 321

输入: -123
输出: -321

输入: 120
输出: 21

注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
```

## 2. 题解

### 2.2. 字符串

转换为字符串翻转在合成整型数字

```python
class Solution:
    def reverse(self, x: int) -> int:
        # 判断x的正负
        flag = -1 if x < 0 else 1
        # 取x的绝对值,转换为字符串并翻转, 转换为整数,并乘上原来的符号
        x = int(str(abs(x))[::-1]) * flag
        # 判断x的值是否在题目要求的取值范围内
        return x if -2**31-1 < x < 2**31 else 0
```

### 2.3. 数学

整除求余运算

```python
class Solution:
    def reverse(self, x: int) -> int:
        flag = -1 if x < 0 else 1
        x = abs(x)
        y = 0
        while x:
        	y = y*10 + x % 10
        	x //= 10
        	y *= flag

		return y if -2**31 -1 < y  < 2**31 else 0
```

