# 配置中心
地址： https://gitee.com/xuxueli0323/xxl-conf.git
todo: https://github.com/ctripcorp/apollo


### 需求：
- 配置项能持久存储
- 审计：
    - 查看工程中的配置的变更记录

- 提供接口：
    - 获取配置接口
    - 更新通知接口

### 客户端：
- 能在本地缓存功能，防止配置服务器失效
- 配置设置
    - spring 注解设置参数的值
    - spring xml 的设置参数的值

- 更新本地配置
    - 更新 spring 注解的值
    - 更新 spring  xml

### 技术设计：
##### 核心模型：
- 工程 project
    - name：名称
- 环境 env
    - name：名称
- 配置 conf_node
    - project_id：工程id
    - env_id：环境id
    - key：
    - value：值

- 配置变更日志 conf_node_log 作为审计用
    - conf_node_id：配置id
    - key：
    - value：值

### 设计：
- 整体时序
![时序图](https://github.com/liangxiong/liang.tech/blob/master/开源项目/协调/xxl-conf/res/xxl-conf_sequeue.jpg)

```sequence
客户端->服务端: 首次获取数据
服务端->客户端: 给
客户端->客户端: 我缓存
客户端->服务端: 我listener
服务端->服务端: 有变化我告诉你
服务端->客户端: 有变化了
客户端->客户端: 开发哥哥你带我入坑了
客户端->客户端: spring 的配置我怎么更新
客户端->客户端: 数据库的连接我怎么更新
```



- 关键流程图:
![流程图](https://github.com/liangxiong/liang.tech/blob/master/开源项目/协调/xxl-conf/res/xxl-conf_process.jpg)


```flow
st=>start: 开始
e=>end: 结束
app_init=>operation: 系统初始化
config_load=>operation: 配置加载
config_cache_load_cond=>condition: 本地是否有缓存？
find_cache_config=>inputoutput: 获取缓存配置
find_remote_config=>inputoutput: 加载远程缓存
st->app_init->config_load
config_load->config_cache_load_cond
config_cache_load_cond(yes)->find_cache_config->e
config_cache_load_cond(no)->find_remote_config

```



### 服务端：
- 主存储：
    - mysql

- 缓存：
    - 本地文件  properties
    - 他的实现：一个key 一个配置文件，粒度太细，文件太多
    - 我认为他的粒度太细了，应该基于项目+环境维度

### 客户端
- 配置：调用http接口获取，并加入本地缓存
- 镜像 mirror
    - 基于延迟返回 spring mvc 的  DeferredResult 能够及时在配置修改后，实时返回
    - 客户端 通过listener 观察者模式，解耦：配置变化导致的 业务变化。

- spring  
    - 注解的实现
    - xml的配置实现
    - 解决：利用继承 InstantiationAwareBeanPostProcessorAdapter，可在bean 的生命周期内回调，集成业务代码
        - 注解：postProcessAfterInstantiation  初始化后，如果有自定义的注解，应该解析，重新设置配置中心的值
            - 并且监听该值是否变化，如果变化，重新设置，并且和该值相关的类，如：数据库可能要重新初始化
        - xml配置：postProcessPropertyValues
            - 和上面类似
    - 代码参考：
```
//注解的配置设置实现
@Override
    public boolean postProcessAfterInstantiation(final Object bean, final String beanName) throws BeansException {
        // 1、Annotation('@XxlConf')：resolves conf + watch
        if (!beanName.equals(this.beanName)) {
            ReflectionUtils.doWithFields(bean.getClass(), new ReflectionUtils.FieldCallback() {
                @Override
                public void doWith(Field field) throws IllegalArgumentException, IllegalAccessException {
                    if (field.isAnnotationPresent(XxlConf.class)) {
                        String propertyName = field.getName();
                        XxlConf xxlConf = field.getAnnotation(XxlConf.class);
                        String confKey = xxlConf.value();
                        String confValue = XxlConfClient.get(confKey, xxlConf.defaultValue());
                        // resolves placeholders
                        BeanRefreshXxlConfListener.BeanField beanField = new BeanRefreshXxlConfListener.BeanField(beanName, propertyName);
                        refreshBeanField(beanField, confValue, bean);
                        // watch
                        if (xxlConf.callback()) {
                            BeanRefreshXxlConfListener.addBeanField(confKey, beanField);
                        }
                    }
                }
            });
        }
        return super.postProcessAfterInstantiation(bean, beanName);
    }

//xml配置设置实现
@Override
    public PropertyValues postProcessPropertyValues(PropertyValues pvs, PropertyDescriptor[] pds, Object bean, String beanName) throws BeansException {
        // 2、XML('$XxlConf{...}')：resolves placeholders + watch
        if (!beanName.equals(this.beanName)) {
            PropertyValue[] pvArray = pvs.getPropertyValues();
            for (PropertyValue pv : pvArray) {
                if (pv.getValue() instanceof TypedStringValue) {
                    String propertyName = pv.getName();
                    String typeStringVal = ((TypedStringValue) pv.getValue()).getValue();
                    if (xmlKeyValid(typeStringVal)) {
                        // object + property
                        String confKey = xmlKeyParse(typeStringVal);
                        String confValue = XxlConfClient.get(confKey, "");
                        // resolves placeholders
                        BeanRefreshXxlConfListener.BeanField beanField = new BeanRefreshXxlConfListener.BeanField(beanName, propertyName);
                        //refreshBeanField(beanField, confValue, bean);
                        Class propClass = String.class;
                        for (PropertyDescriptor item: pds) {
                            if (beanField.getProperty().equals(item.getName())) {
                                propClass = item.getPropertyType();
                            }
                        }
                        Object valueObj = FieldReflectionUtil.parseValue(propClass, confValue);
                        pv.setConvertedValue(valueObj);
                        // watch
                        BeanRefreshXxlConfListener.addBeanField(confKey, beanField);
                    }
                }
            }
        }
        return super.postProcessPropertyValues(pvs, pds, bean, beanName);
    }
```
