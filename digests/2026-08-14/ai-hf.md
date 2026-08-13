# Hugging Face 热门模型日报 2026-08-14

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-13 23:34 UTC

---

# Hugging Face 热门模型日报（2026-08-14）

## 今日速览

本周 Hugging Face 周榜上，视频生成是绝对主线：MiniMax-H3 官方模型及其 ComfyUI、LoRA、GGUF、Turbo 衍生品占据超过三分之一席位，单文件版下载量突破千万。Kimi-K3 以 10,620 点赞成为本周人气王，显示大厂开源多模态模型仍是最大流量入口。语言模型方面，DeepSeek-V4-Flash 下载超 140 万，Qwen3.8 MoE 与 NVIDIA Nemotron Lightning 构成新的基座矩阵。开源社区围绕 30B 级模型展开的 GGUF/FP8 量化与角色扮演微调异常活跃，本地部署和 ComfyUI 工作流已成为重要分发场景。

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,316 | 1,431,587 | DeepSeek V4 的 Flash 文本生成模型，主打高效对话与低成本部署。本周点赞和下载双高，是语言模型板块的流量担当。 |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 779 | 1,012 | Qwen3.8 系列 MoE，2.4T 总参数、约 95B 激活。目前下载量不高，但新架构和配套 FP8 版本值得关注。 |
| [LiquidAI/LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 602 | 116,640 | Liquid AI 的 2.6B 小参数语言模型。适合端侧和资源受限场景，116K 下载说明小模型仍有明确需求。 |
| [deepgrove/maple-preview](https://huggingface.co/deepgrove/maple-preview) | deepgrove | 352 | 3,868 | 混合专家结构的文本生成预览模型。处于早期发布阶段，适合关注 MoE 架构演进的开发者。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 273 | 0 | DeepSeek V4 的 Pro 版，定位强推理能力。目前 0 下载，说明属于刚发布、尚未扩散的新权重。 |
| [inclusionAI/Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 216 | 1,292 | 页面任务未标注，采用 bailing_hybrid 混合架构标记的轻量级模型。MIT 协议加持，适合研究与实验。 |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 129 | 22,279 | NVIDIA Nemotron 3.5 Lightning 的 BF16 基座版本。总参 30B、激活约 3B，是高效 MoE 推理的代表。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,620 | 1,871,575 | Kimi 最新的 image-text-to-text 多模态模型，支持图像与文本输入。10,620 赞高居周榜第一，带有 compressed-tensors 标签，压缩与表征能力值得研究。 |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 3,818 | 1,605,940 | MiniMax H3 视频生成基座，支持 image-text-to-video。3,818 赞、1.6M 下载，是本周视频生成生态的核心源头。 |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,414 | 121,042 | meta-models 的 30B 多模态对话模型，支持图像与文本输入。1,414 赞，是官方与社区多个量化版本的共同基座。 |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,288 | 10,365,210 | MiniMax-H3 的 ComfyUI 单文件打包版。10.36M 下载为全榜第一，体现 ComfyUI 在视频生成部署中的核心地位。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 715 | 57,287 | Lightricks 2.5 代视频生成模型，支持 image-to-video、text-to-video、video-to-video 等。适合创意工具链和视频生成实验。 |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 459 | 91,455 | H3 的 Turbo 加速版本，任务为 image-to-video。91K 下载说明社区对低延迟视频生成有强烈需求。 |
| [nvidia/NVIDIA-NemotronLabs-VoiceChat-11B](https://huggingface.co/nvidia/NVIDIA-NemotronLabs-VoiceChat-11B) | nvidia | 371 | 1,164 | NVIDIA 的 11B 语音对话模型，面向英语语音交互场景。371 赞，是本周语音方向的重要代表。 |
| [Kijai/MiniMax-H3_comfy](https://huggingface.co/Kijai/MiniMax-H3_comfy) | Kijai | 303 | 0 | Kijai 提供的 MiniMax-H3 ComfyUI 适配仓库。0 下载但获得 303 赞，适合本地用户把 H3 接入 ComfyUI 工作流。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 287 | 25 | MiniMax 音乐生成模型，支持 text-to-audio。刚发布不久，下载量很低，但音乐生成赛道值得留意。 |
| [Kijai/MiniMax-H3-experimental](https://huggingface.co/Kijai/MiniMax-H3-experimental) | Kijai | 221 | 0 | Kijai 的 H3 实验性适配版本。0 下载但获 221 赞，适合尝试 ComfyUI 最新实验功能。 |
| [endless-frontier/BigBang-v1](https://huggingface.co/endless-frontier/BigBang-v1) | endless-frontier | 188 | 3,184 | 基于 qwen3_5_moe 的 image-text-to-text 对话模型。188 赞，是社区在 Qwen 多模态 MoE 上的探索。 |

### 🔧 专用模型（代码、数学、医疗、嵌入）

本期没有模型明确归入该分类，故省略表格。

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 1,986 | 2,793,115 | 基于 Qwen3.6-27B 的社区融合微调模型，GGUF 量化，主打角色扮演与去审查方向。2.79M 下载说明本地化角色扮演与生成类场景需求很大。 |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 725 | 0 | MiniMax-H3 Turbo 的 LoRA 适配器。725 赞但 0 下载，可能刚发布，适合在 H3 生成流程中做风格或质量增强。 |
| [ethanfel/Qwen3-VL-32B-Ultra-Heretic-H3-ComfyUI-INT8-ConvRot](https://huggingface.co/ethanfel/Qwen3-VL-32B-Ultra-Heretic-H3-ComfyUI-INT8-ConvRot) | ethanfel | 483 | 0 | Qwen3-VL-32B 的社区魔改版，集成 H3/ComfyUI 兼容并做 INT8 量化。483 赞但 0 下载，属于早期话题性发布。 |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 390 | 352,023 | unsloth 制作的 Muse-Glimmer-30B GGUF 量化版。352K 下载甚至超过原版，是本地部署多模态模型的热门选择。 |
| [drbaph/MiniMax-H3-Turbo-Lora-ComfyUI](https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI) | drbaph | 314 | 0 | MiniMax-H3 Turbo LoRA 的 ComfyUI 封装。314 赞、0 下载，适合在 ComfyUI 工作流中直接调用。 |
| [SexGod1979/PinkCherry_MiniMax-H3](https://huggingface.co/SexGod1979/PinkCherry_MiniMax-H3) | SexGod1979 | 298 | 324 | 社区制作的 MiniMax-H3 二次风格化版本。298 赞，兼容 transformer endpoints，面向视频生成场景。 |
| [meta-models/Muse-Glimmer-30B-GGUF](https://huggingface.co/meta-models/Muse-Glimmer-30B-GGUF) | meta-models | 257 | 136,783 | 作者方发布的 Muse-Glimmer-30B GGUF 版本。257 赞、136K 下载，与 unsloth 量化版形成互补。 |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 229 | 44,859 | NVIDIA 官方 NVFP4 4-bit 量化版。适合在 NVIDIA GPU 上做高吞吐推理部署。 |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 157 | 4,000 | Qwen3.8 MoE 的 FP8 量化版。为超大 MoE 推理降低显存和带宽门槛。 |
| [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 157 | 4,692 | fal 提供的 H3 写实人物 LoRA。用于视频生成中的人物写实增强，下载量在 H3 LoRA 中较突出。 |
| [unsloth/MiniMax-H3-GGUF](https://huggingface.co/unsloth/MiniMax-H3-GGUF) | unsloth | 149 | 111,222 | MiniMax-H3 的 GGUF 量化版。111K 下载说明视频生成模型也开始出现 GGUF/stable-diffusion.cpp 部署路线。 |
| [lightx2v/MiniMax-H3-Prompt-Rewriter-LoRA](https://huggingface.co/lightx2v/MiniMax-H3-Prompt-Rewriter-LoRA) | lightx2v | 148 | 652 | 面向 MiniMax-H3 的提示词改写 LoRA。用于优化视频生成提示词，是 H3 工作流中的实用组件。 |

## 生态信号

本周生态最突出的是 MiniMax-H3：从官方权重到 ComfyUI 单文件、Turbo 加速、LoRA 与 GGUF 衍生，已形成完整工具链。开源权重仍是主流，Kimi、Qwen、DeepSeek、NVIDIA 等大厂均选择开放模型换取生态影响力。量化活动集中在 30B 级模型，GGUF/FP8/NVFP4/INT8 多路并进；社区微调则明显偏向角色扮演、去审查与 ComfyUI 部署场景。

## 值得探索

- [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)：本周点赞第一、下载 187 万。compressed-tensors 与 feature-extraction 标签让它不仅是对话模型，更是多模态压缩表征的研究样本。
- [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) 与 [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3)：H3 是当下最热视频生成基座，ComfyUI 版本下载超千万，适合进一步研究视频生成与 ComfyUI 生态结合。
- [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B)：Qwen 新一代 MoE，2.4T 总参、约 95B 激活，配合 FP8 量化版本，适合测试超大规模 MoE 的推理性价比。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*