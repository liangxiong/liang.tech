# 流Stream
## 是什么
- 是一个流，数据源：集合，数组，I/O channel， 产生器generator
- 声明的方式处理数据
- 类似用 SQL 语句从数据库查询数据的直观方式来提供一种对 Java 集合运算和表达的高阶抽象
- 聚合操作 类似SQL语句一样的操作 总结：
  - 申明为 stream
  - 过滤 filter
  - 排序 sorted
  - map
  - 收集 collect

## 基本特性
- Pipelining 中间操作都会返回流对象本身
  - 多个操作可以串联成一个管道，如同流式风格（fluent style）

- 内部迭代：
  - Iterator或者ForEach的，显式的外部进行迭代
  - 提供了内部迭代的方式， 通过访问者模式(Visitor)实现

## 两大流
- stream() − 为集合创建串行流
- parallelStream() − 为集合创建并行流

## 常用方法：
- forEach 迭代流中的每个数据

```
new Random().ints().limit(10).forEach(System.out::println);
```

- map 映射每个元素到对应的结果

```
stream().map( i -> i*i).distinct()
```

- filter 设置的条件过滤出元素

```
filter(string -> string.isEmpty())
```

- limit 获取指定数量的流

```
strem().limit(10)
```

- sorted 对流进行排序

```
stream().sorted()
```

- Collectors 归约操作,将流转换成集合和聚合元素, 返回列表或字符串
  - Collectors.toList()
  - Collectors.joining(", ")

- 统计：
  - mapToInt

```
IntSummaryStatistics stats = numbers.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("列表中最大的数 : " + stats.getMax());
System.out.println("列表中最小的数 : " + stats.getMin());
System.out.println("所有数之和 : " + stats.getSum());
System.out.println("平均数 : " + stats.getAverage());
```
