# 动态逻辑管理平台
- [官网 https://www.xuxueli.com/xxl-glue/](https://www.xuxueli.com/xxl-glue/)

## 名词解释
- 逻辑单元：一个单独 的handler 胶水代码类

## 快速了解
- 使用该逻辑单元管理平台：维护 逻辑单元 代码，包括 代码的在线编辑，发布功能
- 发布后，使用 “GroovyClassLoader” 实时编译生效，扩展JVM的动态语言支持
- 兼容Spring：GLUE代码中支持@Resource和@Autowired两种方式注入Spring容器中服务

## 架构
![一图了解](http://res.liang3307.tech/liang.tech/%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%99/xxl-glue/res/diagram.jpg)

<img src = "http://res.liang3307.tech/liang.tech/%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%99/xxl-glue/res/diagram.jpg">

## 使用场景
### 日常场景：
- 给运营提供的临时数据处理
- 大触期间的 临时开关

### 三种常用模式
- 托管 “配置信息”:
  - 把配置信息作为该handler的返回值

- 托管 “静态方法”
  - 作为一个业务类实现：托管公共组件，方便组件统一维护和升级

- 托管 “动态服务”：
  - 灵活组装接口和服务，扩展服务的动态特性，作为公共服务
  - 场景：做业务 ”编排” 使用

## 关键技术点
### 实时编译生效：GroovyClassLoader.parseClass(codeSource);
- spring 注入：[SpringGlueFactory.injectService](https://github.com/liangxiong/liang.tech/blob/master/开源项目/任务调度/xxl-job/code/SpringGlueFactory.java)

### GroovyClassLoader 导致的 Permanet Generation空间被占满引起的Full GC
- 减少Class装载频率
  - 针对每个GlueHandler解析后生成的Java对象实例做缓存
  - 在接收到清除缓存的广播时解析生成新的GlueHandler实例对象，避免groovy的频繁解析，减少Class装载频率
- 周期性的异步刷新类加载器，避免因全局类加载器装载Class过多且不及时卸载导致的PermGen被用满 ??

### 代码管理：
- 代码存储在数据库
- 调试：把代码从数据库下载到工程的 resources/config/glue/xxxx.groovy
  - 和平时一样的启动调试的handler + “断点”

### zookeeper
- [zookeeper](https://github.com/liangxiong/liang.tech/blob/master/开源项目/协调/zookeeper/zookeeper.md)
