---
title: Linux入门学习
date: 2024-10-17 00:26:59
categories: 程序知识
tags: Linux
cover: https://pic.imgdb.cn/item/670fea8ad29ded1a8c73263a.png
---
## linux介绍

Linux 是一个开源的类 Unix 操作系统内核，由 Linus Torvalds 
在1991年首次发布。它遵循自由软件和开源开发的原则，任何人都可以自由地使用、修改和分发 Linux 内核。Linux 
内核是许多流行操作系统的核心，这些操作系统统称为 Linux 发行版。

以下是 Linux 系统的一些关键特点：

1. **开源**：Linux 内核的源代码是公开的，任何人都可以查看、修改和重新分发。
  
2. **多用户多任务**：Linux 支持多用户同时使用系统，并且可以同时运行多个任务。
  
3. **稳定性和安全性**：Linux 以其高稳定性和安全性而闻名，适合服务器和关键任务应用。
  
4. **灵活性**：Linux 提供了高度的定制性，用户可以根据自己的需要配置系统。
  
5. **跨平台**：Linux 可以在多种硬件平台上运行，从嵌入式系统到超级计算机。
  
6. **丰富的软件生态**：有大量的开源软件可供选择，涵盖了从办公软件到开发工具的各个方面。
  
7. **社区支持**：Linux 有一个活跃的社区，用户可以从中获得帮助和支持。
  
8. **免费**：Linux 内核本身是免费的，许多 Linux 发行版也是免费的。
  
9. **命令行界面**：虽然大多数现代 Linux 发行版都提供了图形用户界面（GUI），但命令行界面（CLI）仍然是一个强大的工具，用于系统管理和自动化任务。
  
10. **文件系统支持**：Linux 支持多种文件系统，包括其自己的 ext4，以及其他如 NTFS、FAT32、XFS 等。
  

Linux 发行版有很多，包括但不限于：

- **Ubuntu**：用户友好，适合初学者。
- **Debian**：稳定，是许多其他发行版的基础。
- **Fedora**：注重最新技术，适合开发者和早期采用者。
- **CentOS**：企业级，注重稳定性。
- **Arch Linux**：滚动发布，高度可定制。
- **openSUSE**：适合开发者和系统管理员。
- **Red Hat Enterprise Linux (RHEL)**：企业级解决方案，需要订阅。

Linux 系统广泛应用于服务器、桌面、移动设备、嵌入式系统和超级计算机等领域。

## 常用虚拟工具

