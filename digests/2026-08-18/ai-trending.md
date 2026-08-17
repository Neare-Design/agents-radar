# AI 开源趋势日报 2026-08-18

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-08-17 23:16 UTC

---

# AI 开源趋势日报（2026-08-18）

> 筛选说明：已剔除 Nautilus Trader、Immich、Cordis、Motrix 等非 AI 项目，以及 dev-tools 类别中的 Puppeteer、Bruno、Streamlit、Yazi、Files 等通用工具。今日 Trending 榜单独有仓库未提供总 star 数，表中以“—”标注，括号内为今日新增。

---

## 1. 今日速览

今日最吸睛的仍是 **AI 视频生成**：MoneyPrinterTurbo 单日新增 1,275 Stars，位居 Trending 第一。与此同时，**AI 安全/红队工具首次集中登榜**，strix 与 Anthropic-Cybersecurity-Skills 同时进入今日热榜，说明 Agent 规模化后的安全需求正在爆发。**Agent 记忆与本地推理** 也成为新热点：ai-memory 解决 Agent 长期记忆与跨厂商交接问题，llmfit、omlx 则瞄准“我的硬件能跑什么模型”这一本地部署痛点。RAG 与向量数据库生态依然稳固，Dify、RAGFlow、LlamaIndex 等头部项目保持高热度。

---

## 2. 各维度热门项目

