# 嵌入式JavaScript引擎
## 什么
- Nashorn取代Rhino(JDK 1.6, JDK1.7)，带来2到10倍的性能提升
- 支持ECMAScript 5.1规范以及一些扩展
- 基于JSR 292的新语言特性
- 包含在JDK 7中引入的 invokedynamic，将JavaScript编译成Java字节码


## 使用
- jjs sample.js
- Java 中调用 JavaScript

```
ScriptEngineManager scriptEngineManager = new ScriptEngineManager();
ScriptEngine nashorn = scriptEngineManager.getEngineByName("nashorn");
String name = "Runoob";
nashorn.eval("print('" + name + "')");
Integer result = (Integer) nashorn.eval("10 + 2");
System.out.println(result.toString());

```

- JavaScript 中调用 Java

```
var BigDecimal = Java.type('java.math.BigDecimal');
function calculate(amount, percentage) {
   var result = new BigDecimal(amount).multiply(
   new BigDecimal(percentage)).divide(new BigDecimal("100"), 2, BigDecimal.ROUND_HALF_EVEN);
   return result.toPlainString();
}
var result = calculate(568000000000000000023,13.9);
print(result);
```
