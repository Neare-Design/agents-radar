# AI 开源趋势日报 2026-08-16

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-08-15 23:14 UTC

---

# AI 开源趋势日报（2026-08-16）

> 本次已按「明确涉及 AI/ML」原则过滤：Trending 中的 `public-apis`、`holehe`、`cordis` 等通用工具未纳入；主题搜索仅保留与 LLM、Agent、RAG、向量库、生成式 AI 等直接相关的项目。  
> 注：Trending 仓库原始数据中总量字段为 0（未提供），按原样保留；括号内为今日新增 stars。

---

## 1. 今日速览

今日 GitHub AI 热榜有两个鲜明主线：一是「Agent 基础设施」正在快速平台化，`cursor/plugins` 官方插件规范、`ego-lite` Agent 浏览器、`CLI-Anything` 的 agent-native 改造同时登榜；二是「边缘与低资源 AI」爆发，`needle` 以 14MB 基础模型切入微型设备，`FluidVoice` 做纯本地方案，`Soup` 可在 4GB 笔记本 GPU 上流式微调 8B 模型。主题搜索侧，Agent 框架与 RAG/记忆类项目继续占据高 star 位，Claude Code 生态的 skill、上下文持久化、token 优化成为新增量。视频生成应用层也保持活跃，agentic video pipeline 正在成为新的开源热点。

---

## 2. 各维度热门项目

