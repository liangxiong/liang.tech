- 入口：
DefaultSqlSessionFactory.openSession

## MapperMethod:
- 支持4中查询：
    - INSERT/UPDATE/DELETE
    - CALL 存储过程
    - SELECT    
        - executeWithResultHandler
        - executeForMany：多了一层对数组返回的包装
        - executeForMap
        - executeForCursor

- 执行入口：
    - execute
    - getNamedParams：ParamNameResolver.getNamedParams：dao层的入参到xml sql 层的入参转换。不同点在于放入有入参：RowBounds，ResultHandle
        - 0个参数：null
        - 1个无注解参数：
        - 其他：使用map 包装成ognl的取值上下文
            - 有注解的单个参数    
            - 多个参数
            - 默认：都会在map放入：<param + index, args[ index ]  >

    - SqlSession.sql：进入sql 执行
        - DefaultSqlSession.selectOne
            - selectList(pram，RowBounds.DEFAULT)
                - MappedStatement ms = configuration.getMappedStatement(statement);
                - executor.query(ms, wrapCollection(parameter), rowBounds, Executor.NO_RESULT_HANDLER);

        -CachingExecutor.query  二级缓存
            - BoundSql：ms.getBoundSql(parameterObject);
            - CacheKey：createCacheKey,极其长
            - query：判断是否有缓存
            - 没有缓存：SimpleExecutor.doQuery
    - rowCountResult：返回数据包装

- 核心成员：
    - SqlCommand
        - name：MappedStatement.id
        - SqlCommandType：UNKNOWN, INSERT, UPDATE, DELETE, SELECT, FLUSH;
    - MethodSignature
        - returnsMany：是否返回多个
        - returnsMap：是否返回map
        - mapKey：返回map的key
        - returnsVoid：是否不返回
        - returnsCursor：是否返回游标
        - returnType：返回类型
        - ResultHandle：resultHandlerIndex：第几个是入参解析器
        - RowBounds：rowBoundsIndex：第几个入参是 分页参数
        - ParamNameResolver：paramNameResolver：入参解析器
            - SortedMap<Integer,String> names：key位置从0开始，value：注解的名称，或jdk1.8以上的参数名 或 有效入参的位置从0开始
            - hasParamAnnotation：是否有注解

## SqlSession:
- 4个实现类：
    - DefaultSqlSession
    - SqlSessionManager
    - SqlSessionTemplate spring 使用

## SqlSource
- 方法：
    - getBoundSql 根据入参返回具体的Bound sql

- 实现类：
    - DynamicSqlSource：动态 SQL，只要包含 包含${}，或者包含 <if>、<where> 等标签时
    - RawSqlSource
    - StaticSqlSource
    - ProviderSqlSource
    - VelocitySqlSource



## BoundSql 由上面的 SqlSource创建
- 来源：MappedStatement.getBoundSql
- 字段：
|变量名                                      |   类型           | 用途                                                                        |
|-----------------------------------|:-------------:|----------------------------------------------------------:|
| sql                                            | String          |  一个完整的 SQL 语句，可能会包含问号 ? 占位符|
| parameterMappings                |  List             | 入参映射列表，SQL 中的每个 #{xxx} 占位符都会被解析成相应的 ParameterMapping 对象 |
| parameterObject                     | Object          | 运行时参数，即用户传入的参数，比如 Article 对象，或是其他的参数 |

| additionalParameters              |  Map            | 附加参数集合，用于存储一些额外的信息，比如 datebaseId 等 |

| metaParameters                      | MetaObject  |additionalParameters 的元信息对象 |





## SqlSourceBuilder:   解析 #{}

#### ParameterMapping

- 核心功能：

    - ParameterMapping. parseParameterMapping ：使用的是：ParameterExpression，将 #{xxx} 占位符中的内容解析成 Map 如：

        - #{age,javaType=int,jdbcType=NUMERIC,typeHandler=MyTypeHandler}

        - {"property": "age", "typeHandler": "MyTypeHandler","jdbcType": "NUMERIC", "javaType": "int"}

- 字段：
    - property：java字段名
    - mode：ParameterMode.IN | OUT |  INOUT
    - javaType：java类型
    - jdbcType：jdbc类型
    - numericScale
    - typeHandler
    - resultMapId
    - jdbcTypeName
    - expression

## 执行者

- Executor：由DefaultSqlSessionFactory.openSession
    - BaseExecutor：公共方法
        - BatchExecutor
        - ClosedExecutor
        - ReuseExecutor
        - SimpleExecutor：
    - CachingExecutor：装饰器，二级缓存功能，默认是打开的


## StatementHandler
- BaseStatementHandler
    - CallableStatementHandler
    - PreparedStatementHandler
    - SimpleStatementHandler
- RoutingStatementHandler
    - StatementHandler delegate

## DynamicContext
- sql生成的上下文，也存放执行sql的入参
- 变量：
    - bindings：绑定的入参
    - sqlBuilder：sql
    - uniqueNumber：
- 实现类：
    - DynamicContext
    - ForEachSqlNode.FilteredDynamicContext
    - ForEachSqlNode.PrefixedContext
    - TrimSqlNode.FilteredDynamicContext
