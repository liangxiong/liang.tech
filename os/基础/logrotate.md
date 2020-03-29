# logrotate 日志轮转

-------------------------------------日志的压缩旋转
- 配置参考
  - 在：/etc/logrotate.d/ 编辑文件
```
/var/log/nginx/*.log {
        weekly
        missingok
        rotate 52
        compress
        delaycompress
        notifempty
        create 0640 www-data adm
        sharedscripts
        prerotate
                if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
                        run-parts /etc/logrotate.d/httpd-prerotate; \
                fi \
        endscript
        postrotate
                [ -s /usr/local/tengine/logs/nginx.pid ] && kill -USR1 `cat /usr/local/tengine/logs/nginx.pid`
        endscript
}
```
- 参数
  - -v ：显示过程
  - -d ：debug 模式运行，不更改日志文件
  - -f ：强制执行

- 执行
  - logrotate -v /etc/logrotate.conf

- 强制
  - logrotate -vf /etc/logrotate.conf
  - logrotate -v /etc/logrotate.conf
  - logrotate -vf /etc/logrotate.d/tengine
  - logrotate -vf /etc/logrotate.d/rsyslog

- 配置在： /etc/crontab
- /etc/cron.daily/目录下，不能有后缀
- CentOS 在：    /etc/anacrontab

## 参考：
- [参数说明](http://www.2cto.com/os/201203/122074.html)
