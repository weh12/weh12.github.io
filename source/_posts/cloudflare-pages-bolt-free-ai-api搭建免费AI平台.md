---
title: cloudflare pages+bolt+free ai api搭建免费AI平台
date: 2024-11-13 04:02:43
categories: 教程
tags: 网站服务
cover: https://pic.imgdb.cn/item/6733b524d29ded1a8ce300d0.webp
---
## bolt是什么, 它能做什么?
### Bolt项目概念

Bolt项目是一个包含bolt-project.yaml文件的目录，用于存储Bolt内容，如计划和任务。
它是Bolt工具的起点，允许用户针对特定基础设施创建特定的编排内容。

- Bolt项目的作用

Bolt项目使得用户能够将Bolt内容与组织中的其他用户共享。
通过版本控制提交项目目录，以便其他用户使用。

- 创建Bolt项目

用户可以通过运行bolt project init命令来创建一个新的Bolt项目。
该命令会在项目目录中创建一个包含项目名称的bolt-project.yaml文件。

## Bolt.new项目介绍

Bolt.new 的 Cole Medin 分支

这个 Bolt.new 的分支版本允许你为每个提示选择使用的 LLM（大型语言模型）！目前，你可以使用 OpenAI、Anthropic、Ollama、OpenRouter、Gemini 或 Groq 模型——并且可以轻松扩展以使用 Vercel AI SDK 支持的任何其他模型！请参见下面的说明，了解如何在本地运行此分支并扩展它以包含更多模型。
欢迎对这个分支提出改进建议并贡献代码！

```bash
✅ 集成 OpenRouter (@coleam00)
✅ 集成 Gemini (@jonathands)
✅ 从下载的内容自动生成 Ollama 模型 (@yunatamos)
✅ 按提供商过滤模型 (@jasonm23)
✅ 将项目下载为 ZIP 文件 (@fabwaseem)
✅ 改进 app\lib\.server\llm\prompts.ts 中的主要 Bolt.new 提示 (@kofi-bhr)
✅ 集成 DeepSeek API (@zenith110)
✅ 集成 Mistral API (@ArulGandhi)
✅ 集成“类似 Open AI”的 API (@ZerxZ)
✅ 将文件同步（单向同步）到本地文件夹 (@muzafferkadir)
✅ 使用 Docker 容器化应用程序以便于安装 (@aaronbolton)
✅ 直接将项目发布到 GitHub (@goncaloalves)
✅ 在 UI 中输入 API 密钥的能力 (@ali00209)
✅ xAI Grok Beta 集成 (@milutinke)
⬜ 高优先级 - 防止 Bolt 频繁重写文件（文件锁定和差异）
⬜ 高优先级 - 对小型 LLMs 进行更好的提示（代码窗口有时不启动）
⬜ 高优先级 - 加载本地项目到应用程序
⬜ 高优先级 - 将图像附加到提示
⬜ 高优先级 - 在后端运行代理而不是单一模型调用
⬜ LM Studio 集成
⬜ Together 集成
⬜ Azure Open AI API 集成
⬜ HuggingFace 集成
⬜ Perplexity 集成
⬜ Vertex AI 集成
⬜ Cohere 集成
⬜ 直接部署到 Vercel/Netlify/其他类似平台
⬜ 恢复代码到早期版本的能力
⬜ 提示缓存
⬜ 更好的提示增强
⬜ 让 LLM 在 MD 文件中规划项目以获得更好的结果/透明度
⬜ 与 git 类似的确认集成 VSCode
⬜ 上传文档以获取知识 - UI 设计模板、参考编码风格的代码库等。
⬜ 语音提示
```

这是github上一个大佬魔改的bolt, 项目地址在这里[https://github.com/coleam00/bolt.new-any-llm](https://github.com/coleam00/bolt.new-any-llm)开源免费部署, 且可以通过docker直接部署, 我这里部署方式选择在我的cloudflare的子域名下部署

## 部署流程

- 1.首先我们fork一下项目到本地你自己的github仓库

![](https://pic.imgdb.cn/item/6733b998d29ded1a8ce4c403.png)

> PS: 【注意】如果后续部署不成功，请记得在你fork的仓库中，将根目录下的 .tool-versions 的内容清空

- 2.然后在cloudflare中,设置我们的Worker Pages页面的github连接

![](https://pic.imgdb.cn/item/6733badbd29ded1a8ce54a34.png)
![](https://pic.imgdb.cn/item/6733badbd29ded1a8ce54a66.png)

> 记得把授权项目选择你fork的项目

- 3.接着使用Worker进行部署

![](https://pic.imgdb.cn/item/6733bbe7d29ded1a8ce62b2a.png)

> 架构我们使用Remix模板, 构建代码我们选择使用pnpm, 修改成pnpm run build

![](https://pic.imgdb.cn/item/6733bc44d29ded1a8ce64edd.png)

- 4.点击构建项目按钮(如果构建失败原因已经说啦)

- 5.自定义域名

![](https://pic.imgdb.cn/item/6733bd52d29ded1a8ce6c0a1.png)
> 修改给定的项目自动分配的域为自己的根域名或者子域名

- 6.配置自定义API令牌指定APIKey

![](https://pic.imgdb.cn/item/6733be8dd29ded1a8ce7357f.png)

> 我这里选用Mistral平台的免费APIKey,你也可以选择使用OpenApiKey

- 7.重新部署, 过一两分钟就可以测试网站了

![](https://pic.imgdb.cn/item/6733bf3fd29ded1a8ce77efd.png)

## 博主自己的网站地址
 
### [ai.tongyeweh.top](ai.tongyeweh.top)