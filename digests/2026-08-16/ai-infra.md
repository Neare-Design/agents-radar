# AI 基础设施日报 2026-08-16

> 生成时间: 2026-08-15 23:14 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# AI 基础设施横向对比分析报告 · 2026-08-16

## 1. 生态全景

模型发布节奏已从“月度”进入“周度”：DeepSeek-V4-Flash、Kimi-K3、Qwen3.8、Gemma 4、MiniMax 系列在同一天内出现在 5 个项目的动态中，推理引擎集体进入“先适配、后稳定”的追赶状态。更危险的信号是**静默正确性错误**密集出现——SGLang DSA 在 >65535 token 的 extend 上静默跳过 attention 计算、vLLM 在 ROCm 上出现静默检索损坏——这类“输出不报错但结果错”的问题比崩溃更难排查。上游内核创新（KDA 线性注意力、三元 MoE、稀疏 MLA）与下游应用需求（Agent 工具调用、流式解析、多模态回传）在同步快速膨胀，网关层则出现了首批安全审计报告。整体判断：**功能适配速度正在超越稳定性保障能力，生产部署需要更保守的版本策略。**

## 2. 各项目活跃度对比

基于日报提及的核心条目计数（非当日全量流量）：

| 项目 | Issues 数 | PRs 数 | Release / 构建情况 |
|---|---|---|---|
| vLLM | ~22 | ~10 | 无正式 Release；0.27.0 升级后 DeepSeek-V4-Flash 存在兼容问题 |
| SGLang | ~22 | ~19 | 无 Release；PD 协议统一与 Radix Cache 重构进行中 |
| llama.cpp | ~11 | ~13 | 无正式 Release；合入 b10437–b10448 多个 build |
| Ollama | ~18 | ~8 | **v0.32.14-rc0**（WebP 转码、Qwen 渲染器容错） |
| LiteLLM | ~12 | ~9 | 无 Release；3 个安全审计报告已关闭待修复 |
| Unsloth | ~20 | ~15 | 无 Release；Studio/Desktop 产品化迭代活跃 |

> 注：Issue/PR 计数以日报中提及的编号为准，各项目实际当日流量可能更高。

## 3. 模型支持竞速

- **llama.cpp 跑在最前面**：24 小时内合入 **Kimi-K3 完整混合架构**（KDA 线性注意力 + MLA + latent MoE，b10448）和 **MiniMaxText01 / MiniMaxM1**（b10437），另有 Maple 20B-A1B 三元 MoE、TML Inkling 在审查中；MTP 草稿模型支持自动发现（b10444）。架构覆盖广度与合入速度均为当日第一。
- **vLLM 宽度领先但稳定性拖后腿**：新增 GLM-5.2 TurboQuant 稀疏后端、SM120 FlashInfer sparse MLA DCP、CUDA 13.4 / Rubin（sm_107）预发布管线；但 **DeepSeek-V4-Flash 在 0.26→0.27 后闪崩**，H100/H20/ROCm 上多个崩溃与静默损坏 issue 未修复。
- **SGLang 多模态与新内核同步推进**：Hunyuan3D Paint/Delight 原生支持、LTX-2/2.3 NPU 优化；Kimi-Linear 路线到原生 Cake kernel（零拷贝 checkpoint + packed decode）是最值得关注的架构级推进。但 DSA/DSPARK 在 DeepSeek-V4-Flash / Kimi K3 上的正确性风险使其“支持列表”需要打折扣看待。
- **Ollama 节奏最保守**：官方仓库今日无新模型合入；Qwen3.8 系列的 system message 回归、MTP 性能减半、AMD gfx1200 加载失败均为社区侧反馈，`qwen3moe` 架构自动检测修复仍在审查中。
- **Unsloth 主打“新模型先能训练”**：社区已在跑 Gemma-4-26B-A4B、Qwen3.8-27B-GGUF、Ideogram 4；Studio 新增 oMLX 本地模型扫描，训练侧适配速度快于推理侧。
- **LiteLLM 属网关适配**：voyage-code-4 嵌入模型已加入成本表，只需映射参数无需支持架构。

## 4. 性能优化前沿

优化火力集中在五个方向：

| 方向 | 代表动态 | 量化效果 |
|---|---|---|
| **KV cache 量化与访存** | llama.cpp 修复混合 K/V 类型禁用 FA（~30x prefill 回退）、Q4 KV prefill 回退修复；SGLang MLA page_first 128-bit 写入；SYCL TILE kernel 覆盖量化 KV decode | SYCL 上 +42%~169%（Qwen3.6/Gemma 4，32K/118K context）；MLA gather +32% |
| **稀疏注意力 / MLA 算子** | vLLM ROCm gfx942 indexer 重写（native FP8 MFMA + LDS occupancy 门控）；SGLang dsa-topk fused kernel、Kimi-Linear 原生 Cake kernel | 针对 DeepSeek-V4 系稀疏注意力；尚在 PR 阶段 |
| **Speculative decoding 调度** | llama.cpp server yield_to_queue 线程模型重设计、MTP 自动发现；vLLM suffix drafter GPU 化；SGLang DSPARK target-only trajectory scoring | 官方宣称提升高并发/agentic 重复请求的 CPU/GPU overlap |
| **加载与启动开销** | Unsloth GGUF 缓存复用（7 次 Hub round-trip → 1 次）、max_steps 小步训练预处理 11m14s → 接近零；Ollama GetModel/Capabilities 缓存（~300ms/请求） | 高 QPS 推理服务收益明显 |
| **分布式 / PD 基础设施** | SGLang PD 协议统一（mooncake/nixl/mori 三合一）、vLLM SM120 DCP 上下文并行、LiteLLM Ollama 远程 api_base 超时修复（-8s/请求） | 架构级重构，短期无性能收益但有稳定性价值 |

值得注意：当日没有出现新的训练侧并行策略或通信优化，**优化重心明显从“单卡算子极致”转向“组合路径的稳定性修复与调度效率”**。

## 5. 分层定位差异

```
┌─────────────────────────────────────────────────┐
│  LiteLLM：AI 网关/代理层                          │
│  统一 API、预算/限流/Guardrail、成本核算、安全边界  │
├─────────────────────────────────────────────────┤
│  vLLM / SGLang：数据中心/生产级推理引擎            │
│  高吞吐服务化、PD/PP/TP/DCP、RadixCache、         │
│  量化内核、Blackwell/ROCm 深度优化                │
├─────────────────────────────────────────────────┤
│  llama.cpp / Ollama：本地/桌面/边缘运行时          │
│  llama.cpp 跨 CPU/GPU/SYCL/Vulkan/NPU；           │
│  Ollama 在其上做产品化封装（模型管理、API、桌面）   │
├─────────────────────────────────────────────────┤
│  Unsloth：训练/微调优化框架 → Studio 产品化       │
│  LoRA/量化/长上下文训练、GGUF 导出；              │
│  正在向本地推理 API 延伸                          │
└─────────────────────────────────────────────────┘
```

核心差异：**vLLM/SGLang 竞争的是“多卡多租户生产环境的最优吞吐与稳定性上限”；llama.cpp/Ollama 竞争的是“单机/个人设备上最快的模型运行覆盖面”；LiteLLM 竞争的是“模型路由与成本治理的控制面”；Unsloth 竞争的是“新模型的训练到部署最短路径”。** 今日动态中，vLLM/SGLang 的问题集中在长上下文 + 多卡组合路径，Ollama/llama.cpp 的问题集中在特定硬件回归（Intel A770、Pascal、AMD RDNA4），LiteLLM 的问题集中在权限与计费，分层特征非常清晰。

## 6. 值得关注的趋势信号

1. **模型发布节奏已快于推理栈的稳定周期**：同一个 DeepSeek-V4-Flash 在 vLLM 上 0.27 崩溃、在 SGLang 上 DSPARK 损坏输出、在 llama.cpp 上却可运行；Kimi-K3 在 llama.cpp 合入、在 SGLang 上 PD+DCP+DSPARK 崩溃。**不同引擎对同一模型的可用性出现显著分化**，选型需按模型逐一做验证矩阵。

2. **静默正确性错误成为生产级基础设施的头号风险**：当日共出现至少 5 起“输出不报错但结果错”的问题（SGLang DSA 空算、DSPARK 标识符损坏、FP8 scale 丢弃、vLLM ROCm 静默检索损坏、Ollama audio 静默丢弃）。**建议在应用层增加输出的随机性/重复性检测和黄金样本定期校验。**

