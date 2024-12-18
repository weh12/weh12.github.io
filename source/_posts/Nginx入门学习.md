---
title: Nginx入门学习
date: 2024-10-24 04:14:31
categories: 教程
tags: Nginx
cover: https://pic.imgdb.cn/item/671959dad29ded1a8c43da37.png
---
## Nginx介绍

Nginx（发音为“engine-x”）是一个高性能的HTTP和反向代理服务器, 它是通过C语言开发的，也是一个常用的邮件代理服务器和通用TCP/UDP代理服务器。它最初由俄罗斯的IgorSysoev开发，并于2004年首次公开发布。Nginx以其高稳定性、丰富的功能集、简单的配置和低资源消耗而闻名。

Nginx是为了解决C10K=10000 concurrent connection, 同时处理10000个并发连接, 这个问题是在当时是非常严重的, 因为当时的服务器软件都是单线程的, 所以在高并发的情况下服务器的性能会变得非常的差

![](https://pic.imgdb.cn/item/6762e665d0e0a243d4e62d2b.jpg)

Nginx的出现很好的解决了这个问题, Nginx可以支持5万个并发连接, 在后来的发展中Nginx也逐渐成为了最流行的Web服务器软件

以下是Nginx的一些主要特点和用途：

1. **高性能的Web服务器**：Nginx能够处理大量的并发连接，而内存消耗相对较低，这使得它非常适合作为静态内容的Web服务器。
  
2. **反向代理服务器**：Nginx可以作为反向代理服务器，将客户端的请求转发到后端服务器，如Apache或另一台Nginx服务器。这有助于负载均衡和提高网站的可用性。
  
3. **负载均衡器**：Nginx支持多种负载均衡策略，如轮询、最少连接、IP哈希等，可以将流量分配给多个后端服务器，以提高性能和可靠性。
  
4. **HTTP缓存**：Nginx可以缓存静态内容，减少后端服务器的负载，提高响应速度。
  
5. **SSL终端代理**：Nginx支持SSL/TLS加密，可以作为SSL终端代理，处理HTTPS连接。
  
6. **模块化**：Nginx具有高度模块化的设计，可以通过安装额外的模块来扩展其功能。
  
7. **配置简单**：Nginx的配置文件结构清晰，易于理解和管理。
  
8. **跨平台**：Nginx可以在多种操作系统上运行，包括Linux、FreeBSD、Solaris、Mac OS X和Windows。
  
9. **邮件代理**：Nginx也可以作为邮件代理服务器，支持SMTP、POP3和IMAP协议。
  
10. **通用TCP/UDP代理**：Nginx可以转发TCP和UDP流量，使其成为一个灵活的网络代理工具。
  

Nginx的这些特性使其成为构建高性能、高可用性和高可扩展性网络服务的理想选择。它广泛应用于互联网上的网站、API服务和内部网络服务。

## Nginx的安装

Nginx有三种方式安装:

> 第一种直接通过包管理器安装

![](https://pic.imgdb.cn/item/67192a6dd29ded1a8c26bbfc.png)

> 第二种通过编译安装

![](https://pic.imgdb.cn/item/67192b12d29ded1a8c273a4b.png)

> 第三种通过Docker安装

![](https://pic.imgdb.cn/item/67192b9cd29ded1a8c27bc99.png)

## Nginx服务启动和停止

我是通过Docker进行安装的, 因为为了更方便的学习不至于下载nginx

### 安装启动

下面简单说说Docker中Nginx下载配置:

```bash
docker pull nginx # 直接下载镜像,简单粗暴
```

下载后我们查看镜像是否安装:

```bash
docker images #列出所有镜像
```

没有问题, 之后直接创建一个容器, 本地映射到8080端口, 名字叫tongye-nginx

```bash
docker run --name tongye-nginx -d -p 8080:80 nginx # -d表示在后台运行
```

这样我们的nginx服务就启动了, 在浏览器中输入localhost:8080直接就能打开

![](https://pic.imgdb.cn/item/67193c54d29ded1a8c345d4c.png)

现在查看我们的端口占用情况, 由于我映射了端口, 在8080, 不是Docker安装启动可能是其他端口, 视情况而定

```bash
sudo lsof -i :8080 #查看8080端口占用情况
```

我们可以直接看到docker-pr执行了

```bash
COMMAND     PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
docker-pr 33558 root    4u  IPv4 223350      0t0  TCP *:http-alt (LISTEN)
docker-pr 33564 root    4u  IPv6 217634      0t0  TCP *:http-alt (LISTEN)
```

我们也可以直接查看nginx的进程

```bash
❯ ps -ef|grep nginx
root       33608   33587  0 02:10 ?        00:00:00 nginx: master process nginx -g daemon off;
message+   33658   33608  0 02:10 ?        00:00:00 nginx: worker process
message+   33659   33608  0 02:10 ?        00:00:00 nginx: worker process
message+   33660   33608  0 02:10 ?        00:00:00 nginx: worker process
```

这里的master进程和worker进程分别为Nginx的主进程和工作进程, master用来管理worker子进程, worker则负责实际的请求, master进程只有一个, worker可以有多个

### 停止服务

如果和我一样是Docker安装启动的, 可以直接通过来停止

```bash
docker stop 容器ID(容器名:标签)
```

非Docker安装可以直接通过命令停止服务

```bash
# 非docker安装
nginx -s [signal] # signal分为quit,stop,reload,reopen-----一般都是stop
```

## 静态站点部署

> 查看nginx安装位置, 我是Docker安装所以还要进入容器内部

```bash
docker exec -it tongye-nginx /bin/bash #进入容器内
```

非Docker安装用户, 直接通过命令查看

```bash
nginx -V #记住要大写
```

我们在输出的信息中查看到 **--conf-path=路径** 这一行, 这个位置和你使用的操作系统以及安装方式有关, 因此各不相同

![](https://pic.imgdb.cn/item/67194355d29ded1a8c391f8b.png)

由于我本地没有写配置文件到容器, 所以conf文件中并能没有内容, 所以在这之前就应该执行

```bash
docker run -d -p 80:8080 -v /path/to/your/nginx.conf:/etc/nginx/nginx.conf:ro nginx
```

- 这条命令会将宿主机上的 `/path/to/your/nginx.conf` 文件挂载到容器的 `/etc/nginx/nginx.conf` 路径，并以只读方式打开。
  
- **确认配置文件语法**：在启动 Nginx 之前，使用 `nginx -t` 命令来测试配置文件的语法是否正确。这可以确保你的配置文件没有语法错误，从而避免 Nginx 启动失败。
  

> 这里就简单讲解一下nginx.conf配置文件的基本内容

```nginx
# 用户和组可以根据您的系统进行更改
user  nginx;
worker_processes  1; # 这里是nginx的进程数

# 错误日志的位置，可以设置为任何有效的路径
error_log  /var/log/nginx/error.log warn;
# 访问日志的位置，可以设置为任何有效的路径
pid        /var/run/nginx.pid;

# 主要是一些服务器和客户端之间网络连接的一些配置
events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    # 日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    # 包含全局的配置文件
    include /etc/nginx/conf.d/*.conf;
    # 包含站点的配置文件
    include /etc/nginx/sites-enabled/*;

    # 俗称虚拟主机块
    server {
        listen       80;
        server_name  localhost;

        # 默认请求的处理
        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }

        # 错误页面的处理
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }

        # 禁止访问 .ht* 文件
        location ~ /\.ht {
            deny  all;
        }
    }
}
```

往下翻我们找到server部分, 然后在里面有一个location, 这个location就是用来匹配我们在浏览器中输入的URL的, 比如这里的斜线/就表示匹配根目录, 也就是说我们只输入localhost后面不跟端口就会匹配到root后面的内容(相对路径目录), 然后就会到它里面指定目录下去找一个index.html的文件, 也就是我们的默认页面

### 怎么把开发好的站点部署在Nginx

> 第一种方法很简单, 直接把网站网页的输出页面比如public文件复制到root的根目录之下就行了

```bash
server {
    listen 80;
    server_name your_domain_or_IP;
    root /path/to/hexo/public;
    index index.html index.htm;
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

修改完成后，重启Nginx以应用配置更改。

```bash
sudo systemctl restart nginx  # CentOS/Debian/Ubuntu
```

请注意，这个过程可能需要根据你的具体服务器环境和配置进行调整。如果你使用的是云服务，可能还需要配置安全组规则以允许HTTP（80端口）和HTTPS（443端口）的流量。此外，如果你的博客托管在自定义域名下，还需要配置DNS记录以指向你的服务器IP地址，并在服务器上配置SSL证书以启用HTTPS

## 反向代理和负载均衡

### 正向代理和反向代理

简单来说 正向代理就是代理客户端, 而反向代理就是代理服务端

**配置反向代理**：

打开 Nginx 的配置文件，通常位于 `/etc/nginx/nginx.conf` 或 `/etc/nginx/sites-available/default`。
  
在 `http` 块中添加一个新的 `server` 块，配置反向代理：
  
```nginx
  server {
      listen 80;
      server_name example.com;
  
      location / {
          proxy_pass http://backend_server;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
  }
```
  
- 这里的 `backend_server` 可以是另一个 Nginx 服务器的地址，或者是你的后端应用服务器的地址。
  

**重启 Nginx**：

保存配置文件后，重启 Nginx 以应用更改：

```bash
sudo systemctl restart nginx
```

### 负载均衡

负载均衡（Load Balancing）是一种将网络流量分发到多个服务器的方法，以优化资源使用、最大化吞吐量、最小化响应时间，并避免任何单点过载。

**配置负载均衡**：

在相同的 `server` 块中，你可以配置多个 `upstream` 服务器：
  
```nginx
  # 反向代理配置代码
  upstream backend {
      ip_hash; #ip哈希策略, 它会根据客户端的IP地址来进行一个哈希,同一个客户端的请求就会被分配到同一个服务器上,这样就解决了一些session相关的问题
      server backend1.example.com weight=3; #可以给服务器增加权重, 以增加请求
      server backend2.example.com;
      server backend3.example.com;
  }
  
  server {
      listen 80;
      server_name example.com;
  
      location / {
          proxy_pass http://backend;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
  }
```
  
这里的 `upstream` 定义了一个服务器组，Nginx 将使用轮询（默认）或其他算法（如最少连接、IP哈希等）来分配请求到这些服务器。
  

**重启 Nginx**：

保存配置文件后，重启 Nginx 以应用更改：

```bash
sudo systemctl restart nginx
```

## HTTPS配置

HTTPS 的基本步骤：

1. **获取 SSL/TLS 证书**：
  
  - 你可以从证书颁发机构（CA）购买证书，或者使用 Let's Encrypt 免费获取证书。
  - 如果使用 Let's Encrypt，你可以使用 Certbot 这样的工具来自动化证书的获取和续签过程。
2. **安装 SSL 模块**：
  
  - 确保 Nginx 编译时包含了 SSL 模块。通常，Nginx 的默认安装已经包含了 SSL 模块。
3. **配置 Nginx**：
  
  - 编辑 Nginx 配置文件，通常位于 `/etc/nginx/nginx.conf` 或 `/etc/nginx/sites-available/` 目录下的某个文件。
  - 在 server 块中添加 SSL 配置。以下是一个基本的示例：

```nginx
server {
    listen 443 ssl; # 监听 443 端口，并启用 SSL
    server_name example.com; # 你的域名

    ssl_certificate /etc/nginx/ssl/example.com.crt; # SSL 证书路径
    ssl_certificate_key /etc/nginx/ssl/example.com.key; # SSL 私钥路径

    # SSL 参数
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout 10m;

    # 推荐的安全配置
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # 使用 TLS 版本
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    # 启用 HSTS (HTTP Strict Transport Security)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    # 配置你的网站内容的位置
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }

    # 重定向 HTTP 到 HTTPS
    server {
        listen 80;
        server_name example.com;
        return 301 https://$server_name$request_uri;
    }
}
```

4. **重载 Nginx 配置**：
  
  - 保存配置文件后，重载 Nginx 以应用更改：

  - ```bash
    sudo nginx -t # 测试配置文件是否有语法错误
    sudo systemctl reload nginx # 重载 Nginx 配置
  ```
    
- **测试 HTTPS 连接**：
  
  - 使用浏览器或 SSL 测试工具（如 SSL Labs 的 SSL Server Test）来测试你的 HTTPS 配置。

## Nginx常用命令

以下是一些常用的 Nginx 命令：

1. **启动 Nginx：**

```bash
sudo service nginx start
```

或者使用 `systemctl`（如果你使用的是使用 `systemd` 的系统）：

- ```bash
  sudo systemctl start nginx
  ```
  
- **停止 Nginx：**
  

```bash
sudo service nginx stop
```

或者使用 `systemctl`：

- ```bash
  sudo systemctl stop nginx
  ```
  
- **重启 Nginx：**
  

```bash
sudo service nginx restart
```

或者使用 `systemctl`：

- ```bash
  sudo systemctl restart nginx
  ```
  
- **重新加载 Nginx 配置文件：**
  

```bash
sudo nginx -s reload
```

或者使用 `systemctl`：

- ```bash
  sudo systemctl reload nginx
  ```
  
- **查看 Nginx 的版本：**
  
- ```bash
  nginx -v
  ```
  
- **查看 Nginx 的配置文件是否正确：**
  
- ```bash
  sudo nginx -t
  ```
  
- **查看 Nginx 的运行状态：**
  

```bash
sudo service nginx status
```

或者使用 `systemctl`：

- ```bash
  sudo systemctl status nginx
  ```
  
- **查看 Nginx 的访问日志：**  
  Nginx 的默认访问日志位置通常在 `/var/log/nginx/access.log`。
  
- **查看 Nginx 的错误日志：**  
  Nginx 的默认错误日志位置通常在 `/var/log/nginx/error.log`。
  
- **查看 Nginx 的进程信息：**
  
- ```bash
  ps aux | grep nginx
  ```
  
- **查看 Nginx 的开放端口：**
  
- ```bash
  sudo netstat -tulnp | grep nginx
  ```
  
- **查看 Nginx 的工作进程数：**
  

1. ```bash
  ps -eLf | grep nginx
  ```
  

这些命令在大多数 Linux 发行版上都是通用的