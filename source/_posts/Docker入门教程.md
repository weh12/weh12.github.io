---
title: Docker入门教程
date: 2024-10-22 19:30:56
categories: 教程
tags: Docker
cover: https://pic.imgdb.cn/item/671713a9d29ded1a8ca43b5f.jpg
---
## Docker介绍

### 基本概念

Docker 是一个开源的应用容器引擎，它允许开发者打包他们的应用以及应用的依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

![](https://pic.imgdb.cn/item/671714b1d29ded1a8ca52875.png)

以下是 Docker 的一些关键特性和优势：

1. **轻量级**：Docker 容器共享宿主机的操作系统内核，不需要像虚拟机那样复制整个操作系统，因此启动更快，占用资源更少。
  
2. **可移植性**：Docker 容器可以在任何安装了 Docker 的机器上运行，无论是物理机、虚拟机、数据中心的服务器还是云服务器。
  
3. **版本控制**：Docker 支持版本控制，开发者可以使用版本控制系统（如 Git）来管理 Dockerfile 和镜像。
  
4. **快速迭代**：Docker 使得应用的部署、扩展和版本管理变得简单快捷。
  
5. **隔离性**：每个容器都是相互隔离的，运行在容器中的应用不会相互影响。
  
6. **可扩展性**：Docker 容器可以轻松地扩展或缩减，以满足应用需求的变化。
  
7. **自动化**：Docker 支持自动化的构建和部署流程，可以与持续集成/持续部署（CI/CD）工具链无缝集成。
  
8. **安全性**：Docker 提供了多种安全特性，包括容器的隔离、镜像扫描和安全更新。
  

Docker 的核心组件包括：

- **Docker 镜像（Images）**：只读的模板，用于创建 Docker 容器。镜像可以包含操作系统、应用及其依赖。
  
- **Docker 容器（Containers）**：镜像的运行实例。容器是隔离的、安全的，并且可以控制其对底层系统资源的使用。
  
- **Docker 客户端/服务器（Client/Server）**：Docker 客户端与 Docker 守护进程通信，后者负责构建、运行和分发容器。
  
- **Dockerfile**：一个文本文件，包含了用于构建 Docker 镜像的所有命令。
  
- **Docker 仓库（Registries）**：存储 Docker 镜像的服务，最著名的是 Docker Hub，用户可以上传和下载镜像。
  

Docker 适用于多种场景，包括开发、测试、持续集成、部署和微服务架构。它已经成为现代软件开发和运维领域中不可或缺的工具之一。

### 为什么要用Docker

> 你是否在本地开发环境得心应手, 但是到了测试和生产环境却出现很多问题, 在其他主机上还要重复配置环境和构建应用

这是一个测试开发和部署一个网站服务的的简单示例步骤

![](https://pic.imgdb.cn/item/67171afdd29ded1a8cb270c4.png)

在开发环境配置网站应用-到新的测试环境还要进行重复配置然后部署, 有了Docker就能实现自动构建环境然后运行应用和部署, Docker能将开发环境和应用打包成一个个集装箱, 只要你在开发环境中运行成功就行

无需再通过配置环境和开发执行等重复操作就能够运行应用程序和镜像文件

### 理解原理

很多人在不了解Docker原理的情况下盲目学习, 甚至放弃了学习, 这个概念就是Docker中的镜像 容器和仓库

![](https://pic.imgdb.cn/item/67171e67d29ded1a8cb86b59.png)

> **镜像是一个只读的模板**, 它可以用来创建容器, **容器是Docker的运行实例**, 它提供了一个独立的可移植的环境, 可以在这个环境中运行应用程序

镜像和容器的关系, 就像编程语言中类和实例的关系一样, 我们可以定义一个类中有那些属性和方法, 这个定义的类就是一个模板, 然后我们可以根据这个模板来创建多个实例, 这些实例就是这个类的对象, 对应到Docker中, 镜像就是一个模板, 容器就是这个模板的实例可以有一个也可以多个

> 镜像如何分享给别人? 这就涉及到了Docker仓库, 是用来存储Docker镜像的地方, 最流行和最常用的仓库就是Dockerhub, 它是一个公共的Docker仓库, 用来集中存储和管理Docker镜像, 也可以在这里下载和上传各种镜像, 从而实现镜像的共享和复用

## Docker的安装和配置

Docker的安装非常简单, 我们只需要在Docker官网找到包下来就行, 安装完成之后一定不要忘记启动Docker, 否则后面的操作都无法进行

### 在 Ubuntu 上安装 Docker

推荐csdn上的安装教程: [ubuntu24.04版本安装教程]([Ubuntu24.04安装Docker-CSDN博客](https://blog.csdn.net/qq_33653203/article/details/140855996?ops_request_misc=%257B%2522request%255Fid%2522%253A%25221C546306-D685-4FEC-BE2C-7BAAC4FBFFD0%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=1C546306-D685-4FEC-BE2C-7BAAC4FBFFD0&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-140855996-null-null.142^v100^pc_search_result_base6&utm_term=ubuntu24.04%E5%AE%89%E8%A3%85docker&spm=1018.2226.3001.4187))

### 在 CentOS 上安装 Docker

**卸载旧版本（如果安装过）：**

- ```bash
  sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-engine
  ```
  
- **安装需要的软件包：**
  
- ```bash
  sudo yum install -y yum-utils
  ```
  
- **设置稳定版仓库：**
  
- ```bash
  sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
  ```
  
- **安装 Docker Engine：**
  
- ```bash
  sudo yum install docker-ce docker-ce-cli containerd.io
  ```
  
- **启动 Docker 服务：**
  
- ```bash
  sudo systemctl start docker
  ```
  
- **验证 Docker 是否正确安装：**
  
- ```bash
  sudo docker run hello-world
  ```
  

### 在 Windows 上安装 Docker

对于 Windows，你可以使用 Docker Desktop for Windows。请访问 Docker 官网下载并安装 Docker Desktop，然后按照安装向导进行安装。

### 在 macOS 上安装 Docker

对于 macOS，你可以使用 Docker Desktop for Mac。同样地，访问 Docker 官网下载并安装 Docker Desktop，然后按照安装向导进行安装。

安装完成后，你可以通过命令行或者 Docker Desktop 的图形界面来管理你的容器和镜像。记得在安装过程中，根据你的系统环境选择正确的安装包和命令。

这时就能使用

```bash
docker version
```

看到版本信息,其中包含了Client和Server信息

这里再简单说明一下, Docker是使用**Client-Server**架构模式, **Docker Client**和**Docker Server**之间通过Socket或者RESTful API进行通信, **Docker Daemon**就是服务端的守护进程, 它负责管理Docker的各种资源, Docker Client负责向Docker Daemon发送请求, Docker Daemon将请求进行处理, 然后将结果返回给Docker Client, 所以我们在终端中输入的各种Docker命令都是通过Docker客户端发送给Docker Daemon的

## 容器化和Dockerfile

### 容器化(containerization)

顾名思义就是将应用程序打包成容器, 然后在容器中运行应用程序的过程

这个过程简单来说可以分为三个步骤

1. 首先需要创建一个Dockerfile, 来告诉Docker构建应用程序镜像所需要的步骤和配置
  
2. 然后使用Dockerfile来构建镜像
  
3. 使用镜像创建和运行容器
  

### 什么是Dockerfile

Dockerfile是一个文本文件, 里面包含了一条条的指令, 用来告诉Docker如何来构建镜像,

这个镜像包裹了我们应用程序执行的所有命令

## 实践案例

### 简单构建一个Docker镜像

构建 Docker 镜像的基本语法如下：

```bash
docker build [OPTIONS] PATH
```

其中，`[OPTIONS]` 是可选参数，`PATH` 是上下文路径，可以是包含 Dockerfile 的目录路径，也可以是一个 URL 或者是一个 tar 压缩包。

一些常用的选项包括：

- `-t` 或 `--tag`：为镜像指定一个标签（name），通常格式为 `<仓库名>:<标签>`，例如 `myimage:latest`。
- `--no-cache`：构建过程中不使用缓存。
- `--build-arg`：传递变量到构建过程中。
- `--network`：设置构建时的网络模式。
- `--pull`：尝试拉取最新的镜像作为构建的基础镜像。
- `--target`：构建指定的目标阶段。

一个简单的 `Dockerfile` 示例如下：

```Dockerfile
# 使用官方的 Python 运行时作为父镜像
FROM python:3.8-slim

# 设置工作目录
WORKDIR /app

# 将当前目录内容复制到位于容器内的工作目录
COPY . /app

# 安装任何所需的包
RUN pip install --no-cache-dir -r requirements.txt

# 制作容器时运行的应用
CMD ["python", "./app.py"]
```

我们来创建一个最简单的案例, 来实际演示一下编写Dockerfile以及创建镜像, 启动容器的这个过程:

> 首先创建一个HelloDocker的文件夹, 在文件夹中我们创建一个index.js的文件来演示在新环境下配置nodejs的开发环境, 这是Dockerfile所需的步骤:

![](https://pic.imgdb.cn/item/67177438d29ded1a8c4631fb.png)

只要把命令写清楚, 剩下的工作就交给Docker来自动完成

> 我们在根目录直接创建一个叫Dockerfile的文件

如果使用vscode编辑器, 可以安装一个叫Docker的扩展以便更好操作

> 在Dockerfile中我们需要先指定一个基础的镜像

镜像是按层次结构来构建的, 每一层都是基于上一层的, 然后在这个镜像上添加我们的应用程序, 我们可以从一个基础的Linux镜像开始, 然后在这个镜像基础上安装nodejs和我们的应用程序, 而我直接使用了以linux构建的nodejs

> 配置完运行环境之后, 还需要把我们的应用程序复制到镜像中, 可以使用**COPY命令**来复制文件, COPY后面可以加上两个参数

第一个参数表示原路径, 第二个参数表示目标路径

这里的原路径是指相对于Dockerfile文件的路径, 目标路径是相对于镜像的路径

> 然后我们需要在镜像中运行应用程序, 可以使用CMD命令来运行应用程序

CMD命令可以使用方括号[ ], 方括号里的第一个参数表示可执行程序的名字, 第二个参数表示这个可执行程序接收到的参数, 或者也可以直接简单的写成**名字+参数**

> 保存Dockerfile, 我们执行Docker构建命令

```bash
# hello-docker就是镜像的名字,后面加上一个.表示当前目录
docker build -t hello-docker .
```

> 可以使用docker images或者docker image ls来查看我们所有的镜像

![](https://pic.imgdb.cn/item/67177be1d29ded1a8c4cd869.png)

这里的TAG中的latest表示的是镜像的标签, 也就是镜像的版本, 如果不指定版本默认就是**latest**

如果你想指定版本, 那么就可以在镜像名字后面加上冒号和版本号, 这样就完成了镜像的构建

最终文件内容:

```Dockerfile
FROM node:14-alpine
COPY index.js /index.js
CMD node /index.js
```

### 运行Docker镜像

那怎么来运行呢? 非常简单!!!

> 运行构建的镜像

只需要通过run命令

```bash
docker run hello-docker
```

> 如果你需要在另一个环境中运行这个应用程序, 那么就只需要把这个镜像文件复制过去, 然后执行命令就行, 也可以把这个镜像文件上传到DockerHub或者镜像仓库中, 之后, 任何人都可以在任何地方通过<mark>docker pull</mark>命令来下载这个镜像文件

### 删除Docker镜像

使用 `docker rmi` 命令来删除一个或多个镜像。你需要提供镜像的名称和标签（如果有的话），或者直接提供镜像ID。

```bash
docker rmi [镜像名称]:[标签]
```

或者，如果你知道镜像的ID：

```bash
docker rmi [镜像ID]
```

如果你想要删除多个镜像，可以一次性列出它们：

- ```bash
  docker rmi [镜像1名称]:[标签1] [镜像2名称]:[标签2] ...
  ```
  
- **强制删除**：  
  如果镜像正在被使用（例如，有容器正在使用该镜像运行），Docker 默认不允许删除。在这种情况下，你可以使用 `-f` 或 `--force` 选项来强制删除镜像。但请注意，这将删除所有依赖于该镜像的容器。
  
- ```bash
  docker rmi -f [镜像名称]:[标签]
  ```
  
- **删除无用的镜像**：  
  如果你想要清理所有未被任何容器使用的悬空镜像（dangling images），可以使用 `docker image prune` 命令。
  

```bash
docker image prune
```

```bash
docker rmi -f $(docker images -aq)
#删除全部镜像  -a 意思为显示全部, -q 意思为只显示ID
```

这将删除所有未被任何容器引用的镜像。

### 容器具体操作

1. 查看正在运行的容器
  
  ```bash
  docker ps
  docker ps -a # 查看所有容器
  #加格式化方式访问，格式会更加清爽
  docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"
  ```
  
2. 创建容器
  

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
常用参数：
--name=NAME #为容器指定名字为NAME，不使用的话系统自动为容器命名
-d: 后台运行容器并返回容器ID，也即启动守护式容器(后台运行)；

-i：以交互模式运行容器，通常与 -t 同时使用；
-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；
也即启动交互式容器(前台有伪终端，等待交互，一般连用，即-it)；

-P: 随机端口映射，大写P
-p: 指定端口映射，小写p
# 端口格式前面表示运行容器的端口,后面是映射到宿主机的端口

# 创建并允许 Nginx 容器

docker run -d --name nginx -p 80:80 nginx
```

3. 启动守护式容器（后台运行）

```bash
docker run -d 容器名
docker run -d redis:6.0.8
```

4. 停止容器

```bash
docker stop 容器名
docker stop nginx
```

5. 启动容器

```bash
docker start 容器名
docker start nginx
docker restart 容器名
docker restart nginx
```

6. 进入正在运行的容器

```bash
docker exec -it 容器名 bashshell
docker exec -it nginx /bin/bash
```

7. 停止容器

```bash
docker stop 容器名
docker stop nginx
```

8. 强制停止容器

```bash
docker kill 容器名
docker kill nginx
```

9. 删除容器

```bash
#删除一个
docker rm 容器ID  
docker rm nginx
docker rm -f 容器ID #强制删除
docker rm -f nginx

#删除多个
docker rm -f $(docker ps -a -q)
或
docker ps -a -q | xargs docker rm
```

## Docker Compose

![](https://pic.imgdb.cn/item/671786fad29ded1a8c65c0cf.png)

### Docker compose介绍

Docker Compose 是 Docker 官方编排工具，用于定义和运行多容器 Docker 应用程序。使用 Docker Compose，您可以通过一个 YAML 文件来配置您的应用服务，然后使用一个简单的命令来启动和停止所有服务。

以下是 Docker Compose 的一些关键特性和用途：

1. **多容器编排**：可以定义多个容器，每个容器运行不同的服务，如 web 服务器、数据库、缓存等。
  
2. **服务定义**：通过一个 `docker-compose.yml` 文件来定义服务、网络和卷。这个文件描述了组成应用的各个部分。
  
3. **一键部署**：使用 `docker-compose up` 命令可以启动所有服务，而 `docker-compose down` 命令可以停止并移除这些服务。
  
4. **环境一致性**：可以在开发环境、测试环境和生产环境之间保持一致的配置。
  
5. **依赖管理**：可以定义服务之间的依赖关系，确保在启动和停止服务时按照正确的顺序进行。
  
6. **网络管理**：可以定义服务之间的网络连接，使得服务之间可以通过容器名互相访问。
  
7. **数据卷管理**：可以定义数据卷，用于持久化数据，即使容器被删除，数据也不会丢失。
  
8. **扩展性**：可以轻松扩展服务的实例数量，例如，增加多个 web 服务实例来负载均衡。
  
9. **易于版本控制**：`docker-compose.yml` 文件可以被版本控制系统跟踪，便于团队协作和版本管理。
  
10. **与 Docker Swarm 集成**：Docker Compose 文件可以被转换成 Docker Swarm 模式的部署，便于在集群环境中部署和管理容器。
  

一个简单的 `docker-compose.yml` 文件示例可能如下所示：

```yaml
version: '3'
services:
  web:
    image: "nginx:latest"
    ports:
      - "80:80"
    depends_on:
      - db
  db:
    image: "postgres:latest"
    environment:
      POSTGRES_DB: "mydb"
      POSTGRES_USER: "user"
      POSTGRES_PASSWORD: "password"
```

在这个例子中，定义了两个服务：`web` 和 `db`。`web` 服务使用最新的 Nginx 镜像，并映射了容器的 80 端口到宿主机的 80 端口。`db` 服务使用最新的 PostgreSQL 镜像，并设置了数据库的环境变量。

要启动这个配置，只需在包含 `docker-compose.yml` 文件的目录下运行 `docker-compose up` 命令。要停止服务，可以运行 `docker-compose down` 命令。

## docker系统命令

在Ubuntu系统中关闭Docker服务，可以通过命令行来完成。以下是关闭Docker服务的步骤：

1. **停止Docker服务**：  
  打开终端，输入以下命令来停止Docker服务：

- ```bash
  sudo systemctl stop docker
  ```
  
- **禁用Docker服务**：  
  如果你希望在系统启动时Docker服务不会自动启动，可以使用以下命令来禁用它：
  
- ```bash
  sudo systemctl disable docker
  ```
  
- **卸载Docker**：  
  如果你想要完全卸载Docker，可以使用以下命令：
  
- ```bash
  sudo apt-get purge docker-engine docker docker.io docker-ce
  ```
  
  这将卸载Docker CE（社区版），包括所有配置文件和镜像。
  
- **清理Docker残留数据**：  
  卸载Docker后，你可能还需要清理残留的数据，例如镜像、容器、卷等。你可以使用以下命令来清理：
  
- ```bash
  sudo rm -rf /var/lib/docker
  ```
  
  请注意，这个命令会删除所有Docker数据，包括镜像和容器，所以在执行前请确保你已经备份了需要的数据。
  
- **重新加载系统服务**：  
  在进行上述操作后，你可能需要重新加载系统服务，以确保更改生效：
  

1. ```bash
  sudo systemctl daemon-reload
  ```
  

请确保在执行这些操作时具有相应的权限，通常需要使用`sudo`来获取管理员权限。如果你在执行过程中遇到任何问题，可以查看Docker的官方文档或寻求社区的帮助。

**重新启用Docker服务**：  
使用以下命令来重新启用Docker服务，这样它就会在系统启动时自动启动：

- ```bash
  sudo systemctl enable docker
  ```
  
- **启动Docker服务**：  
  如果你想要立即启动Docker服务，可以使用以下命令：
  
- ```bash
  sudo systemctl start docker
  ```
  
- **检查Docker服务状态**：  
  你可以使用以下命令来检查Docker服务的状态，确保它正在运行：
  

```bash
sudo systemctl status docker
```

当你尝试停止 Docker 服务时遇到 `'docker.service', but its triggering units are still active: docker.socket` 的错误时，这通常意味着 Docker 服务虽然停止了，但是相关的 socket 服务仍在运行。以下是一些可能的解决方案：

1. **检查并停止 Docker socket**：  
  如果 Docker socket 服务仍在运行，你需要停止它。使用以下命令：
  

- ```bash
  sudo systemctl stop docker.socket
  ```
  
- **检查 Docker 服务状态**：  
  检查 Docker 服务和 socket 的状态，以确认它们是否都已停止：
  
- ```bash
  sudo systemctl status docker
  sudo systemctl status docker.socket
  ```
  
- **重新启动 Docker 服务**：  
  如果需要，尝试重新启动 Docker 服务：
  
- ```bash
  sudo systemctl enable docker
  sudo systemctl start docker.socket
  sudo systemctl start docker
  ```