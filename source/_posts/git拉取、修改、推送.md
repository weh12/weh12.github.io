---
title: git拉取、修改、推送
categories: 教程
tags: git
cover: https://pic.imgdb.cn/item/67178df4d29ded1a8c753815.jpg
---
添加远程库

要添加新的远程仓库，可以指定一个别名，以方便下面命令引用，命令格式如下：

    git remote add [shortname] [git远程仓url]

由于本地的git仓库和github长裤之间的传输是通过SSH加密的，所以我们需要配置验证信息，那么就先生成SSH key，命令如下：

    ssh-keygen -t rsa -C "youremail@xxx.com"

上面的邮箱地址写你在github上注册的邮箱地址，命令执行后会要求你确认路径和输入密码，一路回车。成功的话就会在~/下生成.ssh文件夹，进去打开id_rsa.pub，复制里面所有内容，也就是key。
登录到github上，进入Account Setting,左边选择SSH Keys,Add SSH Key，title随便填，粘贴在你电脑上生成的key。

![这里写图片描述](https://pic.imgdb.cn/item/67178df4d29ded1a8c753815.jpg)

这里写图片描述

看看是否验证成功，命令如下：

    ssh -T git@github.com

![这里写图片描述](https://img-blog.csdn.net/20160726201932428)

这里写图片描述
以上图片显示连接成功。如果Hi后面有用户名表示已经连接github成功了。

回到github上，点击“New repository”新建仓库，如图所示：

![这里写图片描述](https://img-blog.csdn.net/20160726202307338)

这里写图片描述
继续

![这里写图片描述](https://img-blog.csdn.net/20160726202607733)这里写图片描述
创建成功如下：

![这里写图片描述](https://img-blog.csdn.net/20160726202900483)这里写图片描述
上面信息显示，可以从这个仓库克隆出新的仓库，也可以把本地仓库的内容推送到github仓库。
常用命令如下：

    touch test.txt      //新建一个记录提交操作的文档
    git init            //初始化本地仓库
    git add test.txt    //添加
    git add [项目包含的文件]   //添加项目
    git commit -m "first commit"    //提交到远程仓库，描述
    git remote add origin git@github.com:huyida/test.git    //连接远程仓库并建一个别名origin
    git remote -v       //查看当前配置的远程仓库

克隆远程仓库到本地

    1git clone git@github.com:huyida/test.git //克隆到本地

推送到远程仓库

    git init //初始化本地仓库
    git add . //添加当前目录下所有文件包括文件夹
    git commit -m 'This is a xxx' //添加描述
    git remote add origin git@github.com:huyida/test.git //建立远程仓库连接
    git pull origin master //拉取远程仓库
    git push -u origin master //将本地仓库的文件提交到地址是origin的地址，master分支下

删除远程仓库

``````
git remote rm [别名] //删除远程仓库
```
