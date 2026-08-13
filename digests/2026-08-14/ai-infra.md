# AI 基础设施日报 2026-08-14

> 生成时间: 2026-08-13 23:34 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施生态横向对比分析报告（2026-08-14）

## 1. 生态全景

当前 AI 基础设施生态由 DeepSeek-V4 稀疏 MLA 与 Kimi-K3 新一代稀疏注意力架构主导，几乎所有推理引擎的优化火力都集中在端到端适配这两条技术路线上。vLLM、SGLang、llama.cpp 三大引擎同步推进 DSV4/Kimi-K3 支持，分别在不同硬件后端上取得进展。同时，推测解码（MTP/Draft Speculation）已从可选项演进为默认能力，但其调度策略在长上下文与动态 batch 场景下暴露出吞吐塌陷和崩溃风险，成为新的稳定性焦点。多节点分布式推理的可靠性问题（死锁、rank 发散）在本日动态中多次出现，说明该方向仍处于支撑早期采用者的阶段。网关层则聚焦权限一致性与计费精度，为 Agent 应用的治理需求做好承接。

## 2. 各项目活跃度对比

| 项目 | 今日 Release | 活跃 PR（摘要提及） | 活跃 Issue（摘要提及） | 版本线 |
|---|---|---|---|---|
| **vLLM** | 无 | ~8 | ~11 | v0.27.x（存在严重回归） |
| **SGLang** | 无 | ~11 | ~15 | v0.5.x（rolling） |
| **llama.cpp** | **8 个**（b10411–b10423） | ~21 | ~12 | 滚动发布（日更） |
| **Ollama** | 无 | ~17 | ~17 | 0.30+（PR 在途） |
| **LiteLLM** | v1.98.0-dev.2（dev 预发布） | 官方 **277** | 官方 **64** | v1.98 dev |
| **Unsloth** | **v0.1.702-beta**（桌面应用正式上线） | ~11 | ~18 | 独立桌面版+Python 包双轨 |

> 注：vLLM/SGLang/llama.cpp/Ollama/Unsloth 的 PR/Issue 数仅统计当日摘要中列出的条目，非全量数据；LiteLLM 为官方统计。

## 3. 模型支持竞速

**推理引擎第一梯队（vLLM / SGLang）在最新架构上深度绑定：**
- **DeepSeek-V4-Flash 稀疏 MLA 端到端修复**（vLLM #51538）：覆盖普通解码、MTP、DSpark 三种模式，在 8×RTX PRO 6000 Blackwell 上验证，是 DSV4 生态的关键节点。
- **Kimi-K3 FP8 MLA**（vLLM #51040）：解除 `num_heads % 16 == 0` 限制，TP8 下每 rank 12 heads 可走 FP8 asm 预填充路径。
- SGLang 侧同步推进 TRT-LLM DSv4 Attention for SM100/103（#30805）、Kimi K3 Day0 PR（#32541）及 DSpark 性能修复。

**本地运行时（llama.cpp）保持着最快的架构跟进速度：** Kimi-K3 文本模型 PR（#26185）、MiniMax-Text-01/M1 支持（#27018）、OpenVINO 后端新增 gpt-oss MoE 与 MXFP4 支持、Metal 后端新增 TQ2_0 三元量化（#26980），并实现 MTP 草稿模型类型自动检测（#26814/#27005），降低配置成本。

**网关与开发者工具侧重接入广度与计费覆盖：** LiteLLM 新增 Voyage 4 embedding 家族、Meta Muse Spark 1.2；Ollama 新增 DeepSeek Harness（`ollama launch dsh`）与 Muse Code 两个 first-party 集成。

**结论：** vLLM 在 DeepSeek-V4 生产级推理上领先半个身位；llama.cpp 在新架构的本地/边缘运行上覆盖最快；LiteLLM 胜在模型货币化的商业就绪度。

## 4. 性能优化前沿

**今日优化火力集中在四个方向：**

- **KV Cache 管理**：vLLM 为 DeepSeek-V4 C4A 层添加 IndexCache（#51209）；SGLang 针对 MiniMax-M3 decode 阶段引入 lightning indexer 的 IndexCache 复用（#34583），消除约 22% step 时间的稀疏块选择重复计算。缓存复用从 radix attention 向 indexer 语义演进。

- **推测解码调度**：vLLM 提出上下文长度感知的推测调度 RFC（#48627），将 `num_speculative_tokens` 从按 batch 调度扩展为按 `(batch, ctx)` 二维表；llama.cpp 启用 dflash/dspark 草稿模型的 GPU 端后端采样（#26958），降低回退延迟。

- **算子/内核级优化**：SGLang 新增 Radix-4 MoE top-k router kernel（#34490）；vLLM 消除 KV sharing prefill 元数据中两次 GPU→CPU 同步（#42850）；llama.cpp 在 ggml-cpu 向量化 FA V-cache F16→F32 转换（#26947），并为 Metal 优化 TQ2_0 mul_mv kernel。

- **多后端适配**：llama.cpp SYCL Host Pinned Memory 加速 Host→Device 传输（#26789）；Windows 混合 CPU 线程亲和性优化（#27033）；Ollama Windows-on-Arm CPU 算子的 `GGML_CPU_ARM_ARCH` 修复（#17654），预计显著提升 WoA 推理吞吐。

**一个隐忧：** 多节点场景的稳定性优化明显滞后于单节点性能。vLLM v0.27.0 多节点空闲卡死（#51921）、SGLang. 多节点 TP rank 发散死锁（#33289）均无修复 PR，分布式推理仍是最大的生产风险点。

## 5. 分层定位差异

| 项目 | 分层 | 核心定位 | 关键差异点 |
|---|---|---|---|
| **vLLM** | 生产级推理引擎 | 面向在线服务的多节点、多模态高吞吐推理 | 稳定性问题是当前短板（v0.27.x） |
| **SGLang** | 前沿推理引擎 | 快速适配新架构（DSV4/Kimi-K3），深度优化 DSpark | 与 DeepSeek 生态绑定最紧 |
| **llama.cpp** | 本地/边缘运行时 | 跨 CPU/Metal/Vulkan/SYCL 多后端轻量推理 | 日更节奏最快，量化支持（TQ2_0/MXFP4）最广 |
| **Ollama** | 本地开发者运行时 | 一键拉起模型+Launch 集成，降低 Agent 开发门槛 | 强在集成生态，弱在深度架构适配 |
| **LiteLLM** | LLM 网关 | 请求路由、权限治理、计费/成本核算 | 今日 64 个活跃 Issue 中大量为授权一致性 |
| **Unsloth** | 训练/微调+桌面推理 | 微调与部署一体化（Unsloth Desktop） | 首日桌面版暴露大量安装/硬件兼容问题 |

**一句话总结：** vLLM/SGLang 是生产环境的两大“引擎舱”，llama.cpp/Ollama 是本地开发的“驾驶舱”，LiteLLM 是连接两者的“调度塔”，Unsloth 则是从训练车间直达驾驶舱的“改装厂”。

## 6. 值得关注的趋势信号

1. **DeepSeek-V4 生态是当下事实上的行业标准。** vLLM、SGLang、llama.cpp 三线并进，但 DSV4 在 vLLM 侧还伴随严重回归（#51758 flash error，仅一天 17 条评论），Kimi-K3 也仍在 FP8/ROCm 适配的早期阶段。**建议：** 采用这两类模型的团队应锁定 nightly commit 而非正式 release，并做长时间多节点压测。

2. **推测解码的“默认开启”正在制造新的性能陷阱。** vLLM 的动态 MTP 在 batch 阈值切换时出现 ~14% 单流损失及灾难性吞吐下降（#49548）；SGLang DSpark 此前每个 decode step 有 21 次 `cudaStreamSynchronize`、累计 71.4ms 开销（#34782）。**建议：** 生产环境先压测再决定是否启用动态调度，必要时固定 token 数。

