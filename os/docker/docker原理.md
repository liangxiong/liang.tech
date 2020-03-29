容器 = cgroup + namespace + rootfs + 容器引擎

## Namespace：访问隔离，隔离出自己的空间

#### Mount Namespace  挂载点视图
- 参数：CLONE_NEWNS  编号：4026531840  版本：>=2.4.19
- NEWNS ( new Namespace )   命令： mount -t proc proc /proc
- 类似：根目录切换：chroot （change root directory）
- 隔离原系统的目录结构

#### UTS ( UNIX Time-sharing System) namespace
- 参数：CLONE_NEWUTS 编号：4026531838  版本：>=2.6.19
- 隔离 nodename 和 domainname

#### IPC ( Interprocess Communication) namespace
- 参数：CLONE_NEWIPC 编号：4026531839  版本：>=2.6.19  
- 用于隔离 System V IPC 和 POSIX message queues
- 命令：
  - ipcs -q 查看
  - ipcmk -Q 创建

#### PID namespace
- 参数：CLONE_NEWPID 编号：4026531836  版本：>=2.6.24
- 进程PID重新标号,不同的namespace 可以有相同的pid，都有自己的计数程序
- 是一个树状结构 1个root namespace , 多个child namespace 子节点
  - 父可以看到子节点，可通过信号量等方式对子节点中的进程产生影响
  - 子节点不可能通过 kill 和 ptrace 影响父或兄弟节点
  - pid = 1，会像传统Linux中的init进程一样拥有特权，起特殊作用，不可结束

- 命令：
  - pstree -pl：以树状图的方式展现进程之间的派生关系

#### Network Namespace：
- 参数：CLONE_NEWNET 编号：4026531957 版本：>=2.6.29
- 隔离网络设备，IP地址端口等网络栈的Namespace
- 让每个容器拥有独立的 （虚拟）网络设备
- 容器内的应用可以绑定到在的端口，各个namespace 不会冲突
- 牵涉问题：容器间的通讯 ，搭建网桥

#### User Namespace：
- 参数：CLONE_NEWUSER 编号：4026531837 版本：>=3.8
- 隔离用户组id
- NEWUSER ( new user)
- 宿主机的非root用户运行创建一个User Namespace，可以映射成root的权限，操作：/proc/pid/uid_map

- 命令：
  - id

## Cgroup ( control group) ：资源控制，对一组进程及子进程的资源限制，控制和统计的能力,资源的Qos
- 配置在：/sys/fs/cgroup

#### devices 设备权限控制

#### cpuset 分配指定的cpu和内存节点
- cpuset.cpus：允许使用的cpu列表
- cpuset.mems：允许进程使用的内存节点列表

#### cpu 控制cpu占用率
- cpu 比重：cpu.shares ，在争抢时，按比例分配
- cpu 带宽限制：普通进程一个周期内定额的执行，其他时间强制睡眠
  - cpu.cfs_period_us：周期
  - cpu.cfs_auota_us：定额

- 实时进程 cpu 带宽限制：
  - cpu.rt_period_us：周期
  - cpu.rt_runtime_us：定额

#### cpuacct 统计各个cgroup的cpu使用情况
- cpuacct.stat：报告cgroup 在用户态和内核态消耗的cpu时间，单位：USER_HZ，在x86 1USER_HZ = 0.01S

- cpuacct.usage：报告这个cgroup 消耗的总cpu时间，单位：纳秒

- cpuacct.usage_percpu：报告这个cgroup 在各个cpu上消耗的cpu时间，总和=cpuacct.usage

#### memory 限制内存的使用上限
- memory.limit_in_bytes：设定内存上线，单位字节。也可使用k/K，m/M，g/G

- 默认策略：超过上线，尝试回收内存，如果无法回收，触发OOM，选择并kill 进程

- memory.memsw.limit_in_bytes：设定内存+交换分区的使用总量

- memory.oom_control：0或1标志来关闭开启
  - 1：开启，如果尝试申请内存超过允许，被系统OOM，killer终止
  - 0：关闭，申请的内存超过允许，那么会被暂停，直到额外的内存被释放

- memory.stat：汇报内存使用信息

#### blkio 限制 block I/O带宽
- blkio.weight：设置权重，范围在100到1000之间，是比重分配，在争取同一块设备的带宽才会起作用

- blkio.weight_device：对具体的设备设置权重值
- blkio.throttle.read_bps_device：设置每秒读磁盘的带宽上限
- blkio.throttle.write_bps_device：设置每秒写磁盘的上限
- blkio.throttle.read_iops_device：设置每秒读磁盘的IOPS上限
- blkio.throttle.write_iops_device：设置每秒写磁盘的IOPS上限

## devices 控制对哪些设备有访问全新啊
- devices.list：显示目前允许被访问的设备列表
  - 格式：
    - a：所有设备
    - c：字符设备
    - d：块设备
  - 设备号：格式为 major:minor的设备号
  - 权限：
    - r：可读
    - w：可写
    - m：可创建设备结点 (mknod)

- devices.allow：允许相应的设备访问权限
- devices.deny：禁止相应的设备访问权限

#### freezer
- 用于 挂起（suspend）和 恢复（resume）cgroup 中的进程

#### net_cls
- 用于将cgroup中进程产生的网络包分类，以便tc（ traffic controller ）可以根据分类，区分出来自某个cgroup 的包并做限流或监控

#### net_prio
- 限制进程的网络流量优先级

####额外
- android 前后台应用 分到不同的Cgroup，配置不同的系统资源限额

## rootfs ：文件系统隔离 Union File System
跟内核的无缝融合 ，其在运行效率上的优势和绩效的系统开销

## 容器引擎 ：生命周期控制
## Runc  libContainer
#### LibContainer  
- docker 通过Libcontainer 实现对容器生命周期的管理，信息的设置和查询，以及监控和通信等功能
