# 线上故障调查常用命令：

## 常看
### 系统
- top 命令：
  - top - 23:47:07 up 2 days,  8:13,  0 users,  load average: 0.35, 0.30, 0.33
    - 当前时间，运行时间，当前登录用户数，系统负载：1分钟、5分钟、15分钟前到现在的平均值 （cpu个数）
  - Tasks:  33 total,   1 running,  28 sleeping,   2 stopped,   2
    - 进程总数， 正在运行的进程数，睡眠的进程数，停止的进程数，僵尸进程数
  - Cpu0  :  2.7%us,  2.7%sy,  0.0%ni, 89.7%id,  0.0%wa,  0.0%hi,  0.0%si,  5.0%st
    - us 用户空间占用CPU百分比,
    - sy 内核空间占用CPU百分比
    - ni 用户进程空间内改变过优先级的进程占用CPU百分比
    - id 空闲CPU百分比
    - wa 等待输入输出的CPU时间百分比
    - hi：硬件CPU中断占用百分比
    - si：软中断占用百分比
    - st：虚拟机占用百分比, 被别个虚拟机占用的百分比
  - Mem:   1016200 total,   949520 used,    66680 free,    56300 buffers
    - total   物理内存总量
    - used    使用的物理内存总量
    - free    空闲内存总量
    - buffers 用作内核缓存的内存量
  - Swap:        0k total,        0k used,        0k free,   241704k cached
    - total 交换区总量
    - used  使用的交换区总量
    - free  空闲交换区总量
    - cached 缓冲的交换区总量,内存中的内容被换出到交换区，而后又被换入到内存，但使用过的交换区尚未被覆盖
  - PID USER  PR  NI  VIRT  RES  SHR S %CPU %MEM  TIME+ COMMAND
    - PID     进程id
    - USER    进程所有者的用户名
    - PR      优先级
    - NI      nice值。负值表示高优先级，正值表示低优先级
    - VIRT    进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
    - RES     进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
    - SHR     共享内存大小，单位kb
    - S       进程状态(D=不可中断的睡眠状态,R=运行,S=睡眠,T=跟踪/停止,Z=僵尸进程)
    - COMMAND 命令名/命令行

- vmstat 5  5  每隔5秒采样5次
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 3103820      0 156928    0    0     0     0    9   51  4  2 90  0  3
 - procs 进程
    - r: 运行队列中进程数量
    - b： 等待IO的进程数量
 - Memory（内存）：
    - swpd: 使用虚拟内存大小
    - free: 可用内存大小
    - buff: 用作缓冲的内存大小
    - cache: 用作缓存的内存大小
 - Swap：
    - si: 每秒从交换区写到内存的大小
    - so: 每秒写入交换区的内存大小
 - IO：块的大小为1024bytes
    - bi: 每秒读取的块数
    - bo: 每秒写入的块数
 - System 系统
    - in: 每秒中断数，包括时钟中断。【interrupt】
    - cs: 每秒上下文切换数。       【count/second】
 - CPU（以百分比表示）：
    - us: 用户进程执行时间(user time)
    - sy: 系统进程执行时间(system time)
    - id: 空闲时间(包括IO等待时间),中央处理器的空闲时间 。以百分比表示
    - wa: 等待IO时间
  - 总结：
    - r > 4 & id < 40 表示负荷重
    - cs 使用： pidstat -w -p pid 查看明细

- pidstat -w -p $pid 查看上下文切换
  - Cswch/s:每秒主动任务上下文切换数量
    - 如调用 sleep()、join()、wait()、Lock
  - Nvcswch/s:每秒被动任务上下文切换数量
    - 分配的cpu时间片用完了被调度器调度所致

### 网络
- 查看当前网络连接数：用于网络连接 TIME_WAIT状态导致新连接打开不了
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'

### cpu
- 查看cpu 前10的进程：
  - ps -aeo pcpu,user,pid,cmd | sort -nr | head -10


- 查看 cpu 闲置率,使用率 还是用top 命令吧
  - grep "cpu " /proc/stat | awk -F ' ' '{total = $2 + $3 + $4 + $5} END {print "idle \t used\n" $5*100/total "% " $2*100/total "%"}'


### 内存：
- 按内存使用前10：
  - ps axo %mem,pid,euser,cmd | sort -nr | head -10

