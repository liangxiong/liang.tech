#如何进行 code review
# why
- 代码混乱、Bug频出的状况
- 提高产品的代码质量
- 个人在开发上有很多经验，也希望这些知识能够在团队内传播

# how
## 流程与规则
- Git Flow + Pull Request（PR）模式来做Code Review
  - 流程：
    - 我无权往仓库或分支提交代码
    - 需要有权限的人确认：你请求有权限的人把你的分支代码合并到指定的分支

  - 功能：
    - 可以进行 diff
    - 对可疑代码进行评论，可以回复评论

## 开发模式
### 流程和规则
- 分支
  - master：面向生产
  - develop：对应本地测试环境
  - QA：只测试 master，develop

- 合并代码流程：
  - 只有项目leader 和 架构师 操作develop
  - QA 操作 master

- 开发流程：
  - 先把 master -> develop -> feature/v1
  - 各位开发 在 feature/v1 创建 feature/v1-xxx 各自的任务分支，代码量要少于 2个人日
  - 开发完feature/v1-xxx 开始 pull request 到 feature/v1
  - 如果驳回，在 feature/v1-xxx 修改后，重新 pull request 到 feature/v1
  - 其他人在收到 合并通知后 从 feature/v1 合并代码分支

  - 其他：
    - 少于1个人日的功能，直接基于develop分支创建特性分支即可
    - 如果是bug
      - 使用缺陷跟踪管理系统的ID，如bugfix/login-1354
      - 如果没有缺陷跟踪，命名如：bugfix/login_exception

### 执行
- 提倡质量优先，不能为了进度牺牲质量，并在团队内部达成了共识
- 
