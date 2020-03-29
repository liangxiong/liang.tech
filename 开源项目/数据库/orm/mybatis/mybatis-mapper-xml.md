## XMLMapperBuilder
- 成员变量：
    - MapperBuilderAssistant <-- newe MapperBuilderAssistant(configuration, resource);
    - XPathParser
    - sqlFragments <-- configuration.sqlFragments
    - resource

### 解析过程：
#### cache-ref：其他命名空间缓存配置的引用。
- 放在： configuration.cacheRefMap
- 异常会放在：configuration.incompleteCacheRefs  在：parsePendingCacheRefs 重新解析  ？？？
- CacheRefResolver.resolveCacheRef
    - MapperBuilderAssistant
    - cacheRefNamespace

#### cache : 缓存，给定命名空间的缓存配置
- configuration.caches
- MapperBuilderAssistant.useNewCache
    - CacheBuilder
        - id：nameSpace
        - implementation：存储底层 默认：PerpetualCache
        - decorators[] : 装饰器 默认：LruCache
        - size：缓存对象个数
        - clearInterval：刷新间隔？ 怎么实现
        - readWrite：true：表示返回的缓存对象不可修改
        - properties：属性 设置：setCacheProperties
        - blocking：

```

<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
    <property name="timeToIdleSeconds" value="3600"/>
    <property name="timeToLiveSeconds" value="3600"/>
    <property name="maxEntriesLocalHeap" value="1000"/>
    <property name="maxEntriesLocalDisk" value="10000000"/>
    <property name="memoryStoreEvictionPolicy" value="LRU"/>
</cache>

<cache  eviction="FIFO 淘汰策略"  flushInterval="60000 每隔60s刷新"  size="512 缓存对象个数" readOnly="true 返回的缓存对象"/>    

```

####  /mapper/parameterMap 入参

####  /mapper/resultMap

##### ResultMap.ResultMapping
- ResultMapping 单个元素 <-- MapperBuilderAssistant.buildResultMapping <-- ResultMapping.Builder
- 对象：
    - ResultMapping
      - configuration
      - property：java字段
      - column：大写的数据库字段
      - javaType
      - jdbcType
      - typeHandler
      - nestedResultMapId
      - nestedQueryId
      - notNullColumns
      - columnPrefix
      - flags : ResultFlag.ID | CONSTRUCTOR
      - composites：
      - resultSet
      - foreignColumn
      - lazy: ??? 如何实现


- 配置参考：
```
<mapper namespace="xyz.coolblog.dao.ArticleDao">
    <resultMap id="articleResult" type="xyz.coolblog.model.Article">
        <constructor>
            <idArg column="id" name="id"/>
            <arg column="title" name="title"/>
            <arg column="content" name="content"/>
        </constructor>
        <id property="id" column="id"/>
        <result property="author" column="author"/>
        <result property="createTime" column="create_time"/>
    </resultMap>
</mapper>
```



##### ResultMap: <-- ResultMapResolver.resolve  <-- MapperBuilderAssistant.addResultMap <-- ResultMap.Builder.build

- bean的成员变量
    - id
    - type
    - mappedColumns：存储 <id>、<result>、<idArg>、<arg> 节点 。所有的数据库字段类型，大写
    - mappedProperties：于存储 <id> 和 <result> 节点的 property 属性 或 <idArgs> 和 <arg> 节点的 name 属性。java类型字段，包含构造函数字段
    - constructorResultMappings：用于存储 <idArgs> 和 <arg> 节点，构造函数字段
    - propertyResultMappings：用于存储 <id> 和 <result> 节点，java类型字段，和mappedProperties 区别，不包含构造函数字段
    - idResultMappings：用于存储 <id> 和 <idArg> 节点。如果开发没指定，自动把所有的字段设置存入
    - extend  <-- configuration.getResultMap(extend).getResultMappings()
    - discriminator
    - resultMappings：ResultMapping
    - autoMapping ：

#### /mapper/sql sqlElement
- 存放在 configuration.sqlFragments
- databaseIdMatchesCurrent(id, databaseId, requiredDatabaseId) 判断 daabaseId 是否相同，来决定是否加载

