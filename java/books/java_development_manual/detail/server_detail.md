# 服务器规约
- 高并发的server 调小TCP协议的 time_wait 超时时间
  - 默认是240秒
  - vim /etc/sysctl.conf   net.ipv4.tcp_fin_timeout = 30

- 调大 服务器的 fd ( File Descriptor )，默认是1024，
  - linux 将TCP | UDP 连接采用与文件一样的方式管理，fd不足会导致 open too many files 问题

-  JVM 设置-XX:+HeapDumpOnOutOfMemoryError 参数，遇到OOM 输出dump信息

- 内部重定向使用forward，外部使用url 拼接工具类生成
