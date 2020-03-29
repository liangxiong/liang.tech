- 需求：
    - 资源加载
    - 接口类的代理实现
    - 数据类型的转换
    - xml 配置中的动态脚本实现
    - 一二级缓存的实现
    - 扩展：
        - 数据源切换成多个数据库
- SqlSessionFactory
    - XMLConfigBuilder
        - parse：Configuration
            - parseConfiguration：
                - propertiesElement： Configuration.variables
                - loadCustomVfs：

### 配置解析：

#### Configuration

##### properties：属性，可动态替换

- 放在：configuration.variables

```

<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>

${url}

```

##### settings：配置信息
- 放在：configuration.成员变量中

```

<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>

    <setting name="logImpl" value="LogFactory  SLF4J、LOG4J、LOG4J2、JDK_LOGGING、COMMONS_LOGGING、STDOUT_LOGGING、NO_LOGGING"/>

```

- 校验配置是否可配置,使用反射

MetaClass metaConfig = MetaClass.forClass(Configuration.class, localReflectorFactory);
 for (Object key : props.keySet()) {
      if (!metaConfig.hasSetter(String.valueOf(key))) {
        throw new BuilderException("The setting " + key + " is not known. Make sure you spelled it correctly (case sensitive).");
      }
    }



##### typeAliases：类型别名，内建常见的java类型，_integer，integer，collection
- 放在：configuration.typeAliasRegistry

```

<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <package name="domain.blog"/> 自动扫描类，首字母小写
</typeAliases>

```



##### plugins：插件 字段： interceptorChain 分页插件

- 放在：configuration.interceptorChain

- 可拦截的方法：

    - Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)

    - ParameterHandler (getParameterObject, setParameters)

    - ResultSetHandler (handleResultSets, handleOutputParameters)

    - StatementHandler (prepare, parameterize, batch, update, query)

```

<plugins>
  <plugin interceptor="org.mybatis.example.ExamplePlugin">
    <property name="someProperty" value="100"/>
  </plugin>
</plugins>

```



```

拦截在 Executor 实例中所有的 “update” 方法调用

@Intercepts({@Signature(
  type= Executor.class,
  method = "update",
  args = {MappedStatement.class,Object.class})})
public class ExamplePlugin implements Interceptor {
  public Object intercept(Invocation invocation) throws Throwable {
    return invocation.proceed();
  }
  public Object plugin(Object target) {
    return Plugin.wrap(target, this);
  }
  public void setProperties(Properties properties) {
  }
}

```

##### objectFactory：创建结果对象的新实例时，可加入特殊功能  DefaultObjectFactory implements ObjectFactory

- configuration.objectFactory

```

<objectFactory type="org.mybatis.example.ExampleObjectFactory">
  <property name="someProperty" value="100"/>
</objectFactory

```



##### objectWrapperFactory

- configuration.objectWrapperFactory

##### reflectorFactory

- configuration.reflectorFactory



##### environments

- configuration.environment

- Environment

    - id

    - TransactionFactory  <-  TransactionFactory

    - DataSource  <-  DataSourceFactory

```

<environments default="development">
  <environment id="development">
    <transactionManager type="[JDBC|MANAGED]">  查看：TransactionFactory
          <property name="..." value="..."/>
    </transactionManager>


    <dataSource type="POOLED"> 查看：DataSourceFactory
          <property name="driver" value="${driver}"/>
          <property name="url" value="${url}"/>
          <property name="username" value="${username}"/>
          <property name="password" value="${password}"/>
    </dataSource>



    <dataSource type="org.myproject.C3P0DataSourceFactory">
          <property name="driver" value="org.postgresql.Driver"/>
          <property name="url" value="jdbc:postgresql:mydb"/>
          <property name="username" value="postgres"/>
          <property name="password" value="root"/>
    </dataSource>
  </environment>
</environments>

```

##### databaseIdProvider：获取数据库的类型：mysql

- configuration.databaseId

```

<databaseIdProvider type="DB_VENDOR">
  <property name="SQL Server" value="sqlserver"/>
  <property name="DB2" value="db2"/>        
</databaseIdProvider>

```

##### typeHandlers：PreparedStatement  不同值的set，get

- configuration.typeHandlerRegistry

- 如：IntegerTypeHandler

- 配置信息放在：TypeHandlerRegistry  成员变量：

    - JDBC_TYPE_HANDLER_MAP  <jdbcType,TypeHandler>

    - TYPE_HANDLER_MAP <javaType,<jdbcType,TypeHandler>>

        - jdbcType 可为空

    - UNKNOWN_TYPE_HANDLER

    - ALL_TYPE_HANDLERS_MAP <TypeHandler.clss,TypeHandler>

    - NULL_TYPE_HANDLER_MAP

    -

```

<typeHandlers>
  <typeHandler handler="org.apache.ibatis.type.EnumOrdinalTypeHandler" javaType="java.math.RoundingMode"/>
    <typeHandler jdbcType="TINYINT" javaType="xyz.coolblog.constant.ArticleTypeEnum" handler="xyz.coolblog.mybatis.ArticleTypeHandler"/>
</typeHandlers>

```

##### mappers：映射器

- configuration.mapperRegistry

- 各个po对象sql的xml mapper 解析

```

<mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/> <!-- 使用相对于类路径的资源引用 -->

    <mapper url="file:///var/mappers/AuthorMapper.xml"/> <!-- 使用完全限定资源定位符（URL） -->

     <mapper class="org.mybatis.builder.BlogMapper"/> <!-- 使用映射器接口实现类的完全限定类名 -->

    <package name="org.mybatis.builder"/>  <!-- 将包内的映射器接口实现全部注册为映射器 -->
  </mappers>

```

##### VFS ？？

### 核心组件
- SqlSession 作为MyBatis工作的主要顶层API，表示和数据库交互的会话，完成必要数据库增删改查功能
- Executor MyBatis执行器，是MyBatis 调度的核心，负责SQL语句的生成和查询缓存的维护
    - CachingExecutor : 二级缓存执行类
    - 三种：SimpleExecutor，ReuseExecutor，BatchExecutor

- StatementHandler 封装了JDBC Statement操作，负责对JDBC statement 的操作，如设置参数、将Statement结果集转换成List集合。
    - PreparedStatementHandler
- ParameterHandler 负责对用户传递的参数转换成JDBC Statement 所需要的参数，
- ResultSetHandler 负责将JDBC返回的ResultSet结果集对象转换成List类型的集合；
    - 实现类：DefaultResultSetHandler

- TypeHandler 负责java数据类型和jdbc数据类型之间的映射和转换
- MappedStatement MappedStatement维护了一条<select|update|delete|insert>节点的封装，
- SqlSource 负责根据用户传递的parameterObject，动态地生成SQL语句，将信息封装到BoundSql对象中，并返回
- BoundSql 表示动态生成的SQL语句以及相应的参数信息
- Configuration MyBatis所有的配置信息都维持在Configuration对象之中。
- MapperProxyFactory 接口代理工厂类这个类主要是试用jdk代理 mapper的实现 以实现调用SqlSession，接口类名称对应配置文件的nameSpace的值，等标签的id 对应方法名称

##### MapperStatement
##### ParamterStatement
##### ResultStatement
