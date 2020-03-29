# 主从同步
- master将数据通过binlog的方式同步给slave
- slave节点的数据理解为master节点数据的全量备份。


### 安装
- 安装
```
sudo apt-get install mysql-server
sudo apt-get install mysql-client libmysqlclient-dev
```

- 卸载
```
sudo apt-get purge mysql-server
sudo apt-get remove mysql-common
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
sudo  rm -rf  /var/lib/mysql/
sudo rm -rf /var/lib/mysql/
```

- 判断
```
sudo netstat -tap | grep mysql
```

- 修改任意ip登录权限

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'psw1' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

### 配置主从
- 修改主 vi /etc/mysql/my.cnf
```
[mysqld]
log-bin=mysql-bin   //[必须]启用二进制日志
server-id=1      //[必须]服务器唯一ID，默认是1，一般取IP最后一段
replicate-do-db=cio
replicate-ignore-db=mysql
replicate-ignore-db=performance_schema
log_bin                 = /var/log/mysql/mysql-bin.log
binlog_do_db     = cio
binlog-ignore-db = mysql
binlog-ignore-db = performance_schema
auto-increment-increment=2
auto-increment-offset   =1

change master to master_host='192.168.199.227',master_port=3306,master_user='root',master_password='psw1',master_log_file='mysql-bin.000018',master_log_pos=952;

show master status;

```


- 修改从 vi /etc/mysql/my.cnf

```
[mysqld]
log-bin=mysql-bin   //[必须]启用二进制日志
server-id=2      //[必须]服务器唯一ID，默认是1，一般取IP最后一段
replicate-do-db=cio
replicate-ignore-db=mysql
replicate-ignore-db=performance_schema
log_bin          = /var/log/mysql/mysql-bin.log

binlog_do_db     = cio
binlog-ignore-db = mysql
binlog-ignore-db = performance_schema

auto-increment-increment=2
auto-increment-offset   =1

change master to master_host='192.168.199.131',master_port=3306,master_user='root',master_password='psw1',master_log_file='mysql-bin.000007',master_log_pos=1835;

slave start;
show slave status \G;

```


### 读写分离 proxy
```
Amoeba
```

### 其他脚本：
```
show variables like 'server_id';  

```
