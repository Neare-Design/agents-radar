# AI Infrastructure Digest 2026-08-15

> Generated: 2026-08-14 23:14 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project AI Infrastructure Digest — 2026-08-15

## 1. Ecosystem Overview

The inference stack is mid-transition: DeepSeek-V4 and Kimi-K3 (the first hybrid attention + Mamba/GDN/MTP architectures at frontier scale) are consuming most engineering capacity across vLLM and SGLang, while AMD/ROCm enablement remains the single largest stability battleground — vLLM reports unresolved silent retrieval corruption on MI325X, and SGLang still has no fix for a sparse-attention crash on long-context requests. Meanwhile, the local/edge tier moved fast this week: llama.cpp, Ollama, and Unsloth all shipped new releases with Qwen3.8 and MiniMax support, and ollama launch + reasoning-effort propagation signal that agent workloads are now a first-class target. The main reliability theme across every project is the same: new hybrid architectures and quantization paths (FP8/NVFP4/MXFP4) are landing faster than correctness validation can keep up, especially on non-NVIDIA hardware.

## 2. Activity Comparison

| Project | Issues (24h) | PRs (24h) | Releases (24h) | Notable Events |
|---|---|---|---|---|
| vLLM | 101 updated | 500 updated | None | MRV2 spec-decode −15% (MTP=3, DeepSeek-V4-Flash); 2 critical ROCm issues open |
| SGLang | No count given | No count given | None | 3 broken + 11 flaky CI tests; Kimi-K3 PP8 TTFT floor (~30 s) |
| llama.cpp | No count given | No count given | 11 (b10425–b10435) | Server stays responsive during decode; SYCL fusions for Arc |
| Ollama | No count given | No count given | 3 (v0.32.11–13) | Qwen3.8 27B support; DeepSeek Harness + Muse Code via launch |
| LiteLLM | No count given | No count given | None | Critical Admin UI auth lockout; revert in flight |
| Unsloth | No count given | No count given | 1 (v0.1.800-beta) | Qwen3.8-27B dynamic GGUF on 17 GB RAM; CUDA 13.2 IQ-quant warning |

## 3. Model Support Race

**Frontier multi-GPU serving — vLLM and SGLang are in a dead heat.** vLLM landed DCP partial-prefix cache hits and ROCm torch.compile for Kimi-K3, plus MiMo-V2 fused FP8 QKV sharding and pipeline-parallel speculative decoding. SGLang shipped Kimi-K3 cookbook enablement with explicit SiTU activation via DeepGEMM, validated Qwen3.8-27B-FP8 on GB10, and added a MiniMax-H3 quality/tuning recipe for 8×B300.

**Local/edge — llama.cpp, Ollama, and Unsloth all shipped.** llama.cpp added MiniMax-Text-01/M1 support via lightning attention (closing a long-standing request) and has Kimi-K3 text (KDA+MLA) in progress. Ollama released Qwen3.8 27B with Apple Silicon tuning. Unsloth released v0.1.800-beta supporting Qwen3.8-27B/2.4T local inference, fine-tuning, and NVFP4 quants on 17 GB RAM.

**Gateway — LiteLLM added nothing this window.**

**Verdict:** vLLM/SGLang lead on frontier-scale hybrid architectures (DeepSeek-V4, Kimi-K3), but both have stability gaps. llama.cpp is closing the architecture gap fastest on the local side; Ollama and Unsloth are winning the "default agent model on consumer hardware" race with Qwen3.8 distribution.

## 4. Performance Frontier

Optimization effort is concentrated in five areas:

- **Speculative decoding:** vLLM's adaptive verification for MTP=3 cut DeepSeek-V4-Flash benchmark duration from 96.0 s → 81.3 s at concurrency 64 (~15%). SGLang added MTP cache-mode for GDN final-state recompute with FlashInfer. Both projects hit model-dependent quality walls (e.g., Qwen3.5 dflash acceptance stuck at 5–6 tokens).
- **KV cache / offload:** vLLM shipped partial-prefix reuse under DCP and a new CPU/tiered-offload capacity metric; SGLang continues HiCache L1+L2+Mooncake SSD work but reports variable hit rates; llama.cpp added gated-delta-net state writeback fusion.
- **Quantization:** NVFP4→MXFP4 online requantization for AMD MI355x (SGLang), NVFP4 quants for Qwen3.8 (Unsloth), FP8 KV-cache and router correctness issues on AMD (both vLLM and SGLang).
- **Kernels / hardware-specific:** ROCm Triton sparse-MLA decode for gfx950 (vLLM), DeepGEMM SM 12.x gaps for consumer Blackwell (open), llama.cpp SYCL q4_K FFN fusion (+2.8% measured) and TILE KV decode (+42–169% on Battlemage).
- **Distributed serving:** PP8 disaggregated prefill (SGLang) exposed a load-independent ~30 s TTFT floor on Kimi-K3; vLLM lifted the PP rejection for spec-decode drafters and is tracking decode context parallelism + NCCL 2.30 integration.

## 5. Layer Positioning

| Project | Layer | Core Value | Primary Risk Today |
|---|---|---|---|
| **vLLM** | Multi-GPU serving engine | Highest-throughput serving for frontier models; MRV2 becoming default execution path | ROCm silent corruption on MI325X for DeepSeek-V4; MRV2 migration breakage |
| **SGLang** | Multi-GPU serving engine | Fastest iteration on new architectures (Kimi-K3, DeepSeek-V4 sparse attention); strong AMD push | Long-context sparse-attention crash; PP8 prefill latency floor |
| **llama.cpp** | Local/single-node runtime | Broadest architecture coverage, lightweight, SYCL for Intel + rapid multi-backend support | SYCL regression on A770; Vulkan perf regression |
| **Ollama** | End-user local runtime | Docker-onedevice distribution, agent integrations via `ollama launch`, Apple Silicon focus | CUDA prefill crash on qwen3.6:35b; AMD VRAM misdetection in containers |
| **LiteLLM** | AI gateway / proxy / auth | Multi-provider routing, spend tracking, model management for agents | Auth regressions (Admin UI lockout); pass-through corruption with Anthropic brotli |
| **Unsloth** | Fine-tuning framework + local runtime + Studio | Fast fine-tuning + GGUF quantization + local serving; bridges training → inference | AMD ROCm AOTriton gate causing OOM; CUDA 13.2 IQ-quant silent gibberish |

## 6. Trend Signals

