# LeetCode 146.LRU缓存机制

## 1. 题目描述

运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果密钥已经存在，则变更其数据值；如果密钥不存在，则插入该组「密钥/数据值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

##### 进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？

```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得密钥 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得密钥 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```

## 2. 题解

### 2.1 暴力解题

思路: 

使用一个字典保存key和value

使用一个列表保存key的修改顺序,越往后代表修改时间约近

不管是put还是get操作,都会使列表顺序改变

注意: 列表的append和pop操作都是O(1),但remove不是,所以这个时间复杂度不符合进阶操作

```python
class LRUCache:

    def __init__(self, capacity: int):
        # 保存key,并保证修改顺序
        self.l = []
        # 存放值
        self.d = {}
        # 缓存大小
        self.n = capacity


    def get(self, key: int) -> int:
        # 如果key存在
        if self.d.get(key) != None:
            # 修改key的修改顺序,改为最近修改
            self.l.remove(key)
            self.l.append(key)
            # 返回值
            return self.d[key]
        else:
            return -1

    def put(self, key: int, value: int) -> None:
        # 如果键已经存在,则在顺序列表中删除
        if self.d.get(key) != None:
            self.l.remove(key)
        # 如果不存在key,切缓存区已满,则删除最久为修改的元素
        elif len(self.l) + 1 > self.n:
            old_key = self.l[0]
            self.l.pop(0)
            self.d.pop(old_key)
       	# 将key设置为最近修改
        self.l.append(key)
        # 将键值对添加到字典中
        self.d[key] = value
```

### 2.2. 双向链表+字典

思路:

由于列表的时间复杂度太高,所以采用链表形式,单链表前一个元素无法使用O(1)获取,所以使用双向链表

字典中保存key和链表节点

每个链表节点都保存了key和value,包括前后节点对象

剩余处理思路和2.1类似,只不过换成了链表

链表最前面代表操作时间最近的key

```python
class DLinkList():
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.next = None
        self.prev = None

class LRUCache:

    def __init__(self, capacity: int):
        # 创建字典,保存key和key对应的链表节点对象
        self.dic = {}
        self.n = capacity
        # 创建头尾节点
        self.head = DLinkList()
        self.tail = DLinkList
        # 设置双向链表
        self.head.next = self.tail
        self.tail.prev = self.head

    def get(self, key: int) -> int:
        if key in self.dic:
            # 将节点修改到链表首位
            link = self.dic[key]
            self.removeLink(link)
            self.addLink(link)
            # 返回结果
            return self.dic[key].value
        else:
            return -1

    def put(self, key: int, value: int) -> None:
        # 如果键存在
        if key in self.dic:
            # 删除节点
            self.removeLink(self.dic[key])
        # 如果键不存在且缓存区已满
        elif len(self.dic) + 1 > self.n:
            # 删除最后的节点
            over_link = self.tail.prev
            self.removeLink(over_link)
            # 字典中弹出
            self.dic.pop(over_link.key)

        # 创建节点
        new_link = DLinkList(key, value)
        # 添加或更新到字典
        self.dic[key] = new_link
        # 添加到链表最前面
        self.addLink(new_link)

    def addLink(self, new_link):
        """
        添加结点到最前面
        """
        link = self.head.next
        self.head.next = new_link
        new_link.next = link
        link.prev = new_link
        new_link.prev = self.head

    def removeLink(self, link):
        """
        删除节点
        """
        link.prev.next = link.next
        link.next.prev = link.prev
```

