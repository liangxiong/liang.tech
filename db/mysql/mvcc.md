# MVVC 多版本并发控制

- 流程
  - 回滚段：每条记录在更新时都会同时记录一条回滚操作
  - 记录的最新值，通过回滚操作，都可以得到前一个状态的值
  - 回滚段在mysql <= 5.5时候放在ibdata

- 优点：
  - InnoDB的事务隔离级别下执行一致性
  - 查询一些正在被另一个事务更新的行
  - 并且可以看到它们被更新之前的值
  - 增强并发性的强大的技术，查询就不用等待另一个事务释放锁

- 手段：
  - DB_TRX_ID
  - DB_ROLL_PTR
  - DB_ROW_ID

- 存储方式：
  - Append only的方式，新旧版本存储在同一个表空间，例如基于LSM-Tree的存储引擎
  - ？

- 流程：
  - SELECT
    - 读取创建版本小于或等于当前事务版本号，
    - 并且删除版本为空或大于当前事务版本号的记录。
    - 这样可以保证在读取之前记录是存在的。
   - INSERT
     - 将当前事务的版本号保存至行的创建版本号
   - UPDATE
     - 新插入一行，并以当前事务的版本号作为新行的创建版本号，同时将原记录行的删除版本号设置为当前事务版本号
   - DELETE
     - 将当前事务的版本号保存至行的删除版本号
  - 什么时候删除：
    - 系统判定 当么有事务再需要用到这些回滚日志时，他会删除
    - 没有比这个回滚日志更早的 read-view 的时候，所以不要用长事务,导致回滚段清理问题

- 长事务：
  - 查询：select * from information_schma.innodb_trx where TIME_TO_SEC(timdiff(now(),trx_started)) > 60


## 事务并发执行遇到的问题
- 脏写 Dirty Write：
  - 一个事务修改了另一个未提交事务修改过的数据

- 脏读（Dirty Read）
  - 一个事务读到了另一个未提交事务修改过的数据，

- 不可重复读（Non-Repeatable Read）
  - 一个事务只能读到另一个已经提交的事务修改过的数据，并且其他事务每对该数据进行一次修改并提交后，该事务都能查询得到最新值

- 幻读（Phantom）
  - 一个事务先根据某些条件查询出一些记录，
  - 之后另一个事务又向表中插入了符合这些条件的记录，
  - 原先的事务再次按照该条件查询时，能把另一个事务插入的记录也读出来

## 四种隔离级别
- 根据问题严重性：脏写 > 脏读 > 不可重复读 > 幻读
- 隔离级别：
  - READ UNCOMMITTED：未提交读
  - READ COMMITTED：已提交读
  - REPEATABLE READ：可重复读
  - SERIALIZABLE：可串行化

隔离级别          |  脏读   | 不可重复读 | 幻读
:--------------: | :----: | :------: | :--:
READ UNCOMMITTED | 可能    | 可能      | 可能
READ COMMITTED   | 不可能  | 可能      | 可能
REPEATABLE READ  | 不可能  | 不可能    | 可能
SERIALIZABLE     | 不可能  | 不可能    | 不可能

- oracle: 只支持 READ COMMITTED 和 SERIALIZABLE
- 设置隔离级别：
  - 进程级别：SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
  - 会话级别： SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
  - 只下一个事务产生影响：SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
- 查看隔离级别：
  - SELECT @@transaction_isolation;

## ReadView
- m_ids：表示在生成ReadView时当前系统中活跃的读写事务的事务id列表
- min_trx_id：表示在生成ReadView时当前系统中活跃的读写事务中最小的事务id，也就是m_ids中的最小值
- max_trx_id：表示生成ReadView时系统中应该分配给下一个事务的id值
- creator_trx_id：表示生成该ReadView的事务的事务id
