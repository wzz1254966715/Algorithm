# LeetCode 64. 求1+2+...+n

## 1. 题目描述

求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

```
输入: n = 3
输出: 6

输入: n = 9
输出: 45
```

**限制：**

- `1 <= n <= 10000`

## 2. 题解

### 2.1. 递归

n + n-1 +...+0

```python
class Solution:
    def sumNums(self, n: int) -> int:
    	return 0 if n==0 else (n + self.sumNums(n - 1))
```

### 2.2. 循环

```python
class Solution:
    def sumNums(self, n: int) -> int:
    	s = 0
    	while n:
    		s += n
    		n -= 1
    	return s
```

### 2.3. 内置函数

序列求和

```python
class Solution:
    def sumNums(self, n: int) -> int:
    	return sum(range(1, n+1))
```

