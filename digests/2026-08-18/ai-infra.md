# AI 基础设施日报 2026-08-18

> 生成时间: 2026-08-17 23:16 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施生态横向对比分析报告（2026-08-18）

## 1. 生态全景

当前 AI 基础设施进入 **"新模型发布驱动适配、稳定性问题集中爆发"** 的收敛期。DeepSeek-V4、Kimi-K3、Qwen3.x 等新一代模型（尤其是 MoE、MLA、Hybrid 架构）蜂拥而至，vLLM、SGLang、llama.cpp 三大推理引擎均面临大量与此相关的崩溃、挂起和性能退化问题，生态整体呈现 **"上游模型迭代速度远超下游推理引擎稳定化周期"** 的典型张力。硬件侧，AMD ROCm/gfx950、Intel XPU、Blackwell SM120 等新平台支持在各项目同步推进，成为差异化竞争重点。项目间竞争已从单一"支持多少模型"转向 **"谁能先稳定跑好新一代模型+新硬件组合"**，同时 KV cache 优化、投机解码、确定性推理、低成本部署仍是横跨所有项目的共同攻坚方向。MTP/投机解码、前缀缓存、disaggregated serving 等先进特性正在从"加分项"演变为"标配项"，但因复杂度带来的稳定性风险也正在成为生产环境的主要负担。

## 2. 各项目活跃度对比

| 项目 | 定位 | Issues 更新数 | PR 更新数 | 新 Release | 核心活跃度信号 |
|---|---|---|---|---|---|
| **vLLM** | 生产级推理引擎/Serving 框架 | ~45+（含跟踪 issue） | ~15+ | 无 | 大量稳定性排查 + 新硬件支持（CUDA 13.4/sm_107、ROCm）+ MRV2 内存优化推进 |
| **SGLang** | 高性能推理引擎 | ~25+ | ~15+ | 无 | 配置系统大规模重构 + DeepSeek-V4 问题集中爆发 + 性能优化活跃 |
| **llama.cpp** | 本地/边缘推理运行时 | ~20+ | ~10+ | **4 个**（b10472/b10470/b10456/b10455） | 高频迭代，AMD APU 修复 + SYCL 性能优化是今日主线 |
| **Ollama** | 本地模型运行时/桌面应用 | 20 | 22 | 无 | 高活跃但集中在 qwen3.8 系列问题与 v0.32.14 回归排查 |
| **LiteLLM** | LLM 网关/代理层 | ~15+（含跨版本 issue） | ~15+ | 无 | 计费/配额、安全加固、新 Provider 接入（FLUX 3、Comprehend Medical） |
| **Unsloth** | 微调框架/Studio | ~12+ | ~15+ | 无 | Studio 稳定性修复 + 非 NVIDIA 硬件支持补齐 |

**数据说明**：vLLM/SGLang 数据含 main 分支追踪 issue 与 PR；Ollama 为 24h 仓库活动计数；llama.cpp 发布版本数为实际 tag。

---

## 3. 模型支持竞速

| 项目 | 今日新增模型/架构 | 亮点与卡点 |
|---|---|---|
| **vLLM** | Kimi-K3 ROCm 路线图（#50682）；CUDA 13.4/sm_107（Rubin）预发布镜像；XPU INC int4 w4a8 后端；DeepSeek V4 ROCm fused AR draft 路径；Gemma4 官方镜像启动失败（兼容性回归） | 平台覆盖最广（CUDA/ROCm/XPU），但对新模型的稳定性风险最突出：Gemma4 镜像不可用、Kimi-K2.7 在 gfx942 失败 |
| **SGLang** | Kimi-K3 gfx950 12-head MLA FP8 decode；Intel XPU 三类 embedding 模型；GLM-5.2 兼容性问题（#29562）；DeepSeek-V4 系列问题集中爆发 | 在 AMD 新硬件（gfx950）上领先，但 DSV4/DSPARK 组合风险最高 |
| **llama.cpp** | GraniteSWA/GraniteMoeSWA 转换支持；dots3-note 模型；SYCL AdamW/SGD 微调算子；Z-Image Adreno xmem SDPA | 小模型/架构覆盖节奏稳，面向本地场景，与云端引擎差异化明显 |
| **Ollama** | MLX 新增 Ling-3.0-tiny/flash 架构（PR #17643）；qwen3.8 系列问题集中（工具调用、视觉、下载失败）；deepseek-v4-flash:cloud 的 agent 死循环 | MLX 上跟进新模型较快，但新模型在桌面端的可靠性问题突出 |
| **LiteLLM** | FLUX 3 视频生成；Amazon Comprehend Medical；Azure OCR 原生格式；Anthropic Guardrail 回归 | 作为网关以 Provider 支持为主，垂直场景接入快 |
| **Unsloth** | 无新模型架构；Intel Arc B580 导入失败确认（#3533）；Safetensors 图像/视频模型 Hub 运行支持 | 微调侧没有跟随新模型，更关注硬件适配与 Studio 体验 |

**结论**：vLLM 与 SGLang 处于 **"模型支持数量第一梯队、稳定性风险也第一梯队"**；llama.cpp 走"小而快"的本地路线；Ollama 被动跟随（问题集中在模型适配质量）；LiteLLM 以 Provider 接入广度取胜；Unsloth 专注微调场景未直接卷推理模型数量。

---

## 4. 性能优化前沿

| 优化方向 | vLLM | SGLang | llama.cpp | Ollama | LiteLLM | Unsloth |
|---|---|---|---|---|---|---|
| **KV Cache 管理** | ✓ 可扩展（growable）KV cache 草稿、MRV2 batch-sharded sampling、混合 KV cache+prefix caching 正确性修复 | ✓ Spec 缓冲区尺寸动态化、长上下文 DSA 问题暴露 | ✓ SYCL 量化 KV decode TILE kernel（+42%~169%）；4-bit KV cache 回归 | ✗ 无 | ✗ 无 | ✓ mmproj 显存占位修复 |
| **批处理与调度** | ✓ batch queue 采样修复、Batch Invariant 确定性推理收尾 | ✓ 配置系统重构（config bags）、CUDA graph 对称内存竞态修复 | ✗ 无 | ✗ 无 | ✓ 批处理定价修复 | ✓ 滚动上下文保留 evicted turns |
| **投机解码/采样** | ✓ MTP 稳定性排查、全异步投机解码（消除 Host↔GPU sync）、MRV2 Gumbel 采样修复 | ✓ EAGLE 工程清理、DSPARK 数据损坏问题（糟糕 👎） | ✓ 自适应 MTP draft 深度、滚动窗口启发式；MTP 性能回退 | ✗ 无 | ✗ 无 | ✗ 无 |
| **分布式推理** | ✓ 多节点 TP 稳定性修复、DeepEP-V2 图编译兼容、ROCm CPU offload | ✓ NIXL/UCX 段错误复现、PD 分离 + DCP 崩溃 | ✓ RPC 多节点崩溃 | ✗ 无 | ✗ 无 | ✗ 无 |
| **算子级优化** | ✓ PTX 9.4 `ldmatrix.s8.s4`（INT4→INT8 硬件扩展）、XPU INC 后端 | ✓ DSv4 MLA prefill 去 head padding、aiter FP8 Gluon | ✓ SYCL 量化 cpy kernel、SYCL TILE kernel、OpenCL Adreno xmem | ✗ 无 | ✗ 无 | ✗ 无 |
| **显存/内存效率** | ✓ growable KV cache、ModelOpt checkpoint 加载性能 | ✗ 无 | ✓ AMD APU 显存检测修复 | ✗ MLX 前缀缓存缺失（反向 🧊） | ✗ 长驻内存 OOM | ✗ GGUF 系统内存不释放 |

**火力分布结论**：今日的优化竞争集中在 **KV cache 效率、投机解码鲁棒性、分布式确定性** 三个战场。vLLM 在"系统级确定性"与"显存效率"上布局最深（Batch Invariant、MRV2、growable KV cache）；SGLang 在"硬件适配性能"（AMD/XPU）与"配置灵活性"上激进，但 DSPARK 数据损坏问题压制其技术信用；llama.cpp 以 SYCL/量化 kernel 为突破口，在本地硬件上实现显著的速度跃升。