3. **长上下文 + 投机解码 + 量化 + 分布式组合开始暴露“组合爆炸”问题**：llama.cpp 量化目标下 spec decode 与 greedy 偏离、vLLM MTP + xgrammar livelock、Ollama MTP 性能减半。单个特性可用不代表组合可用，**生产环境启用组合前必须做分级验证（单特性 → 双特性 → 全组合）**。

4. **Agent/工具调用是跨项目的高频故障域**：tool_choice 静默忽略（vLLM）、流式 chunk 丢数据（SGLang）、tool_calls index 串线（Unsloth）、reasoning 边界泄漏（vLLM gpt-oss）。Agent 框架层应建立**流式解析缓冲兜底 + 工具调用参数强校验**，不要信任任何单一引擎的流式输出边界。

5. **网关层的安全与计费正在成为新的敏感面**：LiteLLM 当日 3 个安全报告全部是“配置不当导致权限绕过或认证缺失”，另有流式 cost 被外部值干扰、Gemini TTS 不计费等计费漂移问题。**网关部署应做安全基线检查（master key、api_base 白名单、预算变更权限），并监控单位成本漂移。**

6. **本地/桌面 AI 产品化在加速**：Ollama 发布 rc 版修复渲染器回归、Unsloth Studio 增加 oMLX 模型发现并持续完善桌面端、社区对 LAN 直连（0.0.0.0）的呼声持续升温。**个人多设备推理网络正在成形**，远端运行时 + 本地模型管理的混合架构值得提前布局。

**给 Agent/应用开发者的简洁建议**：锁定已验证的引擎版本（vLLM 锁 0.26.x、Ollama 规避 0.32.7+ 的 text completion 回归）；长上下文启用 chunked prefill 规避 >65535 token 静默错误；DSPARK/量化 KV 输出与 greedy bf16 基线做对照；显式传参（temperature、thinking、tool_choice）不依赖默认值；流式 tool-call 做缓冲重试；网关层立即核查 LiteLLM 三个安全问题；升级 llama.cpp 后优先回归 speculative decoding 与 SWA 路径。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 动态日报 · 2026-08-16

> 数据更新截至 2026-08-15；覆盖过去 24h Releases / Issues / PRs。

## 今日速览

