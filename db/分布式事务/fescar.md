# 分布式事务 fescar

- 2PC
    - 准备资源
    - 资源提交

- Saga
    - 补偿协议：业务流程中每个参与者都提交本地事物，当出现某个参与者失败，则补偿前面已经成功的参与者
    - 一阶段正向服务
    - 二阶段逆向回滚操作
    - 依据：
        - Saga 理论出自 Hector & Kenneth 1987发表的论文 Sagas


- Sega 基本流程：
## 架构
![sega](https://github.com/liangxiong/liang.tech/blob/master/db/res/sega.jpg)

```sequence
TM -> TM : 开启分布式事务
TM -> TC : 注册全局事务记录
RM -> TC : 汇报资源准备状态
TM -> TM : 结束分布式事务
TM -> TC : 通知：提交/回滚分布式事务  第一阶段结束
TC -> TC : 汇总事物信息，决定提交还是回滚
TC -> RM : 提交/回滚 资源 第二阶段结束

```



## 模式
### AT（Automatic Transaction）模式
- 优缺点：
    - 无侵入
    - 对操作的数据行数有一定影响
- 流程：
    - 一阶段：关注业务sql
        - 框架拦截业务sql，找到要更新的业务数据，保存为 before image
        - 执行业务sql，更新数据,变更后的保存为 after image
        - 生成行锁

    - 二阶段提交：
        - 框架会自动生成事务的二阶段提交和回滚操作

    - 二阶段回滚：回滚一阶段已经执行的“业务 SQL”，还原业务数据
        - 对比校验 脏写：对比当前数据和after image
        - 用“before image”还原业务数据

### TCC模式： MT（Manual Transaction）
- 优缺点：
    - 业务有一定的侵入性
    - 对性能有很高要求的场景，无AT模式的全局行锁，性能比AT模式高

- 流程：
    - 一阶段Try：资源预留
    - 二阶段提交Confirm：业务操作提交
    - 二阶段回滚Cancel：预留资源释放

#### 遇到的问题：
- 空回滚
    - Try 阶段丢包，没执行，管理器触发 回滚 Cancel
    - 要返回回滚执行成功
- 防悬挂控制：Cancel 比 Try 接口先执行
    - Try 由于 由于网络拥堵而超时，触发了Cancel接口，最终又收到了Try接口调用
    - Try 要检查这条事物xid或业务主键是否已经标记为回滚成功过
- 幂等性
    - Try，Cancel 保持幂等性

### 原则：
- 宁可 长款，不可短款
    - 转账：先扣用户，再入账

## 名词解释

## mysql
- 文档:
    - https://www.cnblogs.com/f-ck-need-u/archive/2018/05/08/9010872.html

- buffer pool -> log buffer  -> os buffer  -> log files

- undo log： 回滚日志
    - 逻辑日志，每行记录进行记录

- redo log
    - innodb层产生
    - 物理日志，记录的是数据页的物理修改，用来恢复 最后一次提交后的物理数据页

- 二进制日志
    - 存储引擎的上层产生，并且先于redo log被记录
    - 逻辑性的语句，基于 行格式的记录方式，每列值是什么
    - 每次事物提交时，一次性写入缓存中的日志文件，

![fescar](https://github.com/liangxiong/liang.tech/blob/master/db/res/fescar.png)

## 结构：
- TM transaction manager：
    - 全局事务管理器，在标注开启fescar分布式事务的服务端开启，并将全局事务发送到TC事务控制端管理

- TC  transaction coordinator：
    - 事务控制中心，控制全局事务的提交或者回滚。这个组件需要独立部署维护，目前只支持单机版本，后续迭代计划会有集群版本

- RM resource manager：
    - 资源管理器，主要负责分支事务的上报，本地事务的管理

## 方案

### 无侵入性

- XA

### 有侵入性
- 基于可靠消息的最终一致性方案
- TCC
- Saga
- 缺陷：
    - 实现正向和反向的幂等接口


## 缺点：
- 最高支持：读已提交 的水平



大家想读源码的话，可以重点关注一下几个类。有问题一起探讨。
- TM相关
    - com.alibaba.fescar.tm.api.TransactionalTemplate
- RM相关
    - com.alibaba.fescar.rm.datasource.exec.SelectForUpdateExecutor
    - com.alibaba.fescar.rm.datasource.ConnectionProxy
    - com.alibaba.fescar.rm.datasource.exec.AbstractDMLBaseExecutor
    - com.alibaba.fescar.rm.RMHandlerAT
- TC相关
    - com.alibaba.fescar.server.coordinator.DefaultCoordinator
    - com.alibaba.fescar.server.coordinator.DefaultCore
    - com.alibaba.fescar.server.lock.DefaultLockManagerImpl




## 参考：
https://mp.weixin.qq.com/s/8JPpVCVxB5fuz2F0fcBtwA

https://help.aliyun.com/document_detail/48726.html?spm=a2c4g.11174283.6.542.a9fe735dePMP1W

https://my.oschina.net/keking/blog/3011509

https://mp.weixin.qq.com/s/EzmZ-DAi-hxJhRkFvFhlJQ

http://www.kailing.pub/article/index/arcid/173.html

https://www.jianshu.com/p/67ea0e0e2d88

https://www.jianshu.com/p/051ff5640e1b

https://www.cnblogs.com/jajian/p/10014145.html

https://www.cnblogs.com/DKSL/p/fescar.html

https://blog.csdn.net/hosaos/article/details/89136666
