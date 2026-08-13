# AI Open Source Trends 2026-08-14

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-08-13 23:34 UTC

---

## Filtering Note

From the 17 trending repos and 106 topic-search results, I excluded non-AI/general-purpose projects: OSINT/security tools, browser automation, file managers, API testers, generic data frameworks, animation engines, and non-ML programming languages. The remaining AI/ML projects are grouped below by primary category; multi-category projects are placed where they are most representative.

---

## Today's Highlights

Several related moves dominated today: Agent Skills went mainstream, with `diagram-design` (+4504) and Anthropic’s official skills repo (+383) leading the fastest-growing niche. `macro` (+1180) and `holaOS` (+380) show that shared-memory agent workspaces are the next battleground. On the model side, `cactus-compute/needle` (+768) demonstrated that a 14MB foundation model can fit tiny edge devices, while NVIDIA’s `Switchyard` (+408) gives LLM apps a portable routing layer. RAGFlow (+473) remains the strongest RAG performer, and LTX-2 (+201) brings open audio-video generation tooling to PyTorch users.

---

## Top Projects by Category

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 164,078 | The de-facto framework for state-of-the-art models in text, vision, audio, and multimodal. Foundational for both inference and training in almost every LLM stack. |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 102,359 | Core deep learning framework with strong GPU acceleration. Continues to be the base for many AI research and production workloads. |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 178,477 | Simplest local runtime for open-weight models like Kimi, DeepSeek, Qwen, and Gemma. Still the default entry point for running LLMs locally. |
| [NVIDIA-NeMo/Switchyard](https://github.com/NVIDIA-NeMo/Switchyard) | Rust | 0 (+408) | API-compatible traffic router for LLM apps across models/providers, preserving native OpenAI/Anthropic compatibility. Enables cost/performance switching and benchmarking without code changes. |
| [unslothai/unsloth](https://github.com/unslothai/unsloth) | Python | 0 (+354) | Local UI to run and train LLMs and diffusion models, including Qwen3.8, Kimi K3, DeepSeek-V4, and FLUX. Extends Unsloth’s training optimizations to a broader end-user audience. |
| [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | Rust | 76,032 | CLI proxy that cuts LLM token consumption 60–90% on common dev commands. A zero-dependency Rust binary, showing rising demand for token-cost control. |
| [semantica-agi/semantica](https://github.com/semantica-agi/semantica) | Python | 0 (+727) | Graph-native infrastructure for context and accountable AI systems. Early momentum (+727 today) suggests interest in alternatives to vector-only memory. |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 66,230 | Compresses tool outputs, logs, files, and RAG chunks before they reach the LLM. Reduces tokens by 20–95% without changing answers. |

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [anthropics/skills](https://github.com/anthropics/skills) | Python | 0 (+383) | Official public repo for Agent Skills. Signals a standardized, shareable skill artifact for Claude and other agents. |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | 0 (+4504) | Self-contained HTML/SVG diagram types for Claude Code. Today’s top momentum (+4504), showing that vertical agent skills can go viral. |
| [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) |  | 0 (+411) | Agent skills for Obsidian, teaching agents to use Obsidian CLI and open formats like Markdown, Bases, and JSON Canvas. Brings the skills pattern to knowledge management. |
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | Shell | 0 (+762) | Collection of specialized AI agents for a complete “AI agency,” from frontend wizards to Reddit community ninjas. +762 today shows demand for ready-made agent roles. |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | TypeScript | 83,951 | AI-driven development platform. One of the most-established autonomous coding agent projects. |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 144,185 | The agent engineering platform for context-aware, reasoning applications. Remains central in agent orchestration. |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 152,367 | Full-stack platform for agentic workflows and RAG pipelines with rich model/tool support. A top choice for production agent apps. |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 109,118 | Makes websites accessible to AI agents for online task automation. Key layer for web-native agents. |

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [macro-inc/macro](https://github.com/macro-inc/macro) | Rust | 0 (+1180) | Unified team workspace: email, chat, docs, tasks, agents, calls, and CRM, @-linked with shared AI memory. The biggest non-skill trending project today. |
| [holaboss-ai/holaOS](https://github.com/holaboss-ai/holaOS) | TypeScript | 0 (+380) | Open-source all-in-one AI agent workspace. Runs agents like Claude Code and Codex across 100+ integrations and MCP with shared memory. |
| [altic-dev/FluidVoice](https://github.com/altic-dev/FluidVoice) | Swift | 0 (+187) | Fast macOS dictation app with on-device STT and a custom AI enhancement model. A local Wispr Flow alternative, highlighting edge AI in productivity. |
| [lightningpixel/modly](https://github.com/lightningpixel/modly) | TypeScript | 0 (+221) | Desktop app that generates 3D models from images using local AI entirely on GPU. A new local generative-3D use case. |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 148,713 | User-friendly AI interface supporting Ollama and OpenAI APIs. A leading self-hosted chat UI. |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 50,425 | AI productivity studio with smart chat, autonomous agents, and 300+ assistants. Strong all-in-one client for frontier LLMs. |
| [calesthio/OpenMontage](https://github.com/calesthio/OpenMontage) | Python | 47,963 | Open-source agentic video production system with 12 production pipelines and 700+ agent skill files. Turns a coding agent into a full video studio. |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 103,105 | Generates HD short videos from a topic or keyword using AI models and automated workflows. Represents the booming AI short-video application space. |

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [cactus-compute/needle](https://github.com/cactus-compute/needle) | Python | 0 (+768) | A 14MB foundation model for tiny devices: phones, wearables, smart home, and robots. Strong signal for on-device/small-model AI. |
| [Lightricks/LTX-2](https://github.com/Lightricks/LTX-2) | Python | 0 (+201) | Official Python inference and LoRA trainer package for the LTX-2 audio–video generative model. Direct tooling for open audio-video generation. |
| [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) | Jupyter Notebook | 102,609 | Step-by-step PyTorch implementation of a ChatGPT-like LLM. A canonical learning resource as new developers enter LLM engineering. |
| [NVlabs/Sana](https://github.com/NVlabs/Sana) | Python | 8,758 | Efficient high-resolution image synthesis with a linear diffusion transformer. High-impact research code from NVIDIA. |
| [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) | Python | 4,483 | Learn LLM inference on Apple Silicon by building a tiny vLLM + Qwen. Education for systems-level AI engineering. |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,299 | LLM evaluation platform supporting 100+ datasets and many model families. Essential for benchmarking in a crowded open-model landscape. |
| [AarambhDevHub/aarambh-studio](https://github.com/AarambhDevHub/aarambh-studio) | Rust | 76 | Decoder-only LLM built from scratch in pure Rust/Candle with Gated DeltaNet, sparse attention, and fine-grained MoE. A novel Rust-native LLM stack. |
| [thinkwee/AgentsMeetRL](https://github.com/thinkwee/AgentsMeetRL) | HTML | 1,779 | Awesome list for agentic reinforcement learning. Tracks the emerging convergence of RL and agent training. |

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 88,001 (+473) | Leading open-source RAG engine fusing RAG with agent capabilities for a superior LLM context layer. Strong today’s momentum (+473). |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 106,027 | Turns codebases, docs, schemas, and PDFs into a queryable knowledge graph. Provides a `/graphify` skill for Claude Code, Cursor, Codex, and Gemini CLI. |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 51,623 | Leading document agent and OCR platform for RAG. Continues to define the data framework for LLM applications. |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 45,628 | High-performance, cloud-native vector database for scalable vector ANN search. Core infrastructure for RAG at scale. |
| [qdrant/qdrant](https://github.com/qdrant/qdrant) | Rust | 33,965 | High-performance vector database and search engine. A popular choice for production RAG workloads. |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | JavaScript | 90,650 | Captures everything agents do during sessions, compresses it with AI, and injects relevant context later. Persistent memory for Claude Code, Codex, Gemini, and more. |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 63,210 | Universal memory layer for AI agents, enabling long-term memory across sessions. Increasingly central to agent workflows. |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 64,698 | Local-first “rent nothing” agent experience: everything needed for local AI, RAG, and document interaction. A strong self-hosted alternative. |

---

## Trend Signal Analysis

The day’s biggest signal is the rise of the “agent skill” as a shareable software artifact. `diagram-design` (+4504 today), `obsidian-skills` (+411), and Anthropic’s official skills repo are not new model or framework projects — they package specialized agent behavior. This suggests the community is moving from monolithic agent platforms to composable, installable capabilities. At the same time, `macro` (+1180) and `holaOS` (+380) compete for the “unified agent workspace” slot, adding shared AI memory across email, docs, chat, and tools.

Two genuinely new directions stand out. First, graph-native and “vectorless” context: `semantica` (+727) and `Graphify` build graph infrastructure for accountable AI, while `PageIndex` proposes reasoning-based RAG without vector stores. This challenges the dominant vector-DB stack. Second, tiny on-device models: `needle`’s 14MB foundation model and `FluidVoice`’s on-device STT push AI into phones and desktops, aligning with privacy and latency demands.

Finally, NVIDIA’s `Switchyard` (+408) and tools like `rtk` and `headroom` point to a maturing “LLM gateway” layer: route traffic across Qwen, Kimi, DeepSeek, Gemma, and other open-weight releases while optimizing token costs. Following the recent open-model release wave, the community is now building the pipelines, skills, memory, and routing infrastructure around them.

---

## Community Hot Spots

- **Agent Skills as a distribution format** — [anthropics/skills](https://github.com/anthropics/skills), [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design), and [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills). The official Anthropic repo plus massive skill-specific star counts signal a low-friction way to package agent expertise.

- **Shared context / agent memory** — [macro-inc/macro](https://github.com/macro-inc/macro), [holaboss-ai/holaOS](https://github.com/holaboss-ai/holaOS), and [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem). Long-running agents need cross-session memory; this is becoming the core moat for agent workspaces.

- **Edge and on-device AI** — [cactus-compute/needle](https://github.com/cactus-compute/needle), [altic-dev/FluidVoice](https://github.com/altic-dev/FluidVoice), and [lightningpixel/modly](https://github.com/lightningpixel/modly). The push toward small, private, local-first AI is accelerating beyond chatbots into dictation and 3D generation.

- **LLM routing and token-cost control** — [NVIDIA-NeMo/Switchyard](https://github.com/NVIDIA-NeMo/Switchyard), [rtk-ai/rtk](https://github.com/rtk-ai/rtk), and [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom). As model choices multiply, developers need API-compatible gateways and compression layers to control cost.

- **RAG moving beyond vector search** — [infiniflow/ragflow](https://github.com/infiniflow/ragflow), [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify), and [mem0ai/mem0](https://github.com/mem0ai/mem0). Knowledge graphs, memory layers, and reasoning-based retrieval are becoming first-class alternatives to raw vector databases.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*