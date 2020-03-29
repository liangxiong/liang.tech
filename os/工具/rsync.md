# 文件同步 rsync

## 服务端
- 安装
  - aptitude install rsync
  - apt-get install rsync
  - yum install rsync

- 配置
  - vi /etc/default/rsync，允许 --daemon 启动：
    - RSYNC_ENABLE=false 改为 true

  - 创建配置文件/etc/rsyncd.conf
```
sudo vi /etc/rsyncd.conf
uid = root
gid = root
max connections = 10
read only = true
hosts allow = client_ip
transfer logging = true
slp refresh = 300
log format = %h %o %f %l %b

/# 默认写入syslog
log file = /var/log/rsyncd.log
pid file = /var/run/rsyncd.pid
lock file = /var/run/rsyncd.lock
secrets file = /etc/rsyncd.secrets
[web]
path = /home/folder
read only = true
list = false
auth users = root
```

- 创建密码文件 vi /etc/rsyncd.secrets
```
user:password
```

- 修改权限
sudo chmod 400 /etc/rsyncd.secrets
sudo /etc/init.d/rsync restart
sudo /etc/init.d/rsync status



## 客户端部分
- 安装 aptitude install rsync

- 创建密码文件 vim /etc/rsyncd.secrets
```
password
```

- 修改当前权限 只能当前用户访问
  - sudo chmod 400 /etc/rsyncd_client.secrets

- 同步脚本 /usr/local/bin/sync.sh
```
#!/bin/bash
rsync -vzrtopg --progress --delete --log-format="%t %f" --password-file=/home/www/rsyncd.secrets rsync://user@host/web /home/www/folder

```

- 创建定时任务
```
*/1 * * * * flock -xn /var/lock/rsync_client.lock /usr/local/bin/sync.sh >> /var/log/rsyncd_client.log 2>&1 &
```
  - 1：标准输出
  - 2：错误输出
  - flock：文件锁

- 单个文件主动推送同步：
```
rsync -aR css user@host:/home/www/folder
```

## 参数
- [参考](http://www.linuxidc.com/Linux/2012-10/71705.htm)
log-format:
%h 远程主机名
- %a 远程IP地址
- %l 文件长度字符数
- %p 该次rsync会话的进程id
- %o 操作类型："send"或"recv"
- %f 文件名
- %P 模块路径
- %m 模块名
- %t 当前时间
- %u 认证的用户名(匿名时是null)
- %b 实际传输的字节数
- %c 当发送文件时，该字段记录该文件的校验码

(xfer#3, to-check=1584849/1588633)
- xfer: 传输的文件

## 参考
- [日志格式](http://blog.csdn.net/joliny/article/details/1800767)
- [diff file 核心算法](http://www.oschina.net/question/28_54213)
- 问题：
  - rsync同步数据时，需要扫描所有文件后进行比对，进行差量传输
  - 如果文件数量达到了百万甚至千万量级，扫描所有文件将是非常耗时的,发生变化的往往是其中很少的一部分，这是非常低效的方式改进
  - 通过 inotify + rsyncd 实时同步。需要2.6.13以上内核  参考：http://ixdba.blog.51cto.com/2895551/580280/