3. **多节点推理稳定性是当前最大的共性短板。** vLLM v0.27.0 引擎彻底卡死（#51921）、SGLang TP rank 死锁（#33289）、llama.cpp CUDA sm_120 Q8_0 越界崩溃（#24399）——均无修复。**建议：** 大型部署升级前必须做空闲-恢复-压力三步测试，关注各项目本周后续修复动向。

4. **Agent 工具调用从“能不能调”进入“可治理”阶段。** LiteLLM 围绕 access group/team 授权一致性密集修复（#36843/#36825/#36837）；llama.cpp 增强 `/v1/responses` JSON schema 兼容性（#26013）；Unsloth 桌面版为所有外部 Provider 开放工具调用，但出现工具调用污染对话历史的问题（#8734），落地时应用层需清洗消息。

5. **计费与成本治理正在成为网关层的核心战场。** LiteLLM 今日明确多个 cost-map 修正（Azure GPT-5.6、Vertex Claude regional 10% 上调、PTU 双重计费修复）。**建议：** 依赖 SpendLogs 做预算告警的团队升级后需对账，数字可能变化。

6. **本地化/桌面化是一条正在成型的新赛道。** Unsloth Desktop 正式上线，Ollama 持续扩展 `ollama launch` 集成矩阵。桌面版的首日问题（AMD GPU 识别、Windows 安装超时）也证明这一赛道仍处于早期，但方向已清晰：训练、微调、推理、部署一体化将向个人开发者下沉。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 2026-08-14

## 今日速览

DeepSeek-V4 生态是当前最热主线：修复稀疏 MLA 的端到端 PR 已就绪，IndexCache 与 ROCm 优化持续推进。Kimi-K3 在 ROCm 后端的 FP8 预填充扩展也有重要 PR。稳定性方面，多个高热度 Issue 集中在 MTP/推测解码的崩溃与性能回退（#40756、#49548），以及 v0.27.0 多节点部署的引擎完全卡死（#51921），需要社区重点关注。

## 新模型与硬件支持

- **DeepSeek-V4-Flash 稀疏 MLA 端到端修复** — PR #51538 修复了七个阻碍 DSV4 在 SM120 稀疏 MLA 后端运行的缺陷，覆盖普通解码、MTP、DSpark 三种模式，已在 8×RTX PRO 6000 Blackwell 上验证。
  https://github.com/vllm-project/vllm/pull/51538

- **ROCm：Kimi-K3 FP8 MLA 预填充支持非 divisor 头数** — PR #51040 解除了 `num_heads % 16 == 0` 的限制，使 Kimi-K3 在 TP8 下（每 rank 12 heads）可走 FP8 asm 预填充路径，避免回退到 BF16 解压缩。
  https://github.com/vllm-project/vllm/pull/51040

- **Cosmos3-Edge 视觉编码器 CUDA Graph 支持** — PR #52229 为 SigLIP2 编码器添加精确预算的 CUDA Graph，解决 eager 模式 281 个 kernel 的 launch-bound 问题。
  https://github.com/vllm-project/vllm/pull/52229

- **Gemma4-E2B 异构逐层配置适配** — PR #52206 修复 Transformers 5.x 逐层 `head_dim` 导致模型加载失败的问题。
  https://github.com/vllm-project/vllm/pull/52206

## 性能与优化

- **DeepSeek-V4 IndexCache** — PR #51209 为 C4A 层添加 IndexCache 支持，在 DSpark 之上进一步提升 V4-Flash 服务性能。
  https://github.com/vllm-project/vllm/pull/51209

- **消除 KV sharing prefill 元数据中的两次 GPU→CPU 同步** — PR #42850 去掉了 `make_kv_sharing_fast_prefill_common_attn_metadata` 中的 `.item()` 调用，减少 Host 等待。
  https://github.com/vllm-project/vllm/pull/42850

- **上下文长度感知的推测调度 RFC** — Issue #48627 提议将 `num_speculative_tokens_per_batch_size` 从仅按 batch 调度扩展为按 `(batch, ctx)` 二维表，以解决长上下文下 K 值选择不优的问题。
  https://github.com/vllm-project/vllm/issues/48627

- **Qwen3-VL 类多模态模型的 ViT 全 CUDA Graph 支持** — Issue #38175 作为 tracker 跟踪生产场景中 ViT encoder 大量 kernel 启动开销的优化需求。（更新中，27 条评论）
  https://github.com/vllm-project/vllm/issues/38175

## 稳定性与回归

- **[严重] v0.27.0 引擎空闲后完全卡死** — Issue #51921：4 节点 TP=4（GB10/aarch64）下，引擎空闲约 1 分钟后永久 stall，请求无法进入 scheduler，`shm_broadcast` writer 饿死。未标 fix PR。
  https://github.com/vllm-project/vllm/issues/51921

- **[严重] Decode Context Parallelism 输出漂移/乱码** — Issue #41623：v0.21.0 起 `--decode-context-parallel-size` 产生错误输出，最新 nightly 仍复现。
  https://github.com/vllm-project/vllm/issues/41623

- **[严重] 升级 v0.26.0→v0.27.0 后 DeepSeek V4 运行报 flash error** — Issue #51758：仅一天前创建，已有 17 条评论。
  https://github.com/vllm-project/vllm/issues/51758

- **[高] MTP 推测解码 CUDA illegal memory access（Qwen3.6-27B-FP8）** — Issue #40756：长序列上使用 `num_spec_tokens=5` 崩溃，36 条评论。
  https://github.com/vllm-project/vllm/issues/40756

- **[高] 动态推测解码在 batch 阈值处聚合吞吐塌陷** — Issue #49548：`FULL_AND_PIECEWISE → PIECEWISE` 切换导致 ~14% 单流损失，且在并发下出现灾难性吞吐下降。
  https://github.com/vllm-project/vllm/issues/49548

- **[中] 流水线并行下 MTP 推测解码强制要求 `SupportsPP`** — Issue #52071 / PR #52117：目标模型 PP 配置被复制到 draft model，导致所有 MTP 配置失败。PR 已提交修复。
  https://github.com/vllm-project/vllm/issues/52071
  https://github.com/vllm-project/vllm/pull/52117

- **[中] GPT-OSS Harmony tool-calling 失败** — Issue #23567（47 条评论，gpt-oss-120b 多轮对话）与 PR #51020（严格 tool-call grammar 与真实 Harmony 渲染不匹配）同时活跃。
  https://github.com/vllm-project/vllm/issues/23567
  https://github.com/vllm-project/vllm/pull/51020

- **[中] vLLM 镜像启动 Gemma4 失败（Transformers 5.15.0）** — Issue #51744：QAT-NVFP4 模型在 `vllm-openai:latest` 中无法加载。
  https://github.com/vllm-project/vllm/issues/51744

- **[中] Intel Arc B60 GPTQ 全部报 `UR_RESULT_ERROR_DEVICE_LOST`** — Issue #52203：XPU 后端所有 GPTQ checkpoint 在 profile_run 阶段失败。
  https://github.com/vllm-project/vllm/issues/52203

## 对应用开发者的意义

- **谨慎升级到 v0.27.x**：多节点部署（#51921）和 DeepSeek-V4（#51758）均有严重回归报告，升级前需要针对部署拓扑做长时间空闲和加载测试。
- **DeepSeek-V4 / Kimi-K3 集成仍处于快速演进期**：如果你是这两类模型的早期采用者，建议持续跟进 #51538、#51040、#41820、#50682，当前最佳实践是锁定 nightly commit 而非正式 release。
- **推测解码已是默认选项，但有性能陷阱**：动态 MTP 的 batch 阈值切换会导致吞吐塌陷（#49548）。生产环境建议先做压力测试，必要时固定 `num_speculative_tokens` 或关闭动态调度。
- **工具调用（GPT-OSS/Harmony）用户**：严格模式下的 parser 仍在修正中，遇到 Harmony 解析错误时可在服务端临时设置 `VLLM_ENFORCE_STRICT_TOOL_CALLING=False` 绕过。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 — 2026-08-14

