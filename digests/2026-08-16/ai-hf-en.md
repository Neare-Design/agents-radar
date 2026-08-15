# Hugging Face Trending Models Digest 2026-08-16

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-08-15 23:14 UTC

---

## Today's Highlights

Kimi-K3 led the week on likes (10,722), closely followed by Qwen's Qwen3.8-27B (9,762), making multimodal conversational AI the clear center of attention. MiniMax-H3 solidified itself as the dominant open video-generation ecosystem, with the Comfy-Org distribution reaching 12.79M downloads and widespread Turbo/LoRA/GGUF extensions. DeepSeek's V4 Flash/Pro pair and NVIDIA's Nemotron 3.5 Lightning refreshed the text-generation frontier with efficient sparse and quantized formats. The top 30 also shows heavy quantization and fine-tuning activity around Qwen, Muse-Glimmer, and MiniMax-H3, plus new momentum in music and video generation.

## Trending Models

### 🧠 Language Models

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 966 | 6,381 | Sparse MoE text-generation model from Qwen with 2.4T total parameters and 95B active. Trending as a frontier-scale open-weight model for efficient inference and conversational performance. |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 488 | 19,945 | Pro text-generation model refreshed in August 2026. This release accompanies DeepSeek's more widely adopted Flash variant. |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,418 | 1,798,247 | Fast text-generation model built for high-volume conversational workloads. Its 1.79M downloads and strong like count make it one of the week's most-used LLMs. |
| [inclusionAI/Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 256 | 4,832 | Compact model from inclusionAI tagged bailing_hybrid and custom_code, released under MIT. It is trending for its permissive license and lightweight footprint. |
| [LiquidAI/LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 627 | 135,448 | Liquid AI's 2.6B text-generation model built on the LFM2 line. Its 135K downloads highlight strong demand for small, efficient local LLMs. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 150 | 62,965 | NVIDIA's 30B-to-A3B sparse text-generation model in BF16. A solid enterprise-grade reference point for efficient open LLM inference. |

### 🎨 Multimodal & Generation

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 9,762 | 91,917 | Qwen's 27B image-text-to-text model and the week's second-most-liked release. It anchors a large family of official and community quantizations and fine-tunes. |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,574 | 246,454 | Meta's image-text-to-text model for conversational multimodal understanding. Its 246K downloads indicate strong real-world adoption despite lower like counts. |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 937 | 378,439 | Image-to-video diffusion model from Lightricks. The 378K downloads and single-file format make it a popular quick-start for video generation. |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 766 | 5,079 | Text-to-music generation model from MiniMax using the diffusers format. It is early in adoption but quickly gaining likes in the generative audio niche. |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 3,971 | 2,212,155 | Series-leading image-text-to-video model with 2.21M downloads. It anchors a massive ecosystem of LoRA, GGUF, ComfyUI, and Turbo derivatives. |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 513 | 211,917 | Turbo variant of MiniMax-H3 optimized for image-to-video generation. Its 211K downloads reflect demand for faster video generation from the H3 base. |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,722 | 2,100,680 | Kimi's latest image-text-to-text model with compressed-tensors and feature-extraction tags. It leads the list with 10,722 likes and 2.1M downloads, making it a standout open multimodal release. |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,344 | 12,790,850 | ComfyUI-ready distribution of MiniMax-H3 as a diffusion single-file. At 12.79M downloads, it is the week's most downloaded artifact and a major signal of local video generation's mainstream status. |
| [Gazingstars123/Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 188 | 16,829 | Text-to-image diffusion model packaged for ComfyUI. Its 2.9B-size, art-focused design gives creators a lightweight image generation option. |
| [dots-studio/dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 160 | 240 | Preview image-text-to-text model from dots-studio aimed at note-oriented multimodal tasks. Low downloads but potential interest as a specialized assistant. |
| [Kijai/MiniMax-H3_comfy](https://huggingface.co/Kijai/MiniMax-H3_comfy) | Kijai | 352 | 0 | ComfyUI-oriented integration/helper repo for MiniMax-H3. It appears because local video-generation tooling is a critical part of the H3 ecosystem. |

### 📦 Fine-tunes & Quantizations

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 1,212 | 867,963 | Unsloth's GGUF quantization of Qwen3.8-27B. It makes the flagship multimodal model easier to run on consumer hardware through GGUF-compatible runtimes. |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 434 | 682,188 | GGUF version of Meta's Muse-Glimmer-30B by Unsloth. Its 682K downloads show strong demand for quantized multimodal assistants. |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 424 | 123,157 | Official FP8 quantized release of Qwen3.8-27B. It reduces memory footprint and speeds inference on FP8-capable GPUs. |
| [meta-models/Muse-Glimmer-30B-GGUF](https://huggingface.co/meta-models/Muse-Glimmer-30B-GGUF) | meta-models | 277 | 321,049 | Meta's own GGUF packaging of Muse-Glimmer-30B. Useful for environments where GGUF loaders such as stable-diffusion.cpp are preferred. |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 270 | 170,554 | NVFP4 quantized version of NVIDIA's 30B-A3B Lightning model. It offers a compact, high-efficiency deployment option for enterprise inference. |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,049 | 2,983,500 | Community GGUF fine-tune of Qwen3.6-27B with MTP support. Its 2.98M downloads highlight sustained appetite for uncensored and roleplay-oriented community models. |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 756 | 0 | LoRA adapter for MiniMax-H3 Turbo targeting text-to-video and text-to-audio flows. Interesting for lightweight extension of the video base, despite zero tracked downloads. |
| [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 192 | 12,737 | Fal's LoRA for MiniMax-H3 designed to improve realism in people-focused video generation. A practical community add-on in the growing H3 ecosystem. |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 193 | 10,745 | Official FP8 variant of Qwen's 2.4T-parameter MoE model. Makes a massive open-weight model more practical on FP8 infrastructure. |
| [unsloth/MiniMax-H3-GGUF](https://huggingface.co/unsloth/MiniMax-H3-GGUF) | unsloth | 164 | 173,741 | GGUF format of MiniMax-H3 from Unsloth. Enables local video generation through stable-diffusion.cpp-compatible runtimes. |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 166 | 90,924 | NVFP4 quantization of Qwen3.8-27B by Unsloth, optimized for NVIDIA GPUs. Provides a memory-efficient alternative to FP8 and BF16 for the flagship multimodal model. |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 148 | 0 | FP8 uncensored community variant of Qwen3.8-27B. It is trending on likes even with zero tracked downloads, showing niche demand for alternative guardrail-free versions. |
| [SexGod1979/PinkCherry_MiniMax-H3](https://huggingface.co/SexGod1979/PinkCherry_MiniMax-H3) | SexGod1979 | 330 | 633 | Community text-to-video fine-tune of MiniMax-H3 with a stylized/creative focus. It reflects the expanding marketplace of specialized video models built around H3. |

## Ecosystem Signal

Qwen and MiniMax-H3 are the clearest momentum stories: Qwen3.8-27B generated a broad spectrum of official and community quantizations, while MiniMax-H3 grew from a base video model into an ecosystem with ComfyUI packaging, Turbo/LoRA/GGUF derivatives, and the list's highest download count. DeepSeek's V4 Flash/Pro pair and NVIDIA's Nemotron 3.5 Lightning show that large text-generation remains actively refreshed, with sparse MoE and FP8/NVFP4 formats becoming standard. Meta's Muse-Glimmer extends the open multimodal field, and Kimi-K3 demonstrates the growing importance of compressed tensors for efficient frontier models.

The trend list is overwhelmingly open-weight; even endpoint-compatible releases are distributed as downloadable weights. Quantization is a primary adoption lever: Unsloth, official FP8/NVFP4, GGUF, and community LoRA variants outnumber base models and drive millions of downloads. Fine-tuning activity is concentrated on Qwen3.8 and MiniMax-H3, with smaller niches for music generation and custom video styles. Overall, frontier labs are releasing large open models, but local/edge deployment through quantization and harnesses determines real-world reach.

## Worth Exploring

- [**moonshotai/Kimi-K3**](https://huggingface.co/moonshotai/Kimi-K3): The week's highest-liked model, with compressed-tensors and feature-extraction capabilities. It is worth studying as a frontier open multimodal release that prioritizes efficiency and embedding-style use cases.
- [**Qwen/Qwen3.8-2.4T-A95B**](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B): A 2.4T-parameter MoE with only 95B active parameters. It offers a rare window into sparse scaling and FP8 deployment of massive open-weight language models.
- [**Comfy-Org/MiniMax-H3**](https://huggingface.co/Comfy-Org/MiniMax-H3): The most-downloaded model on the list at 12.79M downloads. Exploring it reveals how ComfyUI integration and single-file packaging accelerate real-world adoption of video generation models.

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*