**1. AMD/ROCm is the adoption frontier, but trust is the bottleneck.** Every project is investing heavily (vLLM sparse-MLA on gfx950, SGLang MI350 nightly CI, llama.cpp Windows ROCm fixes, Ollama Strix Halo / Radeon 780M work). Yet the unresolved failure classes are severe: silent corruption (vLLM #52109), illegal memory access (SGLang #34718), and VRAM misdetection (Ollama #16462). Decision: pin exact ROCm/PyTorch versions and add output-level validation before serving DeepSeek-V4/Kimi-K3 on AMD.

**2. Hybrid architectures are stressing the whole stack.** KDA+MLA, Mamba/GDN hybrids, and MTP heads break assumptions baked into CUDA graphs, prefix caches, spec-decode verification, and memory estimators (vLLM negative −35 GiB CUDA-graph estimate, SGLang hybrid-Mamba spec-decode crash, llama.cpp gated-delta-net writeback). Engines that internalize these architectures cleanly (MRV2 in vLLM) will gain a durable advantage.

**3. Local + agent is converging.** `ollama launch` for agent harnesses, `reasoning_effort` propagation in llama.cpp templates, Unsloth's tool-loop fixes, and the macOS Ask bar all point to local runtimes becoming the default agent execution substrate. Watch tool-calling correctness (PDF attachments breaking tool calls, MCP breakage) — it's the top churn area.

**4. Speculative decoding is real but fragile.** The ~15% MTP=3 win is the best evidence yet that adaptive verification works, but acceptance-length collapse on some Qwen derivatives and CUDA-graph memory startup crashes mean teams should benchmark per-model before enabling.

**5. CUDA 13 packaging is a new failure class.** vLLM's `libcudart.so.13` wheel failure on CUDA 12.6 hosts and Unsloth's CUDA 13.2 gibberish for IQ2/IQ3 quants are early signs of toolchain fragmentation. Pin CUDA 12.8/13.0 binaries for now.

**6. Gateway correctness matters more than features.** LiteLLM's Admin UI lockout (auth regression), brotli pass-through corruption, and Vertex AI `count_tokens` returning 0 are all correctness issues that break workflows silently. The stability sprint roadmap (#30484) is the right investment; application teams should pin known-good gateway versions.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-15

101 issues and 500 PRs were updated in the last 24 hours. DeepSeek-V4 and Kimi-K3 remain the dominant focus, with Model Runner V2 (MRV2) correctness and ROCm enablement as the two main threads.

## 1. Today's Highlights

Two storylines stand out: (1) MRV2 is gaining real performance ground — acceptance estimation for adaptive spec-decode verification ([#52228](https://github.com/vllm-project/vllm/pull/52228)) reports DeepSeek-V4-Flash + MTP=3 benchmark duration improving from 96.0s to 81.3s at concurrency 64 (~15% faster); (2) a new fix ([#52401](https://github.com/vllm-project/vllm/pull/52401)) resolves DeepSeek-V4 CUDA-graph output corruption by selecting the eager cudagraph region per model runner, removing the MRV2-only default that penalized ROCm. The main reliability caveat: silent retrieval corruption and worker crashes on MI325X for DeepSeek-V4 remain unresolved ([#52109](https://github.com/vllm-project/vllm/issues/52109), [#48266](https://github.com/vllm-project/vllm/issues/48266)).

## 2. Releases & Breaking Changes

None in the last 24 hours — no new tags, API changes, or migration notes.

## 3. New Model & Hardware Support

- **[Kimi-K3] DCP partial prefix cache hit** ([#50493](https://github.com/vllm-project/vllm/pull/50493)) — adds hash-aligned partial-prefix reuse under DCP, plus an MRV2 block-table geometry fix for sharded attention + replicated Mamba.
- **[Kimi-K3][ROCm] torch.compile enablement** ([#52190](https://github.com/vllm-project/vllm/pull/52190)) — marks model classes with `@support_torch_compile` so AITER post-grad fusion passes (`fused_qk_rmsnorm_kernel`, `allreduce_fusion_kernel_1stage`) can apply.
- **MiMo-V2: TP > num_kv_heads for fused FP8 QKV** ([#46755](https://github.com/vllm-project/vllm/pull/46755)) — fixes sharding of the fused FP8 `qkv_proj` in the base and MTP loaders.
- **Speculative decoding under pipeline parallelism** ([#50514](https://github.com/vllm-project/vllm/pull/50514)) — lifts the outright rejection of `eagle3` / `dflash` / `dspark` with PP by running the drafter on the last PP rank.
- **MRV2 feature parity**: prompt embeds ([#42963](https://github.com/vllm-project/vllm/pull/42963)) and draft-model spec decode ([#43091](https://github.com/vllm-project/vllm/pull/43091)).
- **Tracking**: ViT Full CUDA Graph for multimodal encoders — Qwen3-VL, Qwen3.5, GLM-V, Kimi K2.5 ([#38175](https://github.com/vllm-project/vllm/issues/38175)); DeepGEMM SM 12.x kernel gaps for DeepSeek-V4-Flash on consumer Blackwell/GB10 ([#41063](https://github.com/vllm-project/vllm/issues/41063)).

## 4. Performance & Optimization

- **MRV2 adaptive verification (spec decode)** ([#52228](https://github.com/vllm-project/vllm/pull/52228)) — DeepSeek-V4-Flash + MTP=3, 2K/2K, concurrency 64: duration 96.01s → 81.25s (~15%), with 256/256 successful requests.
- **ROCm Triton sparse-MLA decode for gfx950/MI355X** ([#52212](https://github.com/vllm-project/vllm/pull/52212)) — adds a gfx950 specialization and workload-aware split policy; pure-Triton, preserves non-gfx950 paths.
- **ROCm tracking efforts**: Kimi-K3 gap/roadmap including AITER fused-MoE a16w4/a8w4 ([#50682](https://github.com/vllm-project/vllm/issues/50682)); DeepSeek-V4 end-to-end checklist (mHC/HCA/CSA/MoE/MTP) ([#41820](https://github.com/vllm-project/vllm/issues/41820)).
- **MI325X MTP perf is untuned**: DeepSeek-V4-Pro 1.6T (FP4/FP8) shows scattered, poor throughput on agentic-trace workloads ([#51853](https://github.com/vllm-project/vllm/issues/51853)).
- **LMCache KV connector shipped in ROCm Docker image** ([#51208](https://github.com/vllm-project/vllm/pull/51208)), matching the CUDA release pipeline.
- **New KV-offload capacity metric** ([#49307](https://github.com/vllm-project/vllm/pull/49307)) — Info gauge reporting `num_blocks`, `blocks_per_gpu` for the native CPU/tiered offload tier.
- **Qwen3.5-35B-A3B dflash underperforms** ([#50722](https://github.com/vllm-project/vllm/issues/50722)) — poor throughput with and without DFlash, with accepted-token length stuck at ~5–6.

## 5. Stability & Regressions

**Critical**

- **DeepSeek-V4-Flash silent retrieval corruption on ROCm** ([#52109](https://github.com/vllm-project/vllm/issues/52109)) — MI325X (gfx942), AITER sparse indexer, prompts ≥ ~4–5k tokens. Silent corruption is the worst failure class; reproduces even with backports of #52058/#51252. No fix in flight.
- **GPU memory access fault / worker crash** ([#48266](https://github.com/vllm-project/vllm/issues/48266)) — ROCm/gfx942, DeepSeek-V4 flash + `sparse_attn_indexer` + FP8 KV cache, sequences crossing 2048 tokens, MI325X TP=4.
- **Garbled output with CUDA graph** ([#41331](https://github.com/vllm-project/vllm/issues/41331)) — DeepSeek-V4 under concurrent identical input requests with CUDA graph enabled.

**High**

- **DeepSeekV4-Flash inline system-message regression** ([#46710](https://github.com/vllm-project/vllm/issues/46710)) — incorrect output after PR [#46025](https://github.com/vllm-project/vllm/pull/46025) changed `chat_template` handling into three behavior paths; the template-raises path (e.g., Qwen) misbehaves when preserved in-place.
- **DeepSeek-V4-Pro TP=16 fp8 shape failure** ([#42384](https://github.com/vllm-project/vllm/issues/42384)) — `shared_experts.down_proj` fails the fp8 block-shape check, contradicting the official recipes.vllm.ai config.
- **Mamba-2 Triton illegal instruction on SM121** ([#37431](https://github.com/vllm-project/vllm/issues/37431)) — DGX Spark crashes in async mode without `CUDA_LAUNCH_BLOCKING=1`.
- **Negative CUDA-graph memory estimate (-35 GiB)** ([#44740](https://github.com/vllm-project/vllm/issues/44740)) — MTP spec decode on GB10 causes KV-cache over-allocation and OOM at startup.
- **ROCm pybind11 mismatch breaks Kimi K2.6/K3** ([#52400](https://github.com/vllm-project/vllm/pull/52400)) — Dockerfile.rocm installs pybind11 3.1.0 while AITER kernels were built with 3.0.4; PR drops the explicit pybind11 install.
- **Prefix caching ineffective on Mamba-2/GDN hybrid** ([#51250](https://github.com/vllm-project/vllm/issues/51250)) — Qwen3.6-35B-A3B on GB10, unverified on current main.

**Moderate**

- **`libcudart.so.13` ImportError** ([#52300](https://github.com/vllm-project/vllm/issues/52300)) — `pip install vllm==0.21.0` fails in a CUDA 12.6 environment; the wheel appears to require a CUDA 13 runtime.
- **NVFP4 Marlin EngineDeadError** ([#49926](https://github.com/vllm-project/vllm/issues/49926)).
- **Qwen3.5 flashinfer GDN TimeoutError** ([#38916](https://github.com/vllm-project/vllm/issues/38916)).

**Fix PRs in flight**

- [#52401](https://github.com/vllm-project/vllm/pull/52401) — DeepSeek-V4 eager cudagraph region selected per model runner; fixes MRV1 corruption from #51430 and unblocks MRV1 on ROCm (rejected by #51768's MRV2 default).
- [#52311](https://github.com/vllm-project/vllm/pull/52311) — off-by-one fix in `_bad_words_kernel` draft-prefix matching (spec decode, present since #33433).
- [#52399](https://github.com/vllm-project/vllm/pull/52399) — `/inference/v1/generate` now returns all choices when `n > 1` (non-streaming path kept `CUMULATIVE` output and dropped extra outputs).
- [#52397](https://github.com/vllm-project/vllm/pull/52397) — fixes `max_offload_tokens` assertion in the native CPU/tiered `OffloadingConnector` on hybrid Mamba-attention models.
- [#52396](https://github.com/vllm-project/vllm/pull/52396) — DSpark + unquantized draft no longer crashes at engine init in `get_draft_quant_config`.
- [#52395](https://github.com/vllm-project/vllm/pull/52395) — `supports_mm_prefix` returns False for ROCm attention (Prefix-LM not implemented).
- [#50272](https://github.com/vllm-project/vllm/pull/50272) — speculative decoding fixed for short-conv (LFM2) models; [#43249](https://github.com/vllm-project/vllm/pull/43249) — MRV2 Gumbel sampling with `-inf` logits.

## 6. What This Means for Application Developers

- **Do not trust DeepSeek-V4 / Kimi-K3 outputs on ROCm without validation.** Unresolved silent corruption ([#52109](https://github.com/vllm-project/vllm/issues/52109)) and worker crashes ([#48266](https://github.com/vllm-project/vllm/issues/48266)) on MI325X make output-level checks mandatory if you serve these models on AMD. Consider pinning NVIDIA until the AITER sparse-indexer path is fixed.
- **MRV2 is becoming the default execution path.** Spec-decode with draft models ([#43091](https://github.com/vllm-project/vllm/pull/43091)), prompt embeds ([#42963](https://github.com/vllm-project/vllm/pull/42963)), sampling fixes ([#43249](https://github.com/vllm-project/vllm/pull/43249), [#52311](https://github.com/vllm-project/vllm/pull/52311)), and pipeline-parallel spec decode ([#50514](https://github.com/vllm-project/vllm/pull/50514)) are all landing against MRV2. If you pinned MRV1 for DeepSeek-V4, watch [#52401](https://github.com/vllm-project/vllm/pull/52401).
- **Speculative decoding wins are real but model-dependent.** The ~15% MTP=3 gain from [#52228](https://github.com/vllm-project/vllm/pull/52228) is promising, but DFlash acceptance is poor on some Qwen3.5 derivatives ([#50722](https://github.com/vllm-project/vllm/issues/50722)), and MTP + CUDA-graph memory estimation can still crash startup on unified-memory parts ([#44740](https://github.com/vllm-project/vllm/issues/44740)).
- **API serving fixes are worth upgrading for.** Multi-choice responses (`n > 1`) from `/inference/v1/generate` were silently dropping outputs for non-streaming requests; fixed in [#52399](https://github.com/vllm-project/vllm/pull/52399).
- **Packaging caveats**: the 0.21.0 pip wheel can fail with `libcudart.so.13` on CUDA 12.6 hosts ([#52300](https://github.com/vllm-project/vllm/issues/52300)), and current ROCm images risk the pybind11/AITER break ([#52400](https://github.com/vllm-project/vllm/pull/52400)). Use nightly images once these merge.
- **Operational RFCs on the horizon**: standardized semantic entrypoint errors (2k+ raw raises today) ([#48227](https://github.com/vllm-project/vllm/issues/48227)) and race-free port allocation for multi-worker deployments ([#51275](https://github.com/vllm-project/vllm/issues/51275)) — both worth tracking if you run at scale.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-15

## Today's Highlights

No new release landed in the last 24 hours; activity is concentrated on Kimi-K3 enablement, AMD/ROCm paths, and long-context stability. The most critical open items are a load-independent ~30s TTFT floor on Kimi-K3 with PP8 disaggregated prefill ([#34815](https://github.com/sgl-project/sglang/issues/34815)) and an illegal memory access in DeepSeek-V4 sparse attention on long-context requests ([#34718](https://github.com/sgl-project/sglang/issues/34718)). CI remains a watch item: 3 broken and 11 flaky tests are currently tracked on `main` ([#17050](https://github.com/sgl-project/sglang/issues/17050)).

## Releases & Breaking Changes

- No new releases or versioned changes in the last 24 hours.
- No merged API/config changes. Note: closed RFC [#34295](https://github.com/sgl-project/sglang/issues/34295) proposes removing the long-broken `--torchao-config` integration, so avoid depending on it.

## New Model & Hardware Support

- **Kimi-K3**: cookbook now points to standard Python/Docker install paths instead of prerelease images ([#34886](https://github.com/sgl-project/sglang/pull/34886)); MegaMoE now uses explicit SiTU activation via DeepGEMM, pending upstream DeepGEMM support ([#34883](https://github.com/sgl-project/sglang/pull/34883)).
- **AMD**: NVFP4 → MXFP4 online requantization for AMD MI355x-class hardware is in progress ([#29328](https://github.com/sgl-project/sglang/pull/29328)); Kimi-K3 8-GPU MI35x nightly accuracy CI proposed ([#32568](https://github.com/sgl-project/sglang/pull/32568)); M3 optimization on MI350 ([#34014](https://github.com/sgl-project/sglang/pull/34014)); GPT-OSS perf coverage added to ROCm 7.2 nightly ([#34645](https://github.com/sgl-project/sglang/pull/34645)).
- **NPU**: LTX-2/2.3 diffusion optimization work for NPU is open ([#34722](https://github.com/sgl-project/sglang/pull/34722)).
- **Qwen3 MoE**: DeepEP-class backends and early EPLB state support for Mooncake EP / EPLB paths ([#34810](https://github.com/sgl-project/sglang/pull/34810)).
- **NemotronLabs VoiceChat 11B**: S2S model support is proposed ([#34873](https://github.com/sgl-project/sglang/pull/34873)).
- **Qwen3.8-27B-FP8**: validated on one DGX Spark / GB10 node, including image digest and model revision ([#34872](https://github.com/sgl-project/sglang/issues/34872)).
- **MLX/Torch**: RFC proposes consolidating the MLX runner-stub split into one Torch-owned SRT path plus an exported whole-model MLX region ([#32321](https://github.com/sgl-project/sglang/issues/32321)).

## Performance & Optimization

- **DeepSeek-V4 sparse attention**: fused norm + RoPE + uniform FP8 store for TRT-LLM backend in progress ([#32975](https://github.com/sgl-project/sglang/pull/32975)).
- **GDN**: MTP cache mode for final-state recompute with FlashInfer integration and overlapped CUDA-graph state recovery ([#30967](https://github.com/sgl-project/sglang/pull/30967)).
- **MiniMax-H3**: validated `quality="high"` on 8×B300 with a retuned Cache-DiT schedule `(4, 0.24, 3)` ([#34841](https://github.com/sgl-project/sglang/pull/34841)).
- **Qwen3.5 FP8 GB300 nightly**: CI perf batches trimmed to reduce redundant runtime coverage ([#34882](https://github.com/sgl-project/sglang/pull/34882)).
- **Roadmaps**: Context Parallelism for 2026 Q3 ([#21788](https://github.com/sgl-project/sglang/issues/21788)); Decode Context Parallelism + Helix Parallelism ([#29736](https://github.com/sgl-project/sglang/issues/29736)); NCCL 2.30 feature integration including NCCL EP and communicator checkpointing ([#32774](https://github.com/sgl-project/sglang/issues/32774)); kernel auto-tuner for MoE/attention configs ([#13363](https://github.com/sgl-project/sglang/issues/13363)).

## Stability & Regressions

Ranked by likely production impact:

1. **High — Kimi-K3 PP8 disaggregated prefill**: ~30s load-independent TTFT floor despite no load dependency ([#34815](https://github.com/sgl-project/sglang/issues/34815)).
2. **High — DeepSeek-V4 sparse attention crash**: illegal memory access in `fp8_paged_mqa_logits` on long-context requests ([#34718](https://github.com/sgl-project/sglang/issues/34718)).
3. **High — Diffusion backend regression**: attention backend fallback change introduces errors on most diffusion models ([#34389](https://github.com/sgl-project/sglang/issues/34389)).
4. **High/Medium — Router GEMM dtype correctness**: fp32 output required for deterministic DeepSeek V3/V4 routing, plus related ROCm/AITER and NPU issues: [#34758](https://github.com/sgl-project/sglang/issues/34758), [#34857](https://github.com/sgl-project/sglang/issues/34857), [#34861](https://github.com/sgl-project/sglang/issues/34861).
5. **Medium — Hybrid-Mamba + speculative decoding crash**: `TypeError` in `set_mamba_track_indices_from_reqs`; duplicate issue [#34787](https://github.com/sgl-project/sglang/issues/34787) closed, original remains open ([#34786](https://github.com/sgl-project/sglang/issues/34786)).
6. **Medium — Detokenization performance**: `SGLANG_SIMULATE_ACC_LEN` silently degrades to byte-fallback O(n²) detokenization ([#34740](https://github.com/sgl-project/sglang/issues/34740)).
7. **Medium — API inconsistency**: `/v1/responses` returns `created_at` as float in streaming events but int in non-streaming responses ([#34716](https://github.com/sgl-project/sglang/issues/34716)).
8. **Medium — HiCache cache-hit drop-off**: L1+L2+Mooncake SSD path shows variable hit rates even with sufficient SSD capacity ([#33984](https://github.com/sgl-project/sglang/issues/33984)).
9. **Low — Telemetry**: `num_requests_running` metric is incorrect with `cp=8` ([#31896](https://github.com/sgl-project/sglang/issues/31896)).
10. **Closed/Resolved**: DeepSeek-V4-Flash startup abort from duplicate TVM FFI registration was closed ([#34858](https://github.com/sgl-project/sglang/issues/34858)); Cache-DiT silently disabled with breakable CUDA graphs is closed ([#34177](https://github.com/sgl-project/sglang/issues/34177)); FlashInfer fused top-k fix for packed PAGED rows was merged ([#33006](https://github.com/sgl-project/sglang/pull/33006)).

## What This Means for Application Developers

- If you serve **Kimi-K3** with PP8 disaggregated prefill, validate TTFT on your exact topology before committing to latency SLOs ([#34815](https://github.com/sgl-project/sglang/issues/34815)).
- For **DeepSeek-V4 long-context workloads**, pin to a known-good build and test the sparse attention path before upgrading; [#34718](https://github.com/sgl-project/sglang/issues/34718) can hard-crash the engine.
- Prefer released images over `main`: CI currently reports 3 broken + 11 flaky tests ([#17050](https://github.com/sgl-project/sglang/issues/17050)).
- Do not rely on `--torchao-config`; removal is already being proposed ([#34295](https://github.com/sgl-project/sglang/issues/34295)).
- On **AMD/MI350**, MoE/quantization support is improving, but router GEMM dtype issues may affect deterministic or bitwise-reproducible inference ([#34758](https://github.com/sgl-project/sglang/issues/34758), [#34857](https://github.com/sgl-project/sglang/issues/34857)).
- API clients consuming `/v1/responses` should normalize `created_at` to a single type to avoid streaming/non-streaming drift ([#34716](https://github.com/sgl-project/sglang/issues/34716)).

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-15

## Today's Highlights
The server now stays responsive during long decodes: `/metrics` and `/slots` are reachable while `llama_decode()` is running ([b10429](https://github.com/ggml-org/llama.cpp/releases/tag/b10429)). Template handling gained `reasoning_effort` propagation from OpenAI Chat Completions ([b10434](https://github.com/ggml-org/llama.cpp/releases/tag/b10434)), and multiple SYCL kernel fusions landed for Arc GPUs ([b10427](https://github.com/ggml-org/llama.cpp/releases/tag/b10427), [b10425](https://github.com/ggml-org/llama.cpp/releases/tag/b10425)). On the stability side, a critical SYCL regression on Intel A770 was reported today ([#27063](https://github.com/ggml-org/llama.cpp/issues/27063)).

## Releases & Breaking Changes
- **b10435** – Fix quadratic cost in Jinja `gather_string_parts` ([#27034](https://github.com/ggml-org/llama.cpp/pull/27034)). Significant templating performance fix.
- **b10434** – `reasoning_effort` is now passed to chat templates. The `common_chat_templates_inputs` struct gained a new field; custom templates may need updates if they reference the context.
- **b10430** – Virtual iGPU devices are now allowed in addition to physical devices ([#26953](https://github.com/ggml-org/llama.cpp/pull/26953)). Affects device selection / offload logic.
- **b10429** – Server now processes metrics/slots requests even during `llama_decode()`. Improves observability under load.
- **b10431** – Baseline for recurrent state rollback in `ggml_ssm_scan` (CPU/CUDA) for neural state-space models (e.g. Nemotron) ([#26623](https://github.com/ggml-org/llama.cpp/pull/26623)).
- **b10428** – Scrubs developer-specific home paths from docs/test fixtures ([#27043](https://github.com/ggml-org/llama.cpp/pull/27043)).
- **b10426** – Force single-threaded execution on WASI ([#25686](https://github.com/ggml-org/llama.cpp/pull/25686)).

## New Model & Hardware Support
- **MiniMax-Text-01 / MiniMax-M1** support added via lightning attention ([PR #27018](https://github.com/ggml-org/llama.cpp/pull/27018)). Closes the long-standing request [#11290](https://github.com/ggml-org/llama.cpp/issues/11290).
- **Kimi-K3** text model (hybrid KDA+MLA) is in progress ([PR #26185](https://github.com/ggml-org/llama.cpp/pull/26185)).
- **Virtual iGPU devices** are now supported, expanding GPU offload flexibility on systems with virtualized GPU partitions ([b10430](https://github.com/ggml-org/llama.cpp/releases/tag/b10430)).
- **SYCL FFN fusion for dense q4_K** and **gated-delta-net state writeback fusion** both landed, improving Intel Arc efficiency ([b10427](https://github.com/ggml-org/llama.cpp/releases/tag/b10427), [b10425](https://github.com/ggml-org/llama.cpp/releases/tag/b10425)).

## Performance & Optimization
- **Jinja templating** – quadratic `gather_string_parts` replaced with linear-time appends, fixing a major slowdown on chat templates ([b10435](https://github.com/ggml-org/llama.cpp/releases/tag/b10435)).
- **SYCL q4_K dense FFN fusion** – measured on Arc Pro B70 (`tg128`): qwen2.5-3B Q4_K_M `154.18 → 158.53 t/s` (+2.8%), gemma-2-2b-it Q4_K_M `162.45 → 16x` (truncated in report) ([b10427](https://github.com/ggml-org/llama.cpp/releases/tag/b10427)).
- **SYCL gated-delta-net writeback fusion** – ported from CUDA; benchmarks on Qwen 3.6 27B show measurable decode gains (figures truncated) ([b10425](https://github.com/ggml-org/llama.cpp/releases/tag/b10425)).
- **SYCL TILE kernel for quantized KV decode** – +42% to +169% on Intel Battlemage across Qwen3.6-35B, Gemma 4 26B/12B at 32K/118K context, zero regressions ([PR #26689](https://github.com/ggml-org/llama.cpp/pull/26689)).
- **DSA RoPE optimization** – avoid `ggml_concat` in DSA indexer head ([PR #27091](https://github.com/ggml-org/llama.cpp/pull/27091)).
- **Prefill-shaped flash attention perf tests** added to catch regression earlier ([PR #27088](https://github.com/ggml-org/llama.cpp/pull/27088)).

## Stability & Regressions
Top issues reported/updated in the last 24h, ranked by severity:

1. **SYCL completely broken on Intel A770** – crashes with any model on recent build 10428 ([#27063](https://github.com/ggml-org/llama.cpp/issues/27063)). No fix yet; Intel B60 apparently unaffected.
2. **SIGSEGV on GPU offload (resolve_fused_ops false positives)** – affects Intel Lunar Lake iGPU and reproduces on other architectures ([#27046](https://github.com/ggml-org/llama.cpp/issues/27046)).
3. **Gemma4Assistant context init failure** – `llama_init_from_model` fails with Gemma 4 models; 32 👍, still open ([#24343](https://github.com/ggml-org/llama.cpp/issues/24343)).
4. **Windows ROCm 7.14 release missing `hipblas.dll`** – GPUs not detected; `--list-devices` empty ([#26996](https://github.com/ggml-org/llama.cpp/issues/26996)).
5. **DeepSeek-V4-Flash repetition / special-token leaks** in long agentic chats on Metal b10289 ([#26694](https://github.com/ggml-org/llama.cpp/issues/26694)).
6. **SYCL host-pinned memory high CPU utilization** for large allocations ([#27038](https://github.com/ggml-org/llama.cpp/issues/27038)).
7. **ROCm gfx1151 RPC worker crash** in `GGML_OP_TOP_K` during DeepSeek V4 prefill after 4K tokens ([#26746](https://github.com/ggml-org/llama.cpp/issues/26746)).
8. **Vulkan performance drop in recent builds** – still open with 39 comments ([#24066](https://github.com/ggml-org/llama.cpp/issues/24066)).
9. **RPC server NULL-pointer dereference** – unauthenticated remote crash via node id 0 ([#25299](https://github.com/ggml-org/llama.cpp/issues/25299)).
10. **Qwen3.6-27B full re-processing** due to missing cache data, closed but affecting many users ([#22746](https://github.com/ggml-org/llama.cpp/issues/22746)).

Relevant fixes in flight: LoRA file bounds checking ([PR #27056](https://github.com/ggml-org/llama.cpp/pull/27056)), UMA memory detection for AMD iGPUs ([PR #26932](https://github.com/ggml-org/llama.cpp/pull/26932)), HIP UMA override skip ([PR #27083](https://github.com/ggml-org/llama.cpp/pull/27083)), and a batch of MTMD/metadata fixes ([PR #27071](https://github.com/ggml-org/llama.cpp/pull/27071)).

## What This Means for Application Developers
- **Server observability improved** – you can now poll `/metrics` and `/slots` during long decode phases without blocking, which helps with autoscaling and monitoring agents.
- **Reasoning effort is now available to templates** – if you’re relying on prompt-level control of reasoning depth, upgrade and update custom Jinja templates accordingly.
- **Intel/SYCL users should test carefully** – while new fusions deliver solid gains, the A770 regression ([#27063](https://github.com/ggml-org/llama.cpp/issues/27063)) is a blocker for that GPU. Consider staying a few builds back or using Vulkan fallback until resolved.
- **AMD iGPU / Strix Halo users** – upcoming UMA fixes will correct free-VRAM detection and may improve offload decisions. Watch for merge of [#26932](https://github.com/ggml-org/llama.cpp/pull/26932) and [#27083](https://github.com/ggml-org/llama.cpp/pull/27083).
- **New model support is expanding** – MiniMax-Text-01/M1 and Kimi-K3 widen the set of architectures you can serve. If you’ve been holding back on these, llama.cpp is getting close to production-ready support.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

## Ollama Digest — 2026-08-15

**Today's Highlights**

Ollama shipped three releases in the last 24h: v0.32.13 with a Qwen3.8 developer-instruction fix, v0.32.12 adding Qwen 3.8 27B support with Apple Silicon tuning, and v0.32.11 adding DeepSeek Harness and Meta Muse Code via `ollama launch`. The most active engineering effort is around Qwen3.8/agent-client compatibility, with renderer and developer-role fixes landing in [ollama/ollama#17757](https://github.com/ollama/ollama/pull/17757) and [ollama/ollama#17749](https://github.com/ollama/ollama/pull/17749). Meanwhile, several regressions are on the radar: CUDA prefill crashes on qwen3.6:35b, SillyTavern empty responses, and AMD/ROCm/Vulkan device regressions.

### Releases & Breaking Changes

- [v0.32.13](https://github.com/ollama/ollama/releases/tag/v0.32.13) — Qwen3.8 developer-instruction support; hotfix over v0.32.12.
- [v0.32.12](https://github.com/ollama/ollama/releases/tag/v0.32.12) — Adds Qwen 3.8 27B model support; explicitly optimized for Apple Silicon devices.
- [v0.32.11](https://github.com/ollama/ollama/releases/tag/v0.32.11) — `ollama launch dsh` for DeepSeek Harness, `ollama launch muse` for Meta's Muse Code, plus OpenAI-compatible endpoint work.
- No explicit migration notes for these releases. The missing Docker image tag for v0.32.8 ([#17668](https://github.com/ollama/ollama/issues/17668)) was closed.

### New Model & Hardware Support

- **Qwen 3.8 27B** — new in v0.32.12, with GGUF and MLX variants. [ollama/ollama#17745](https://github.com/ollama/ollama/pull/17745) adds the Qwen3.8 renderer and MLX import path; the architecture stays Qwen3.5-based, but the chat template now carries reasoning-effort and preserved-thinking semantics.
- **`ollama launch` integrations** — DeepSeek Harness and Meta Muse Code are now supported. [ollama/ollama#17758](https://github.com/ollama/ollama/pull/17758) adds an `npx` fallback for DeepSeek Harness when no global install is found.
- Apple Silicon Qwen 3.8 27B is the headline hardware optimization in v0.32.12.

### Performance & Optimization

- [ollama/ollama#17752](https://github.com/ollama/ollama/pull/17752) adds a model-metadata cache to avoid re-reading GGUF metadata on every inference call; the PR estimates ~300 ms per-request overhead removed.
- Qwen 3.8 27B on Apple Silicon received targeted performance work in v0.32.12, though no concrete throughput/latency numbers were published.
- [ollama/ollama#17747](https://github.com/ollama/ollama/pull/17747) adds `normalize: false` to `/api/embed`, preserving raw embeddings for pipelines that require unnormalized vectors.

### Stability & Regressions

Ranked by severity:

1. **Ollama Cloud API 503** — [ollama/ollama#17756](https://github.com/ollama/ollama/issues/17756): All requests to the dedicated API endpoint failing with HTTP 503 since Aug 14; not account-specific. No resolution noted.
2. **CUDA illegal memory access in `launch_mul_mat_q` on qwen3.6:35b** — [ollama/ollama#17740](https://github.com/ollama/ollama/issues/17740): Deterministic crash during prefill for prompts ~684 tokens and above; regression between 0.31.2 and 0.32.9. No fix PR yet.
3. **AMD Strix Halo VRAM detection regression in containers** — [ollama/ollama#16462](https://github.com/ollama/ollama/issues/16462): Since 0.30.0-rocm, only 2 GB VRAM is reported instead of full system memory.
4. **AMD Radeon 780M Vulkan regression** — [ollama/ollama#17748](https://github.com/ollama/ollama/issues/17748): 0.32.11 fails with `ErrorDeviceLost` on larger models; works in earlier versions.
5. **Quantized KV cache breaks ROCm tool calling** — [ollama/ollama#17347](https://github.com/ollama/ollama/issues/17347): qwen3.5/qwen3.6 under ROCm stops generating mid-turn instead of emitting tool calls; severity tracks quant precision.
6. **SillyTavern text completion empty responses** — [ollama/ollama#17700](https://github.com/ollama/ollama/issues/17700): Reverting to 0.32.7 restores behavior; no fix PR yet.
7. **Qwen3.8 system/developer-message 500s** — [ollama/ollama#17754](https://github.com/ollama/ollama/issues/17754) and [ollama/ollama#17750](https://github.com/ollama/ollama/issues/17750): Affected `ollama launch claude` and `ollama launch codex`. Fix PRs exist: [ollama/ollama#17757](https://github.com/ollama/ollama/pull/17757) tolerates non-leading system messages, and [ollama/ollama#17749](https://github.com/ollama/ollama/pull/17749) folds developer instructions into the leading system turn.
8. **`/save` fails for nemotron-3.5-lightning** — [ollama/ollama#17735](https://github.com/ollama/ollama/issues/17735): `pull model manifest: file does not exist` despite a valid local manifest.
9. **`/v1/chat/completions` ignores Modelfile temperature** — [ollama/ollama#17744](https://github.com/ollama/ollama/issues/17744): Greedy decode works on `/api/chat` but not on the OpenAI-compatible endpoint; request-level `temperature` is the workaround.
10. **Nemotron3.5-lightning stalls on AMD AI395+** — [ollama/ollama#17692](https://github.com/ollama/ollama/issues/17692): Stalls mid-thinking on Framework Desktop / AMD AI395+.

### What This Means for Application Developers

- If you are using **Qwen3.8 with coding agents** (Claude Code, Codex, Muse), upgrade to v0.32.13. The 500s around system/developer messages should be addressed by the current PR set, but runtime system messages should still be placed at the start of the conversation until those fixes are confirmed.
- Do not rely on **Modelfile `temperature` for OpenAI-compatible `/v1/chat/completions`** calls; pass `temperature` explicitly if you depend on greedy or low-temperature decoding.
- `ollama launch` is becoming the primary agent integration path. DeepSeek Harness and Muse Code are available now; DeepSeek Harness gains an `npx` fallback via [ollama/ollama#17758](https://github.com/ollama/ollama/pull/17758).
- **AMD/ROCm/Vulkan users should pin and validate versions** before upgrading — the Strix Halo container VRAM issue and Radeon 780M Vulkan failure are open regressions.
- For embedding pipelines, [ollama/ollama#17747](https://github.com/ollama/ollama/pull/17747) will add `normalize: false` to `/api/embed`. If you need raw unnormalized embeddings, track that PR.
- Sharded GGUF support is progressing: [ollama/ollama#17743](https://github.com/ollama/ollama/pull/17743) enables pulling multi-file GGUFs from Hugging Face, while the general multi-file import request remains tracked in [ollama/ollama#5245](https://github.com/ollama/ollama/issues/5245).

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

## LiteLLM Digest — 2026-08-15

### 1. Today's Highlights

Activity today is dominated by a severe auth regression introduced by #36837, which locked all Admin UI users out; maintainers opened both a direct revert (#36982) and a more targeted exemption for the reserved UI session team (#36976). Several correctness fixes are also in flight: Anthropic pass-through brotli responses (#36977), Vertex AI `count_tokens` returning zero (#36981), and GPT-5.x `max_tokens:1` being misreported as “model unavailable” (#36859). No new LiteLLM releases were published in the last 24 hours.

### 2. Releases & Breaking Changes

None. No new releases were published in the last 24 hours.

### 3. New Model & Hardware Support

No new model or hardware support landed in this window. Open requests continue for:

- Fireworks AI models via Azure Foundry: [#26618](https://github.com/BerriAI/litellm/issues/26618)
- Ollama text-to-image support in `litellm.image_generation`: [#28026](https://github.com/BerriAI/litellm/issues/28026)
- Xiaomi MiMo `output_config` compatibility with Claude Code: [#24549](https://github.com/BerriAI/litellm/issues/24549)

### 4. Performance & Optimization

No throughput, latency, or kernel-level changes were visible in the PR list. The closest operational improvements are:

- **Shadow eval reverse-direction jobs** to measure router quality regressions after key adoption: [#36865](https://github.com/BerriAI/litellm/pull/36865)
- **Multi-key shadow eval scoping** so one job can evaluate a router across several keys: [#36871](https://github.com/BerriAI/litellm/pull/36871)
- **Redis spend buffer transaction requeue** to avoid losing spend events on DB commit failure: [#33881](https://github.com/BerriAI/litellm/pull/33881)

No concrete benchmark numbers were included.

### 5. Stability & Regressions

Ranked by likely impact:

- **Critical — Admin UI lockout after #36837**: The team-fallback auth change made every UI session appear as a deleted team, causing 404s on all dashboard requests. Fixes: revert [#36982](https://github.com/BerriAI/litellm/pull/36982) and targeted exemption [#36976](https://github.com/BerriAI/litellm/pull/36976).
- **High — Anthropic pass-through brotli corruption**: Forwarding client `Accept-Encoding` upstream means proxies receive brotli bodies they cannot decode, stripping `Content-Encoding` and leaving unreadable JSON. Fix: [#36977](https://github.com/BerriAI/litellm/pull/36977).
- **High — Vertex AI `count_tokens` returns 0**: `acount_tokens()` for Vertex Gemini models never converts `messages` to Gemini `contents`, so the request body is malformed. Fix: [#36981](https://github.com/BerriAI/litellm/pull/36981).
- **Medium — Anthropic `/v1/models` missing nullable token-limit keys**: Entries drop `max_input_tokens`/`max_tokens` when unknown, breaking strict client schemas. Fix: [#36961](https://github.com/BerriAI/litellm/pull/36961).
- **Medium — GPT-5.x `max_tokens:1` treated as model unavailable**: Providers 400 when `max_tokens` fits no visible token; fix returns a length-truncated 200. PR: [#36859](https://github.com/BerriAI/litellm/pull/36859).
- **Medium — MCP logging metadata leaks caller host and upstream headers**: PR drops `host` and configured upstream headers from logged metadata: [#36901](https://github.com/BerriAI/litellm/pull/36901).
- **Medium — Langfuse trace steering could leak team credentials**: `update_trace_keys` now restricts steering keys to real Langfuse trace fields: [#36862](https://github.com/BerriAI/litellm/pull/36862).

Other notable bug reports updated in the last 24h:

- Mid-conversation system-role hoist invalidates prompt-cache prefix: [#36559](https://github.com/BerriAI/litellm/issues/36559)
- Vertex AI custom `api_base` still forces Google credentials: [#19138](https://github.com/BerriAI/litellm/issues/19138)
- Prisma query engine crashes on Windows for LiteLLM 1.82.x/1.83.0: [#25260](https://github.com/BerriAI/litellm/issues/25260)
- `store_prompts_in_spend_logs` still persists empty messages in v1.93.0: [#34747](https://github.com/BerriAI/litellm/issues/34747)
- Tag budgets never reset, causing permanent block after first overage: [#27481](https://github.com/BerriAI/litellm/issues/27481)
- The LiteLLM stability sprint roadmap remains active: [#30484](https://github.com/BerriAI/litellm/issues/30484)

### 6. What This Means for Application Developers

- **If you are on a recent proxy release, validate Admin UI access before upgrading further**; both the revert and the targeted auth fix are still open, so pin to a known-good version until one merges.
- **Vertex AI Gemini users should not trust `count_tokens` today**; expect it to return `total_tokens=0` until [#36981](https://github.com/BerriAI/litellm/pull/36981) merges.
- **Anthropic pass-through users may see compressed/unreadable response bodies** if their upstream defaults to brotli; the fix in [#36977](https://github.com/BerriAI/litellm/pull/36977) is important for proxy deployments where the gateway sits between clients and Anthropic.
- **Strict Anthropic `/v1/models` clients** will reject entries that omit token-limit keys today; upgrade once [#36961](https://github.com/BerriAI/litellm/pull/36961) lands.
- **If you run GPT-5.x agent probes**, do not interpret `400` on `max_tokens:1` as model unavailability; a proper length-truncated 200 is incoming via [#36859](https://github.com/BerriAI/litellm/pull/36859).
- **For router evaluation workflows**, the new shadow-eval improvements ([#36865](https://github.com/BerriAI/litellm/pull/36865), [#36871](https://github.com/BerriAI/litellm/pull/36871)) will make it easier to measure both pre-adoption and post-adoption router quality across multiple keys.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-15

## Today's Highlights
Unsloth shipped **v0.1.800-beta** with local inference and fine-tuning for **Qwen3.8-27B**, including Dynamic GGUF support that runs on 17GB RAM and new NVFP4 quants. On the AMD front, a cluster of fixes landed or entered review: the ROCm AOTriton attention gate (#8821), Windows AMD VRAM reporting via LUID (#8863), and crash containment for native ROCm APU telemetry (#8853). Studio also gained a macOS system-wide Ask bar (#8728) and a fix for embedded MTP performance under partial GPU offload (#8875).

## Releases & Breaking Changes
- **v0.1.800-beta: Qwen3.8-27B** — Local inference for Qwen3.8-27B and Qwen3.8-2.4T via Dynamic GGUFs on 17GB RAM; fine-tuning supported; NVFP4 quants uploaded. [Release](https://github.com/unslothai/unsloth/releases) · [Guide](https://unsloth.ai/docs/models/qwen3.8)
- **CUDA 13.2 warning** — llama.cpp builds on CUDA 13.2 produce gibberish for IQ3_S/IQ3_XXS/IQ2_M quants; use CUDA 12.8/13.0 binaries or Unsloth Studio. [Issue #4849](https://github.com/unslothai/unsloth/issues/4849)
- **Hardened pip/uv policy** — PR #8781 documents that the installer deliberately relaxes operator `require-hashes` / `no-build` / `only-binary` policies for its own dependency installs; worth auditing for locked-down environments. [PR #8781](https://github.com/unslothai/unsloth/pull/8781)

## New Model & Hardware Support
- **Qwen3.8-27B / 2.4T** — Local GGUF inference, Dynamic GGUF on 17GB RAM, fine-tuning, and NVFP4 quantization. [Release](https://github.com/unslothai/unsloth/releases)
- **MLX models via OpenAI-compatible API** — PR #8768 makes MLX models downloaded through Desktop visible to `/v1/models` and chat-completions, fixing a 404 `model_not_found` gap. [PR #8768](https://github.com/unslothai/unsloth/pull/8768)
- **Non-GGUF image/video models in the hub** — PR #8855 removes the `canChat` gate so safetensors image models (e.g., Z-Image-Turbo) get a working Run button. [PR #8855](https://github.com/unslothai/unsloth/pull/8855)
- **Ling 3.0 support requested** — [Issue #8532](https://github.com/unslothai/unsloth/issues/8532)

## Performance & Optimization
- **Embedded MTP partial-offload fix** — Studio produced ~3.5 tok/s on Qwen3.8-27B UD-IQ2_M with default partial offload; PR #8875 fixes embedded MTP head placement. [PR #8875](https://github.com/unslothai/unsloth/pull/8875)
- **ROCm SDPA fallback** — With the AOTriton gate shut, SDPA falls through to MATH, causing finetuning OOM at a fraction of the card's context; fix in PR #8821. [Issue #8819](https://github.com/unslothai/unsloth/issues/8819) · [PR #8821](https://github.com/unslothai/unsloth/pull/8821)
- **Chat UI stream coalescing** — PR #8845 coalesces streamed chunks when the renderer falls behind, preventing the UI from lagging on fast local replies. [PR #8845](https://github.com/unslothai/unsloth/pull/8845)
- **Qwen3.8-27B-NVFP4 slow on RTX 5090/Windows** — Reported "extremely slow inference"; no fix yet. [Issue #8861](https://github.com/unslothai/unsloth/issues/8861)

## Stability & Regressions
Ranked by severity; fix PRs noted where they exist.

1. **Security: wrong IP bound on macOS** — `-H 0.0.0.0` serves the wrong address, exposing an unintended interface. [Issue #8868](https://github.com/unslothai/unsloth/issues/8868)
2. **ROCm AOTriton gate → finetuning OOM** — SDPA degrades to MATH, limiting usable context drastically. Fix in review: [PR #8821](https://github.com/unslothai/unsloth/pull/8821) (also [Issue #8819](https://github.com/unslothai/unsloth/issues/8819))
3. **macOS M4: llama-server fails to start; excessive idle RAM** — Loading local GGUF models broken on Apple Silicon M4/16GB; no fix PR yet. [Issue #8566](https://github.com/unslothai/unsloth/issues/8566)
4. **V1 endpoint & MCP breakage** — Desktop as a v1 endpoint has issues; MCPs broken; troubleshooting report open. [Issue #8790](https://github.com/unslothai/unsloth/issues/8790)
5. **GGUF export regression** — Saving a trained model to GGUF now requires intermediate 16-bit weights (~40GB download); users report this used to be direct. [Issue #8717](https://github.com/unslothai/unsloth/issues/8717)
6. **PDF attachment breaks tool calls** — Attaching a PDF causes generation errors with tools on Windows/ROCm. [Issue #8858](https://github.com/unslothai/unsloth/issues/8858)
7. **Transformers in-place loss crash** — Fatal RuntimeError during CPT/SFT on transformers ≥4.43; fix PR submitted. [PR #8869](https://github.com/unslothai/unsloth/pull/8869)
8. **macOS app second-launch error** — [Issue #8610](https://github.com/unslothai/unsloth/issues/8610)
9. **AMD Strix Halo memory misdetection** — 110GB free GPU memory ignored; model load restricted to 19GB system RAM. [Issue #6834](https://github.com/unslothai/unsloth/issues/6834)
10. **AMD installer/backend mismatch** — Installer reports AMD GPU, backend runs CPU-only with no reconciliation. [Issue #8473](https://github.com/unslothai/unsloth/issues/8473); Fedora/Bazzite installs CPU PyTorch on gfx1201: [Issue #8731](https://github.com/unslothai/unsloth/issues/8731)
11. **Windows install 2-hour cap** — Closed: download of cu126 PyTorch hit the cap with no progress output. [Issue #8698](https://github.com/unslothai/unsloth/issues/8698)
12. **JSONL export fixed** — Raw jsonl not exported as real jsonl, closed. [Issue #8733](https://github.com/unslothai/unsloth/issues/8733)

## What This Means for Application Developers
- **Qwen3.8-27B is now a credible local agent model** — Dynamic GGUF on 17GB RAM plus NVFP4 quants makes it deployable on mid-range machines; watch the RTX 5090/Windows NVFP4 slowness report if you target Blackwell Windows boxes. [Issue #8861](https://github.com/unslothai/unsloth/issues/8861)
- **Tool-calling and MCP flows need pinning** — The V1-endpoint/MCP breakage (#8790), PDF-attachment tool-call corruption (#8858), and a PR preserving pre-tool reasoning in the GGUF tool loop (#8581) indicate the tool path is under active churn. If you build on Studio's tool loop, test against the latest beta and subscribe to #8790. [PR #8581](https://github.com/unslothai/unsloth/pull/8581)
- **AMD ROCm remains a rough edge (Windows and Linux)** — The AOTriton gate (#8819/#8821), VRAM telemetry fixes (#8863, #8853), and CPU-only fallbacks (#8473, #8731) mean production AMD deployments should pin exact ROCm/PyTorch versions rather than trusting auto-detection.
- **On Apple Silicon, MLX serving is maturing** — PR #8768 exposes MLX models through the OpenAI-compatible API, and the macOS Ask bar (#8728) hints at system-level agent surfaces. The M4 llama-server failure (#8566) is the main blocker to verify before relying on Desktop inference on macOS.
- **CUDA 13.2 avoidance for IQ quants** — Pin CUDA 12.8/13.0 or use Unsloth Studio to avoid silent gibberish on IQ2/IQ3 quantization families. [Issue #4849](https://github.com/unslothai/unsloth/issues/4849)

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*