#### select|insert|update|delete

##### 构建：MappedStatement
- 存放在：configuration.mappedStatements
- 解析过程：
    - XMLStatementBuilder.parseStatementNode
    - 解析 include：XMLIncludeTransformer.applyIncludes()
    - parseSelectKeyNode：主键的处理，
        - SqlSource
        - 创建selectKey的 MapperBuilderAssistant.addMappedStatement

    - MapperBuilderAssistant.addMappedStatement
    - MappedStatement.Builder.build

- MappedStatement:
    - resource：xml文件地址
    - id：nameSpace + statementId
    - fetchSize：
    - timeout
    - statementType：默认 PREPARED，org.apache.ibatis.mapping.StatementType
    - resultSetType：
    - sqlSource
        - StaticSqlSource，DynamicSqlSource
    - cache
    - parameterMap
    - resultMaps：返回的对象
    - flushCacheRequired：select 默认false，其他true  和 useCache刚好相反
    - useCache：select 默认true，其他：false
    - resultOrdered：默认false
    - sqlCommandType
    - keyGenerator
    - keyProperties
    - keyColumns
    - hasNestedResultMaps
    - databaseId
    - statementLog
    - lang
    - resultSets

##### ParameterMap  入参
- 字段：
    - nameSpace + statementId + -Inline
    - parameterTypeClass
    - parameterMappings  不知道是啥？

##### 解析：include节点
- XMLIncludeTransformer.applyIncludes(解析 include)
- 递归调用：
    - 如果是普通节点
        - 递归子节点
    - 如果是include 节点，有点绕，递归了
        - 通过refId 找到引用的节点toIncludeSql
        - 把：toIncludeSql 的内容 替换占位符 成  sql文本
        - 讲：toIncludeSql 代码替换到 <include> 节点
        - 把：toIncludeSql 内的文本节点，insertBefore toIncludeSql
        - 最后把 toIncludeSql 删除
    - 如果是文本节点
        - 替换占位符

- xml 类型：
|子节点                                               |类型                                        |描述            |
|-----------------------------------------|:---------------------------------:|-------------:|
|SELECT                                             | 3     Node.TEXT_NODE         | 文本节点    |
|<include refid=“TableName”/>          | 1     Node.ELEMENT_NODE | 普通节点   |
| WHERE id = #{id}                             | 3     Node.TEXT_NODE         | 文本节点   |



##### MappedStatement.SqlSource 解析SQL得到SqlSource
- 入口：XMLScriptBuilder.parseScriptNode
- 过程：
    - parseDynamicTags 解析 SQL 语句节点
        - 判断是否包含 ${}  基于：DynamicCheckerTokenParser.isDynamic <----  GenericTokenParser 判断
            - 是：TextSqlNode  
            - 否：StaticTextSqlNode
        - 判断是否是动态表达式标签：
            - 是：重新构建该表达式下面的语法树

- LanguageDriver:
    - RawLanguageDriver
    - XMLLanguageDriver

- SqlSource
    - 关键字段：
        - SqlNode rootSqlNode
    - 实现类：
        - RawSqlSource 静态sql
        - DynamicSqlSource 动态sql

- SqlNode：
    - MixedSqlNode 混合的sql
        - StaticTextSqlNode 静态sql | #{id}片段
    - TextSqlNode：${tableName} 代码片段
    - ChooseSqlNode    <---->   
    - ForEachSqlNode   <---->  
    - IfSqlNode   <---->  
    -TrimSqlNode  <----> TrimHandler
        - SetSqlNode   <---->   
        - WhereSqlNode  <---->  WhereHandler
    - VarDeclSqlNode

- NodeHandler:
    - TrimHandler
    - WhereHandler
    - SetHandler
    - ForEachHandler
    - IfHandler
    - ChooseHandler
    - IfHandler
    - OtherwiseHandler
    - BindHandler

##### 解析 selectKey节点，解决insert into 主键问题
- 放在：configuration.keyGenerators  key:insertSysMenu!selectKey
- 过程：
    - 创建 SqlSource 实例
    - 构建并缓存 MappedStatement 实例，他也是个select。id: nameSpace+selectId+!selectKey
    - 构建并缓存 SelectKeyGenerator 实例
