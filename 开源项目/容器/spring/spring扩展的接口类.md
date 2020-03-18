# 常用的接口
## BeanDefinitionRegistryPostProcessor 动态添加到spring容器
- postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry)
  - registry.registerBeanDefinition("xxx", bean)

## InitializingBean 进行初始化
- afterPropertiesSet()

## ApplicationContextAware 注入Spring的容器
- setApplicationContext(ApplicationContext context)

## BeanNameAware 注入spring容器内的bean名称
- setBeanName(String name)
