# Date-Time API (JSR 310)

## 老版的问题
- 非线程安全 − java.util.Date
- 设计很差
  - 在java.util和java.sql的包中都有日期类
    - java.util.Date同时包含日期和时间
    - java.sql.Date仅包含日期
    - 类都有相同的名字，设计糟糕
  - 格式化和解析的类在java.text包中定义
- 时区处理麻烦 − 日期类并不提供国际化，为支持时区，引入了java.util.Calendar和java.util.TimeZone类

## 核心类
- LocalTime
- ZonedDateTime