- 使用swamp 临时交换分区
  - for file in /proc/*/status ; do awk '/VmSwap|Name|^Pid/{printf $2 " " $3}END{ print ""}' $file; done | sort -k 3 -n -r | head -10

## 磁盘：
- 查看文件占用大小：
  - du -sh *
  - df -h

- 查询超过500M的文件
  - find . -type f -size +500M

## java
- java 内存组成：
  - Heap（堆区）
  - Code Cache（代码缓存区)
  -  Metaspace（元空间）
  - Symbol tables（符号表）
  - Thread stacks（线程栈区）
  - Direct buffers（堆外内存）
  - JVM structures（其他的一些 JVM 自身占用）
  - Mapped files（内存映射文件）
  - Native Libraries（本地库）+ ...

### 进程
- jps 显示所有的java 进程

### 线程：
- jstack： jstack pid
  - -l 长列表. 打印关于锁的附加信息
  - -F 强制打印栈信息
  - -m 打印java和native c/c++框架的所有栈信息.
  - -h | -help 打印帮助信息
  - 线程状态：
    - Deadlock 死锁（重点关注）
    - Waiting on condition 等待资源（重点关注）
    - Waiting on monitor entry 等待获取监视器（重点关注）
    - Blocked 阻塞（重点关注）
    - Runnable  执行中
    - Suspended 暂停，
    - Object.wait() 或 TIMED_WAITING  对象等待中，
    - Parked 停止
  - 查看个线程状态数量
    - jstack $pid | grep java.lang.Thread.State:|sort|uniq -c | awk '{sum+=$1; split($0,a,":");gsub(/^[ \t]+|[ \t]+$/, "", a[2]);printf "%s: %s\n", a[2], $1}; END {printf "TOTAL: %s",sum}';



- 找出线程中 占cpu最高的：
  - top -Hp pid  找出使用cpu最高的线程
  - $printf "%x\n" threadId 根据线程id获取十六进制
  - jstack pid | grep -A 10 threadId
  - [学习代码参考](https://github.com/oldratlee/useful-scripts/blob/dev-2.x/docs/java.md#%E7%94%A8%E6%B3%95)


### 内存信息：
- jmap -help
  - heap 打印堆的统计
  - histo:live 打印活跃的堆中的统计信息
    - 对象数量、内存大小(单位：字节)、完全限定的类名
    - 根据类型，结合业务场景，找出泄漏点
  - clstats 类加载器信息
    - 名称、活跃度、地址、父类加载器、所加载的类的数量和大小
  - finalizerinfo 等待终结的对象信息
  - dump:format
    - jmap -dump:format=b,file=heapdump.phrof pid
  - 查询占用排名前50的的对象
    jmap –histo:live $pid | sort -n -r -k2 | head -n 50   


- jhat(Java堆分析工具) Java Heap Analysis Tool, 将hprof文件转成html的
  - jhat -J-Xmx512M a1.log

### 致命异常：
- java.lang.OutOfMemoryError: Java heap space。原因：堆中（新生代和老年代）无法继续分配对象了、某些对象的引用长期被持有没有被释放，垃圾回收器无法回收、使用了大量的 Finalizer 对象，这些对象并不在 GC 的回收周期内等。一般堆溢出都是由于内存泄漏引起的，如果确认没有内存泄漏，可以适当通过增大堆内存。

- java.lang.OutOfMemoryError：GC overhead limit exceeded。原因：垃圾回收器超过98%的时间用来垃圾回收，但回收不到2%的堆内存，一般是因为存在内存泄漏或堆空间过小。

- java.lang.OutOfMemoryError: Metaspace或java.lang.OutOfMemoryError: PermGen space。排查思路：检查是否有动态的类加载但没有及时卸载，是否有大量的字符串常量池化，永久代/元空间是否设置过小等。

- java.lang.OutOfMemoryError : unable to create new native Thread。原因：虚拟机在拓展栈空间时，无法申请到足够的内存空间。可适当降低每个线程栈的大小以及应用整体的线程个数。此外，系统里总体的进程/线程创建总数也受到系统空闲内存和操作系统的限制，请仔细检查。注：这种栈溢出，和 StackOverflowError 不同，后者是由于方法调用层次太深，分配的栈内存不够新建栈帧导致。


### GC:
- 显示最后一次或当前正在发生的垃圾收集的诱发原因
  - jstat -gccause $pid

- 显示各个代的容量及使用情况
  - jstat -gccapacity $pid

- 显示新生代容量及使用情况
  - jstat -gcnewcapacity $pid

- 显示老年代容量
  - jstat -gcoldcapacity $pid

- 显示垃圾收集信息（间隔1秒持续输出）
  - jstat -gcutil $pid 1000

- jstat -gcutil