### 🔧 AI 基础工具（框架、SDK、推理引擎、开发工具、CLI）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [vllm-project/vllm](https://github.com/vllm-project/vllm) | Python | 89,275 | LLM 高性能推理与服务引擎，支持 continuous batching 等生产级特性。是本地/私有化部署大模型的核心底座。 |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 178,807 | 极简本地大模型运行工具，一条命令拉起 Qwen、DeepSeek、GPT-OSS 等模型。已成为本地 AI 开发的事实标准。 |
| [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | Rust | 76,404 | 可将常见开发命令的 LLM token 消耗降低 60-90% 的 CLI 代理。Rust 单二进制、零依赖，是成本优化方向的重要项目。 |
| [AlexsJones/llmfit](https://github.com/AlexsJones/llmfit) | Rust | —（+239） | 一条命令探测本机硬件可运行哪些模型，覆盖数百个模型与 Provider。今日热门，反映本地模型选型需求。 |
| [jundot/omlx](https://github.com/jundot/omlx) | Python | —（+96） | 面向 Apple Silicon 的 LLM 推理服务器，支持连续批处理与 SSD 缓存，并可通过 macOS 菜单栏管理。 |
| [Mirrowel/LLM-API-Key-Proxy](https://github.com/Mirrowel/LLM-API-Key-Proxy) | Python | 543 | 统一 LLM API 网关，兼容 OpenAI/Anthropic 端点并做多 Provider 负载均衡。适合多模型混用的生产场景。 |
| [apache/casbin-gateway](https://github.com/apache/casbin-gateway) | Go | 565 | 面向 AI 与 MCP 的 HTTP 安全网关，为 Agent/模型调用链提供权限控制与安全防护。 |
| [Picovoice/picollm](https://github.com/Picovoice/picollm) | Python | 317 | 端侧 LLM 推理库，主打 X-Bit 量化与低资源设备部署，契合边缘 AI 与本地化趋势。 |

### 🤖 AI 智能体/工作流（Agent 框架、自动化、多智能体）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | Python | 232,003 | “随你成长的 Agent”，主打自主演进式智能体形态。高 Star 项目，代表 Agent 从单任务向长期自主方向演进。 |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 144,414 | Agent 工程化平台，提供跨模型工具调用、记忆与 RAG 编排。LLM 应用生态的“标准层”项目。 |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 186,655 | 最早引爆通用 Agent 概念的开源项目，现已演化为可被直接使用的 Agent 工具集。社区关注度持续稳定。 |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 240,695 | Agent Harness 性能优化系统，覆盖 Skills、记忆、安全等能力。与 Claude Code/Cursor 等协同，是 Agent 开发栈新贵。 |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | TypeScript | 84,333 | AI 驱动的软件开发 Agent，可自主完成编码、调试与发布任务。AI Coding 场景的头号开源方案。 |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 109,524 | 让 AI Agent 像人一样操作浏览器并完成网页自动化任务。Web Agent 场景的关键基础设施。 |
| [usestrix/strix](https://github.com/usestrix/strix) | Python | —（+656） | 开源 AI 渗透测试工具，自动发现并修复应用漏洞。今日新增近 700 Stars，说明 AI 安全与红队方向正在升温。 |
| [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) | Python | —（+156） | 为 AI Agent 提供 817 个结构化网络安全技能，映射 MITRE ATT&CK、NIST CSF 2.0 等框架。安全 Agent 技能标准化尝试。 |

### 📦 AI 应用（具体应用产品、垂直场景解决方案）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 105,933（+1,275） | 根据主题或关键词一键生成高清短视频的 AI 自动化工作流。今日热榜第一，是 AI 视频创作应用层标志性项目。 |
| [santifer/career-ops](https://github.com/santifer/career-ops) | JavaScript | 64,586（+147） | 开源 AI 求职助手，可扫描职位、A-F 打分、定制简历并跟踪申请。展示 LLM 在垂直效率场景的落地。 |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 149,046 | 自托管 AI 聊天界面，兼容 Ollama/OpenAI API。从“工具”变成“用户入口”，是本地 AI 应用层流量担当。 |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 64,834 | 本地优先的全栈 AI 知识库/Agent 桌面应用，强调数据私有化与“不租用智能”。 |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 50,663 | AI 生产力工作室，集成智能聊天、自主 Agent 与 300+ 助手。是多模型统一入口的典型产品。 |
| [calesthio/OpenMontage](https://github.com/calesthio/OpenMontage) | Python | 48,595 | 开源 Agent 化视频生产系统，包含 12 条制作流水线与 100+ 工具。把 Coding Agent 变成视频工作室。 |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | Python | 47,490 | 将文档/主题转换为原生 PPT，支持动画、图表和音频解说。AI 办公场景的落地应用。 |
| [Anil-matcha/Open-Generative-AI](https://github.com/Anil-matcha/Open-Generative-AI) | JavaScript | 26,571 | 无限制开源 AI 图像/视频生成平台，内置 500+ 模型。满足自由部署与去中心化内容生成需求。 |

### 🧠 大模型/训练（模型权重、训练框架、微调工具）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | C++ | 196,990 | 通用机器学习框架，与 Keras 深度集成。虽然近年讨论热度被 PyTorch 分流，仍是生产级训练基础设施。 |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 164,195 | 模型定义与加载的事实标准，支持文本/视觉/语音多模态模型。几乎所有主流模型权重都会同步提供 Transformers 实现。 |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 102,439 | 深度学习训练与研究首选框架，动态图与 GPU 加速生态强大。最新模型和研究代码大多基于 PyTorch 发布。 |
| [keras-team/keras](https://github.com/keras-team/keras) | Python | 64,236 | 高 API 层深度学习框架，适合快速原型与教学，在 2026 年仍保持稳定开发者基础。 |
| [NVlabs/Sana](https://github.com/NVlabs/Sana) | Python | 8,780 | NVIDIA 的高效图像生成扩散模型，主打高分辨率与线性 Diffusion Transformer。代表图像生成模型权重的新进展。 |

### 🔍 RAG/知识库（向量数据库、检索增强、知识管理）

| 项目 | 语言 | Stars（总量 / 今日） | 简要说明 |
| :--- | :--- | ---: | :--- |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 152,721 | 可视化构建 Agentic Workflow 和 RAG 流水线的 LLMOps 平台。可私有化部署，是 RAG 应用最主流入口之一。 |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 88,678 | 领先的开源 RAG 引擎，将检索增强与 Agent 能力融合。面向深度文档理解场景，是同赛道头部项目。 |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 51,707 | 数据连接与 RAG 编排框架，支持文档 Agent 与 OCR 平台。处于“文档到知识”管线的核心位置。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 63,466 | AI Agent 的通用记忆层，为跨会话交互保留长期记忆。Agent 记忆主题的代表项目。 |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 45,666 | 云原生向量数据库，面向大规模向量近似搜索。是 RAG 架构中最常用的存储底座之一。 |
| [qdrant/qdrant](https://github.com/qdrant/qdrant) | Rust | 34,029 | 高性能向量数据库，专为 AI/下一代搜索引擎设计。Rust 实现使其在性能与资源占用上具有明显优势。 |
| [topoteretes/cognee](https://github.com/topoteretes/cognee) | Python | 30,083 | 开源 AI 记忆平台，用知识图谱为 Agent 提供持久长期记忆。与 ai-memory 同属 Agent 记忆技术栈。 |
| [akitaonrails/ai-memory](https://github.com/akitaonrails/ai-memory) | Rust | —（+207） | 为 Coding CLI Agent 提供长期记忆，并支持不同 Agent 供应商之间的无缝切换。今日 Trending 新面孔。 |

---

## 3. 趋势信号分析

从今日热榜看，AI 视频生成仍是社区最直接的流量入口，MoneyPrinterTurbo 单日新增 1,275 Stars 位居第一；但更值得关注的是 **AI 安全/红队工具首次集中登榜**，strix 与 Anthropic-Cybersecurity-Skills 分别新增 656 和 156 Stars，说明 Agent 规模扩大后，安全与防护需求正在变成刚需。

与此同时，llmfit、omlx 等本地推理/硬件选型工具上榜，配合此前的 vLLM、Ollama 生态，反映出开发者对可控、低成本、私有模型部署路径愈发重视。ai-memory 的登榜则指向 **Agent 记忆和跨厂商切换** 这个新痛点，未来记忆层可能成为类似 RAG 的基础设施。

整体来看，今日热点正从“造模型”转向“用 Agent”，并沿 **安全、记忆、成本** 三个方向快速分化。Rust 在 AI 工具链中的出现频率也在上升，RTK、llmfit、qdrant 等高性能基础设施项目正在形成新的技术偏好。

---

## 4. 社区关注热点

- **AI 安全/红队工具**：strix 单日 +656 Stars，Anthropic-Cybersecurity-Skills +156 Stars，说明 Agent 安全和漏洞自动化修复成为新刚需。
- **Agent 记忆层**：ai-memory 今日登榜，Mem0、Cognee、claude-mem 持续活跃。跨 Agent 供应商的长期记忆与“交接”能力正在成为 Agent 基础设施的重要组成。
- **本地模型选型与推理**：llmfit 解决“我的硬件能跑哪些模型”，omlx 则深耕 Apple Silicon 推理。本地化、低资源部署是当前最确定的增长方向之一。
- **LLM Token 成本控制**：rtk、headroom、caveman 等项目持续受到关注，“花更少 token 完成同样任务”已成为 AI 工程化的重要课题。
- **AI 视频生成应用**：MoneyPrinterTurbo 登顶今日热榜，OpenMontage、clipforge、ArcReel 等 Agent 驱动视频生产项目也在快速演进，内容生产自动化赛道仍有大量机会。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*