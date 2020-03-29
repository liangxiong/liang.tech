# 软件设计原则

## 二十四中设计模式
- [创建类设计模式](https://github.com/liangxiong/liang.tech/blob/master/career/24_design_pattern/create.md)
- [结构类设计模式](https://github.com/liangxiong/liang.tech/blob/master/career/24_design_pattern/structure.md)
- [行为类设计模式](https://github.com/liangxiong/liang.tech/blob/master/career/24_design_pattern/action.md)


## 设计原则：
- 单一职责 Single Responsibility Principle SRP：我单纯 我快乐  针对的是单个类
  - 优点：
    - 降低类的复杂性，提高可读性 可维护性
    - 降低变更的风险

- 接口隔离原则 Interface Segregation Principle：接口的纯洁性
  - 调用者（或实现者）无需被迫依赖它用不到的方法
  - 接口分：
      - 实例接口：new po对象，就是现实世界中的描述
      - 类接口（java语言中的接口）
  - 隔离：
      - 客户端不应该依赖它不需要的接口，别去写胖接口
      - 类间的依赖关系应该建立在最小的接口上
  - 怎么实践：经验，领悟
  - 和 单一职责的区别呢：
      - 单一职责：针对的是单个类
      - 接口隔离：针对类之间的依赖耦合关系

- 迪米特法则 Law of Demeter | 最少知识原则 Least Knowledge Principle：
  - 一个对象应当对其他对象有尽可能少的了解，只和你的朋友谈话
  - 尽量减少public 方法，类间解耦，弱耦合
    -  通过方法的参数传递的对象
    - 当前实例变量引用的对象
    - 方法内创建或实例化的对象

- 依赖倒转原则 Dependence Inversion Principle：依赖抽象，面向接口编程（OOD Object Oriented Design）
  - 高层模块 不应该 依赖低层模块，两者都应该依赖其抽象
  - 抽象不应该依赖细节，细节应该依赖抽象
  - 正置：类的依赖是实实在在的实现类的依赖
  - 优点
      - 由于面向接口编程，降低并行开发的风险

- 开闭原则 Open Closed Princciple
  - 对扩展开放，对修改关闭

- 里氏代换原则 Liskov Substitution Principle ：父类出现的地方，子类都能替换

- 优先使用 组合而非继承
    - 继承是静态，子类都要有父类的功能
    - 组合可以运行是改变，动态
- 其他：
    - 一个类需要的数据，应该隐藏在类的内部
    - 类之间应该玲耦合，或者只有传导耦合
    - 在水平方向上尽可能统一地分布系统功能
    - 可阅读性
    - 公用性
