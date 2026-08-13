# Hugging Face Trending Models Digest 2026-08-14

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-13 23:34 UTC

---

# Hugging Face Trending Models Digest — 2026-08-14

## 1. Today's Highlights

This week's Hugging Face trending list is defined by two forces: open-weight frontier LLMs and a rapidly expanding MiniMax-H3 video ecosystem. Kimi-K3 leads likes with 10,620, while DeepSeek-V4-Flash and MiniMax-H3 both hold seven-figure download counts. Qwen released a 2.4T-parameter MoE with 95B active plus an FP8 version, and NVIDIA/Unsloth are pushing NVFP4 and GGUF quantization across LLM and video models. The MiniMax-H3 stack is the week's clearest ecosystem story — Comfy-Org alone has over 10M downloads, surrounded by Turbo variants, LoRAs, and ComfyUI wrappers.

---

## 2. Trending Models

### 🧠 Language Models

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,316 | 1,431,587 | Flash-tier DeepSeek-V4 checkpoint for fast conversational text generation. It has 3,316 likes and 1,431,587 downloads, making it one of the most-adopted new LLMs in this digest. |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 779 | 1,012 | A massive sparse-MoE language model from Qwen with 2.4T total parameters and 95B active parameters. Its Qwen3.5-MoE tags and 779 likes signal a scale-focused frontier release. |
| [LiquidAI/LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 602 | 116,640 | A compact 2.6B Liquid Foundation Model for efficient text generation. It has 602 likes and 116,640 downloads, pointing to continued interest in small, liquid-architecture models. |
| [deepgrove/maple-preview](https://huggingface.co/deepgrove/maple-preview) | deepgrove | 352 | 3,868 | A preview Mixture-of-Experts causal language model from DeepGrove. It has 352 likes and 3,868 downloads, suggesting early community evaluation of a new MoE entrant. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 273 | 0 | Pro-tier DeepSeek-V4 LLM dated 0813, designed for higher-capability conversational generation. It has 273 likes but zero downloads, likely a staged or API-first release. |
| [inclusionAI/Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 216 | 1,292 | A tiny model in the Ling 3.0 line using a hybrid custom architecture under an MIT license. It has 216 likes and 1,292 downloads, with a US-region availability tag. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 129 | 22,279 | BF16 release of NVIDIA's 30B-A3B Lightning language model with about 3B active parameters. It has 129 likes and 22,279 downloads, pairing with the NVFP4 version for different deployment paths. |

### 🎨 Multimodal & Generation

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,620 | 1,871,575 | Kimi-K3 is Moonshot AI's compressed-tensors image-text-to-text model, topping this digest with 10,620 likes. It blends multimodal understanding with efficient, compact tensor formats. |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 3,818 | 1,605,940 | MiniMax's flagship image-text-to-video model for text-to-video and image-to-video generation. It anchors a broad ecosystem, with 3,818 likes and 1,605,940 downloads. |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,414 | 121,042 | An open 30B image-text-to-text conversational model from meta-models. Its 1,414 likes and 121,042 downloads show immediate adoption, boosted by the Unsloth GGUF variant. |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,288 | 10,365,210 | ComfyUI-ready single-file packaging of MiniMax-H3 for local video workflows. It is the most-downloaded model in this digest at 10,365,210 downloads. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 715 | 57,287 | Lightricks's image-to-video diffusion model supporting text-to-video, image-to-video, and video-to-video. It has 715 likes and 57,287 downloads, positioning it as a flexible video-generation alternative. |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 459 | 91,455 | A Turbo variant of MiniMax-H3 optimized for faster image/video generation. It has 459 likes and 91,455 downloads, showing community demand for accelerated video models. |
| [nvidia/NVIDIA-NemotronLabs-VoiceChat-11B](https://huggingface.co/nvidia/NVIDIA-NemotronLabs-VoiceChat-11B) | nvidia | 371 | 1,164 | NVIDIA Nemotron Labs' 11B voice-chat model for spoken interaction. It has 371 likes and 1,164 downloads, marking growing Hugging Face interest in audio dialogue models. |
| [Kijai/MiniMax-H3_comfy](https://huggingface.co/Kijai/MiniMax-H3_comfy) | Kijai | 303 | 0 | A ComfyUI-oriented repository for running MiniMax-H3 locally. It has 303 likes and zero downloads, indicating a code/pipeline artifact rather than a weight release. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 287 | 25 | MiniMax's music-generation model with a dedicated minimax_music3 architecture. It has 287 likes but only 25 downloads, suggesting a preview or restricted deployable. |
| [Kijai/MiniMax-H3-experimental](https://huggingface.co/Kijai/MiniMax-H3-experimental) | Kijai | 221 | 0 | An experimental ComfyUI implementation for MiniMax-H3 from Kijai. It has 221 likes and zero downloads, useful for tracking cutting-edge integration work. |
| [endless-frontier/BigBang-v1](https://huggingface.co/endless-frontier/BigBang-v1) | endless-frontier | 188 | 3,184 | BigBang-v1 is an image-text-to-text conversational model based on a Qwen3.5-MoE architecture from endless-frontier. It has 188 likes and 3,184 downloads as a new multimodal MoE checkpoint. |

### 📦 Fine-tunes & Quantizations

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 1,986 | 2,793,115 | A community GGUF fine-tune of Qwen3.6 aimed at uncensored/heretic role-play and creative tasks. It has 1,986 likes and 2,793,115 downloads, showing a strong niche market. |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 725 | 0 | Community LoRA for MiniMax-H3 Turbo with text-to-video, audio-video, and text-to-audio tags. It has 725 likes but zero downloads, suggesting an early release or code/framework artifact. |
| [ethanfel/Qwen3-VL-32B-Ultra-Heretic-H3-ComfyUI-INT8-ConvRot](https://huggingface.co/ethanfel/Qwen3-VL-32B-Ultra-Heretic-H3-ComfyUI-INT8-ConvRot) | ethanfel | 483 | 0 | An INT8 ComfyUI-adapted version of Qwen3-VL-32B with a custom "heretic/H3" twist. It has 483 likes and zero downloads, reflecting demand for local, niche-optimized vision-language builds. |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 390 | 352,023 | Unsloth's GGUF quantization of Muse-Glimmer-30B for efficient local inference. It has 390 likes and 352,023 downloads, surpassing the base model in uptake. |
| [drbaph/MiniMax-H3-Turbo-Lora-ComfyUI](https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI) | drbaph | 314 | 0 | ComfyUI-targeted LoRA adapter for MiniMax-H3 Turbo. It has 314 likes and zero downloads, likely a component for custom ComfyUI pipelines. |
| [SexGod1979/PinkCherry_MiniMax-H3](https://huggingface.co/SexGod1979/PinkCherry_MiniMax-H3) | SexGod1979 | 298 | 324 | A community text-to-video fine-tune of MiniMax-H3 with a specific aesthetic style. It has 298 likes and 324 downloads, showing continued interest in stylized video LoRAs. |
| [meta-models/Muse-Glimmer-30B-GGUF](https://huggingface.co/meta-models/Muse-Glimmer-30B-GGUF) | meta-models | 257 | 136,783 | GGUF release of Muse-Glimmer-30B published by meta-models. It has 257 likes and 136,783 downloads, making the multimodal 30B easier to run locally. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 229 | 44,859 | NVFP4-quantized variant of NVIDIA's 30B-A3B Lightning language model. It has 229 likes and 44,859 downloads, validating NVIDIA's low-precision format for LLM deployment. |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 157 | 4,000 | FP8 version of Qwen's 2.4T-total/95B-active MoE model for reduced memory and faster inference. It has 157 likes and 4,000 downloads as an official quantization of the flagship Qwen3.8. |
| [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 157 | 4,692 | fal's MiniMax-H3 LoRA for realistic people generation. It has 157 likes and 4,692 downloads, giving video creators a targeted character-realism adapter. |
| [unsloth/MiniMax-H3-GGUF](https://huggingface.co/unsloth/MiniMax-H3-GGUF) | unsloth | 149 | 111,222 | Unsloth's GGUF conversion of MiniMax-H3 for local video-generation pipelines via stable-diffusion.cpp. It has 149 likes and 111,222 downloads, extending MiniMax-H3 beyond GPU-heavy setups. |
| [lightx2v/MiniMax-H3-Prompt-Rewriter-LoRA](https://huggingface.co/lightx2v/MiniMax-H3-Prompt-Rewriter-LoRA) | lightx2v | 148 | 652 | A LoRA adapter designed to rewrite prompts specifically for MiniMax-H3. It has 148 likes and 652 downloads, highlighting the emerging prompt-engineering layer around video models. |

---

## 3. Ecosystem Signal

MiniMax-H3 is the week's clearest ecosystem core. Besides the official image-text-to-video model, the Hub now contains ComfyUI single-file distributions (10.4M downloads), Kijai's experimental wrappers, GGUF conversions, a Turbo variant, and LoRAs for realism, prompt rewriting, and niche aesthetics. This pattern suggests open video generation has moved into a plug-and-play stage.

On the LLM side, extreme MoE scale is ascendant: Qwen's 2.4T/95B, NVIDIA's 30B-A3B Lightning, and DeepGrove's MoE preview all favor sparse activation. At the same time, DeepSeek-V4 Flash/Pro, Qwen3.8, Kimi-K3, and Liquid's LFM2.5 show a broad open-weight front, though several "Pro" or "preview" entries have zero downloads, hinting at staged releases or API-centric access.

Quantization is now mainstream across model types: GGUF for Muse-Glimmer and MiniMax-H3, FP8 for Qwen, NVFP4 for NVIDIA, and compressed tensors for Kimi. Community fine-tunes, including uncensored/role-play variants, remain a persistent niche in the ecosystem.

---

## 4. Worth Exploring

- **MiniMaxAI/MiniMax-H3** — The hub of the largest tooling ecosystem in this digest. Understanding its base weights explains the 10M+ download ComfyUI port, Turbo derivatives, and LoRA stack.

- **moonshotai/Kimi-K3** — Highest likes by a wide margin (10,620) and a compressed-tensors multimodal design. It is a strong candidate for studying efficient multimodal deployment and release strategy.

- **Qwen/Qwen3.8-2.4T-A95B** — A 2.4T-parameter Mixture-of-Experts with only 95B active parameters. Combined with the FP8 variant, it represents the frontier of sparse LLM scale and practical inference.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*