# LeetCode 76. 最小覆盖子串

## 1. 题目描述

给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字符的最小子串

```
输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"

说明：
如果 S 中不存这样的子串，则返回空字符串 ""。
如果 S 中存在这样的子串，我们保证它是唯一的答案。
```

## 2. 题解

### 滑动窗口

子串问题一般采用滑动窗口来解决

#### 解题思路:

1. 从左向右不停扩张窗口,直到窗口中包含了所有T串中的字符

2. 固定右侧窗口,移动左侧窗口,期间记录子串的开始和结束位置(判断是否小于当前子串长度)
3. 直到窗口中不能完全包含T串时,继续向右扩张
4. 重复执行,直到窗口移动到S串末尾,结束
5. 返回结果,两种可能性,找到和没找到,找到就返回子串,没找到就返回空串

#### 具体实现:

由于有重复的字符,所以使用两张字典

字典1用来作为窗口的字符记录,记录窗口中包含T字符串中的字符以及字符个数

字典2用来作为T串的字符记录,记录T串中包含的字符和字符个数

变量: 

1. 左右指针,也就是窗口两端

2. 子串的起始位置和长度,用来返回结果时取值
3. 记录窗口中各字符串满足的个数

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        win, t_dic  = {}, {}
        # 记录t字符串到字典中
        for i in t:
            if t_dic.get(i):
                t_dic[i] += 1
            else:
                t_dic[i] = 1
        # 设置左右指针
        left, right = 0, 0
        # 子串开始位置和长度,首个长度设置足够大
        start, end = 0, len(s) + 1
        # 记录win字典中是否满足了条件(内部包含t串的所有字符)
        count = 0
        # 开始滑动吧!
        while right < len(s):
            ch = s[right]
            # 向右移动窗口
            right += 1
            # 如果是t串中的字符就放到win中
            if t_dic.get(ch):
                if win.get(ch):
                    win[ch] += 1
                else:
                    win[ch] = 1
                # 窗口中的当前字符个数与t中一致,就将满足的个数加1
                if win[ch] == t_dic[ch]:
                    count += 1
            # count等于t_dic长度时,代表窗口中包含了所有t串中的字符
            while count == len(t_dic):
                # 记录子串的其实位置和长度
                if (right-left) < end:
                    start = left
                    end = right - left
                # 记录将要移出窗口的字符   
                ch = s[left]
                # 向左收缩窗口
                left += 1
                # 移除的字符是否在t串中
                if ch in t:
                    # 如果移除的字符在窗口中,则窗口计数-1
                    if t_dic[ch] == win[ch]:
                        # 如果移除后窗口不满足,则将满足数-1
                        count -= 1
                    win[ch] -= 1
        # 判断是否包含子串
        if end == len(s) + 1:
            return ""
        else:
            # 返回子串,起始位置到起始位置加子串长度
            return s[start : start + end]
```

