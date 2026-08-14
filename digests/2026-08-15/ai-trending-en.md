# AI Open Source Trends 2026-08-15

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-08-14 23:14 UTC

---

# AI Open Source Trends Report — 2026-08-15

*Note: For projects appearing in both Trending and Topic Search, totals are taken from Topic Search and today's stars from Trending. Trending-only entries may show 0 for total because the trending list supplied only daily star deltas.*

---

## 1. Today's Highlights

Today's AI open-source activity is overwhelmingly about **agent harnesses, agent skills, and context/memory layers** rather than raw model releases. The top trending repo, `cathrynlavery/diagram-design`, gained +3,651 stars by packaging 29 diagram types as a Claude Code skill — a strong sign that "skills" are becoming the new distribution format for AI workflows. Infrastructure for persistent memory and graph-native context is also accelerating, with `semantica-agi/semantica` at +1,183 stars today. Meanwhile, local/edge AI momentum is visible in `cactus-compute/needle` (+661), a 14MB foundation model for tiny devices, and `unslothai/unsloth` (+502), which now covers a broad range of the latest open-weight LLMs and diffusion models. Overall, developers are racing to build the operating layer for agents: memory, browser state, MCP integrations, token efficiency, and reusable skills.

---

## 2. Top Projects by Category

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 178,545 | Local LLM runtime now supporting Kimi-K2.6, GLM-5.2, MiniMax, DeepSeek, gpt-oss, Qwen, Gemma and more. Remains the default self-hosted inference layer for agent and RAG stacks. |
| [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) | TypeScript | 167,434 | Context API to search, scrape, and interact with the web at scale. Increasingly the data ingestion layer for LLM-powered research, agents, and RAG pipelines. |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 164,106 | The model-definition framework for state-of-the-art text, vision, audio, and multimodal models. Fast absorption of new open weights into this library continues to drive the whole ecosystem. |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 102,375 | Core dynamic neural network framework with strong GPU acceleration. Foundation for many fine-tuning and agent models in this report. |
| [rtk-ai/rtk](https://github.com/rtk-ai/rtk) | Rust | 76,168 | CLI proxy that reduces LLM token consumption by 60–90% on common dev commands. Zero-dependency Rust binary; a strong response to the cost pressure in agent loops. |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 66,368 | Compresses tool outputs, logs, files, and RAG chunks before they reach the LLM. 60–95% token reduction for JSON is a standout efficiency signal. |
| [semantica-agi/semantica](https://github.com/semantica-agi/semantica) | Python | 0 (+1,183 today) | Graph-native infrastructure for context and accountable AI systems. +1,183 today signals fast-growing interest in verifiable memory/context layers. |
| [citrolabs/ego-lite](https://github.com/citrolabs/ego-lite) | JavaScript | 0 (+153 today) | Browser built for AI agents to run automation with shared logged-in browser state. New category emerging: agent-native browsing without disturbing the user. |

---

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 240,165 | Agent harness performance system with skills, instincts, memory, security, and research-first development for Claude Code, Codex, Cursor, and beyond. Highest-starred LLM agent tooling in today's topic search. |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | Python | 230,638 | An agent that "grows with you," reflecting strong community interest in adaptive agent harnesses. One of the largest AI-agent repos tracked in the LLM topic. |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 186,624 | Pioneering accessible autonomous agent platform. Continues to be a reference for multi-step automation and agent-as-a-service thinking. |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 144,262 | Agent engineering platform providing memory, tools, model wrappers, and orchestration. Still the de facto framework for production agent workflows. |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 109,245 | Makes websites accessible to AI agents for browser automation. Complementary to ego-lite and the general shift toward agent-first browsing. |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | TypeScript | 84,056 | AI-driven development platform for building and running coding agents. Strong signal for agentic software engineering as a mainstream use case. |
| [holaboss-ai/holaOS](https://github.com/holaboss-ai/holaOS) | TypeScript | 0 (+769 today) | All-in-one AI agent workspace with 100+ integrations, MCP support, shared memory, and BYOK. +769 today shows strong demand for unified agent control planes. |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | 0 (+3,651 today) | 29 editorial diagram types packaged for Claude Code as self-contained HTML/SVG. Biggest star-gainer in today's trending list, proving agent-skill distribution is a viral category. |

---

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 148,802 | User-friendly AI interface supporting Ollama and OpenAI-compatible APIs. Key front-door for self-hosted and local-first LLM deployments. |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 103,559 | Generates HD short videos from a topic or keyword using AI workflows and automation. High-starred example of agentic content production. |
| [santifer/career-ops](https://github.com/santifer/career-ops) | JavaScript | 63,851 | Open-source AI job search: scans job portals, evaluates listings with an A–F rubric, tailors CVs, and tracks applications. Demonstrates vertical agent applications. |
| [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) | Python | 62,877 | LLM-powered multi-market stock analysis with real-time news, decision dashboards, and automated notifications. Popular example of domain-specific AI agent dashboards. |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 50,478 | AI productivity studio with smart chat, autonomous agents, and 300+ assistants. Unifies access to frontier LLMs in one desktop application. |
| [calesthio/OpenMontage](https://github.com/calesthio/OpenMontage) | Python | 48,106 | Open-source agentic video production system with 12 pipelines, 100+ tools, and 700+ agent-skill files. Pushes AI coding assistants into full video-studio territory. |
| [macro-inc/macro](https://github.com/macro-inc/macro) | Rust | 0 (+435 today) | Unified workspace for email, chat, docs, tasks, agents, calls, and CRM, with shared AI memory. +435 today marks a new entrant in AI-native collaboration. |
| [lightningpixel/modly](https://github.com/lightningpixel/modly) | TypeScript | 0 (+580 today) | Desktop app for generating 3D models from images or prompts using local AI on GPU. Privacy-first 3D asset generation is an emerging niche. |

---

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [cactus-compute/needle](https://github.com/cactus-compute/needle) | Python | 0 (+661 today) | 14MB foundation model for phones, wearables, smart home, and robots. Edge-model momentum is unusually strong today. |
| [unslothai/unsloth](https://github.com/unslothai/unsloth) | Python | 0 (+502 today) | Local UI to run and train LLMs and diffusion models, including Qwen3.8, Kimi K3, MiniMax-H3, Gemma 4, DeepSeek-V4, and FLUX. Key tool for local fine-tuning workflows. |
| [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) | Jupyter Notebook | 102,666 | Step-by-step PyTorch implementation of a ChatGPT-like LLM from scratch. Essential educational resource as custom model training becomes more accessible. |
| [NVlabs/Sana](https://github.com/NVlabs/Sana) | Python | 8,766 | Efficient high-resolution image synthesis with a linear diffusion transformer. Reference architecture for fast generative image models. |

---

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 106,367 | Turns codebases, docs, SQL schemas, configs, and PDFs into queryable knowledge graphs via local AST parsing — no vector store. High-starred alternative to classical vector RAG. |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | JavaScript | 90,764 | Captures every agent session, compresses it with AI, and injects relevant context into future sessions. Persistent context is the hottest agent-memory pattern today. |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 88,387 (+474 today) | Leading open-source RAG engine fusing retrieval-augmented generation with agent capabilities. +474 today keeps it firmly in the Trending list. |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 64,716 | Local-first all-in-one agent/RAG experience with document management and vector storage. "Own your intelligence" positioning continues to resonate. |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 63,270 | Universal memory layer for AI agents, providing persistent long-term memory across sessions. Critical component for stateful agent applications. |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 51,641 | Document agent and OCR platform plus RAG data framework. Broad adoption across production retrieval stacks. |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 45,637 | High-performance, cloud-native vector database built for scalable vector ANN search. Standard backbone for high-scale RAG. |
| [qdrant/qdrant](https://github.com/qdrant/qdrant) | Rust | 33,977 | High-performance vector database and vector search engine for next-generation AI. Rust-based performance makes it a strong RAG backend. |

---

## 3. Trend Signal Analysis

Today's list is dominated by **agent harnesses, skills, and context layers** rather than raw model weight releases. The biggest star-gainer, `cathrynlavery/diagram-design` (+3,651), is a content pack for Claude Code; `holaboss-ai/holaOS` (+769) and `macro-inc/macro` (+435) show the race to become the "agent operating system" / workspace. `semantica-agi/semantica` (+1,183) signals graph-native memory as the next foundation layer. Token economics is also central: `rtk-ai/rtk` and `headroomlabs-ai/headroom` are increasingly popular because long-horizon agent loops make context compression a necessity.

A new stack is forming around **agent infrastructure**: shared browser state (`citrolabs/ego-lite`), MCP security gateways (`apache/casbin-gateway`), and official plugin specifications (`cursor/plugins`). Edge AI is emerging in a big way: `cactus-compute/needle` is a 14MB foundation model, and `Picovoice/picollm` brings on-device X-bit quantization. Local run/train UIs such as `unslothai/unsloth` and `ollama/ollama` are already updated for the latest open-weight releases — Kimi-K2.6, GLM-5.2, gpt-oss, DeepSeek, Qwen3.8, Gemma 4, DeepSeek-V4 — indicating that recent model waves are being absorbed into local/agent stacks very quickly.

The connection to industry events is clear: as frontier labs release smaller, permissively-licensed models, the center of gravity shifts from model access to **orchestration, memory, and application packaging**. The most explosive community attention is not on any single model but on making agents reliable, cheap, and genuinely useful.

---

## 4. Community Hot Spots

- **Claude Code / agent-skill ecosystem** — `cathrynlavery/diagram-design` (+3,651), `affaan-m/ECC`, `ComposioHQ/awesome-claude-skills`, and `deepseek-ai/awesome-deepseek-agent` (+203) show that "skills" are becoming the new plugin format for AI coding agents.

- **AI memory / context layer** — `thedotmack/claude-mem`, `mem0ai/mem0`, `topoteretes/cognee`, and `semantica-agi/semantica` are all pushing persistent memory and graph-based context for long-running agents.

- **Local & edge inference** — `ollama/ollama`, `unslothai/unsloth`, `cactus-compute/needle`, and `Picovoice/picollm` reflect strong demand for private, zero-latency deployment of the latest open-weight models.

- **Agentic media generation** — `calesthio/OpenMontage`, `harry0703/MoneyPrinterTurbo`, `ArcReel/ArcReel`, and `lightningpixel/modly` show that AI-driven video and 3D content production is one of the highest-velocity application areas.

- **Token cost optimization** — `rtk-ai/rtk`, `headroomlabs-ai/headroom`, and `JuliusBrussee/caveman` highlight a new efficiency layer: reducing token spend in agent loops without sacrificing output quality.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*