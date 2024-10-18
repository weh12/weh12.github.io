---
title: Ollama完整教程：本地LLM管理、WebUI对话、Python/Java客户端API应用
date: 2024-10-18 17:29:51
categories: AI
tags: 大语言模型
cover: https://pic.imgdb.cn/item/67125858d29ded1a8ce4fbf6.jpg
---
# Ollama 是什么，它与 Llama 有什么关系？
Ollama官网：https://ollama.com/，官方网站的介绍就一句话: Get up and running with large language models. （开始使用大语言模型。）

Ollama是一个开源的 LLM（大型语言模型）服务工具，用于简化在本地运行大语言模型、降低使用大语言模型的门槛，使得大模型的开发者、研究人员和爱好者能够在本地环境快速实验、管理和部署最新大语言模型，包括如Qwen2、Llama3、Phi3、Gemma2等开源的大型语言模型。

Ollama支持的大语言模型列表，可通过搜索模型名称查看：https://ollama.com/library

Ollama官方 GitHub 源代码仓库：https://github.com/ollama/ollama/

Llama是 Meta 公司开源的备受欢迎的一个通用大语言模型，和其他大模型一样，Llama可以通过Ollama进行管理部署和推理等。

因此，Ollama与Llama的关系：Llama是大语言模型，而Ollama是大语言模型（不限于Llama模型）便捷的管理和运维工具，它们只是名字后面部分恰巧相同而已！
![](https://pic.imgdb.cn/item/67122bf8d29ded1a8ca57996.png)
![](https://pic.imgdb.cn/item/67122b5ed29ded1a8ca4b910.png)
# Ollama 安装和常用系统参数设置
在官网首页，我们可以直接下载Ollama安装程序（支持 Windows/MacOS/Linux）：https://ollama.com/

Ollama的安装过程，与安装其他普通软件并没有什么两样，安装完成之后，有几个常用的系统环境变量参数建议进行设置：

    OLLAMA_MODELS：模型文件存放目录，默认目录为当前用户目录（Windows 目录：C:\Users%username%.ollama\models，MacOS 目录：~/.ollama/models，Linux 目录：/usr/share/ollama/.ollama/models），如果是 Windows 系统建议修改（如：D:\OllamaModels），避免 C 盘空间吃紧

    OLLAMA_HOST：Ollama 服务监听的网络地址，默认为127.0.0.1，如果允许其他电脑访问 Ollama（如：局域网中的其他电脑），建议设置成0.0.0.0，从而允许其他网络访问

    OLLAMA_PORT：Ollama 服务监听的默认端口，默认为11434，如果端口有冲突，可以修改设置成其他端口（如：8080等）

    OLLAMA_ORIGINS：HTTP 客户端请求来源，半角逗号分隔列表，若本地使用无严格要求，可以设置成星号，代表不受限制

    OLLAMA_KEEP_ALIVE：大模型加载到内存中后的存活时间，默认为5m即 5 分钟（如：纯数字如 300 代表 300 秒，0 代表处理请求响应后立即卸载模型，任何负数则表示一直存活）；我们可设置成24h，即模型在内存中保持 24 小时，提高访问速度

    OLLAMA_NUM_PARALLEL：请求处理并发数量，默认为1，即单并发串行处理请求，可根据实际情况进行调整

    OLLAMA_MAX_QUEUE：请求队列长度，默认值为512，可以根据情况设置，超过队列长度请求被抛弃

    OLLAMA_DEBUG：输出 Debug 日志标识，应用研发阶段可以设置成1，即输出详细日志信息，便于排查问题

    OLLAMA_MAX_LOADED_MODELS：最多同时加载到内存中模型的数量，默认为1，即只能有 1 个模型在内存中
# Ollama 管理本地已有大模型
## 【展示本地大模型列表：ollama list】
![](https://pic.imgdb.cn/item/67122cf1d29ded1a8ca67313.png)
可以看到，本地有 2 个大模型，它们的名称（NAME）分别为gemma2:9b和qwen2:7b。

## 【删除单个本地大模型：ollama rm 本地模型名称】
![](https://pic.imgdb.cn/item/67122d44d29ded1a8ca6daad.png)
通过rm命令删除了gemma2:9b大模型之后，再次通过list命令查看，本地只有qwen2:7b一个大模型了。

## 【启动本地模型：ollama run 本地模型名】
```bash
>ollama run qwen2:0.5b
>>>>
```
- PS:要在模型出运行

启动成功之后，就可以通过终端对话界面进行对话了（本命令下面也会讲到，其他详细暂且忽略）

## 【查看本地运行中模型列表：ollama ps】
![](https://pic.imgdb.cn/item/67122dead29ded1a8ca78a20.png)
通过ps命名可以看到，本地qwen2:0.5b大模型正在运行中。

## 【复制本地大模型：ollama cp 本地存在的模型名 新复制模型名】
![](https://pic.imgdb.cn/item/67122e49d29ded1a8ca7f2e5.png)
上面cp命令，把本地qwen2:0.5b复制了一份，新模型名为Qwen2-0.5B

# Ollama 下载到本地大模型
下面介绍三种通过 Ollama 下载到本地大模型方式：

- 方式一：直接通过 Ollama 远程仓库下载，这是最直接的方式，也是最推荐、最常用的方式

- 方式二：如果已经有 GGUF 模型权重文件了，不想重新下载，也可以通过 Ollama 把该文件直接导入到本地（不推荐、不常用）

- 方式三：如果已经有 safetensors 模型权重文件，也不想重新下载，也可以通过 Ollama 把该文件直接导入到本地（不推荐、不常用）

## 方式一：Ollama 从远程仓库下载大模型到本地
【下载或者更新本地大模型：ollama pull 本地/远程仓库模型名称】

本pull命令从 Ollama 远程仓库完整下载或增量更新模型文件，模型名称格式为：模型名称:参数规格；如ollama pull qwen2:0.5b 则代表从 Ollama 仓库下载qwen2大模型的0.5b参数规格大模型文件到本地磁盘:
![](https://pic.imgdb.cn/item/67122f2bd29ded1a8ca8e382.png)
Qwen2模型列表

我第一次尝试下载模型是失败的,后来看github大佬解释,需要到api.md官网用curl下载:
> 官方调用地址: https://github.com/ollama/ollama/blob/main/docs/api.md#list-running-models
- 下载拉取模型(pull):
```bash
curl http://localhost:11434/api/pull -d '{
  "name": "llama3.2"
}'
```
- 创建新模型:
```bash
curl http://localhost:11434/api/create -d '{
  "name": "mario",
  "modelfile": "FROM llama3\nSYSTEM You are mario from Super Mario Bros."
}'
```

如果参数规格标记为latest则代表为默认参数规格，下载时可以不用指定，如Qwen2的7b被标记为latest，则ollama pull qwen2和ollama pull qwen2:7b这 2 个命令的意义是一样的，都下载的为7b参数规格模型。为了保证后续维护方便、避免误操作等，老牛同学建议不管是否为默认参数规格，我们下载命令中均明确参数规格。

值得一提的是，今天开始GLM4支持 Ollama 部署和推理，老牛同学特意列出它的下载命令：ollama pull glm4:9b（和其他模型相比，其实并没有特殊支出）。需要注意的是：Ollama 最低版本为0.2.0才能支持GLM4大模型！
![](https://pic.imgdb.cn/item/67122f75d29ded1a8ca93ff5.png)
GLM4模型列表
![](https://pic.imgdb.cn/item/67122fa3d29ded1a8ca971f6.png)
若本地不存在大模型，则下载完整模型文件到本地磁盘；若本地磁盘存在该大模型，则增量下载大模型更新文件到本地磁盘。

从上面最后的list命令结果可以看到，老牛同学本地存在了qwen2:0.5b这个名称的大模型。

【下载且运行本地大模型：ollama run 本地/远程仓库模型名称】

```bash
>ollama run qwen2:0.5b
>>>>
```
若本地不存在大模型，则下载完整模型文件到本地磁盘（类似于pull命令），然后启动大模型；若本地存在大模型，则直接启动（不进行更新）。

启动成功后，默认为终端对客界面：
![](https://pic.imgdb.cn/item/67124d91d29ded1a8cd4f15f.png)
Ollama终端对话界面

- 1.若需要输入多行文本，需要用三引号包裹，如："""这里是多行文本"""

- 2./clear清除对话上下文信息

- 3./bye则退出对话窗口

- 4./set parameter num_ctx 4096可设置窗口大小为 4096 个 Token，也可以通过请求设置，如：curl <http://localhost:11434/api/generate> -d '{ "model": "qwen2:7b", "prompt": "Why is the sky blue?", "options": { "num_ctx": 4096 }}'

- 5./show info可以查看当前模型详情

![](https://pic.imgdb.cn/item/67124e47d29ded1a8cd68bdd.png)

## 方式二：Ollama 导入 GGUF 模型文件到本地磁盘
若我们已经从 HF 或者 ModeScope 下载了 GGUF 文件（文件名为：Meta-Llama-3-8B-Instruct.Q4_K_M.gguf），在我们存放Llama3-8B的 GGUF 模型文件目录中，创建一个文件名为Modelfile的文件，该文件的内容如下：
```bash
FROM ./Meta-Llama-3-8B-Instruct.Q4_K_M.gguf
```
然后，打开终端，执行命令导入模型文件：ollama create 模型名称 -f ./Modelfile
```bash
>ollama create Mistral-7B-v0.3 -f ./Modelfile
transferring model data
using existing layer sha256:647a2b64cbcdbe670432d0502ebb2592b36dd364d51a9ef7a1387b7a4365781f
creating new layer sha256:459d7c837b2bd7f895a15b0a5213846912693beedaf0257fbba2a508bc1c88d9
writing manifest
success
```
导入成功之后，我们就可以通过list命名，看到名为Mistral-7B-v0.3的本地模型了，后续可以和其他模型一样进行管理了。

## 方式三：Ollama 导入 safetensors 模型文件到到本地磁盘
官方操作文档：https://ollama.fan/getting-started/import/#importing-pytorch-safetensors

若我们已经从 HF 或者 ModeScope 下载了 safetensors 文件（文件目录为：Mistral-7B）
```bash
git lfs install

git clone https://www.modelscope.cn/rubraAI/Mistral-7B-Instruct-v0.3.git Mistral-7B
```
然后，我们转换模型（结果：Mistral-7B-v0.3.bin）：
```bash
python llm/llama.cpp/convert.py ./Mistral-7B --outtype f16 --outfile Mistral-7B-v0.3.bin
```
接下来，进行量化量化：
```bash
llm/llama.cpp/quantize Mistral-7B-v0.3.bin Mistral-7B-v0.3_Q4.bin q4_0
```
最后，通过 Ollama 导入到本地磁盘，创建Modelfile模型文件：
```bash
FROM Mistral-7B-v0.3_Q4.bin
```
执行导入命令，导入模型文件：ollama create 模型名称 -f ./Modelfile
```bash
>ollama create Mistral-7B-v0.3 -f ./Modelfile
transferring model data
using existing layer sha256:647a2b64cbcdbe670432d0502ebb2592b36dd364d51a9ef7a1387b7a4365781f
creating new layer sha256:459d7c837b2bd7f895a15b0a5213846912693beedaf0257fbba2a508bc1c88d9
writing manifest
success
```
导入成功之后，我们就可以通过list命名，看到名为Mistral-7B-v0.3的本地模型了，后续可以和其他模型一样进行管理了。

# 基于 WebUI 部署 Ollama 可视化对话界面
Ollama自带控制台对话界面体验总归是不太好，接下来部署 Web 可视化聊天界面：

- 下载并安装 Node.js 工具：https://nodejs.org/zh-cn
- 下载ollama-webui工程代码：git clone https://github.com/ollama-webui/ollama-webui-lite ollama-webui
- 切换ollama-webui代码的目录：cd ollama-webui
- 设置 Node.js 工具包镜像源（下载提速）：npm config set registry http://mirrors.cloud.tencent.com/npm/
- 安装 Node.js 依赖的工具包：npm install
- 最后，启动 Web 可视化界面：npm run dev
![](https://pic.imgdb.cn/item/67124f99d29ded1a8cd99df8.jpg)

Ollam WebUI启动成功

如果看到以上输出，代表 Web 可视化界面已经成功了！

浏览器打开 Web 可视化界面：http://localhost:3000/
![](https://pic.imgdb.cn/item/67124fddd29ded1a8cda600f.png)
Ollam WebUI对话界面

# Ollama 客户端：HTTP 访问服务
Ollama 默认提供了generate和chat这 2 个原始的 API 接口，使用方式如下：
- 1.generate接口的使用样例：
![](https://pic.imgdb.cn/item/67125089d29ded1a8cdc148c.png)
- 2.chat接口的使用样例：
![](https://pic.imgdb.cn/item/671250d0d29ded1a8cdcbdd8.png)

接下来的Python和Java客户端应用，都是对这 2 个接口的封装。

# Ollama 客户端：Python API 应用
我们把 Ollama 集成到 Python 应用中，只需要以下简单 2 步即可：

第一步，安装 Python 依赖包：
```bash
pip install ollama
```
第二步，使用 Ollama 接口，stream=True代表按照流式输出：
![](https://pic.imgdb.cn/item/6712516cd29ded1a8cde4470.png)

# Ollama 客户端：Java API 应用（SpringBoot 应用）
我们也可以把 Ollama 集成到 SpringBoot 应用中，只需要以下简单 3 步即可：

第一步，在总pom.xml中新增 SpringBoot Starter 依赖：
![](https://pic.imgdb.cn/item/671251b3d29ded1a8cdeedb5.png)

第二步，在 SpringBoot 配置文件application.properties中增加 Ollama 配置信息：
![](https://pic.imgdb.cn/item/671251e0d29ded1a8cdf471a.png)

第三步，使用OllamaChatClient进行文字生成或者对话：
![](https://pic.imgdb.cn/item/67125201d29ded1a8cdf8031.png)
![](https://pic.imgdb.cn/item/67125226d29ded1a8cdf9fb0.png)

以上是 Java 客户端的简单样例，我们可以通过OllamaChatClient访问 Ollama 接口，既可以使用默认大模型，也可以在参数指定模型名称！