## 今日速览

过去 24 小时无新版本发布，但 DeepSeek-V4 生态成为绝对焦点：`#34788` 分离 SWA 与 compressed RoPE、TRT-LLM DSv4 Attention for SM100/103（`#30805`）持续推进，DSpark 侧连发两个性能修复 PR（`#34759`、`#34782`）。稳定性方面，多节点 TP rank 发散死锁（`#33289`）与 VLM embedding 请求崩溃（`#34769`，已有修复 PR）值得重点关注。

## 版本发布与破坏性变更

- **无新 Release**。
- **Docker 镜像更新**：`#34770` 将 MI300X 镜像 tag 更新至 `v0.5.17-rocm700-mi30x-20260813`（已合并，纯文档变更）。
- **配置/行为变更**：PR `#34788` 将 DeepSeek-V4 的 SWA RoPE 与 C4/C128 compressed YaRN-RoPE 拆分为独立表，并为 NPU 设置独立的 rotary cache owner——运行 DSV4 的自建部署需关注该行为变化。

## 新模型与硬件支持

- **Kimi K3**：Roadmap `#32607` 更新，Day0 PR `#32541` 已落地，包含 Cookbook、LMSYS Day0 博客与 DSpark 权重（16👍）。
- **DeepSeek-V4**：Roadmap `#23602` 中 W4A16 on Hopper 已完成；TRT-LLM DSv4 Attention for SM100/103 集成 PR `#30805` 仍在推进，标记为 `high priority + release-highlight`。
- **AMD**：`#34490` 新增 Radix-4 MoE top-k router kernel，面向 Kimi-K3 路由；`#34583` 为 MiniMax-M3 decode 阶段引入 lightning indexer 的 IndexCache 复用。
- **Apple Silicon / DCP**：`#34780` 修复统一 radix cache 中 Mamba checkpoint 深度校验，确保 `H(S)` 仅绑定在累积深度 S 的节点上。
- **ROCm**：`#34770` 更新 MI300X Docker 镜像至 ROCm 700 系列。

## 性能与优化

- **DSpark 性能修复**：
  - `#34782`（已合并）：将 draft `num_token_non_padded` 的 H2D 拷贝改为非阻塞。此前每个 decode step 出现 21 次 `cudaStreamSynchronize`，累计 71.4 ms（约每 step 3.5 ms，占 8.4 ms step 的 42%）。
  - `#34759`：修复 EP1 decode 性能回退——`DraftBlockProposer` 在 `enable_num_token_non_padded()` 为 false 时仍构建无用的 CUDA int32 scalar。
- **DSV4 paged_mqa_metadata**（`#25855`）：原 kernel Phase 3 由单 warp lane 0 串行推进 prefix 数组，导致 132/133 SMs 空转；针对 `bs≥1024` 场景改为并行化遍历。
- **MiniMax-M3**（`#34583`）：decode 阶段 lightning indexer + sparse-attn bucket 约 3.5 ms / 16 ms step（22%），IndexCache 复用稀疏块选择结果以消除重复计算。
- **DSV4 nonpaged indexer**（`#33857`，已合并）：当行候选数不超过 `index_topk` 时，跳过无意义的 DeepGEMM logits 计算。
- **Flux2**（`#34785`）：以 fused `SiluAndMul` 替代分离的 silu + mul kernel launch，消除中间显存往返。
- **CI 减负**（`#34309`）：将 7 项源代码 AST 检查移出 CPU 测试套件、精简 logprob 笛卡尔积测试，并以单 workspace 运行 `cargo test`。

## 稳定性与回归

**高严重度**

- **多节点 TP rank 发散死锁**（`#33289`）：DeepSeek-V4 + DSpark 在 2×DGX Spark（TP=2）下，一个 rank 卡在 NCCL logits all-gather，另一个 rank 在 request broadcast 空转，运行数分钟至数小时后死锁。无修复 PR。
- **VLM embedding 请求必崩**：`#34769` 指出 `EmbeddingReqInput` 缺少 `mm_content_hashes` 属性，导致所有 multimodal embedding 请求抛 AttributeError。已有修复 PR。
- **DSV4 SWA/compressed RoPE 耦合**：`#34788` 修复 SWA RoPE 被错误缩放、compressed path 使用错误 RoPE 表的问题，已附带 CPU 回归测试。
- **DSpark CUDA Graph 隐式请求槽位几何不匹配**（`#34384`）：compact ragged CUDA Graph 对同一 token tier 使用了不兼容的 request-slot geometry，暂无修复。

**中严重度**

- **ROCm MI355 HiCache 失效**（`#34611`）：真实 Agentic 负载下性能严重劣化，暂无修复。
- **Diffusion native-fallback 丢失 CPU offload 决策**（`#34772`）：当组件自定义加载路径失败回退到 native 路径时，`--text-encoder-cpu-offload` 等显式/自动 offload 决策被静默丢弃，8GB 显存 GPU 直接 OOM。
- **sgl-model-gateway 拒绝 `custom` tool 类型**（`#30781`）：Rust router v0.3.2 与 Python `protocol.py` 不同步，导致 OpenAI Codex CLI 等客户端 `/v1/responses` 请求被拒。
- **Paged KV allocator 先启 kernel 后查 OOM**（`#34399`）：OOM 检查晚于 allocation kernel 启动，存在潜在非法内存访问风险。
- **Prefill 侧 fd 耗尽**（`#31766`）：长时间运行后文件描述符耗尽，无修复。

**低严重度 / 已关闭**

- `#34787`（已关闭）：hybrid-mamba + NEXTN 投机解码下 `mamba_next_track_idx is None` 导致的 TypeError。
- `#9867`（已关闭）：DeepGEMM v2 长 warmup 导致 NCCL timeout（标记 inactive）。
- `#27987`（已关闭）：GLM-5.1 NVFP4 trtllm MoE 的 DP-attention illegal memory access。
- `#23579`（已关闭）：Session reaper 竞态 + `/v1/completions` 参数丢弃。
- 其余：`#31310` fa3 backend 在 H20 page-size 64 下性能慢；`#30595` 特定 MoE shape（hidden 4096 / inter 2048）触发 fused_moe_triton 断言；`#31019` GPT-OSS 结构化输出格式错误。

## 对应用开发者的意义

- **Agentic/KV 复用路线明确**：`#27574`（Programmatic KV Cache）与 `#30928`（Position-Independent KV Cache Reuse）两份 RFC 均在本周更新——前者支持上层 orchestrator 预判 KV 块价值，后者解决 RadixAttention 对字节及偏移高度敏感的问题。若你的 Agent 负载大量复用工具 schema / 检索文档，值得跟进这两个设计。
- **推理网关兼容性风险**：`#30781` 意味着基于 `sgl-model-gateway` 的部署会拒绝带 `custom` tool 的 `/v1/responses` 请求——这对 Codex CLI 等非标准客户端是硬阻塞，升级前需确认路由版本与 `protocol.py` 对齐。
- **reasoning_tokens 计数语义**：`#34634` 修复了未开启 thinking block 时仍累计 `reasoning_tokens` 的问题——若你的计费/用量逻辑依赖该字段，建议关注合入版本。
- **DSpark / KV 优化落地中**：EP1 回归修复与 H2D 同步消除预计可显著改善单节点 decode 吞吐；但 `#33289` 的多节点死锁仍在，生产环境多节点 DSV4 + DSpark 需谨慎。
- **单元测试基建补强**：`#20865`（82 comments）持续跟踪核心模块单测覆盖率提升，可作为项目质量风向标。

> 数据来源：GitHub `sgl-project/sglang` Issues/PRs（更新截止 2026-08-13 22:51 UTC）。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报 2026-08-14

