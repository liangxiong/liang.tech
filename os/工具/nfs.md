# 磁盘共享 nfs


## 服务端
- 安装nfs sudo apt-get install nfs-kernel-server

- 配置 vim /etc/exports，在文件最后写入：
```
home/user/folder  172.168.30.0/24(rw,sync,no_root_squash,no_subtree_check)
```

- 服务端配置参数
  - /data/user/folder：要共享的目录
  - * ：允许所有的网段访问
  - rw ：读写权限
  - sync：资料同步写入内在和硬盘
  - no_root_squash：nfs客户端共享目录使用者权限

- 查看挂载信息
  - showmount -e host

- 重启服务
  - sudo /etc/init.d/nfs-kernel-server restart
  - sudo /etc/init.d/nfs-kernel-server reload

- 显示共享出的目录
  - showmount -e

## 客户端
- 本机上挂载测试
  - sudo mount -t nfs localhost:/home/www/folder /mnt

- 挂载 :需要安装nfs
  - sudo mount -t nfs 192.168.2.11:/home/user/folder  /home/user/folder

- 取消挂载
  - sudo umount /home/www/folder

- 强制取消挂载
  - sudo umount -f /home/user/folder
  - sudo umount -i -f  /home/user/folder
  - sudo umount -i -d -r -n -v -f /home/user/folder


## autofs
- 解决 服务端失去响应，自动进行重连

- 安装
  - sudo aptitude install autofs
  - sudo aptitude download autofs

- 配置 vi /etc/auto.master
```
 /nfs  /etc/auto.nfs
```
- vi /etc/auto.nfs
```
/local/folder -rw,soft,intr 192.168.1.1:/home/user/folder
```

- 重启
  - sudo service autofs restart
  - sudo service autofs status



- 自动挂载 vim /etc/fstab
```
192.168.1.1:/home/user/folder /root/shared nfs defaults,bootwait 0 0
```
