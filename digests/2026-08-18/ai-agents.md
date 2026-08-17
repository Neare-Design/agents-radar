# OpenClaw 生态日报 2026-08-18

> Issues: 500 | PRs: 500 | 覆盖项目: 12 个 | 生成时间: 2026-08-17 23:16 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [NanoBot](https://github.com/HKUDS/nanobot)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [PicoClaw](https://github.com/sipeed/picoclaw)
- [NanoClaw](https://github.com/qwibitai/nanoclaw)
- [NullClaw](https://github.com/nullclaw/nullclaw)
- [IronClaw](https://github.com/nearai/ironclaw)
- [LobsterAI](https://github.com/netease-youdao/LobsterAI)
- [Moltis](https://github.com/moltis-org/moltis)
- [CoPaw](https://github.com/agentscope-ai/CoPaw)
- [ZeptoClaw](https://github.com/qhkm/zeptoclaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

好的，这是基于 2026-08-18 日 GitHub 数据生成的 OpenClaw 项目动态日报。

---

# OpenClaw 项目动态日报 - 2026-08-18

## 1. 今日速览

项目今日活跃度极高，24小时内收到 500 条 Issue 更新与 500 条 PR 更新，但净关闭量较低（Issue 关闭 12 / PR 关闭/合并 98），反映出社区反馈量大而维护端处理能力存在瓶颈。Issue 堆积严重，大量 P0/P1 级稳定性问题（崩溃、消息丢失、会话阻塞、认证失败）仍在反复出现，且多个问题已持续数月未合入修复 PR。PR 侧以修复为主，围绕会话恢复、UI 进度卡片统一、Windows 运行时兼容性与安全边界收紧展开。无新版本发布，项目处于“高活跃产出/高积压待清”状态。

## 2. 版本发布

今日无新版本发布（最新 Releases 为空）。

## 3. 项目进展

今日合并/关闭的 PR 数量为 98，虽未包含重大功能特性，但在稳定性、安全性和基础设施方面有所推进。

- **测试基础设施修复**：PR #125440 修复了浏览器测试共享 `/tmp` 目录导致互相污染的问题；PR #125441 进一步消除了受管子进程检查中的竞态条件，解决了 CI 偶发失败（如阻塞 PR #124950），提升了开发与测试的可靠性。
- **安全边界收紧**：PR #124687 停止在引导/CLI 输出中打印可复用的 Gateway Token（防止进入终端历史与日志）；PR #125434 将所有面向模型的节点操作重定向到专用策略过滤表面，阻止通过泛化工具绕过权限豁免。
- **多端 UI 交互统一**：关于“持久进度卡”的跨端迁移正在推进（PR #125438、#125442、#125444），该系列 PR 旨在解决长时任务状态在 Web/桌面/iOS/Android 端展示不一致、且随 run 结束即消失的问题。
- **插件发现机制加固**：PR #125117 修复了插件元数据扫描时静默丢弃损坏/非对象清单的问题，避免后续 Provider 认证、静态模型目录和 Doctor 迁移丢失配置。

## 4. 社区热点

- **#77598（评论 23）**：一份针对核心维护者 Pash 开发 Agent 的 24 小时观察日志（全程不干预、不重启）。该 Issue 成为最热讨论，反映出社区对 Agent 行为透明度、轨迹追踪与可观测性的强烈兴趣。
- **#91009（评论 20）**：`@openclaw/codex` 集成中，Codex PreToolUse Hook 中继会生成多个 CPU 占比 100%+ 的 `openclaw-hooks` 进程，并阻塞 Gateway RPC。该问题被标记为 `platinum hermit`（最高优先级），直接影响生产吞吐。
- **#62505（评论 15）**：编码 Agent 在 2026.4.2 之后出现严重回归——“从不完成任何工作”，仅返回模糊状态更新。该 Issue 距今已 4 个月仍无 fix PR，用户表达了强烈不满。
- **#96834（评论 15）**：WhatsApp 1:1 发送原生图片会堵塞主消息通道约 3 分钟才处理，多模态输入导致 `active_reply_work` 卡死。
- **#68596（评论 15）**：DeepSeek-R1 / Kimi-K2.5 等长思考模型触发流式 Watchdog 误报（30s 无更新即重置状态），用户要求提供可配置的 Watchdog 超时阈值。

## 5. Bug 与稳定性

以下 Bug 按严重程度排列，**严重性标注依据 Issue 标签（P0/P1/Platinum Hermit 等）**。

- **P0 级（长期阻塞）**：
  - **#70903**：Provider 返回 402 后，OpenClaw 将 `disabledUntil` 持久化到文件，账单恢复后用户仍被禁用数小时。该 P0 已滞留近 4 个月（创建于 4 月 24 日），虽标记 stale 但影响恶劣，需紧急干预。

- **P1 级（关键回归/崩溃）**：
  - **#62505**【回归】：编码 Agent 完全不执行任务（4 月 7 日创建，无 fix PR）。
  - **#91009**【崩溃/性能】：Codex Hook 进程 CPU 100% + Gateway RPC 停滞（无 fix PR）。
  - **#96834**【阻塞】：WhatsApp 图片消息阻塞通道 3 分钟（无 fix PR，需 live repro）。
  - **#78493**【权限崩溃】：`sudo openclaw update` 导致文件 root/用户混属，后续 `doctor` 在 EACCES 后覆写配置。
  - **#97616**【资源泄漏】：Hook/工具子进程未被回收，僵尸进程累积导致运行时性能下降。
  - **#38327**【回归】：Google Vertex/Gemini-3.1 下报 “Cannot convert undefined or null to object”（3 月 6 日创建，无 fix PR）。

- **P1 级（偶发/特定场景）**：
  - **#50093**：WhatsApp 断线重连后，离线期间消息静默丢失。
  - **#53408**：长对话（15+ 轮）后，`write`/`exec` 工具参数被静默丢弃。
  - **#86215**：Codex OAuth 刷新失败，Agent 无告警卡死数小时且不触发配置轮换。
  - **#51429**【安全/合规】：代码中硬编码开发者本地路径 `/Users/wangtao` 被合入并发布，导致新用户工作区被重置。

- **已有修复 PR 但未合并**：部分 Issue 已存在 `linked-pr-open` 标签，包括 #39476（A2A sessions_send 重复消息）、#77930（Discord Channel 在 2026.5.4 回归）、#62328（node:sqlite 缺 FTS5）、#112196（memory_search 同步超时被误报为永久故障），需维护者推进 review 与合并。

## 6. 功能请求与路线图信号

- **多租户/实例级配置分化**：今日最强路线图信号。多个 Issue（#71058 多 Teams 机器人、#66252 按 Agent 配置 TTS/STT、#67413 按 Agent 控制 Dreaming 内存整理、#50199 技能优先级配置）均指向同一诉求：允许单 Gateway 运行多个身份、行为差异化的 Agent。**对应 PR #112811 已就绪**（支持单 Gateway 多 Teams 机器人），或随下个版本合并。
- **上下文管理与长会话体验**：#67419 指出 Bootstrap 文件（MEMORY.md、SOUL.md 等）每轮重复注入浪费 20-30% Token；#68596 请求可配置 Watchdog 阈值。这些表明社区开始在长会话/高成本模型下关注 TCO 与稳定性。
- **UI/UX 打磨**：Control UI 的 LaTeX 渲染（#42840，👍 10）、Dashboard 增加持久任务状态面板（#52640）以及 UI 无障碍/可读性重设计（#75947）获得较高呼声。PR #125280（非 Git 目录不显示 Worktree 选项）和 #125438（Dashboard 增加 Live Progress Card 渲染）已在处理相关交互细节。
- **第三方集成增强**：#63930（支持 Anthropic advisor 服务端工具）、#71195（macOS Talk 模式接入 OpenAI Realtime 语音）反映了将 OpenClaw 作为**多模态/语音中控**的技术趋势。

## 7. 用户反馈摘要

- **核心痛点：更新即回归**。多位用户提交了“之前正常，升级后瘫痪”的报告（#62505、#38327、#77930），反映出稳定版本的质量把控亟待加强。
- **对供应链审查的质疑**：Issue #51429 发现并曝光了硬编码的个人本地路径被合并发布，用户戏谑称“这位 wangtao 是谁？”，该问题严重影响了项目声誉。
- **对“静默失败”的深度不满**：工具参数丢失（#53408）、模型切换无响应（#58957）、MEDIA: token 因 Markdown 代码块被吞（PR #80396）等都因为没有清晰报错而将用户困在“黑盒”中，极度消耗信任。
- **真实价值被认可，但需生产级标签**：#73537 用户反馈称，“感谢你构建了 OpenClaw……它已成为我们家庭和业务日常流程的一部分（Telegram、Home Assistant、自动化）”，但强烈要求提供“生产就绪”稳定性标签。这说明了用户留存率高，但对版本稳定性极度敏感。

## 8. 待处理积压

以下 P0/P1 问题长期未闭环或存在大量讨论但无结论，建议维护者优先调度：

- **#70903**（P0，4月24日）：Provider cooldown 账单恢复后仍锁死用户，已滞留近 4 个月。
- **#62505**（P1，4月7日）：编码 Agent 核心功能回归，4 个月无 fix PR。
- **#50093**（P1，3月19日）：WhatsApp 断线重连丢消息，涉及核心数据可靠性。
- **#51429**（P1，3月21日）：硬编码开发者路径被合入，需紧急修复与流程整改。
- **待审 PR（stale）**：#75299（4月30日，优先级命令队列饥饿保护）与 #72314（4月26日，无 ID 入站重试去重）均已标记 stale 且超过 3 个月无人跟进，建议维护者 review 后决定合并或关闭，避免无效积压。

---

## 横向生态对比

# 个人 AI 助手 / 自主智能体开源生态横向对比报告（2026-08-18）

## 1. 生态全景

当前个人 AI 助手与自主智能体开源生态正处于 **高活跃、高强度迭代** 阶段：头部项目单日可产生数百条 Issue/PR 动态，社区规模与应用场景持续扩张。但与此同时，**稳定性与可靠性已成为全局性痛点**——消息丢失、静默失败、工具调用空转等问题在多个项目中反复出现，表明“能跑通”已不再是用户关注点，“可观测、可诊断、可自愈”成为新基线。技术方向上，多智能体互操作、成本治理、OpenAI 生态兼容、可插拔架构、安全加固等需求正密集涌现，推动框架从单体工具向模块化基础设施演进。生态内部也出现明显分层：少数头部项目占据绝大多数社区流量，大量中小项目在垂直场景或架构创新上寻求差异化。

## 2. 各项目活跃度对比

| 项目 | Issue 更新数 | PR 更新数 | Release | 健康度 / 阶段 |
|---|---:|---:|---|---|
| OpenClaw | 500 | 500 | 无 | 超大规模活跃，高积压，维护瓶颈明显 |
| Hermes Agent | 50 | 50 | v0.20.3 | 高活跃，桌面端与稳定性修复推进中 |
| ZeroClaw | 50 | 50 | 无 | 高活跃，安全加固密集，PR 积压 |
| IronClaw | 28 | 44 | 1.3.0-rc.1 | 高密度迭代，存储性能优化为主线 |
| CoPaw | 14 | 35 | 无 | 高活跃，v2.1.0 密集修复期 |
| NanoClaw | 4 | 39 | 无 | 架构升级期，seam 模块化改革推进 |
| LobsterAI | 7 | 21 | 无 | 中上活跃，清理积压 PR，历史 bug 待解 |
| NanoBot | 3 | 15 | 无 | 快速迭代，但高价值 bug 悬置过久 |
| Moltis | 2 | 9 | 无 | 中等活跃，健康稳定，心跳模块修复中 |
| PicoClaw | 4 | 4 | 无 | 稳定收敛，问题修复闭环快 |
| NullClaw | 0 | 1 | 无 | 低活跃，基本停滞 |
| ZeptoClaw | 0 | 0 | 无 | 无活动 |

## 3. OpenClaw 在生态中的定位

- **社区规模断层领先**：OpenClaw 单日 Issue/PR 更新量达 500 条，是第二名（Hermes Agent、ZeroClaw 各 50 条）的 10 倍。用户反馈量级独一无二，但也暴露出治理压力——今日 Issue 关闭仅 12 条，PR 合并/关闭 98 条，相对 500 条新增而言明显失衡。
- **技术路线：多平台个人助手中枢**。OpenClaw 以 GateWay + 渠道适配器（WhatsApp、Telegram、Discord 等）+ Agent 执行为核心，覆盖广度最大。与 Hermes 的桌面优先、NanoClaw 的 seam 可插拔内核、IronClaw 的生产性能优化相比，OpenClaw 更依赖社区驱动和生态插件，整体走“全功能覆盖”路线。
- **差异化优势与风险并存**：优势是生态位核心、集成多、文档和用户基础厚；风险是历史包袱导致回归频繁（如编码 Agent 瘫痪 #62505、Google Vertex 报错 #38327），P0/P1 问题长期滞留。OpenClaw 的“广覆盖 + 高积压”已成为其需要直面的结构性挑战。

## 4. 共同关注的技术方向

| 方向 | 涉及项目 | 具体诉求 |
|---|---|---|
| **稳定性与静默失败治理** | OpenClaw、NanoBot、PicoClaw、Hermes Agent、CoPaw | 工具调用无限循环、消息丢失、cron 静默失败、渲染卡死等；需要可观测性、显式报错、自愈机制 |
| **多智能体互联与身份隔离** | OpenClaw、Hermes Agent、NanoBot、LobsterAI、CoPaw | 跨网关 bot 通信（`hermes peer`）、会话 @提及、VOKO 跨框架集成；要求 agent 可寻址、路由清晰、上下文隔离 |
| **LLM 成本治理与预算防护** | OpenClaw、NanoBot、ZeroClaw、CoPaw | 预算防火墙、action 配额原子化扣减、上下文 token 计量修复；防止循环或并发导致成本失控 |
| **OpenAI 兼容与生态接入** | ZeroClaw、LobsterAI、OpenClaw、Moltis | 兼容 Chat Completions / OpenAI SDK，接入 Open WebUI、Aider、LangChain 等既有工具链 |
| **可插拔模块化架构** | NanoClaw、Hermes Agent、IronClaw、CoPaw | seam 接缝、外部技能仓库、WASM host functions、provider 统一重构；追求“注册制扩展”而非改源码 |
| **安全与密钥管理** | OpenClaw、Hermes Agent、ZeroClaw、LobsterAI、CoPaw | Gateway Token 泄露、日志脱敏、凭据环境变量清理、附件下载边界、URL 过期污染 |

## 5. 差异化定位分析

- **OpenClaw / PicoClaw / NanoBot**：面向个人用户的轻量全渠道助手。OpenClaw 功能最全但维护压力大；PicoClaw 修复速度快、注重渠道细节（Weixin、IRC）；NanoBot 专注 Gateway 稳定性与 WebUI 多会话体验。
- **Hermes Agent**：桌面级个人 AI 工作台，强调 Profile 生命周期、Bot Mode 群聊、跨网关通信，适合重度桌面/多设备用户。
- **NanoClaw / ZeroClaw**：架构驱动型项目。NanoClaw 以 seam 模式重构可插拔内核，追求极强扩展性；ZeroClaw 以 RFC 治理为特色，注重安全、OpenAI 兼容和长期架构演进。
- **IronClaw**：面向生产/自托管场景的高性能助手，核心在持久层写入优化（目标降低 60% DB 压力）、资源治理、自动化调度，工程化程度高。
- **LobsterAI / CoPaw**：中文生态与多智能体协作方向。LobsterAI 具网易有道背景，专注桌面 Agent 运行时与 Agent 编排；CoPaw 与 Qwen 生态关联，强化多渠道模型配置与 DataPaw 数据应用。
- **Moltis**：Rust 生态小而美项目，聚焦心跳调度、外部代理接入、WebUI 可配置性，适合有定制调度需求的开发者。
- **NullClaw / ZeptoClaw**：活跃度过低，缺乏明确差异化，暂无可参考价值。

## 6. 社区热度与成熟度分层

**超大规模（日更新 >100）**  
- **OpenClaw**：生态核心，社区基数和问题量最大；但维护吞吐不足，正处于“高活跃 + 高积压”的临界状态。

**高活跃（日更新 30–100）**  
- **Hermes Agent、ZeroClaw、IronClaw、CoPaw、NanoClaw**：均处快速迭代阶段，有明确路线图或架构主线。其中 Hermes Agent、IronClaw 已发布版本/RC，质量巩固在进行；ZeroClaw、CoPaw、NanoClaw 则更偏架构和功能扩张。

**中活跃（日更新 5–30）**  
- **NanoBot、PicoClaw、Moltis、LobsterAI**：属于质量巩固/清理期。NanoBot、PicoClaw 正在修复关键稳定性问题，Moltis 补全心跳机制缺陷，LobsterAI 清理 stale PR 和长期 bug。

**低活跃（日更新 <5）**  
- **NullClaw、ZeptoClaw**：实质停滞，仅剩自动依赖 PR 或零动态。

## 7. 值得关注的趋势信号

1. **“可靠性”是智能体进入生产环境的入场券**。多个“静默失败”案例（NanoBot Telegram 轮询停滞、PicoClaw 工具循环、Hermes cron 静默失败）表明用户对黑盒行为容忍度极低。开发者应将可观测性（运行轨迹、审计日志）和显式错误处理作为一等公民。

2. **跨 Agent 通信从概念走向实用**。Hermes 的 `hermes peer`、NanoBot 的 session @提及、LobsterAI 收到 VOKO 集成提案，均指向 A2A（Agent-to-Agent）协议和互操作网络。未来个人 AI 助手不会是孤岛，身份边界和消息路由需提前设计。

3. **LLM 成本治理将内建到框架层**。NanoBot 提出“混合消费防火墙”、ZeroClaw 实现 action 预算原子化、CoPaw 修复上下文用量虚高，说明成本控制正从应用层下沉到框架机制。开发者应关注预算上限、循环检测、配额精确计量。

4. **OpenAI 兼容协议成为生态连接器**。ZeroClaw 的 Chat Completions 兼容 RFC 获得高热度，LobsterAI、Moltis 也在扩展 OpenAI/Anthropic 兼容网关。新框架应优先兼容 OpenAI SDK，而非自建协议，以快速接入现有 AI 工具链。

5. **模块化、可插拔是应对生态碎片化的答案**。NanoClaw 的 seam 架构、Hermes 的 git-backed 共享技能、CoPaw 的 provider 统一重构，都在努力让核心瘦身、功能以注册机制扩展。这样可以降低大型项目的维护熵增。

6. **安全与供应链审查前置化**。OpenClaw 的 Gateway Token 泄露、Hermes 的凭据残留、ZeroClaw 的 Gemini Key 进 URL、LobsterAI 日志裸奔，表明智能体由于涉及用户数据和外部系统，安全设计必须从起步阶段引入，而非上线后补救。

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报

**日期：** 2026-08-18  
**数据范围：** 过去24小时（2026-08-17 至 2026-08-18）  
**数据来源：** [HKUDS/nanobot](https://github.com/HKUDS/nanobot)  
**分析角色：** AI 智能体与个人 AI 助手领域开源项目分析师


## 1. 今日速览

NanoBot 项目在过去24小时内展现出**极高活跃度**：共产生 3 条 Issue 更新（2 条活跃、1 条已关闭）和 15 条 PR 更新（10 条待合并、5 条已合并/关闭），开发节奏明显加快，且大量 PR 集中在 gateway 稳定性、WebUI 体验增强和 Telegram/Slack 渠道修复等方向，显示项目正处于密集迭代期。值得关注的是，**多条 PR 之间存在功能联动**（如 Telegram 停滞修复从 #5301 到 #5156 的递进），说明维护者正在系统性地解决已知稳定性和平台兼容性问题。然而，**#4864 涉及的 complete_goal 无限循环 bug 已持续超过一个月仍未修复**，是当前影响用户使用体验的最大隐患。综合来看，项目整体蓬勃发展、PR 内容丰富，但在 bug 修复的时效性和积压问题上仍需加强。


## 2. 版本发布

**过去24小时无新版本发布。** 最近一条 Release 早于本次报告窗口，暂无版本更新内容可展示。


## 3. 项目进展

过去24小时共有 5 条 PR 被合并/关闭，主要集中在 gateway 基础能力加固、Telegram 稳定性修复、CLI 界面升级与 agent 行为修正四个方向：

- **[PR #5416] fix(gateway): stabilize process identities**（已合并）——替换 macOS 上依赖 locale 的 `ps lstart` 进程身份识别方案，改用原生 `proc_pidinfo` 时间戳，统一了 gateway client lease 的进程身份合约，并保留 Windows FILETIME 和 Linux 兼容逻辑。这是对 gateway 多平台可靠性的一次基础性加固。
- **[PR #5156] fix(telegram): recover from silently stalled polling**（已关闭，修复 #5171）——Telegram 轮询在遭遇瞬态网络故障后永久静默停摆的生产级问题，此 PR 修复了该问题并已关闭对应 issue。搭配此前合并的 **[PR #5301]（bridge stdlib logging + lightweight liveness check）**，Telegram 渠道的可观测性和自愈能力得到完整闭环。
- **[PR #5406] feat(cli): add native TypeScript terminal UI**（已合并）——为 CLI 引入原生 TypeScript 终端 UI，接替此前的 #4329（该 PR 曾因误标记合并被回滚，此 PR 携带完整提交历史及跨终端修复重新合入）。
- **[PR #5410] fix(goal): stop repeating clarification replies**（已合并）——修复了在持续目标（sustained goal）激活时，AgentRunner 将每个纯文本回复误判为需要继续注入澄清循环的问题，使行为与工具调用预算边界对齐。
- **[PR #5406] 与 [PR #5416] 共同** 提升了项目在多平台（macOS / Windows / Linux）上的表现一致性与进程管理健壮性。

**整体判断：** 项目在基础设施稳定性（gateway、Telegram）、开发者体验（CLI TUI）和 agent 核心行为（goal 处理）三个重要维度上均取得实质性进展，尤其是 Telegram 停滞问题的闭环修复直接消除了一个影响生产环境的关键隐患。


## 4. 社区热点

- **[Issue #4864] [OPEN] complete_goal 无限循环 bug**（评论 7，👍 1）——这是当日讨论热度最高的 issue，集中反映了用户对 gateway 工具参数序列化变更后兼容性问题的困扰。用户反馈 `complete_goal` 因 `recap` 参数被当作裸字符串解析而持续报错，陷入死循环。此 issue 已存活超一个月仍处于开放状态，评论区聚集了多个受影响用户的反馈。
- **[Issue #5409] [OPEN] Hybrid Spend Firewall（混合消费防火墙）**（👍 0，评论 0）——虽然数据量不大，但该 issue 提出了一个颇具前瞻性的需求：为商业化场景下的 LLM 成本失控问题提供防护机制。作者从外部视角明确指出“power users running infinite loops and bankrupting your LLM budget”这一规模化痛点，表明有一批用户在真实使用中感受到了成本敞口。

**社区诉求分析：** 当前热点集中在 **工具调用稳定性**（#4864）与 **成本治理**（#5409）两大方向，前者反映近期 gateway 变更对上层工具调用的影响尚未完全收敛，后者则暗示项目商业化进程加速后用户对安全边界的刚性需求。


## 5. Bug 与稳定性

按严重程度排列：

| 严重程度 | 事项 | 状态 | 说明 |
|---------|------|------|------|
| 🔴 高 | [#4864] complete_goal 工具调用无限循环 | 开放，无对应 fix PR | 最近更新于 08-17，已存活超一个月。gateway 解析 `recap` 参数类型错误导致持续报错，直接影响依赖 goal 功能的用户核心流程 |
| 🔴 高 | [#5171] Telegram 轮询静默停滞且永不恢复 | 已关闭（08-17） | 瞬态网络故障后 bot 永久停止收消息，进程存活但日志静默；已由 PR #5156 修复并合入，**已解决** |
| 🟠 中 | [PR #5407] cron 持久化任务无法通过配置禁用 | 待合并（open） | 将 `gateway.heartbeat.enabled=false` 后重启，持久化的系统 job 仍继续执行并消耗 token——配置与运行时状态不一致 |
| 🟠 中 | [PR #5412] gateway 后台子进程输出被块缓冲 | 待合并（open） | Python 向文件非 TTY 输出时 block-buffer，启动日志可能无限期滞留内存，影响故障排查 |
| 🟠 中 | [PR #5413] LLM provider 抛出异常时绕过 fallback 策略 | 待合并（open） | 仅处理了 `LLMResponse(finish_reason="error")`，未捕获裸异常，导致 fallback 链路失效 |
| 🟠 中 | [PR #5415] Windows 上 venv 子进程 PID 不一致 | 待合并（open） | Windows 虚拟环境下 gateway 无法正确采用 venv launcher 的直接 PID，影响进程生命周期管理 |
| 🟢 低 | [PR #5341] Windows PowerShell 中 `curl` 别名问题 | 待合并（open） | 天气 skill 中裸 `curl` 可能被解析为 `Invoke-WebRequest` 别名，导致首次调用失败 |
| 🟢 低 | [PR #5414] Slack 文件下载未验证重定向链 | 待合并（open） | 私有下载 URL 可能被重定向至非预期目标，缺少共享 URL 安全防护 |

**趋势判断：** 当前 bug 修复呈现“集中爆发、分批合入”的特征——Telegram 停滞问题已闭环，gateway 的多个稳定性修复（#5412、#5413、#5415、#5416）正密集推进中，预计未来 1-2 个版本窗口内 gateway 可靠性将显著提升。


## 6. 功能请求与路线图信号

- **[Issue #5409] Hybrid Spend Firewall** —— 用户提出“混合消费防火墙”概念，建议在框架层内置预算上限、循环检测与熔断机制，防止 LLM 预算因失控循环而耗尽。虽然目前无对应 PR，但该需求直指商业化场景的核心痛点，**有较大概率被纳入长期路线图**。结合当前 #4864 等循环类 bug 的存在，这类防护机制具有现实价值。
- **[PR #5408] WebUI 跟进建议（follow-up suggestions）** —— 在每次成功对话后生成临时性 chat-scoped 建议，采用 provider 中立协议，与 DeerFlow 交互模式对齐。属于典型的体验增强功能，**有可能随下一次 WebUI 迭代合入**。
- **[PR #5364] WebUI 临时侧边对话（/side）** —— 支持在现有主题旁开启多个临时对话，独立草稿/消息/流式状态，且主/侧 composer 可并行发送。表明项目正在强化多会话并行管理的能力，**标志着 WebUI 从单线程对话向多线程工作台演化**。
- **[PR #5358] WebUI 会话 @提及消息** —— 为每个持久化 WebUI session 分配服务器端稳定的 `@name`，支持会话间互相寻址和消息投递。这意味着项目在向“多 agent 协作”方向探索——会话不再孤立，而是可以相互通信，**是一个具有战略意义的架构信号**。
- **[PR #5406] 原生 TypeScript 终端 UI** —— 已合入，CLI 交互体验完成了一次代际升级。

**路线图推断：** 短期聚焦 WebUI 交互增强（#5408、#5364、#5358），中期将着力于 gateway 稳定性补全（#5412、#5413、#5415），长期可能引入成本治理与多会话协作机制（#5409、#5358）。整体呈现出“体验优化→稳定性加固→商业化安全边界”的演进路径。


## 7. 用户反馈摘要

- **对 gateway 工具参数序列化变更的抱怨（来自 #4864 评论）**：用户报告 `complete_goal` 的 `recap` 参数被解析为裸字符串而非 JSON 对象，导致持续报错。虽然该 issue 的 7 条评论未展示具体内容，但从问题描述可推断：这次回归影响了持续目标功能的正常使用，用户不得不手动规避。此类评论通常包含“recent update broke my workflow”的挫败感情绪，说明 gateway 的序列化变更缺乏对既有工具调用的向后兼容验证。
- **生产环境稳定性关切（来自 #5171 描述）**：作者在报告中提到“Messages pile up server-side”——Telegram 轮询停滞后，消息在服务端积压，而进程却看似正常。这是一种非常隐蔽且危险的生产故障模式，用户的措辞“process keeps running and the log stays completely silent”表现出对这种“无声故障”的高度担忧。此类问题的修复（#5156 已合入）会显著提升用户对项目在真实环境中的信任度。
- **对 LLM 预算失控的焦虑（来自 #5409 描述）**：作者以“Love the work”开头，正面认可项目价值，同时直接点出商业化过程中可能遭遇的“power users running infinite loops and bankrupting your LLM budget”风险。这反映出当项目从开源走向商业化时，用户群体对成本可控性的敏感度显著提升，且期待框架内置安全机制而非依赖外部编排。


## 8. 待处理积压

| 事项 | 类型 | 创建时间 | 最近更新 | 关注度 | 备注 |
|------|------|----------|----------|--------|------|
| [#4864] complete_goal 无限循环 | 高价值 bug | 2026-07-09 | 2026-08-17（仍活跃） | 评论 7、👍 1 | 已开放超过 40 天，是当前最严重的未解决 bug，直接影响核心功能，社区已有多名用户受影响。**建议维护者优先排期**，并考虑在修复前的临时 workaround 方案。 |
| [#5301] Telegram logging + stalled polling 检测 | 低风险修复 | 2026-08-09 | 2026-08-17（已合并） | — | 已合入，无需继续跟踪（此处仅为记录）。 |
| [#5341] Windows curl 别名修复 | 低风险修复 | 2026-08-11 | 2026-08-17 | — | 待合并 7 天，虽然优先级为 p2，但修复逻辑简单、风险低，长期滞留会持续影响 Windows 用户体验。 |
| [#5358] WebUI session @提及消息 | 功能 PR | 2026-08-12 | 2026-08-17 | — | 待合并 6 天，该功能具有较强的架构前瞻性，长期搁置可能导致后续 WebUI 多会话功能的重复冲突。 |
| [#5364] WebUI 临时侧边对话 | 功能 PR | 2026-08-13 | 2026-08-17 | — | 待合并 5 天，与 #5358 紧密相关，建议一并审阅决策。 |

**维护者关注建议：** 最优先处理 #4864（严重影响用户核心流程且悬置过久）；其次尽快合并/回复 #5341、#5358、#5364 三条等待超 5 天的 PR，避免功能分支与主线 drift；#5407、#5412、#5413、#5415 等稳定性修复 PR 建议在下个版本窗口内集中审阅合入。


## 附：项目健康度评估摘要

| 维度 | 评分 | 说明 |
|------|------|------|
| 活跃度 | ★★★★★ | 24h 内 15 条 PR 更新 + 3 条 Issue 更新，开发非常活跃 |
| 响应速度 | ★★★☆☆ | 新 issue 处理较快，但 #4864 悬置超一月未修复，影响口碑 |
| 代码质量与审查 | ★★★★☆ | 大量 PR 附带完整 Summary、根因分析和回归测试，质量意识强 |
| 用户沟通 | ★★★★☆ | #5156 与 #5171 的正向闭环、#5301 的拆分策略显示维护者善于管理用户预期 |
| 路线图清晰度 | ★★★★☆ | WebUI 增强 + gateway 修复 + 成本治理的方向逐步显现 |

**一句话总结：** 项目正处于高活跃、高产出、强功能推进期，WebUI 多会话与 gateway 稳定性是当前两条主线，唯一红灯是 #4864 迟迟未修复，建议尽快投入资源解决。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目动态日报 — 2026-08-18

## 1. 今日速览

过去 24 小时项目保持高强度迭代：**50 条 Issue 更新（38 条活跃/新开，12 条关闭）**，**50 条 PR 更新（38 条待合并，12 条已合并/关闭）**，并发布了 **v0.20.3（v2026.8.16.2）** 补丁版本，打包约 125 个 PR。今日主线集中在桌面端体验修复（profile 生命周期、Bot Mode 群聊、设置页统一）、跨网关通信新能力（`hermes peer`）、以及安全加固（凭据环境变量清理）。仓库级大型重构史诗 #78647（god-file 分解）宣布 20/20 全部完成，是架构治理的重要里程碑。整体活跃度极高，但若干 P1/P2 级稳定性问题（文件描述符泄漏、cron 静默失败）已悬置多时，值得关注。

## 2. 版本发布

**v2026.8.16.2 / Hermes Agent v0.20.3**（[Release 链接](https://github.com/NousResearch/hermes-agent/releases)）

- **类型**：Patch release
- **内容**：自 v0.20.2 以来合并的约 125 个 PR 统一收口为稳定标签，面向下游 Docker 镜像、托管部署与全新安装分发。
- **破坏性变更**：未明确说明，属于常规 patch 升级。
- **迁移注意事项**：无特别提示；建议下游消费者基于该 tag 重建镜像，以纳入近期全部修复。

## 3. 项目进展

今日合入/关闭的 PR 体现了两个方向：**桌面端生命周期治理** 与 **跨网关/跨安装互联**。

**桌面端修复（teknium1 系列）**

- [#88732 fix(desktop): re-home backend and retire sockets on profile rename](https://github.com/NousResearch/hermes-agent/pull/88732) — 关闭。修复 profile 重命名后旧后端继续运行、导致"幽灵 profile"复现的问题；替身自 [#51852](https://github.com/NousResearch/hermes-agent/pull/51852)。
- [#88739 fix(desktop): await pooled backend teardown to stop orphaned profile backends](https://github.com/NousResearch/hermes-agent/pull/88739) — 开放。解决清理流程中 SIGTERM 发出后立即退出、导致后端进程变成孤儿的问题；实测曾积累 41 个游离后端。
- [#88721 fix(hermes-bots): group chats render Hermes, not @default](https://github.com/NousResearch/hermes-agent/pull/88721) — 关闭。群聊中主 agent 显示为 "Hermes" 而非 "@default"，点击说话者可查看完整身份。
- [#88687 fix(desktop): make Bot Chat visibility toggle effective](https://github.com/NousResearch/hermes-agent/pull/88687) — 关闭。修复 Bot Chat 会话"隐藏"开关失效的问题。

**新功能**

- [#88738 feat(desktop): Discord-style group-chat rows in Bot Mode](https://github.com/NousResearch/hermes-agent/pull/88738) — 开放。群聊以扁平列表行展示，并在主聊天窗口打开，不再局限于侧栏。
- [#88725 feat(cli): hermes peer — bot-to-bot DMs across machines and gateways](https://github.com/NousResearch/hermes-agent/pull/88725) — 关闭。支持跨网关的 bot-to-bot 私信，无需桌面端参与。
- [#88719 feat(skills): add skills.external_repo for git-backed shared skills](https://github.com/NousResearch/hermes-agent/pull/88719) — 开放。通过 git 仓库在多台设备间同步共享技能。
- [#85774 feat(providers): add Inworld model provider](https://github.com/NousResearch/hermes-agent/pull/85774) — 开放。新增 Inworld 模型提供商。
- [#88212 feat(providers): add native Cursor provider via CURSOR_API_KEY](https://github.com/NousResearch/hermes-agent/pull/88212) — 开放。支持 Cursor 仪表盘 API key 直连，跳过 CLIProxy。

**架构与治理**

- [#88734 fix(gateway): resolve the session DB inside the active profile scope](https://github.com/NousResearch/hermes-agent/pull/88734) — 开放。多路复用网关上每个 profile 的会话物理写入各自的 `state.db`，替身自 #88632。
- [#78789 / #80059 / #80065](https://github.com/NousResearch/hermes-agent/issues/78789) — 关闭。Telegram 菜单按钮、WhatsApp 群消息/贴纸等请求被折叠进更大的 Feature Package 规划，说明项目正在收敛重复 Issue、推行统一功能包机制。
- [#78647 Large-file decomposition: 20/20 done](https://github.com/NousResearch/hermes-agent/issues/78647) — 关闭。仓库级 god-file 分片史诗全部完成，作为长期重构治理的里程碑。

**项目整体推进判断**：桌面端生命周期管理迎来一轮集中修复；跨网关 peer 能力与多个新 provider 扩展了连接性；session/profile 数据隔离正在改善。当前主线与 v0.21 功能面高度吻合。

## 4. 社区热点

- [#78647 [COMPLETE] Large-file decomposition: 20/20 done](https://github.com/NousResearch/hermes-agent/issues/78647) — **76 条评论**，今日讨论热度最高。尽管已关闭，社区仍在关注 god-file 分片的完整方法论。背后的诉求是希望项目长期保持"小文件、清晰模块、可维护性优先"的工程纪律。
- [#84834 Webhook Feature Package — graph-gated repair (meta-issue)](https://github.com/NousResearch/hermes-agent/issues/84834) — **17 条评论**。作为 webhook 全表面（ingress、execution、delivery、配置、管理 UI、部署、文档）的 5×2×3 修复计划跟踪器，讨论热度高说明社区对 webhook 链路稳定性诉求强烈。
- [#53902 Renderer stuck in fontations+temporal_rs loop — GPU 98%, 13W sustained](https://github.com/NousResearch/hermes-agent/issues/53902) — **7 条评论**。性能类问题，持续近两月未解决，用户对桌面端高功耗非常敏感。
- [#79742 hermes_state: SessionDB leaks per-thread WAL read connections](https://github.com/NousResearch/hermes-agent/issues/79742) — **4 条评论 + 1 👍**。P1 严重性问题但关注度不高，存在被低估风险。

## 5. Bug 与稳定性

**P1 级**

- [#79742 SessionDB 每线程 WAL 读连接泄漏 → fd 耗尽 → EMFILE](https://github.com/NousResearch/hermes-agent/issues/79742) — 8 月 5 日报告，至今无关联 fix PR。长期运行进程会因文件描述符耗尽而崩溃。高风险。
- [#88655 Scheduler 级 cron 处理错误绕过 failure_nudge 告警，作业静默失败 5 小时](https://github.com/NousResearch/hermes-agent/issues/88655) — 8 月 17 日报告，尚无修复。cron 任务在调度层抛错时未触发失败通知，需要补充告警链路。

**P2 级**

- [#87654 Vision tools 首次可用性探测后从会话中消失](https://github.com/NousResearch/hermes-agent/issues/87654) — `_AuxProbeClientStub` 被错误缓存导致。已有明确根因，尚无 fix PR。
- [#88713 `/save` 会话导出崩溃：`'GatewayRunner' object has no attribute 'get_adapter'`](https://github.com/NousResearch/hermes-agent/issues/88713) — 高频用户路径，已标记 duplicate。
- [#53666 clarify 工具提示不在聊天 UI 渲染，用户看不到问题、回复为空](https://github.com/NousResearch/hermes-agent/issues/53666) — 自 6 月 27 日开放至今。
- [#72716 optimize-storage 在中断的 demote 后可写入空 FTS，永久丢失搜索索引](https://github.com/NousResearch/hermes-agent/issues/72716) — 数据丢失类问题，无 fix PR。
- [#37751 Desktop 与 Gateway 配置双写冲突导致配置矛盾状态](https://github.com/NousResearch/hermes-agent/issues/37751) — 影响模型切换，长期开放。
- [#87823 / #86601 Desktop "Read Aloud Replies" 每条消息 TTS 重复合成并播放两次](https://github.com/NousResearch/hermes-agent/issues/87823) — 两个报告大概率同根因，目前分别开放。

**安全相关**

- [#4775 Hermes 静默重写用户 config.yaml，展开默认值并解析环境变量密钥](https://github.com/NousResearch/hermes-agent/issues/4775) — 8 月 17 日仍有更新，用户关注度高（👍 1）。配置自动改写是敏感行为，建议尽快约束。
- PR [#70370](https://github.com/NousResearch/hermes-agent/pull/70370) / [#70372](https://github.com/NousResearch/hermes-agent/pull/70372) — 从嵌入式终端 PTY 与 `hermes serve` 子进程环境中清除凭据。两项安全修复自 7 月 23 日提交后至今仍未合并，建议优先 review。

## 6. 功能请求与路线图信号

- **跨网关 bot 通信**（#88725 已作为 PR 合入）与 **群聊桌面 UI**（#88738 已开 PR）：大概率进入 v0.21。
- **git-backed 共享技能**（[#88719](https://github.com/NousResearch/hermes-agent/pull/88719)）：符合多设备场景，方向被社区认可。
- **新模型提供商**：Inworld（#85774）与 Cursor（#88212）均已开 PR，扩展 provider 生态。
- **单一事务化部署计划**（[#88683](https://github.com/NousResearch/hermes-agent/issues/88683)）：安装/更新/引导路径统一为单一部署契约的架构级请求，可能成为未来版本的核心重构项。
- **安全加固战役**（[#88706](https://github.com/NousResearch/hermes-agent/issues/88706)）：要求收敛使用时间、来源与授权边界的十项安全加固，尚未有 PR。
- **MCP 双协议边界正确性**（[#88698](https://github.com/NousResearch/hermes-agent/issues/88698)）：MCP 2.0 SDK 与 1.x peer 兼容，以及新旧 `initialize` / `server/discover` 握手的边界修正。
- **Cron/会话恢复按确切所有权代次的围栏对账**（[#88688](https://github.com/NousResearch/hermes-agent/issues/88688)）：面向数据丢失与静默卡死的架构级修复。

## 7. 用户反馈摘要

- **对大型重构的认可**：#78647（god-file 分解 20/20）获得大量评论，用户明显支持"绝不回退到 god-file"的工程政策，认为这提升了可维护性。
- **桌面端资源占用**：#53902 用户报告 Electron Renderer 卡在 `fontations`+`temporal_rs` 循环、GPU 98% 活跃、功耗 13W（约为正常空载 4 倍），对续航和风扇噪声影响显著。
- **配置被改写的不满**：#4775 用户指出 Hermes 会在执行配置命令时将含默认值与环境变量展开后的完整运行配置写回用户手写文件；#37751（中文用户）进一步报告 Desktop 与 Gateway 双写导致 `provider: dashscope` + `base_url: localhost` 的矛盾状态。这类"配置所有权"问题已引发中文社区关注。
- **UI 混乱**：#88200 BOTS 侧栏预览与点击打开会话不一致；#86601 TTS 自动朗读同一回复播放两遍。都属于"功能可用但体验割裂"的典型反馈。
- **崩溃与数据安全**：#88713 `/save` 命令必现崩溃；#79742 文件描述符泄漏可能导致长期运行后无法启动新连接。稳定性痛点正在积累。

## 8. 待处理积压

以下问题或 PR 长期未得到响应，建议维护者优先关注：

- [#79742](https://github.com/NousResearch/hermes-agent/issues/79742) — **P1** SessionDB 每线程 WAL 连接泄漏（8 月 5 日报告，无 fix PR，风险高）。
- [#72716](https://github.com/NousResearch/hermes-agent/issues/72716) — **P2** `optimize-storage` 可导致 FTS 永久清空（7 月 27 日报告，数据丢失风险）。
- [#53666](https://github.com/NousResearch/hermes-agent/issues/53666) — **P2** clarify 工具提示不在聊天 UI 渲染（6 月 27 日报告，核心交互缺失）。
- [#53902](https://github.com/NousResearch/hermes-agent/issues/53902) — **P2** 渲染进程 GPU 占用/功耗异常（6 月 28 日报告，近两月未解决，7 条评论）。
- [#61828](https://github.com/NousResearch/hermes-agent/issues/61828) — **P2** `install.sh --stage` 协议遮蔽阶段失败（7 月 10 日报告，安装可信度问题）。
- [PR #70370](https://github.com/NousResearch/hermes-agent/pull/70370) / [PR #70372](https://github.com/NousResearch/hermes-agent/pull/70372) — **安全** 桌面终端与 serve 进程凭据环境变量清理（7 月 23 日提交，至今未合并）。
- [#87025](https://github.com/NousResearch/hermes-agent/issues/87025) — **P3 安全** `hermes doctor` 报告 npm 漏洞，已有明确修复方案（nanoid override + vite 升级）待执行。

---

*报告基于 GitHub 公开数据自动生成，数据时间窗口为 2026-08-17 至 2026-08-18。*

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报（2026-08-18）

## 今日速览

过去24小时项目活跃度中等偏上：共产生4条 Issue 更新（3条新开/活跃、1条已关闭）和4条 PR 更新（1条待合并、3条已合并/关闭）。核心亮点是 **#3312 已合入**，修复了"工具反复失败导致用户永远等不到回答"的严重生产问题（对应 Issue #3311 已关闭）；同时，新提交的 **PR #3340** 针对 Slack 媒体上传失败提供了修复，目前待合并。另一方面，两个新 Bug（#3339 Antigravity 429 错误、#3338 Slack 媒体上传）于昨日集中出现，需持续关注修复进度。整体来看，项目修复速度快、社区反馈活跃，健康度良好。

- 当前待合并 PR：1 个（#3340）
- 新报告 Bug：2 个（#3339、#3338）
- 旧 PR 关闭：2 个（#271、#2606）
- 版本发布：无

---

## 版本发布

无新版本发布。

---

## 项目进展

今日合并/关闭了3个 PR，对项目稳定性与配置灵活性有实质性提升：

- **[PR #3312]（已合并）fix(agent): stop turn early on repeated identical tool failure** — 针对 Issue #3311 的修复：当工具在每轮调用中都以相同错误失败时，智能体不再无限重试直到 `max_tool_iterations`，而是提前终止并给用户反馈。这一改进解决了生产环境中用户长时间等待却得不到任何回答的问题，直接提升了 agent 的可靠性。
  https://github.com/sipeed/picoclaw/pull/3312

- **[PR #271]（已关闭）fix: env overrides when config.json is missing and add regression test** — 修复了在无 config.json 环境下（如 Fly 部署仅使用 secrets/env）使用默认模型且因凭据缺失而失败的问题。该 PR 确保 `env.Parse(cfg)` 即使在配置文件缺失时也会执行，并附带回归测试。此修复对云原生部署场景有实际价值。
  https://github.com/sipeed/picoclaw/pull/271

- **[PR #2606]（已关闭）feat: enhance Weixin channel support and configuration** — 增强微信渠道多实例支持与配置管理，包括渠道目录、动态实例处理、非法渠道名校验与错误处理、多实例流程稳定性等。该 PR 曾被标记为 stale，现已完成生命周期。
  https://github.com/sipeed/picoclaw/pull/2606

**小结**：今日项目向"稳定可控"方向前进了一步——agent 失败循环得到根治，配置兼容性与渠道支持也有补强。

---

## 社区热点

- **[Issue #3287]（6条评论）[Feature] Better support long messages in IRC** — 这是过去24小时内评论最多的讨论。用户希望 PicoClaw 能理解 IRCv3 协议下超长消息（超过512字节）的自动分片，并将其视为一条完整信息。该 Issue 创建于7月22日，已被标记为 stale，但仍在持续讨论。
  https://github.com/sipeed/picoclaw/issues/3287

- **[Issue #3311]（2条评论）[BUG] Repeated identical tool failure loops silently to max_tool_iterations** — 虽已关闭，但该 Issue 关联的生产事故引发关注。用户 lucapette 报告在 Telegram 生产环境中，请求 agent 执行 `git` 命令时因连词相同的错误反复重试，用户在长达数分钟内得不到任何回复。对应的修复 PR #3312 今日已合并，形成闭环。
  https://github.com/sipeed/picoclaw/issues/3311

**热点分析**：社区当前最关注的两个方向——(1) 协议兼容性与消息完整性（IRC长消息）；(2) agent 的失败恢复与超时策略（避免"静默卡死"）。前者是功能需求，后者是稳定性刚需。

---

## Bug 与稳定性

按严重程度排列：

1. **[Issue #3339]（高）[BUG] Antigravity generation returns generic 429 despite valid OAuth scopes and successful model discovery** — 新建。Google Antigravity 的认证与模型发现均正常，但每次生成请求都返回 `429 RESOURCE_EXHAUSTED`，且响应中没有配额错误详情。怀疑是 API 端配置或参数传递问题。**暂无 fix PR**。
   https://github.com/sipeed/picoclaw/issues/3339

2. **[Issue #3338]（高）[BUG] Slack does not attach image media content** — 新建。Slack 媒体上传总是失败，报错 `file.upload.v2: file size cannot be 0`。根因定位清晰：`SendMedia` 构建 `slack.UploadFileParameters` 时未设置 `FileSize`，导致 slack-go SDK 在发起网络请求前即拒绝。**已有修复 PR #3340 待合并**。
   https://github.com/sipeed/picoclaw/issues/3338

3. **[Issue #3311]（中，已解决）Repeated identical tool failure loops silently** — 今日已通过 PR #3312 修复并关闭，生产环境问题告一段落。
   https://github.com/sipeed/picoclaw/issues/3311

**稳定性评估**：核心 agent 循环问题已修复，但新增两个与外部平台集成的 Bug（Antigravity/Slack），提示渠道适配层仍需加强测试。

---

## 功能请求与路线图信号

- **[Issue #3287] IRC 长消息支持** — 用户希望将 IRCv3 自动分片的长消息合并为单一语义单元。这是对渠道输入能力的增强，涉及消息预处理层。目前暂无对应 PR，但社区讨论活跃（6条评论），可能被纳入后续版本。
  https://github.com/sipeed/picoclaw/issues/3287

- **[PR #2606] Weixin 渠道多实例与配置管理增强** — 已完成的增强功能，暗示项目正在扩展中国市场渠道的运维能力，包括动态实例、错误校验与文档完善。虽然 PR 关闭时间较早，但其内容反映微信渠道在企业场景中的实际需求。
  https://github.com/sipeed/picoclaw/pull/2606

**路线图信号**：未来版本的重点可能继续围绕"多渠道输入规范化"（IRC 长消息）与"渠道配置弹性"（多云/多实例）展开。Antigravity 429 问题若为平台侧限制，也可能推动项目增加配额管理与退避重试机制。

---

## 用户反馈摘要

- **生产环境痛点**（来自 #3311 作者 lucapette）：在 Telegram 生产环境中，一个简单的 `git` 命令请求因工具失败循环而"石沉大海"，用户明确表示"从未收到回复"。这表明 **agent 在异常路径下的可观测性与反馈闭环** 是真实痛点。修复合入后，期待社区验证效果。
  https://github.com/sipeed/picoclaw/issues/3311

- **集成层技术反馈**（来自 #3338 作者 octavioturra）：该用户同时提交了 Issue 与修复 PR，指出 `SendMedia` 未设置 `FileSize` 是导致 Slack 上传失败的根因，并提供了详尽的 SDK 行为分析。这表明部分用户具备较强的源码级排查能力，对项目内部实现有深入理解；同时也提示项目渠道适配层参数完整性需要系统性检查。
  https://github.com/sipeed/picoclaw/issues/3338

- **协议理解诉求**（来自 #3287 作者 superuser-does）：用户期望 PicoClaw 遵循 IRCv3 协议惯例处理长消息。这属于进阶协议支持需求，反映部分用户对聊天客户端"拟人化"体验的期待。

---

## 待处理积压

以下 Issue/PR 长期未获得有效响应或被标记为 stale，建议维护者评估处理优先级：

- **[Issue #3287]（stale，创建于7月22日，6条评论）IRC 长消息支持** — 需求明确但已超30天无实质进展，建议明确是否纳入路线图或关闭。
  https://github.com/sipeed/picoclaw/issues/3287

- **[PR #2606]（stale 且已关闭）** — 虽已关闭，但在关闭前处于 stale 状态较长时间，可能反映微信渠道增强的验收/合并流程存在拖延。后续类似渠道类 PR 可注意时效性。
  https://github.com/sipeed/picoclaw/pull/2606

- **[Issue #3338] / [PR #3340]（新积压风险）** — Slack 媒体上传问题已有修复 PR，但截至目前仍未合并。若长期挂起，可能影响相关用户的使用体验，建议优先 review。
  https://github.com/sipeed/picoclaw/issues/3338
  https://github.com/sipeed/picoclaw/pull/3340

- **[Issue #3339]（新，尚无评论）** — Antigravity 429 问题暂未引起社区讨论，也无人认领，需关注是否由维护者介入诊断。
  https://github.com/sipeed/picoclaw/issues/3339

---

*本日报基于 2026-08-18 的 PicoClaw GitHub 仓库数据生成。*

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目日报 · 2026-08-18

> 数据窗口：过去 24 小时（截至 2026-08-17 23:59 UTC）

## 1. 今日速览

过去 24 小时 NanoClaw 活跃度很高：新增/更新 Issue 4 条（3 开放、1 关闭），PR 更新 39 条（16 条待合并、23 条已关闭/合并）。核心团队（core-team）继续推进 **"接缝化"（seam）架构改造**——Channel 桥接钩子、Session Runtime Driver 抽象、MCP 工具扩展机制、Setup 向导扩展点等一批 PR 集中合并，项目向模块化、可插拔方向迈出了明显一步。社区端则集中暴露了 2 个回归类问题（[#3301](https://github.com/nanocoai/nanoclaw/issues/3301)、[#3289](https://github.com/nanocoai/nanoclaw/issues/3289)），均已由报告者提交修复 PR，响应闭环较快。此外，两个功能几乎相同的本地 Web Chat PR（[#3298](https://github.com/nanocoai/nanoclaw/pull/3298)、[#3290](https://github.com/nanocoai/nanoclaw/pull/3290)）同日出现，需求信号强烈。今日无新版本发布。

## 2. 版本发布

无。

## 3. 项目进展

今日合并/关闭的 PR 中，大多数来自 core-team 的架构接缝系列，核心目的是将「修改源码才能扩展」变为「注册模块即可扩展」：

**Channel 层**
- [#3305](https://github.com/nanocoai/nanoclaw/pull/3305)：关闭。Slack 共享 channel-layer 库 + canvas cluster（wave A，包含 main 同步）
- [#3292](https://github.com/nanocoai/nanoclaw/pull/3292)：关闭。Chat SDK 桥接 inbound-policy 注册接缝，统一拦截入站派发路径
- [#3295](https://github.com/nanocoai/nanoclaw/pull/3295)：关闭。通用 membership-event 钩子，channel 模块可接管房间成员行为
- [#3304](https://github.com/nanocoai/nanoclaw/pull/3304)：关闭。adapter 声明 session-mode 上下文默认值，支持 thread-per-conversation 模式

**Router / Delivery**
- [#3293](https://github.com/nanocoai/nanoclaw/pull/3293)：关闭。session-created 钩子，新会话创建时通知已注册模块
- [#3294](https://github.com/nanocoai/nanoclaw/pull/3294)：关闭。post-delivery 钩子，带 first-delivery 上下文标记

**Agent Runner / Setup**
- [#3296](https://github.com/nanocoai/nanoclaw/pull/3296)：关闭。`extendTool`——无侵入扩展 MCP 工具 schema/description/passthrough
- [#3297](https://github.com/nanocoai/nanoclaw/pull/3297)：关闭。Setup 向导新增 per-channel pre-step 与 companion-skill 扩展点

**在途架构推进**（待合并）

- [#3306](https://github.com/nanocoai/nanoclaw/pull/3306)：session-runtime driver seam，Docker 为内置实现（纯增量，无调用点变更）
- [#3307](https://github.com/nanocoai/nanoclaw/pull/3307)：host 会话生命周期（spawn、adoption、supervision）路由至 driver seam
- [#3308](https://github.com/nanocoai/nanoclaw/pull/3308)：拒绝在已存在未处置文件夹上创建新 agent group，关闭数据丢失风险

整体来看，项目正从单体 host/container 向 **可插拔、多运行时、多 Channel** 的模块化内核演进。

## 4. 社区热点

**Codex 生态稳定性（高关注）**

- [#3203](https://github.com/nanocoai/nanoclaw/issues/3203)：`codex` provider 发出未声明的 `file` ProviderEvent，导致 `/add-codex` 在主分支无法通过 typecheck，且生成的图片被静默丢弃。这是直接影响官方技能可用性的技术债。
- [#3299](https://github.com/nanocoai/nanoclaw/pull/3299)：`/add-codex` 技能仍 pin `@openai/codex` 0.138.0，而该版本默认模型 GPT-5.4 将于 **2026-08-31** 从 Codex 退役。PR 建议升级到 0.146.0，属于时间敏感型修复。

**本地 Web Chat：重复需求出现**

- [#3298](https://github.com/nanocoai/nanoclaw/pull/3298)（amit-shafnir）与 [#3290](https://github.com/nanocoai/nanoclaw/pull/3290)（viiluxx）同日提交了两个本地浏览器聊天实现。前者走 Channel adapter 框架，后者基于 Node `http` builtin 零依赖实现。两者定位高度重叠，说明「无需外部 SaaS 的本地聊天界面」是真实且未被满足的社区需求；维护者需尽快决定合二为一或明确取舍。

**回归修复快速跟进**

- [#3301](https://github.com/nanocoai/nanoclaw/issues/3301) 报告后，同一位用户 glifocat 当天即提交修复 PR [#3303](https://github.com/nanocoai/nanoclaw/pull/3303)，属于高效的社区自愈案例。

## 5. Bug 与稳定性

按严重程度排列：

| 严重度 | 编号 | 问题 | 修复状态 |
|---|---|---|---|
| 高 | [#3301](https://github.com/nanocoai/nanoclaw/issues/3301) | 聊天会话中触发 task 会整体切换至任务模式，导致 run logs 丢失、回复被吞、系列不可见（#2988 引入的回归） | 已有修复 PR [#3303](https://github.com/nanocoai/nanoclaw/pull/3303) |
| 高 | [#3203](https://github.com/nanocoai/nanoclaw/issues/3203) | `codex` provider 发射未声明的 `file` ProviderEvent，主分支 typecheck 失败，codex 生成图片被静默丢弃 | 暂无直接修复 PR（[#3299](https://github.com/nanocoai/nanoclaw/pull/3299) 仅解决 pin 版本，不覆盖此问题） |
| 中高 | [#3289](https://github.com/nanocoai/nanoclaw/issues/3289) | `getPendingMessages()` 将全部到期 pending 行加载进 JS 后才应用 limit，积压时存在内存与轮询压力 | 已有修复 PR [#3291](https://github.com/nanocoai/nanoclaw/pull/3291) |
| 中 | [#3300](https://github.com/nanocoai/nanoclaw/pull/3300) | agent-facing XML 中 attachment `type` 字段未转义，存在格式与注入风险 | 修复 PR 本身待合并 |
| 中 | [#3302](https://github.com/nanocoai/nanoclaw/pull/3302) | OneCLI gateway 默认绑定地址错误，`ONECLI_URL` 写入 Docker 网桥地址，外部无法访问（对应 [#2903](https://github.com/nanocoai/nanoclaw/issues/2903)） | 修复 PR 待合并 |
| 低 | [#1143](https://github.com/nanocoai/nanoclaw/issues/1143) | Skills 文档引用已移除的 `/data/env` 路径 | 已关闭（今日） |

**今日新增回归值得特别关注**：[#3301](https://github.com/nanocoai/nanoclaw/issues/3301) 是自 2.1.48 引入的行为回归，且影响面覆盖所有历史任务行 + 会话内新任务行，建议优先 review [#3303](https://github.com/nanocoai/nanoclaw/pull/3303)。

## 6. 功能请求与路线图信号

- **本地 Web Chat**（[#3298](https://github.com/nanocoai/nanoclaw/pull/3298)、[#3290](https://github.com/nanocoai/nanoclaw/pull/3290)）：两个独立实现同时出现，大概率会被纳入下一版本 Channel 体系，但需要维护者先解决重复 PR 的取舍。
- **Session Runtime Driver 抽象**（[#3306](https://github.com/nanocoai/nanoclaw/pull/3306)、[#3307](https://github.com/nanocoai/nanoclaw/pull/3307)）：虽为 core-team 主动架构推进，但为将来非 Docker runtime（本地进程、云沙箱）打开了明确的路线图空间。
- **可观测性仪表盘**（[#3288](https://github.com/nanocoai/nanoclaw/pull/3288)）：`/add-clawmetry` 技能，安装只读本地仪表盘，用于浏览会话与过夜活动，属于自托管可观测性需求。
- **CLI 结构化输入**（[#3218](https://github.com/nanocoai/nanoclaw/pull/3218)）：`--stdin-json` 有界 JSON 输入模式，服务脚本自动化场景；PR 已开放一周多，尚未获 review。

## 7. 用户反馈摘要

- **任务日志不可追溯是真实痛点**（[#3301](https://github.com/nanocoai/nanoclaw/issues/3301)）：用户指出任务在 chat session 内触发后「logs dropped、replies eaten、series unlisted」，日志与审计能力被破坏，直接影响了用户对任务执行的信任。
- **文档断裂拖累新手**（[#1143](https://github.com/nanocoai/nanoclaw/issues/1143)）：该文档问题自 3 月 16 日创建，直到 8 月 17 日才关闭，持续 5 个月。用户按 skills 文档操作必然会踩到已不存在的路径。
- **静默失败比报错更让人困扰**（[#3203](https://github.com/nanocoai/nanoclaw/issues/3203)）：codex 生成的图片被丢弃但无任何报错，用户很难自行定位，体验伤害远大于显式错误。
- **社区贡献者非常积极**：glifocat 一天内报告 2 个回归并各自提交修复 PR（[#3303](https://github.com/nanocoai/nanoclaw/pull/3303)、[#3291](https://github.com/nanocoai/nanoclaw/pull/3291)），对项目健康度是正向信号。

## 8. 待处理积压

- [#3203](https://github.com/nanocoai/nanoclaw/issues/3203)：开启已 9 天，直接影响官方 `/add-codex` 技能且图片被静默丢弃，目前仍无归属人、无直接修复 PR，建议尽快排期。
- [#3218](https://github.com/nanocoai/nanoclaw/pull/3218)：开启 8 天，`--stdin-json` PR 至今无 review 意见。如不在当前路线图内，建议维护者明确回复，避免 contributor 长期等待。
- [#3299](https://github.com/nanocoai/nanoclaw/pull/3299)：OpenAI Codex 模型退役日期为 **2026-08-31**，属于硬时限依赖升级，不宜拖延。

---

**项目健康度总评**：高活跃 + 架构推进清晰 + Bug 修复闭环快；但 codex 类型问题悬置、双 WebChat PR 待决策、文档类 issue 清理周期过长是当前主要风险点。

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

# NullClaw 项目动态日报 — 2026-08-18

> 数据来源：GitHub (github.com/nullclaw/nullclaw) | 统计窗口：2026-08-17 ~ 2026-08-18

---

## 1. 今日速览

过去 24 小时 NullClaw 项目活跃度较低：无新增 Issue、无 Issue 关闭、无新版本发布，仅 1 个 PR 处于待合并状态（依赖更新）。唯一动态是依赖机器人提交的 Docker 基础镜像升级 PR（#956），该 PR 已搁置两个月仍未合并，表明维护者注意力可能集中在其他事务上。整体来看，项目当前处于平静期，既无重大功能推进，也无新增 Bug 报告，健康度中等偏稳定。

---

## 2. 版本发布

今日无新版本 Release。

---

## 3. 项目进展

### 今日合并 / 关闭 PR：无

今日无 PR 被合并或关闭，项目主分支无新代码流入。

### 待合并 PR（持续关注）

| PR | 内容 | 状态 |
|---|---|---|
| [#956](https://github.com/nullclaw/nullclaw/pull/956) | [dependencies, docker] ci(deps): bump alpine from 3.23 to 3.24 in the docker-images group | OPEN（待合并） |

**说明**：此 PR 由 Dependabot 自动发起，将 Docker 镜像基础依赖 Alpine Linux 从 3.23 升级至 3.24。Alpine 3.24 是新的稳定分支，包含安全修复与工具链更新。该 PR 自 6 月 15 日创建以来已超过两个月未合并，虽然不阻塞当前功能开发，但长期不合并会导致依赖版本滞后，建议维护者尽快评估并处理。

---

## 4. 社区热点

今日无高讨论量 Issue / PR。唯一动态是 PR #956，虽无评论互动，但该 PR 的长期滞留某种程度上反映出容器镜像依赖升级未获得优先关注。在开源项目中，这类自动依赖 PR 通常意味着**基础运维层面的技术债务**——若不及时处理，后续升级可能面临更大的兼容性跳跃。

---

## 5. Bug 与稳定性

今日无新增 Bug 报告、崩溃或回归问题。项目稳定性指标良好。

---

## 6. 功能请求与路线图信号

今日无新功能请求。结合 PR #956 来看，项目在 **Docker 镜像基础环境现代化**方面存在一个未落地的改进点。Alpine 3.24 升级如获合并，将为项目带来更新的 musl libc、OpenSSL 等基础组件，从侧面增强容器运行时的安全性与兼容性。建议将其纳入近期维护里程碑。

---

## 7. 用户反馈摘要

今日无 Issue 评论可提取。由于 PR #956 由 Dependabot 自动创建、无人工评论，无法从社区获取直接用户反馈。如需更深入的用户声音，建议关注后续有实际互动的新 Issue / PR。

---

## 8. 待处理积压

| 项目 | 详情 | 搁置时长 | 优先级建议 |
|---|---|---|---|
| [PR #956](https://github.com/nullclaw/nullclaw/pull/956) | Dependabot 提交的 Alpine 3.23 → 3.24 升级，状态 OPEN | 约 2 个月（创建于 2026-06-15） | 中 — 建议尽快合并或关闭，避免技术债累积 |

**提醒**：该 PR 属于自动化维护请求，虽不紧急，但长期未处理会降低 Dependabot 后续自动化效率，也可能在 CI 中产生告警噪音。维护者如当前版本对 Alpine 版本有兼容性顾虑，建议明确关闭并在 Issue 中说明原因，以便社区知晓。

---

*本日报由 AI 自动生成，数据抓取于 2026-08-18。所有链接均指向 GitHub 仓库 nullclaw/nullclaw。*

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报 — 2026-08-18

## 1. 今日速览

过去 24 小时项目活跃度极高：共 28 条 Issue 更新（新开/活跃 22 条、关闭 6 条）与 44 条 PR 更新（待合并 28 条、合并/关闭 16 条），并发布 1.3.0-rc.1 候选版本。核心主线是围绕 Epic #7591（降低持久层 DB 写入压力约 60%）展开的多层级优化，多个 Tier 2/3 子任务在今日集中推进并有关闭，同时新增了 libSQL 写连接饥饿导致资源治理器级联故障（#7714，已有修复 PR）。此外，Slack 连接体验（#7681/#7682/#7710）、WebUI 通知中心泛化为持久收件箱（#7687-#7691）等用户可见改进也在并行推进。项目整体处于高密度迭代期，健康度良好，但数据库写入压力和生产稳定性问题仍需重点关注。

## 2. 版本发布

**ironclaw-v1.3.0-rc.1**（2026-08-17）

- Release 链接：https://github.com/nearai/ironclaw/releases/tag/ironclaw-v1.3.0-rc.1
- 提供 shell 脚本与 PowerShell 脚本两种预编译二进制安装方式。
- ⚠️ 注意：Release Notes 为空，未提供任何变更说明、破坏性变更或迁移指南。建议维护者尽快补充发布说明，便于社区评估升级风险。

## 3. 项目进展

今日关闭/合并的 PR 与 Issue 揭示了以下关键推进：

**存储写入压力优化（Epic #7591）**
- [#7594 [Tier 1] Route loop milestone sink through CoalescingEventSink](https://github.com/nearai/ironclaw/issues/7594) — 已关闭。将里程碑事件写入合并为批量 INSERT，每个 turn 节省约 30 次连接池检出。
- [#7598 [Tier 2] Collapse capability invocation-state writes to gate/terminal edges](https://github.com/nearai/ironclaw/issues/7598) — 已关闭。Epic 中最大单项优化，预计每 turn 节省约 40 行写入。
- [#7605 [Tier 3] Fold message lookup-index sibling rows into the message row](https://github.com/nearai/ironclaw/issues/7605) — 已关闭。消除热表上的索引触发扇出，每消息节省 1-3 行。

**1.2 修复前向移植**
- [PR #7663 fix(release): forward-port 1.2 fixes and thread repair](https://github.com/nearai/ironclaw/pull/7663) — 已合并。将 Windows 文件系统/发布冒烟测试可靠性、Windows JSON 输出清理、健康检查运行时 curl 等 1.2 修复同步至 main 分支。

**Slack 连接体验修复**
- [PR #7710 fix(slack): address multi-agent review findings on #7682](https://github.com/nearai/ironclaw/pull/7710) — 已关闭，修复 WebUI 连接链接落地页硬化，并将审查发现合并入 #7682 的 PR 分支。

**设计系统类型安全**
- [#7637 Type the design-system component boundary](https://github.com/nearai/ironclaw/issues/7637) — 已关闭。为共享设计系统组件补充显式 prop 类型，防止非法 variant/size/事件处理器绕过 TS 校验。

**自动化调度确定性**
- [#7647 feat(automations): add a deterministic no-delivery outcome for scheduled runs](https://github.com/nearai/ironclaw/issues/7647) — 已关闭。为定时运行增加确定性"不投递"结果，避免仅靠提示词无法保证静默抑制的问题。

**综合评估**：项目在"降低 DB 写入压力"和"自动化可靠性"两条主线上均有实质进展；RC 发布表明距离 1.3.0 正式版已近。但今日并未看到新功能的大规模合并，多数 PR 仍在审查中（28 条待合并）。

## 4. 社区热点

**讨论最活跃的 Issue：**

| Issue | 评论数 | 说明 |
|-------|--------|------|
| [#7275 Reborn: verify explicit persistent memory recall across conversations in production](https://github.com/nearai/ironclaw/issues/7275) | 4 | 已关闭。验证生产环境跨会话显式持久记忆的可靠性，是用户反馈的直接跟进 |
| [#7591 Epic: reduce durable DB write pressure ~60% while keeping multi-worker safety](https://github.com/nearai/ironclaw/issues/7591) | 3 | 当前最核心的性能优化 Epic，衍生多层级子任务 |
| [#7701 [Tier 2] Collapse resource-governor reserve+reconcile into one post-call spend write](https://github.com/nearai/ironclaw/issues/7701) | 2 | Epic 创建后发现的新缺口，每 turn 节省 11 行 |
| [#7603 [Tier 3] Batch BeforeModel checkpoints per-N iterations](https://github.com/nearai/ironclaw/issues/7603) | 2 | 因集成测试证明原方案不安全，拆分出替代方案 #7707 |
| [#7604 [Tier 3] Collapse paired row writes](https://github.com/nearai/ironclaw/issues/7604) | 2 | 四处独立的成对写入折叠 |

**分析**：讨论热度集中在 #7591 Epic 及其子任务上，反映维护团队正集中精力解决持久层写入放大问题。值得关注的是 #7603 → #7707 的拆分：原方案因集成测试证明不安全而被放弃，真正的优化转移到了 #7707（在进程行上显式跟踪 side-effect-outstanding 状态），这体现了项目对"安全优先"的坚持。

## 5. Bug 与稳定性

按严重程度排列：

**高严重度**
- [#7714 libSQL: single shared write connection starves the resource-governor journal under bench load](https://github.com/nearai/ironclaw/issues/7714)（2026-08-17，无评论）— 资源治理器 delta journal 在 PinchBench 负载下反复停滞约 40 秒，引发级联故障：authority 失效、journal 替换、持久状态重载，并导致预留永久泄漏。**已有修复 PR #7717 待合并。**

**中严重度**
- [#7702 Obligation audit records (AuditBefore/AuditAfter) are never attached in production, violating the documented host-api contract](https://github.com/nearai/ironclaw/issues/7702)（2026-08-17）— 审计记录根本未写入，违反文档规定的 host-api 契约，属于功能缺失而非性能问题。尚无 fix PR。
- [#7705 Follow-ups from #7631: unbounded shutdown flush and latching pending_flush_error in CoalescingEventSink](https://github.com/nearai/ironclaw/issues/7705)（2026-08-17）— 审查 #7631 时发现两个非阻塞问题：关闭时可能因事件后端卡死而挂起；pending_flush_error 状态锁存。尚无 fix PR。

**低严重度 / 体验问题**
- [#7716 [bug_bash_P2] Add MCP server flow missing bearer key auth and STDIO/HTTP transport options](https://github.com/nearai/ironclaw/issues/7716)（2026-08-17）— QA 测试发现 MCP server 添加流程缺少 Bearer 认证和传输选项。尚无 fix PR。
- [#7715 [bug_bash_P2] Telegram connection flow lacks consent/selection between bot and personal account](https://github.com/nearai/ironclaw/issues/7715)（2026-08-17）— Telegram 连接流程未让用户选择 bot 或个人账号模式。尚无 fix PR。

**稳定性趋势**：#7714 是今日最严重的稳定性问题，与 Epic #7591 的写压力主题直接相关，修复 PR 已就绪，预计将很快合入。值得注意的是 #7704（Daily failure taxonomy）报告了 clawbench 84 个非通过用例的最大可修复缺陷也是"存储写通道竞争"——与 #7714 为同一根源。

## 6. 功能请求与路线图信号

**今日新提出的功能需求：**

- [#7719 Expose GitHub Projects v2 field manipulation in GitHub tool](https://github.com/nearai/ironclaw/issues/7719)（2026-08-17）— 请求将 GitHub Projects v2 字段（如 Main backlog priority）的更新能力暴露给 IronClaw 工具。该需求直接阻塞了 #7716 的优先级更新工作流，属于开发自举需求，大概率会被采纳。

- [#7687-#7691 通知中心泛化为持久用户收件箱系列（5 个 Issue）](https://github.com/nearai/ironclaw/issues/7687)（2026-08-17，italic-jinxin）— 将仅支持自动化审批的通知中心扩展为持久的、用户粒度的收件箱，支持审批、认证要求、阻塞运行、运行失败/完成等通知类型。5 个 Issue 构成完整实施路径，属于明确的路线图规划。

- [#7707 Track side-effect-outstanding explicitly on the process row](https://github.com/nearai/ironclaw/issues/7707)（2026-08-17）— 拆分自 #7603，真正的 ~14 行/turn 优化所在。将进程行上显式跟踪副作用未完成状态，而非从最新检查点类型推断。技术性重构，属于 #7591 Epic。

**已有关联 PR 的功能：**

- [PR #7708 feat(automations): add run-now across trigger domain and WebUI](https://github.com/nearai/ironclaw/pull/7708)（待合并）— 为自动化增加"立即运行"能力，保留调度计划。若合入，将进入 v1.3.0。
- [PR #7718 fix(google-docs): add semantic editing tools](https://github.com/nearai/ironclaw/pull/7718)（待合并）— 增加四个语义化 Google Docs 能力（结构化检查、锚定批量编辑、填充表格、确定性验证），同时保留 11 个旧工具。若合入，将进入 v1.3.0。
- [PR #7694 feat: add durable backend suggestions](https://github.com/nearai/ironclaw/pull/7694)（待合并）— 新品面无关的 `suggestions.*` 操作集，通过异步 canonical runner 生成建议。
- [PR #7693 feat: add native structured output finalization](https://github.com/nearai/ironclaw/pull/7693)（待合并）— 无需内置工具即可实现结构化输出最终化。

## 7. 用户反馈摘要

- **跨会话记忆可靠性（#7275）**：用户反馈（源自 #7185）明确表示，在同一用户的不同会话中，先前对话中明确建立的信息无法被可靠回忆。该 Issue 已关闭，说明项目团队已进行验证并可能已解决或确认行为，但未在 Release Notes 中体现。

- **Slack 未关联用户连接体验（#7681）**：在共享频道中，未关联 IronClaw 账号的用户收到的是公开的连接提示，且需要手动多步骤操作，用户困惑"连接链接到底是什么"。修复 PR #7682 已提交并获多智能体审查（#7710），将改为私密投递 + 一键连接链接，体验将有显著改善。

- **MCP server 配置缺项（#7716）**：QA 用户发现添加 MCP server 时仅需填写名称 ID 和地址，缺少 Bearer token 认证和 STDIO/HTTP 传输选项，无法连接需要认证的 MCP 服务。

- **Telegram 连接模式不明确（#7715）**：QA 用户反馈 Telegram 连接流程同时支持 bot 和个人账号，但用户无法选择模式，也不清楚当前连接的是哪个。

- **AGENTS.md 编辑不生效（#3762）**：用户编辑 AGENTS.md 后保存成功，但系统提示词不更新，影响当前和未来会话。该 Issue 已存在 3 个月，标记为 suggested_P1 但尚未关闭。

## 8. 待处理积压

| 项目 | 类型 | 创建时间 | 最后更新 | 积压天数 | 说明 |
|------|------|----------|----------|----------|------|
| [#3762 Editing AGENTS.md in the web UI does not update the system prompt](https://github.com/nearai/ironclaw/issues/3762) | Issue | 2026-05-18 | 2026-08-17 | 92 天 | 标记为 suggested_P1 和 v1.4.0，客户可见功能缺陷，长期未关闭 |
| [#6994 feat(webui): OOBE automation-tasks prototype](https://github.com/nearai/ironclaw/pull/6994) | PR | 2026-08-01 | 2026-08-17 | 17 天 | 新用户引导体验改进，设计+实现均已完成但仍在审查中 |
| [#7184 feat: Nostr host functions for WASM tools (reborn)](https://github.com/nearai/ironclaw/pull/7184) | PR | 2026-08-04 | 2026-08-17 | 14 天 | 新贡献者 Kampouse 提交，Nostr 签名等功能已实现，等待审查 |
| [#7406 chore(deps): bump the actions group with 4 updates](https://github.com/nearai/ironclaw/pull/7406) | PR | 2026-08-09 | 2026-08-17 | 9 天 | Dependabot 依赖更新，长期未合并可能导致 CI 依赖过旧 |
| [#7513 feat(cli): add ACP serve command with streaming + cancel support](https://github.com/nearai/ironclaw/pull/7513) | PR | 2026-08-11 | 2026-08-17 | 7 天 | 新贡献者 Kampouse 提交，实现 ACP 协议支持，等待审查 |

---

**总结**：IronClaw 今日处于高密度开发状态，核心焦点是持久层写入压力优化（#7591 Epic）和随之暴露的稳定性问题（#7714）。1.3.0-rc.1 已发布但缺少 Release Notes，建议尽快补充。社区反馈聚焦于可用性细节（Slack 私密连接、MCP 认证、Telegram 模式选择），这些均有望在近期版本中得到改善。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报 — 2026-08-18

> 数据来源：github.com/netease-youdao/LobsterAI ｜ 统计窗口：过去 24 小时

## 1. 今日速览

本周期 LobsterAI 仓库活跃度较高。统计窗口内共发生 **7 条 Issue 更新**（全部保持开放、无新增关闭）与 **21 条 PR 更新**（17 条被合并/关闭，4 条仍在等待合并），另有 **1 个全新 Issue（#2500）** 提交，无新版本发布。活动形态呈"新贡献持续流入、旧积压集中清理"特征：当天提交的 4 个新 PR（#2501–#2505）均快速进入合并/关闭流程，同时 4 月中旬遗留下的 12 个 stale 标记 PR 被批量处理；Issue 侧除 VOKO 集成提案外，其余 6 条均为已 stale 旧 Issue 的自动更新。整体看，PR 闭合效率高、外部贡献者参与积极，但 Ollama/MCP/文件转换等遗留功能缺陷已积压 4 个月未修复，项目健康度评为**中上**。

## 2. 版本发布

本周期无新版本发布。

## 3. 项目进展

过去 24 小时有 **17 条 PR 被关闭/合并**，可拆分为两类：**当天提交并快速处理的新 PR**，以及 **4 月积压的 stale PR 集中清理**。后者虽然占多数，但若这些 PR 最终被合并，将使项目在 Agent 协作体验、国际化、安全与运行时层面获得一次批量升级。

### 当天提交的新 PR（高质量、响应快）

| PR | 内容 | 状态 |
|---|---|---|
| [PR #2505](netease-youdao/LobsterAI%20PR%20#2505) | DSH（DeepSeek Harness）进程启动器 | 已关闭 |
| [PR #2502](netease-youdao/LobsterAI%20PR%20#2502) | DSH 引擎集成，含 macOS 平台构建适配 | 已关闭 |
| [PR #2503](netease-youdao/LobsterAI%20PR%20#2503) | Electron 主窗口文本输入框增加右键编辑菜单（剪切/复制/粘贴/全选） | 已关闭 |
| [PR #2501](netease-youdao/LobsterAI%20PR%20#2501) | 修复技能升级进度浮层未覆盖整个应用外壳的问题，并补充相关 renderer 日志 | 已关闭 |

这组 PR 由贡献者 `fisherdaddy` 与 `liuzhq1986` 密集提交，展现了良好的外部协作生态，尤其是 **DSH（DeepSeek Harness）运行时集成** 对开发者社区有明确价值。

### 积压 PR 清理（4 月创建、本周期关闭/合并）

若这些 PR 最终被合并，项目将获得以下能力：

- **OpenClaw 运行时升级**：[PR #1663](netease-youdao/LobsterAI%20PR%20#1663) 将 OpenClaw 从 v2026.3.2 升至 v2026.4.12，并将 openclaw-weixin 插件从 1.0.3 升至 2.1.8
- **多 Agent 工作目录隔离**：[PR #1668](netease-youdao/LobsterAI%20PR%20#1668) 为每个非 main Agent 增加独立工作目录配置
- **Cowork 聊天体验**：[PR #1636](netease-youdao/LobsterAI%20PR%20#1636) 新增「滚动到底部」悬浮按钮、[PR #1637](netease-youdao/LobsterAI%20PR%20#1637) 增加 AI 回复「重新生成」按钮、[PR #1640](netease-youdao/LobsterAI%20PR%20#1640) 为工具执行结果增加一键复制
- **弹窗交互统一**：[PR #1641](netease-youdao/LobsterAI%20PR%20#1641) 所有 Modal 支持 Esc 关闭
- **安全加固**：[PR #1661](netease-youdao/LobsterAI%20PR%20#1661) 导出的日志脱敏，避免明文 API Key/Token 泄露
- **国际化**：[PR #1639](netease-youdao/LobsterAI%20PR%20#1639) 修复多处按钮 tooltip 硬编码英文的问题
- **设置页体验**：[PR #1669](netease-youdao/LobsterAI%20PR%20#1669) 修复模型提供商「测试连接」按钮禁用逻辑；[PR #1667](netease-youdao/LobsterAI%20PR%20#1667) 将 Qwen 控制台链接迁移至阿里云百炼
- **Windows 集成**：[PR #1642](netease-youdao/LobsterAI%20PR%20#1642) 新增注册表级右键菜单

### 仍在等待合并的 PR（4 条）

- [PR #2506](netease-youdao/LobsterAI%20PR%20#2506)：DSH 运行时安装文档
- [PR #2504](netease-youdao/LobsterAI%20PR%20#2504)：新增 OrcaRouter 网关提供商（镜像 OpenRouter 全链路）
- [PR #1277](netease-youdao/LobsterAI%20PR%20#1277)：dependabot 的 Electron 40→43 大版本依赖更新
- [PR #1660](netease-youdao/LobsterAI%20PR%20#1660)：非 main agent 首页欢迎区域显示 agent 名称与描述

## 4. 社区热点

本周期讨论热度不高，但有一个值得注意的新信号：

- **[Issue #2500](netease-youdao/LobsterAI%20Issue%20#2500)（新开）**：开源项目 VOKO 作者主动提交集成提案，希望将 LobsterAI 接入其"AI 智能体跨平台通信层"，以实现不同 Agent 框架/IM 渠道间互通，目前已接入 OpenClaw、VOKO IM、AstrBot。这反映出外部项目对 LobsterAI 生态位（桌面 Agent 运行时）的认可，也暗示**Agent 互操作（A2A）**正在成为社区关注方向。
- **[Issue #1653](netease-youdao/LobsterAI%20Issue%20#1653)** 有 2 条评论并持续被更新，是本期评论最多的 Issue，用户反复报告配置被覆盖但始终未获官方响应。
- 本轮 PR 评论量数据未在导出中提供，无法对 PR 讨论热度和 reviewer 反馈量做量化分析。

## 5. Bug 与稳定性

按严重程度排列（均未有已合并的 fix PR）：

| 严重度 | Issue | 描述 | 状态 |
|---|---|---|---|
| 🔴 高 | [Issue #1635](netease-youdao/LobsterAI%20Issue%20#1635) | Ollama 本地模型（qwen3、gemma4 等）无法使用，报错；用户确认 Ollama 本身正常、CherryStudio 可正常调用 | 开放中，stale |
| 🔴 高 | [Issue #1671](netease-youdao/LobsterAI%20Issue%20#1671) | MD 转 Word 执行到一半中断，报 `sse response finish reason: full`，疑似长文本截断 | 开放中，stale |
| 🟠 中 | [Issue #1662](netease-youdao/LobsterAI%20Issue%20#1662) | 除 SSE 外的 MCP 引擎（stdio/HTTP）无法被发现和使用 | 开放中，stale |
| 🟠 中 | [Issue #1653](netease-youdao/LobsterAI%20Issue%20#1653) | groupPolicy 每隔一段时间被自动覆盖回 allowlist，用户配置不持久 | 开放中，stale |
| 🟡 低 | [Issue #1643](netease-youdao/LobsterAI%20Issue%20#1643) | 手动创建定时任务时提示"还有内容未保存"，但实际已保存，存在误导性交互 | 开放中，stale |

值得肯定的是，日志脱敏修复 [PR #1661](netease-youdao/LobsterAI%20PR%20#1661) 已进入关闭/合并流程，主进程日志不再泄露 API Key 与 Token，属于上期发现的安全痛点，本期已得到处置。

## 6. 功能请求与路线图信号

- **Agent 主动编排**：[Issue #1644](netease-youdao/LobsterAI%20Issue%20#1644) 用户强烈希望 main agent 能感知并调度其它 agent（非 spawn 方式），通过基于 Markdown 的工作流完成复杂任务。该需求涉及 Agent 间共享记忆与协调，很可能成为下一阶段架构重点。
- **外部集成邀请**：[Issue #2500](netease-youdao/LobsterAI%20Issue%20#2500) VOKO 提议为 LobsterAI 提供跨平台 Agent 通信层与群协作能力。是否会纳入路线图尚未有官方回应，但方向与 #1644 的"多 Agent 协作"诉求一致。
- **新增 Provider 支持**：[PR #2504](netease-youdao/LobsterAI%20PR%20#2504) 将 OrcaRouter（Anthropic/OpenAI 兼容网关）接入 Provider 注册表。考虑到 OpenRouter 已是原生接入，新增网关表明项目在持续扩大模型访问生态。
- **已落地或接近落地**：Agent 独立工作目录（#1668）、会话按时间分组（#1675）、重新生成按钮（#1637）、tooltip 国际化（#1639）等此前社区呼声较高的功能，已在本次清理批次中出现，若并入主干将显著改善日常使用体验。

## 7. 用户反馈摘要

真实用户反馈集中在以下典型场景：

- **多 Agent 隔离是主要痛点**：[Issue #1644](netease-youdao/LobsterAI%20Issue%20#1644) 用户尝试让主 agent 调用已创建的「文章分析 agent」，系统通过 `memory_search` 和 `agents_list` 均无法检索到该 agent，主 agent 只能回答"没有找到文章分析 agent 的记录"并**主动提出替用户创建一个**。这暴露了当前多个 Agent 之间互不可见、记忆互不共享的架构短板，用户原话背景中还指出了"除非是它自己 spawn 出来的 openclaw subagent"这一限制。
- **本地模型调用失败引发困惑**：[Issue #1635](netease-youdao/LobsterAI%20Issue%20#1635) 用户强调"ollama 本身没有问题，使用 cherrystudio 客户端这两个模型都是好用的"，说明问题定位在 LobsterAI 与 Ollama 的集成链路，而非用户环境。
- **配置被静默覆盖**：[Issue #1653](netease-youdao/LobsterAI%20Issue%20#1653) "每次过一会就会被覆盖"，用户对 groupPolicy 的自动回退机制表示无法理解，且 4 个月未获官方解释，挫败感较强。
- **交互文案误导**：[Issue #1643](netease-youdao/LobsterAI%20Issue%20#1643) "提示还有内容未保存，但应用已经保存成功了"，用户对矛盾提示产生困惑。
- **VOKO 作者主动示好**：[Issue #2500](netease-youdao/LobsterAI%20Issue%20#2500) "先给您的项目点个 star 表示支持 👍"，反映出外部项目对 LobsterAI 的认可及对生态互通意愿。

## 8. 待处理积压

- **6 个 stale 状态的核心 Bug** 从 2026-04 起长期未解，本周期仅被 stale 机制自动标记更新，无实际修复进展，需要维护者重点关注并配置资源：
  - [Issue #1635](netease-youdao/LobsterAI%20Issue%20#1635)：Ollama 模型无法使用（高影响）
  - [Issue #1662](netease-youdao/LobsterAI%20Issue%20#1662)：非 SSE MCP 不可用（高影响）
  - [Issue #1671](netease-youdao/LobsterAI%20Issue%20#1671)：MD 转 Word 中途中断
  - [Issue #1644](netease-youdao/LobsterAI%20Issue%20#1644)：Agent 编排功能请求
  - [Issue #1653](netease-youdao/LobsterAI%20Issue%20#1653)：groupPolicy 被覆盖
  - [Issue #1643](netease-youdao/LobsterAI%20Issue%20#1643)：定时任务保存提示歧义
- **长时间未合并的依赖更新**：[PR #1277](netease-youdao/LobsterAI%20PR%20#1277) dependabot 发起、将 Electron 从 40.2.1 升级至 43.4.0，已开放 4 个多月未处理；跨大版本升级越晚合并，冲突与回归风险越高，建议尽快安排升级验证。
- **长期未响应的功能 PR**：[PR #1660](netease-youdao/LobsterAI%20PR%20#1660) 为非 main agent 增加个性化欢迎区域，自 2026-04-13 起开放至今，若实现无争议建议尽快合入或明确拒绝，避免贡献者等待过久。

---

*本日报由 AI 分析师生成，数据基于 GitHub API 快照，部分评论数为空为导出字段缺失所致。如有统计口径问题，请以 GitHub 实际页面为准。*

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis 项目动态日报 — 2026-08-18

## 1. 今日速览

过去 24 小时 Moltis 项目保持活跃：2 个 Issue 全部关闭（1 个 CI 门禁修复 + 1 个功能请求落地），9 个 PR 中有 6 个已合并/关闭，3 个仍待合并。值得关注的是，3 个待合并 PR 中有 2 个直指 `heartbeat` 模块的功能缺陷（配置补丁语义与 active hours 失效），1 个是体量较大的 Files 库与设置浏览器功能；若合并，将显著改善心跳配置体验并扩展文件管理能力。依赖更新与既有功能增强也在稳步推进，项目整体健康度良好。

## 3. 项目进展

今日共 6 个 PR 关闭/合并，涵盖功能增强、Bug 修复与依赖维护：

- **PR #1125**（已合并）— Support model and effort selection for external agents：为外部代理（external-agent providers）增加 `/model` 中的模型与 effort 选择支持，包括 `models`、`efforts` 配置项、`external-agent/<kind>` 分组以及模型/effort 元数据持久化。该 PR 从 6 月中旬持续迭代至今，落地后显著增强了外部代理的可配置性。
- **PR #1103**（已合并）— fix(browser): pierce shadow DOM lookups efficiently：优化浏览器快照与基于 ref 的查询路径，使其高效穿透 Shadow DOM，修复相关查找问题。这是对 #1100 的替代/更新路径，补齐了 review 意见。
- **PR #1130**（已合并）— feat: make webui rpc timeout configurable：让 WebUI 的 RPC 超时时间可配置，直接解决 #1127。
- **PR #1204**（已合并）— feat: add MiniMax Code ACP agent：新增 `acp-minimax-code` 外部代理类型（基于 `mcode acp`），并将 MiniMax Code 纳入默认可执行文件检测与代理注册表，附文档与配置校验同步。
- **PR #1207 / #1087**（已合并）— chore(deps): 依赖组批量更新（wasmtime-wasi、cmov、quinn-proto、serde_with；tar 0.4.45→0.4.46）。

整体来看，项目今日在外部代理生态扩展、WebUI 可配置性、浏览器 Shadow DOM 处理等方向均有实际进展，同时保持了依赖的及时更新。

## 4. 社区热点

今日无高评论量或高反应度的讨论热帖，两个关闭的 Issue 均为已解决状态，讨论已收敛。相对值得关注的是 PR #1206（Add managed Files library and Settings browser，OPEN），作为当前最大的待合并功能 PR（新增文件库 API、设置浏览器、Docker/Podman/Apple Container 挂载等），其设计范围广，后续 review 阶段预计会引发较多讨论。

## 5. Bug 与稳定性

今日新增 Bug 报告 0 条，但有 2 个 Bug 相关修复 PR 待合并：

| 严重程度 | 问题描述 | 状态 | 修复 PR |
|---------|---------|------|---------|
| 中 | `heartbeat.update` 将参数反序列化为 `HeartbeatConfig` 后**整体覆盖**配置，导致未传字段被重置为默认值，而非按 JSON Patch 语义合并（对应 Issue #1187） | 待合并 | PR #1209 |
| 中 | `heartbeat.active_hours` 从未生效：`is_within_active_hours` 虽已实现、文档化并测试，但**无任何调用方**，调度器在非活跃时段仍会触发心跳（对应 Issue #1205） | 待合并 | PR #1208 |

另有 CI 门禁问题已修复：Issue #1202 报告 `main` 分支上两个文件超出行数限制导致 Format 作业变红，目前已关闭（对应提交 9b47001a 引入的问题，已处理）。

## 6. 功能请求与路线图信号

- **WebUI RPC 超时配置**（Issue #1127）：用户请求允许配置 RPC 超时，已通过 PR #1130 实现并合并，进入主分支。
- **心跳 active_hours 的生效**（Issue #1205）：属于功能性缺陷而非新需求，对应修复 PR #1208 待合并，预计很快进入主分支。
- **外部代理模型与 effort 选择**（PR #1125）：虽是 PR 而非 Issue，但反映了用户对外部代理多模型切换的诉求，已合并。
- **托管文件库与设置浏览器**（PR #1206）：当前最大的开放功能 PR，如获合并，将为 Moltis 带来文件管理能力和更完整的设置 UI，值得关注其 review 进展。

## 7. 用户反馈摘要

今日可直接获取的用户反馈有限（0 条 Issue 评论），但可从 PR 描述中提炼出几点：

- `heartbeat.update` 的覆盖式反序列化行为出乎用户预期（Issue #1187），用户希望其表现为“补丁”（patch）而非“全量替换”——即未提供的字段应保持原值。这反映了用户对配置 API 语义精细化的需求。
- `heartbeat.active_hours` 已实现却被完全忽略（Issue #1205），属于“写了文档但功能未接线”的典型问题，说明用户在实际运行中确实依赖该配置来控制心跳时段，并已发现其不生效。

## 8. 待处理积压

- **PR #1209**（OPEN，创建于 2026-08-17）：修复 `heartbeat.update` 补丁语义。问题存在已久（Issue #1187），且属于配置正确性 Bug，建议尽快合并。
- **PR #1208**（OPEN，创建于 2026-08-17）：修复 `heartbeat.active_hours` 完全不生效的问题（Issue #1205）。功能已实现但未被调用，修复逻辑清晰，建议优先处理。
- **PR #1206**（OPEN，创建于 2026-08-17）：Add managed Files library and Settings browser。体量大、涉及面广，需要细致 review，建议安排专人跟进以免长期滞留。

---

*日报生成时间：2026-08-18 | 数据来源：Moltis GitHub 仓库*

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw 项目动态日报（2026-08-18）

## 今日速览

过去 24 小时 CoPaw 保持高活跃度：14 条 Issues 更新（新开/活跃 8 条，关闭 6 条），35 条 PR 更新（待合并 13 条，合并/关闭 22 条），无新版本发布。项目当前处于 v2.1.0 的密集迭代修复期，合并/关闭 PR 占总数约 63%，维护节奏紧凑。社区反馈集中在 MCP/工具调用稳定性、Console 前端交互细节和多会话身份隔离三个方向，其中部分问题（如远程媒体 URL 过期）已有对应修复 PR。

## 版本发布

无新版本发布。

## 项目进展

今日合并/关闭了 22 条 PR，主要推进方向如下：

- **DataPaw 应用落地**：[#6940](https://github.com/agentscope-ai/QwenPaw/pull/6940) 合并，新增原生 DataPaw App 运行时与持久化分析工作区；[#7089](https://github.com/agentscope-ai/QwenPaw/pull/7089) 在其基础上提出独立版本驱动的发布流水线，打通插件 CDN 发布链路。
- **上下文用量计算修复**：[#6968](https://github.com/agentscope-ai/QwenPaw/pull/6968) 停止将图片 base64 按文本启发式估算 token，修复上传 1-2 张图片即令上下文环虚高至 100% 的问题；[#6975](https://github.com/agentscope-ai/QwenPaw/pull/6975) 修复 `/compact` 后上下文用量环不刷新的问题。
- **Console 交互优化**：[#7017](https://github.com/agentscope-ai/QwenPaw/pull/7017) 新装 PawApps 无需手动刷新即可打开；[#7036](https://github.com/agentscope-ai/QwenPaw/pull/7036) 为聊天媒体附件增加统一下载控件（播放栏按钮顺序调整为 播放 → 下载 → 音量 → 进度）；[#7083](https://github.com/agentscope-ai/QwenPaw/pull/7083) 压缩后台任务面板高度并增加滚动提示。
- **Bug 修复**：[#5151](https://github.com/agentscope-ai/QwenPaw/pull/5151) 修复 GitPanel 标签样式因 `ant-` 前缀与 `qwenpaw` prefixCls 不匹配而失效的问题（6 月 12 日创建，历时两个月后合并）；[#6981](https://github.com/agentscope-ai/QwenPaw/pull/6981) 清理全部 7 个语言文件中的 `/approve`、`/deny` 审批命令占位提示。
- **功能推进**：[#7078](https://github.com/agentscope-ai/QwenPaw/pull/7078) 为 Console 文件工作区增加系统提示词文件选择器，Profile 页仅显示内置与已启用的自定义提示词文件。

待合并的重要 PR 中，[#7087](https://github.com/agentscope-ai/QwenPaw/pull/7087)（模型请求前在客户端本地化远程媒体 URL）直击 #7088 一类问题；[#6515](https://github.com/agentscope-ai/QwenPaw/pull/6515)（新增火山方舟 Agent Plan 与小米 MiMo V2.5 内置提供商）与 [#6302](https://github.com/agentscope-ai/QwenPaw/pull/6302)（统一 provider 发现/模型元数据/路由的大型重构）已开放较久，值得关注。

## 社区热点

- **[#6405](https://github.com/agentscope-ai/QwenPaw/issues/6405)（已关闭，7 条评论）**：升级 2.0 后 MCP 工具持续提示 "Tool not found"。用户反馈工具名已变为 `[mcp-key]__[tool_name]` 但仍无法调用，评论中讨论了命名规则、Docker 版 2.0.0post3 环境的兼容性问题，以及升级前后配置差异。背后诉求是 MCP 工具命名变更的迁移文档与兼容层缺失。
- **[#7011](https://github.com/agentscope-ai/QwenPaw/issues/7011)（开放，6 条评论）**：Console UI 的停止请求可取消一个活跃的 Feishu 会话。用户于 8 月 14 日更新了结论，补充了会话身份值在两个 UI 会话间交叉的直接证据。该问题涉及多 UI 会话身份隔离的并发边界，属于较深层的架构问题，维护者需关注。
- **[#7085](https://github.com/agentscope-ai/QwenPaw/issues/7085)（开放，3 条评论）**：按频道独立配置模型的 Feature Request。用户给出钉钉（gpt-4o）、微信（qwen-max）、控制台（本地 llama.cpp）的具体差异化场景，指出当前 `agent.json` 的 `active_model` 字段与全局默认 LLM 设置无法满足渠道级隔离。

## Bug 与稳定性

按严重程度排列：

| 严重程度 | Issue / PR | 状态 | 说明 |
| --- | --- | --- | --- |
| **崩溃** | [#7063](https://github.com/agentscope-ai/QwenPaw/issues/7063) | 已关闭（invalid） | Agent 执行工具调用时必现崩溃，`_acting` 返回 coroutine 被 `async for` 遍历导致 TypeError。已标记 invalid，需确认是否用户侧异步误用 |
| **初始化失败** | [#7082](https://github.com/agentscope-ai/QwenPaw/issues/7082) | 开放 | Pydantic 报 `_StructuredOutputDynamicClass is not fully defined`，控制台渠道 agent/toolkit 初始化失败，暂无 fix PR |
| **会话数据影响** | [#7011](https://github.com/agentscope-ai/QwenPaw/issues/7011) | 开放 | Console 停止请求取消活跃 Feishu 会话，跨 UI 会话身份值交叉，暂无 fix PR |
| | [#7088](https://github.com/agentscope-ai/QwenPaw/issues/7088) | 已关闭 | OneBot 通道将短签名 QQ 图片 URL 直传模型，rkey 过期后模型侧下载 400，陈旧 URL 污染后续会话。**[#7087](https://github.com/agentscope-ai/QwenPaw/pull/7087) 可作为修复参考** |
| | [#7051](https://github.com/agentscope-ai/QwenPaw/issues/7051) | 已关闭 | Console 聊天中的图片附件在会话重载后丢失，前端显示破图与纯文本回退 |
| **UI 交互** | [#7084](https://github.com/agentscope-ai/QwenPaw/issues/7084) | 开放 | 历史会话仅一条时，新开聊天后点击历史会话无反应；当新聊天产生第二条会话后恢复，属列表状态切换边缘 Bug |
| | [#7077](https://github.com/agentscope-ai/QwenPaw/issues/7077) | 已关闭 | 插件运行时钩子在工作区重载（热安装）后静默丢失，`workspace_created` 回调不再触发 |
| **CLI/配置** | [#7048](https://github.com/agentscope-ai/QwenPaw/issues/7048) | 已关闭 | `qwenpaw cron update --text` 返回成功但 agent 类型任务 prompt 未更新，CLI 文档与行为不一致 |
| | [#7076](https://github.com/agentscope-ai/QwenPaw/issues/7076) | 开放 | qwenpaw-creator LLM 模型配置报 404（v2.1.0） |
| **升级兼容** | [#6405](https://github.com/agentscope-ai/QwenPaw/issues/6405) | 已关闭 | 2.0 后 MCP 工具命名变化导致 Tool not found，升级体验受损 |

需要强调的是，#7088、#7051、#7077 均已关闭，但数据中未见明确关联的 fix PR 被合并，建议维护者确认关闭原因（已修复 / 重复 / 无效），并在对应 PR 中补充关联以便追溯。

## 功能请求与路线图信号

- **按频道独立配置模型（[#7085](https://github.com/agentscope-ai/QwenPaw/issues/7085)）**：需求明确、场景具体，涉及 `agent.json` 模型配置模型从全局/智能体级下沉到渠道级，或引入路由覆盖层。可能会与 [#6302](https://github.com/agentscope-ai/QwenPaw/pull/6302) 的 provider 统一重构产生交集。
- **智能体协作在同一会话窗口（[#6925](https://github.com/agentscope-ai/QwenPaw/issues/6925)）**：用户对多智能体协作时“一对话一新建会话、需手动切换查看”的体验不满。改善协作信息架构可能成为多智能体体验的重要优化点。
- **定时任务运行细节（[#7075](https://github.com/agentscope-ai/QwenPaw/issues/7075)）**：希望在任务运行中/结束后展示开始时间、运行时长、结束时间与结果，而非仅在失败时给出信息，对长耗时任务的可观测性诉求明确。
- **可选 PowerContext 长期记忆后端（[#7079](https://github.com/agentscope-ai/QwenPaw/issues/7079)）**：已有对应 PR [#7080](https://github.com/agentscope-ai/QwenPaw/pull/7080)（均为同作者首次贡献），实现 `BaseMemoryManager` 抽象并注册为 `powercontext`，作为 ReMeLight 之外的可选后端。
- **AnySearch 集成（[#7081](https://github.com/agentscope-ai/QwenPaw/pull/7081)）**：首次贡献者提交，基于此前关闭的 [#6817](https://github.com/agentscope-ai/QwenPaw/pull/6817) 重新提交，提供可插拔 `web_search` 后端 + MCP 客户端 + 按智能体 Console 配置。

其余开放 PR 中，[#6976](https://github.com/agentscope-ai/QwenPaw/pull/6976)（会话级多项目目录）与 [#6719](https://github.com/agentscope-ai/QwenPaw/pull/6719)（持久化工作区工件卡片）代表了工作区/制品体验的演进方向，可能进入后续版本路线图。

## 用户反馈摘要

- **升级兼容性（#6405）**：用户升级 2.0 后 MCP 工具改名规则不透明、旧配置未迁移，导致工具不可用。反馈显示升级路径的兼容层或迁移说明文档需要加强。
- **并发会话隔离（#7011）**：用户两次更新 issue，提供“会话身份值在两个 UI 会话间交叉”的具体证据链。用户对问题的定位能力较强，报告质量高，值得优先处理。
- **CLI 可预期性（#7048）**：`cron update` 返回成功但数据未变，用户对命令行工具“文档与行为一致”的期望落空。CLI 需加强幂等校验或错误回显。
- **多智能体体验（#6925）**：用户认为协作对话每次新建会话并需手动切换智能体查看是“不知道为什么”的割裂体验，协作信息架构需要重新设计。
- **UI 边缘问题（#7084、#7051）**：历史会话单条时无法点击、图片附件重载后丢失，均属高频操作中的边缘缺陷，用户遇到后明显影响使用信心。

## 待处理积压

- **[PR #6302](https://github.com/agentscope-ai/QwenPaw/pull/6302)**（7 月 21 日创建，近 4 周）：统一 provider 发现、模型元数据、路由与智能体控制的大型重构，长期未合并。涉及面广，建议维护者安排评审或明确搁置原因。
- **[PR #6515](https://github.com/agentscope-ai/QwenPaw/pull/6515)**（7 月 28 日创建，3 周）：新增火山方舟 Agent Plan 与小米 MiMo V2.5 内置提供商，功能独立、风险可控，但持续未合并。
- **[PR #6719](https://github.com/agentscope-ai/QwenPaw/pull/6719)**（8 月 5 日创建，近 2 周）：WorkBuddy 风格的工作区工件卡片，已实现持久化 manifest，等待评审。
- **[Issue #7011](https://github.com/agentscope-ai/QwenPaw/issues/7011)**（8 月 14 日创建）：多 UI 会话身份交叉问题持续更新，尚未有 maintainer 介入回复，建议至少标注计划版本。
- **[PR #6986](https://github.com/agentscope-ai/QwenPaw/pull/6986)**（8 月 13 日创建）：沙箱杀软拦截修复，但描述仍为模板占位，需作者补充具体场景与安全考量后再进入评审。
- **[PR #5151](https://github.com/agentscope-ai/QwenPaw/pull/5151)**（6 月 12 日创建，今日合并）：GitPanel 样式修复从创建到合并历时 2 个月，建议复盘是否存在评审阻塞或贡献者沟通问题，以优化首次贡献者体验。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目动态日报 — 2026-08-18

## 1. 今日速览

过去 24 小时 ZeroClaw 保持高活跃度：50 条 Issue 更新（其中 43 条新开/活跃、7 条关闭），50 条 PR 更新（16 条已合并/关闭、34 条待合并），无新版本发布。今日主线是**安全与稳定性加固**——5 个安全类 PR 完成合并，包括 Gemini API Key 泄露、附件下载无限制、action 预算非原子扣减等 P1 级问题的修复。社区讨论热度集中在大型 RFC 上（3 条 Issue 评论超过 20），话题聚焦于 OpenAI 兼容 API、Goal mode、shell 命令策略等 v0.9.0 架构方向。目前项目处于 v0.9.0 开发周期的高度活跃期，但 34 条 PR 积压待合并，维护者评审速度值得关注。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

今日 16 条 PR 合并/关闭，按主题可归纳为三条推进线：

**安全加固（今日重点）**

- [PR #9973](https://github.com/zeroclaw-labs/zeroclaw/pull/9973) `fix(providers): keep Gemini API keys out of URLs` — Gemini API Key 不再出现在生成和 warmup 请求 URL 中，改由 `x-goog-api-key` 头传递并标记为敏感值，消除 URL 日志泄露风险。
- [PR #10000](https://github.com/zeroclaw-labs/zeroclaw/pull/10000) `fix(channels): bound QQ and Mattermost downloads` — 引入共享的有界 HTTP 响应读取器，为 QQ 和 Mattermost 的附件下载设置大小上限（QQ 复用 10 MiB、Mattermost 强制 25 MiB），防止 Content-Length 缺失时无限制下载。
- [PR #9993](https://github.com/zeroclaw-labs/zeroclaw/pull/9993) `fix(email): stop implicit attachment file reads` — 出站 Email 附件现在仅从 `MediaAttachment.data` 构造，空 payload 不再可能触发隐式本地文件读取。
- [PR #9612](https://github.com/zeroclaw-labs/zeroclaw/pull/9612) `fix(channels): tie the WhatsApp Cloud approval token to a guard so no exit orphans it` — 修复 `request_approval` 在进程全局 `PENDING_APPROVALS` 注册 token 后两条退出路径导致 token 孤儿化的问题。
- [PR #9996](https://github.com/zeroclaw-labs/zeroclaw/pull/9996) `fix(security): make action budget accounting atomic` — 在工具进入副作用边界前原子性地预留 sender-scoped 预算容量，解决并行工具调用可共同超出 `max_actions_per_hour` 的竞态；同时修复 [#9594](https://github.com/zeroclaw-labs/zeroclaw/issues/9594)（coding-agent 工具重复计费）和 [#9849](https://github.com/zeroclaw-labs/zeroclaw/issues/9849)（check-before/record-after 非原子检查）。

**稳定性与正确性修复**

- [PR #9544](https://github.com/zeroclaw-labs/zeroclaw/pull/9544) `fix(delegate): honor configured provider fallbacks` — 委托目标改用 canonical session provider builder 构建，使 delegate 正确使用配置的 aliases、routes、retries 和 fallback 候选，而非绕过它们直接使用 raw primary alias。
- [PR #9765](https://github.com/zeroclaw-labs/zeroclaw/pull/9765) `fix(sop): load SOP definitions from the shared workspace, not data_dir` — 修复 `build_sop_engine` 将 workspace_dir 同时用于 durable run store（`<data_dir>/sop/runs.db`）和 SOP 定义加载导致的两个无关目录角色混淆。
- [PR #10010](https://github.com/zeroclaw-labs/zeroclaw/pull/10010) `test(cron): avoid ETXTBSY race in custom shell test` — 用指向 PATH 解析 `sh` 的每测试 symlink 替换运行期写入的 executable 脚本，消除并发 fork 子进程持有写入描述符导致的 ETXTBSY 调度窗口。

**CI 与基础设施**

- [PR #9398](https://github.com/zeroclaw-labs/zeroclaw/pull/9398) `ci(tests): add scheduled macOS and Windows tests` — 新增 nightly 的 macOS/Windows 计划测试工作流，首次为 [#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) 的 Windows 测试失败提供持续可见性。
- [PR #10039](https://github.com/zeroclaw-labs/zeroclaw/pull/10039) `ci(clippy): share Clippy command runner across workflows` — 提取重复的 Linux/macOS/Windows Clippy 命令与诊断逻辑到 `scripts/ci/run_clippy.sh`，防止工作流间漂移。
- [PR #10043](https://github.com/zeroclaw-labs/zeroclaw/pull/10043) `ci(lint): remove duplicate architecture test guards` — 移除 Lint 中重复的 config-write 隔离和 Fluent coverage 架构测试（Test 工作流已在跑）。
- [PR #9547](https://github.com/zeroclaw-labs/zeroclaw/pull/9547) `chore(channels): upgrade CPAL to 0.18` — Voice Wake 迁移到 CPAL 0.18 统一 API，默认输入采样率选取逻辑更健壮。

**整体评估**：v0.9.0 安全架构相关的底层修复正在密集落地，尤其是 budget 原子化、密钥隔离、下载边界这三项直接影响生产安全；CI 跨平台覆盖也补上了 Windows/macOS 的空白。但 34 条 PR 待合并、其中多条 P1/P2 级安全修复，合并队列存在一定积压。

## 4. 社区热点

今日最活跃的讨论集中在 v0.9.0 的架构类 RFC 上，评论区超过 15 条的有 8 条：

- [Issue #6808](https://github.com/zeroclaw-labs/zeroclaw/issues/6808)（23 评论）— RFC: Work Lanes, Board Automation, and Label Cleanup。已进入 Rev.26，状态 Ratified / rollout in progress。核心诉求是让工作路由自动化，减少维护者手动维护额外系统的负担。
- [Issue #8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)（23 评论）— RFC: ZeroClaw Chat Completions profile。呼声很高：Open WebUI、LobeChat、Continue.dev、Aider、LangChain、OpenAI SDK 等客户端无法连接 ZeroClaw，社区对 OpenAI 协议兼容有明确需求。
- [Issue #8303](https://github.com/zeroclaw-labs/zeroclaw/issues/8303)（22 评论）— RFC: Goal mode v1。讨论如何在不引入过重机制的前提下，让 ZeroClaw 能跨多个 agent turn 持久地追踪一个有界用户目标。
- [Issue #7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)（20 评论）— RFC: 高风险 shell 命令的分级确认 + Claude Code 风格命令策略（allow/ask/deny）。安全与易用性之间的平衡是讨论焦点。
- [Issue #9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)（19 评论）— RFC: Runtime-owned conversation sessions and transport surface adapters。探讨将所有入口迁移到 `InboundAction` 提交、运行时持有会话的边界问题。
- [Issue #9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488)（18 评论）— RFC: Unified attachment architecture for web chat and channels。统一各渠道附件处理。
- [Issue #7141](https://github.com/zeroclaw-labs/zeroclaw/issues/7141)（16 评论）— RFC: Pluggable inbound authentication and canonical principals，已进入 Rev 8。
- [Issue #7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462)（16 评论）— Windows 74 个测试失败详情，社区对跨平台质量保障高度关注。

**社区诉求分析**：热度最高的几个讨论指向三类需求——① 希望融入既有 AI 工具生态（OpenAI 协议兼容）；② 希望安全策略可配置、可解释、不过度阻断（shell 策略、认证、会话边界）；③ 希望项目治理和 RFC 流程本身更轻量（[#9496](https://github.com/zeroclaw-labs/zeroclaw/issues/9496) 明确抱怨 RFC 流程比决策本身更慢）。

## 5. Bug 与稳定性

按严重程度排列今日活跃的 Bug：

**P1 / 高风险**

- [Issue #7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) — Windows 11（简体中文 + CP936）下 74 个测试失败，涉及 Unix-only 命令、路径语义、控制台编码。**已有缓解**：[PR #9398](https://github.com/zeroclaw-labs/zeroclaw/pull/9398) 新增 nightly macOS/Windows 测试工作流，但测试本身尚未修复，CI 的 Linux-only 限制仍未完全消除。
- [Issue #9973 对应修复已合并](https://github.com/zeroclaw-labs/zeroclaw/pull/9973) — Gemini API Key 出现在 URL 中，存在泄露风险，**已修复**。
- [Issue #10000 对应修复已合并](https://github.com/zeroclaw-labs/zeroclaw/pull/10000) — QQ/Mattermost 附件下载无大小限制，**已修复**。
- [Issue #9612 对应修复已合并](https://github.com/zeroclaw-labs/zeroclaw/pull/9612) — WhatsApp Cloud approval token 退出路径孤儿化，**已修复**。

**P2 / 中风险**

- [Issue #9594](https://github.com/zeroclaw-labs/zeroclaw/issues/9594) — coding-agent 工具一次成功调用扣两次 action 预算，**已关闭**，由 [#9996](https://github.com/zeroclaw-labs/zeroclaw/pull/9996) 修复。
- [Issue #9849](https://github.com/zeroclaw-labs/zeroclaw/issues/9849) — `RateLimitedTool` 预算检查非原子（先检查、成功后记录），并行调度可突破限额，**已关闭**，由 [#9996](https://github.com/zeroclaw-labs/zeroclaw/pull/9996) 修复。
- [Issue #10023](https://github.com/zeroclaw-labs/zeroclaw/issues/10023) — 固定 fallback 模型服务时，重试和 cooldown 失败日志记录的是请求模型而非实际服务的 pinned 模型，误导排障。**暂无 fix PR**。
- [Issue #10038 对应 PR](https://github.com/zeroclaw-labs/zeroclaw/pull/10038) — `POST /api/cron` 对 `session_target` 的非 `isolated`/`main` 值（如 `shared`、`mian`、空串）仍返回 200 并持久化。FIX PR 已提交，待合并（当前标记 `needs-author-action`）。

**测试稳定性**

- [PR #10010](https://github.com/zeroclaw-labs/zeroclaw/pull/10010) 修复了 cron 自定义 shell 测试的 ETXTBSY 竞态，**已合并**。

## 6. 功能请求与路线图信号

多个 accepted 状态的 RFC 是 v0.9.0 路线图的直接信号：

**已 accepted、预计进入 v0.9.0 的功能方向**

- OpenAI Chat Completions 兼容层 — [#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)：为 WebSocket/ACP/webhook 之外的 HTTP 客户端打开入口，是 ZeroClaw 融入现有 AI 工具生态的关键一步。
- Goal mode v1（有界前台 Matrix 工作）— [#8303](https://github.com/zeroclaw-labs/zeroclaw/issues/8303)：跨多轮 agent turn 的持久目标执行模式。
- 高风险 shell 命令分级确认策略 — [#7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)：allow/ask/deny 三档策略 + per-execution 确认。
- 可插拔入站认证与 canonical principals — [#7141](https://github.com/zeroclaw-labs/zeroclaw/issues/7141)：已到 Rev 8，目标 Identity & Access milestone。
- 运行时自有安全决策管线 + restrictive overlays — [#7142](https://github.com/zeroclaw-labs/zeroclaw/issues/7142)：目标 v0.9.0 安全架构。
- 运行时自有会话与传输适配器 — [#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)、统一附件架构 — [#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488)：重构网关入口边界。
- 统一 package/capability/config/runtime-state catalog 契约 — [#9346](https://github.com/zeroclaw-labs/zeroclaw/issues/9346)。
- 分阶段 opt-in 产品遥测 — [#9621](https://github.com/zeroclaw-labs/zeroclaw/issues/9621)。
- SOP capability 权限契约 — [#9598](https://github.com/zeroclaw-labs/zeroclaw/issues/9598)。

**治理流程信号**

- [Issue #9496](https://github.com/zeroclaw-labs/zeroclaw/issues/9496) — RFC 流程简化已 accepted，方向是缩短 7 天讨论期、降低全票一致要求、减少手动投票协调。该条如落地将直接影响后续所有 RFC 的推进速度。

**在途功能类 PR**

- [PR #9109](https://github.com/zeroclaw-labs/zeroclaw/pull/9109) — 原生 Hailo-Ollama provider 支持（open，`needs-author-action`）。
- [PR #10065](https://github.com/zeroclaw-labs/zeroclaw/pull/10065) — ZeroCode 文件资源管理器搜索模式支持 Up/Down 键移动高亮（open，新提交）。
- [PR #10021](https://github.com/zeroclaw-labs/zeroclaw/pull/10021) — 将目标 runtime profile 的 thinking 策略应用到 agentic independent delegates（open）。

## 7. 用户反馈摘要

从今日活跃 Issue 与评论中可提炼以下用户声音：

- **跨平台质量是真实痛点**：[#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) 明确指出 CI 只在 Linux 上跑测试导致 Windows 上的 74 个失败长期未被发现，且描述细节（简体中文、代码页 936）表明这影响到非英语环境的实际用户。
- **日志可诊断性不足**：[#10023](https://github.com/zeroclaw-labs/zeroclaw/issues/10023) 中用户反馈，当 pinned fallback 模型与请求模型不同时，失败日志记录的是请求模型而非实际服务的模型，"a gemini fallback entry pinned to gemini-3-flash show..." —— 这类误导性日志会显著拖慢生产排障。
- **对生态兼容的强烈渴望**：[#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) 一口气列举了 Open WebUI、LobeChat、Continue.dev、Aider、LangChain、OpenAI SDK 等众多客户端，说明用户希望 ZeroClaw 能直接接入已有 AI 工具链，而不是要求用户学习新协议。
- **RFC 流程负担的抱怨**：[#9496](https://github.com/zeroclaw-labs/zeroclaw/issues/9496) 摘要原话为 "ZeroClaw's RFC process has become slower and more cumbersome than the decisions it is meant to support"，这是维护者自身对治理效率的反思。
- **核心膨胀担忧**：[#6165](https://github.com/zeroclaw-labs/zeroclaw/issues/6165) 指出积攒的集成"vary widely in how central they are"，建议通过外部集成方式让核心更轻。
- **维护者手动负担**：[#6808](https://github.com/zeroclaw-labs/zeroclaw/issues/6808) 摘要中 "without making maintainers keep another manual system" 直观反映了对工作流自动化的需求。

## 8. 待处理积压

需要维护者关注的长期未决项目：

**等待维护者评审的关键 RFC（`needs-maintainer-review`）**

- [Issue #9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) — Runtime 会话与传输适配器（19 评论，已 open 21 天）
- [Issue #9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488) — 统一附件架构（18 评论，已 open 21 天）
- [Issue #9598](https://github.com/zeroclaw-labs/zeroclaw/issues/9598) — SOP 权限契约（已 open 18 天）
- [Issue #6165](https://github.com/zeroclaw-labs/zeroclaw/issues/6165) — 核心瘦身/外部集成（15 评论，已 open 超过 3 个月）

**长期未合并的 PR**

- [PR #9808](https://github.com/zeroclaw-labs/zeroclaw/pull/9808) — rust-all 组 46 项依赖批量更新，size:L、risk:high，已 open 11 天，建议尽快拆分或安排评审。
- [PR #9314](https://github.com/zeroclaw-labs/zeroclaw/pull/9314) — Telegram 长轮询 offset 仅在投递/永久跳过之后推进（P1、size:XL），已 open 26 天未合并，该修复关系到 Telegram 消息不丢失。
- [PR #9109](https://github.com/zeroclaw-labs/zeroclaw/pull/9109) — Hailo-Ollama 支持，`needs-author-action`，需要作者回应。
- [PR #9056](https://github.com/zeroclaw-labs/zeroclaw/pull/9056) — provider 失败诊断改进，`stale-candidate` + `needs-author-action`，已接近陈旧状态。

**被接受的 RFC 尚无实现跟踪**

[#7100](https://github.com/zeroclaw-labs/zeroclaw/issues/7100)（per-model capability/context-window 配置）、[#7897](https://github.com/zeroclaw-labs/zeroclaw/issues/7897)（免 reload 应用安全配置）、[#9621](https://github.com/zeroclaw-labs/zeroclaw/issues/9621)（产品遥测）等均已 accepted 但暂无对应 PR，[#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) 维护者决策队列是跟踪这些事项的入口。

---

**健康度小结**：ZeroClaw 当前处于 v0.9.0 冲刺前的高强度开发期——安全修复密集且质量高、CI 覆盖在补课、大型架构 RFC 讨论充分。需要注意的风险点是：① PR 合并队列积压较多（34 条 open），P1 级 Telegram 修复搁置近 4 周；② 多个 RFC 处于 accepted 状态但实现推进缓慢；③ Windows 测试修复本身尚无对应 PR，跨平台问题的根治仍需投入。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*