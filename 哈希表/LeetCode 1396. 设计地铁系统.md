# LeetCode 1396. 设计地铁系统

## 1. 题目描述

请你实现一个类 UndergroundSystem ，它支持以下 3 种方法：

1. checkIn(int id, string stationName, int t)

   编号为 id 的乘客在 t 时刻进入地铁站 stationName 。
   一个乘客在同一时间只能在一个地铁站进入或者离开。

2. checkOut(int id, string stationName, int t)

   编号为 id 的乘客在 t 时刻离开地铁站 stationName 。

3. getAverageTime(string startStation, string endStation) 

   返回从地铁站 startStation 到地铁站 endStation 的平均花费时间。
   平均时间计算的行程包括当前为止所有从 startStation 直接到达 endStation 的行程。
   调用 getAverageTime 时，询问的路线至少包含一趟行程。
   你可以假设所有对 checkIn 和 checkOut 的调用都是符合逻辑的。也就是说，如果一个顾客在 t1 时刻到达某个地铁站，那么他离开的时间 t2 一定满足 t2 > t1 。所有的事件都按时间顺序给出。

   ```
   说明:
   1. 调用getAverageTime时,会保证这个路线一定存在至少一个
   2. 平均时间: 所有这个路线的总消耗时间除以总人数
   3. 时间不需要判断,一定是先进站后出站
   ```

   

```
输入：
["UndergroundSystem","checkIn","checkIn","checkIn","checkOut","checkOut","checkOut","getAverageTime","getAverageTime","checkIn","getAverageTime","checkOut","getAverageTime"]
[[],[45,"Leyton",3],[32,"Paradise",8],[27,"Leyton",10],[45,"Waterloo",15],[27,"Waterloo",20],[32,"Cambridge",22],["Paradise","Cambridge"],["Leyton","Waterloo"],[10,"Leyton",24],["Leyton","Waterloo"],[10,"Waterloo",38],["Leyton","Waterloo"]]

输出：
[null,null,null,null,null,null,null,14.0,11.0,null,11.0,null,12.0]

解释：
UndergroundSystem undergroundSystem = new UndergroundSystem();
undergroundSystem.checkIn(45, "Leyton", 3);
undergroundSystem.checkIn(32, "Paradise", 8);
undergroundSystem.checkIn(27, "Leyton", 10);
undergroundSystem.checkOut(45, "Waterloo", 15);
undergroundSystem.checkOut(27, "Waterloo", 20);
undergroundSystem.checkOut(32, "Cambridge", 22);
undergroundSystem.getAverageTime("Paradise", "Cambridge");       // 返回 14.0。从 "Paradise"（时刻 8）到 "Cambridge"(时刻 22)的行程只有一趟
undergroundSystem.getAverageTime("Leyton", "Waterloo");          // 返回 11.0。总共有 2 躺从 "Leyton" 到 "Waterloo" 的行程，编号为 id=45 的乘客出发于 time=3 到达于 time=15，编号为 id=27 的乘客于 time=10 出发于 time=20 到达。所以平均时间为 ( (15-3) + (20-10) ) / 2 = 11.0
undergroundSystem.checkIn(10, "Leyton", 24);
undergroundSystem.getAverageTime("Leyton", "Waterloo");          // 返回 11.0
undergroundSystem.checkOut(10, "Waterloo", 38);
undergroundSystem.getAverageTime("Leyton", "Waterloo");          // 返回 12.0
```

## 2. 题解

##### 思路:

使用字典存储

2.1. 进站字典:

​	key: id

​	value: 进站名称和时间

​	作用: 用于出站时获取每个人的行程和消耗时间

2.2. 出站字典: 

​	key: 进站名称和出站名称也就是行程以元组的形式存放

​	value: 当前行程的总消耗时间和总人数

​	作用: 保存每个行程所消耗的总时间和总人数

2.3. 平均时间

​	按照进站点和出站点在出站字典中查找消耗总时间和总人数,做除法,得到平均消耗时间

```python
class UndergroundSystem:

    def __init__(self):
        # 进站字典
        self.in_dic = {}
        # 出站字典
        self.out_dic = {}

    def checkIn(self, id: int, stationName: str, t: int) -> None:
        # 添加进站信息
        self.in_dic[id] = (stationName, t)

    def checkOut(self, id: int, stationName: str, t: int) -> None:
        # 获取进站时间
        start_time = self.in_dic[id][1]
        # 获取出发站和终点站名称
        start_end_name = (self.in_dic[id][0], stationName)
        # 获取出发站和终点站原有消耗时间和总人数, 缺省为(0, 0)
        start_end_sum = self.out_dic.get(start_end_name, (0, 0))
        # 累加消耗时间和总人数
        self.out_dic[start_end_name] = (start_end_sum[0] + t - start_time, start_end_sum[1] + 1)

    def getAverageTime(self, startStation: str, endStation: str) -> float:
        # 获取当前出发站-终点站的总消耗时间和总人数
        # 由于行程一定存在,所以不需要使用get方法,直接取值就可以
        total_time, total_people = self.out_dic[(startStation, endStation)]
        # 直接除就好,不要使用整除,小数部分不能省,会报错....
        return total_time / total_people
```

