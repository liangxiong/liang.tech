# Zebra 设计方式

## 概况
- 基于JDBC API协议上开发出的高可用、高性能的数据库访问层解决方案
  - 配置集中管理，动态刷新
  - 支持读写分离、分库分表
  - 丰富的监控信息在CAT上展现
  - 异步化数据库请求，多数据源支持

- 架构：
  - 最上层的是ShardDataSource
    - 用于进行分库分表。
    - 包含了若干个GroupDataSource，每个连接的数据库集群相当于1个分片(Shard)。

  - 中间一层是GroupDataSource
    - 主要用于读写分离。下面通过一组SingleDataSource连接一个数据库服务组集群，分为一个主和若干个从。

  - 最下一层是SingleDataSource
    - 和mysql集群中的单个mysql实例直接建立连接。
      - 支持6种连接池：dbcp、dbcp2、druid、tomcat-jdbc、c3p0，hikaricp
      - 用户无需直接使用SingleDataSource

  - 概况：
    - ShardDataSource、GroupDataSource都实现了JDBC协议的javax.sql.DataSource接口
    - 可以把二者都当做一个普通的数据库连接池来使用
    - 所有读写分离、分库分表的底层实现逻辑，都对用户进行了屏蔽

## 整体设计

### 数据源
#### 读写分离 GroupDataSource

#### 分库分表 ShardDataSource



### 路由设计
- 按流量权重路由 会根据优先级选择对应中心内所有读库按权重路由，未开启则整个区域内按权重路由
- 按机房路由，路由顺序如下:
  - 选择同机房的读库，按流量权重路由
  - 选择同中心其他读库，按流量权重路由
  - 按优先级选择其他机房内读库，按流量权重路由
  - 选择非中心的读库，按流量权重路由
https://github.com/Meituan-Dianping/Zebra/wiki/Zebra%E8%B7%AF%E7%94%B1%E8%AE%BE%E8%AE%A1
https://raw.githubusercontent.com/wiki/Meituan-Dianping/Zebra/image/zebra-router-design.png

### 流量控制
https://github.com/Meituan-Dianping/Zebra/wiki/Zebra%E6%B5%81%E9%87%8F%E6%8E%A7%E5%88%B6

- 动态限流，可动态配置限流策略与流量大小

- 支持限制某个数据源上的某些特定的SQL（sqlId）或者某些类型的SQL

- 支持对事务的限流优化（特定条件下生效）

- 名词：
  - SqlId：mybatis mapper 的 方法名
  - filter
  - rule
