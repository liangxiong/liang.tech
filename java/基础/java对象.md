# 对象
## 对象头
- 自身运行元数据，如：哈希码，GC分代年龄，锁状态标志等，同事包含：类型指针指向 元数据，表明该所属的类型
- 32位系统 占用 8byte
- 64位系统 占用 16byte
- 可以用 JOL

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

## 实例数据
- 真正存储的有效信息 包括
  - 程序代码中定义的各种类型的字段，包括extend

## 对齐填充
- 不是必须存储，起到占位符作用
