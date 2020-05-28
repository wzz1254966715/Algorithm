# LeetCode 394. 字符串解码

## 1. 题目描述

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

```
s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".
```

## 2. 题解

正则表达式

循环匹配替换,匹配格式: (数字一个至多个)[(字母一个或多个)]

注意:

​	数字使用\d+匹配

​	字母使用\w+匹配(题目中说没有特殊字符,所以直接使用关键字匹配)

​	分组,由于需要的值是数字和字母,所以直接分组,提取的时候好提取

```python
class Solution:
    def decodeString(self, s: str) -> str:
        # 正则表达式: 匹配最内层格式为: 数字[字母]
        patten = re.compile(r'(\d+)\[(\w+)\]')
        while True:
            ss = patten.search(s)
            if ss != None:
                # 获取数字,转型,乘上字符,替换到原来的位置,只替换一个
                s = patten.sub(int(ss.group(1)) * ss.group(2), s, 1)
            else:
                break
        return s
```

