# LeetCode 67. 二进制求和

## 1. 题目描述

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`

**示例 1:**

```
输入: a = "11", b = "1"
输出: "100"
```

**示例 2:**

```
输入: a = "1010", b = "1011"
输出: "10101"
```

**提示：**

- 每个字符串仅由字符 `'0'` 或 `'1'` 组成。
- `1 <= a.length, b.length <= 10^4`
- 字符串如果不是 `"0"` ，就都不含前导零。

## 2. 题解

### 2.1. 类型强制转换

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        # 使用int()方法将字符串强转为整型, 且设置base参数为2, 代表二进制转十进制整型
        # 使用格式化输出, 将十进制整型按照二进制的字符串输出
        return '{0:b}'.format(int(a, 2) + int(b, 2))
```

### 2.2. 模拟

使用循环模拟二进制的加法

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        c = ''
        size = max(len(a), len(b))
        t = 0
        for i in range(1, size + 1):
            # 模拟二进制加法, 从后向前遍历
            t += ord(a[-i]) - ord('0') if i <= len(a) else 0
            t += ord(b[-i]) - ord('0') if i <= len(b) else 0
            # 满二进一
            c += str(t % 2)
            t //= 2
        if t > 0:
            c += '1'
        return c[::-1]
```

