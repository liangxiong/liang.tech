# xxl-job

## 概要
- [官网 https://www.xuxueli.com/xxl-job/](https://www.xuxueli.com/xxl-job/)

- 分布式任务调度:
  - 任务统一由调度中心 集中管理，统一下发到 "执行器" 执行
  - 通过负载均衡策略，执行者收到消息执行

### 名词解释
- 调度器：下发任务，是一个应用
- 执行器：干活的，是一个应用
- 触发器：”调度器” 和 ”触发器” 两个解耦的中间对象，只是一个逻辑对象

## 架构
- [架构图](https://github.com/liangxiong/liang.tech/blob/master/开源项目/任务调度/xxl-job/res/diagram.jpg)
<img src = 'http://res.liang3307.tech/liang.tech/开源项目/任务调度/xxl-job/res/diagram.jpg'>

- 任务调度通讯流程
  - “调度中心” 向 “执行器” 发送 调度请求
  - “执行器” 接收请求: 底层使用 xxl_rpc
  - “执行器” 执行任务逻辑
  - “执行器” http回调 “调度器” 调度结果，
  - “调度器” 做日志记录

### 调度中心业务组件

#### CRON任务 线程
- 计算任务何时执行
- 执行过程：
  - 后台操作：任务状态是打开情况下，通过配置的cron表达式计算下一次运行的时间
  - "调度器" 获取5秒内要执行的任务
  - 下发到调度中心触发器队列
  - 重新计算下次执行的时间

- 表：xxl_job_info
- 关键计算参考：new CronExpression(cron).getNextValidTimeAfter(fromTime);

#### 调度中心触发器线程
- 业务由他下发到 "执行器"
- 线程类：JobTriggerPoolHelper
  - fastTriggerPool: 快速触发
    - 200个线程
  - slowTriggerPool: 慢速触发
    - 100个线程
  - 根据：job 的 timeout > 10 判断为 slow

- 执行逻辑：
  - JobTriggerPoolHelper.addTrigger : 添加任务的触发器
    - XxlJobTrigger.trigger  : 加入到线程池中异步执行
    - XxlJobTrigger.processTrigger
      - ExecutorRouter.route 负载均衡器的选择策略，选择rpc的地址。 配置可看: ExecutorRouteStrategyEnum
    - XxlJobTrigger.runExecutor 执行rpc的代码
      - ExecutorBiz.run

#### 注册中心 “执行器” 失效监控线程
- 线程类：JobRegistryMonitorHelper
- 3个周期的BEAT判断失效，失效就从任务里去掉
- 涉及到的表：
  - xxl_job_group 执行器
  - xxl_job_registry 执行器注册

#### 监控执行失败的日志线程
- 线程类：JobFailMonitorHelper
- 失败的执行日志，
  - 小于重试次数，进行重试
  - 进行告警

#### 日志报表日结线程
- 统计最近3天的 指标：运行中，执行成功失败
  - 表：xxl_job_log_report

- 清理超过 logretentiondays 的日志
  - 表：xxl_job_log

#### 注意点：
- 调度的任务过期处理策略
  - 可能原因：服务重启；调度线程被阻塞，线程被耗尽；上次调度持续阻塞，下次调度被错过；
  - 处理策略：
    - 过期超5s：本次忽略，当前时间开始计算下次触发时间
    - 过期5s内：立即触发一次，当前时间开始计算下次触发时间

## 运维：
### 调度器 的高可用
- 调度器部署集群
  - CRON定时任务调度
    - 先通过：select * from xxl_job_lock where lock_name = 'schedule_lock' for update; 抢到锁
    - 下发到调度中心触发器队列
  - 其他线程：
    - 注册中心失效监控线程：会同时执行，数据覆盖
    - 监控执行失败的日志线程：会同事执行，有可能会
    - 日志报表日结线程：会同时执行，数据覆盖
  - 官网的 “调度中心在集群部署时会自动进行任务平均分配”：真心没看到 如何做均衡的，冷备的作用是有的

### 执行器的高可用
- “执行器” 关机的 主动注销
- “调度器” 失效 ”注册器” 线程的监控
- 负载均衡策略的地址 活跃地址选择
- ”调度器” 的失败重试

### 扩展性：
#### 调度器：
- 由于他只是任务触发，暂时瓶颈不会在他这
- 调高 xxl.job.triggerpool.fast.max xxl.job.triggerpool.slow.max 线程数

#### 执行器：
- 通过部署多台解决，“调度器” 使用负载均衡策略下发执行任务到不同 “执行器”

### 安全性：他的重点没在这
- rpc 通讯数据: 使用Hessian1Serializer Hessian2Serializer
  - 使用RequestModel和ResponseModel两个对象封装调度请求参数和响应数据

- http 通讯：有一点作用
  - 访问令牌（AccessToken）

### 整体问题
- 数据由于放到内存队列中，极端情况下会出现数据丢失

## 关键技术点：

### 代码 运行模式
- GLUE模式(Java)
  - 动态编译代码：groovyClassLoader.parseClass(codeSource);
  - spring 注入：[SpringGlueFactory.injectService](https://github.com/liangxiong/liang.tech/blob/master/开源项目/任务调度/xxl-job/code/SpringGlueFactory.java)

- GLUE模式(Shell) + GLUE模式(Python) + GLUE模式(NodeJS)
  - 执行机器先按照该运行环境，
  - 把代码写入磁盘
  - 执行代码 [ScriptUtil.execToFile](https://github.com/liangxiong/liang.tech/blob/master/开源项目/任务调度/xxl-job/code/ScriptUtil.java)
  - 技巧学习：
    - 正常输出和 异常输出，启动不同的线程写入文件

#### CRON表达式计算
- 可精确到 秒，分，小时，日，年，年
- 方法
  - 下一次精确计算时间：new CronExpression(cron).getNextValidTimeAfter(fromTime);

#### 优雅退出,没有注册java 的退出钩子

```
volatile toStop = true; \\标志位
thread.interrupt(); \\下发打断标识
thread.join(); \\等待执行完
```

#### 路由策略： 配置文件: ExecutorRouteStrategyEnum
- 第一个 ExecutorRouteFirst
- 最后一个 ExecutorRouteLast
- 轮询 ExecutorRouteRound
- 随机 ExecutorRouteRandom
- 一致性HASH  ExecutorRouteConsistentHash
  - 对任务id 进行 hash
  - 通过 addressList 放入 TreeMap
  - 利用： tailMap(jobHash).firstKey()
- 最不经常使用 ExecutorRouteLFU
  - 算次数
- 最近最久未使用 ExecutorRouteLRU
  - 使用 LinkedHashMap
- 故障转移 ExecutorRouteFailover
  - 选择地址先进行 beat
- 忙碌转移 ExecutorRouteBusyover
  - 选择地址后，对 ”执行器” 该任务是否有在执行或在队列中
