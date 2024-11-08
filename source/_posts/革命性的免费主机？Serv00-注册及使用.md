---
title: 革命性的免费主机？Serv00 注册及使用
date: 2024-11-08 19:59:22
categories: 教程
tags: 免费主机
cover: https://pic.imgdb.cn/item/672dfef2d29ded1a8c56dcd3.png
---
## 介绍

Serv00 提供完全免费的虚拟主机，并且可以连接 SSH 和运行第三方程序，以下是其配置：

![](https://s2.loli.net/2024/11/08/eyaWE5xQwLKSH3M.png)

serv00在官网上也宣传了这款虚拟主机其他特性:

![](https://s2.loli.net/2024/11/08/pnwJot9KI8gfdMq.png)

Serv00 在其官网上称其是“Revolutionary Free Hosting（革命性的免费主机）”，真不是空口无凭！  
另外 Serv00 的母公司 MyDevil 还有个同样的免费虚拟主机 ct8，与 Serv00 配置相同，注册流程也相同，但是注册需要波兰 IP。地址：[https://www.ct8.pl/](https://www.ct8.pl/)

## 开始白嫖

### 注册帐号

Serv00 的帐号注册十分简单，不需要手机号什么的，一个邮箱就够了。打开 [注册页面](https://www.serv00.com/offer/create_new_account)，按图填写必要的信息：

![](https://s2.loli.net/2024/11/08/H1ZchaY76TINnm8.jpg)

勾选“I accept Terms of Service”，点击 **Create account**，稍等片刻，如果注册成功，你会看到这样的信息：

![](https://pic.imgdb.cn/item/672df8c3d29ded1a8c51d6c3.jpg)

现在，就可以去邮箱里查看你的各种登录信息了。

### 登录控制面板

来到邮箱，找到 Serv00 发来的邮件（没在收件箱里的话，可能在垃圾桶里），长这样：

![](https://img.picui.cn/free/2024/11/08/672df9d787215.jpg)

其中，yourname 为注册时填写的用户名，Password 
后面那段（图中星号部分）即为初始登录密码（SSH、控制面板）。邮件里还有其他信息，比如 MySQL 和 SSH 服务器的地址。DevilWEB 
webpanel 后面那个链接即为控制面板登录链接，比如在图中是 [https://panel2.serv00.com/](https://panel2.serv00.com/)。打开控制面板登录链接，填入邮件里的用户名和密码，点击那个红色的登录按钮，即可登录。

![](https://pic.imgdb.cn/item/672df96bd29ded1a8c523f85.png)

登录成功后，就可进行各项服务的管理。Serv00 用的是他们家自研的 DevilWEB 面板，挺好上手的，不过没有中文，只有英语和波兰语，默认语言貌似是波兰语，可以用浏览器翻译。当然我没用浏览器翻译，我将语言设置成了英语。（同下文）  
**注意：3 个月没登录控制面板或者通过 SSH 连接主机的话，Serv00 就会删掉用户的帐号。**

![](https://pic.imgdb.cn/item/672dfa1cd29ded1a8c52b303.png)

### 更改 PHP 版本

有些程序需要指定的 PHP 版本才能正常运行~~（比如某探针）~~，Serv00 的控制面板里没有直接修改 PHP 版本的选项，不过 Serv00 官方给出了通过 .htaccess 文件修改的方法：在网站根目录创建一个 .htacess 文件，然后填入：

```bash
AddType application/x-httpd-php83 .php
```

这样就将 PHP 版本设置成了 8.3，如果要修改成 7.4 则是：

```bash
AddType application/x-httpd-php74 .php
```

如果要修改命令行的 PHP 版本，可以：

```bash
mkdir -p ~/bin
ln -s /usr/local/bin/php71 ~/bin/php
echo 'export PATH=$HOME/bin:$PATH' >> $HOME/.bash_profile
source $HOME/.bash_profile
```

这样就把 PHP 版本改成了7.1.同理，要将 PHP 版本改成 8.2，将上面命令中的 71 改成 82 即可。

### 允许运行第三方可执行文件

Serv00 
默认设置是不允许运行第三方可执行文件的。为此，需要在控制面板里修改。登录面板，点击“Additional services”，然后点击“Run 
your own applications”，再点击最下面的“Enable”按钮，如果变成这样，就可以了：  
[![设置完成](https://img.picui.cn/free/2024/11/08/672dfb197cb69.jpg)](https://s21.ax1x.com/2024/06/13/pkabTs0.jpg)

## 问题汇总

### 帐号有效期问题

网上有人说 Serv00 账号有效期十年，可能是对下图的误解：

![](https://pic.imgdb.cn/item/672dfbccd29ded1a8c53c603.png)

实际上，Serv00 帐号是永久有效的（直到倒闭）。[Serv00 官方论坛](https://forum.serv00.com/)的管理员的说法：

![](https://pic.imgdb.cn/item/672dfbf5d29ded1a8c53e268.png)

也就是说，每次登录控制面板、通过 SSH 或者 SFTP（不包括 FTP 和 FTPS，因为 FTP/FTPS 帐号是用户自己添加的）连接服务器时，这个“过期日期”都会增加。

### Serv00 免费的原因

按论坛管理员的说法，他们想要展示“it is possible to provide good services for free without any tricks（无需任何技巧即可免费提供优质服务）”。

### IP 被墙及解决办法

刚注册时我网站还好好的，最近发现不科学上网根本打不开，换域名也一样，使用某网站对所在服务器 IP 进行全国 ping 测试的结果如图：

![](https://s2.loli.net/2024/11/08/GWokCEylYvJ9zB4.jpg)

> 很明显是 IP 被墙了。。。  
> 解决方法有以下几种：

**1、用 Serv00 的备用 IP**

ping 一下 cache2.serv00.com（如果是 s1 服务器则是 cache1.serv00.com，其他同理），记下获得的 IP 地址，然后将域名解析到这个 IP ，当然也可以直接 cname 解析。 这是我不久前才发现并且目前在用的方法。  

**2、使用 FRP。**

前提是有一台拥有未被墙公网 IP 的服务器并且运行了 FRP 服务端，第三方的也行。这是我之前用过的方法。  

**3、套 CF（CloudFlare）的 CDN。**

优点是简单快速，缺点是不能用多级域名（如 domain1.test.example.com），因为 CF 的 SSL 证书不支持。。。  

**4、反代。**

和第二条一样需要别的有未被墙公网 IP 的服务器。  

**5、科学上网。**

这个不必多说。

## serv00几个提升体现的配置

![](https://pic.imgdb.cn/item/672dff22d29ded1a8c570606.png)

[SERV00注册必备的1个关键设置和3个必备工具配置好环境后准备起飞](https://youtu.be/3JIQUa7vi6Q)