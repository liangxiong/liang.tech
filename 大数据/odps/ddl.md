# DDL
- 修改生命周期
```
ALTER TABLE table SET lifecycle 3;
```

- 增加字段：alter table ods_csci_shop_order_ref add columns( shop_id bigint comment '店铺id')
- 删除字段：alter table tablename delte columns( cname varchar comment 'xxx')
