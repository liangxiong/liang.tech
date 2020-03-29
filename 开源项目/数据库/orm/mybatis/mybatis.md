# mybatis


- [中文官网] (https://mybatis.org/mybatis-3/zh/index.html)


## 基本流程：

- 读取配置

- 创建SqlSessionFactoryBuilder

- 通过SqlSessionFactoryBuilder 创建 SqlSessionFactory

- 通过 SqlSessionFactory 创建 SqlSession

- 为 Dao 接口生成代理

- 调用接口方法访问数据库

![整体架构](https://github.com/liangxiong/liang.tech/blob/master/开源项目/数据库/orm/mybatis/res/mybatis_execute.png)
