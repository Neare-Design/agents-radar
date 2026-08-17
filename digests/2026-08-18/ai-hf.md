# Hugging Face 热门模型日报 2026-08-18

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-17 23:16 UTC

---

# Hugging Face 热门模型日报（2026-08-18）

## 今日速览

2026-08-18 的 Hugging Face 热门榜由多模态模型主导：Kimi-K3 以 10,800 周点赞登顶，Qwen3.8-27B 以 10,698 紧随其后。Qwen 家族成为最大赢家，官方权重与 GGUF/FP8/NVFP4 量化版本同时上榜。MiniMax-H3 的视频生成生态持续爆发，衍生出 ComfyUI 单文件、Turbo 变体和 LoRA 等二次分发。DeepSeek-V4-Flash 下载量接近 200 万，多款 Qwen GGUF 则超过 200 万，反映轻量部署与低比特量化是社区刚需。总体来看，开源权重、多模态生成和量化微调工具链正形成强耦合生态。

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,496 | 1,978,298 | DeepSeek V4 的 Flash 轻量版（7/31 版本），面向高并发文本生成。下载量接近 200 万，显示低成本推理场景的强烈需求。 |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 1,041 | 9,465 | Qwen 3.8 系列超大规模 MoE 文本生成模型，总参数 2.4T、激活 95B。官方首发即获破千点赞，是当前最受关注的旗舰开源 MoE。 |
| [LiquidAI/LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 653 | 147,270 | LiquidAI 的 2.6B 语言模型，主打高效文本生成。参数规模小但下载量可观，适合边缘部署和学术研究。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 573 | 25,006 | DeepSeek V4 的 Pro 旗舰版本（8/13 更新）。本周点赞相对内敛，但作为重量级模型仍值得跟踪。 |
| [inclusionAI/Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 306 | 6,266 | 任务类型未标注的轻量级模型，采用 bailing_hybrid 自定义结构并开放 MIT 许可。tiny 尺寸适合本地与多场景实验，社区关注度上升。 |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 169 | 69,833 | NVIDIA Nemotron 3.5 Lightning 的 BF16 原始精度版本，30B 参数、3B 激活的 MoE 架构。官方原生权重为后续量化与部署提供基线。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,800 | 2,163,953 | Kimi 最新多模态模型，支持图像+文本输入，并集成压缩张量技术。周点赞 10,800 登顶，下载量超 216 万，是本周现象级开源发布。 |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 10,698 | 415,039 | Qwen 3.8 系列 27B 图文对话模型，可直接进行视觉问答。以 10,698 点赞位居第二，带动了后续多款量化与微调衍生版本。 |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,086 | 2,403,238 | MiniMax 的图像到视频生成大模型，也支持文本到视频。点赞 4,086、下载 240 万，衍生出 ComfyUI/Turbo/LoRA 等生态。 |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,660 | 334,099 | 30B 参数的多模态对话模型，面向图文理解生成。周点赞 1,660，是本周新晋视觉语言模型中的黑马。 |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,402 | 14,015,769 | MiniMax-H3 的 ComfyUI 单文件分发版，可在 ComfyUI 中直接加载。下载量高达 14,015,769，是生态中分发最广的视频模型格式。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 1,104 | 465,529 | Lightricks 推出的图像/文本到视频扩散模型，支持多类视频任务。以 1,104 点赞稳居视频生成第一梯队，下载量超过 46 万。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 900 | 10,375 | Music3 是 MiniMax 的文本到音乐生成模型，专注音乐创作场景。900 周点赞表明音频生成赛道也在升温。 |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 583 | 264,351 | MiniMax-H3 的 Turbo 版本，支持图像/文本到视频，主打更快推理。下载量 26 万，是 H3 生态的社区增强变体。 |
| [Gazingstars123/Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 233 | 23,202 | 文生图扩散单文件模型，兼容 ComfyUI。2.9B 轻量参数，适合本地图像生成实验。 |
| [dots-studio/dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 205 | 633 | dots3 系列的多模态预览版，任务为图文输入。虽下载量不高，但新团队作品进入榜单值得尝鲜关注。 |
| [Comfy-Org/MiniMax-Music-3](https://huggingface.co/Comfy-Org/MiniMax-Music-3) | Comfy-Org | 166 | 256,988 | MiniMax Music3 的 ComfyUI 单文件版，允许在 ComfyUI 中生成音乐。下载量 25 万+，说明音频生成与 ComfyUI 工作流的结合需求强劲。 |
| [LiquidAI/LFM2.5-VL-3B](https://huggingface.co/LiquidAI/LFM2.5-VL-3B) | LiquidAI | 162 | 6,816 | LiquidAI 的 3B 视觉语言模型，支持图文多模态任务。轻量级定位与扩展生态值得关注。 |

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,119 | 3,033,928 | 基于 Qwen 3.6 的大型社区微调版本，主打 Uncensored/Heretic 风格并输出 GGUF 格式。下载量超 303 万，说明去审查微调市场热度极高。 |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 1,623 | 2,727,609 | Qwen3.8-27B 的 unsloth 量化版，GGUF 格式便于本地部署。下载量高达 272 万，是本地运行该多模态模型的首选格式之一。 |
| [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates) | froggeric | 1,209 | 0 | 修复 Qwen3.5 系列对话模板的 Jinja/MLX 工具包，并非模型权重。0 下载仍获 1,209 点赞，体现社区对对话模板兼容性的强烈需求。 |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 786 | 0 | 针对 MiniMax-H3-Turbo 的 LoRA 适配器，支持文本/音频驱动的视频生成。0 下载而高赞，可能是刚发布的新资源，值得关注。 |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 527 | 495,646 | Qwen 官方发布的 FP8 量化多模态模型，在精度和显存占用中取得平衡。下载量近 50 万，是官方量化路线的重要组成。 |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 470 | 755,125 | unsloth 将 Muse-Glimmer-30B 转为 GGUF，便于 llama.cpp 本地部署。75 万+下载显示该视觉语言模型在端侧/本地社区受欢迎。 |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 430 | 15,812 | Qwen3.8-27B 的 abliterated 去审查 FP8 版。针对本地/边缘场景降低门槛，适合需要更少回答限制的用户。 |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 307 | 231,271 | NVIDIA 官方 NVFP4 量化版 Nemotron 3.5 Lightning。30B MoE 在低比特下保持高效，下载量已达 23 万。 |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 297 | 357,701 | Qwen3.8-27B 的社区去审查 GGUF 版本，兼容 llama.cpp 并包含 MTP 支持。35 万下载显示该方向的本地需求。 |
| [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 243 | 18,562 | fal 为 MiniMax-H3 制作的现实主义人物 LoRA。用于提升生成视频中人物写实度，是 H3 微调生态中的热门组件。 |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 237 | 378,177 | unsloth 推出的 Qwen3.8-27B NVFP4 量化版。适配 NVIDIA 新一代硬件，下载量 37.8 万，兼顾多模态能力与显存效率。 |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 219 | 12,295 | Qwen 3.8 2.4T MoE 的官方 FP8 量化版。显著降低存储与推理资源要求，适合在受限环境运行超大模型。 |

## 生态信号

本周 Qwen 家族势头最盛：原版、FP8、NVFP4、GGUF、Uncensored 多个变体同时出现在榜，形成从旗舰权重到本地部署的完整链路。MiniMax-H3 在视频生成侧同样形成生态，ComfyUI 单文件、Turbo 与 LoRA 互相加持；音乐生成（MiniMax-Music3）也开始被 ComfyUI 承接。DeepSeek V4 双版本上榜，Flash 下载量接近 200 万。开源权重仍是主导，但社区注意力已明显从“能用”转向“更好用”——低比特量化、模板修复、LoRA 定向微调成为口碑来源。闭源模型未进入榜单。

## 值得探索

1. **[Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)** —— 周点赞第二，多模态对话能力强，且官方/社区量化版本丰富，适合做视觉聊天和本地部署实验。
2. **[moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** —— 周点赞第一，具备 compressed-tensors 和 feature-extraction 标签，值得研究压缩多模态表征与高效推理。
3. **[MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** —— 本周视频生成“爆款”，配合 Comfy-Org 单文件与 LoRA 可快速搭建视频生成工作流。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*