## 今日速览

今日发布节奏密集，共 8 个新 Release（b10411–b10423），重点在 OpenVINO 后端支持 Qwen3.5 与内存优化、Metal 后端新增 TQ2_0 量化支持、以及跨工具 CPU 参数统一。社区活跃度集中在 Vulkan/SYCL 后端正确性回归与 MoE 模型推理稳定性上，多个长期 issue 出现新进展，服务器端并发访问 /metrics 与 /slots 的修复 PR 已提交。

## 版本发布与破坏性变更

- **b10423**: [common: apply CPU parameters across tools](https://github.com/ggml-org/llama.cpp/releases/tag/b10423) (#27026)。将 CPU 参数（如线程数、亲和性设置）统一应用到所有工具。使用自定义 CPU 参数配置的开发者需验证各 CLI 工具行为是否一致。
- **b10419**: [OpenVINO: Qwen3.5, memory optimization, and test-recurrent-state-rollback](https://github.com/ggml-org/llama.cpp/releases/tag/b10419) (#26952)。OpenVINO 后端新增 gpt-oss MoE 与 mxfp4 支持，并包含 FILL 算子支持及内存优化。使用 OpenVINO 后端的用户建议关注内存占用变化。
- **b10416**: [server: serve index.html with no-cache](https://github.com/ggml-org/llama.cpp/releases/tag/b10416) (#27006)。修复 index.html 被浏览器长期缓存导致 WebUI 固定旧版本的问题，改为 ETag 重新验证。前端用户需留意首次加载行为变化。
- **b10413**: [common: auto-detect spec type from draft GGUF metadata](https://github.com/ggml-org/llama.cpp/releases/tag/b10413) (#26814)。本地草稿模型加载时不再依赖 `--spec-type` 参数，通过 GGUF 元数据自动检测。使用 `-md` 加载本地草稿模型时配置可简化。
- 完整 release 列表见 [Releases 页面](https://github.com/ggml-org/llama.cpp/releases)。

## 新模型与硬件支持

- **OpenVINO 后端支持 Qwen3.5 与 MXFP4 格式**：通过 [b10419 发布说明](https://github.com/ggml-org/llama.cpp/releases/tag/b10419) 的 OpenVINO 集成，gpt-oss MoE 架构也可在 OpenVINO 上运行。
- **Metal 后端新增 TQ2_0 量化支持**：[b10414 的 PR #26980](https://github.com/ggml-org/llama.cpp/pull/26980) 为 Metal 添加了三元量化（2 bits/element）支持，并优化了 mul_mv kernel（浮点运算替代整数运算、预计算）。
- **MTP 草稿模型类型自动检测**：[PR #27005](https://github.com/ggml-org/llama.cpp/pull/27005)（b10415）与 [PR #26814](https://github.com/ggml-org/llama.cpp/pull/26814)（b10413）分别覆盖远程与本地草稿模型，自动识别 spec/MTP 类型，减少手动配置。
- **新模型架构 PR 推进中**：
  - [Kimi-K3 文本模型支持](https://github.com/ggml-org/llama.cpp/pull/26185)（含跨层残差注意力、latent MoE、situ 激活）。
  - [MiniMax-Text-01 / MiniMax-M1 支持](https://github.com/ggml-org/llama.cpp/pull/27018)（lightning attention 架构）。
- **LFM2 / LFM2MOE 张量并行支持**：[PR #26993](https://github.com/ggml-org/llama.cpp/pull/26993) 新增 `--split-mode tensor` 支持。

## 性能与优化

- **Flash-attention V-cache F16→F32 转换向量化**：[PR #26947](https://github.com/ggml-org/llama.cpp/pull/26947)（b10411）在 ggml-cpu 中将 V-cache 类型转换向量化，预期提升 CPU 后端 prefill 吞吐。
- **SYCL Host Pinned Memory**：[PR #26789](https://github.com/ggml-org/llama.cpp/pull/26789)（b10418）通过主机固定内存加速 SYCL 后端 Host→Device 传输，并修复线程安全问题。
- **OpenVINO 内存优化**：[b10419](https://github.com/ggml-org/llama.cpp/releases/tag/b10419) 包含 set_rows 等算子优化。OpenVINO 用户可对比峰值显存占用。
- **Windows 混合 CPU 线程调度优化**：[PR #27033](https://github.com/ggml-org/llama.cpp/pull/27033) 在 Windows 上过滤 E-core，优化混合架构（Alder/Raptor/Arrow Lake 等）线程亲和性。
- **Jinja 模板解析二次复杂度修复**：[PR #27034](https://github.com/ggml-org/llama.cpp/pull/27034) 修复 `gather_string_parts` 中 `vector::erase` 与 `string::append` 的二次开销，长模板渲染可受益。
- **后端采样（backend sampling）**：[PR #26958](https://github.com/ggml-org/llama.cpp/pull/26958)（b10412）启用 dflash 与 dspark 草稿模型的 GPU 端采样，并支持 `p_min > 0`，降低 speculative decoding 回退延迟。

## 稳定性与回归

以下为今日更新的高严重度问题，按影响程度排序：

| 严重度 | 问题 | 状态 |
|---|---|---|
| 🔴 高 | [CUDA sm_120（Blackwell）Q8_0 共享内存越界崩溃](https://github.com/ggml-org/llama.cpp/issues/24399)：RTX 5090 上 `mul_mat_q<Q8_0,128>` MMA 写回 epilogue 非法共享内存访问，间歇性崩溃。移除 Q8_0 权重可规避，尚无 fix PR。 | 开放 |
| 🔴 高 | [Vulkan DeviceLost（Strix Halo / AMD APU）](https://github.com/ggml-org/llama.cpp/issues/25664)：DeepSeek-V4-Flash 多轮对话后设备丢失；另有 [APU 任务超时问题 #21724](https://github.com/ggml-org/llama.cpp/issues/21724)。无 fix PR。 | 开放 |
| 🟠 中 | [SYCL 第二轮 prompt 输出乱码（多架构）](https://github.com/ggml-org/llama.cpp/issues/26845)：Intel Arc Pro B60 复现；同类历史 issue #21589、#23797。无 fix PR。 | 开放 |
| 🟠 中 | [Vulkan MoE 批量解码吞吐量悬崖](https://github.com/ggml-org/llama.cpp/issues/25356)：Strix Halo iGPU 上 512-expert MoE 在 n_tokens=9 时吞吐从 122.5 t/s 骤降至 82.9 t/s，与 MMV dispatch 的固定 8-token 阈值相关。 | 开放 |
| 🟠 中 | [SYCL Q8_0 预填充性能下降 42%](https://github.com/ggml-org/llama.cpp/issues/25203)：Arc B70 上 Q8_0 reorder 导致，llama-bench 无法复现。 | 开放 |
| 🟡 低 | [DeepSeek-V4-Flash 长 agentic 对话重复与特殊 token 泄漏](https://github.com/ggml-org/llama.cpp/issues/26694)（Metal，M3 Ultra） | 开放 |
| 🟡 低 | [DFlash drafter 绑定失败](https://github.com/ggml-org/llama.cpp/issues/26894)：目标 GGUF 包含数组格式 `attention.sliding_window_pattern` 时 `vector::_M_range_check` 崩溃。 | 开放 |
| 🟡 低 | [vision 模型 KV cache 保存无效](https://github.com/ggml-org/llama.cpp/issues/19466)（/slots/3?action=save） | 开放 |

**今日新出现的关闭/修复动向**：

- [PR #27044](https://github.com/ggml-org/llama.cpp/pull/27044)：修复 CUDA `mul_mat_q` ids 路径中尾部 padding 计算错误（`ne11` → `ne11_flat`），解决 MoE gate/up projection 的显存分配过小问题。已提交，可观察合入状态。
- [PR #27042](https://github.com/ggml-org/llama.cpp/pull/27042)：Hexagon 后端修复 FLASH_ATTN_EXT 非确定性结果（issue #26759）。
- [PR #26294](https://github.com/ggml-org/llama.cpp/pull/26294)：修复 CUDA `mul_mat_id` 中重复 expert id 压实时段错误（issue #24591）。
- [PR #26434](https://github.com/ggml-org/llama.cpp/pull/26434)：OpenCL flash-attention tile kernel 的 WAR race 竞态修复。
- [PR #26040](https://github.com/ggml-org/llama.cpp/pull/26040)：修复 ggml 后端拆分调度器并发复用内存导致的 race condition（Qwen `-nkvo` + Vulkan 拆分场景）。

## 对应用开发者的意义

1. **服务器并发可观测性提升**：[PR #27041](https://github.com/ggml-org/llama.cpp/pull/27041) 实现在 `llama_decode()` 执行期间仍可访问 `/metrics` 与 `/slots`，解决监控系统在推理高峰期被阻塞的问题，对生产环境的可观测性有直接帮助。
2. **WebUI 缓存修复**：[b10416](https://github.com/ggml-org/llama.cpp/releases/tag/b10416) 修复了 index.html 被强制缓存导致 UI 与后端版本不匹配的问题。多实例部署时建议确认浏览器端 ETag 行为正常。
3. **配置简化**：MTP/spec 草稿模型现在可在加载时自动检测类型（[b10413](https://github.com/ggml-org/llama.cpp/releases/tag/b10413)、[b10415](https://github.com/ggml-org/llama.cpp/releases/tag/b10415)），构建 Agent 时无需在多个环境中维护 `--spec-type` 参数。
4. **OpenAI Responses API 兼容性**：[PR #26013](https://github.com/ggml-org/llama.cpp/pull/26013) 持续增强 `/v1/responses` 的 JSON schema 与流式兼容性（需求源自 [#19138](https://github.com/ggml-org/llama.cpp/issues/19138)）。使用 Responses API 的 Agent 框架可留意进展。
5. **回归风险提示**：Vulkan 与 SYCL 后端在 MoE/长上下文场景仍存在崩溃与乱码问题（见稳定性部分），建议生产环境在更新前对目标模型做回归测试。CUDA Blackwell 用户应优先跟进 Q8_0 崩溃的修复合入情况。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 2026-08-14

## 1. 今日速览

- **MLX 结构化输出支持取得关键进展**：两个实现 PR（[#17690](https://github.com/ollama/ollama/pull/17690)、[#17697](https://github.com/ollama/ollama/pull/17697)）已提交，有望解决 MLX 模型长期存在的"静默忽略 JSON Schema"问题（[#16563](https://github.com/ollama/ollama/issues/16563)）。
- **AMD Strix Halo VRAM 检测回归已有修复方案**：0.30+ 容器部署仅识别 2GB 显存的问题（[#16462](https://github.com/ollama/ollama/issues/16462)），对应修复 PR 已提交（[#17685](https://github.com/ollama/ollama/pull/17685)）。
- **Launch 集成矩阵继续扩展**：新增 DeepSeek Harness（[#17733](https://github.com/ollama/ollama/pull/17733)）与 Muse Code（[#17594](https://github.com/ollama/ollama/pull/17594)）两个 first-party 集成。

## 2. 版本发布与破坏性变更

过去 24 小时无新版本发布。需注意以下变更：

- **Muse Glimmer 推理渲染模板变更**（[#17732](https://github.com/ollama/ollama/pull/17732)，已关闭）：Go renderer 与发布版 Jinja 模板对齐。显式 system prompt 中的 "Reasoning effort" 将被规范化为 "Reasoning strength"，且不再由 renderer 额外添加 reasoning 行。依赖该模型 system prompt 行为的应用需回归验证。
- **公开 API 模型列表不完整**（[#17725](https://github.com/ollama/ollama/issues/17725)，已关闭）：`https://ollama.com/v1/models` 未返回全部云模型。依赖该端点做模型枚举的客户端可能遗漏 `:cloud` 模型。

## 3. 新模型与硬件支持

- **DeepSeek Harness 集成**（[#17733](https://github.com/ollama/ollama/pull/17733)）：新增 `ollama launch dsh`，按需从 npm 安装 `@deepseek-ai/dsh`，支持本地与云模型。
- **Muse Code 集成**（[#17594](https://github.com/ollama/ollama/pull/17594)）：新增 `ollama launch muse`，通过独立 settings.json 管理模型目录。
- **Nemotron H MLX 视觉支持**（[#17714](https://github.com/ollama/ollama/pull/17714)，进行中）：实现 RADIO 视觉编码器与 projector，覆盖动态分辨率预处理、确定性 placeholder 展开、分块 feature scattering 与 MTP offsets。
- **AMD Strix Halo 大显存 iGPU 修复**（[#17685](https://github.com/ollama/ollama/pull/17685)，进行中）：修正 `hipMemGetInfo()` 返回系统空闲内存而非 GPU VRAM 的检测逻辑，同时引入 `OLLAMA_GPU_MEMORY` 环境变量作为显存覆盖手段。
- **长期功能请求：多文件 GGUF 导入**（[#5245](https://github.com/ollama/ollama/issues/5245)）：已有 157 👍 与 107 条评论，仍开放，关注大模型分片导入的开发者可跟进。
- **新模型请求：Qwen3.8 云模型**（[#17720](https://github.com/ollama/ollama/issues/17720)）：社区希望在 Pro/Max 订阅中提供 Qwen3.8-2.4T-A95B-FP8。

## 4. 性能与优化

- **Windows-on-Arm CPU 算子性能修复**（[#17654](https://github.com/ollama/ollama/pull/17654)，进行中）：CPU runner 目前使用 baseline `armv8-a` 编译，无任何 dot-product/matrix 指令。PR 通过设置 `GGML_CPU_ARM_ARCH` 一行修复，对 WoA 设备推理吞吐应有显著提升。
- **后端加载规划集中化**（[#17165](https://github.com/ollama/ollama/pull/17165)，进行中）：将 scheduler preflight、request option setup、runner startup 中分散的内存策略合并，消除负载路径上不同阶段估算不一致的问题，间接改善 iGPU/mmproj 场景的显存规划。

## 5. 稳定性与回归

按严重程度排列：

- **[高] AMD Strix Halo VRAM 仅识别 2GB**（[#16462](https://github.com/ollama/ollama/issues/16462)）：0.30+ 容器部署回归，已定位为 `hipMemGetInfo()` 行为变化。修复 PR [#17685](https://github.com/ollama/ollama/pull/17685) 已在评审中。
- **[高] llama3.3:70b 自 v0.32.2 起生成垃圾 token**（[#17379](https://github.com/ollama/ollama/issues/17379)）：DEV/PROD 均复现，用户尚未定位到具体提交。
- **[中] MLX 结构化输出被静默忽略**（[#16563](https://github.com/ollama/ollama/issues/16563)）：JSON Schema 约束在 MLX 模型上完全不生效。修复 PR [#17690](https://github.com/ollama/ollama/pull/17690)（grammar/JSON Schema sampling）与 [#17697](https://github.com/ollama/ollama/pull/17697)（XGrammar 约束解码）已提交，开发者应关注合入进度。
- **[中] Nemotron3.5-lightning 在 AMD AI395+ 上卡死**（[#17692](https://github.com/ollama/ollama/issues/17692)）：thinking 阶段 stall，TUI 下 CTRL+C 可中断，尚无可复现的稳定步骤。
- **[中] Claude Code + qwen3-coder 无响应**（[#17671](https://github.com/ollama/ollama/issues/17671)）：`ollama launch claude --model qwen3-coder:30b` 生成成功但 Claude Code 界面无输出，属于 Launch 集成缺陷。
- **[中] /api/chat 静默丢弃音频字段**（[#17730](https://github.com/ollama/ollama/issues/17730)）：对 `gemma4:e4b` 传 `audios`/`audio` 字段返回 HTTP 200，但模型实际未收到音频。应报错而非静默忽略。
- **[中] 0.30.0+ Docker 无法加载模型**（[#17285](https://github.com/ollama/ollama/issues/17285)，已关闭）：Ryzen 5750G Vega8 用户被迫停留在 0.24.0。
- **[中] Vulkan 后端在接近上下文极限时 CPU 100% 卡死**（[#13461](https://github.com/ollama/ollama/issues/13461)）：0.13.3 起出现，单核 spin loop 且内存不释放。
- **[低] Mac 更新流程问题**（[#11972](https://github.com/ollama/ollama/issues/11972)）：非管理员账户下 "Restart to update" 需要手动授权，流程不流畅。
- **在途修复 PR**：重复 token 检测器误杀（[#17360](https://github.com/ollama/ollama/pull/17360)）、gpt-oss 长上下文崩溃的 flash attention 显式请求（[#17477](https://github.com/ollama/ollama/pull/17477)）、MLX 无限生成挂起的 `num_predict` 边界修复（[#17494](https://github.com/ollama/ollama/pull/17494)）、Gemma4 "->" 标记与 thinking 中止修复（[#17570](https://github.com/ollama/ollama/pull/17570)）、GraphSize KV 显存预估修复（[#17615](https://github.com/ollama/ollama/pull/17615)）、云 API 流式空行响应修复（[#17207](https://github.com/ollama/ollama/pull/17207)）。
- **低优先级但值得关注**：`WriteWithBackup` 文件备份时间戳碰撞（[#17713](https://github.com/ollama/ollama/issues/17713)），一秒内两次写入会选中同一备份路径，可能导致写坏；Modelfile 解析器将 CRLF 计为两行的修复已提交（[#17734](https://github.com/ollama/ollama/pull/17734)）。

## 6. 对应用开发者的意义

- **MLX 结构化输出即将改变**：目前 MLX 模型静默忽略 `response_format`，Agent 应用拿到的输出可能不符合 JSON Schema。两个修复 PR（[#17690](https://github.com/ollama/ollama/pull/17690)、[#17697](https://github.com/ollama/ollama/pull/17697)）合入后行为将变更为强制约束解码。在合入前，请勿假设 MLX 后端支持结构化输出。
- **OpenAI 兼容端点将暴露上下文长度**：[#17422](https://github.com/ollama/ollama/pull/17422) 为 `/v1/models` 与 `/v1/models/{model}` 增加 `context_length` 字段，客户端可据此动态调整提示词长度与压缩策略。
- **Claude Code 集成仍需验证**：模型不在 Claude Code 已知列表时会回退 200k 自动压缩窗口（[#17717](https://github.com/ollama/ollama/issues/17717)）、`[1m]` 上下文后缀被拒绝（[#17584](https://github.com/ollama/ollama/issues/17584)）、部分模型无响应（[#17671](https://github.com/ollama/ollama/issues/17671)）。生产环境使用前建议逐模型验证。
- **音频多模态调用存在静默失败陷阱**：向音频模型传 `audios` 字段会返回 200 但音频被丢弃（[#17730](https://github.com/ollama/ollama/issues/17730)）。应用层需自行确认模型能力或等待服务端改为显式报错。
- **新 Launch 集成降低 Agent 开发门槛**：Muse Code（[#17594](https://github.com/ollama/ollama/pull/17594)）与 DeepSeek Harness（[#17733](https://github.com/ollama/ollama/pull/17733)）加入后，Agent 开发栈中可以统一通过 `ollama launch` 拉起多类前端工具。
- **Agent Host Protocol 支持被提出**：[#17729](https://github.com/ollama/ollama/issues/17729) 请求接入微软 agent-host-protocol，若实现将影响 Agent 编排方式，值得关注。
- **云模型订阅可用性仍然是社区痛点**：Kimi K3 上线两周仍未向 Pro/Max 订阅开放（[#17715](https://github.com/ollama/ollama/issues/17715)），依赖特定云模型的开发者需要为订阅计划做备选方案。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 动态日报 2026-08-14

> 数据来源：BerriAI/litellm GitHub（过去 24h：64 条活跃 Issue，277 条 PR）

## 今日速览

今日主版本动态是 dev 预发布 `v1.98.0-dev.2`，除 cosign 镜像签名说明外暂无 changelog。新模型侧新增 Voyage 4 家族与 Meta Muse Spark 1.2；PR 侧则集中在 access group/team/key 权限一致性、MCP OAuth 多 worker 可靠性，以及一批成本计费修正。

## 版本发布与破坏性变更

- **v1.98.0-dev.2**：目前可见变更只有 Docker 镜像签名说明——所有 LiteLLM 镜像都使用 cosign 签名，推荐在部署前验证镜像签名。未见明确的 API/配置破坏性变更。[Release v1.98.0-dev.2](https://github.com/BerriAI/litellm/releases/tag/v1.98.0-dev.2)
- **运维注意**：本轮多个 cost-map 修正（Azure GPT-5.6、Vertex Claude regional、PTU 计费）会影响 SpendLogs 金额，不是 API 破坏，但可能导致历史/当前成本报表数字变化。

## 新模型与硬件支持

- **Voyage 4 embedding 家族**：PR [#35091](https://github.com/BerriAI/litellm/pull/35091) 新增 `voyage/voyage-4` 与 `voyage/voyage-context-4` 到模型成本/上下文 map，并修复 contextual embedding 输入 `list[str]` 被 400 的问题。
- **Meta Muse Spark 1.2**：PR [#36717](https://github.com/BerriAI/litellm/pull/36717) 新增 `meta/muse-spark-1.2` 及 contributor 档位，并补上 `reasoning_effort`、web search grounding（$2.50/1k 次查询）的计费。
- **Bedrock GPT 5.5（Mantle）**：Issue [#30941](https://github.com/BerriAI/litellm/issues/30941) 关闭，对应 Chat Completions → Response API 自动转换能力已落地。
- 无 CUDA/ROCm/Metal/CPU/量化格式相关变更。

## 性能与优化

- 今日没有看到 Kernel/算子/显存/吞吐类优化。
- 与“延迟”相关的更多是流式正确性问题：Anthropic passthrough `/v1/messages` 在 pre-stream 处理阶段不向客户端发送任何字节，大 prompt 时 TTFT 过长导致客户端超时（[#32491](https://github.com/BerriAI/litellm/issues/32491)，open）。
- 修复 PR [#36761](https://github.com/BerriAI/litellm/pull/36761) 主要解决 Responses API 流式日志/成本追踪状态不同步，属于计费与日志修复，不是性能优化。

## 稳定性与回归

按严重程度排列：

### 高危 / 安全 / 计费

- **响应串话 / 数据泄漏（Critical）**：Redis Cluster + OpenShift 环境下响应偶尔返回给错误客户端。[#25447](https://github.com/BerriAI/litellm/issues/25447) 已关闭，但今日数据中未看到对应 fix PR，相关环境建议重新验证。
- **429 错误体泄漏完整 SHA-256 token hash**：限流返回体包含 64 字符虚拟 key 哈希，属敏感信息暴露。[#27884](https://github.com/BerriAI/litellm/issues/27884)（open，无 fix PR）
- **`end_user` 回归**：共享虚拟 key 上所有后续请求的 `end_user` 被钉在首个请求的 `user`，v1.87.0 回归。[#31441](https://github.com/BerriAI/litellm/issues/31441)（open，无 fix PR）
- **Azure GPT-5.6 terra/luna cost-map 错误**：使用了 OpenAI 降价后价格，未反映 Azure 发布价。[#36192](https://github.com/BerriAI/litellm/issues/36192)（open，无 fix PR）
- **Vertex Claude regional 少计费 10%**：修复 PR [#36833](https://github.com/BerriAI/litellm/pull/36833) 为 `vertex_location` 增加 1.1x regional uplift multiplier。
- **PTU deployment 双重计费**：PTU 部署在固定容量费用之外仍按 token 计费；修复 PR [#36829](https://github.com/BerriAI/litellm/pull/36829) 会在存储 PTU 配置时把所有 per-token 价格写 0。

### 功能正确性

- **Usage 仪表盘 “Ask AI” 对模型别名/model group 失效**：[#35461](https://github.com/BerriAI/litellm/issues/35461) 直接调用 `litellm.acompletion()` 绕过 Router，无法解析 model group；同源问题见 [#24513](https://github.com/BerriAI/litellm/issues/24513)。均 open，无 fix PR。
- **添加自定义 MCP Server 失败**：UI 中报 `Could not find...`。[#23869](https://github.com/BerriAI/litellm/issues/23869)（open，无 fix PR）
- **Xiaomi MiMo 与 Claude Code 兼容性**：`output_config` 参数导致 `AsyncCompletions.create()` 失败。[#24549](https://github.com/BerriAI/litellm/issues/24549)（open，无 fix PR）
- **OpenAPI→MCP 工具生成丢失 `$ref` body schema**：FastAPI/Pydantic 生成的 spec 转 MCP 后 `inputSchema` 为空。[#36765](https://github.com/BerriAI/litellm/issues/36765)（open，无 fix PR）
- **OTel exporter 中 `gen_ai.system` 仍为 None**：PR #26713 只修了 span attribute 入口，metrics/events 路径仍报错。[#36759](https://github.com/BerriAI/litellm/issues/36759)（open，无 fix PR）
- **Guardrails 数据缺失**：`litellm_content_filter` evaluations 未出现在 request logs / Guardrails Monitor。[#36566](https://github.com/BerriAI/litellm/issues/36566)（open，无 fix PR）
- **Azure Responses `additional_tools` namespace**：空 namespace 描述被原样转发。[#36366](https://github.com/BerriAI/litellm/issues/36366)（open，无 fix PR）
- **Anthropic 系统消息 hoist 破坏 prompt cache**：中间插入 system-role 后会导致整个 prompt-cache 前缀失效。[#36559](https://github.com/BerriAI/litellm/issues/36559)（open，无 fix PR）
- **用户 max budget 无法重置为 unlimited**：[#32474](https://github.com/BerriAI/litellm/issues/32474)（open，无 fix PR）
- **config.yaml 中的 vector store 不生效**：[#25947](https://github.com/BerriAI/litellm/issues/25947)（open，无 fix PR）

### 今日重点修复 PR（均仍为 open）

- **Access group / key / team 权限同步**：[#36843](https://github.com/BerriAI/litellm/pull/36843) 修复 key attach access group 后实际未授权；[#36825](https://github.com/BerriAI/litellm/pull/36825) 修复 team 写入路径同步 `assigned_team_ids`。
- **权限越权 / 未授权扩大**：[#36837](https://github.com/BerriAI/litellm/pull/36837) 阻止 team fallback 扩大模型访问范围；[#36826](https://github.com/BerriAI/litellm/pull/36826) 让 `lite login` token 携带 team grants；[#36819](https://github.com/BerriAI/litellm/pull/36819) 清理 team 删除后的 dangling references/cache。
- **Team 成员删除 cleanup 修复**：[#36839](https://github.com/BerriAI/litellm/pull/36839) 改为按 user id 清理，而不是按邮箱。
- **MCP OAuth 多 worker 可靠性**：[#36844](https://github.com/BerriAI/litellm/pull/36844) 将 pending OAuth server 改为 DB-backed draft，解决多 worker 随机 404；[#36831](https://github.com/BerriAI/litellm/pull/36831) 支持清空/重新授权 MCP server 已存 OAuth token。
- **成本/计费修复**：[#36833](https://github.com/BerriAI/litellm/pull/36833) Vertex Claude regional 10% 上调；[#36829](https://github.com/BerriAI/litellm/pull/36829) PTU 停止按 token 计费。
- **流式日志修复**：[#36761](https://github.com/BerriAI/litellm/pull/36761) 让 Responses 内部流按真实 stream 状态记录，避免成本漏记。
- **Vertex passthrough 错误信息**：[#36836](https://github.com/BerriAI/litellm/pull/36836) Google token minting 失败时返回可操作的 401，而不是裸 500。

### 已关闭 / 确认修复的老 issue

- OCI Gemini tool call 异常：已关闭 [#18654](https://github.com/BerriAI/litellm/issues/18654)
- Responses API streaming 缺少 SSE setup events：已关闭 [#20975](https://github.com/BerriAI/litellm/issues/20975)
- vector store 使用 model mapping public name 创建失败：已关闭 [#23980](https://github.com/BerriAI/litellm/issues/23980)
- DB 中的 models 不显示在 AI Hub：已关闭 [#19853](https://github.com/BerriAI/litellm/issues/19853)

## 对应用开发者的意义

- **多租户/团队权限场景建议跟进新版**：Access group 挂在 key 上“看起来成功但实际不授权”、team fallback 可能放宽模型访问、`lite login` 不带 team grants 等都属于核心授权一致性问题。今天已有多个修复 PR，但均未合并，依赖这些功能的项目建议等下一个稳定版再升级。
- **MCP + 多 worker Proxy 用户应关注**：MCP OAuth 授权流程在多 worker 下可能随机 404，DB-backed drafts 修复对此很有价值；另外 OpenAPI→MCP 工具生成丢 `$ref` schema 的问题会影响 FastAPI/Pydantic agent 工具接入，建议留意 [#36765](https://github.com/BerriAI/litellm/issues/36765)。
- **计费影响**：Azure GPT-5.6、Vertex Claude regional、PTU deployment 的成本统计都在修正。如果依赖 SpendLogs 做预算告警，升级后数字可能变化。
- **新模型可直接体验**：Voyage 4 embedding、Meta Muse Spark 1.2 已进入 cost map；用 `/v1/models` 拉取即可看到。
- **新 CLI**：`lite pi` 命令可以让 pi coding agent 直接走 proxy，省去手动生成虚拟 key，类似 `lite claude`/`lite codex`（暂隐藏于 `lite --help`）。[PR #36841](https://github.com/BerriAI/litellm/pull/36841)

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 动态日报 — 2026-08-14

## 1. 今日速览

Unsloth 正式发布 **v0.1.702-beta**，推出跨平台桌面应用 **Unsloth Desktop**（Windows/macOS/Linux），并同步为所有外部 Provider 增加工具调用/网络搜索支持。与此同时，桌面版首日暴露出大量安装与硬件兼容问题（AMD GPU 识别、Windows 安装超时、macOS 二次启动崩溃），官方已通过多个 PR 快速响应修复。

---

## 2. 版本发布与破坏性变更

### v0.1.702-beta
- **[发布]** [Unsloth Desktop 正式上线](https://github.com/unslothai/unsloth/releases) — 首个可在本地运行与训练 AI 模型的桌面应用，支持研究、导出与部署一体化流程，覆盖 Windows、macOS 和 Linux。
- **[变更]** 工具调用 / 网络搜索现已支持**所有外部 Provider**（不再局限于原生 GGUF/safetensors 模型）。

**开发者注意**：
- 若使用云端 API（外部 Provider），升级后工具调用行为可能变化，需验证已有 Agent 工作流。
- 桌面版与 CLI/Python 包并行发布，安装/升级路径相互独立，注意区分。

---

## 3. 新模型与硬件支持

### 模型相关
- **MiniMax-H3**：桌面版出现两个 bug — `stable-diffusion.cpp` 版本过旧不支持（[#8507](https://github.com/unslothai/unsloth/issues/8507)，已关闭）；视频生成时 Qwen3VL 文本编码器权重加载失败（[#8666](https://github.com/unslothai/unsloth/issues/8666)，OPEN）。MiniMax-H3 在桌面版上尚不可用。
- **DeepReinforce Ornith-1.0**：社区强烈要求支持（23 👍，[#6721](https://github.com/unslothai/unsloth/issues/6721)），暂未排期。
- **Kimi K3**：通过 API key 访问时模型不可用（[#8735](https://github.com/unslothai/unsloth/issues/8735)）。

### 硬件兼容
- **AMD GPU**：RX 5700XT 在 Unsloth Desktop 中无法识别（[#8529](https://github.com/unslothai/unsloth/issues/8529)）；Windows 平台 AMD GPU 安装失败已有 fix（[#8508](https://github.com/unslothai/unsloth/issues/8508)，标记 fixed）；ROCm 多 GPU 自动选择误选 iGPU 问题已确认修复（[#7624](https://github.com/unslothai/unsloth/issues/7624)，closed）。
- **ROCm/Strix Halo**：`GGML_CUDA_ENABLE_UNIFIED_MEMORY=1` 导致 GPU 无法 offload（[#8651](https://github.com/unslothai/unsloth/issues/8651)，已关闭，疑似有解）。

---

## 4. 性能与优化

| 方向 | 内容 | 状态 |
|------|------|------|
| **CPU 使用率** | [PR #8750](https://github.com/unslothai/unsloth/pull/8750) 修复长流式回复导致 Studio CPU 饱和、DOM 膨胀、UI 无响应的问题 — 采用增量 Markdown 解析 + 减少动画 DOM | OPEN |
| **Metal 显存** | [PR #8709](https://github.com/unslothai/unsloth/pull/8709) 修复 Apple Silicon 上 `-c 0` 被解析为完整 native context，导致 `--fit` 无法缩减上下文、内存超限的问题 | OPEN |
| **Kaggle 存储** | [PR #8439](https://github.com/unslothai/unsloth/pull/8439) 使用 Kaggle 大容量 overlay 保存模型，并在 GGUF 导出超出配额时主动拒绝，避免静默失败；同时修复 `IS_KAGGLE_ENVIRONMENT` 检测误报 | OPEN |
| **下载进度** | [PR #8384](https://github.com/unslothai/unsloth/pull/8384) 修复训练开始时模型已存在却显示"Downloading 99%"、最后下载与首步之间无进度反馈的问题 | CLOSED |
| **后端 CI** | [PR #8749](https://github.com/unslothai/unsloth/pull/8749) + [#8740](https://github.com/unslothai/unsloth/pull/8740) 修复 Backend CI 采集阶段崩溃（heavy imports），暴露出 4 个此前被遮蔽的真实测试失败，并逐一修复 | OPEN |

---

## 5. 稳定性与回归

按严重程度排列：

### 🔴 高危（安装/启动失败）
- **Windows 安装因 2 小时超时被杀**，下载 cu126 PyTorch 无进度输出（[#8698](https://github.com/unslothai/unsloth/issues/8698)，OPEN，从 #8546 拆分）
- **Windows 安装流程不完整**：后端启动时 `SSLKEYLOGFILE` 不可写导致崩溃（[#8546](https://github.com/unslothai/unsloth/issues/8546)，OPEN）
- **RX 5700XT 在桌面版中完全不识别**（[#8529](https://github.com/unslothai/unsloth/issues/8529)，OPEN）
- **macOS M4 二次启动报错**（[#8610](https://github.com/unslothai/unsloth/issues/8610)，OPEN）

### 🟠 中危（运行时崩溃/功能不可用）
- **MiniMax-H3 视频生成崩溃**：`sd-cli exited -6`，Qwen3VL 权重加载失败（[#8666](https://github.com/unslothai/unsloth/issues/8666)，OPEN）
- **macOS 上 llama-server 无法启动本地 GGUF 模型**，且空闲内存占用异常（[#8566](https://github.com/unslothai/unsloth/issues/8566)，OPEN）
- **Deep Research 冻结在 "Writing The Report" 阶段**，无 token 计数可查（[#8483](https://github.com/unslothai/unsloth/issues/8483)，OPEN）

### 🟡 功能回归
- **GGUF 导出流程回归**：必须先导出 16bit 全量权重（40GB+），无法直接导出 GGUF（[#8717](https://github.com/unslothai/unsloth/issues/8717)，OPEN，用户强烈不满）
- **工具调用污染对话历史**：tool calling 的消息被写入 chat history 造成上下文污染（[#8734](https://github.com/unslothai/unsloth/issues/8734)，OPEN，严重性较高）
- **Claude Code 401**：Unsloth API 只接受 `Authorization: Bearer`，不兼容 Anthropic 的 `x-api-key` 头（[#8663](https://github.com/unslothai/unsloth/issues/8663)，已关闭，需确认修复方式）
- **MLX 模型不在 `/v1/models` 列表**，API 自动切换模型时无法加载（[#8748](https://github.com/unslothai/unsloth/issues/8748)，OPEN）

### ✅ 已有修复 PR
- **Windows HF 数据集符号链接失败**（WinError 1314）→ 添加回退到普通文件缓存模式（[PR #8741](https://github.com/unslothai/unsloth/pull/8741)）
- **工具审批卡片延迟** → 添加 SSE keepalive（[PR #8628](https://github.com/unslothai/unsloth/pull/8628)，CLOSED）
- **安全扫描误报** → 修复 package scanner 将 `model.eval()` 误读为内置 `eval`（[PR #8386](https://github.com/unslothai/unsloth/pull/8386)，OPEN）
- **桌面版下载链接失效**（Linux .deb 断裂）→ 统一指向 canonical 下载页（[PR #8730](https://github.com/unslothai/unsloth/pull/8730)，CLOSED）

---

## 6. 对应用开发者的意义

1. **工具调用栈已开放**：v0.1.702 起外部 Provider 也支持工具调用/网络搜索 — Agent 类应用可统一通过 Unsloth 网关接入，但需关注 [#8734](https://github.com/unslothai/unsloth/issues/8734) 中工具调用污染对话历史的问题，生产环境建议在应用层清洗消息。

2. **API 兼容性注意事项**：
   - 目前仅接受 `Authorization: Bearer`，**不支持 Anthropic 的 `x-api-key` 头**（[#8663](https://github.com/unslothai/unsloth/issues/8663)）— Claude Code 类工具无法直接接入。
   - 桌面版 API 默认只监听 `127.0.0.1`，局域网访问需自行加隧道（[#8578](https://github.com/unslothai/unsloth/issues/8578)），已有 Cloudflare Tunnel 选项但无 `0.0.0.0` 绑定。
   - MLX 模型当前不会出现在 `/v1/models` 中（[#8748](https://github.com/unslothai/unsloth/issues/8748)），API 自动模型切换会失败。

3. **GGUF 导出成本上升**：当前流程要求先导出 16bit 模型（40GB+），再转 GGUF（[#8717](https://github.com/unslothai/unsloth/issues/8717)）。对高频导出 GGUF 的流水线，建议暂缓升级或评估替代路径。

4. **AMD 用户谨慎升级桌面版**：RX 5000 系列完全不识别（[#8529](https://github.com/unslothai/unsloth/issues/8529)），Windows 安装流程在 AMD 平台仍有失败案例（[#8546](https://github.com/unslothai/unsloth/issues/8546)），ROCm 多 GPU 机器注意 iGPU 被误选的问题（[#7624](https://github.com/unslothai/unsloth/issues/7624)）。

5. **llama-server 参数透传**：[PR #8702](https://github.com/unslothai/unsloth/pull/8702) 将为模型设置添加额外的 llama-server 参数输入框 — 对需要细粒度控制采样/上下文/GPU 层数的开发者是重要补充，目前 README 中 283 个 flag 中仅约 115 个被 Studio UI 管理。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*