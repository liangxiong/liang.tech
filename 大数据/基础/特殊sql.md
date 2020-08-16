# 特殊sql
- 逗号分隔，行列转换：
  - help_topic 表只有一个id 字段，从0 到 1000

```
SELECT a.id,a.action_tags,b.id,
SUBSTRING_INDEX(SUBSTRING_INDEX(a.action_tags,',',b.id+1),',',-1) as name
from order a left join help_topic b
on b.id < (LENGTH(a.action_tags) - LENGTH(REPLACE(a.action_tags,',',''))+1)
where a.biz_order_id = 835773411710340413
ORDER BY a.biz_order_id;
```
