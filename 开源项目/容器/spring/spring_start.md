# spring start


## 自定义Starter

@ConfigurationProperties(prefix = "hello")
//定义为配置类
@Configuration
//在web条件下成立
@ConditionalOnWebApplication
//启用HelloWorldProperties配置功能，并加入到IOC容器中
@EnableConfigurationProperties({HelloWorldProperties.class})
//导入HelloWorldService组件
@Import(HelloWorldServiceImpl.class)


## @EnableAutoConfiguration

- 是使用SpringFactoriesLoader.loadFactoryNames 扫码各自的starter jar下的：META-INF/spring.factories
- 配置 如：
```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  io.liang.boot.HelloWorldAutoConfiguration
```
## yaml 或 properties 自动提示：配置元数据

- META-INF / spring-configuration-metadata.json

### groups  配置类信息

- name 简称



- type 配置类的类名 String

    - @ConfigurationProperties注解的类

- description 简短的group描述 String



- sourceType String

    - 就是以上的属性，最终要配置的属性类

- sourceMethod  String

    - 以上的属性类，要调用什么方法来获取



### properties 配置类的属性

- name 属性的名称 String  必须要有

    - 为小写虚线分割的形式（比如server.servlet-path）

- type 数据类型的类名 String

    - 如java.lang.String

    - 使用包装类

- description 简短描述  String  

    - 展示给用户的描述

- sourceType 配置类的全名 String

    - @ConfigurationProperties注解的类

- defaultValue 没有设置时的默认值 String

- 过期有关的属性：

    - deprecated  是否过期 Deprecated

    - level  弃用级别 String

        - 警告：仍然应该在环境中绑定

        - 错误：该属性不再受管理，也不受约束

    - reason 弃用的原因的简短描述 String

    - replacement  替换这个废弃属性的属性的全名  String



### hints  默认属性值和提示

- value Object

    - 默认的属性值

- description String

    -  默认属性值的提示



