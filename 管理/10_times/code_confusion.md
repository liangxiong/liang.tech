# 你的代码是怎么变混乱
- 解决从：把函数写短 开始
- [极客时间原文](https://time.geekbang.org/column/article/87845)

## what
- 代码是如何腐化 如：新的需求是在原有逻辑上添加一个新功能：
  - 往往是：在函数中添加一段新的if分支逻辑，提交完工 （产生了坏味道）
  - 开发解释：
    - 这段代码都这样了，我不敢乱改
    - 之前就是这么写的，我只是遵循别人的风格
  - 这是低头完成一项任务，而代码就变得更糟糕了

- 代码段子：
  - 对程序员来说：爱也代码，抱怨也代码
  - 最大的惩罚是：让他自己维护3个月前的代码
  - 惩罚措施：是维护一段有年龄的代码

## why
- 线性思考习惯，新的需求代码，只看到在这个函数内部会使用

## how
### 怎么破局：
- 不断的去思考如何 编写短代码

- 重构
  - 重构前先了解下 软件设计的原则
  - 怎么重构 [待整理中]

### 软件设计的原则：SOLID 原则 [详解打开](https://github.com/liangxiong/liang.tech/blob/master/career/24_design_pattern/index.md)
- 分别：
  - 单一职责原则 Single responsibility principle, SRP
  - 开放封闭原则 Open closed principle, OCP
  - Liskov 替换原则 Liskov substitution principle, Lsp
  - 接口隔离原则 Interface segregation principle, ISP
  - 依赖倒置原则 Dependency inversion principle，Dip

- 作用：
  - 设计原则是 “道”
  - 设计模式是 ”术”
  - 当你在 “术” 上下过功夫，才能知道 “道” 的价值
  - 当你不理解 “道”，只能死记硬背 “术”
  - “道” 可以帮你建立更完整的只是体系，不必在 “术” 的低层次上不断徘徊

### 演习
- 单一职责原则
  - 根据 Robert Marion，《架构整洁之道》Clean Architecture。一个类就应该仅对一类actor负责
    - actor：对系统有共同需求的人，调用方
  - 你能看到一个模块为多少 actor 服务，取决于你的分解能力

- 方法，类，服务接口
  - 写一个方法时：问问这些代码是不是在一个层面上
  - 写一个类：问问这些方法是不是为一类actor服务
  - 写一个服务：问问从业务角度看提供是否一类的服务

- 你看到的粒度越细，越能发现问题
