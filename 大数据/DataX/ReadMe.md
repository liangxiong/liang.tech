# DataX

## 逻辑执行模型

- Job: Job是DataX用以描述从一个源头到一个目的端的同步作业，是DataX数据同步的最小业务单元。比如：从一张mysql的表同步到odps的一个表的特定分区。

- Task: Job拆分出来的最小执行单元
  - 如：读一张有1024个分表的mysql分库分表的Job，拆分成1024个读Task，用若干个并发执行

- TaskGroup:描述的是一组Task集合
  - 在同一个TaskGroupContainer执行下的Task集合称之为TaskGroup

- JobContainer: Job执行器
  - 负责Job全局拆分、调度、前置语句和后置语句等工作的工作单元。类似Yarn中的JobTracker

- TaskGroupContainer: TaskGroup执行器
  - 负责执行一组Task的工作单元，类似Yarn中的TaskTracker。

## 运行模式：
- Standalone: 单进程运行，没有外部依赖。
- Local: 单进程运行，统计信息、错误信息汇报到集中存储。
- Distrubuted: 分布式多进程运行，依赖DataX Service服务。

## 核心类：
### Job接口功能如下：
- init: Job对象初始化工作
  - 可以通过super.getPluginJobConf()获取与本插件相关的配置
  - 读插件获得配置中reader部分，写插件获得writer部分。

- prepare: 全局准备工作
  - 比如: odpswriter清空目标表

- split: 拆分Task
  - 参数adviceNumber框架建议的拆分数，一般是运行时所配置的并发度
  - 返回的值是Task的配置列表

- post: 全局的后置工作
  - 比如: mysqlwriter同步完影子表后的rename操作

- destroy: Job对象自身的销毁工作

### Task接口功能如下
- init：Task对象的初始化
  - 此时可以通过super.getPluginJobConf()获取与本Task相关的配置。
  - 这里的配置是Job的split方法返回的配置列表中的其中一个

- prepare：局部的准备工作
  - ?

- startRead: 从数据源读数据，写入到RecordSender中
  - RecordSender会把数据写入连接Reader和Writer的缓存队列。

- startWrite：从RecordReceiver中读取数据，写入目标数据源
  - RecordReceiver中的数据来自Reader和Writer之间的缓存队列。

- post: 局部的后置工作

- destroy: Task象自身的销毁工作

### 注意点：
- 不能有共享变量，因为分布式运行时不能保证共享变量会被正确初始化。两者之间只能通过配置文件进行依赖。
prepare和post在Job和Task中都存在，插件需要根据实际情况确定在什么地方执行操作。

- 执行流程：
<img src = 'http://res.liang3307.tech/liang.tech/big_data/DataX/res/plugin_dev_guide_1.png'>



python datax.py -r mysqlreader -w mysqlwriter

python datax.py -r streamreader -w streamwriter

java -server -Xms1g -Xmx1g -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/Users/liang/project/java/open/alibaba/DataX/target/datax/datax/log -Xms1g -Xmx1g -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/Users/liang/project/java/open/alibaba/DataX/target/datax/datax/log -Dloglevel=info -Dfile.encoding=UTF-8 -Dlogback.statusListenerClass=ch.qos.logback.core.status.NopStatusListener -Djava.security.egd=file:///dev/urandom -Ddatax.home=/Users/liang/project/java/open/alibaba/DataX/target/datax/datax -Dlogback.configurationFile=/Users/liang/project/java/open/alibaba/DataX/target/datax/datax/conf/logback.xml -classpath /Users/liang/project/java/open/alibaba/DataX/target/datax/datax/lib/*:.  -Dlog.file.name=b_stream2stream_json com.alibaba.datax.core.Engine -mode standalone -jobid -1 -job /Users/liang/project/java/open/alibaba/DataX/target/datax/datax/job/stream2stream.json
