# insert 插入
- 判断唯一：
    - UNIQUE索引
    - PRIMARY KEY

## replace:
replace into table_name(col_name, ...) values(...)

- 过程：
    - 尝试插入新行
    - 发现重复：
        - 删除
        - 继续插入

-结果：
    - 返回的影响的行数：被删除+被插入之和


## insert ignore into
insert ignore into table_name(email,phone,user_id) values('test9@163.com','99999','9999')

- 过程
    - 有重复数据忽略


## ON DUPLICATE KEY UPDATE
INSERT INTO table (a,b,c) VALUES (1,2,3),(4,5,6)
ON DUPLICATE KEY UPDATE c=VALUES(a)+VALUES(b);

- 过程：
    - 出现重复则执行UPDATE
- UPDATE子句中使用VALUES(col_name)函数从INSERT...UPDATE语句的INSERT部分引用列值
