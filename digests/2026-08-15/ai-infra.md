# AI 基础设施日报 2026-08-15

> 生成时间: 2026-08-14 23:14 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施生态横向分析报告（2026-08-15）

## 1. 生态全景

当前推理栈的竞争焦点已从"跑通模型"转向"在新硬件（ROCm、Blackwell、消费级 GPU）与新型模型架构（DeepSeek-V4 稀疏 MLA、Kimi-K3、混合线性注意力）上同时保证性能与正确性"。三大引擎（vLLM / SGLang / llama.cpp）均在 24 小时内密集提交 kernel 级优化与严重 bug 修复，其中 AMD ROCm 正确性问题（静默检索损坏、非法内存访问）已成为生产部署的头号风险。本地/微调侧（Ollama / Unsloth）则被 Qwen3.8 的发布节奏牵引，快速跟进低资源运行与量化支持。网关层（LiteLLM）无明显新功能，集中于回归修复与安全漏洞，反映其作为中间层在生态快速变化时承受的兼容性压力。

## 2. 各项目活跃度对比

*以下 Issue/PR 数为当日简报中明确涉及的去重计数，非 GitHub 全量口径。*

| 项目 | Issues 提及 | PRs 提及 | Releases | 严重度分布 | 定位标签 |
|---|---|---|---|---|---|
| vLLM | ~17 | ~10（5 笔 fix 进入 Ready） | 0 | 🔴 4 无修复；🟠 6（4 有 fix） | 生产级推理引擎 |
| SGLang | ~18 | ~15（3 笔已合并/关闭） | 0 | 🔴 2 无修复；🟠 4；🟡 8 | 推理引擎 + Day-0 硬件覆盖 |
| llama.cpp | ~18 | ~16 | **10**（b10425–b10435） | P0 3 / P1 3 / P2 多 | 轻量本地/边缘运行时 |
| Ollama | ~9 | ~8 | **3**（v0.32.11–13） | 🟠 2 无修复；🟡 3 | 本地运行时 + 产品封装 |
| LiteLLM | ~11 | ~10 | 0 | 🔴 2（有 fix）；🟠 3 | LLM 网关 / 代理层 |
| Unsloth | ~19 | ~9（含 2 重复/已关） | 1（v0.1.800-beta） | ✅ 5 已修复；⚠️ 9 待查 | 微调 + 本地推理 |

**节奏判断**：llama.cpp 以高频发版（24h 10 版）换取快速迭代，但 P0 问题集中在 SYCL/RPC 后端，呈"功能跑得快、稳定性风险同步放大"态势。vLLM 与 SGLang 发版克制，通过 issue 跟踪 + PR Ready 的方式收敛复杂正确性问题，适合对稳定性敏感的生产用户。

## 3. 模型支持竞速

| 新模型/新架构 | vLLM | SGLang | llama.cpp | Ollama | Unsloth |
|---|---|---|---|---|---|
| **Qwen3.8 系列** | — | ✅ FP8 DGX Spark 社区验证 | — | ✅ **v0.32.12 官方支持**（含 MLX/Apple Silicon） | ✅ **v0.1.800-beta 支持 27B/2.4T**，17GB RAM + NVFP4 |
| **Kimi-K3** | ✅ ROCm 使能 + torch.compile | ✅ AMD MI35x CI + 安装文档 | 🔄 文本模型 PR #26185 推进中 | — | — |
| **DeepSeek-V4（含 Flash/Pro）** | ✅ 主力支持 + 多笔正确性修复 | ✅ sparse attention 优化 + 稳定性跟踪 | ⚠️ 多后端崩溃/退化 issue | — | — |
| **MiniMax-Text-01 / M1** | — | — | 🔄 PR #27018（补 2025 年老支持） | — | — |
| **MiniMax-H3** | — | ✅ 8×B300 验证 | — | — | ⚠️ AMD 环境无法运行 |
| **Nemotron VoiceChat-11B S2S** | — | ✅ 新增支持 | — | — | — |

**判断**：企业级前沿模型（DeepSeek-V4 / Kimi-K3）的赛道上，**vLLM 与 SGLang 明显领先**，且差距体现在 kernel 层（AITER/DeepGEMM/FlashInfer）而非模型适配壳。消费级与个人开发者赛道上，**Ollama 与 Unsloth 凭借 Qwen3.8 的低资源方案（GGUF/MLX/NVFP4）跑在最前**。llama.cpp 对 MiniMax/Kimi-K3 的支持仍处 PR 审查期，落地速度落后约一个迭代周期。

## 4. 性能优化前沿

| 优化方向 | 代表动作 | 量化效果 |
|---|---|---|
| **稀疏/MLA 注意力** | vLLM gfx950 sparse-MLA decode 专用分支；SGLang TRT-LLM sparse attention 的 fused norm+RoPE+fp8 store | 目标提升 MI355X decode 吞吐；SPA 正确性问题仍是瓶颈 |
| **投机解码 / MTP** | vLLM MRV2 acceptance estimation（#52228）+ bad_words off-by-one 修复；SGLang GDN MTP cache mode + FlashInfer 集成 | MRV2 场景 benchmark duration 96.0s→81.3s（**~15%**） |
| **SYCL / ROCm 算子** | llama.cpp SYCL q4_K FFN 三算子融合 + TILE KV decode；vLLM AITER pybind11 版本对齐 | TILE kernel 在 BMG 上 **+42%~169%**；q4_K FFN +2.8% |
| **CUDA Graph 覆盖扩大** | vLLM ViT Encoder 全量捕获 RFC；SGLang breakable CUDA graphs 修复 | 多模态 prefill 延迟潜在数量级收益 |
| **KV cache 与上下文** | vLLM LMCache ROCm 镜像集成 + KV offload 断言修复；Ollama 模型元数据缓存 | 单次请求固定开销减少 ~300ms |
| **量化路径** | SGLang NVFP4→MXFP4 在线重量化（AMD）；Unsloth NVFP4 权重 + Dynamic GGUF | 消费级硬件上运行 2.4T 模型成为可能 |

**共性趋势**：火力集中在三处——① 让新型稀疏/线性注意力模型（DeepSeek-V4、GDN、Mamba-2）在 AMD + 消费级 Blackwell 上端到端跑通；② 投机解码从"能用"走向"算得准"（接受率估计、PP 并行、状态恢复）；③ 以 CUDA Graph 捕获边界扩展和算子融合削减 launch 开销。

## 5. 分层定位差异

| 层级 | 项目 | 核心价值主张 | 今日动态反映的竞争焦点 |
|---|---|---|---|
| **推理引擎（生产）** | vLLM / SGLang | 高吞吐 serving、多卡并行、kernel 深度优化 | DeepSeek-V4/Kimi-K3 的 ROCm 正确性与 MRV2 投机解码收敛 |
| **推理引擎（本地/边缘）** | llama.cpp | 单机/边缘部署、多后端（SYCL/Vulkan/Metal/CPU）、低资源 | SYCL 算子提速与 P0 稳定性回归的赛跑 |
| **本地运行时 + 产品** | Ollama / Unsloth | 一键部署、模型管理、Agent 工具链、微调+推理闭环 | Qwen3.8 的消费级体验（MLX/17GB RAM）；Unsloth 以微调差异化切入 |
| **网关 / 代理层** | LiteLLM | 多 provider 路由、鉴权、成本追踪、可观测性 | 回归修复（Admin UI 404）+ 安全漏洞（Langfuse 凭据泄露） |

**结构性观察**：vLLM 与 SGLang 的正面竞争已进入 kernel 层面（同一模型、同一 AMD 硬件上的 AITER/DeepGEMM 支持互相比对）；llama.cpp 与 Ollama 存在上下游关系（llama.cpp 是 Ollama 的推理内核之一），但 Ollama 正在通过 MLX 后端与 agent 工具链建立自己的生态位。Unsloth 是唯一从"训练/微调"切入推理的项目，其 Studio 产品正在模糊这一边界。

