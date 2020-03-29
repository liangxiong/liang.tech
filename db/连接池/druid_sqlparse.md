# druid

## SqlParse

- 语句解析->表达式解析->词法解析
- 对应的主要类分别是MySqlStatementParser，MySqlExprParser，MySqlLexer

- SQLParser.java
    - errorEndPos :解析出错记录位置
    - lexer：词法解析
    - dbType：数据库类型

SQLStatement.java
    - exprParser：表达式解析类
    - SQLCreateTableParser：建表语句解析类，因为建表语句比较复杂，所以单拿出来。其他DDL语句都在本类SQLStatement中解析
    - parseValuesSize：记录解析结果集大小
    - keepComments：是否保留注释
    - parseCompleteValues：是否全部解析完成

- MySqlStatementParser.java:
    - 静态关键词：比如auto increment，collate等，对于DDL语句或者DCL语句
    - exprParser：针对MySQL语句的parser

- SQLExprParser：
    - AGGREGATE_FUNCTIONS：一些统计函数的关键词
    - aggregateFunctions：保存统计函数的关键词

- MySqlSQLExprParser：
    - AGGREGATE_FUNCTIONS：针对MySql统计函数的关键词

- Lexer： 词法分析器
    - text：保存目前的整个SQL语
    - pos：当前处理位置
    - mark：当前处理词的开始位置
    - ch：当前处理字符
    - buf：当前缓存的处理词
    - bufPos：用于取出词的标记，当前从text中取出的词应该为从mark位置开始，mark+bufPos结束的
    - token：当前位于的关键词
    - stringVal：当前处理词
    - comments：注释
    - skipComment：是否跳过注释
    - savePoint：保存点
    - varIndex：针对？表达式
    - lines：总行数
    - digits：数字ASCII码
    - EOF:是否结尾
    - keepComments：是否保留注释
    - endOfComment：是否注释结尾
    - line：当前处理行数
    - commentHandler：注释处理器
    - allowComment:是否允许注释        
    - KeyWords：所有关键词集合

语法分析的作用是将一个输入的‘字符串’变换为一个描述这个字符串的‘结构体’,让计算机可以更容易的理解用户输入的字符串是什么意义。这个阶段包含三个过程,分别是词法分析、语法分析、输出抽象语法树
