# Lombok

## 使用

```
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.20</version>
</dependency>
```

## idea 先设置：
- 安装lombok 插件 : preferences -> plugins -> lombok
- 勾选 enable annotation procesing "Settings > Build > Compiler > Annotation Processors"  

## 功能

- @EqualsAndHashCode：实现equals()方法和hashCode()方法 @ToString：实现toString()方法
-@Data ：注解在类上；提供类所有属性的 getting 和 setting 方法，此外还提供了equals、canEqual、hashCode、toString 方法

- @Setter：注解在属性上；为属性提供 setting 方法
- @Getter：注解在属性上；为属性提供 getting 方法
- @Log4j ：注解在类上；为类提供一个 属性名为log 的 log4j 日志对象
- @NoArgsConstructor：注解在类上；为类提供一个无参的构造方法
- @AllArgsConstructor：注解在类上；为类提供一个全参的构造方法
- @Cleanup：关闭流
- @Synchronized：对象同步 @SneakyThrows：抛出异常
- @NonNull: 避免空指针

## 原理
- 1)javac对源代码进行分析，生成一棵抽象语法树(AST)
- 2)运行过程中调用实现了"JSR 269 API"的 lombok 程序
- 3)此时lombok 程序就第一步骤得到的抽象语法树(AST)处理，生产get，set方法
- 4)javac使用修改后的抽象语法树(AST)生成字节码文件





## 优缺点
- 优点：
    - 减少开发者的工作，get，set代码减少

- 缺少
    - 降低了源代码文件的可读性和完整性，降低了阅读源代码的舒适度
