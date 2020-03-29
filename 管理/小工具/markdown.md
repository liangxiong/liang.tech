# markdown

- 参考地址 http://www.wiz.cn/feature-markdown.html

- 目录  添加 [TOC]

- 标题：一共6种大小 用 #表示
- 列表
    - 前面带 -  | *
    - 有序列表 前面带数字点号  1.

- 插入图片
![这是一张图片](http://cdn.wiz.cn/wp-content/uploads/2015/06/wiz_logo.png)
- 插入链接：[描述](链接地址)
[为知笔记](http://www.wiz.cn)


- 引用 文字前面带 >

> 如果你无法简洁的表达你的想法，那只说明你还不够了解它。 -- 阿尔伯特·爱因斯坦

- 表格：

| 为知笔记|更新 | 版本 |
|------------|-----------|--------|
| WizNote | Markdown| Latest |



- 代码：

``` java

public void static main(String[] args){

      system.out.println(“zyl”);

}

```

- 脚注：
hello[^1]
[^1]: hi


- 公式：
可以创建行内公式，例如：$\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$
或者块级公式，
$$ x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a} $$

- 字体：
    - 加粗 **加粗**
    - 斜体 *斜体*
    - 删除线 ~~删除线~~

### 流程图

```flow
st=>start: 开始
e=>end: 结束
op1=>operation: 操作
sub1=>subroutine: 子程序
cond=>condition: 判断？
io=>inputoutput: 输入
st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```
- flow:
    - tag=>type: content
    - tag：唯一标识
    - type：6种类型 注意：冒号后面 带一个空格
        - start
        - end
        - operation：操作
        - subroutine：子程序
        - condition：判断，有yes和no2额分支
        - inputoutput：输入
    - content：文本中内容

### 时序图
- title 标题

- participant 对象

- note 说明
    - left of：左侧
    - right of：右侧
    - over：上方

- 箭头：
    - ->：实线实箭头
    - -->：虚线实箭头
    - ->>：实线虚箭头
    - -->>：虚线虚箭头

- 换行 \n


``` sequence
title: 三个臭皮匠的故事
participant 小王
participant 小李
participant 小异常
note left of 小王: 我是小王
note over 小李: 我是小李
note right of 小异常: 大家好！\n我是小异常
小王->小王: 小王想：今天要去见两个好朋友咯~
小王->小李: 嘿，小李好久不见啊~
小李-->>小王: 是啊
小李->小异常: 小异常，你好啊
小异常-->小王: 哈，小王！\n最近身体怎么样了？
小王->>小异常: 还可以吧
```
