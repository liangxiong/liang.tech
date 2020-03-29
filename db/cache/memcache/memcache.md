# 安装
- yum -y install memcached
- 启动命令：memcached -m 512 -u root -d -l 127.0.0.1 -p 11211  

- 参数：
  -m 指定缓存所使用的最大内存容量，单位是Megabytes，默认是64MB
  -u 只有以root身份运行时才指定该参数
  -d 以daemon的形式运行
  -l 指定监听的地址
  -p 指定监听的TCP端口号，默认是11211
