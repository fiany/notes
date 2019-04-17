## redis特性

### 为什么那么快（100000+QPS）

- 完全基于内存，绝大部分请求是纯粹的内存操作，执行效率高。
- 数据结构简单，最数据操作也简单

- 采用单线程，单线程也能处理高并发请求，想多核也可以启动多实例
- 使用多路I/O复用模型，非阻塞IO

### 数据类型

- String : 最基本的数据类型，二进制安全
  - set
  - get
- Hash : String元素组成的字典，适合用于存储对象
  - hmset
  - hget
- List : 列表，按照String元素插入顺序排序
  - lpush
  - lrange
- Set : String元素组成的无序集合，通过哈希表实现，不允许重复（可以计算交集，并集，差集）
  - sadd
  - smembers 查看所有
- Sorted Set : 通过分数来为集合中的成员进行从小到大的排序
  - zadd
  - zrangebysore myzset 0 10
- 用于技术的HyperLogLog， 用于支持存储地理位置信息的Geo

### 从海量Key里查询出某一固定前缀的Key

- 确认元素数量

- KEYS pattern : 查找所有符合给定模式pattern的key
  - KEYS指令一次性返回所有匹配的key
  - 键的数量过大回事服务卡顿
- SCAN cursor [MATCH pattern] [COUNT count]
  - 基于游标的迭代器，需要基于上一次的游标延续之前的迭代过程
  - 以0作为游标开始一次新的迭代，知道命令返回游标0完成一次遍历
  - 不保证每次执行都返回某个给定数量的元素，支持模糊查询
  - 一次返回的数量不可控，只能是大概率符合count参数

### 分布式锁

#### 需要解决的问题

- 互斥性
- 安全性
- 死锁
- 容错

#### redis实现

- setnx locknx test
- getset
- set key value [][][EX seconds] [Px milliseconds] [NX|XX] 原子性的setnx并且带有过期时间

### redis消息队列

使用List作为队列，rpush生产消息，lpop消费消息

- 缺点：没有等待队列里有值就直接去消费
- 弥补：可以通过在应用层引入sleep机制去调用lpop重试

blpop阻塞会等待队列有消息，或者超时

- 缺点：只能供一个消费者消费

pub/sub：主题订阅者模式

- 消息的发布是无状态的，无法保证可达

### redis持久化

- rdb（快照）持久化：保存某个时间点的全量数据快照
  - save：阻塞redis的服务器进程，知道rdb文件被创建完毕
  - bgsave：fork出一个子进程来创建rdb文件，不阻塞服务器进程

- aof(append-only-file) 持久化:保存写状态
  - 记录下 除了查询以外的所有变更数据库状态的指令
  - 以append的形式追加保存到aof文件中（增量）
- rdb-aof混合持久化方式
  - bgsave做镜像全量持久化，aof做增量持久化
- redis数据的恢复
  - rdb和aof文件共存情况下的恢复流程

### redis集群原理

#### 从海量数据中快速找到所需

- 分片：按照某种规则去划分数据，分散存储在多个节点上
- 常规的按照哈希划分无法实现节点的动态增减

#### 一致性哈希算法：对2^32取模，将哈希值空间组织成虚拟的圆环

- Hash环的数据倾斜问题
  - 解决：引入虚拟节点解决数据倾斜问题