```

{
  "hints": [],
  "groups": [
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis",
      "type": "org.mybatis.spring.boot.autoconfigure.MybatisProperties"
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis.configuration",
      "sourceMethod": "getConfiguration()",
      "type": "org.apache.ibatis.session.Configuration"
    }
  ],
  "properties": [
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "defaultValue": false,
      "name": "mybatis.check-config-location",
      "description": "Indicates whether perform presence check of the MyBatis xml config file.",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "deprecated": true,
      "name": "mybatis.config",
      "type": "java.lang.String",
      "deprecation": {}
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis.config-location",
      "description": "Location of MyBatis xml config file.",
      "type": "java.lang.String"
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis.configuration-properties",
      "description": "Externalized properties for MyBatis configuration.",
      "type": "java.util.Properties"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.aggressive-lazy-loading",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.auto-mapping-behavior",
      "type": "org.apache.ibatis.session.AutoMappingBehavior"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.auto-mapping-unknown-column-behavior",
      "type": "org.apache.ibatis.session.AutoMappingUnknownColumnBehavior"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.cache-enabled",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.cache-names",
      "type": "java.util.Collection<java.lang.String>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.caches",
      "type": "java.util.Collection<org.apache.ibatis.cache.Cache>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.call-setters-on-nulls",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.configuration-factory",
      "type": "java.lang.Class<?>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.database-id",
      "type": "java.lang.String"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "defaultValue": "org.apache.ibatis.type.EnumTypeHandler",
      "name": "mybatis.configuration.default-enum-type-handler",
      "description": "A default TypeHandler class for Enum.",
      "type": "java.lang.Class<? extends org.apache.ibatis.type.TypeHandler>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.default-executor-type",
      "type": "org.apache.ibatis.session.ExecutorType"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.default-fetch-size",
      "type": "java.lang.Integer"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "defaultValue": "org.apache.ibatis.scripting.xmltags.XMLLanguageDriver",
      "name": "mybatis.configuration.default-scripting-language",
      "description": "A default LanguageDriver class.",
      "type": "java.lang.Class<? extends org.apache.ibatis.scripting.LanguageDriver>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.default-statement-timeout",
      "type": "java.lang.Integer"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.environment",
      "type": "org.apache.ibatis.mapping.Environment"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.incomplete-cache-refs",
      "type": "java.util.Collection<org.apache.ibatis.builder.CacheRefResolver>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.incomplete-methods",
      "type": "java.util.Collection<org.apache.ibatis.builder.annotation.MethodResolver>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.incomplete-result-maps",
      "type": "java.util.Collection<org.apache.ibatis.builder.ResultMapResolver>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.incomplete-statements",
      "type": "java.util.Collection<org.apache.ibatis.builder.xml.XMLStatementBuilder>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.interceptors",
      "type": "java.util.List<org.apache.ibatis.plugin.Interceptor>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.jdbc-type-for-null",
      "type": "org.apache.ibatis.type.JdbcType"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.key-generator-names",
      "type": "java.util.Collection<java.lang.String>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.key-generators",
      "type": "java.util.Collection<org.apache.ibatis.executor.keygen.KeyGenerator>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.lazy-load-trigger-methods",
      "type": "java.util.Set<java.lang.String>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.lazy-loading-enabled",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.local-cache-scope",
      "type": "org.apache.ibatis.session.LocalCacheScope"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.log-impl",
      "type": "java.lang.Class<? extends org.apache.ibatis.logging.Log>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.log-prefix",
      "type": "java.lang.String"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.map-underscore-to-camel-case",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.mapped-statement-names",
      "type": "java.util.Collection<java.lang.String>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.mapped-statements",
      "type": "java.util.Collection<org.apache.ibatis.mapping.MappedStatement>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.multiple-result-sets-enabled",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.object-factory",
      "type": "org.apache.ibatis.reflection.factory.ObjectFactory"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.object-wrapper-factory",
      "type": "org.apache.ibatis.reflection.wrapper.ObjectWrapperFactory"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.parameter-map-names",
      "type": "java.util.Collection<java.lang.String>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.parameter-maps",
      "type": "java.util.Collection<org.apache.ibatis.mapping.ParameterMap>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.proxy-factory",
      "type": "org.apache.ibatis.executor.loader.ProxyFactory"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.reflector-factory",
      "type": "org.apache.ibatis.reflection.ReflectorFactory"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.result-map-names",
      "type": "java.util.Collection<java.lang.String>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.result-maps",
      "type": "java.util.Collection<org.apache.ibatis.mapping.ResultMap>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.return-instance-for-empty-row",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.safe-result-handler-enabled",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.safe-row-bounds-enabled",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.sql-fragments",
      "type": "java.util.Map<java.lang.String,org.apache.ibatis.parsing.XNode>"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.use-actual-param-name",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.use-column-label",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.use-generated-keys",
      "type": "java.lang.Boolean"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.variables",
      "type": "java.util.Properties"
    },
    {
      "sourceType": "org.apache.ibatis.session.Configuration",
      "name": "mybatis.configuration.vfs-impl",
      "type": "java.lang.Class<? extends org.apache.ibatis.io.VFS>"
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis.executor-type",
      "description": "Execution mode for {@link org.mybatis.spring.SqlSessionTemplate}.",
      "type": "org.apache.ibatis.session.ExecutorType"
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis.mapper-locations",
      "description": "Locations of MyBatis mapper files.",
      "type": "java.lang.String[]"
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis.type-aliases-package",
      "description": "Packages to search type aliases. (Package delimiters are \",; \\t\\n\")",
      "type": "java.lang.String"
    },
    {
      "sourceType": "org.mybatis.spring.boot.autoconfigure.MybatisProperties",
      "name": "mybatis.type-handlers-package",
      "description": "Packages to search for type handlers. (Package delimiters are \",; \\t\\n\")",
      "type": "java.lang.String"
    }
  ]
}

```
