# 方法引用

## 什么是方法引用
- 通过方法的名字来指向一个方法
- 使用一对冒号 ::
- 好处：可以使语言的构造更紧凑简洁，减少冗余代码


## 使用
- 静态方法来引用 System.out::println
### 构造器引用
- 语法：Class::new

### 静态方法引用
- 语法：Class::static_method

### 特定类的任意对象的方法引用
- 语法：Class::method

### 特定对象的方法引用
- 语法：instance::method
