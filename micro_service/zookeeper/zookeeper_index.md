# zookeeper： Zoo+keeper

## what：
-  分布式应用协调服务的框架，解决分布式一致性问题
- 通过解决 节点，数据的变化达到集群的管理

- 解决：
  - 分布式锁
  - 分布式协调
  - 分布式消息队列

- 特性：    
    - 数据结构是 类似 标准的文件目录结构，一个树结构
    - znode：一个节点
        - 每个节点都可以存在子节点，除了临时节点不能有子节点：EPHEMERAL
        - 每个节点都可以存储数据，可以存储多版本数据
    - EPHEMERAL 临时节点
        - 当客户端与服务端 失去联系，自动删除。
        - 客户端与服务端保持长连接，称为：session
    - 节点可以被监控：当数据修改，子目录结构变更    

## 启动：
- down:
  - wget http://apache.claz.org/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
- config
  - cd zookeeper/conf
  - cp zoo_sample.cfg zoo.cfg
- config paramter:
  - tickTime=2000  滴答时间
  - initLimit=10   Leader 等待 Follower节点完成数据同步最大次数  时间：initLimit * tickTime
  - syncLimit=5   Leader 等待 Follower 心跳的最大延时 时间：syncLimit * tickTime
  - dataDir=/tmp/zookeeper    存放快照数据的目录
  - clientPort=2181
  - maxClientCnxns：最大客户端连接数。如果客户端的数量超过了指定的数量，则新连接的客户端会抛出java.io.IOException: Connection reset by peer异常。
  - 集群配置：最少3台，使用奇数
    - server.A=B:C:D
    - A：数字，表示第几号实例
    - B：ip
    - C：与leader交互的端口
    - D：选举时服务器相互通信的端口

- run
    - server：./zkServer.sh start
    - client：./zkCli.sh

## 使用：
- 注意：
  - path：大小写敏感
- 帮助: help

- 创建节点
  - create /path data

- 获取数据
    - get /path
    - 字段：
      - cZxid：0x0 当前节点事务的xxid。
      - mZxid:  0x0 当前节点最近修改的xxid
      - ctime：Thu Jan 01 08:00:00 HKT 1970 当前节点的创建时间。
      - mtime：Thu Jan 01 08:00:00 HKT 1970 这个节点的最后修改时间。
      - pZxid ： 0x0 子节点的最后版本
      - cversion：这个节点的子节点变化次数，创建一个子节点会加1。
      - dataVersion：数据的版本，每次修改会加一。
      - aclVersion：这个节点的ACL变化次数。
      - ephemeralOwner：如果这个节点是临时节点，表示创建者的会话id。如果不是临时节点，这个值是0。
      - dataLength：这个节点存放的数据长度。
      - numChildren：这个节点的子节点个数。


- 修改数据
    - set /path data

- 删除节点：
    - delete /path data

- 其他：
    - 查看历史命令：history
    - 退出：quit

## 数据结构：
- PERSISTENT
    - 持久化目录节点，存储的数据不会丢失。
- PERSISTENT_SEQUENTIAL
     - 顺序自动编号的持久化目录节点，存储的数据不会丢失，并且根据当前已近存在的节点数自动加 1，然后返回给客户端已经成功创建的目录节点名。
- EPHEMERAL
     - 临时目录节点，一旦创建这个节点的客户端与服务器端口也就是session 超时，这种节点会被自动删除。
- EPHEMERAL_SEQUENTIAL
     - 临时自动编号节点，一旦创建这个节点的客户端与服务器端口也就是session 超时，这种节点会被自动删除，并且根据当前已近存在的节点数自动加 1，然后返回给客户端已经成功创建的目录节点名。

## 典型场景
### 配置管理
- 结构：
    - /configuration
        - xx应用
            - xx 日常环境1：get data / watch（变化通知）
            - xx 预发环境2：get data / watch（变化通知）
            - xx 生产环境3：get data / watch（变化通知）
- 过程：
    - 启动过程：先主动获取数据   get /configuration/taobao/production
    - 运行过程：主动监听数据的变更

### 集群管理
- 使用EPHEMERAL节点，当下线后自动删除
- client 进行watch，会收到通知
- 场景：
    - hadoop 集群的 nameNode
    - hbase 的Master Election、Server 之间状态同步
- 结构：
    - /taobao  watch（变化通知）
        - EPHEMERAL server 1
        - EPHEMERAL server 2
        - EPHEMERAL server xxx
- 过程:
    - 启动过程：主动获取该应用下的所有节点
    - 停止过程：这个EPHEMERAL 节点自动删除，zk 主动对其他server  的通知
    - 运行过程：接受通知，主动该应用下的所有节点

### Leader 选举
- 使用 EPHEMERAL SEQUENTIAL
- 当master 死，选举最小的节点做master节点
- 场景：
    - Master 选举
- 结构：
    - /taobao_leader watch（变化通知）
        - EPHEMERAL_SEQUENTIAL server 1
        - EPHEMERAL_SEQUENTIAL server 2
        - EPHEMERAL_SEQUENTIAL server xxx
- 过程：
    - 启动过程：
        - 注册并监听 临时顺序节点
        - 获取数据，判断最小节点是否是自己
        - 如果是自己，设置自己为leader
    - 死亡时候：自己的临时顺序节点自动删除，其他节点收到通知
    - 运行过程：收到节点编号通知，获取数据，判断自己是否是最小，如果最小，设置自己为leader

### 共享锁（Locks）
- 也是利用 leader 选举的过程
- 性能慢

## 内部实现：
### 选举：
- zab协议：
  - 模式：
    - 恢复模式（选主）：选主 + server 和 leader 同步完数据
    - 广播模式（同步）：

- server 的三种状态：
  - LOOKING：Server不知道leader是谁，正在搜寻
  - LEADING：当前Server即为选举出来的leader
  - FOLLOWING：leader已经选举出来，当前Server与之同步



- 过程：todo
    - 2种实现：
        - basic paxos
        - fast paxos 默认
## 参考：
- 官网：https://zookeeper.apache.org/doc/r3.4.6/zookeeperProgrammers.html#sc_zkStatStructure

- ibm：https://www.ibm.com/developerworks/cn/opensource/os-cn-zookeeper/index.html

- http://ifeve.com
