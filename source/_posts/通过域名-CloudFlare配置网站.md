---
title: 通过域名+CloudFlare配置网站
date: 2024-10-29 03:57:12
categories: 教程
tags: 网站服务
cover: https://pic.imgdb.cn/item/671fed62d29ded1a8c907d11.webp
---
前言

对于国内大陆用户而言，想要直接访问用 github
托管的个人博客不是件容易的事，为了解决网络不可达问题，可以采用 [CDN(content
deliverynetwork)](https://www.cloudflare.com/zh-hans-cn/learning/cdn/what-is-a-cdn/)将网页内容分发到全球各地的服务器上，同时还能缩短网站加载时间。

![](https://pic.imgdb.cn/item/671fdf39d29ded1a8c889d66.png)

但是大部分国内的CDN服务都是收费的，所以就把目光转向了cloudflare。cloudflare 作为全球最大的网络服务提供商，提供免费的cdn服务，虽然 cdn节点都在国外，但还是比直接访问github.io要快的多，不过免费版请求次数限制有10w次的限制，但对于我们博客而言是绰绰有余了，下面介绍配置过程。

## 准备

使用 cloudflare 的 cdn服务需要我们拥有一个可配置的域名，所以需要先购买一个域名，本人是在腾讯云上购买的`.space`的后缀的域名，10年价格也只要一百多，还是很便宜的，购买域名的教程就跳过了。我个人配置的是namesilo的域名。

## 教程

> ps: 教程中的qinyu.space都是其他人的域名，所以在以下内容中仅为参考。

### 配置 cloudflare

- 进入[https://www.cloudflare-cn.com/](https://www.cloudflare-cn.com/)，注册账号并登录
  
- 在左侧栏中进入`网站`一栏，点击右方`添加站点`
  

![](https://pic.imgdb.cn/item/671fe118d29ded1a8c89cfe1.png)

- 输入自己的域名，**注意不要带`www`或者`https`**，比如我的就直接填写`qinyu.space`
  

![](https://pic.imgdb.cn/item/671fe17ed29ded1a8c8a0f17.png)

- 选择套餐,free即可
  

![](https://pic.imgdb.cn/item/671fe1c6d29ded1a8c8a31f1.png)

- 点击继续后 cloudflare 会自动扫描域名的 dns
  记录，如果是刚刚创建的域名，可能扫描的结果为空。截图中的几条记录可以不用管
  

![](https://pic.imgdb.cn/item/671fe3e4d29ded1a8c8b4bd0.png)

- 这一步很重要，点击添加记录，按照如下方式添加类型为A，名称为@，IPv4地址为`185.199.108.153`等四条记录分别添加
  

![](https://pic.imgdb.cn/item/671fe443d29ded1a8c8b8083.png)

一共是四条记录, 这个是github源IP地址, 具体可查看

【[https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site]()】

```bash
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

- 点击继续后，cloudflare会要求将我们DNS服务器修改为以下图中所示的的服务器，可以先截个图或者存文档里：
  

![](https://pic.imgdb.cn/item/671fe539d29ded1a8c8c0eff.png)

- 点击下方继续后会有一个快速入门指南，里面的配置可以都开启：
  

![](https://pic.imgdb.cn/item/671fe55cd29ded1a8c8c235d.png)

### 修改DNS服务器

进入腾讯云控制台，修改的DNS服务器为3.1 第7步中 cloudflare提供的DNS服务器，如下所示：

![](https://pic.imgdb.cn/item/671fe5cbd29ded1a8c8c6f96.png)

DNS服务器更改后生效需要一段时间，少则几分钟，慢则需要几个小时

过一段时间可以看到 DNS服务器已经修改成功了

![](https://pic.imgdb.cn/item/671fe652d29ded1a8c8cc059.png)

返回 cloudflare，如果看到 Cloudflare正在保护您的站点, 说明已经配置成功了：

![](https://pic.imgdb.cn/item/671fe684d29ded1a8c8ce087.png)

### 设置Github page

进入github.io对应的仓库，进入 `Settings`：

![](https://pic.imgdb.cn/item/671fe762d29ded1a8c8d5403.png)

进入左栏中的`pages`，在 `Custom domain`中输入自己的域名，点击`save`，如果成功会显示下图：

![](https://pic.imgdb.cn/item/671fe794d29ded1a8c8d6c2f.png)

这样就可以通过域名来访问自己的博客了，还可以在上图中勾选 `Enforcrs HTTPS`，这样网站仅会通过https提供服务。

> 如果如下图显示dns配置不正确，推测可能是使用了cloudflare后，GitHub验证DNS时返回的是cdn服务器的ip地址，而不是在cloudflare上开始配置的4个GitHub
> page的ip地址，可以在线dig一下自己的域名验证一下。不过只要网站能通过域名正常访问就没什么问题。

### 配置HTTPS和SSL证书

在cloudflare网站中的SSL配置中找到边缘服务器配置, 找到始终使用HTTPS

![](https://pic.imgdb.cn/item/671fe897d29ded1a8c8dfa7e.png)

在SSL/TLS概述里设置加密模式

![](https://pic.imgdb.cn/item/671febddd29ded1a8c8f7034.png)

> 至于SSL证书, 有大佬给出了答案
- 大佬文章链接：https://blog.fjy.zone/archives/http-ca-cf-free

<iframe id="video" width="100%" src="//player.bilibili.com/player.html?isOutside=true&aid=537567880&bvid=BV1wi4y1Y7go&cid=1375872812&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
<script type="text/javascript">document.getElementById("video").style.height=document.getElementById("video").scrollWidth*0.8+"px"</script>
