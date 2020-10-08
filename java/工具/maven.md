# maven

## 依赖
- 优先原则
  - 最短路径优先
  - 相同路径下配置在前的优先
    - 在同一pom文件，第二原则不再适用
  - 相同文件内，配置在后为准

- 可选依赖 <optional>true</optional> 依赖不是必须的

- 排除依赖 <exclusions></exclusions>

- 依赖范围
  - compile(默认): 编译范围，编译和打包都会依赖。
  - provided： 提供范围，编译时依赖，但不会打包进去。如：servlet-api.jar
  - runtime： 运行时范围，打包时依赖，编译不会。如：mysql-connector-java.jar
  - test： 测试范围，编译运行测试用例依赖，不会打包进去。如：junit.jar
  - system： 表示由系统中classpath指定。编译时依赖，不会打包进去。配合 <systemPath></systemPath> 使用
    - 放在 项目的 lib 目录下
    - 配合 maven-dependency-plugin 插件配合使用
    - 手动放在本仓库：
```
mvn install:install-file -Dfile=xxx.jar -DgroupId=io.liang -DartifactId=xxx -Dversion=1.0.0-RELEASE -Dpackaging=jar
```

## 项目结构
### 聚合
- 聚合是指将多个模块整合在一起，统一构建，避免一个一个的构建

```
<modules>
  <module>mykit-dao</module>
  <module>mykit-service</module>
</modules>
```

### 继承
- 子工程直接继承父工程 当中的属性、依赖、插件等配置，避免重复配置

- 属性继承
- 依赖继承 <dependencyManagement></dependencyManagement
- 插件继承

### 依赖管理

### 项目属性
- ${basedir} 项目根目录
- ${version} 表示项目版本;
- ${project.basedir} 同${basedir}
- ${project.version} 表示项目版本,与${version}相同
- ${project.build.directory} 构建目录，缺省为target
- ${project.build.sourceEncoding} 表示主源码的编码格式
- ${project.build.sourceDirectory} 表示主源码路径;
- ${project.build.finalName} 表示输出文件名称
- ${project.build.outputDirectory} 构建过程输出目录，缺省为target/classes


## 构建配置
- resources：build过程中涉及的资源文件
- targetPath：资源文件的目标路径
- directory：资源文件的路径，默认位于${basedir}/src/main/resources/目录下
- includes：文件名的匹配模式，被构建过程处理
- excludes：文件名的匹配模式，被匹配的资源文件将被构建过程忽略。同时被includes和excludes匹配的资源文件，将被忽略。
- filtering：默认false ，true 表示 通过参数 对 资源文件中 的${key} 在编译时进行动态变更。
  - 替换源 -Dkey 和pom 中的 值 或 中指定的properties 文件
