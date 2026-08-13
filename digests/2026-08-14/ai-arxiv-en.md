# ArXiv AI Research Digest 2026-08-14

> Source: [ArXiv](https://arxiv.org/) (cs.AI, cs.CL, cs.LG) | 50 papers | Generated: 2026-08-13 23:34 UTC

---

## Today's Highlights

Today’s papers challenge several core assumptions in AI research: long-context training can undermine parametric knowledge, LLM rankings are not stable across inference budgets, and using a single frozen LLM simulator in multi-agent RL leads to policies that fail to generalize. At the same time, agentic systems are moving toward production-scale workloads—legacy HPC modernization, enterprise API+retrieval reasoning, and structured spatial reasoning. Retrieval-augmented generation is becoming more structured and multimodal, with SQL-aware retrieval and visual key-value cache reuse. Test-time capability transfer and process-reward training are emerging as practical alternatives to retraining. Several high-impact applications also stand out in clinical RAG, drug-combination screening, and scientific diagram understanding.

## Key Papers

### 🧠 Large Language Models

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Information Abundance Paradox: Long-Context Training Undermines Parametric Knowledge](http://arxiv.org/abs/2608.12218v1) | Arda Uzunoglu, Benjamin van Durme, Daniel Khashabi | Challenges the assumption that longer training contexts are always beneficial by showing they can weaken stored parametric knowledge. This has major implications for how future LLMs balance in-context evidence versus learned memory. |
| [Who Thinks Best Depends on How Long You Let Them: Budget-Dependent Rankings in LLM Evaluation](http://arxiv.org/abs/2608.12150v1) | Rodrigo Guedes de Souza, Alison R. Panisson | Varies token-generation budgets from 64 to 4,096 tokens across models and reasoning benchmarks, showing that model rankings shift substantially. The results question fixed-budget leaderboards and motivate budget-aware evaluation protocols. |
| [Massive Activations in Hybrid Linear Attention Large Language Models: Pre-Attention Spikes and Inter-Spike Plateaus](http://arxiv.org/abs/2608.12149v1) | Zunhai Su, Bohan Sun, Xialie Zhuang et al. | Provides the first systematic study of massive activations in layer-interleaved hybrid linear-attention LLMs. Identifies pre-attention spikes and inter-spike plateaus, which are important for interpretability, quantization, and future architecture design. |
| [AI4AI at Test-Time: Strong-to-Weak Capability Transfer via Harnesses](http://arxiv.org/abs/2608.12307v1) | Cheng Qian, Wenting Zhao, Liangwei Yang et al. | Proposes transferring capabilities from strong to weak models at test time using “harnesses,” without updating parameters. This offers a complementary path to distillation and reduces the need for expensive retraining. |

### 🤖 Agents & Reasoning

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [One Frozen Simulator Is Not Enough: Simulator Collapse in Multi-Agent RL](http://arxiv.org/abs/2608.12253v1) | Simon Yu, Nicholas Tomlin, Marwa Abdulhai et al. | Shows that multi-agent RL trained against a single frozen LLM simulator systematically fails to generalize due to “simulator collapse.” Argues for diverse or adaptive user simulators when training human-AI interaction policies. |
| [VAKRA: Evaluating Multi-Hop Reasoning Across APIs and Retrieval Under Tool-Use Policies](http://arxiv.org/abs/2608.12282v1) | Ankita Rajaram Naik, Anupama Murthi, Benjamin Elder et al. | Introduces a benchmark for enterprise agents that must reason across structured APIs and document collections under realistic tool-use policies. Fills a gap left by benchmarks that evaluate API reasoning and retrieval in isolation. |
| [An Agentic Workflow for Legacy HPC Modernization: Converting the Two-Electron-Integral Core of GAMESS](http://arxiv.org/abs/2608.12249v1) | Yuzhong Shen, Masha Sosonkina, Peng Xu et al. | Presents an agentic workflow for modernizing large legacy Fortran codebases at production scale, demonstrated on a core component of GAMESS. Shows that routine but voluminous code transformations can be automated with AI agents. |
| [SCOUT: Unlocking Enhanced Spatial Reasoning via Structured Chain-of-Thought and Multi-Objective Process Reward](http://arxiv.org/abs/2608.12220v1) | Zile Zhou, Huining Yuan, Weichen Zhang et al. | Combines structured chain-of-thought with multi-objective process reward modeling to improve spatial reasoning in vision-language models. Addresses credit assignment issues that arise in RL for verifiable spatial tasks. |

### 🔧 Methods & Frameworks

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [SAG: SQL-Retrieval Augmented Generation with Query-Time Dynamic Hyperedges](http://arxiv.org/abs/2608.12129v1) | Yuchao Wu, Junqin Li, XingCheng Liang et al. | Extends retrieval-augmented generation with SQL-structured retrieval and query-time dynamic hyperedges. Improves handling of structured constraints and multi-hop relational reasoning beyond dense retrieval. |
| [QV-PIC: Query-Aware Visual Position-Independent Caching for Efficient RAG Serving](http://arxiv.org/abs/2608.12121v1) | Yilin Liu, Rui Meng, Wangze Ni et al. | Proposes query-aware, position-independent KV-cache reuse for visual tokens in multimodal RAG serving. Reduces redundant prefill computation and improves serving efficiency for visual document workloads. |
| [HAMP-LIC: Hessian-Aware Mixed-Precision Post-Training Quantization for Learned Image Compression](http://arxiv.org/abs/2608.12239v1) | Yuefeng Zhang | Introduces a Hessian-aware mixed-precision PTQ framework for learned image compression models. Balances hardware heterogeneity and encoding-decoding mismatch while preserving rate-distortion performance. |

### 📊 Applications

| Paper | Authors | Summary |
| :--- | :--- | :--- |
| [Diagram-MMU: A Multi-Modal Benchmark for Scientific Diagrams](http://arxiv.org/abs/2608.12262v1) | Weihao Bo, Shan Zhang, Yanpeng Sun et al. | Presents a benchmark for evaluating MLLMs on scientific diagram understanding, including conversion to LaTeX TikZ. Targets the growing role of multimodal models in scientific writing and collaboration. |
| [How Organizations Use AI: Evidence from ChatGPT](http://arxiv.org/abs/2608.12236v1) | Aaron Chatterji, David Holtz, Neel Rakholia et al. | Links ChatGPT Enterprise account records to usage, worker roles, and financial data through March 2026. Provides privacy-preserving empirical evidence on adoption patterns and message-level usage inside real organizations. |
| [ScreenShot: A Foundation Model for Few-Shot Combination Drug Screening](http://arxiv.org/abs/2608.12219v1) | Antoine de Mathelin, Christopher Tosh, Wesley Tansey | Develops a foundation model for few-shot prediction of drug combination effects. Helps reduce the cost and infeasibility of large combinatorial drug screens. |
| [A corpus-specific clinical RAG system matches or outperforms newer frontier LLMs on HealthBench](http://arxiv.org/abs/2608.12138v1) | Praveen Reddy, Charuta Mandke, Suvrankar Datta et al. | Describes VITA, a corpus-specific retrieval-augmented clinical system evaluated on HealthBench. Shows that domain-specific RAG can match or beat newer frontier LLMs in medical settings. |

## Research Trend Signal

Several clear directions emerge from today’s submissions. First, evaluation is becoming more sensitivity-aware: model rankings are shown to depend on inference budgets, and long-context training is being scrutinized for side effects on parametric knowledge. Second, agentic systems are moving from prototypes to production-grade workflows—legacy HPC modernization, enterprise API+retrieval tasks, and structured spatial reasoning—while security and simulator diversity are emerging as first-class concerns. Third, RAG is evolving beyond flat text retrieval: SQL-structured hyperedges, query-aware visual caching, and corpus-specific clinical retrieval all point toward more specialized, efficient retrieval designs. Fourth, test-time computation is increasingly leveraged for capability transfer and reasoning, paralleling continued work in quantization and efficient architectures. Finally, high-impact applications are broadening into drug combination screening, organizational usage analysis, and scientific diagram understanding, signaling a shift from general chatbot capability toward measurable domain outcomes.

## Worth Deep Reading

1. **Information Abundance Paradox: Long-Context Training Undermines Parametric Knowledge** — This paper directly challenges a core scaling assumption in modern LLM development. Its findings could reshape decisions about context length, training data composition, and memory allocation in future models.

2. **One Frozen Simulator Is Not Enough: Simulator Collapse in Multi-Agent RL** — A concise but important negative result for multi-agent RL. It demonstrates a systematic failure mode when LLM simulators are used carelessly, with broad implications for human-AI interaction research and evaluation.

3. **Who Thinks Best Depends on How Long You Let Them: Budget-Dependent Rankings in LLM Evaluation** — This is a practical intervention in LLM evaluation methodology. Showing that rankings flip across token budgets is essential for anyone designing or interpreting model benchmarks, and it motivates more deployment-aware evaluation.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*