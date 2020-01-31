# 函数接口

## 什么
- 一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口
- 可以被隐式转换为 lambda 表达式
- @FunctionalInterface

## 常用的约定函数接口
- 在：java.util.function

- 总结：
  - 判断：Predicate boolean test(Object)
  - 接受处理：Consumer  accept(Object)
  - 方法引用：Supplier
  - 空指针判断：Optional
    - Optional.ofNullable
    - 存在则开干 Optional.ifPresent(System.out::println)
    - 存在则返回，无则返回屁 Optional.orElse(UNKNOW)
    - 存在则返回，无则由函数产生 Optional.orElse(() -> 函数)
    - 夺命连环null检查

- 明细：

序号 | 名称 |  描述  
-|-|-
1 | BiConsumer<T, U> | 接受两个输入参数，不返回结果 |
2 | BiFunction<T,U,R> | 接受两个输入参数，返回一个结果 |
3 | BinaryOperator<T> | 两个同类型操作符的操作，并且返回了操作符同类型的结果 |
4 | BiPredicate<T,U>  | 两个参数的boolean值方法 |
5 | BooleanSupplier   |  boolean值结果的提供方 |
6 | Consumer<T>       | 接受一个输入参数并且无返回的操作 |
7 | DoubleBinaryOperator | 作用于两个double值操作符的操作，并且返回了一个double值的结果 |
8 | DoubleConsumer    | 一个double值参数，不返回结果 |
9 | DoubleFunction<R> | 一个double值参数的方法，并且返回结果 |
10 | DoublePredicate  |  一个double值参数的boolean返回值方法 |
11 | DoubleSupplier   | double值结构的提供方 |
12 | DoubleToIntFunction | 一个double类型输入，返回一个int类型结果 |
13 | DoubleToLongFunction | 一个double类型输入，返回一个long类型结果  |
14 | DoubleUnaryOperator | 一个参数同为类型double,返回值类型也为double  |
15 | Function | 一个输入参数，返回一个结果 |
16 | IntBinaryOperator | 两个参数同为类型int,返回值类型也为int |
17 | IntConsumer | 一个int类型的输入参数，无返回值 |
18 | IntFunction<R> | 一个int类型输入参数，返回一个结果 |
19 | IntPredicate | 一个int输入参数，返回一个布尔值的结果 |
20 | IntSupplier  | int类型提供方 |
21 | IntToDoubleFunction | 一个int类型输入，返回一个double类型结果  |
22 | IntToLongFunction | 一个int类型输入，返回一个long类型结果 |
23 | IntUnaryOperator | 一个参数同为类型int,返回值类型也为int  |
24 | LongBinaryOperator | 两个参数同为类型long,返回值类型也为long |
25 | LongConsumer | 一个long类型的输入参数，无返回值 |
26 | LongFunction<R> | 一个long类型输入参数，返回一个结果 |
27 | LongPredicate | 一个long输入参数，返回一个布尔值类型结果 |
28 | LongSupplier | long类型提供方 |
29 | LongToDoubleFunction | 一个long类型输入，返回一个double类型结果 |
30 | LongToIntFunction | 一个long类型输入，返回一个int类型结果 |
31 | LongUnaryOperator | 一个参数同为类型long,返回值类型也为long |
32 | ObjDoubleConsumer<T> | 一个object类型和一个double类型的输入参数，无返回值 |
33 | ObjIntConsumer<T> | 一个object类型和一个int类型的输入参数，无返回值 |
34 | ObjLongConsumer<T> | 一个object类型和一个long类型的输入参数，无返回值 |
35 | Predicate<T> | 一个输入参数，返回一个布尔值结果  |
36 | Supplier<T> | 无参数，返回一个结果 |
37 | ToDoubleBiFunction<T,U> | 两个输入参数，返回一个double类型结果 |
38 | ToDoubleFunction<T> | 一个输入参数，返回一个double类型结果 |
39 | ToIntBiFunction<T,U> | 两个输入参数，返回一个int类型结果 |
40 | ToIntFunction<T> | 一个输入参数，返回一个int类型结果 |
41 | ToLongBiFunction<T,U> | 两个输入参数，返回一个long类型结果 |
42 | ToLongFunction<T> | 一个输入参数，返回一个long类型结果 |
43 | UnaryOperator<T> | 一个参数为类型T,返回值类型也为T |
