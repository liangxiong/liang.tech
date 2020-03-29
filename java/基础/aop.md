# aop

- 切点 pintcut
    - 连接点的匹配规则，通过切点定位到连接点
    - spring aop 使用规则引擎负责解析切点所设定的查询条件

- 连接点 joinpoint
    - 原类执行的特殊点，方法

- 目标对象target
    - 要做增强的目标方法

- 增强的功能
    - 增强 Advice enhancer
        - 织入目标类连接点上的程序代码
        - spring 提供的增强接口分：
            - BeforeAdvice
            - AfterReturningAdvice
            - ThrowsAdvice
    - 引入 introduction
        - 特殊增强，为类添加一些属性和方法

- 代理 proxy
    - 最终生产的结果
    - 原类的子类
    - 和原类具有相同接口的类

- 织入 Weaving
    - 将增强添加到目标类的具体连接点上的过程
    - 三种织入方式
        - 编译期：特殊的java 编译器   (AspectJ)
        - 类装载：特殊的类装载器  (AspectJ)
        - 动态代理：运行期为目标类添加增强生成子类的方式  （spring）

- 切面 Aspect
    - 由切点和增强（引入）组成


前置
后置
环绕增强


- jdk 动态代理

```
private Object target;
public DynamicProxy(Object target){
    this.target = target;
}

public <T> T getProxy(){
    return (T)Proxy.newProxyInstance(target.getClass().getClassLoader(),
        target.getClass().getInterfaces(),
        this);
}

@Override
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    before();
    Object result = method.invoke(target,args);
    after();
    return null;
}

public void before(){
    System.out.println("before");
}

public void after(){
    System.out.println("after");
}
```



- cglib 动态代理

```
public<T> T getProxy(Class<T> cls){
        return (T)Enhancer.create(cls,this);
    }
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        before();
        Object result = methodProxy.invokeSuper(o,objects);
        after();
        return result;
    }
    public void before(){
        System.out.println("before");
    }
    public void after(){
        System.out.println("after");
    }

```
