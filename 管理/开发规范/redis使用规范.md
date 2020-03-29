# redis使用规范

#### key名设计  
- 注意：可读性和可管理性
    - 以业务名(或数据库名)为前缀，用冒号分隔，比如 业务名:表名:id

- 简洁性
    - 保证语义的前提下，控制key的长度，当key较多时，内存占用也不容忽视
    - 如：user:{uid}:friends:messages:{mid}简化为u:{uid}:fr:m:{mid}。
    - 不要包含特殊字符：空格、换行、单双引号以及其他转义字符

####  value设计  
- 拒绝bigkey(防止网卡流量、慢查询)
    - string类型控制在10KB以内，
    - hash、list、set、zset元素个数不要超过5000
    - 不要使用del删除 非字符串的bigkey, 使用hscan、sscan、zscan方式渐进式删除  因为 时间复杂度是 O(N), hscan、sscan、zscan方式渐进式删除
    - 遇到大bigkey 过期，会触发del，造成主线程阻塞
- 一定设置过期时间，不是垃圾桶



#### 技巧
- O(N)命令： 注意如果遇到bigkey，阻塞整个线程，需要指定数量
- 遍历可以使用：hscan、sscan、zscan  渐进处理
- 批量操作支持提高效率，注意别超过500，实际和元素字节有关
    - 原生：mget, mset
    - 非原生，非原子：pipeline  打包命令实现
- 多数据库较弱，都是数字表示
- 禁用命令 ：keys、flushall、flushdb   通过redis的rename机制禁掉命令
- monitor ： 监控当前正在执行的命令
- 事务：redis 不支持事务回滚，



#### 配置
- 算法策略：
    - 默认：volatile-lru，即超过最大内存后，在过期键中使用lru算法进行key的剔除，保证不过期数据不被删除，但是可能会出现OOM问题
    - allkeys-lru：根据LRU算法删除键，不管数据有没有设置超时属性，直到腾出足够空间为止
    - allkeys-random：随机删除所有键，直到腾出足够空间为止
    - volatile-random: 随机删除过期键，直到腾出足够空间为止
    - volatile-ttl：根据键值对象的ttl属性，删除最近将要过期数据。如果没有，回退到noeviction策略
    - noeviction：不剔除数据，拒绝所有写入操作并返回客户端错误信息"(error) OOM command not allowed when used memory"，此时Redis只响应读操作



#### 客户端使用
- 避免多个应用使用一个Redis实例
- 高并发下建议客户端添加熔断功能(例如netflix hystrix)
- 设置合理的密码，如有必要会用SSL访问
