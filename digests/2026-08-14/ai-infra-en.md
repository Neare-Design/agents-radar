# AI Infrastructure Digest 2026-08-14

> Generated: 2026-08-13 23:34 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project AI Infrastructure Comparison — 2026-08-14

## 1. Ecosystem Overview

The AI infrastructure ecosystem is in a familiar pattern: rapid frontier-model enablement is colliding with upgrade instability. DeepSeek-V4, Kimi-K3, Qwen3.5, Gemma4, and MiniMax variants dominate engineering time across vLLM, SGLang, and llama.cpp. Serving engines are focusing on sparse MLA/attention kernels, speculative-decoding overhead, and KV-cache reuse, while local runtime projects are shipping dense release trains and desktop/agent integration work. At the same time, several severe multi-node and version-upgrade regressions are forcing operators to pin versions carefully. The overall posture is “fast feature convergence, but stability is the bottleneck.”

## 2. Activity Comparison

Counts are notable issues/PRs referenced in today’s digests, not full GitHub totals.

| Project | Issues referenced | PRs referenced | Release status |
|---|---:|---:|---|
| vLLM | ~19 | ~14 | No new release; v0.27.x under regression pressure |
| SGLang | ~13 | ~12 | No new release; `main` blocked for multimodal embeddings |
| llama.cpp | ~11 | ~20 | Dense release train: `b10411` → `b10423` |
| Ollama | ~18 | ~13 | No new release; regression in llama3.3:70b since v0.32.2 |
| LiteLLM | ~10 | ~14 | `v1.98.0-dev.2` with cosign image verification |
| Unsloth | ~26 | ~8 | `v0.1.702-beta` — Unsloth Desktop launch |

## 3. Model Support Race

- **vLLM** is the most advanced on serving frontier models: DeepSeek-V4 sparse MLA now works end-to-end on SM120 Blackwell, including MTP and DSpark modes; Kimi-K3 ROCm FP8 MLA prefill and decode-context partial prefix cache landed; Gemma4-E2B heterogeneous per-layer config is handled.
- **SGLang** is close behind on DeepSeek-V4 and Kimi-K3, with active TRT-LLM DSv4 attention integration for Blackwell SM100/103, an AMD Radix-4 MoE router for Kimi-K3, miniMax-M3 IndexCache, and DSpark decode overhead fixes.
- **llama.cpp** is the fastest-moving local runtime: OpenVINO gained Qwen3.5/gpt-oss/MXFP4, Metal added TQ2_0, and PRs propose Kimi-K3, MiniMax-Text-01/M1, and LFM2 tensor-split support.
- **Ollama** is dependent on llama.cpp/MLX but is closing feature gaps: MLX structured-output support is in review, and Nemotron MLX vision plus new `ollama launch` integrations for Muse Code and DeepSeek Harness landed.
- **LiteLLM** is not competing on model execution; its support work is cost-map and request-metadata correctness: Meta Muse Spark 1.2 and Voyage 4 embeddings were added.
- **Unsloth** is mostly a Desktop/fine-tuning layer; MiniMax-H3 appears in the UI but is not reliably runnable yet.

**Who is ahead:** vLLM and SGLang are leading the serving race for DeepSeek-V4/Kimi-K3-class models. llama.cpp is leading local/edge architecture breadth. Ollama is a fast follower via backend consolidation.

## 4. Performance Frontier

Optimization effort is concentrated in four areas:

- **KV-cache and context reuse** — vLLM landed DeepSeek-V4 IndexCache and Kimi-K3 partial-prefix reuse; SGLang proposed MiniMax-M3 IndexCache and is pursuing programmatic/position-independent KV-cache reuse for agentic workloads; Ollama fixed KV-cache accounting for scheduler VRAM prediction.
- **Speculative decoding** — vLLM has multiple open MTP/DSD regressions and an RFC to remove heuristic speculative-method detection; SGLang is actively cutting DSpark decode-step overhead (e.g. removing 21 syncs / ~71 ms); llama.cpp added MTP auto-detection from GGUF metadata and backend sampling for DFlash/DSpark.
- **Kernel/quantization work** — vLLM consolidated ModelOpt LinearMethod classes for FP8/NVFP4/MXFP8 and fixed FP8 sparse MLA on Blackwell; llama.cpp vectorized CPU flash-attention V-cache conversion and optimized Metal TQ2_0 kernels; SGLang is rewriting the DeepSeek-V4 paged MQA metadata kernel.
- **Distributed serving stability** — vLLM has a critical 4-node GB10 idle-stall bug; SGLang has a DeepSeek-V4 + DSpark 2-node deadlock; llama.cpp is fixing backend-split scheduler races and Windows hybrid-core scheduling.

## 5. Layer Positioning

- **vLLM and SGLang** are production serving engines: high-throughput, tensor/pipeline parallel, speculative decoding, quantization, and CUDA/ROCm kernel ownership.
- **llama.cpp** is the local/edge runtime: quantized GGUF support, broad CPU/GPU backend coverage (Metal, SYCL, OpenVINO, Vulkan, CUDA, ROCm), and a rapid server/observability release cadence.
- **Ollama** is the local/desktop developer runtime and API layer: it wraps backend runtimes, adds model management, MLX support, `ollama launch` agent integrations, and a simpler `/v1/`-style API.
- **LiteLLM** is the gateway/control plane: model routing, auth/access control, spend tracking, MCP session handling, and provider cost-map correctness.
- **Unsloth** is the fine-tuning/training layer: efficient fine-tuning, GGUF/MLX export, desktop training/serving, and tool-calling for external providers.

These layers are converging: serving engines are adding more local-friendly features, local runtimes are adding server APIs and observability, and gateways are absorbing agent/MCP and auth concerns.

## 6. Trend Signals

- **Frontier-model enablement is the main driver.** DeepSeek-V4, Kimi-K3, Qwen3.5, and Gemma4 are consuming most kernel and model-support work. Sparse attention, MLA, IndexCache, and MTP are now first-class optimization targets.
- **Speculative decoding is still the least stable performance feature.** MTP crashes, DSD throughput collapse, and wrong-output-under-PP errors mean spec-decode configurations need rigorous A/B validation. Expect breaking config changes if vLLM’s RFC to require explicit method selection merges.
- **Multi-node serving is fragile.** vLLM’s GB10 idle-stall and SGLang’s NCCL deadlock are production-blocking. Health checks must exercise generation, not just API liveness.
- **Agentic workloads are shaping roadmaps.** KV-cache reuse, structured output, tool-calling correctness, MCP OAuth persistence, and access-control revocation are all getting dedicated fixes. Application developers should watch these areas for near-term latency and correctness wins.
- **Local runtime maturity is accelerating.** llama.cpp’s release train, Ollama’s MLX grammar support, and Unsloth Desktop signal that local-first inference is becoming production-relevant for small/edge deployments.
- **Operational hygiene is improving.** LiteLLM added cosign image signing, OpenAI-style access-group revocation, and cost-map corrections. Observability is also improving, e.g. llama.cpp exposing `/metrics` and `/slots` during `llama_decode()`.

**Actionable for developers:** pin known-good versions when serving DeepSeek-V4/Gemma4/llama3.3; validate speculative decoding before production; do not trust silent APIs (e.g. Ollama audio dropping); and plan for MLX structured-output support to change Apple Silicon behavior once merged.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-14

## 1. Today's Highlights

