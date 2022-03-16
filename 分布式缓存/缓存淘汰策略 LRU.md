## 为啥要淘汰

缓存存储在内存，内存有限。缓存满了，得考虑留下的价值。

## 淘汰策略

### FIFO(First In First Out)

先进先出，like队列。

生命期固定，到了就出局，即使访问频繁

问题：无法动态调整价值

### LFU(Least Frequently Used)

按访问的次数决定价值

生命期取决于访问频率

问题：收敛慢，以前价值高，未来不一定价值高；维护频次需要的额外开销大

### LRU(Least Recently Used)

相对折中的方案，在FIFO的基础上当数据被访问就重置生命期，兼顾了访问频次的问题。



## LRU算法

### 数据结构

![lru](lru.jpg)

双向链表+map

Map[string]*entry

List node value : entry

### 查找

- 访问map找到在链表中对应的节点
- 将其移到队首

### 删除（淘汰）

删除对象：队尾节点

- 删除节点
- 删除map项
- 重新计算空间

### 新增/更新

- 存在则更新
- 不存在则添加 list && map
- 更新空间