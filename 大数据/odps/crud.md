# curd sql 整理


## DDL



### 删除数据
- 非分区表

```
truncate table table
```

- 分区表
```
ALTER TABLE table DROP if exists partition(ds='20200102');
yes
```