## 6. 值得关注的趋势信号

1. **AMD ROCm 已从"支持项"变为"一等公民"，但生产风险仍高**。三大引擎当日均有 ROCm 相关动作，但 vLLM 存在两笔无修复的严重问题（DeepSeek-V4-Flash 静默检索损坏 #52109、GPU 内存访问错误 #48266），SGLang 亦有类似非法访问。**决策者若在 MI325X 上部署 DeepSeek-V4，应坚持 CUDA 后端或等待修复确认。**

2. **Qwen3.8 成为消费级生态的"标准载荷"**。Ollama/Unsloth 同日发布官方支持，SGLang 完成 DGX Spark 验证，且 API 兼容性问题（system message 位置、developer role）在多项目同时出现——说明新模型发布对工具链的冲击是系统性的，应用层需预留适配窗口。

3. **Agent 工作流正在反向塑造基础设施需求**。Ollama 将 DeepSeek Harness/Muse Code 直接嵌入 `ollama launch`；Unsloth 修复工具调用 reasoning 传递与 PDF 附件问题；vLLM/SGLang 在 Responses API 上暴露 streaming/非 streaming 类型不一致与 DELETE 缺失。**Agent 开发者应关注：工具调用链路上的状态保存、日期感知、附件处理已成为 runtime 的差异化竞争力。**

4. **正确性 bug 集中在"新型注意力 + 投机解码 + 长上下文"三重组合**。静默检索损坏、CUDA Graph 输出乱码、accepted length 异常均出现在此交集。任何在生产环境组合这三者（如 DeepSeek-V4 + MTP + 长 prompt）的团队，都应建立针对输出语义的回归测试，而非仅依赖 benchmark 指标。

5. **安全事件开始出现在网关与 RPC 层**。LiteLLM 的 Langfuse 凭据泄露风险（#36862）与 llama.cpp RPC 未认证空指针解引用（#25299）分别代表两类问题：**应用层配置权限过宽、基础设施层协议未认证**。随着多实例/多节点部署普及，这两类风险将更频繁暴露。

6. **错误码标准化与可观测性成为公共议题**。vLLM 计划将 2k+ 处裸异常收敛为语义化类型（#48227）；llama.cpp 在 decode 期间开放 `/metrics`；Ollama 做元数据缓存减延迟。**对平台团队的建议：现在开始减少对异常字符串匹配的依赖，并统一遥测采集标准，以降低未来 breaking change 的迁移成本。**

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 — 2026-08-15

## 今日速览

过去 24 小时 vLLM 无新版本 Release，社区动态集中于 DeepSeek-V4 系列在 ROCm/Blackwell 平台上的正确性与性能问题修复、Kimi-K3 的 ROCm 支持补强，以及 Model Runner V2（MRV2）在投机解码路径上的多笔 Bugfix。值得关注的是，围绕 DeepSeek-V4（含 Flash/Pro 变体）的 GPU 内存访问错误、静默检索损坏及 CUDA Graph 输出乱码等线上严重问题仍在持续发酵，多笔修复 PR 已进入 Ready 状态。

---

## 版本发布与破坏性变更

**无新版本 Release。**

但需注意以下合并/进行中的行为变更：

