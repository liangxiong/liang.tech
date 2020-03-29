# nginx


## auth basic
- printf "admin:$(openssl passwd -crypt 5slive)\n" >> htpasswd

## nginx 配置
- location    location [=|~|~*|^~] /uri/ { … }
  - = 开头表示精确匹配
  - ^~ 开头表示uri以某个常规字符串开头，理解为匹配 url路径即可。nginx不对url做编码，因此请求为/static/20%/aa，可以被规则^~ /static/ /aa匹配到（注意是空格）
  - ~ 开头表示区分大小写的正则匹配
  - ~* 开头表示不区分大小写的正则匹配
  - !~和!~*分别为区分大小写不匹配及不区分大小写不匹配 的正则
  - / 通用匹配，任何请求都会匹配到
  - 优先级：多个location配置的情况下匹配顺序为（
    - 首先匹配 =
    - 其次匹配^~
    - 其次是按文件中顺序的正则匹配
    - 最后是交给 / 通用匹配。

```
location / {
  auth_basic "nginx basic http test for";
  auth_basic_user_file htpasswd;
  autoindex on;
  try_files $uri @backend;
}
```

- upstream
```
upstream squid {
    ip_hash;#[轮询默认 | weight(指定轮询几率) | ip_hash | fair(响应时间短的优先分配) | url_hash(url hash)]
    server ip1:por max_fails=2 fail_timeout=5;
    server ip1:port max_fails=2 fail_timeout=5;
    #健康检查
    check interval=3000 rise=2 fall=5 timeout=1000 type=http;
    check_http_send "GET /api? HTTP/1.0\r\n\r\n";
    check_http_expect_alive http_2xx http_3xx;
}
```
- 负载均衡策略
  - down 表示单前的server暂时不参与负载
  - weight 默认为1.weight越大，负载的权重就越大。
  - max_fails ：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream 模块定义的错误
  - fail_timeout:max_fails次失败后，暂停的时间。
  - backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。


- 缓存
```
nginx.conf:
    proxy_connect_timeout 10;
    proxy_read_timeout 60;
    proxy_send_timeout 10;
    proxy_buffer_size 16k;
    proxy_buffers 4 64k;
    proxy_busy_buffers_size 128k;
    proxy_temp_file_write_size 128k;
    proxy_temp_path /var/cache/tengine/temp_dir;
    proxy_cache_path /var/cache/tengine/cache levels=1:2 keys_zone=cache_one:50m inactive=20m max_size=100g;

```

- 配置 nginx监控
  - Active connections: 当前 Nginx 正处理的活动连接数.
  - server accepts handled requests: 连接数，握手数，请求数，总响应时间（ms）
  - Reading: Nginx 读取到客户端的 Header 信息数.
  - Writing: Nginx 返回给客户端的 Header 信息数.
  - Waiting: 开启 keep-alive 的情况下, 这个值等于 Active - (Reading + Writing), 意思就是 Nginx 已经处理完正在等候下一次请求指令的驻留连接.
```
location /nginx_status {
    stub_status on;
    access_log off;
    allow 127.0.0.1;
    deny all;
}
```


## 启动
- 测试 sudo /usr/local/tengine/sbin/nginx -t
- 停止 sudo /usr/local/tengine/sbin/nginx -s stop
- 平滑重启 sudo /usr/local/tengine/sbin/nginx -s reload
- 启动 sudo /usr/local/tengine/sbin/nginx
- 加入系统服务 chkconfig --add nginx
