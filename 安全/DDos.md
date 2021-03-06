# Ddos  Denial of Service  拒绝服务

- 目的：
  - 使计算机或网络无法提供正常的服务，停止响应甚至奔溃
  - 不包括侵入目标服务器或目标网络设备

- 常见：
  - 带宽攻击
  - 连通性攻击

- 手段：
  - 攻击网络协议实现的缺陷
  - 耗尽被攻击对象的资源
    - 带宽资源
    - 文件系统空间容量
    - 服务器的进程数，线程数，连接数

- Pingflood
  - 短时间内向目的主机发送大量ping包，造成网络堵塞或主机资源耗尽

- Synflood
  - 攻击者：以多个随机的源主机地址向目的主机发送SYN包
  - 攻击者：而在收到目的主机的SYN ACK后并不回应
  - 目的主机：为这些源主机建立了大量的连接队列，
    - 而且由于没有收到ACK一直维护着这些队列
    - 造成了资源的大量消耗而不能向正常请求提供服务。

- Smurf
  - 向一个子网的广播地址发一个带有特定请求（如ICMP回应请求）的包
  - 将源地址伪装成想要攻击的主机地址
  - 子网上所有主机都回应广播包请求而向被攻击主机发包，使该主机受到攻击

- Land-based
  - 攻击者将一个包的源地址和目的地址都设置为目标主机的地址
  - 然后将该包通过IP欺骗的方式发送给被攻击主机
  - 这种包可以造成被攻击主机因试图与自己建立连接而陷入死循环

- Ping of Death
  - 根据TCP/IP的规范，一个包的长度最大为65536字节
  - 一个包分成的多个片段的叠加超过65536字节
  - 当一个主机收到了长度大于65536字节的包时，他不知道该怎么干：宕机或重新启动

- Teardrop
  - IP数据包在网络传递时，数据包可以分成更小的片段
  - 攻击者可以通过发送两段（或者更多）数据包来实现TearDrop攻击：
    - 第一个包的偏移量为0，长度为N
    - 第二个包的偏移量小于N
  - 为了合并这些数据段，TCP/IP堆栈会分配超乎寻常的巨大资源，从而造成系统资源的缺乏甚至机器的重新启动

- PingSweep
  - ICMP Echo轮询多个主机
