# AI Infrastructure Digest 2026-08-16

> Generated: 2026-08-15 23:14 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project Comparison Report — AI Inference Infrastructure
**Digest date: 2026-08-16**

---

## 1. Ecosystem Overview

The serving stack is consolidating around a handful of hybrid linear-attention architectures — DeepSeek-V4-Flash, Kimi-K3, GLM-5.2, Qwen3.5/3.8, and MiniMax — all of which strain the existing kernel, cache, and speculative-decoding layers. The dominant theme today is **hardening, not feature velocity**: no new releases shipped from vLLM, SGLang, LiteLLM, or Unsloth, while llama.cpp alone pushed nine builds. The highest-severity signals are silent-correctness bugs (undetected attention dropout, silent retrieval corruption on ROCm), followed by a broad cluster of speculative-decoding + structured-output failures across every engine. Teams are being explicitly advised to treat several hardware/model combinations as **unsafe for production** until targeted fixes land.

## 2. Activity Comparison

Counts below reflect issues/PRs surfaced in today's digests (not total repository volume).

| Project | Issues Surfaced | PRs Surfaced | Release Status |
|---|---|---|---|
| **vLLM** | 11 (1 silent-corruption, 3 crash-class) | 11 | No release in 24h |
| **SGLang** | 17 (3 critical: silent dropout ×2, DSPARK corruption) | 15 | No release in 24h |
| **llama.cpp** | 16+ (SYCL/Vulkan breakage, KV regressions) | 23+ | **Active** — 9 builds (b10436–b10448) |
| **Ollama** | 14+ (CUDA IMA, system-message 500s, GPU regressions) | 7 | **v0.32.14-rc0** shipped |
| **LiteLLM** | 18 (incl. 3 security findings) | 9 | No release; v1.96.2 upgrade hazard (FastAPI pin) |
| **Unsloth** | 56 updated | 69 updated | No release in 24h |

**Read:** llama.cpp is the clear momentum leader on release cadence and model breadth; vLLM and SGLang are in stabilization mode with high issue-to-fix ratios on DeepSeek-V4-Flash; Ollama shipped a targeted rc to fix a Qwen agent-breaking regression; LiteLLM's digest is dominated by gateway correctness/security debt.

## 3. Model Support Race

