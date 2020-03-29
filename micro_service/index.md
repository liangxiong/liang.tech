# 微服务

## 部署
- [微服务部署种类](https://github.com/liangxiong/liang.tech/blob/master/micro_service/部署/微服务部署种类.md)
- [JSON不适合做配置语言](https://mp.weixin.qq.com/s?__biz=MjM5NzM0MjcyMQ==&mid=2650082846&idx=3&sn=03d634144300ba71b37f86c8df52a6d0&chksm=bedad57089ad5c66c271f993f7f81e9ccfe8eb39193aef5a357d74b807e1e24237c20676904f&scene=0&key=a55472677b369124b9a0a83dd2d299e3d1d6e6a9b1e06aa8371a283162f799dd6d2e9afa0c7e5a5351affdff06eeb1fa143de36e89f5f6d971461c2944a1f066e73a1b4d8815b6a4b24be9c128ad3da0)

## RPC
### Dubbo
-[Dubbo 的原理](https://mp.weixin.qq.com/s?__biz=MzIxMzEzMjM5NQ==&mid=2651032242&idx=1&sn=c725c9e996885dce686c53f378de1f52&chksm=8c4c5fb6bb3bd6a0b8b57454026497a7f951b7cade1a0d4488391bd6221c0e0473cab38d556f)

#### 原理
- [Dubbo 服务启动要过程](https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247485785&idx=1&sn=d0c9f25b7c470228685d15b98eec14b4&chksm=fba6e15accd1684c2f047ab3f3038ffb9e02b587a1c288a894166bf7a69b90a1fccce3677779)


## 网关
- [nginx](https://github.com/liangxiong/liang.tech/blob/master/micro_service/网关/nginx.md)

### 网关日常总结
- [微服务架构之 网关](https://mp.weixin.qq.com/s?__biz=MzAxNjM2MTk0Ng==&mid=2247487206&idx=2&sn=e088fba1d5f3ed617b9fa565e4bcdd76)

- [Envoy为什么能战胜Ngnix——线程模型分析篇](https://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=2653550028&idx=1&sn=eced5eb129ca32a5b9b3daa6b3f01078&chksm=813a6454b64ded429914952882eb5127d45d4ecadd10d58c86553503e1a1da2ec393e0075836&scene=0&key=2b6f9a12891ea20aefc5d0dba41833ef8c2f3c4e57855ea98941a930851b2a019bf440529646f374916615e347dd4d2433641a4d0eb3ad0c81f0200ad81229546ca960162999376b5869403973f71270)

- [架构修炼之道 | 一个传统网关系统有几种 “死” 法
](https://mp.weixin.qq.com/s?__biz=MzIxMzEzMjM5NQ==&mid=2651031833&idx=1&sn=5c26363e02c2a500383f5396edad3e8b)

- [Spring Cloud Zuul ](https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247485728&idx=1&sn=7d132fd1cb8f2d34d55d884b38010194&chksm=fba6e123ccd168355bb00f25787237f29458811d2c43885830f0a3e27772dce42d795bc4a9cc)

- [微服务接口限流的设计与思考](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2651008444&idx=1&sn=a579c3ceb143ea30930bd4c6d4a8d7e2&chksm=bdbed5ef8ac95cf93e71c5393f08e3b97a7e19e8232ce3872231f2cae74f7a19ab15501aeb44&scene=0&key=5fcec68271b32b0cddb14313f44424c17a411bdb3e8a852d69721766d04f7ace533c0dc5f50e6a08718be16fdc30f7846804a95a3bcac4af09d344de528f5ceb05d4b4349eeb484e3afbd0f958ef7898)

## 注册中心

### 日常总结
- [注册中心选型](https://mp.weixin.qq.com/s?__biz=MzAxNjM2MTk0Ng==&mid=2247487171&idx=1&sn=1227f5761419b2331c8b170b97633dee)

### SOFARegistry
#### 介绍
- [海量数据下的注册中心 - SOFARegistry 架构介绍](https://mp.weixin.qq.com/s/mZo7Dg6gfNqXoetaqgwMww)



## 协调器
### zookeeper
#### 原理
- [zookeeper功能特性](https://github.com/liangxiong/liang.tech/blob/master/micro_service/zookeeper/zookeeper_index.md)

- [Basic Paxos](https://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665515948&idx=1&sn=e6b2dd3f3a7ef0b93bc9dc7d2bf87a9e)

## 微服务注册组件
### 思想
- [需要注册的缘由](https://mp.weixin.qq.com/s?__biz=MzAxNjM2MTk0Ng==&mid=2247486965&idx=2&sn=dbcc8099f63131fc73c03c7d3c06b786)


## 微服务熔断器组件
### Hystrix
#### Hystrix 原理
- [Hystrix系列之熔断器的实现原理
](https://mp.weixin.qq.com/s?__biz=MzIwMzY1OTU1NQ==&mid=2247484489&idx=1&sn=2c040f79e1a5ec55d9b658f0fd3229ac)

- [Hystrix系列之执行原理分析](https://mp.weixin.qq.com/s?__biz=MzIwMzY1OTU1NQ==&mid=2247484425&idx=1&sn=dd34cb1f7a5ba0c66c7cf1096433c767)

- [Hystrix系列之ThreadLocal跨线程传递问题
](https://mp.weixin.qq.com/s?__biz=MzIwMzY1OTU1NQ==&mid=2247484515&idx=1&sn=6a9097174aa0e8214db1e167a347ccd2)

#### Hystrix 日常总结
- [Hystrix的两个bug](https://mp.weixin.qq.com/s?__biz=MzIwMzY1OTU1NQ==&mid=2247484549&idx=1&sn=24f2b98bf2249832ca8396f705b381a8)


### 分布式追踪
#### 分布式追踪 原理
- [分布式追踪概念](https://github.com/liangxiong/liang.tech/blob/master/micro_service/trace/0401.md)

### 微服务 原理
- [SpringCloud微服务如何优雅停机及源码分析](https://mp.weixin.qq.com/s?__biz=MzAxNjM2MTk0Ng==&mid=2247487189&idx=1&sn=df3711df17655f582b0eafb429bb3b4c)


### 微服务 日常总结
- [提升你的微服务架构可用性](https://mp.weixin.qq.com/s?__biz=MzI4MTY5NTk4Ng==&mid=2247489302&idx=1&sn=4e7082ce4aed16e4db7dd402cf4ab0da)

- [58沈剑：做好你的服务拆分](https://mp.weixin.qq.com/s?__biz=MzI4MTY5NTk4Ng==&mid=2247489294&idx=1&sn=b5020776862ebd3c13763ff5d787a49d)

- [虎牙直播的微服务](https://mp.weixin.qq.com/s?__biz=MzIxMzEzMjM5NQ==&mid=2651031410&idx=1&sn=11b5b2f0db5816fc0fc230ab69214395)

- [Dubbo实践，演进及未来规划](https://mp.weixin.qq.com/s?__biz=MzIxMzEzMjM5NQ==&mid=2651031168&idx=1&sn=9c59aa42d36d1f430e98199da707a4b9)

- [李林峰讲解微服务治理的技术演进和架构实践](https://mp.weixin.qq.com/s?__biz=MzIxMzEzMjM5NQ==&mid=2651031921&idx=1&sn=a49148735e167bd0191c9d52d01092c7)

- [微服务监控](https://mp.weixin.qq.com/s?__biz=MzAxNjM2MTk0Ng==&mid=2247487407&idx=2&sn=96636bf836c4afac8811420a424afc94&chksm=9bf4bf1aac83360c06f46c998c6c7c0a43d7d8d895a6c70db15876e1c20f0afb0f6ad8f409b8)

- [某互联网金融企业(4500W用户规模)如何将单体应用迁移到微服务](https://mp.weixin.qq.com/s?__biz=MzIxMzEzMjM5NQ==&mid=2651032743&idx=1&sn=feec4072fdff3191d478f699d21e9dfd&chksm=8c4c59a3bb3bd0b5d3429a57164bebe647ed02d4a8c0e4a2afebdb774fe05b1a4e2891746e7a&mpshare=1&scene=1&srcid=&sharer_sharetime=1564837422511&sharer_shareid=8e682eed63e0c3dc5350dc02cefd65a3&key=c38930807a42c0fa1d565197ba6983a9ce81ddb4309cb5b516b93ad7001d0cc7de37c27345b1bf7811f0dd01b68a72d5f7e4c69186b4c7fbe1075e1d884606b90101168642790842b84059064af19ae1)