1. **DeepSeek-V4-Flash 是今日主旋律**：0.26→0.27 升级即触发 flash error（[#51758](https://github.com/vllm-project/vllm/issues/51758)），H100/H20/ROCm 上分别出现稀疏注意力崩溃与静默数据损坏（[#52109](https://github.com/vllm-project/vllm/issues/52109)、[#51743](https://github.com/vllm-project/vllm/issues/51743)、[#52339](https://github.com/vllm-project/vllm/issues/52339)）；上游已提交 C128A revert（[#51318](https://github.com/vllm-project/vllm/pull/51318)）与 gfx942 indexer 重写（[#52402](https://github.com/vllm-project/vllm/pull/52402)），但均未合入。
2. **Gemma 4 parser 曝出两个独立回归**：强制 tool_choice 被静默忽略（[#50477](https://github.com/vllm-project/vllm/issues/50477)）、`enable_thinking` 默认值与模板不一致（[#52410](https://github.com/vllm-project/vllm/issues/52410)）。
3. **MRv2 生态持续演进**：Mamba + prefix caching 启动崩溃已有 fix（[#52460](https://github.com/vllm-project/vllm/pull/52460)）；CUDA 13.4 / Rubin（sm_107）预发布镜像管线启动（[#52379](https://github.com/vllm-project/vllm/pull/52379)）。

## 版本发布与破坏性变更

- 过去 24 小时无正式 Release。
- **升级注意**：[#51758](https://github.com/vllm-project/vllm/issues/51758) 报告 vLLM 0.26.0 → 0.27.0 后 DeepSeek-V4-Flash 运行即报 flash error。在相关 fix 合入前，生产环境建议锁定 0.26.x。

## 新模型与硬件支持

- **CUDA 13.4rc1 / Rubin（sm_107）预发布构建管线**：[#52379](https://github.com/vllm-project/vllm/pull/52379)。为下一代 NVIDIA 硬件准备镜像与依赖 pin。
- **SM120 FlashInfer 稀疏 MLA 解码启用 DCP**：[#47779](https://github.com/vllm-project/vllm/pull/47779)，补充 Blackwell 消费级（RTX 50 系）在 FlashInfer sparse MLA 路径上的上下文并行能力。
- **GLM-5.2 TurboQuant 稀疏后端**：[#52472](https://github.com/vllm-project/vllm/pull/52472)，新增 packed 4-bit latent KV、稀疏 prefill/decode、MoE MTP 链路，并修复 DCP/MTP/PP 正确性。
- **Qwen3.8 Quark INT4 加载失败**：[#52454](https://github.com/vllm-project/vllm/issues/52454) 暴露 Quark 结构化 config 兼容问题，已由 [#52474](https://github.com/vllm-project/vllm/pull/52474) 修复。

## 性能与优化

- **ROCm gfx942 稀疏注意力 indexer 重写**（[#52402](https://github.com/vllm-project/vllm/pull/52402)）：`fp8_mqa_logits_gfx942` 路径引入 native FP8 MFMA 与修正后的 LDS occupancy 门控，针对 DeepSeek-V4 sparse-attention 索引器；不触碰 CUDA 路径。
- **W4A8-INT8 加载优化提案**（[#49529](https://github.com/vllm-project/vllm/issues/49529)）：建议采用 PTX ISA 9.4 新指令 `ldmatrix.s8.s4`（硬件 INT4→INT8 扩展加载），当前为 feature request。
- **KV offloading back-pressure 检测**（[#50045](https://github.com/vllm-project/vllm/pull/50045)）：当 Disk/Shared Storage/P2P 次级 tier 写延迟饱和时停止级联下发，避免任务无限堆积。
- **Suffix drafter GPU 化**（[#52097](https://github.com/vllm-project/vllm/pull/52097)）：为 async scheduling 增加 `suffix_gpu` drafter，针对高并发/agentic 重复请求提升 CPU/GPU overlap。

## 稳定性与回归

按严重程度排列；PR 状态以标注为准。

**A. 数据损坏 / 挂起 / 启动崩溃**

- [#52109](https://github.com/vllm-project/vllm/issues/52109)（ROCm/gfx942）DeepSeek-V4-Flash 静默检索损坏，prompt ≥ 4-5k tokens 时出现；作者已 backport #51821（merged）并验证 open PR #52058/#51252，问题仍可复现。**静默错误，风险最高**。
- [#49237](https://github.com/vllm-project/vllm/issues/49237) `POST /wake_up` 后 engine 假死（health 仍绿但 completions hang）；`init_fp8_kv_scales` 对 list 调用 `.zero_()` 触发。**暂无 PR**。
- [#49210](https://github.com/vllm-project/vllm/issues/49210) EngineCore livelock（100% CPU）由 MTP spec decode + xgrammar 触发，自 v0.24.0 回归。**暂无 PR**。
- [#51743](https://github.com/vllm-project/vllm/issues/51743) H100 TP4 下 `--max-num-batched-tokens >= 24576` 崩溃于 fused qnorm/rope/kv-insert；该中间分配对 memory profiler 不可见。**暂无 PR**。
- [#52339](https://github.com/vllm-project/vllm/issues/52339) H20-3e TP8 近 161K context 时 FlashMLA sparse prefill phase1.cuh:614 崩溃。**暂无 PR**。
- [#52247](https://github.com/vllm-project/vllm/issues/52247) EngineCore 在 `copy_event.synchronize()` 无限阻塞（GPU kernel 永不终止），无超时保护。**暂无 PR**。
- [#52317](https://github.com/vllm-project/vllm/issues/52317) MRv2 + Mamba prefix-cache `all` 模式 + dspark spec decode 启动崩溃。**已有修复 PR [#52460](https://github.com/vllm-project/vllm/pull/52460)**（fallback 到 `align`）。
- [#51758](https://github.com/vllm-project/vllm/issues/51758) 0.26→0.27 升级后 DeepSeek-V4-Flash flash error。**暂无 PR**。

**B. 功能 / 正确性回归**

- **Gemma 4 系列**：[#50477](https://github.com/vllm-project/vllm/issues/50477) 强制 tool_choice 被静默忽略（0.21.0 正常）；[#52410](https://github.com/vllm-project/vllm/issues/52410) parser 默认 `enable_thinking=true`，与模板默认 false 相反。
- [#43338](https://github.com/vllm-project/vllm/issues/43338) grammar-mask spec-decode 修复未覆盖多 token reasoning 边界，gpt-oss 仍泄漏 reasoning token（Qwen3 已修复）。
- [#38488](https://github.com/vllm-project/vllm/issues/38488) 历史 assistant 消息中的 `reasoning_content` 被静默丢弃（`chat_utils.py` 只读 `reasoning`）。
- [#37974](https://github.com/vllm-project/vllm/issues/37974) Kimi-K2.5 在 PP≥2 时 KeyError。
- [#52434](https://github.com/vllm-project/vllm/issues/52434) `ParallelLMHead` 缺少 `output_size_per_partition` 属性，影响新模型/量化路径。
- [#51884](https://github.com/vllm-project/vllm/issues/51884) FP8 block-scaled 权重在 sm120（RTX 5090）加载时 DeepGEMM 报 Unknown SF transformation。
- [#52155](https://github.com/vllm-project/vllm/issues/52155) `VLLM_BATCH_INVARIANT` 未覆盖 VAE 卷积，影响 ROCm 多模态。
- [#32180](https://github.com/vllm-project/vllm/issues/32180) AMD gfx1151（Strix Halo）V1 引擎性能瓶颈与不稳定。
- [#52300](https://github.com/vllm-project/vllm/issues/52300) vLLM 0.21.0 + CUDA 12.6 环境出现 `libcudart.so.13` ImportError。

**C. 已有修复 / 规避**

- DeepSeek-V4 C128A adaptive metadata packing 与 CUDA graph replay 不兼容，**Revert PR [#51318](https://github.com/vllm-project/vllm/pull/51318)**（open）。
- Mamba causal conv1d metadata alignment 问题，**fix PR [#52476](https://github.com/vllm-project/vllm/pull/52476)**（closed）。
- Qwen3.8 Quark INT4 加载，**fix PR [#52474](https://github.com/vllm-project/vllm/pull/52474)**（closed）。

## 对应用开发者的意义

- **DeepSeek-V4-Flash 生产部署风险偏高**：长上下文（≥4k）与 TP≥8 环境存在崩溃或静默损坏可能；0.27.0 升级不兼容。建议锁定镜像版本，等待 [#51318](https://github.com/vllm-project/vllm/pull/51318) / [#52402](https://github.com/vllm-project/vllm/pull/52402) / [#52109](https://github.com/vllm-project/vllm/issues/52109) 的后续修复。
- **Gemma 4 接入 Agent / tool-calling**：强制 tool_choice 会失效，thinking 默认值需显式指定，建议在应用层做参数校验，规避 [#50477](https://github.com/vllm-project/vllm/issues/50477) 与 [#52410](https://github.com/vllm-project/vllm/issues/52410)。
- **结构化输出 + spec decode**：gpt-oss 仍存在 reasoning 泄漏，不要依赖 grammar mask 处理多 token 边界；Qwen3 系不受影响。
- **Mamba-hybrid + MRv2 / DSpark / Context Parallelism**：先手动启用 `--mamba-cache-mode align`，避免踩中 [#52317](https://github.com/vllm-project/vllm/issues/52317) 的启动崩溃（修复见 [#52460](https://github.com/vllm-project/vllm/pull/52460)）。
- **环境兼容性提醒**：RTX 8000 等 sm75 老卡受 FA2 计算能力下限限制（[#52181](https://github.com/vllm-project/vllm/issues/52181)）；CUDA 12.6 + vLLM 0.21.0 会遇到 `libcudart.so.13` 缺失（[#52300](https://github.com/vllm-project/vllm/issues/52300)）。
- **多进程/分布式部署**：端口分配存在 `get_open_port()` 竞态窗口（[#51275](https://github.com/vllm-project/vllm/issues/51275)），高并发拉起 worker 时可能偶发 ZMQ `Address already in use`。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 动态日报 2026-08-16

## 今日速览

过去24小时无新版本发布，正确性问题集中爆发：最严重的是 DSA sparse-MLA prefill 在单次超过 65535 tokens 的 extend 上静默跳过全部 attention 计算（[#34947](https://github.com/sgl-project/sglang/issues/34947)），DSPARK 在 DeepSeek-V4-Flash / Kimi K3 组合下也出现输出损坏与确定性崩溃（[#34959](https://github.com/sgl-project/sglang/issues/34959)、[#34920](https://github.com/sgl-project/sglang/issues/34920)）。修复侧，MiniMax-H3 `quality="high"` 审计门（[#34978](https://github.com/sgl-project/sglang/pull/34978)）、unified-cache load-back pin（[#34975](https://github.com/sgl-project/sglang/pull/34975)）、SWA eviction frontier（[#34870](https://github.com/sgl-project/sglang/pull/34870)）等 PR 已落地或进入审核。KDA/Blackwell 方向 Kimi-Linear 原生 Cake kernel 集成（[#34299](https://github.com/sgl-project/sglang/pull/34299) / [#34946](https://github.com/sgl-project/sglang/pull/34946)）是当前最值得关注的架构推进。

## 版本发布与破坏性变更

过去24小时无新版本发布，无已合入的破坏性变更。

两项进行中的重构/RFC 未来可能带来迁移影响：

- **PD disaggregation 统一协议层**：mooncake/nixl/mori 三个传输后端目前各自实现完整 bootstrap + 传输控制协议，计划收敛为单一协议层、按后端插拔传输实现（[#33861](https://github.com/sgl-project/sglang/issues/33861)）
- **Unified Hybrid Radix Cache 重构**：统一 RadixCache/MambaRadixCache/SWARadixCache 等多份分叉实现（[#20415](https://github.com/sgl-project/sglang/issues/20415)）

## 新模型与硬件支持

- **Hunyuan3D Paint / Delight 原生支持**：以原生 SGLang 模型替代 Diffusers-owned 模块，新增 SD 2.1 兼容 UNet 与 Stable Diffusion VAE 配置（[#34980](https://github.com/sgl-project/sglang/pull/34980)）
- **Kimi-Linear 路由到原生 Cake kernels**：零拷贝 prefill checkpoints + packed decode，依赖 FlashInfer 原生 `lower_bound=None` kernels（[#34946](https://github.com/sgl-project/sglang/pull/34946) / [#34299](https://github.com/sgl-project/sglang/pull/34299)）
- **[AMD] Fast Triton Sparse MLA 后端**：为 ROCm DSA fp8 路径新增 `--dsa-prefill-backend triton` / `--dsa-decode-backend triton`（[#30575](https://github.com/sgl-project/sglang/pull/30575)）
- **[Blackwell] TRT-LLM DSv4 Attention for SM100/103**：high priority 集成仍在推进（[#30805](https://github.com/sgl-project/sglang/pull/30805)）
- **[AMD] K3 AITER prefill kernel 支持**：新增 concat_and_cast_mha_k_pad_kernel 支持 12-head（[#34837](https://github.com/sgl-project/sglang/pull/34837)）
- **[NPU] LTX-2/2.3 推理性能优化**（[#34722](https://github.com/sgl-project/sglang/pull/34722)）
- **Playground 新增验证单元格**：Qwen3.8-27B / rtx6000 / nvfp4 / balanced / single（[#34918](https://github.com/sgl-project/sglang/issues/34918)）

## 性能与优化

- **[ROCm/HiCache] MLA page_first KV gather 提速约 32%**：写入宽度提升至 128-bit，`block_quota` 默认 16 并支持运行时环境变量覆盖（[#30024](https://github.com/sgl-project/sglang/pull/30024)）
- **GDN speculative target 验证省去 QKV 物化**：`causal_conv1d_update` 已产出 packed QKV，不再需要额外 kernel 拆成三个连续张量（[#33778](https://github.com/sgl-project/sglang/pull/33778)）
- **[DSv4] 新增 `--dsa-topk-backend flashinfer` fused top-k**（[#33237](https://github.com/sgl-project/sglang/pull/33237)）
- **[NPU] DSpark folded 路径修复**：解决数值与 graph-replay parity 问题，同时提升性能（[#34944](https://github.com/sgl-project/sglang/pull/34944)）
- **[AMD] GPT-OSS benchmark 纳入 ROCm 7.2 nightly**，补上 Triton 3.7 性能回归的覆盖盲区（[#34645](https://github.com/sgl-project/sglang/pull/34645)）
- **实时 ASR 长音频模式**：segment snapshots 实现有界长音频转录（[#32682](https://github.com/sgl-project/sglang/pull/32682)）
- **Profiling 增强 [2/3]**：DTail 详细执行步骤注解（[#24911](https://github.com/sgl-project/sglang/pull/24911)）
- **性能反例**：PD disaggregation + Mooncake 在 H200 上、32k 输入/512 输出的单机对比测试中无明显吞吐收益，仍为开放 help wanted（[#24488](https://github.com/sgl-project/sglang/issues/24488)）

## 稳定性与回归

### 静默正确性错误（最危险）

- ⚠️ **DSA sparse-MLA prefill 静默空算**：SM100 上单次 unchunked extend > 65535 tokens 时，trtllm-gen `gridDim.z` 限制未防护，直接启动零个 attention kernel，输出静默错误。重复提交两个 issue（[#34947](https://github.com/sgl-project/sglang/issues/34947) / [#34941](https://github.com/sgl-project/sglang/issues/34941)），暂无 fix
- ⚠️ **DSPARK 静默破坏 DeepSeek-V4-Flash 标识符**：投机解码结果不可信（[#34959](https://github.com/sgl-project/sglang/issues/34959)），暂无 fix
- ⚠️ **compressed-tensors FP8 `lm_head.weight_scale` 被静默丢弃**：unsloth/Qwen3.8-27B-NVFP4 出现退化性重复生成（[#34895](https://github.com/sgl-project/sglang/issues/34895)），暂无 fix
- ⚠️ **Tool-call 解析器在流式 chunk 边界丢失数据**：多个 detector 受影响，根因包括 `parse_stream_chunk` 没有 end-of-stream flush（[#31915](https://github.com/sgl-project/sglang/issues/31915)），暂无 fix

### 崩溃与挂起

- ⚠️ **Scheduler 进程崩溃**：`token_ids_logprob` 请求与普通请求混批时 `AttributeError: 'list' object has no attribute 'tolist'`，影响 v0.5.14–v0.5.17（[#34719](https://github.com/sgl-project/sglang/issues/34719)），暂无 fix
- ⚠️ **Kimi K3 decode 确定性崩溃**：PD disaggregation + DCP + DSPARK 组合下 `cumsum(extend_prefix_lens=None)`，所有 TP rank 退出（[#34920](https://github.com/sgl-project/sglang/issues/34920)），暂无 fix
- ⚠️ **`--enable-eplb` + DSPARK CUDA graph 捕获崩溃**：`scatter_add_` 维度不匹配（[#34974](https://github.com/sgl-project/sglang/issues/34974)），暂无 fix
- ⚠️ **HF3FS HiCache + DeepSeek-V4 触发 ZeroDivisionError**（[#34969](https://github.com/sgl-project/sglang/issues/34969)），暂无 fix
- ⚠️ **HiCache 内存检查误报**：`psutil.virtual_memory().available` 未计入 HugePages 保留，导致假“Not enough host memory”（[#34972](https://github.com/sgl-project/sglang/issues/34972)），暂无 fix
- ⚠️ **`--profile-by-stage` + 投机解码冻结调度器约 25 秒**：TARGET_VERIFY 被误判为 prefill 导致 profiler 不停止，延迟的 stop 条件泄漏到后续请求（[#34943](https://github.com/sgl-project/sglang/issues/34943) / [#34942](https://github.com/sgl-project/sglang/issues/34942)），暂无 fix
- ⚠️ **Responses API 中 `function_call_output` 的 `input_image` 未转换为 `image_url`**：旧版报 400，当前 main 上被静默丢弃（[#34927](https://github.com/sgl-project/sglang/issues/34927)）

### 已关闭的回归

- Diffusion attention 后端 fallback 变更在多数模型上引入错误（[#34389](https://github.com/sgl-project/sglang/issues/34389)）
- flashinfer_trtllm MoE runner 在 B200 上破坏 MiniMax-M2.7-NVFP4 并对 DeepSeek-V4-Flash 触发 assert（[#26324](https://github.com/sgl-project/sglang/issues/26324)）
- Spec V2 verify + DSA + HiCache 的间歇性 NCCL 挂起（[#28011](https://github.com/sgl-project/sglang/issues/28011)）

### 已有修复 PR

- **MiniMax-H3 `quality="high"` 审计门缺失**：`validate_quality_deployment` 只验 server args 与 device，不检查实际 resident 的 DiT 权重，导致蒸馏 LoRA 与 high 质量模式可被混用。PR 将直接 fail closed（[#34978](https://github.com/sgl-project/sglang/pull/34978)，关闭 [#34954](https://github.com/sgl-project/sglang/issues/34954)）
- **Unified cache load-back pin 断言崩溃**：允许同一节点并发多个 load-back pin（[#34975](https://github.com/sgl-project/sglang/pull/34975)）
- **SWA eviction frontier 在 bigram keys 上的 pool-accounting 崩溃**：根因修复（[#34870](https://github.com/sgl-project/sglang/pull/34870)）
- **DSPARK target-only trajectory scoring 支持**（[#34979](https://github.com/sgl-project/sglang/pull/34979)）

### CI 与测试工程

- CI 跟踪：#17050 当前状态为 3 broken、11 flaky、669 recently fixed（[#17050](https://github.com/sgl-project/sglang/issues/17050)）
- 单元测试覆盖增强：600+ 测试文件多为 E2E，core modules（managers/mem_cache/entrypoints/sampling/parser/function_call/utils）仍需补充单测，标记为 good first issue（[#20865](https://github.com/sgl-project/sglang/issues/20865)）
- Unified Radix Cache 需要 bit-exact 正确性覆盖（[#34899](https://github.com/sgl-project/sglang/issues/34899)）

## 对应用开发者的意义

- **`token_ids_logprob` 混批会拖垮整个调度器**：v0.5.14–v0.5.17 受影响。如果使用该参数，请确保所有请求统一启用，或升级到包含修复的版本（[#34719](https://github.com/sgl-project/sglang/issues/34719)）
- **长上下文 + DSA/SM100 存在静默错误风险**：单次 extend 超过 65535 tokens 时输出不会报错但完全错误。启用 `--chunked-prefill-size` 可规避（[#34947](https://github.com/sgl-project/sglang/issues/34947)）
- **DSPARK 在部分模型上尚不可信**：DeepSeek-V4-Flash 存在标识符损坏，Kimi K3 + PD disagg + DCP 组合会确定性崩溃。生产环境建议暂缓或加严验证（[#34959](https://github.com/sgl-project/sglang/issues/34959) / [#34920](https://github.com/sgl-project/sglang/issues/34920)）
- **流式工具调用解析器有已知丢数据边界**：Agent 框架层应增加缓冲兜底，不要假设 chunk 边界安全（[#31915](https://github.com/sgl-project/sglang/issues/31915)）
- **多模态 Responses API 的 `input_image` 回传当前不可用**：`function_call_output` 中图片会触发 400 或静默丢弃（[#34927](https://github.com/sgl-project/sglang/issues/34927)）
- **缓存与 PD 重构正在路上**：Unified Radix Cache（[#20415](https://github.com/sgl-project/sglang/issues/20415)）和 PD 协议统一（[#33861](https://github.com/sgl-project/sglang/issues/33861)）都会改变底层行为。如果使用 `--enable-hierarchical-cache`，升级后重点回归 eviction/load-back 路径（[#34870](https://github.com/sgl-project/sglang/pull/34870) / [#34975](https://github.com/sgl-project/sglang/pull/34975)）
- **Benchmark 工具修复了 fork 崩溃**：`sglang.benchmark.*` 不再继承父进程已初始化的 accelerator 状态（[#34712](https://github.com/sgl-project/sglang/pull/34712)）

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 动态日报 2026-08-16

## 今日速览

今日最核心的动态是 **Kimi-K3 文本模型正式合入主线（b10448）**——该混合架构（KDA 线性注意力 + MLA 全注意力，附带回转注意力特征和 latent MoE）在模型架构层面有较大增量；同时 **server 端 yield_to_queue 线程模型被重新设计（b10447）**，将 speculative 处理逻辑移入 worker 线程，有并发/调度行为的实际影响。稳定性方面，SYCL 在 Intel A770 上的完全崩溃（#27063）仍无修复 PR，值得关注。

## 版本发布与破坏性变更

- **b10441：废弃 `--mmap`/`--no-mmap` 等旧标志，改为统一的 `--load-mode` 参数**
  该 PR 将已弃用的 `--mmap`、`--no-mmap`、`--mlock`、`--direct-io` 合并为单一 `--load-mode`，同步更新了脚本、示例与文档。使用旧标志的启动命令会收到内部警告，建议尽早迁移。
  https://github.com/ggml-org/llama.cpp/pull/26934 （已被合入 b10441）

- **b10447：server 端 yield_to_queue 线程模型重新设计**
  核心变更：`common_speculative_process` 改在 worker 中运行，worker 与主线程的职责对调。影响面集中在 speculative decoding 的 server/gateway 场景，预计对多请求并发和任务队列语义有影响，建议关注后续 issue 反馈。
  https://github.com/ggml-org/llama.cpp/pull/27133 （b10447）

- **b10444：`--models-dir` 支持自动加载 MTP 助手模型**
  该功能为 speculative decoding 的用户体验改进——现在可以从模型目录自动发现并加载 MTP 草稿模型，无需手动指定。
  https://github.com/ggml-org/llama.cpp/pull/24431 （b10444）

## 新模型与硬件支持

- **Kimi-K3 文本模型（b10448，已合入）**
  混合架构：KDA（线性）注意力 + MLA（全）注意力，并新增 5 项架构特性：cross-layer residual attention（`attn_res_block_size`）、latent MoE 等。这是主线上少见的完整新架构合入。
  https://github.com/ggml-org/llama.cpp/pull/26185

- **MiniMaxText01 / MiniMaxM1 支持（b10437，已合入）**
  新增两种 MiniMax 因果语言模型架构支持；针对 MiniMax-Text-01 的 embeddings 零值 token，模型还会额外设置 logits mask。此外，代码中为 MiniMax-M1 保留了独立分支，表明模型文件的 CI 覆盖已同步接入。
  https://github.com/ggml-org/llama.cpp/pull/27018

- **Maple 20B-A1B 三元 MoE 架构（PR #27000，审查中）**
  新增 DeepGrove 的 Maple 架构支持：24 层、256 专家（8 active）、SWA-512 与全局注意力 3:1 交错、TQ1_0/TQ2_0 三元权重。目前仅 CPU 路径，头部为 `maple`。
  https://github.com/ggml-org/llama.cpp/pull/27000

- **TML Inkling 架构（PR #25731，审查中）**
  新架构支持，含 safetensors→GGUF 转换器、图构建及所需 kernel 变更；实现中为 Flash Attention 新增 banded attention kernel 变体。
  https://github.com/ggml-org/llama.cpp/pull/25731

- **ROCm Docker 升级至 7.14.0（PR #27145）**
  同步升级 base image 至 Ubuntu 26.04，新增一批已支持 GPU 架构（gfx9xx），并附带一个“no usable GPU found”的 workaround。针对 ROCm Docker 用户的部署配置需要跟随更新。
  https://github.com/ggml-org/llama.cpp/pull/27145

## 性能与优化

- **CUDA：混合 K/V 类型时 Flash Attention 不再被禁用（PR #27150，进行中）**
  当前若 `-ctk` 与 `-ctv` 类型不同，CUDA 会整体关闭 flash attention、退回 CPU 执行，导致 prefill 慢 ~30x 且无任何警告。该 PR 将类型组合判断放开以保留 FA 路径，对 KV 缓存量化的实际部署有明显收益。
  https://github.com/ggml-org/llama.cpp/pull/27150

- **CUDA：小 KV 量化在 prefill 上的性能回退修复（PR #27140）**
  Q4 等低比特 KV 量化在 prefill 阶段会变得极慢，该 PR 定位并修复了该问题。作者报告 Q8_0 压缩率不足、Q4 等小 KV 量化 prefill 严重退化，是当前 KV 量化方案选择的重要参考（见 Issue #27109）。
  https://github.com/ggml-org/llama.cpp/pull/27140

- **SYCL: Q4_K 多列 MMVQ 去除冗余重建（PR #27062）**
  修复 Q4_K 在目标列上的重复解量化问题。作者在 DFlash 测试中发现 Q4 性能低于 Q8/FP16，根因即此，合入后可改善 SYCL 后端 Q4_K 推理表现。
  https://github.com/ggml-org/llama.cpp/pull/27062

- **SYCL: TILE kernel 覆盖量化 KV decode（PR #26689，merge ready）**
  让 decoded 阶段用 TILE kernel 处理 q4_0/q8_0 的 KV cache。作者在 BMG 上测得该变化对 Qwen3.6-35B、Gemma 4 26B/12B 在 32K/118K context 下普遍 **+42% ~ +169%**，且无回退。目前仅 gate 修改，风险较低。
  https://github.com/ggml-org/llama.cpp/pull/26689

- **mtmd：视觉编码器一次性初始化提前到 warmup 阶段（PR #27152）**
  当前视觉模型的 warmup 不启动任何 kernel，导致首次视觉请求会触发一次性初始化、响应明显滞后；申请改为 warmup 阶段执行一次 dummy encode。
  https://github.com/ggml-org/llama.cpp/pull/27152

## 稳定性与回归

以下按当前严重程度/影响面排列。尚无对应 fix PR 的条目已标注。

- **【严重】SYCL 在 Intel A770 上完全崩溃（#27063，无 fix）**
  Linux + A770（Arc）上任意模型均崩溃，B60 上工作正常，疑似 A770 独有的 SYCL 回归。此问题自 8 月 14 日上报，影响面为 Intel Arc 系列 SYCL 用户。
  https://github.com/ggml-org/llama.cpp/issues/27063

- **【严重】SWA/循环记忆导致 server 强制全量重算 prompt（#21831，52 评论，无 fix）**
  复现于 Windows + CUDA（RTX 5060 Ti 16GB），在启用 SWA/循环记忆的模型上，多次请求时 server 会丢弃缓存的 KV 并完整重算 prompt。是当前 issue 区热度最高的未解决问题，涉及会话型 Agent 场景的核心体验。
  https://github.com/ggml-org/llama.cpp/issues/21831

- **【严重】DSV4-Flash 在 churned-reuse 时 SWA KV-cache 耗尽（#25452，无 fix）**
  5 卡混合 GPU（3090 + 5060 Ti + 4060 Ti）上跑 DeepSeek-V4-Flash，触发 crash + stall；与 #21831 同属 SWA/recurrent memory 生命周期管理问题，推测同根因。
  https://github.com/ggml-org/llama.cpp/issues/25452

- **【高】Vulkan 性能下降（#24066，40 评论）**
  Vulkan 后端近版本在 RX 6600 上出现 token/s 下降，open 状态。另有一个 DeviceLost 问题 #25664（Strix Halo，Linux 7.x + RADV），主线上已有 Vulkan coopmat1 修复（b10442），可能部分缓解。
  https://github.com/ggml-org/llama.cpp/issues/24066
  https://github.com/ggml-org/llama.cpp/issues/25664

- **【高】4-bit KV cache 使 Qwen 3.5 混合模型 prefill 暴跌至 ~34 t/s（#27109，无 fix）**
  在 RTX 3090 上 q4_1/q4_0 KV 量化触发 prefill 崩溃式性能下降，MMQ 守卫通过但仍异常。与 PR #27140 高度相关，若合入可解决该问题的大部分；但 #27140 目前仍开放。
  https://github.com/ggml-org/llama.cpp/issues/27109

- **【中】speculative decoding 在量化目标上输出与 greedy 偏离（#25618）**
  draft-mtp/draft-dspark 在 `temperature=0, top_k=1` 下，若目标模型为 Q4_K_M 等量化版本，输出与未量化的 bf16 目标不一致；ngram 推测在同量化目标下正常。复现于 CUDA。
  https://github.com/ggml-org/llama.cpp/issues/25618

- **【中】Windows 编译时选择 AOCL 会失败（#25413）**
  Windows + BLAS + AOCL 组合编译失败，使用 OpenBLAS 正常。不影响默认构建（默认不启用 BLAS）。
  https://github.com/ggml-org/llama.cpp/issues/25413

- **【中】Windows Defender AI 引擎在新版本 CPU 构建中误报（#26343）**
  用户报告 b10195-win-cpu-x64 在 Defender 下被标记为病毒。社区对此类误报已有常规沟通渠道，通常会在几轮更新后消失。
  https://github.com/ggml-org/llama.cpp/issues/26343

- **【低】Glimmer Q8_0 多卡 tensor split 触发 GGML_ASSERT（#26902）**
  4x Tesla T10 上使用 tensor split 加载 Glimmer 时触发 `GGML_BACKEND_SPLIT_AXIS_UNKNOWN` 断言。属于多卡 split 的特有失败路径。
  https://github.com/ggml-org/llama.cpp/issues/26902

- **【视觉】Qwen 27B 3.6/3.8 在 AMD AI Max 上视觉失效（#27124，无 fix）**
  Vulkan 后端 + Ryzen AI MAX+ 395 上，Qwen 27B 3.6/3.8 的视觉能力不可用，欢迎有同类硬件的开发者协助复现。
  https://github.com/ggml-org/llama.cpp/issues/27124

## 对应用开发者的意义

1. **`--load-mode` 迁移必须尽早完成**：`--mmap`/`--no-mmap`/`--mlock`/`--direct-io` 的兼容层预计会在后续版本移除。对将启动参数硬编码在运维脚本/容器内的应用，建议在升级至 b10441 及以后版本时优先处理。

2. **server 线程模型的变更影响 speculative decoding 类网关/Agent 应用**：b10447 将 speculative 处理移入 worker 线程，多请求并发调度行为可能与旧版本不一致。建议对此类部署做回归测试（尤其是高并发场景），并持续关注 issue #21831（server 在 SWA 模型上强制全量重算）的进展——该问题直接影响有状态交互类 Agent 的体验和成本。

3. **KV 量化选择的实用建议**：当前主线上 `-ctk q4_0/q4_1` 等低比特 KV 量化在 prefill 上仍有性能回退问题（#27109），在 PR #27140 合入之前，对 prefill 延迟敏感的生产环境建议坚持使用 fp16/bf16 KV 或 Q8_0。混合 K/V 类型目前也会触发禁用 FA 的 30x 变慢兜底路径，同样值得留意。

4. **新架构落地的信号**：主线在 24 小时内合入了 Kimi-K3（b10448）和 MiniMax 系列（b10437），且 MTP 草稿模型可通过 `--models-dir` 自动发现（b10444）——这标志着 speculative decoding（MTP/NextN）将从“手动指定”走向“标准配置”，对依赖此能力优化首 token 延迟的 Agent 框架是好消息。

5. **ROCm 容器用户注意**：Docker 镜像的 ROCm 版本已随 PR #27145 升级至 7.14.0 / Ubuntu 26.04，如有自定义 GPU 白名单或驱动兼容性要求，需在镜像升级后重新验证。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 动态日报 2026-08-16

## 1. 今日速览

v0.32.14-rc0 发布，带来 WebP 图像转码和 Qwen 渲染器对非开头 system 消息的容错。社区焦点集中在 Qwen3.8 系列回归：system message 位置校验导致 Claude Code 集成 500、AMD gfx1200 加载失败、Apple Silicon 上 MTP 变体性能减半。API 兼容层多个修复 PR 已提交（temperature 覆盖、audio 字段静默丢弃、错误上下文保留），值得应用开发者关注。

## 2. 版本发布与破坏性变更

- **v0.32.14-rc0** 发布，包含两项变更：
  - `llm: transcode WebP images for llama-server` —— 多模态图像预处理改进，可能同时修复 minicpm-v 的 WebP SIGSEGV（#16162）
  - `renderers/qwen: tolerate non-leading system messages` —— 放宽 Qwen 渲染器对 system 消息必须位于开头的限制，直接回应今日多个 500 报错
  - 链接：https://github.com/ollama/ollama/compare/v0.32.13...v0.32.14-rc0

- 今日同步合并了 upstream llama.cpp 和 MLX 更新（PR #17760、#17761），无破坏性变更公告。

## 3. 新模型与硬件支持

- 官方仓库今日无新模型或新后端合并。
- 社区模型需求持续累积：DeepSeek V4 Pro 0813（[#17775](https://github.com/ollama/ollama/issues/17775)）、GLM 5.3（[#17741](https://github.com/ollama/ollama/issues/17741)）、Solar Pro 4（[#17773](https://github.com/ollama/ollama/issues/17773)）、deepseek-v4-flash 0731（[#17510](https://github.com/ollama/ollama/issues/17510)）均在等待官方接入。
- **架构对接缺口**：直接从 Hugging Face `ollama pull hf.co/...` 拉取 `qwen3moe` 架构（如 Qwen3-Coder-30B-A3B-Instruct）时，无法自动匹配内置的 `qwen3-coder` RENDERER/PARSER，导致模板和工具调用解析回退到通用逻辑。修复 PR [#17769](https://github.com/ollama/ollama/pull/17769) 正在审查中。

## 4. 性能与优化

- **进行中**：PR [#16161](https://github.com/ollama/ollama/pull/16161) 提议缓存 `GetModel()` 和 `Capabilities()`，避免每个推理请求都重新读取模型 manifest、重新解析 GGUF 元数据——即使模型已经加载在 GPU 显存中。作者估算每次请求可节省约 **300ms** 开销，对高 QPS 推理服务收益明显。该 PR 已持续数月，今日有更新。
- **可观测性**：PR [#11159](https://github.com/ollama/ollama/pull/11159) 在 `/metrics` 端点增加模型级评估指标（`ollama_eval_duration_total`、`ollama_eval_total` 等），基于 #6537 扩展。
- **调试体验**：PR [#17762](https://github.com/ollama/ollama/pull/17762) 修复 `OLLAMA_DEBUG_LOG_REQUESTS` 在 `c.Next()` 之后才打印日志的问题，改为请求处理前输出，对长耗时推理的现场观测有意义。

## 5. 稳定性与回归

按严重程度排列，标注修复进展：

- **🔴 Qwen3.8 system message 500**（[#17754](https://github.com/ollama/ollama/issues/17754)、[#17774](https://github.com/ollama/ollama/issues/17774)、[#17768](https://github.com/ollama/ollama/issues/17768)）：`ollama launch claude --model qwen3.8:27b` 和 `/v1/messages` 请求报 `500 system message must be at the beginning`，API 层强制校验 system 消息位置且未处理中途消息。两个修复路径已出现：v0.32.14-rc0 的渲染器容错 + PR [#17769](https://github.com/ollama/ollama/pull/17769) 架构自动检测。最高热度（👍 7）。
- **🔴 CUDA illegal memory access**（[#17434](https://github.com/ollama/ollama/issues/17434)）：DGX Spark（GB10 arm64）上 `qwen3.6:35b` 同时启用 JSON-schema format + `think:false` 时 100% 崩溃，修改任一条件即消失。尚无修复 PR。
- **🟠 AMD RDNA4 加载失败**（[#17782](https://github.com/ollama/ollama/issues/17782)）：RX 9060 XT（gfx1200）运行 Qwen3.8-27B 一段时间后报 `Could not load "TensileLibrary_lazy_gfx1200.dat"`，ROCm TensileLibrary 对新 GPU 的覆盖缺口。
- **🟠 Pascal GPU 回归**（[#17766](https://github.com/ollama/ollama/issues/17766)）：P6000/P4000 从 0.32.11 起无法工作，与官方 GPU 支持文档冲突。
- **🟠 Vulkan 崩溃**（[#17748](https://github.com/ollama/ollama/issues/17748)）：AMD Radeon 780M + Vulkan 后端在 0.32.11 上运行大模型报 `ErrorDeviceLost` / `Not enough memory for command submission`，同配置 0.32.10 正常。
- **🟠 Qwen3.8-27B MTP 性能减半**（[#17776](https://github.com/ollama/ollama/issues/17776)）：Apple Silicon 上 MTP 变体比同量化非 MTP 版本慢约 2 倍，官方未确认是 speculative path 缺陷还是预期行为。
- **🟡 minicpm-v WebP SIGSEGV**（[#16162](https://github.com/ollama/ollama/issues/16162)）：特定 WebP 图像使多模态 tokenizer 崩溃；v0.32.14-rc0 的 WebP 转码逻辑可能覆盖此问题。
- **🟡 /api/chat 静默丢弃 audio 字段**（[#17764](https://github.com/ollama/ollama/pull/17764)）：当前 audio 字段在 JSON 反序列化时被静默丢弃，模型在"没听见"的情况下生成看似合理的回答。修复后此类请求将返回 **400** 而非盲答。
- **🟡 Jetson 平台回归**：更新至 0.32.7 后模型文件全部消失（[#17661](https://github.com/ollama/ollama/issues/17661)）；0.32.2 起 gemma4:e2b/e4b 在 Orin Nano 上内存占用异常（[#17787](https://github.com/ollama/ollama/issues/17787)）。
- **🟡 Ollama.com 账户安全**（[#17682](https://github.com/ollama/ollama/issues/17682)）：修改密码/邮箱后旧会话不失效；另有 MFA 缺失的长期请求（[#16224](https://github.com/ollama/ollama/issues/16224)）。

## 6. 对应用开发者的意义

- **Claude Code + Qwen3.8 用户注意**：在 0.32.14 正式版发布前，避免在对话中途插入 system 消息；社区验证回退到 0.32.7 可规避该 500 问题（[#17700](https://github.com/ollama/ollama/issues/17700)）。
- **OpenAI 兼容端点存在 temperature 覆盖缺陷**：请求未显式传 `temperature` 时，兼容层会注入硬编码 1.0 覆盖 Modelfile 中的 `PARAMETER temperature`，修复 PR [#17763](https://github.com/ollama/ollama/pull/17763) 合并前请先显式传参。
- **多模态应用注意静默失败**：向 `/api/chat` 发送 audio 字段目前会被静默丢弃，模型会"无中生有"地作答。升级到包含 [#17764](https://github.com/ollama/ollama/pull/17764) 的版本后，此类请求会返回 400，反而更容易定位问题。
- **SillyTavern 与 text completion 用户**：0.32.7 以上版本存在 text completion 空响应回归（[#17700](https://github.com/ollama/ollama/issues/17700)），升级前建议安排回归测试；chat completion 不受影响。
- **调试基础设施**：`OLLAMA_DEBUG_LOG_REQUESTS` 将在请求处理前输出日志（[#17762](https://github.com/ollama/ollama/pull/17762)），长耗时推理的请求级观测将不再滞后。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 动态日报 — 2026-08-16

## 今日速览

昨日无新 Release，但提交了 3 个安全审计报告（预算绕过、SSRF/密钥泄露、无认证模式），其中两个为 Medium 级，相关修复方向需关注。PR 侧核心动作集中在批量任务成本核算、Ollama 远程 api_base 超时修复和 Guardrail 全链路扫描修复，另有多个稳定性修复进入合入/测试阶段。

## 版本发布与破坏性变更

无新版本发布。以下行为变更请关注：

- **停止向上游转发客户端 Accept-Encoding**（[PR #37058](https://github.com/BerriAI/litellm/pull/37058)，已合入）：Anthropic 会对非流式 JSON 做 brotli 压缩，而镜像内无 brotli 解码库，导致原始 brotli 字节被中继。本次修复后，若客户端依赖压缩响应感知网络字节数，需重新评估。
- **流式 usage.cost 不再无条件信任**（[PR #37060](https://github.com/BerriAI/litellm/pull/37060)）：泛化网关可能以非 USD 单位上报成本，此前该值可覆盖配置定价并错误消耗预算。修复后仅对已知 USD 供应商信任该值，OpenRouter 行为不变。使用自定义网关接流式响应的项目需检查计费是否符合预期。

## 新模型与硬件支持

- **vovoyage-code-4 嵌入模型**（[PR #36820](https://github.com/BerriAI/litellm/pull/36820)）：已加入成本与上下文窗口映射表，因尚未正式发布，临时引用 voyage-code-3 的参数。等待官方发布后需复核定价。

## 性能与优化

- **Legacy spend logs 分页**（[PR #37027](https://github.com/BerriAI/litellm/pull/37027)）：过去无过滤的支出日志会加载全部历史记录，过滤单条日志的路径也缺少数据库 limit。现已支持稳定 take/skip 分页，避免提取接口内存压力。
- **Ollama 远程 api_base 超时修复**（[PR #37062](https://github.com/BerriAI/litellm/pull/37062)，对应 [Issue #37041](https://github.com/BerriAI/litellm/issues/37041)）：此前每次 completion 会对 localhost:11434 发起约两次 4s TCP 连接超时，原因在于模型信息查询忽略了请求 api_base。修复后会向 Ollama 实际主机查询 /api/show，显著减少非默认主机场景的尾延迟。

## 稳定性与回归

按严重程度排列：

- **[安全] 非管理员可通过 temp_budget_increase 提升自身 max_budget**（[#37052](https://github.com/BerriAI/litellm/issues/37052)，CLOSED）：`/key/update` 端点未校验权限，密钥持有者可绕过预算上限。请核查线上版本是否受此影响。
- **[安全] 自定义 api_base 的 SSRF/密钥泄露风险**（[#37053](https://github.com/BerriAI/litellm/issues/37053)，CLOSED）：客户端自行提供 api_base 的防护校验被绕过，存在向任意地址转发 provider key 的风险。
- **[安全] 未设置 LITELLM_MASTER_KEY 时进入无认证模式**（[#37054](https://github.com/BerriAI/litellm/issues/37054)，CLOSED）：默认 docker-compose 未强制设置该变量，暴露后无鉴权。建议对照部署检查。
- **[数据库] LiteLLM_SpendLogs 缺少 (api_key, startTime) 索引**（[#35766](https://github.com/BerriAI/litellm/issues/35766)）：预算窗口 spend-reseed 触发全表扫描，在 2 vCPU RDS 上导致事务超时（P2028）。建议自行补充联合索引。
- **[数据丢失] Prisma 引擎启动竞态**（[#27704](https://github.com/BerriAI/litellm/issues/27704)）：滚动部署时 Uvicorn 早于 Prisma Query Engine 就绪，后台任务（spend 更新/凭据加载）失败可能造成支出数据丢失。
- **[回归] FastAPI get_flat_dependant 兼容性崩溃**（[#36922](https://github.com/BerriAI/litellm/issues/36922)）：uv tool update 后 v1.96.2 启动失败，影响所有 FastAPI 版本升级路径。预计需要锁定依赖或等待官方修复。
- **[回归] gpt-5.4 返回空 final Responses 输出**（[#25429](https://github.com/BerriAI/litellm/issues/25429)）：`chatgpt/gpt-5.4` 在 Responses API 桥接中返回空结果，且 completion() 桥接报 “Unknown items” 错误，复现率稳定。
- **[功能] Bedrock 托管批量任务无法取消**（[#33986](https://github.com/BerriAI/litellm/issues/33986)）：`POST /v1/batches/{id}/cancel` 未覆盖 bedrock（仅 openai/azure/vertex 支持），批量任务进入 InProgress 后无法取消。
- **[正确性] Anthropic messages[] 中 role:"system" 被静默丢弃**（[#36917](https://github.com/BerriAI/litellm/issues/36917)）：该消息不会透传到后端，若客户端依赖系统消息在对话中间变更，会出现行为缺失。
- **[计费] Gemini TTS 不记 spend**（[#37015](https://github.com/BerriAI/litellm/issues/37015)）：/v1/audio/speech 返回 200 但密钥花费保持 0，影响成本核算。
- **[计费] service_tier=priority 未正确计费**（[#37046](https://github.com/BerriAI/litellm/issues/37046)）：gpt-4o/4.1 系列使用 priority tier 时按默认费率计费，逐日期快照模型缺失对应价格键。

已有修复 PR 的条目：#37041 对应 [PR #37062](https://github.com/BerriAI/litellm/pull/37062)；批次成本重复计算已修复（[PR #37050](https://github.com/BerriAI/litellm/pull/37050)）；Guardrail 相关有三个修复（[#37038](https://github.com/BerriAI/litellm/pull/37038)、[#37036](https://github.com/BerriAI/litellm/pull/37036)、[#36894](https://github.com/BerriAI/litellm/pull/36894)），分别覆盖 PANW AIRS 工具调用报错、阻断响应字段丢失和 Azure Content Safety 未实际扫描的问题。

## 对应用开发者的意义

- **预算与安全**：若在共享环境运行代理，优先确认是否受影响于 #37052 预算提升、#37053 SSRF 及 #37054 无认证模式；这三个 issue 已关闭但未出正式安全公告，建议自行验证版本。
- **批量任务**：已修复成本重复核算的问题（[PR #37050](https://github.com/BerriAI/litellm/pull/37050)），使用 Bedrock 托管批量的用户仍无法通过 LiteLLM 取消任务，需评估替代管理通道。
- **Ollama 用户**：升级包含 [PR #37062](https://github.com/BerriAI/litellm/pull/37062) 的版本可消除每次 completion 的额外 8 秒超时，建议跟进合入发布。
- **响应压缩**：若客户端依赖上游压缩响应，注意代理版本更新后的透传行为变化。
- **流式计费**：来自非标准网关的流式 usage.cost 不再作为计费依据，需确保定价配置完整，避免预算数据被外部值干扰。
- **Guardrail 工程**：近期 PR 修复了多个 Guardrail 断裂场景（包括 apply_guardrail 端点空转、工具调用被误扫描、阻断响应信息不全），在严格审计环境可关注这些合入点。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 动态日报 — 2026-08-16

## 1. 今日速览

Unsloth 的社区重心正明显从训练内核转向 **Studio/Desktop 产品化**：大量 issue/PR 集中在模型管理器、API 暴露方式、桌面端媒体权限与图形界面细节。同时，多个与训练/推理正确性相关的 PR 已在今日提交（量化 KV cache 张量并行、GGUF 缓存加载、tool-call 路由），并附有实测数据。没有新版本 Release，但一个 GGUF 导出流程变更（要求 16bit 权重）和一个 torch 2.13 安全修复被依赖阻塞的问题值得注意。

## 2. 版本发布与破坏性变更

- **无新 Release**。过去 24 小时 unslothai/unsloth 没有发布新版本。
- **GGUF 导出流程变更引发争议（已关闭）**：用户反馈训练后导出 GGUF 现在必须先保存 16bit 全量权重，导致需要等待 40GB 下载，且无法直接导出。该 issue 已关闭，但属于近期行为变更，如你的流水线依赖「训练→直接转 GGUF」需要重新验证。 [#8717](https://github.com/unslothai/unsloth/issues/8717)
- **torch 2.13 安全修复（GHSA-rrmf-rvhw-rf47）被依赖约束阻塞**：当前 unsloth 发布的依赖固定范围会阻止 torch 2.13 安全更新，需要关注后续修复版本。 [#8926](https://github.com/unslothai/unsloth/issues/8926)

## 3. 新模型与硬件支持

- **Studio 将支持发现 oMLX 安装的模型（PR）**：macOS MLX runner oMLX 将模型存储在 `~/.omlx/models`，新增 PR 让 Studio 自动扫描该目录，扩大 macOS 本地模型来源。 [#8937](https://github.com/unslothai/unsloth/pull/8937)
- **社区已在跑新硬件/新模型**：Gemma-4-26B-A4B（#8483）、Qwen3.8-27B-GGUF（#8875）、Ideogram 4（#8940）被用户在实际环境中使用，但其中 Ampere 以下旧 GPU 仍会遇到「GPU too old」阻断，目前标记为 URGENT 修复中。 [#1998](https://github.com/unslothai/unsloth/issues/1998)
- **AMD/Intel GPU 仍是重灾区**：AMD VULKAN/ROCm 下 VRAM 识别错误、显存未被使用（#8878）；Intel GPU 只能通过 vulkan llama.cpp 跑核心、无法使用 Studio UI（#8931）；AMD iGPU 被报告 VRAM 虚高（#8942）。目前多为 open 状态，等待修复。

## 4. 性能与优化

- **max_steps 训练现在只需预处理实际用到的行（PR）**：修复前一颗 30 步的 Qwen3-0.6B 训练中，预处理 `unsloth/open_math_reasoning`（27GB）耗时 **11m14s**，而训练仅 **1m54s**。该 PR 可大幅缩短大数据集上的小步数训练等待时间。 [#8890](https://github.com/unslothai/unsloth/pull/8890)
- **GGUF 加载减少 Hub 重复往返（PR）**：加载已缓存 GGUF 时，单个请求此前会对同一文件做 7 次 Hub round-trip 验证；重构后复用缓存结果，显著降低模型加载延迟。 [#8771](https://github.com/unslothai/unsloth/pull/8771)
- **本地模型清单速度优化（PR）**：109 行模型库存冷启动耗时约 5s，且阻塞其他 API 请求超 4s；PR 移除了三处不必要的工作，避免 API 循环被卡。 [#8770](https://github.com/unslothai/unsloth/pull/8770)
- **部分 GPU offload 下 MTP 推理性能修复（PR）**：修复 Qwen3.8-27B-GGUF 在 UD-IQ2_M + 默认设置下仅有约 **3.5 token/s** 的问题，原因是 embedded MTP head 未跟随主模型放置。 [#8875](https://github.com/unslothai/unsloth/pull/8875)
- **流式输出渲染优化（PR）**：快速流式回复时，聊天 UI 容易在 250ms 刷新周期内堆积消息重建；PR 将同一帧内的多个 chunk 合并发布，降低前端开销。 [#8845](https://github.com/unslothai/unsloth/pull/8845)
- **长代码块增量 tokenize（PR）**：超过 2000 字符的流式代码块此前每次刷新都对全量代码重新做 Shiki tokenize，PR 改为复用已高亮行，仅处理新增尾部。 [#8935](https://github.com/unslothai/unsloth/pull/8935)

## 5. 稳定性与回归

按严重程度排列，标注修复状态：

- **[严重] 训练崩溃（无 fix PR）**：`unsloth/Qwen3-0.6B-unsloth-bnb-4bit` 在 Colab T4 上训练时稳定触发 `RuntimeError: PassManager::run failed`，已有 18 条评论，是目前最活跃的训练阻断问题。 [#2482](https://github.com/unslothai/unsloth/issues/2482)
- **[严重] 旧 GPU 硬阻断（标记 URGENT 修复中）**：`Unsloth: Your GPU is too old!` 报错让 Maxwell/Pascal 等用户无法启动；官方已标记为「currently fixing」。 [#1998](https://github.com/unslothai/unsloth/issues/1998)
- **[中] Ideogram 4 在 Mac 上无法加载**：Studio 桌面版报 `'_Noop' object is not iterable`，新反馈，暂无 PR。 [#8940](https://github.com/unslothai/unsloth/issues/8940)
- **[中] Deep Research 卡死在「Writing The Report」**：Gemma-4-26B-A4B 运行时 Deep Research 冻结，且取消后无法查看 token 用量。 [#8483](https://github.com/unslothai/unsloth/issues/8483)
- **[中] 断点续传按钮失效**：「Partial Download. Click to continue.」点击无反应，用户无法恢复损坏下载。 [#8927](https://github.com/unslothai/unsloth/issues/8927)
- **[中] CUDA_VISIBLE_DEVICES 使用 UUID 时 GPU 选择器消失**：多 GPU 主机上会静默隐藏 per-model GPU 选择器。 [#8873](https://github.com/unslothai/unsloth/issues/8873)
- **[中] 视频 GGUF/扩散模型下载后误报传输错误**：模型已下载完成但仍错误提示「earlier partial download used a different transport」，该 issue 已被关闭。 [#8941](https://github.com/unslothai/unsloth/issues/8941)
- **[中] 麦克风权限被桌面端拦截**：Ubuntu 下 `.deb` 安装的 Unsloth Desktop 无法调用语音输入，疑似 WebKitGTK 未启用 media-stream。 [#8678](https://github.com/unslothai/unsloth/issues/8678)
- **[低] 量化 KV cache 在 tensor parallel 下被静默丢弃（已有 fix PR）**：`load_model` 对非 f16/bf16/f32 的 KV cache 类型在 TP 开启时直接移除并降级，PR #8939 修复。 [#8939](https://github.com/unslothai/unsloth/pull/8939)
- **[低] Windows 链接文件夹二进制损坏（已有 fix PR）**：`folder_sync._snapshot` 未用 `O_BINARY` 打开文件，导致 CRLF 被折叠、文件截断；PR #8621 修复。 [#8621](https://github.com/unslothai/unsloth/pull/8621)
- **[低] macOS AppleDouble 文件被识别为 GGUF（已有 fix PR）**：exFAT 等卷上的 `._` 元数据侧车文件被模型库扫描误认为 GGUF，PR #8919 修复。 [#8919](https://github.com/unslothai/unsloth/pull/8919)

## 6. 对应用开发者的意义

- **Tool-call 路由正确性修复（两个 PR）**：一个修复多轮 tool-call 中 `delta.tool_calls[].index` 重置导致的参数流串线（#8754）；另一个修复裸 tool-call 片段被错误路由到错误调用槽（#8755）。若你的 Agent 应用重度依赖 OpenAI 兼容流式 tool-call，这两个修复值得优先跟进。 [#8754](https://github.com/unslothai/unsloth/pull/8754) [#8755](https://github.com/unslothai/unsloth/pull/8755)
- **媒体 API 将支持自动模型切换（PR）**：此前 `/v1/images/generations` 在没有手动选模型时返回 503 且 `model` 字段不可选；PR 将 chat API 的 `auto_switch_model` 行为扩展到图像/视频 API。 [#8766](https://github.com/unslothai/unsloth/pull/8766)
- **局域网/0.0.0.0 监听需求持续升温**：多用户要求 Unsloth Desktop/Studio 的 API 支持监听 `0.0.0.0` 并提供 LAN 直连配置，避免强制走 Cloudflare 隧道。目前仍是 feature request，暂无实现 PR，但 👍 数较高，值得关注。 [#8578](https://github.com/unslothai/unsloth/issues/8578) [#8898](https://github.com/unslothai/unsloth/issues/8898) [#8934](https://github.com/unslothai/unsloth/issues/8934)
- **Qwen3 推理缺少 reasoning effort 控制**：Studio 聊天界面未暴露 reasoning effort 滑块，用户只能手动改 Jinja 模板。对于构建需要「快速 vs 深度推理」切换的 Agent，这是一个可预见的集成痛点。 [#8881](https://github.com/unslothai/unsloth/issues/8881)
- **工具生态在扩展**：社区在请求 Filesystem 工具（#8915）、日期注入到 system prompt（#8859）、compaction/rolling context（#7472）。同时新 PR 允许把已保存 prompt 作为 system prompt 使用，并为每天从 Cursor/Claude Code 迁移到 Studio 的开发者提供聊天记录一键导入。 [#8816](https://github.com/unslothai/unsloth/pull/8816) [#8561](https://github.com/unslothai/unsloth/pull/8561)

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*