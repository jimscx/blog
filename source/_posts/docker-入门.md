---
title: docker 入门
date: 2016-9-1 21:22:33
tags: Linux 
---
#### 1. dcoker 环境安装
在本机玩docker,到[官网](https://docs.docker.com/engine/getstarted/step_one/),使用的环境是mac

#### 2.docker 容器和镜像 
docker镜像在我的理解中就是一个小型的linux系统的压缩包，而容器就是根据这个压缩包启动一个Linux环境，在制作镜像的时候你可以给这个Linux系统安装好多的工具和环境，比如GCC,vim,node......
当制作好一个镜像后然后启动这个镜像就形成了一个docker容器，ok 现在你可以进入这个容器了，里面有根据你的镜像形成的环境。

#### 2.1. docker镜像的制作
大多数的时候我们可以去[dokerhub上](https://hub.docker.com/)寻找需要的，然后pull。根据基础镜像搭建自己的镜像那就需要书写Dockerfile了，其实这个文件和Makefile有点像，书写好之后docker build 一下就好啦
 
 #### 2.1.1 Dockerfile
 Dockfile是一种被Docker程序解释的脚本，Dockerfile由一条一条的指令组成，每条指令对应Linux下面的一条命令。Docker程序将这些Dockerfile指令翻译真正的Linux命令
```
# VERSION    1.0.0 

FROM daocloud.io/node:5 #基于哪个镜像

# 指定端口
ENV HTTP_PORT 8000

# 复制当前文件夹中的所有文件到镜像中的 /genee-wechat 中，指定启动容器时的工作目录也就是cd命令
COPY . /genee-wechat
WORKDIR /genee-wechat

# 安装软件用
RUN npm --registry=https://registry.npm.taobao.org --disturl=https://npm.taobao.org/dist install \
    && npm install  gulp -g 

EXPOSE 8000

# container启动时执行的命令，但是一个Dockerfile中只能有一条CMD命令，多条则只执行最后一条CMD.
CMD ["npm", "start"] 
```
* 创建镜像`docker build -t <镜像名> <Dockerfile路径>` 比如：

>  如Dockerfile在当前路径： docker build -t xx/gitlab .

##### 以下是常用的的和镜像有关的几个命令 
* 拉取镜像
```
docker pull <镜像名:tag>
docker pull node:0.12.5
```

* 查看已经拉取得镜像
```
docker images
```

* 删除镜像
```
docker rmi xxx:tag
```

#### docker 容器
* 根据镜像创建自己的docker容器并启动
```
docker run --name jimscx.node -i -t -p 9003:9000 -p 9013:9010 -d -v /User/home:/home node
```
> --name 指定容器的名字 -p端口映射(容器外的端口：容器里的端口)可以有多个端口映射只是不要和别的容器的冲突就好 -i表示同步container的stdin，-t表示同步container的输出，-d表示deamon，以后台启动这个container -v:目录映射 （容器外的目录：容器里边的目录） 最后是镜像的名字

* 进入自己创建的容器
`docker exec -it xxx bash`
>xxx便是容器的名字

* 查看正在运行的容器

```
docker ps
docker ps -a为查看所有的容器，包括已经停止的。
```
* 查看容器详情

```
docker inspect xxxx
```
* 查看容log

```
docker logs xxxx
```

* 删除单个容器

```
docker rm <容器名orID>
```
* 停止、启动、杀死一个容器

```
docker stop <容器名orID>
docker start <容器名orID>
docker kill <容器名orID>
```


