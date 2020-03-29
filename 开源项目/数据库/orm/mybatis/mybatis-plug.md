##### 允许拦截的方法

- Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
- ParameterHandler (getParameterObject, setParameters)
- ResultSetHandler (handleResultSets, handleOutputParameters)
- StatementHandler (prepare, parameterize, batch, update, query)

```

@Intercepts({
    @Signature(
        type = Executor.class,
        method = "query",
        args ={MappedStatement.class, Object.class, RowBounds.class, ResultHandler.class}
    )
})
public class ExamplePlugin implements Interceptor {
    // 省略逻辑
}

```

##### 执行流程：
- 代码：DefaultSqlSessionFactory
- DefaultSqlSessionFactory.openSession
    - openSessionFromDataSource
        - configuration.newExecutor
            - BatchExecutor
            - ReuseExecutor
            - SimpleExecutor
            - CachingExecutor
            - executor = (Executor) interceptorChain.pluginAll(executor)
                - Plugin.wrap(target, this)
