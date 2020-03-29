# 锁
  
#### 根据锁定对象：
- 行锁
    - 操作：
        - insert
        - update
        - delete
        - selsct for update
- 表锁

#### 根据并发事务锁定的关系上看
- 共享锁定 读锁
- 独占锁定 写锁

####

- 悲观锁：
    - 别人会去修改它所要操作的数据

- 乐观锁：
    - 认为别人不会去修改
    - 在提交更新的时候去检查数据的状态。
    - 通常是给数据增加一个字段来标识数据的版本

- record locks（记录锁）：在索引记录上加锁
- gap locks（间隙锁）：在索引记录之间加锁，或者在第一个索引记录之前加锁，或者在最后一个索引记录之后加锁
- next-Key Locks：在索引记录上加锁，并且在索引记录之前的间隙加锁。它相当于是Record Locks与Gap Locks的一个结合

- 快照读：读取的是快照版本，也就是历史版本
    - SELECT

- 当前读：读取的是最新版本
    - UPDATE、DELETE、INSERT、SELECT ... LOCK IN SHARE MODE、SELECT ... FOR UPDATE

- 一致性非锁定读
- 锁定读

#### 隔离级别：
- read uncommitted 读未提交：
    - 一个事务未提交，它的变更能被其他事物看到
    - 实现：直接返回记录的最新值
    - 问题：脏读，不可重复读，幻读

- read committed 读已提交：
    - 一个事务提交之后，他的变更能被其他事物看到
    - 实现：每个sql开始执行的时候创建
    - 问题：不可重复读，幻读    

- repeatable read 可重复读：
    - 一个事务执行过程中看到数据，总是跟这个事物在启动时看到的数据是一致的
    - 问题：其避免了脏读和不可重复读问题，但幻读依然存在
    - 实现：开启事物，创建视图
    - 场景：
        - 查询当前额度，开启事物后不会随着交易记录变化而变化
    - 问题：幻读

- serializable 串行化：
    - 事务串行执行
    - “写” 会加 “写锁”，“读”会加“读锁”，todo：读写锁 研究透？？？

- mysql：默认是 repeatable read 可重复读，并且解决了幻读问题，脏读、幻读、不可重复读
select @@tx_isolation
不可重复读重点在于update和delete，而幻读的重点在于insert

- oracle：默认是读提交
- dirty red 脏读
- non-repeatable red 不可重复读
- phantom red 幻读
