# 应用程序协调服务

## 内部流程：


### Watch 监听
- 特点: 只会监听一次，如果有变化，要先重新监听，然后 再取数据（这是最新的数据）




## 数据类型
### 数据类型 CreateMode：
- 持久节点：创建后一直存在，除非主动删除
- 临时节点：创建改节点的客户端会话失效会消失，ephemeral
- 顺序节点：会附加一个单调递增的值

- PERSISTENT：持久节点
- PERSISTENT_SEQUENTIAL：持久顺序节点
- EPHEMERAL：临时节点
- EPHEMERAL_SEQUENTIAL：临时顺序节点

### WatchedEvent
- Event.KeeperState.Expired

- Event.EventType.NodeDataChanged