- SelectKeyGenerator
    - executeBefore：默认after
    - keyStatement：MappedStatement

- 实现类：
    - SelectKeyGenerator：显示的在xml配置 获取主键的id
    - NoKeyGenerator：
    - Jdbc3KeyGenerator：

- 方法：
    - processBefore
    - processAfter

```
<sql id="TableName">
        ${table_name}
</sql>

 <select id="selectSysMenuAll" resultMap="SysMenuResult">
        select
         <include refid="TableName">
             <property name="table_name" value="sys_menu"/>
        </include>
        from
        <include refid="TableName" />
        order by level,order_num
    </select>

 <insert id="insertSysMenu" parameterType="package.auth.po.SysMenu">

         <selectKey resultType="java.lang.Integer" keyProperty="id">  insertSysMenu!selectKey
            CALL IDENTITY()
        </selectKey>


        insert into
        <include refid="TableName" />
        (
            id,
            name,
            gmt_modified
        )values(
            #{id},
            #{name},
            #{gmtModified}
        )

    </insert>
```



##### mapper接口绑定
- 存放在：
    - configuration.mapperRegistry    
    - configuration.mapperRegistry.knownMappers
- 入口：XMLMapperBuilder.bindMapperForNamespace
- 职责：
    - 绑定mapper 和 proxy
    - 基于注解的解析，也会重新去解析xml（但不会真执行）

```

基本工具类：
 获取反射类型：

Configuration：
 isResourceLoaded: 是否加载了该类型
 cacheRefMap: namespace --> targetNameSpace（到caches 中找）,缓存信息是有互相关联的
 caches：构建好的缓存类
 incompleteCacheRefs: 不完整的cache 关联 等待补充？？？
 variables：
 sqlFragments:sql 片段，key=nameSpace + id
 resultMaps: ResultMap

TypeHandler: 类型处理器,对复杂对象的转换，如：枚举
 可配置全局 或 单个mapper中
 常用方法：
  setParameter
  getResult
  getResult
  getResult

 BaseTypeHandler
 setNonNullParameter
 getNullableResult(ResultSet)
 getNullableResult(CallableStatement)

TypeAliases：类型的别名 ,类型 和 TypeHandler的绑定关系
 Map<String, Class<?>>
 默认有：常用类型的别名

XMLMapperBuilder -> BaseBuilder (Configuration,TypeAliasRegistry,TypeHandlerRegistry)
  XPathParser
  sqlFragments : sql 片段
  resource
  MapperBuilderAssistant : 助手
   currentNamespace
   resource
   currentCache
   unresolvedCacheRef
  起到了 和 configuration 的交互

parameterMap：<-- ParameterMap.Builder
 <parameterMap type="Book.dao.Book po对象" id="BookParameterMap 唯一号">
   <parameter property="bookName" resultMap="BookResultMap" />  
   <parameter property="bookPrice" resultMap="BookResultMap" />  
 </parameterMap>
 id
 type
 parameterMappings 单个元素 <-- ParameterMapping.Builder


Statement： <-- XMLStatementBuilder.parseStatementNode
 <select id="selectSysMenuById" parameterType="Long" resultMap="SysMenuResult">
        select
        <include refid="TableField" />
        from
        <include refid="TableName" />
        where id = #{id}
    </select>
    parseStatementNode 方法：
    include 代码的解析: XMLIncludeTransformer
    selectKey:
     MapperBuilderAssistant.addMappedStatement

source.getNodeName() + ":" + source.getNodeType() + ":" + source.getTextContent()

for (int i = 0; i < children.getLength(); i++) {
    System.out.println(i + " " + children.item(i).getNodeName() + ":" + children.item(i).getNodeType() + ":" children.item(i).getAttributes().getNamedItem("refid") + ":" + children.item(i).getTextContent());   
}


0 #text:3:
        select
1 include:1:
2 #text:3:
        from
3 include:1:

4 #text:3:
        order by level,order_num
        select
        `id`,
        `name`,
        `parent_id`,
        `order_num`,
        `url`,
        `type`,
        `visible`,
        `perms`,
        `icon`,
        `level`,
        `remark`,
        `status`,
        `version`,
        `create_by`,
        `gmt_create`,
        `modified_by`,
        `gmt_modified`
        from
        order by level,order_num

xml解析：XPathParser

mapper 解析：
 XMLMapperBuilder

```

