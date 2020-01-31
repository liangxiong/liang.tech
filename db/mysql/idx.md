# 索引
- 是一种数据结构，能够帮助我们快速的检索数据库中的数据

## 优化因素
- 联合索引：最左前匹配

## 执行计划 explain
- 查询优化器：一条SQL语句的查询，可以有不同的执行方案，需要通过优化器进行选择，选择执行成本最低的方案
  - 根据搜索条件，找出所有可能使用的索引
  - 计算全表扫描的代价
  - 计算使用不同索引执行查询的代价
  - 对比各种执行方案的代价，找出成本最低的那一个

## InnoDB 引擎索引
### B+ Tree
#### 特点  
- 天然有序
- 左子节点小于父节点、父节点小于右子节点
#### 适合场景
- 排序
- 范围查询

### Hash 索引
#### 特点
- key-value存储数据的结构

#### 适合场景
- 等值查询

### 聚簇索引
- 主键索引
- 存储了整行数据

### 非聚簇索引
- 存储的是主键值
- 查询要做多次回表  执行计划：TABLE ACCESS BY INDEX ROWID

### 覆盖索引
- 查询语句的执行只用从索引中就能够取得，不必回表查询

## 各版本特点：
### 5.6 新增特性：索引下推  Index Condition Pushdown
- people表中（zipcode，lastname，firstname）构成一个索引
- 查询条件：zipcode='95054' AND lastname LIKE '%etrunia%' AND address LIKE '%Main Street%'
- 原来逻辑：
  - 会通过zipcode='95054'从存储引擎中查询数据，返回到MySQL服务端（回表）
  - MySQL服务端基于lastname LIKE '%etrunia%'和address LIKE '%Main Street%'来判断数据是否符合条件
- 索引下推逻辑：可以在有like条件查询的情况下，减少回表次数
  - MYSQL首先会返回符合zipcode='95054'的索引
  - 然后根据lastname LIKE '%etrunia%'和address LIKE '%Main Street%'来判断索引是否符合条件。
    - 如果符合条件，则根据该索引来定位对应的数据，
    - 如果不符合，则直接reject掉
