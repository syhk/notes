安装可以在直接在官网查看。

官网安装参考手册：[https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)

## Docker 启动

```latex
systemctl  start docker
```

查看 Docker版本：

docker version

查看安装的镜像：

docker images

测试运行hello

docker run hello-world

卸载Docker

```latex
systemctl stop docker
yum -y remove docker-ce docker-ce-cli containerd.io
rm -rf /var/lib/docker
```

## Docker 帮助命令

```latex
docker version # 显示 Docker 版本信息。
docker info # 显示 Docker 系统信息，包括镜像和容器数。
docker --help # 帮助
```

## Docker常用命令

```latex
docker search  镜像名称  //搜索镜像
可选项：
docker search mysql --filter=stars=1000
筛选出点赞数在1000以上的镜像
```

### 下载镜像

```latex
docker pull  镜像名称   //不写版本号默认是最新版本
```

### 删除镜像

```latex
docker rmi -f  镜像名称或者镜像Id
```

### 窗口的启动



```latex
docker run -it centos /bin/bash
```

退出窗口使用： exit

不关闭容器放入后台使用 ctrl+p+q;

## 容器查看

```latex
docker ps   //列出当前正在使用的容器 
docker ps -a  //列出所有的容器
```

## 启动停止容器

```latex
docker start  容器名or容器id    启动容器
docker restart ...            重启容器
docker stop    ...            停止容器
docker kill    ...            强制停止容器
```

## 删除容器

```latex
docker rm 容器id     删除指定容器
docker rm -f $(docker ps -a -q)   删除所有容器
docker ps -a -q |xargs docker rm  删除所有容器
```

用后台方式启动容器： docker run -d centos

## 清理停止的容器

```latex
docker container prune
```

# 查看容器日志

```latex
docker logs -f -t --tial  容器id
docker logs 容器id
```

## 查看正在运行容器的进程信息

```latex
docker top 容器Id
```

## 查看容器/镜像的元数据

```latex
docker inspect 容器id
```

# 进入正在运行的容器

命令一：

```latex
docker exec -it   容器id   bashShell
docker attach     容器id 
区别：
exec 是在容器中打开新的终端，并且可以启动新的进程
attach 直接进入容器启动命令的终端，不会启动新的进程
```

## 容器内拷贝文件到主机上

```latex
docker cp  容器id:容器内路径  目的主机路径
eg: docker cp 7f9ead6f906b:/home/f1 /home
```

# Docker 可视化

Portainer是Docker的图形化管理工具，提供状态显示面板、应用模板快速部署、容器镜像网络数据卷 的基本操作（包括上传下载镜像，创建容器等操作）、事件日志显示、容器控制台操作、Swarm集群和 服务等集中管理和操作、登录用户管理和控制等功能。功能十分全面，基本能满足中小型单位对容器管理的全部需求。

## docker 提交镜像

docker commit 从容器创建一个新的镜像。

```latex
docker commit -m="提交的描述信息"  -a="作者"  容器id 要创建的目标镜像名：【标签名】
```

# Docker 数据卷

在用 docker run 命令的时候，使用 -v 来标记创建一个数据卷并挂载到容器里。

```latex
docker run -it -v 宿主机绝对路径目录:容器内目录 镜像名
```

挂载成功以后，在容器中创建的文件在宿主机里面也会看到。

# DockerFile

DockerFile 是用来构建Docker镜像的构建文件，是由一些列命令和参数构成的脚本。

1.  新建一个DockerFile
2.  build生成镜像 

build生成镜像，获得一个新镜像 test-centos，注意最后面有个    .

```bash
docker build -f /home/docker-test-volume/dockerfile1 -t test-centos .
```

那个点一定不能忘记

然后就可以启动容器：

```bash
docker run -it test-centos /bin/bash
```

eg:



# 匿名挂载和具名挂载

-v 容器内路径

```bash
docker run -d -P --name nginx01 -v /etc/nginx nginx
```

可通过命令 `docker volume ls` 查看挂载的列表.

这些没指定名字的都是匿名挂载，我们 -v 只写了容器内路径，并没写容器外路径

```bash
[root@VM-0-6-centos ~]# docker volume ls
DRIVER    VOLUME NAME
local     4d0221bc0d8b9e44fb2e878cd3efcacb9b4bd51c8e135d79c549f7a6345f3a24
local     7a1e6924fed1cc5ea6a386d9b2542c0ffc53fada1755bc7d09601274dff6ddd0
local     7adb0e2e33503b17abfd453fded4b0cd9d9e8b05e064d248dc47de0da6456788
local     adaa3053cb2ff95afc7bab51451f4b1167aa1b9056398ed44b0d4cae9580db52
```

挂载目录是： `/var/lib/docker/volumes/VOLUME-NAME/_data`

匿名挂载的缺点，就是不好维护，不清楚目录挂载的是哪个容器

## 具名挂载

-v 卷名:/容器内路径

```
[root@VM-0-6-centos ~]# docker run -d -P --name nginx02 -v juming:/etc/nginx nginx
112f36599f077eada56197c22dd3b3a3eaba2e5bb38bf2cb19adc783163991e7
[root@VM-0-6-centos ~]# docker volume ls
DRIVER    VOLUME NAME
local     4d0221bc0d8b9e44fb2e878cd3efcacb9b4bd51c8e135d79c549f7a6345f3a24
local     7a1e6924fed1cc5ea6a386d9b2542c0ffc53fada1755bc7d09601274dff6ddd0
local     7adb0e2e33503b17abfd453fded4b0cd9d9e8b05e064d248dc47de0da6456788
local     adaa3053cb2ff95afc7bab51451f4b1167aa1b9056398ed44b0d4cae9580db52
local     juming
```

查看挂载的目录：`docker volume VOLUME-NAME`

挂载操作中，没指定目录名情况下，默认在 `/var/lib/docker/volumes/` 目录下

# CMD 和 ENTRYPOINT 的区别

我们之前说过，两个命令都是指定一个容器启动时要运行的命令

- CMD
Dockerfile 中可以有多个CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换！
- ENTRYPOINT
docker run 之后的参数会被当做参数传递给 ENTRYPOINT，之后形成新的命令组合！



# Docker Compose

定义运行多个容器；

Compose是docker官方的开源项目

Dockerfile 让程序在任何地方运行。

eg:



Compose:

- 服务services，容器。应用。
- 项目project。一组关联的容器。

下载

```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

上面那个下载太慢用国内的下载更快

```bash
curl -L "https://get.daocloud.io/docker/compose/releases/download/1.26.2/docker-compose=$(uname -$)-$(uname -m)" -o /usr/local/bin/docker-compose
```



# Docker Swarm

# Docker Stack

# Docker secret

# Docker Config
