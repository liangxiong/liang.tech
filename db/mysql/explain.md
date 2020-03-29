# explain

explain 显示 mysql server 处理 select 查询用到的表，索引情况

###  字段解释
mysql> explain select * from jpress_user;

| id            | select_type   | table            | type    | possible_keys    | key      | key_len     | ref       | rows   | Extra   |
|:-----------|:-----------|:----------:|:------|:--------------|:------|:---------|:------|:------|:------|
|  1             | SIMPLE         | jpress_user  | ALL     | NULL                  | NULL    | NULL        | NULL   |    1      | NULL   |


- id: select 标识符，查询序列号

- select type :
    - simple: 简单查询，不适用 union 或 子查询
    - primary：(查询中若包含任何复杂的子部分,最外层的select被标记为PRIMARY)
    - union: union 中第2个或后面的select 语句
    - dependent union : union 中的第2个或后面的select 语句
    - union result : union 的结果
    - subquery : 子查询的第1个select ，取决于于外面的查询
    - dependent subquery : 子查询的第1个select，取决于外面的查询
    - derived：导出表的select

- table : 所引用的表

- type：
    - ALL：Full Table Scan， 全表扫描
    - index: Full Index Scan，全遍历索引树
    - range:只检索给定范围的行，使用一个索引来选择行
    - system:表仅有一行，是const类型的特例
    - const

- possible_keys
    - 指出MySQL能使用哪个索引在表中找到记录

- key
    - 显示MySQL实际决定使用的键（索引）

- key_len
    - 索引中使用的字节数。索引字段的最大可能长度，并非实际使用长度

- ref

    - 表的连接匹配条件，即哪些列或常量被用于查找索引列上的值

- rows
    - 表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数

- Extra
  - 该列包含MySQL解决查询的详细信息,有以下几种情况
    - Using where:列数据是从仅仅使用了索引中的信息而没有读取实际的行动的表返回的，这发生在对表的全部的请求列都是同一个索引的部分的时候，表示mysql服务器将在存储引擎检索行后再进行过滤
    - Using temporary：表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询
    - Using filesort：MySQL中无法利用索引完成的排序操作称为“文件排序”
    - Using join buffer：改值强调了在获取连接条件时没有使用索引，并且需要连接缓冲区来存储中间结果。如果出现了这个值，那应该注意，根据查询的具体情况可能需要添加索引来改进能。
    - Impossible where：这个值强调了where语句会导致没有符合条件的行。
    - Select tables optimized away：这个值意味着仅通过使用索引，优化器可能仅从聚合函数结果中返回一行


explain select id from jpress_user a union select id jpress_comment

explain select a.id from jpress_user a, jpress_comment b where a.id=b.id
