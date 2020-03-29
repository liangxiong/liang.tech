### 反射：
- 获取泛型类型：org.apache.ibatis.type.TypeReference   class.getGenericSuperclass
- 反射 org.apache.ibatis.reflection.Reflector ：获取类的set get方法
    - 成员变量：
        - setMethods  Map<String, Invoker> 用于保存属性名称到 Invoke 的映射。setter 方法会被封装到 MethodInvoker 对象中，Invoke 实现类比较简单，大家自行分析
        - getMethods  Map<String, Invoker> 用于保存属性名称到 Invoke 的映射。同上，getter 方法也会被封装到 MethodInvoker 对象中
        - setTypes  Map<String, Class<?>>  用于保存 setter 对应的属性名与参数类型的映射
        - getTypes    Map<String, Class<?>>  用于保存 getter 对应的属性名与返回值类型的映射
        - writeablePropertyNames    String[]  可写属性名称数组，用于保存 setter 方法对应的属性名称
        - readablePropertyNames  String[]   可读属性名称数组，用于保存 getter 方法对应的属性名称
    - 学习：
        - get方法冲突如何解决：
            - 如果方法返回值类型为 boolean，且方法名以 "is" 开头，
            - 选择子类的方法

- 反射属性分词器：PropertyTokenizer
    - 解决：author.id 的问题

- MetaObject metaCache = SystemMetaObject.forObject(cache);
    - 里面包含setValue，getValue 方法

- 查询类的工具：ResolverUtil.find(parentObj,package) -> VFS.getInstance().list(packagePath);
    - 查询这个包下的继承该类的类

- Jdbc类型 JdbcType
- 缓存淘汰策略：LruCache


### 标记解析器：
- ${} GenericTokenParser
