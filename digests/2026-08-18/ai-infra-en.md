# AI Infrastructure Digest 2026-08-18

> Generated: 2026-08-17 23:16 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project Comparison Report — 2026-08-18

## 1. Ecosystem Overview

The AI infrastructure landscape is in a **stabilization and platform-expansion phase**, not a feature-release phase: only llama.cpp shipped releases in the 24-hour window, while the other five projects concentrated on bug fixes and in-flight PRs. **Speculative decoding is simultaneously the most active development area and the most common source of high-severity regressions** — both vLLM and SGLang carry open reports of illegal memory access, hangs, and even silent token corruption in MTP/EAGLE/DSPARK paths. The clearest forward investment across every layer is **AMD/ROCm/XPU enablement**, from serving-engine roadmaps to fine-tuning backend recovery. Meanwhile, agentic workloads are exposing new failure modes at the application boundary — reasoning-token leaks, tool-call retry hangs, and budget-enforcement bypasses — pushing reliability work into gateway and runtime layers.

## 2. Activity Comparison

Counts reflect issues/PRs explicitly referenced in each digest, not full project totals.

| Project | Layer | Digest-Referenced Issues | Digest-Referenced PRs | Release Status (24h) |
|---|---|---|---|---|
| vLLM | Production serving engine | ~17 | ~11 | None |
| SGLang | Serving engine (spec-decode focus) | 13 | ~16 (incl. #35022–35029 series) | None |
| llama.cpp | Local/edge runtime | 15 | ~17 | **4 releases** (b10455–b10472) |
| Ollama | Local runtime / desktop + agent UX | 16 | 13 | None |
| LiteLLM | Gateway / proxy | 18 | 13 | None |
| Unsloth | Fine-tuning / Studio serving | 17 | 14 | None |

**Takeaway:** llama.cpp is the only project shipping — and shipping fast (four patch releases covering AMD APU memory detection, SYCL kernel fixes, and release automation). The serving/gateway/training layer is in a `main`-only fix window, which raises the cost of pinning for production users.

## 3. Model Support Race

| Project | New Model / Architecture Support This Digest |
|---|---|
| **llama.cpp** | GraniteSWA / GraniteMoeSWA interleaved sliding-window attention (#25505), dots3-note DSA+SWA family (#27060), DeepSeek 4 tensor-split with mirrored FA for single-K-head MoE (#26490), ROCm 7.14 Docker builds (#27145) |
| **vLLM** | Kimi-K3 ROCm roadmap (#50682, in progress), ROCm DeepSeek V4 fused AR draft metadata (#52628), XPU INC int4 w4a8 backend (#50501), CUDA 13.4 / Rubin `sm_107` image pipeline (#52379, proposed) |
| **SGLang** | AMD gfx950 Kimi-K3 12-head MLA FP8 Gluon decode (#34647), Intel XPU encoder embedding models (#35213) |
| **Ollama** | MLX Bailing MoE V3 / Ling-3.0 support (#17643), big-endian GGUF byte-swapping (#17826), cloud-model metadata population (#17828) |
| **LiteLLM** | FLUX 3 video generation (#37224), Amazon Comprehend Medical passthrough (#37229), Azure Document Intelligence native OCR output (#37194) |
| **Unsloth** | AMD Vulkan fallback backend (#8412), non-GGUF image/video Hub models (#8855) |

**Who is ahead:** *llama.cpp* leads in breadth of local model architectures, absorbing new HF architectures quickly. *vLLM* and *SGLang* are racing on serving-day-0 readiness for frontier models (DeepSeek V4, Kimi-K3) — especially speculative decoding and ROCm — rather than raw architecture count. *LiteLLM* leads in provider-surface breadth. *Ollama* and *Unsloth* are differentiated by Apple-silicon (MLX) and fine-tuning-toolchain enablement respectively.

## 4. Performance Frontier

| Area | Leading Activity |
|---|---|
| **KV cache** | vLLM: extensible/growable KV cache redesign (#50779, draft). llama.cpp: SYCL TILE quantized-KV decode with **+42% to +169%** on Qwen3.6-35B/Gemma 4 at 32K–118K context (#26689). SGLang: symmetric-memory fix preventing silent wrong outputs under CUDA-graph capture with TP>1 (#34230) |
| **Speculative decoding** | vLLM: fully async spec-decode (#29134), MRV2 batch-sharded sampling cutting logits memory to O(1/TP) (#50465). SGLang: EAGLE draft-extend CUDA graph staging (#35126), DSpark mask-filling draft heads (#33831). llama.cpp: adaptive MTP draft depth with counting state machine (#27210) |
| **Kernels / quantization** | vLLM: W4A8-INT8 via PTX 9.4 `ldmatrix.s8.s4` (#49529). SGLang: removal of 64-head TP padding in DSv4 MLA prefill (#35104). llama.cpp: SYCL quantized-cpy thread/block fix with large Arc 70 q4_0→f32 uplift (b10456) |
| **Distributed serving** | Mostly regressions: vLLM multi-node `shm_broadcast` idle stall on GB10 (#51921); SGLang NIXL/UCX prefill segfault on B200/CUDA 13.0 (#35189, root cause still unknown) |
| **System-level efficiency** | Unsloth: startup-time reduction via pandas removal (#8962), vision-projector VRAM accounting (#9063). Ollama: **missing MLX prefix caching** — full 20–30K token re-prefill every agent step (#17829, open). LiteLLM: OOM/continuous memory growth (#25219), prompt-injection check blocking the event loop (#19499) |

## 5. Layer Positioning

- **vLLM** — *Production serving engine.* Multi-node TP/DP, EngineCore, CUDA-graph optimization, and the broadest GPU/quantization matrix. The default choice for high-throughput GPU fleets; today's digest shows it absorbing the cost of that breadth (10 ranked stability issues, several in spec-decode and multi-node paths).
- **SGLang** — *Serving engine, innovation lead in speculative decoding.* EAGLE/NEXTN/DSPARK, PD disaggregation, and sparse attention (DSA). The most aggressive on frontier-model decode paths (DeepSeek V4, Kimi-K3), but also carrying the most concerning correctness risk — silent identifier corruption (#34959).
- **llama.cpp** — *Local/edge runtime.* The broadest backend coverage (CUDA, SYCL, OpenCL, ROCm, Vulkan) and the only project shipping releases. Increasingly security-conscious (three OOB-write fixes in review) and expanding into new architectures faster than anyone.
- **Ollama** — *Local runtime + developer experience.* Built on llama.cpp + MLX; closest to the agent developer (Claude Code integrations, cloud model catalog, `ollama launch claude`). Today's digest shows agent-facing regressions (DeepSeek V4 `</think>` leak, Qwen3.8 retry hang) that make it the riskiest layer for agentic production use right now.
- **LiteLLM** — *Control-plane gateway.* No inference kernels; owns routing, budgets, billing, credential management, and provider translation. The only pure control-plane project here, and the one where **budget enforcement is currently unreliable** — a governance gap as agent workloads scale.
- **Unsloth** — *Fine-tuning + Studio serving.* Differentiated by training (LoRA/QLoRA, quantization, GGUF conversion) and converging toward local serving: an OpenAI-compatible API with optional no-auth mode (#9102) makes it an Ollama/LM Studio competitor. ROCm/Vulkan reliability is the main blocker.

Note the convergence: Unsloth Studio and Ollama are both becoming local OpenAI-compatible servers; vLLM and SGLang target the same frontier models with overlapping spec-decode features; LiteLLM sits in front of all of them.

## 6. Trend Signals

1. **Speculative decoding: the new battleground, and the new risk.** Every engine is investing heavily (vLLM MTP/async, SGLang EAGLE/DSPARK, llama.cpp adaptive MTP), yet spec-decode failures dominate the top stability lists — including SGLang's *silent identifier corruption* (#34959) and vLLM's MTP illegal memory access (#40756). **Watch:** do not enable spec-decode by default in production; pin known-good commits per GPU/quantization combo.

2. **AMD/ROCm/XPU is now day-0, not a port.** vLLM has a Kimi-K3 ROCm roadmap; SGLang landed AMD gfx950 FP8 decode; llama.cpp ships ROCm 7.14 Docker builds; Unsloth is actively rescuing its ROCm backend; Ollama's Intel iGPU request has 75 👍. **Watch:** AMD parity claims are moving fast but uneven — validate per workload.

3. **Agentic correctness is the new frontier.** The most agent-impacting failures are not throughput but semantics: Ollama's DeepSeek V4 cloud leaking `</think>` into history (driving 193 identical tool calls), Qwen3.8 tool-call retry hanging forever, Unsloth normalizing tool-call IDs, LiteLLM budgets being bypassable. **Watch:** add loop guards, bound retries, strip reasoning tokens before echoing history, and audit budget enforcement as a hard stop.

4. **Long-context + sparse attention has silent failure modes.** SGLang's DSA path computing *zero attention* beyond 65K tokens (#34941), DeepSeek V4 hangs at ~245K context (#33549), and HiCache silently degrading to 0% prefix hits on long agentic sessions (#35129). **Watch:** validate output quality at your actual context length; instrument cache-hit rates.

5. **Security hardening is arriving — unevenly.** LiteLLM has an open credential-leakage path (callback creds in plaintext logs, `/health` exposing tokens), while llama.cpp is shipping review for three exploitable OOB-write fixes. **Watch:** rotate any credentials that passed through proxy metadata; treat untrusted GGUF/mmproj as untrusted input.

6. **Release cadence divergence is sharp.** llama.cpp shipped 4 releases in 24h; every other project's fixes exist only on `main`. **Watch:** for vLLM, SGLang, Ollama, LiteLLM, and Unsloth, adopters must cherry-pick revisions containing specific PRs — a material operational cost as the fix windows lengthen.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-08-18

## 1. Today's Highlights

No releases were cut in the last 24 hours; activity is concentrated on stabilization and platform expansion. Speculative decoding reliability is the hottest topic — MTP memory faults on Qwen3.6-FP8 and a draft-model TP>1 init crash each drew significant discussion, with a WIP diagnostic PR now attached to the latter. On the platform side, ROCm enablement is accelerating: a Kimi-K3 gap/roadmap tracker is open, DeepSeek V4 fused AR draft metadata support landed for AMD, and a CUDA 13.4 prerelease (Rubin `sm_107`) image pipeline was proposed.

## 2. Releases & Breaking Changes

None. No new tags or releases were published in the last 24 hours.

## 3. New Model & Hardware Support

- **Kimi-K3 ROCm roadmap** ([#50682](https://github.com/vllm-project/vllm/issues/50682)): Upstream tracking for AMD enablement, including AITER fused-moe a16w4/a8w4 integrations and day-0 baseline features.
- **CUDA 13.4 prerelease pipeline for Rubin** ([#52379](https://github.com/vllm-project/vllm/pull/52379)): Adds an image-only CUDA 13.4rc1 path for `sm_107`, overlaying NVIDIA's prerelease `cuda-toolkit` PyPI packages and pinned nightly torch builds.
- **XPU INC int4 w4a8 backend** ([#50501](https://github.com/vllm-project/vllm/pull/50501)): Introduces `INCXPUW4A8LinearMethod` (int4 weights + dynamic per-token int8 activations), reusing the w4a16 layout with ARK as the primary dispatch.
- **ROCm CPU offload alignment** ([#43018](https://github.com/vllm-project/vllm/pull/43018)): Aligns `hipMemcpyBatchAsync` parameters and performance across ROCm 7.13+/7.14x for the CPU-offload path.
- **ROCm DeepSeek V4 fused AR draft metadata** ([#52628](https://github.com/vllm-project/vllm/pull/52628)): Builds on #46849 to enable fused multi-step draft decode graphs on AMD by enabling sparse SWA metadata updates.
- **Rust frontend parity roadmap** ([#44280](https://github.com/vllm-project/vllm/issues/44280)): Tracks the remaining feature gap for the experimental `VLLM_USE_RUST_FRONTEND=1` server.

## 4. Performance & Optimization

- **Batch-invariant (deterministic) inference tracking** ([#27433](https://github.com/vllm-project/vllm/issues/27433)): 71 comments and 19 👍; the core implementation is in but follow-up work is still open on the project board.
- **PTX 9.4 `ldmatrix.s8.s4` for W4A8-INT8** ([#49529](https://github.com/vllm-project/vllm/issues/49529)): Proposal to leverage CUDA 13.4's hardware INT4→INT8 expanding shared-memory load to cut sign-extension overhead in W4A8 kernels.
- **Fully async spec-decoding** ([#29134](https://github.com/vllm-project/vllm/issues/29134)): Making `seq_lens_cpu` optional in `CommonAttentionMetadata` would remove host↔GPU syncs currently blocking input-prep/forward overlap.
- **Extensible (growable) KV cache** ([#50779](https://github.com/vllm-project/vllm/pull/50779)): Large opt-in redesign of the KV cache; still a draft stacked on #51718.
- **MRV2 batch-sharded sampling** ([#50465](https://github.com/vllm-project/vllm/pull/50465)): Cuts per-step logits memory from `O(batch × (spec+1) × vocab)` to `O(1/TP)` of that by sharding sampler inputs across tensor-parallel ranks.
- **ModelOpt Llama-4 slow checkpoint loading** ([#31624](https://github.com/vllm-project/vllm/issues/31624)): Load times exceed 5 minutes from page cache due to MoE state-dict hack logic; flagged as a good first issue.

## 5. Stability & Regressions

Ranked by severity:

1. **MTP spec decode illegal memory access on long sequences** ([#40756](https://github.com/vllm-project/vllm/issues/40756)): Qwen3.6-27B-FP8, v0.19.1, `num_spec_tokens=5`. Open, 38 comments.
2. **v0.27.0 multi-node idle stall** ([#51921](https://github.com/vllm-project/vllm/issues/51921)): 4-node TP=4 on GB10/sm_121; `shm_broadcast` writer starves after ~60s idle and requests never reach the scheduler. Open, 12 comments.
3. **Qwen3.5 CUDA illegal memory access in GDN kernel** ([#34948](https://github.com/vllm-project/vllm/issues/34948)): Reproduced on H200 nightly, CUDA 13.0. Open, 24 comments.
4. **Draft-model spec decode init crash under TP>1** ([#52023](https://github.com/vllm-project/vllm/issues/52023)): Crashes when draft `hidden_size` > target due to TRT-LLM fused allreduce+RMSNorm workspace sizing. WIP diagnostic patch: [#52193](https://github.com/vllm-project/vllm/pull/52193).
5. **Gemma4 fails to start in latest image** ([#51744](https://github.com/vllm-project/vllm/issues/51744)): v0.27.0 + Transformers 5.15.0 incompatibility with NVFP4 checkpoints; 8 👍.
6. **Mamba-2 Triton crash on DGX Spark** ([#37431](https://github.com/vllm-project/vllm/issues/37431)): `cudaErrorIllegalInstruction` on SM121 in async mode; worked around by `CUDA_LAUNCH_BLOCKING=1`.
7. **NIXL disagg mismatch for Qwen3.5 hybrid** ([#42895](https://github.com/vllm-project/vllm/issues/42895)): Prefill TP4 / decode DP8 with different physical block sizes fails.
8. **Prefix-caching determinism bug** ([#40896](https://github.com/vllm-project/vllm/issues/40896)): First request differs from identical later requests at temperature=0 in v1 with APC enabled.
9. **DeepEP-V2 startup crash on decode/cudagraph path** ([#52632](https://github.com/vllm-project/vllm/pull/52632)): Fix PR ensures `expert_tokens_meta` is `None` when `recv_expert_num_tokens` is empty.
10. **lm-format-enforcer import break** ([#52661](https://github.com/vllm-project/vllm/pull/52661)): Restores the removed `vllm.transformers_utils.tokenizer` shim; `lm-format-enforcer==0.11.3` still imports it.

Also reported: hybrid multi-group KV crash on connector error blocks ([#50687](https://github.com/vllm-project/vllm/issues/50687)), KVConnector V1 external-hit reservation lifecycle gap ([#42372](https://github.com/vllm-project/vllm/issues/42372)), streaming_update shared-prefix `-1` token leak ([#42490](https://github.com/vllm-project/vllm/issues/42490)), deepseek_v4 parser misrouting `</think>`-less replies ([#48645](https://github.com/vllm-project/vllm/issues/48645)), standalone EngineCore batch-sampling skip ([#52660](https://github.com/vllm-project/vllm/pull/52660)), and MRV2 Gumbel sampling with non-finite logits ([#43249](https://github.com/vllm-project/vllm/pull/43249)). Closed items include the ROCm AWQ JSON-schema "!" loop ([#39348](https://github.com/vllm-project/vllm/issues/39348)), the v0.18.2 ROCm attention-backend regression ([#39965](https://github.com/vllm-project/vllm/issues/39965)), and the Kimi-K2.7-Coder `mla_gluon` gfx942 assertion on MI308X ([#51964](https://github.com/vllm-project/vllm/issues/51964)).

## 6. What This Means for Application Developers

- **Pin versions carefully for Qwen3.5/3.6 + speculative decoding.** Multiple open crash reports (MTP illegal access, GDN kernel faults, draft-model TP>1 init) mean you should validate on your exact GPU/quantization/spec-token combo before rolling out.
- **The multi-node idle stall is a release blocker signal for v0.27.0** on GB10/sm_121. If you run multi-node TP with long idle gaps, hold off or add a health-check that detects "requests accepted but never scheduled."
- **Prefix caching still can't guarantee bit-exact temperature=0 reproducibility** ([#40896](https://github.com/vllm-project/vllm/issues/40896)). Test first-request behavior explicitly if your eval harness assumes APC determinism.
- **ROCm/AMD support is maturing quickly**, with Kimi-K3 tracked as a first-class roadmap item and CPU-offload/perf alignment PRs in flight. Expect AMD parity to improve but treat it as fast-moving.
- **The Rust frontend remains experimental.** The parity gap is large; keep `VLLM_USE_RUST_FRONTEND=1` out of production.
- **If you use `lm-format-enforcer` in structured-output pipelines**, the tokenizer shim removal breaks current images; pin an older image until [#52661](https://github.com/vllm-project/vllm/pull/52661) merges.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-08-18

## 1. Today's Highlights

No SGLang release shipped in the last 24 hours, but a substantial config-system refactor series (#35022–#35029) and multiple DeepSeek-V4/Kimi-K3 speculative-decoding fixes are moving through PRs. The most urgent signals are correctness risks in current DSPARK/DeepSeek-V4 paths: a new report of silent identifier corruption (#34959) and a DSA sparse-MLA prefill path that can compute zero attention for long extends (#34941). Production users on speculative decoding should treat these as high-risk until fixed.

## 2. Releases & Breaking Changes

- No new tagged releases or release candidates in the last 24 hours.
- Config-system refactor in review: #35022–#35029 moves runtime config readers onto “config bags,” consolidating control-plane logs and provenance (#35027, #35028) and retiring multi-engine accommodation (#35022). This can change `/server_info` readback and config-resolution behavior once merged, but no migration notes are published yet.

## 3. New Model & Hardware Support

- [PR #35213](https://github.com/sgl-project/sglang/pull/35213): Adds Intel XPU support for encoder embedding models: `BAAI/bge-base-en-v1.5`, `nomic-ai/nomic-embed-text-v1.5`, and `ibm-granite/granite-embedding-english-r2`.
- [PR #34647](https://github.com/sgl-project/sglang/pull/34647): Enables 12-head MLA Aiter FP8 Gluon decode (`batched bh16bn128`) on AMD gfx950 for Kimi-K3 TP8.
- [Issue #29562](https://github.com/sgl-project/sglang/issues/29562) and [Issue #29796](https://github.com/sgl-project/sglang/issues/29796) report GLM-5.2 issues on Pro 6000 and Ascend NPU respectively — not new support, but unresolved backend gaps.

## 4. Performance & Optimization

- [PR #35104](https://github.com/sgl-project/sglang/pull/35104): Removes the 64-head TP padding overhead on the DeepSeek-V4 MLA prefill path for `attn_tp_size=2`; avoids running attention on 32 useless padded rows.
- [PR #35126](https://github.com/sgl-project/sglang/pull/35126): Splits EAGLE draft-extend CUDA graph staging into pre/post verify halves, intended to reduce launch overhead without behavior change.
- [PR #35024](https://github.com/sgl-project/sglang/pull/35024): Sizes speculative-decoding buffers from runtime config bags rather than the startup record, fixing adaptive step-config paths.
- [PR #33831](https://github.com/sgl-project/sglang/pull/33831): Adds DSpark support for DFlash-style mask-filling draft heads with `verify width = gamma`, unblocking more speculative-decoding layouts.
- [PR #34230](https://github.com/sgl-project/sglang/pull/34230): Fixes symmetric-memory allocation during CUDA graph capture; prevents silent incorrect outputs with TP>1 + speculative decoding, and stabilizes NCCL symmetric windows.
- [PR #35197](https://github.com/sgl-project/sglang/pull/35197): Fixes Helion KDA small-token prefill shape handling and rejects non-power-of-2 head dims in decode.

No concrete throughput/latency numbers were included in these items.

## 5. Stability & Regressions

Ranked by severity:

- [Issue #34959](https://github.com/sgl-project/sglang/issues/34959): **DSPARK silently corrupts identifiers** on DeepSeek-V4-Flash, making speculative decoding unsafe. No fix PR attached yet.
- [Issue #34941](https://github.com/sgl-project/sglang/issues/34941): **DSA sparse-MLA prefill computes no attention at all** for a single unchunked extend >65,535 tokens due to `gridDim.z` overflow. Reported and closed; verify against main before relying on long-context DSA.
- [Issue #35189](https://github.com/sgl-project/sglang/issues/35189): **NIXL/UCX prefill segfault persists** on B200/CUDA 13.0; previous fixes #23489/#23499 were closed without root cause.
- [Issue #34920](https://github.com/sgl-project/sglang/issues/34920): **Kimi K3 decode crashes** deterministically with PD disaggregation + DCP + DSPARK (`extend_prefix_lens=None` in `dcp/planner.py`).
- [Issue #35144](https://github.com/sgl-project/sglang/issues/35144): **EAGLE/NEXTN TP=2 hangs at warmup on Intel XPU** after #34238 moved the verify-decision TP broadcast.
- [Issue #33549](https://github.com/sgl-project/sglang/issues/33549): **DeepSeek-V4 decode hangs indefinitely at ~245K context** on 8×H20 with DSPARK TP=8; all GPUs spin at 100% until watchdog abort.
- [Issue #34235](https://github.com/sgl-project/sglang/issues/34235): **Scheduler hang in DSV4 sparse prefill** with hierarchical cache + chunked prefill, plus device-side sampling assert on recent versions.
- [Issue #34718](https://github.com/sgl-project/sglang/issues/34718): **Illegal memory access** in DeepSeek-V4 sparse attention indexer with long-context requests.
- [Issue #35129](https://github.com/sgl-project/sglang/issues/35129): **HiCache returns `#cached-token: 0` on long agentic sessions** despite stable 50%+ prefix; short requests hit ~98%, suggesting silent cache-lookup regression.
- [Issue #35148](https://github.com/sgl-project/sglang/issues/35148): **Qwen3.8-27B-FP8 reasoning content fails to parse** in the Rust `sgl-model-gateway`.
- [Issue #33985](https://github.com/sgl-project/sglang/issues/33985): **DSpark speculative decoding cannot start on SM120** because decode-dsv4 lacks `topk=192`, falling through to a prefill-kernel assert.

Fix PRs exist for some adjacent kernels: #35104 (DSv4 MLA prefill), #35197 (Helion), #34230 (CUDA-graph symmetric memory), #35126 (EAGLE staging).

## 6. What This Means for Application Developers

- **Do not blindly enable DSPARK on DeepSeek-V4/Kimi-K3 in production right now.** Multiple open and recently reported issues involve hangs, illegal memory access, and one case of silent identifier corruption (#34959). Pin to a known-good commit and run correctness checks before rollout.
- **Long-context + sparse attention is particularly risky.** Both #34941 (silent no-attention) and #33549 (decode hang) target long-context DSv4 paths. If your workload exceeds ~64K tokens, validate output quality and cache hit rates explicitly.
- **Monitor cache behavior on agentic workloads.** #35129 shows HiCache can silently degrade to 0% prefix hits for long sessions. Add cache-hit-rate metrics to your observability.
- **The config refactor (#35022–#35029) will change runtime config readback.** If you consume `/server_info` or rely on tokenizer/config provenance, test against a main-branch build before adopting.
- **No new stable release means these fixes are only on `main`** — choose a revision that includes the specific PRs you need, especially if you are on Blackwell/AMD XPU or use speculative decoding.

For live tracking, see: [SGLang issues](https://github.com/sgl-project/sglang/issues) and [SGLang PRs](https://github.com/sgl-project/sglang/pulls).

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-08-18

## 1. Today's Highlights

Release **b10472** fixes AMD APU memory detection on HIP builds, replacing `MemAvailable`-based over-promising with accurate `hipMemGetInfo` reporting on small-carveout systems (fixes #18159). Three SYCL improvements also landed: quantized cpy thread/block sizing (**b10456**) with a dramatic q4_0→f32 uplift on Arc 70, and OPT_STEP_ADAMW/SGD op support (**b10455**). Notable open PRs: adaptive MTP draft depth ([#27210](https://github.com/ggml-org/llama.cpp/pull/27210)), an Electron-based desktop app ([#27287](https://github.com/ggml-org/llama.cpp/pull/27287)), and three security-hardening fixes for OOB access in ggml-cpu ([#27286](https://github.com/ggml-org/llama.cpp/pull/27286), [#27284](https://github.com/ggml-org/llama.cpp/pull/27284), [#27285](https://github.com/ggml-org/llama.cpp/pull/27285)).

## 2. Releases & Breaking Changes

- **b10472** — [CUDA: skip UMA override for HIP builds (#27083)](https://github.com/ggml-org/llama.cpp/releases/tag/b10472). AMD APUs now report device memory via `hipMemGetInfo`; prior behavior over-promised on small-carveout systems. **Action:** re-check auto-allocation/`--n-gpu-layers` heuristics on AMD APU deployments.
- **b10470** — [CI: push release tag explicitly in release.yml (#27261)](https://github.com/ggml-org/llama.cpp/releases/tag/b10470). Release automation only; no runtime impact.
- **b10456** — [SYCL: fix thread/block count in quantized cpy kernel launches (#27160)](https://github.com/ggml-org/llama.cpp/releases/tag/b10456). Largest gain on q4_0→f32: Arc 70 throughput goes from 20.21 GB/s (full figure truncated in notes).
- **b10455** — [SYCL: support OPT_STEP_ADAMW, OPT_STEP_SGD (#25268)](https://github.com/ggml-org/llama.cpp/releases/tag/b10455).

No breaking API/config changes in this window.

## 3. New Model & Hardware Support

- **[PR #25505](https://github.com/ggml-org/llama.cpp/pull/25505)** — GraniteSWAForCausalLM / GraniteMoeSWAForCausalLM support (interleaved sliding-window attention + attention sinks), mirroring huggingface/transformers#47179. Weights not yet public.
- **[PR #27060](https://github.com/ggml-org/llama.cpp/pull/27060)** — dots3-note model family (DSA + SWA), extends the KV-cache DSA layer.
- **[PR #26490](https://github.com/ggml-org/llama.cpp/pull/26490)** — DeepSeek 4 tensor-split support (`-sm tensor`), with mirrored flash attention for single-K-head MoE.
- **[PR #27145](https://github.com/ggml-org/llama.cpp/pull/27145)** — ROCm 7.14.0 Docker builds on Ubuntu 26.04, new GPU list, plus a workaround for the "no usable GPU found" error.
- **[PR #26331](https://github.com/ggml-org/llama.cpp/pull/26331)** — OpenCL Adreno xmem SDPA path for non-causal diffusion attention.

## 4. Performance & Optimization

- **Landed (b10456):** SYCL quantized cpy kernels now scale thread/block counts to quant size, fixing under/over-subscription; q4_0→f32 on Arc 70 improves from 20.21 GB/s.
- **In progress — [PR #26689](https://github.com/ggml-org/llama.cpp/pull/26689):** SYCL TILE kernel for quantized KV decode shows **+42% to +169%** on Qwen3.6-35B, Gemma 4 26B, and Gemma 4 12B at 32K and 118K context, zero regressions on Battlemage.
- **In progress — adaptive speculative decoding:** [PR #27210](https://github.com/ggml-org/llama.cpp/pull/27210) adds `--spec-type draft-mtp-adaptive` with a counting-based state machine and weighted drop-pressure accumulator (suggested `--spec-draft-n-max 12`); [PR #25726](https://github.com/ggml-org/llama.cpp/pull/25726) adds a rolling-window draft-length heuristic.
- **Regression watch:** [#25489](https://github.com/ggml-org/llama.cpp/issues/25489) — MTP performance drops since b9935 on Windows, still open.

## 5. Stability & Regressions

**Security (fixes in review):**
- **[PR #27286](https://github.com/ggml-org/llama.cpp/pull/27286)** — `mul_mat_id` validates expert IDs only via debug assert; release builds can hit heap OOB write with an out-of-range expert index.
- **[PR #27284](https://github.com/ggml-org/llama.cpp/pull/27284)** — im2col backward widens offset stride to int64_t (CWE-680/787).
- **[PR #27285](https://github.com/ggml-org/llama.cpp/pull/27285)** — mtmd vision graph builders deref optional tensors before null checks; SIGSEGV via crafted mmproj GGUF.

**Crashes / correctness (open):**
- [#27102](https://github.com/ggml-org/llama.cpp/issues/27102) — CUDA kernel stall on RTX Pro 6000 Blackwell MAX-Q, killed by watchdog.
- [#27046](https://github.com/ggml-org/llama.cpp/issues/27046) — SIGSEGV on Intel Lunar Lake iGPU (Arc 140V); `resolve_fused_ops` false positives, also reproduces on gemma4/qwen2 builds.
- [#26746](https://github.com/ggml-org/llama.cpp/issues/26746) — ROCm gfx1151 RPC worker crash in GGML_OP_TOP_K during DeepSeek V4 prefill past 4096 tokens.
- [#26902](https://github.com/ggml-org/llama.cpp/issues/26902) / [#27116](https://github.com/ggml-org/llama.cpp/issues/27116) — `GGML_ASSERT(ret.axis != GGML_BACKEND_SPLIT_AXIS_UNKNOWN)` with tensor split on 4x Tesla T10 (Glimmer Q8_0) and 2x RTX 5060 Ti with iq4_nl KV cache.
- [#26996](https://github.com/ggml-org/llama.cpp/issues/26996) — Windows ROCm 7.14 release missing `hipblas.dll`; GPU not detected, `--list-devices` empty.
- [#26558](https://github.com/ggml-org/llama.cpp/issues/26558) — Hard crash `cublasSgemm INVALID_VALUE` with `--spec-type draft-mtp` under KV-cache saturation.
- [#27109](https://github.com/ggml-org/llama.cpp/issues/27109) — 4-bit KV cache (q4_1/q4_0) collapses prefill to ~34 t/s on Qwen3.5 hybrid, RTX 3090.
- [#26583](https://github.com/ggml-org/llama.cpp/issues/26583) — GLM-5.2 multi-node CUDA RPC: graph_compute fails with invalid data ptr.
- [#27038](https://github.com/ggml-org/llama.cpp/issues/27038) — SYCL host-pinned memory change causes high CPU utilization for large allocations.
- [#27174](https://github.com/ggml-org/llama.cpp/issues/27174) — `/v1/completions` returns logprobs for generated tokens only; `echo: true` + `logprobs` silently drops prompt logprobs, breaking lm-eval-harness loglikelihood evals.
- [#22360](https://github.com/ggml-org/llama.cpp/issues/22360) — Server throughput degrades over time, requires periodic restarts.
- [#24712](https://github.com/ggml-org/llama.cpp/issues/24712) — Fused Gated Delta Net tensor assigned to CUDA0 while layer is on CPU (sched_reserve warning).

## 6. What This Means for Application Developers

- **AMD APU deployments:** b10472 corrects VRAM reporting on HIP builds — capacity planning now reflects true device memory. Re-validate `--n-gpu-layers` and KV-cache sizing on small-carveout iGPU systems.
- **Speculative decoding:** adaptive MTP depth ([#27210](https://github.com/ggml-org/llama.cpp/pull/27210)) is close to landing — servers will soon tune draft length at runtime instead of a fixed max. Budget re-benchmark time; the open regression [#25489](https://github.com/ggml-org/llama.cpp/issues/25489) shows MTP remains in flux.
- **Observability:** OTLP/HTTP tracing ([#27280](https://github.com/ggml-org/llama.cpp/pull/27280)) is coming to llama-server behind a compile-time flag — plan trace ingestion if you operate multi-instance gateways.
- **Security posture:** if you serve untrusted GGUF/mmproj files, the expert-ID and im2col fixes ([#27286](https://github.com/ggml-org/llama.cpp/pull/27286), [#27284](https://github.com/ggml-org/llama.cpp/pull/27284)) close exploitable OOB-write paths in release builds.
- **Evaluation pipelines:** the `/v1/completions` logprobs bug ([#27174](https://github.com/ggml-org/llama.cpp/issues/27174)) silently corrupts loglikelihood evals — pin builds or patch before running lm-eval against llama-server.
- **Roadmap:** disaggregated prefill/decode ([#21266](https://github.com/ggml-org/llama.cpp/issues/21266)) and router-mode multi-preset for a single loaded model ([#23704](https://github.com/ggml-org/llama.cpp/issues/23704)) are actively tracked — relevant for anyone architecting multi-tenant serving around llama-server.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-08-18

## Today’s Highlights
No Ollama release shipped in the last 24 hours; the project is in a bug-fix window with several v0.32.14 regressions reported across CPU usage, MLX inference, and Qwen3.8-family models. The most agent-impacting reports are a DeepSeek V4 cloud model leaking `</think>` tokens into history and a Qwen3.8 tool-call retry that hangs forever after a 500. Meanwhile, useful fixes are moving through review: big-endian GGUF support, MLX Ling-3.0 support, cloud-model metadata, and installer/CLI robustness.

## Releases & Breaking Changes
None in the last 24 hours. No API/config changes or migration notes.

## New Model & Hardware Support
- **MLX: Bailing MoE V3 / Ling-3.0** — [#17643](https://github.com/ollama/ollama/pull/17643) adds MLX engine support for `BailingMoeV3ForCausalLM`, the architecture behind Ling-3.0-tiny/Ling-3.0-flash, on Apple silicon.
- **Big-endian GGUF support** — [#17826](https://github.com/ollama/ollama/pull/17826) fixes tensor data corruption on big-endian hosts (s390x) by byte-swapping GGUF little-endian tensors.
- **Cloud model metadata** — [#17828](https://github.com/ollama/ollama/pull/17828) fills metadata gaps for cloud models without local manifests (e.g. `kimi-k3:cloud`), ensuring context window/reasoning are populated in launch configs.
- **MLX preflight coverage** — [#17622](https://github.com/ollama/ollama/pull/17622) adds an `apple-silicon-mlx` preflight profile for gemma4 MLX-store exports.
- **Intel iGPU support** — [#3113](https://github.com/ollama/ollama/issues/3113) remains open with 75 👍 as a still-requested backend.

## Performance & Optimization
- **MLX missing prompt/prefix caching** — [#17829](https://github.com/ollama/ollama/issues/17829): MLX fully re-prefills 20–30K tokens on every agent step; TTFT grows monotonically. No fix PR yet.
- **v0.32.14 CPU regression** — [#17833](https://github.com/ollama/ollama/issues/17833): model fully in VRAM still causes 50–80% CPU usage; v0.32.13 with the same setup does not.
- **Embed truncation warning** — [#17799](https://github.com/ollama/ollama/pull/17799) adds a server warning when `/api/embed` silently truncates over-length input, instead of returning 200 with an embedding computed from different text.
- **Duration label rounding** — [#17827](https://github.com/ollama/ollama/pull/17827) fixes `humanDuration` using a truncated hour count for the years label.

## Stability & Regressions
Ranked by severity:

### High
- **DeepSeek V4 cloud agent loop** — [#17617](https://github.com/ollama/ollama/issues/17617): `deepseek-v4-flash:cloud` leaks a literal `</think>` into assistant history, driving Claude Code into 193 identical tool calls (~31M tokens). No fix PR yet.
- **Qwen3.8 tool-call retry hang** — [#17825](https://github.com/ollama/ollama/issues/17825): after a tool-call XML parse failure returns 500, re-submitting the identical request hangs indefinitely with no log output until runner recycle. No fix PR yet.
- **Local API auth error with no cloud creds** — [#17822](https://github.com/ollama/ollama/issues/17822): clean local setup returns `500 tokenize error: Invalid API Key (401)` on `/api/embed` and `/api/generate` with no Ollama Cloud sign-in, no `OLLAMA_API_KEY`, and no proxy.

### MLX / vision
- **125GB Metal buffer request** — [#17804](https://github.com/ollama/ollama/issues/17804): MLX vision runner crashes on a 5712×4284 JPEG with Qwen3.8-27B on a 48GB M5 Pro, requesting ~125GB Metal buffer.
- **`think: false` returns empty content** — [#17823](https://github.com/ollama/ollama/issues/17823): Gemma 4 MLX on v0.32.14 returns `content: ""` with `think: false`; v0.32.5 handles the same request correctly.
- **Duplicate image dimensions collapse** — [#17814](https://github.com/ollama/ollama/issues/17814): two images with identical pixel dimensions in one request are collapsed into one, with no error and an unpredictable survivor.

### Qwen3.8 family
- **Download failure** — [#17816](https://github.com/ollama/ollama/issues/17816): `ollama run qwen3.8` fails with `EOF` while pulling the manifest.
- **Web search broken** — [#17812](https://github.com/ollama/ollama/issues/17812): `qwen3.8:27b` web search fails with `500 Internal Server Error: no user query found in messages`.
- **`/api/generate` ignores `think`** — [#17544](https://github.com/ollama/ollama/issues/17544): `/api/generate` silently ignores `think: true` when `format` is set; `/api/chat` handles it correctly.

### Server/config
- **Session lost on network drop** — [#17821](https://github.com/ollama/ollama/issues/17821): Ollama restarts and loses the session when the internet connection drops.
- **IPv4/IPv6 host binding** — [#17831](https://github.com/ollama/ollama/issues/17831): `OLLAMA_HOST=0.0.0.0:8200` on Ubuntu 26.04 binds IPv6 rather than IPv4.
- **`CUDA_VISIBLE_DEVICES` handling** — [#17832](https://github.com/ollama/ollama/issues/17832): setting `CUDA_VISIBLE_DEVICES=n` on a three-H200 system does not behave as expected with v0.32.14.
- **`ollama launch claude` failure** — [#17811](https://github.com/ollama/ollama/issues/17811): interactive Claude Code launch fails with `Input must be provided either through stdin or as a prompt argument`.

### Fixes in review / recently merged
- **Null integration config panic** — [#17624](https://github.com/ollama/ollama/pull/17624) handles `"integrations": {"claude": null}` without dereferencing nil.
- **`reasoning_effort: minimal`** — [#17267](https://github.com/ollama/ollama/pull/17267) maps `minimal` to `low` instead of rejecting with 400.
- **Claude context window suffix** — [#17623](https://github.com/ollama/ollama/pull/17623) allows the documented `[1m]` model suffix through `/api/show` validation.
- **Bench output truncation** — [#17572](https://github.com/ollama/ollama/pull/17572) truncates existing `ollama-bench -output` files before writing, preventing stale tail data.
- **ANSI control chars in non-TTY** — [#17112](https://github.com/ollama/ollama/pull/17112) suppresses ANSI escape sequences when stderr is not a TTY.
- **Unicode corruption in directives** — [#17809](https://github.com/ollama/ollama/pull/17809) preserves zero-width/non-breaking runes in `SYSTEM`/`TEMPLATE`/`MESSAGE`/`LICENSE` values.
- **CLI image path parsing** — [#16715](https://github.com/ollama/ollama/pull/16715) fixes quoted, `file://`, tilde-expanded, and URL-encoded image paths in interactive mode.

## What This Means for Application Developers
- **Agentic clients are at risk right now.** If you use DeepSeek V4 cloud or Qwen3.8 tool-calling through Ollama, add loop guards, bound tool-call iterations, extract/strip reasoning tokens before echoing history, and do not blindly retry a 500 — a retry may hang forever.
- **MLX is not yet safe for high-throughput agentic or vision-heavy workloads.** Missing prefix caching means wasted re-prefill on every step; high-resolution inputs can crash the runner; `think: false` has a regression on Gemma 4. Pin to v0.32.5 or use CUDA/Metal llama.cpp paths for production if you depend on these.
- **Validate embeddings before storing them.** Over-long inputs are silently truncated and still return 200; once [#17799](https://github.com/ollama/ollama/pull/17799) lands, treat the new truncation warning as a hard signal to chunk.
- **Cloud model metadata is incomplete.** Follow [#17828](https://github.com/ollama/ollama/pull/17828) if you launch usage-billed or newly released cloud models — otherwise context window and reasoning config may be wrong.
- **Test v0.32.14 before rollouts.** The CPU regression and local API auth errors are configuration-sensitive; verify `OLLAMA_HOST`, `CUDA_VISIBLE_DEVICES`, and absence of accidentally configured cloud credentials.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-08-18

## Today's Highlights

No new LiteLLM release landed in the last 24 hours, but the proxy stack is seeing a cluster of high-signal fixes around billing correctness, credential leakage, and Anthropic translation. The most important work in flight: stripping callback credentials from request metadata, pricing batches/realtime sessions against the actual deployment, and fixing bedrock guardrail usage tracking. Budget enforcement remains a critical open area — multiple issues report bypasses despite `max_budget` being exceeded.

## Releases & Breaking Changes

- **No releases published in the last 24h.**  
- Upcoming user-facing changes to watch:  
  - `PATCH /access-groups` route added; edit modal migrated to RHF/zod/shadcn — [#37189](https://github.com/BerriAI/litellm/pull/37189)  
  - `/v1/ocr` adds opt-in `req_format=native` to return Azure Document Intelligence's native `analyzeResult` payload — [#37194](https://github.com/BerriAI/litellm/pull/37194)  
  - Complexity router introduces `plan_mode_min_tier` and operator-defined `tier_definitions`; these are additive but change auto-router behavior for coding-agent clients — [#37230](https://github.com/BerriAI/litellm/pull/37230), [#37226](https://github.com/BerriAI/litellm/pull/37226)  

## New Model & Hardware Support

- **FLUX 3 video generation** added for Black Forest Labs: text-to-video, image-to-video, continuation, draft mode, keyframes, duration/resolution/aspect ratio, and audio — [#37224](https://github.com/BerriAI/litellm/pull/37224)  
- **Amazon Comprehend Medical passthrough provider** added under `/comprehendmedical`, with SigV4 signing — [#37229](https://github.com/BerriAI/litellm/pull/37229)  
- **Azure Document Intelligence native OCR output** now available via `req_format=native` — [#37194](https://github.com/BerriAI/litellm/pull/37194)  
- Model pricing database fixes proposed for `azure/gpt-audio-mini-2025-10-06` and `azure/gpt-audio-1.5-2026-02-23` entries — [#37169](https://github.com/BerriAI/litellm/issues/37169), [#37170](https://github.com/BerriAI/litellm/issues/37170)  
- No new hardware, CUDA/ROCm/Metal, or quantization-format work appeared in the last 24h.

## Performance & Optimization

No concrete throughput/latency numbers landed in the last 24h. The notable performance-adjacent items are:

- **OOM kills / continuous memory growth** remains open; reproductions reported from `main-v1.82.0-stable` onward — [#25219](https://github.com/BerriAI/litellm/issues/25219)  
- **Prompt-injection heuristic check blocks the event loop**, causing Kubernetes pod restarts — [#19499](https://github.com/BerriAI/litellm/issues/19499)  
- Feature request for **adaptive similarity threshold in valkey-semantic cache** would replace the static `similarity_threshold=0.8` — [#36124](https://github.com/BerriAI/litellm/issues/36124)  

## Stability & Regressions

Ranked roughly by severity:

1. **Budget enforcement bypass** — key/user/global `max_budget` is not enforced after spend exceeds the limit in v1.82.3; a separate report says the global `max_budget_limiter` is instantiated but never registered. No fix PR linked yet.  
   - [#26672](https://github.com/BerriAI/litellm/issues/26672)  
   - [#27381](https://github.com/BerriAI/litellm/issues/27381)  
   - Project budgets are also missing from the atomic pre-call reservation path — [#34101](https://github.com/BerriAI/litellm/issues/34101)

2. **Credential leakage in proxy metadata** — callback credentials (team/key/project/org) are stamped into request metadata and logged in plaintext; fix PR strips them from the stamped auth object — [#37233](https://github.com/BerriAI/litellm/pull/37233)  
   `GET /health` also returns `extra_headers` and `aws_session_token` in plaintext — [#36898](https://github.com/BerriAI/litellm/issues/36898)

3. **Anthropic translation regressions** — the `vector_store_ids`/`vector_store` pass-through breaks Anthropic 400s; mid-stream fallback injects `prefix=True` assistant prefill that some Anthropic models reject; experimental `/v1/messages` pass-through to OpenAI/Azure has multiple bugs. A guardrail fix is already in PR.  
   - [#23741](https://github.com/BerriAI/litellm/issues/23741)  
   - [#27967](https://github.com/BerriAI/litellm/issues/27967)  
   - [#23841](https://github.com/BerriAI/litellm/issues/23841)  
   - Fix PR: [#37231](https://github.com/BerriAI/litellm/pull/37231)

4. **Billing/pricing correctness** — streamed `usage.cost` can be priced against the wrong model alias; realtime sessions ignore deployment pricing overrides; retrieved batches can price at $0 or public rates. Fixes are in PR.  
   - [#36879](https://github.com/BerriAI/litellm/pull/36879)  
   - [#36958](https://github.com/BerriAI/litellm/pull/36958)  
   - [#37077](https://github.com/BerriAI/litellm/pull/37077) / [#37219](https://github.com/BerriAI/litellm/pull/37219)  
   - `service_tier=priority` is silently billed at default rates for `gpt-4o`/`gpt-4.1` families — [#37046](https://github.com/BerriAI/litellm/issues/37046)

5. **Adaptive router can permanently 500** — one persisted alpha/beta=0 cell bricks the whole router with `gammavariate: alpha and beta must be > 0.0` — [#35590](https://github.com/BerriAI/litellm/issues/35590)

6. **DB-backed deployments dropped during router upsert** when first loaded through `upsert_deployment()` — [#35577](https://github.com/BerriAI/litellm/issues/35577)

7. **Batch API parity** — `GET /v1/batches` accepted invalid `limit` values; now rejects out-of-range with OpenAI-parity 400 — [#37198](https://github.com/BerriAI/litellm/pull/37198)  
   Managed Bedrock batch cancellation still unsupported — [#33986](https://github.com/BerriAI/litellm/issues/33986)

8. **UI/data visibility** — Spend Logs show "Request/Response Data Not Available" even when prompt storage is enabled; docs reference a nonexistent `litellm.turn_on_message_logging` function.  
   - [#23636](https://github.com/BerriAI/litellm/issues/23636)  
   - [#37143](https://github.com/BerriAI/litellm/issues/37143)

## What This Means for Application Developers

- **If you rely on LiteLLM budget enforcement, audit it now.** The key/user/global budget bypass is still open and no fix PR is visible. Do not treat `max_budget` as a hard stop in production until this is resolved.  
- **Assume sensitive callback/team credentials may leak into logs** unless you are on a build containing the metadata-stripping fix. Check `/health` output for plaintext secrets and rotate anything exposed.  
- **Verify cost accounting for batches and realtime sessions**, especially on Bedrock and deployments with custom pricing overrides. Multiple fixes are in flight but not yet released.  
- **Anthropic pass-through remains risky** for advanced features like `vector_store_ids`, mid-stream fallbacks, and guardrail-modified system prompts. Pin to a known-good version and test those paths before rolling out.  
- **New surface area is expanding**: FLUX 3 video generation, Amazon Comprehend Medical passthrough, and native Azure OCR output are coming. These are useful additions but should be treated as fresh attack surface for auth/logging/spend tracking.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-08-18

## 1. Today's Highlights

AMD ROCm reliability is the dominant theme: PRs are in review to fix Windows VRAM reporting via DXGI LUID matching ([#8863](https://github.com/unslothai/unsloth/pull/8863), [#8793](https://github.com/unslothai/unsloth/pull/8793)) and to recover the ROCm backend when it refuses to load any models ([#9002](https://github.com/unslothai/unsloth/pull/9002) → [#8998](https://github.com/unslothai/unsloth/issues/8998)). Studio tool-call handling is being hardened in parallel: false-positive tool "nudges" are gated behind explicit settings ([#9125](https://github.com/unslothai/unsloth/pull/9125), [#9126](https://github.com/unslothai/unsloth/pull/9126)) and replayed tool-call IDs are normalized to satisfy OpenAI/Mistral ID constraints ([#9116](https://github.com/unslothai/unsloth/pull/9116)). A severe Studio server deadlock in `sqlite3.connect()`/`close()` was also reported ([#9008](https://github.com/unslothai/unsloth/issues/9008)).

## 2. Releases & Breaking Changes

No releases in the last 24h. Two in-flight changes will alter behavior once merged:

- **API key opt-out** ([#9102](https://github.com/unslothai/unsloth/pull/9102)): Studio can serve its OpenAI-compatible API with no credential when the user opts in, matching LM Studio/Ollama defaults.
- **Installer/backend changes** ([#8412](https://github.com/unslothai/unsloth/pull/8412)): Linux CPU moves to torch 2.11; Vulkan becomes an AMD backend when ROCm is absent; gfx1033 is gated. Also improves GPU detection output to name the actual card ([#8762](https://github.com/unslothai/unsloth/pull/8762)).

## 3. New Model & Hardware Support

- **Intel Arc / XPU**: Import fails on Intel Arc B580 because `unsloth_zoo/temporary_patches/gpt_oss.py` calls `torch.xpu.memory.mem_get_info()`, which is unsupported on that GPU ([#3533](https://github.com/unslothai/unsloth/issues/3533)). Separate requests ask for Intel GPU support in Studio beyond the Vulkan llama.cpp path ([#8931](https://github.com/unslothai/unsloth/issues/8931), [#8972](https://github.com/unslothai/unsloth/issues/8972)).
- **AMD Vulkan fallback**: Installer now targets Vulkan for AMD hosts without ROCm ([#8412](https://github.com/unslothai/unsloth/pull/8412)).
- **Non-GGUF image/video models in Hub**: Closed PR [#8855](https://github.com/unslothai/unsloth/pull/8855) lets the Hub run safetensors image/video models (e.g. `Z-Image-Turbo-unsloth-bnb-4bit`) that previously rendered a disabled "No run" button.
- **Model loading bugs**: `'_Noop' object is not iterable` when loading Ideogram 4 on macOS ([#8940](https://github.com/unslothai/unsloth/issues/8940)); Qwen3-VL LoRA adapters fail to load on vLLM ([#3560](https://github.com/unslothai/unsloth/issues/3560)); request for Ternary Bonsai support via custom llama.cpp installs ([#9059](https://github.com/unslothai/unsloth/issues/9059)).
- **Image/video input handling**: Feature request for scaling/cropping reference images and trimming input videos for models like MiniMax H3 ([#9069](https://github.com/unslothai/unsloth/issues/9069)).

## 4. Performance & Optimization

- **Re-prefill regression**: Long Qwen3.8 GGUF chats lose reusable prompt state after model reload, forcing an ~11-minute full prefill ([#9037](https://github.com/unslothai/unsloth/issues/9037)).
- **Startup import graph**: [#8962](https://github.com/unslothai/unsloth/pull/8962) removes pandas from the Studio backend startup chain (`import main` 7.28s → `routes` 5.72s → `data_recipe` 2.31s), cutting a significant slice of cold-start time.
- **Vision projector VRAM accounting**: [#9063](https://github.com/unslothai/unsloth/pull/9063) charges the mmproj projector against VRAM during GGUF placement and moves it to CPU when it doesn't fit — prevents post-load OOM for vision models.
- **Rolling context eviction**: [#9074](https://github.com/unslothai/unsloth/pull/9074) keeps evicted turns searchable after rolling-context eviction, preserving long-chat usability.

## 5. Stability & Regressions

Ranked by severity, with fix PR status:

1. **Studio server deadlock** — every thread blocked in `sqlite3.connect()`/`close()`; listening socket never accepts; high CPU. No fix PR yet ([#9008](https://github.com/unslothai/unsloth/issues/9008)).
2. **ROCm backend loads no models** — HIP/ROCR library mismatch; fix PR [#9002](https://github.com/unslothai/unsloth/pull/9002) retries llama-server with bundled HIP ([#8998](https://github.com/unslothai/unsloth/issues/8998)).
3. **AMD GPU reported but backend runs CPU-only** — installer claims ROCm is up to date while Live monitor shows "No visible GPU"; maintainer-filed ([#8473](https://github.com/unslothai/unsloth/issues/8473)).
4. **Intel Arc B580 import crash** — unsupported `torch.xpu.memory.mem_get_info()` breaks Unsloth import entirely ([#3533](https://github.com/unslothai/unsloth/issues/3533)).
5. **System RAM not released** — Windows GGUF loads with `-ngl -1` keep significant system memory allocated even with model+KV fully in VRAM ([#9033](https://github.com/unslothai/unsloth/issues/9033)).
6. **False tool-call nudges** — Studio prompts models to call tools when none were made; fixes in [#9125](https://github.com/unslothai/unsloth/pull/9125)/[#9126](https://github.com/unslothai/unsloth/pull/9126) gate nudges and preserve retry context ([#8907](https://github.com/unslothai/unsloth/issues/8907)).
7. **External-provider stream corruption** — template literals (e.g. `` `Hi, ${name}!` ``) lose interpolation mid-stream; closed, maintainer-filed ([#9098](https://github.com/unslothai/unsloth/issues/9098)).
8. **MLX Train/Export greyed out on macOS** — startup thread race on first `transformers` import, not a broken install ([#9120](https://github.com/unslothai/unsloth/issues/9120)).
9. **LAN blank page** — Studio web UI renders blank over plain HTTP at LAN addresses; fixed by restoring `crypto.randomUUID` before bundle evaluation ([#9075](https://github.com/unslothai/unsloth/pull/9075)).
10. **Session drops on phone screen-off** — streaming terminates when the phone idles; no fix yet ([#8925](https://github.com/unslothai/unsloth/issues/8925)).

Also closed: microphone permission stuck after "Don't allow" on Windows/Ubuntu ([#9001](https://github.com/unslothai/unsloth/issues/9001), [#8678](https://github.com/unslothai/unsloth/issues/8678)); project creation failure on Windows 11 ([#8936](https://github.com/unslothai/unsloth/issues/8936)).

## 6. What This Means for Application Developers

- **Tool-calling apps on external providers**: ID normalization ([#9116](https://github.com/unslothai/unsloth/pull/9116)) fixes multi-turn tool calls that previously broke OpenAI's 64-char `call_id` cap and Mistral's 9-alphanumeric requirement. If you persist tool-call IDs, expect them to be replayed in normalized form.
- **Nudge behavior changes**: `nudge_tool_calls` is now threaded through the external `ToolLoopPolicy` ([#9125](https://github.com/unslothai/unsloth/pull/9125), [#9126](https://github.com/unslothai/unsloth/pull/9126)); explicit `true`/`false` request values behave consistently across GGUF and external loops.
- **No-auth API endpoints**: With [#9102](https://github.com/unslothai/unsloth/pull/9102), Studio can serve an OpenAI-compatible endpoint without an API key, making it a drop-in replacement for LM Studio/Ollama in local toolchains.
- **Windows AMD / ROCm users**: VRAM reporting and backend loading are under active repair. Until [#9002](https://github.com/unslothai/unsloth/pull/9002) and [#8863](https://github.com/unslothai/unsloth/pull/8863) land, treat reported VRAM figures and backend health with suspicion on ROCm.
- **Vision GGUF deployments**: After [#9063](https://github.com/unslothai/unsloth/pull/9063) lands, the vision projector is explicitly accounted for in VRAM placement; models that barely fit today may see the projector offloaded to CPU.
- **Long-running Studio instances**: The sqlite3 deadlock ([#9008](https://github.com/unslothai/unsloth/issues/9008)) is a production blocker for headless Studio; pin a known-good version and watch for a fix before upgrading.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*