# zabbix

- 常用的表结构
```

select * from hosts where hostid = 10107 \G;

select * from graphs where graphid=576 \G;

select * from graphs_items where graphid=576 \G;

select * from items where itemid in (23805,23807) \G;

select * from history_uint where itemid=23805 limit 0,10 \G;

select * from history where itemid=23805 limit 0,10 \G;

select itemid,FROM_UNIXTIME(clock),value,ns from history_uint where itemid=23805 and clock > UNIX_TIMESTAMP("2016-05-28 10:50:00")

```

- [参考](http://waringid.blog.51cto.com/65148/955939)
