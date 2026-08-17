# AI Open Source Trends 2026-08-18

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-08-17 23:16 UTC

---

Filtered to AI/ML-relevant repositories only. Non-AI trending repos (e.g. Motrix, cordis, immich, nautilus_trader) and non-AI topic-search dev tools (e.g. Puppeteer, Bruno, Files, Yazi) were excluded. For repos appearing in both Trending and Topic Search, total stars are taken from Topic Search and today’s delta from Trending.

## 1. Today's Highlights

On 2026-08-18, AI video generation is the biggest momentum story: MoneyPrinterTurbo gained +1275 today, while OpenMontage and Open-Generative-AI keep pushing agent-driven video production forward. Agent memory and portability are also crystallizing as a core layer — ai-memory (+207) and claude-mem both target persistent context and cross-vendor handoff between coding agents. Security for AI agents is suddenly mainstream, with strix (+656) and Anthropic-Cybersecurity-Skills (+156, mapped to NIST/MITRE frameworks) attracting strong attention. Finally, local/on-device LLM tooling is accelerating: llmfit (+239) solves the “which model fits my hardware?” problem, while omlx (+96) brings a Mac-menu-bar inference server with continuous batching to Apple Silicon.

## 2. Top Projects by Category

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [vllm-project/vllm](https://github.com/vllm-project/vllm) | Python | 89,275 | High-throughput, memory-efficient LLM inference and serving engine. It remains the default open-source runtime for production LLM deployments. |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 178,807 | Local LLM runtime now supporting Kimi-K2.6, GLM-5.2, MiniMax, DeepSeek, gpt-oss, Qwen and more. Its README directly reflects the current wave of open-weight releases. |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 164,195 | State-of-the-art model-definition framework for text, vision, audio and multimodal models. It remains the central integration point for open-source model adoption. |
| [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | Rust | 76,404 | CLI proxy that reduces LLM token consumption by 60–90% on common dev commands. Its popularity signals that token cost optimization is now core AI infrastructure. |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 66,667 | Compresses tool outputs, logs, files and RAG chunks before they reach the LLM. Claims 20% fewer tokens for coding agents and 60–95% fewer tokens for JSON. |
| [AlexsJones/llmfit](https://github.com/AlexsJones/llmfit) | Rust | 0 (+239) | Finds which LLMs and providers can actually run on a user’s hardware with one command. The +239 today shows how painful local model selection has become. |
| [jundot/omlx](https://github.com/jundot/omlx) | Python | 0 (+96) | LLM inference server with continuous batching and SSD caching for Apple Silicon, managed from the macOS menu bar. Brings datacenter-style serving closer to consumer Macs. |
| [0xPlaygrounds/rig](https://github.com/0xPlaygrounds/rig) | Rust | 8,300 | Modular Rust framework for building LLM applications. It represents a growing Rust-native stack for AI tooling. |

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 240,695 | Agent harness performance optimization system with skills, instincts, memory and security for Claude Code, Codex, Cursor and beyond. Its huge star count makes it a reference point for agent operations. |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | Python | 232,003 | “The agent that grows with you,” emphasizing personalized, evolving agent behavior. It shows massive demand for adaptable personal AI agents. |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 186,655 | Vision-driven autonomous agent platform. It remains a bellwether for the open-source agent ecosystem. |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 144,414 | The agent engineering platform. It underpins a large share of RAG and agent applications in this data set. |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | JavaScript | 91,011 | Captures agent sessions, compresses them with AI, and injects relevant context back into future sessions. It works across Claude Code, Codex, Gemini, Copilot, OpenCode and more. |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | TypeScript | 84,333 | AI-driven development platform. One of the strongest open-source coding agent communities. |
| [Eigenwise/atomic-agents](https://github.com/Eigenwise/atomic-agents) | Python | 6,185 | Modular framework for building AI agents “atomically.” It represents a componentized alternative to monolithic agent frameworks. |
| [akitaonrails/ai-memory](https://github.com/akitaonrails/ai-memory) | Rust | 0 (+207) | Long-term memory solution for agent coding CLIs, designed to facilitate handoff between different agent vendors. The +207 today signals memory portability is a hot new problem. |

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 152,721 | Build agentic workflows and RAG pipelines on one collaborative workspace. It is the most-starred RAG/agent workflow platform in this dataset. |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 107,494 | Turns codebases, docs, SQL schemas and PDFs into queryable knowledge graphs with deterministic AST parsing and no vector store. A strong signal that “vectorless RAG” is gaining traction. |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 88,678 | Leading open-source RAG engine that fuses retrieval-augmented generation with agent capabilities. It positions itself as a superior context layer for LLMs. |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 51,707 | Leading document agent and OCR platform. Core infrastructure for RAG and document-centric AI workflows. |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 45,666 | High-performance, cloud-native vector database for scalable ANN search. It remains a mainstay for production vector retrieval. |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 63,466 | Universal memory layer for AI agents. It sits at the intersection of RAG, memory and persistent knowledge. |
| [qdrant/qdrant](https://github.com/qdrant/qdrant) | Rust | 34,029 | High-performance vector database and vector search engine. Frequently used as the retrieval backbone for RAG applications. |
| [topoteretes/cognee](https://github.com/topoteretes/cognee) | Python | 30,083 | Open-source AI memory platform for agents, backed by a self-hosted knowledge graph engine. It complements vector DBs with graph-based entity memory. |

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 105,933 (+1275) | AI-powered automated workflow that generates HD short videos from a topic or keyword. +1275 today makes it the clear AI winner on the trending list. |
| [usestrix/strix](https://github.com/usestrix/strix) | Python | 0 (+656) | Open-source AI penetration testing tool to find and fix application vulnerabilities. The +656 today shows surging interest in AI security and red-teaming. |
| [santifer/career-ops](https://github.com/santifer/career-ops) | JavaScript | 64,586 (+147) | AI job search tool that scans job portals, scores listings with an A–F rubric, tailors CVs and tracks applications locally. It runs inside Claude Code, Codex, OpenCode and similar AI coding CLIs. |
| [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) | Python | 0 (+156) | 817 structured cybersecurity skills for AI agents, mapped to MITRE ATT&CK, NIST CSF 2.0, MITRE ATLAS, D3FEND, NIST AI RMF and MITRE F3. It directly productizes agent security skills and compliance frameworks. |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 50,663 | AI productivity studio with smart chat, autonomous agents and 300+ assistants. It represents the consumer-facing AI workspace trend. |
| [calesthio/OpenMontage](https://github.com/calesthio/OpenMontage) | Python | 48,595 | Open-source, agentic video production system with 12 production pipelines and 100+ tools. It turns an AI coding assistant into a full video production studio. |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | Python | 47,490 | Turns documents or topics into native PowerPoint decks with shapes, transitions, charts, audio narration and templates. Demonstrates strong document-to-content automation demand. |
| [Anil-matcha/Open-Generative-AI](https://github.com/Anil-matcha/Open-Generative-AI) | JavaScript | 26,571 | Self-hosted, unrestricted open-source alternative to AI video platforms with 500+ models. MIT-licensed local deployment makes it popular among community users. |

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [ultralytics/ultralytics](https://github.com/ultralytics/ultralytics) | Python | 60,696 | YOLO26, YOLO11 and YOLOv8 training/inference suite. It remains the most-used open-source toolkit for computer vision model training. |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,310 | LLM evaluation platform supporting 100+ datasets and a wide range of open and proprietary models. Evaluation infrastructure is becoming critical as open models proliferate. |
| [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) | Python | 4,497 | Educational project to build a tiny vLLM + Qwen inference system on Apple Silicon. It signals growing interest from systems engineers in understanding LLM inference internals. |
| [NVlabs/Sana](https://github.com/NVlabs/Sana) | Python | 8,780 | Efficient high-resolution image synthesis with a linear diffusion transformer. It is a notable open visual-generation model with strong efficiency characteristics. |

## 3. Trend Signal Analysis

Today’s data shows explosive community attention on the **agent operational layer** rather than raw model training. The top AI gainers are tools that make agents cheaper, safer and easier to run locally: strix for AI penetration testing, llmfit for hardware matching, ai-memory for cross-vendor memory, and MoneyPrinterTurbo for end-to-end AI content creation. Token efficiency has become a category of its own — rtk and headroom reduce token consumption by large margins, while caveman famously cuts token usage through compressed style.

New directions are appearing for the first time with meaningful traction. First, “agent memory as a portable abstraction”: ai-memory explicitly targets handoff between Claude Code, Copilot, Codex, Cursor and Gemini CLI. Second, local/on-device inference is moving from generic servers to personal hardware: omlx serves Apple Silicon, picollm focuses on on-device quantization, and tiny-llm teaches inference systems engineering. Third, RAG is splitting into “vectorless” knowledge-graph approaches — Graphify and VectifyAI/PageIndex emphasize explainable edges over opaque vector retrieval.

These trends connect directly to the open-weight release cycle. Ollama’s support list — Kimi, GLM, DeepSeek, gpt-oss, Qwen — reflects continuous model fragmentation, which is precisely why llmfit and omlx are gaining users. At the same time, Anthropic-Cybersecurity-Skills maps agent abilities to NIST AI RMF and MITRE ATLAS, showing that AI security frameworks are becoming implemented engineering standards rather than policy documents. Specialist agent harnesses such as DeepSeek-Reasonix also suggest vendor-specific coding agents are consolidating around popular model families.

## 4. Community Hot Spots

- **Agent memory and handoff**: [ai-memory](https://github.com/akitaonrails/ai-memory), [claude-mem](https://github.com/thedotmack/claude-mem), [mem0](https://github.com/mem0ai/mem0), [cognee](https://github.com/topoteretes/cognee). Multi-session coding agents need durable memory, and cross-vendor handoff is now an explicit design goal.

- **AI security and red-teaming**: [strix](https://github.com/usestrix/strix), [Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills), [casbin-gateway](https://github.com/apache/casbin-gateway). As agents gain autonomy, security is becoming the next bottleneck — and today’s star growth confirms it.

- **Local/on-device inference**: [llmfit](https://github.com/AlexsJones/llmfit), [omlx](https://github.com/jundot/omlx), [picollm](https://github.com/Picovoice/picollm), [tiny-llm](https://github.com/skyzh/tiny-llm). The open-model ecosystem is fragmenting fast, so hardware-aware selection and Apple Silicon serving are increasingly valuable.

- **AI video generation**: [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo), [OpenMontage](https://github.com/calesthio/OpenMontage), [Open-Generative-AI](https://github.com/Anil-matcha/Open-Generative-AI), [ArcReel](https://github.com/ArcReel/ArcReel). Automated short-form video workflows are monetizable, creator-focused, and increasingly dominated by open-source tooling.

- **Token/cost optimization**: [rtk](https://github.com/rtk-ai/rtk), [headroom](https://github.com/headroomlabs-ai/headroom), [caveman](https://github.com/JuliusBrussee/caveman). Agent usage at scale makes token spend one of the top blockers; today’s data shows a fast-growing toolkit around reducing it.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*