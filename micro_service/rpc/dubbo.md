# dubbo
http://dubbo.io/Developer+Guide-zh.htm#DeveloperGuide-zh-%E6%95%B4%E4%BD%93%E8%AE%BE%E8%AE%A1



http://dubbo.io/Administrator+Guide-zh.htm#AdministratorGuide-zh-%E7%A4%BA%E4%BE%8B%E6%8F%90%E4%BE%9B%E8%80%85%E5%AE%89%E8%A3%85



http://dubbo.io/User+Guide-zh.htm







### 各层说明

config，配置层，对外配置接口，以ServiceConfig, ReferenceConfig为中心，可以直接new配置类，也可以通过spring解析配置生成配置类

proxy，服务代理层，服务接口透明代理，生成服务的客户端Stub和服务器端Skeleton，以ServiceProxy为中心，扩展接口为ProxyFactory

registry，注册中心层，封装服务地址的注册与发现，以服务URL为中心，扩展接口为RegistryFactory, Registry, RegistryService

cluster，路由层，封装多个提供者的路由及负载均衡，并桥接注册中心，以Invoker为中心，扩展接口为Cluster, Directory, Router, LoadBalance

monitor，监控层，RPC调用次数和调用时间监控，以Statistics为中心，扩展接口为MonitorFactory, Monitor, MonitorService

protocol，远程调用层，封将RPC调用，以Invocation, Result为中心，扩展接口为Protocol, Invoker, Exporter

exchange，信息交换层，封装请求响应模式，同步转异步，以Request, Response为中心，扩展接口为Exchanger, ExchangeChannel, ExchangeClient, ExchangeServer

transport，网络传输层，抽象mina和netty为统一接口，以Message为中心，扩展接口为Channel, Transporter, Client, Server, Codec

serialize，数据序列化层，可复用的一些工具，扩展接口为Serialization, ObjectInput, ObjectOutput, ThreadPool



### 模块说明

- dubbo-common 公共逻辑模块，包括Util类和通用模型。

- dubbo-remoting 远程通讯模块，相当于Dubbo协议的实现，如果RPC用RMI协议则不需要使用此包。

- dubbo-rpc 远程调用模块，抽象各种协议，以及动态代理，只包含一对一的调用，不关心集群的管理。

- dubbo-cluster 集群模块，将多个服务提供方伪装为一个提供方，包括：负载均衡, 容错，路由等，集群的地址列表可以是静态配置的，也可以是由注册中心下发。

- dubbo-registry 注册中心模块，基于注册中心下发地址的集群方式，以及对各种注册中心的抽象。

- dubbo-monitor 监控模块，统计服务调用次数，调用时间的，调用链跟踪的服务。

- dubbo-config 配置模块，是Dubbo对外的API，用户通过Config使用Dubbo，隐藏Dubbo所有细节。

- dubbo-container 容器模块，是一个Standlone的容器，以简单的Main加载Spring启动，因为服务通常不需要Tomcat/JBoss等Web容器的特性，没必要用Web容器去加载服务。




### 领域模型

- Protocol是服务域，它是Invoker暴露和引用的主功能入口，它负责Invoker的生命周期管理。

- Invoker是实体域，它是Dubbo的核心模型，其它模型都向它靠扰，或转换成它，它代表一个可执行体，可向它发起invoke调用，它有可能是一个本地的实现，也可能是一个远程的实现，也可能一个集群实现。

- Invocation是会话域，它持有调用过程中的变量，比如方法名，参数等。



### 基本原则

- 采用Microkernel + Plugin模式，Microkernel只负责组将Plugin，Dubbo自身的功能也是通过扩展点实现的，也就是Dubbo的所有功能点都可被用户自定义扩展所替换。

- 采用URL作为配置信息的统一格式，所有扩展点都通过传递URL携带配置信息。



组件：

- filter: 过滤链



- cluster: 集群



- router:

    - 负载均衡

        - random

        - RoundRobin

        - LeastActive





- 合并结果扩展

    - Array List Map Set



- 注册中心扩展

    - Registry

    有：

        - 广播

        - zk

        - redis



- 监控中心扩展 com.alibaba.dubbo.monitor.Monitor

    -



- 动态代理扩展

    javassist ?



- 编译器扩展



- 消息派发扩展



- 线程池扩展



- 序列号扩展



- 网络传输扩展



- 组网扩展 com.alibaba.dubbo.remoting.p2p.Networker



- Telnet命令扩展  com.alibaba.dubbo.remoting.telnet.TelnetHandler



- 扩展点加载扩展点 com.alibaba.dubbo.common.extension.ExtensionFactory



- 容器扩展
com.alibaba.dubbo.container.Container

- 页面扩展 com.alibaba.dubbo.container.page.PageHandler

- 缓存扩展 com.alibaba.dubbo.cache.CacheFactory

- 验证扩展 com.alibaba.dubbo.validation.Validation



- 日志适配器扩展
com.alibaba.dubbo.common.logger.LoggerAdapter


### 补充技能
- 设计原则：
    http://dubbo.io/Developer+Guide-zh.htm#DeveloperGuide-zh-%E6%95%B4%E4%BD%93%E8%AE%BE%E8%AE%A1

- 线程池：
    ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(3, new NamedThreadFactory("DubboMonitorSendTimer", true));
- 注解类的开发
- java SPI的理解  http://singleant.iteye.com/blog/1497259
- 核心扩展点写TCK (Technology Compatibility Kit)
