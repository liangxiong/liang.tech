# 总结：

## 需求：
- 全局唯一性：不能出现重复的ID号，既然是唯一标识，这是最基本的要求
- 趋势递增：在MySQL InnoDB引擎中使用的是聚集索引，由于多数RDBMS使用B-tree的数据结构来存储索引数据，在主键的选择上面我们应该尽量使用有序的主键保证写入性能。
- 单调递增：保证下一个ID一定大于上一个ID，例如事务版本号、IM增量消息、排序等特殊需求。
- 信息安全：不可猜测，ID不能是连续的

## 非功能性需求：
- 平均延迟和TP999延迟都要尽可能低
- 可用性5个9
- 高QPS


## 其他方案：
### UUID
- 缺点：
  - 不易存储：16字节128位，通常以36长度的字符串表示
  - 非聚集索引都会包含主键列

### Mongo ID
- 时间 + 机器码 + pid + inc 4 + 3 + 2 + 3 = 12个字节


### 数据库生成
```
REPLACE INTO Tickets64 (stub) VALUES ('b');
SELECT LAST_INSERT_ID();
```
- 优点：
  - 非常简单，利用现有数据库系统的功能实现，成本小，有DBA专业维护。
  - ID号单调自增，可以实现一些对ID有特殊要求的业务。

- 缺点：
  - ID发号性能瓶颈限制在单台MySQL的读写性能。
  - 强依赖DB，当DB异常时整个系统不可用，属于致命问题
    - 主从切换时的不一致可能会导致重复发号

#### DB 不可用,性能解决：[Flickr] (http://code.flickr.net/2010/02/08/ticket-servers-distributed-unique-primary-keys-on-the-cheap/)
  - 增长步长设置为0
  - 一个机器从1开始，另外一台从2开始
```
TicketServer1:
# 自增长字段每次递增的量，其默认值是1，取值范围是1 .. 65535
auto-increment-increment = 2  
# 自增长字段从那个数开始 取值范围是1 .. 65535
auto-increment-offset = 1

TicketServer2:
auto-increment-increment = 2
auto-increment-offset = 2
```

- 缺点：
  - 系统水平扩展比较困难，定义好了步长和机器台数之后，要添加机器很麻烦
  - ID没有了单调递增的特性，只能趋势递增，这个缺点对于一般业务需求不是很重要，可以容忍
  - 每次获取ID都得读写一次数据库，只能靠堆机器来提高性能






## 参考
- [美团Leaf](https://tech.meituan.com/2017/04/21/mt-leaf.html)
