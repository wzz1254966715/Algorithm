# LeetCode 874. 模拟行走机器人

## 1. 题目描述

机器人在一个无限大小的网格上行走，从点 (0, 0) 处开始出发，面向北方。该机器人可以接收以下三种类型的命令：

-2：向左转 90 度
-1：向右转 90 度
1 <= x <= 9：向前移动 x 个单位长度
在网格上有一些格子被视为障碍物。

第 i 个障碍物位于网格点  (obstacles\[i][0], obstacles\[i][1])

机器人无法走到障碍物上，它将会停留在障碍物的前一个网格方块上，但仍然可以继续该路线的其余部分。

返回从原点到机器人所有经过的路径点（坐标为整数）的最大欧式距离的平方 

```
输入: commands = [4,-1,3], obstacles = []
输出: 25
解释: 机器人将会到达 (3, 4)

输入: commands = [4,-1,4,-2,4], obstacles = [[2,4]]
输出: 65
解释: 机器人在左转走到 (1, 8) 之前将被困在 (1, 4) 处

提示：

0 <= commands.length <= 10000
0 <= obstacles.length <= 10000
-30000 <= obstacle[i][0] <= 30000
-30000 <= obstacle[i][1] <= 30000
答案保证小于 2**31
```

## 2. 题解

思路: 模拟行走

向上走: x不动,y每次加一

向右走: x每次加一, y不动

向下走: x不动, y每次减一

向左走: x每次减一, y不动

根据对应关系, 生成两个列表, 代表四个方向, x和y每次应该加的值

一个变量控制方向, 这个变量需要在上面列表长度之间循环,也就是四个方向循环

```python
class Solution:
    def robotSim(self, commands: List[int], obstacles: List[List[int]]) -> int:
        # 上下左右, x值每一步增加多少
        dx = [0, 1, 0, -1]
        # 上下左右, y值每一步增加多少
        dy = [1, 0, -1, 0]
        # x,y代表当前位置的坐标
        # di 代表当前方向
        # r 代表最大欧式距离平方
        x = y = di = r = 0
        
        # 将二维列表转化为集合, 改变数据结构, 遍历减少时间复杂度
        obstacles_set = set(map(tuple, obstacles))

        # 遍历数组
        for i in commands:
            if i == -1:
                # 右转
                # 保证di在四个方向循环
                di = (di + 1) % 4
            elif i == -2:
                di = (di - 1) % 4
            else:
                # 根据当前方向,取得每步x,y各走多少(0, 1, -1)
                m, n = dx[di], dy[di]
                # 循环内部直接写成x+=dx[di] y+=dy[di]也可以过,但是速度很慢,列表数据结构的问题
                # 开始走
                for j in range(i):
                    # 判断下一步是否是障碍物
                    if (x + dx[di], y + dy[di]) in obstacles_set:
                        break
                    # 移动x,y
                    x += m
                    y += n
                # 判断最大欧氏距离平方和
                r = max(r, x**2 + y**2)

        return r

```