- **DeepSeek-V4 Eager CUDA Graph Region 选择逻辑调整**（[PR #52401](https://github.com/vllm-project/vllm/pull/52401)）：此前 #51768 将 DeepSeek-V4 默认切换至 MRV2 并拒绝 MRV1 + PIECEWISE 组合，导致 ROCm 后端性能回退。该 PR 改为按 Model Runner 选择 region（MRV1 走窄 region、MRV2 走宽 region），并恢复 MRV1 在 ROCm 上的可用性。若你正在 ROCm 上运行 DeepSeek-V4，此变更可能影响当前 runner 选择行为。

---

## 新模型与硬件支持

- **[ROCm] Kimi-K3 支持与 Roadmap 跟踪**（[Issue #50682](https://github.com/vllm-project/vllm/issues/50682)）：社区正在系统跟踪 Kimi-K3 在 ROCm 上的 Day-0 功能基线（AITER fused-moe a16w4/a8w4）与后续性能优化，预计将逐步补齐与 CUDA 后端的特性对齐。
- **[ROCm] DeepSeek-V4 端到端使能与优化清单**（[Issue #41820](https://github.com/vllm-project/vllm/issues/41820)）：跟踪 mHC/HCA/CSA/MoE/MTP 等关键模块在 ROCm 上的落地状态，当前仍有多个 kernel 与运行时层面的 block 未闭环。
- **[ROCm] Docker 镜像集成 LMCache KV Connector**（[PR #51208](https://github.com/vllm-project/vllm/pull/51208)）：ROCm 镜像将随发布流程安装 LMCache，与 CUDA 镜像行为对齐，解决开箱即用问题。
- **[ROCm] 移除 Dockerfile.rocm 中的 pybind11 固定版本**（[PR #52400](https://github.com/vllm-project/vllm/pull/52400)）：AITER kernels 构建时使用 pybind11 3.0.4，而 Dockerfile 安装 3.1.0 导致 API 不兼容，影响 Kimi K2.6/K3 等模型。此 PR 消除版本漂移风险。
- **[ROCm] Kimi-K3 启用 torch.compile 支持**（[PR #52190](https://github.com/vllm-project/vllm/pull/52190)）：为 Kimi-K3 模型类添加 `@support_torch_compile`，使 post-grad fusion passes（如 `aiter::fused_qk_rmsnorm_kernel`、`aiter::allreduce_fusion_kernel_1stage`）得以生效。
- **[RFC] ViT Full CUDA Graph**（[Issue #38175](https://github.com/vllm-project/vllm/issues/38175)）：面向 Qwen3-VL、GLM-V、Kimi K2.5 等多模态模型，计划将 ViT Encoder 纳入完整 CUDA Graph 捕获，减少 kernel 启动开销。已获 27 条评论，仍处设计讨论阶段。

---

## 性能与优化

- **[ROCm/gfx950] DeepSeek-V4 sparse-MLA Decode 优化**（[PR #52212](https://github.com/vllm-project/vllm/pull/52212)）：新增 gfx950 专用分支与 workload-aware split policy，纯 Triton 实现，不改变输出行为，目标提升 MI355X 上的 decode 吞吐。
- **[MRV2] 投机解码 Adaptive Verification 的 Acceptance Estimation**（[PR #52228](https://github.com/vllm-project/vllm/pull/52228)）：为 DeepSeek-V4-Flash + MTP=3 场景提供非 dspark 路径的接受率估计。基准测试显示并发 64 下，相同负载 benchmark duration 从 96.01s 降至 81.25s（约 15% 提升）。
- **[RFC] ViT Full CUDA Graph（详见上节）**：对多模态生产场景的 prefill 延迟有潜在数量级收益，值得关注后续设计定稿。
- **[DeepGEMM] SM 12.x Kernel 覆盖缺口跟踪**（[Issue #41063](https://github.com/vllm-project/vllm/issues/41063)）：DeepSeek-V4-Flash 在 RTX 50 / GB10 上无法端到端跑通 DeepGEMM，已整理 10 层缺失算子地图，是消费级 Blackwell 用户的主要性能瓶颈。

---

## 稳定性与回归

按严重程度排序，标注是否已有修复 PR：

### 🔴 严重（需立即关注）

- **[ROCm/gfx942] DeepSeek-V4-Flash 静默检索损坏**（[Issue #52109](https://github.com/vllm-project/vllm/issues/52109)）：prompt ≥ 4-5k tokens 时 AITER sparse indexer 产生静默 corruption，不报错但输出错误。**无 fix PR**，ROCm 7.14 + MI325X 环境受影响。
- **[ROCm/gfx942] DeepSeek-V4 GPU 内存访问错误**（[Issue #48266](https://github.com/vllm-project/vllm/issues/48266)）：序列跨 2048 tokens 时 worker crash，涉及 sparse_attn_indexer + FP8 KV cache，MI325X TP=4 必现。**无 fix PR**。
- **[Bug] libcudart.so.13 ImportError**（[Issue #52300](https://github.com/vllm-project/vllm/issues/52300)）：vLLM 0.21.0 + CUDA 12.6 环境下 `from vllm.entrypoints.llm` 直接 ImportError，影响所有使用 verl 等外部框架的用户。**无 fix PR**，建议先确认是否误装 CUDA 13 运行时。
- **[Bug] NVFP4 Marlin EngineDeadError**（[Issue #49926](https://github.com/vllm-project/vllm/issues/49926)）：NVFP4 量化模型在推理中触发 EngineDeadError，具体触发条件待确认。**无 fix PR**。

### 🟠 中高（有修复进行中）

- **[Bug] DeepSeekV4-Flash 内联系统消息输出错误**（[Issue #46710](https://github.com/vllm-project/vllm/issues/46710)）：PR #46025 改变了 `chat_template` 行为，导致内联 system message 处理异常。**无 fix PR**，受影响用户需关注 chat template 相关变更。
- **[Bug] DeepSeek-V4 CUDA Graph 并发乱码**（[Issue #41331](https://github.com/vllm-project/vllm/issues/41331)）：并发相同请求时输出 garbled，疑似 CUDA Graph 捕获与输入缓冲区复用冲突。**无 fix PR**。
- **[Bug] bad_words 投机解码 off-by-one**（[PR #52311](https://github.com/vllm-project/vllm/pull/52311)）：MRV2 spec-decode 分支中 `_bad_words_kernel` 的 draft-prefix 匹配存在 off-by-one，**已有 fix PR（Ready 状态）**。
- **[Bug] KV Offload `max_offload_tokens` 断言错误**（[PR #52397](https://github.com/vllm-project/vllm/pull/52397)）：混合 Mamba-attention 模型下 partial-tail 边界触碰断言，**已有 fix PR**。
- **[Bug] DSpark 未量化 Draft 模型初始化崩溃**（[PR #52396](https://github.com/vllm-project/vllm/pull/52396)）：`hf_overrides` 为非 dict 时 `get_draft_quant_config` 抛异常，**已有 fix PR**。

### 🟡 中低（跟踪中）

- **[Bug] Qwen3.5-35B-A3B DFlash 性能异常**（[Issue #50722](https://github.com/vllm-project/vllm/issues/50722)）：DFlash accepted length 仅 5-6，吞吐未达预期。**无 fix PR**。
- **[Bug] Mamba-2/GDN 混合模型 Prefix Caching 无效**（[Issue #51250](https://github.com/vllm-project/vllm/issues/51250)）：Qwen3.6-35B-A3B 上启用 prefix caching 后复用率为 0。**无 fix PR**。
- **[Bug] MTP 场景 CUDA Graph 显存估算为负**（[Issue #44740](https://github.com/vllm-project/vllm/issues/44740)）：-35 GiB 的估算导致 KV cache 过度分配并 OOM，GB10 Unified Memory 上触发。**无 fix PR**。
- **[Bug] Mamba-2 Triton 非法指令**（[Issue #37431](https://github.com/vllm-project/vllm/issues/37431)）：DGX Spark（SM121）上 `CUDA_LAUNCH_BLOCKING=1` 可绕过，根因待查。**无 fix PR**。
- **[Bug] ROCm Attention 误报 mm_prefix 支持**（[PR #52395](https://github.com/vllm-project/vllm/pull/52395)）：修正 `supports_mm_prefix` 返回 False，避免 ROCm 上 Prefix-LM 误用。**已有 fix PR**。

---

## 对应用开发者的意义

1. **DeepSeek-V4 系列在 ROCm 上仍具高风险**：如果你在 AMD MI325X 上生产部署 DeepSeek-V4-Flash/Pro，务必关注 #52109（静默检索损坏）与 #48266（内存访问错误）——它们不报错或直接 crash，且目前无修复。建议在修复前避免长 prompt 或降级到 CUDA 后端。

2. **MRV2 时代的投机解码行为正在快速收敛**：#52311（bad_words）、#52228（acceptance estimation）、#50514（pipeline parallel 支持）等多笔 PR 同时在途，EAGLE3/DSpark/MTP 在 MRV2 + PP 下的行为将显著变化。若你的 serving 栈深度定制了 spec decode 逻辑，建议关注这些合入后对默认行为的影响。

3. **入口点错误码标准化已在路上**（[Issue #48227](https://github.com/vllm-project/vllm/issues/48227)）：vLLM 计划将 2k+ 处裸 `raise ValueError/TypeError` 收敛为语义化异常类型。这属于 breaking change 的前置 RFC——若你的应用依赖字符串匹配异常信息，请尽早规划迁移。

4. **Responses API 缺失 DELETE 端点**（[Issue #39624](https://github.com/vllm-project/vllm/issues/39624)）：`VLLM_ENABLE_RESPONSES_API_STORE=1` 下，已存储的 response 无法显式释放，存在内存泄漏风险。当前仅 closed 状态，未合入实现。

5. **多模态推理的 CUDA Graph 覆盖即将扩大**（[Issue #38175](https://github.com/vllm-project/vllm/issues/38175)）：若你服务 Qwen3-VL / Kimi K2.5 等模型，ViT Encoder 的 CUDA Graph 支持将直接降低首 token 延迟和 CPU 开销，建议跟踪该 RFC 的后续实现。

6. **本地多实例部署需注意端口竞态**（[Issue #51275](https://github.com/vllm-project/vllm/issues/51275)）：`get_open_port()` 存在 TOCTOU 竞态，多实例并发启动时可能随机路由到错误实例。该 RFC 若落地将改变端口分配语义，当前建议显式指定 `--port` 规避。

---

*本日报数据来源：vllm-project/vllm GitHub Issue/PR 更新（2026-08-14 至 2026-08-15）。*

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 2026-08-15

> 基于 2026-08-14 夜间的 GitHub 数据生成。过去 24 小时无新 Release。

## 今日速览

- 无新版本发布，社区焦点集中在 Kimi-K3 的 AMD/GB200 支持、Router GEMM 精度问题，以及 Context Parallelism 路线图更新。
- 多个高影响 Bug 仍在活跃：DeepSeek-V4 长上下文 sparse attention 非法内存访问、Kimi-K3 PP8 分离式 Prefill 的 TTFT 异常、Diffusion attention backend 回退引入广泛错误。
- CI 追踪显示当前 `main` 分支仍有 3 个 broken、11 个 flaky，近期已修复 672 个问题。

## 新模型与硬件支持

- **Kimi-K3 安装文档与 MegaMoE 激活修正**
  - [PR #34886](https://github.com/sgl-project/sglang/pull/34886)：Kimi-K3 cookbook 切换到标准 Python/Docker 安装路径，不再要求预发布镜像。
  - [PR #34883](https://github.com/sgl-project/sglang/pull/34883)：Kimi-K3 MegaMoE 直接以 `activation="situ"` 调用 DeepGEMM，并补了 CPU 单测，依赖 DeepGEMM PR #78。
- **AMD 支持**
  - [PR #32568](https://github.com/sgl-project/sglang/pull/32568)：新增 Kimi-K3 8-GPU MI35x nightly accuracy CI，补齐 AMD Day-0 覆盖。
  - [PR #29328](https://github.com/sgl-project/sglang/pull/29328)：支持在 AMD 上将 NVFP4 checkpoint 在线重量化到 MXFP4，适配 MI355x 推理。
- **NemotronLabs Voicechat**
  - [PR #34873](https://github.com/sgl-project/sglang/pull/34873)：新增 NVIDIA-NemotronLabs-VoiceChat-11B S2S 模型部署支持。
- **MiniMax-H3 / Qwen3.8 / NPU**
  - [PR #34841](https://github.com/sgl-project/sglang/pull/34841)：验证 MiniMax-H3 `quality="high"` 在 8×B300 上的部署，并为每个验证过的部署单独配置 Cache-DiT 调度参数。
  - [Issue #34872](https://github.com/sgl-project/sglang/issues/34872)：Qwen3.8-27B-FP8 在单台 DGX Spark（GB10/SM121）上经社区验证通过。
  - [PR #34722](https://github.com/sgl-project/sglang/pull/34722)：针对 NPU 优化 LTX-2/2.3 diffusion 推理性能。

## 性能与优化

- **DeepSeek-V4 sparse attention**
  - [PR #32975](https://github.com/sgl-project/sglang/pull/32975)：为 TRT-LLM DSv4 sparse attention 增加 fused norm + RoPE + uniform fp8 store，属于性能优化。
- **M3 / GDN / DSA**
  - [PR #34014](https://github.com/sgl-project/sglang/pull/34014)：提升 M3 在 MI350 上的性能。
  - [PR #30967](https://github.com/sgl-project/sglang/pull/30967)：为 GDN 增加 MTP cache mode（final-state recompute），集成 FlashInfer kernel，并实现 overlap 的 CUDA-graph state recovery。
  - [PR #33006](https://github.com/sgl-project/sglang/pull/33006)（已关闭/合并）：DSA 场景改用 FlashInfer fused top-k 处理 packed PAGED rows，替代之前 fallback 的 SGL kernel。
- **CI / Benchmark 优化**
  - [PR #34882](https://github.com/sgl-project/sglang/pull/34882)：裁剪 Qwen3.5 FP8 GB300 nightly 性能矩阵，减少冗余覆盖和运行时间。
- **Qwen3 MoE 后端兼容**
  - [PR #34810](https://github.com/sgl-project/sglang/pull/34810)（已关闭/合并）：让 Qwen3 MoE 识别 DeepEP-class 后端，并修复 Mooncake EP / EPLB 的早期状态问题。

## 稳定性与回归

按严重程度排列：

- **严重：非法内存访问 / 启动崩溃**
  - [Issue #34718](https://github.com/sgl-project/sglang/issues/34718)：DeepSeek-V4 `fp8_paged_mqa_logits` sparse attention indexer 在长上下文请求下 illegal memory access，Open，暂无 fix PR。
  - [Issue #32283](https://github.com/sgl-project/sglang/issues/32283)：FlashInfer `RadixTopKRenormProbKernel_MultiCTA` 出现 CUDA coredump，Open，暂无 fix PR。
  - [Issue #34858](https://github.com/sgl-project/sglang/issues/34858)：DeepSeek-V4-Flash 在 GB200 启动时因重复 TVM FFI registration 中止，已 Closed，未发现对应修复 PR。
  - [Issue #26715](https://github.com/sgl-project/sglang/issues/26715)：`flashinfer_trtllm` BF16 MoE 在 CUDA graph capture 时触发非法内存访问回归，已 Closed 且标记 inactive，当前无修复。

- **高：功能错误 / 静默降级 / 延迟异常**
  - [Issue #34389](https://github.com/sgl-project/sglang/issues/34389)：Diffusion 的 attention backend fallback 变更导致大多数模型报错，Open，暂无 fix PR。
  - [Issue #34815](https://github.com/sgl-project/sglang/issues/34815)：Kimi-K3 使用 PP8 + PD-disaggregated prefill 时存在约 30 秒的负载无关 TTFT floor，Open，暂无 fix PR。
  - [Issue #34740](https://github.com/sgl-project/sglang/issues/34740)：`SGLANG_SIMULATE_ACC_LEN` 会静默把 detokenization 退化为 O(n²)，Open，暂无 fix PR。
  - [Issue #34177](https://github.com/sgl-project/sglang/issues/34177)：开启 breakable CUDA graphs 后 Cache-DiT 被静默禁用，已 Closed，未发现修复 PR。

- **中：精度 / API / 特定硬件兼容**
  - Router GEMM 输出精度相关三个 issue：NPU 上应为 fp32（[#34861](https://github.com/sgl-project/sglang/issues/34861)）、ROCm/AITER 上不应 cast to bf16（[#34857](https://github.com/sgl-project/sglang/issues/34857)）、deterministic 模式下 DeepSeek router 应保持 fp32（[#34758](https://github.com/sgl-project/sglang/issues/34758)），均 Open。
  - [Issue #34720](https://github.com/sgl-project/sglang/issues/34720)：XPU 上 Qwen3.5 GDN + speculative decode 传入 `intermediate_conv_window` 导致 `causal_conv1d_update_xpu()` 报错，Open。
  - [Issue #34786](https://github.com/sgl-project/sglang/issues/34786)：NEXTN 投机解码 + hybrid-mamba + lazy buffer 时 `mamba_next_track_idx is None` 导致 `TypeError`，Open；重复 issue #34787 已关闭。
  - [Issue #34716](https://github.com/sgl-project/sglang/issues/34716)：`/v1/responses` 接口中 `created_at` 在 streaming 返回 float、non-streaming 返回 int，Open。
  - [Issue #31896](https://github.com/sgl-project/sglang/issues/31896)：`cp=8` 时 `num_requests_running` 指标显示错误，Open。
  - [Issue #33984](https://github.com/sgl-project/sglang/issues/33984)：HiCache L1+L2+Mooncake(SSD) 后端出现 cache hit rate 非预期波动，Open。

- **CI 与基础设施**
  - [Issue #17050](https://github.com/sgl-project/sglang/issues/17050)：CI 追踪显示 3 broken / 11 flaky / 672 recently fixed。
  - [PR #34875](https://github.com/sgl-project/sglang/pull/34875)（已关闭/合并）：清理 11 条失效 CODEOWNERS 规则。

## 对应用开发者的意义

- **API 一致性**：如果直接消费 `/v1/responses`，注意 streaming 与 non-streaming 的 `created_at` 类型不一致；在官方修复前应避免在业务代码中强依赖其类型。
- **LoRA 运行时校验**：[PR #34885](https://github.com/sgl-project/sglang/pull/34885) 修复了 `Runtime.generate()` 对单个 string prompt 的 LoRA 路径校验问题，把 `assert` 换成显式 `ValueError`。对程序化调用 SGLang runtime 的 Agent/工具链更友好。
- **进程生命周期**：[PR #34884](https://github.com/sgl-project/sglang/pull/34884) 修正了 `BaseConnector` 的信号处理语义，可避免容器/子进程退出时临时目录泄漏或错误信号传播。部署自定义 connector 的团队值得关注。
- **精度与确定性**：Router GEMM 在 NPU/ROCm/deterministic 模式下的 dtype 问题会影响 DeepSeek 系列复现性。做精度回归或 benchmark 对比时，建议等修复后升级。
- **稳定性风险**：若使用 DeepSeek-V4 长上下文、Kimi-K3 PP8 PD 分离、hybrid-mamba + NEXTN 投机解码等组合，当前存在未修复的崩溃/性能风险。生产环境建议避开这些路径或关注对应 issue 的 fix PR。
- **新模型部署**：Kimi-K3、NemotronLabs Voicechat、MiniMax-H3 B300、Qwen3.8 DGX Spark 等支持正在快速补齐，但部分依赖未合并的 DeepGEMM PR（如 #34883），使用预构建 wheel 时需注意版本对齐。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报 2026-08-15

## 今日速览

过去 24 小时密集发布 b10425–b10435 共 10 个版本，主线是 SYCL 算子融合提速（q4_K FFN、gated-delta-net 写回）和 jinja 模板引擎的二次方复杂度修复。模型支持侧，MiniMax-Text-01/M1（#27018，修复 2025 年 1 月的老 issue #11290）和 Kimi-K3 文本模型（#26185）两个 PR 正在推进。稳定性方面，SYCL 在 Intel A770 上被报告完全崩溃（#27063），Windows ROCm 7.14 发行包缺失 hipblas.dll（#26996），需持续关注。

## 版本发布与破坏性变更

**b10435** — 修复 jinja `gather_string_parts` 的二次方开销（[#27034](https://github.com/ggml-org/llama.cpp/pull/27034)），解决长模板/长上下文下 prompt 处理耗时随长度平方增长的问题。  
🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10435

**b10434** — chat 模板新增 `reasoning_effort` 字段，OpenAI Chat Completions 中的 `reasoning_effort` 参数现在会传给 jinja 模板（含模型特定映射）。模板开发者可据此动态调整推理行为。  
🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10434

**b10431** — 为 `ggml_ssm_scan` 添加循环状态回滚（[#26623](https://github.com/ggml-org/llama.cpp/pull/26623)），先落地 CUDA。当前 CPU 端未启用，将在后续 PR 跟进。  
🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10431

**b10430** — 允许虚拟 iGPU 设备（[#26953](https://github.com/ggml-org/llama.cpp/pull/26953)）。  
🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10430

**b10429** — server 在 `llama_decode()` 执行期间可响应 `/metrics` 和 `/slots` 请求（[#27041](https://github.com/ggml-org/llama.cpp/pull/27041)），提升 decode 阶段的可观测性。  
🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10429

**b10426** — WebAssembly (wasi) 后端强制单线程，规避该平台多线程不稳定问题。  
🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10426

**b10428** — 清理示例文档和测试中的开发者个人路径（`/home/<user>/` → `/path/to/llama.cpp/`），无功能影响。  
🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10428

## 新模型与硬件支持

- **MiniMax-Text-01 / MiniMax-M1 支持**（[#27018](https://github.com/ggml-org/llama.cpp/pull/27018)）：基于 lightning attention 的模型支持 PR，修复 #11290。作者标注为“cleaned-up implementation from January 2025”，属于老模型补支持。
- **Kimi-K3 文本模型**（[#26185](https://github.com/ggml-org/llama.cpp/pull/26185)）：混合 KDA（线性）+ MLA（full）注意力，额外引入 cross-layer residual attention、latent MoE、situ activation 等新结构。
- **AMD iGPU UMA 内存检测修复**（[#27083](https://github.com/ggml-org/llama.cpp/pull/27083)）：CUDA 后端跳过 HIP 构建的 UMA override——AMD APU 通过 `hipMemGetInfo` 能报告准确显存，用 MemAvailable 会高估小显存系统的可用量。另有 [#26932](https://github.com/ggml-org/llama.cpp/pull/26932) 通过 sysfs VRAM 修复 Strix Halo 的检测问题，两个 PR 路径不同。
- **Laguna S 2.1 DFlash 支持请求**（[#26669](https://github.com/ggml-org/llama.cpp/issues/26669)）仍在开放中。

## 性能与优化

- **SYCL：q4_K 密集 FFN 三算子融合**（[#26779](https://github.com/ggml-org/llama.cpp/pull/26779)，已合入 b10427）：将 `mul_mat(gate) + mul_mat(up) + GLU` 融合为单个 q4_K reorder mat-vec。Arc Pro B70 实测（llama-bench tg128）：
  - Qwen2.5-3B-Instruct Q4_K_M：154.18 → 158.53 t/s（**+2.8%**）
  - Gemma-2-2b-it Q4_K_M：162.45 → 16x.xx t/s（数据截断，预计同量级提升）
  🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10427

- **SYCL：gated-delta-net 状态写回 cpy 融合**（[#26643](https://github.com/ggml-org/llama.cpp/pull/26643)，已合入 b10425）：移植自 CUDA 的 #23940，Qwen 3.6 27B Q4_K（48/64 块 gated_delta_net）在 Arc Pro B70 上 tg128 吞吐 23.91 t/s（A/B 对比数据未完整展示）。
  🔗 https://github.com/ggml-org/llama.cpp/releases/tag/b10425

- **SYCL：TILE kernel 用于量化 KV decode**（[#26689](https://github.com/ggml-org/llama.cpp/pull/26689)）：BMG 上 TILE 全面胜过 VEC，Qwen3.6-35B、Gemma 4 26B/12B 在 32K/118K 上下文下 **+42% ~ +169%**，无回归。

- **jinja `gather_string_parts` 二次方修复**（[#27034](https://github.com/ggml-org/llama.cpp/pull/27034)，合入 b10435）：作者在 issue #26974 中定位 `vector::erase` 循环与 `string::append` 返回值的双重二次方开销。

- **Arm 服务端 CPU 的 `-fa auto` 修正**（[#27092](https://github.com/ggml-org/llama.cpp/pull/27092)）：在同时具备 i8mm+SVE 的 Neoverse V1/V2（AWS Graviton3/4）上，将 `-fa auto` 解析为 off——tiled CPU flash-attention 在这些核心上显著慢于直接路径。

- **DSA RoPE 优化**（[#27091](https://github.com/ggml-org/llama.cpp/pull/27091)）：通过直接用 `n_rot` 替代 `ggml_concat` 优化 DSA 索引头，降低大张量 concat 开销。

- **RPC 后端事件与异步 API**（[#18626](https://github.com/ggml-org/llama.cpp/pull/18626)）：实现 backend 接口的 event/async API，为 RPC 后端流水线并行铺路，仍在开放中。

## 稳定性与回归

按严重度排序，均待修复（暂无对应 fix PR）：

**P0 级**
- **SYCL 在 Intel A770 上完全崩溃**（[#27063](https://github.com/ggml-org/llama.cpp/issues/27063)）：build 10428，任意模型在 A770 上 crash，但 B60 正常，指向 A770 专属回归，14 条评论。
- **SIGSEGV 空指针跳转（GPU offload）**（[#27046](https://github.com/ggml-org/llama.cpp/issues/27046)）：Intel Lunar Lake iGPU（Arc 140V）上 `resolve_fused_ops` 误判导致，已在 build 10423/10289/10050 复现，非单一版本回归。
- **ggml-rpc 未认证空指针解引用**（[#25299](https://github.com/ggml-org/llama.cpp/issues/25299)）：`graph_compute()` 中节点 id 0 解析为 nullptr 后直接使用，可被远程触发，属安全相关。

**P1 级**
- **Windows ROCm 7.14 发行包缺 hipblas.dll**（[#26996](https://github.com/ggml-org/llama.cpp/issues/26996)）：b10400 起 GPU 无法被识别，`--list-devices` 为空。Windows ROCm 用户建议暂缓升级或保留旧包。
- **ROCm gfx1151 RPC worker 在 DeepSeek V4 prefill 4096 token 后崩溃**（[#26746](https://github.com/ggml-org/llama.cpp/issues/26746)）：`GGML_OP_TOP_K` 处崩溃，影响 2× Ryzen AI Max+ 395 的 RPC 分布式部署。
- **SYCL 新 host-pinned 内存导致高 CPU 占用**（[#27038](https://github.com/ggml-org/llama.cpp/issues/27038)）：大内存分配场景下 CPU 利用率异常，与 "Support host pinned mem" 提交相关。

**P2 级**
- **DeepSeek-V4-Flash 长 agentic 对话中退化为重复并泄漏特殊 token**（[#26694](https://github.com/ggml-org/llama.cpp/issues/26694)）：Metal 后端，M3 Ultra 实测。
- **Qwen3-VL image embedding 不工作**（[#25088](https://github.com/ggml-org/llama.cpp/issues/25088)）：Vulkan 后端，已 stale 但仍开放。
- **Gemma 4 tg128 在 RTX 5060 Ti 上性能异常低**（[#26674](https://github.com/ggml-org/llama.cpp/issues/26674)）与 **Vulkan 整体性能下降**（[#24066](https://github.com/ggml-org/llama.cpp/issues/24066)）：均为性能回归类问题，社区在跟。
- **Gemma 4 12B `Gemma4Assistant` context 初始化失败**（[#24343](https://github.com/ggml-org/llama.cpp/issues/24343)）：llama-server 直接报错退出。
- **Qwen3.6-27B 在 CUDA 上导致 llama-server 崩溃**（[#23210](https://github.com/ggml-org/llama.cpp/issues/23210)）与 **Qwen3.6-27B 强制全量 prompt 重处理**（[#22746](https://github.com/ggml-org/llama.cpp/issues/22746)，已关闭但 126 条评论）：Qwen3.6 系列在缓存/崩溃两个方向都有问题。

**已关闭（stale 自动关闭，供参考）**
- SYCL MTP 在 Intel Arc 无加速（[#23533](https://github.com/ggml-org/llama.cpp/issues/23533)）
- MiniMax-Text-01 feature request（[#11290](https://github.com/ggml-org/llama.cpp/issues/11290)，已在 #27018 中实现）
- 路由器模式子进程 GPU 占用（[#19379](https://github.com/ggml-org/llama.cpp/issues/19379)）

## 对应用开发者的意义

1. **模板可获得 `reasoning_effort`**（b10434）：基于 OpenAI 兼容接口开发 Agent 应用时，`reasoning_effort` 会传入 jinja 模板。模板作者可以据此切换 prompt 策略（如 low effort 时省略推理引导），需注意与旧模板的兼容性——未使用该字段的模板不受影响。

2. **decode 期间可观测性提升**（b10429）：`/metrics` 和 `/slots` 在 `llama_decode()` 期间可访问，意味着负载高峰期监控不再被阻塞，对自建推理网关的排障和容量管理是实际改进。

3. **长会话 checkpoint 修复在途**：两个 PR 同时处理混合/循环模型的 checkpoint 保存/恢复（[#25592](https://github.com/ggml-org/llama.cpp/pull/25592) 和 [#26004](https://github.com/ggml-org/llama.cpp/pull/26004)）。当前 `slot save → restore` 对 SWA 和 Qwen3-Next 等模型会触发全量 re-prefill，构建持久化会话服务的团队应关注合入进展。

4. **MiniMax 与 Kimi-K3 支持将至**：若应用层有 MiniMax-Text-01/M1 或 Kimi-K3 部署需求，可追踪 [#27018](https://github.com/ggml-org/llama.cpp/pull/27018) 和 [#26185](https://github.com/ggml-org/llama.cpp/pull/26185) 的 review 状态。

5. **发布节奏与稳定性权衡**：24 小时 10 个版本，功能推进极快，但 P0 级问题集中在 SYCL（A770 崩溃、Lunar Lake SIGSEGV）和 RPC 安全（#25299）。生产环境若使用这些后端，建议暂缓升级至 b10425+，等待修复确认；CUDA 用户也需注意 Qwen3.6-27B 相关崩溃（#23210）是否影响自身负载。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 · 2026-08-15

## 今日速览

今日 Ollama 连续发布 v0.32.11/v0.32.12/v0.32.13 三个小版本，核心是新增 **Qwen 3.8 27B** 模型支持（含 Apple Silicon 优化）与 developer instructions 处理，同时 `ollama launch` 扩展了 DeepSeek Harness、Muse Code 两个 Agent 工具链。社区侧，Qwen 3.8 的 API 兼容性问题（system message 位置、developer role）已由 PR #17757/#17749 修复，但 CUDA 与 AMD Vulkan 的若干回归仍待解决。

---

## 版本发布与破坏性变更

- **v0.32.13**：为 qwen3.8 增加 developer instructions 支持，修复相关模板校验问题。  
  [Release v0.32.13](https://github.com/ollama/ollama/releases/tag/v0.32.13)
- **v0.32.12**：正式支持 **Qwen 3.8 27B**，并对 Apple Silicon 设备做了内存与推理优化。  
  [Release v0.32.12](https://github.com/ollama/ollama/releases/tag/v0.32.12)
- **v0.32.11**：`ollama launch dsh` 接入 DeepSeek Harness；`ollama launch muse` 接入 Meta Muse Code；同时改进了 OpenAI 兼容响应处理。  
  [Release v0.32.11](https://github.com/ollama/ollama/releases/tag/v0.32.11)

暂无明确破坏性变更，但 Qwen 3.8 新增的 chat template 语义可能影响旧请求格式，建议升级后回归测试。

---

## 新模型与硬件支持

- **Qwen 3.8 27B**：v0.32.12 正式支持，新增 safetensors 导入、Qwen3.8 renderer 以及 MLX 后端支持（PR #17745）。  
  [PR #17745](https://github.com/ollama/ollama/pull/17745)
- **Agent 工具链扩展**：`ollama launch` 新支持 DeepSeek Harness 与 Muse Code，底层会进行模型适配与客户端联通。  
  [Release v0.32.11](https://github.com/ollama/ollama/releases/tag/v0.32.11)
- **分片 GGUF 拉取（进行中）**：PR #17743 计划支持从 Hugging Face 拉取多文件分片 GGUF 模型，解决 #5245。  
  [PR #17743](https://github.com/ollama/ollama/pull/17743) · [Issue #5245](https://github.com/ollama/ollama/issues/5245)

---

## 性能与优化

- **模型元数据缓存（PR #17752）**：避免每次 chat/generate 请求重复读取 GGUF 元数据，单次推理开销约减少 **300ms**，并通过 manifest 变更自动失效缓存。  
  [PR #17752](https://github.com/ollama/ollama/pull/17752)
- **MLX 模型移植工作流（PR #15530）**：建立可重复的 MLX 模型 porting 流程，为后续新模型支持提供工程基础，目前仍为 draft。  
  [PR #15530](https://github.com/ollama/ollama/pull/15530)

---

## 稳定性与回归

按严重程度排列：

1. **Ollama Cloud API 503 宕机**：api.ollama.cloud 自 8 月 14 日起对所有 API 请求返回 503，影响所有 key，目前无修复 PR。  
   [Issue #17756](https://github.com/ollama/ollama/issues/17756)
2. **CUDA 非法内存访问（qwen3.6:35b）**：prompt 超过约 684 tokens 后 prefill 阶段确定性崩溃，0.31.2–0.32.9 回归，无 fix PR。  
   [Issue #17740](https://github.com/ollama/ollama/issues/17740)
3. **AMD Radeon 780M Vulkan 回归**：0.32.11 起在 Vulkan 后端出现 `DeviceLost`，大模型无法加载，建议暂缓升级至 0.32.11。  
   [Issue #17748](https://github.com/ollama/ollama/issues/17748)
4. **AMD Strix Halo VRAM 检测回归（容器部署）**：0.30+ 只显示 2GB 可用 VRAM，老 issue 持续有用户反馈，暂无修复。  
   [Issue #16462](https://github.com/ollama/ollama/issues/16462)
5. **qwen3.8:27b 500 “system message must be at the beginning”**：Claude Code 等客户端插入非首条 system 消息时触发，**已有 PR #17757 修复**（容忍非 leading system 消息）。  
   [Issue #17754](https://github.com/ollama/ollama/issues/17754) · [PR #17757](https://github.com/ollama/ollama/pull/17757)
6. **qwen3.8:27b-mlx 拒绝 developer role**：破坏 `ollama launch codex`，由同一 PR #17757 覆盖。  
   [Issue #17750](https://github.com/ollama/ollama/issues/17750) · [PR #17757](https://github.com/ollama/ollama/pull/17757)
7. **SillyTavern 空响应**：0.32.7+ 出现，回退到 0.32.7 可恢复，等待更多信息。  
   [Issue #17700](https://github.com/ollama/ollama/issues/17700)
8. **Nemotron3.5-lightning 中途停止**：AMD AI395+ 上生成/思考阶段卡住，需 CTRL+C 中断，尚无根因。  
   [Issue #17692](https://github.com/ollama/ollama/issues/17692)

---

## 对应用开发者的意义

- **Qwen 3.8 集成需谨慎**：虽已支持 developer instructions，但非首条 system message 仍会触发 500；建议等待包含 PR #17757 的版本，或自行将 system 消息前移。  
  [PR #17757](https://github.com/ollama/ollama/pull/17757)
- **Agent 工具链更丰富**：`ollama launch dsh`/`muse` 让本地模型可直接接入 DeepSeek Harness、Muse Code，适合快速验证 Agent 工作流。  
  [Release v0.32.11](https://github.com/ollama/ollama/releases/tag/v0.32.11)
- **值得关注的 API 增强**：PR #17747 为 `/api/embed` 增加 `normalize` 选项；PR #17739 提供全局 `OLLAMA_NO_THINK` 开关，均会提升上层应用的灵活度。  
  [PR #17747](https://github.com/ollama/ollama/pull/17747) · [PR #17739](https://github.com/ollama/ollama/pull/17739)
- **性能提升可预期**：模型元数据缓存落地后，高频小请求的固定开销将显著降低，对实时交互类应用友好。  
  [PR #17752](https://github.com/ollama/ollama/pull/17752)
- **AMD 用户暂停升级**：Strix Halo、Radeon 780M 用户建议暂留 0.32.10 或更早版本，等待回归修复。  
  [Issue #16462](https://github.com/ollama/ollama/issues/16462) · [Issue #17748](https://github.com/ollama/ollama/issues/17748)

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 动态日报 — 2026-08-15

## 1. 今日速览

今日核心动态集中在**稳定性回归修复**：PR #36837 导致 Admin UI 全面 404，社区已提交 Revert（#36982）并开发了更精准的修复（#36976）；另有多个涉及安全与正确性的修复正在推进，包括 Langfuse 凭据泄露风险（#36862）、Anthropic brotli 压缩导致 passthrough 响应不可读（#36977）、Vertex AI count_tokens 返回 0（#36981）。无新版本 Release，但 PR 活跃度较高，大量修复集中在 auth、MCP、Auto Router 与 UI 层。

## 2. 版本发布与破坏性变更

- 过去 24 小时无新版本 Release。
- ⚠️ **注意**：PR #36837（team fallback 模型访问收紧）已被标记为导致 **Admin UI 所有请求 404** 的回归来源。若已部署包含该提交的版本（`ab2333b6c4`），建议关注 #36982 / #36976 的合入状态或临时回滚。
  - 链接: https://github.com/BerriAI/litellm/pull/36982 | https://github.com/BerriAI/litellm/pull/36976

## 3. 新模型与硬件支持

- **今日无新增模型/硬件支持落地**。社区侧有两项相关请求仍在开放中，可关注进展：
  - Azure Foundry 增加 Fireworks AI 模型支持（DeepSeek V3.2、gpt-oss-120b、Kimi K2.5、MiniMax M2.5）— #26618（已关闭，但未确认是否合入）
  - Ollama text-to-image 在 `image_generation` 中的 first-class 支持 — #28026
  - 链接: https://github.com/BerriAI/litellm/issues/26618 | https://github.com/BerriAI/litellm/issues/28026

## 4. 性能与优化

- 今日无专门的性能优化 PR（吞吐、延迟、显存等）或量化格式相关更新。
- 与性能间接相关：Auto Router shadow eval 新增反向方向（#36865）和多 key 范围（#36871），可视为对评估成本的优化（减少重复 job）。
  - 链接: https://github.com/BerriAI/litellm/pull/36865 | https://github.com/BerriAI/litellm/pull/36871

## 5. 稳定性与回归

按严重程度排列：

| 严重度 | 问题 | 状态 |
|---|---|---|
| 🔴 严重 | **Admin UI 全面 404 回归**：团队 fallback 逻辑收紧后，UI 会话 key（指向无行 team）被硬拒绝，所有仪表盘请求失败 | 已有 Revert PR #36982；另有更精准的修复 #36976（保留保留 UI session 豁免） |
| 🔴 严重 | **Langfuse 凭据泄露**：`update_trace_keys` 可将任意 request-metadata key 原样写入 trace，团队成员可借此拉取团队 Langfuse 凭据 | 已有 fix PR #36862（除非 operator 显式开启，否则 no-op） |
| 🟠 高 | **Anthropic passthrough 响应乱码**：Anthropic 默认启用 brotli 压缩，proxy 转发客户端 `Accept-Encoding` 后无法解码，且剥离 `Content-Encoding` 导致响应体不可读 | 已有 fix PR #36977（不再转发上游 Accept-Encoding） |
| 🟠 高 | **Vertex AI count_tokens 统计错误**：`litellm.acount_tokens()` 对 `vertex_ai/gemini-*` 返回 `total_tokens=0`，消息未转换为 Gemini contents | 已有 fix PR #36981 |
| 🟠 高 | **Windows 上 Prisma 查询引擎崩溃**（LiteLLM 1.82.x/1.83.0）：首次查询即崩溃，1.81.16 正常 | 开放中，暂无 fix PR — #25260 |
| 🟡 中 | **GPT-5.6 家族函数工具调用报错**：`reasoning_effort` 参数与 tools 同时使用时 gpt-5.6-sol/luna/terra 返回错误 | Issue 已关闭，修复已合入 — #33221 |
| 🟡 中 | **`store_prompts_in_spend_logs` 不生效**：v1.93.0 配置被加载但 SpendLogs.messages 仍为空（acompletion 与 aresponses 均受影响） | 开放中，暂无 fix PR — #34747 |
| 🟡 中 | **Tag 预算永不重置**：`ResetBudgetJob` 缺少 tag handler，tag 一次性封禁后永久阻塞 | 开放中，暂无 fix PR — #27481 |
| 🟡 中 | **/metrics 端点无法匿名访问**（1.84.0 回归）：反代后 Prometheus 抓取受挫 | 开放中，暂无 fix PR — #27926 |
| 🟡 中 | **MCP Server 管理在 Control Plane 失效**：路由被误分类为 LLM API 路由，所有变更操作 404 | 开放中，相关 fix PR：日志元数据清理 #36901、OAuth endpoint 保留 #36888 |

此外还有多项开放中的正确性 bug 值得关注：Anthropic 系统角色提升破坏 prompt-cache 前缀（#36559）、GoogleGenAI 同一函数多次调用生成重复 tool_call_id（#27078）、Responses API `incomplete` 事件未处理（#27186）、`/v1/responses` 跨 provider 消息 ID 冲突（#27333）等。

## 6. 对应用开发者的意义

- **如果使用 LiteLLM Admin UI**：请确认当前版本是否包含 #36837，若包含且遇到 404，请关注 #36982 / #36976 的修复进度，或考虑临时回滚。这意味着仪表盘可能完全不可用。
- **如果使用 Anthropic 模型且走 passthrough 模式**：客户端 `Accept-Encoding` 被上游转发导致的乱码问题已有修复（#36977），升级后即可正常解析响应体。
- **如果使用 Langfuse callback 并在团队中开放 `update_trace_keys`**：存在凭据泄露风险，建议立即限制该项配置，待 #36862 合入后升级。
- **如果使用 Vertex AI Gemini 的 token 统计**：当前 `count_tokens` 返回 0 会导致成本估算和限额控制失效，计划中修复详见 #36981。
- **如果使用 Windows 部署 LiteLLM Proxy**：1.82.x/1.83.0 存在 Prisma 崩溃问题，建议避免升级到这些版本，或关注 #25260 的修复进展。
- **如果使用 OpenAI GPT-5.6 系模型 + function tools**：此前 `reasoning_effort` 导致的报错已在最新版本修复（#33221），升级后即可恢复正常调用。
- **如果使用 MCP Server 管理**：Control Plane 的管理操作当前不可用，且 MCP 请求的日志元数据存在 header 泄露风险，修复 PR 已提交，建议跟踪合入。

---

*数据来源：github.com/BerriAI/litellm（Issues/PRs 更新于 2026-08-14 ~ 2026-08-15）*

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 动态日报 2026-08-15

## 今日速览
- 发布 **v0.1.800-beta**，正式支持 **Qwen3.8-27B / 2.4T** 本地运行，最低仅需 17GB RAM，并同步提供 NVFP4 量化版本，是近期最重磅的模型支持更新。
- 社区反馈继续集中于 **Unsloth Studio 的 AMD ROCm 支持、工具调用一致性与桌面端稳定性**，其中 ROCm SDPA 回退、macOS 启动失败等问题已有对应修复 PR 在进行中。

## 版本发布与破坏性变更
- [Release v0.1.800-beta](https://github.com/unslothai/unsloth/releases)：新增 **Qwen3.8-27B** 与 **Qwen3.8-2.4T** 本地推理支持，通过 Unsloth Dynamic GGUFs 可在 **17GB RAM** 下运行；同时支持对 Qwen3.8-27B 进行微调；已上传 **NVFP4** 量化权重。
  - 官方 Guide: https://unsloth.ai/docs/models/qwen3.8
  - 暂未发现破坏性 API/配置变更。

## 新模型与硬件支持
- **Qwen3.8-27B / 2.4T**：新增本地运行与微调支持，配套 Dynamic GGUF 和 NVFP4 量化格式（见版本发布）。
- **Ling 3.0 支持请求**：[Issue #8532](https://github.com/unslothai/unsloth/issues/8532) — 社区请求在 Unsloth Studio 中直接下载、加载和托管 Ling 3.0 模型。
- **AMD 相关支持进展**：多起 AMD 识别/显存报告仍在处理中，包括 RX 5700XT 不识别（[#8529](https://github.com/unslothai/unsloth/issues/8529)）、Strix Halo APU 显存限制（[#6834](https://github.com/unslothai/unsloth/issues/6834)）、Studio 报 AMD GPU 但后端 CPU-only（[#8473](https://github.com/unslothai/unsloth/issues/8473)）、Bazzite 安装到 CPU PyTorch（[#8731](https://github.com/unslothai/unsloth/issues/8731)）。

## 性能与优化
- **MTP 部分卸载性能修复**：[PR #8875](https://github.com/unslothai/unsloth/pull/8875) — 修复 Qwen3.8-27B-GGUF 在 UD-IQ2_M 及默认配置下仅约 3.5 token/s 的异常性能；嵌入式 MTP 头现在跟随主模型放置策略。
- **流式渲染合并**：[PR #8845](https://github.com/unslothai/unsloth/pull/8845) — 将快速流式输出在浏览器下一帧前合并、减少消息重建，解决 UI 落后于流的问题。
- **ROCm AOTriton 注意力门控修复**：[PR #8821](https://github.com/unslothai/unsloth/pull/8821) — 允许库用户在 torch 侧启用 AOTriton flash / mem-efficient SDPA 内核，避免回退到 MATH 导致的性能骤降与 OOM。
- **性能问题报告**：[Issue #8861](https://github.com/unslothai/unsloth/issues/8861) — Qwen3.8-27B-NVFP4 在 Windows / RTX 5090 上推理极慢，待查。

## 稳定性与回归
以下按严重程度排列，✅ 表示已有修复 PR：

- ✅ **ROCm AOTriton 门控关闭导致微调 OOM**：[Issue #8819](https://github.com/unslothai/unsloth/issues/8819) — SDPA 回退至 MATH，短上下文即 OOM；由 [PR #8821](https://github.com/unslothai/unsloth/pull/8821) 修复。
- ✅ **Transformers >=4.43 在 CPT/SFT 中 in-place loss 导致崩溃**：由 [PR #8869](https://github.com/unslothai/unsloth/pull/8869) 修复。
- ✅ **媒体生成与模型 teardown 竞态**：修复 [Issue #8309](https://github.com/unslothai/unsloth/issues/8309)，对应 [PR #8866](https://github.com/unslothai/unsloth/pull/8866)（此前 [#8839](https://github.com/unslothai/unsloth/pull/8839) 为重复 PR，已关闭）。
- ✅ **CUDA 13.2 下 IQ3_S/IQ3_XXS/IQ2_M 输出乱码**：[Issue #4849](https://github.com/unslothai/unsloth/issues/4849)（已关闭）— 官方方案：使用 CUDA 12.8/13.0 二进制，或使用编译于 CUDA 13.0 的 Unsloth Studio。
- ⚠️ **Qwen3.8-27B-NVFP4 在 RTX 5090 上推理极慢**：[Issue #8861](https://github.com/unslothai/unsloth/issues/8861) — 待查。
- ⚠️ **macOS M4 Studio：llama-server 启动失败 + 空闲 RAM 占用过高**：[Issue #8566](https://github.com/unslothai/unsloth/issues/8566)。
- ⚠️ **macOS 桌面应用二次启动报错**：[Issue #8610](https://github.com/unslothai/unsloth/issues/8610)。
- ⚠️ **Windows 安装被 2 小时超时终止，且无进度输出**：[Issue #8698](https://github.com/unslothai/unsloth/issues/8698)。
- ⚠️ **PDF 附件导致工具调用错误/生成中断**：[Issue #8858](https://github.com/unslothai/unsloth/issues/8858)。
- ⚠️ **`-H 0.0.0.0` 在 macOS 上暴露错误 IP（安全）**：[Issue #8868](https://github.com/unslothai/unsloth/issues/8868)。
- ⚠️ **MiniMax-H3 在 AMD + Linux 下无法运行**：[Issue #8814](https://github.com/unslothai/unsloth/issues/8814)。
- ⚠️ **V1 endpoint 访问问题与 MCP 集成损坏**：[Issue #8790](https://github.com/unslothai/unsloth/issues/8790)。
- ⚠️ **AMD RX 5700XT 在 Unsloth Desktop 中不被识别**：[Issue #8529](https://github.com/unslothai/unsloth/issues/8529)。
- ⚠️ **训练后无法直接保存 GGUF（要求先导出 16bit 大文件）**：[Issue #8717](https://github.com/unslothai/unsloth/issues/8717)。
- ✅ **Linux AppImage 缺库无法启动**：[Issue #8463](https://github.com/unslothai/unsloth/issues/8463)（已关闭）。
- ✅ **后端 tokenizer 实例化失败**：[Issue #4417](https://github.com/unslothai/unsloth/issues/4417)（已关闭）。

## 对应用开发者的意义
- **OpenAI 兼容 API 现在覆盖 MLX 模型**：[PR #8768](https://github.com/unslothai/unsloth/pull/8768)（修复 #8748）——通过 Unsloth Desktop 安装的 MLX 模型不再需要手动加载即可通过 `/v1/models` 与 chat-completions 访问，对构建本地 Agent 工具链更有价值。
- **工具调用推理保留**：[PR #8581](https://github.com/unslothai/unsloth/pull/8581) — 修复 GGUF 工具循环中 tool call 前的 reasoning 不传递给 tool result 后的问题，减少 MCP 搜索类工具的重复调用。
- **新功能：模型感知当前日期**：[PR #8879](https://github.com/unslothai/unsloth/pull/8879)（关闭 #8859）——增加 `include_current_date_in_pro...` 全局设置，解决 Deep Research 基于训练截止时间规划搜索、导致信息过期的问题。
- **注意**：PDF 附件目前可能触发工具调用异常（[#8858](https://github.com/unslothai/unsloth/issues/8858)），生产环境使用文档上传时建议先验证。
- **安全提醒**：macOS 上以 `-H 0.0.0.0` 启动时可能绑定错误 IP（[#8868](https://github.com/unslothai/unsloth/issues/8868)），涉及远程访问时请检查实际监听地址。
- **API 差异化需求**：社区请求为音频/图像/视频生成提供程序化 API（[#8752](https://github.com/unslothai/unsloth/issues/8752)）、实时 prompt 处理速度展示（[#8528](https://github.com/unslothai/unsloth/issues/8528)）以及上下文压缩/滚动窗口（[#7472](https://github.com/unslothai/unsloth/issues/7472)），预计会影响后续 Studio 服务端能力。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*