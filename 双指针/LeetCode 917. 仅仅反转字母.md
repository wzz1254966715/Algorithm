# LeetCode 917. 仅仅反转字母

## 1. 题目描述:

给定一个字符串 `S`，返回 “反转后的” 字符串，其中不是字母的字符都保留在原地，而所有字母的位置发生反转。

```
输入："ab-cd"
输出："dc-ba"

输入："a-bC-dEf-ghIj"
输出："j-Ih-gfE-dCba"

输入："Test1ng-Leet=code-Q!"
输出："Qedo1ct-eeLg=ntse-T!"

提示：
    S.length <= 100
    33 <= S[i].ASCIIcode <= 122 
    S 中不包含 \ or "
```

## 2. 题解

思路: 双指针

将字符串转换为列表(字符串无法交换元素位置, 不可变数据类型)

左右指针向中间位置遍历列表, 如果遇到字母就交换, 如果某一个指针遇到非字母字符, 则循环后移

直到指针相遇, 已经翻转完毕

```python
class Solution:
    def reverseOnlyLetters(self, S: str) -> str:
        # 构建左右指针
        left = 0
        right = len(S) - 1
        # 字符串转换为列表
        S = list(S)
        # 遍历列表
        while left < right:
            # 如果左指针遇到非字母, 则后移
            while left < right and not S[left].isalpha():
                left += 1
            # 如果右指针遇到非字母, 则前移
            while left < right and not S[right].isalpha():
                right -= 1
            # 当两个指针都指向字母, 交换字符位置
            S[left], S[right] = S[right], S[left]
            # 移动指针
            left += 1
            right -= 1
        # 将列表转换为字符串返回
        return ''.join(S)
```

 