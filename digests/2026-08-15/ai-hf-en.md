# Hugging Face Trending Models Digest 2026-08-15

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-14 23:14 UTC

---

## 1. Today's Highlights

Today’s HF trending list is defined by multimodal expansion and a vibrant video-generation ecosystem. **moonshotai/Kimi-K3** is the week’s most-liked model (10,670 likes), while **Qwen/Qwen3.8-27B** landed with 8,910 likes despite only 2 downloads—clear evidence of pre-release anticipation. **MiniMax-H3** is no longer just a model; Comfy-Org’s packaging has 11.7M downloads, and dozens of LoRA/GGUF variants make it the week’s community hub. **DeepSeek-V4-Flash** and **Qwen’s 2.4T-A95B MoE** anchor the LLM side, and quantization formats (FP8, NVFP4, GGUF) appear across every major family.

---

## 2. Trending Models

### 🧠 Language Models

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 911 | 3,832 | A massive sparse MoE text-generation model from Qwen’s 3.8 family, with 2.4T total parameters and 95B active. It is trending as a flagship open-weight frontier LLM, though still very new in terms of adoption. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,380 | 1,606,491 | DeepSeek’s fast V4 text-generation model, optimized for conversational performance. Its strong like/download ratio shows broad production interest in a high-throughput open-weights LLM. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 431 | 245 | A newer Pro variant of DeepSeek-V4, likely targeting higher reasoning quality. With low downloads so far, it is an early release gaining attention from evaluators. |
| [inclusionAI/Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 234 | 2,283 | A tiny hybrid-architecture model with custom code and MIT licensing. It is notable for efficient edge-style inference experiments rather than raw scale. |
| [LiquidAI/LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 614 | 124,172 | A small 2.6B text-generation model from Liquid AI’s LF 2.5 family. Its popularity reflects growing interest in efficient, small open-weights LLMs. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 141 | 34,137 | NVIDIA’s BF16 Lightning model, a 30B MoE with 3B active parameters. It is trending as a low-latency enterprise LLM competitor. |

### 🎨 Multimodal & Generation

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 8,910 | 2 | A 27B image-text-to-text vision-language model from Qwen’s 3.8 family. It has one of the highest like counts this week despite almost no downloads yet, signaling strong anticipation for a new multimodal flagship. |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,509 | 165,300 | Meta’s 30B multimodal conversational model with image understanding and generation capabilities. It is gaining rapid adoption as one of the most liked open multimodal releases this week. |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 3,917 | 1,997,541 | A powerful image-text-to-video generation model and the center of a large ecosystem. It is trending because of its high-quality video outputs and huge community follow-up adoption. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 851 | 207,830 | A versatile image-to-video / text-to-video diffusion model. It stands out for supporting multiple video workflows including video-to-video transformation. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 646 | 63 | A text-to-music generation model from MiniMax. It is early in adoption but demonstrates the growing interest in audio generation within the MiniMax ecosystem. |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 493 | 149,865 | A Turbo-style video generation variant of MiniMax-H3 for faster image-to-video inference. Its strong download count makes it one of the more widely used community video-generation models. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,670 | 1,974,635 | The most-liked model of the week, a compressed-tensors multimodal model supporting image-text-to-text and feature extraction. It combines high community enthusiasm with near-2M downloads. |
| [Gazingstars123/Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 160 | 10,106 | A 2.9B text-to-image diffusion single-file model ready for ComfyUI. It is trending as a lightweight, locally drivable image generator. |
| [nvidia/NVIDIA-NemotronLabs-VoiceChat-11B](https://huggingface.co/nvidia/NVIDIA-NemotronLabs-VoiceChat-11B) | nvidia | 381 | 1,366 | NVIDIA’s 11B voice chat model targeting spoken conversational AI. It is drawing attention as a specialized open-weight voice assistant base. |
| [dots-studio/dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 136 | 11 | A niche image-text-to-text model from dots-studio, likely focused on note-taking or document-style multimodal tasks. It is very new, with high likes relative to downloads. |

### 📦 Fine-tunes & Quantizations

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 766 | 0 | A GGUF quantization of Qwen3.8-27B from unsloth. It is positioned for local CPU/GPU inference and is trending thanks to its base model’s 8.9K likes. |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 414 | 596,774 | A GGUF version of Meta’s Muse-Glimmer-30B for efficient local deployment. It is one of the most downloaded multimodal GGUF conversions this week. |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,317 | 11,768,622 | A single-file ComfyUI-ready packaging of MiniMax-H3. It has the highest download count in the entire list, making it the default integration point for ComfyUI users. |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 741 | 0 | A community LoRA for turning MiniMax-H3 into a faster “Turbo” video generator. It has strong likes despite zero downloads so far, indicating heavy wishlisting or early pre-release interest. |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 289 | 0 | An official FP8 precision variant of Qwen3.8-27B. It targets efficient inference on FP8-capable hardware while keeping near-base model quality. |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,015 | 2,891,524 | A large community GGUF fine-tune based on Qwen3.6-27B, often labeled as an uncensored/creative variant. Its 2.9M downloads make it one of the most popular community fine-tunes this week. |
| [meta-models/Muse-Glimmer-30B-GGUF](https://huggingface.co/meta-models/Muse-Glimmer-30B-GGUF) | meta-models | 269 | 228,364 | The official GGUF release of Meta’s Muse-Glimmer-30B. It gives users a direct quantized route to running the multimodal Muse-Glimmer model locally. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 257 | 119,572 | NVIDIA’s NVFP4-quantized version of the Nemotron 3.5 Lightning MoE. It is trending for its extremely low-precision, high-efficiency inference story. |
| [Kijai/MiniMax-H3_comfy](https://huggingface.co/Kijai/MiniMax-H3_comfy) | Kijai | 339 | 0 | A ComfyUI-oriented packaging of MiniMax-H3 from Kijai, a known ComfyUI developer. It is useful for integrating MiniMax-H3 into custom ComfyUI pipelines. |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 184 | 9,334 | An FP8 quantized version of Qwen’s 2.4T-parameter MoE model. It offers a practical way to deploy the massive model on FP8-optimized clusters. |
| [SexGod1979/PinkCherry_MiniMax-H3](https://huggingface.co/SexGod1979/PinkCherry_MiniMax-H3) | SexGod1979 | 309 | 473 | A community text-to-video fine-tune of MiniMax-H3 with endpoints-compatible packaging. It reflects the rapid community experimentation happening around MiniMax video generation. |
| [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 176 | 9,060 | A LoRA from fal designed to improve realism for people in MiniMax-H3 video outputs. It is trending as a practical, high-quality style-enhancement adapter. |
| [unsloth/MiniMax-H3-GGUF](https://huggingface.co/unsloth/MiniMax-H3-GGUF) | unsloth | 155 | 136,774 | A GGUF version of MiniMax-H3 for local video generation workflows. It shows growing demand for compressed video models in GGUF format. |
| [drbaph/MiniMax-H3-Turbo-Lora-ComfyUI](https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI) | drbaph | 318 | 112,975 | A ComfyUI-friendly LoRA adapter for MiniMax-H3 Turbo-style generation. It is widely downloaded, indicating heavy use inside ComfyUI video pipelines. |

---

## 3. Ecosystem Signal

Qwen is consolidating a full-stack **3.8 family**: a 27B dense VLM, a 2.4T MoE text model, and FP8 variants of both. DeepSeek is maintaining dual **Flash/Pro** cadence, with Flash seeing massive production downloads. Meta’s **Muse-Glimmer** is gaining traction mostly through GGUF conversions, while NVIDIA’s **Nemotron 3.5 Lightning** pushes low-active-parameter MoE efficiency.

The clearest ecosystem signal is **MiniMax-H3**: official model, ComfyUI single-file, LoRAs, GGUF, and Turbo variants all appear in the top 30. Video generation now has a genuine model-ecosystem pattern similar to LLMs. Open weights dominate this list, with no proprietary-only model present. Quantization is becoming both a first-party feature (Qwen FP8, NVIDIA NVFP4) and a third-party wave (unsloth GGUF). Community fine-tuning is focused on aesthetic/uncensored variants and domain-specific video LoRAs.

---

## 4. Worth Exploring

- **moonshotai/Kimi-K3** — Highest likes of the week and 1.97M downloads. Its compressed-tensors and feature-extraction tags make it a great case study for efficient multimodal deployment.
- **Qwen/Qwen3.8-27B** — A brand-new flagship VLM with 8.9K likes despite almost no downloads. It is worth studying as the likely source of the next wave of Qwen multimodal fine-tunes.
- **Comfy-Org/MiniMax-H3** — With 11.7M downloads, this is the week’s most practical artifact. It represents how an open video model becomes a usable production tool through ecosystem packaging.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*