# BeanPostProcessor

## 作用：
- 扩展接口

## 方法：
- Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException;
  - bean初始化方法调用前被调用

- Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException;
  - bean初始化方法调用后被调用


# BeanFactoryPostProcessor
- 管理我们的bean工厂内所有的beandefinition（未实例化）数据
