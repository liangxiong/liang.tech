# 创建表：
```
CREATE TABLE 表名 (列的信息
     列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称],
)
    [[DEFAULT] CHARACTER SET 字符集名称]
    [COLLATE 比较规则名称]]

ALTER TABLE 表名
    [[DEFAULT] CHARACTER SET 字符集名称]
    [COLLATE 比较规则名称]

```

- 字段修改：
```
 ALTER TABLE 表名 MODIFY 列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称];
```
