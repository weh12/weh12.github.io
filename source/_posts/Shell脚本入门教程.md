---
title: Shell脚本入门教程
date: 2024-10-20 19:20:52
categories: 教程
tags: Shell
cover: https://pic.imgdb.cn/item/6714e741d29ded1a8ccf5582.jpg
---
## Shell介绍

![](https://pic.imgdb.cn/item/6714b86ad29ded1a8c79e9eb.png)

Shell 是一个命令行解释器，它提供了一个用户界面，用户可以通过它来与操作系统交互。在计算机科学中，Shell 是一个程序，它接收用户输入的命令，并将这些命令转换成操作系统能够理解的系统调用。以下是关于 Shell 的一些基本介绍：

1. **命令行界面（CLI）**：Shell 提供了一个命令行界面，用户可以通过输入文本命令来执行操作，与图形用户界面（GUI）相对。
  
2. **脚本编写**：用户可以编写 Shell 脚本来自动化任务，这些脚本可以重复执行相同的命令序列。
  
3. **环境变量**：Shell 允许用户设置和使用环境变量，这些变量可以被操作系统和运行在该系统上的程序使用。
  
4. **管道和重定向**：Shell 支持管道（|）操作，允许将一个命令的输出作为另一个命令的输入。同时，它也支持输入/输出重定向，可以将命令的输出重定向到文件或从文件中读取输入。
  
5. **通配符**：Shell 支持通配符，如 `*` 和 `?`，允许用户对文件名进行模式匹配。
  
6. **内置命令**：Shell 提供了一些内置命令，如 `cd`（改变目录）、`ls`（列出目录内容）、`pwd`（打印工作目录）等。
  
7. **程序调用**：用户可以通过 Shell 调用其他程序，例如编译器、文本编辑器等。
  
8. **多用户支持**：Shell 支持多用户操作，不同的用户可以登录并使用 Shell 执行自己的任务。
  
9. **交互式和非交互式**：Shell 可以以交互式模式运行，用户可以实时输入命令；也可以以非交互式模式运行，如在脚本中执行一系列命令。
  
10. **不同的Shell**：存在多种不同的 Shell，如 Bourne Shell（sh）、Bourne Again Shell（bash）、C Shell（csh）、Korn Shell（ksh）、Z Shell（zsh）等，每种 Shell 都有自己的特点和扩展。
  

Shell 是一个强大的工具，对于系统管理员和开发者来说尤其重要，因为它提供了对操作系统的直接控制。

> 常见的个种类型shell

![](https://pic.imgdb.cn/item/6714b8bfd29ded1a8c7abc42.png)

微软也有自己的命令提示符以及Windows用户经常用的**PowerShell**等等, 不同的系统种Shell的版本之间会存在差异化, 但是大部份命令都是通用的, Linux和Mac系统中默认安装的都是Bash

## Shell基本操作

### 查看系统中Shell的版本

打开bash可以通过

```bash
cat /etc/shells
```

查看系统中记录的所有Shell版本, 也可以通过系统环境变量来确定自己用的是哪个版本Shell

在Linux中有很多环境变量用来存储系统的配置信息, 比如:

```bash
#用来存储家目录
echo $HOME 
#用来存储系统的查找路径
echo $PATH
#用来查找但前默认使用的Shell路径
echo $SHELL
#当前正在执行的脚本名称
echo $0
```

### Shell脚本操作

Shell命令通过简单的命令交互起来非常方便, 但是如果要执行一些复杂的操作, 或者需要重复执行一些命令, 这样简单操作命令就显得比较麻烦了

比如, 如何自动执行多项操作和定时清理日志文件；等等诸如此类的复杂操作

这个时候我们就可以把想要执行的命令写入到一个文件中, 通过执行文件来一次性执行多个命令和操作, 这个文件就是一个**Shell脚本**(Shell Script)来执行一些自动化任务, 这样我们的系统维护和安装软件就会简化很多操作

#### 编写简单Shell脚本文件

Shell文件的扩展名一般以 **.sh** 结尾, 这只是一种约定俗成的规范, 其实Shell脚本文件后缀可以是任何名字, 但一般还是建议使用默认的名字规范

Shell脚本的第一行一般都是以 **#!** 开始, 后面加上一个/bin/bash用来表示这个脚本用的是Bash解释器, 系统会自动调用你选择的解释器执行文件

下面是简单的一个Shell脚本:

```bash
#!/bin/bash

echo "Hello Shell"
date
whoami
```

保存文件之后记得给权限

```bash
chmod a+x hello.sh
```

一般通过 **./+脚本名.sh** 或者 **sh 脚本名.sh** 就能执行脚本

下面我们通过ChatGPT来生成一个Shell脚本实例:

> 允许用户输入一个数字并根据奇偶性进行分类

```bash
#!/bin/bash

# 这个函数用于检查传入的数字是奇数还是偶数。
check_number_parity() {
    # 将传入的参数（假设为第一个命令行参数）赋值给变量 num。
    local num=$1

    # 检查是否有命令行参数传递，并且至少需要一个参数。
    if [ $num -lt 2 ]; then
        echo "请提供一个正整数作为参数。"
        exit 1
    fi
    
    # 使用模运算判断是否为偶数
    if [ $((num % 2)) -eq 0 ]; then
        # 偶数
        echo $num"是偶数"
    else
        # 奇数
        echo $num"是奇数"
    fi
}

# 主程序入口点，用于接收用户输入并调用检查函数。
main() {
    # 用户输入
    read -p "请输入一个整数值:" input

    # 将第一个参数赋值给 num 变量，然后调用检查函数。
    check_number_parity "$input"
}

# 确保脚本作为主入口点时执行。
main "$@"
```

解释：

1. **程序开始**：脚本使用 `#!/bin/bash` 作为首行，指明使用 Bash 脚本来解释此文件。
  
2. **定义函数**：`check_parity` 函数接收一个参数（数字），通过检查余数来判断其是否为偶数或奇数，并输出相应的信息。
  
3. **主程序 `main()`**: 此函数用于处理用户输入。它首先提示用户输入一个整数，读取用户的输入到变量 `user_input`。然后验证输入是否为有效的整数（使用正则表达式进行检查）。如果输入无效或不是数字，则输出错误信息并退出脚本。如果输入有效，则调用 `check_parity()` 函数处理数字的奇偶性。
  
4. **程序入口点**：通过 `if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then` 判断是否作为主脚本来执行（即，当脚本被直接运行时），以确保函数 `main()` 只在脚本作为启动点时运行。
  
5. **结束**：脚本的执行在 `main()` 函数内部完成。如果用户输入有效且正确，它会调用并显示奇偶性判断的结果。
  

> 1.注意这里的 **local** 不同于其他编程语言的变量命名, 它只是一个局部变量, **[ ]** 的中间必须要有空格, 不然会报错
> 
> 2.**-eq** 表示equal, 也就是相等的意思, 反之, **-ne** 则表示不等于
> 
> 3.**read** 命令的作用是交互式的读取用户的输入, **$1** 表示用户输入的第一个参数
> 
> 4.当我们在输出变量报错是, 如果是变量命名错误, 这时候可以添加命令替换语法来包裹整个语句, 一般都使用 **$()**

<mark>记住Shell脚本中的一些特殊变量</mark>

![](https://pic.imgdb.cn/item/6714d73dd29ded1a8cb0f5f1.png)

> 在Shell脚本中如果需要永久保存自定义变量
> 
> 可以通过 **export +变量=赋值** 来定义环境变量, 存入 **.profile**或者 **.bashrc**文件
> 
> <mark>PS:注意存入系统文件中的变量需要重新执行读取操作, 新脚本才会生效, 所以需要再执行source .bashrc(.profile)或者. .bashrc(.profile)</mark>

#### Shell中的if条件判断

Shell命令中有自己的if条件判断格式, 下面列出来if的基本语法:

![](https://pic.imgdb.cn/item/6714ddd0d29ded1a8cc0795f.png)

#### Shell中的循环操作

循环是一种常用的控制结构，用于重复执行一组命令。以下是几种常见的循环类型及其用法：

##### 1. `for`循环

`for`循环用于遍历一个列表或序列。

基本用法:

```bash
for variable in item1 item2 item3 ...
do
    commands
done
```

**示例：**

```bash
for i in 1 2 3 4 5
do
    echo "Number $i"
done
```

C风格的`for`循环

```bash
for (( init; condition; increment ))
do
    commands
done
```

**示例：**

```bash
for (( i=0; i<5; i++ ))
do
    echo "Number $i"
done
```

##### 2. `while`循环

`while`循环在条件为真时重复执行一组命令。

```bash
while condition
do
    commands
done
```

**示例：**

```bash
i=0
while [ $i -lt 5 ]
do
    echo "Number $i"
    i=$((i+1))
done
```

##### 3. `until`循环

`until`循环在条件为真时停止执行，与`while`循环相反。

```bash
until condition
do
    commands
done
```

**示例：**

```bash
i=0
until [ $i -ge 5 ]
do
    echo "Number $i"
    i=$((i+1))
done
```

##### 4. 使用`break`和`continue`

- `break`用于完全终止循环。
- `continue`用于跳过当前迭代，继续下一次循环。

**示例：**

![](https://pic.imgdb.cn/item/6714e247d29ded1a8cc697e8.png)

```bash
for i in 1 2 3 4 5
do
    if [ $i -eq 3 ]; then
        break
    fi
    echo "Number $i"
done

echo "Loop ended."

for i in 1 2 3 4 5
do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo "Number $i"
done
```

##### 5. 使用`select`进行菜单式循环

`select`命令用于创建一个简单的菜单，并根据用户的选择执行命令。

```bash
select variable in item1 item2 item3 ...
do
    commands
    break
done
```

**示例：**

```bash
echo "Choose an option:"
select opt in "Option 1" "Option 2" "Option 3" "Quit"
do
    case $opt in
        "Option 1" )
            echo "You chose Option 1"
            ;;
        "Option 2" )
            echo "You chose Option 2"
            ;;
        "Option 3" )
            echo "You chose Option 3"
            ;;
        "Quit" )
            echo "Exiting..."
            break
            ;;
        * )
            echo "Invalid option $REPLY"
            ;;
    esac
done
```

这些循环结构使你能够编写更复杂和强大的Shell脚本。记得在编写循环时，要确保循环条件最终会结束，以避免创建无限循环。

## Shell小示例

> 写一个通过Shell来git文件推送到github仓库的.sh脚本

要在 Linux 中使用 Shell 编写一个用于将文件推送到 GitHub 仓库的脚本，你需要确保有 `git` 和 `curl` 命令可用，并且你的项目已经在本地初始化了一个 Git 存储库。以下是一个简单的 Shell 脚本来完成这个任务：

```bash
#!/bin/bash

# 需要安装或确认已存在 git、curl 命令
command -v git &> /dev/null || { echo "Error: 'git' command not found. Please install Git."; exit 1; }
command -v curl &> /dev/null || { echo "Error: 'curl' command not found. Please install cURL."; exit 1; }

# 定义全局变量
REPO_URL="https://github.com/username/repository.git"
GITHUB_TOKEN_FILE=".git/github-token.txt"
LOCAL_REPO_PATH="/path/to/local/repo"
REMOTE_BRANCH="main" # 默认为 "main"

# 检查 GitHub token 是否存在，并且可用的 Git repo 是否已经被克隆
if [ -f "$GITHUB_TOKEN_FILE" ]; then
    echo "GitHub token found, proceeding with push."
else
    echo "No GitHub token found. Please create one at https://github.com/settings/tokens/new and add it to '$GITHUB_TOKEN_FILE'."
    exit 1
fi

# 使用 `git` 命令添加本地仓库的远程 URL
echo 'Adding remote GitHub repository...'
git config --global user.email "you@example.com" && git config --global user.name "Your Name"
git remote set-url origin "$REPO_URL"

# 检查是否需要将远程分支切换到目标分支
if [[ $(git rev-parse --abbrev-ref HEAD) != "$REMOTE_BRANCH" ]]; then
    echo 'Switching to the target branch...'
    git checkout $REMOTE_BRANCH || git checkout -b $REMOTE_BRANCH
fi

# 提交更改并推送至远程仓库
echo 'Committing changes and pushing to GitHub...'
git add .
git commit -m "Automatic update from shell script"
git push origin $REMOTE_BRANCH

echo "Push completed successfully."
```

### 脚本说明：

1. **初始化配置**：确保 `git` 和 `cURL` 命令可用。
  
2. **定义全局变量**：
  
  - `REPO_URL`: GitHub仓库的 URL。
  - `GITHUB_TOKEN_FILE`: 包含 GitHub token 的文件路径（用于授权）。
  - `LOCAL_REPO_PATH`: 本地项目的目录路径。
  - `REMOTE_BRANCH`: 要推送到的目标分支，默认为 "main"。
3. **检查 GitHub token**：确保有有效的 token 并位于指定的文件中。
  
4. **设置 Git 配置**（可选，根据需要）：可以添加或更新用户信息等配置项。
  
5. **添加远程仓库**：将本地仓库与 GitHub 仓库关联。
  
6. **切换到目标分支**（如果需要）：确保本地仓库中的当前分支和指定的远程分支一致。
  
7. **提交更改并推送**：提交本地所有更改，并推送到远程仓库。