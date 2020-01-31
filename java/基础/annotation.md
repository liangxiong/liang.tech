# 注解
## 内置注解
- @Override
  - 检查该方法是否是重载方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。
- @Deprecated
  - 标记过时方法。如果使用该方法，会报编译警告。
- @SuppressWarnings
  - 指示编译器去忽略注解中声明的警告。

## 元注解
- @Retention
  - 标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。
- @Documented
  - 标记这些注解是否包含在用户文档中。
- @Target
  - 标记这个注解应该是哪种 Java 成员。
- @Inherited
  - 标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类)

- @SafeVarargs
  - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。
- @FunctionalInterface
  - Java 8 开始支持，标识一个匿名函数或函数式接口。
- @Repeatable
  - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。

## 明细
### Annotation
- ElementType 什么的位置
  - TYPE 类、接口（包括注释类型）或枚举声明
  - FIELD 字段声明（包括枚举常量）
  - METHOD 方法声明
  - PARAMETER 参数声明
  - CONSTRUCTOR 构造方法声明
  - LOCAL_VARIABLE 局部变量声明
  - ANNOTATION_TYPE 注释类型声明
  - PACKAGE  包声明

- RetentionPolicy  保留策略
  - SOURCE
    - 信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了
  - CLASS
    - 编译器将Annotation存储于类对应的.class文件中。默认行为
  - RUNTIME   
    - 编译器将Annotation存储于class文件中，并且可由JVM读入
