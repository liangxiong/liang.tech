# 零copy
## what
- 数据不需要 从内核态 到 用户态的来回的拷贝
- 简答说：
  - 不管用户态 还是 内核态 都是引用，只有一份数据
  - 每个引用的修改，都能改变数据
- 提高性能，
- 场景：
  - java nio
  - netty
  - kafka
  - RocketMQ


## how

### RocketMQ
- 顺序写到commitlog文件
- 利用consume queue文件作为索引
- 零拷贝mmap + write的方式来回应Consumer的请求

### kafka
- 顺序写到持久化到硬盘
- 网络发送采用 sendfile零拷贝方式

### nginx
- sendfile指令 发送文件采用sendfile
- 原本流程：
  - 硬盘
  - kernel buffer
  - user buffer
  - kernel socket buffer
  - 协议栈
