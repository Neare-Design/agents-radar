# AI 开源趋势日报 2026-08-14

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-08-13 23:34 UTC

---

> 已过滤与 AI/ML 无关的通用工具：holehe、SpiderFoot、manim、puppeteer、bruno、Files、yazi、appsmith、it-tools 等。

## 今日速览

今日 AI 开源最大看点是 **Agent Skills 生态集中爆发**：Claude Code 相关技能仓库占据热榜半壁江山，其中 `diagram-design` 单日新增 stars 达 +4,504，Anthropic 官方 `skills` 仓库也正式开放。端侧 AI 明显升温，14MB 小模型 `needle`、本地听写 `FluidVoice` 和本地 3D 生成 `modly` 同时在榜。基础设施层面，NVIDIA `Switchyard` 与 `semantica` 分别代表“模型路由”和“图上下文”两个新方向。RAG 与视频生成赛道持续活跃：`RAGFlow` 继续领跑，`LTX-2`、`OpenMontage` 推动生成式视频走向工程化。

## 各维度热门项目

### 🔧 AI 基础工具

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [semantica-agi/semantica](https://github.com/semantica-agi/semantica) | Python | 0（+727） | Graph-Native Infrastructure，面向可审计 AI 系统的图原生上下文基础设施。今日新登榜，代表“图结构上下文治理”这一新方向。 |
| [NVIDIA-NeMo/Switchyard](https://github.com/NVIDIA-NeMo/Switchyard) | Rust | 0（+408） | 让 LLM 应用在多个模型/供应商间路由流量，同时保持 OpenAI/Anthropic API 兼容。今日 +408，多模型成本与性能优化需求明显上升。 |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | C++ | 197,006 | 面向所有人的开源机器学习框架。作为传统 AI 基础设施底座，仍是生态兼容与长期维护的关键参照。 |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 178,477 | 本地运行 Kimi、GLM、MiniMax、DeepSeek、Qwen、Gemma 等模型。是“模型本地化”浪潮中最常用的运行时之一。 |
| [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) | TypeScript | 166,959 | 面向 LLM 的 Web 搜索、抓取与交互 Context API。为 Agent 和 RAG 提供大规模实时网页数据接入。 |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 164,078 | 统一描述 SOTA 文本/视觉/音频/多模态模型的框架。几乎覆盖今日所有新模型的推理与训练生态。 |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 102,359 | 张量计算与动态神经网络框架，GPU 加速。今天榜单中大量训练/推理项目仍以 PyTorch 为基础。 |
| [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | Rust | 76,032 | 将常见开发命令的 LLM token 消耗降低 60-90% 的 CLI 代理。单 Rust 二进制、零依赖，是 LLM 成本治理的代表工具。 |

### 🤖 AI 智能体/工作流

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | 0（+4,504） | 为 Claude Code 提供的 29 种编辑级图表类型，自包含 HTML+SVG。今日热榜第一，说明“Agent 技能包”正成为最热门的分发形态。 |
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | Shell | 0（+762） | “一站式 AI 代理机构”，从前端到社区运营再到质检，每个 Agent 都带人格与完整流程。今日 +762，展示多智能体协作的工程化落地。 |
| [anthropics/skills](https://github.com/anthropics/skills) | Python | 0（+383） | Anthropic 官方 Agent Skills 仓库。官方入局意味着“可复用技能”正在成为 Agent 开发的标准单元。 |
| [holaboss-ai/holaOS](https://github.com/holaboss-ai/holaOS) | TypeScript | 0（+380） | 开源 All-in-One AI Agent 工作区，支持 Claude Code、Codex 等，并集成 100+ 工具和 MCP。今日 +380，是 Agent 工作区赛道的热门新品。 |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 239,961 | Agent Harness 性能优化系统，为 Claude Code、Codex、Cursor 等提供技能、记忆、安全与研究优先开发体验。在 dev-tools 类目中 star 极高。 |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 186,592 | “人人可用、人人可构建”的 Agent 愿景。作为开源 Agent 框架元老，仍是社区讨论和二次开发的基础。 |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 144,185 | Agent 工程平台。连接模型、工具、记忆与工作流，是当前最主流的 Agent 开发框架之一。 |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | TypeScript | 83,951 | AI 驱动软件开发平台。从需求到代码、执行与调试，展示 Agent 在真实研发流程中的完整闭环。 |

### 📦 AI 应用

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [macro-inc/macro](https://github.com/macro-inc/macro) | Rust | 0（+1,180） | 面向团队的统一工作区：邮件、聊天、文档、任务、Agent 与 CRM 通过共享 AI 记忆联在一起。今日 +1,180，是“AI 原生团队协作”的高增长项目。 |
| [lightningpixel/modly](https://github.com/lightningpixel/modly) | TypeScript | 0（+221） | 桌面应用，用本地 AI 从图片生成 3D 模型，完全运行在 GPU 上。端侧生成式 3D 的实用化尝试。 |
| [altic-dev/FluidVoice](https://github.com/altic-dev/FluidVoice) | Swift | 0（+187） | macOS 最快听写应用，端侧 STT + 自定义 AI 增强模型，定位本地版 Wispr Flow。代表端侧语音 AI 的产品化。 |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 148,713 | 用户友好的 AI 对话界面，支持 Ollama、OpenAI API 等。是自托管 LLM 应用最常见的入口之一。 |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 103,105 | 根据主题/关键词一键生成高清短视频。AI 自动化内容生产的现象级项目，持续保持高热度。 |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 50,425 | AI 生产力工作室：智能聊天、自主 Agent、300+ 助手，统一接入前沿 LLM。面向个人用户的“AI 全家桶”。 |
| [calesthio/OpenMontage](https://github.com/calesthio/OpenMontage) | Python | 47,963 | 号称全球首个开源 Agent 化视频生产系统，12 条生产流水线、100+ 工具。把 AI 编程助手变成完整视频制作工作室。 |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | Python | 46,496 | 将文档/主题转为原生 PowerPoint 演示文稿，支持原生形状、动画、图表与配音。AI 办公垂直场景的落地代表。 |

### 🧠 大模型/训练

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [cactus-compute/needle](https://github.com/cactus-compute/needle) | Python | 0（+768） | 14MB 基础模型，面向手机、可穿戴、智能家居和机器人。今日 +768，显示“极小参数 + 端侧部署”成为新关注点。 |
| [unslothai/unsloth](https://github.com/unslothai/unsloth) | Python | 0（+354） | 本地 UI 运行和训练 LLM/扩散模型，支持 Qwen3.8、Kimi K3、MiniMax-H3、Gemma 4、DeepSeek-V4、FLUX 等。降低本地微调与推理门槛。 |
| [Lightricks/LTX-2](https://github.com/Lightricks/LTX-2) | Python | 0（+201） | LTX-2 音视频生成模型的官方 Python 推理与 LoRA 训练包。开源音视频生成模型是今日值得关注的新方向。 |
| [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) | Jupyter Notebook | 102,609 | 从零实现 ChatGPT 类 LLM 的 PyTorch 教程。社区学习大模型原理的经典资源。 |
| [NVlabs/Sana](https://github.com/NVlabs/Sana) | Python | 8,758 | 高效高分辨率图像合成线性扩散 Transformer。在生成式图像模型训练与推理上有较高参考价值。 |
| [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) | Python | 4,483 | 面向系统工程师的 Apple Silicon LLM 推理教学项目，从零构建微型 vLLM + Qwen。 |
| [kandinskylab/kandinsky-5](https://github.com/kandinskylab/kandinsky-5) | Python | 808 | Kandinsky 5.0 系列视频与图像生成扩散模型。开源视频生成模型家族的新进展。 |
| [AarambhDevHub/aarambh-studio](https://github.com/AarambhDevHub/aarambh-studio) | Rust | 76 | 纯 Rust + Candle 从零构建的 decoder-only LLM，支持 MoE、量化感知训练。展示无 Python/PyTorch 依赖的极简训练路线。 |

### 🔍 RAG/知识库

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 88,001（+473） | 开源 RAG 引擎，融合 RAG 与 Agent 能力，为 LLM 提供上下文层。今日继续在热榜获得 +473，是 RAG 领域最受关注的项目之一。 |
| [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) | — | 0（+411） | 教 Agent 使用 Obsidian CLI 和 Markdown、Bases、JSON Canvas 等开放格式。把知识库管理能力赋予 AI Agent，今日 +411。 |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 106,027 | 将代码库、文档、SQL Schema、PDF 转化为可查询知识图谱，支持 Claude Code、Cursor、Gemini CLI。代表“无向量库、基于图谱的 RAG”路线。 |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | JavaScript | 90,650 | 为所有 Agent 保留跨会话持久上下文，自动压缩并注入相关记忆。是 Agent 长期记忆层的流行方案。 |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 64,698 | 本地优先的 All-in-One AI 与 RAG 体验。让用户不“租用”智能，而是拥有自己的本地知识库与 Agent 环境。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 63,210 | AI Agent 的通用记忆层。解决跨会话记忆与个性化问题，是 RAG 向“记忆基础设施”演进的重要组件。 |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 51,623 | 领先的文档 Agent 与 OCR 平台。连接私有数据与 LLM，是 RAG 应用开发的核心框架之一。 |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 45,628 | 高性能云原生向量数据库，专为可扩展向量 ANN 搜索设计。是大规模 RAG 与向量检索场景的基础设施。 |

## 趋势信号分析

今日热榜最突出的是 **Agent Skills 生态集中爆发**：`anthropics/skills` 官方仓库公开，`diagram-design`、`obsidian-skills`、`agency-agents` 等围绕 Claude Code 的技能包占据大量今日 stars，说明 Agent 正从“单次对话”走向“可复用技能包”。其次，**边缘/端侧 AI 明显升温**：`needle` 以 14MB 参数登上热榜，`FluidVoice` 主打端侧 STT，`modly` 在本地 GPU 生成 3D，AI 正在从云端大模型向手机、IoT 和桌面端下沉。基础设施方面，NVIDIA `Switchyard` 与 `semantica` 分别从模型路由和图上下文切入，反映多模型治理与可审计上下文成为新议题。视频生成赛道继续活跃，`LTX-2`、`OpenMontage` 等将 AI 从文本推向音视频工作流。

## 社区关注热点

- **Agent Skills 标准化**：`anthropics/skills` 官方仓库 + `diagram-design` 单日 +4,504，技能包正在成为 Agent 生态的“插件市场”。
- **端侧小模型与设备 AI**：`needle`（14MB）、`FluidVoice`（本地 STT）、`modly`（本地 3D）显示 AI 推理正在大规模向个人设备迁移。
- **多模型路由与成本治理**：`NVIDIA-NeMo/Switchyard`、`rtk-ai/rtk` 说明企业不再只关心“哪个模型更强”，也开始关心多模型切换、token 成本与供应商绑定。
- **记忆与图知识层**：`graphify`、`claude-mem`、`mem0`、`semantica` 等将长期记忆/知识图谱引入 RAG 与 Agent，推动上下文从“检索片段”升级为“可推理知识结构”。
- **AI 视频生产工程化**：`LTX-2` 开源音视频模型与 `OpenMontage` 的 Agent 化流水线，正在把“脚本到成片”变成可本地运行的开源工作流。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*