---
title: Tauri快速全平台开发框架学习
date: 2024-12-09 12:40:20
categories: 教程
tags: 框架
cover: https://pic.imgdb.cn/item/6756605bd0e0a243d4e01e1a.png
---
## Tauri 是什么？有什么优势

Tauri 是一个构建适用于所有主流桌面和移动平台的轻快二进制文件的框架。开发者们可以集成任何用于创建用户界面的可以被编译成 HTML、JavaScript 和 CSS 的前端框架，同时可以在必要时使用 Rust、Swift 和 Kotlin 等语言编写后端逻辑。

![](https://pic.imgdb.cn/item/6756605bd0e0a243d4e01e1a.png)

你可以使用下面的任意一个命令以利用  [`create-tauri-app`](https://github.com/tauri-apps/create-tauri-app)  创建一个新的项目。请务必参考[前置要求文档](https://tauri.app/zh-cn/start/prerequisites/)安装所有 Tauri 必须的依赖，并阅读[前端配置指南](https://tauri.app/zh-cn/start/frontend/)了解推荐的前端配置方案。

{% tabs test %}

<!-- tab Bash -->

```bash
sh <(curl https://create.tauri.app/sh)
```

<!-- endtab -->

<!-- tab PowerShell -->

```bash
irm https://create.tauri.app/ps | iex
```

<!-- endtab -->

<!-- tab npm -->

```bash
npm create tauri-app@latest
```

<!-- endtab -->

<!-- tab Yarn -->

```bash
yarn create tauri-app
```

<!-- endtab -->

<!-- tab pnpm -->

```bash
pnpm create tauri-app
```

<!-- endtab -->

<!-- tab deno -->

```bash
deno run -A npm:create-tauri-app
```

<!-- endtab -->

<!-- tab bun -->

```bash
bun create tauri-app
```

<!-- endtab -->

<!-- tab Cargo -->

```bash
cargo install create-tauri-app --locked
cargo create-tauri-app
```

<!-- endtab -->

{% endtabs %}

在你成功创建了你的第一个应用后，你可以在[功能及秘诀列表](https://tauri.app/zh-cn/plugin/)探索 Tauri 的不同功能及秘诀。

### 为什么选用 Tauri？
对于开发者而言，Tauri 有三个主要优势：
- 构建应用所需的可靠基础
- 使用系统原生 webview（网页视图）带来的更小打包体积
- 使用任何前端技术和多种语言绑定带来的灵活性
阅读 Tauri 1.0 博客文章深入了解 Tauri 哲学思想。

### 可靠的基础
由于 Tauri 是使用 Rust 构建的，它可以利用 Rust 提供的内存、线程和类型安全方面的优势。而使用 Tauri 开发的应用也可以从中受益，甚至无需由 Rust 专家开发这些应用。

Tauri 的每一个主要和次要版本都会接受安全审计。这一审计不光涵盖了 Tauri 组织范围内的代码，还包括了 Tauri 所使用的上游依赖。当然，这些举措不能消除所有的风险，但它们能够为开发者提供一个坚实的基础。

阅读 [Tauri 安全策略](https://github.com/tauri-apps/tauri/security/policy)和 Tauri 1.0 审计报告了解更多信息。

### 更小的体积
Tauri 利用了已经存在于每一个用户系统的 webview。Tauri 应用中只包含了该应用专属的代码和资源文件，不需要在每个应用中都打包一个浏览器引擎，这意味着一个最小化的 Tauri 应用体积可能小于 600KB。

在应用体积概念中你可以深入学习如何创建一个最优化的应用。

### 灵活的架构
由于 Tauri 使用了 web 技术，这也意味着几乎所有的前端框架都与 Tauri 兼容。
开发者可以在 JavaScript 中使用 invoke 函数实现 JavaScript 与 Rust 绑定，而 Swift 和 Kotlin 绑定则是作为 [Tauri 插件](https://tauri.app/zh-cn/develop/plugins/)

TAO 被用于管理 Tauri 窗口创建，WRY 则被用于管理 webview 渲染。这些库由 Tauri 维护，并且如果需要 Tauri 没有暴露的更深入的系统集成时可以直接使用。
除此之外，Tauri 还维护了一些插件拓展 Tauri 核心暴露的系统能力。你可以在功能及秘诀章节同时找到官方和社区提供的插件。

## 安装所需依赖
为了开始使用 Tauri 构建项目，你首先需要安装一些依赖项：
- 系统依赖项
- Rust
- 移动端配置 （仅在针对移动设备进行开发时才需要）

> windows安装Rust可查看这篇博文：[https://blog.csdn.net/xinyingzai/article/details/135459640](https://blog.csdn.net/xinyingzai/article/details/135459640)

> 具体依赖项目和安装系统可查看官方文档：[tauri官方](https://tauri.app/zh-cn/start/prerequisites/#系统依赖项)

## 创建项目
在 create-tauri-app 创建完项目后，您可以进入项目文件夹，安装依赖，然后使用 Tauri CLI 启动开发服务器：

![](https://pic.imgdb.cn/item/6756715bd0e0a243d4e0345a.png)

```bash
cd tauri-app
pnpm install
pnpm tauri dev
```
您将会看到一个新的窗口被打开，该窗口正在运行您的应用。
恭喜您！ 您已经创建了您自己的 Tauri 应用！🚀

## 如何打包项目
Tauri 通过 build、android build 和 ios build 命令直接从其 CLI 构建您的应用程序。

```bash
pnpm tauri build
```
如果您需要进一步自定义平台捆绑包的生成方式，可以拆分构建和捆绑包步骤：

```bash
pnpm tauri build --no-bundle
# bundle for distribution outside the macOS App Store
pnpm tauri bundle --bundles app,dmg
# bundle for App Store distribution
pnpm tauri bundle --bundles app --config src-tauri/tauri.appstore.conf.json
```

>具体操作可查看官方细节文档：[https://tauri.app/zh-cn/distribute/](https://tauri.app/zh-cn/distribute/)

OK，任务完成，感谢你阅读到这里，如果你觉得这篇文章对你有所启发，不妨给我一个小小的鼓励，通过关注和打赏来支持我继续创作更多有价值的内容。