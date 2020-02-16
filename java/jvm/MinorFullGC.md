# GC

## Stop the World
- 停止所有的应用程序线程执行


## Minor GC
![MinorGC工作原理图](https://github.com/liangxiong/liang.tech/blob/master/java/jvm/res/MinorGC工作原理图.jpg)

## Full GC
### 触发FullGC前提
- System.gc

- 老年代空间不足
  - 如果回收不不足，抛出错误：java.lang.OutOfMemoryError:Java heap space

- 永久代空间满了
  - 加载，反射的类多时，未配置采用CMS GC情况下会执行 Full GC
  - 如果回收不不足，抛出错误：java.lang.OutOfMemoryError:PermGen Space

- CMS GC 出现 Promotion Failed 和 Concurrent Mode Failure
  - 采用CMS GC 进行老年代GC
  - Promotion Failed:
    - 表示在Minor GC时，Survivor Space 放不下，只能放到老年代，而老年代也放不下
  - Concurrent Mode Failure
    - 在执行CMS GC时，同时有对象要放入老年代，而此时老年代空间不足造成
  - 解决：
    - 增大 Survivor Space，老年代空间
    - 调低触发并发GC的比率，
      - -XX:CMSMaxAbortablePrecleanTime=5 (单位：毫秒) 表示preclean阶段最多持续多少毫秒 默认 5s

- 统计得到Minor GC 晋升到老年代的平均大小 大于 老年代剩余空间
