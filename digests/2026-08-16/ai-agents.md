# OpenClaw 生态日报 2026-08-16

> Issues: 500 | PRs: 500 | 覆盖项目: 12 个 | 生成时间: 2026-08-15 23:14 UTC

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

# OpenClaw 项目动态日报 — 2026-08-16

## 1. 今日速览

过去24小时内，OpenClaw 社区活跃度极高：共产生 500 条 Issue 更新（96% 为新开或活跃）及 500 条 PR 更新（其中 448 条待合并），同时发布了 `v2026.8.1-beta.2` 新版本。Bug 报告集中在 DeepSeek/Claude/Codex 等多模型后端的消息处理与会话状态问题，社区对错误诊断与模型故障转移机制的改进呼声强烈。值得关注的是，已有多个修复 PR（如 memory_search 死锁、Telegram 富文本消息等）处于待合并状态，说明维护者正在大量吸收社区反馈并推进修复。


## 2. 版本发布

### v2026.8.1-beta.2（2026-08-16 发布）

**更新亮点：**

- **Secret egress host binding（安全加固）**：将每个共享存储 secret 绑定到精确的 HTTPS 目标主机，覆盖 CLI、Gateway RPC 和 Control UI 三条路径。未绑定的 sentinel 替换将在明文出口前以 fail-closed 方式失败。感谢 @shakkernerd 的贡献。
- **GPT-5.6 Ultra 支持与运行时切换**：新增对 GPT-5.6 Ultra 的支持，并优化运行时切换能力。

**注意：** 该版本为 beta 版本，生产环境升级前建议先在测试环境验证 Secret 绑定策略是否影响现有工作流（特别是依赖多个自定义域名的 Secret 引用场景）。