---

## 5. 分层定位差异

| 项目 | 分层定位 | 核心用户 | 今日动态反映的定位强化 |
|---|---|---|---|
| **vLLM** | **生产级推理引擎 + Serving 框架**（云端/数据中心） | 企业 AI 平台、模型服务团队 | 强化多节点、多 GPU、确定性推理、硬件平台矩阵；问题是复杂度攀升后稳定性管理挑战增大 |
| **SGLang** | **高性能推理引擎**（极致性能导向） | 追求吞吐/延迟极限的研究与工程团队 | 激进适配新模型（DSV4/DSPARK）与新硬件（gfx950），但稳定性风险同步上升 |
| **llama.cpp** | **本地/边缘推理运行时**（ggml 生态） | 个人开发者、桌面应用、端侧设备 | 高频版本迭代（4 releases/日）、SYCL/OpenCL/ROCm 多后端覆盖，做"最后一公里"硬件适配 |
| **Ollama** | **本地模型运行时 + 用户友好封装** | 非深度技术用户、Agent 原型开发者 | 持续在"易用性"与"模型生态跟进"之间游走；MLX 回归与工具调用问题暴露其工程纵深不足 |
| **LiteLLM** | **LLM 网关/代理/统一接口层** | 平台工程团队、企业 IT | 强化 Provider 生态广度、计费准确性、安全加固；不碰推理性能，专注"接入与管理" |
| **Unsloth** | **微调框架 + Studio 桌面端**（训练/微调领域） | 模型微调开发者、研究团队 | 专注训练侧稳定性、硬件兼容（AMD/CPU）、Studio 体验；不参与推理引擎竞争 |

**关键判断**：vLLM 与 SGLang 在推理引擎层直接竞争，但 vLLM 更偏"企业级稳定性"，SGLang 更偏"激进性能"；llama.cpp 与 Ollama 同属本地层但前者面向开发者、后者面向终端用户；LiteLLM 站在所有引擎之上做聚合，与推理引擎是互补而非竞争关系；Unsloth 与推理引擎在生态上互补（微调产物 → 部署），但其 Studio 产品正在向"本地推理+微调一体化"方向延伸，与 Ollama 形成潜在重叠。

---

## 6. 值得关注的趋势信号

### 6.1 行业趋势

1. **新一代模型（DSV4/Kimi-K3/Qwen3.8）是本轮基础设施压力的核心来源**——跨所有项目的 issue 中，与这些模型相关的崩溃/挂起/错误占绝对主导（SGLang 尤其明显）。基础设施团队正面临"模型先发 vs 稳定后置"的 2-4 周追赶窗口，**建议技术决策者将上游模型大版本升级视为基础设施事故的高危时段**，建立模型级回归测试集。

2. **投机解码/ MTP 成为最大的稳定性黑洞**——vLLM（MTP 非法内存访问、TP>1 崩溃）、SGLang（DSPARK 静默数据损坏）、llama.cpp（MTP 性能回退、KV cache 饱和时崩溃）、Ollama（MLX 前缀缓存缺失）四大项目同时被投机解码相关问题困扰。性能增益虽诱人，但**当前投机解码在多节点/多卡/长上下文场景的工程成熟度尚未达标**，生产环境应视为"高阶实验特性"。

3. **硬件平台竞争进入多线作战时代**——AMD ROCm/gfx950、Intel XPU/Arc、Apple MLX、Blackwell SM120/SM121 同时成为各项目的适配目标。CUDA 一家独大格局已松动，但 **ROCm 与 XPU 的稳定性坑（gfx942 缺失算子、Arc 导入失败、XPU warmup 挂起）依然密集**。多云/混合硬件部署策略应预留两种以上硬件路径。

4. **确定性推理（Batch Invariant）与可扩展 KV cache 成为企业级需求**——vLLM 的 #27433 项目接近完成，反映大客户对可复现推理与显存效率的真实诉求。**任何考虑上生产的多租户平台，都应关注这一方向**。

5. **网关层的成本控制与安全可见性在强化**——LiteLLM 密集修复计费/预算绕过、敏感信息明文泄露、流式 usage 统计，说明在企业规模化落地阶段，**"推理性能"不再是唯一指标，"可审计、可控成本、安全合规"正在成为新的采购门槛**。

6. **本地/边缘推理与云端引擎的差距扩大**——llama.cpp（4 个 release/日）与 Ollama（高频迭代）持续高频迭代，但两者的优化聚焦在硬件适配与易用性，与 vLLM/SGLang 的系统级能力（KV cache、投机解码、分布式）差距日益明显。对于需要高性能的本地推理场景，建议仍优先考虑面向服务器级硬件优化的引擎（如 vLLM 的 LLM 类 API 或 SGLang 的离线模式）。

### 6.2 Agent / 应用开发者应当关注

- **DeepSeek-V4 生态风险最高、但也最需跟进**：多个项目对其支持尚不成熟（SGLang 的 decode 挂起与 token 损坏、Ollama 的 193 次工具调用死循环/3100 万 token 消耗）。在生产环境使用 DSV4-Flash 前必须做长上下文 soak test，并在 agent 层设置**硬性防护措施**（最大连续工具调用次数、重复调用检测、`<think>` 泄漏清洗、重试熔断）。
- **工具调用可靠性是 Agent 产品级交付的拦路虎**：Ollama 的 qwen3.8 工具解析失败后重试永久挂起、Unsloth 的工具调用误触发、LiteLLM 的 Anthropic 兼容层 400 错误——**工具调用栈仍是全生态最薄弱的环节**，建议 Agent 侧增加超时、重试上限与 fallback 策略。
- **结构化输出 / 推理内容解析的正确性需要额外验证**：vLLM 的 tokenizer shim 回归、SGLang 的 reasoning 内容解析错误、Ollama 的 `think:false` 空内容、llama.cpp 的 logprobs 缺失——**依赖输出格式稳定性的应用（评估、结构化数据提取）需在升级前后做回归验证**。
- **基础设置检查清单**：锁定并使用经过验证的版本组合（而非追随 latest 镜像）；对 `/health` 响应做脱敏扫描；为 Embedding 类请求自行分块而非依赖服务端截断；涉及多节点的部署务必增加应用层健康检查与自动重启机制；Windows/Linux 桌面端场景选用 ROCm/Metal 等平台时，先将测试周期与上游稳定版本对齐后再上生产。
- **推理基础设施的竞争焦点正向"可运维、可观测、可解释"转移**——vLLM 的 Batch Invariant、LiteLLM 的安全加固、SGLang 的配置系统重构、llama.cpp 的 OTLP/tracing 提交，信号一致：**性能之外，"确定性 + 可审计性 + 运营可靠性"正成为选型的新分水岭**。Agent/应用开发者在做技术选型时，建议把多轮长会话的 KV/前缀缓存命中率、失败后的重试语义、以及推理服务在工具调用失败场景下的行为纳入评估指标。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 — 2026-08-18

## 今日速览

过去 24 小时无新 Release，社区焦点集中在 v0.27.0 暴露的稳定性问题上：MTP 投机解码在长序列下非法内存访问（#40756）、4 节点 TP=4 引擎空闲后永久 stall（#51921）、TP>1 下 draft model 初始化崩溃（#52023）均已进入密集排查阶段，其中 #52023 已有 WIP 修复 PR。硬件侧，ROCm 的 Kimi-K3 支持路线图与 CUDA 13.4/sm_107 预发布镜像管道成为平台扩展亮点，同时多项 MRV2 与 KV cache 内存优化仍在推进。

## 版本发布与破坏性变更

**无新版本发布。** 但以下兼容性/行为变更值得注意：

