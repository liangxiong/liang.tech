# tomcat

- 参考：https://www.iteye.com/category/283575

## 架构

- 核心组件
  - Server
    - Service
      - Connector：处理请求监听 TCP端口绑定
      - Container：处理请求处理，用于封装和管理Servlet，以及具体处理Request请求
        - Engine: 引擎，用来管理多个站点，一个Service最多只能有一个Engine
        - Host: 代表一个站点，也可以叫虚拟主机，通过配置Host就可以添加站点
        - Context: 代表一个应用程序
        - Wrapper: 每一Wrapper封装着一个Servlet
      - Jasper
      - Naming
      - Session
      - Loging
      - JMX

- 执行：Logger.info
  - callAppenders
    - parent.appendLoopOnAppenders(ILoggingEvent)


### 核心类：
#### ProtocolHandler
- Endpoint 处理底层Socket的网络连接
  - AbstractEndpoint
    - Acceptor 监听请求
    - AsyncTimeout 检查异步Request的超时
    - Handler 处理接收到的Socket，在内部调用Processor进行处理

- Processor 将Endpoint接收到的Socket封装成Request
- Adapter 用Request交给Container进行具体的处理
