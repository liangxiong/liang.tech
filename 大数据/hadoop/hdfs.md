## HDFS
- 只能执行追加操作

- 架构
	- NameNode
	- DataNode

- 机架感知
	- 确保数据副本存放在不同的机架上
	- 存储默认策略：
		- 一份数据放在本节点
		- 另外数据放在同一个机架的另外节点上
		- 第三方数据放在另外机架的节点上

	- 读取策略
		- 离客户端最近的副本上

### 工具
- distcp：用于2个hadoop集群间的复制
	- 解析为 MapReduce 任务
	- Map 任务最少要执行 256MB的数据量
	- 每个集群最多执行20个Map任务

### 命令
- web 命令：
	- http://ip:50070
- 本地复制到hdfs
	- hadoop fs -copyFromLocal test/a.txt hdfs://localhost/user/in/hello.txt

- 从hdfs复制到本地
	- hadoop fs -copyToLocal  user/in/hello.txt test/hell.copy.txt
