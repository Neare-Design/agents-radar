# Hugging Face Trending Models Digest 2026-08-18

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-17 23:16 UTC

---

# Hugging Face Trending Models Digest — 2026-08-18

## Today's Highlights

The week is dominated by the Qwen 3.5/3.8 ecosystem: **Qwen/Qwen3.8-27B** leads multimodal chat with ~10.7K likes and has already spawned official FP8/NVFP4 quantizations plus community GGUF and uncensored variants. On the video side, **MiniMax-H3** is the clear winner, especially through Comfy-Org's single-file distribution with over 14M downloads, plus Turbo and LoRA spin-offs. Among text models, **DeepSeek-V4-Flash** and **moonshotai/Kimi-K3** show strong demand for efficient open-weight LLMs; Kimi-K3 tops the chart with 10,800 likes and compressed-tensor support. Smaller models from LiquidAI and inclusionAI also signal rising interest in compact, efficient architectures, while chat-template utility repos highlight the growing tooling friction around Qwen.

## Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,041 | 9,465 | Qwen's flagship text-only MoE model with 2.4T total parameters and 95B active. It is trending as the scale-oriented sibling of Qwen3.8-27B, with an official FP8 quantization already available. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 573 | 25,006 | A professional DeepSeek-V4 text-generation checkpoint from August 13. It is trending alongside DeepSeek-V4-Flash as the family's higher-capability tier. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,496 | 1,978,298 | A faster, lighter DeepSeek-V4 variant aimed at practical deployment. With ~2M downloads, it has become the most-used DeepSeek-V4 release this week. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 169 | 69,833 | NVIDIA's 30B-total/3B-active MoE language model in BF16. It is trending for efficient inference and tight integration with the NVIDIA/Nemotron stack. |
| [inclusionAI/Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 306 | 6,266 | A tiny hybrid-architecture model from inclusionAI with MIT licensing and custom code. It is notable for lightweight language modeling and regional deployment flexibility. |
| [LiquidAI/LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 653 | 147,270 | A 2.6B text-generation model from Liquid AI's LFM2.5 family. It is gaining attention for delivering strong quality in a very small, efficient package. |

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 10,698 | 415,039 | A large multimodal image-text-to-text chat model from the Qwen 3.8 line. It is one of the week's most-liked models and anchors an extensive ecosystem of quantized and uncensored variants. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,104 | 465,529 | A diffusion single-file image-to-video model with text/video/image-to-video support. It is trending for high-quality, flexible video generation in open workflows. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 900 | 10,375 | A text-to-music generation model built on the MiniMax Music 3 stack. It is trending as the audio companion to MiniMax-H3 and has already been adapted for ComfyUI. |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,660 | 334,099 | A 30B multimodal image-text-to-text chat model. It ranks high on likes and has a well-used unsloth GGUF port that broadens its reach. |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,086 | 2,403,238 | A video-generation model covering text-to-video, image-to-video, and video-to-video tasks. It is a dominant open video model this week, with turbo versions and LoRA fine-tunes appearing quickly. |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 583 | 264,351 | A community Turbo variant of MiniMax-H3 focused on image-to-video and related video modes. It is trending because it offers faster or lower-cost generation while keeping H3 compatibility. |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,402 | 14,015,769 | A ComfyUI single-file distribution of MiniMax-H3. Its 14M+ downloads make it the most-downloaded asset in the digest and a major reason the H3 video ecosystem is thriving. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,800 | 2,163,953 | A multimodal image-text-to-text model with compressed-tensor and feature-extraction support. It has the highest likes count this week and stands out for efficient large-model serving. |
| [Gazingstars123/Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 233 | 23,202 | A 2.9B text-to-image diffusion single-file model with ComfyUI support. It is trending as a compact, community-friendly image-generation option. |
| [dots-studio/dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 205 | 633 | A preview multimodal note/chat model in the Dots3 family, accepting image and text inputs for text generation. It is early and low-download but interesting for experimental note-taking and long-context workflows. |
| [LiquidAI/LFM2.5-VL-3B](https://huggingface.co/LiquidAI/LFM2.5-VL-3B) | LiquidAI | 162 | 6,816 | Liquid AI's 3B vision-language model based on the LFM2.5 architecture. It is trending for bringing efficient liquid-model design to small multimodal tasks. |
| [Comfy-Org/MiniMax-Music-3](https://huggingface.co/Comfy-Org/MiniMax-Music-3) | Comfy-Org | 166 | 256,988 | A ComfyUI single-file distribution of MiniMax Music-3 under Apache-2.0. It is the easy on-ramp for music generation in ComfyUI, with over 250K downloads. |

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 1,623 | 2,727,609 | A GGUF quantization of Qwen3.8-27B from unsloth. With 2.7M downloads, it is the most popular way to run Qwen3.8-27B locally. |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 527 | 495,646 | Official FP8 quantization of Qwen3.8-27B for reduced memory and faster inference. It is trending as the trusted vendor FP8 option beside unsloth's NVFP4 build. |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 430 | 15,812 | An abliterated, uncensored FP8 variant of Qwen3.8-27B. It is trending within a niche of FP8 users who want less restrictive chat behavior in a quantized multimodal model. |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 297 | 357,701 | A llama.cpp GGUF uncensored version of Qwen3.8-27B with MTP support. It gives local llama.cpp users a high-quality uncensored multimodal LLM option. |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 470 | 755,125 | A GGUF port of meta-models/Muse-Glimmer-30B for efficient local multimodal inference. Its 755K downloads show strong demand for quantized 30B vision-language models. |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 237 | 378,177 | A NVIDIA NVFP4 4-bit quantization of Qwen3.8-27B by unsloth. It is trending as a memory-efficient deployment format tuned for NVIDIA GPUs. |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,119 | 3,033,928 | A community GGUF fine-tune known for the "uncensored/heretic" style, with a long feature-heavy name. It is one of the most-downloaded community Qwen GGUF builds, surpassing 3M downloads. |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 219 | 12,295 | Official FP8 quantization of Qwen's 2.4T-parameter MoE text model. It makes the massive Qwen3.8 MoE practical to serve at lower precision. |
| [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 243 | 18,562 | A LoRA fine-tune for MiniMax-H3 aimed at realistic people and portrait generation. It is trending because it gives video creators a lightweight way to steer H3 toward realism. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 307 | 231,271 | NVIDIA's NVFP4-quantized Nemotron 3.5 Lightning 30B-A3B release. It is trending as an official 4-bit MoE that is even more optimized for inference on NVIDIA hardware. |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 786 | 0 | A text-to-video LoRA for MiniMax-H3 Turbo with tags for audio-video and text-to-audio. It is trending in the build-up of community H3 add-ons, despite not having public download counts yet. |

## Ecosystem Signal

The Qwen 3.5/3.8 family is the clearest momentum leader: a single base multimodal model, **Qwen3.8-27B**, has generated official FP8/NVFP4 quantizations, unsloth GGUF builds, and community uncensored/abliterated variants, while Qwen's 2.4T MoE adds a high-scale text option. **MiniMax-H3** is the video-generation winner, with nearly 14M downloads for the ComfyUI single-file distribution and a growing layer of Turbo/LoRA tools around it. Open-weight models continue to dominate the trending list; DeepSeek-V4-Flash and Kimi-K3 show that efficiency and compression are as important as raw scale. Quantization and fine-tuning activity is intense: GGUF, FP8, NVFP4, LoRA, and "uncensored" variants account for roughly a third of the list. Even non-model tooling matters—**froggeric/Qwen-Fixed-Chat-Templates** collected 1,209 likes with zero downloads, indicating chat-template compatibility is now a pain point in the Qwen ecosystem.

## Worth Exploring

- **[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** — The highest-liked model this week with 10,800 likes and 2.16M downloads. Its compressed-tensor and feature-extraction support make it an excellent case study in efficient large multimodal serving.
- **[Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3)** — With 14M+ downloads, this single-file ComfyUI distribution shows how a video model becomes a real-world creative production tool. It is the best entry point for experimenting with the MiniMax-H3 ecosystem.
- **[Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)** — The hub model for the week's Qwen ecosystem. Comparing its official FP8, unsloth NVFP4, GGUF, and uncensored variants is a practical lesson in quantization trade-offs.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*