- 从 mybatis-config.xml 加载

```
<mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/> <!-- 使用相对于类路径的资源引用 -->
    <mapper url="file:///var/mappers/AuthorMapper.xml"/> <!-- 使用完全限定资源定位符（URL） -->
     <mapper class="org.mybatis.builder.BlogMapper"/> <!-- 使用映射器接口实现类的完全限定类名 -->
    <package name="org.mybatis.builder"/>  <!-- 将包内的映射器接口实现全部注册为映射器 -->
</mappers>

```

- 方式：
    - resource：资源文件加载映射文件,
    - url：URL的方式加载和解析
    - class：mapper 接口加载映射信息,映射信息可以配置在注解中
    - package：通过包扫描的方式获取到某个包下的所有类，并且使用第三种方式为每个类解析映射信息

### Mapper xml 文件：

```

<mapper namespace="org.apache.ibatis.submitted.rounding.Mapper">

    <cache eviction="FIFO" flushInterval="60000"  size="512"  readOnly="true"/>

    <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
        <property name="timeToIdleSeconds" value="3600"/>
        <property name="timeToLiveSeconds" value="3600"/>
        <property name="maxEntriesLocalHeap" value="1000"/>
        <property name="maxEntriesLocalDisk" value="10000000"/>
        <property name="memoryStoreEvictionPolicy" value="LRU"/>
    </cache>

    <!-- Mapper1 与 Mapper2 共用一个二级缓存 -->
    <cache-ref namespace="xyz.coolblog.dao.Mapper2"/>



    <resultMap type="org.apache.ibatis.submitted.rounding.User" id="usermap">

        <constructor>
            <idArg column="id" name="id"/>
            <arg column="title" name="title"/>
            <arg column="content" name="content"/>

            <association property="article_author" column="article_author_id" javaType="Author" resultMap="authorResult"/>
        </constructor>


        <id column="id" property="id"/>
        <result column="name" property="name"/>

        <!-- 引用 authorResult -->
        <association property="article_author" column="article_author_id" javaType="Author" resultMap="authorResult"/>

        <!-- resultMap 嵌套 -->
        <association property="article_author" javaType="Author">
            <id property="id" column="author_id"/>
            <result property="name" column="author_name"/>
        </association>

    </resultMap>



     <sql id="TableName">
        sys_menu
    </sql>



    <select id="getUser" resultMap="usermap">
        select * from <include refid="TableField" />
    </select>
    <insert id="insert">
        insert into users (id, name, funkyNumber, roundingMode) values (
            #{id}, #{name}, #{funkyNumber}, #{roundingMode}
        )
    </insert>

    <sql id="TableName">
        `ant_taskflow`
    </sql>
    <resultMap type="org.apache.ibatis.submitted.rounding.User" id="usermap2">
        <id column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="funkyNumber" property="funkyNumber"/>
        <result column="roundingMode" property="roundingMode" typeHandler="org.apache.ibatis.type.EnumTypeHandler"/>
    </resultMap>
    <select id="getUser2" resultMap="usermap2">
        select * from users2
    </select>
    <insert id="insert2">
        insert into users2 (id, name, funkyNumber, roundingMode) values (
            #{id}, #{name}, #{funkyNumber}, #{roundingMode, typeHandler=org.apache.ibatis.type.EnumTypeHandler}
        )
    </insert>
</mapper>

```

##### 核心类：


##### parameterMap – 已废弃！老式风格的参数映射。内联参数是首选,这个元素可能在将来被移除，这里不会记录
##### sql – 可被其他语句引用的可重用语句块。
- DynamicSqlSource：静SQL
    - 含有${}或是其他动态的标签：比如，if，trim，foreach，choose，bind节点等

- RawSqlSource StaticSqlSource：静态SQL


##### insert – 映射插入语句
##### update – 映射更新语句
##### delete – 映射删除语句
##### select – 映射查询语句
