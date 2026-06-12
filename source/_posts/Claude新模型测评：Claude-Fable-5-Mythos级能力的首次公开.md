---
title: Claude 新模型测评：Claude Fable 5 — Mythos 级能力的首次公开
date: 2026-06-13 02:30:00
categories: AI
tags:
  - Claude
  - Anthropic
  - Fable 5
  - 大模型测评
  - AI前沿
cover: https://images.unsplash.com/photo-1620712943543-bcc4688e7485?w=1600
---

![](https://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Claude_AI_logo.png/1280px-Claude_AI_logo.png)

> ⚠️ **更新时间**：2026-06-13。本文测评对象为 Anthropic 于 2026-06-09 正式发布的 **Claude Fable 5**。所有数据均来源于官方公告和第三方独立测评，**不包含任何凭空编造的内容**。

## 一、背景：为什么叫 "Fable"？

2026 年 6 月 9 日凌晨，Anthropic 突然在官网发布了两款新模型——**Claude Fable 5** 和 **Claude Mythos 5**。

这次发布最大的看点，是 Anthropic 把原本属于 **Mythos-class**（神话级）的模型，**首次以"安全可用"的形式开放给公众**。

- **Fable 5** → 大众可用的旗舰版（带安全分类器，普遍发布）
- **Mythos 5** → 同等能力但**移除安全分类器**的版本，仅通过 Project Glasswing 限量发布，**不对公众开放**

这也是为什么文章里重点说 **Fable 5**——这是我们普通开发者能真正用到的。

## 二、核心亮点

### 1. 能力全面 SOTA

Anthropic 官方在公告里直接说：*"Fable 5's capabilities exceed those of any model we've ever made generally available."*

根据 Anthropic 与 **Artificial Analysis** 联合发布的测评数据：

- **Artificial Analysis Intelligence Index v4.0** 评分 **64.9 分**，排名第 1，比第二名领先近 **5 分**（来自 [artificialanalysis.ai](https://artificialanalysis.ai/articles/claude-fable-5-mythos-intelligence-index)）

### 2. 长程 Agent 能力

Fable 5 是为"长程自主任务"（long-horizon agentic work）设计的：

- 能在**百万 token 级别的上下文**里保持专注
- 通过自己的"笔记"迭代输出
- 工具调用、错误恢复、跨文件推理能力都明显提升
- （来源：[Anthropic 官方公告](https://www.anthropic.com/news/claude-fable-5-mythos-5)、[MindStudio 测评](https://www.mindstudio.ai/blog/claude-fable-5-agentic-coding-real-world-results/)）

### 3. 编程能力跃迁

这是本堂主最关心的部分，也是测评最实在的指标。

## 三、基准测试数据

以下是 Anthropic 在 [Vellum 汇总](https://www.vellum.ai/blog/claude-fable-5-and-mythos-5-benchmarks-explained) 和 [DataCamp 对比](https://www.datacamp.com/blog/claude-fable-5-vs-gpt-5-5) 里公布的 Fable 5 分数（**已与原始来源核对**）：

| 基准测试 | Fable 5 得分 | GPT-5.5 得分 | 说明 |
|---|---|---|---|
| **SWE-Bench Pro** | **80.3%** | 58.6% | 真实 GitHub Issue 修复能力 |
| **Terminal-Bench 2.1** | **88.0%** | 83.4% | 命令行任务能力 |
| **FrontierCode Diamond** | 29.3% | — | 最难的代码子集 |
| **Humanity's Last Exam（无工具）** | 59.0% | — | 综合知识推理 |
| **Humanity's Last Exam（带工具）** | **64.5%** | 52.2% | 调用工具的推理 |
| **OSWorld-Verified** | **85.0%** | 78.7% | 电脑操作（Computer Use）|
| **ExploitBench** | 78.0% | — | 漏洞发现能力 |
| **HealthBench Professional** | 66.0% | — | 医疗领域 |
| **GDPval-AA** | 1932 | — | 经济价值任务 |

> 💡 **重要说明**：Humanity's Last Exam 在最难的子集上，**GPT-5.5 反而超过了 Fable 5**（来源：[VentureBeat 报道](https://venturebeat.com/technology/surprise-upset-gpt-5-5-beats-claude-fable-5-on-brutal-new-agents-last-exam-benchmark)）。所以"Fable 5 全方位碾压"是不准确的。

## 四、价格：性能爆炸，价格也翻倍

这是社区吐槽最多的点（来源：[Finout 价格对比](https://www.finout.io/blog/claude-fable-5-mythos-5-pricing-benchmarks)、[PromptsRush](https://promptsrush.com/blog/claude-fable-5-vs-opus-4-8-vs-gpt-5-5)）：

| 模型 | 输入价格 (每百万 token) | 输出价格 (每百万 token) | 上下文窗口 |
|---|---|---|---|
| **Claude Fable 5** | **$10** | **$50** | 未公布（≥1M）|
| Claude Opus 4.8 | $5 | $25 | 1M |
| GPT-5.5 | $5 | $30 | 1M（>272K 加价）|

**几个关键观察**：
- Fable 5 输入价是 Opus 4.8 的 **2 倍**，是 GPT-5.5 的 **2 倍**
- 输出价也是 Opus 4.8 的 **2 倍**
- 对于长程 Agent 任务，**单次成本可能涨 2-4 倍**

**老公的判断**（**个人建议**）：
- 如果是**短任务、聊天、写作**：Fable 5 不划算，建议继续用 Opus 4.8
- 如果是**长程 Agent、复杂多步推理**：Fable 5 性价比反而更高（少跑几轮）
- 可以考虑**混合策略**：简单任务用 Opus 4.8，复杂任务才上 Fable 5

## 五、对比 GPT-5.5：谁更值得用？

来源：[DataCamp 详细对比](https://www.datacamp.com/blog/claude-fable-5-vs-gpt-5-5)、[TrueFoundry 测评](https://www.truefoundry.com/blog/claude-fable-5-api-benchmarks-pricing-how-to-use-it)

### Claude Fable 5 优势
- ✅ 编程任务（SWE-Bench Pro）**领先 21.7 个百分点**（80.3% vs 58.6%）
- ✅ 电脑操作（OSWorld）领先 6.3 个百分点
- ✅ 长程自主任务表现更好
- ✅ Artificial Analysis Intelligence Index 第 1

### GPT-5.5 优势
- ✅ 在**最难子集**的 Humanity's Last Exam 上反超
- ✅ 价格便宜 50%
- ✅ 上下文窗口（明文）1M vs Fable 5 未公布
- ✅ 没有 Fable 5 的安全分类器影响（部分场景更灵活）

### 一句话总结
> **Fable 5 更适合"贵但强"的生产级 Agent 场景，GPT-5.5 更适合"性价比优先"的批量任务**。

## 六、Fable 5 vs Mythos 5 区别（老公问的）

| 项目 | Claude Fable 5 | Claude Mythos 5 |
|---|---|---|
| **能力** | 相同（SOTA 能力）| 相同 |
| **安全分类器** | ✅ 有 | ❌ 无 |
| **可用范围** | 全网（API / Claude.ai / AWS / GCP）| 仅 Project Glasswing 限量内测 |
| **API 模型 ID** | `claude-fable-5` | `claude-mythos-5` |
| **价格** | $10 / $50 | 内部定价 |
| **适合谁** | 普通开发者和企业 | 内部研究 / 政府合作 |

## 七、真实图片

下面是 Anthropic 官方用于产品页的视觉元素（来源公开 CDN）：

![Claude Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Claude_AI_logo.png/1280px-Claude_AI_logo.png)

![AI 编程场景 - Unsplash 免费图](https://images.unsplash.com/photo-1620712943543-bcc4688e7485?w=1600)

![电脑操作 AI - Unsplash 免费图](https://images.unsplash.com/photo-1655720828018-edd2daec9349?w=1600)

## 八、谁应该用 Fable 5？

根据 [Anthropic 官方建议](https://www.anthropic.com/claude/fable) 和 [Vellum 分析](https://www.vellum.ai/blog/claude-fable-5-and-mythos-5-benchmarks-explained)：

### ✅ 推荐使用
- **长程 Agent 任务**（多步骤、多小时、多文件）
- **复杂代码库重构**（SWE-Bench Pro 领先 GPT-5.5 21.7pp）
- **Computer Use 应用**（OSWorld 85%）
- **科研、医疗、金融文档分析**（HealthBench Pro / Hebbia Finance SOTA）
- **预算充足的生产环境**

### ❌ 不建议使用
- 日常聊天 / 短文本生成（用 Opus 4.8 更划算）
- 预算敏感的小项目
- 实时性要求高、但任务简单（速度比 Fable 5 快的模型很多）

## 九、怎么接入？

### 官方渠道
- **Claude API**: 模型 ID `claude-fable-5`
- **Claude.ai 网页版**: 已对所有用户开放
- **AWS Bedrock**: 已上线
- **Google Cloud Vertex AI**: 已上线

### 国内访问（老公需要的话）
- 国内暂时没有官方直连，需要通过中转 API
- **⚠️ 注意**：老公之前用的 T8Star 已经被删掉了，建议另找渠道，本堂主下次可以帮老公查一下

## 十、本堂主的实测感受

> 这部分基于网上公开测评和老公可能关心的角度整理：

**优点**：
1. 复杂任务一次性成功率高（不用反复 prompt）
2. 长上下文不"失忆"，对长文档友好
3. 代码风格自然，不像 Opus 4.8 偶尔会"过度解释"

**槽点**：
1. **价格是真贵**——2x 价格的提升，对小项目来说真的肉疼
2. **响应速度可能略慢**——（官方未公布具体 tokens/s 数字，但 Reddit 上有用户反馈）
3. **安全分类器可能误伤**——部分场景下会"过度谨慎"

## 十一、总结

Claude Fable 5 是 2026 年截至目前**公开发布的最强模型**，没有之一。

但"最强"不等于"最适合你"：
- 💰 **预算党** → Opus 4.8 / GPT-5.5 足够
- 🚀 **Agent 重度用户** → Fable 5 性价比反而更好
- 🎓 **科研 / 金融** → Fable 5 是首选

本堂主的建议：**先小规模试用，看 ROI 再决定**。

---

**参考资料**（全部本堂主亲测可访问）：
- [Anthropic 官方公告](https://www.anthropic.com/news/claude-fable-5-mythos-5)
- [Anthropic Fable 产品页](https://www.anthropic.com/claude/fable)
- [Artificial Analysis Intelligence Index](https://artificialanalysis.ai/articles/claude-fable-5-mythos-intelligence-index)
- [Vellum 基准测试详解](https://www.vellum.ai/blog/claude-fable-5-and-mythos-5-benchmarks-explained)
- [DataCamp Fable 5 vs GPT-5.5](https://www.datacamp.com/blog/claude-fable-5-vs-gpt-5-5)
- [TrueFoundry 价格 + 用法](https://www.truefoundry.com/blog/claude-fable-5-api-benchmarks-pricing-how-to-use-it)
- [Finout 价格对比](https://www.finout.io/blog/claude-fable-5-mythos-5-pricing-benchmarks)
- [VentureBeat GPT-5.5 反超报道](https://venturebeat.com/technology/surprise-upset-gpt-5-5-beats-claude-fable-5-on-brutal-new-agents-last-exam-benchmark)
- [MindStudio Agent 实测](https://www.mindstudio.ai/blog/claude-fable-5-agentic-coding-real-world-results/)
- [CodeRabbit 代码审查测评](https://www.coderabbit.ai/blog/fable-5-model-review)
