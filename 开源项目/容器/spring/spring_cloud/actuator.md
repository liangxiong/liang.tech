# actuator

- /health
- /routes

## 数据分

- 应用配置信息类
    应用配置，环境变量，自动化配置报告
    - /autoconfig
           positiveMatches   匹配成功
           negativeMatches  匹配不成功     
    - /beans
    - /configprops       
        endpoints.configprops.enabled=false
    - env  属性  
        获取应用的环境属性：操作系统，jvm，应用
    - mappings  spirng mvc 控制器映射
    - info info 开头的配置

- 度量指标信息类
    内存，线程，http请求统计
    - /metrics  内存，线程，垃圾回收              ？？？ 持久化到哪？
    - /metrics/mem
        - processors uptime systemload.average
        - mem.* ：来之java.lang.Runtime
        - heap.*  ： 堆内存使用，来之 java.lang.management.MemoryMXBean.getHeapMemoryUsage => java.lang.mangement.MemoryUsage    
        - nonheap* ： 非堆内存使用  来之 java.lang.management.MemoryMXBean.getNonHeapMemoryUsage => java.lang.mangement.MemoryUsage
        - threads.* : 线程数，守护线程，线程峰值。来之java.lang.management.ThreadMXBean
        - classes* : 加载和卸载的类统计。 来之 java.lang.management.ClassLoadingMXBean
        - gc.* : 次数，耗时 来之 java.lang.management.GarbageCollectorMXBean
        - httpsessions.*  tomcat 的session信息
        - gauge*： http的请求耗时
        - counter.*  http 请求次数

    - /health 实现 HealthIndicator 接口
        - UP    
        - DOWN
        - UNKNOWN    ?
        - OUT_OF_SERVICE    ?
    - /dump 程序运行的线程信息
        来之 java.lang.management.ThreadMXBean.dumpAllThreads

    - /trace 返回http跟踪信息
        存储到 org.springframework.boot.actuate.trace.InMemoryTraceRepository     内存 100条

- 操作控制类
    读应用的关闭，刷新配置  
    - /shutdown
    endpoints.shutdown.enabled=true  
