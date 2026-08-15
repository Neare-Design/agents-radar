# Hugging Face 热门模型日报 2026-08-16

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-08-15 23:14 UTC

---

# Hugging Face 热门模型日报（2026-08-16）

## 今日速览

今日 Hugging Face 热门榜由多模态模型主导：Kimi-K3 以 10,722 周点赞领跑，Qwen3.8-27B 紧随其后。语言模型方面，DeepSeek V4 系列集中发布 Pro/Flash 新版本，显示头部厂商竞争激烈。视频生成领域 MiniMax-H3 生态爆发，衍生出 LoRA、GGUF、ComfyUI 等众多版本，其中 Comfy-Org 适配版下载量突破千万。社区量化与微调活动持续活跃，GGUF、FP8、NVFP4 等格式覆盖了多数热门模型。

## 热门模型

### 🧠 语言模型（LLM、对话模型、指令微调）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731) | deepseek-ai | 3,418 | 1,798,247 | DeepSeek V4 的 Flash 版本，面向低延迟与高并发推理场景。下载量接近 180 万，是语言模型中下载量最高的型号，凭借高效性能成为开发者首选。 |
| [Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B) | Qwen | 966 | 6,381 | Qwen 的超大规模 MoE 文本模型，总参数 2.4T，激活参数约 95B。作为旗舰文本生成模型，其发布标志着 Qwen 在稀疏架构与效率优化上的最新进展。 |
| [LFM2.5-2.6B](https://huggingface.co/LiquidAI/LFM2.5-2.6B) | LiquidAI | 627 | 135,448 | Liquid AI 推出的 2.6B 小参数语言模型，主打低资源设备上的高效推理。以轻量级设计获得社区关注，周点赞 627。 |
| [DeepSeek-V4-Pro-0813](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813) | deepseek-ai | 488 | 19,945 | DeepSeek V4 的 Pro 版本，侧重复杂推理与专业任务，是新发布的旗舰型号之一。虽然点赞数暂不突出，但下载量已接近 2 万，后续潜力值得关注。 |
| [Ling-3.0-tiny](https://huggingface.co/inclusionAI/Ling-3.0-tiny) | inclusionAI | 256 | 4,832 | Ling-3.0 的 Tiny 版本，定位轻量级语言模型，使用自定义代码实现。点赞 256，作为新面孔进入榜单。 |
| [NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | nvidia | 150 | 62,965 | NVIDIA Nemotron 3.5 Lightning 的 BF16 版，30B 总参数、3B 激活的 MoE 语言模型。代表英伟达在高效对话模型领域的最新尝试。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3) | moonshotai | 10,722 | 2,100,680 | 月之暗面的多模态大模型，支持图像与文本混合输入，并采用压缩张量技术。以 10,722 周点赞高居榜首，下载量超 210 万，是当前社区最热模型。 |
| [Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 9,762 | 91,917 | Qwen 3.8 系列多模态对话模型，可处理图像与文本输入，生成自然语言回复。周点赞 9,762，下载量超 9 万，是通用多模态助手的有力竞争者。 |
| [MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 3,971 | 2,212,155 | MiniMax 的视频生成模型，支持文本/图像/视频条件生成视频。下载量超 221 万，点赞 3,971，是视频生成赛道的热门基座模型。 |
| [Muse-Glimmer-30B](https://huggingface.co/meta-models/Muse-Glimmer-30B) | meta-models | 1,574 | 246,454 | Meta 推出的 30B 多模态模型，擅长图像-文本对话任务。下载量达 24.6 万，显示其在视觉语言研究中的广泛使用。 |
| [LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 937 | 378,439 | Lightricks 开发的视频生成模型，支持图像到视频、文本到视频及视频到视频。下载量 37.8 万，以高灵活性和生成质量受到关注。 |
| [MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3) | MiniMaxAI | 766 | 5,079 | MiniMax 的音乐生成模型，可根据文本描述生成完整音乐片段。作为少见的文本到音乐模型上榜，点赞 766，展示了音频生成的发展。 |
| [Minimax-h3-Turbo](https://huggingface.co/lightx2v/Minimax-h3-Turbo) | lightx2v | 513 | 211,917 | MiniMax-H3 的 Turbo 版本，优化推理速度以适应实时生成场景。点赞 513，下载 21 万，是视频生成任务中低延迟需求的常见选择。 |
| [Anima-2.9B](https://huggingface.co/Gazingstars123/Anima-2.9B) | Gazingstars123 | 188 | 16,829 | 独立开发者推出的文生图模型，采用单文件 diffusion 格式并兼容 ComfyUI。点赞 188，是社区创意生态中的新作品。 |
| [dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev) | dots-studio | 160 | 240 | dots-studio 的多模态模型预览版，支持图像文本输入与文本生成。点赞 160，处于早期发布阶段。 |

### 🔧 专用模型（代码、数学、医疗、嵌入）

（本分类暂无上榜模型）

### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞 | 下载 | 简要说明 |
| :--- | :--- | ---: | ---: | :--- |
| [DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF) | DavidAU | 2,049 | 2,983,500 | 社区用户制作的高强度微调版 Qwen3.6-27B，采用 GGUF 格式，主打无审查与角色扮演风格。点赞 2,049，下载量近 300 万，是社区微调领域的现象级作品。 |
| [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3) | Comfy-Org | 1,344 | 12,790,850 | Comfy-Org 为 MiniMax-H3 发布的 ComfyUI 专用单文件版本，大大降低了视频模型的使用门槛。下载量高达 1,279 万，是榜单中下载量最高的模型资源。 |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 1,212 | 867,963 | unsloth 提供的 Qwen3.8-27B GGUF 量化版，可在消费级硬件上运行。下载量 86.8 万，是 Qwen3.8 本地部署的主流选择。 |
| [larryvrh/MiniMax-H3-Turbo-Lora](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora) | larryvrh | 756 | 0 | 为 MiniMax-H3-Turbo 设计的 LoRA 模块，支持文本到视频生成调整。点赞 756，目前下载为 0，属于新鲜发布时间段。 |
| [unsloth/Muse-Glimmer-30B-GGUF](https://huggingface.co/unsloth/Muse-Glimmer-30B-GGUF) | unsloth | 434 | 682,188 | unsloth 制作的 Muse-Glimmer-30B GGUF 量化版本，便于本地推理。下载量 68.2 万，是该模型生态中最受欢迎的量化选择之一。 |
| [Qwen3.8-27B-FP8](https://huggingface.co/Qwen/Qwen3.8-27B-FP8) | Qwen | 424 | 123,157 | Qwen 官方发布的 FP8 量化版 27B 模型，在减少显存占用同时保持较好精度。点赞 424，适合高端推理卡部署。 |
| [Kijai/MiniMax-H3_comfy](https://huggingface.co/Kijai/MiniMax-H3_comfy) | Kijai | 352 | 0 | Kijai 为 MiniMax-H3 适配的 ComfyUI 工作流组件，帮助用户在 ComfyUI 中直接调用视频模型。点赞 352，目前尚在早期发布。 |
| [PinkCherry_MiniMax-H3](https://huggingface.co/SexGod1979/PinkCherry_MiniMax-H3) | SexGod1979 | 330 | 633 | 社区微调的 MiniMax-H3 变体，针对特定视觉风格进行优化。点赞 330，为视频生成提供更细分的风格选项。 |
| [meta-models/Muse-Glimmer-30B-GGUF](https://huggingface.co/meta-models/Muse-Glimmer-30B-GGUF) | meta-models | 277 | 321,049 | Meta 官方发布的 Muse-Glimmer GGUF 量化版，方便用户直接下载运行。点赞 277，下载 32 万，是官方生态的重要补充。 |
| [NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) | nvidia | 270 | 170,554 | NVIDIA 对 Nemotron Lightning 30B 的 NVFP4 量化版，针对最新一代 GPU 优化。点赞 270，为高端硬件提供高效推理方案。 |
| [Qwen3.8-2.4T-A95B-FP8](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B-FP8) | Qwen | 193 | 10,745 | Qwen 超大规模 MoE 模型的 FP8 量化版，降低巨大模型的部署门槛。点赞 193，适合有强烈算力需求的用户。 |
| [MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) | fal | 192 | 12,737 | fal 提供的 MiniMax-H3 写实人物 LoRA，增强视频生成中的人脸与人体真实感。点赞 192，为人物视频生成提供垂直优化。 |
| [unsloth/Qwen3.8-27B-NVFP4](https://huggingface.co/unsloth/Qwen3.8-27B-NVFP4) | unsloth | 166 | 90,924 | unsloth 制作的 NVFP4 量化版，适配 NVIDIA 新硬件。点赞 166，下载 9 万，是 FP8 之外的另一种精度补充。 |
| [unsloth/MiniMax-H3-GGUF](https://huggingface.co/unsloth/MiniMax-H3-GGUF) | unsloth | 164 | 173,741 | unsloth 将视频模型 MiniMax-H3 量化为 GGUF 格式，支持通过 stable-diffusion.cpp 等框架运行。点赞 164，是视频模型轻量化的稀有尝试。 |
| [orcarouter/Qwen3.8-27B-Uncensored-FP8](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-FP8) | orcarouter | 148 | 0 | 社区微调的 “Uncensored” Qwen3.8-27B，并采用 FP8 精度。点赞 148，下载为 0，是刚上线的极端风格微调。 |

## 生态信号

当前生态呈现显著的“多模态+视频生成”爆发期，Qwen、MiniMax、Kimi 等头部家族均在多模态领域发力，其中 MiniMax-H3 衍生出完整的工具链，从 LoRA 到 ComfyUI 再到 GGUF，社区协作深度空前。语言模型方面，DeepSeek V4 与 Nemotron 3.5 以 MoE 架构争夺效率高地，开源权重仍是主流，但全开源与部分开源并存。量化活动高度活跃，GGUF 与 NVFP4/FP8 双线并行，覆盖从本地 CPU 到数据中心 GPU 的部署需求；社区微调则呈现出“风格化、去审查、角色定制”等个性化趋势，也反映出非常规使用场景的旺盛需求。

## 值得探索

1. **[Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** — 周点赞和下载量双高，且采用压缩张量技术，代表多模态模型效率优化的前沿方向，值得深入研究。

2. **[Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3)** — 千万级下载量表明其将前沿视频生成无缝接入 ComfyUI，是本地视频创作的最佳实践，适合视频开发者体验。

3. **[DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.6-27B-Fable-Fusion-711-Uncensored-Heretic-NM-DAU-NEO-MAX-MTP-GGUF)** — 作为社区微调代表，展示了个性化模型在开源社区的传播力，也可用于研究“去审查”微调对模型行为的影响。

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*