| Model / Architecture | vLLM | SGLang | llama.cpp | Ollama | Unsloth |
|---|---|---|---|---|---|
| **DeepSeek-V4-Flash** | Stabilizing (ROCm corruption #52109, H20/H100 crashes) | Stabilizing + TRT-LLM attention for SM100/103 (#30805) | SWA KV-cache exhaustion (#25452) | Requested (#17510) | — |
| **Kimi-K3 (hybrid KDA/MLA)** | — | AITER prefill on AMD (#34837); DSPARK crashes | **Shipped** b10448 (#26185) | — | — |
| **GLM-5.2 TurboQuant MLA** | **Shipped** backend (#52472) | — | — | Requested (#17741) | — |
| **MiniMax-Text-01 / M1** | — | MiniMax-H3 diffusion fixes | **Shipped** b10437 (#27018) | — | — |
| **Qwen3.5/3.8 hybrid** | — | — | Conversion fix (#27132) | Qwen 500s, CUDA IMA, MTP regression | Qwen3.8-27B GGUF training (#8875) |
| **New hardware** | Rubin sm_107 CI (#52379) | Blackwell SM100/103; AMD ROCm 7.2 | Maple ternary MoE (#27000), TML Inkling (#25731) | — | Intel GPU requested (#8931) |

**Who is ahead:** **llama.cpp** is fastest at shipping *new model families* end-to-end (Kimi-K3, MiniMax in a single day), reflecting its lower integration barrier. **SGLang** leads on *kernel enablement for new architectures* across AMD/Blackwell/NPU. **vLLM** leads on *production feature depth* (DCP/PP/MTP for sparse MLA, TurboQuant) but is paying for it in regression surface. Ollama and LiteLLM are followers here — Ollama waits for llama.cpp upstream; LiteLLM only tracks cost-map entries.

## 4. Performance Frontier

Optimization effort is concentrated in five areas:

- **Sparse / hybrid attention kernels (MLA, KDA, NSA):** vLLM's gfx942 FP8 MFMA sparse-attention indexer (#52402), SGLang's AMD Fast Triton Sparse MLA (#30575) and 128-bit HiCache gather (+32%, #30024), llama.cpp's mixed K/V-type flash-attention fix (#27150) and small-KV-quant prefill fix (#27140).
- **Speculative decoding:** vLLM suffix GPU drafter for async scheduling (#52097) + in-progress NGram GPU speculator (#40704); SGLang GDN verification avoids QKV materialization (#33778); llama.cpp reworked server-side spec-decode threading (#27133) and supports MTP assistant loading (#24431). Still the most failure-prone subsystem in the ecosystem — Ollama reports MTP variants **2× slower** on Apple Silicon (#17776).
- **Quantized KV cache:** llama.cpp SYCL TILE quantized-KV decode (+42–169% on Battlemage, #26689); Unsloth fixes silent q8_0→f16 KV replacement under tensor split (#8939); vLLM requests PTX `ldmatrix.s8.s4` for W4A8-INT8 (#49529). Correctness risk is high — llama.cpp warns of a **~30× prefill fallback** when K/V types mismatch.
- **Scheduling and offloading:** vLLM KV-cache offload back-pressure (#50045); SGLang PD-disaggregation is showing **no throughput gain** on H200 with 32K input (#24488) — an open challenge to the architecture.
- **Fine-tuning data efficiency:** Unsloth's `max_steps` preprocessing fix (#8890) — one run spent 11m14s tokenizing vs. 1m54s training. This is the kind of win that compounds across the ecosystem.

## 5. Layer Positioning

| Project | Layer | Core Value Proposition | Today's Bottleneck |
|---|---|---|---|
| **vLLM** | High-throughput serving engine | Production multi-GPU serving, DCP/PP/MTP, deep kernel control | ROCm correctness, large-batch EngineCore crashes |
| **SGLang** | Serving engine (radix-cache focused) | Fastest adoption of new architectures; strong AMD/Blackwell kernels | DSPARK correctness, silent attention dropout at >65K extends |
| **llama.cpp** | Local/edge runtime + GGUF ecosystem | Broadest hardware (CUDA/SYCL/Vulkan/Metal/ROCm) and fastest model support | SYCL/Vulkan instability, CLI churn, SWA prompt reprocessing |
| **Ollama** | End-user local runtime (wraps llama.cpp/MLX) | Distribution, model management, desktop experience | Downstream regressions from upstream, older-GPU breakage |
| **LiteLLM** | LLM gateway / proxy | Multi-provider routing, spend tracking, guardrails | Translation-layer bugs, security debt, spend-accounting gaps |
| **Unsloth** | Fine-tuning framework + Studio desktop app | Fast training, quantized fine-tunes, GGUF export | GGUF export workflow regression (40GB intermediate), Studio streaming bugs |

The stack is cleanly layered: **Unsloth produces models → llama.cpp/Ollama serve them locally → vLLM/SGLang serve them at scale → LiteLLM routes and bills.** The seams are where the pain lives (GGUF export, tool-call parsing, spec-decode interactions).

## 6. Trend Signals

1. **DeepSeek-V4-Flash is the industry's stress test — and it's failing on some hardware.** Silent retrieval corruption on ROCm MI325X (vLLM #52109), sparse-prefill crashes on H20/H100, and DSPARK identifier corruption (SGLang #34959) mean **hardware-specific "known bad" matrices are now required** for production deployment. This is the new normal for frontier open-weight models.

2. **Hybrid linear-attention is winning the architecture race, and the kernel ecosystem is scrambling.** Kimi-K3, Qwen3.5/3.8, GLM-5.2, MiniMax, and DeepSeek-V4 all mix linear/KDA attention with sparse MLA. The competitive moat is shifting from model quality to **who can serve sparse attention reliably at long context** — and today, nobody fully can (attention dropout at >65K extends, SWA KV exhaustion, gfx942 corruption).

3. **Speculative decoding + structured output is the most fragile combination in the stack.** Livelocks (vLLM #49210), divergence on quantized targets (llama.cpp #25618), CUDA-graph crashes (SGLang #34974), and 2× slowdowns (Ollama #17776) span every project. **Agent/application developers should treat spec-decode + grammar as opt-in, not default.**

4. **Correctness is the new performance.** The top-severity items today are silent: corrupted retrieval, dropped attention kernels, quietly dropped `weight_scale`, zero-token-usage accounting. Infrastructure buyers should prioritize **validation harnesses and canary testing** over throughput claims.

5. **Gateway security hygiene is lagging serving-engine maturity.** LiteLLM's read-only survey surfaced no-auth proxy defaults, SSRF/provider-key exfiltration via `api_base`, and a `temp_budget_increase` budget bypass. Multi-tenant operators should audit gateway configuration independently of feature upgrades.

6. **The local/edge tier is churning fast with migration costs.** llama.cpp's `--load-mode` CLI break, Ollama regressions on Pascal/older AMD GPUs, and SYCL/Vulkan instability mean local deployments need **pinning discipline** — upstream moves fast but not always safely.

7. **Fine-tuning workflows are getting cheaper to iterate but more expensive to ship.** Unsloth's preprocessing fix cuts short-run wall-clock ~5–6×, but GGUF export now demands a ~40GB 16-bit intermediate download. **CI pipelines that produce GGUF artifacts need re-budgeting.**

**Bottom line for technical decision-makers:** if you serve DeepSeek-V4-Flash, verify your exact GPU/context-length combination against the open issue trackers before rollout. If you build agent workflows, disable speculative decoding with structured outputs for now. If you operate a gateway, audit auth and budget-bypass paths this week. The ecosystem is advancing fastest at the kernel and model-support edges, but the reliability layer is not keeping pace — plan for a quarter of stabilization, not innovation.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

## 1. Today’s Highlights

No new releases landed in the last 24 hours. Activity is concentrated on stabilizing **DeepSeek-V4-Flash** serving across ROCm/H100/H20: new high-severity reports cover silent retrieval corruption on gfx942, an H20 sparse-prefill crash, and an H100 EngineCore crash with large batches (#52109, #52339, #51743). On the fix side, notable PRs landed for ROCm gfx942 sparse-attention performance (#52402), Quark INT4 config handling (#52474), Mamba cache fallback under Model Runner V2 (#52460), and an initial CUDA 13.4/Rubin CI image pipeline (#52379).

## 2. Releases & Breaking Changes

- **No new releases in the last 24h.** No version changes, deprecations, or migration notes to report.

## 3. New Model & Hardware Support

- **[PR #52379]** Adds a CUDA 13.4 prerelease image pipeline for **Rubin (`sm_107`)**, including PyPI CUDA toolkit overlays and compatible PyTorch nightlies — with driver stub preservation.
- **[PR #47779]** Enables **DCP for FlashInfer sparse MLA decode on SM120** (Blackwell), expanding decode context parallelism support.
- **[PR #52472]** Adds a **GLM-5.2 TurboQuant sparse MLA backend** with packed 4-bit latent KV storage, fused sparse decode/prefill, and DCP/MTP/PP support.
- **[Issue #52181]** Surfaces a limitation: **FlashAttention 2 paths still require compute capability ≥ 8**, blocking some Volta/Turing GPUs (e.g., Quadro RTX 8000) for newer Qwen models.

## 4. Performance & Optimization

- **[PR #52402]** Optimizes the **gfx942 (MI300X/MI325X) DeepSeek-V4 sparse-attention indexer** with native FP8 MFMA and a corrected LDS occupancy gate. This is in-progress work targeting the ROCm corruption/perf issues reported in #52109.
- **[PR #52097]** Adds a **suffix GPU drafter for async scheduling**, allowing model-free suffix decoding to benefit from CPU/GPU overlap in high-concurrency agentic workloads.
- **[PR #40704]** In-progress **ModelRunner V2 NGram GPU speculator** implementation, moving NGram proposal fully onto GPU.
- **[PR #50045]** Adds **back-pressure detection to the KV cache offloading manager**, preventing unbounded job growth when secondary tiers (disk/shared storage/P2P) become slow.
- **[Issue #49529]** Feature request to adopt **PTX 9.4 `ldmatrix.s8.s4`** for W4A8-INT8 paths — an in-flight INT4→INT8 expanding load that could reduce kernel complexity and improve throughput.
- No concrete benchmark numbers were posted in the last 24h.

## 5. Stability & Regressions

Ranked by severity:

- **Silent correctness corruption — ROCm gfx942 DeepSeek-V4-Flash**: [#52109](https://github.com/vllm-project/vllm/issues/52109) reports silent retrieval corruption for prompts ≥ ~4-5K tokens on MI325X. This is the most dangerous class of bug — incorrect output without an explicit error. A targeted ROCm fix is open in [#52402](https://github.com/vllm-project/vllm/pull/52402).
- **Sparse prefill crash at long context — H20/TP8**: [#52339](https://github.com/vllm-project/vllm/issues/52339) — DeepSeek-V4-Flash FlashMLA sparse prefill crashes at ~161K context in `phase1.cuh` on 8×H20-3e.
- **EngineCore crash with large batches — H100**: [#51743](https://github.com/vllm-project/vllm/issues/51743) — `--max-num-batched-tokens >= 24576` crashes in fused qnorm/rope/kv-insert; allocation is invisible to the memory profiler.
- **Engine livelock / hang — MTP + structured output**: [#49210](https://github.com/vllm-project/vllm/issues/49210) — spec decode plus xgrammar causes 100% CPU livelock; regression from v0.24.0.
- **MRv2 startup crash — Mamba prefix caching**: [#52317](https://github.com/vllm-project/vllm/issues/52317) — `--enable-prefix-caching` with `mamba_cache_mode 'all'` and DSpark/DFlash crashes at startup. Fix available in [#52460](https://github.com/vllm-project/vllm/pull/52460).
- **Quark INT4 loading failure**: [#52454](https://github.com/vllm-project/vllm/issues/52454) — new structured Quark configs break `WeightsMapper` list handling. Fix in [#52474](https://github.com/vllm-project/vllm/pull/52474), already closed.
- **DSv4 metadata packing regression**: [#51318](https://github.com/vllm-project/vllm/pull/51318) — reverts adaptive C128A metadata packing that interacts badly with CUDA graph capture; pending merge.
- **EngineCore hangs without timeout**: [#52247](https://github.com/vllm-project/vllm/issues/52247) — `copy_event.synchronize()` blocks forever when a GPU kernel never terminates.
- **FP8 block-scale failure on sm120**: [#51884](https://github.com/vllm-project/vllm/issues/51884) — DeepGEMM “Unknown SF transformation” on RTX 5090 during weight loading.
- **Environment/package issues**: [#52300](https://github.com/vllm-project/vllm/issues/52300) reports `libcudart.so.13` import failure when using `vllm==0.21.0` with CUDA 12.6; likely a wheel/CUDA mismatch.
- **Gemma 4 parser defaults**: [#52410](https://github.com/vllm-project/vllm/issues/52410) — parser defaults `enable_thinking=true` while chat template defaults false. A multimodal frame-count fix PR is also open ([#52441](https://github.com/vllm-project/vllm/pull/52441)).

## 6. What This Means for Application Developers

- **DeepSeek-V4-Flash on ROCm should be treated as unsafe for production correctness today** — especially on gfx942 with longer prompts. Track [#52109](https://github.com/vllm-project/vllm/issues/52109) and the associated fix before rolling out.
- If you run DeepSeek-V4-Flash on H100/H20/TP8, avoid large `--max-num-batched-tokens` (>24K) and very long prefill until [#51743](https://github.com/vllm-project/vllm/issues/51743) and [#52339](https://github.com/vllm-project/vllm/issues/52339) are resolved.
- **Structured output + speculative decoding remains fragile** across reasoning models (livelock, grammar bypass at reasoning boundaries). Consider disabling spec decode when using xgrammar for agentic workloads.
- **Quark INT4 models with structured quantization configs** should become loadable again after PR #52474 lands in a release.
- **Mamba hybrid models with prefix caching + MRv2 features** (spec decode, context parallelism) may crash at startup; PR #52460 provides a fallback, but avoid combining those features until the next release.
- No new release means the safest current approach is to pin a known-good version and cherry-pick specific fixes (e.g., #52474, #52460) rather than upgrading blindly.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

## Today's Highlights

SGLang did not ship a new release in the last 24h, but activity was heavy across correctness hardening, unified cache fixes, and AMD/Blackwell kernel enablement. The most urgent signals are two open reports of **silent attention dropout on DSA/TRT-LLM prefill for extends >65,535 tokens** ([#34947](https://github.com/sgl-project/sglang/issues/34947), [#34941](https://github.com/sgl-project/sglang/issues/34941)) and a string of DSPARK speculative-decoding correctness/crash bugs ([#34959](https://github.com/sgl-project/sglang/issues/34959), [#34974](https://github.com/sgl-project/sglang/issues/34974)). CI tracking shows 3 broken, 11 flaky, and 669 recently fixed tests ([#17050](https://github.com/sgl-project/sglang/issues/17050)).

## Releases & Breaking Changes

None in the last 24h.

## New Model & Hardware Support

- **DeepSeek-V4 TRT-LLM attention for SM100/103** is in progress: [PR #30805](https://github.com/sgl-project/sglang/pull/30805).
- **Kimi-Linear/KDA route through native Cake kernels** is being enabled, depending on FlashInfer native kernels: [PR #34946](https://github.com/sgl-project/sglang/pull/34946), [PR #34299](https://github.com/sgl-project/sglang/pull/34299).
- **AMD Fast Triton Sparse MLA backend** for DSA prefill/decode: [PR #30575](https://github.com/sgl-project/sglang/pull/30575).
- **Kimi K3 AITER prefill kernel on AMD** with 12-head support: [PR #34837](https://github.com/sgl-project/sglang/pull/34837).
- **Native Hunyuan3D Paint and Delight models** replacing Diffusers-owned modules: [PR #34980](https://github.com/sgl-project/sglang/pull/34980).
- **LTX-2/2.3 NPU inference optimization**: [PR #34722](https://github.com/sgl-project/sglang/pull/34722).
- **ROCm 7.2 nightly GPT-OSS perf coverage** added: [PR #34645](https://github.com/sgl-project/sglang/pull/34645).

## Performance & Optimization

- **KDA path gains zero-copy native prefill checkpoints and packed decode**: [PR #34299](https://github.com/sgl-project/sglang/pull/34299), with Kimi-Linear routing through native Cake kernels in [PR #34946](https://github.com/sgl-project/sglang/pull/34946).
- **GDN speculative verification avoids materializing QKV tensors**: [PR #33778](https://github.com/sgl-project/sglang/pull/33778).
- **AMD HiCache MLA gather widened to 128-bit, +32% on the gather kernel**, and `block_quota` now honors `SGLANG_HICACHE_BLOCK_QUOTA` at runtime: [PR #30024](https://github.com/sgl-project/sglang/pull/30024).
- **Profiling/execution-step annotations** continue for deeper performance analysis: [PR #24911](https://github.com/sgl-project/sglang/pull/24911).
- Open concern: **PD disaggregation on H200 shows no throughput gain** over single-node deployment with 32k input / 512 output ([#24488](https://github.com/sgl-project/sglang/issues/24488)).

## Stability & Regressions

Ranked by severity:

1. **Silent loss of attention on DSA/TRT-LLM sparse-MLA prefill** for a single unchunked extend >65,535 tokens on SM100 — zero attention kernels launch, no error reported. Duplicated reports: [#34947](https://github.com/sgl-project/sglang/issues/34947), [#34941](https://github.com/sgl-project/sglang/issues/34941). No fix PR yet.
2. **DSPARK correctness and crash cluster**:
   - Silently corrupts identifiers on DeepSeek-V4-Flash, making speculative decoding unsafe: [#34959](https://github.com/sgl-project/sglang/issues/34959).
   - `--enable-eplb + DSPARK` crashes during draft CUDA graph capture: [#34974](https://github.com/sgl-project/sglang/issues/34974).
   - Kimi K3 decode crash with PD disaggregation + DCP + DSPARK: `cumsum(extend_prefix_lens=None)`: [#34920](https://github.com/sgl-project/sglang/issues/34920).
   - Folded NPU path parity fix in progress: [PR #34944](https://github.com/sgl-project/sglang/pull/34944).
3. **Scheduler crash on mixed batches using `token_ids_logprob`** (`AttributeError: 'list' object has no attribute 'tolist'`), affecting v0.5.14–v0.5.17: [#34719](https://github.com/sgl-project/sglang/issues/34719).
4. **FP8 `lm_head` weight_scale silently dropped** with compressed-tensors, causing degenerate repetition: [#34895](https://github.com/sgl-project/sglang/issues/34895).
5. **Hierarchical cache / HiCache issues**:
   - Host-pool memory check ignores reserved HugePages, causing false “Not enough host memory” failures: [#34972](https://github.com/sgl-project/sglang/issues/34972).
   - HF3FS HiCache ZeroDivisionError with DeepSeek-V4 logical KV anchor: [#34969](https://github.com/sgl-project/sglang/issues/34969).
   - Unified-cache load-back pin assertion under device-eviction cycling; open fix in [PR #34975](https://github.com/sgl-project/sglang/pull/34975).
6. **MiniMax-H3 diffusion serving**:
   - `quality:"high"` is not gated against Turbo-LoRA-substituted DiT weights; fix PR opens a fail-closed audit: [#34954](https://github.com/sgl-project/sglang/issues/34954), [PR #34978](https://github.com/sgl-project/sglang/pull/34978).
   - Resident serving stages a full DiT copy per rank through host RAM (~57 GB/rank), with silent SIGKILL below threshold: [#34902](https://github.com/sgl-project/sglang/issues/34902).
7. **Tool-call parser reliability**:
   - Kimi-K3 parser fails ~190×/24h in production (`TypeError` / JSON parse errors): [#34604](https://github.com/sgl-project/sglang/issues/34604).
   - Streaming chunk boundaries lose or corrupt tool-call data across detectors: [#31915](https://github.com/sgl-project/sglang/issues/31915).
   - Responses API `input_image` parts in `function_call_output` not converted to `image_url`: [#34927](https://github.com/sgl-project/sglang/issues/34927).
8. **By-stage profiler freezes serving ~25s under speculative decoding** when stopped; deferred stop leaks into later requests: [#34943](https://github.com/sgl-project/sglang/issues/34943), [#34942](https://github.com/sgl-project/sglang/issues/34942).
9. **CI/stability infrastructure**: SWA eviction frontier fix for bigram keys in [PR #34870](https://github.com/sgl-project/sglang/pull/34870); overall CI tracking in [#17050](https://github.com/sgl-project/sglang/issues/17050).

## What This Means for Application Developers

- **Do not rely on DSA/TRT-LLM prefill for single >65,535-token extends** on SM100 until the `gridDim.z` guard lands; chunk long inputs explicitly.
- **Treat DSPARK as unsafe for DeepSeek-V4-Flash and untested combinations** with EPLB/DCP — disable it in production until the crash and corruption reports are resolved.
- **Kimi-K3 tool-call streaming remains brittle**; build in parser fallbacks/retries if you depend on structured tool calls.
- **MiniMax-H3 resident serving** requires careful host RAM budgeting; the documented RTX 5090 recipe is not enough guidance for multi-rank deployments.
- **Unified radix cache work is moving fast** ([roadmap #20415](https://github.com/sgl-project/sglang/issues/20415), [PR #34870](https://github.com/sgl-project/sglang/pull/34870)); expect behavior changes around hierarchical cache and eviction semantics in upcoming releases.
- **No new release today**; if you are on v0.5.14–v0.5.17 and use `token_ids_logprob`, avoid mixed batches or pin to a patched build once [#34719](https://github.com/sgl-project/sglang/issues/34719) is fixed.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-16

## Today's Highlights

- **Major new model support landed**: `b10448` adds Kimi-K3 text model support ([#26185](https://github.com/ggml-org/llama.cpp/pull/26185)), and `b10437` adds MiniMax-Text-01 / MiniMax-M1 support ([#27018](https://github.com/ggml-org/llama.cpp/pull/27018)).
- **Server speculative decoding was reworked**: `b10447` redesigns the `yield_to_queue` thread model and moves `common_speculative_process` into a worker thread ([#27133](https://github.com/ggml-org/llama.cpp/pull/27133)).
- **CLI migration warning**: `b10441` replaces deprecated `--mmap` / `--no-mmap` / `--mlock` / `--direct-io` with a unified `--load-mode` flag ([#26934](https://github.com/ggml-org/llama.cpp/pull/26934)).

## Releases & Breaking Changes

- **b10448** — Add Kimi-K3 text model: hybrid KDA (linear) + MLA (full) attention, cross-layer residual attention, and latent MoE ([#26185](https://github.com/ggml-org/llama.cpp/pull/26185)).
- **b10447** — Server: re-design `yield_to_queue` thread model; speculative decoding now runs via worker thread ([#27133](https://github.com/ggml-org/llama.cpp/pull/27133)).
- **b10446** — Vendor: update BoringSSL to `0.20260813.0` ([#27099](https://github.com/ggml-org/llama.cpp/pull/27099)).
- **b10444** — `common`: `--models-dir` can now load MTP assistant models; preset handling updated for other draft types, EAGLE3 dropped ([#24431](https://github.com/ggml-org/llama.cpp/pull/24431)).
- **b10443** — Fix: check GGUF array type before reading ([#27075](https://github.com/ggml-org/llama.cpp/pull/27075)).
- **b10442** — Vulkan: add `SHMEM_STRIDE_PAD` / `APPLY_SLM_A_RESHAPE` for `coopmat mul_mm` on Intel Xe ([#25380](https://github.com/ggml-org/llama.cpp/pull/25380)).
- **b10441** — **Breaking CLI change**: `--mmap`, `--no-mmap`, `--mlock`, and `--direct-io` migrated to `--load-mode` ([#26934](https://github.com/ggml-org/llama.cpp/pull/26934)).
- **b10437** — Add MiniMaxText01ForCausalLM and MiniMaxM1ForCausalLM; includes logits-mask handling for zero-valued embedding rows ([#27018](https://github.com/ggml-org/llama.cpp/pull/27018)).
- **b10436** — `mtmd`, `common`: various fixes ([#27071](https://github.com/ggml-org/llama.cpp/pull/27071)).

## New Model & Hardware Support

- **Kimi-K3 text model** — hybrid KDA/MLA attention with cross-layer residual attention and latent MoE ([#26185](https://github.com/ggml-org/llama.cpp/pull/26185)).
- **MiniMax-Text-01 / MiniMax-M1** — new model family support ([#27018](https://github.com/ggml-org/llama.cpp/pull/27018)).
- **Maple 20B-A1B ternary MoE** — open PR adds CPU support: 24 layers, 256 experts, 8 active, SWA-512 + global attention, TQ1_0/TQ2_0 ([#27000](https://github.com/ggml-org/llama.cpp/pull/27000)).
- **TML Inkling architecture** — open PR adds safetensors-to-GGUF converter, graph build, kernel changes, and banded flash attention ([#25731](https://github.com/ggml-org/llama.cpp/pull/25731)).
- **Qwen3.5 hybrid linear-attention conversion fix** — `ssm_conv1d` kernel dims and `in_proj_a/b` layout corrected ([#27132](https://github.com/ggml-org/llama.cpp/pull/27132)).
- **MTP / speculative draft loading** — `--models-dir` MTP assistant loading merged ([#24431](https://github.com/ggml-org/llama.cpp/pull/24431)); support for `speculators`-format checkpoints in progress ([#26275](https://github.com/ggml-org/llama.cpp/pull/26275)).
- **ROCm 7.14 Docker build support** — open PR adds newer GPU targets and a workaround for “no usable GPU found” ([#27145](https://github.com/ggml-org/llama.cpp/pull/27145)).
- **SYCL MKL FA env var cleanup** — `GGML_SYCL_ENABLE_MKL_FA` read once at startup ([#26863](https://github.com/ggml-org/llama.cpp/pull/26863)).

## Performance & Optimization

- **CUDA: allow mixed K/V types in flash attention** — currently, different `-ctk` / `-ctv` types disable FA and fall back to CPU attention, making prefill roughly 30x slower. Fix open in ([#27150](https://github.com/ggml-org/llama.cpp/pull/27150)).
- **CUDA: slow prefill on small KV quants** — open fix for quantized KV cache hitting very low prefill throughput ([#27140](https://github.com/ggml-org/llama.cpp/pull/27140)).
- **SYCL: TILE kernel for quantized KV decode** — measured **+42% to +169%** decode improvement on Battlemage for Qwen3.6-35B, Gemma 4 26B, and Gemma 4 12B at 32K/118K context ([#26689](https://github.com/ggml-org/llama.cpp/pull/26689)).
- **SYCL: Q4_K multi-column MMVQ optimization** — removes redundant weight reconstruction in destination columns ([#27062](https://github.com/ggml-org/llama.cpp/pull/27062)).
- **Vulkan: tiled transpose for `0<->2` permuted CONT** — avoids slow generic strided copy path ([#26585](https://github.com/ggml-org/llama.cpp/pull/26585)).
- **Vision warmup** — open PR performs a real dummy-image encode during warmup to avoid one-time vision-encoder init on first request ([#27152](https://github.com/ggml-org/llama.cpp/pull/27152)).
- **Metal performance on AMD discrete GPUs** — open PR reports ~5.3 → 60.4 t/s on AMD Radeon Pro 5300M ([#19527](https://github.com/ggml-org/llama.cpp/pull/19527)).
- **FR-Spec-style draft-vocab trimming** — research/implementation in progress for MTP speculative decoding ([#25187](https://github.com/ggml-org/llama.cpp/issues/25187)).
- **Server memory observability** — per-device memory usage exposed on `/metrics` and `/memory` in progress ([#26130](https://github.com/ggml-org/llama.cpp/pull/26130)).

## Stability & Regressions

Active issues updated in the last 24h, ranked roughly by severity:

1. **SYCL completely broken on Intel A770** — crashes with any model; reportedly works on Intel B60 ([#27063](https://github.com/ggml-org/llama.cpp/issues/27063)).
2. **Vulkan `DeviceLostError` on Linux 7.x / Strix Halo** — `vk::Queue::submit` device lost ([#25664](https://github.com/ggml-org/llama.cpp/issues/25664)).
3. **DSV4-Flash SWA KV-cache exhaustion** — churned-reuse causes crash/stall ([#25452](https://github.com/ggml-org/llama.cpp/issues/25452)).
4. **SYCL `MUL_MAT_ID` prefill produces wrong results on Arc Pro B70** — causes garbage output on MoE models, 28/792 backend-op failures ([#25455](https://github.com/ggml-org/llama.cpp/issues/25455)).
5. **Vulkan performance drop in recent builds** — 40 comments, still open ([#24066](https://github.com/ggml-org/llama.cpp/issues/24066)).
6. **Server forces full prompt reprocessing on subsequent requests** — SWA/recurrent memory error ([#21831](https://github.com/ggml-org/llama.cpp/issues/21831)).
7. **CUDA 4-bit KV cache collapses prefill to ~34 t/s on Qwen3.5 hybrid** — likely related to mixed K/V FA fallback ([#27109](https://github.com/ggml-org/llama.cpp/issues/27109)); fix PRs open ([#27140](https://github.com/ggml-org/llama.cpp/pull/27140), [#27150](https://github.com/ggml-org/llama.cpp/pull/27150)).
8. **Vision not working with Qwen 27B 3.6/3.8 on AMD AI Max** ([#27124](https://github.com/ggml-org/llama.cpp/issues/27124)).
9. **Glimmer memory/prefill regression after first mmproj use** — memory grows and prefill slows ([#26873](https://github.com/ggml-org/llama.cpp/issues/26873)).
10. **Speculative decoding (MTP/DSpark) diverges from vanilla greedy on quantized targets** — matches on bf16 only ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618)).
11. **Qwen3-Coder parser lazy tool-call trigger never fires** — model can skip both `<tool_call>` and `<function=`, breaking tool orchestration ([#26987](https://github.com/ggml-org/llama.cpp/issues/26987)).
12. **`reasoning_effort` seems broken** ([#27023](https://github.com/ggml-org/llama.cpp/issues/27023)).
13. **Latest Windows ROCm build not using GPU** ([#26964](https://github.com/ggml-org/llama.cpp/issues/26964)).
14. **Windows BLAS compilation with AOCL fails** — OpenBLAS works as a workaround ([#25413](https://github.com/ggml-org/llama.cpp/issues/25413)).
15. **llama-bench prefill crashes on MoE/SSM models** — `ggml_cuda_mul_mat_q` / `mm_ids_helper` regression ([#24937](https://github.com/ggml-org/llama.cpp/issues/24937)).
16. **Windows Defender false positive** on `b10195-bin-win-cpu-x64` ([#26343](https://github.com/ggml-org/llama.cpp/issues/26343)).

Closed/stale items also surfaced: Vulkan device-loss on RX 5700 XT ([#18962](https://github.com/ggml-org/llama.cpp/issues/18962)), `llama-cli > file.txt` output regression ([#19256](https://github.com/ggml-org/llama.cpp/issues/19256)), Qwen3 ROCm/HIP gfx906 segfault ([#17586](https://github.com/ggml-org/llama.cpp/issues/17586)).

## What This Means for Application Developers

- **Update your launch scripts**: `--load-mode` replaces `--mmap`, `--no-mmap`, `--mlock`, and `--direct-io` ([#26934](https://github.com/ggml-org/llama.cpp/pull/26934)).
- **If you run SWA/recurrent or hybrid models in production, test server behavior carefully**: full prompt reprocessing ([#21831](https://github.com/ggml-org/llama.cpp/issues/21831)) and the new speculative thread model ([#27133](https://github.com/ggml-org/llama.cpp/pull/27133)) can affect latency.
- **Avoid mixed quantized K/V types on CUDA today**; wait for the flash-attention K/V type fix ([#27150](https://github.com/ggml-org/llama.cpp/pull/27150)) or the small-KV-quant prefill fix ([#27140](https://github.com/ggml-org/llama.cpp/pull/27140)).
- **If you serve Qwen3.5 hybrid linear-attention models**, make sure conversion uses the corrected tensor layouts from ([#27132](https://github.com/ggml-org/llama.cpp/pull/27132)).
- **Agent/tool-call developers**: watch Qwen3-Coder lazy tool-call handling ([#26987](https://github.com/ggml-org/llama.cpp/issues/26987)) and `reasoning_effort` behavior ([#27023](https://github.com/ggml-org/llama.cpp/issues/27023)).
- **Vision-serving latency should improve** once warmup initializes the vision encoder during startup instead of first request ([#27152](https://github.com/ggml-org/llama.cpp/pull/27152)).
- **For speculative decoding deployments**, MTP/draft-model loading via `--models-dir` is now available ([#24431](https://github.com/ggml-org/llama.cpp/pull/24431)), and `speculators`-format checkpoints are being added ([#26275](https://github.com/ggml-org/llama.cpp/pull/26275)).
- **Observability/API work to track**: per-device memory metrics ([#26130](https://github.com/ggml-org/llama.cpp/pull/26130)), OpenAI Responses API JSON schema support ([#26013](https://github.com/ggml-org/llama.cpp/pull/26013)), and hidden-state extraction API proposal ([#27073](https://github.com/ggml-org/llama.cpp/pull/27073)).

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

## Today's Highlights

Ollama shipped **v0.32.14-rc0**, adding WebP image transcoding for llama-server and a Qwen renderer fix for non-leading system messages — the latter addressing a cluster of 500 errors breaking Qwen3.8-based agent/tool workflows. The most severe open regressions remain Qwen3.6 CUDA illegal-memory-access with JSON-schema + `think:false`, and several AMD/NVIDIA backend regressions from 0.32.11+. A pending performance PR promises to remove ~300ms of per-request GGUF re-parsing overhead.

## Releases & Breaking Changes

- **[v0.32.14-rc0](https://github.com/ollama/ollama/releases/tag/v0.32.14-rc0)** — `llm: transcode WebP images for llama-server` and `renderers/qwen: tolerate non-leading system messages`. No migration notes; likely fixes Qwen system-message 500s.
- Closed dependency bumps: **[llama.cpp update #17760](https://github.com/ollama/ollama/pull/17760)** and **[MLX update #17761](https://github.com/ollama/ollama/pull/17761)**.

## New Model & Hardware Support

No new model binaries were published in the last 24h, but several model requests are open:

- [deepseek-v4-flash:0731 #17510](https://github.com/ollama/ollama/issues/17510)
- [DeepSeek V4 Pro 0813 #17775](https://github.com/ollama/ollama/issues/17775)
- [GLM-5.3 #17741](https://github.com/ollama/ollama/issues/17741)
- [Upstage Solar Pro 4 #17773](https://github.com/ollama/ollama/issues/17773)

A pending PR adds renderer/parser auto-detection for `qwen3moe` architectures when pulling Qwen3-Coder GGUF files directly from Hugging Face: **[server: auto-detect qwen3-coder renderer/parser for qwen3moe architecture #17769](https://github.com/ollama/ollama/pull/17769)**.

## Performance & Optimization

- **[perf: eliminate ~300ms of wasted overhead per inference request #16161](https://github.com/ollama/ollama/pull/16161)** — caches `GetModel()`/`Capabilities()` to avoid re-reading and re-parsing model metadata on every request, even when the model is already in GPU memory.
- **[Qwen3.8-27B MTP variants 2x slower than non-MTP on Apple Silicon #17776](https://github.com/ollama/ollama/issues/17776)** — reported as a possible Metal speculative-decoding regression; no fix yet.
- **[test: harden integration tests, lint, add create scope #17425](https://github.com/ollama/ollama/pull/17425)** — CI/scope improvement, not runtime performance, but helps keep large blob uploads out of release test scope.

## Stability & Regressions

Ranked by severity:

- **Qwen3.8/Qwen3.6 “system message must be at the beginning” 500s** — [#17754](https://github.com/ollama/ollama/issues/17754), [#17768](https://github.com/ollama/ollama/issues/17768), [#17774](https://github.com/ollama/ollama/issues/17774). Breaks `ollama launch claude` and OpenAI-compatible `/v1/messages`. The v0.32.14-rc0 Qwen renderer fix addresses this.
- **CUDA illegal memory access** — [#17434](https://github.com/ollama/ollama/issues/17434): `qwen3.6:35b` + JSON-schema `format` + `think:false` crashes the CUDA runner 100% reproducibly. No fix PR listed.
- **Qwen3.8 chat streaming “no user query found in messages”** — [#17778](https://github.com/ollama/ollama/issues/17778): 500 during tool-calling loops.
- **SillyTavern Text Completion empty response** — [#17700](https://github.com/ollama/ollama/issues/17700): regression since 0.32.7; no logs and no request reaches Ollama.
- **GPU backend regressions**:
  - [Pascal P6000/P4000 no longer working since 0.32.11 #17766](https://github.com/ollama/ollama/issues/17766)
  - [AMD Radeon 780M Vulkan `DeviceLost` in 0.32.11 #17748](https://github.com/ollama/ollama/issues/17748)
  - [RX 9060 XT `TensileLibrary_lazy_gfx1200.dat` load failure #17782](https://github.com/ollama/ollama/issues/17782)
- **Image input bugs** — [Ollama 0.30.5 not recognizing JPEG #16532](https://github.com/ollama/ollama/issues/16532); [minicpm-v WebP SIGSEGV #16162](https://github.com/ollama/ollama/issues/16162) (closed).
- **Memory-related** — [gemma4 MLX memory grows per prompt #17783](https://github.com/ollama/ollama/issues/17783); [gemma4 memory regression on Jetson Orin Nano #17787](https://github.com/ollama/ollama/issues/17787); [models disappeared after 0.32.7 update #17661](https://github.com/ollama/ollama/issues/17661).
- **Security/Cloud** — [Sessions not revoked after password/email change #17682](https://github.com/ollama/ollama/issues/17682); [Ollama Cloud API 503 since Aug 14 #17756](https://github.com/ollama/ollama/issues/17756).

## What This Means for Application Developers

- **Qwen3.8 agent users should test v0.32.14-rc0 immediately.** The “system message must be at the beginning” errors were breaking Claude Code and OpenAI-compatible message flows; the rc includes a targeted renderer fix.
- **Avoid `qwen3.6:35b` with JSON-schema + `think:false` on CUDA** until [#17434](https://github.com/ollama/ollama/issues/17434) is fixed — it is a hard crash, not a graceful error.
- **Watch PR [#17763](https://github.com/ollama/ollama/pull/17763)** if you rely on Modelfile `temperature`: currently `/v1/chat/completions` injects a hardcoded default of 1.0 when temperature is omitted, overriding the model definition.
- **Don’t send `audio` fields to `/api/chat` yet.** [#17764](https://github.com/ollama/ollama/pull/17764) will make the server return 400; today they are silently dropped and the model responds “blind.”
- **If you pull Qwen3-Coder GGUF files directly from HF**, [#17769](https://github.com/ollama/ollama/pull/17769) is needed for correct renderer/parser assignment; until then tool-call parsing can fail.
- **For live request debugging**, [#17762](https://github.com/ollama/ollama/pull/17762) makes `OLLAMA_DEBUG_LOG_REQUESTS` log requests before inference instead of after, which will make replay/curl debugging actually useful for long-running requests.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-16

## Today's Highlights
A read-only security survey of master surfaced three findings — no-auth proxy when `LITELLM_MASTER_KEY` is unset, SSRF/provider-key exfiltration via client-supplied `api_base`, and a budget-bypass via `temp_budget_increase` — all closed as triaged. The translation layer dominates open bugs: `gpt-5.4` Responses bridge failures, Bedrock Converse rejecting forwarded `betas`, and Gemini TTS never being spend-tracked. On the fix side, PRs landed for managed batch cost accounting, brotli passthrough corruption, Ollama `api_base` timeouts, and legacy spend-log pagination.

## Releases & Breaking Changes
No releases in the last 24 hours. One upgrade hazard: proxy startup fails after `uv tool update litellm["proxy"]` to v1.96.2 due to a FastAPI `get_flat_dependant` incompatibility — pin FastAPI before upgrading ([#36922](https://github.com/BerriAI/litellm/issues/36922)).

## New Model & Hardware Support
- **voyage-code-4 embedding model** added to the cost map in both `model_prices_and_context_window.json` and its backup (pre-release entry mirroring voyage-code-3 pricing) — [PR #36820](https://github.com/BerriAI/litellm/pull/36820)

## Performance & Optimization
- **Spend-log seq-scan saturation (P2028):** `LiteLLM_SpendLogs` has no `(api_key, startTime)` index, so budget-window spend reseeds pin the DB and fail spend-update transactions under normal traffic (2 vCPU RDS reported) ([#35766](https://github.com/BerriAI/litellm/issues/35766)). Interim mitigation: add the composite index.
- **Legacy spend-log pagination:** PR bounds individual-log queries with stable take/skip, preventing full-history loads on unfiltered paths — [PR #37027](https://github.com/BerriAI/litellm/pull/37027)
- **Ollama model-info lookup ignores `api_base`:** every completion pays ~8s of silent localhost connect timeouts before the model is invoked. Fix PR forwards `api_base` to `/api/show` — [Issue #37041](https://github.com/BerriAI/litellm/issues/37041), [PR #37062](https://github.com/BerriAI/litellm/pull/37062)

## Stability & Regressions
**High severity:**
- **`gpt-5.4` Responses bridge broken:** `litellm.responses()` returns empty final output and `completion()` fails with "Unknown items in responses API response: []" — reproducible on 1.83.4 with ChatGPT subscription auth ([#25429](https://github.com/BerriAI/litellm/issues/25429)).
- **Brotli passthrough corruption:** Anthropic now brotli-compresses non-streaming JSON when clients advertise `br`; LiteLLM forwards `Accept-Encoding` verbatim and relays undecodable bytes (Docker images lack the brotli package). Fix PR stops forwarding client `Accept-Encoding` upstream — [PR #37058](https://github.com/BerriAI/litellm/pull/37058)
- **Managed Bedrock batches cannot be cancelled** via `POST /v1/batches/{id}/cancel`; provider op is missing for bedrock ([#33986](https://github.com/BerriAI/litellm/issues/33986)).

**Medium:**
- **Prisma startup race during rolling deployments:** Uvicorn schedules background jobs (spend updates, credential load, budget cache) before the embedded Prisma engine accepts connections, causing spend data loss ([#27704](https://github.com/BerriAI/litellm/issues/27704)).
- **Bedrock Converse rejects `betas` field** forwarded on Anthropic 1M-context requests — HTTP 400 from AWS ([#28081](https://github.com/BerriAI/litellm/issues/28081)).
- **`service_tier=priority` silently billed at default rate** for gpt-4o/gpt-4.1 family; pricing keys missing on dated-snapshot model entries ([#37046](https://github.com/BerriAI/litellm/issues/37046)).
- **Gemini native custom `api_base`** serializes `system_instruction` (snake_case) instead of canonical `systemInstruction`; some proxies reject it as a duplicate oneof assignment ([#37028](https://github.com/BerriAI/litellm/issues/37028)).
- **Anthropic `/v1/messages` with `role:"system"` inside `messages[]`** is silently dropped before reaching the backend ([#36917](https://github.com/BerriAI/litellm/issues/36917)).
- **Gemini TTS (`/v1/audio/speech`) is never spend-tracked** — audio returns 200 but no spend log and key spend stays 0 ([#37015](https://github.com/BerriAI/litellm/issues/37015)).
- **Guardrail-blocked `/v1/responses` report zero token usage** despite real upstream consumption (hardcoded zero usage object) ([#36880](https://github.com/BerriAI/litellm/issues/36880)).
- **`RateLimitError` does not distinguish `insufficient_quota`** from retryable 429s; standard retry logic loops indefinitely on billing errors ([#32785](https://github.com/BerriAI/litellm/issues/32785)).
- **`interactions.create()` silently drops `response_format`** when routed via the proxy for Gemini ([#36928](https://github.com/BerriAI/litellm/issues/36928)).

**Security (read-only survey of master @ 7a5b98e6; closed as triaged):**
- **No-auth proxy when `LITELLM_MASTER_KEY` unset;** shipped docker-compose default does not set it — CWE-306/287, low ([#37054](https://github.com/BerriAI/litellm/issues/37054)).
- **SSRF / provider-key exfiltration** via client-supplied `api_base` under clientside-auth opt-ins; the proxy-wide ban is dead code — CWE-918/522, medium ([#37053](https://github.com/BerriAI/litellm/issues/37053)).
- **Budget bypass:** non-admin key owner raises own `max_budget` via `temp_budget_increase` on `/key/update` — CWE-863/770, medium ([#37052](https://github.com/BerriAI/litellm/issues/37052)).
- **Admin UI session cookie is a non-HttpOnly JWT** whose `key` claim is the caller's real proxy key ([#36997](https://github.com/BerriAI/litellm/issues/36997)).

**Closed regressions (previously reported, now resolved/stale):** thinking+tools failure on 1.81.14 ([#22997](https://github.com/BerriAI/litellm/issues/22997)), tool `function.arguments` lost in OpenAI→Anthropic conversion on 1.83.7 ([#27469](https://github.com/BerriAI/litellm/issues/27469)), Gemini function-call ordering ([#26755](https://github.com/BerriAI/litellm/issues/26755)), and streaming `usage.cost` loss on the Chat→Responses auto-route ([#27459](https://github.com/BerriAI/litellm/issues/27459)).

## What This Means for Application Developers
- **Audit spend tracking on non-chat modalities:** Gemini TTS, guardrail-blocked responses, and retrieved batch costs all have known accounting gaps; fixes are in flight ([#37015](https://github.com/BerriAI/litellm/issues/37015), [#36880](https://github.com/BerriAI/litellm/issues/36880), [PR #37050](https://github.com/BerriAI/litellm/pull/37050)).
- **Do not treat `RateLimitError` as unconditionally retryable:** `insufficient_quota` is a persistent billing state; add explicit `code` inspection or you will spin retry loops indefinitely ([#32785](https://github.com/BerriAI/litellm/issues/32785)).
- **Pin FastAPI and test after any `uv tool update`:** proxy startup can break on dependency drift ([#36922](https://github.com/BerriAI/litellm/issues/36922)).
- **Multi-tenant operators should harden immediately:** set `LITELLM_MASTER_KEY` explicitly and watch for patches to the `api_base` SSRF and `temp_budget_increase` findings ([#37052](https://github.com/BerriAI/litellm/issues/37052), [#37053](https://github.com/BerriAI/litellm/issues/37053), [#37054](https://github.com/BerriAI/litellm/issues/37054)).
- **Ollama users:** upgrade to pick up the `api_base` fix — ~8s of silent connect timeouts per completion disappears ([PR #37062](https://github.com/BerriAI/litellm/pull/37062)).
- **Guardrail ecosystem work is active:** Prisma AIRS tool-call scanning, Azure Content Safety `apply_guardrail`, and full PANW scan-response fields are all landing — re-test guardrail policies after upgrading ([PR #37038](https://github.com/BerriAI/litellm/pull/37038), [PR #36894](https://github.com/BerriAI/litellm/pull/36894), [PR #37036](https://github.com/BerriAI/litellm/pull/37036), [PR #37055](https://github.com/BerriAI/litellm/pull/37055)).

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-16

## Today's Highlights
No new releases in the last 24h, but activity is intense around Unsloth Studio: 56 issues and 69 PRs were updated, with most work converging on desktop-app performance and streaming correctness. The most impactful item is a PR that eliminates whole-dataset preprocessing for `max_steps` training runs — one measured case spent 11m14s tokenizing vs. 1m54s actually training ([#8890](https://github.com/unslothai/unsloth/pull/8890)). On the regression side, GGUF export now forces a ~40GB 16-bit download before conversion, which broke a previously easy workflow ([#8717](https://github.com/unslothai/unsloth/issues/8717)).

## Releases & Breaking Changes
None in the last 24 hours.

## New Model & Hardware Support
- **oMLX model discovery** — [PR #8937](https://github.com/unslothai/unsloth/pull/8937) adds detection of models installed via oMLX (`~/.omlx/models`, `<publisher>/<model>` layout) to Studio's local inventory.
- **Media API auto-switch** — [PR #8766](https://github.com/unslothai/unsloth/pull/8766) adds opt-in model auto-switching for `/v1/images/generations` and video endpoints, matching the existing chat auto-switch behavior.
- **Intel GPU support (requested)** — [Issue #8931](https://github.com/unslothai/unsloth/issues/8931) asks for Intel GPU installation in Studio (currently only via Vulkan llama.cpp), targeting CSM-1b.
- **Models surfacing this cycle:** Gemma-4-26B-A4B ([#8483](https://github.com/unslothai/unsloth/issues/8483)), Qwen3.8-27B-GGUF ([#8875](https://github.com/unslothai/unsloth/pull/8875)), Ideogram 4 ([#8940](https://github.com/unslothai/unsloth/issues/8940)).

## Performance & Optimization
- **Dataset preprocessing for `max_steps`** — [PR #8890](https://github.com/unslothai/unsloth/pull/8890) fixes tokenizing the entire dataset before a single step. Measured: 11m14s preprocessing vs. 1m54s training on a 27GB dataset for a 30-step Qwen3-0.6B run; the fix preprocesses only rows the run will use.
- **Embedded MTP under partial GPU offload** — [PR #8875](https://github.com/unslothai/unsloth/pull/8875) fixes ~3.5 tok/s with UD-IQ2_M on Qwen3.8-27B by correcting MTP head placement relative to the main model.
- **Streaming UI coalescing** — [PR #8845](https://github.com/unslothai/unsloth/pull/8845) batches streamed chunks when the browser renderer falls behind, preventing message-rebuild queue buildup on fast local replies.
- **Code-fence tokenization** — [PR #8935](https://github.com/unslothai/unsloth/pull/8935) makes large streaming code fences (>2,000 chars) incrementally tokenized; refreshes no longer re-tokenize the entire block every 250ms.
- **Local model inventory** — [PR #8770](https://github.com/unslothai/unsloth/pull/8770) cuts a ~5s cold `GET /api/hub/local` (109 models) that blocked unrelated API work for 4+ seconds.
- **Cached GGUF loads** — [PR #8771](https://github.com/unslothai/unsloth/pull/8771) eliminates 7 redundant Hub round trips when loading an already-cached, fully-verified GGUF.
- **Quantized KV cache on tensor split** — [PR #8939](https://github.com/unslothai/unsloth/pull/8939) fixes a bug where q8_0 (and other quantized) KV caches were silently replaced with f16/bf16/f32 when tensor parallelism was enabled.

## Stability & Regressions
Ranked by severity; fix PRs noted where present.

1. **Torch security fix blocked** — [Issue #8926](https://github.com/unslothai/unsloth/issues/8926): published constraints prevent upgrading to torch 2.13, blocking remediation of GHSA-rrmf-rvhw-rf47. Open; no fix PR.
2. **GGUF export workflow regression** — [Issue #8717](https://github.com/unslothai/unsloth/issues/8717, closed): conversion now requires a 16-bit export (~40GB download) before producing GGUF; users report this is a deliberate but painful change.
3. **Qwen3-0.6B training failure on Colab T4** — [Issue #2482](https://github.com/unslothai/unsloth/issues/2482): `RuntimeError: PassManager::run failed` with `trl.SFTTrainer`; 18 comments, still open.
4. **`torch.float8_e8m0fnu` missing** — [Issue #8933](https://github.com/unslothai/unsloth/issues/8933): Studio training fails to import ML libraries on a torch build lacking the float8 attribute — version-mismatch class of bug.
5. **Deep Research freeze** — [Issue #8483](https://github.com/unslothai/unsloth/issues/8483): froze at "Writing The Report" with Gemma-4-26B-A4B; no token accounting on cancel.
6. **Partial-download dead end** — [Issue #8927](https://github.com/unslothai/unsloth/issues/8927): "Partial Download. Click to continue." does nothing; related download-state issues at [#8941](https://github.com/unslothai/unsloth/issues/8941) and [#8928–8930](https://github.com/unslothai/unsloth/issues/8928) (placeholder-titled, closed).
7. **AMD VRAM misreporting** — [Issue #8942](https://github.com/unslothai/unsloth/issues/8942) (overinflated iGPU VRAM) and [#8878](https://github.com/unslothai/unsloth/issues/8878) (unknown VRAM on VULKAN/ROCm).
8. **Microphone blocked on Ubuntu Mate** — [Issue #8678](https://github.com/unslothai/unsloth/issues/8678): WebKitGTK built without media-stream; voice dictation broken.
9. **CUDA_VISIBLE_DEVICES UUID hides GPU picker** — [Issue #8873](https://github.com/unslothai/unsloth/issues/8873): multi-GPU host with UUID-form env var loses the per-model GPU selector.
10. **Ideogram 4 fails on macOS** — [Issue #8940](https://github.com/unslothai/unsloth/issues/8940): `'_Noop' object is not iterable` in Studio desktop.

**Fixes in flight:**
- [PR #8943](https://github.com/unslothai/unsloth/pull/8943) stops Studio from reloading an already-loaded model, which killed an in-flight streaming reply (#8893).
- [PR #8754](https://github.com/unslothai/unsloth/pull/8754) / [#8755](https://github.com/unslothai/unsloth/pull/8755) fix tool-call fragment mis-routing when delta indices restart per round (#8734).
- [PR #8621](https://github.com/unslothai/unsloth/pull/8621) fixes Windows linked-folder sync reading files in text mode (CRLF collapse, Ctrl-Z truncation) (#8617).
- [PR #8816](https://github.com/unslothai/unsloth/pull/8816) adds applying saved prompts as the chat system prompt without touching sampling settings.
- [PR #8890](https://github.com/unslothai/unsloth/pull/8890) also corrects `get_statistics()` ignoring `force_download=False` ([#8899](https://github.com/unslothai/unsloth/issues/8899)).

## What This Means for Application Developers
- **GGUF deployment pipelines are currently blocked by the export change** — a 40GB 16-bit intermediate download is now required to convert fine-tunes. Track [#8717](https://github.com/unslothai/unsloth/issues/8717) before relying on quick GGUF artifacts in CI.
- **`max_steps` experimentation gets dramatically cheaper** after [PR #8890](https://github.com/unslothai/unsloth/pull/8890) — expect ~5–6x shorter job wall-clock for short runs on large datasets.
- **Agent/tool-call streaming bugs are actively being fixed** ([#8754](https://github.com/unslothai/unsloth/pull/8754), [#8755](https://github.com/unslothai/unsloth/pull/8755)) — if you build on Studio's OpenAI-compatible endpoint with multi-round tool calls, plan to pick up these fixes.
- **LAN serving without Cloudflare tunnels is the top requested networking feature** ([#8578](https://github.com/unslothai/unsloth/issues/8578), [#8898](https://github.com/unslothai/unsloth/issues/8898), [#8934](https://github.com/unslothai/unsloth/issues/8934)) — relevant if you need local-network inference without a middleman.
- **Reasoning-model ergonomics are incomplete** — no reasoning-effort slider for Qwen3-class models in Studio ([#8881](https://github.com/unslothai/unsloth/issues/8881)); teams building reasoning pipelines must inject Jinja templates manually for now.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*