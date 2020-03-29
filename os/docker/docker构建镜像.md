# docker 构建镜像

#### example
- docker build --no-cache -t  name:version

#### 指令
- 注释
    - \# version:0.0.1

- base镜像
    - FROM ubuntu:14.04

- 描述镜像的创建者，名称和邮箱  
    - MAINTAINER name  <email>

- RUN 执行构建指令
    - RUN["cmd","param1","param2"]
    - RUN["sh","-c","echo","$HOME"]
    - RUN["apt-get","install","-y","nginx"]

- 环境变量 ENV name value   |  ENV  name=value

- 指定运行用户  USER admin

- VOLUME  数据卷 ??

- 复制文件
    - ADD   src   dest   
        - 如果是压缩文件 gzip bzip2 xz  归档文件 提取（extraction） 和解开 （decompression）
        - 支持 远程文件
    - COPY  src  dest  只是复制文件，和Dockerfile 同一目录
    - uid 和 gid 都设置为0，目录的模式：0755

- 开放端口
    - EXPOSE 80

- 工作目录 WORKDIR

- 触发 ONBUILD ，该镜像被用作其他进行的基础镜像，
   - ONBUILD ADD . /app

- 容器启动运行
    - CMD [""]
    - ENTRYPOINT ["/usr/bin/ebash"]    和cmd 类似，不能被 run 覆盖，覆盖带上选线：--entrypoint
    - 备注：指令可使用数组，不使用数组 docker 会在命令前  /bin/sh -c "xxxx"
    - 可推荐用monit, supervisor 来启动多进程

- 忽略 .dockerignore
    - 排除构建镜像时不需要的文件或目录 对 COPY  ADD 有效
    - 采用的是Go语言的 filepath

#### 缓存：不依赖外部的指令，可使用缓存
- 主动屏蔽 缓存 --no-cache

#### 优化措施
