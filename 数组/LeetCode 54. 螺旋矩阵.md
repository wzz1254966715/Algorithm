# LeetCode 54. 螺旋矩阵

## 1. 题目描述

给定一个包含 *m* x *n* 个元素的矩阵（*m* 行, *n* 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]

输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## 2. 题解

思路: 按照顺时针的顺序,一层一层的遍历

构建四个顶点,上下左右,形成一个环,顺时针遍历环

收缩四个顶点,形成一个新的环

直到收缩到最后一个环,或者最后一行(4X4或者3X4)

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        # 判空
        if not len(matrix) or not len(matrix[0]):
            return []
        
        # 构建顶点
        top, left, bottom, right = 0, 0, len(matrix)-1, len(matrix[0])-1
        # 构建一维列表用于返回
        l = []

        # 右侧大于左侧或者下侧大于上侧时,代表已经全部遍历完成
        while left<=right and top<=bottom:
            # 从左到右遍历
            for c in range(left, right+1):
                l.append(matrix[top][c])
            # 从上到下遍历(注意: 最上面的元素已经在上面遍历完了,所以从第二个元素开始)
            for r in range(top+1, bottom+1):
                l.append(matrix[r][right])
            '''
              判断一下:
              如果是大于则遍历完成,
              如果是等于则代表剩余一行,直接从左到右遍历就好了
            '''
            if left < right and top < bottom:
                # 从右到左遍历(注意: 最右边的元素已经被遍历,从第二个元素开始)
                for c in range(right-1, left-1, -1):
                    l.append(matrix[bottom][c])
                # 从下到上遍历(注意: 最上边和最下边的元素都已经被遍历)
                for r in range(bottom-1, top, -1):
                    l.append(matrix[r][left])
            # 收缩环
            top, left, bottom, right = top+1, left+1, bottom-1, right-1

        return l
```

