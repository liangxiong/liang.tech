##### BoundSql


- sql    String    一个完整的 SQL 语句，可能会包含问号 ? 占位符
- parameterMappings    List    参数映射列表，SQL 中的每个 #{xxx} 占位符都会被解析成相应的 ParameterMapping 对象
- parameterObject    Object    运行时参数，即用户传入的参数，比如 Article 对象，或是其他的参数
- additionalParameters    Map    附加参数集合，用于存储一些额外的信息，比如 datebaseId 等
- metaParameters    MetaObject    additionalParameters 的元信息对象



##### SqlSource

- DynamicSqlSource
- RawSqlSource
- StaticSqlSource

    - sql

    - parameterMappings

    - configuration
- ProviderSqlSource
- VelocitySqlSource



##### SqlNode

- StaticTextSqlNode 静态

- TextSqlNode 带有 ${} 占位符的文本

- IfSqlNode 存储 <if> 节点的内容

- MixedSqlNode 内部维护了一个 SqlNode 集合

- WhereSqlNode

- TrimSqlNode
