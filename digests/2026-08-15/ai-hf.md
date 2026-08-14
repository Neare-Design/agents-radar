# Hugging Face 热门模型日报 2026-08-15

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-14 23:14 UTC

---

# Hugging Face 热门模型日报（2026-08-15）

## 今日速览

本周 Hugging Face 被多模态与视频生成统治：Moonshot AI 的 Kimi-K3 周点赞破万，成为榜首；MiniMax-H3 系列衍生出大量量化、LoRA 和 ComfyUI 版本，实际下载量超千万。Qwen3.8 系列和 DeepSeek-V4 分别以多模态和高效文本生成领跑各自赛道，且“发布即量化”成为标配。开源权重继续主导，NVIDIA、Meta 等厂商也以开放姿态抢占生态入口。

## 热门模型

### 🧠 语言模型

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,380 | 1,606,491 | DeepSeek V4 的 Flash 版本，定位于高效文本生成。下载量超 160 万，是本周最受欢迎的轻量级 LLM 之一。 |
| [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 911 | 3,832 | Qwen3.8 系列的超大规模 MoE 模型，总参数 2.4T，激活 95B。代表新一代稀疏架构，发布后迅速获得近千点赞。 |
| [LiquidAI/LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 614 | 124,172 | 2.6B 小参数语言模型，主打轻量高效部署。下载量超过 12 万，适合边缘端或资源受限场景。 |
| [deepseek-ai/DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 431 | 245 | DeepSeek V4 的 Pro 版本，侧重生成质量。虽然刚发布下载不多，但已获得 431 赞，被视为 V4 系列旗舰。 |
| [inclusionAI/Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 234 | 2,283 | Ling 3.0 的 Tiny 版本，采用 bailing_hybrid 定制架构。面向轻量部署，自定义代码给开发者带来较高可玩性。 |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 141 | 34,137 | Nemotron 3.5 Lightning 的 BF16 版，30B 总量、3B 激活。适合研究高效推理，与 NVFP4 量化版互补。 |

### 🎨 多模态与生成

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,670 | 1,974,635 | Moonshot AI 的多模态模型，支持图像文本理解与特征提取。周点赞破万，压缩张量技术是重要看点。 |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 8,910 | 2 | Qwen3.8 系列的 27B 多模态对话模型。周点赞近 9 千，说明社区对新一代 Qwen 多模态能力充满期待。 |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 3,917 | 1,997,541 | MiniMax 最新视频生成模型，支持图像/文本到视频。下载量接近 200 万，是本周视频生成的核心模型。 |
| [meta-models/Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,509 | 165,300 | Meta 开源的 30B 多模态模型，专注于视觉-语言任务。下载超 16 万，大厂开源权重关注度高。 |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,317 | 11,768,622 | 为 ComfyUI 优化的 MiniMax-H3 单文件版。下载量突破 1176 万，是实际社区使用最广的视频模型文件之一。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 851 | 207,830 | 图像到视频模型，支持文本/图像/视频到视频。下载超 20 万，是 MiniMax 之外的重要视频生成选择。 |
| [MiniMaxAI/MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 646 | 63 | 文本到音乐生成模型，拓展 MiniMax 音频能力。发布即获 646 赞，但下载尚少，属于早期关注阶段。 |
| [lightx2v/Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 493 | 149,865 | 社区加速版 MiniMax-H3 Turbo，针对图像/视频生成优化。下载近 15 万，主打更快推理。 |
| [nvidia/NVIDIA-NemotronLabs-VoiceChat-11B](https://huggingface.co/nvidia/NVIDIA-NemotronLabs-VoiceChat-11B) | nvidia | 381 | 1,366 | NVIDIA 的端到端语音对话模型，集成多篇最新论文技术。适合实时语音交互，是语音多模态方向的重要探索。 |
| [Gazingstars123/Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 160 | 10,106 | 2.9B 文本到图像扩散模型，单文件格式方便本地使用。下载过万，社区活跃度不错。 |
| [dots-studio/dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 136 | 11 | dots3 系列预览版多模态模型，支持图像文本到文本。目前处于早期预览，点赞上升但下载量很小。 |

### 🔧 专用模型

本日榜单暂无明显代码、数学、医疗等专用模型，此分类省略。

### 📦 微调与量化

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,015 | 2,891,524 | 社区微调 Qwen3.6 27B 模型，带有多种后续微调与“非审查”取向，提供 GGUF。下载近 290 万，是当前最热门的自定义模型之一。 |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 766 | 0 | Unsloth 出品的 Qwen3.8-27B GGUF 量化版，方便本地部署。刚发布下载为 0，但已经获得大量关注。 |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 741 | 0 | 面向 MiniMax-H3-Turbo 的 LoRA，扩展文本到视频及音频-视频生成。发布初期暂无下载，但点赞 741，说明社区兴趣浓厚。 |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 414 | 596,774 | Meta Muse-Glimmer-30B 的 GGUF 版本，为多模态模型提供便捷量化格式。下载近 60 万，是量化生态中非常受欢迎的选择。 |
| [Kijai/MiniMax-H3_comfy](https://huggingface.co/Kijai/MiniMax-H3_comfy) | Kijai | 339 | 0 | 专为 ComfyUI 工作流准备的 MiniMax-H3 版本。点赞 339，下载为 0，主要采用“工具型”发布形态。 |
| [drbaph/MiniMax-H3-Turbo-Lora-ComfyUI](https://huggingface.co/drbaph/MiniMax-H3-Turbo-Lora-ComfyUI) | drbaph | 318 | 112,975 | MiniMax-H3-Turbo LoRA 的 ComfyUI 适配版，方便在 ComfyUI 中直接加载。下载超 11 万，是视频工作流常用组件。 |
| [SexGod1979/PinkCherry_MiniMax-H3](https://huggingface.co/SexGod1979/PinkCherry_MiniMax-H3) | SexGod1979 | 309 | 473 | 基于 MiniMax-H3 的社区微调视频模型，主打“PinkCherry”风格。采用 Apache-2.0 许可，可商用，关注度较高。 |
| [Qwen/Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 289 | 0 | Qwen3.8-27B 的官方 FP8 量化版，节省显存并适合高吞吐。发布即获得关注，下载暂为 0。 |
| [meta-models/Muse-Glimmer-30B-GGUF](https://huggingface.co/meta-models/Muse-Glimmer-30B-GGUF) | meta-models | 269 | 228,364 | Meta 官方发布的 Muse-Glimmer-30B GGUF 版本，下载超 22 万。官方直接提供量化格式，降低多模态模型使用门槛。 |
| [nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 257 | 119,572 | Nemotron 3.5 Lightning 的 NVFP4 量化版，以 4 位浮点实现高效推理。下载近 12 万，适合 GPU 资源优化。 |
| [Qwen/Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 184 | 9,334 | 超大规模 Qwen3.8 MoE 模型 FP8 版本，降低 2.4T 总参数的部署门槛。下载近万，适合多卡推理实验。 |
| [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 176 | 9,060 | fal.ai 推出的 MiniMax-H3 写实人物 LoRA，增强人物真实感。下载超 9 千，是视频创作常用的定制组件。 |
| [unsloth/MiniMax-H3-GGUF](https://huggingface.co/unsloth/MiniMax-H3-GGUF) | unsloth | 155 | 136,774 | MiniMax-H3 视频模型的 GGUF 版本，可在 stable-diffusion.cpp 中运行。下载超 13 万，扩展了视频模型的跨生态兼容性。 |

## 生态信号

本周模型生态呈现三个明显趋势：

1. **Qwen 与 MiniMax 家族领跑**：Qwen3.8 系列（多模态、MoE、FP8/GGUF）形成完整发布矩阵；MiniMax-H3 则通过 Turbo、LoRA、ComfyUI、GGUF 等周边衍生，快速构建起视频生成工具链。

2. **开源权重成为主流**：榜单上几乎所有模型都开放权重，且不少采用 Apache-2.0 等宽松许可；NVIDIA、Meta 等大厂也以开放姿态入场，进一步挤压闭源竞争空间。

3. **量化与微调空前活跃**：GGUF、FP8、NVFP4 等量化格式覆盖了从 2.6B 到 2.4T 全部量级，Unsloth 与 ComfyUI 是最关键的生态中间层；社区微调集中在“非审查”对话和视频 LoRA 上，热门模型下载量动辄数十万。

## 值得探索

- [moonshotai/Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)：周赞第一的多模态模型，压缩张量技术让部署门槛大幅降低，最适合研究如何在资源受限环境中使用强模型。
- [Qwen/Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B)：2.4T 总参、95B 激活的超大规模 MoE，是探索下一代稀疏架构和超大规模推理的最佳样本。
- [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3)：下载量突破 1176 万的视频模型单文件版，配合 ComfyUI 生态，是当前 AIGC 视频创作最容易落地的选择。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*