# join
- https://juejin.im/book/5bffcbc9f265da614b11b731/section/5c061b0cf265da612577e0f4



- 嵌套循环连接（Nested-Loop Join）
  - 嵌套循环


- 基于块的嵌套循环连接（Block Nested-Loop Join）
  - 把n条驱动表结果集的记录装在这个join buffer中，查询列 + 过滤条件 放到：join buffer
  - 每一条被驱动表的记录一次性和join buffer中的多条驱动表记录做匹配
