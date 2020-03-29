# ntp ubuntu安装时间校对服务


## 1. 安装服务端
- sudo aptitude install ntp

- 修改配置文件/etc/ntp.conf，内容如下：

```
driftfile /var/lib/ntp/ntp.drift
statistics loopstats peerstats clockstats
filegen loopstats file loopstats type day enable
filegen peerstats file peerstats type day enable
filegen clockstats file clockstats type day enable
server 2.cn.pool.ntp.org
server 0.asia.pool.ntp.org
server 2.asia.pool.ntp.org
server ntp.ubuntu.com
server 127.127.1.0
fudge 127.127.1.0 stratum 8
restrict -4 default kod notrap nomodify nopeer noquery
restrict -6 default kod notrap nomodify nopeer noquery
restrict 127.0.0.1
restrict ::1
```

- 重启ntp服务： /etc/init.d/ntp restart

## 2. 安装客户端

- 安装 sudo aptitude install ntpdate

- 设置自动核准
  - cat /etc/crontab
  - 0,30 * * * * /usr/sbin/ntpdate 2.cn.pool.ntp.org
