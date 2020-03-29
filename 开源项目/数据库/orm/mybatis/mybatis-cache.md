
### 一级缓存
- SqlSession级别的缓存
- 代码：BaseExecutor#queryFromDatabase

- 过程：
    - 开启会话，创建一个SqlSession
    - SqlSession 持有：新的Executor
    - Executor  持有：PerpetualCache:localCache
    - PerpetualCache clearCache
        - SqlSession.close()
        - SqlSession.clearCache()
        - SqlSession.update().insert().delete()
        - SqlSession.commit().rollback()

### 二级缓存
- Mapper级别
- 代码在：CachingExecutor#query

### 核心类：
- CacheKey
    - 在： BaseExecutor#createCacheKey

- 实现类：
    - PerpetualCache : 缓存类 使用HashMap
    - ConcreteDecorator 包装类：
    - LruCache： 最久未访问，基于：LinkedHashMap -> https://www.tianxiaobo.com/2018/01/24/LinkedHashMap-%E6%BA%90%E7%A0%81%E8%AF%A6%E7%BB%86%E5%88%86%E6%9E%90%EF%BC%88JDK1-8%EF%BC%89/

    - ScheduledCache：定时清理
    - SerializedCache：返回的值是一份深copy
    - LoggingCache（必要）：会输出命中率，
    - SynchronizedCache（必要）：synchronized set，put 线程安全
    - BlockingCache：如果值为空，对该key 阻塞，知道put | remove 值 释放锁。这更是一种开放规范
```

Simple and inefficient version of EhCache's BlockingCache decorator.
It sets a lock over a cache key when the element is not found in cache.
This way, other threads will wait until this element is filled instead of hitting the database.
```

## 缓存：
### cacheKey
- 类：CacheKey
    - count
    - checksum
    - hashcode
    - keyString
        - statment.id
        - offset
        - limit
        - sql
        - parameter
        - environment


#### 一级缓存


#### 二级缓存
TransactionalCacheManager
    TransactionalCache
