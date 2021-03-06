# Lambda
## what
- 被赋给一个变量的函数 或 代码块，就是一个Lambda表达式
- Lambda表达式：本身就是一个接口的实现，只是只有一个实现
- 函数式接口: 只有一个方法需要被实现的接口类，申明用：@FunctionalInterface

## 语法
- (parameters) -> expression
- (parameters) -> { statements; }

- 代码块分解：
  - (s) -> System.out.println(s)
  - 参数和函数间增加 ->
  - 大致等于
```
aBlock = public void fun(String str){
  System.out.println(str);
}
```

- 特性：
  - 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值
  - 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号
  - 可选的大括号：如果主体包含了一个语句，就不需要使用大括号
  - 可选的返回关键字：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值

## 优点：
- 免去使用匿名方法的麻烦，给予Java简单但是强大的函数化的编程能力。
- 主要用来定义行内执行的方法类型接口


## 怎么用
### 作用域：
- 变量作用域
  - 只能引用标记了 final 的外层局部变量
  - 不能在 lambda 内部修改定义在域外的局部变量，否则会编译错误
  - 不允许声明一个与局部变量同名的参数或者局部变量
