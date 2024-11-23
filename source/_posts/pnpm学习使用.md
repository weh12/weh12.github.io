---
title: pnpm学习使用
date: 2024-11-24 02:17:06
categories: 教程
tags: Node.js
cover: https://pic.imgdb.cn/item/67421ab0d29ded1a8c5170d9.jpg
---
# pnpm介绍

## pnpm是什么？

pnpm 是一个流行的包管理器，用于Node.js项目，它专注于提高性能和节省磁盘空间。以下是pnpm的一些主要特点和优势：

- 性能提升：pnpm在安装包时，会并行下载所有依赖项，这可以显著减少安装时间，特别是在大型项目中。

- 节省磁盘空间：pnpm使用一个全局的存储来保存项目的所有依赖项，这意味着不同项目之间可以共享相同的依赖副本，从而减少磁盘空间的使用。

- 确定性安装：pnpm确保在不同环境中安装相同的依赖项，这有助于减少因环境差异导致的问题。

- 自动清理：pnpm会自动清理不再需要的依赖项，以保持项目的清洁和高效。

- 支持工作区：pnpm支持Yarn工作区的概念，允许你在一个仓库中管理多个包。

- 兼容性：pnpm与npm和yarn的命令行接口兼容，这意味着如果你之前使用过这些工具，可以很容易地切换到pnpm。

- 脚本运行：pnpm允许你运行脚本，类似于npm和yarn的run命令。

- 环境友好：pnpm在安装时会检查依赖项是否已经安装，如果没有变化，则不会重复安装，这有助于减少不必要的计算资源消耗。

- 版本锁定：pnpm使用pnpm-lock.yaml文件来锁定依赖项的版本，确保在不同环境中的一致性。

- 社区支持：pnpm有一个活跃的社区，不断提供支持和更新。

pnpm是Node.js开发者的一个强大工具，特别是对于那些需要高性能和优化资源使用的大型项目

## pnpm安装和使用

> 安装和使用 `pnpm` 的步骤如下：

### 安装 `pnpm`

1. **安装 Node.js**：首先，确保你的系统上安装了 Node.js。`pnpm` 需要 Node.js 环境来运行。
  
2. **全局安装 `pnpm`**：你可以通过 npm（Node.js 的包管理器）来全局安装 `pnpm`。打开终端或命令提示符，运行以下命令：
  
  ```text
  npm install -g pnpm
  ```
  
  这将安装 `pnpm` 到你的系统上，使其可以在任何地方使用。
  
3. **验证安装**：安装完成后，你可以通过运行以下命令来验证 `pnpm` 是否正确安装：
  
  ```text
  pnpm -v
  ```
  
  这将显示 `pnpm` 的版本号，如果安装成功，你将看到版本信息。
  

### 使用 `pnpm`

1. **创建一个新的 Node.js 项目**：你可以使用 `pnpm` 来创建一个新的 Node.js 项目。在终端中运行：
  
  ```text
  mkdir my-projectcd my-projectpnpm init
  ```
  
  这将创建一个新的目录 `my-project` 并在其中初始化一个新的 Node.js 项目。
  
2. **安装依赖项**：在项目目录中，你可以使用 `pnpm` 来安装项目依赖。例如，安装 `express`：
  
  ```text
  pnpm add express
  ```
  
  这将把 `express` 添加到你的 `package.json` 文件中，并下载到 `node_modules` 目录。
  
3. **安装开发依赖项**：如果你需要安装仅在开发时使用的依赖项，可以使用：
  
  ```text
  pnpm add -D typescript
  ```
  
  `-D` 或 `--save-dev` 标志表示这是一个开发依赖项。
  
4. **运行脚本**：在 `package.json` 中定义脚本后，你可以使用 `pnpm` 来运行这些脚本。例如：
  
  ```text
  pnpm run start
  ```
  
  这将运行 `package.json` 中定义的 `start` 脚本。
  
5. **列出已安装的包**：你可以使用以下命令来列出所有已安装的包：
  
  ```text
  pnpm list
  ```
  
6. **更新包**：要更新项目中的包，可以使用：
  
  ```text
  pnpm update
  ```
  
  这将更新所有包到最新版本。
  
7. **卸载包**：如果你需要从项目中移除一个包，可以使用：
  
  ```text
  pnpm remove <package-name>
  ```
  
  替换 `<package-name>` 为你想要移除的包的名称。
  
8. **使用 `pnpm` 工作区**：如果你的项目是一个包含多个包的 monorepo，`pnpm` 支持工作区功能，允许你在一个仓库中管理多个包。
  

通过这些基本步骤，你可以开始使用 `pnpm` 来管理你的 Node.js 项目的依赖项。`pnpm` 提供了许多其他高级功能和选项，你可以通过查阅 `pnpm` 的官方文档来了解更多信息。