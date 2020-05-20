# Buffle Pool

- 默认情况下Buffer Pool：128M，最小值为5M

[server]
innodb_buffer_pool_size = 268435456

- Buffer Pool内部组成：
  - 由：控制块 + 页组成
  - 控制块：MySQL5.7.21 808字节
    - 表空间号、页号之类的信息
  - 页：16KB，innodb_buffer_pool_size 只是配置了页的大小

- 空闲 free链表：
  - 空闲 没使用的 buffer pool

- 已缓存：
  - 表空间号 + 页号作为key，缓存页作为value

- 已修改页 flush链表：
  - 修改了Buffer Pool中某个缓存页的数据，那它就和磁盘上的页不一致了，这样的缓存页也被称为脏页
  - 存储脏页的链表

- 淘汰策略
  - LRU链表（LRU的英文全称：Least Recently Used）
  - 分：
    - young区域 热区域
    - old区域 冷区域   默认 37%
      - SHOW VARIABLES LIKE 'innodb_old_blocks_pct'; SET GLOBAL innodb_old_blocks_pct = 40;
      - [server] innodb_old_blocks_pct = 40

- 线性预读 ， 随机预读
  - 优缺点

- 多个Buffer Pool实例
[server]
innodb_buffer_pool_instances = 2
在Buffer Pool大于或等于1G的时候设置多个Buffer Pool实例。

Buffer Pool Instance
  chunk  innodb_buffer_pool_chunk_size 134217728 128M
    页 + 控制块

SHOW ENGINE INNODB STATUS\G
