# docker常用命令

## 启动过程：
bootfs -> rootfs
docker run --name demo -d -i ubuntu /bin/bash -c 'while true; do echo hello world `date`; sleep 1; done'

## info/version
- -info: 信息
- -version：版本

## 镜像管理
- images：列出镜像
- rmi：删除镜像
- tag：标记镜像的tag
- build：构建
    - -t : tag
- history：镜像的创建历史脚本
- save：镜像保存成归档文件
    - docker save -o xx.tar image
- import：从归档文件中创建镜像
    - docker importxx.tar image  

## 镜像仓库
- login：登录
    - -u：账户
    - -p：密码
- logout：登出
- pull：下拉
- push：推送
- search：
    - -s ：列出最少的star
    - --no-trunc：显示完整的镜像描述

## 容器rootfs
- commit：进入容器，执行命令，commit
    - -m="备注"
    - -a | --author="作者"
- cp：主机和容器间复制
    - docker cp /www container id:/www/  从主机复制到容器
    - docker cp container id:/www/  /tmp/  从容器复制到主机
- diff：检查容器里文件结构的变更

## 生命周期管理
- run / exec
    - -i ：开启 STDIN
    - -t ：分配伪tty
    - -d：守护式
    - --restart ：no（不重启） ，always （一直重启），on-failure:5（重试次数）
    - --name : 容器名称
    - --link container id:alias ：容器名:别名。可通过别名连接
    - --volumes-from：挂载数据卷容器
    - -h | --hostname：主机名
    - --privileged：启动docker的特权模式，docker 运行docker：https://github.com/jpetazzo/dind

- start / stop / restart
    - start  name | container id  启动一个或多少已经被停止的容器
    - stop name | container id  停止一个运行中的容器
        - 发送 SIGTERM
        - -t ：等待20S

    - restart name | container id 重启容器

- kill name | container id 杀掉一个运行中的容器
    - 发送 SIGKILL：kill -s KILL redis

- rm name | container id 删除一个或多少容器
    - -f：通过SIGKILL信号强制删除一个运行中的容器
    - -l：移除容器的网络连接
    - -v：删除与容器关联的卷

- pause / unpause  暂停|恢复容器

- exec 容器内运行
    - example ：docker exec -d container id command

## 容器操作
- ps  容器列表
    - -a ：所有
    - -l  ：latest  最后运行的容器
    - -q ：显示numeric id
- top  name | container id   查看进程
- attach name | container id  重新进入容器
- logs
    - -f ：跟随
    - -t ：显示时间
    - --tail=lines ：tail 行数

- wait  阻塞运行直到容器停止

- export 将容器导出成文件
    - docker export -o xx.tar container id

- port container  列出容器的端口

- inspect  name | container id  查看镜像的详情
    - --format='{{ .State.Running}} {{.Name}}'  Go语言的模板
