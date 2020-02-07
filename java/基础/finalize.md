# finalize

## finalization 机制
- 三种状态
  - 可达
  - 可复活
  - 不可达
- 过程：
  - 垃圾回收器在对象没有引用情况下才调用其finalize方法
  - finalize方法可能为当前添加新的引用，对象回到可达状态
  - 再次出现没有引用存在，finalize 不会再次被调用
- 实现代码 [FinalizerThread 源码] (https://www.infoq.cn/article/jvm-source-code-analysis-finalreference/)
  - 实现原理
    - 初始化Object类时将构造函数里的return指令替换为_return_register_finalizer指令
    - 该指令并不是标准的字节码指令，是 hotspot 扩展的指令，这样在处理该指令时调用Finalizer.register方法，以很小的侵入性代价完美地解决了这个问题

## 注意点
- finalize 可能会导致对象复活
- finalize()函数的执行时间是没有保障，由GC线程决定
- finalize()函数有可能影响GC的性能