🔗 [查看 Release 详情](https://github.com/openclaw/openclaw/releases)


## 3. 项目进展

今日有两项重要 PR 被合并关闭，均来自 @jesse-merhi 的安全相关功能：

1. **安装策略警告确认机制（核心 CLI）** — PR #116489（已关闭，XL，安全边界风险）
   - 新增外部 `security.installPolicy` 命令返回 `warn` 状态的能力，允许授权操作员在安装可疑插件或技能前审查原因和发现，并可选择继续或中止安装。
   - 交互式 CLI 安装会显示有界的原因信息和发现项，并要求输入确切目标名称以确认继续。

2. **安装策略警告审查 UI（Control UI）** — PR #120900（已关闭，XL，安全边界风险）
   - 已认证管理员可在 Control UI 中审查安装策略警告，并选择继续该插件安装。
   - `plugins.install` 接受可选字面量 `acknowledgeInstallPolicyWarning: true`，该布尔值仅作为该次安装调用的确认，不改变全局策略。

这两项 PR 构成了完整的安全安装流程（CLI + UI），显著增强了第三方插件/技能安装环节的风险控制能力。

另有多个值得关注的待合并 PR（详见 Bug 与稳定性部分）。

🔗 [PR #116489](https://github.com/openclaw/openclaw/pull/116489) | [PR #120900](https://github.com/openclaw/openclaw/pull/120900)


## 4. 社区热点

### 今日最受关注 Issue TOP 5

1. **#121953** — [P1] Cron agent 在 DeepSeek 上停滞（20 条评论）
   - 问题：OpenClaw 给 cron agent 每条用户消息添加 `[cron:<jobId> <jobName>]` 前缀，DeepSeek API 边缘节点会将以此前缀开头的请求调度到低优先级队列，导致任务停滞数十秒至数分钟。
   - 诉求：降低该前缀对 DeepSeek 请求优先级的影响，或提供配置项禁用此前缀。
   - 🔗 https://github.com/openclaw/openclaw/issues/121953

2. **#91009** — [P1] Codex PreToolUse native hook relay 产生 CPU 密集进程并阻塞 Gateway RPC（20 条评论）
   - 问题：`openclaw-hooks relay --provider codex --event pre_tool_use` 衍生多个进程，每个占用 ~100%+ CPU，导致 Gateway RPC 停滞。
   - 状态：已标记 `needs-live-repro`、`needs-maintainer-review`，尚无修复 PR。
   - 🔗 https://github.com/openclaw/openclaw/issues/91009

3. **#79902** — [P3] 添加 companion-friendly SQLite transcript/session 接口（13 条评论）
   - 诉求：在 database-first 运行时之上提供 SQLite transcript/session 查询层，让高级用户无需解析不透明 blob 就能访问规范化的运行时状态。
   - 🔗 https://github.com/openclaw/openclaw/issues/79902

4. **#69208** — [P1] Umbrella：跨渠道 transcript、replay 和 context assembly 重复问题（13 条评论）
   - 维护者已确认这是一个跨 MSTeams、webchat、Telegram、followup queue 等多个路径的通用问题类别，正在系统性归集。
   - 🔗 https://github.com/openclaw/openclaw/issues/69208

5. **#51429** — [P2] 工作路径被硬编码进代码并发布（13 条评论）
   - 用户报告 OpenClaw 创建了 `/Users/wangtao` 文件夹并将工作区设置为该目录，疑似有开发者的本地路径被硬编码进发布版本。
   - 该 Issue 有 13 条评论，用户情绪可见一斑。
   - 🔗 https://github.com/openclaw/openclaw/issues/51429

**分析**：社区热点集中在两类诉求——多模型后端兼容性（DeepSeek/Claude/Codex）和运行时状态可观测性。前者反映了 OpenClaw 作为多模型网关的定位正在被广泛使用，不同供应商的 API 行为差异成为实际痛点；后者则说明高级用户对底层数据访问和二次开发的需求在上升。


## 5. Bug 与稳定性

### P0 / 高影响

1. **#70903** — [P0] 持久化 provider cooldown 导致用户在账单恢复后仍被阻塞数小时
   - 问题：402 计费错误后，`disabledUntil` 时间戳持久化到 auth-state 文件且跨 Gateway 重启保留，用户充值后仍需等待。
   - 状态：已标记 `stale`，无修复 PR，影响面大。
   - 🔗 https://github.com/openclaw/openclaw/issues/70903

### P1 严重问题

| Issue | 问题 | 状态 | 链接 |
|-------|------|------|------|
| #121953 | Cron agent 在 DeepSeek 上因前缀被低优先级处理而停滞 | 无 fix PR，待产品决策 | 🔗 https://github.com/openclaw/openclaw/issues/121953 |
| #91009 | Codex PreToolUse hook relay 产生 CPU 密集进程阻塞 gateway | 需 live repro，无 fix PR | 🔗 https://github.com/openclaw/openclaw/issues/91009 |
| #38327 | GPT-5.1-pro-preview 报 "Cannot convert undefined or null to object"（2026.3.2 回归） | 需 maintainer review | 🔗 https://github.com/openclaw/openclaw/issues/38327 |
| #123799 | 生产环境受 Codex compact 404 影响，需安全升级/回退指导 | 新开，7 条评论 | 🔗 https://github.com/openclaw/openclaw/issues/123799 |
| #119087 | Gateway 冷启动在 1-vCPU 容器上回归约 2.5 倍 | 有 linked PR #124303/#124267 | 🔗 https://github.com/openclaw/openclaw/issues/119087 |
| #103231 | `claude-cli` 后端 native compaction 假设不成立，会话增长超 200% 且恢复路径静默失败 | 需 live repro，无 fix PR | 🔗 https://github.com/openclaw/openclaw/issues/103231 |
| #86214 | Codex app-server 在 image/tool 请求 + 大 logs_2.sqlite 时中途断开 | 需 maintainer review | 🔗 https://github.com/openclaw/openclaw/issues/86214 |
| #94939 | 6.x 状态迁移导致 channel conversation-store SQLite 为 0 字节，MS Teams 主动发送失败 | 有 linked PR | 🔗 https://github.com/openclaw/openclaw/issues/94939 |
| #123073 | dev channel 更新失败：EUNSUPPORTEDPROTOCOL on workspace:*（npm vs pnpm） | 有 fix-shape-clear，待认领 | 🔗 https://github.com/openclaw/openclaw/issues/123073 |
| #43374 | 多 agent 并发时所有 LLM API 调用同时超时（内部瓶颈非 provider 问题） | 需 maintainer review | 🔗 https://github.com/openclaw/openclaw/issues/43374 |
| #118793 | Claude CLI session limit 错误不触发 fallback 链 | 有 linked PR | 🔗 https://github.com/openclaw/openclaw/issues/118793 |
| #84662 | Codex app-server 将每轮 runtime context 存入 native history，导致 input 膨胀 | 需 maintainer review | 🔗 https://github.com/openclaw/openclaw/issues/84662 |

### 已有修复 PR 的 Bug（值得关注）

- **#121043**（memory_search 全索引重建）→ PR #121044 已就绪待合并，Platinum hermit 评分
- **#121759**（Codex 在 supervision disabled 时 catalog continuation）→ PR #121760 待合并（L 大 PR）
- **#54409**（飞书快速连发消息不合并）→ PR #124214 待合并（XL 大 PR）
- **#124125**（Windows 上 cron 全部失效）→ PR #124293 待审核（缺证明）
- **#123886**（Telegram richMessages 下 /models 确认编辑混乱）→ PR #124222 待审核（缺证明）
- **#119350**（memory-core 重复追加 transcript）→ PR #119367 由 clawsweeper[bot] 自动生成修复

🔗 [查看全部 Open Issues](https://github.com/openclaw/openclaw/issues) | [查看全部 Open PRs](https://github.com/openclaw/openclaw/pulls)


## 6. 功能请求与路线图信号

### 高讨论度功能请求

1. **动态模型发现（OpenRouter 等）** — #10687（10 条评论，👍 3）
   - 现状模型目录是静态生成的（pi-ai/models.generated），对 OpenRouter 这类快速更新的提供商不友好。
   - 🔗 https://github.com/openclaw/openclaw/issues/10687

2. **模型 fallback 测试命令 `/models test-fallback`** — #6599（12 条评论，👍 1）
   - 用户希望在真实故障前验证 fallback 链配置是否正确。
   - 🔗 https://github.com/openclaw/openclaw/issues/6599

3. **Per-model 使用量/成本日志** — #13219（8 条评论，👍 1）
   - 当前仅 session JSONL 中有 usage 数据，无聚合视图。
   - 🔗 https://github.com/openclaw/openclaw/issues/13219

4. **内置 pace-aware 速率限制** — #45771（7 条评论，👍 2）
   - 自主循环 agent 容易耗尽 API 配额，需要内置的消耗速率感知机制。
   - 🔗 https://github.com/openclaw/openclaw/issues/45771

5. **Companion-friendly SQLite transcript/session 接口** — #79902（13 条评论，👍 2）
   - 详见社区热点部分。

### 路线图信号

- **Google Vertex/Gemini**：v2026.8.1-beta.2 已支持 GPT-5.6 Ultra，模型更新节奏快。与此对应，**Amazon Bedrock 工具参数解析性能优化** PR #120248（XL）正在等待作者更新，说明多云模型支持仍是重点。
- **Telegram rich messages**：PR #124222 修复 `/models` 富文本确认渲染问题，说明 Telegram 富消息功能仍在迭代中。
- **Control UI**：多个 Web UI 重构 PR（#124301、#123874）在途，界面侧重点在统一侧边栏和 composer 多行化。
- **安全加固**：install policy 警告确认机制（CLI+UI）已合并，安全工作仍在持续。


## 7. 用户反馈摘要

以下从 Issues 评论中提炼真实用户声音：

1. **对硬编码路径的强烈不满**（#51429）：“今天刚安装的最新版，结果 OpenClaw 建了个 /Users/wangtao 的文件夹并把工作区设成了这个目录。这位 wangtao 是谁？”——用户对发布质量产生质疑，该问题 13 条评论，情绪明显。

2. **对稳定性标签的期望**（#73537）：“我们已经在生产环境中将它作为家庭和商务助手使用（Telegram 集成、自动化、cron 任务、Home Assistant 控制），它已成为我们日常工作流的一部分。真的非常感谢你和团队的付出。**问题是……**”——用户表达感谢的同时，请求为发布版添加生产就绪稳定性标签（production-readiness stability label）。

3. **工具调用失败导致的消息刷屏困扰**（#55694）：“Agent 陷入工具调用失败死循环，每次重试前都发送消息，导致用户收到大量重复消息刷屏。在飞书对话中，`exec` 工具参数错误后，Agent 连续重试 20+ 次，用户收到了 6+ 条几乎相同的消息。”

4. **对无提示静默失败的沮丧**（#119401）：“`agents.defaults.silentReply` 无法在直接/DM 聊天中强制可见回复。即使策略已配置，字面量 `NO_REPLY` 完成也会在投递前被无条件剥离。”

5. **对 sub-agent 超时丢失工作的担忧**（#6625）：“当 sub-agent 达到 `runTimeoutSeconds` 时，它会被立即终止，没有任何保存进度的机会。**所有未保存的工作都会丢失**——代码、研究、分析、生成的内容，什么都留不下。”

6. **对升级破坏性的无奈**（#90378）：“从 2026.5.28 升级到 2026.6.1 时，cron store 静默迁移到 SQLite，但新作业默认使用 `delivery.mode=announce` 导致渠道错误。迁移过程对用户不可见。”

7. **对快速修复的认可**：多个 PR 标记为 "AI-assisted (Claude/Codex, via OpenClaw)"，社区对 AI 辅助开发流程的接受度较高，同时也体现了 OpenClaw 的 dogfooding 文化。


## 8. 待处理积压

### 长期未响应的关键 Issue

| Issue | 创建时间 | 天数 | 问题 | 当前状态 |
|-------|---------|------|------|---------|
| #70903 | 2026-04-24 | 113 天 | [P0] 持久化 provider cooldown 阻塞用户 | 已 stale，无 fix PR |
| #6599 | 2026-02-01 | 196 天 | [P3] /models test-fallback 命令 | 需 maintainer review |
| #6625 | 2026-02-01 | 196 天 | 优雅 sub-agent 超时警告 | 需 maintainer review |
| #10687 | 2026-02-06 | 191 天 | 动态模型发现 | 需 maintainer review + product decision |
| #38327 | 2026-03-06 | 162 天 | GPT-5.1 pro preview 报错（P1 回归） | 需 maintainer review + live repro |
| #56653 | 2026-03-28 | 140 天 | Slack reaction 事件不投递 | 需 maintainer review + info |
| #43374 | 2026-03-11 | 157 天 | 多 agent 并发 LLM 超时 | 需 maintainer review |
| #51429 | 2026-03-21 | 147 天 | 硬编码路径被合并 | 需 maintainer review + product decision |

### 重要 PR 等待合并

| PR | 创建时间 | 天数 | 内容 | 状态 |
|----|---------|------|------|------|
| #112811 | 2026-07-23 | 23 天 | MSTeams 多 bot 账号支持（XL，compat 风险） | waiting on author |
| #121760 | 2026-08-10 | 5 天 | Codex 在 supervision disabled 时 catalog continuation（L） | waiting on author |
| #124214 | 2026-08-15 | <1 天 | 飞书 per-chat queue lane 修复（XL） | waiting on author |
| #119367 | 2026-08-04 | 11 天 | memory-core resume validated transcript appends | waiting on author |
| #120248 | 2026-08-07 | 8 天 | Amazon Bedrock O(n²) 参数解析优化（XL） | waiting on author |
| #123874 | 2026-08-14 | 1 天 | Control UI 统一侧边栏 tab（XL） | waiting on author |
| #117328 | 2026-08-01 | 14 天 | context assembly 失败时保留历史（session-state 风险） | waiting on author |
| #124293 | 2026-08-15 | <1 天 | Windows cron 全部失效修复（P1，缺证明） | needs proof |

### 维护者提醒

- **#70903（P0）** 已 stale 且 113 天无进展，涉及计费恢复后不可用的严重用户体验问题，建议优先排查。
- **#38327（P1 回归）** 162 天未解决，涉及 Google Vertex/Gemini 核心链路，影响面可能较大。
- **#123073**（dev 渠道更新失败，EUNSUPPORTEDPROTOCOL）修复形状已明确（fix-shape-clear），可标记为 queueable-fix 供社区认领。
- **#51429**（硬编码路径）虽然已 147 天，但因涉及发布质量信任问题，建议至少给一个公开说明。

---

*日报生成时间：2026-08-16 | 数据窗口：过去 24 小时 | 数据来源：[github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)*

---

## 横向生态对比

# 个人 AI 助手 / 自主智能体开源生态横向分析报告

**报告日期：2026-08-16** | **数据窗口：过去 24 小时**


## 1. 生态全景

个人 AI 助手开源生态正处于**从“可用”向“可信”过渡的关键阶段**。以 OpenClaw 为龙头（单日 500 Issue + 500 PR 动态），生态呈现“一超多强”格局——ZeroClaw、Hermes Agent、IronClaw 等第二梯队项目在架构治理与性能工程上各有突破。全行业的核心精力正在从“堆功能”转向解决多模型供应商兼容性、会话/记忆持久化、安全边界与可观测性等生产级问题；同时，OpenAI Chat Completions 兼容协议（ZeroClaw #8603）等呼声表明生态正在向“平台化互操作”方向演进。整体而言，社区活跃度分层明显，头部项目通过快速响应与架构升级持续巩固领先地位，尾部项目活跃度低迷。

## 2. 各项目活跃度对比

| 项目 | Issues 更新 | PR 更新 | 合并/关闭率 | Release | 健康度评估 |
|------|------------|---------|------------|---------|-----------|
| **OpenClaw** | 500（96% 活跃） | 500（448 待合并） | 待合并 89.6% | ✅ v2026.8.1-beta.2 | ★★★★☆ 高活跃；修复响应快，但 P0 积压（如 #70903 停滞 113 天） |
| **ZeroClaw** | 50（46 活跃，4 关闭） | 50（44 待合并，6 合并） | 合并 12% | 无 | ★★★★☆ 高活跃；30 余条 PR 含 3 个安全高危项待审，RFC 决策积压严重 |
| **Hermes Agent** | 50（42 活跃，8 关闭） | 50（47 待合并，3 合并） | 合并 6% | 无 | ★★★★☆ 架构治理里程碑（Epic 20/20）落地；但 47 条待合并 PR 积压，P1 Linux 桌面问题 54 天未修 |
| **NanoBot** | 2（1 新增，1 关闭） | 16（7 合并/关闭，9 待合并） | 合并 44% | 无 | ★★★☆☆ 高效闭环，但 P0 PR #5271 卡冲突 10 天 |
| **IronClaw** | 27（6 活跃，21 关闭） | 13（6 合并，7 待合并） | 合并 46% | 无 | ★★★★☆ 闭环率高；unbound-turns 架构切换完成，但 Canary 连续 30/30 全红影响 CI 信号 |
| **NanoClaw** | 0 新增 | 22（3 关闭，19 待合并） | 合并 14% | 无 | ★★★★☆ 核心团队密集开发，路线图清晰；但单人多批量 PR 对审查带宽构成压力 |
| **CoPaw** | 10（9 活跃，1 关闭） | 11 待合并（0 合并） | 合并 0% | 无 | ★★★☆☆ 新 Bug 当日即有 fix PR，响应快；但 24 小时内零合并，评审/CI 或成瓶颈 |
| **Moltis** | 0 | 6（3 合并，3 待合并） | 合并 50% | 无 | ★★★★☆ 功能迭代节奏平稳，无用户反馈积压 |
| **LobsterAI** | 18（16 关闭，2 开放） | 6（2 关闭，4 搁置） | 合并 33% | 无 | ★★☆☆☆ 低活跃维护期；stale 清理为主，付费会员 Bug 悬置 3.2 个月 |
| **PicoClaw** | 0 | 2 待合并（均 stale） | 合并 0% | 无 | ★★☆☆☆ 低活跃；WhatsApp 通道阻断修复 PR 搁置 9 天未审 |
| **NullClaw** | 1 新增 | 1 待合并 | 合并 0% | 无 | ★★★☆☆ 低活跃但方向清晰；无积压，代理支持诉求明确 |
| **ZeptoClaw** | 0 | 0 | — | 无 | ★☆☆☆☆ 停滞 |

## 3. OpenClaw 在生态中的定位

**OpenClaw 是当前生态的绝对核心与参照基准。** 从数据看：

- **社区规模断层领先**：单日 500 Issue + 500 PR 动态，是第二梯队（ZeroClaw、Hermes 各 50 条）的 10 倍；v2026.8.1-beta.2 的发布节奏也远超其他项目。
- **技术路线：多模型网关 + agent 运行时 + 全渠道接入**。其核心优势在于对 DeepSeek/Claude/Codex/GPT-5.6 等多后端的深度适配（今日 5 个高热度 Issue 中有 3 个直接涉及模型后端行为差异），以及安全加固的持续推进（Secret egress 绑定、安装策略警告确认机制）。
- **生态定位差异**：与 ZeroClaw 的“协议平台化”（OpenAI Chat Completions 兼容 RFC）相比，OpenClaw 更倾向于“网关 + 运行时一体化”路径；与 Hermes 的“架构治理驱动”相比，OpenClaw 更依赖社区反馈驱动迭代。其短板同样明显——待合并 PR 高达 448 条，P0/P1 级 Bug 存在长期搁置（如 #70903 计费恢复阻塞 113 天），维护者审查带宽已成为最大瓶颈。

## 4. 共同关注的技术方向

| 技术方向 | 涉及项目 | 具体诉求 |
|---------|---------|---------|
| **多模型后端兼容性** | OpenClaw、Hermes、NanoClaw、ZeroClaw | DeepSeek 前缀调度致停滞（#121953）；慢速本地模型超时（Hermes #87292）；Claude 限流时心跳停摆（NanoClaw #3251）；Anthropic 拒答/回退闭环（ZeroClaw #9262-9268） |
| **会话与记忆持久化** | OpenClaw、NanoBot、Hermes、IronClaw | SQLite transcript 接口（OpenClaw #79902）；consolidation 截断致消息静默丢失（NanoBot #5377）；压缩死循环（Hermes #84371）；unbound-turns 架构切换（IronClaw） |
| **安全边界精细化** | OpenClaw、NanoBot、Hermes、ZeroClaw | Secret egress 主机绑定；插件缓存技能目录越权（NanoBot #5369）；`detect_dangerous_command` 可被包装绕过（Hermes #84551）；知识图谱/session 工具越权（ZeroClaw #9745/#9746）；webhook 审计凭证未脱敏（ZeroClaw #9995） |
| **WebUI/交互一致性** | NanoBot、Hermes、CoPaw、ZeroClaw | 生成中误显操作按钮（NanoBot #5368）；桌面陈旧 “Thinking” 状态（Hermes #50159）；聊天图片附件重载丢失（CoPaw #7051）；macOS 空白窗口（ZeroClaw #7527） |
| **渠道适配与消息可靠性** | OpenClaw、NanoClaw、Moltis、PicoClaw | Telegram 富文本；Discord 附件不可读（NanoClaw #2752）；Slack 原生任务卡片（Moltis）；WhatsApp 客户端过期（PicoClaw #3320）；飞书消息合并（OpenClaw #54409） |
| **可观测性与成本控制** | OpenClaw、Hermes、ZeroClaw、NullClaw | Per-model 用量/成本日志（OpenClaw #13219）；请求/响应成对转储（Hermes #66512）；AI-assisted PR pre-review（ZeroClaw #9330）；提示词前缀缓存拆分（NullClaw #987） |
| **代理与网络接入** | NullClaw、OpenClaw、LobsterAI | HTTP(S)/SOCKS5 代理支持（NullClaw #988）；受限网络环境访问 provider |

## 5. 差异化定位分析

| 项目 | 功能侧重 | 目标用户 | 技术架构 |
|------|---------|---------|---------|
| **OpenClaw** | 多模型网关 + 全渠道 agent；安全加固 | 个人/家庭助理 + 开发者 | 单体仓库 + 多 provider 适配层 + CLI/UI 双入口；快速迭代，社区驱动 |
| **ZeroClaw** | 协议兼容 + 平台化（Chat Completions、ACP） | 想用现有 OpenAI 生态工具的开发者 | RFC 驱动的架构演进；强调设计文档与决策队列；Rust 实现 |
| **Hermes Agent** | 架构治理 + 跨平台可靠性（Win/macOS/Linux） | 追求长期可维护性的高级用户 | 大型文件分片 Epic 强制规范；桌面端 Electron + TUI；调试转储完善 |
| **IronClaw** | 性能工程 + 运行时架构转型 | 基础设施敏感型开发者 | Rust/Wasmtime；unbound-turns 模型；性能优化以“减少 journal/CAS 写入”为纲 |
| **NanoClaw** | 多渠道适配器 + 多会话管理 | Telegram/Discord 重度用户 + 多 bot 运维者 | A 系列任务编号路线图；核心团队集中提交；容器心跳/生命周期治理 |
| **NanoBot** | 轻量 + WebUI 体验优化 | 快速部署、重视界面交互的个人用户 | WebUI 优先；修复闭环率高；P0 风险控制意识强 |
| **Moltis** | 远程开发环境 + 连接器生态（日历/邮件/频道） | 远程团队/使用 Coder 的开发者 | 沙箱后端扩展 + 持久化连接器；Slack 原生卡片 |
| **CoPaw** | 技能系统 + 多渠道 agent | 技能插件开发者 | 动态技能加载（PR #7033）；DataPaw 原生应用运行时（PR #6940） |
| **LobsterAI** | OpenClaw 的网易本地化封装 | 国内用户（网易模型） | 依赖上游 OpenClaw；配置同步修复；stale 清理为主 |
| **PicoClaw / NullClaw** | 轻量级 / 边缘场景 | 资源受限环境 | 低依赖；性能优化（前缀缓存、循环卫生） |
| **ZeptoClaw** | — | — | — |

## 6. 社区热度与成熟度

- **第一梯队（快速迭代 + 社区大规模参与）**：**OpenClaw、ZeroClaw、Hermes Agent**
  — OpenClaw 以绝对体量保持生态中心地位，版本发布与安全功能（install policy 确认）双线推进。ZeroClaw 社区讨论质量高（RFC 评论密集，如 #8603 获 20 条评论），但决策吞吐不足正在消耗社区耐心。Hermes 在架构治理上树立标杆（god-file 分片 Epic 以 20/20 子任务完成关闭）， Windows/更新类 Bug 反复出现说明平台可靠性仍是短板。

- **第二梯队（质量巩固 / 功能密集交付）**：**IronClaw、NanoBot、NanoClaw、Moltis、CoPaw**
  — IronClaw 与 NanoBot 闭环率最高（合并/关闭率 44%–46%），工程纪律性强。NanoClaw 处于核心团队集中开发期（14 条 core-team PR），但需警惕“只提交不合并”导致的长时间分叉。Moltis 功能迭代健康（50% 合并率），无反馈积压。CoPaw 响应速度快（新 Bug 当日有 fix PR），但 11 条 PR 零合并，评审速度亟待提高。

- **第三梯队（低活跃 / 维护停滞）**：**LobsterAI、PicoClaw、NullClaw、ZeptoClaw**
  — 前两者处于维护期，以 stale 清理和少量 PR 为主；LobsterAI 有付费功能 Bug 长期未修（#1903，3.2 个月）与安全漏洞待确认（#1885），存在口碑风险。PicoClaw 的 WhatsApp 通道阻断修复 PR 已 stale 9 天，若持续无响应将影响用户留存。ZeptoClaw 完全停滞。

## 7. 值得关注的趋势信号

1. **多模型兼容已从“锦上添花”变成“生存刚需”。** OpenClaw 的 DeepSeek 前缀调度问题、NanoClaw 的 Claude 限流心跳停摆、ZeroClaw 的 Anthropic 拒答/回退功能栈，共同表明社区正花费大量精力处理不同模型供应商的行为差异。**对开发者的参考价值**：设计 agent 时应将 provider 差异性视为一等公民，内置故障转移与优先级感知机制，而不能假设所有模型同质。

2. **安全边界正在从“鉴权”升级到“数据流向治理”。** 零信任趋势明显——OpenClaw 的 Secret egress 主机绑定、ZeroClaw 的知识图谱/工具越权修复、NanoBot 的插件缓存技能目录重验证，都在回答同一个问题：“即使内部组件被攻破，如何防止横向移动？” 安全功能（如 ZeroClaw #9825 区块链地址误报）还需要上下文感知能力来避免误伤合法场景。

3. **从“修 Bug”到“修测试”的转变。** IronClaw Canary 连续 30/30 全红（根因是 harness 缺陷，非产品缺陷）、ZeroClaw 测试基础设施因 ETXTBSY 导致无关 PR 误标红、Hermes 自动化探针持续失败——头部项目正集体意识到，不稳定的 CI 信号已成为开发效率的最大隐性杀手。修复测试框架本身比再修一个 Bug 更有杠杆。

4. **“平台化协议”呼声渐起。** ZeroClaw 的 OpenAI Chat Completions 兼容 RFC（20 条评论）是最明确信号——用户希望现有 OpenAI 生态工具（Open WebUI、Continue.dev、Aider、LangChain）能直接接入自托管 agent 后端。这与 Moltis 的连接器持久化、OpenClaw 的运行时状态 SQLite 接口诉求同源，都指向**数据与接口的可移植性**。

5. **长时运行稳定性的专项优化成为新战场。** NullClaw 的“循环卫生”（stablePrefixHash 缓存拆分 + 工具输出压缩 + 重复调用防护）、NanoClaw 的心跳/容器生命周期治理、OpenClaw 的 cron + 多 agent 并发超时，说明 agent 从“demo 可用”走向“7×24 生产可用”的过程中，**资源泄漏与静默失败**是最需要优先解决的两类问题。

6. **AI 辅助开发与 AI 治理正在进入矛盾期。** 一方面社区认可 AI-assisted 修复 PR（OpenClaw 多个“AI-assisted” PR 被标记），另一方面 Hermes 出现 AI 生成的 skills-index 降级、ZeroClaw 的 AI pre-review 提案，说明**“AI 写代码的速度”已经超过“人类审查代码的速度”**，维护者决策队列的 throughput 正在成为生态共同瓶颈。

---

*本报告基于 2026-08-16 各项目 GitHub 社区动态数据（OpenClaw、NanoBot、Hermes Agent、PicoClaw、NanoClaw、NullClaw、IronClaw、LobsterAI、Moltis、CoPaw、ZeptoClaw、ZeroClaw）自动生成。*

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报（2026-08-16）

## 1. 今日速览

过去 24 小时 NanoBot 项目提交密度较高：共 16 条 PR 动态，其中 7 条已合并/关闭、9 条待合并；Issues 新增 1 条、关闭 1 条。今日无新版本发布，但代码合入节奏活跃，重心集中在 WebUI 体验修正、Agent/Cron 稳定性与安全边界加固。最值得关注的是数据完整性问题 #5377 在报告当天即有 PR #5379 承接修复，项目对高风险问题的响应速度很快。整体项目健康度良好：10+ 位外部贡献者活跃推进，修复类 PR 普遍携带 test/文档标签，质量门槛清晰。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

今日共 7 条 PR 状态变为 CLOSED，覆盖稳定性、安全、WebUI 与 provider 扩展四个方向。

**稳定性与安全**

- [#5369 fix(plugins): revalidate cached skill roots](https://github.com/HKUDS/nanobot/pull/5369)：修复插件包变更后缓存技能目录仍可读的安全问题，避免受限项目被绕过权限读取；已合并/关闭。
- [#5370 fix(agent): bound per-session file state lifecycle](https://github.com/HKUDS/nanobot/pull/5370)：限制 FileStateStore 无界增长，同时修复 `/new`、SDK 清理后文件状态残留的问题；已合并/关闭。
- [#5376 fix(cron): keep scheduler alive when job-store persistence fails](https://github.com/HKUDS/nanobot/pull/5376)：修复 CronService 持久化异常逃逸导致调度器静默终止的问题；已合并/关闭。

**WebUI 体验修正**

- [#5371 fix(webui): hide assistant actions until turn end](https://github.com/HKUDS/nanobot/pull/5371)：在 Agent turn 结束前隐藏复制/fork 操作，消除完成信号冲突，并关闭 Issue #5368；已合并/关闭。
- [#5397 fix(webui): preserve range selection and turn timing](https://github.com/HKUDS/nanobot/pull/5397)：支持 macOS 风格 Shift 范围选择，并保持运行中 turn 的身份与计时一致性；已合并/关闭。
- [#5399 fix(webui): clarify model preset display names](https://github.com/HKUDS/nanobot/pull/5399)：区分预设展示标签与稳定 `/model` 命令名，消除编辑预设时的命名歧义；已合并/关闭。

**Provider 扩展**

- [#5328 feat(providers): add OrcaRouter as a named gateway provider](https://github.com/HKUDS/nanobot/pull/5328)：新增 OrcaRouter 网关 provider，统一接入 150+ 模型（OpenAI、Anthropic、Google、DeepSeek 等）；已合并/关闭。

整体来看，今日合入内容以可靠性治理与 UX 打磨为主：3 条是核心链路稳定性/安全修复，3 条是 WebUI 交互一致性改进，另有一条 provider 扩展。项目在“稳定现有功能”和“扩展接入能力”上同步推进。

## 4. 社区热点

今日讨论热度最集中的是 Issue [#5377 [OPEN] Bug: consolidation truncates archive input but advances past the full message batch](https://github.com/HKUDS/nanobot/issues/5377)，这也是当前唯一带有 2 条评论的 issue。用户 dajiaohuang 报告：Consolidator 将对话截断到模型 token 预算内，但调用方仍然将 `Session.last_consolidated` 推进到完整批次之后，导致被截断的消息永远无法被归档或回顾，造成静默数据丢失。该问题当天即得到 PR [#5379 fix(memory): preserve full consolidation input](https://github.com/HKUDS/nanobot/pull/5379) 的修复——以无损分块替代截断。社区对这一问题的快速接力，反映出“对话历史不可丢失”是用户最在意的底线之一。

另一组前后端联动热点是 [#5368 WebUI: hide copy and fork actions while an Agent turn is still running](https://github.com/HKUDS/nanobot/issues/5368) 与修复 PR [#5371](https://github.com/HKUDS/nanobot/pull/5371)，描述的是 Agent 仍在生成时页面却已出现可操作按钮，造成“冲突的完成信号”，两天内完成 issue 到修复合入，社区反馈转化效率较高。

## 5. Bug 与稳定性

按严重程度降序排列：

1. **P0 / 数据覆盖风险**：[#5271 fix(session): prevent stale background task saves from overwriting session data](https://github.com/HKUDS/nanobot/pull/5271)，带 `priority: p0` 与 `conflict` 标签。该 PR 防止 `/new` 或会话生命周期替换后，过期后台任务将旧数据写回覆盖新会话，目前仍处于 OPEN 状态且存在合并冲突，需要优先处理。
2. **高 / 数据完整性**：[#5377 consolidation 截断输入但游标推进导致消息静默丢失](https://github.com/HKUDS/nanobot/issues/5377)。已有对应修复 PR [#5379](https://github.com/HKUDS/nanobot/pull/5379) 处于 OPEN，建议尽快与 issue 一并闭环。
3. **中 / 调度器稳定性**：[#5376 cron 持久化失败导致调度器静默退出](https://github.com/HKUDS/nanobot/pull/5376)。单次磁盘故障即可让 `_arm_timer()` 不再触发，修复已合并/关闭。
4. **中 / 安全边界**：[#5369 插件包变更后缓存技能目录未重新验证](https://github.com/HKUDS/nanobot/pull/5369)。存在绕过受限项目权限读取的风险，修复已合并/关闭。
5. **中 / 资源泄漏**：[#5370 FileStateStore 无限增长](https://github.com/HKUDS/nanobot/pull/5370)。高基数临时会话会导致内存/表无界膨胀，修复已合并/关闭。
6. **低 / UI 状态一致**：[#5368 生成中展示复制/fork 操作](https://github.com/HKUDS/nanobot/issues/5368)。修复 PR [#5371](https://github.com/HKUDS/nanobot/pull/5371) 已合并/关闭。

今日新增 bug 仅 1 条（#5377），且已有配套修复；其余为存量问题合入。项目 bug 修复闭环率较高，P0 是当前唯一遗留高风险项。

## 6. 功能请求与路线图信号

今日没有新增功能请求类型的 issue，但有 6 个功能性 PR 在途，构成下一版本候选方向：

**WebUI 会话协作与组织（信号最强）**

- [#5358 feat(webui): add session collaboration via mentions](https://github.com/HKUDS/nanobot/pull/5358)：为持久化会话分配稳定 `@name`，支持通过 composer 提及并跨会话协作。
- [#5364 feat(webui): add temporary side conversations](https://github.com/HKUDS/nanobot/pull/5364)：新增 `/side` 临时侧边对话，支持多标签并行，带 `conflict` 标签。
- [#5389 feat(webui): add drag-and-drop session organization](https://github.com/HKUDS/nanobot/pull/5389)：拖拽排序/分组会话，拖到另一个会话上可建组，带 `conflict` 标签。

**Provider 扩展**

- [#5398 feat(providers): add DashScope (Bailian) native protocol support](https://github.com/HKUDS/nanobot/pull/5398)：新增 DashScope 原生协议 provider，解锁比 OpenAI 兼容模式更完整的参数面。

**模型与配置规范化**

- [#5400 refactor(models): unify preset names](https://github.com/HKUDS/nanobot/pull/5400)：统一模型预设在各层的 canonical name，并支持在 WebUI 重命名。配合今日已合并的 #5399，模型预设体系正在收敛。

**连接可靠性**

- [#5401 fix(webui): make mutations reconnect-safe](https://github.com/HKUDS/nanobot/pull/5401)：重连后自动重试 pending mutations，避免请求重复执行。

若维护者计划在下一版本纳入 WebUI 协作类功能，建议优先解决 #5364 与 #5389 的冲突标记，这两个 PR 均已进入可合入但被冲突阻塞的状态。

## 7. 用户反馈摘要

今日仅有 Issue #5377 带 2 条评论（评论具体内容未在数据中给出），以下基于 issue/PR 文本提炼：

- **对历史完整性的质疑（#5377）**：用户 dajiaohuang 直接指出 consolidation 截断输入后仍然推进游标，会导致“消息永远消失”。该用户还亲自提交了修复 PR #5379，属于“发现问题—定位根因—提交修复”的深度贡献者行为。
- **对完成信号混淆的不适（#5368）**：用户在 Agent 仍在生成时看到可操作按钮，认为这是“冲突的完成信号”，对 UI 状态可信度提出了明确要求，修复已在 #5371 落地。
- **对模型预设命名的困惑（#5399 背景）**：编辑名为 `openai` 的预设却显示为 `minimax`，这种展示名与命令名错位会让用户怀疑自己改错了配置。
- **综合画像**：NanoBot 用户既关心“数据不能丢”的底线性质量，也关注 WebUI 操作反馈是否真实可信。今日两个最受关注的问题都围绕“一致性”——对话数据的一致性和界面状态的一致性。

## 8. 待处理积压

以下条目长期未合入或存在冲突，提醒维护者关注：

- [#5271 [P0, conflict] fix(session): prevent stale background task saves from overwriting session data](https://github.com/HKUDS/nanobot/pull/5271)：创建于 08-06，已 10 天，优先级最高但被冲突阻塞，数据覆盖风险持续暴露。
- [#5291 [P2] fix(agent): persist subagent conversation transcripts](https://github.com/HKUDS/nanobot/pull/5291)：创建于 08-07，已 9 天未合入。子代理完整对话记录目前无法追溯。
- [#5358 [feature] feat(webui): add session collaboration via mentions](https://github.com/HKUDS/nanobot/pull/5358)：创建于 08-12，4 天未更新，属较完整的功能 PR。
- [#5364 [feature, conflict] feat(webui): add temporary side conversations](https://github.com/HKUDS/nanobot/pull/5364)：带冲突标记，需解决后方可合入。
- [#5389 [feature, conflict] feat(webui): add drag-and-drop session organization](https://github.com/HKUDS/nanobot/pull/5389)：带冲突标记，需解决后方可合入。
- [#5379 [bugfix] fix(memory): preserve full consolidation input](https://github.com/HKUDS/nanobot/pull/5379)：与 #5377 呼应，建议在合入后关闭对应 issue，完成闭环。

**维护建议**：优先解决 #5271 的合并冲突（P0 数据风险），其次是两个带冲突的 WebUI 功能 PR（#5364、#5389），避免社区功能贡献因冲突搁置过久。

---

**总结**：NanoBot 今日处于高活跃、高修复效率状态。无新版本发布，但 7 条 PR 合入/关闭让稳定性、安全、WebUI 与 provider 四线均有推进。当前最大风险点是 P0 的 #5271 长期卡在冲突中，建议尽快处理。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目日报 — 2026-08-16

## 1. 今日速览

过去 24 小时项目保持高强度活跃：50 条 Issue 更新（42 条新开/活跃、8 条关闭）与 50 条 PR 更新（47 条待合并、3 条合并/关闭），无新版本发布。架构治理里程碑 **#78647「大型文件分解 Epic」以 20/20 子任务全部完成正式关闭**，仓库确立"所有 god-file 必须分片、不可回退"的长期策略。Windows 更新自锁（#83569）、会话序号校验（#69107）、WebSocket 鉴权启动循环（#85496）等 7 个历史 Bug 于今日关闭，同时新报告的桌面双实例杀后端（#87295）与慢速本地模型超时（#87292）**当日即有对应修复 PR（#87314、#87310）**，社区响应闭环迅速。主要隐忧是待合并 PR 积压至 47 条，且 P1 级 Linux 桌面 sandbox 问题（#51327）已开放 54 天无 fix，合并节奏与高危 Bug 处置需维护者重点关注。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

今日合并/关闭 3 个 PR，推进了以下方向：

- **[PR #87139] fix(cron): 尊重显式 Slack 频道目标** — 修复 cron 任务在线程内创建时错误继承父线程的问题，仅当目标显式包含 `:THREAD_ID` 或使用 `origin` 时才保留线程投递，并新增同频道线程内创建作业的回归测试。[链接](https://github.com/NousResearch/hermes-agent/pull/87139)
- **[PR #66512] feat: 在请求转储旁捕获模型响应（HERMES_DUMP_REQUESTS）** — 扩展调试转储能力，请求与响应成对落盘，显著提升可观测性与问题复现效率（关闭 #66530）。[链接](https://github.com/NousResearch/hermes-agent/pull/66512)
- **[PR #13746] fix: 稳定本地 Hermes UX 与供应商选择** — 削减 Telegram DM 会话 prompt 开销、稳定 NVIDIA curated catalog 选择与回退逻辑、修复 TUI 状态栏换行残影。[链接](https://github.com/NousResearch/hermes-agent/pull/13746)

今日关闭的 8 个 Issue 中，除 #78647 架构分片 Epic 完成外，[#83569](https://github.com/NousResearch/hermes-agent/issues/83569)（Windows 更新自锁 `cryptography._rust.pyd`）、[#69107](https://github.com/NousResearch/hermes-agent/issues/69107)（`truncate_before_user_ordinal` 拒绝合法序号）、[#85496](https://github.com/NousResearch/hermes-agent/issues/85496)（桌面 `/api/ws?token=` 升级被 401 拒导致启动循环）均为影响真实用户的稳定性修复。整体上，项目在**架构治理、跨平台更新可靠性、会话状态一致性**三条线上均有实质进展。

## 4. 社区热点

- **[Issue #78647] 大型文件分解 Epic（78 评论，已关闭）** — 获 78 条评论，为本周最热议题。社区围绕 god-file 分片方案展开了含实施计划、验证策略、回滚纪律的深度讨论，最终以 20/20 子任务全部完成收官，成为仓库架构治理的标杆案例。[链接](https://github.com/NousResearch/hermes-agent/issues/78647)
- **[Issue #66616] Skills 索引过期/降级（36 评论，开放中）** — 自动化探针检测到技能索引已 29.8 小时未重建（限制 26 小时），`/docs/skills` 依赖的 `skills-index.json` 链路持续引发讨论，已挂 `sweeper:risk-automation` 标签。[链接](https://github.com/NousResearch/hermes-agent/issues/66616)
- **[Issue #4178] python-olm 构建失败（11 评论，已关闭）** — 升级 0.5.0→0.6.0 时出现的错误，虽不影响行为但获 2 个 👍，说明有一定用户基数遭遇同类告警。[链接](https://github.com/NousResearch/hermes-agent/issues/4178)
- **[Issue #51327] Linux 桌面启动器静默失败（9 评论，开放中，P1）** — Electron `chrome-sandbox` 缺少 setuid 4755 导致无窗口无报错退出，P1 级别且与安全边界相关，讨论热度持续。[链接](https://github.com/NousResearch/hermes-agent/issues/51327)

**诉求分析**：热点高度集中于三类——安装/更新链路可靠性、桌面端打包与权限、仓库内部自动化工具链（索引重建、分片治理）的健康度。用户对"装得上、更新不坏、启动有反馈"的诉求明显强于新功能。

## 5. Bug 与稳定性

**P1（高危）**

- **[#51327] Linux `.desktop` 启动器静默失败**（chrome-sandbox 无 setuid 4755，无窗口无报错）— 开放中，无 fix PR。影响 Linux 桌面用户首次启动体验。[链接](https://github.com/NousResearch/hermes-agent/issues/51327)

**P2（安全边界）**

- **[#84551] `detect_dangerous_command` 不解析 `timeout` / `bash -c` 包装，可绕过审批门禁** — 恶意命令经简单包装即被分类为"不危险"并免审批执行，属安全边界风险，开放中。[链接](https://github.com/NousResearch/hermes-agent/issues/84551)
- **[#85315] `auxiliary.free_only` 门控拒绝显式请求的 `:free` 模型，且误报为支付/凭据错误** — 错误归因影响用户排障，开放中。[链接](https://github.com/NousResearch/hermes-agent/issues/85315)

**P2（稳定性/平台）**

- **[#87295] 桌面端二次启动静默杀死运行中应用的后端** — 当日报告，**已有 fix PR #87314**。[链接](https://github.com/NousResearch/hermes-agent/issues/87295)
- **[#87292] 慢速本地模型（>16 TPS）出现两类超时**（WinError 10053 与 provider 无响应）— 用户反馈"最近几天开始"，疑似回归，**已有 fix PR #87310**。[链接](https://github.com/NousResearch/hermes-agent/issues/87292)
- **[#87309] `delegate_task` 在目标 CLI 不支持 `--acp` 时挂起约 600s**（Claude Code v2.x 实测 109s 起）— 开放中。[链接](https://github.com/NousResearch/hermes-agent/issues/87309)
- **[#87280] cron lifecycle guard 对 bash 算术除法 `$(( x / y ))` 误报**，阻止合法 `hermes cron create` — 开放中。[链接](https://github.com/NousResearch/hermes-agent/issues/87280)
- **[#87051] Telegram `/loop` 响应在网关重启后投递到错误话题** — 开放中，挂 `risk-message-delivery`。[链接](https://github.com/NousResearch/hermes-agent/issues/87051)
- **[#87268] `install.sh --commit` 使用短 SHA 时静默安装未固定 main 并退出 0** — 安装器可信度问题，开放中。[链接](https://github.com/NousResearch/hermes-agent/issues/87268)
- **[#87200] Windows 桌面端子代理超时后 "computing…" 指示器卡死直至重启** — 开放中（含土耳其语报告，国际化用户活跃）。[链接](https://github.com/NousResearch/hermes-agent/issues/87200)
- **[#84371] 压缩死循环：预检计入完整推理回放但尾部预算排除之（middle=0）** — 深层会话压缩无效循环，开放中。[链接](https://github.com/NousResearch/hermes-agent/issues/84371)
- **[#85868] macOS 热更新后渲染器残留、空白重载与退出守卫失效** — 开放中。[链接](https://github.com/NousResearch/hermes-agent/issues/85868)
- **[#75584] Windows 中断安装后 `hermes.exe` 缺失 + `node_modules` ENOTEMPTY + 桌面更新未完成** — 开放 16 天，与当日多个 Windows 更新类 Bug 同源。[链接](https://github.com/NousResearch/hermes-agent/issues/75584)

**当日小结**：新报告 Bug 以 P2 为主，且 2 个（#87295、#87292）在 24 小时内即获得修复 PR；但安全边界类（#84551）与安装器类（#87268）问题尚无修复，建议优先处置。

## 6. 功能请求与路线图信号

**新功能请求**

- **[#40306] Auto reasoning mode（ChatGPT 式自动推理）** — 老 Issue 今日仍有更新，用户希望 `reasoning_effort` 支持 `auto`，简单问答直答、复杂任务自动思考，呼声持续。[链接](https://github.com/NousResearch/hermes-agent/issues/40306)
- **[#86986] Termux 原生 pkg 安装/升级作为一等 Android 路径** — 直击 Android 端多次安装/更新失败模式，要求摆脱 manylinux 假设，属平台体验补全。[链接](https://github.com/NousResearch/hermes-agent/issues/86986)
- **[#87267] 新增 MAX（VK 旗下俄罗斯即时通讯）平台插件** — 用户以"俄语区无任何受支持平台"为由提议，体现多平台接入的多样性诉求。[链接](https://github.com/NousResearch/hermes-agent/issues/87267)

**路线图信号**

- **[#83565] 子进程凭据继承收敛（campaign epic）** — 将修复受信凭据泄漏至模型编写子进程的所有 PR/Issue 绑定至 #77027，安全加固方向明确。[链接](https://github.com/NousResearch/hermes-agent/issues/83565)
- **[#79564] Discord Feature Parity & Alignment Campaign（API v10）meta-issue** — 持续推进 Discord 全功能对齐。[链接](https://github.com/NousResearch/hermes-agent/issues/79564)
- **[#82591] Kanban 零权威 worker、持久发布与大型文件根除（tracker）** — 与 #78647 一脉相承的仓库治理延伸。[链接](https://github.com/NousResearch/hermes-agent/issues/82591)

**可能进入下一版本的 PR**（均与官方安全/桌面/插件治理方向一致，合并优先级预计较高）

- [PR #87312] feat(desktop): Capabilities 级 profile 作用域 + Skills 页一键 Hub 安装（teknium1）[链接](https://github.com/NousResearch/hermes-agent/pull/87312)
- [PR #87311] feat(plugins): 安装前披露编排者 worker 行为 [链接](https://github.com/NousResearch/hermes-agent/pull/87311)
- [PR #86948] feat(memory): provider 超时参数可配置化 [链接](https://github.com/NousResearch/hermes-agent/pull/86948)
- [PR #76772] feat(tui/slash): `/widgets` 子命令族（list/reload/load/unload/update）[链接](https://github.com/NousResearch/hermes-agent/pull/76772)
- [PR #86625] feat(desktop): 侧边栏聚类 `[Topic]` 前缀会话 [链接](https://github.com/NousResearch/hermes-agent/pull/86625)

## 7. 用户反馈摘要

- **更新/安装链路是最大痛点**：Windows 上 `cryptography._rust.pyd` 自锁（#83569）、中断安装后 exe 丢失与 ENOTEMPTY（#75584）、`install.sh` 短 SHA 静默装错版本（#87268）——多名用户报告"更新即坏"，且即使无网关/桌面/REPL 运行也 100% 复现，信任度受损。[#83569](https://github.com/NousResearch/hermes-agent/issues/83569)
- **慢速本地模型用户挫败感强**：>16 TPS 即触发断连或 provider 无响应，用户明确表示"最近几天才开始"，指向回归；#87310 正在修复，建议加快合并。[#87292](https://github.com/NousResearch/hermes-agent/issues/87292)
- **桌面端状态一致性影响日常使用**：陈旧 "Thinking" 状态（#50159）、子代理超时后指示器卡死（#87200）、二次启动杀死后端（#87295）均在无提示下发生，用户无法自我恢复，体验接近"崩溃"。[#50159](https://github.com/NousResearch/hermes-agent/issues/50159)
- **网关消息语义受质疑**：Telegram `/loop` 在网关重启后投递出错位话题，用户明确表达对会话连续性的不信任。[#87051](https://github.com/NousResearch/hermes-agent/issues/87051)
- **正面信号**：大型文件分解 Epic 获得大量深度参与（78 评论）并成功关闭；社区贡献者当日密集提交 kanban、config、桌面修复共 3 个 `fangliquanflq` 系列 PR，说明开发体验与贡献流程对第三方相对友好。

## 8. 待处理积压

以下问题开放时间较长且无修复，提醒维护者关注：

- **[#66616] Skills 索引降级**（开放 29 天，36 评论）— 自动化探针持续失败，`/docs/skills` 文档可信度受损，挂 `sweeper:risk-automation`。[链接](https://github.com/NousResearch/hermes-agent/issues/66616)
- **[#51327] Linux 桌面 chrome-sandbox setuid 静默失败**（P1，开放 54 天）— 唯一 P1 桌面 Bug，无 fix PR，Linux 用户首次启动直接失败。[链接](https://github.com/NousResearch/hermes-agent/issues/51327)
- **[#50159] 桌面端陈旧 "Thinking" 状态**（P2，开放 56 天，1 👍）— 高频可见且直接影响使用信任，长期无进展。[链接](https://github.com/NousResearch/hermes-agent/issues/50159)
- **[#75584] Windows 中断安装后恢复**（P2，开放 16 天）— 与今日多个 Windows 更新类 Bug 同源，建议与 #83569 的修复模式合并处理。[链接](https://github.com/NousResearch/hermes-agent/issues/75584)
- **[#40306] Auto reasoning 模式**（P3，开放 71 天）— 持续获得社区关注但未有实施迹象，建议纳入路线图评审。[链接](https://github.com/NousResearch/hermes-agent/issues/40306)

**额外提醒**：当前 47 条待合并 PR 中，7-15 天以上的 P2/P3 修复包括 [PR #75154](https://github.com/NousResearch/hermes-agent/pull/75154)（update 保留本地提交）、[PR #78058](https://github.com/NousResearch/hermes-agent/pull/78058)（desktop 保留会话历史）、[PR #81843](https://github.com/NousResearch/hermes-agent/pull/81843)（kanban 环境剥离）等，建议安排集中 review 以降低积压风险。

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报 — 2026-08-16

## 1. 今日速览

截至 2026-08-16，PicoClaw 项目在过去 24 小时内无新 Issue、无新版本发布，仅 2 条 PR 处于待合并状态且均已标记为 stale。这两个 PR 分别聚焦于**前缀缓存优化**（#3321）与 **WhatsApp 客户端过期修复**（#3320），均由同一作者 `grrowl` 提交，创建于 8 月 7 日，最近一次更新为 8 月 15 日。整体来看，项目近 9 天处于低活跃度维护阶段，无重大代码合并，社区讨论冷清，但两个悬而未决的 PR 若被合并，将修复实际运行问题并带来性能提升，值得维护者优先处理。

## 2. 版本发布

无新版本发布，此部分省略。

## 3. 项目进展

今日无任何 PR 被合并或关闭，项目代码库没有向前推进。但当前有 2 个已提交、待合并的 PR 隐含了后续可能的进展方向：

- **[PR #3321] fix(agent): move dynamic context after history to preserve prefix caching**
  - 将动态上下文块（当前时间、运行时、会话、发送者等）从系统消息前部移至对话历史之后，以避免改变前缀 token 而导致缓存失效。若合并，将显著降低长对话场景下的推理延迟与成本。
- **[PR #3320] fix(deps): bump whatsmeow to unblock WhatsApp "client outdated (405)"**
  - 升级 `go.mau.fi/whatsmeow` 依赖，解决 WhatsApp 原生通道因客户端版本过旧而被服务器拒绝（405）的问题。若合并，将恢复 WhatsApp 通道的可用性。

此外，这两个 PR 已在 8 月 15 日被标记为 stale，意味着维护者超过一周未进行审阅或操作。项目整体的「合并速度」目前偏慢，积压风险在上升。

## 4. 社区热点

两项 PR 均没有评论数据（评论数为 undefined，实际为零），也无人点赞，未形成讨论热点。但这两个 PR 本身承载了明确的社区痛点信号：

- **WhatsApp 通道不可用**（#3320）：用户依赖的通讯通道已断连数日，却无人跟进。
- **长对话性能优化**（#3321）：前端缓存机制失效会直接影响高频用户的使用成本与体验。

从 PR 的 stale 状态来看，这些呼声尚未得到维护团队回应，建议社区成员在 issue 区进一步补充实际影响案例，推动维护者关注。

## 5. Bug 与稳定性

当前有 1 个实际 Bug 已提交修复 PR，另有 1 个性能退化问题等待优化。

| 严重程度 | 描述 | 对应 PR | 状态 |
|---------|------|---------|------|
| 中高 | WhatsApp 原生通道完全不可用：服务器拒绝客户端版本，连接在约 5 秒后被断开，且无重连机制 | [PR #3320](https://github.com/sipeed/picoclaw/pull/3320) | 待合并（stale） |
| 中 | 前缀缓存因动态上下文插入而失效，导致长会话推理成本上升、响应延迟增加 | [PR #3321](https://github.com/sipeed/picoclaw/pull/3321) | 待合并（stale） |

两个问题均已有对应修复 PR，但均未得到审阅合并。

## 6. 功能请求与路线图信号

今日无新功能请求。从已有 PR 中可提炼出以下技术方向信号：

- **基础设施健壮性**：升级外部依赖（whatsmeow）以保持通道兼容性，是维持项目可用性的必要维护动作。
- **性能优化**：通过调整上下文排列来利用前缀缓存，表明项目对长上下文场景的成本控制已有考量，未来可能在 agent 对话记忆管理方面继续优化。

这两个方向都偏向「修复与优化」而非「新功能扩展」，说明下一版本更可能是补丁型 release，而非功能型更新。

## 7. 用户反馈摘要

由于今日无新 Issue 评论，无法直接引用用户原话。但可根据 PR 描述推导出用户侧真实痛点：

- **通讯通道失效影响实际使用**：WhatsApp 通道因客户端版本过期而被断连（405），且无自动重连，用户被迫切换到其他通道或中断工作流（见 [PR #3320](https://github.com/sipeed/picoclaw/pull/3320)）。
- **长对话体验与成本关乎高频用户**：前缀缓存未命中意味着每次请求都要从头计算 prompt，长会话下延迟与费用线性增长，对于重度用户不可忽略（见 [PR #3321](https://github.com/sipeed/picoclaw/pull/3321)）。
- **维护响应滞后**：两个 PR 已存在 9 天并进入 stale 状态，用户/贡献者可能感到进度停滞，需维护者及时回应。

## 8. 待处理积压

以下为目前积压的 PR，均为 stale 状态，持续超过 8 天未获审阅操作，建议维护者尽快响应：

- **[PR #3320](https://github.com/sipeed/picoclaw/pull/3320) fix(deps): bump whatsmeow to unblock WhatsApp "client outdated (405)"**
  - 创建：2026-08-07
  - 最近更新：2026-08-15
  - 影响：WhatsApp 通道持续不可用，属功能性阻断问题
  - 建议：优先审阅并合入，或明确给出暂缓合并的技术理由

- **[PR #3321](https://github.com/sipeed/picoclaw/pull/3321) fix(agent): move dynamic context after history to preserve prefix caching**
  - 创建：2026-08-07
  - 最近更新：2026-08-15
  - 影响：长会话推理成本与延迟无法优化
  - 建议：补充性能基准测试数据后推进合并

---

**报告日期**：2026-08-16  
**数据来源**：sipeed/picoclaw GitHub 仓库  
**免责声明**：本日报基于 GitHub API 提供的结构化数据及 PR/Issue 描述生成，所有链接均指向官方仓库，信息截止至 2026-08-16。

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报 — 2026-08-16

**数据窗口**：过去 24 小时 ｜ **数据源**：[github.com/qwibitai/nanoclaw](https://github.com/qwibitai/nanoclaw)

## 1. 今日速览

过去 24 小时 NanoClaw 仓库无新 Issue、无新版本发布，活跃度全部集中在 Pull Request 层面：共 22 条 PR 更新，其中 19 条待合并、3 条已关闭/合并。核心团队成员 gavrielc 批量提交了 14 条带 `core-team` / `follows-guidelines` 标签的 PR，覆盖权限策略、渠道适配器能力、跨会话上下文、投递修复与数据库迁移，显示项目处于有计划的功能密集开发期。稳定性修复占比可观（心跳机制、轮询泄漏、消息投递路由），社区贡献者也贡献了 2 条关键修复。整体项目健康度良好，但 19:3 的待合并/关闭比例提示维护者审查带宽可能成为近期瓶颈。

## 2. 版本发布

今日无新版本 Release（过去 24 小时新版本发布数为 0），无可报告的更新内容、破坏性变更或迁移注意事项。

## 3. 项目进展

过去 24 小时共有 3 条 PR 被关闭/合并：

- **PR #3268 — fix(poll-loop): 停止的轮询循环泄漏活动查询的 follow-up poller**（[链接](https://github.com/qwibitai/nanoclaw/pull/3268)）
  核心团队稳定性修复。根因是 `runPollLoop` 仅在迭代间隙检查 `config.signal`，而循环通常阻塞在 `processQuery` 的常开流上，导致中止的轮询循环泄漏其活动查询及 500ms 周期的 follow-up 轮询器。修复后资源释放更彻底，属于后端基建的重要补强。

- **PR #3117 — feat(skill): add-omarchy-statusbar（Waybar 状态栏指示器）**（[链接](https://github.com/qwibitai/nanoclaw/pull/3117)）
  社区贡献者 mmneimne 的 Utility skill，为 Linux 桌面用户提供 Waybar 状态栏集成。该 PR 于 7 月 22 日创建、今日关闭/合并，标志 NanoClaw 生态向桌面端可观测性场景延伸。

- **PR #37 — Rename to DotClaw and switch from WhatsApp to Telegram**（[链接](https://github.com/qwibitai/nanoclaw/pull/37)）
  这条创建于 2026-02-02 的战略级提案在持续 6 个月后于今日关闭。从当前仓库仍以 NanoClaw / nanoclaw 命名来看，更名方案未最终落地；但 Telegram 相关改进已拆分为独立 PR（如 #3250）继续推进。这提示维护者更倾向于"保留项目名、按需替换渠道"的渐进式演进，而非整体重塑。

## 4. 社区热点

> 说明：本次 20 条展示 PR 的评论数字段均缺失，以下按更新活跃度、PR 集群规模与外部参与度评估。

- **gavrielc 的 A/C 系列功能 PR 集群**（[#3266](https://github.com/qwibitai/nanoclaw/pull/3266) A4、[#3265](https://github.com/qwibitai/nanoclaw/pull/3265) A3、[#3264](https://github.com/qwibitai/nanoclaw/pull/3264) A2、[#3263](https://github.com/qwibitai/nanoclaw/pull/3263) A1、[#3262](https://github.com/qwibitai/nanoclaw/pull/3262) A8+C4）
  14 条 PR 带 A1-A4/A8/C4 任务编号，显示团队正在执行一份既定路线图。涵盖权限拦截 seam、`suppressCreatedNotify`、交付批次预览 hook、适配器热启动、DM 表面标准化等。这是今日最大的"热点"——不是讨论热度，而是核心开发集中度，说明项目正批量落地架构级能力。

- **PR #2752 — 暂存仅暴露 url 的入站附件（Discord）**（[链接](https://github.com/qwibitai/nanoclaw/pull/2752)）
  创建于 6 月 12 日、今日仍有更新的社区 PR，是当前开放最久的投递相关修复。它直击 Discord 接入的真实痛点：带附件的消息到达 agent 时只剩 `[file: message.txt]` / `[image: foo.png]` 占位符，无字节内容也无路径。长期未合并使其成为社区持续关注的对象。

- **PR #3251 — fix(agent-runner): 防止限流期间心跳停摆**（[链接](https://github.com/qwibitai/nanoclaw/pull/3251)）
  社区开发者 DawoudIO 的修复，针对 Claude API 限流/挂起时心跳文件长时间不更新、导致容器被误判为 stale 并击杀的问题。外部贡献者修复关键稳定性缺陷，是社区价值输出的积极信号。

## 5. Bug 与稳定性

按严重程度排列（均已有对应修复 PR）：

| 严重度 | 问题描述 | 修复 PR |
|---|---|---|
| 🔴 严重 | 心跳机制在 API 限流/挂起时最长停摆 30+ 分钟，触发虚假 stale-container 击杀 | [#3251](https://github.com/qwibitai/nanoclaw/pull/3251)（开放） |
| 🔴 严重 | 无 `.heartbeat` 文件的空闲容器可永久豁免绝对上限击杀，构成资源泄漏缺口 | [#3252](https://github.com/qwibitai/nanoclaw/pull/3252)（开放） |
| 🟠 中等 | 轮询循环停止后泄漏活动查询及其 follow-up 轮询器（500ms 周期） | [#3268](https://github.com/qwibitai/nanoclaw/pull/3268)（已关闭，修复完成） |
| 🟠 中等 | 多 bot 身份共享同一平台地址时，outbound 投递可能解析到任意兄弟实例行，存在错投风险 | [#3255](https://github.com/qwibitai/nanoclaw/pull/3255)（开放） |
| 🟠 中等 | 入站批次选择中 context 行（trigger=0）可挤掉到期任务行，导致唤醒后任务未被执行 | [#3254](https://github.com/qwibitai/nanoclaw/pull/3254)（开放） |
| 🟡 轻微 | Telegram legacy Markdown 清洗器将 `**bold**` 降级渲染为 *italic* | [#3250](https://github.com/qwibitai/nanoclaw/pull/3250)（开放） |
| 🟡 轻微 | opencode 集成未遵守群组配置的 reasoning effort | [#3253](https://github.com/qwibitai/nanoclaw/pull/3253)（开放） |

其中 #3251 与 #3252 同时指向容器生命周期管理的可靠性缺陷，是当前最值得优先处理的稳定性领域。

## 6. 功能请求与路线图信号

从今日 PR 集群可清晰看到下一阶段的路线图轮廓：

- **渠道适配器能力扩展**：`setTyping` 状态行/状态类型、`setThreadTitle`、`setSuggestedPrompts` 等可选能力（[#3261](https://github.com/qwibitai/nanoclaw/pull/3261)）；Chat SDK bridge 的 DM 线程归一化、`app_context` 捕获与 `dm-opened` 钩子（[#3262](https://github.com/qwibitai/nanoclaw/pull/3262)）
- **权限策略精细化**：新增第四种未知发送者策略 `decline_notify`——礼貌拒绝未知发送者并仅向 owner 发送一行 FYI，无需审批卡片（[#3260](https://github.com/qwibitai/nanoclaw/pull/3260)）；注册卡片构建前的通用拦截 seam（[#3266](https://github.com/qwibitai/nanoclaw/pull/3266)）
- **会话与上下文管理**：跨会话上下文模块（fan-out、DM backfill、echo 裁剪）与新增 CLI `ncl sessions history`（[#3257](https://github.com/qwibitai/nanoclaw/pull/3257)）；`messaging_groups.detached_at` 迁移 022，机器人被移出会话后拒绝投递（[#3256](https://github.com/qwibitai/nanoclaw/pull/3256)）
- **运行时灵活性**：适配器热启动 API `startChannelAdapter(key)`（[#3263](https://github.com/qwibitai/nanoclaw/pull/3263)）；`CreateAgentOptions.suppressCreatedNotify` 以支持 wrapper 二次配置场景（[#3265](https://github.com/qwibitai/nanoclaw/pull/3265)）

A 系列任务编号表明这些多为规划内交付。结合 #3117（Waybar 状态栏）已被合并，下一版本预计将显著增强多渠道体验与多会话/多身份场景的可用性。

## 7. 用户反馈摘要

从今日活跃 PR 描述中提炼的真实用户痛点与使用场景：

- **Discord 附件不可读**："agent sees a bare `[file: message.txt]` / `[image: foo.png]` with no bytes and no path"——文本粘贴与图片附件均无法被 agent 消费，社区等待修复已超两个月（[#2752](https://github.com/qwibitai/nanoclaw/pull/2752)）
- **Telegram 加粗渲染为斜体**：agent 输出的 `**bold**` 被降级为 *italic*，影响机器人输出可读性（[#3250](https://github.com/qwibitai/nanoclaw/pull/3250)）
- **容器被误杀**：API 限流期间心跳停摆导致容器被误判为 stale 并杀死，实际运行任务被中断；心跳机制只在收到 API 事件时刷新是根因（[#3251](https://github.com/qwibitai/nanoclaw/pull/3251)）
- **多身份同房间投递错乱**：同一群聊存在多个 bot 身份时，消息可能投递到"任意兄弟实例"，多租户/多身份用户受影响明显（[#3255](https://github.com/qwibitai/nanoclaw/pull/3255)）
- **任务被上下文淹没**：累积的 context 行将到期任务挤出批次，产生"唤醒触发但任务未执行"的隐性故障（[#3254](https://github.com/qwibitai/nanoclaw/pull/3254)）

使用场景方面，今日 PR 覆盖 Linux 桌面（Waybar）、Discord 社区、Telegram 群聊、多会话 agent 组等，显示 NanoClaw 用户群体与部署形态的多样性正在提升。

## 8. 待处理积压

- **⏳ PR #2752（Discord 附件暂存）**：开放中，创建于 2026-06-12，已积压约两个月且今日仍有更新。这是社区最关心的修复之一，建议维护者优先安排 review/merge（[链接](https://github.com/qwibitai/nanoclaw/pull/2752)）
- **⏳ gavrielc 的 12+ 条开放 PR**：包括 A 系列功能与多项修复，均带 `core-team` / `follows-guidelines` 标签且为同一开发者集中提交。大批量提交对合并带宽构成压力，建议分批审查、按依赖关系排序，避免长期分叉（[例：#3257](https://github.com/qwibitai/nanoclaw/pull/3257)）
- **⏳ PR #3253（opencode 推理努力值）**：社区开发者 simonechecchia 的修复，属小体量 Fix，建议与 #3250 等轻量修复一并快速合入（[链接](https://github.com/qwibitai/nanoclaw/pull/3253)）
- **✅ PR #37 已了结**：这条 2 月 2 日创建的战略性 PR 历时 6 个月后关闭。建议维护者如后续仍有意调整项目命名或渠道战略，可发布公开说明澄清方向，避免社区重复提交同类提案（[链接](https://github.com/qwibitai/nanoclaw/pull/37)）

---

*本报告由 AI 分析师基于 2026-08-16 GitHub 数据自动生成，所有条目均附带原始链接，供进一步核查。*

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

# NullClaw 项目动态日报 — 2026-08-16

## 1. 今日速览

过去 24 小时 NullClaw 项目整体活跃度较低但方向明确：共新增 1 个 Issue（#988）和 1 个 PR（#987），无新版本发布，无 Issue/PR 关闭或合并。Issue #988 提出了 HTTP(S) 与 SOCKS(5) 代理支持需求，PR #987 则针对长时本地工具密集运行场景进行「循环卫生」优化。虽然当前合并动作为零，但 PR #987 涉及系统提示词缓存拆分、工具输出压缩等设计，若合入将明显改善长任务稳定性和 token 效率。整体项目状态健康，属于正常的低峰期迭代节奏。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

今日没有已合并或关闭的 PR，因此没有直接进入主干的代码变更。值得关注的是唯一活跃 PR：

- **[#987 [OPEN] feat(agent): loop hygiene for long local tool-heavy runs](https://github.com/nullclaw/nullclaw/pull/987)**  
  作者：vernonstinebaker  
  该 PR 提出三项核心改进：
  1. 将系统提示词拆分为「稳定前缀 + 可变时间尾部」，并通过 `stablePrefixHash` 提升缓存友好性；
  2. 在历史注入前压缩工具输出（`result_compress.zig`），同时保留 observer 日志中的完整输出；
  3. 增加每轮相同调用的防护逻辑（summary 被截断，但从标题看是避免重复调用/循环异常）。

  这代表项目在**长时运行稳定性**和**上下文窗口效率**上的主动优化，是 agent 工程走向生产可用的重要方向。该 PR 目前为待合并状态，尚需维护者 review。

## 4. 社区热点

今日社区讨论极少，仅有 1 个新 Issue 和 1 个新 PR，均无评论。

- **[#988 [OPEN] [enhancement] proxy support](https://github.com/nullclaw/nullclaw/issues/988)**  
  用户 anpic 请求为 providers 增加 HTTP(s) 与 SOCKS(5h) 代理支持。背后诉求主要是：在使用外部大模型 API 时，部分用户处于受限网络环境，或需要通过代理进行请求转发、隐私保护和合规访问。这一问题在面向企业/个人开发者的 AI 网关类项目中非常常见，属于**网络接入能力的基础需求**。

- **[#987 [OPEN] feat(agent) ...](https://github.com/nullclaw/nullclaw/pull/987)**  
  虽无评论，但 PR 描述揭示了作者在长时工具调用场景下的实际痛点：循环失控、输出过大、缓存失效。这也是 agent 开发者普遍关心的问题。

## 5. Bug 与稳定性

今日未报告新的 Bug、崩溃或回归问题。值得注意的稳定性相关工作是 PR #987，虽然它不是 bugfix，但针对的是「长时本地工具密集运行」场景下的稳定性与资源效率，可视为潜在稳定性隐患的预防性修复。目前无已确认的线上故障。

## 6. 功能请求与路线图信号

- **[#988 proxy support](https://github.com/nullclaw/nullclaw/issues/988)**：用户明确希望增加 HTTP(S) 和 SOCKS5 代理支持，便于在受限网络环境下访问 provider。该需求若被接受，预计会涉及网络层抽象和 provider 配置扩展，很可能进入下一版本的配置项能力中。

- **[#987 loop hygiene](https://github.com/nullclaw/nullclaw/pull/987)**：虽然不是新功能，但其「提示词缓存拆分 + 工具输出压缩 + 重复调用防护」的组合，很可能成为后续版本 agent 执行引擎的默认行为。这暗示项目路线图正在向**长时间自主运行**和**低 token 消耗**方向倾斜。

综合来看，社区目前对外部连接能力（代理）和内部执行效率（长时运行）两条线索有明确期待。

## 7. 用户反馈摘要

今日仅有一名用户在 Issue #988 中提出需求，无评论互动。核心用户声音为：

- 需要为 providers 增加 HTTP(s) 和 SOCKS(5h) 代理支持；
- 动机未详细说明（`_No response_`），但可推测与网络隔离、访问稳定性或隐私需求有关。

由于没有更多评论，无法提炼出更丰富的满意/不满意反馈。建议维护者在 #988 下追问具体使用场景，以便设计更贴合的代理配置模型。

## 8. 待处理积压

目前暂无长期未响应或被忽视的重要 Issue/PR。当前两个开放项均为昨日（2026-08-15）新创建，尚未进入「积压」状态。但需要关注：

- [Issue #988](https://github.com/nullclaw/nullclaw/issues/988)：新增功能请求，等待维护者确认是否纳入 roadmap。
- [PR #987](https://github.com/nullclaw/nullclaw/pull/987)：核心优化 PR，建议尽快安排 review，避免长期挂起导致大 diff 合并风险增加。

总体来看，NullClaw 项目今日处于「低活跃、高质量输入」状态，社区需求清晰，无稳定性警报。建议维护者优先推进 PR #987 的 code review，并对 Issue #988 的代理需求作出初步规划回应。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报 — 2026-08-16

## 1. 今日速览

过去 24 小时 IronClaw 保持高活跃度：共 27 条 Issue 更新（6 条新增/活跃、21 条关闭，闭环率约 78%），13 条 PR 更新（6 条合并/关闭、7 条待合并），无新版本发布。项目重心集中在三件事：**unbound-turns 架构切换正式完成**（#7562/#7634 合并）、**#7591 性能优化史诗批量落地**（#7628/#7629/#7676 三笔 perf PR 合入）、以及**大量 Reborn 迁移遗留 Issue 清理关闭**。风险信号是 Live Canary 已连续 30/30 次全红（根因是测试框架缺陷，修复 PR #7679 在途），且 #7634 评审衍生出 5 个新的架构/质量跟进项。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

### 架构里程碑：unbound-turns 模型完成切换
- [PR #7562](https://github.com/nearai/ironclaw/pull/7562)（已合并）：unbound-turns 基础 PR，包含两份设计文档 + Phase 1 完整实现（prepared-context accept door、unbound run lane、kernel binding-ref deletion）。
- [PR #7634](https://github.com/nearai/ironclaw/pull/7634)（已合并）：**完成向 prepared-context turns 的全面切换**，携带 #7633 全部 follow-up，并对两份设计文档执行 71 条一致性审计，所有偏差均已收敛。

这是今日最重大的架构进展，标志着 Reborn 运行模型的关键转型正式落地。

### 性能优化：#7591 史诗持续推进
- [PR #7628](https://github.com/nearai/ironclaw/pull/7628)（已合并）：停止追加 `ProcessJournalKind::Heartbeat` journal 行并取消心跳游标预留（对应 [Issue #7593](https://github.com/nearai/ironclaw/issues/7593)），每进程每天减少约 2,880 条永久 journal 行，同时将心跳区间调整至 15 秒。
- [PR #7629](https://github.com/nearai/ironclaw/pull/7629)（已合并）：将 trigger run-history 清理从每次 Running 行更新移到首次 fire claim，消除每次触发 2-3 次无条件相关子查询 DELETE，并在恢复路径保留完成时清理以保证严格保留语义（对应 [Issue #7595](https://github.com/nearai/ironclaw/issues/7595)）。
- [PR #7676](https://github.com/nearai/ironclaw/pull/7676)（已合并）：合并 bursty 线程活动 touch 为有界写入，每 turn 最多 7 次 CAS 行重写降为每线程每间隔 ≤1 次，并通过单调 CAS 保持多 worker 正确性（对应 [Issue #7596](https://github.com/nearai/ironclaw/issues/7596)）。

### 清理与基础设施
- 21 条 Issue 关闭，覆盖 Reborn/Crabshack 迁移遗留（[#4629](https://github.com/nearai/ironclaw/issues/4629)、[#4922](https://github.com/nearai/ironclaw/issues/4922)、[#4775](https://github.com/nearai/ironclaw/issues/4775)、[#3236](https://github.com/nearai/ironclaw/issues/3236)、[#3423](https://github.com/nearai/ironclaw/issues/3423)）、性能浪费项（[#7597](https://github.com/nearai/ironclaw/issues/7597)、[#7599](https://github.com/nearai/ironclaw/issues/7599)）及 bug 修复，整体技术债清偿速度可观。
- [PR #7670](https://github.com/nearai/ironclaw/pull/7670)（已合并）：CI 每日任务自动刷新 codebase knowledge graph。

## 4. 社区热点

- **[Issue #467](https://github.com/nearai/ironclaw/issues/467) — Trajectory benchmark system（4 条评论）**：今日评论最多的 Issue，自 2026-03-02 开放至今仍在活跃讨论。诉求是构建跑真实用户场景、真实 LLM 调用的轨迹评测系统，通过硬断言（工具选择、响应内容、成本、延迟）+ LLM-as-judge 双层标准评估 agent 质量，反映社区对可量化评测体系的强烈需求。
- **[Issue #3236](https://github.com/nearai/ironclaw/issues/3236) — Reborn 同线程 follow-up 与 steering 策略（3 条评论，已关闭）**：围绕同规范线程下 `/btw` 显式 steering、队列可见性与排序、取消交互、blocked-run 行为等运行控制语义的讨论，最终关闭，说明策略已收敛。
- **#7634 评审衍生 5 个新 Issue**：[#7671](https://github.com/nearai/ironclaw/issues/7671)（栈压力）、[#7672](https://github.com/nearai/ironclaw/issues/7672)（Typed ToolChoice）、[#7673](https://github.com/nearai/ironclaw/issues/7673)（BudgetLedger 核算）、[#7674](https://github.com/nearai/ironclaw/issues/7674)（符号级架构测试）、[#7675](https://github.com/nearai/ironclaw/issues/7675)（E2E flake），全部由评审者 henrypark133 从 #7634 review 线程中提炼。评审密度高、跟进及时，是健康的工程文化信号。

## 5. Bug 与稳定性

**高优先级：**
- **Live Canary 连续 30/30 次全红**：定位为 4 个 harness 缺陷（3 个误报正确产品行为、1 个 liveness 代理误伤持久化证据充分的用例），已有修复 [PR #7679](https://github.com/nearai/ironclaw/pull/7679)（开放中，XL，risk: low）。
- **[Issue #7675](https://github.com/nearai/ironclaw/issues/7675)（开放）**：`qa_6c_gmail_to_sheet_live_chat` 间歇性资源类能力失败，且 flake 会级联拖垮整个 provider-contracts 会话；已确认与 #7634 无关，属于既有测试稳定性问题。
- **[Issue #7671](https://github.com/nearai/ironclaw/issues/7671)（开放）**：LoopCapabilityPort 装饰器链编译为超大 poll frame，导致默认 2 MiB 测试线程栈溢出（首次出现于 reborn_integration_model_recovery）；f1f396cd8 已通过 chain-boxing 修复该套件，但内核沙箱路径仍接近栈上限，需后续缓解。

**中低优先级（均已关闭）：**
- [#5239](https://github.com/nearai/ironclaw/issues/5239)：`turn_scheduler` 将 run 完成后的陈旧心跳误判为 runner 故障，触发错误终止路径和 `Co…` 重试。
- [#5237](https://github.com/nearai/ironclaw/issues/5237)：`IRONCLAW_REBORN_LOG=debug` 时 Wasmtime/Cranelift 编译器 DEBUG 日志淹没 Railway。
- [#6835](https://github.com/nearai/ironclaw/issues/6835)：MCP `AuthRequired` 被归类为 `Client` 而非触发重新认证门禁。
- [#4992](https://github.com/nearai/ironclaw/issues/4992)：local-dev SSO 访问不匹配导致 Railway 自动化在 run/thread 创建前失败。

## 6. 功能请求与路线图信号

- **[Issue #467](https://github.com/nearai/ironclaw/issues/467)**：轨迹基准评测系统，今日仍活跃，是评测能力路线图的核心候选，建议纳入正式规划。
- **[Issue #7672](https://github.com/nearai/ironclaw/issues/7672)**：引入类型化 ToolChoice，替代 rig_adapter、bedrock、nearai_chat、gemini_oauth、codex_chatgpt、openai_codex_provider 共 6 个编码器中字符串匹配 "auto"/"required"/"none" 的重载设计，属 API 健壮性改进。
- **[Issue #7673](https://github.com/nearai/ironclaw/issues/7673)**：BudgetLedger 两项核算细化——截断启动窗口双重计费、计费持久性；当前实现偏保守（多计 → 提前停止，不会超限）。
- **[Issue #7674](https://github.com/nearai/ironclaw/issues/7674)**：为 openai-compat → threads 新增依赖边增加符号级架构测试白名单，补充现有 crate 级边界检查粒度不足的问题。
- 开放 PR 中的新功能：[#7651](https://github.com/nearai/ironclaw/pull/7651)（自动化确定性 no-result 抑制）、[#7516](https://github.com/nearai/ironclaw/pull/7516)（WebUI 操作员界面打通 IronHub agent link）、[#7491](https://github.com/nearai/ironclaw/pull/7491)（omp core-tool 契约 + 引擎 + 基准测试臂）。

## 7. 用户反馈摘要

- **IronHub 搜索与目录脱节（[#6821](https://github.com/nearai/ironclaw/issues/6821)，已关闭）**：用户询问"可安装内容"时 agent 仅报 3 个工具（签名目录实际 18 个）；再次询问列出 21 个技能，其中 20 个不是目录条目。搜索接地失败是明显的体验痛点，问题虽已关闭但值得作为评测用例沉淀。
- **Provider 工具数上限（[#4407](https://github.com/nearai/ironclaw/issues/4407)，已关闭）**：在 openai/gpt-5.4-nano 路由下，宿主暴露能力超过 provider `tools` 数组限制导致请求失败，说明能力选择需对模型可见。
- **CI 信号信任度下降（[#7679](https://github.com/nearai/ironclaw/pull/7679)、[#7675](https://github.com/nearai/ironclaw/issues/7675)）**：Canary 连续 30/30 全红，且 #7675 已确认与目标 PR 无关，维护者难以从 CI 结果判断真实质量，测试框架可靠性成为当前最突出的稳定性痛点。
- **运维噪音（[#5237](https://github.com/nearai/ironclaw/issues/5237)，已关闭）**：调试日志放开后低层编译器输出淹没 Railway，影响生产排障。

## 8. 待处理积压

- **[Issue #467](https://github.com/nearai/ironclaw/issues/467)**：2026-03-02 创建，已开放超 5 个月且今日仍在更新，轨迹评测系统尚无实现 PR，建议维护者明确排期或标注 milestone，避免长期悬置。
- **7 个开放 PR 待合并，其中 6 个为 XL 规模**：[#7651](https://github.com/nearai/ironclaw/pull/7651)、[#7491](https://github.com/nearai/ironclaw/pull/7491)、[#7679](https://github.com/nearai/ironclaw/pull/7679)、[#7678](https://github.com/nearai/ironclaw/pull/7678)、[#7677](https://github.com/nearai/ironclaw/pull/7677)、[#7516](https://github.com/nearai/ironclaw/pull/7516)。其中 [#7491](https://github.com/nearai/ironclaw/pull/7491) 自 08-11 起等待评审，建议优先处理，避免大 PR 持续膨胀增加合并成本。
- **[#7679](https://github.com/nearai/ironclaw/pull/7679)**：鉴于 Canary 已 30/30 全红，强烈建议优先合入以恢复 CI 信号可信度。
- **新贡献者 PR [#7516](https://github.com/nearai/ironclaw/pull/7516)**（neo-sky，WebUI IronHub agent link 操作员界面）：建议维护者及时给予 review 反馈，保持外部贡献者参与积极性。
- **[#7675](https://github.com/nearai/ironclaw/issues/7675)**：qa_6c flake 级联影响整个 provider-contracts 会话，目前无 fix PR，建议在下次 E2E 稳定性批次中优先处理。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报

**日期：2026-08-16** | 数据窗口：过去 24 小时

---

## 1. 今日速览

过去 24 小时项目整体处于**低活跃维护期**：无新版本发布；18 条 Issue 更新中 16 条被关闭（全部为 stale 自动清理），仅 2 条保持开放；6 条 PR 更新中 2 条功能修复 PR 关闭（#1879、#2234），4 条 dependabot 依赖更新 PR 仍搁置未合并。社区讨论热度集中在**会员登录失败**（#1903）与**Agent 记忆体系建议**（#2046）两个长期开放议题上。整体看，项目当前以积压清理和维护性修复为主，无重大功能推进信号。

---

## 2. 版本发布

无。

---

## 3. 项目进展

今日关闭 2 条功能相关 PR，若已合并则代表两项重要修复落地：

- **[PR #1879] fix: preserve manually-added plugin load paths on config sync**（closed）
  修复 `OpenClawConfigSync.sync()` 在写入 `openclaw.json` 时覆盖用户手动添加插件路径（如 `pm install` 安装的 `memory-lancedb-pro`）的问题，避免社区插件配置在同步后丢失。
  → [netease-youdao/LobsterAI PR #1879](https://github.com/netease-youdao/LobsterAI/issues/1879)

- **[PR #2234] fix(openclaw): cron yield descendant finalization**（closed）
  修复 cron 任务中 `sessions_yield` 后子 agent 完成事件无法驱动父 agent 继续执行的问题，增加 yield continuation 循环，覆盖普通会话/cron 并行/cron 串行三种子 agent 场景。
  → [netease-youdao/LobsterAI PR #2234](https://github.com/netease-youdao/LobsterAI/issues/2234)

此外 4 条 CI 依赖更新 PR（#2164、#2165、#2166、#2167）仍处于待合并状态，已停滞超过 2 个月。

---

## 4. 社区热点

今日大部分 Issue 更新来自 stale bot 批量关闭，真正活跃的讨论集中在两个长期开放议题：

- **[Issue #1903] 会员登录频繁失败**（OPEN，3 条评论）
  用户反馈会员登录持续失败，无法使用网易付费模型，并附带了错误截图。该问题自 5 月 7 日创建至今已 3 个月未被解决，今日仍标记为 OPEN，是当前社区最尖锐的痛点之一。
  → [netease-youdao/LobsterAI Issue #1903](https://github.com/netease-youdao/LobsterAI/issues/1903)

- **[Issue #2046] OpenClaw/LobsterAI 产品建议：Agent 记忆体系**（OPEN，2 条评论）
  用户系统性地提出 Agent 记忆能力缺失的问题，包括 session 标题/元数据持久化到文件系统、跨 session 上下文检索等具体建议，是近期最有价值的产品反馈之一。
  → [netease-youdao/LobsterAI Issue #2046](https://github.com/netease-youdao/LobsterAI/issues/2046)

另外，虽然 #2040（OpenClaw 五大薄弱点）和 #2041（记忆系统是最大瓶颈）今日被 stale 关闭，但两篇内容均为深度分析，评论区具备一定参考价值，建议维护者关注其观点。

---

## 5. Bug 与稳定性

按严重程度排列：

| 严重度 | Issue | 状态 | 是否有 Fix PR |
|--------|-------|------|--------------|
| 🔴 高 | **会员登录频繁失败**（#1903）— 付费功能不可用，用户无法使用网易模型 | OPEN | 无 |
| 🔴 高 | **邮箱 SKILL 路径穿越漏洞**（#1885）— `downloadAttachments` 未过滤附件名，存在路径穿越安全风险 | CLOSED (stale) | 无 |
| 🟠 中 | **追问无限 NO_REPLY 或输出中断**（#1849）— 任务提前 complete 但模型仍在输出，页面无数据响应 | CLOSED (stale) | 无 |
| 🟠 中 | **阿里百炼 coding plan 强制调用网易模型**（#1988）— 配置文件被系统强制覆盖，`qwen3.6-plus` 无法正常使用 | CLOSED (stale) | 无 |
| 🟠 中 | **微信扫码后无法输入验证码**（#1878）— 客户端缺少 6 位数字输入界面，IM 配置流程受阻 | CLOSED (stale) | 无 |
| 🟡 低 | **AI engine connection lost**（#1993）— 桌面端连接不稳定，IM Bot 正常 | CLOSED (stale) | 无 |
| 🟡 低 | **UI 空白加载/空状态缺图标**（#1920、#1921）— 体验问题 | CLOSED (stale) | 无 |

注意：除 #1903 外上述问题均被 stale 机制关闭，但多数未显示有对应修复 PR，需确认是否真实解决。

---

## 6. 功能请求与路线图信号

今日收集到的功能/产品需求信号：

- **[#2046] Agent 记忆体系**（OPEN）— 高优先级信号，包含 3 条具体建议：
  1. Session 对话标题/元数据持久化到文件系统（现存 IndexedDB，Agent 不可读）
  2. 跨 session 记忆检索与关联
  3. 用户手动维护依赖度降低
  → [Issue #2046](https://github.com/netease-youdao/LobsterAI/issues/2046)

- **[#1880] 增加 Hermes Agent 功能**（CLOSED）— 希望参照 Open WebUI 将 Hermes Agent 与 OpenClaw 一同接入
  → [Issue #1880](https://github.com/netease-youdao/LobsterAI/issues/1880)

- **[#2016] 增加 openhuman 引擎功能**（CLOSED）
  → [Issue #2016](https://github.com/netease-youdao/LobsterAI/issues/2016)

- **[#2036] OpenClaw gateway 增加 agent:turn / agent:loop 事件**（CLOSED）— 为实现实时落盘而提出的底层事件机制建议
  → [Issue #2036](https://github.com/netease-youdao/LobsterAI/issues/2036)

综合 #2040/#2041 的深度分析，社区对**记忆系统**和**安全加固**的呼声最高，推测这两方面是下一版本迭代的重点候选方向。

---

## 7. 用户反馈摘要

来自 Issue 评论的真实用户声音：

- **付费功能不可用最伤口碑**：“会员登录不进去，无法使用网易付费的模型”（#1903）
- **配置被强制覆盖引发不满**：“修改配置文件也没用，系统会强制改成错误的”（#1988）
- **IM 集成流程有断裂**：“扫码后会提示要求在 openclaw 端输入对应的 6 位数字，但客户端未给出输入界面”（#1878）
- **本地部署门槛偏高**：“提示未检测到内置 OpenClaw runtime（cfmind），请先执行打包前构建脚本”（#2017）
- **界面设计被直接吐槽**：“相比起其他竞品过于丑了，用起来不太舒服”（#1836）
- **记忆缺失导致重复劳动**：“每个新对话 session 独立存在，Agent 无法自动感知、检索、关联历史对话”（#2046）

---

## 8. 待处理积压

以下项目长期未获有效响应，提醒维护者关注：

| 项目 | 创建时间 | 搁置时长 | 说明 |
|------|----------|----------|------|
| [#1903] 会员登录频繁失败 | 2026-05-07 | ~3.2 个月 | OPEN，无 fix PR，直接影响付费用户体验 |
| [#2046] Agent 记忆体系建议 | 2026-05-25 | ~2.8 个月 | OPEN，产品路线图信号，社区呼声高 |
| [#2164] trufflehog CI 升级 | 2026-06-15 | ~2 个月 | dependabot PR，待合并（stale） |
| [#2165] actions/checkout v4→v6 | 2026-06-15 | ~2 个月 | dependabot PR，待合并（stale） |
| [#2166] paths-filter v3→v4 | 2026-06-15 | ~2 个月 | dependabot PR，待合并（stale） |
| [#2167] actions/stale v9→v10 | 2026-06-15 | ~2 个月 | dependabot PR，待合并（stale） |
| [#1885] 邮箱 SKILL 路径穿越安全漏洞 | 2026-05-06 | ~3.3 个月 | 虽被 stale 关闭，但属安全风险，建议确认修复状态 |

---

**数据来源**：LobsterAI GitHub 仓库（netease-youdao/LobsterAI） | 统计窗口：2026-08-15 至 2026-08-16

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis 项目动态日报 — 2026-08-16

## 今日速览
过去 24 小时 Moltis 保持较高的功能迭代节奏：共产生 6 条 PR 更新，其中 3 条已合并/关闭，3 条待合并；无新版本发布，也无 Issue 新增或关闭。项目当前集中于三大方向：远程工作区沙箱扩展、连接器生态（日历/邮件/频道）持久化，以及 Slack 原生交互体验增强。Issues 活跃度为 0，说明用户侧问题反馈较少，项目健康度良好，正处于活跃开发期而非被动修 bug 阶段。

## 版本发布
今日无新版本发布。

## 项目进展
今日共有 3 条 PR 被合并/关闭，均为功能改进或问题修复，项目核心能力得到稳步推进：

- **[#1196] Fix ClawHub skill search results**（已关闭）— 修复 ClawHub 技能搜索因逐条调用 metadata 接口导致 RPC 超时的问题，改为直接消费搜索元数据并统一 owner-qualified 引用；同时处理了重装时的 legacy bare-slug 兼容。该 PR 直接改善了技能发现流程的可靠性与响应速度，属于搜索链路的稳定性修复。
- **[#1197] Start agent chats from command palette**（已关闭）— 在命令面板中为非空查询追加 "Ask agent" 操作，支持在 debounced 会话搜索未完成时立即发起 agent 对话，并创建全新会话即时发送查询。该功能显著降低了用户启动 agent 聊天的操作成本，属于交互体验优化。
- **[#1198] Route OpenAI reasoning tool calls through Responses**（已关闭）— 当内置 OpenAI 请求同时使用 function tools 和 `reasoning_effort` 时自动切换到 Responses API，且在无 tools/reasoning 或使用第三方兼容 provider 时保持 Chat Completions 行为不变。该改动让推理模型 + 工具调用的组合不再受限，是模型能力对接层面的重要兼容性升级。

整体来看，项目今日向前迈进了三个半级迭代：搜索修复、命令面板快速入口、OpenAI 推理工具调用链路，均直接提升现有用户的可感知体验。

## 社区热点
今日无 Issue 更新，PR 也均无评论数据，未出现明显的高互动/高争议讨论。从 PR 类型判断，社区关注点集中在：

- **远程开发体验**：[PR #1199](https://github.com/moltis-org/moltis/pull/1199) 引入 Coder 沙箱后端，支持通过 REST API 创建临时工作区并在 reconnecting PTY WebSocket 上执行命令，面向使用 Coder 作为远程开发后端的团队。
- **连接器生态补全**：[PR #1190](https://github.com/moltis-org/moltis/pull/1190) 增加 CalDAV、Gmail、Himalaya v2 与频道历史数据集，属于推进"持久化连接器"基础设施的工作，预计会为后续日历/邮件/频道数据用例铺路。
- **Slack 原生交互**：[PR #1195](https://github.com/moltis-org/moltis/pull/1195) 将工具生命周期更新渲染为 Slack 原生 plan/task 卡片，并增强隐私保护（opaque run ID + 注册制 tool 名）。

以上三条 PR 均处于待合并状态，说明这些方向是项目当前主动推进的能力，且预计很快进入主分支。没有公开的用户讨论或反对意见，此类功能有望平滑落地。

## Bug 与稳定性
今日没有新 Issue 报告 Bug，但有 1 条 PR 属于明确的稳定性修复：

- **[#1196] ClawHub skill search results**（已关闭）— 严重程度：中高。该 Bug 导致技能搜索在结果较多时因 RPC 超时失败，直接影响用户发现和安装技能。修复路径为消除逐条 metadata 请求带来的延迟尖峰，直接消费搜索元数据，属性能与可用性的双重改进。已有对应 fix PR 并已关闭。

## 功能请求与路线图信号
今日没有来自用户的新增 Feature Request，但 3 条待合并 PR 清晰展示了项目近期路线图信号：

1. **Coder 远程工作区沙箱**（[PR #1199](https://github.com/moltis-org/moltis/pull/1199)）— 说明项目正在从纯本地执行向多云/远程沙箱执行演进，支持 template/preset/rich parameter/TTL 等企业级配置。预计会成为自定义 runner/sandbox 体系的一部分。
2. **连接器持久化 + CalDAV/Gmail/Himalaya v2**（[PR #1190](https://github.com/moltis-org/moltis/pull/1190)）— 这是一个较大的基础设施性 PR，涉及原子快照、调度、投影与本地全文搜索。推演下一步可能对外提供日历/邮件/频道数据的统一查询能力，或基于此构建定时任务与提醒功能。
3. **Slack 原生任务卡片**（[PR #1195](https://github.com/moltis-org/moltis/pull/1195)）— 属于渠道交互体验增强，表明项目在重视"agent 工具在多人协作软件中的可视化呈现"，后续可能向 Teams/Discord 等渠道复制同一套"channel-neutral lifecycle update" 模式。

结合上述信号，下一版本（预计 2026-08 下旬或 09 月初）可能以沙箱后端（Coder）、连接器持久化与 Slack 卡片为三大发布特性。

## 用户反馈摘要
因今日无 Issue 讨论数据，无法从评论中提炼直接的用户反馈。但可从 PR 设计反向推断当前用户的使用痛点与场景：

- 用户在 Coder 等远程开发环境中无法直接运行 agent，需要本地环境依赖，这可能是 [PR #1199](https://github.com/moltis-org/moltis/pull/1199) 试图解决的场景。
- 用户需要 agent 访问日历、邮件与历史频道消息作为决策上下文，但缺少持久化、可检索的连接器数据层，这由 [PR #1190](https://github.com/moltis-org/moltis/pull/1190) 覆盖。
- 用户希望 agent 的任务执行状态在 Slack 中以原生、结构化形式呈现，而不是纯文本流，这是 [PR #1195](https://github.com/moltis-org/moltis/pull/1195) 的直观动力。
- 技能搜索超时会影响用户安装和使用第三方技能，[PR #1196](https://github.com/moltis-org/moltis/pull/1196) 反映了这一现实痛点的快速修复过程。

整体未出现明显的负面反馈，项目处于功能前置建设期。

## 待处理积压
以下 PR 为待合并状态，建议维护者关注其 review 进度，避免长期滞留：

- **[#1190] Add durable calendar, channel, and email connectors**（创建于 2026-08-11，5 天未合并）— 体量较大，涉及连接器持久化与多个数据源实现，可能需要较长的 review 周期，但建议给出阶段性反馈以免阻塞后续日历/邮件相关功能。
- **[#1195] Add Slack native live task cards**（创建于 2026-08-15，待合并）— 功能独立且直接面向用户，建议优先合入。
- **[#1199] Add Coder remote workspace sandbox support**（创建于 2026-08-15，待合并）— 属于新后端能力，若项目计划支持多 sandbox 后端，建议尽早安排设计评审。

今日无长期未响应的 Issue 积压，整体 backlog 状态健康。

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw 项目动态日报 — 2026-08-16

## 今日速览

昨日 CoPaw 项目保持高活跃度：24 小时内产生 10 条 Issue 更新（9 条新增/活跃、1 条关闭）和 11 条待合并 PR，但无新版本发布，且 PR 合并数为 0。Issue 侧呈现典型的"功能请求与 Bug 报告双轮驱动"态势——新增 5 个功能/增强请求、5 个 Bug 报告，覆盖视频处理、OAuth2 刷新、插件权限、CLI 一致性等多个模块；PR 侧则有 2 个针对昨日新报告 Bug 的修复提交（#7061、#7055），响应速度值得肯定。值得关注的是 7 月提出的 Matrix 加密问题（#6476）已关闭，以及 4 月提出的 WebUI 虚拟滚动需求（#3915）仍无对应 PR，积压问题解决节奏偏慢。

## 版本发布

24 小时内无新版本发布。

## 项目进展

今日无 PR 被合并或关闭，但提交活跃，共 11 个 PR 处于待合并状态。以下 PR 对应的功能/修复方向值得关注：

- **#7061 fix(video): deliver tool-result videos on OpenAI Responses API** — 由 xiaoka76 提交，直接修复 #7059 和 #7060 两个视频静默丢失/硬编码限制问题，属于当日报告当日修复的高效响应。若合并，将解决 OpenAI Responses API（如 Volcengine Ark）下模型完全收不到视频帧的严重缺陷。
- **#7055 fix(cli): sync top-level text on agent cron --text update** — 由 lcq225 提交，修复 `qwenpaw cron update --text` 返回成功但 prompt 实际未更新的问题（#7048）。CLI 相关修复通常能快速提升用户信任度。
- **#7033 feat(skill-system): dynamic skill loading + auto-unload + frontmatter fix** — 为技能系统引入动态加载/自动卸载生命周期，解决技能静态化、闲置不释放等问题，是运行时技能管理的基础设施建设。
- **#6940 feat(pawapp): add native DataPaw app runtime and durable analysis workspace** — 由 first-time contributor 提交的大功能 PR，新增 DataPaw 原生应用运行时和持久化分析工作区，若合并将显著扩展产品形态。
- **#6302 feat: unify provider discovery, model metadata, routing, and agent controls** — 大型架构统一 PR，引入目录驱动的 provider 模型系统、运行时模型发现、能力感知路由与回退支持，涉及面广，已开放近一个月仍在评审。

整体来看，项目在视频链路修复、CLI 体验、技能系统、跨端运行时上均有实质推进，但所有 PR 均未合并，代码落地节奏受评审/CI 速度制约。

## 社区热点

- **#6476 [CLOSED] matrix 端到端加密不可用** — 3 条评论，2026-07-26 创建，昨日关闭。用户详细记录了通过 apt 安装 libolm-dev、uv pip install matrix-nio[e2e] 的完整排障路径，最终问题解决。此类"用户自行摸索 + 社区协助"的闭环案例对项目文档改进有参考价值。

- **#3915 Introduce virtual scrolling for Console WebUI** — 3 条评论，2026-04-28 创建，至今仍 OPEN。长对话历史导致 DOM 全量渲染严重卡顿，是 WebUI 常见性能瓶颈。该 Issue 已存在 3.5 个月无对应 PR，近期获得 +1 反应，说明用户诉求仍在积累。

- **#7056 后台任务回调/通知机制** — 2026-08-15 新开，用户坦承查阅了 `agent_management.py`、`console.py`、`config.py` 源码后提出改进建议，是典型的"用户主动深入代码后提出增强"的高质量反馈。此类 Issue 通常意味着真实的使用痛点，值得维护者优先评估。

- **#6940 feat(pawapp): add native DataPaw app runtime** — 附带 2 张截图展示新应用运行效果，属于视觉冲击力较强的 PR，容易吸引社区注意力。

整体来看，社区讨论集中在"长会话渲染性能"、"Matrix 加密可用性"、"后台任务通知"三大主题，分别对应 WebUI、多渠道接入、任务编排三个方向。

## Bug 与稳定性

| 严重程度 | Issue | 描述 | 状态 | Fix PR |
|---|---|---|---|---|
| P0 | [#7059](https://github.com/agentscope-ai/CoPaw/issues/7059) | `view_video` 工具结果对 OpenAI Responses API 静默丢弃，模型完全收不到视频帧，无错误无警告 | OPEN | [#7061](https://github.com/agentscope-ai/CoPaw/pull/7061) 已提交 |
| P0 | [#7060](https://github.com/agentscope-ai/CoPaw/issues/7060) | `view_video` 内联媒体限制硬编码为 2 MB，`max_inline_media_bytes` 设置对视频路径无效 | OPEN | [#7061](https://github.com/agentscope-ai/CoPaw/pull/7061) 已提交（部分修复） |
| P1 | [#7053](https://github.com/agentscope-ai/CoPaw/issues/7053) | OAuth2 刷新令牌永不轮换（rotating refresh_token 不持久化），远程 MCP 服务器永久降级为手动重新认证 | OPEN | 无 |
| P1 | [#7051](https://github.com/agentscope-ai/CoPaw/issues/7051) | Console 聊天中的图片附件在会话重新加载后丢失，前端显示损坏缩略图 | OPEN | 无 |
| P2 | [#7048](https://github.com/agentscope-ai/CoPaw/issues/7048) | `qwenpaw cron update --text` 返回成功但 prompt 实际未更新（agent 类型任务） | OPEN | [#7055](https://github.com/agentscope-ai/CoPaw/pull/7055) 已提交 |
| P2 | [#6476](https://github.com/agentscope-ai/CoPaw/issues/6476) | Matrix 端到端加密不可用（matrix-nio 需要 olm 库） | CLOSED（已解决） | — |

两个视频链路 Bug 均于 8 月 15 日报告，同日即有 fix PR 跟进，响应效率良好。但 #7053（OAuth2 轮换）和 #7051（图片附件丢失）尚无修复迹象，且均为影响核心功能（远程 MCP 认证、聊天记录完整性）的稳定问题，建议优先分配资源。

## 功能请求与路线图信号

- **#3915 Console WebUI 虚拟滚动** — 长会话性能问题的根治方案，已积压 3.5 个月。结合 [#7049 PR（/chats/{chat_id} 分页）](https://github.com/agentscope-ai/CoPaw/pull/7049)，后端加载机制已有对应 PR，若前端也落实虚拟滚动，可完整解决该体验问题，建议纳入下一版本。
- **#7056 后台任务回调/通知机制** — 用户明确指出现有 `submit_to_agent` + 轮询模式无法满足需求。这是流程编排场景的常见需求，与 agent 生态完善度直接相关，是潜在的路线图级功能。
- **#7058 恢复 native context 策略选项** — v2.1.0 移除了 WebUI 中的上下文策略选择器，但后端仍支持 `native` 策略。该 Issue 描述了一个"功能回归"场景——后端能力存在但 UI 不可达，修复成本预计较低，收益明确。
- **#7052 插件 API 增加 system_prompt 权限** — 企业用户希望在插件互动界面隐藏公司提示词，涉及权限模型调整和 UI 展示控制，属安全/隐私增强方向。
- **[PR #7033](https://github.com/agentscope-ai/CoPaw/pull/7033) 动态技能加载/自动卸载** — 已提交 PR，若合并将为技能系统带来运行时管理能力，是插件生态系统的重要基础设施。
- **[PR #7050](https://github.com/agentscope-ai/CoPaw/pull/7050) Cron 任务模型覆盖选择器** — 为每个 cron 任务增加独立模型选择，提升任务调度的灵活性，与 #7048（cron 更新失败）共同构成 cron 体系体验改进。

## 用户反馈摘要

从 Issue 描述和评论中可以提炼出以下用户痛点：

- **"命令成功但实际未生效"破坏信任感**（#7048）：`qwenpaw cron update --text` 返回 rc=0 且输出任务 JSON，但 prompt 从未更新。这种"假成功"极易造成生产事故，用户被迫通过 `cron list` 二次验证。修复 PR #7055 已提交，需尽快合并发布。
- **静默失败比报错更可怕**（#7059）：视频加载返回 "Video loaded" 但模型实际收不到任何帧——无错误无警告，用户难以排查。此类问题对 AI 应用的可靠性打击巨大，建议在工具结果路径中增加显式的传输确认/日志。
- **配置项不生效的困惑**（#7060）：`max_inline_media_bytes` 只影响图片路径，视频路径硬编码 2MB 限制（2097152 bytes）。用户在 issue 中精确指出代码逻辑矛盾，说明用户在排查上花费了大量精力。
- **企业用户对提示词隐私的关注**（#7052）：公司插件场景下，用户不希望 system_prompt 被终端用户看到，反映了 AI 应用在企业内部部署时对"模型指令与用户可见性"边界的真实需求。
- **用户主动研读源码提建议**（#7056、#7053）：多位用户引用具体文件、函数名和调用链来佐证问题，说明社区存在高质量的技术用户群体，维护者回应此类 Issue 可有效提升社区参与度。

## 待处理积压

- **[Issue #3915](https://github.com/agentscope-ai/CoPaw/issues/3915) 虚拟滚动**：2026-04-28 创建，已开放约 3.5 个月，获得社区反应但仍无对应 PR，是当前最长时间的待处理功能请求。建议明确排期或给出暂缓原因。
- **[PR #6302](https://github.com/agentscope-ai/CoPaw/pull/6302) provider 发现/模型元数据统一**：2026-07-21 创建，已开放近 1 个月，属于架构级改动，涉及面广可能评审周期长，但长期悬置会积累 merge 冲突成本，建议指定负责人推进。
- **[Issue #3915 相关 PR #7049](https://github.com/agentscope-ai/CoPaw/pull/7049) 聊天分页**：2026-08-15 由 first-time contributor 提交，是虚拟滚动的基础依赖，但尚无维护者响应。新贡献者的 PR 若久未回应可能打击贡献意愿。
- **[PR #6623](https://github.com/agentscope-ai/CoPaw/pull/6623) ACP 通知与响应竞态修复**：2026-08-01 创建，标注 "Under Review" 已两周，涉及 ACP 传输协议的通知/响应竞态问题，属于稳定性关键修复，建议加快评审。

---

*本日报基于 CoPaw 公开 GitHub 数据自动生成，数据时间范围：2026-08-15 至 2026-08-16。*

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目动态日报 — 2026-08-16

## 1. 今日速览

过去 24 小时项目保持极高活跃度：Issues 更新 50 条（新开/活跃 46，关闭 4），PR 更新 50 条（待合并 44，合并/关闭 6），无新版本发布。社区讨论焦点集中于多个大型架构 RFC，其中 OpenAI Chat Completions 兼容协议（#8603）以 20 条评论成为最热议题。重要的进展是 Anthropic 拒答/回退功能栈 5 个 PR 全部关闭（大概率已合并），为 provider 可靠性做了完整的闭环增强。需要关注的是大量高优先级修复 PR 仍处于 `needs-author-action` 或 `needs-maintainer-review` 状态，审查与作者响应节奏成为当前项目吞吐量的主要瓶颈。

---

## 2. 项目进展

### 2.1 Anthropic 拒答/回退功能栈完成合并

过去 24 小时关闭的 PR 中，可确认的 5 条均来自同一功能栈，由 `IftekharUddin` 提交，形成了一个从"原生拒答识别"到"客户端回退"再到"服务端回退"再到"用户可见提示"的完整链路：

- **#9262** `feat(providers): surface native anthropic refusals as typed errors` — 将 Anthropic HTTP 200 安全拒答（`stop_reason: "refusal"`）识别为类型化错误，取代原先将空拒答视为成功的错误处理。
- **#9263** `feat(providers): route refusals through client-side fallback entries` — 让既有的 client-side reliability 层消费该类型化错误，使拒答能够走向备选模型。
- **#9265** `feat(providers): opt-in anthropic server-side fallback requests` — 新增 `server_fallback_models` 配置，支持 Anthropic 单次 API 调用内的服务端模型兜底。
- **#9266** `feat(providers): detect anthropic server-side fallback responses` — 解析原生响应信号（`NativeChatResponse.model` 与 `AnthropicUsage.iterations`），识别实际由哪个模型服务了本轮对话。
- **#9268** `feat(channels): surface safeguard fallback notices` — 在信道编排层将 fallback 提示可见地暴露给用户。

该功能栈同时触及 `provider:anthropic`、`provider:reliable`、`channel:core`、`channel:acp` 与 CLI 等模块，意味着用户使用 Anthropic 模型时遭遇拒答（如安全回绝）将获得更加透明、可恢复的体验。这可能是近期最大的一次闭环功能合并。

### 2.2 关闭的 Issues

- **#4760**（`duplicate`）— 关于"使用 schema 校验的工具调用做记忆整合"的功能请求被标记为重复并关闭。
- **#7527**（`bug`，macOS 桌面空白窗口）— S1 严重度问题已关闭，推测已修复或确认 workaround，值得后续跟进确认。

---

## 3. 社区热点

### 3.1 最热议题：#8603 引入 OpenAI Chat Completions 协议（20 条评论）

**[#8603] RFC: ZeroClaw Chat Completions profile**（`OPEN`，`risk:high`，`needs-maintainer-review`）

该 RFC 提出为 ZeroClaw 增加 OpenAI Chat Completions 兼容协议层，让 Open WebUI、LobeChat、Continue.dev、Aider、LangChain 及 OpenAI SDK 等生态客户端可以直接接入 ZeroClaw。其本质诉求是：**将 ZeroClaw 的 Agent 能力暴露给现有 OpenAI 生态，降低接入门槛**。20 条评论说明社区对协议互操作性有强烈兴趣，这可能是项目从"自有 WebSocket/ACP 协议"走向"开放协议兼容"的重要信号。

### 3.2 运行时架构演进讨论集中爆发

- **#9487** `RFC: Runtime-owned conversation sessions and transport surface adapters`（17 条评论）— 提出将会话所有权收归运行时，并在传输面做适配层。涉及 ACP、Gateway、Web 等多个面。
- **#9488** `RFC: Unified attachment architecture for web chat and channels`（15 条评论）— 统一 Web 聊天与各信道的附件处理架构，跨 `tool`、`security`、`channel` 多个领域。
- **#8692** `[Tracker]: Maintainer decision queue for RFCs and design issues`（13 条评论）— 维护者决策队列本身已成为活跃讨论点，侧面说明社区对 RFC 决策效率存在焦虑。

**分析**：社区讨论呈现出明显的"平台化"倾向——外部生态兼容 + 内部架构统一。高层级设计 RFC 密集出现，但大量 RFC 停留在 `needs-maintainer-review`，决策通道的 throughput 可能成为社区贡献的瓶颈。

---

## 4. Bug 与稳定性

### 4.1 新报告 / 活跃 Bug

- **[#9965]** `cron custom-shell test hits ETXTBSY under the parallel runtime gate`（`P1`，`accepted`） — 测试基建不稳定：`cron::scheduler::tests::build_cron_shell_command_executes_with_custom_native_shell` 在并行运行时门禁下触发 `ETXTBSY` 竞争条件，导致无关 PR 被误标红。属于需要优先处理的测试基础设施问题。

- **[#7527]** `macOS desktop app can reopen blank or without a window`（`P1`，`S1`，已关闭） — 用户在 macOS 15.7.7 上安装后权限检测失效、响应丢失、重启后窗口消失。该问题已关闭，但严重度较高，建议确认修复方式与回归覆盖。

- **[#7870]** `agent runtime options can leak from first configured provider`（`P2`，`accepted` tracker） — Agent 与工具 provider 构造时可能错误解析配置文件中的第一个 provider 的运行时选项，而不是所选 provider 的。属于配置正确性问题，当前以 tracker 形式跟踪。

### 4.2 待合并的修复 PR

以下修复 PR 已提交但仍未合并，值得关注：

| PR | 问题 | 严重度 |
|---|---|---|
| **#9320** `fix(cron): bound agent job runs with a wall-clock timeout that releases the lock` | Cron agent job 无墙钟超时，provider 挂起时 sqlite `locked_at` 锁永远不释放 | P1 |
| **#9002** `fix(gateway): keep agent turns alive after viewer disconnect` | Dashboard WebSocket 断开时正在进行的 agent turn 被取消，浏览器休眠或网络波动会丢失工作 | P1 |
| **#9745** `fix(memory): add per-agent attribution and scoping to the knowledge graph` | 所有 agent 共享同一个知识图谱，任意 agent 可读取/修改其他 agent 的捕获知识与交互日志 | 安全高危 |
| **#9746** `fix(tools): per-agent ownership scoping for session tools and discord_search` | `sessions_*` 工具缺少所有权边界，越权列举、读取、发送会话 | 安全高危 |
| **#9995** `fix(hooks): harden webhook audit exports` | Webhook 审计导出未脱敏，凭证、provider token、内联图片标记会被写入审计参数 | 安全高危 |
| **#9954** `fix(sop): unwrap a double-encoded step output before schema validation` | 双重编码的 JSON 字符串被当作有效 JSON 早退，跳过了 schema 校验恢复逻辑 | P2 |
| **#9957** `fix(sop): record why a failed run failed` | `SopEngine::finish_run` 接收了失败原因却将其丢弃，调用方提供的失败原因无法持久化 | P2 |
| **#9229** `fix(runtime): make interactive Ctrl+C state-aware` | 交互式 REPL 中 Ctrl+C 依赖每 turn 的 listener，缺少统一的 `Idle/Active/Stopping` 状态生命周期 | P2 |

安全类修复（#9745、#9746、#9995）的高风险性质与它们的 `needs-author-action` 状态形成鲜明对比——建议维护者优先推动作者补全信息并进入审查队列。

---

## 5. 功能请求与路线图信号

### 5.1 最可能纳入下一版本的方向

- **OpenAI Chat Completions 兼容层（#8603）**：呼声最高，评论数达 20。若采纳，将打开 OpenAI 生态的客户端接入渠道。属于战略级功能，预计会走完整 RFC 流程，不太可能短期落地，但值得关注其推进速度。

- **Agent Plugins 1.0 标准支持（#9810）**：加载 `plugin.json + skills/ + mcp.json` 形式的社区插件包。这是面向生态建设的功能，符合项目跨平台 Agent 的定位。

- **AI-assisted PR pre-review（#9330）**：利用 CI 结果触发 AI 辅助评审，同时保留人类最终审批权。若落地将直接影响项目自身的开发效率。

- **PR 风险/尺寸标签自动化（#9867 PR + #9345 Issue）**：自动重算 `size:*` 与 `risk:*` 标签，当前 PR 已处于开放状态。属于维护流程优化，与 #9512（CI 门禁注释来源）一起体现项目对协作流程的重视。

### 5.2 其他活跃 RFC 信号

- **#8780** `Realtime speech-to-speech channel for Gemini Live` — 已在 2026-08-16 更新至 Rev 2，改为 broker 契约设计。实时语音信道是差异化功能，但复杂度高。
- **#6909** `Computer-use support for desktop screen interaction` — 需要作者行动，已等待 80+ 天，推进缓慢。
- **#9621** `Staged opt-in product telemetry` — 维护者希望了解已发布功能的真实使用率，为取舍决策提供数据。
- **#9598** `Define the SOP capability permission contract` — SOP 权限契约 Rev 3 已就绪，面向 v0.9.0 目标。
- **#7108** `Improve cached Rust builds and CI critical path` — CI 基础设施优化，已接受，等待推进。

---

## 6. 用户反馈摘要

- **来自 #8603**：用户明确表示希望使用 Open WebUI、LobeChat、Continue.dev、Aider、LangChain 与 OpenAI SDK 作为 ZeroClaw 的客户端。当前仅 WebSocket/ACP/webhook 的暴露方式将大量现有工具链挡在门外。这是集成诉求最直接、最强烈的一条。

- **来自 #9825**：用户反馈 outbound leak detector 将**公共区块链地址**识别为高熵机密并删改，导致支付请求 URL 无法投递。用户指出这是"误报而非 bug"——检测器按设计工作，但设计本身忽略了公开区块链标识符这一合法场景。体现安全功能需要更精细的上下文感知。

- **来自 #7762**：用户指出 Cron 文档完全缺失，且无法为 cron 任务指定特定模型——"想用小任务跑最便宜的 agent（如 gemma）"，目前无法配置。是明确的文档 + 配置功能缺口。

- **来自 #9103 评论**：维护者在讨论中直接询问 "Lucid 或 Qdrant 是否有真实生产环境使用"，以决定如何设计 `memory.backend`。这反映了维护者对实际部署情况的信息缺口，社区用户如在使用应向该 issue 留言。

- **来自 #7527**：macOS 桌面用户遭遇权限检测失效、应用空白/无窗口的 S1 问题，属于直接影响交付的痛点，该问题已关闭，但如果用户仍可复现，值得继续追踪。

---

## 7. 待处理积压

以下是长期未解决、需要维护者或作者关注的重点事项：

### 7.1 等待维护者决策的长期 RFC

- **#6954** `RFC: Provenance, conversation binding, and reply contract for internally initiated agent turns`（创建 2026-05-26，已等待 **82 天**，`needs-maintainer-review`）— 内部发起的 Agent 轮次的来源追踪与会话绑定契约。
- **#6971** `RFC: Security posture, credential boundaries, and universal ingress policy`（创建 2026-05-27，已等待 **81 天**，`needs-maintainer-review`）— 安全态势与凭证边界的总纲性 RFC，与多个安全子项关联。
- **#6909** `RFC: Computer-use support for desktop screen interaction`（创建 2026-05-25，已等待 **83 天**，`needs-author-action`）— 作者长时间未响应，若不再推进建议明确关闭或移交。

### 7.2 决策队列本身

- **#8692** `[Tracker]: Maintainer decision queue for RFCs and design issues`（创建 2026-07-04，13 条评论）— 这是项目当前决策机制的"待办看板"。它的存在说明设计讨论的积压已经成为一个需要专门跟踪的问题。建议优先为队列中的每个 RFC 给出明确的接受/拒绝/延后决定。

### 7.3 高优先级修复 PR 等待合并

- **#9320**（P1，cron 锁不释放）与 **#9002**（P1，viewer 断开取消 turn）是用户可直接感知的稳定性修复，合并优先级应高于新功能。
- **#9137** `feat(plugins): add shared egress policy foundation` 依赖 **#9580**，其头部携带 #9580 的 commit，需要先处理依赖 PR。
- **#9002 / #9745 / #9746** 这三个 PR 均为高风险的修复，涉及网关/内存/工具层的安全问题，建议维护者重点 review。

---

## 8. 总结与健康度评估

**项目健康度：活跃但存在决策积压。**

- **优势**：社区讨论质量高，RFC 有清晰的修订历史与状态管理；贡献者体系成熟（`distinguished contributor` 标签出现频繁）；安全与风险分级意识强（`risk:high`、`security:*` 标签使用规范）；CI 自身也在持续优化（#7108、#9512、#9867）。
- **风险**：大量 80+ 天未决策的高风险 RFC 停留在 `needs-maintainer-review`；高优先级修复 PR 合入速度慢于新功能开发；无新版本发布意味着已完成的功能栈（如 Anthropic 拒答回退）尚未触达最终用户。
- **建议**：维护者应优先处理 #8692 决策队列，对长期未决 RFC 给出明确结论；推动 #9320、#9002、#9745、#9746 等 P1/安全修复尽快合入；关注 #8603 的协议兼容讨论，判断是否将其放入下一版本路线图。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*