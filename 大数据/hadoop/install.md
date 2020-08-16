# 安装

## 前置条件
- 生成账户
  - sudo useradd -m hadoop -s /bin/bash
  - sudo passwd hadoop          
  - sudo adduser hadoop sudo
  - su - hadoop
  - sudo apt-get update  

- 免登
  - sudo apt-get install openssh-server  
  - ssh localhost                       
  - exit     
  - cd ~/.ssh/                         
  - ssh-keygen -t rsa　　
  - cat ./id_rsa.pub >> ./authorized_keys

- 安装java 环境

- 下载环境：
  - wget 'http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz'
  - sudo tar -zxvf  hadoop-2.7.7.tar.gz -C /usr/local
  - cd /usr/local
  - sudo mv  hadoop-2.7.7    hadoop
  - sudo chown -R hadoop ./hadoop
  - 增加环境变量  vim ~/.bashrc  配置增加后，要 source ~/.bashrc 才能生效
    - export HADOOP_HOME=/usr/local/hadoop
    - export CLASSPATH=$($HADOOP_HOME/bin/hadoop classpath):$CLASSPATH
    - export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
    - export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

## 伪分布式
- 工作目录：/usr/local/hadoop/etc/hadoop/
- hadoop-env.sh
  - 修改 JAVA_HOME 配置
- core-site.xml  增加配置
  - hadoop.tmp.dir 默认：/tmp/hadoo-hadoop
```
  <property>
     <name>hadoop.tmp.dir</name>
     <value>file:/usr/local/hadoop/tmp</value>
     <description>Abase for other temporary directories.</description>
  </property>
  <property>
      <name>fs.defaultFS</name>
      <value>hdfs://localhost:9000</value>
  </property>
```
- hdfs-site.xml 增加配置
```
  <property>
      <name>dfs.replication</name>
      <value>1</value>
  </property>
  <property>
      <name>dfs.namenode.name.dir</name>
      <value>file:/usr/local/hadoop/tmp/dfs/name</value>
  </property>
  <property>
      <name>dfs.datanode.data.dir</name>
      <value>file:/usr/local/hadoop/tmp/dfs/data</value>
  </property>
```

### 启动
- NameNode格式化
  - hdfs namenode -format

- 启动NameNode 和 DataNode进程
  - start-dfs.sh
  - jps 查看启动了哪些java 进程

- 停止
  - stop-dfs.sh

- 访问： 120.27.46.121
  -  Web 界面 http://localhost:50070 查看 NameNode 和 Datanode 信息
