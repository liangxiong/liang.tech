# 进程监控 monit

- 安装 aptitude install monit
- 配置：vim /etc/monit/conf.d/atop
```
check process sshd with pidfile /var/run/atop.pid
start program "/etc/init.d/atop start"
stop  program "/etc/init.d/atop stop"
sudo /etc/init.d/monit restart
```
