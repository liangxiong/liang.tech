# redo 日志 物理日志 重做日志

- 记录了一下事务对数据库做了哪些修改
- 记录一下在某个页面的某个偏移量处修改了几个字节的值，具体被修改的内容是啥就好了

- 结构：
  - type
    - MLOG_1BYTE  表示在页面的某个偏移量处写入1个字节的redo日志类型
    - MLOG_2BYTE
    - MLOG_4BYTE
    - MLOG_8BYTE
    - MLOG_WRITE_STRING 表示在页面的某个偏移量处写入一串数据
    - 其他
    - MLOG_REC_INSERT（对应的十进制数字为9）：表示插入一条使用非紧凑行格式的记录时的redo日志类型。
    - MLOG_COMP_REC_INSERT（对应的十进制数字为38）：表示插入一条使用紧凑行格式的记录时的redo日志类型。

    - MLOG_MULTI_REC_END： 结尾redo，表示之前到现在的redo日志为一个原子

  - space ID
  - page number
  - data
    - offset
    - length
    - 具体数据


- redo日志占用的空间非常小
- redo日志是顺序写入磁盘的

- redo日志会把事务在执行过程中对数据库所做的所有修改都记录下来，在之后系统崩溃重启后可以把事务所做的任何修改都恢复出来

- 悲观插入 了挂插入

- redo 日志做原子：
  - 以组的形式写入redo日志
  - MLOG_MULTI_REC_END： 结尾redo，表示之前到现在的redo日志为一个原子

- Mini-Transaction mtr 一次原子访问的过程
  - 作用：在进行崩溃恢复时这一组redo日志作为一个不可分割的整体
  - 内容：
    - 一个事务可以包含若干条语句
    - 每一条语句其实是由若干个mtr组成
    - 每一个mtr又可以包含若干条redo日志

  - 放在了大小为512字节的页中，把用来存储redo日志的页称为block
    - 12字节 redo log block
      - log block hdr no
      - log block hdr data length
      - log block first rec group
      - log block checkpoint no
    - 496字节 log block body
    - 4字节 log blcok trailer
      - 4字节 log block checksum

- innodb_log_buffer_size 来指定log buffer的大小, 在 Mysql 5.7.21默认：16MB

- mtr_1开始时对应的lsn，写入页a对应的控制块的oldest_modification属性中
- mtr_1结束时对应的lsn，写入页a对应的控制块的newest_modification属性中

- flush链表中的脏页按照修改发生的时间顺序进行排序，也就是按照oldest_modification代表的LSN值进行排序，被多次更新的页面不会重复插入到flush链表中，但是会更新newest_modification属性的值