### 🔧 AI 基础工具（框架、SDK、推理引擎、开发工具、CLI）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 178,602 | 本地大模型运行和管理工具，支持 Kimi、GLM、DeepSeek、Qwen 等最新模型。是个人与团队部署开源 LLM 的第一入口。 |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 164,120 | 模型定义与训练/推理框架，覆盖文本、视觉、音频、多模态。作为 AI 开源生态地基，长期占据头部位置。 |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | C++ | 197,045 | 经典开源机器学习框架，覆盖训练、部署与移动端。 |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 102,393 | 动态神经网络框架，研究和生产中最常用的深度学习平台之一。 |
| [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | Rust | 76,239 | 面向开发命令的 LLM token 削减 CLI 代理，可减少 60-90% 消耗。Rust 单二进制、零依赖是轻量部署亮点。 |
| [cursor/plugins](https://github.com/cursor/plugins) | TypeScript | 0（+152） | Cursor 官方插件规范与插件库，让 AI IDE 生态可扩展。今日登榜说明编码代理插件体系正在平台化。 |
| [0xPlaygrounds/rig](https://github.com/0xPlaygrounds/rig) | Rust | 8,277 | Rust 生态的模块化 LLM 应用开发框架，适合构建高性能、低资源占用的 Agent 服务。 |
| [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) | Python | 4,488 | 从零构建类 vLLM 推理系统的教学项目，帮助系统工程师理解 LLM 推理栈。 |

---

### 🤖 AI 智能体/工作流（Agent 框架、自动化、多智能体）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 240,292 | Agent harness 性能优化系统，覆盖技能、记忆、安全与研发流程。是当前大热的 Claude Code / Codex 增强层。 |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 186,621 | 让 AI 自主规划并执行任务的经典 Agent 项目。经多轮迭代，已发展为通用 Agent 工具平台。 |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 152,544 | 可视化构建 Agentic Workflow 与 RAG 管道，支持云、VPC 或自托管。企业级 AI 应用开发的重要基座。 |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 109,342 | 让 AI Agent 能操作浏览器的开源方案，自动化网页任务。是网页 Agent 方向引用最多的项目之一。 |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | TypeScript | 84,142 | AI 驱动开发平台，能自主完成编码、调试和运维任务。代表“AI 程序员”工作流正在爆发。 |
| [citrolabs/ego-lite](https://github.com/citrolabs/ego-lite) | JavaScript | 0（+546） | 为 AI Agent 设计的浏览器，可将登录态共享给 Codex、Claude Code 执行自动化任务。今日热榜显示 Agent 基础设施正从“工具”走向“环境”。 |
| [HKUDS/CLI-Anything](https://github.com/HKUDS/CLI-Anything) | Python | 0（+100） | 提出“让所有软件 Agent-Native”的 CLI 改造方法，使传统软件可被 Agent 通过 CLI 调用。 |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | 0（+1,619） | 为 Claude Code 提供 29 种编辑级图表模板，纯 HTML/SVG 输出。今日新增 1.6k+ star，说明开发者正追求 AI 产出物的质量与设计感。 |

---

### 📦 AI 应用（具体应用产品、垂直场景解决方案）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 148,871 | 自托管 AI 对话界面，兼容 Ollama 与 OpenAI API。是本地 LLM 部署后最常用的前端入口。 |
| [Shubhamsaboo/awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) | Python | 132,752 | 收录 100+ 免费开源的 AI Agents、Agent Skills 与 RAG 应用。是开发者寻找生产级参考实现的重要索引。 |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 103,928 | 基于大模型和自动化工作流，根据主题或关键词一键生成高清短视频。持续高热说明内容创作自动化需求旺盛。 |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 64,738 | 本地优先的全能 LLM 应用环境，支持内置 RAG、Agent 与多模型管理。主打“拥有自己的智能”。 |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 50,519 | AI 生产力工作室，支持智能对话、自主 Agent、300+ 助手。面向日常办公的统一 AI 客户端。 |
| [calesthio/OpenMontage](https://github.com/calesthio/OpenMontage) | Python | 48,218 | 开源 agentic 视频生产系统，内置 12 条生产流水线、100+ 工具、700+ 技能文件。把 AI 编码助手变成视频制作工作室。 |
| [ToolJet/ToolJet](https://github.com/ToolJet/ToolJet) | JavaScript | 0（+553） | 开源低代码平台，用于快速搭建内部工具、仪表盘和 AI Agents。今日 +553 star，显示低代码 + AI Agent 平台关注度回升。 |
| [altic-dev/FluidVoice](https://github.com/altic-dev/FluidVoice) | Swift | 0（+165） | macOS 端本地听写应用，使用端侧 STT 与自训练 AI 增强模型。作为本地 Wispr Flow 替代方案登榜，代表隐私优先的 AI 应用趋势。 |

---

### 🧠 大模型/训练（模型权重、训练框架、微调工具）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [cactus-compute/needle](https://github.com/cactus-compute/needle) | Python | 0（+551） | 14MB 基础模型，面向手机、穿戴设备、智能家居和机器人。今日 +551 star，边缘小模型是当前最受关注的新方向之一。 |
| [unslothai/unsloth](https://github.com/unslothai/unsloth) | Python | 0（+435） | 本地 UI，可运行和训练 LLM 与扩散模型，支持 Qwen、Kimi、Gemma、DeepSeek、FLUX 等。大幅降低本地微调门槛。 |
| [MakazhanAlpamys/Soup](https://github.com/MakazhanAlpamys/Soup) | Python | 0（+303） | 通过一个 YAML 微调 LLM，层流式训练可在 4GB 笔记本 GPU 上训练 8B 模型。低资源微调的代表性新项目。 |
| [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) | Jupyter Notebook | 102,731 | 从零开始用 PyTorch 实现 ChatGPT 类 LLM 的教程项目。是深度学习开发者理解大模型原理的经典资源。 |
| [NVlabs/Sana](https://github.com/NVlabs/Sana) | Python | 8,769 | 高效高分辨率图像合成模型，基于线性 Diffusion Transformer。 |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,307 | LLM 评测平台，支持 100+ 数据集与多种主流模型。是模型发布后社区评测基建的重要组成部分。 |
| [llm-jp/awesome-japanese-llm](https://github.com/llm-jp/awesome-japanese-llm) | TypeScript | 1,424 | 日本语 LLM 全景列表，覆盖模型、评测、数据集与应用。 |
| [Picovoice/picollm](https://github.com/Picovoice/picollm) | Python | 317 | 端侧 LLM 推理库，使用 X-Bit 量化技术，适合资源受限设备。 |

---

### 🔍 RAG/知识库（向量数据库、检索增强、知识管理）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) | TypeScript | 167,787 | 面向 LLM 的网页上下文 API，可搜索、抓取并交互式读取网页。是大模型应用的数据摄入层重要基础设施。 |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 106,710 | 将代码库、文档、SQL Schema、PDF 转为可查询知识图谱。支持 Claude Code、Cursor、Codex，是“无向量库 RAG”的新思路。 |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | JavaScript | 90,834 | 为所有 Agent 提供跨会话持久上下文，捕获、压缩并注入历史上下文。解决 Agent“失忆”问题的关键工具。 |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 88,551 | 开源 RAG 引擎，深度融合 RAG 与 Agent 能力，提供高质量 LLM 上下文层。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 63,329 | AI Agent 的通用记忆层，支持跨应用、跨会话的长期记忆。是记忆赛道 star 增长最快的项目之一。 |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 51,662 | 领先的文档 Agent 与 OCR 平台，也是 RAG 应用最常用的编排框架之一。 |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 45,646 | 云原生向量数据库，专为大规模向量 ANN 搜索设计。 |
| [qdrant/qdrant](https://github.com/qdrant/qdrant) | Rust | 33,992 | 高性能、大规模向量数据库与检索引擎，面向下一代 AI 应用。 |

---

## 3. 趋势信号分析

今日热榜中，Agent 基础设施与边缘推理形成双主线。`cursor/plugins` 的官方插件规范、`ego-lite` 的 Agent 浏览器、`CLI-Anything` 的 agent-native 改造，都在把 AI 从对话式工具推向“代理即平台”。同时，`needle` 以 14MB 模型切入手机/穿戴设备，`FluidVoice` 做本地听写，`Soup` 在 4GB 笔记本上流式微调 8B 模型，说明小模型和低资源训练正从概念变成可用方案。搜索侧，RAG/记忆赛道的高 star 项目（`claude-mem`、`mem0`、`Graphify`）持续吸睛，反映 Agent 对跨会话持久化上下文的需求成为刚需。整体与近期 Claude Code、Cursor 等编码代理生态快速发展一致——技能、插件、上下文管理正在形成新的开源组件层。

---

## 4. 社区关注热点

- **Agent 生态标准化**：关注 `cursor/plugins`、`CLI-Anything`、`cathrynlavery/diagram-design`——插件规范、Agent 原生 CLI 与高质量技能正在成为新平台层。
- **边缘小模型**：`cactus-compute/needle` 仅 14MB，直接服务手机、穿戴设备与机器人，值得关注“tiny foundation model”赛道。
- **低资源微调**：`MakazhanAlpamys/Soup` 与 `unslothai/unsloth` 把 8B 模型训练/微调压到消费级 GPU，开发者个人实验门槛大幅下降。
- **Agent 持久记忆**：`thedotmack/claude-mem`、`mem0ai/mem0`、`Graphify-Labs/graphify` 正在解决 Agent 上下文断裂问题，是本阶段最实用的增量方向。
- **AI 视频生成应用层**：`calesthio/OpenMontage`、`ArcReel/ArcReel`、`xixihhhh/clipforge` 等项目密集出现，agentic video pipeline 正在成为新的内容生产基础设施。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*