The v0.27.x line is showing upgrade friction: DeepSeek-V4-Flash fails after upgrading from 0.26.0 to 0.27.0 ([#51758](https://github.com/vllm-project/vllm/issues/51758)), the latest `vllm-openai` image cannot load Gemma4 with Transformers 5.15.0 ([#51744](https://github.com/vllm-project/vllm/issues/51744)), and a severe multi-node stall (requests never reaching the scheduler) was reported on 4-node TP=4 GB10 systems ([#51921](https://github.com/vllm-project/vllm/issues/51921)). On the positive side, the DeepSeek-V4/Kimi-K3 ecosystem continues to mature: a large fix makes DSV4 sparse MLA work end-to-end on SM120 Blackwell ([#51538](https://github.com/vllm-project/vllm/pull/51538)), and ROCm-specific FP8 MLA prefill support for Kimi-K3 non-divisor head counts is ready ([#51040](https://github.com/vllm-project/vllm/pull/51040)). Speculative decoding remains the least stable surface area, with four separate issues around MTP/DSD correctness and performance regressions.

## 2. Releases & Breaking Changes

No new releases in the last 24h. However, several upgrade-related regressions are worth flagging:

- **v0.26.0 → v0.27.0 breaks DeepSeek-V4-Flash** with a flash-attention error ([#51758](https://github.com/vllm-project/vllm/issues/51758)) — 17 comments, 0👍.
- **`vllm/vllm-openai:latest` (v0.27.0) fails to start Google Gemma4** 31B QAT NVFP4 when the bundled Transformers is 5.15.0 ([#51744](https://github.com/vllm-project/vllm/issues/51744)) — 14 comments, 5👍.
- **v0.27.0 engine permanently stalls after ~1 minute idle** on 4-node TP=4 (GB10/sm_121, aarch64); `shm_broadcast` writer starves and requests never enter the scheduler ([#51921](https://github.com/vllm-project/vllm/issues/51921)) — 11 comments.
- PR **[#51338](https://github.com/vllm-project/vllm/pull/51338) (RFC)** proposes removing heuristic speculative-method guessing (path/name substrings like `eagle3`, `dflash`, `dspark`) and requiring explicit method selection — a breaking behavior change for users relying on auto-detection.

## 3. New Model & Hardware Support

- **[DeepSeek-V4 sparse MLA end-to-end](https://github.com/vllm-project/vllm/pull/51538)** ([#51538](https://github.com/vllm-project/vllm/pull/51538)): fixes seven defects blocking DSV4-Flash-0731 on the SM120 sparse MLA backend across plain decode, MTP, and DSpark mode; verified on 8×RTX PRO 6000 Blackwell.
- **[DeepSeek-V4 IndexCache](https://github.com/vllm-project/vllm/pull/51209)** ([#51209](https://github.com/vllm-project/vllm/pull/51209)): adds DSA IndexCache so C4A layers reuse top-k indices from the previous layer; validated on V4-Flash-0731 with DSpark acceleration.
- **[Kimi-K3 ROCm enablement](https://github.com/vllm-project/vllm/issues/50682)** ([#50682](https://github.com/vllm-project/vllm/issues/50682)): upstream tracking issue for feature enablement and perf on ROCm; companion PR [#51040](https://github.com/vllm-project/vllm/pull/51040) extends AITER FP8 asm MLA prefill to non-divisor head counts (96 heads / kv_lora_rank=512 → 12 heads/rank at TP8).
- **[Kimi-K3 DCP partial prefix cache hit](https://github.com/vllm-project/vllm/pull/50493)** ([#50493](https://github.com/vllm-project/vllm/pull/50493)): hash-aligned partial-prefix reuse under decode context parallelism; also fixes MRV2 block-table geometry for cache groups.
- **[Gemma4-E2B heterogeneous per-layer config support](https://github.com/vllm-project/vllm/pull/52206)** ([#52206](https://github.com/vllm-project/vllm/pull/52206)): handles transformers 5.x per-layer `head_dim` (not a flat global attribute) which currently breaks model load.
- **[Cosmos3-Edge encoder CUDA graphs](https://github.com/vllm-project/vllm/pull/52229)** ([#52229](https://github.com/vllm-project/vllm/pull/52229)): exact-budget image-encoder CUDA graphs for the 27-layer SigLIP2 vision encoder (eager path launches 281 kernels per 64-token image).
- **RDT sharded weight sync** ([#43375](https://github.com/vllm-project/vllm/pull/43375)): WIP prototype for RDT sharded weight transfer.
- **Regression risk — Intel Arc B60 (XPU)**: all GPTQ checkpoints fail with `UR_RESULT_ERROR_DEVICE_LOST` at `profile_run` ([#52203](https://github.com/vllm-project/vllm/issues/52203)); environment is v0.27.2.dev + oneAPI 2026.0 + vllm-xpu-kernels 0.1.12.

## 4. Performance & Optimization

**Landed / ready:**

- **DSV4 sparse MLA** ([#51538](https://github.com/vllm-project/vllm/pull/51538)): unblocks FP8 sparse MLA on Blackwell across plain decode, MTP, and DSpark.
- **Kimi-K3 ROCm FP8 MLA prefill** ([#51040](https://github.com/vllm-project/vllm/pull/51040)): removes the `num_heads % 16 == 0` gate that forced BF16 FMHA decompress fallback at TP8.
- **Eliminate two GPU→CPU syncs** ([#42850](https://github.com/vllm-project/vllm/pull/42850)): removes `.max().item()` and `.sum().item()` blocking calls in `make_kv_sharing_fast_prefill_common_attn_metadata`.
- **ModelOpt LinearMethod consolidation** ([#49381](https://github.com/vllm-project/vllm/pull/49381)): replaces six near-duplicate LinearMethod classes (FP8 variants, NVFP4 W4A4/W4A16, MXFP8) with one generic QuantKey-driven implementation.
- **DeepSeek-V4 IndexCache** ([#51209](https://github.com/vllm-project/vllm/pull/51209)): reuses top-k indices across C4A layers to accelerate DSpark serving.

**Open performance concerns:**

- **Qwen3.5 native MTP slower than no-MTP CUDA graph baseline** ([#47277](https://github.com/vllm-project/vllm/issues/47277)): despite 82–88% MTP1 acceptance, OCR workload throughput regresses; 9 comments.
- **Dynamic speculative decoding (DSD) catastrophic throughput collapse at batch-size threshold** under concurrency with MTP k=2; the documented FULL_AND_PIECEWISE → PIECEWISE cudagraph downgrade costs ~14% single-stream ([#49548](https://github.com/vllm-project/vllm/issues/49548)).
- **DSD arms pay a large baseline tax vs no-spec** under production defaults; PIECEWISE override identified as one factor ([#49986](https://github.com/vllm-project/vllm/issues/49986)).
- **ViT full CUDA graph tracker** ([#38175](https://github.com/vllm-project/vllm/issues/38175)): RFC to eliminate the large number of launch-bound kernels in ViT encoders for Qwen3-VL, GLM-V, Kimi K2.5; 27 comments.
- **Fully async spec-decoding** ([#29134](https://github.com/vllm-project/vllm/issues/29134)): blocked by two host↔GPU syncs when computing `seq_lens_cpu`.
- **Qwen3.5 27B prefix caching** ([#38988](https://github.com/vllm-project/vllm/issues/38988)): users report prefix caching is unusable with Qwen3.5; 4👍.
- **Context-length-aware speculative token scheduling RFC** ([#48627](https://github.com/vllm-project/vllm/issues/48627)): extends `num_speculative_tokens_per_batch_size` with a `(batch, ctx)` K-selection table.

## 5. Stability & Regressions

Ranked by severity:

1. **v0.27.0 engine permanent stall on 4-node TP=4 (GB10/aarch64)** ([#51921](https://github.com/vllm-project/vllm/issues/51921)) — total availability loss after ~1min idle; `shm_broadcast` writer starves, requests never reach scheduler. No fix PR yet. *Critical.*
2. **DeepSeek-V4-Flash regression on 0.26.0→0.27.0 upgrade** ([#51758](https://github.com/vllm-project/vllm/issues/51758)) — 17 comments, no fix PR referenced. *High.*
3. **MTP speculative decoding illegal memory access on long sequences** (Qwen3.6-27B-FP8, `num_spec_tokens=5`, v0.19.1) ([#40756](https://github.com/vllm-project/vllm/issues/40756)) — 36 comments, 6👍. Related: `cudaErrorIllegalAddress` in `gdn_attn.py:237` with `qwen3_next_mtp` under load ([#37035](https://github.com/vllm-project/vllm/issues/37035)). *High.*
4. **Gemma4 startup failure with Transformers 5.15.0 in official image** ([#51744](https://github.com/vllm-project/vllm/issues/51744)) — 14 comments, 5👍; user-provided model `yasu-oh/gemma-4-31B-it-qat-NVFP4`. *High.*
5. **Decode context parallelism output drift/gibberish** in v0.21.0 and latest nightly (`--decode-context-parallel-size`) ([#41623](https://github.com/vllm-project/vllm/issues/41623)) — 20 comments. *High.*
6. **Speculative decoding under pipeline parallelism produces wrong output** with `--no-async-scheduling` (nightly, PP 2/4/8, two methods, two model families) ([#52071](https://github.com/vllm-project/vllm/issues/52071)); fix PR [#52117](https://github.com/vllm-project/vllm/pull/52117) removes the erroneous `SupportsPP` requirement for MTP draft models under PP. *High.*
7. **GPT-OSS multi-turn HarmonyError** — `unexpected tokens remaining in message header` with gpt-oss-120b on v0.10.1/v0.10.1.1 ([#23567](https://github.com/vllm-project/vllm/issues/23567)) — 47 comments, 22👍. Fix PR [#51020](https://github.com/vllm-project/vllm/pull/51020) aligns strict tool-call grammar with real Harmony renders. *Medium — long-running.*
8. **Microsoft's `supports_sparse_mla` fix for DSV4 in plain decode, MTP, DSpark** ([#51538](https://github.com/vllm-project/vllm/pull/51538)) — addresses multiple blocking defects on SM120. *Medium.*
9. **FA4 disabled for head-dim 256 on Blackwell** ([#52050](https://github.com/vllm-project/vllm/pull/52050)) — SM100 2-CTA kernel rejects `seqused_q/k`; temporarily falls back to FA2 until upstream adds support.
10. **SpeculativeConfig `draft_model` cannot load mixed-precision compressed-tensors checkpoints** (`config_groups`) ([#49893](https://github.com/vllm-project/vllm/issues/49893)). *Low.*
11. **Intel Arc B60 (XPU) GPTQ device-lost** ([#52203](https://github.com/vllm-project/vllm/issues/52203)) — new, 5 comments. *Low (hardware-specific).*
12. **Mooncake KV connector Mamba boundary-state persistence bug** ([#51358](https://github.com/vllm-project/vllm/pull/51358)) — saves exact core-selected boundary blocks, prevents null/stale-state persistence. *Low.*

## 6. What This Means for Application Developers

- **Pin versions carefully if you serve DeepSeek-V4 or Gemma4.** The v0.27.0 line has two open blockers ([#51758](https://github.com/vllm-project/vllm/issues/51758), [#51744](https://github.com/vllm-project/vllm/issues/51744)); if you rely on either model, validate against 0.27.0 before upgrading, and consider staying on 0.26.x until fixes land.
- **Multi-node TP deployments on Grace Blackwell (GB10) are at risk.** The 4-node TP=4 idle-stall bug ([#51921](https://github.com/vllm-project/vllm/issues/51921)) means an idle engine can silently stop accepting requests while still answering `/v1/models` — add health-check coverage that exercises actual generation, not just API liveness, if you operate in this mode.
- **Speculative decoding is still frontier territory.** MTP crashes ([#40756](https://github.com/vllm-project/vllm/issues/40756)), wrong outputs under PP ([#52071](https://github.com/vllm-project/vllm/issues/52071)), and DSD throughput collapse ([#49548](https://github.com/vllm-project/vllm/issues/49548)) mean spec-decode configs need rigorous A/B validation before production. Expect a breaking change if the RFC to require explicit speculative methods ([#51338](https://github.com/vllm-project/vllm/pull/51338)) merges — plan to stop relying on path-based auto-detection for EAGLE/DFlash/DSpark checkpoints.
- **GPT-OSS tool calling remains fragile.** The 47-comment HarmonyError thread ([#23567](https://github.com/vllm-project/vllm/issues/23567)) plus the strict-grammar mismatch fix ([#51020](https://github.com/vllm-project/vllm/pull/51020)) indicate that if you serve GPT-OSS with forced tool calling, pin versions and pin a known-good Harmony parser version.
- **Kimi-K3 and DSV4 enablement is accelerating on both CUDA and ROCm.** If you are on ROCm/AMD, the K3 tracking issue ([#50682](https://github.com/vllm-project/vllm/issues/50682)) and DSV4 ROCm checklist ([#41820](https://github.com/vllm-project/vllm/issues/41820)) are the items to watch before committing AMD capacity.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-14

## Today's Highlights

No new releases landed in the past 24 hours. Engineering activity is concentrated on DeepSeek V4/Kimi K3 enablement, DSpark speculative-decoding performance, and KV-cache reuse for agentic workloads. The most immediately impactful user-facing bug is a crash on all multimodal embedding requests, with a fix currently open in [#34769](https://github.com/sgl-project/sglang/pull/34769).

## Releases & Breaking Changes

None in the last 24 hours. No new versions, API changes, or migration notes to report.

## New Model & Hardware Support

- **DeepSeek V4 remains the top roadmap item**: functional tracking in [#23602](https://github.com/sgl-project/sglang/issues/23602), NVIDIA-only performance tracking in [#33636](https://github.com/sgl-project/sglang/issues/33636), and ongoing Blackwell SM100/103 work to integrate TRT-LLM DSv4 attention ([#30805](https://github.com/sgl-project/sglang/pull/30805)).
- **Kimi K3 roadmap active** ([#32607](https://github.com/sgl-project/sglang/issues/32607)), with an AMD-focused Radix-4 MoE top-k router kernel for Kimi-K3 routing in [#34490](https://github.com/sgl-project/sglang/pull/34490).
- **Qwen3.5 MTP + HiCache startup** fix: language-model attributes were being read from the wrong config location; see [#34560](https://github.com/sgl-project/sglang/pull/34560).
- **MiniMax-M3 decode IndexCache** for sparse block selection is proposed in [#34583](https://github.com/sgl-project/sglang/pull/34583).
- **Flux2 diffusion**: fused `SiluAndMul` activation replaces separate silu/mul launches ([#34785](https://github.com/sgl-project/sglang/pull/34785)).
- MI300X ROCm Docker image tag updated to `v0.5.17-rocm700-mi30x-20260813` ([#34770](https://github.com/sgl-project/sglang/pull/34770)).

## Performance & Optimization

- **DSpark decode-step overhead cut**: `num_token_non_padded` was being built with a pageable host tensor, forcing 21 `cudaStreamSynchronize` calls totaling ~71.4 ms in a profile. The PR makes the H2D copy non-blocking ([#34782](https://github.com/sgl-project/sglang/pull/34782)).
- **MiniMax-M3 decode path**: the lightning indexer + sparse-attn bucket costs ~3.5 ms of a ~16 ms decode step (~22%) on MI350x/TP4; an IndexCache is proposed to reuse sparse block selection ([#34583](https://github.com/sgl-project/sglang/pull/34583)).
- **DeepSeek V4 paged MQA metadata kernel** optimization: the current single-block kernel has a serial Phase 3 costing `O(bs)` dependent smem loads; a JIT-kernel rewrite is in progress ([#25855](https://github.com/sgl-project/sglang/pull/25855)).
- **DSpark EP1 decode regression**: an unused CUDA scalar was being allocated on every speculative decode step for EP size 1; now conditional ([#34759](https://github.com/sgl-project/sglang/pull/34759)).
- **DSV4 nonpaged indexer**: trivial Top-K rows no longer compute unused logits via DeepGEMM ([#33857](https://github.com/sgl-project/sglang/pull/33857), closed).
- **Open question**: should TRT-LLM allreduce fusion accumulate in FP32 like MNNVL backends? ([#34603](https://github.com/sgl-project/sglang/issues/34603)).

## Stability & Regressions

Ranked by severity:

- **Multi-node TP deadlock**: DeepSeek-V4 + DSpark on 2× DGX Spark (GB10) can wedge with one rank stuck in NCCL proxy append and the peer idle at request broadcast. Open, no fix PR yet ([#33289](https://github.com/sgl-project/sglang/issues/33289)).
- **ROCm MI355 HiCache broken**: realistic agentic workloads show poor performance; open ([#34611](https://github.com/sgl-project/sglang/issues/34611)).
- **Multimodal embedding requests crash on `main`**: `AttributeError: 'EmbeddingReqInput' object has no attribute 'mm_content_hashes'`; fix PR open ([#34769](https://github.com/sgl-project/sglang/pull/34769)).
- **DSV4 SWA/compressed RoPE correctness**: SWA RoPE is now kept unscaled while a separate YaRN-scaled table is used for compressed paths; fix PR open ([#34788](https://github.com/sgl-project/sglang/pull/34788)).
- **DSpark CUDA graph geometry mismatch**: compact ragged CUDA graphs use incompatible request-slot geometry for the same token tier. Open ([#34384](https://github.com/sgl-project/sglang/issues/34384)).
- **Paged KV allocator OOM ordering**: allocation kernels launch before OOM is checked. Open ([#34399](https://github.com/sgl-project/sglang/issues/34399)).
- **Diffusion CPU-offload dropped on fallback**: native fallback loading silently loses all CPU-offload decisions, causing OOM on 8 GB GPUs. Open ([#34772](https://github.com/sgl-project/sglang/issues/34772)).
- **Hybrid-Mamba + NEXTN TypeError**: `mamba_next_track_idx is None` during target verify; issue closed ([#34787](https://github.com/sgl-project/sglang/issues/34787)).
- **Rust model gateway protocol drift**: `/v1/responses` rejects tool type `"custom"`, breaking OpenAI Codex CLI compatibility ([#30781](https://github.com/sgl-project/sglang/issues/30781)).
- **CI health**: scheduled CI on `main` currently reports 3 broken and 11 flaky tests ([#17050](https://github.com/sgl-project/sglang/issues/17050)).

## What This Means for Application Developers

- **If you self-host from `main` and use multimodal embeddings, upgrade is blocked until [#34769](https://github.com/sgl-project/sglang/pull/34769) merges.**
- **DeepSeek-V4 + speculative decoding on multi-node setups**: DSpark decode overhead is actively being reduced, but the intermittent multi-node TP deadlock ([#33289](https://github.com/sgl-project/sglang/issues/33289)) means production users should pin known-good versions and test failover.
- **OpenAI Responses API compatibility is not guaranteed**: the Rust gateway is out of sync with Python protocol handling for custom tool types. If you rely on Codex-style clients, account for possible rejection of `type: "custom"` ([#30781](https://github.com/sgl-project/sglang/issues/30781)).
- **Agentic/RAG workloads should watch KV-cache reuse RFCs**: [#27574](https://github.com/sgl-project/sglang/issues/27574) (programmatic KV cache) and [#30928](https://github.com/sgl-project/sglang/issues/30928) (position-independent reuse) both target the byte-identical, same-offset limitations of RadixAttention. This is where the near-term agentic latency wins will come from.
- **ROCm users on MI355**: HiCache is currently unreliable for agentic workloads; MI300X users now have a newer ROCm 7.0 Docker tag ([#34770](https://github.com/sgl-project/sglang/pull/34770)).

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

## llama.cpp Digest — 2026-08-14

### 1. Today's Highlights

A dense 24-hour release train (`b10411` → `b10423`) landed CPU parameter handling across tools, OpenVINO Qwen3.5/gpt-oss/MXFP4 enablement, SYCL pinned-host memory, Metal TQ2_0 kernels, and speculative-decoding auto-detection for local MTP draft models. On the PR side, the project is moving forward on new architectures (Kimi-K3, MiniMax, LFM2 tensor split) and making the server more observable by allowing `/metrics` and `/slots` access during `llama_decode()`.

---

### 2. Releases & Breaking Changes

**No major breaking changes were identified.** The only behavior change worth noting is the server UI cache fix in `b10416`: `index.html` is no longer served with an immutable cache header, so the web UI will now revalidate via ETag.

- [`b10423`](https://github.com/ggml-org/llama.cpp/releases/tag/b10423) — `common: apply CPU parameters across tools (#27026)`
- [`b10419`](https://github.com/ggml-org/llama.cpp/releases/tag/b10419) — OpenVINO: Qwen3.5, memory optimization, gpt-oss MoE, MXFP4, FILL op support (#26952)
- [`b10418`](https://github.com/ggml-org/llama.cpp/releases/tag/b10418) — SYCL host pinned memory support for faster Host-to-Device transfer (#26789)
- [`b10417`](https://github.com/ggml-org/llama.cpp/releases/tag/b10417) — Chat: fix LFM2 tool-call argument name prefix ambiguity (#26960)
- [`b10416`](https://github.com/ggml-org/llama.cpp/releases/tag/b10416) — Server: serve `index.html` with no-cache; prevents stale UI after upgrades (#27006)
- [`b10415`](https://github.com/ggml-org/llama.cpp/releases/tag/b10415) — Spec: auto-detect MTP draft model type (#27005)
- [`b10414`](https://github.com/ggml-org/llama.cpp/releases/tag/b10414) — Metal: add TQ2_0 ternary 2-bit type support (#26980)
- [`b10413`](https://github.com/ggml-org/llama.cpp/releases/tag/b10413) — Common: auto-detect spec type from local draft GGUF metadata (#26814)
- [`b10412`](https://github.com/ggml-org/llama.cpp/releases/tag/b10412) — Spec: enable backend sampling for DFlash and DSpark (#26958)
- [`b10411`](https://github.com/ggml-org/llama.cpp/releases/tag/b10411) — ggml-cpu: vectorize flash-attention V-cache F16→F32 conversion (#26947)

---

### 3. New Model & Hardware Support

- **OpenVINO backend** now supports Qwen3.5, gpt-oss MoE, and MXFP4 data, plus FILL op coverage.  
  https://github.com/ggml-org/llama.cpp/pull/26952
- **SYCL backend** adds host pinned memory, improving host-to-device transfer efficiency.  
  https://github.com/ggml-org/llama.cpp/pull/26789
- **Metal backend** adds TQ2_0 (ternary, 2-bit) quantization support.  
  https://github.com/ggml-org/llama.cpp/pull/26980
- **Speculative decoding**: MTP draft model type can now be auto-detected from local GGUF metadata, reducing manual `--spec-type` configuration.  
  https://github.com/ggml-org/llama.cpp/pull/27005  
  https://github.com/ggml-org/llama.cpp/pull/26814
- **Kimi-K3 text model** support is proposed via PR — hybrid KDA + MLA attention, cross-layer residual attention, and latent MoE.  
  https://github.com/ggml-org/llama.cpp/pull/26185
- **MiniMax-Text-01 / MiniMax-M1** support proposed (lightning-attention models).  
  https://github.com/ggml-org/llama.cpp/pull/27018
- **LFM2 / LFM2-MoE** tensor-split support proposed.  
  https://github.com/ggml-org/llama.cpp/pull/26993
- **EAGLE-3.1 conversion** fix: aux hidden-state layer IDs are now read from `eagle_config`.  
  https://github.com/ggml-org/llama.cpp/pull/27040
- **Cohere2 MoE chat template** gains Responses-API JSON-schema parsing in an open PR.  
  https://github.com/ggml-org/llama.cpp/pull/26013

---

### 4. Performance & Optimization

- **Flash-attention V-cache conversion vectorized** on CPU (`F16` → `F32`), reducing prefill overhead.  
  https://github.com/ggml-org/llama.cpp/pull/26947
- **Metal TQ2_0 `mul_mv` kernel optimized**: float ops replace integer ops, and scale sums are precomputed.  
  https://github.com/ggml-org/llama.cpp/pull/26980
- **Jinja template parser**: quadratic cost in `gather_string_parts` fixed. Relevant for chat-template-heavy serving workloads.  
  https://github.com/ggml-org/llama.cpp/pull/27034
- **Windows hybrid-CPU scheduling**: PR proposes filtering E-cores and improving core affinity on Alder/Raptor/Arrow Lake and hybrid AMD CPUs.  
  https://github.com/ggml-org/llama.cpp/pull/27033
- **CUDA MoE MMQ ids path**: tail padding now sized from flattened row count, fixing a potential allocation bug for MoE gate/up projections.  
  https://github.com/ggml-org/llama.cpp/pull/27044
- **Hexagon FA**: fixes non-deterministic `FLASH_ATTN_EXT` results by correcting HMX queue ordering and packing rescale D matrices.  
  https://github.com/ggml-org/llama.cpp/pull/27042
- **OpenCL flash-attention**: fixes a WAR race in generic tile kernels.  
  https://github.com/ggml-org/llama.cpp/pull/26434

Open performance regressions remain under investigation:

- Vulkan: performance drop in recent builds on RX 6600.  
  https://github.com/ggml-org/llama.cpp/issues/24066
- SYCL: Q8_0 reorder degrades prefill by ~42%, not visible in `llama-bench`.  
  https://github.com/ggml-org/llama.cpp/issues/25203
- Vulkan: batched decode throughput cliff at `n_tokens=9` on many-expert MoE.  
  https://github.com/ggml-org/llama.cpp/issues/25356

---

### 5. Stability & Regressions

Ranked by severity:

1. **Vulkan `DeviceLost` on DeepSeek-V4-Flash** — RADV Strix Halo, occurs within a few turns.  
   https://github.com/ggml-org/llama.cpp/issues/25664
2. **SYCL garbage on second prompt** — Intel Arc Pro B60, recent build.  
   https://github.com/ggml-org/llama.cpp/issues/26845
3. **DeepSeek-V4-Flash degenerates and leaks special tokens** in long agentic chats on Metal.  
   https://github.com/ggml-org/llama.cpp/issues/26694
4. **ROCm gfx1151 RPC worker crash in `GGML_OP_TOP_K`** during DeepSeek V4 prefill after 4096 tokens.  
   https://github.com/ggml-org/llama.cpp/issues/26746
5. **DFlash drafter fails to bind** when target GGUF uses `attention.sliding_window_pattern` as an array (Muse-Glimmer official GGUF).  
   https://github.com/ggml-org/llama.cpp/issues/26894
6. **Muse-Glimmer tool-call formatting error** — "peg-native format" mismatch on CUDA; closed as unconfirmed.  
   https://github.com/ggml-org/llama.cpp/issues/27025
7. **Intel Ultra CPU/GPU breakage after b10215** — Vulkan errors on Core Ultra 7 255H + Arc Pro 140T.  
   https://github.com/ggml-org/llama.cpp/issues/26769
8. **Gemma 4 SWA forgets key details** — potentially sliding-window-attention correctness issue.  
   https://github.com/ggml-org/llama.cpp/issues/25751
9. **Vulkan performance drop** — open regression, no fix PR yet.  
   https://github.com/ggml-org/llama.cpp/issues/24066

Fix PRs in flight for related correctness issues:

- Backend split scheduler race under `-nkvo` / Vulkan+CPU splits.  
  https://github.com/ggml-org/llama.cpp/pull/26040
- CUDA duplicate-expert-id compaction fix in `mul_mat_id`.  
  https://github.com/ggml-org/llama.cpp/pull/26294
- Server observability during `llama_decode()` now available via `/metrics` and `/slots`.  
  https://github.com/ggml-org/llama.cpp/pull/27041

---

### 6. What This Means for Application Developers

- **Update to `b10423`** to pick up the broad CPU-parameter handling and the server UI cache fix; otherwise browser-cached index.html may pin an old UI build.
- **Local MTP draft setups are simpler now**: `--spec-type` can be inferred from GGUF metadata, reducing manual config for speculative decoding.
- **OpenVINO and SYCL users get meaningful backend improvements** — Qwen3.5/MXFP4 on OpenVINO and pinned-host memory on SYCL are worth benchmarking if you run those stacks.
- **Metal now supports TQ2_0**, which matters for ternary-quantized models on Apple Silicon.
- **Server operators should watch the `/metrics`-during-decode PR** — it closes a long-standing observability gap for production serving.
- **For agent/tool-calling applications**, be cautious with Muse-Glimmer and LFM2 tool-call edge cases; fixes landed for LFM2, but Muse still has an open format issue.
- **If you rely on Vulkan or SYCL in production**, the recurring second-prompt/tool-call and MoE-throughput regressions are still unresolved; pin to a known-good build if you hit them.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-08-14

## 1. Today's Highlights

Activity is split between two fronts: the MLX runtime is finally getting structured-output support ([#17690](https://github.com/ollama/ollama/pull/17690), with a deeper XGrammar-based follow-up in [#17697](https://github.com/ollama/ollama/pull/17697)), and the `ollama launch` ecosystem is expanding with Muse Code ([#17594](https://github.com/ollama/ollama/pull/17594)) and DeepSeek Harness ([#17733](https://github.com/ollama/ollama/pull/17733)) integrations. On the regression side, the AMD Strix Halo VRAM-detection breakage in 0.30+ has a candidate fix in [#17685](https://github.com/ollama/ollama/pull/17685), while a llama3.3:70b token-generation regression since v0.32.2 ([#17379](https://github.com/ollama/ollama/issues/17379)) has no fix PR yet.

## 2. Releases & Breaking Changes

No new releases in the last 24 hours.

## 3. New Model & Hardware Support

- **MLX structured output/grammar sampling** — [#17690](https://github.com/ollama/ollama/pull/17690) (closed) adds grammar + JSON Schema constraint support to the MLX runner; [#17697](https://github.com/ollama/ollama/pull/17697) (open) is the fuller implementation using XGrammar with request-scoped matchers and tokenizer metadata. This directly fixes the long-standing "structured outputs ignored on MLX" gap ([#16563](https://github.com/ollama/ollama/issues/16563)).
- **Nemotron MLX vision** — [#17714](https://github.com/ollama/ollama/pull/17714) implements the RADIO vision encoder/projector on the shared MLX media pipeline, with dynamic-resolution preprocessing and deterministic placeholder expansion. Audio remains suppressed.
- **New Launch integrations** — `ollama launch muse` for Meta's Muse Code CLI ([#17594](https://github.com/ollama/ollama/pull/17594), closed) and `ollama launch dsh` for DeepSeek Harness ([#17733](https://github.com/ollama/ollama/pull/17733), closed), supporting local and cloud models.
- **New model requests (no commits yet)** — Qwen3.8 (2.4T-A95B-FP8) for Pro/Max cloud ([#17720](https://github.com/ollama/ollama/issues/17720)); Kimi K3 still not available to subscribers two weeks post-release ([#17715](https://github.com/ollama/ollama/issues/17715)).
- **Feature request** — Microsoft Agent Host Protocol integration ([#17729](https://github.com/ollama/ollama/issues/17729)).

## 4. Performance & Optimization

- **Windows-on-Arm CPU runner** ([#17654](https://github.com/ollama/ollama/pull/17654)): the shipped CPU build falls back to baseline `armv8-a` with zero dot-product/matrix instructions; a one-line `-march` fix in the `cpu_arm64` preset lands the vectorized kernels with no compatibility risk.
- **Backend load planning** ([#17165](https://github.com/ollama/ollama/pull/17165)): centralizes backend memory-policy estimation that is currently split across scheduler preflight, request option setup, and runner startup — fixes inconsistent VRAM estimates in the load path exposed by the iGPU/mmproj fix.
- **MLX open-ended generation budget** ([#17494](https://github.com/ollama/ollama/pull/17494)): bounds open-ended `num_predict` by the request's `num_ctx` rather than the checkpoint's `max_position_embeddings`, preventing indefinite hangs on large models.
- **KV cache accounting** ([#17615](https://github.com/ollama/ollama/pull/17615)): mirrors GraphSize KV accounting in `PredictServerVRAM` to correct Go-side memory prediction used by the scheduler — addresses Qwen loading failures after the llama-server migration.
- **Flash attention explicitness** ([#17477](https://github.com/ollama/ollama/pull/17477)): requests flash attention explicitly for architectures (e.g. GPT-OSS) where llama-server's `auto` mode disables it on partial offload, causing crashes at long context.

## 5. Stability & Regressions

Ranked by severity:

1. **llama3.3:70b generates junk tokens on v0.32.2+** ([#17379](https://github.com/ollama/ollama/issues/17379)) — reproduced in DEV and PROD; prompts ruled out. No fix PR yet. Critical for anyone serving llama3.3.
2. **AMD Strix Halo VRAM detection regression in 0.30+ containers** ([#16462](https://github.com/ollama/ollama/issues/16462)) — only 2GB reported instead of full unified memory; fix PR [#17685](https://github.com/ollama/ollama/pull/17685) adds `OLLAMA_GPU_MEMORY` + `SmallCarveOutIGPU` handling.
3. **`/api/chat` silently drops `audios` on audio-capable models** ([#17730](https://github.com/ollama/ollama/issues/17730)) — HTTP 200, model never receives audio, and confidently answers as text-only. Silent failure, dangerous for audio apps.
4. **MLX models ignore `response_format`** ([#16563](https://github.com/ollama/ollama/issues/16563)) — fix is in review via [#17690](https://github.com/ollama/ollama/pull/17690)/[#17697](https://github.com/ollama/ollama/pull/17697).
5. **`muse-glimmer:30b-mlx` leaks harmony channel tokens and ignores JSON schema** ([#17684](https://github.com/ollama/ollama/issues/17684), closed) — GGUF build unaffected; MLX build prefixes responses with literal control tokens.
6. **Gemma 4 Cloud HTTP 500 with vision + tool calling** ([#17667](https://github.com/ollama/ollama/issues/17667), closed) — presumably acknowledged or fixed.
7. **Ollama launch Claude: no response with qwen3-coder despite successful generation** ([#17671](https://github.com/ollama/ollama/issues/17671)) — Windows 11, Ollama 0.32.8 / Claude Code 2.1.227.
8. **Nemotron3.5-lightning:30b stalls mid-thinking on AMD AI395+** ([#17692](https://github.com/ollama/ollama/issues/17692)) — CTRL+C required to recover.
9. **Vulkan runner: 100% CPU spin near context limit** ([#13461](https://github.com/ollama/ollama/issues/13461)) — one core pegged, memory not released, partial unresponsiveness.
10. **"Token repeat limit reached" false positives** ([#17360](https://github.com/ollama/ollama/pull/17360)) — fix only feeds content-carrying events to the repeat detector; whitespace-only/unfinished events no longer trip the kill switch.
11. **Prequantized MLX imports can report success but produce unloadable models** ([#17731](https://github.com/ollama/ollama/pull/17731)) — fix preserves MLX quantization metadata on `ollama create --experimental`.
12. **Docker: models fail to load after 0.24.0** ([#17285](https://github.com/ollama/ollama/issues/17285), closed) — Ryzen 5750G Vega8/GTT setup; user pinned to 0.24.0.
13. **`WriteWithBackup` collision on rapid writes** ([#17713](https://github.com/ollama/ollama/issues/17713)) — backup names use 1s-resolution timestamps; two writes in the same second collide.
14. **Modelfile parser counts CRLF as two line endings** ([#17734](https://github.com/ollama/ollama/pull/17734)) — breaks line-number reporting in errors on Windows-authored Modelfiles.
15. **Download stalls at 99%** ([#1736](https://github.com/ollama/ollama/issues/1736)) — long-running (since 2023) registry issue: speed collapses from ~13MB/s to tens of KB/s at the end of every pull; still actively commented.

## 6. What This Means for Application Developers

- **MLX structured outputs are imminent**: if you've been soft-coding around `response_format` being ignored on Apple Silicon, the XGrammar work in [#17690](https://github.com/ollama/ollama/pull/17690)/[#17697](https://github.com/ollama/ollama/pull/17697) removes that caveat — test against a nightly or RC once merged.
- **Do not trust `/api/chat` audio today**: the endpoint silently no-ops `audios` on audio-capable models ([#17730](https://github.com/ollama/ollama/issues/17730)), so validate audio receipt or gate on model capability until fixed.
- **Claude Code via `ollama launch` is usable but needs explicit context management**: `kimi-k2.7-code:cloud` triggers a known-model warning and forces a 200k auto-compact window ([#17717](https://github.com/ollama/ollama/issues/17717)), the documented `[1m]` suffix is rejected ([#17584](https://github.com/ollama/ollama/issues/17584)), and qwen3-coder can generate successfully without Claude Code seeing the response ([#17671](https://github.com/ollama/ollama/issues/17671)). Set context window explicitly.
- **llama3.3:70b deployments: hold at ≤0.32.1** until [#17379](https://github.com/ollama/ollama/issues/17379) is fixed — the junk-token regression has no pending PR.
- **Cloud model discovery is unreliable**: `/v1/models` omits some cloud models ([#17725](https://github.com/ollama/ollama/issues/17725)) and new models like Kimi K3 are absent from Pro/Max ([#17715](https://github.com/ollama/ollama/issues/17715)). Validate against the actual endpoint, not docs.
- **`/v1/models` will soon expose `context_length`** ([#17422](https://github.com/ollama/ollama/pull/17422)) — useful for agents that need to size context windows against cloud models.
- **Windows-on-Arm CPU inference will get noticeably faster** when [#17654](https://github.com/ollama/ollama/pull/17654) lands — the runner currently ships without any vectorized dot-product instructions.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-14

## Today's Highlights
LiteLLM published `v1.98.0-dev.2` with cosign image-signature verification. The main engineering push is on auth and access-control correctness: multiple PRs landed or opened around team/access-group grant synchronization, `lite login` CLI session handling, and MCP OAuth persistence across workers. Cost-model work also continues with pricing fixes for Azure GPT-5.6, Vertex Claude regional endpoints, PTU deployments, Meta Muse Spark 1.2, and Voyage 4 embeddings.

## Releases & Breaking Changes
- **v1.98.0-dev.2** — [Release v1.98.0-dev.2](https://github.com/BerriAI/litellm/releases/tag/v1.98.0-dev.2)  
  Focuses on verifying Docker image signatures: all LiteLLM images are signed with cosign using the key introduced in [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0).  
  No explicit API/config breaking changes were called out in the release notes.

## New Model & Hardware Support
- **Meta Muse Spark 1.2** added to the model cost map, including the contributor tier, `reasoning_effort` handling, and web-search grounding pricing.  
  [PR #36717](https://github.com/BerriAI/litellm/pull/36717)
- **Voyage 4 embeddings** added: `voyage-4` family and `voyage-context-4`, plus a fix for contextual embeddings accepting plain `list[str]` input.  
  [PR #35091](https://github.com/BerriAI/litellm/pull/35091)
- No hardware/quantization/backend updates appeared in this window.

## Performance & Optimization
- No throughput/latency/memory kernel PRs landed with concrete numbers in the last 24h.
- **Prompt-cache invalidation risk**: open issue reports that mid-conversation system-role hoisting in `AnthropicMessagesConfig` invalidates the prompt-cache prefix for pre-4.8 Claude models, raising cost/latency on long conversations.  
  [Issue #36559](https://github.com/BerriAI/litellm/issues/36559)
- Closed issue about **large-context Anthropic passthrough streaming** producing no client bytes during pre-stream processing and long TTFT is closed; test against the latest release if you were seeing client no-progress timeouts.  
  [Issue #32491](https://github.com/BerriAI/litellm/issues/32491)

## Stability & Regressions
Ranked by severity:

1. **Redis Cluster response leakage / cross-talk** — previously reported critical multi-tenant isolation bug is closed as of this snapshot; if you run Redis Cluster on OpenShift, verify your deployed version contains the intended fix.  
   [Issue #25447](https://github.com/BerriAI/litellm/issues/25447)
2. **`end_user` in SpendLogs pinned to first request on shared virtual keys** — open regression from v1.87.0; spend attribution is incorrect when different `user` fields are sent through one key.  
   [Issue #31441](https://github.com/BerriAI/litellm/issues/31441)
3. **Azure GPT-5.6 terra/luna cost-map rows incorrectly use OpenAI prices** — open; Azure published meters were never cut, so Azure spend tracking may be wrong.  
   [Issue #36192](https://github.com/BerriAI/litellm/issues/36192)
4. **OTel exporter still receives `gen_ai.system` as `None` in metrics/events paths** — open; PR #26713 fixed only the span-attribute call site.  
   [Issue #36759](https://github.com/BerriAI/litellm/issues/36759)
5. **Guardrails Monitor missing `litellm_content_filter` evaluations** — open; evaluations appear in metadata but not in the Guardrails Monitor UI/logs.  
   [Issue #36566](https://github.com/BerriAI/litellm/issues/36566)
6. **Custom MCP server creation still fails in UI** — open with a provider-agnostic creation error; MCP OAuth session PRs are related but do not fully close this.  
   [Issue #23869](https://github.com/BerriAI/litellm/issues/23869)
7. **Xiaomi MiMo models fail with `output_config` under Claude Code** — open LLM-translation bug; workaround is to avoid that parameter until fixed.  
   [Issue #24549](https://github.com/BerriAI/litellm/issues/24549)
8. **Team/access-group stale grants** — multiple fix PRs are in flight for stale team attachments, key grants, and unsafe auth fallback widening:  
   - [PR #36825](https://github.com/BerriAI/litellm/pull/36825)  
   - [PR #36843](https://github.com/BerriAI/litellm/pull/36843)  
   - [PR #36819](https://github.com/BerriAI/litellm/pull/36819)  
   - [PR #36837](https://github.com/BerriAI/litellm/pull/36837)  
   - [PR #36839](https://github.com/BerriAI/litellm/pull/36839)
9. **Rate-limit 429 error body leaks full SHA-256 token hash** — open security/hygiene issue.  
   [Issue #27884](https://github.com/BerriAI/litellm/issues/27884)

## What This Means for Application Developers
- **CLI auth changes are important**: if you use `lite login`, keep an eye on the new team-grant propagation and session-revocation work. Without it, team-bound CLI sessions may see the full model list and team aliases may not resolve on `/v1/chat/completions`.  
  [PR #36826](https://github.com/BerriAI/litellm/pull/36826) · [PR #36846](https://github.com/BerriAI/litellm/pull/36846)
- **Access-group/team cleanup**: after these PRs, deleting teams/keys or clearing access groups will actually revoke stale grants. Upgrade before relying on team-scoped key cleanup.  
  [PR #36825](https://github.com/BerriAI/litellm/pull/36825) · [PR #36843](https://github.com/BerriAI/litellm/pull/36843)
- **MCP users on multi-worker proxies**: DB-backed OAuth draft persistence should fix random 404s during MCP server authorize/fetch-token flows. Token reauthorization also becomes possible.  
  [PR #36844](https://github.com/BerriAI/litellm/pull/36844) · [PR #36831](https://github.com/BerriAI/litellm/pull/36831)
- **Cost-sensitive workloads**: Azure GPT-5.6 pricing is currently incorrect; avoid relying on spend numbers for those models until fixed. PTU deployments should stop being double-billed after the per-token fallback is removed.  
  [PR #36829](https://github.com/BerriAI/litellm/pull/36829) · [PR #36833](https://github.com/BerriAI/litellm/pull/36833)
- **Voyage/Meta users**: `voyage-4`/`voyage-context-4` and `meta/muse-spark-1.2` now have correct pricing metadata and request handling in the cost map.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-14

## 1. Today's Highlights

v0.1.702-beta ships Unsloth Desktop, the first cross-platform desktop app for running/training local AI models, and adds tool-calling/web search for external providers. The release snapshot is dominated by Desktop install/startup issues on Windows and macOS, while maintainers pushed fixes for Metal memory, streaming-reply CPU saturation, and CI coverage on real T4s. No new quantization formats or benchmark numbers were announced in this window.

## 2. Releases & Breaking Changes

- [v0.1.702-beta](https://github.com/unslothai/unsloth/releases) — “Unsloth Desktop is here!”: local model running/training on Windows, macOS, and Linux, with research/export/deploy workflows. Release notes also add **tool calling / web search & more for all external providers**.
- No explicit migration or config notes were included.
- Desktop installer changes are introducing real friction on Windows: EDR/App Control can block setup ([#8523](https://github.com/unslothai/unsloth/issues/8523), [#8490](https://github.com/unslothai/unsloth/issues/8490)), and installs can be killed by a 2-hour cap while downloading cu126 PyTorch with no progress output ([#8698](https://github.com/unslothai/unsloth/issues/8698)).

## 3. New Model & Hardware Support

- **MiniMax-H3** is surfaced in Unsloth Desktop but is not yet reliably runnable: the bundled stable-diffusion.cpp predates H3 support ([#8507](https://github.com/unslothai/unsloth/issues/8507)), and video generation fails during Qwen3VL text-encoder weight loading ([#8666](https://github.com/unslothai/unsloth/issues/8666)).
- **DeepReinforce Ornith-1.0** optimized variants requested ([#6721](https://github.com/unslothai/unsloth/issues/6721)).
- **External provider tool-calling/web search** is newly enabled in v0.1.702-beta.
- **AMD/ROCm**: RX 5700XT not recognized ([#8529](https://github.com/unslothai/unsloth/issues/8529)); Strix Halo unified-memory offload issue closed ([#8651](https://github.com/unslothai/unsloth/issues/8651)); Radeon 8060S startup segfault closed ([#7331](https://github.com/unslothai/unsloth/issues/7331)).
- **Apple Silicon/Metal**: M4 local GGUF loading fails and idle RAM is excessive ([#8566](https://github.com/unslothai/unsloth/issues/8566)); second-launch error reported ([#8610](https://github.com/unslothai/unsloth/issues/8610)); a fix is in flight to avoid launching llama-server at full native context on Metal ([#8709](https://github.com/unslothai/unsloth/pull/8709)).

## 4. Performance & Optimization

- [#8750](https://github.com/unslothai/unsloth/pull/8750) — Removes unnecessary animation DOM and makes Markdown parsing incremental so long streaming replies no longer saturate Studio's renderer.
- [#8709](https://github.com/unslothai/unsloth/pull/8709) — Prevents llama-server from starting at native context on Metal; `-c 0` previously disabled `--fit` and over-allocated context on Apple Silicon.
- [#8439](https://github.com/unslothai/unsloth/pull/8439) — Uses Kaggle's large overlay for saves and refuses a GGUF export that cannot fit, avoiding silent disk-full aborts.
- [#8440](https://github.com/unslothai/unsloth/pull/8440) — Adds deterministic notebook smoke tests on real Kaggle T4s to catch Turing/sm_75 regressions missing from current CI.
- [#8749](https://github.com/unslothai/unsloth/pull/8749) — Fixes Backend CI collection failures (depends on [#8740](https://github.com/unslothai/unsloth/pull/8740)); four pre-existing pytest failures are now visible and being addressed.
- No concrete throughput/latency numbers were included in this snapshot.

## 5. Stability & Regressions

### High severity
- **Windows Desktop install/startup failures** remain the top cluster:
  - Install process does not finish ([#8546](https://github.com/unslothai/unsloth/issues/8546), open)
  - 2-hour cap during cu126 PyTorch download ([#8698](https://github.com/unslothai/unsloth/issues/8698), open)
  - EDR blocks setup ([#8523](https://github.com/unslothai/unsloth/issues/8523), closed)
  - Application Control blocks `unsloth.exe` ([#8490](https://github.com/unslothai/unsloth/issues/8490), closed)
  - Windows AMD GPU install failure ([#8508](https://github.com/unslothai/unsloth/issues/8508), closed)
  - “Run at login” starts icon but no server ([#8510](https://github.com/unslothai/unsloth/issues/8510), closed)
  - No dedicated fix PRs are attached to the open installers; [#8730](https://github.com/unslothai/unsloth/pull/8730) only fixes stale download links.
- **macOS app reliability**:
  - Second launch error on M4 ([#8610](https://github.com/unslothai/unsloth/issues/8610), open)
  - llama-server fails to start for local GGUF; excessive idle RAM ([#8566](https://github.com/unslothai/unsloth/issues/8566), open)
  - Related Metal context fix is in [#8709](https://github.com/unslothai/unsloth/pull/8709).

### Medium severity
- MiniMax-H3 video generation crashes in Qwen3VL weight loading ([#8666](https://github.com/unslothai/unsloth/issues/8666)) — no fix PR yet.
- Installed MLX models missing from `/v1/models` and API auto-switch cannot load them ([#8748](https://github.com/unslothai/unsloth/issues/8748)).
- Tool calling poisons chat history ([#8734](https://github.com/unslothai/unsloth/issues/8734)).
- `kimi k3` not available via API key ([#8735](https://github.com/unslothai/unsloth/issues/8735)).
- Raw JSONL export is not actually valid JSONL ([#8733](https://github.com/unslothai/unsloth/issues/8733)).
- GGUF export appears to require an intermediate 16-bit model download ([#8717](https://github.com/unslothai/unsloth/issues/8717)).
- Deep Research freezes at “Writing The Report” ([#8483](https://github.com/unslothai/unsloth/issues/8483)).
- Kaggle T4 training crash with Qwen3.5-0.8B bf16 ([#7506](https://github.com/unslothai/unsloth/issues/7506)).
- Ubuntu WebKitGTK microphone permissions block voice features ([#8678](https://github.com/unslothai/unsloth/issues/8678)).
- API server cannot bind to `0.0.0.0` without a tunnel ([#8578](https://github.com/unslothai/unsloth/issues/8578)).

### Closed/resolved in this window — re-test on 0.1.702-beta
- Claude Code API 401 auth header mismatch ([#8663](https://github.com/unslothai/unsloth/issues/8663))
- Remote models unable to do tool calling ([#7282](https://github.com/unslothai/unsloth/issues/7282))
- Strix Halo `GGML_CUDA_ENABLE_UNIFIED_MEMORY=1` offload issue ([#8651](https://github.com/unslothai/unsloth/issues/8651))
- Radeon 8060S ROCm crash ([#7331](https://github.com/unslothai/unsloth/issues/7331))
- ROCm multi-GPU auto-selection picking iGPU ([#7624](https://github.com/unslothai/unsloth/issues/7624))

## 6. What This Means for Application Developers

- **Desktop rollout is not yet frictionless.** On locked-down Windows, expect EDR/App Control interference and long PyTorch downloads. For fleet deployment, pre-seed wheels or use the pip/Python path until installer reliability improves ([#8523](https://github.com/unslothai/unsloth/issues/8523), [#8490](https://github.com/unslothai/unsloth/issues/8490), [#8698](https://github.com/unslothai/unsloth/issues/8698)).
- **Studio API is becoming a more serious agent runtime.** Tool-calling for external providers is now available; validate SSE/tool approval behavior after the keepalive fix ([#8628](https://github.com/unslothai/unsloth/pull/8628)) and watch for tool-call history pollution ([#8734](https://github.com/unslothai/unsloth/issues/8734)).
- **macOS Metal local inference is still immature.** Do not rely on M4/Metal GGUF serving in production yet; track [#8566](https://github.com/unslothai/unsloth/issues/8566) and the context fix in [#8709](https://github.com/unslothai/unsloth/pull/8709).
- **If your app depends on `/v1/models`**, verify installed MLX models are registered and auto-switch works ([#8748](https://github.com/unslothai/unsloth/issues/8748)).
- **GGUF export can be disk-heavy.** Recent behavior appears to require a large 16-bit intermediate download, so plan storage and transfer time accordingly ([#8717](https://github.com/unslothai/unsloth/issues/8717)).

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*