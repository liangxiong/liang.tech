# 二方库规约

- 【强制】定义使用GAV规则
  - G：group：com.{公司/BU}.业务线.[子业务线] 最多4级
  - A：artifactId：产品线-模块名
  - V：version：起始版本：1.0.0
      - 主版本：当做不兼容的api修改，或增加了能改变产品方向的新功能
      - 次版本：当做了向下兼容的功能性新增
      - 修订号：修复bug，没有修改方法签名的功能增强，保持api兼容性

- 【强制】线上包，不要依赖：SNAPSHOT，使用RELEASE包
  - SNAPSHOT：每次构建都下载包

- 【强制】仲裁：无法仲裁时，用 dependency:resolve，dependency:tree找出差异点 用 <excludes> 排除

- 【强制】依赖二方包的群组，使用同一的版本，如 spring的 context，core，beans

- 【强制】返回类型：不允许出现枚举类 或 包含枚举的pojo

- 【强制】不要出现 多次申明同一个group，article，但不同的version

- 【强制】依赖申明在 <dependencies>，仲裁放在<dependencyManagement>

- 【推荐】不要有配置项

- 【参考】移除不必要的依赖，最后只包含：
  - 精简可控原则：包含的代码有：
    - service api
    - 必要的返回领域
    - 工具类
    - 常量，枚举
    - 不要有具体的日志实现类，只依赖日志框架
  - 稳定可追溯原则：
    - 每个版本：被谁，什么时候，为啥改的
