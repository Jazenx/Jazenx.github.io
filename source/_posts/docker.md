---
title: Docker 初识
tag: DevOps
date: 2018-09-23
---

为了解决软件开发的配置问题，一个比传统虚拟机强大先进不少的Docker容器诞生了。Docker是一个开源的引擎，可以轻松的为任何应用创建一个轻量级的、可移植的、自给自足的容器。

**Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。**它是目前最流行的 Linux 容器解决方案。

作为一个linux苦手，这次我会配合我的blog搭建，在实践中搞懂他。

## Docker的核心概念

- **Image**:镜像，分层的，可复用，Docker 把应用程序及其依赖，打包在 image 文件里面。
- **Container**:容器的存在离不开镜像的支持，他是镜像运行时的一个载体。
- **Repository**:Docker 的仓库和 git 的仓库比较相似，拥有仓库名、tag。在本地构建完镜像之后，即可通过仓库进行镜像的分发。

## 安装

- Mac

  - [Mac安装地址](https://docs.docker.com/docker-for-mac/install/)。下载的时候先要注册docker账号，下载最好翻墙，不然速度真的有点慢。
  - 打开dmg文件。直接进行安装。
  - 安装完成后，在命令行中执行 docker ，出现使用说明，则安装成功

- CentOS

  ```sh
  sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
    
  sdoudo yum-config-manager \
      --add-repo \
      https://download.docker.com/linux/centos/docker-ce.repo
  sudo yum install docker-ce docker-ce-cli containerd.io
  ```


## 相关操作

### 基本操作

```sh
systemctl start docker
systemctl daemon-reload
systemctl restart docker
service docker restart
docker service docker stop
docker systemctl stop docker
```

### 镜像网站

[DockerHub](https://hub.docker.com/) 里提供了很多的镜像，可让我们选择镜像。

### 拉取镜像

[docker pull](https://docs.docker.com/engine/reference/commandline/pull/) 拉取镜像到本地。

[docker images ](https://docs.docker.com/engine/reference/commandline/images/) 查看本地镜像。

### 创建 Docker 容器

[docker create](https://docs.docker.com/engine/reference/commandline/create/#options) 可以创建一个 docker 容器

```sh
docker create --name myContainer xxxxx:10.01
```

[docker start](https://docs.docker.com/engine/reference/commandline/start/) 可运行容器。

```sh
docker start myContainer
```

[docker ps](https://docs.docker.com/engine/reference/commandline/ps/#usage) 可查看运行中的 container

[docker exec](https://docs.docker.com/engine/reference/commandline/exec/) 可进入该 container。

[docker run](https://docs.docker.com/engine/reference/commandline/run/) 可以创建并运行一个容器，并且进入该容器。

```sh
docker run -it --name myContainer xxxx:10.01 /bin/bash
```

### 创建镜像

[docker commit](https://docs.docker.com/engine/reference/commandline/commit/) 

```sh
docker commit --author "rccoder" --message "curl+node" 9298 rccoder/myworkspace:v1
```

### 上传到dockerhub

首先注册一个 dockerhub 的账号，然后命令行中执行 [docker push](https://docs.docker.com/engine/reference/commandline/push/)

```sh
docker push urimage
```

### Dockerfile

我们可以用 Dockerfile 来实现 Dokcer 的持续集成。

Dockerfile 是由一堆命令和参数构成的脚本文件，使用 `docker build` 可执行脚本构建镜像,（类似于travis-ci 中的 `.travis.yml`）。

###  Docker的无限可能性

- **多环境的部署切换**：业务开发中往往需要区分开发环境与线上环境，利用 Docker 能原封不动的将开发环境中的 **代码与环境原封不动无污染的** 迁移到线上环境，配合一定的自动化流程即可实现自动的发布。
- **构建前端云**：`node_modules` 对于多人协作来说还是非常操蛋的，利用 Docker 可以在云端新建容器，远程 **无污染、低成本** 构建代码，实现 **不同人用的一定是同一个版本**。
- **快速配置复杂环境**
- **拥有CI**

## 参考

<https://github.com/rccoder/blog/issues/31>