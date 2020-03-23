# 安装
- NPM方法
  - npm -v  查看版本
  - cnpm install npm -g  升级 npm
  - npm install cnpm -g  升级或安装 cnpm
  - cnpm install vue  最新稳定版

- 命令行工具
  - cnpm install --global vue-cli    全局安装 vue-cli
  - vue init webpack my-project      创建一个基于 webpack 模板的新项目
  - cnpm install 安装   
  - cnpm run dev 运行
  - npm run serve 运行

# 目录结构
- build
- config
- node_modules
- src
  - assets
  - components
  - App.vue
  - main.js
- static
- test
- index.html
- package.json
- README.md


# 预发

## 条件
```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>

<h1 v-show="ok">Hello!</h1>
```
## 循环
```
<li v-for="site in sites">
 {{ site.name }}
 </li>
```

## 计算属性
- computed vs methods
  - 可以使用 methods 来替代 computed

## 监听
```
vm.$watch('counter', function(new_val, old_val) {
  alert('计数器值的变化 :' + old_val + ' 变为 ' + new_val + '!');
});
```
