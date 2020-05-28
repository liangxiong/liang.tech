# 对象
## 对象结构
- 对象头：
  - Mark Word 64bit
    - 自身运行元数据，如：哈希码，GC分代年龄，锁状态标志等，同事包含：类型指针指向 元数据，表明该所属的类型

  - Klass Word 64bit 类指针
    - 指向方法区中Class信息的指针，知道是哪个Class的实例
    - 32位的JVM为32bit，64位的JVM为64bit
    - UseCompressedOops 开启指针压缩

  - 数组长度：64bit
    - 当对象是是数组时，才会有这部分

- 对象体：
  - 用于保存对象属性和值的主体部分，占用内存空间取决于对象的属性数量和类型


## 对象头的三个部分
- Mark Word
  - 正常
    - unused: 25bit 未使用
    - identity hashcode: 31bit 对象hashcode
      - 当持有偏向、轻量级、重量级锁时，将hashcode 移动到管程Monitor
    - unused: 1bit 未使用
    - age: 4bit 分代年龄，在Survivor区复制一次，年龄增加1
    - biased_lock: 1bit 值: 0
    - lock: 2bit 值: 01

  - 偏向锁
    - thread: 54bit  持有偏向锁的线程ID
    - epoch: 2bit   偏向时间戳
    - unused: 1bit  未使用
    - age: 4bit 分代年龄
    - biased_lock: 1bit 值: 1  是否启用偏向锁标记
    - lock: 2bit 值: 01

  - 轻量级锁
    - ptr_to_lock_record: 62bit  指向栈中锁记录的指针
    - lock: 2bit 值: 00

  - 重量级锁
    - ptr_to_heavyweight_monitor: 62bit  指向关联的Monitor对象
    - lock: 2bit 值：10

  - GC
    - lock: 11

- 可以用 JOL 查看类，实列信息

```
<dependency>
    <groupId>org.openjdk.jol</groupId>
    <artifactId>jol-core</artifactId>
</dependency>

//查看对象内部信息
ClassLayout.parseInstance(obj).toPrintable();
//查看对象外部信息
GraphLayout.parseInstance(obj).toPrintable();
//获取对象总大小
GraphLayout.parseInstance(obj).totalSize();
```