- **[兼容性回归] 官方镜像无法启动 Gemma4**：`vllm/vllm-openai:latest`（vLLM 0.27.0）内置 Transformers 5.15.0，与 Gemma4 模型不兼容，启动即失败。建议固定 Transformers 版本或等待上游修复。[#51744](https://github.com/vllm-project/vllm/issues/51744)
- **[修复] 恢复 `vllm.transformers_utils.tokenizer` shim**：PR #35024 移除了该模块，导致被 pinned 的 `lm-format-enforcer==0.11.3` 导入失败。PR #52661 正在恢复该 shim，影响所有结构化输出场景。[PR #52661](https://github.com/vllm-project/vllm/pull/52661)
- **[行为变更] standalone v1 修复 batch queue 采样**：PR #52660 移除了 `is_ec_consumer` 检查，修复 standalone v1 下 batch queue 静默跳过 token sampling 的问题。[PR #52660](https://github.com/vllm-project/vllm/pull/52660)
- **[破坏性修复] DeepEP-V2 图模式要求 `expert_tokens_meta=None`**：`--all2all-backend deepep_v2` 在 decode/cudagraph 路径下，`expert_tokens_meta` 必须显式置为 None，否则启动崩溃。[PR #52632](https://github.com/vllm-project/vllm/pull/52632)

## 新模型与硬件支持

- **Kimi-K3 ROCm 支持路线图**：官方跟踪 issue #50682 已建立，覆盖 Day 0 特性基线（AITER fused-moe a16w4/a8w4）、性能优化与后续 enablement 工作。[#50682](https://github.com/vllm-project/vllm/issues/50682)
- **Kimi-K2.7-Coder 在 AMD MI308X（gfx942）启动失败**：`mla_gluon` 算子要求 gfx950（CDNA4），在 gfx942 上直接 assertion error。ROCm 用户需等待算子下沉或使用 CDNA4 设备。[#51964](https://github.com/vllm-project/vllm/issues/51964)
- **CUDA 13.4 / sm_107（Rubin）预发布镜像管道**：新增 image-only CUDA 13.4rc1 构建路径，pin 了兼容的 PyTorch nightlies，为 Rubin GPU 做准备。[PR #52379](https://github.com/vllm-project/vllm/pull/52379)
- **XPU 新增 INC int4 w4a8 后端**：`INCXPUW4A8LinearMethod` 支持 int4 权重 + 动态 per-token int8 激活，复用 w4a16 权重布局，ARK 不可用时回退到 oneDNN `int4_gemm_w4a8`。[PR #50501](https://github.com/vllm-project/vllm/pull/50501)
- **ROCm 启用 DeepSeek V4 fused AR draft metadata 更新**：基于 #46849 恢复 fused multi-step draft decode 图，补齐了 DeepSeek V4 稀疏 SWA 的 ROCm 路径。[PR #52628](https://github.com/vllm-project/vllm/pull/52628)

## 性能与优化

- **Batch Invariant 功能收尾中**：#27433 跟踪页显示基础支持已完成（基于 Thinking Machines 的确定性方案），仍有部分收尾工作，目标是实现批间完全无干扰的确定性推理。[#27433](https://github.com/vllm-project/vllm/issues/27433)
- **W4A8-INT8 路径引入 PTX 9.4 `ldmatrix.s8.s4`**：利用硬件 INT4→INT8 在共享内存加载过程中完成符号扩展，消除显式转换开销（CUDA 13.4.0 Developer Preview）。[#49529](https://github.com/vllm-project/vllm/issues/49529)
- **MRV2 batch-sharded sampling**：将采样前完整 logits 物化改为按 TP 分片，单步采样内存分配降低至原来的 1/P。[PR #50465](https://github.com/vllm-project/vllm/pull/50465)
- **可扩展（growable）KV cache（草稿）**：按需增长而非满额预分配，降低显存浪费；当前为 draft，依赖 #51718 合入。[PR #50779](https://github.com/vllm-project/vllm/pull/50779)
- **全异步投机解码推进**：#29134 提议将 `seq_lens_cpu` 变为可选，以解除 input-prep 与 model forward 之间的 Host↔GPU 同步点，目前仍 open。[#29134](https://github.com/vllm-project/vllm/issues/29134)
- **ModelOpt Llama-4 checkpoint 加载性能差**：即使从 CPU page cache 加载也需 5+ 分钟，根因在 MoE state dict 的 hack 逻辑，已有重构方向。[#31624](https://github.com/vllm-project/vllm/issues/31624)
- **ROCm CPU offload 参数对齐**：修复 `hipMemcpyBatchAsync` 参数匹配问题，预期在 ROCm 7.14x 上获得正确性能表现。[PR #43018](https://github.com/vllm-project/vllm/pull/43018)

## 稳定性与回归

**崩溃/挂起（按严重程度排序）**

- **MTP 投机解码长序列非法内存访问**：Qwen3.6-27B-FP8 + v0.19.1，`num_spec_tokens=5` 时崩溃，38 条评论，目前无 fix PR。[#40756](https://github.com/vllm-project/vllm/issues/40756)
- **Qwen3.5 GDN Kernel 非法内存访问**：vLLM nightly 在 H200 上稳定复现。[#34948](https://github.com/vllm-project/vllm/issues/34948)
- **v0.27.0 4 节点 TP=4 引擎永久 stall**：GB10/aarch64，空闲约 1 分钟后 `shm_broadcast` writer 饿死，请求不再进入 scheduler，API 仍响应但无法恢复。[#51921](https://github.com/vllm-project/vllm/issues/51921)
- **TP>1 投机解码初始化崩溃（有 WIP 修复）**：draft model 的 `hidden_size` 大于 target 时，`fuse_allreduce_rms` workspace 尺寸计算错误导致 init 崩溃。[#52023](https://github.com/vllm-project/vllm/issues/52023) → [PR #52193](https://github.com/vllm-project/vllm/pull/52193)（诊断补丁，已验证）
- **Mamba-2 Triton Kernel 在 SM121 异步模式非法指令**：DGX Spark（GB10）上需设置 `CUDA_LAUNCH_BLOCKING=1` 才能绕过。[#37431](https://github.com/vllm-project/vllm/issues/37431)

**正确性 Bug**

- **NIXL disagg 在 prefill TP4/decode DP8 物理块大小不同时失败**：Qwen3.5 hybrid 模型，跨阶段 KV 传输存在问题。[#42895](https://github.com/vllm-project/vllm/issues/42895)
- **Hybrid multi-group KV connector 崩溃**：connector 上报 load-error 块时 `_update_requests_with_invalid_blocks` 抛 `ValueError: too many values to unpack`。[#50687](https://github.com/vllm-project/vllm/issues/50687)
- **deepseek_v4 parser 内容路由错误**：回复缺少 `</think>` 时，整个答案被路由到 `reasoning_content`，且尾部 EOS 未被剥离。[#48645](https://github.com/vllm-project/vllm/issues/48645)
- **Mamba cache blocks 不足导致启动硬失败**：hybrid 模型 + LoRA 场景下，可用 Mamba cache blocks < `max_num_seqs` 直接 ValueError，建议自动 clamp 并告警。[#49064](https://github.com/vllm-project/vllm/issues/49064)
- **MRV2 Gumbel 采样对 `-inf` logits 处理错误**：已提修复 PR，clamp uniform draw 后若浮点舍入到达上界，Gumbel noise 变为 `inf`。[PR #43249](https://github.com/vllm-project/vllm/pull/43249)
- **混合 KV cache + prefix caching 正确性修复**：滑动窗口、Mamba align、full attention 异构共存时 prefix caching 在 4 处出错；修复后可支持全滑动窗口 DFlash drafter。[PR #50457](https://github.com/vllm-project/vllm/pull/50457)

**已关闭/回归修复**

- **ROCm 相关多个历史问题被 stale 关闭**：Qwen3.5-9B-AWQ JSON 输出卡死（#39348）、Flash MTP init error（#38498）、ROCm 后端选择性能回退（#39965）等均已 closed，如仍受影响建议重新打开或升级验证。

## 对应用开发者的意义

1. **官方镜像升级需谨慎**：`latest` 镜像的 Transformers 5.15.0 会直接导致 Gemma4 无法启动，生产环境请固定版本并关注上游修复。[#51744](https://github.com/vllm-project/vllm/issues/51744)
2. **结构化输出用户注意**：`lm-format-enforcer` 依赖的 tokenizer shim 曾被移除（应用会 import 失败），PR #52661 正在恢复，修复合入前不要升级到受影响版本。[PR #52661](https://github.com/vllm-project/vllm/pull/52661)
3. **TP>1 投机解码约束**：draft model 的 `hidden_size` 不能大于 target，否则初始化即崩溃；如遇此问题可跟踪 #52023 的修复进展。[#52023](https://github.com/vllm-project/vllm/issues/52023)
4. **多节点部署需自建健康检查**：v0.27.0 在 4 节点 TP=4 下存在空闲后永久 stall 的问题，`/v1/models` 仍响应但请求无法进入调度器，建议对这类环境增加应用层超时与自动重启机制。[#51921](https://github.com/vllm-project/vllm/issues/51921)
5. **DeepEP-V2 + 图编译组合需等待修复**：`--all2all-backend deepep_v2` 在 cudagraph 路径下启动崩溃，修复 PR 已提交，合入后再启用该组合。[PR #52632](https://github.com/vllm-project/vllm/pull/52632)
6. **RL 训练场景关注生命周期 RFC**：Sleep/Wake 正确性（#48310）与 LoRA Adapter 生命周期（#48297）两个 RFC 均在讨论中，涉及 `enable_lora=True` 时 Level-2 sleep 的已知崩溃，RL 训练平台需跟进。[#48310](https://github.com/vllm-project/vllm/issues/48310) [#48297](https://github.com/vllm-project/vllm/issues/48297)

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 — 2026-08-18

> 数据范围：GitHub 上 2026-08-17 全天更新的 Issues / PRs（截至 08-17 23:12 UTC）

## 1. 今日速览

- **无新 Release**，但配置系统迎来一轮大规模重构：ch-wan 提交的 #35022–#35029 系列将配置读取统一迁移到 “config bags”，这会改变 `/server_info` 等接口的内部行为。
- **DeepSeek-V4 是当前最集中的问题源**：#33549 decode 挂起、#34959 DSPARK 静默数据损坏、#34941 超长 prefill 静默不计算 attention，多个崩溃与正确性问题同时悬而未决。
- DSpark / HiCache / 长上下文场景下的稳定性问题持续发酵，同时 Intel XPU 和 AMD gfx950 各有新硬件支持或修复在推进。

---

## 2. 版本发布与破坏性变更

无新版本发布。以下为 `main` 分支上的行为变更（如果合并，将影响升级用户）：

- **配置系统重构（ch-wan 系列）**：#35022 移除了多引擎兼容层（进程只持有一份 live config），#35023 改为在进程读取配置前先发布配置，#35024 让 speculative buffer 尺寸从 config bags 读取而非启动记录，避免 adaptive speculative decoding 配置漂移导致分配不当；#35029 是该系列的 CI 载体。这些 PR 尚未合并，但会显著影响配置热更新和 `/server_info` 返回语义。
  - sgl-project/sglang PR #35022 (https://github.com/sgl-project/sglang/pull/35022)
  - sgl-project/sglang PR #35023 (https://github.com/sgl-project/sglang/pull/35023)
  - sgl-project/sglang PR #35024 (https://github.com/sgl-project/sglang/pull/35024)
  - sgl-project/sglang PR #35029 (https://github.com/sgl-project/sglang/pull/35029)
- **清理废弃配置**：#34926 清理了废弃的 DeepSeek-V4 环境变量；#34983 弃用 Prefill CP V1，移除 `SGLANG_ENABLE_CP_V2` 开关和相应 legacy 路径。
  - sgl-project/sglang PR #34926 (https://github.com/sgl-project/sglang/pull/34926)
  - sgl-project/sglang PR #34983 (https://github.com/sgl-project/sglang/pull/34983)
- **修复 #34238 引入的回归**：#35144 报告了 EAGLE/NEXTN TP=2 在 Intel XPU warmup 挂起的问题，原因指向 #34238 将 verify-decision TP broadcast 移出 sampling 分支。
  - sgl-project/sglang Issue #35144 (https://github.com/sgl-project/sglang/issues/35144)

---

## 3. 新模型与硬件支持

- **Intel XPU 编码器嵌入模型**（#35213）：新增 `BAAI/bge-base-en-v1.5`、`nomic-ai/nomic-embed-text-v1.5`、`ibm-granite/granite-embedding-english-r2` 三类 encoder embedding 模型在 XPU 后端上的支持，修复了 bge 在 XPU 上返回错误 embedding 的问题。
  - sgl-project/sglang PR #35213 (https://github.com/sgl-project/sglang/pull/35213)
- **AMD gfx950 / Kimi-K3**（#34647）：为 12-head MLA 启用 aiter FP8 Gluon decode（batched `bh16bn128`），支持 Kimi-K3 TP8（12 个本地 head）在 gfx950 上的解码路径；依赖 [ROCm/aiter#4480](https://github.com/ROCm/aiter/pull/4480)，后者已合并。
  - sgl-project/sglang PR #34647 (https://github.com/sgl-project/sglang/pull/34647)
- **SM120 上 DSpark 不可用**（#33985）：decode-dsv4 缺少 topk=192 的 kernel 实例化，导致 DeepSeek-V4-Flash 在 SM120（RTX PRO 6000 Blackwell）上 speculative decoding 永远无法启动。目前为 open bug，无 fix PR。
  - sgl-project/sglang Issue #33985 (https://github.com/sgl-project/sglang/issues/33985)

---

## 4. 性能与优化

- **移除 DSv4 MLA prefill 的 head padding**（#35104）：`attn_tp_size=2` 时当前实现会构建 64-row query 但只用 32 行，造成一半计算浪费；该 PR 去掉 padding，直接减少 prefill 计算量。
  - sgl-project/sglang PR #35104 (https://github.com/sgl-project/sglang/pull/35104)
- **EAGLE draft-extend CUDA graph staging 拆分**（#35126）：将 draft-extend 的输入 staging 分为 verify 前和 verify 后两阶段，read-done publish 位置不变、无行为变化。为工程清理，便于后续优化。
  - sgl-project/sglang PR #35126 (https://github.com/sgl-project/sglang/pull/35126)
- **修复 CUDA graph capture 期间对称内存分配竞态**（#34230）：启用 symmetric memory + speculative decoding + TP>1 + CUDA graph 时，collective buffer 可能在不同 rank 上被捕获到不同的 NCCL 注册窗口，违反 `NCCL_WIN_COLL_SYMMETRIC` 契约，导致静默错误输出。属于正确性修复，也避免了潜在性能回退的坑。
  - sgl-project/sglang PR #34230 (https://github.com/sgl-project/sglang/pull/34230)
- **Spec 缓冲区尺寸改为动态读取**（#35024）：adaptive spec decode 在运行时通过 `get_context().override` 切换 step config，但 6 条尺寸计算路径仍读 ServerArgs 的启动值，可能分配不足或过度；改为从 config bags 读取后，缓冲区分配与当前 step config 一致。
  - sgl-project/sglang PR #35024 (https://github.com/sgl-project/sglang/pull/35024)

---

## 5. 稳定性与回归

按严重程度排列（⚡=严重，🔧=已有 fix PR 或明确修复方向）：

- **⚡ NIXL/UCX prefill 段错误仍可复现**（#35189）：v0.5.17 / CUDA 13.0 / B200 上 `nixlUcxSharedThread -> cuEventQuery` 崩溃；#23489 和 #23499 均被关闭但没有根因结论，社区已有人二次踩坑。**无 fix PR**。
  - sgl-project/sglang Issue #35189 (https://github.com/sgl-project/sglang/issues/35189)
- **⚡ DeepSeek-V4 (dsv4+DSPARK) TP=8 decode 挂起**（#33549）：8×H20 上约 245K context 时所有 GPU 100% 利用率但低功耗，watchdog 杀死服务器。**无 fix PR**。
  - sgl-project/sglang Issue #33549 (https://github.com/sgl-project/sglang/issues/33549)
- **⚡ DSPARK 静默破坏 token 标识符**（#34959）：DeepSeek-V4-Flash 上 speculative decoding 会静默产生错误 token，属数据损坏级问题，**无 fix PR**。
  - sgl-project/sglang Issue #34959 (https://github.com/sgl-project/sglang/issues/34959)
- **⚡ Kimi K3 decode 确定性崩溃**（#34920）：PD 分离 + DCP（`--dcp-size 8`）+ DSPARK 组合下，首个 target-verify batch 在 `dcp/planner.py` 中死于 `cumsum(extend_prefix_lens=None)`。**无 fix PR**。
  - sgl-project/sglang Issue #34920 (https://github.com/sgl-project/sglang/issues/34920)
- **⚡ DSA sparse-MLA prefill 对 >65535 token 的单个 extend 静默无输出**（#34941）：`gridDim.z` 溢出在非 DP 路径未防护，attention kernel 完全不调度，输出错误但不报错。**无 fix PR**。
  - sgl-project/sglang Issue #34941 (https://github.com/sgl-project/sglang/issues/34941)
- **⚡ EAGLE/NEXTN TP=2 在 Intel XPU warmup 挂起**（#35144）：由 #34238 引入，verify-decision TP broadcast 被移出 sampling 分支导致。**#34238 为可追溯的回归点，亟待 revert/fix**。
  - sgl-project/sglang Issue #35144 (https://github.com/sgl-project/sglang/issues/35144)
- **🔧 Helion KDA 短 prefill 修复**（#35197）：修复 short prefill 时只含 active tokens 的 shape 处理，并明确拒绝非 2 的幂 head dim；有小规模 fix PR。
  - sgl-project/sglang PR #35197 (https://github.com/sgl-project/sglang/pull/35197)
- **🔧 GLM-5.2-NVFP4 在 pro6000 上报错**（#29562）：15 条评论高热度，尚未标记 closed，**无明确 fix PR**。
  - sgl-project/sglang Issue #29562 (https://github.com/sgl-project/sglang/issues/29562)
- **Qwen3.8-27B-FP8 reasoning 内容解析错误**（#35148）：rust sgl-model-gateway 无法正确解析 reasoning content。**无 fix PR**。
  - sgl-project/sglang Issue #35148 (https://github.com/sgl-project/sglang/issues/35148)
- **HiCache 长 agentic 会话缓存命中率归零**（#35129）：DeepSeek-V4-Flash-0731 + dsv4 + DSPARK + HiCache 在长会话下每轮 `#cached-token: 0`，而短请求 ~98% 命中。**无 fix PR**。
  - sgl-project/sglang Issue #35129 (https://github.com/sgl-project/sglang/issues/35129)
- **🔧 预填充 FLOPs 估算失真**（#34298）：`prefix_lens` 被忽略，chunked prefill 下 TFLOPS 指标退化为 1/latency。PR #34313 已修复（closed）。
  - sgl-project/sglang Issue #34298 (https://github.com/sgl-project/sglang/issues/34298)

---

## 6. 对应用开发者的意义

- **DeepSeek-V4 / DSpark 组合目前风险较高**：#33549（decode 挂起）、#34959（token 损坏）、#34941（长 prefill 静默错误）都未经修复。生产环境若使用 DSV4-Flash + speculative decoding + 长上下文，建议：
  - 升级前先跑长上下文 soak test；
  - 对 >64K 的 prefill 请求检查输出是否与关闭稀疏注意力时一致；
  - 密切关注 #35189（NIXL/UCX 段错误），B200 用户尤其需要规避。
- **配置系统行为将变**：ch-wan 系列 PR 合并后，`/server_info` 的返回内容会更清晰地分离“启动时配置”和“运行中变更”，进程内 config 的发布/读取顺序也会调整。依赖该接口做自动扩缩容或配置校验的开发者需在升级后重新验证输出格式，尤其确认 tokenizer 变更、LoRA adapter 切换、spec decode 参数热更新等场景下读到的配置语义。
- **EAGLE/NEXTN 在 XPU 上因 #34238 挂起**：使用 Intel XPU + EAGLE 回退到 TP=1，或等待 #34238 的修复。
- **对长 Agent 会话的影响**：#35129 表明 HiCache 在 8×H20 + DSV4 场景下对长上下文缓存命中率可能降到 0%，直接影响多轮 agent 的 TTFT。若生产环境依赖 HiCache 的 prefix 命中率来保障 SLA，需要先将该 issue 视为高危，评估是否回退到普通 RadixCache 或拉长 `--chunked-prefill` 的 chunk size 来规避。
- **好消息**：#35213 让 bge / nomic / granite 编码器模型在 Intel XPU 上可用；#34647 让 Kimi-K3 在 AMD gfx950 上获得完整的 12-head MLA decode，支持更广的硬件部署选择。

---

*日报生成时间：2026-08-18。所有链接基于 GitHub 数据中的 issue/PR 编号构建，可通过 `https://github.com/sgl-project/sglang/issues/{id}` 或 `/pull/{id}` 访问。*

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报（2026-08-18）

## 今日速览
- 发布 4 个新版本：重点修复 AMD APU 显存检测错误（b10472），并优化 SYCL 量化复制 kernel（b10456）、新增 SYCL 上的 AdamW/SGD 支持（b10455）。
- 社区热度集中在 server 的 `disaggregated prefill/decode` 路线图（#21266）与 MTP 性能回退（#25489）。
- 大量 CUDA/Vulkan/RPC 崩溃上报，另有 `/v1/completions` logprobs 缺陷影响评估工具链。

## 版本发布与破坏性变更
- **b10472**：CUDA 后端跳过 UMA override for HIP builds（[#27083](https://github.com/ggml-org/llama.cpp/pull/27083)），修复 AMD APU 在大型 TTM 分配下可用显存被低估的问题（[#18159](https://github.com/ggml-org/llama.cpp/issues/18159)）。注意：该变更会改变 HIP 构建下的 UMA 内存检测逻辑，AMD APU 用户需验证显存占用与分配行为。
- **b10470**：CI 在 release 任务中显式创建并推送 git tag（[#27261](https://github.com/ggml-org/llama.cpp/pull/27261)），无运行时影响。
- **b10456**：SYCL 量化 cpy kernel 线程/块数按量化大小调整（[#27160](https://github.com/ggml-org/llama.cpp/pull/27160)），显著改善 q4_0→f32 路径吞吐（Arc 70 上原为 20.21 GB/s）。
- **b10455**：SYCL 后端支持 `OPT_STEP_ADAMW` 与 `OPT_STEP_SGD`（[#25268](https://github.com/ggml-org/llama.cpp/pull/25268)），可用于微调/训练场景。

## 新模型与硬件支持
- **PR #25505**：新增 `GraniteSWAForCausalLM` / `GraniteMoeSWAForCausalLM` 转换支持（滑动窗口注意力 + Attention Sinks），权重尚未发布。
- **PR #27060**：新增 `dots3-note` 模型支持，需扩展 `llama-kv-cache-dsa` 以处理 DSA + SWA。
- **PR #27145**：Docker 构建升级至 ROCm 7.14.0（Ubuntu 26.04），并修复了“no usable GPU found”的 workaround。
- **PR #26331**：OpenCL 后端为 Adreno GPU 新增 xmem SDPA 路径（针对 Z-Image 非因果注意力）。
- **b10455**：SYCL 新增 AdamW/SGD 优化器算子，扩展了训练/微调能力。

## 性能与优化
- **b10456**：SYCL 量化 cpy kernel 的 thread/block 计数改为与量化大小成正比，q4_0→f32 路径在 Arc 70 上吞吐量从 20.21 GB/s 显著提升。
- **PR #26689**：SYCL TILE kernel 用于量化 KV decode（q4_0/q8_0），在 Battlemage GPU 上 Qwen3.6-35B、Gemma 4 26B/12B 在 32K/118K 上下文下测试提速 +42% ~ +169%，零回退。
- **PR #27210**：新增自适应 MTP draft 深度（`--spec-type draft-mtp-adaptive`），建议配合 `--spec-draft-n-max 12` 使用，通过计数状态机动态调整 draft 层数。
- **PR #25726**：另一个自适应 draft 长度启发式（滚动窗口），同样针对 MTP-only，改善生成吞吐。

## 稳定性与回归
> 按严重程度排序；若无特殊说明，均为未修复状态。

| 严重度 | 问题 | 描述 | 状态 |
|---|---|---|---|
| 崩溃 | [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) | CUDA kernel stall 被 watchdog 杀死（RTX Pro 6000 Blackwell + Qwen3.8-27B） | 无 fix，需 help |
| 崩溃 | [#27046](https://github.com/ggml-org/llama.cpp/issues/27046) | `resolve_fused_ops` 误报导致 null-ptr SIGSEGV（Intel Lunar Lake iGPU，多架构复现） | 无 fix |
| 崩溃 | [#26583](https://github.com/ggml-org/llama.cpp/issues/26583) | RPC 多节点 GLM-5.2 崩溃（invalid data ptr / graph_compute failed） | 无 fix |
| 崩溃 | [#26558](https://github.com/ggml-org/llama.cpp/issues/26558) | MTP + KV cache 饱和时 `cublasSgemm INVALID_VALUE` 硬崩溃 | 无 fix |
| 崩溃 | [#26746](https://github.com/ggml-org/llama.cpp/issues/26746) | ROCm gfx1151 RPC worker 在 DeepSeek V4 prefill 超过 4096 tokens 时 `GGML_OP_TOP_K` 崩溃 | 无 fix |
| 崩溃 | [#27116](https://github.com/ggml-org/llama.cpp/issues/27116) | `--split-mode tensor` + `iq4_nl` KV cache 触发 `GGML_ASSERT(ret.axis != ...)` | 与 [#26902](https://github.com/ggml-org/llama.cpp/issues/26902) 类似 |
| 回归 | [#27109](https://github.com/ggml-org/llama.cpp/issues/27109) | CUDA 4-bit KV cache（q4_1/q4_0）使 qwen35 hybrid prefill 降到 ~34 t/s | 无 fix |
| 回归 | [#25489](https://github.com/ggml-org/llama.cpp/issues/25489) | MTP 性能自 b9935 起明显下降（Windows） | 无 fix |
| API 缺陷 | [#27174](https://github.com/ggml-org/llama.cpp/issues/27174) | `/v1/completions` 的 logprobs 仅返回生成 token，prompt/echo logprobs 缺失，破坏 lm-eval 等评估 | 无 fix |
| 功能性 | [#26996](https://github.com/ggml-org/llama.cpp/issues/26996) | Windows ROCm 7.14 release 缺失 `hipblas.dll`，GPU 无法被检测 | 无 fix |
| 功能性 | [#26987](https://github.com/ggml-org/llama.cpp/issues/26987) | Qwen3-Coder 解析器 lazy tool-call 触发永不激活 | 无 fix |
| 警告 | [#24712](https://github.com/ggml-org/llama.cpp/issues/24712) | fused Gated Delta Net tensor 被分配到 CPU，而 layer 0 被分配到 CUDA0 | 无 fix |

**已有修复 PR：**
- [#18159](https://github.com/ggml-org/llama.cpp/issues/18159) → b10472 已修复。
- [PR #27286](https://github.com/ggml-org/llama.cpp/pull/27286)：`mul_mat_id` 中 expert id 越界写修复（release 构建下 assert 被编译掉，可能造成堆越界）。
- [PR #27285](https://github.com/ggml-org/llama.cpp/pull/27285)：修复 vision 模型图中可选 tensor 未判空导致的 NULL 页 SIGSEGV。
- [PR #27284](https://github.com/ggml-org/llama.cpp/pull/27284)：修复 `im2col` 反传中 int32 偏移截断（CWE-680/787）。
- [PR #25592](https://github.com/ggml-org/llama.cpp/pull/25592)：修复 hybrid/recurrent 模型的 checkpoint 处理（#24055）。

## 对应用开发者的意义
- **OpenAI 兼容 API 用户**：注意 #27174 的 logprobs 缺陷，lm-eval 等依赖 prompt logprobs 的工具暂不可用；评估时需降级或等待修复。
- **MTP 用户**：若遇到性能回退（#25489），可尝试新 PR 中的自适应 draft 算法（#27210 / #25726），但需自行验证效果。
- **多 GPU / 张量并行用户**：`--split-mode tensor` 与量化 KV cache 组合可能触发 assert（#27116、#26902），建议暂避或等待修复。
- **可观测性**：PR #27280 为 server 添加 OTLP/HTTP tracing，适合需要链路追踪的部署环境。
- **桌面端**：官方桌面应用（PR #27287）正在开发中，未来非技术用户部署门槛降低。
- **AMD APU 用户**：b10472 修复了显存上限误判，升级后可充分利用系统内存。

数据来源：[github.com/ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 — 2026-08-18

## 今日速览

过去 24 小时内 Ollama 未发布新版本，但仓库活跃度极高：共 20 个 Issue 和 22 个 PR 被更新，其中相当比例集中在 **qwen3.8** 系列（工具调用挂起、模型下载失败、视觉输入处理异常）以及 **0.32.14 版本的 MLX 引擎回归**（CPU 高占用、`think:false` 返回空内容）。此外，一个关于 `deepseek-v4-flash:cloud` 模型导致 agent 陷入 193 次工具调用死循环的高成本问题（约 3100 万 token 消耗）值得所有 agent 开发者警惕。

---

## 版本发布与破坏性变更

过去 24 小时无新 Release。但社区报告显示 **v0.32.14 存在两项疑似回归**：

- **CPU 占用飙升**：模型完全载入 VRAM 的情况下，v0.32.14 将 CPU 占用推高至 50-80%，回退到 v0.32.13 后恢复正常（Issue [#17833](https://github.com/ollama/ollama/issues/17833)）
- **MLX 引擎行为回归**：`"think": false` 在 Gemma 4 MLX 模型上返回空 assistant 消息，而 v0.32.5 行为正常（Issue [#17823](https://github.com/ollama/ollama/issues/17823)）

升级前建议评估这两项影响，尤其在生产环境使用 MLX 后端的 macOS 用户。

---

## 新模型与硬件支持

**进行中 / 未合并：**

- **MLX 新增 Ling-3.0 模型架构支持**（[PR #17643](https://github.com/ollama/ollama/pull/17643)）：为 inclusionAI 的 **Ling-3.0-tiny / Ling-3.0-flash**（基于 Bailing MoE V3 架构）实现 MLX 引擎支持，目标平台为 Apple silicon (Metal) 与 NVIDIA DGX。

**社区呼声高的长期请求：**

- **Intel 集成显卡支持**（[Issue #3113](https://github.com/ollama/ollama/issues/3113)）：请求适配 Intel Iris Xe 等集成 GPU。该 Issue 创建于 2024 年，当前 75 👍、34 条评论，仍在开放状态。

**模型拉取风险提示：**

- **qwen3.8 下载失败**（[Issue #17816](https://github.com/ollama/ollama/issues/17816)）：`ollama run qwen3.8` 拉取 manifest 时报 `Error: EOF`，需确认是否为 registry 侧问题。

---

## 性能与优化

**已报告的性能缺陷：**

- **MLX 引擎缺少请求间 prompt/prefix 缓存**（[Issue #17829](https://github.com/ollama/ollama/issues/17829)）：多步 agent 会话中，每一步都会从头重复处理约 20-30K token 的完整 prompt，TTFT 随会话轮次线性恶化。影响 qwen3.8:27b-mlx 等模型，M1 Ultra 128GB 环境下复现。

**优化型 PR（多为可观测性改进）：**

- **Embedding 输入截断时添加警告**（[PR #17799](https://github.com/ollama/ollama/pull/17799)）：`/api/embed` 默认 `truncate: true` 时会静默截断超长输入，该 PR 使服务端在截断时返回警告。
- **非交互式输出净化**（[PR #17112](https://github.com/ollama/ollama/pull/17112)）：`ollama run` 在 stderr 非 TTY 时抑制 ANSI 控制字符，便于脚本/管道解析输出。

---

## 稳定性与回归

按严重程度排列：

| 严重度 | Issue/PR | 描述 | 状态 |
|---|---|---|---|
| 🔴 高 | [#17822](https://github.com/ollama/ollama/issues/17822) | **本地 API 被错误地要求 API Key**：干净本地环境（无云端登录、无 `OLLAMA_API_KEY`）调用 `/api/embed` 和 `/api/generate` 返回 401 `Invalid API Key` | OPEN，无 fix PR |
| 🔴 高 | [#17617](https://github.com/ollama/ollama/issues/17617) | **`deepseek-v4-flash:cloud` 泄露 `<think>` 标记**，经 Anthropic-compat 端点驱动 Claude Code 陷入 **193 次相同工具调用死循环，约 3100 万 token 消耗** | OPEN，无 fix |
| 🔴 高 | [#17825](https://github.com/ollama/ollama/issues/17825) | **qwen3.8:27b 工具调用解析失败（HTTP 500）后，重试相同请求永久挂起**，无响应无日志，直至 runner 被回收 | OPEN，无 fix |
| 🟠 中 | [#17804](https://github.com/ollama/ollama/issues/17804) | **MLX 视觉模型处理 24.5MP 高分辨率图片时申请约 125GB Metal buffer 并崩溃**（Qwen3.8-27-mlx，48GB 内存 MBP） | OPEN，无 fix |
| 🟠 中 | [#17833](https://github.com/ollama/ollama/issues/17833) | **v0.32.14 CPU 占用 50-80% 回归**，即便模型 100% 在 GPU 上 | OPEN，已确认 v0.32.13 正常 |
| 🟠 中 | [#17823](https://github.com/ollama/ollama/issues/17823) | **MLX 引擎`think: false` 对 Gemma 4 返回空内容**（0.32.14 回归） | OPEN |
| 🟠 中 | [#17814](https://github.com/ollama/ollama/issues/17814) | **两张像素尺寸完全相同的图片在一请求中折叠为一张**，qwen3.x 视觉模型静默丢失输入 | OPEN |
| 🟡 低 | [#17831](https://github.com/ollama/ollama/issues/17831) | `OLLAMA_HOST=0.0.0.0:8200` 在 systemd 配置中被解析为 IPv6 而非 IPv4 | OPEN |
| 🟡 低 | [#17832](https://github.com/ollama/ollama/issues/17832) | `CUDA_VISIBLE_DEVICES` 多卡环境（3×H200）下无法正确指定 GPU | OPEN |
| 🟡 低 | [#17821](https://github.com/ollama/ollama/issues/17821) | 网络断开时 Ollama 重启导致会话丢失 | CLOSED |
| 🟡 低 | [#17812](https://github.com/ollama/ollama/issues/17812) | qwen3.8:27b 原生 Web 搜索报 `500: no user query found in messages` | CLOSED |

**相关修复 PR（部分已合入）：**

- [PR #17624](https://github.com/ollama/ollama/pull/17624)（CLOSED）修复 integrations 配置为 `null` 时 `LoadIntegration` 返回 nil 导致 panic 的问题
- [PR #17623](https://github.com/ollama/ollama/pull/17623)（CLOSED）`ollama launch` 接受 Claude Code 的 `[1m]` context window 模型后缀，修复启动校验失败
- [PR #17112](https://github.com/ollama/ollama/pull/17112)（CLOSED）抑制非 TTY 场景下的 ANSI 控制字符

---

## 对应用开发者的意义

1. **⚠️ 云端模型驱动的 Agent 请设置防御性护栏**：`deepseek-v4-flash:cloud` 的 `<think>` 泄漏问题（[#17617](https://github.com/ollama/ollama/issues/17617)）展示了模型输出污染如何演变为高成本死循环（193 次调用 / 31M tokens）。在 Anthropic-compat 端点上构建 agent 时，建议增加最大连续工具调用次数限制、重复调用检测，以及 `<think>` 泄漏的响应清洗。

2. **⚠️ 本地 API 出现意外 401 时先检查云凭证**：如果干净本地环境上的 `/api/embed`、`/api/generate` 突然报 `Invalid API Key`（[#17822](https://github.com/ollama/ollama/issues/17822)），这可能是配置读取逻辑回归。生产环境建议保留 v0.32.13 作为回退版本，并监控升级后的认证行为。

3. **⚠️ 工具调用失败后的重试策略需谨慎**：`qwen3.8:27b` 在工具解析失败后，同一请求的重试会永久挂起（[#17825](https://github.com/ollama/ollama/issues/17825)）。建议在客户端层面对 500 响应后的重试增加超时熔断，或在重试前强制 recycle runner。

4. **⚠️ MLX 后端多步推理成本会线性膨胀**：由于缺少 prefix 缓存（[#17829](https://github.com/ollama/ollama/issues/17829)），长会话的 TTFT 会逐渐恶化。当前建议在 MLX 上使用短 prompt 或手动管理上下文窗口，等待官方缓存实现。

5. **注意 `/api/generate` 与 `/api/chat` 的行为差异**：`/api/generate` 在设置 `format` 时会静默忽略 `think: true`（[#17544](https://github.com/ollama/ollama/issues/17544)）。对 thinking 模型做结构化输出时，优先使用 `/api/chat` 或在请求中加入显式验证。

6. **Embedding 截断是静默的**：`/api/embed` 默认 `truncate: true` 且不返回截断标记（[PR #17799](https://github.com/ollama/ollama/pull/17799) 正在修复中）。对检索质量敏感的应用，请自行对长文本做分块，不要依赖服务端截断。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

## 今日速览

过去 24 小时 LiteLLM 无新版本发布，社区讨论集中在 **预算执行绕过**、**长驻内存增长导致 OOM**、**Anthropic 兼容层多类 400 错误** 三大问题。PR 侧则密集推进了 **定价/成本核算修复**（批处理、流式 usage、priority 计费）与 **安全加固**（callback 凭据脱敏、/health 明文泄露）；同时新增 **FLUX 3 视频生成**、**Amazon Comprehend Medical** 等 Provider 支持。

## 新模型与硬件支持

- **Black Forest Labs FLUX 3 视频生成**：新增 `black_forest_labs/flux-3-video`，覆盖文生视频、图生视频、续写、草稿模式、关键帧等能力（PR #37224）。
  https://github.com/BerriAI/litellm/pull/37224
- **Amazon Comprehend Medical 直通 Provider**：新增 SigV4 签名的 `/comprehendmedical` 路由，使临床文本负载可纳入网关鉴权、日志与支出追踪（PR #37229）。
  https://github.com/BerriAI/litellm/pull/37229
- **Azure Document Intelligence OCR 原生输出**：`/v1/ocr` 新增 `req_format=native`，直接返回 Azure `analyzeResult` 结构，同时保留逐页成本记录（PR #37194）。
  https://github.com/BerriAI/litellm/pull/37194

## 性能与优化

今日无重大吞吐/延迟/显存优化动态，相关 PR 集中在路由能力扩展而非性能数字提升：

- **复杂度路由器可自定义层级**：`tier_definitions` 允许运维定义自己的 tier 集合，不再硬编码 SIMPLE/MEDIUM/COMPLEX/REASONING（PR #37226）。
  https://github.com/BerriAI/litellm/pull/37226
- **plan 模式最低层级约束**：`plan_mode_min_tier` 可防止 coding-agent 在 plan 模式被路由到廉价档位（PR #37230）。
  https://github.com/BerriAI/litellm/pull/37230

## 稳定性与回归

按严重程度排序：

| 严重度 | 问题 | 状态 |
|--------|------|------|
| **严重** | 预算执行绕过：v1.82.3 上 key/user `max_budget` 在 spend 超标后仍不拦截；另有全局 budget limiter 未注册、project budget 未纳入原子预扣路径等问题 | #26672 开放、#27381 关闭、#34101 开放 |
| **严重** | 长驻内存持续增长导致 Pod OOM（升级 v1.82.0 后） | #25219 开放，14 评论 |
| **严重** | 单个持久化 `alpha/beta=0` 单元导致 adaptive_router 永久 HTTP 500（`gammavariate: alpha and beta must be > 0.0`） | #35590 开放 |
| **高** | Anthropic 翻译层：`vector_store_ids` 被透传导致 Anthropic 400 | #23741 开放，12 👍 |
| **高** | Anthropic `/v1/messages` guardrail 系统提示在 1.98.0 回归，Anthropic 400 | 已有 fix PR #37231 |
| **高** | Bedrock CountTokens 对 Claude Opus/Sonnet 5 不支持时静默返回低估 token 数 | #37102 开放，8/16 创建 |
| **高** | 流式 fallback 将 assistant prefill 块发给不支持 `prefix=True` 的目标，导致失败 | #27967 开放 |
| **中** | `GET /health` 以明文返回 `extra_headers`、`aws_session_token` | #36898 开放，8/14 创建 |
| **中** | callback 凭据随 auth 对象写入 request metadata，存在明文泄露风险 | 已有 fix PR #37233 |
| **中** | 批处理检索时按公共费率计价，忽略 deployment 自身定价；Bedrock 批处理成本恒为 $0 | 已修复，PR #37077/#37219 合入 |
| **中** | 流式 `usage.cost` 按客户端 model alias 计价，导致价格表错位及缓存抵扣错误 | 已有 fix PR #36879 |
| **中** | `service_tier=priority` 被静默按默认费率计费（gpt-4o/4.1 系列） | #37046 已关闭 |
| **低** | `GET /v1/batches` 接受超范围 `limit` 值，如 `-1` 导致分页死循环 | 已修复，PR #37198 合入 |
| **低** | shadow_eval 崩溃（tuple 不可原地修改）及 judge 输出超长解析失败 | 已有 fix PR #37232 |
| **低** | Bedrock 批处理输出文件无法从独立输出桶检索 | 已修复，PR #31435 合入 |

## 对应用开发者的意义

1. **计费与配额不可全信预算字段**：若依赖 `max_budget` 做多租户硬限流，请关注 #26672/#34101 的修复进度；在修复版本发布前建议额外用外部配额机制兜底。批处理/流式定价修复合入后，基于批量和流式 token 的费用核算会更准确。
2. **注意安全暴露面**：`/health` 明文泄露敏感 header 与 callback 凭据写入 metadata 是实际风险，建议升级到包含 #37233 的版本，并检查已有日志/审计记录是否被污染。
3. **Anthropic 兼容层仍有坑**：使用 Claude 模型且启用 Guardrail 的团队，1.98.0 存在 400 回归；`vector_store_ids` 透传问题仍在。若被阻塞，可暂时绕过 LiteLLM 直连 Anthropic。
4. **新能力对垂直场景友好**：FLUX 3 视频、Amazon Comprehend Medical、Azure OCR 原生输出可以直接接入现有网关鉴权、日志和成本系统，适合医疗文本和文档处理类应用快速上线。
5. **Coding Agent 路由更可控**：`plan_mode_min_tier` 和自定义 tier 集让复杂路由器的行为可解释、可调整，适合对成本分层有明确要求的 Agent 平台。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

## Unsloth 动态日报 2026-08-18

### 今日速览
过去 24 小时无新版本 Release，官方侧重点转向**Studio 稳定性修复**与**非 NVIDIA 硬件支持补齐**（AMD VRAM 报告、ROCm 加载失败重试）。社区侧主要诉求集中在 **Intel GPU 导入兼容性**（#3533）以及 Studio 桌面端的**工具调用、VoIP 权限与内存释放问题**。

### 新模型与硬件支持
- **Intel Arc 系列导入失败确认**：由于调用 `torch.xpu.memory.mem_get_info()` 不受支持，Unsloth 在 Intel Arc B580 上无法正常 import。目前仍为 Open 状态，无对应修复 PR。 · [Issue #3533](https://github.com/unslothai/unsloth/issues/3533)
- **Windows AMD ROCm VRAM 利用率报告**：新增两个 PR，通过 DXGI LUID 关联适配器计数器，解决 Windows 下 ROCm 显存读取不准确的问题。 · [PR #8863](https://github.com/unslothai/unsloth/pull/8863)、[PR #8793](https://github.com/unslothai/unsloth/pull/8793)
- **Linux CPU / Vulkan 后端安装改进**：installer 支持 Linux 下 torch 2.11 CPU 版、无 ROCm 时 AMD 走 Vulkan，并附带 gfx1033 门控与三个安装修复。 · [PR #8412](https://github.com/unslothai/unsloth/pull/8412)
- **Hub 支持非 GGUF 图像/视频模型运行**：修复合入 `safetensors` 的图像/视频模型（如 `unsloth/Z-Image-Turbo-unsloth-bnb-4bit`）Run 按钮灰色问题。 · [PR #8855](https://github.com/unslothai/unsloth/pull/8855)

### 性能与优化
- **剔除 pandas 启动依赖**：将 pandas 移出后端启动 import 图，减少 Studio 冷启动约 7s 的 Python 解释开销（约 0.7s 来自 pandas）。 · [PR #8962](https://github.com/unslothai/unsloth/pull/8962)
- **滚动上下文窗口保留 evicted turns**：为长对话增加滚动上下文，并持久化被逐出的轮次（支持搜索），避免长对话重载时的全量 prefill。 · [PR #9074](https://github.com/unslothai/unsloth/pull/9074)
- **视觉 projector 显存占位修复**：GGUF 模型加载时，将 mmproj 显存占用计入显存容量计算，放不下时将其放置于 CPU，避免 OOM。 · [PR #9063](https://github.com/unslothai/unsloth/pull/9063)

### 稳定性与回归
- **Studio 后端 SQLite 死锁**：运行数分钟后，所有 worker 阻塞在 `sqlite3.connect()`/`close()`，服务停止接收新连接，CPU 占用大幅升高（无 fix PR）。 · [Issue #9008](https://github.com/unslothai/unsloth/issues/9008)
- **ROCm 后端无法加载任何模型**：`libamdhip64.so.7` 与 `<libhsa-runtime64>` 版本不匹配导致加载失败。已有 fix PR 通过重试捆绑 HIP 库解决。 · [Issue #8998](https://github.com/unslothai/unsloth/issues/8998) → [PR #9002](https://github.com/unslothai/unsloth/pull/9002)
- **长 Qwen3.8 GGUF 对话重载后全量 prefill 约 11 分钟**：模型重载后丢失可复用 prompt 状态，导致长会话恢复时全量 prefill（与 #9074 滚动上下文相关）。 · [Issue #9037](https://github.com/unslothai/unsloth/issues/9037)
- **外部提供商工具调用被误触**：模型未发起工具调用时，Studio 仍可能额外“nudge”一次，影响 agent 流程。修复 PR 增加了 gate 控制并保留 retry 上下文。 · [Issue #8907](https://github.com/unslothai/unsloth/issues/8907) → [PR #9125](https://github.com/unslothai/unsloth/pull/9125)
- **GGUF 模型加载后系统内存不释放**：即使 `-ngl -1` 全量 offload 到 GPU，加载期间占用的部分系统内存仍被保留。 · [Issue #9033](https://github.com/unslothai/unsloth/issues/9033)
- **macOS 语音权限级联问题**：误点“不允许”后无法重新授权，且重启/重装均不生效。涉及 WebKitGTK 与系统 privacy 设置的同步问题。 · [Issue #9001](https://github.com/unslothai/unsloth/issues/9001)、[Issue #8678](https://github.com/unslothai/unsloth/issues/8678)

### 对应用开发者的意义
- **API 可无密钥访问**：允许用户显式开启无鉴权 API（对齐 LM Studio / Ollama），便于快速接入现有 OpenAI 兼容工具链。 · [PR #9102](https://github.com/unslothai/unsloth/pull/9102)
- **LAN 访问修复**：修复通过 `http://<LAN-IP>` 访问 Studio Web UI 时 `crypto.randomUUID` 报错导致的空白页问题，远程开发环境可用性提升。 · [PR #9075](https://github.com/unslothai/unsloth/pull/9075)
- **工具调用 ID 规范化**：将前端生成的 66 字符 `<provider>:<uuid4>` ID 归一化，避免 OpenAI（64 字符上限）和 Mistral（9 位字母数字）等外部提供商拒绝调用。 · [PR #9116](https://github.com/unslothai/unsloth/pull/9116)
- **工具循环行为可配置化**：GGUF 与外部工具循环统一使用同一 nudge 开关，规避误触发的场景，使 agent 自动化更可控。 · [PR #9126](https://github.com/unslothai/unsloth/pull/9126)
- **项目内聊天保活**：修复项目内聊天中切换视图导致生成中断的问题，意味着长任务（如 Deep Research / RAG）可在后台稳定执行并自动恢复。 · [PR #9129](https://github.com/unslothai/unsloth/pull/9129)

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*