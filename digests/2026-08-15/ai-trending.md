# AI 开源趋势日报 2026-08-15

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-08-14 23:14 UTC

---

经 AI 相关性过滤后，已排除 holehe、spiderfoot、rustdesk、OpenCut、spec-kit 等通用工具；以下仅保留与 AI/ML 明确相关的项目。  
> 注：Trending 榜单部分条目未提供总星数，表中以 `—` 表示，括号内为今日新增 Stars。

## 今日速览

2026-08-15 的 GitHub AI 热榜呈现出鲜明的“Agent 生态 + 上下文工程”特征：17 个 Trending 仓库中 12 个与 AI 直接相关，diagram-design（+3,651）、semantica（+1,183）、holaOS（+769）等新面孔占据前列。Agent 正在从单点任务走向带共享内存、浏览器状态与 MCP 集成的工作台形态。同时，14MB 端侧模型 needle（+661）与本地 3D 生成工具 modly（+580）说明“小模型 + 本地生成”正在成为独立赛道。RAGFlow（+474）持续领跑 RAG 引擎，semantica 则让“上下文层”的基础设施路线更加多元。

## 各维度热门项目

### 1. 🔧 AI 基础工具（框架、SDK、推理引擎、开发工具、CLI）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | C++ | 197,024 | 通用机器学习框架，覆盖训练到部署全链路。长期作为 ML 基础设施基座存在，是观测 AI 生态基本面的核心项目。 |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 178,545 | 本地 LLM 运行/服务工具，已适配 Kimi-K2.6、GLM-5.2、MiniMax、DeepSeek 等模型。持续占据“本地推理”第一入口。 |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 164,106 | 模型定义与训练/推理的标准框架，支持文本、视觉、音频与多模态。16.4 万星体量说明它仍是 AI 开源生态的中枢。 |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 102,375 | 动态神经网络与 GPU 加速框架。在 AI Topic 搜索中保持高星，是研究和生产侧最常用的训练框架之一。 |
| [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | Rust | 76,168 | 用 CLI 代理减少 60–90% LLM token 消耗的 Rust 工具。零依赖、单二进制，契合 Agent 高频调用 LLM 的成本优化需求。 |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 66,368 | 压缩工具输出、日志、文件与 RAG 分片的上下文优化库。编码 Agent 可省 20% token，JSON 场景最高省 95%，是“上下文工程”热点工具。 |
| [cursor/plugins](https://github.com/cursor/plugins) | TypeScript | —（+69） | Cursor 官方插件规范与插件仓库。今日 +69 进入 Trending，代表 AI IDE 插件生态正在走向标准化。 |

### 2. 🤖 AI 智能体/工作流（Agent 框架、自动化、多智能体）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 240,165 | Agent Harness 性能调优系统，覆盖技能、记忆、安全与研究型开发。24 万星体量说明开发者对 Agent 系统级能力高度关注。 |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 186,624 | 最早出圈的自主 Agent 项目之一。其“accessible AI for everyone”理念仍是 Agent 社区的重要基线。 |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 152,459 | 可视化构建 Agentic Workflow 与 RAG 管道的协作平台。支持云、VPC、自托管，是 Agent 应用开发的主流底座。 |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 144,262 | Agent 工程平台，提供工具调用、记忆、编排等能力。生态庞大，是 RAG/Agent 应用的事实标准之一。 |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | TypeScript | 84,056 | AI 驱动开发平台，用 Agent 完成编码任务。84k Stars，是“AI + 软件工程”方向的代表性项目。 |
| [holaboss-ai/holaOS](https://github.com/holaboss-ai/holaOS) | TypeScript | —（+769） | 开源 All-in-One AI Agent 工作台，支持 Claude Code、Codex 等，并通过 MCP 连接 100+ 工具。今日 +769，说明统一 Agent 工作台需求强劲。 |
| [citrolabs/ego-lite](https://github.com/citrolabs/ego-lite) | JavaScript | —（+153） | 面向 AI Agent 的浏览器，可共享登录态给 Codex/Claude Code。今日 +153，解决了浏览器自动化中的登录/状态难题。 |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | —（+3,651） | 为 Claude Code 提供 29 种编辑器级图表模板，纯 HTML+SVG。今日 +3,651 登顶 Trending，Agent 技能/模板类需求旺盛。 |

### 3. 📦 AI 应用（具体应用产品、垂直场景解决方案）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 148,802 | 用户友好的自托管 AI 界面，支持 Ollama、OpenAI API 等。是本地部署 AI 应用层的事实标准。 |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 103,559 | 利用 AI 大模型与自动化工作流一键生成高清短视频。从主题到成片，是 AI 视频生成领域的高星应用。 |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 64,716 | 本地优先的 All-in-One AI 工作区，支持 RAG 与多模型。“Own your intelligence”理念契合私有部署趋势。 |
| [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) | Python | 62,877 | LLM 驱动的多市场股票分析系统，含行情、新闻、决策看板与推送。是金融垂直场景的高星应用。 |
| [calesthio/OpenMontage](https://github.com/calesthio/OpenMontage) | Python | 48,106 | 开源 agentic 视频生产系统，12 条生产管线、100+ 工具。把 AI 编码助手变成完整视频工作室。 |
| [macro-inc/macro](https://github.com/macro-inc/macro) | Rust | —（+435） | 面向团队的统一工作台，将邮件、聊天、文档、CRM 与 AI Agent 和共享 AI 记忆 @-link。今日 +435，体现 AI 原生协作产品新形态。 |
| [lightningpixel/modly](https://github.com/lightningpixel/modly) | TypeScript | —（+580） | 本地 AI 生成 3D 模型的桌面应用，完全运行在 GPU 上。今日 +580，反映“本地生成式 3D”关注度上升。 |
| [ToolJet/ToolJet](https://github.com/ToolJet/ToolJet) | JavaScript | —（+302） | 开源低代码平台，也是 ToolJet AI 的基础，可构建内部工具、业务应用与 AI Agent。今日 +302，企业级 AI 低代码热度上升。 |

### 4. 🧠 大模型/训练（模型权重、训练框架、微调工具）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [unslothai/unsloth](https://github.com/unslothai/unsloth) | Python | —（+502） | 本地 UI 运行和训练 LLM/扩散模型，已支持 Qwen3.8、Kimi K3、MiniMax-H3、Gemma 4、DeepSeek-V4、FLUX 等。今日 +502，训练/推理工具正快速追新模型。 |
| [cactus-compute/needle](https://github.com/cactus-compute/needle) | Python | —（+661） | 14MB 端侧基础模型，面向手机、穿戴、智能家居与机器人。今日 +661，是“小模型 + 端侧 AI”的最强新信号。 |
| [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) | Jupyter Notebook | 102,666 | 从零实现 ChatGPT-like LLM 的 PyTorch 教程。10.2 万星，是理解 LLM 原理的必读学习仓库。 |
| [NVlabs/Sana](https://github.com/NVlabs/Sana) | Python | 8,766 | NVIDIA 高效高分辨率图像合成模型，基于线性 Diffusion Transformer。代表生成模型基础研究持续活跃。 |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,301 | 支持 100+ 数据集和 Llama/Qwen/GLM/Claude 等模型的 LLM 评测平台。是评估模型能力的标准工具之一。 |
| [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) | Python | 4,487 | 在 Apple Silicon 上从零构建 tiny vLLM + Qwen 推理系统的学习项目。帮助系统工程师理解 LLM 推理栈。 |
| [Picovoice/picollm](https://github.com/Picovoice/picollm) | Python | 316 | 基于 X-bit 量化的设备端 LLM 推理库。与 needle 等一起指向端侧模型生态。 |

### 5. 🔍 RAG/知识库（向量数据库、检索增强、知识管理）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 88,387（+474） | 开源 RAG 引擎，融合 Agent 能力，构建 LLM 的上下文层。今日 +474，仍是 RAG 赛道的中坚项目。 |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | JavaScript | 90,764 | 为 Agent 提供跨会话持久上下文，压缩并注入相关记忆。支持 Claude Code、Codex、Gemini 等，解决长时记忆刚需。 |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 106,367 | 将代码库、文档、SQL schema 转成可查询知识图，兼容 Claude Code/Cursor/Codex/Gemini CLI。是 Graph RAG 路线的高星代表。 |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 51,641 | 领先的文档 Agent 与 OCR 平台，覆盖数据接入到 RAG 全流程。是 RAG 应用开发的主要框架之一。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 63,270 | 为 AI Agent 提供通用记忆层，支持跨会话长期记忆。与 claude-mem 共同带火“Agent 记忆”赛道。 |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 45,637 | 云原生向量数据库，支持大规模向量 ANN 搜索。是 RAG 基建中的关键组件。 |
| [qdrant/qdrant](https://github.com/qdrant/qdrant) | Rust | 33,977 | 高性能大规模向量数据库与搜索引擎，专为下一代 AI 应用设计。在 vector-db 主题下保持高关注。 |
| [semantica-agi/semantica](https://github.com/semantica-agi/semantica) | Python | —（+1,183） | 图原生的 Context 与可问责 AI 系统基础设施。今日 +1,183，说明“上下文层”正在成为独立基础设施赛道。 |

## 趋势信号分析

今日最强信号是 Agent 基础设施从概念走向产品化：diagram-design（+3,651）、semantica（+1,183）、holaOS（+769）等新面孔占据热榜，开发者更关注 Agent 的状态共享、上下文管理，而非单一模型能力。其次，“小模型 + 端侧”首次形成独立叙事：14MB 的 needle、X-bit 量化的 picollm，指向 TinyML 与低成本部署。第三，上下文/RAG 层持续细分：RAGFlow 稳定增长，headroom 做 token 压缩、claude-mem 做持久记忆、semantica 做图原生上下文，“上下文工程”正成为 Agent 性能核心。最后，新模型迭代极快，ollama/unsloth 已适配 Kimi-K2.6、GLM-5.2、Qwen3.8、DeepSeek-V4，开源工具链与模型发布进入正反馈循环。

## 社区关注热点

- **Agent 工作台/浏览器**：[holaOS](https://github.com/holaboss-ai/holaOS)（+769）与 [ego-lite](https://github.com/citrolabs/ego-lite)（+153）分别从“统一工作区”和“共享浏览器状态”切入，解决 Agent 长期运行与登录态复用问题。
- **上下文与记忆层**：[semantica](https://github.com/semantica-agi/semantica)（+1,183）的图原生 Context 基础设施、[claude-mem](https://github.com/thedotmack/claude-mem) 的跨会话记忆、[headroom](https://github.com/headroomlabs-ai/headroom) 的 token 压缩，正在把“上下文”变成一门独立工程。
- **端侧小模型**：[needle](https://github.com/cactus-compute/needle)（+661）以 14MB 模型杀入 Trending，联合 [picollm](https://github.com/Picovoice/picollm) 的量化推理，显示手机/穿戴/机器人等端侧场景将成为下一波 AI 落地热点。
- **本地生成式 3D/视频**：[modly](https://github.com/lightningpixel/modly)（+580）完全在 GPU 上从图片/提示词生成 3D 模型；[OpenMontage](https://github.com/calesthio/OpenMontage)、[MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) 持续降低视频生产门槛。
- **DeepSeek Agent 生态**：[awesome-deepseek-agent](https://github.com/deepseek-ai/awesome-deepseek-agent)（+203）登榜，加上 [DeepSeek-Reasonix](https://github.com/esengine/DeepSeek-Reasonix) 等终端编码 Agent，说明 DeepSeek 模型在 Agent 场景的采用率正在快速提升。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*