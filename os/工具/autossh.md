# autossh 断点自连ssh

- 安装：
  - sudo apt-get install openssh-server

- 配置：
  - /etc/ssh/sshd_config

- 启动：sudo /etc/init.d/ssh star

- 命令：
  - 复制到服务器：scp -P port file user@host:/home/folder
  - 从服务器上复制下来：scp -P port -r user@host:/home/folder/file  .
  - 反向代理隧道：autossh -M 5678 -NR 1234:localhost:2223 user1@server_host -pPort

- 免密登陆
  - 在 .ssh下 生成秘钥: ssh-keygen -t rsa
  - 把ids_rsa.pub 合并到 ：服务器 authorized_keys  
