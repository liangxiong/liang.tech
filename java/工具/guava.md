# guava

## 基本类型

- AtomicLong :
  - cas 指令从机器指令级别操作保证并发的原子性
  - 高并发情况下：CAS的失败几率更高，重试次数更多

- LongAdder:
  - 先尝试用 cas, 失败
