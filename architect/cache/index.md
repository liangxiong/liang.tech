# 深入分布式缓存 从原理到实践
- 音乐是时间的意思，缓存是软件系统中关于时间的艺术

# 概要
- what：
  - 原始数据复制集，以便于访问

- why: 为了系统性能
  - 指标分：
    - 响应时间
      - 呈现时间
      - 系统响应时间：
        - 网络传输时间
        - 应用延迟时间
    - 延迟时间
    - 吞吐量：单位时间内处理请求的数量
    - 并发用户数
    - 资源利用率


# 分
- 文件缓存
- 客户端缓存
- 页面缓存：
  - h5 mainfest 规范
- 浏览器缓存
  - 开发测试阶段的 绕过缓存 Ctrl + F5刷新
  - http 1.0
    - Expires：Thu，15 Apr  2010  20：00：00  GMT 告诉客户端缓存多久是安全的
      - 缺点: 服务端和客户端的时钟保持同步
    - 发送 if-modified-since 的条件请求来使用缓存，返回 304-Not Modified应答
      - 服务端返回：Last-Modified
      - 下次浏览器请求：if-modified-since
      - 服务端 返回 200 或 304
  - http1.1
    - Cathe-Control：max-age=315360000  被缓存多久，弥补Expires的缺点
    - e-tag标签：资源的唯一标识,请求时询问服务端该文件是否变化，无变化返回 304
- Web 代理缓存
  - 正向代理：Nginx
  - 反向代理：Squid
  - 边缘缓存：Varnish Cdn
    - 遵守：Cache-Control:max-age

- 平台级缓存
  - 带有缓存特性的应用框架
  - 缓存功能的专用库
  - Example：Ehcache, Voldemort

- 应用级缓存
  - Redis
    - master-slave 结构，通过 Gossip协议交换相互的状态
    - PING-PONG 协议 互相感知
    - Slot 迁移平衡
