---
title: n8n工作流本地下载安装使用并实现端口映射
date: 2024-11-17 00:56:08
categories: 教程
tags: n8n
cover: https://pic.imgdb.cn/item/6738cfb6d29ded1a8ceed9a1.jpg
---
## 简介

![](https://pic.imgdb.cn/item/6738cfb6d29ded1a8ceed9a1.jpg)

n8n是一款开源的工作流自动化工具，它可以帮助用户轻松地将不同的应用程序和服务连接起来，实现自动化任务。n8n基于节点，使得用户可以自定义工作流，无需编写代码即可实现复杂的数据操作和流程控制。

## 1. 使用Docker进行安装

### 步骤：

1. **安装Docker**：首先，你需要在你的计算机上安装Docker。访问Docker官方网站，下载并安装适合你操作系统的版本[^5^]。
2. **拉取n8n镜像**：在Docker Desktop中搜索n8n，选择n8nio/n8n官方镜像，并下载[^5^]。
3. **创建容器**：创建一个新的容器，设置端口为`5678`，并将你本地的一个目录映射到容器中的`/home/node/.n8n/`路径，以实现数据持久化[^5^]。
4. **访问n8n**：启动容器后，你可以通过访问`http://localhost:5678/`来进入n8n的管理界面[^5^]。

### Docker Compose方法：

如果你更倾向于使用Docker Compose来部署n8n，可以按照以下步骤操作：

1. **创建Docker Compose文件**：创建一个`docker-compose.yml`文件，并填入以下内容：

 ```yaml
  version: '3.8'
  volumes:
   n8n_storage:
  services:
   n8n:
     image: docker.n8n.io/n8nio/n8n
     restart: always
     ports:
       - 5678:5678
     volumes:
       - n8n_storage:/home/node/.n8n
     command: /bin/sh -c "n8n start --tunnel"
  ```
  
  这样配置后，n8n将使用端口`5678`，并且数据会被存储在`n8n_storage`卷中[^2^]。
  
2. **启动n8n服务**：在包含`docker-compose.yml`的目录下运行`docker-compose up`命令来启动n8n服务[^2^]。
  
3. **访问n8n**：服务启动后，你可以通过`http://localhost:5678/`访问n8n的界面，并进行注册和登录[^2^]。
  

## 2. 使用npx进行安装（不推荐）

你还可以通过npx来安装n8n，命令为`npx n8n`。但这种方法可能会遇到一些问题，因此不推荐使用[^1^]。

以上是两种主流的本地安装n8n的方法，推荐使用Docker方式，因为它更加稳定且易于管理。安装完成后，你就可以开始创建和配置工作流了。

## 3.通过npm方式安装

要使用npm方式下载安装并使用n8n，并保证其一直运行在后台，你可以按照以下步骤操作：

### 1. 安装n8n

首先，你需要确保你的计算机上已经安装了Node.js，n8n需要Node.js 18或以上版本。然后，你可以通过npm全局安装n8n：

```sh
npm install -g n8n
```

如果你需要安装或更新到特定版本的n8n，可以使用`@`语法指定版本，例如：

```sh
npm install -g n8n@0.126.1
```

### 2. 启动n8n

安装完成后，你可以通过以下命令启动n8n：

```sh
n8n start
```

这将启动n8n服务，你可以通过访问`http://localhost:5678`来进入n8n的管理界面。

### 3. 后台运行n8n

为了确保n8n一直在后台运行，你可以使用`nohup`命令或者`screen`会话来实现。以下是使用`nohup`命令的例子：

```sh
nohup n8n start &
```

这个命令会将n8n服务放到后台运行，并且输出日志会被重定向到`nohup.out`文件中。`&`符号表示将命令放到后台执行。

### 注意事项

- Windows用户需要先切换到`.n8n`目录下再运行`n8n start`命令[^8^]。
- 确保你的防火墙设置允许n8n使用的端口（默认是5678）的流量通过。

通过上述步骤，你可以在本地使用npm安装n8n，并确保其在后台持续运行。

## 通过ngork开通隧道实现外网访问你的工作流

### ngork是什么?

ngrok是一个反向代理工具，它能够创建一个安全的通道，将本地服务器暴露到互联网上。这意味着你可以将本地运行的Web服务（例如localhost:8080）映射到一个ngrok提供的公共URL,例如[http://your-app.ngrok.io](http://your-app.ngrok.io)，从而让互联网用户能够访问你的本地服务

![](https://pic.imgdb.cn/item/6738cfb5d29ded1a8ceed835.png)
![](https://pic.imgdb.cn/item/6738cfb6d29ded1a8ceed94f.png)

### 安装 Ngrok

1. **下载 Ngrok**：从 Ngrok 的官方网站下载适合你的操作系统的版本。
  
2. **解压和安装**：解压下载的文件，然后将 `ngrok` 可执行文件移动到你的系统路径中，比如 `/usr/local/bin/`（对于Unix系统）或添加到你的系统环境变量中（对于Windows）。
  
### 使用 Ngrok

1. **基本使用**：
  
  - 运行一个本地服务器。例如，如果你用 Python 的 `http.server` 模块运行一个服务器，命令可能是 `python -m http.server 8000`。
  - 然后，在另一个终端窗口运行 `ngrok http 8000`。这会将你的本地8000端口暴露到互联网上，Ngrok会提供一个公共URL。
2. **设置自定义域名**：
  
  - 你可以通过 Ngrok 的仪表板添加一个自定义域名，并使用 `ngrok http --hostname=yourdomain.ngrok.io 8000` 来启动。
3. **保护你的隧道**：
  
  - 使用 `--basic-auth` 选项来添加HTTP基本认证，例如 `ngrok http --basic-auth 'user:password' 8000`。
4. **检查连接**：
  
  - 打开你的浏览器或使用 `curl` 访问Ngrok提供的URL，你应该能看到你的本地服务器内容。

### 高级功能

- **TCP隧道**：如果你需要公开TCP服务（比如SSH），可以使用 `ngrok tcp 22`。
- **自定义配置**：你可以使用配置文件来定义更多的选项和设置。
- **API和自动化**：Ngrok提供API以便于自动化和集成到你的CI/CD流程中。

### 注意事项

- **安全性**：公开你的本地服务可能会引入安全风险，确保你理解这些风险，尤其是如果你暴露的是未经充分保护的服务。
- **免费版限制**：Ngrok的免费版本有带宽和连接限制，对于更高的需求，你可能需要订阅付费计划。
- **持续连接**：免费版的Ngrok会定期断开连接，如果需要持续连接，考虑使用付费服务或自行部署Ngrok。

Ngrok是一个非常灵活的工具，适合在开发过程中进行快速测试或展示，但对于生产环境，使用更稳定的解决方案通常更为合适。