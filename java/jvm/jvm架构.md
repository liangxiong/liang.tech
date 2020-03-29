# jvm 架构
![jvm架构](https://github.com/liangxiong/liang.tech/blob/master/java/res/jvm.png)

### 类加载器子系统
- 加载
    - Boot Strap类加载器
        负责从引导类路径加载类，除了rt.jar，它具有最高优先级

    - Extension类加载器
        负责加载ext文件夹（jre \ lib）中的类

    - Application类加载器
        负责加载应用程序级类路径，环境变量中指定的路径等信

- 链接
    - 加载 （Verify）
        字节码验证器将验证生成的字节码是否正确，如果验证失败，将提示验证错误；
    - 准备 （Prepare）
        对于所有静态变量，内存将会以默认值进行分配；
    - 解释  （Resolve）
        有符号存储器引用都将替换为来自方法区（Method Area）的原始引用。

- 初始化
    - 这是类加载的最后阶段，所有的静态变量都将被赋予原始值，并且静态区块将被执行。

### 运行时数据区



### 执行引擎