![](https://pic.imgdb.cn/item/670fc7a9d29ded1a8c53fe45.png)

## 常用命令

### 一、文件和命令

#### 1、cd 命令

（它用于切换当前目录，它的参数是要切换到的目录的路径，可以是绝对路径，也可以是相对路径）

```
cd /home 进入 ‘/ home’ 目录

cd .. 返回上一级目录

cd ../.. 返回上两级目录

cd / 返回跟目录

cd - 返回上次所在的目录

mkdir <目录名> 创建目录

mkdir dir1 dir2 同时创建两个目录

mkdir -p /tmp/dir1/dir2 递归创建目录树

rm -f file1 删除’file1’⽂件

rmdir dir1 删除’dir1’⽬录

rm -rf dir1 删除’dir1’⽬录和其内容
-rm -rf dir1 dir2 同时删除两个⽬录及其内容
```

#### 2、pwd 命令

pwd 显示工作路径

#### 3、ls 命令

```
ls 查看目录中的文件
ls -l 显示文件和目录的详细资料
ls -a 列出全部文件，包含隐藏文件
ls -lh 查看⽂件和⽬录的详情列表（增强⽂件⼤⼩易读性）
ls -lSr 查看⽂件和⽬录列表（以⽂件⼤⼩升序查看）
tree 查看⽂件和⽬录的树形结构 （如果没有需要先安装 yum install tree）
ls -R 连同子目录的内容一起列出（递归列出），等于该目录下的所有文件都会显示出来
ls -al /proc/pid/exe 通过pid查询程序正在运行的路径
```

#### 4、cp 命令

（用于复制文件，copy之意，它还可以把多个文件一次性地复制到一个目录下）

```
-a 将文件的特性一起复制
-p 连同文件的属性一起复制，而非使用默认方式，与-a相似，常用于备份
-i 若目标文件已经存在时，在覆盖时会先询问操作的进行
-r 递归持续复制，用于目录的复制行为
-u 目标文件与源文件有差异时才会复制
-cp dir/* . 复制某目录下的所有文件至当前目录
cp -a dir1 dir2 复制目录
cp -a /temp/dir1 . 复制一个目录至当前目录
ln -s file1 link1 创建指向⽂件/⽬录的软链接
ln file1 lnk1 创建指向⽂件/⽬录的物理链接
touch -t 0712250000 file1 修改一个文件或目录的时间戳 - (YYMMDDhhmm)
```

#### 5、mv 命令

```
-f force强制的意思，如果目标文件已经存在，不会询问而直接覆盖
-i 若目标文件已经存在，就会询问是否覆盖
-u 若目标文件已经存在，且比目标文件新，才会更新
mv old_dir new_dir 重命名/移动⽬录
```

#### 6、rm 命令

```
-f ：就是force的意思，忽略不存在的文件，不会出现警告消息
-i ：互动模式，在删除前会询问用户是否操作
-r ：递归删除，最常用于目录删除，它是一个非常危险的参数
```

### 二、查看文件内容

#### 1、cat 命令

（用于查看文本文件的内容，后接要查看的文件名，通常可用管道与 more 和 less 一起使用）

```
cat file1 从第一个字节开始正向查看文件的内容
cat -n file1 标示文件的行数
cat xxx.txt awk ‘NR%2==1’
tac file1 从最后一行开始反向查看一个文件的内容
more file1 查看一个长文件的内容
less file1 类似 more 命令，但允许方向操作
head -n 2 file1 查看一个文件的前两行
tail -f /log/msg 实时查看添加到⽂件中的内容
tail -n 2 file1 查看一个文件的最后两行
tail -n +1000 file1 从1000行开始显示，显示1000行以后的
cat filename | head -n 3000 | tail -n +1000 显示1000行到3000行
cat filename | tail -n +3000 | head -n 1000 从第3000行开始，显示1000(即显示3000~3999行)
grep ss hello.txt 在⽂件hello.txt中查找关键词 ss
grep ^s hello.txt 在⽂件hello.txt中查找以 s 开头的内容
grep [0-9] hello.txt 选择hello.txt⽂件中所有包含数字的⾏
sed 's/ss/mm/g' hello.txt 将hello.txt⽂件中的 ss 替换成 mm
sed '/^$/d' hello.txt 从hello.txt⽂件中删除所有空⽩⾏
sed '/ *#/d; /^$/d' hello.txt 从hello.txt⽂件中删除所有注释和空⽩⾏
sed -e '1d' hello.txt 从⽂件hello.txt 中排除第⼀⾏
sed -n '/s1/p' hello.txt 查看只包含关键词"s1"的⾏
sed -e 's/ *$//' hello.txt 删除每⼀⾏最后的空⽩字符
sed -e 's/s1//g' hello.txt 从⽂档中只删除词汇s1并保留剩余全部
sed -n '1,5p;5q' hello.txt 查看从第⼀⾏到第5⾏内容
sed -n '5p;5q' hello.txt 查看第5⾏
paste file1 file2 合并两个⽂件或两栏的内容
paste -d '+' file1 file2 合并两个⽂件或两栏的内容，中间⽤"+"区分
sort file1 file2 排序两个⽂件的内容
sort file1 file2 uniq
sort file1 file2 uniq -u
sort file1 file2 uniq -d
comm -1 file1 file2 ⽐较两个⽂件的内容(去除’file1’所含内容)
comm -2 file1 file2 ⽐较两个⽂件的内容(去除’file2’所含内容)
comm -3 file1 file2 ⽐较两个⽂件的内容(去除两⽂件共有部分)
```

### 三、文件搜索

#### 1、find 命令

```
find / -name file 从根目录开始搜索文件/目录
find / -user user1 搜索用户 user1 的文件/目录
find /dir -name *.bin 在目录/dir 中搜索带有 .bin 后缀的文件
find / -name file1 从 ‘/’ 开始进入根文件系统搜索文件和目录（完整文件或文件名）
find / -user user1 搜索属于用户 ‘user1’ 的文件和目录
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件
find . -regex '.*\(net\|comm\).*' ‘-regex’ 选项匹配整个路径名，出当前目录树中所有文件名中任意位置包含字符串 net 或 comm 的文件
locate *.mp4 寻找 .mp4结尾的文件
whereis <关键词> 显示某⼆进制⽂件/可执⾏⽂件的路径
whereis halt 显示一个二进制文件、源码或man的位置
which <关键词> 查找系统⽬录下某的⼆进制⽂件
which halt 显示一个二进制文件或可执行文件的完整路径
```

### 四、文件的权限 - 使用 “+” 设置权限，使用 “-” 用于取消

#### 1、chmod 命令

```
ls -lh 显示当前目录所有文件的权限
chmod 777 文件名 修改文件权限（最高权限）
chmod ugo+rwx dir 设置目录的所有人(u)、群组(g)以及其他人(o)以读（r，4 ）、写(w，2)和执行(x，1)的权限
chmod go-rwx dir1 删除群组(g)与其他人(o)对目录的读写执行权限
chmod u+s /bin/file1 设置一个二进制文件的 SUID 位 - 运行该文件的用户也被赋予和所有者同样的权限
chmod u-s /bin/file1` 禁用一个二进制文件的 SUID位
chmod g+s /home/public 设置一个目录的SGID 位 - 类似SUID ，不过这是针对目录的
chmod g-s /home/public 禁用一个目录的 SGID 位
chmod o+t /home/public 设置一个文件的 STIKY 位 - 只允许合法所有人删除文件
chmod o-t /home/public 禁用一个目录的 STIKY 位
chmod +x 文件路径 为所有者、所属组和其他用户添加执行的权限
chmod -x 文件路径 为所有者、所属组和其他用户删除执行的权限
chmod u+x 文件路径 为所有者添加执行的权限
chmod g+x 文件路径 为所属组添加执行的权限
chmod o+x 文件路径 为其他用户添加执行的权限
chmod ug+x 文件路径 为所有者、所属组添加执行的权限
chmod =wx 文件路径 为所有者、所属组和其他用户添加写、执行的权限，取消读权限
chmod ug=wx 文件路径 为所有者、所属组添加写、执行的权限，取消读权限
```

#### 2、chown 命令

（改变文件的所有者）

```
chown user1 file1 改变一个文件的所有人属性
chown -R user1 dir1 改变一个目录的所有人属性并同时改变改目录下所有文件的属性
chown user1:group1 file1 改变一个文件的所有人和群组属性
```

#### 3、chgrp 命令

（改变文件所属用户组）

```
chgrp group1 file1 改变文件的群组
```

### 五、文本处理

#### 1、grep 命令

（分析一行的信息，若当中有我们所需要的信息，就将该行显示出来，该命令通常与管道命令一起使用，用于对一些命令的输出进行筛选加工等等）

```
grep Aug /var/log/messages 在文件 '/var/log/messages’中查找关键词"Aug"
grep ^Aug /var/log/messages 在文件 '/var/log/messages’中查找以"Aug"开始的词汇
grep [0-9] /var/log/messages 选择 ‘/var/log/messages’ 文件中所有包含数字的行
grep Aug -R /var/log/* 在目录 ‘/var/log’ 及随后的目录中搜索字符串"Aug"
sed 's/stringa1/stringa2/g' example.txt 将example.txt文件中的 “string1” 替换成 “string2”
sed '/^$/d' example.txt 从example.txt文件中删除所有空白行
```

#### 2、paste 命令

```
paste file1 file2 合并两个文件或两栏的内容（查看两文件合并后的内容）
paste -d '+' file1 file2 合并两个文件或两栏的内容，中间用"+"区分
```

#### 3、sort 命令

```
sort file1 file2 排序两个文件的内容
sort file1 file2 | uniq 取出两个文件的并集(重复的行只保留一份)
sort file1 file2 | uniq -u 删除交集，留下其他的行
sort file1 file2 | uniq -d 取出两个文件的交集(只留下同时存在于两个文件中的文件)
```

#### 4、comm 命令

```
comm -1 file1 file2 比较两个文件的内容只删除 ‘file1’ 所包含的内容
comm -2 file1 file2 比较两个文件的内容只删除 ‘file2’ 所包含的内容
comm -3 file1 file2 比较两个文件的内容只删除两个文件共有的部
```

### 六、打包和压缩文件

#### 1、tar 命令

对文件进行打包，默认情况并不会压缩，如果指定了相应的参数，它还会调用相应的压缩程序（如gzip和bzip等）进行压缩和解压）推荐

```
-c ：新建打包文件
-t ：查看打包文件的内容含有哪些文件名
-x ：解打包或解压缩的功能，可以搭配-C（大写）指定解压的目录，注意-c,-t,-x不能同时出现在同一条命令中
-j ：通过bzip2的支持进行压缩/解压缩
-z ：通过gzip的支持进行压缩/解压缩
-v ：在压缩/解压缩过程中，将正在处理的文件名显示出来
-f filename ：filename为要处理的文件
-C dir ：指定压缩/解压缩的目录dir
压缩：tar -jcv -f filename.tar.bz2 要被处理的文件或目录名称
查询：tar -jtv -f filename.tar.bz2
解压：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录
bunzip2 file1.bz2 解压一个叫做 'file1.bz2’的文件
bzip2 file1 压缩一个叫做 ‘file1’ 的文件
gunzip file1.gz 解压一个叫做 'file1.gz’的文件
gzip file1 压缩一个叫做 'file1’的文件
gzip -9 file1 最大程度压缩
rar a file1.rar test_file 创建一个叫做 ‘file1.rar’ 的包
rar a file1.rar file1 file2 dir1 同时压缩 ‘file1’, ‘file2’ 以及目录 ‘dir1’
rar x file1.rar 解压rar包
zip file1.zip file1 创建一个zip格式的压缩包
unzip file1.zip 解压一个zip格式压缩包
zip -r file1.zip file1 file2 dir1 将几个文件和目录同时压缩成一个zip格式的压缩包
```

### 七、进程相关的命令

#### 1、ps 命令

用于将某个时间点的进程运行情况选取下来并输出，process之意

```
-A ：所有的进程均显示出来
-a ：不与terminal有关的所有进程
-u ：有效用户的相关进程
-x ：一般与a参数一起使用，可列出较完整的信息
-l ：较长，较详细地将PID的信息列出
```

---

```
ps -ef # 显示所有进程的详细信息。
ps aux # 查看系统所有的进程数据
ps ax # 查看不与terminal有关的所有进程
ps -lA # 查看系统所有的进程数据
ps axjf # 查看连同一部分进程树状态
pstree -aup # 查看正在运行的树桩结构显示
netstat -lntp # 查看各个节点及进程和使用的端口号
```

#### 2、kill 命令

```
kill -9 pid （-9表示强制关闭）
kill -9 程序的名字
kill -
pkill 程序的名字
```

#### 3、Vim 下复制粘贴等操作

```
x,X : 在一行中，x为向后删除一个字符（相当于del键），X为向前删除一个字符（相当于backspace键）

dd : 删除光标所在的那一整行

ndd : n 为数字。从光标开始，删除向下n列

yy : 复制光标所在的那一行

nyy : n为数字。复制光标所在的向下n行

p,P : p 为将已复制的数据粘贴到光标的下一行，P则为贴在光标的上一行

u : 复原前一个操作

CTRL + r : 重做上一个操作

小数点 ‘.’：重复前一个动作

:set number :在每一行设置行标号

:n1,n2 m n3 移动n1-n2行(包括n1,n2)到n3行之下

:n1,n2 co n3 复制n1-n2行(包括n1,n2)到n3行之下

:n1,n2 d 删除n1-n2行(包括n1,n2)行
```

### 八、系统常用命令

#### 1、关机、注销、重启

```
查看进程端口号：netstat -tunlp|grep 端口号 
ss -tnl  查看正在已使用的端口
shutdown -h now 关闭系统(1) 即刻关机
shutdown -h 10 10分钟后关机
shutdown -h 11:00  11:00 关机
shutdown -h +10 预定时间关机（10分钟后关机）
shutdown -c  取消指定时间关机
shutdown -f now 重启
shutdown -r 10  10分钟后重启
shutdown -r 11:00   定时重启
reboot 重启
init 6 重启
init 0 即刻关机
telinit 0 关机
poweroff   立刻关机
halt 关机
sync  buff数据同步到磁盘
logout 退出登录Shell
time 测算一个命令（即程序）的执行时间
```

#### 2、系统信息和性能查看

```
# 查看系统的详细信息
lsb_release -a
# 查看内核/OS/CPU信息
uname -a 
# 查看内核版本
uname -r
# 查看处理器架构
uname -m
# 查看处理器架构
arch
# 查看主机名称
hostname
# 显示当前登录系统的用户
who
# 显示登陆时的用户名
who am i
# 显示当前用户名
whoami
# 查看 linux 版本信息
cat /proc/version
# 查看 CPU 信息
cat /proc/cpuinfo
# 查看中断
cat /proc/interrupts
# 查看系统负载
cat /proc/loadavg
# 查看系统运行时间、用户数、负载
uptime
# 查看系统的环境便令
env
# 查看系统PCI设备信息
lspci -tv
# 查看已加载的系统模块
lsmod
# 查看内存总量
grep MemTotal /proc/meminfo
# 查看空闲内存量
grep MemFree /proc/meminfo
# 查看内存用量和交换区用量
free -m
# 显示系统时间
date
# 显示2021日历表
cal 2021
# 动态显示cpu/内存/进程情况
top
# 每1秒采一次系统状态，采20次
vmstat 1 20 
# 查看io读写/cpu使用情况
iostat
# 查看 cpu 使用情况（1秒1次，共10次）
sar -u 1 10
# 查询磁盘性能
sar -d 1 10

# 找出占用内存资源最多的前 10 个进程
ps -auxf | sort -nr -k 4 | head -10
# 找出占用 CPU 资源最多的前 10 个进程
ps -auxf | sort -nr -k 3 | head -10
# 查看 cpu 内存占用情况
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head
```

#### 3、磁盘和分区

```
# 查看所属有磁盘分区
fdisk -l
# 查看所有交换分区
swapon -s
# 查看磁盘使用情况及挂载点
df -h
# 查看磁盘使用情况及挂载点
df -hl
# 查看指定某个目录大小
du -sh /dir
# 从高到底依次显示文件和目录大小
du -sk * | sort -rn
# 查看内存
free -h
# 查看CPUs
cat /proc/cpuinfo


# 挂在hda2盘
mount /dev/hda2 /mnt/hda2
# 指定⽂件系统类型挂载（如ntfs）
mount -t ntfs /dev/sdc1 /mnt/usbhd1
# 挂载iso⽂件
mount -o loop xxx.iso /mnt/cdrom
# 挂载usb盘/闪存设备
mount /dev/sda1 /mnt/usbdisk
# 通过设备名卸载
umount -v /dev/sda1
# 通过挂载点卸载
umount -v /mnt/mymnt
# 强制卸载(慎⽤)
fuser -km /mnt/hda1
```

#### 4、用户和用户组

```
# 创建用户
useradd ss
# 查看所用系统用户
cut -d: -f1 /etc/passwd
# 删除用户
userdel -r ss
# 创建用户组
groupadd group_name
# 查看系统所有组
cut -d: -f1 /etc/group
# 删除用户组
groupdel group_name
# 修改用户的组
usermod -g group_name user_name
# 将用户添加到组
usermod -aG group_name user_name
# 修改用户 ss 的登录 Shell、主目录及用户组
usermod -s /bin/ksh -d /home/codepig –g dev ss
# 查看 ss 用户所在的组
groups ss
# 切换到另一个用户环境
su user_name
# 修改口令
passwd
# 修改用户密码
passwd ss
# 查看用户活动
w
# 查看指定用户 ss 的信息
id ss
# 查看用户登录日志
last
# 查看当前用户的计划任务
crontab -l
```

#### 5、网络和进程管理

```
# 查看网络接口属性
ifconfig
# 查看某网卡的配置
ifconfig eth0
# 查看路由表
route -n
# 查看所有监听端⼝
netstat -lntp
# 查看已经建立连接的TCP连接
netstat -antp
# 查看TCP/UDP的状态信息
netstat -lutp
# 启⽤eth0⽹络设备
ifup eth0
# 禁⽤eth0⽹络设备
ifdown eth0
# 查看iptables规则
iptables -L
# 配置ip地址
ifconfig eth0 192.168.1.1 netmask 255.255.255.0
# 以dhcp模式启⽤eth0
dhclient eth0
# 配置默认⽹关
route add -net 0/0 gw Gateway_IP
# 配置静态路由到达⽹络'192.168.0.0/16'
route add -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.1.1
# 删除静态路由
route del 0/0 gw Gateway_IP
# 查看主机名
hostname
# 解析主机名
host 主机名  例如：host www.baidu.com
# 查询DNS记录，查看域名解析是否正常
nslookup 主机名  例如：nslookup wwww.baidu.com
# 查看所有进程
ps -ef
# 过滤出你需要的进程
ps -ef|grep redis
# kill指定名称的进程
kill -s name
# kill指定pid的进程
kill -s pid
```

#### 6、查看文件大小

```
# 查看当前目录总大小
du -sh
# 查看当前目录所有子目录大小
du -sh *
# 查看当前目录和所有子目录大小，最后一行会显示当前目录的总大小，不包括隐藏文件
du -ach *
du -h –max-depth=0 *
# 指定文件夹显示层次深度
du -h --max-depth=0
```

#### 7、查看开机自启服务

```
# 查看有哪些自启服务
systemctl list-unit-files --type service | grep enabled
# 查看服务的开机启动状态
systemctl list-unit-files --type service |grep service_name
# 启动（关闭，重启，查看）某个服务
# centos6
service service_name （start|stop|restart|status）
# centos7
systemctl (start|stop|restart|status) service_name

# 设置开机启动或者关闭某个服务
# centos6
开机启动：chkconfig --add service_name  或者  chkconfig  service_name  on
开机关闭：chkconfig  --del   service_name  或者  chkconfig  service_name  off
# centos7
开机启动：systemctl enable service_name
开机关闭：systemctl disable service_name
```

### linux文件结构

![](https://pic.imgdb.cn/item/670fde2dd29ded1a8c67f04e.png)