# Starter

https://mp.weixin.qq.com/s/hBFWvC3MLEOZGwu_m5HEiw

## 约定大于配置
- maven 的目录结构，默认有 resources 文件夹存放配置文件，默认打包方式为 jar
- spring-boot-starter-web 中默认包含 spring mvc 相关依赖以及内置的 tomcat 容器，使得构建一个 web 应用更加简单
- 默认提供 application.properties/yml 文件
- 默认通过 spring.profiles.active 属性来决定运行环境时读取的配置文件
- EnableAutoConfiguration 默认对于依赖的 starter 进行自动装配


## 注解：
- @Configuration
  - Spring3 开始支持 2种bean的配置方式：
  - 基于 xml 文件方式
  - 基于 JavaConfig 的 类名上增加：@Configuration，方法上增加：@Bean

- @ComponentScan 扫包
  - 相当于：xml 配置文件中的 context:component-scan
  - 作用：扫描指定路径下的标识了需要装配的类，自 动装配到 Spring 的 IoC 容器中
  - 装配的类的形式：底层注解类：@Component
    - @Repository
    - @Service
    - @Controller

- @EnableAutoConfiguration spring boot 的灵魂
  - @Enable 开头的注解，在 JavaConfig 进一步的完善，避免配置大量的代码从而降低使用的难度
  - @EnableWebMvc @EnableScheduling @EnableAsync

- @Import XML 形式下的 <import resource/>
  - 普通 Bean 或者带有 @Configuration 的配置文件
  - 实现 ImportSelector 接口进行动态注入: 动态注入 Bean 的技术
    - getAutoConfigurationEntry removeDuplicates、remove、filter 等方法
    - getCandidateConfigurations
    - SpringFactoriesLoader.loadFactoryNames 点进去，发现这里有个变量 FACTORIES_RESOURCE_LOCATION
  - 实现 ImportBeanDefinitionRegistrar 接口进行动态注入

- 条件注解类：
  - @ConditionalOnBean	          在存在某个 bean 的时候
  - @ConditionalOnMissingBean	    不存在某个 bean 的时候
  - @ConditionalOnClass	          当前 classpath 可以找到某个类型的类时
  - @ConditionalOnMissingClass	  当前 classpath 不可以找到某个类型的类时
  - @ConditionalOnResource	      当前 classpath 是否存在某个资源文件
  - @ConditionalOnProperty	      当前 jvm 是否包含某个系统属性为某个值
  - @ConditionalOnWebApplication	当前 spring context 是否是 web 应用程序


## start 命名
- 官方：spring-boot-starter-{name} 如：spring-boot-starter-web
- 非官方： {name}-spring-boot-starter 如：dubbo-spring-boot-starter
