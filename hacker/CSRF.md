# 伪造请求 CSRF（Cross-site request forgery）

## 原理：
- 涉及的接口入参都是公开，可观察
- hacker 伪造请求生成短连给 用户，用户直接点击，造成风险


## 修复：
### 验证 Http Referer
- 缺点：
  - IE6 可以js修改Referer

### 请求增加token信息
- html form表单，通过公用js 脚本，自动增加 toke 表单参数
- a 标签，通过公用js脚本，自动增加 toke参数
- 对于ajax
