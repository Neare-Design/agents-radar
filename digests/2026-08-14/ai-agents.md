# OpenClaw 生态日报 2026-08-14

> Issues: 500 | PRs: 500 | 覆盖项目: 12 个 | 生成时间: 2026-08-13 23:34 UTC

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

# OpenClaw 项目动态日报 — 2026-08-14

## 1. 今日速览

过去 24 小时，OpenClaw 仓库迎来极高活跃度：共产生 500 条 Issue 更新（新开/活跃 340，关闭 160）与 500 条 PR 更新（待合并 416，已合并/关闭 84），无新版本发布。值得关注的是，Issue 侧呈现“老问题长期盘踞 + 新问题快速浮现”的双轨态势——发生在 8 月 9 日的静默回复失败问题（#121058）在 3 天内积累 92 条评论，成为当日讨论度最高的话题；与此同时，本周新开的更新通道故障（#123073）迅速获得 6 条评论与 `fix-shape-clear`、`queueable-fix` 标记，表明维护者已介入。PR 侧今日有 20+ 条新提交，覆盖 Docker 镜像刷新、Gemini 3.7 Flash 支持、cron 冷启动阻塞修复等方向，合并/关闭 84 条 PR 显示清理节奏尚可，但 416 条待合并 PR 的积压规模仍提示合并瓶颈。综合来看，项目处于高吞吐的迭代期，但消息投递可靠性、子代理（subagent）完成丢失、会话状态管理三类问题构成当前最突出的稳定性短板。

## 2. 版本发布

过去 24 小时无新版本发布（Releases: 0）。建议关注 dev 通道的更新故障（#123073，见 Bug 与稳定性部分），其可能影响后续版本分发。

## 3. 项目进展

过去 24 小时共有 84 条 PR 被合并或关闭，其中多条直接解决用户可感知的问题：

**功能推进**

- **Gemini 3.7 Flash 支持** — PR #123366 将 `gemini-3.7-flash`（1,048,576-token 上下文，65,536 最大输出，支持 thinking）加入官方静态模型目录，用户无需依赖 live discovery 即可使用，并带有 LOW thinking floor 的 curated 配置。
- **Docker 镜像定期刷新** — PR #123348 实现了 moving tags（`latest*`、`main*`、`extended-stable*`）的每周自动重建，解决 Debian 点版本安全修复和 npm CLI 更新在两次 stable 发布之间无法触达镜像的问题。
- **Web UI 全屏模式** — PR #123278 为 Desktop preview 增加全屏模式，关闭 #122989。
- **插件静态清单暴露** — PR #122284 在 `openclaw status` 中暴露插件静态清单，方便用户与维护者排查插件状态。

**可靠性修复**

- **cron 冷启动阻塞** — PR #123368 修复 Gateway 启动后首个 isolated 定时任务可能物化完整模型目录、阻塞 Gateway 的问题（关联 #120834）。
- **session 列表缓存失效** — PR #123290 修复 transcript commits 后 `sessions.list` 缓存仍提供陈旧预览的问题。
- **设备加入路由测试隔离** — PR #123370 修复 Gateway CI 中偶发的 device-join 路由测试失败。
- **测试重复清理** — PR #123214 和 #123168 分别清理了 11 个重复/无差异的测试以及 4 处残留的 test-only seam，显示维护者在持续加固 CI 基础设施。

整体来看，项目今日在“模型支持广度”和“基础设施健壮性”两个维度均有实质推进，但大量合并集中在维护者自身的小型修复，社区贡献的较大功能（如 #96113 sessions diagnose、#103648 TUI shell 持久化）仍处于等待状态。

## 4. 社区热点

**#121058 [OPEN] Silent reply failures still recurring — 92 条评论**

链接: https://github.com/openclaw/openclaw/issues/121058

8 月 9 日创建，3 天内成为全仓库讨论热度最高的 Issue。用户 `sloptop-the-terrible` 报告：此前标记为已关闭的静默回复失败问题（#116277）实际并未修复，监控 cron 在关闭后仍持续记录新发生事件。92 条评论表明大量用户可能受同一问题困扰，且对“关闭但未修复”的处理方式表达不满。该 Issue 目前无 `fix-shape-clear` 或 `queueable-fix` 标记，亟需维护者明确回应。

**#7707 [OPEN] Memory Trust Tagging by Source — 48 条评论**

链接: https://github.com/openclaw/openclaw/issues/7707

2 月 3 日创建的老 Issue 仍在持续获得讨论。核心诉求是对记忆条目按来源（用户命令、网页抓取、第三方技能）打信任标签，防止恶意指令通过不可信内容污染记忆、进而影响后续行为。带有 `needs-product-decision` 和 `needs-security-review` 标记，属于安全相关的产品决策项。

**#25592 [OPEN] 工具调用间文本泄漏到消息渠道 — 48 条评论**

链接: https://github.com/openclaw/openclaw/issues/25592

Agent 在工具调用之间产生的内部处理文本（错误处理、处理确认、叙述）会被路由到 Slack、iMessage 等活动消息渠道，造成显著的 UX 问题。该 Issue 带 `linked-pr-open` 标记，对应 PR #96969（丢弃中间独白文本块，见下）。

**#44925 [OPEN] 子代理完成静默丢失 — 27 条评论**

链接: https://github.com/openclaw/openclaw/issues/44925

子代理任务编排存在多个静默失败模式：完成通知失败、超时无重试、无自动重启。用户认为这属于 P1 级别的数据/消息丢失问题。

**社区诉求分析**

当日热点集中在三个方向：**消息投递可靠性**（#121058、#25592、#44925）、**记忆安全**（#7707）、**子代理会话隔离**（#96975、#47975）。这些问题的共同特征是：用户的核心痛点不是“功能缺失”，而是“已有功能在真实场景下静默失败且难以排查”。值得注意，#121058 的高热度可能反映社区对问题闭环流程的不满——Issue 被关闭但故障依旧，这会消耗项目信任度。

## 5. Bug 与稳定性

按严重程度排列（P1 最高）：

### P1 — 消息/数据丢失类

| Issue | 标题 | 状态 | 关联 PR |
|---|---|---|---|
| [#121058](https://github.com/openclaw/openclaw/issues/121058) | 静默回复失败在 #116277 关闭后仍复现 | OPEN，92 评论 | 无 |
| [#44925](https://github.com/openclaw/openclaw/issues/44925) | 子代理完成静默丢失，无重试/通知/自动重启 | OPEN，27 评论 | 无 |
| [#67777](https://github.com/openclaw/openclaw/issues/67777) | 子代理完成投递在 direct-announce 超时/drain/orphan prune 时丢失 | OPEN，10 评论 | 无 |
| [#92433](https://github.com/openclaw/openclaw/issues/92433) | 子代理完成被 steer 进 requester run 后因 run 结束而丢弃 | OPEN，9 评论 | 无 |
| [#91363](https://github.com/openclaw/openclaw/issues/91363) | Isolated cron 持续报 "LLM request failed"（usage.input=0） | OPEN，10 评论，6 👍 | 无 |
| [#121605](https://github.com/openclaw/openclaw/issues/121605) | 模型 fallback 后回复生成成功但未投递到渠道 | **已关闭** | 无（close 原因待查） |

### P1 — 会话阻塞/状态类

| Issue | 标题 | 状态 | 关联 PR |
|---|---|---|---|
| [#47975](https://github.com/openclaw/openclaw/issues/47975) | 子代理会话完成后持久化，主会话无响应 | OPEN，10 评论 | 无 |
| [#54488](https://github.com/openclaw/openclaw/issues/54488) | Followup drain 独占 session lane，入站分发阻塞 20-30 分钟 | OPEN，6 评论 | 无 |
| [#43367](https://github.com/openclaw/openclaw/issues/43367) | 多代理编排不稳定：并发 add/config 覆盖、session-lock 失败 | OPEN，13 评论 | 无 |
| [#43374](https://github.com/openclaw/openclaw/issues/43374) | 4 个代理并发时所有 LLM API 调用同时超时 | OPEN，6 评论 | 无 |
| [#121953](https://github.com/openclaw/openclaw/issues/121953) | DeepSeek 上 cron agent 因 `[cron:` 前缀被降优先级而 stall | OPEN，16 评论 | 无 |
| [#123368](https://github.com/openclaw/openclaw/pull/123368) | cron 冷启动阻塞 Gateway | **PR 已提交**，待合并 | 关联 #120834 |

### P1 — 其他

| Issue | 标题 | 状态 | 关联 PR |
|---|---|---|---|
| [#72015](https://github.com/openclaw/openclaw/issues/72015) | active-memory 插件阻塞回复 + QMD 启动过载 | OPEN，10 评论 | 无 |
| [#89278](https://github.com/openclaw/openclaw/issues/89278) | Codex OAuth 刷新成功但 cron/heartbeat 10s 超时 | OPEN，9 评论 | 无 |
| [#78493](https://github.com/openclaw/openclaw/issues/78493) | `sudo openclaw update` 导致混合属主，doctor 覆盖配置 | OPEN，7 评论 | 无 |
| [#95553](https://github.com/openclaw/openclaw/issues/95553) | 预检压缩硬编码 ~60s 上限，忽略 `compaction.timeoutSeconds` | OPEN，7 评论 | 无 |
| [#115421](https://github.com/openclaw/openclaw/issues/115421) | Schema 降级恢复隔离/清除状态 DB（cron 任务丢失） | OPEN，6 评论 | 无 |
| [#123073](https://github.com/openclaw/openclaw/issues/123073) | dev 通道更新失败：`workspace:*` 协议导致 EUNSUPPORTEDPROTOCOL | OPEN，6 评论，`fix-shape-clear` + `queueable-fix` | 无 |

### P2 — 回归与行为异常

| Issue | 标题 | 状态 |
|---|---|---|
| [#43747](https://github.com/openclaw/openclaw/issues/43747) | 记忆管理混乱：不同用户的存储方式不一致 | OPEN，11 评论 |
| [#111498](https://github.com/openclaw/openclaw/issues/111498) | Anthropic 认证恢复后主代理被 workspace-state 迁移阻塞 | OPEN，8 评论 |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | 泄漏未回收的 hook/tool 子进程（zombie 累积） | OPEN，7 评论 |
| [#111944](https://github.com/openclaw/openclaw/issues/111944) | Codex commentary 不投递到 Telegram 进度/block 流 | OPEN，6 评论 |
| [#95759](https://github.com/openclaw/openclaw/issues/95759) | ACP sessions_spawn 注册成功但 0 字节 transcript | OPEN，6 评论 |
| [#107814](https://github.com/openclaw/openclaw/issues/107814) | gpt-5.3-codex-spark 对必需工具调用发出空参数 | OPEN，7 评论 |
| [#91456](https://github.com/openclaw/openclaw/issues/91456) | Telegram DM lane 在 send timeout 后持续 guarded | **已关闭** |
| [#105342](https://github.com/openclaw/openclaw/issues/105342) | exec 命令输出在 Telegram 上渲染为图片而非文本 | **已关闭** |

**已有关联 PR 的修复进展**

- **#25592（文本泄漏）** — PR #96969 已提交：在共享的 assistant-text 提取层丢弃中间 monologue 文本块，同时影响 Discord 等渠道。状态 `needs proof`，尚无维护者明确批准。
- **#41165（Telegram DM 路由污染）** — 带 `linked-pr-open` 标记，PR 尚未合入。
- **#89278（Codex OAuth 超时）** — 带 `linked-pr-open` 标记，PR 尚未合入。
- **#91456（Telegram DM lane）** — 今日已关闭，修复方式待查。
- **#121605（fallback 后不投递）** — 今日已关闭，但未标注关联 PR，需确认是否真正修复。

**新提交的修复 PR（今日）**

- **#123368** — cron 冷启动阻塞 Gateway 修复，P1，`platinum hermit` 评级。
- **#123290** — sessions.list 缓存陈旧修复，P2。
- **#123274** — Web UI 丢弃附件读取失败无提示修复，P2。
- **#123344** — Portals 页面将 CSP 拒绝误报为不可达，P2。
- **#123339** — 显式 agent 属主不再导致系统界面（Custodian/Skills/Meet）失败，P2。

**评价**：消息投递可靠性问题（子代理完成丢失/静默失败）是当前最大的稳定性债务，至少 5 个 P1 Issue 指向同一类根因（`subagent-announce-delivery.ts` 的投递链路在超时/steer/drain 场景下缺少可靠回退）。此类问题长期未得到根治，且 #121058 显示“修复未生效”的情况，建议维护者考虑对 subagent 完成投递链路做一次系统性重构而非点状修复。

## 6. 功能请求与路线图信号

### 高潜力（已有 PR 或维护者标记）

| Issue/PR | 内容 | 信号 |
|---|---|---|
| [#7707](https://github.com/openclaw/openclaw/issues/7707) | 记忆来源信任标记（防止记忆投毒） | 48 评论，`needs-product-decision`，安全相关 |
| [#25592](https://github.com/openclaw/openclaw/issues/25592) | 工具调用间文本不投递到消息渠道 | 对应 PR #96969 已提交 |
| [#96975](https://github.com/openclaw/openclaw/issues/96975) | 子代理完成默认只返回状态 + child session 链接，不注入父上下文 | 11 评论，与多个 P1 bug 同源 |
| PR [#96113](https://github.com/openclaw/openclaw/pull/96113) | 新增只读 `sessions diagnose` 命令，排查卡死/静默/排队会话 | 大型功能 PR，P1，覆盖 iOS/Android/Web/CLI，`needs proof` |
| [#45771](https://github.com/openclaw/openclaw/issues/45771) | 内置 pace-aware 速率限制（防 LLM API 限流烧穿） | 7 评论，与 #43374（并发超时）呼应 |
| [#16555](https://github.com/openclaw/openclaw/issues/16555) | 投递队列消息 TTL/过期机制 | 6 评论，与 #121058 静默失败问题直接相关 |

### 中期关注

| Issue | 内容 | 备注 |
|---|---|---|
| [#45758](https://github.com/openclaw/openclaw/issues/45758) | YAML 配置格式支持 | P3，9 评论 |
| [#45508](https://github.com/openclaw/openclaw/issues/45508) | WebChat 自托管 STT/TTS（经 Gateway 而非浏览器 API） | P2，7 评论，2 👍 |
| [#9016](https://github.com/openclaw/openclaw/issues/9016) | 向 agent runtime 暴露 OpenRouter 用量成本 | P2，8 评论 |
| [#42276](https://github.com/openclaw/openclaw/issues/42276) | 流式推理/思考过程显示（类 OpenAI/Grok） | P3，6 评论 |
| [#46058](https://github.com/openclaw/openclaw/issues/46058) | 独立 chat-first Android 客户端 | P3 讨论，6 评论 |
| [#41366](https://github.com/openclaw/openclaw/issues/41366) | 持久化自然语言规则学习 + 多提及回复语义 | P3，7 评论 |
| [#51028](https://github.com/openclaw/openclaw/issues/51028) | 会话面板按“最后有意义活动”排序 | P3，6 评论 |
| [#45501](https://github.com/openclaw/openclaw/issues/45501) | `session.resetPrompt` 可配置化 | P2，6 评论 |

**路线图判断**：最可能进入下一版本的是 #25592 的修复（PR #96969 已就绪，只差 proof + maintainer review）和 #121953 的 cron 前缀问题（DeepSeek 特定场景，修复成本低、收益明确）。#7707（记忆信任标签）作为安全增强，需要产品决策，中期可能随记忆模块重构一并落地。值得关注的是 PR #123105（用 managed llama-server 替换 node-llama-cpp）——这是一个 XL 规模、P1、带 `dependencies-changed` 的架构级改动，说明维护者在推进本地模型运行时的现代化。

## 7. 用户反馈摘要

### 最痛的点：静默失败

> “#116277 被关闭了，但静默回复失败仍在继续。监控 cron 在 Issue 关闭后持续记录新发生事件——包括今天（2026-08-09）的一次。”
> — [#121058](https://github.com/openclaw/openclaw/issues/121058)

> “子代理任务编排存在多个静默失败模式：完成通知失败、超时无重试、无自动重启。”
> — [#44925](https://github.com/openclaw/openclaw/issues/44925)

**诉求本质**：用户对“失败但无感知”的容忍度极低。多个 Issue 均强调“没有错误输出、没有日志、没有重试”——这类问题比明确报错更伤害信任。**建议**：至少在日志中输出可排查的错误码，并在投递失败时提供告警钩子。

### 多代理场景不可靠

> “我从 CLI 尝试编排一个小的并行编码批次，遇到了一连串故障：`openclaw agents add` 并发调用不安全、配置被反复覆盖、session 锁失败、子任务脱离。”
> — [#43367](https://github.com/openclaw/openclaw/issues/43367)

> “4 个代理并发时所有 LLM API 调用同时超时——同一时刻用 curl 请求同样的 API 完全正常。”
> — [#43374](https://github.com/openclaw/openclaw/issues/43374)

### 记忆管理困惑

> “我们 3 个人都在用 openclaw。我从未看到任何人的记忆以相同方式管理：我的在做 chunking/embedding 存到 `main.sqlite`；同事 A 的 claw 存在别处……”
> — [#43747](https://github.com/openclaw/openclaw/issues/43747)

### 对配置/更新流程的吐槽

> “`sudo openclaw update` 升级成功了，但也把状态/配置/插件文件的属主搞成了 root/user 混合。之后普通用户跑 `openclaw doctor` 遇到 EACCES……”
> — [#78493](https://github.com/openclaw/openclaw/issues/78493)

### 正面反馈

- Todo 今日合并的 #123366（Gemini 3.7 Flash）和 #123348（Docker 定期刷新）都是社区长期请求的功能，二者合入应获好评。
- #123290 修复了 `sessions.list` 缓存陈旧问题，表明维护者在认真处理用户可见的 UI/会话一致性问题。

### 值得注意的“关闭但未解决”风险

- #121605 今日关闭（模型 fallback 后不投递），但无关联 PR，需要确认是否真正修复。
- #42273（backup create 大目录 stall）标记为 `close:already-fixed` 关闭。
- #85714（agent 忘记调用投递工具导致消息 stranded）今日关闭，但该 Issue 带 `linked-pr-open` 标记，关闭原因需复查。

## 8. 待处理积压

### 高危（P1，长时间无有效修复 PR）

| Issue | 创建时间 | 持续天数 | 备注 |
|---|---|---|---|
| [#43367](https://github.com/openclaw/openclaw/issues/43367) 多代理编排不稳定 | 2026-03-11 | 156 天 | 并发 config 覆盖 + session-lock 失败 + 子任务脱离 |
| [#47975](https://github.com/openclaw/openclaw/issues/47975) 子代理会话完成后主会话无响应 | 2026-03-16 | 151 天 | 无 PR |
| [#54488](https://github.com/openclaw/openclaw/issues/54488) 会话 lane 饥饿，入站阻塞 20-30 分钟 | 2026-03-25 | 142 天 | 无 PR |
| [#67777](https://github.com/openclaw/openclaw/issues/67777) 子代理完成投递在多条件下丢失 | 2026-04-16 | 120 天 | 无 PR |
| [#91363](https://github.com/openclaw/openclaw/issues/91363) Isolated cron 稳定失败（LLM request failed） | 2026-06-08 | 67 天 | 6 👍，无 PR |
| [#92433](https://github.com/openclaw/openclaw/issues/92433) 子代理完成 steer 后丢失 | 2026-06-12 | 63 天 | 无 PR |
| [#97983](https://github.com/openclaw/openclaw/issues/97983) iOS/WebChat 消息不触发回复 | 2026-06-30 | 45 天 | `diamond lobster` 评级，无 PR |

### 中危（P1/P2，有 PR 但长期未合入）

| Issue/PR | 创建时间 | 持续天数 | 备注 |
|---|---|---|---|
| PR [#82540](https://github.com/openclaw/openclaw/pull/82540) 微信热重载保留账号 | 2026-05-16 | 90 天 | 大型 PR（XL），`needs proof` |
| PR [#82023](https://github.com/openclaw/openclaw/pull/82023) Telegram 子代理绑定 forum topic | 2026-05-15 | 91 天 | 大型 PR（XL），`needs proof`，覆盖多渠道 |
| PR [#96969](https://github.com/openclaw/openclaw/pull/96969) 丢弃中间独白文本 | 2026-06-26 | 49 天 | 直接修复 #25592，P1，`needs proof` |
| PR [#96113](https://github.com/openclaw/openclaw/pull/96113) sessions diagnose 命令 | 2026-06-23 | 52 天 | P1，大型功能，`needs proof` |
| [#89278](https://github.com/openclaw/openclaw/issues/89278) Codex OAuth 10s 超时 | 2026-06-02 | 73 天 | `linked-pr-open`，但 PR 未合入 |

### 维护者提醒

1. **subagent 完成投递链路**是当前最大的技术债集中地——#67777、#92433、#44925、#47975、#96975、#121605 均指向同一 subsystem。建议成立专项，参照 #96975 的“隔离子代理完成内容”方向做一次架构简化。
2. **#121058** 的高评论量（92 条）已经构成社区信任风险，即使根因复杂，也建议至少给出明确的状态更新和排查计划，避免“关闭即失联”的观感。
3. **PR 积压**：416 条待合并 PR 中，多带有 `needs proof` 或 `waiting on author` 标记。建议对超过 60 天无更新的 PR 做一次批量 triage，关闭僵尸 PR 或明确下一步动作。
4. **#123073**（dev 通道更新失败）已经标记为 `fix-shape-clear` + `queueable-fix`，这是本周新开的高优先级问题，影响所有 dev 通道用户，建议尽快合入修复。

---

*报告生成时间：2026-08-14 | 数据来源：github.com/openclaw/openclaw | 统计周期：过去 24 小时*

---

## 横向生态对比

# 个人 AI 助手/自主智能体开源生态横向对比报告

**报告日期：2026-08-14**
**数据来源：各项目 GitHub 仓库过去 24 小时动态**

---

## 1. 生态全景

个人 AI 助手/自主智能体开源生态已进入 **高吞吐迭代与稳定性债务并存的阶段**。核心项目普遍保持极高的 Issue/PR 更新量（OpenClaw 单日各 500 条，Hermes/ZeroClaw/IronClaw 各 50 条），但讨论焦点已从“功能缺失”全面转向“已有功能在真实场景下静默失败且难以排查”。供应链安全（镜像签名、CI 门禁、CSPRNG）、记忆与上下文工程（信任标记、无损整合）、以及模型成本可观测性成为跨项目共同主题。同时，多个项目通过发布版本（Hermes v0.20.1、NanoClaw v2.2.0、IronClaw v1.2.0、CoPaw v2.1.0）或架构级重构（IronClaw Pluggable agent loops）推进能力边界。整体生态健康但分化明显：头部项目维持高吞吐，尾部项目进入依赖维护或停滞状态。

## 2. 各项目活跃度对比

| 项目 | Issue 更新（开/关） | PR 更新（待/合） | Release | 健康度评估 |
|---|---|---|---|---|
| **OpenClaw** | 500（340 开 / 160 关） | 500（416 待 / 84 合） | 无 | 极高活跃，但 P1 稳定性债务集中（子代理投递丢失、静默回复失败），合并瓶颈显著 |
| **Hermes Agent** | 50（45 开 / 5 关） | 50（42 待 / 8 合） | **v0.20.1** | 极高活跃，P1 TUI 核心流程连续 13 天未修复，Windows 问题密集 |
| **ZeroClaw** | 50（37 开 / 13 关） | 50（43 待 / 7 合） | 无 | 高活跃，安全加固进展快（路径穿越、凭证完整性），但 RFC 决策与 PR 评审瓶颈明显 |
| **IronClaw** | 50 | 50（26 合） | **v1.2.0** | 健康且高产，性能优化与架构规划并行，社区贡献者参与积极 |
| **CoPaw** | 42（25 开 / 17 关） | 50（31 待 / 19 合） | **v2.1.0 + beta.5** | 高活跃，功能推进快，但安全反馈（端口暴露、插件权限）与任务中断问题需重点跟进 |
| **NanoBot** | 13（12 开 / 1 关） | 31（22 待 / 9 关） | 无 | 健康，缺陷响应链路通畅，cron 修复三版迭代体现质量把控 |
| **NanoClaw** | 2 | 19（6 待 / 13 合） | **v2.2.0** | 高产出，供应链安全 CI 门禁闭环，阻断性 Bug 响应 <24h |
| **LobsterAI** | 1 | 6 合 | 无 | 短期迭代良好，但 5 条 PR 积压超 4 个月（含安全关键测试） |
| **Moltis** | 1 | 4（0 合） | 无 | 稳定但停滞，3 个迁移修复 PR 待合入，无合并动作 |
| **PicoClaw** | 3 | 9（6 待 / 3 stale 关） | 无 | 低活跃，依赖 Dependabot 驱动，Web UI 长会话卡顿近一个月未响应 |
| **NullClaw / ZeptoClaw** | 无活动 | 无活动 | — | 停滞 |

## 3. OpenClaw 在生态中的定位

- **社区规模绝对领先**：单日 500 Issue + 500 PR 的更新量是第二梯队（Hermes/ZeroClaw/IronClaw 各 50）的 10 倍，评论热度最高 Issue 达 92 条。其生态位相当于“个人 AI 助手领域的 Kubernetes”——基础设施级项目，众多衍生项目（LobsterAI、Moltis、NanoClaw 等）均基于其代码库二次开发。

- **技术路线：通用型“智能体操作系统”**。与其他项目相比，OpenClaw 覆盖最广：多渠道（Slack/Telegram/iMessage）、多模型（Gemini/DeepSeek/Codex）、子代理编排、插件系统、Docker 镜像分发。这种广度使其面临其他项目少见的**系统性复杂度和技术债**：至少 5 个 P1 Issue（#67777、#92433、#44925、#47975、#121605）指向同一根因——`subagent-announce-delivery.ts` 投递链路在超时/steer/drain 场景下缺少可靠回退。

- **对比与差异化**：
  - **Hermes Agent** 更偏桌面端（TUI/Desktop），MCP 集成深入，但 Windows 体验短板明显；
  - **ZeroClaw** 以 RFC 治理和安全加固见长，代码严谨性高于 OpenClaw，但功能推进速度慢；
  - **IronClaw** 走“内核化/可插拔 harness”路线，架构愿景宏大但尚处执行早期；
  - **CoPaw** 偏任务自动化与国内模型/云生态（阿里云 token plan）；
  - **NanoClaw** 专注供应链安全与 Agent 模板/插件标准。

OpenClaw 的核心优势是 **生态吸附能力**——Issue/PR 讨论量、第三方集成、渠道适配数量均领先；核心劣势是**可靠性欠账积累正在消耗社区信任**（#121058 “关闭但未修复”触发 92 条评论即可见一斑）。

## 4. 共同关注的技术方向

### ① 任务连续性与中断恢复
| 项目 | 具体诉求 |
|---|---|
| OpenClaw | 静默回复失败复现（#121058）、子代理完成静默丢失（#44925） |
| Hermes | verification-stop 丢弃流式最终答案与 cron 报告（#62142）、TUI 核心工作流不可用（#69592） |
| CoPaw | 多步任务频繁自行中断，需手动“继续”（#6921） |
| NanoBot | cron 调度器在单次持久化失败后永久死亡（#5373） |
| ZeroClaw | headless SOP step turn 有 session path 但从未持久化（#9929） |

**共性本质**：用户对“失败但无感知”的容忍度极低；多个项目均存在“任务执行中断/结果丢失且无重试、无告警”的问题。

### ② 记忆与上下文管理
| 项目 | 具体诉求 |
|---|---|
| OpenClaw | 记忆来源信任标记防投毒（#7707）、多用户记忆存储方式不一致（#43747） |
| CoPaw | 记忆文档与行为不符（#6853）、外部记忆方案 ViBo 提案（#7003） |
| ZeroClaw | 记忆生命周期策略与存储后端解耦（#6850）、跨模型 JSON 解析脆弱（#6998） |
| Hermes | 本地优先 Memory Provider 提案（#85418） |
| NanoBot | 记忆整合截断导致静默丢失（#5377） |

**共性本质**：从“记忆有没有”进入“记忆如何被信任、被无损管理、被低成本调用”阶段。

### ③ 模型路由与成本控制
| 项目 | 具体诉求 |
|---|---|
| OpenClaw | 4 代理并发时所有 LLM 调用超时（#43374） |
| Hermes | cron 钉死已死模型连续 402（#85215）、无 repin 路径（#70050） |
| ZeroClaw | OpenRouter 无稳定 session_id 导致 prompt cache 失效（#9631） |
| CoPaw | 阿里云百炼 token plan 支持（#6973） |
| NanoBot | MCP 工具 schema 上下文预算（#5298） |
| IronClaw | 每轮 Postgres 写放大度量（#7630） |

**共性本质**：多模型/多 Provider 策略下，**成本可观测性、缓存友好性、failover 可靠性**成为新刚需。

### ④ 供应链与执行安全
| 项目 | 具体诉求 |
|---|---|
| NanoClaw | 配对码用 Math.random()（#3229）、镜像签名验证被静默跳过（#3158）、CI 门禁（#3238） |
| ZeroClaw | Zhipu 凭证转发原始 token（#9968）、dashboard 路径穿越（#9969）、未认证配对接口锁定绕过（#9389） |
| CoPaw | 8088 端口公网暴露/API 无鉴权（#6992/#6993） |
| Hermes | 硬编码 OSV endpoint 防绕过（#6722）、跨主机 redirect 剥离 Bearer（#63826） |

**共性本质**：供应链攻击面（镜像、依赖、配对码、凭证处理）成为全生态一致的高优先级议题。

### ⑤ 渠道一致性与消息投递
| 项目 | 具体诉求 |
|---|---|
| OpenClaw | 工具调用文本泄漏到 Slack/iMessage（#25592）、fallback 后消息不投递（#121605） |
| CoPaw | Matrix 群组所有成员共享同一上下文（#7001）、Telegram /new 不轮换 session（#6966） |
| Hermes | Signal 缺引用/回复/编辑/已读回执（#39043） |
| NanoBot | Telegram 贴纸收发支持（#5387） |

**共性本质**：渠道适配从“能收发消息”走向“原生交互完整性”和“会话级隔离”。

## 5. 差异化定位分析

| 项目 | 功能侧重 | 目标用户 | 技术架构关键差异 |
|---|---|---|---|
| **OpenClaw** | 全渠道个人 AI 助手、子代理编排、插件生态 | 开发者、个人重度用户 | 单体高集成，Golang 后端 + 广泛适配层 |
| **Hermes Agent** | 桌面端（TUI/Desktop）、MCP 深度集成 | macOS/桌面开发者 | 本地优先，Python 生态，路由/认证模型丰富 |
| **ZeroClaw** | 安全加固、RFC 治理、可验证意图 | 企业、安全敏感团队 | Rust，严格准入契约 + 配置/行为一致性审计 |
| **IronClaw** | 可插拔 agent loops、云/边缘编排 | 云部署、多租户场景 | 内核化思路，Harness 抽象，Postgres-heavy |
| **CoPaw** | 任务自动化、OS Shell、国内云模型 | 中文用户、自动化场景 | 服务端强制 max_iterations、app center 统一目录 |
| **NanoBot** | 轻量级多渠道网关、MCP + 渠道补齐 | 轻部署个人/团队 | 模块化较小，cron/session 层可靠性修复活跃 |
| **NanoClaw** | Agent 模板与插件标准、供应链 CI | 平台运营方 | 镜像验证→签名审批→自动提升的完整供应链闭环 |
| **LobsterAI** | 企业版、UI 统一化、定时任务 | 企业用户 | 基于 OpenClaw 二次开发，Electron 客户端 |
| **Moltis** | 连接器持久化、历史数据接入 | 数据集成场景 | provider-neutral connector + 原子快照 + 全文搜索 |
| **PicoClaw** | 嵌入式/轻量级 | 教育、DIY | 资源占用低，开发节奏靠 Dependabot 维持 |

## 6. 社区热度与成熟度

**活跃度分层：**

1. **极高吞吐/快速迭代**：OpenClaw（500+500）、Hermes（50+50）、ZeroClaw（50+50）、IronClaw（50+50）
   - 共同特征：功能推进与隐患排查并行，P1 问题长期悬置与新增缺陷同时存在，PR 合并量均达 7–84 条。
   - 分化：OpenClaw/Hermes 属“功能广度驱动”，ZeroClaw/IronClaw 属“架构与质量驱动”。

2. **高活跃/定向推进**：CoPaw（42+50）、NanoBot（13+31）、NanoClaw（19 PR/2 Issue）
   - 共同特征：有明确迭代主线（CoPaw 任务自动化、NanoBot 稳定性修复、NanoClaw 供应链安全），版本发布频繁。

3. **中等活跃/质量巩固期**：LobsterAI、Moltis
   - LobsterAI 短期合入 6 条 PR 但长期积压 5 条 stale（均超 4 个月）；Moltis 迁移修复齐备但无合并动作。

4. **低活跃/停滞**：PicoClaw、NullClaw、ZeptoClaw
   - 无实质代码合并，依赖机器人维护或完全无活动。

**成熟度判断：**

- **快速迭代阶段**：OpenClaw、Hermes、CoPaw、IronClaw —— 新功能不断涌入，但稳定性债务同步累积。
- **质量巩固阶段**：ZeroClaw（安全加固）、NanoClaw（供应链门禁）、Moltis（迁移收尾）—— 节奏放缓但正确性优先。
- **风险预警**：OpenClaw 的“关闭但未修复”问题、Hermes 的 TUI P1 长期无人认领、PicoClaw 的核心体验 Bug 无响应，均属社区信任消耗信号。

## 7. 值得关注的趋势信号

1. **“静默失败”成为信任头号杀手**
   OpenClaw #121058 用 92 条评论证明：用户可接受 Bug，但不能接受“关闭但故障依旧”和“无日志、无重试、无告警”。Hermes #62142、CoPaw #6921、NanoBot #5373 呈现同样模式。**可观测性（错误码、告警钩子、兜底日志）将从加分项变为必选项。**

2. **供应链安全从“建议”走向“门禁”**
   NanoClaw 将镜像验证从路径过滤改为全量 PR 门禁、由签名充当审批 review；ZeroClaw 修复路径穿越和凭证转发；CoPaw 被曝端口暴露。**Sigstore 签名、CSPRNG、fail-closed 凭证处理正在成为新一代 agent 基础设施的基线要求。**

3. **记忆工程进入“信任与成本”深水区**
   多项目同时出现：记忆来源信任标记（OpenClaw #7707）、记忆无损整合（NanoBot #5377）、跨模型 JSON 鲁棒性（ZeroClaw #6998）、本地优先记忆（Hermes #85418）、外部记忆方案（CoPaw #7003）。**记忆不再只是存储，而是需要可审计、防投毒、低成本访问的一等公民。**

4. **Agent 架构向“可插拔内核”演进**
   IronClaw #7482 明确将自身定位为“内核”（调度/隔离/审计/入站），agent loop 交由外部 harness；OpenClaw 通过插件静态清单和模板插件化（NanoClaw #3220）同样指向标准化 + 可替换。**未来的个人 AI 助手将从“单体应用”走向“核心 + harness 生态”。**

5. **成本可观测性成为模型路由标配**
   ZeroClaw #9631（稳定 session_id 启用 prompt cache）、NanoBot #5298（MCP schema 预算）、Hermes #85690（记录实际成本）、IronClaw #7630（量化 Postgres 写入）——**用户已不满足于“能调用模型”，而要求“知道每一分钱/每一次 token 去了哪里”。**

6. **多代理编排的可靠性缺口集中爆发**
   OpenClaw 子代理完成丢失（5 个 P1 同根因）、CoPaw 任务自行中断、Hermes cron 钉死模型、ZeroClaw 子代理预算不受约束——**“让 agent 派生子 agent 并可靠回收结果”仍是一个未解决的系统性问题。** 这将是下一阶段竞争的技术制高点。

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报 — 2026-08-14

## 1. 今日速览

NanoBot 过去 24 小时活跃度较高：共 13 条 Issue 更新（12 开 1 关）、31 条 PR 更新（22 待审 9 关闭），无新版本发布。项目当前处于**密集 bug 修复与功能增强并行**阶段：cron 调度器、会话持久化、记忆整合等稳定性问题均有对应修复 PR 在审，MCP 工具集、Telegram 贴纸、Matrix 验证等方向也有新功能推进。值得注意的是，同一 cron 修复出现三版 PR（#5374/#5375/#5376），最终 #5376 接替前两版进入审查，显示维护者对修复质量要求较高且迭代反馈迅速。整体项目健康度良好，缺陷响应链路通畅。

## 2. 版本发布

无新版本发布（最新 releases 为空）。此前的安全修复（#5306）与多项 PR 尚未进入正式发布，建议维护者关注版本节奏。

## 3. 项目进展

今日有多项重要 PR 关闭或取得实质推进：

**已关闭的 PR（可能为合并或废弃，需跟进确认）**
- [#5384 fix(webui): restore transcript-only session history](https://github.com/HKUDS/nanobot/pull/5384) — 恢复 WebUI 侧栏中仅保留纯文本记录的会话历史，解决会话 JSONL 丢失后的历史不可见问题。
- [#5381 feat(webui): add native workspace folder picker](https://github.com/HKUDS/nanobot/pull/5381) — 为本地部署的 WebUI 增加 macOS/Windows/Linux 原生文件夹选择器，仅在 loopback 网关场景下启用。
- [#5374](https://github.com/HKUDS/nanobot/pull/5374) / [#5375](https://github.com/HKUDS/nanobot/pull/5375) — cron 调度器持久化容错的早期版本，被 [#5376](https://github.com/HKUDS/nanobot/pull/5376) 取代。
- [#4556 feat(dream): wire up model_override for Dream consolidation](https://github.com/HKUDS/nanobot/pull/4556) / [#4550 fix(cron): use per-run session key](https://github.com/HKUDS/nanobot/pull/4550) — 两个从 6 月挂起至今的 PR 今日关闭。

**新提交/在审的重要 PR（open）**
- [#5376 fix(cron): keep scheduler alive when job-store persistence fails](https://github.com/HKUDS/nanobot/pull/5376) — 修复 cron 调度器在存储持久化失败后永久死亡的问题。
- [#5379 fix(memory): preserve full consolidation input](https://github.com/HKUDS/nanobot/pull/5379) — 将记忆整合的截断替换为无损分块，避免历史数据丢失。
- [#5380 fix(session): restore state when file-cap archive fails](https://github.com/HKUDS/nanobot/pull/5380) — 文件归档失败时恢复会话状态。
- [#5385 fix(matrix): complete Element SAS request flow](https://github.com/HKUDS/nanobot/pull/5385) — 完善 Matrix 设备交叉签名验证流程。
- [#5387 feat(telegram): support reusable sticker replies](https://github.com/HKUDS/nanobot/pull/5387) — 支持 Telegram 贴纸收发。
- [#5388 feat(agent): budget model-visible MCP schemas](https://github.com/HKUDS/nanobot/pull/5388) — 为 MCP 工具 schema 提供字节预算以控制上下文开销。
- [#5386 feat(mcp): preserve MCP Apps result metadata](https://github.com/HKUDS/nanobot/pull/5386) — 保留 MCP Apps 调用的结构化结果元数据。

项目整体在 **cron 可靠性、会话一致性、记忆无损化、新渠道能力** 四个维度上同步向前推进。

## 4. 社区热点

- **[#4010 Feature proposal: text-to-speech / voice output support](https://github.com/HKUDS/nanobot/issues/4010)** — 今日评论最活跃的 Issue（3 评论 / 3 👍），提出增加语音回复能力，让智能体在支持语音消息的渠道上“说”出来。该 Issue 已从 5 月持续至今仍是开放状态，说明语音输出是用户持续关注的重要方向。
- **[#5289 feat(telegram): support sending stickers and agent-initiated message reactions](https://github.com/HKUDS/nanobot/issues/5289)** — Telegram 渠道目前完全无法处理贴纸消息。已有 [#5387](https://github.com/HKUDS/nanobot/pull/5387) 在审，预计将随下一版本落地。
- **[#5373 Cron scheduler dies permanently after a single job-store persistence failure](https://github.com/HKUDS/nanobot/issues/5373)** — 用户详细描述了 cron 调度器因磁盘满/权限变化导致永久停摆的故障链，该 Issue 引发了一个修复 PR 的三次迭代，是今日社区与维护者互动最紧密的话题。

## 5. Bug 与稳定性

按严重程度排列：

- **[安全] [#5306 exec.allowPatterns shell-chain bypass allows unintended command execution](https://github.com/HKUDS/nanobot/issues/5306)** — `exec.allowPatterns` 白名单可被 shell 链式命令绕过，导致未预期命令执行。该 Issue **今日已关闭**，需确认修复是否已进入发布分支（当前无新 release）。
- **[高] [#5373 cron 调度器在单次持久化失败后永久死亡](https://github.com/HKUDS/nanobot/issues/5373)** — 单次 `_save_store()` 异常可使整个调度器静默停止。已有修复 PR [#5376](https://github.com/HKUDS/nanobot/pull/5376)（含测试）。
- **[高] [#5378 file-cap 归档失败会先变更会话再持久化](https://github.com/HKUDS/nanobot/issues/5378)** — 归档回调抛错时，内存会话已把超限消息丢弃，后续保存无法恢复。已有修复 PR [#5380](https://github.com/HKUDS/nanobot/pull/5380)。
- **[高] [#5377 记忆整合截断输入但游标越过整批消息](https://github.com/HKUDS/nanobot/issues/5377)** — 被截断的消息永远不会重新整合，造成记忆静默丢失。已有修复 PR [#5379](https://github.com/HKUDS/nanobot/pull/5379)。
- **[中] [#5368 WebUI: 任务仍在生成时展示 copy/fork 操作](https://github.com/HKUDS/nanobot/issues/5368)** — 界面同时呈现“工作中”与可复制/分叉操作，产生完成信号冲突。
- **[中] Windows 下 `os.replace()` 瞬时 PermissionError 可致网关崩溃** — 见 PR [#5382](https://github.com/HKUDS/nanobot/pull/5382)（bug 描述），已在 gateway.log 中确认两次。

## 6. 功能请求与路线图信号

| 需求 Issue | 对应 PR | 状态 |
|---|---|---|
| [#5298 MCP 大工具集 schema 上下文预算](https://github.com/HKUDS/nanobot/issues/5298) | [#5388 feat(agent): budget model-visible MCP schemas](https://github.com/HKUDS/nanobot/pull/5388) | 在审，opt-in 默认关闭 |
| [#5289 Telegram 贴纸支持](https://github.com/HKUDS/nanobot/issues/5289) | [#5387 feat(telegram): support reusable sticker replies](https://github.com/HKUDS/nanobot/pull/5387) | 在审 |
| [#5251 MCP Apps 主机支持](https://github.com/HKUDS/nanobot/issues/5251) | [#5386 feat(mcp): preserve MCP Apps result metadata](https://github.com/HKUDS/nanobot/pull/5386) | 在审 |
| [#4841 Matrix 设备信任/交叉签名](https://github.com/HKUDS/nanobot/issues/4841) | [#5385 fix(matrix): complete Element SAS request flow](https://github.com/HKUDS/nanobot/pull/5385) | 在审 |
| [#4010 语音输出支持](https://github.com/HKUDS/nanobot/issues/4010) | 无对应 PR | 长期开放 |
| [#5350 QwenCloud provider 兼容路径](https://github.com/HKUDS/nanobot/issues/5350) | 无对应 PR | 新提议 |
| [#5366 WebUI 本地化 Agent 活动文案](https://github.com/HKUDS/nanobot/issues/5366) | 无对应 PR | 新提议 |

从 PR 活跃度推断，**MCP 生态增强、Telegram 渠道补齐、Matrix 安全体验** 最可能进入下一版本。

## 7. 用户反馈摘要

- **MCP 工具集上下文成本是真实痛点**（[#5298](https://github.com/HKUDS/nanobot/issues/5298)）：当 MCP 工具数量较大时，所有 schema 会全部传给模型，用户已明确感受到 token 开销，希望做“预算/裁剪”。
- **语音闭环缺失**（[#4010](https://github.com/HKUDS/nanobot/issues/4010)）：用户指出“语音输入已有，但回复永远是文本”，在支持语音消息的渠道上体验不完整。
- **Matrix 信任警告让用户困扰**（[#4841](https://github.com/HKUDS/nanobot/issues/4841)）：Element 客户端一直显示 bot 设备“不受信任”，普通用户不会处理交叉签名，直接影响可用性。
- **WebUI 文案不跟随界面语言**（[#5366](https://github.com/HKUDS/nanobot/issues/5366)）：界面已切换语言但 Agent 活动提示仍为英文，本地化未完成。
- **agent 持久记忆缺失**（[#5372](https://github.com/HKUDS/nanobot/issues/5372)）：外部用户推荐 ViBo 记忆系统，指出“每次会话从零开始、重复发送上下文浪费 token”的痛点。虽是推广贴，但反馈的问题值得关注。

## 8. 待处理积压

- **[#4010 语音输出请求（5 月创建，仍开放，3 评论 3 👍）](https://github.com/HKUDS/nanobot/issues/4010)** — 长时间未获得明确路线图回应，建议维护者给出计划或探讨技术可行性。
- **[#4841 Matrix 交叉签名验证（7 月创建，已有 #5385 修复 PR）](https://github.com/HKUDS/nanobot/issues/4841)** — PR 已在审，需尽快确认合并并纳入发布。
- **长期挂起 PR：**[#4549 feat(heartbeat): add model_override config](https://github.com/HKUDS/nanobot/pull/4549) 与 [#4551 feat(heartbeat): add isolated_session config](https://github.com/HKUDS/nanobot/pull/4551) 自 6 月 26 日提交至今仍待合并（priority: p2），且今天其兄弟 PR #4556/#4550 被关闭，建议尽快明确这两项的取舍。
- **PR 标注 conflict：** [#5358 feat(webui): add session collaboration via mentions](https://github.com/HKUDS/nanobot/pull/5358)、[#5357 fix(webui): cancel active turn before deleting sessions](https://github.com/HKUDS/nanobot/pull/5357)、[#5383 fix(session): serialize canonical file access](https://github.com/HKUDS/nanobot/pull/5383) 均带 conflict 标记，如不及时解决可能阻塞合入。
- 安全 Issue [#5306](https://github.com/HKUDS/nanobot/issues/5306) 虽已关闭，但当前无新 release，用户可能仍运行在受影响版本上，建议确认真实修复状态并考虑补发。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目动态日报 — 2026-08-14

## 1. 今日速览

过去24小时项目保持极高活跃度：**50条 Issue 更新**（新开/活跃 45、关闭 5）、**50条 PR 更新**（待合并 42、合并/关闭 8），并发布了 **v0.20.1 (v2026.8.13) 补丁版**，将自 v0.20.0 以来约 656 个 PR 收编为稳定标签供下游使用。社区焦点集中在全新的 Webhook 修复战役（#84834，16 条评论）、**已持续 13 天的 TUI 核心工作流故障**（#69592，P1）以及 Signal 适配器功能缺口（#39043，3 👍）。此外，两条 Discord typing indicator 卡死 bug 被标记为已修复并关闭，一条 models.dev 阻塞问题被作为重复关闭。整体来看，项目合并速度与问题上报速度基本持平，但 **P1 级 TUI 问题长期未获修复**是当前最主要的风险信号。

---

## 2. 版本发布

### v2026.8.13 / v0.20.1（2026-08-13 发布）

- **性质**：补丁版（Patch release）
- **核心内容**：将 v0.20.0 以来约 **656 个合并 PR** 统一收编为稳定标签，供 Docker 镜像、托管部署及按标签安装的下游消费者使用。
- **破坏性变更**：无明确说明，属于增量累积的补丁版本。
- **迁移注意**：本次为 tag 收编性质，下游从 `latest` 标签拉取的用户建议验证自身依赖与 v0.20.0 的兼容性后再升级。

链接：[v2026.8.13 Release](https://github.com/NousResearch/hermes-agent/releases)

---

## 3. 项目进展

今日共有 **8 个 PR 被合并/关闭**，公开可见的 4 个均已完成合并，具体如下：

| PR | 内容 | 意义 |
|---|---|---|
| [#85591](https://github.com/NousResearch/hermes-agent/pull/85591) | **隔离 Agent 浏览器研究**，禁止 Browser Use / 内置浏览器 / Kanban worker 等自动附加或启动用户可见 Chrome | 重要的隐私/安全边界改进，默认策略转向隔离/无头模式 |
| [#85698](https://github.com/NousResearch/hermes-agent/pull/85698) | **修复 suggestion pills 点击一次后变死按钮**（三处独立缺陷） | 提升 MCP 连接体验，消除“已添加却无法再点击”的困惑 |
| [#85690](https://github.com/NousResearch/hermes-agent/pull/85690) | **记录 OpenRouter 实际报告成本**（`usage.cost`），并保留 models-catalog 估算做独立对比 | 让用量报告更贴近真实支出 |
| [#85343](https://github.com/NousResearch/hermes-agent/pull/85343) | **恢复 Mistral Voxtral STT** 到 tools_config 目录（此前因恶意包被隔离禁用） | 解除误伤，恢复语音转文字能力 |

此外，v0.20.1 一次性收编约 656 个 PR，意味着过去数周的大量功能与修复（路由、桌面端、MCP、安全等领域）已具备稳定发布形态。值得注意的 **待合并重要 PR** 包括：#63826（跨主机 redirect 剥离 Bearer 凭证）、#6722（硬编码 OSV endpoint 防绕过）、#84876（同会话并发 agent turn 串行化）。

---

## 4. 社区热点

近期讨论最活跃、反映诉求最强烈的三个议题：

### ① Webhook Revolution 修复战役（Meta-Issue）— 16 条评论
[#84834](https://github.com/NousResearch/hermes-agent/issues/84834) 由 andrexibiza 发起，提出对 Hermes 全 Webhook surface（ingress、执行、投递、配置、管理 UI、部署、文档）进行 **graph-gated 5×2×3 修复**的大规模重构，属于 EPIC 级元问题。评论热度最高，说明社区对 webhook 稳定性的诉求集中且强烈，希望系统性解决而非零散修补。

### ② TUI /sessions 与 /models 面板不可见 — 12 条评论，P1
[#69592](https://github.com/NousResearch/hermes-agent/issues/69592) 已持续 **Day 13**。在 ambient widgets（默认 TUI dock 模式）下，`/sessions`、`/switch`、`/resume` 面板被遮挡不可见，用户无法切换会话或更换模型，`/reload` 也无效。评论集中在“核心工作流被阻断”的严重性上，是当前社区最关切的稳定性问题。

### ③ Signal 适配器原生引用/回复/编辑/删除/已读回执 — 7 条评论，3 👍
[#39043](https://github.com/NousResearch/hermes-agent/issues/39043) 是功能缺口类的经典诉求，社区希望 Signal adapter 完整暴露 signal-cli 的端到端能力（原生 outbound quote/reply、时间戳 ID、编辑、remote delete、已读回执）。获得 3 个 👍，是功能类 issue 中支持度最高的之一。

---

## 5. Bug 与稳定性

### P1 — 严重（核心功能受阻）

| Issue | 描述 | 状态 |
|---|---|---|
| [#69592](https://github.com/NousResearch/hermes-agent/issues/69592) | TUI 中 `/sessions`、`/models` overlay 被 ambient widget 遮挡不可见，无法恢复会话/切换模型，`/reload` 无效。已持续 13 天 | 未修复，无对应 PR |
| [#62142](https://github.com/NousResearch/hermes-agent/issues/62142) | verification-stop 可丢弃已流式输出的最终答案和 cron 报告，用户在 Desktop/TUI 看到答案但 durable transcript 中丢失 | 未修复 |
| [#82168](https://github.com/NousResearch/hermes-agent/issues/82168) | Windows 安装程序既更新又重装（UI 显示双重行为），安装流程异常 | 待复现/处理 |

### P2 — 中等

| Issue | 描述 | 状态 |
|---|---|---|
| [#83427](https://github.com/NousResearch/hermes-agent/issues/83427) | `browser_exec` 在 Desktop 应用（PYTHONPATH 指向 Hermes venv）下报 `pydantic_core` ModuleNotFoundError | 未修复 |
| [#75791](https://github.com/NousResearch/hermes-agent/issues/75791) | Windows 11 25H2 上 `hermes dashboard --status` 误报没有 dashboard 进程（实际在 127.0.0.1:9119 正常服务） | 未修复 |
| [#70131](https://github.com/NousResearch/hermes-agent/issues/70131) | Emoji sign-off 修复（#14572）不完整，Dingbats 字符 ✨(U+2728) ✅(U+2705) 仍触发截断循环 | 未修复 |
| [#85215](https://github.com/NousResearch/hermes-agent/issues/85215) | Cron job pin 到已死模型且忽略 fallback_providers，连续数天以 HTTP 402 失败 | 未修复 |
| [#70050](https://github.com/NousResearch/hermes-agent/issues/70050) | Cron drift 保护使用户无法重新 pin 模型（`cron edit` 无 `--model`，cronjob update 丢 model，dashboard 缺编辑入口），free→free 被卡死 | 未修复 |
| [#65085](https://github.com/NousResearch/hermes-agent/issues/65085) | Telegram group observe 归因函数替换 event.source，破坏斜杠命令的 admin gating | 未修复 |
| [#85406](https://github.com/NousResearch/hermes-agent/issues/85406) | Windows host + Docker terminal 下 `vision_analyze` 将 POSIX 路径转成反斜杠，容器内 exec-read 失败 | 未修复 |
| [#85614](https://github.com/NousResearch/hermes-agent/issues/85614) | Slack peer bot ID 与最终 bot 授权检查不一致，bot-to-bot 场景可能被错误拒绝 | 未修复 |
| [#85658](https://github.com/NousResearch/hermes-agent/issues/85658) | 被中断的命令会让当前 chat 继承另一个会话的工作目录，后续命令全部跑错目录 | 未修复 |
| [#85659](https://github.com/NousResearch/hermes-agent/issues/85659) | Hermes Desktop 更新 PowerShell 脚本在法语版 Windows 上因 locale 报错 | 未修复 |
| [#85104](https://github.com/NousResearch/hermes-agent/issues/85104) | Desktop（Windows）同一 assistant 消息在聊天视图渲染两次，DB 中仅存一条，疑似前端渲染缺陷 | 未修复 |
| [#84876](https://github.com/NousResearch/hermes-agent/pull/84876) | 同一 session 并发 agent turns（/v1/chat/completions 与 /v1/runs 重叠）可加载陈旧历史，导致双循环 | **已有 fix PR 待合并** |

### P3 — 较低但仍需关注

- [#84058](https://github.com/NousResearch/hermes-agent/issues/84058) Desktop composer 在工具调用开始流式渲染时丢失光标
- [#85331](https://github.com/NousResearch/hermes-agent/issues/85331) Desktop 侧边栏在手动压缩链重组后渲染幽灵无标题行
- [#85672](https://github.com/NousResearch/hermes-agent/issues/85672) macOS Desktop 通过 SSH 连接 Ubuntu VPS 时 Kanban 附件下载路径解析错误
- [#85669](https://github.com/NousResearch/hermes-agent/issues/85669) Desktop 多 profile 下 `config.set` 将焦点会话设置写入启动 profile

### 今日已关闭的 Bug

- [#35838](https://github.com/NousResearch/hermes-agent/issues/35838)（blocking bug）— 标记为 duplicate 关闭
- [#26854](https://github.com/NousResearch/hermes-agent/issues/26854) & [#26728](https://github.com/NousResearch/hermes-agent/issues/26728)（Discord typing indicator 卡死）— 标记为 implemented-on-main 关闭

---

## 6. 功能请求与路线图信号

### 高概率进入下一版本

- **MCP 非交互配置** — 今日有两份互补 PR：[#85688](https://github.com/NousResearch/hermes-agent/pull/85688)（`hermes mcp configure` 增加非交互工具选择）与 [#85686](https://github.com/NousResearch/hermes-agent/pull/85686)（允许脚本配置已启用工具）。说明维护者正在积极补齐 MCP 的自动化/CI 场景，方向明确。
- **免认证 multi-provider failover pool** — [#85631](https://github.com/NousResearch/hermes-agent/pull/85631) 提出让 provider 插件声明为 no-auth loopback 后端，便于接入聚合免费层的代理。属于“Freemaxxing”方向。
- **内存压力可视化** — [#84965](https://github.com/NousResearch/hermes-agent/pull/84965) 将 gateway 已有的心跳内存采样、非干净退出判定、缓存驱逐信号暴露给用户，回应托管实例频繁 OOM 却看似健康的痛点。

### 值得关注的新提案

- **Delegation 持久化/路由/时序三连**（[#85646](https://github.com/NousResearch/hermes-agent/issues/85646)、[#85647](https://github.com/NousResearch/hermes-agent/issues/85647)、[#85648](https://github.com/NousResearch/hermes-agent/issues/85648)）— 同一作者 Xipong 提出批量 delegation 子任务的持久化、独立投递与实时影响父任务的能力，直指“慢兄弟阻塞快结果”的并行效率问题。
- **本地优先 Memory Provider** — [#85418](https://github.com/NousResearch/hermes-agent/issues/85418) 由 2ndNatureAI 提交，基于 Hermes 构建零依赖本地记忆层并对标 Honcho，尚处 needs-decision 阶段。
- **Signal 完整消息能力** — [#39043](https://github.com/NousResearch/hermes-agent/issues/39043)（引用/回复/编辑/远程删除/已读回执）呼声最高，值得纳入路线图。
- **Credential pool TTL 可配置** — [#33049](https://github.com/NousResearch/hermes-agent/issues/33049) 将硬编码的 `EXHAUSTED_TTL_*_SECONDS` 改为可配置。
- **Telegram cold boot 可选丢弃 pending updates** — [#84317](https://github.com/NousResearch/hermes-agent/issues/84317)。
- **隐藏 Vite 警告** — [#76207](https://github.com/NousResearch/hermes-agent/issues/76207)，提升 `hermes update` 输出整洁度（1 👍）。
- **Desktop 关闭到托盘** — [#78343](https://github.com/NousResearch/hermes-agent/pull/78343)，需求明确且实现已就绪，等待决策。

---

## 7. 用户反馈摘要

- **TUI 核心流程受阻最令用户沮丧**（[#69592](https://github.com/NousResearch/hermes-agent/issues/69592)）：用户 apoapostolov 直接指出“这是默认配置下两个核心 TUI 工作流都死了”。Day 13 仍未修复，社区耐心正在消耗。
- **Cron 模型钉死无人能解**（[#70050](https://github.com/NousResearch/hermes-agent/issues/70050)、[#85215](https://github.com/NousResearch/hermes-agent/issues/85215)）：用户 bgexpert 是典型受害者——免费套餐间无法重新指定模型，`cron edit` 缺 `--model`、Dashboard 缺编辑入口，**“no supported repin path”** 是用户最直接的挫败点。CPLANETAI 则报告 cron 连续数天 402 失败，且忽略已配置的 fallback_providers。
- **Windows 平台问题密集**（[#75791](https://github.com/NousResearch/hermes-agent/issues/75791)、[#85406](https://github.com/NousResearch/hermes-agent/issues/85406)、[#85659](https://github.com/NousResearch/hermes-agent/issues/85659)、[#82168](https://github.com/NousResearch/hermes-agent/issues/82168)）：dashboard 状态误报、Docker 路径转义、PowerShell locale 报错、更新与重装叠加——Windows 用户体验仍明显落后于 macOS/Linux。
- **Emoji 截断修复不彻底**（[#70131](https://github.com/NousResearch/hermes-agent/issues/70131)）：用户 yeounhyeok 精确指出 `ord(last) >= 0x1F300` 的单边界判断遗漏 Dingbats 区段（U+2700–27BF），属于“修了但没修全”的典型反馈。
- **桌面端细节问题较多**（[#85104](https://github.com/NousResearch/hermes-agent/issues/85104)、[#85331](https://github.com/NousResearch/hermes-agent/issues/85331)、[#84058](https://github.com/NousResearch/hermes-agent/issues/84058)、[#85669](https://github.com/NousResearch/hermes-agent/issues/85669)）：重复渲染、幽灵行、光标丢失、多 profile 写入错误——前端渲染层与配置隔离性成为最集中的抱怨来源。
- **正向反馈**：[#85418](https://github.com/NousResearch/hermes-agent/issues/85418) 中用户 2ndNatureAI 明确感谢 `--autoConnect` 解决了 Chrome DevTools 空白 profile 问题（“fixed it for good”），可见修复质量在个别场景获得了用户背书。

---

## 8. 待处理积压

### 长期未响应/未合并

| 编号 | 类型 | 创建时间 | 等待天数 | 说明 |
|---|---|---|---|---|
| [#6722](https://github.com/NousResearch/hermes-agent/pull/6722) | 安全 PR | 2026-04-09 | **127 天** | 硬编码 OSV endpoint 防 env 覆盖绕过，严重性明确但长期滞留 |
| [#39043](https://github.com/NousResearch/hermes-agent/issues/39043) | 功能需求 | 2026-06-04 | 71 天 | Signal 原生消息能力，3 👍，社区呼声高 |
| [#33049](https://github.com/NousResearch/hermes-agent/issues/33049) | 功能需求 | 2026-05-27 | 79 天 | Credential pool TTL 可配置化 |
| [#61375](https://github.com/NousResearch/hermes-agent/pull/61375) | 功能 PR | 2026-07-09 | 36 天 | 发出 machine-readable turn-dead 标记，补可观测性缺口 |
| [#63826](https://github.com/NousResearch/hermes-agent/pull/63826) | 安全 PR | 2026-07-13 | 32 天 | 跨主机 redirect 剥离 Bearer 凭证，安全边界相关 |

### 高优先级 P1 未修复

- [#69592](https://github.com/NousResearch/hermes-agent/issues/69592) — **P1，Day 13**，TUI 核心工作流不可用，无 assigned PR
- [#62142](https://github.com/NousResearch/hermes-agent/issues/62142) — **P1**，verification-stop 丢弃流式最终答案与 cron 报告，影响会话完整性与投递可靠性

### 积压中可能被低估的问题

- [#70050](https://github.com/NousResearch/hermes-agent/issues/70050) + [#85215](https://github.com/NousResearch/hermes-agent/issues/85215) 组合：Cron model pinning 与 drift protection 存在系统性缺陷（多个关联 issue：#68380、#24258、#27530、#19615），用户被卡死且无官方规避路径，建议维护者以 meta-issue 形式统一跟踪。
- [#75791](https://github.com/NousResearch/hermes-agent/issues/75791) Windows dashboard 状态误报可能影响更多 Windows 用户的运维判断，建议提高优先级。

---

**总结**：Hermes Agent 项目活跃度极高，v0.20.1 稳定版发布与 Webhook 修复战役展示了项目的前进动能；但 **P1 TUI 问题连续 13 天未修复、Cron 模型钉死系统性缺陷、Windows 平台体验短板**是当前社区反馈中最集中的健康度隐患。安全类 PR（#6722、#63826）积压时间过长，建议优先合并。

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报 — 2026-08-14

## 今日速览

过去24小时内，PicoClaw 项目共产生 3 条 Issue 更新（2 条新开，1 条已有讨论继续）与 9 条 PR 更新（6 条待合并，3 条已关闭/合并）。其中，6 条待合并 PR 均为 Dependabot 依赖自动升级（AWS SDK、Anthropic SDK、Mautrix 等），而 3 条已关闭 PR 也全部为 Dependabot 发起并因 stale 标记被自动关闭，**今日没有实质性代码合并进入主分支**。版本发布方面，本周无新 Release。总体而言，项目当前处于"依赖维护驱动"的节奏，社区活跃度中等，新功能开发与既有 Bug 修复进展较缓。

---

## 版本发布

本周无新版本发布。

---

## 项目进展

**今日无实质性代码合并。**

3 条已关闭 PR（[#3304](https://github.com/sipeed/picoclaw/pull/3304)、[#3305](https://github.com/sipeed/picoclaw/pull/3305)、[#3306](https://github.com/sipeed/picoclaw/pull/3306)）均为 Dependabot 于 2026-07-30 发起的依赖升级请求（Anthropic SDK 1.55.1→1.61.0、AWS Bedrock Runtime 1.53.3→1.56.2、AWS Config 1.32.25→1.32.33），今日全部因 `stale` 标签被自动关闭——**这不是正常的合并关闭，而是机器人维护流程的自动过期处理**。由于同一批依赖的更新版本今日已由 Dependabot 重新发起（见 PR #3332-#3336），可视为依赖升级工作的延续，而非项目功能推进。

值得注意的是，[#3318](https://github.com/sipeed/picoclaw/pull/3318)（修复 `web/frontend/pnpm-lock.yaml` 重复键导致的 pnpm lockfile 损坏）已带有 `stale` 标签，该 PR 是当前积压中**唯一与功能修复相关的待合并项**，若再无人响应可能同样面临自动关闭风险。

---

## 社区热点

**Issue [#3281](https://github.com/sipeed/picoclaw/issues/3281)「Web UI chat input is very laggy when history has a little bit long」** 是本日社区讨论的绝对焦点：

- 评论 5 条，是全项目唯一产生讨论的 Issue
- 已获得 1 个 👍
- 创建于 2026-07-21，至今已近一个月，期间仍有用户在跟进

**诉求分析**：用户报告在 Web UI 中，当单个会话的历史消息达到一定长度后，输入框出现严重卡顿。这暴露的是 **前端长列表渲染性能问题**——很可能是消息组件未经虚拟化或 memo 优化，导致每次输入触发全量重渲染。考虑到 PicoClaw 定位于个人 AI 助手，长会话是高频使用场景，此问题直接关乎核心体验，社区期待修复的意愿较为强烈。

---

## Bug 与稳定性

| 严重程度 | Issue | 描述 | 状态 |
|---------|-------|------|------|
| 中 | [#3281](https://github.com/sipeed/picoclaw/issues/3281) | Web UI 输入框在长历史会话中严重卡顿（滞后） | 未修复，无关联 fix PR |

该 Bug 目前**没有对应的修复 PR**，且在 [PR #3318](https://github.com/sipeed/picoclaw/pull/3318)（web 前端 lockfile 修复）中也可窥见 Web 前端构建链路存在健康度问题——`pnpm-lock.yaml` 的损坏可能导致前端依赖安装失败，间接影响 Web UI 的可用性。

另外，今日数据中**没有崩溃类、安全类或数据丢失类的高危 Bug 报告**。

---

## 功能请求与路线图信号

今日出现 2 个新功能请求，均反映了用户对 **模型层灵活性** 的深层需求：

**1. [#3331](https://github.com/sipeed/picoclaw/issues/3331) — 支持任意 `/audio/transcriptions` 兼容端点**

- 诉求：目前语音转录仅支持名称包含 `*-whisper-*` 的模型，导致用户无法使用更新的转录模型（如较新的非 whisper 命名模型），且现有模型"太老、太慢"
- 建议：在模型配置或语音配置中增加 `whisper-transcription` 标志，由用户决定是否强制走 whisper 路径
- **路线图信号**：语音功能尚未完全模型无关化，自定义模型接入是社区明确诉求

**2. [#3330](https://github.com/sipeed/picoclaw/issues/3330) — `delegate`/`spawn`/`subagent` 工具支持调用时动态指定模型**

- 诉求：目前子代理的模型选择是静态的（`delegate` 固定使用目标 agent 的配置模型，`spawn` 固定使用主 agent 的 `defaultModel`），用户希望在调用时动态覆盖
- **路线图信号**：这属于 Agent 编排能力的精细化，表明社区用户已开始在多 agent 协作场景中追求更细粒度的模型路由控制

结合当前待合并的 Dependabot 依赖升级（如 Anthropic SDK v1.62.0、AWS SDK v1.43.4 等），下一版本可能在**模型接入与云服务兼容性**方面有底层能力增强，但上述两个功能请求是否被纳入路线图，仍需要维护者明确表态。

---

## 用户反馈摘要

基于当前 Issues 讨论，可以提炼出以下真实用户反馈：

**痛点（来自 #3281）**：
- Web UI 是用户高频使用的入口，但长会话下的输入延迟严重影响了使用体验
- 问题已存在近一个月（2026-07-21 创建至今），社区用户在期待维护者回应，但尚未得到修复

**使用场景**：
- 用户明显将 PicoClaw Web 用作日常 AI 助手对话工具，会进行长篇幅的连续对话
- 语音转录功能（#3331）属于实际使用中被认为不够灵活的能力，用户自述"whisper-*"模型"too old and slow"，说明已有用户在生产环境中使用语音功能

**满意度**：
- 数据中无明确表达满意或不满意的评价性评论，但 #3281 的 5 条评论与 1 个 👍 表明用户在持续关注且期待修复

---

## 待处理积压

以下问题长期未获维护者响应，建议优先关注：

| 事项 | 类型 | 创建时间 | 持续天数* | 建议操作 |
|------|------|---------|----------|---------|
| [#3281](https://github.com/sipeed/picoclaw/issues/3281) Web UI 输入卡顿 | Bug | 2026-07-21 | ~24 天 | 确认可复现，标记里程碑，分配修复 |
| [#3318](https://github.com/sipeed/picoclaw/pull/3318) pnpm-lock.yaml 修复 | PR | 2026-08-05 | ~9 天 | 已被 stale 标记，需尽快 review 或明确拒绝 |
| [#3304](https://github.com/sipeed/picoclaw/pull/3304) / [#3305](https://github.com/sipeed/picoclaw/pull/3305) / [#3306](https://github.com/sipeed/picoclaw/pull/3306) 依赖升级被 stale 关闭 | PR | 2026-07-30 | 已关闭 | 新版本 PR（#3332-#3336）已重新发起，注意避免再被 stale |

\* 截至 2026-08-14。

---

**总结**：PicoClaw 当前处于低频开发的平稳期，依赖机器人维护在持续推进，但社区反馈的两个核心问题——Web UI 长会话性能与模型接入灵活性——目前均未得到维护者公开响应。建议项目方优先回应 #3281 和 #3331/#3330 的功能请求，明确路线图优先级，以避免社区贡献者流失。

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报 · 2026-08-14

## 1. 今日速览

过去 24 小时 NanoClaw 保持**高活跃度**：19 条 PR 更新（13 条合并/关闭、6 条待合并）、2 条 Issue 更新，并发布 **v2.2.0**。核心团队围绕 agent 镜像供应链安全完成了一轮集中收尾——`verify-agent-image` 从"顾问式"工作流升级为真正的 PR 门禁，并引入基于发布者签名的自动审批（默认关闭）。同日，Agent 模板正式迁移为 **Agent Plugins 1.0.0 目录格式**（#3220），配套 setup 向导流程同步落地。社区侧新报告 1 个 Bug：webhook/机器人发送者会触发无界审批卡片（#3235），尚无修复；此前报告的模板组 `ag-` 前缀缺失问题已在 24 小时内关闭。整体看，项目处于**功能迭代与供应链安全加固并行**的高产出阶段。

## 2. 版本发布：v2.2.0

- 发布 PR：[#3237 chore(release): v2.2.0](https://github.com/nanocoai/nanoclaw/pull/3237)（已合并）
- 发布说明（原文截断，仅展示可见部分）：
  - **Stamped plugins 原地更新**：当 group 已携带模板的插件时，再次执行 `ncl groups create --template <ref>` 将触发**原地更新**，而不再生成重复 agent；dry-run 模式会打印插件所辖全部 surface（插件文件、skills、MCP servers 等）的更新计划。
- **破坏性变更（同批合入）**：[#3220 feat!: agent templates become Agent Plugins 1.0.0 directories](https://github.com/nanocoai/nanoclaw/pull/3220) —— 模板存储格式发生引擎级迁移（含 stamp-time 符号链接/caps/secret 加固）。
- **迁移注意事项**：
  - 旧模板目录需要按 Agent Plugins 1.0.0 目录规范重新布局；
  - setup 向导新增模板流程与首 agent 盖章逻辑（[#2909](https://github.com/nanocoai/nanoclaw/pull/2909)，14 个 setup 侧文件 +927/-52）；
  - 建议升级后先对存量 group 执行 `ncl groups create --template <ref>` dry-run，确认插件 surface 更新计划后再实际落地。

## 3. 项目进展

### 供应链安全 / CI 门禁（核心团队 gavrielc 主导）
- [#3238 ci: let verify-agent-image run on every PR so it can gate](https://github.com/nanocoai/nanoclaw/pull/3238) —— 移除 `paths: versions.json` 过滤，使镜像验证成为可强制要求的状态检查。
- [#3158 verify-agent-image: pin the publisher identity, and check attestations per arch](https://github.com/nanocoai/nanoclaw/pull/3158) —— 修复签名验证因 `AGENT_IMAGE_SIGNER_IDENTITY`/`_ISSUER` 变量不存在而被静默跳过的问题，接入 keyless Sigstore 真实发布者身份。
- [#3240 ci: open the agent-image bump PR from a dispatch](https://github.com/nanocoai/nanoclaw/pull/3240) —— agent 镜像提升循环的"后半段"：AWS worker 验证并推送镜像后，通过 `repository_dispatch` 自动打开 `versions.json` PR。
- [#3241 ci: let a verified signature be the approving review](https://github.com/nanocoai/nanoclaw/pull/3241) —— 发布者签名可充当 pin bump 的审批 review（默认关闭，需 `AGENT_IMAGE_AUTO_APPROVE=true`）。
- [#3236 versions: repin the agent image to hardened-2026-08-13](https://github.com/nanocoai/nanoclaw/pull/3236) —— 镜像 `af60e54f`（620,725,759 B）→ `ccde3d9c`（620,769,684 B），本次携带项目自身内容而非单纯基础镜像刷新。

### Agent Plugins 引擎（amit-shafnir 主导）
- [#3220 feat!: agent templates become Agent Plugins 1.0.0 directories](https://github.com/nanocoai/nanoclaw/pull/3220) —— 模板功能格式迁移 + stamp-time 安全加固（破坏性变更，见第 2 节）。
- [#2909 feat(setup): template setup flow in the wizard and first-agent stamping](https://github.com/nanocoai/nanoclaw/pull/2909) —— 搭建向导模板流程，按序合入 #3220 之后。
- [#3231 feat(codex,opencode): honor plugin MCP cwd in both provider config writers](https://github.com/nanocoai/nanoclaw/pull/3231) —— Codex/OpenCode 两套 provider 配置写入器统一支持插件 MCP 工作目录。

### 安全修复
- [#3229 fix(telegram): generate pairing codes with a CSPRNG, not Math.random()](https://github.com/nanocoai/nanoclaw/pull/3229) —— Telegram 配对码改用 `crypto.randomInt` 并扩大取值空间，消除可预测性风险。

### 数据与配置修复
- [#3145 fix(db): backfill destinations for existing wirings](https://github.com/nanocoai/nanoclaw/pull/3145) —— 新增 migration 021，为既有 messaging-group wirings 补建 channel destinations，保留原目标与自定义名称。
- [#2624 feat: per-server disabledTools in McpServerConfig](https://github.com/nanocoai/nanoclaw/pull/2624) —— MCP 服务器级工具禁用能力落地。

## 4. 社区热点

- [#3234 Template-stamped agent groups get a bare UUID id](https://github.com/nanocoai/nanoclaw/issues/3234)（已关闭，1 条评论）—— 模板创建的 agent group 拿到裸 UUID、缺少 `ag-` 前缀，导致 OneCLI `ensureAgent` 拒绝。从报告（08-12）到关闭（08-13）不足 24 小时，体现核心团队对阻断性问题响应迅速。
- [#3235 Unknown-sender approval: unbounded approval cards](https://github.com/nanocoai/nanoclaw/issues/3235)（新开，0 评论）—— 今日唯一新开 Issue，社区反映机器人/webhook 发送者触发无界审批卡片，是 `unknown_sender_policy` 机制的重要缺口，预计将推动策略差异化设计。
- **公开 CI 演练序列**：[#3239 smoke test](https://github.com/nanocoai/nanoclaw/pull/3239)（已关闭）与 [#3242 live-fire test of the signature approver](https://github.com/nanocoai/nanoclaw/pull/3242)（仍打开）均标注 "DO NOT MERGE"，展示团队在真实 PR 流上验证签名审批链的工程实践。

## 5. Bug 与稳定性

| 严重度 | 条目 | 状态 | 说明 |
|---|---|---|---|
| 高（功能阻断） | [#3234 模板组缺 `ag-` 前缀](https://github.com/nanocoai/nanoclaw/issues/3234) | ✅ 已关闭 | 裸 UUID 以数字开头时被 OneCLI `ensureAgent` 拒绝；报告后 24 小时内关闭。 |
| 中（运维循环） | [#3235 机器人发送者产生无界审批卡片](https://github.com/nanocoai/nanoclaw/issues/3235) | ⚠️ 未修复 | `request_approval` 策略把 webhook/机器人当人类审批，拒绝不持久、卡片无限累积；暂无关联 fix PR。 |
| 中（安全） | [#3229 Telegram 配对码用 `Math.random()`](https://github.com/nanocoai/nanoclaw/pull/3229) | ✅ 已合并 | 改用 CSPRNG 并扩大空间，降低暴力枚举风险。 |
| 中（CI 失效） | [#3158 签名验证被静默跳过](https://github.com/nanocoai/nanoclaw/pull/3158) | ✅ 已合并 | 引用不存在的变量导致验证从未运行，现已接入真实发布者身份。 |

## 6. 功能请求与路线图信号

- **未知发送者策略差异化**（来自 [#3235](https://github.com/nanocoai/nanoclaw/issues/3235)）：诉求是区分自动化发送者（webhook/其他 bot）与人类，并持久化拒绝状态——预期后续版本会为 `unknown_sender_policy` 增加"按发送者类型分流"能力。
- **CLI bounded stdin JSON**（[#3218 feat(cli): accept bounded JSON from stdin](https://github.com/nanocoai/nanoclaw/pull/3218)，open）：为主机与容器端 `ncl` 增加 `--stdin-json` 模式，不改变现有请求框架与鉴权，属于低风险 CLI 增强，可能进入 v2.2.x 或 v2.3。
- **/add-hindsight 记忆技能**（[#2420](https://github.com/nanocoai/nanoclaw/pull/2420)，open）：内置 Hindsight 长时记忆 MCP 包装器，08-13 仍有更新，是"记忆"方向路线图的重要候选，需关注与 Agent Plugins 1.0.0 格式的兼容。
- **未知斜杠命令回退为普通聊天**（[#2346](https://github.com/nanocoai/nanoclaw/pull/2346)，open）：修复未知命令被错误归类为 passthrough 导致响应静默丢弃的问题，方向明确。

## 7. 用户反馈摘要

- **#3234 评论**：报告者指出 `--template` 与 `--folder` 两条路径生成的 group ID 格式不一致（裸 UUID vs `ag-<uuid>`），暴露了模板路径与常规路径的**行为分叉**；团队当日关闭，修复效率获认可。
- **#3235 反馈**：用户 `pentar69` 描述真实场景——群组配置审批策略后，**平台 webhook 每次推送都产生一张新审批卡**：既无法合理批准（非人类操作），拒绝也无法持久化。核心痛点是**缺少发送者类型识别**与**审批状态记忆**，该场景直接指向消息管线对自动化身份的建模不足。

## 8. 待处理积压

- [#2346 fix(formatter): treat unknown slash commands as normal chat](https://github.com/nanocoai/nanoclaw/pull/2346) —— 自 **2026-05-08** 开启，积压超 3 个月。影响面：所有使用未知斜杠命令的用户会静默丢失响应。**建议优先评审。**
- [#2420 feat(skills): /add-hindsight — bundled MCP wrapper for Hindsight memory](https://github.com/nanocoai/nanoclaw/pull/2420) —— 自 **2026-05-11** 开启，但 08-13 仍有更新，处于活跃推进状态；需验证与 v2.2.0 模板格式的兼容性。
- [#3218 feat(cli): accept bounded JSON from stdin](https://github.com/nanocoai/nanoclaw/pull/3218) —— 自 08-09 开启，低风险 CLI 增强，等待评审。
- [#3230 fix(skills): stop removal docs pointing at the retired data/env mirror](https://github.com/nanocoai/nanoclaw/pull/3230) —— 自 08-12 开启，文档修正类，可快速合入。
- [#3243 verify-agent-image: arming auto-merge is not a verdict](https://github.com/nanocoai/nanoclaw/pull/3243) —— 核心团队开放 PR，指出 auto-merge 步骤失败不应决定 verify job 结论，涉及 CI 门禁正确性，建议尽快跟进。
- [#3242 DO NOT MERGE — live-fire test of the signature approver](https://github.com/nanocoai/nanoclaw/pull/3242) —— 按计划将在验证完成后关闭，无需合并。

---

**项目健康度小结**：24 小时窗口内 13 条 PR 关闭/合并、1 个版本发布，核心链路（镜像验证→签名审批→自动提升）从"不可用"走到"可门禁"，Agent Plugins 引擎迁移按序落地；社区反馈渠道通畅，阻断性 Bug 响应速度快。主要风险点集中在 #3235 的审批机制缺口与两条超 3 个月的 PR 积压，建议下一迭代优先处理。

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报 — 2026-08-14

## 1. 今日速览

过去24小时项目活跃度**极高**。**ironclaw-v1.2.0 稳定版正式发布**，完成了从 RC3 到 stable 的晋升；核心 Epic **#7482（Pluggable agent loops）** 完成大规模规划落地——18 个关联子任务在 8 月 13 日密集创建（其中 14 个已关闭，4 个开放待实施），标志着项目从设计阶段正式进入执行阶段。与此同时，社区贡献者提交的 **ACP serve 命令 PR（#7513）** 与核心团队的性能优化 PR 系列（#7628-#7631）构成今日另一条主线。Issues 与 PR 各 50 条的更新量、26 条 PR 合并/关闭的结果，均指向一个健康且高产的开发周期。

---

## 2. 版本发布

### ironclaw-v1.2.0（2026-08-13）

**发布说明摘要：** 这是 `1.2.0-rc.3` 的稳定晋升，包含 RC2 和 RC3 中验证的修复，以及 RC1 的完整功能集。

**RC3 修复内容：**
- 运行时容器镜像现在安装了 `curl`，使容器内的 HTTP 健康检查可以执行。编排器使用该工具对 worker 进行探活。

**迁移注意事项：** 从 1.1.x 或更早版本升级的用户需注意，本版本为稳定晋升，无额外的破坏性变更披露。建议检查依赖 `curl` 进行容器健康检查的部署配置。

参考链接：[ironclaw-v1.2.0 Release](https://github.com/nearai/ironclaw/releases) | [晋升 PR #7625](https://github.com/nearai/ironclaw/pull/7625)

---

## 3. 项目进展

今日合并/关闭的 PR 反映了**性能优化、稳定性修复、文档治理**三条并行推进的路线：

**性能优化（核心团队 serrrfirat）：**
- [#7631 perf(events): coalesce runtime milestone writes](https://github.com/nearai/ironclaw/pull/7631) — 合并运行时里程碑事件写入，共享 `CoalescingEventSink`，减少持久化次数。
- [#7629 perf: reduce trigger and outbound state writes](https://github.com/nearai/ironclaw/pull/7629) — 将触发器运行历史修剪从每次 Running 行更新移动到初始 fire 声明，显著降低写入频率。
- [#7628 perf(processes): remove heartbeat journal churn](https://github.com/nearai/ironclaw/pull/7628) — 停止追加心跳日志行，保留心跳租约时间戳作为唯一权威，并附带 15 秒的 turn-runner 保护。
- [#7630 perf(stress): measure per-turn Postgres writes](https://github.com/nearai/ironclaw/pull/7630) — 新增固定压力测试预设，量化每轮对话的 Postgres 写入，为后续优化提供数据基线。

**功能落地：**
- [#7163 feat(documents)](https://github.com/nearai/ironclaw/pull/7163) — **结构性编辑 docx/xlsx/pptx**，支持从 HTML 渲染 PDF，并修复 #7109 引入的文本日志回归（关闭 #6898 item 3）。这是文档能力的重要补全。
- [#7548 feat(automations): add structured execution contracts](https://github.com/nearai/ironclaw/pull/7548) — 为定时自动化任务引入版本化的结构化执行契约（目标、成功标准、输出指令、允许能力等）。

**社区/文档治理：**
- [#7376 CI 参考门扩展至 docs/ 表面](https://github.com/nearai/ironclaw/pull/7376)（doc-truth PR 2/5）与 [#7378 doc-fact 契约测试](https://github.com/nearai/ironclaw/pull/7378)（doc-truth PR 3/5）——确保文档声明与真实行为一致，提升项目可信度。
- [#7576 固定 AgentExecution 缝的准入契约](https://github.com/nearai/ironclaw/pull/7576)（PR A，测试先行）——为 Phase 2 的 AgentExecution 端口迁移铺路。

**Live Canary 稳定性：**
- [#7590 修复内置技能标记所有者不匹配问题](https://github.com/nearai/ironclaw/pull/7590) + [#7579 扩大 Slack 授权并添加 scrub 判定日志](https://github.com/nearai/ironclaw/pull/7579)——在首次运行中即发现并修复了技能快照验证失败的问题，验证了 canary 系统的有效性。

**整体评估：** v1.2.0 稳定版的落地 + 文档编辑功能闭环 + 性能优化组合拳（事件合并、心跳消除、触发器修剪），标志着项目在**功能广度、运行效率、文档可信度**三个维度同时向前迈进。

---

## 4. 社区热点

**🔥 最热议题：#7482 Epic: Pluggable agent loops（6 条评论）**

这是当前项目的中枢议题，提出了一个宏大的架构愿景：IronClaw 成为"内核"（kernel），负责调度、租户隔离、能力边界、密钥中介、出口边界、持久审计和入站通道，**不再持有 agent 循环和单个集成的工具代码**。循环变成"现成的 harness"（Claude Code、pi、codex 等），集成变成约 30 行配置文件。

今日该 Epic 衍生出 **18 个关联子任务**（#7606-#7623），覆盖：
- **WS1 Egress 边缘**：sandbox 出口 wiring、代理配置渲染器、审计桥接、模型透传、策略记录
- **WS3 外国 harness 执行**：HarnessDriver 契约、HarnessLoopExecutor、phase-0 适配器（claude-code/pi/codex）、固定 agent 镜像
- **WS4 能力访问与 rollout**：能力 socket、ic CLI、合规套件、profile 路由 + shadow 模式
- **WS5 工作区与镜像**：每线程工作区挂载、GC、集成策略记录

**解读：** 社区（尤其是核心贡献者 serrrfirat）正在将"最大风险假设"（sandbox 中运行外部 harness 的安全性）拆解为可执行的工作流。其中 **#7624 被明确标记为"现在唯一需要建设的可插拔循环工作项"**，其余三个整合问题（#7621/#7622/#7623）将按阶梯推进。这是项目未来数周的主航道。

参考链接：[Issue #7482](https://github.com/nearai/ironclaw/issues/7482) | [Issue #7624](https://github.com/nearai/ironclaw/issues/7624)

**💬 次热议题：**
- [#6257 PDF 附件 mime_type 错误（4 条评论，已关闭）](https://github.com/nearai/ironclaw/issues/6257)——用户 Michael Kelly 在 Slack 报告发送/生成 PDF 时报 `Invalid value (attachments.mime_type)` 错误。此问题已关闭，说明已解决或在修复中。
- [#2117 ironclaw-bridge 本地文件/MCP 桥接守护进程（2 条评论，1 👍）](https://github.com/nearai/ironclaw/issues/2117)——云托管部署无法访问用户本地文件（Obsidian vaults、本地项目目录），已有 `src/tun...` 隧道系统，但需要更完善的方案。该 Issue 自 4 月以来持续获得关注。

---

## 5. Bug 与稳定性

按严重程度排列：

| 严重度 | Issue | 描述 | 状态 |
|--------|-------|------|------|
| 🔴 高 | [#6257 PDF mime_type 错误](https://github.com/nearai/ironclaw/issues/6257) | 发送/生成 PDF 文件报 `Invalid value (attachments.mime_type)`，影响文档类工作流 | ✅ 已关闭（有修复） |
| 🔴 高 | [#7589 NEAR AI Cloud Sonnet-5 500 错误](https://github.com/nearai/ironclaw/issues/7589) | Sonnet-5 持续三天返回 500，关联 nearai/cloud-api#920 | ✅ 已关闭 |
| 🟠 中 | [#7626 自定义 MCP 认证卡住](https://github.com/nearai/ironclaw/issues/7626) | 需要浏览器/邮箱验证的 MCP（如 MKT1）连接时卡住，Hermes 已弹浏览器但 IronClaw 无响应 | 🆕 开放，无 fix PR |
| 🟠 中 | [#7627 GitHub 扩展假连接状态](https://github.com/nearai/ironclaw/issues/7627) | 输入任意凭据（用户测试用 "1"）后扩展即显示"已连接"，实际验证不通过 | 🆕 开放，无 fix PR |
| 🟡 低 | [#7185 跨会话记忆不可靠](https://github.com/nearai/ironclaw/issues/7185) | 多个测试者独立观察到：一个对话中建立的上下文/信息无法在后续对话中可靠召回 | 开放中，未关联 fix PR |

**值得关注：** #7626 与 #7627 均为今日新增 bug，涉及认证流程与扩展状态管理，且目前**无关联修复 PR**。考虑到 #7482 Epic 中"授权时批准（grant-time approvals）"的设计决策已绑定，这两个认证相关问题可能需要在新的能力框架中一并解决。

参考链接：[#7626](https://github.com/nearai/ironclaw/issues/7626) | [#7627](https://github.com/nearai/ironclaw/issues/7627) | [#7185](https://github.com/nearai/ironclaw/issues/7185)

---

## 6. 功能请求与路线图信号

**新功能请求（来自今日 Issues）：**
- [#7580 在 Web UI 中暴露 Reborn 版本号](https://github.com/nearai/ironclaw/issues/7580)——用户询问如何从 web UI 查看版本号，说明当前 UI 未明确展示或不易发现。这是一个低成本 UX 改进，大概率会被纳入下一版本。
- [#2117 ironclaw-bridge 本地文件桥接](https://github.com/nearai/ironclaw/issues/2117)——云托管用户访问本地文件的诉求，自 4 月以来未关闭。在 AI 编程助手场景（本地仓库、Obsidian vault）中需求强烈。

**路线图信号：**
- **#7513 ACP serve 命令（PR，开放）**——社区贡献者 Kampouse（标注为 new contributor）提交了通过 ACP 协议在 stdio 上暴露 agent 的 CLI 命令，支持外部工具（VS Code、GitHub Copilot CLI）连接。这与 #7482 Epic 中"ACP executor"方向一致，虽然 Epic 中 ACP 被降级为"one driver implementation"，但此 PR 为社区提供了早期的 ACP 接入路径。
- **#7562 detached turns 设计文档（PR，开放）**——BenKurrek 提交的两个内部设计文档，提出"线程作为工作单元、单次 submit_turn"的模型，涉及 TurnCoordinator → 进程运行时 → 规范循环的执行服务架构。这可能是未来线程/任务模型演进的基础。

**判断：** #7580 预计会快速合入；#2117 需要核心团队评估是否纳入 Reborn 的 sandbox 能力通道（恰好与 #7621 egress edge 和 #7623 capability access 的工作相关）；#7513 的 ACP 方向与 Epic 规划存在交互，建议维护者关注其设计以对齐。

---

## 7. 用户反馈摘要

- **PDF 文档处理受阻（#6257）：** 用户 Michael Kelly 通过 Slack 反馈发送/生成 PDF 报错，怀疑与客户端或 API 层 mime_type 校验有关。该问题已关闭，但反映了**文档类工具链稳定性**是用户高感知区域。

- **AI 记忆不连续（#7185）：** 来自 2026-07-23 Champions 周会的多名测试者（Devon、Tobias 转述等）发现 agent 无法跨对话保留上下文。对法律等专业领域用户（Devon）而言，这直接影响了工作连续性。该问题至今开放，建议维护者关注记忆机制的持久化层。

- **模型服务可靠性（#7589）：** 用户报告 Sonnet-5 连续三天 500 错误，引用 nearai/cloud-api#920。虽然已关闭，但"外部模型服务不稳定"对依赖多模型 provider 的用户影响面较大——这恰好是 #7610 模型透传设计要解决的痛点。

- **认证流程体验缺陷（#7626、#7627）：** 两个新报告均指向认证环节。用户对"extension 显示已连接但实际不可用"的状态感到困惑；MCP 认证卡住则直接阻塞了付费 MCP 工具的使用。这些反馈暗示**凭据生命周期管理**和**连接状态真实性**需要加强。

---

## 8. 待处理积压

| 类型 | 项目 | 持续时长 | 内容 | 建议 |
|------|------|----------|------|------|
| Issue | [#2117 ironclaw-bridge 本地文件桥接](https://github.com/nearai/ironclaw/issues/2117) | 4 个月+（4/7 创建） | 本地文件/MCP 桥接守护进程，有 1 👍，2 条评论 | 与 #7623 能力访问、#7621 egress 设计有协同，建议在 Reborn 能力框架中评估 |
| Issue | [#7185 跨会话记忆不可靠](https://github.com/nearai/ironclaw/issues/7185) | 10 天（8/4 创建） | 记忆召回失败，来自 Champions 周会多名测试者 | 属于核心体验问题，建议在 1.2.0 后安排专项 |
| PR | [#7513 ACP serve 命令](https://github.com/nearai/ironclaw/pull/7513) | 3 天（8/11 创建） | 新贡献者提供 ACP stdio 支持，与 Epic #7482 相关 | 建议核心团队 review 并与 #7624 的设计对齐，避免方向冲突 |
| PR | [#7378 / #7376 doc-truth 系列](https://github.com/nearai/ironclaw/pull/7378) | 7 天（8/7 创建） | 文档契约测试与 CI 门扩展，experienced 贡献者 | 文档治理是长期价值，建议尽快合入避免冲突累积 |

---

**总结：** IronClaw 正处于 **v1.2.0 发布后的架构升级关键期**。核心团队在性能优化（写放大缩减）、文档真实性基建、以及 #7482 的可插拔循环执行框架三个方向同时发力，社区贡献者也通过 ACP 支持、文档测试等渠道积极参与。需要注意的是：#7626/#7627 两个认证相关 bug 尚无修复 PR，且 #7185 记忆问题持续悬置，这三者共同指向"用户体验层的连通性与连续性"是当前短板。建议维护者在下个迭代周期将认证状态管理与跨会话记忆纳入优先项。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报 2026-08-14

---

## 1. 今日速览

项目整体活跃度较高，核心开发节奏集中在渲染层 UI 统一化与企业版能力推进：今日合并/关闭 6 条 PR，其中 4 条为 8 月 13 日新提交并当日合入（skills/MCP 视图合并、cowork 管理 UI 重构、MCP 卡片样式统一、签到活动常驻化），另外还关闭了一条存在 4 个月的定时任务推送修复 PR（#1232）。值得关注的是，当前仍有 5 条 3 月 31 日提交的 PR 处于 stale 状态，长期积压未合入。新 Issue 活动仅 1 条，为测试补强相关的 stale issue 更新（#1162），无新版本发布。

---

## 2. 版本发布

- 今日无新版本发布（最新 Releases 为空）

---

## 3. 项目进展

今日共有 6 条 PR 被关闭/合并，主要集中在界面重构、活动系统与企业版基础建设：

| PR 链接 | 标题 | 类型 | 状态 | 说明 |
| --- | --- | --- | --- | --- |
| [#2487](https://github.com/netease-youdao/LobsterAI/pull/2487) | refactor(skills): merge skills and mcp views into unified skills-and-connectors view | 重构（renderer） | CLOSED | 将 skills 与 MCP 视图合并为统一的「技能与连接器」视图，是 UI 整合的重要一步 |
| [#2488](https://github.com/netease-youdao/LobsterAI/pull/2488) | Refactor/cowork btw and management UI | 重构（renderer, cowork） | CLOSED | Cowork 管理与「btw」界面重构，提升管理界面一致性与可用性 |
| [#2486](https://github.com/netease-youdao/LobsterAI/pull/2486) | refactor(mcp): unify MCP card/detail UI with kits and skills styling | 重构（renderer） | CLOSED | 抽取共享的 CardOverflowMenu 与 managementTypography，统一 MCP/套件/技能三者的卡片与详情样式，改善视觉一致性和维护性 |
| [#2485](https://github.com/netease-youdao/LobsterAI/pull/2485) | feat(activity): support evergreen daily check-in | 功能（renderer, cowork） | CLOSED | 将签到活动调整为常驻（evergreen）形态，复用现有服务端能力；改积分入口为跳转网页 |
| [#2484](https://github.com/netease-youdao/LobsterAI/pull/2484) | Feat/enterprise edition | 功能（main, openclaw, docs…） | CLOSED | 企业版相关功能合入（PR 描述中未提供详细改动内容，建议关注后续版本说明） |
| [#1232](https://github.com/netease-youdao/LobsterAI/pull/1232) | fix(scheduledTask): 修复定时任务首次执行结果不推送到 UI 的问题 | 修复（stale） | CLOSED | 修复 `pollOnce()` 中 `previousRunAtMs > 0` 条件导致首次执行结果不推送的问题，跨 4 个月后最终合入 |

项目整体推进方向清晰：**统一 UI 组件与交互范式 → 活动系统常驻化 → 企业版功能落地 → 定时任务体验补齐**，在界面一致性、用户留存与稳定性三个维度上均有实质进展。

---

## 4. 社区热点

今日社区讨论热度不高，唯一有明显互动的是历史标签 issue：

- **[Issue #1162 — 为 openclawMemoryFile 和 openclawLocalTimeContextPrompt 补充 Vitest 单元测试](https://github.com/netease-youdao/LobsterAI/issues/1162)**
  - 作者：MaoQianTu | 评论：1 | 更新：2026-08-13
  - 该 issue 与 PR #1165 直接关联，核心诉求是消除核心模块（记忆文件管理、时间上下文 Prompt）的零测试状态。两个模块分别被 `coworkRunner`、IM 自动审批等关键路径依赖，此前完全没有测试保护。从 3 月创建至今经历了 stale 标记，但 PR 已提交（75 个测试用例），只是长期未合入。

另一个值得关注的是今日新开的 **[PR #2483 — fix(openclaw): key skill entries by frontmatter name](https://github.com/netease-youdao/LobsterAI/pull/2483)**，指向 #2444 问题：OpenClaw 技能启用开关因目录名与 frontmatter 名称不匹配而静默失效。这是对既有 issue 的实际修复动作，反映了用户在配置技能开关时的真实困惑。

---

## 5. Bug 与稳定性

按严重程度排列：

| 严重程度 | 议题 | 问题描述 | Fix 状态 |
| --- | --- | --- | --- |
| 🟠 中 | [PR #2483](https://github.com/netease-youdao/LobsterAI/pull/2483)（OPEN） | OpenClaw 根据 frontmatter `name` 解析技能启用覆盖，目录名/名称不匹配会导致 UI 开关配置静默无效 | 已有修复 PR 待合并，尚未合入 |
| 🟡 低 | [#1232](https://github.com/netease-youdao/LobsterAI/pull/1232)（已合并） | 定时任务首次执行结果不推送 UI，用户需等到第二次执行才能看到结果 | 今日已合并，修复已在主分支 |
| 🟡 低 | [PR #1163](https://github.com/netease-youdao/LobsterAI/pull/1163)（OPEN，stale） | 定时任务「立即运行」按钮无加载反馈、状态更新依赖 15 秒轮询、IPC handler 同步阻塞 | 修复 PR 待合入，已 stale |
| 🟡 低 | [PR #1166](https://github.com/netease-youdao/LobsterAI/pull/1166)（OPEN，stale） | 创建自定义 agent 时未校验重名，导致 agent 列表产生歧义 | 修复 PR 待合入，已 stale |

稳定性总结：今日无严重崩溃或数据丢失类 bug 报告；长期积压的定时任务推送问题已解决；OpenClaw 技能开关失效问题有修复 PR 但因 stale 存在搁置风险。

---

## 6. 功能请求与路线图信号

从今日合并/新建 PR 观察，项目近期路线图信号包括：

1. **企业版功能上线**（[PR #2484](https://github.com/netease-youdao/LobsterAI/pull/2484)）— 大跨度的企业版特性已合入 main，涉及 main / renderer / docs / openclaw 多个领域，预计将在下个 Release 中向用户面释放
2. **UI 统一化与合并收敛**（[#2487](https://github.com/netease-youdao/LobsterAI/pull/2487)、[#2486](https://github.com/netease-youdao/LobsterAI/pull/2486)、[#2488](https://github.com/netease-youdao/LobsterAI/pull/2488)）— 将 skills、MCP、kits 的卡片/详情/菜单收敛为一致体验，属于持续性的 UI 规范建设
3. **签到活动常驻化**（[#2485](https://github.com/netease-youdao/LobsterAI/pull/2485)）— 从短期活动改为 evergreen 形态，并联动账户积分入口改版，说明商业化/用户活跃留存手段在加码
4. **测试基建补强**（[#1156](https://github.com/netease-youdao/LobsterAI/pull/1156)、[#1165](https://github.com/netease-youdao/LobsterAI/pull/1165)）— 为安全关键模块和记忆管理模块补充 Vitest 单元测试（合计超过 100 个用例），虽为内部质量建设，但反映项目正在提升核心模块的可靠性，为后续迭代提供安全网

判断：企业版 + 签到常驻化有较大概率进入下一版本（`release/2026.8.x`）；UI 合并重构和测试补强是中期质量工程主线。

---

## 7. 用户反馈摘要

今日 24 小时内有效用户反馈较少，提炼如下：

- **关于测试缺失的担忧**（Issue #1162）：MaoQianTu 指出 `openclawMemoryFile.ts` 和 `openclawLocalTimeContextPrompt.ts` 此前"零测试覆盖"，其中记忆文件模块"被多处业务逻辑依赖"（SQLite 迁移、工作区切换同步），而 `commandSafety.ts` 的误判可能导致 AI 静默执行 `rm -rf` 等破坏性命令。说明用户对核心模块缺乏测试保护感到不安，也间接说明当前自动化测试覆盖还有较大提升空间。
- **关于 OpenClaw 技能开关配置失效**（PR #2483 相关联的 #2444）：用户发现 UI 中的技能启用/停用开关在 OpenClaw 侧"静默地无效"，因为目录名与 frontmatter 名称不一致时，OpenClaw 按 frontmatter 名解析覆盖条目。这暴露了 UI 层与 OpenClaw 运行时之间的契约不透明问题。

---

## 8. 待处理积压

⚠️ 以下多条 PR 创建于 2026-03-31，已存在近 4 个半月且被标记为 stale，建议维护者尽快处理：

| 类型 | 链接 | 标题 | 积压天数 | 风险 |
| --- | --- | --- | --- | --- |
| PR | [#1156](https://github.com/netease-youdao/LobsterAI/pull/1156) | 为 commandSafety 和 coworkMemoryJudge 补充 Vitest 单元测试 | ~137 天 | 高风险：`commandSafety.ts` 是危险命令检测模块（rm -rf / git push --force 等），误判将导致破坏性操作被 AI 静默执行，且被 `coworkRunner` 与 IM 自动审批共同依赖 |
| PR | [#1163](https://github.com/netease-youdao/LobsterAI/pull/1163) | fix(定时任务): 补全"立即运行"交互反馈 | ~137 天 | 影响定时任务操作体验，用户无感知反馈、轮询延迟 15 秒 |
| PR | [#1165](https://github.com/netease-youdao/LobsterAI/pull/1165) | 为 openclawMemoryFile 和 openclawLocalTimeContextPrompt 补充 Vitest 单元测试 | ~137 天 | 中风险：记忆文件模块零测试覆盖，涉及 SQLite 迁移与多工作区同步 |
| PR | [#1166](https://github.com/netease-youdao/LobsterAI/pull/1166) | fix(agent): prevent duplicate custom agent names | ~137 天 | 自定义 agent 重名导致列表歧义 |
| Issue | [#1162](https://github.com/netease-youdao/LobsterAI/issues/1162) | 为 openclawMemoryFile 和 openclawLocalTimeContextPrompt 补充 Vitest 单元测试 | ~137 天 | 与 PR #1165 对应，若 PR 被关闭将导致问题继续无人跟进 |

另外，今日新开的 [PR #2483](https://github.com/netease-youdao/LobsterAI/pull/2483)（OpenClaw 技能按 frontmatter name 键控）目前仅创建一天，状态正常，建议在 stale 前尽快评审。

---

**健康度评估**：短期迭代速度良好（合并 6 条、新开 1 条），但长期 PR 积压（5 条 stale，均超过 4 个月）是项目健康度主要隐患——若这些测试补强和安全相关修复持续搁置，后续改动引入的回归风险将逐步累积。建议维护团队尽快安排 stale PR 评审或明确关闭理由，避免技术债继续沉淀。

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis 项目动态日报 — 2026-08-14

## 1. 今日速览

过去 24 小时 Moltis 项目保持稳定活跃：新增/更新 Issue 1 条，PR 更新 4 条，无新版本发布，今日无 PR 被合并或关闭。活跃度主要集中在对 openclaw 组织迁移的后续修复（sandbox 构建、skill 安装、macOS 兼容性）以及一个大型连接器功能 PR 的评审推进。另新增一条 flaky test 报告，涉及全量测试套件下的并发稳定性问题。整体项目健康度良好，处于正常的开发与修复并行节奏。

## 2. 版本发布

过去 24 小时无新版本发布。

## 3. 项目进展

今日无 PR 被合并或关闭。处于待合并状态的 4 个 PR 反映出两个明确方向：

- **迁移收尾修复**：`#1191`（[gogcli module path 指向 openclaw org](https://github.com/moltis-org/moltis/pull/1191)）、`#1192`（[wacrawl install metadata 指向 openclaw org](https://github.com/moltis-org/moltis/pull/1192)）、`#1194`（[macOS bash 3.2 空数组展开守卫](https://github.com/moltis-org/moltis/pull/1194)）三个修复各自解除 sandbox 构建、skill 安装、本地验证脚本的实际阻断。
- **功能扩展**：`#1190`（[Add durable CalDAV and channel history connectors](https://github.com/moltis-org/moltis/pull/1190)）是一个规模较大的功能 PR，引入连接器持久化、原子快照、调度/投影和本地全文搜索能力，并新增 CalDAV、Slack、Discord、Matrix、Teams 的数据集支持。

综合来看，虽然今日没有合并动作，但这批 PR 若顺利合入，将显著改善本地 macOS 开发体验、修复迁移后的依赖路径问题，并大幅扩展 Moltis 的连接器生态。

## 4. 社区热点

今日所有 Issue/PR 的评论数据均未提供或为 0，没有形成激烈讨论。相对最值得关注的是以下两个条目：

- **[PR #1190 — Add durable CalDAV and channel history connectors](https://github.com/moltis-org/moltis/pull/1190)**：从 8/11 创建到 8/13 有更新，涉及面广（新依赖、持久化、全文搜索、多个数据集连接器），是当前 open PR 中体量和技术影响最大的一个。
- **[Issue #1193 — Flaky test: push fanout timeout assertion races under full-suite load](https://github.com/moltis-org/moltis/issues/1193)**：测试稳定性问题通常会引起维护者重视，虽然当前无评论，但标记为 intermittent 且在全量套件下出现，属于需要排期处理的类型。

背后的诉求很清晰：社区一方面在推进功能扩展（连接器/数据持久化），另一方面也在反馈本地工具链和测试可靠性的实际问题。

## 5. Bug 与稳定性

按严重程度从高到低排列：

| 严重程度 | 问题描述 | 状态 |
|---|---|---|
| 🔴 高 | **[PR #1191](https://github.com/moltis-org/moltis/pull/1191)：`moltis sandbox build` 在全部预构建镜像上失败**。生成的 Dockerfile 执行 `go install github.com/steipete/gogcli/cmd/gog@latest`，但 gogcli 已迁移至 `openclaw` 组织，GitHub 返回 HTTP 重定向导致安装失败。属于完全阻断 sandbox 构建的问题。 | 已有 fix PR 待合并 |
| 🟠 中 | **[PR #1192](https://github.com/moltis-org/moltis/pull/1192)：wacrawl skill 的 Go install 路径失效**。`requires.install` 指向 `github.com/steipete/wacrawl`，但项目已迁至 `openclaw` 组织，导致 skill 安装失败。 | 已有 fix PR 待合并 |
| 🟠 中 | **[PR #1194](https://github.com/moltis-org/moltis/pull/1194)：macOS bash 3.2 下 `just local-validate-full` 死掉**。`set -euo pipefail` 下展开空数组 `args[@]` 触发 `unbound variable` 错误，影响 macOS 用户的本地验证流程。 | 已有 fix PR 待合并 |
| 🟡 低 | **[Issue #1193](https://github.com/moltis-org/moltis/issues/1193)：flaky test — `fanout_is_bounded_and_times_out_a_hung_endpoint` 在全量测试套件下间歇性失败**。在空闲的 10 核 macOS 机器上 3 次全量运行失败 2 次（2296/7017），疑似超时断言与全量负载间的竞态。 | 暂无对应 fix PR，仅报告 |

整体来看，前三项为迁移后的路径问题，均有现成修复；flaky test 需要进一步定位，但影响范围限于 CI/测试可信度，不涉及生产功能。

## 6. 功能请求与路线图信号

- **[PR #1190 — connector persistence + CalDAV + channel history](https://github.com/moltis-org/moltis/pull/1190)**：这是当前最明确的路线图信号。该 PR 引入 provider-neutral 连接器持久化、原子快照、调度、投影和本地全文搜索，并添加 CalDAV 数据集以及 Slack/Discord/Matrix/Teams 的历史消息数据集。值得注意的是它以实际代码实现而非 issue 讨论的形式呈现，说明这些能力已被纳入开发计划。

- 结合该项目此前已具备的网关能力，`#1190` 显示了 Moltis 从“实时消息网关”向“带持久化历史的数据接入层”演进的趋势。该 PR 若被合并，下一版本可能会显著扩展可接入的数据源类型和离线/历史数据能力。

## 7. 用户反馈摘要

今日所有 Issue 和 PR 均无用户评论数据。从提交内容可提炼的间接反馈如下：

- **macOS 用户实际使用痛点**：`just local-validate-full` 在 macOS 默认 bash 3.2 环境下直接崩溃，`set -euo pipefail` 与空数组展开的兼容性问题真实存在且阻断本地工作流（来源：[PR #1194](https://github.com/moltis-org/moltis/pull/1194)）。
- **迁移后的依赖断裂**：多个工具（gogcli、wacrawl）迁移到 openclaw 组织后，旧路径的 `go install` 因 GitHub 重定向而失败，说明项目中硬编码的依赖路径存在维护缝隙（来源：[PR #1191](https://github.com/moltis-org/moltis/pull/1191)、[PR #1192](https://github.com/moltis-org/moltis/pull/1192)）。
- **测试可靠性关注**：用户在全量测试下观察到偶发失败，主动报告 flaky test 并注明复现频率和硬件环境（来源：[Issue #1193](https://github.com/moltis-org/moltis/issues/1193)）。

以上三个反馈均来自同一提交者 `Lstarsky0`，且都附带修复 PR，说明该用户深入使用项目本地工具链并积极贡献修复。

## 8. 待处理积压

当前无长期未响应（超过 7 天）的 Issue 或 PR。需要提示维护者的事项：

- **[PR #1190](https://github.com/moltis-org/moltis/pull/1190)** 自 8/11 创建至今已 2 天，涉及 connector 持久化和多个数据源新增，属于大型 PR。为避免 diff 规模过大导致 review 困难，建议尽早安排评审，必要时拆分为多个较小的 PR。
- **[Issue #1193](https://github.com/moltis-org/moltis/issues/1193)** 虽为 flaky test，但失败率较高（2/3 全量运行），且发生在 macOS 10 核空闲机器上，可能是测试逻辑本身存在真实竞态。建议在 `#1191`、`#1192`、`#1194` 三个修复 PR 合入后，优先排查该测试的超时断言实现。

---

*本日报由 AI 基于 GitHub 公开数据自动生成，仅供参考。数据时间范围：2026-08-13 至 2026-08-14。*

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw 项目动态日报（2026-08-14）

> 数据源：agentscope-ai/QwenPaw（过去 24 小时 GitHub 事件）

---

## 1. 今日速览

过去 24 小时 CoPaw 整体活跃度较高：共发生 42 条 Issue 更新（新开/活跃 25，关闭 17），50 条 PR 更新（待合并 31，已合并/关闭 19），并发布 2 个新版本。v2.1.0 正式版引入 **QwenPaw OS Shell**，是近期最重要的功能里程碑；v2.1.0-beta.5 则集中修复对话解析、记忆引导与文档问题。社区讨论热点集中在 **多步任务中断、安全/鉴权疑虑、长期记忆透明性、Windows 客户端稳定性** 等方向。整体看项目迭代节奏快，但高影响 Bug 和安全类反馈仍需重点跟进。

---

## 2. 版本发布

### v2.1.0（正式版）

核心更新：

- **QwenPaw OS Shell**：应用现在可以在可移动、可缩放的窗口中打开，内置启动器、任务栏、通知体系与保存布局能力。
- **App Center 与应用商店统一目录**：已安装应用和 Marketplace 应用共享同一应用目录。
- 相关 PR：[#6645](https://github.com/agentscope-ai/QwenPaw/pull/6645)

发布说明未明确列出破坏性变更。由于 v2.1.0 引入新的 Shell 层，建议升级前备份 workspace 和配置文件，并关注 Windows 端相关稳定性反馈（如 [#6847](https://github.com/agentscope-ai/QwenPaw/issues/6847)、[#6780](https://github.com/agentscope-ai/QwenPaw/issues/6780)）。

### v2.1.0-beta.5

- 修复对话中 dict-like 模型响应处理问题：[#6816](https://github.com/agentscope-ai/QwenPaw/pull/6816)
- 简化长期记忆引导逻辑：[#6942](https://github.com/agentscope-ai/QwenPaw/pull/6942)
- 更新 website 中 Files workspace 相关文档

---

## 3. 项目进展

今日共合并/关闭 19 个 PR，重要进展包括：

- [PR #6652](https://github.com/agentscope-ai/QwenPaw/pull/6652)：**Mission Mode 在服务端强制 max_iterations**，修复 controller 可无限制派生子 Agent、直至耗尽模型余额的问题。
- [PR #6636](https://github.com/agentscope-ai/QwenPaw/pull/6636)：**Chat 历史接口增加分页并启用 GZip**，缓解长对话 1MB+ 响应导致 30s 超时的问题。
- [PR #6884](https://github.com/agentscope-ai/QwenPaw/pull/6884)：**Auto-Dream 集成容错**，单个畸形 LLM 结构化输出不再中断整个任务。
- [PR #6387](https://github.com/agentscope-ai/QwenPaw/pull/6387)：**Channels 可选依赖按需安装**，保留 Console 可见性的同时减小默认依赖。
- [PR #6989](https://github.com/agentscope-ai/QwenPaw/pull/6989)：更新 v2.1.0 release notes。

仍在审核中的高价值 PR 也较多，若合并将进一步扩展项目能力：

- [PR #6960](https://github.com/agentscope-ai/QwenPaw/pull/6960)：支持从 Codex、Qoder 等 Agent 导入配置、技能、插件与项目。
- [PR #6976](https://github.com/agentscope-ai/QwenPaw/pull/6976)：支持 session 级多项目目录绑定。
- [PR #6998](https://github.com/agentscope-ai/QwenPaw/pull/6998)：修复 LLM 流未消费导致的 semaphore 泄漏。
- [PR #7001](https://github.com/agentscope-ai/QwenPaw/pull/7001)：Matrix 群组房间按发送者隔离 session 与 memory。
- [PR #7004](https://github.com/agentscope-ai/QwenPaw/pull/7004)：持久化 spawn 子会话与父会话关系。
- [PR #6975](https://github.com/agentscope-ai/QwenPaw/pull/6975)：修复 `/compact` 后 Console 上下文用量环不更新的问题。
- [PR #6990](https://github.com/agentscope-ai/QwenPaw/pull/6990)：为 system prompts 与 Skills 文件增加缓存，减少文件 IO。

整体来看，项目不仅在持续修复稳定性，也在推进“多项目工作区、跨 Agent 导入、渠道级隔离、资源占用优化”等方向，路线图信号清晰。

---

## 4. 社区热点

今日评论数最高的 Issue 集中在以下几类：

- [Issue #6921](https://github.com/agentscope-ai/QwenPaw/issues/6921)（6 条评论）：**多步任务频繁自行中断**，经常输出“Now 2.1, 3.1, 3.2. Let me do all three.”后直接停止，需要用户说“继续”才会执行。这是当前直接影响任务自动化体验的高频问题。
- [Issue #6973](https://github.com/agentscope-ai/QwenPaw/issues/6973)（5 条评论）：用户希望 **QwenPaw Creator 支持阿里云百炼 token plan**，反映国内开发者对降低模型调用成本的需求。
- [Issue #6811](https://github.com/agentscope-ai/QwenPaw/issues/6811)（5 条评论，已关闭）：OpenAI Responses 续写摘要忽略 `disable_thinking`，且把 60 秒取消误报为 malformed output。
- [Issue #6853](https://github.com/agentscope-ai/QwenPaw/issues/6853)（5 条评论，已关闭）：**prompts.py 对 Agent 的描述与实际行为不符**，声称 Dream 会自动同步 digest 到 MEMORY.md，但实际从未实现。
- [Issue #6992](https://github.com/agentscope-ai/QwenPaw/issues/6992) / [#6993](https://github.com/agentscope-ai/QwenPaw/issues/6993)：用户报告 **8088 端口公网暴露、插件 API 无鉴权、可被植入后门** 等严重安全问题，虽然被标记为 invalid 关闭，但同类安全反馈仍在社区引起关注。

社区诉求关键词：**任务连续性、模型成本、记忆行为透明性、安全边界**。

---

## 5. Bug 与稳定性

按严重程度排列：

| 严重度 | Issue/PR | 描述 | 状态 |
|---|---|---|---|
| 严重 | [Issue #6992](https://github.com/agentscope-ai/QwenPaw/issues/6992) / [#6993](https://github.com/agentscope-ai/QwenPaw/issues/6993) | 端口暴露、API 无鉴权、恶意插件可取得立足点并持久化 | 已关闭（invalid），建议维护者复核 |
| 严重 | [Issue #6916](https://github.com/agentscope-ai/QwenPaw/issues/6916) | 插件可在无用户确认的情况下创建 cron 任务并注入用户可见消息 | 已关闭，安全权限模型缺口 |
| 高 | [Issue #6921](https://github.com/agentscope-ai/QwenPaw/issues/6921) | 多步任务自行停止，无提示，需用户说“继续” | Open，无明确 fix PR |
| 高 | [Issue #6768](https://github.com/agentscope-ai/QwenPaw/issues/6768) | Agent 完成多步任务后进入无限循环，会话阻塞数小时 | 已关闭 |
| 高 | [Issue #6047](https://github.com/agentscope-ai/QwenPaw/issues/6047) | 升级后新聊天会重新打开旧 session，chats.json 顺序错乱 | 已关闭 |
| 中 | [Issue #6951](https://github.com/agentscope-ai/QwenPaw/issues/6951) | Scroll 压缩后重新进入会话，压缩前记录不可见 | Open |
| 中 | [Issue #6955](https://github.com/agentscope-ai/QwenPaw/issues/6955) | Windows 下概率性启动报错、崩溃退出 | Open |
| 中 | [Issue #7007](https://github.com/agentscope-ai/QwenPaw/issues/7007) | Windows Desktop TUI 无法启动，报 `transport: Connection closed` | Open |
| 中 | [Issue #7008](https://github.com/agentscope-ai/QwenPaw/issues/7008) | Anthropic 模型端误判“图片敏感”，导致长历史会话中断 | Open |
| 中 | [Issue #7005](https://github.com/agentscope-ai/QwenPaw/issues/7005) | 启用 Shabox 后 UV 无法写入 `~/.cache/uv` | Open |
| 中 | [Issue #6780](https://github.com/agentscope-ai/QwenPaw/issues/6780) | 闲置几十分钟后进程卡死，只能重启 | Open |

今日也有多个稳定性修复 PR 进入待合并阶段：

- [PR #6998](https://github.com/agentscope-ai/QwenPaw/pull/6998)：修复流式 LLM 调用被中止时 semaphore 泄漏。
- [PR #6996](https://github.com/agentscope-ai/QwenPaw/pull/6996)：插件 reload 前恢复 workspace 状态，避免热替换丢状态。
- [PR #6990](https://github.com/agentscope-ai/QwenPaw/pull/6990)：加入文件缓存，减少 system prompts 与 Skills 重复 IO。

---

## 6. 功能请求与路线图信号

近期用户提出的新功能需求包括：

- [Issue #6970](https://github.com/agentscope-ai/QwenPaw/issues/6970)：支持 **可嵌入的 Chat 子页面**，去掉侧边栏/头部栏；URL 携带 API key 绕过鉴权；session 列表支持日期与 sessionId 精确筛选。
- [Issue #7002](https://github.com/agentscope-ai/QwenPaw/issues/7002)：希望提供 **服务器端部署 + 个人电脑轻量代理客户端** 的架构，以复用服务器 Agent 并控制本地桌面。
- [Issue #6995](https://github.com/agentscope-ai/QwenPaw/issues/6995)：在 shell 子进程环境中注入 `QWENPAW_CHANNEL` 环境变量。
- [Issue #7003](https://github.com/agentscope-ai/QwenPaw/issues/7003)：提出 **ViBo 记忆方案**，宣称可减少 97.5% 的 memory tokens。

结合已提交 PR，以下功能很可能进入后续版本：

- **跨 Agent 导入**：[PR #6960](https://github.com/agentscope-ai/QwenPaw/pull/6960) 已实现从 Codex/Qoder 导入配置与技能，生态兼容方向明确。
- **多项目目录**：[PR #6976](https://github.com/agentscope-ai/QwenPaw/pull/6976) 将 session 绑定到有序项目目录列表。
- **渠道级隔离**：[PR #7001](https://github.com/agentscope-ai/QwenPaw/pull/7001) 修复 Matrix 群组所有成员共享同一上下文的问题。
- **自定义 Provider 能力模板**：[PR #6823](https://github.com/agentscope-ai/QwenPaw/pull/6823) 为自定义 OpenAI-compatible provider 自动套用官方模型能力模板。
- **记忆可观测性**：[PR #6984](https://github.com/agentscope-ai/QwenPaw/pull/6984) 重新设计 ReMe 运行时状态 Dashboard。

---

## 7. 用户反馈摘要

从今日活跃 Issue 中可以提炼出以下真实用户痛点：

- **任务自动化中断是最大痛点**：用户在多步操作中反复遇到“规划完成但未执行”的情况，必须手动说“继续”，严重影响无人值守场景。
- **Windows 端稳定性信心不足**：部分用户反馈启动崩溃（[#6955](https://github.com/agentscope-ai/QwenPaw/issues/6955)）、闲置卡死（[#6780](https://github.com/agentscope-ai/QwenPaw/issues/6780)）、甚至被杀软误杀（[#6847](https://github.com/agentscope-ai/QwenPaw/issues/6847)）。
- **记忆行为需要更透明**：有用户指出 long-term memory 文档与实际行为不符（[#6853](https://github.com/agentscope-ai/QwenPaw/issues/6853)），也有用户遇到上下文压缩后聊天记录不可见（[#6951](https://github.com/agentscope-ai/QwenPaw/issues/6951)）。
- **国内云服务集成需求明显**：多个中文用户希望接入阿里云百炼 token plan（[#6973](https://github.com/agentscope-ai/QwenPaw/issues/6973)），并咨询 CopilotKit 集成方式（[#6882](https://github.com/agentscope-ai/QwenPaw/issues/6882)）。
- **渠道行为一致性有待提升**：Telegram 中 `/new` 不会轮换 session ID，导致上下文无限增长（[#6966](https://github.com/agentscope-ai/QwenPaw/issues/6966)）；Matrix 群组会话隔离缺失（[PR #7001](https://github.com/agentscope-ai/QwenPaw/pull/7001)）。
- **UI 细节影响注意力**：有用户反馈聊天输入框“已接收字符数”动态闪烁没有关闭入口（[#6585](https://github.com/agentscope-ai/QwenPaw/issues/6585)）。

---

## 8. 待处理积压

以下重要 PR/Issue 从创建时间看已在队列中停留较久，建议维护者关注：

- [PR #6302](https://github.com/agentscope-ai/QwenPaw/pull/6302)（创建于 2026-07-21）：统一 provider 发现、模型元数据、路由与 Agent 模型控制，涉及核心架构，已近 4 周仍未合并。
- [PR #6715](https://github.com/agentscope-ai/QwenPaw/pull/6715)（创建于 2026-08-05）：OneBot 入站媒体本地化，已进入 Under Review，但需要维护者持续推进。
- [PR #6823](https://github.com/agentscope-ai/QwenPaw/pull/6823)（创建于 2026-08-08）：自定义 provider 能力模板自动匹配，社区有明确价值，仍处于 first-time-contributor 待审状态。
- [Issue #6780](https://github.com/agentscope-ai/QwenPaw/issues/6780)（创建于 2026-08-07）：Windows 闲置卡死问题仍 Open，影响桌面端基本可用性。
- [Issue #7003](https://github.com/agentscope-ai/QwenPaw/issues/7003)（创建于 2026-08-13）：外部记忆方案提案，需要官方明确长期记忆路线图，避免社区重复造轮子。

---

**总结**：CoPaw 今日发布 v2.1.0 正式版，功能推进明显，同时有大量稳定性与安全相关反馈进入视野。项目活跃度高、迭代节奏快，但需在 **任务执行连续性、权限模型、Windows 端稳定性、记忆行为透明性** 上投入更多精力，以巩固用户信任。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目动态日报 — 2026-08-14

## 1. 今日速览

过去 24 小时 ZeroClaw 保持高活跃度：50 条 Issue 更新（37 条新开/活跃、13 条关闭），50 条 PR 更新（43 条待合并、7 条已合并/关闭），无新版本发布。项目正处于 v0.9.0 安全加固与架构决策密集期，多条 RFC（shell 命令策略 #7155、运行时会话所有权 #9487、Goal mode #8303）等待维护者裁决。安全修复进展显著：dashboard 静态资源路径穿越/符号链接逃逸修复（[PR #9969](https://github.com/zeroclaw-labs/zeroclaw/pull/9969)）已合并，Zhipu 兼容提供商 JWT 凭证完整性修复（[PR #9968](https://github.com/zeroclaw-labs/zeroclaw/pull/9968)）已提交。整体项目健康度良好，但 43 条待合并 PR 与多条 blocked/needs-author-action Issue 显示决策评审已成为当前主要瓶颈。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

今日 7 条 PR 合并/关闭，集中在安全加固、稳定性与 CI 基础设施，累计推进如下：

**安全加固**
- [PR #9969 fix(gateway): contain filesystem dashboard assets](https://github.com/zeroclaw-labs/zeroclaw/pull/9969)：对文件系统型 dashboard 资源路径做 canonicalize，并在读取时校验其必须位于配置的 distribution root 内，拒绝稳定符号链接逃逸。网关资产加载路径的关键安全边界已补上。

**稳定性**
- [PR #9674 fix(infra): preserve session queue serialization during eviction](https://github.com/zeroclaw-labs/zeroclaw/pull/9674)：在会话槽位锁仍持有时注册会话请求，配合 RAII guard 跟踪 pending 计数，避免空闲驱逐在 pending 数可见之前移除已选槽位，修复并发竞态。

**CI 效率**
- [PR #9980 ci(docker): sticky-disk layer cache for PR image builds](https://github.com/zeroclaw-labs/zeroclaw/pull/9980)：PR 镜像构建（约 78 次/两周、单次 15–49 分钟）引入 sticky-disk 层缓存，缓解 GitHub 10GB/repo 缓存抖动。
- [PR #9932 ci(codeql): drop rust/hard-coded-cryptographic-value](https://github.com/zeroclaw-labs/zeroclaw/pull/9932)：移除 CodeQL 中 27 条全为 false positive 的告警查询，降低 CI 噪音。

**文档与国际化**
- [PR #9639 docs(architecture): document provider routing lifecycle](https://github.com/zeroclaw-labs/zeroclaw/pull/9639)：新增 provider 路由生命周期文档，覆盖 profile 构造、hint 路由、重试/回退顺序、冷却、流恢复与 no-replay 边界。
- [PR #8546 fix(cli): localize status fragments](https://github.com/zeroclaw-labs/zeroclaw/pull/8546)：将 `zeroclaw status` 的 agent 风险概要片段接入 Fluent i18n 层。
- [PR #9984](https://github.com/zeroclaw-labs/zeroclaw/pull/9984) 为验证 Blacksmith rust-cache 路径的临时 PR，已按预期关闭（不合并）。

**评估**：安全与工程基建维度推进扎实，但功能特性类 PR（Telegram 媒体组 #8955、LSP #5907 等）仍积压在评审队列中。

## 4. 社区热点

按评论数排序的讨论焦点（均于今日更新）：

| Issue | 评论数 | 主题 |
|---|---|---|
| [#8303 RFC: Goal mode v1](https://github.com/zeroclaw-labs/zeroclaw/issues/8303) | 20 | 有界用户目标跨多轮持久执行；此前提案因耦合重启交接、广播信道、Web 与异步子任务而范围过大，社区正收敛首版交付边界 |
| [#7155 RFC: 高风险 shell 命令确认层级](https://github.com/zeroclaw-labs/zeroclaw/issues/7155) | 18 | 已修订至 Rev 3，规范范围收窄回 shell 策略契约；借鉴 Claude Code 的 allow/ask/deny 模式（P1） |
| [#8692 Tracker: 维护者 RFC 决策队列](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) | 13 | 作为所有 RFC/设计 Issue 的公开决策队列，持续累积，反映架构决策成为项目主要瓶颈 |
| [#6850 RFC: 解耦内存生命周期策略与存储后端](https://github.com/zeroclaw-labs/zeroclaw/issues/6850) | 12 | Memory trait 应只负责存储，合并/治理等生命周期决策不应由各 gateway/channel 重复实现 |
| [#9328 Bug: verifiable-intent 未验证凭证链](https://github.com/zeroclaw-labs/zeroclaw/issues/9328) | 12 | vi_verify 的 evaluate_constraints 对调用方提供的 fulfillment 直接评估 L2 约束，而参考实现要求先经链上加密验证（已接受/进行中） |
| [#9487 RFC: 运行时拥有的会话与传输面适配器](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) | 11 | 确立 #9487/#9488/#9600 所有权边界，要求所有迁移入口提交 InboundAction，加入持久准入与歧义结果语义 |

**诉求分析**：社区最关心三类问题——（1）**安全策略**：shell 命令执行确认、凭证链验证（#7155、#9328）；（2）**架构所有权清晰化**：会话持久化、内存生命周期、命令注册表多个 workstream 触碰同一契约，需 tracker 协调（#9487、#9600、#8692）；（3）**多轮长任务能力**：Goal mode、SOP 持久化，说明生产级 agent 场景需求正在浮现。

## 5. Bug 与稳定性

今日活跃 Bug 按严重程度排列：

**严重 / 高优先级（P1, risk:high）**
- [已关闭] [#9389 unauthenticated POST /api/pair 以攻击者可控 header 作为锁定键](https://github.com/zeroclaw-labs/zeroclaw/issues/9389)：未认证配对接口的 lockout 状态绑定在攻击者可伪造的头字段上，可被用于绕过锁定或制造 DoS。已关闭（security:pairing）。
- [修复已合并] [PR #9969 dashboard 资产路径穿越/符号链接逃逸](https://github.com/zeroclaw-labs/zeroclaw/pull/9969)：已在读取前 canonicalize 并校验 containment。
- [修复 PR 待审] [PR #9968 Zhipu 兼容提供商凭证完整性](https://github.com/zeroclaw-labs/zeroclaw/pull/9968)：Zhipu 凭证无法生成合法 JWT 时当前会转发原始凭证为 bearer token；PR 改为 fail closed + 结构化 JSON 构建 JWT payload。
- [进行中/已接受] [#9328 verifiable-intent 凭证链缺失](https://github.com/zeroclaw-labs/zeroclaw/issues/9328)：约束评估未验证凭证链。相关 [PR #9942](https://github.com/zeroclaw-labs/zeroclaw/pull/9942) 先将 vi_verify 被扣留的说明通过配置面暴露（解决 log_persistence=none 时 trace 无 sink 的问题），核心修复仍需跟进。
- [已关闭] [#9643 wit/VERSIONING.md 未分类向已有 enum 添加变体](https://github.com/zeroclaw-labs/zeroclaw/issues/9643)：该变更会破坏所有已编译插件，文档缺失导致兼容性事故（docs, P1）。

**中等（P2）**
- [已接受/阻塞] [#9929 headless SOP step turn 有 session path 但从未持久化](https://github.com/zeroclaw-labs/zeroclaw/issues/9929)：`drive_headless_run` 在 executor.rs:211-212 构建 session_path 却未落库，S2 降级行为，由 #9600 tracker 接管。
- [已关闭] [#9951 WeChat channel 的 51 个单元测试在 CI 中从不编译/执行](https://github.com/zeroclaw-labs/zeroclaw/issues/9951)：`channel-wechat` feature 不在任何 CI feature 组合中，代码与测试处于"假绿"状态（type:ci）。
- [已关闭] [#9366 WhatsApp Web 接受 approval_timeout_secs 配置但从不读取](https://github.com/zeroclaw-labs/zeroclaw/issues/9366)：配置面与行为面不一致（由 #9348 拆分）。

**轻微（P3）**
- [已关闭] [#9710 macOS desktop 截图临时文件两条早退路径未清理](https://github.com/zeroclaw-labs/zeroclaw/issues/9710)。
- [已关闭] [#9706 Edge TTS 临时文件部分错误路径未清理](https://github.com/zeroclaw-labs/zeroclaw/issues/9706)。

**健康度观察**：安全类 Bug 修复速度较快（#9389、#9969 均当日关闭/合并），但 #9328 凭证链、#9929 SOP 持久化等深层正确性问题仍处 in-progress/blocked。两个已关闭 Bug（#9951、#9366）属于"配置/测试表面接受但实际未生效"类问题，是值得警惕的质量信号。

## 6. 功能请求与路线图信号

今日活跃的新功能/增强请求：

- [#9631 向 OpenRouter 发送稳定 session_id 以启用 prompt cache](https://github.com/zeroclaw-labs/zeroclaw/issues/9631)（P2, blocked）：解决"每轮重放 system prompt 与 tool schema 导致成本虚高"的痛点，落地后将直接降低所有 OpenRouter 用户的 API 成本，商业价值明确。
- [#9895 Telegram /model 选择器改为 provider 分组 + 分页 inline-keyboard](https://github.com/zeroclaw-labs/zeroclaw/issues/9895)（P2, accepted）：移动端文本命令在路由众多时难以操作。
- [#9945 browser 工具暴露 agent-browser 的 100+ 命令](https://github.com/zeroclaw-labs/zeroclaw/issues/9945)（P2, blocked/accepted）：当前仅 16 个 action，iframe、JS dialog、tab、表单控件均不可达，限制浏览器自动化深度。
- [#9887 超大图片降采样而非直接丢弃 + 可用 0 禁用限制](https://github.com/zeroclaw-labs/zeroclaw/issues/9887)（P2, blocked/accepted）：>5MiB 的合法图片被直接拒绝并提示"无法加载"。
- [#9810 RFC: 加载 Agent Plugins 1.0 skill 与 MCP 包](https://github.com/zeroclaw-labs/zeroclaw/issues/9810)（P2, blocked）：支持 vendor-neutral 的 plugin.json + skills/ + mcp.json 标准，扩大生态兼容性。
- [#5907 ZeroCode 编码工作流可选 LSP 支持](https://github.com/zeroclaw-labs/zeroclaw/issues/5907)（P2, needs-author-action，4 月提出）：语言服务器作为减少幻觉的 backstop，本地模型场景尤其受益。

**路线图判断**：多个已接受但 blocked 的功能（#9945、#9887、#9895）显示 v0.9.0 功能范围已锁定，新功能需等安全与架构决策完成后排入。成本优化类（#9631）与生态标准接入（#9810）是呼声较高的下一阶段方向。

## 7. 用户反馈摘要

- **成本痛点（OpenRouter）**："单个会话产生几十次 LLM 请求，system prompt 和 tool schemas 每轮都被重放"——#9631 指出无 session_id 时 prompt 缓存完全失效，长会话成本被显著推高。
- **安全功能误伤真实场景（区块链支付）**：[#9825](https://github.com/zeroclaw-labs/zeroclaw/issues/9825) 指出外向泄漏检测器的高熵启发式把**公开的区块链地址**也一并红act，导致支付请求 URL 无法投递。用户强调"这是假阳性而非 bug——检测器按设计工作"，需要 publish-safe 例外机制。
- **工具能力落差（browser tool）**："16 个 action vs 100+ 命令"，iframe、JS 弹窗、标签页和表单控件不可达——用户对 agent 浏览器自动化的完整度有明确期待。
- **移动端体验（Telegram）**：路由众多时文本式 /model 命令"在手机上仍然笨重"，需要分组分页选择器。
- **跨模型兼容性（memory 合并）**：[#6998](https://github.com/zeroclaw-labs/zeroclaw/issues/6998) 指出依赖模型输出 JSON 再 serde_json 解析的方式"跨 provider 和模型都脆弱"，Markdown fence、多余 prose、缺字段或非法 JSON 频发。
- **对 CI 质量的信任**：#9951 揭示 WeChat channel 代码与 51 个测试从不被 CI 编译，"假绿"状态会直接降低用户对发布质量的信任。
- **安全能力可见性**：[PR #9942](https://github.com/zeroclaw-labs/zeroclaw/pull/9942) 的动机是 vi_verify 被扣留的说明只通过 runtime trace 传递，而 `log_persistence = "none"` 时该 trace 没有 sink——安全能力被静默隐藏，operator 无从知晓。

## 8. 待处理积压

**长期未决 Issue**
- [#5907 ZeroCode 可选 LSP 支持](https://github.com/zeroclaw-labs/zeroclaw/issues/5907)（2026-04-19 提出，近 4 个月无实质进展，needs-author-action）。
- [#6850 RFC: 内存生命周期策略与存储后端解耦](https://github.com/zeroclaw-labs/zeroclaw/issues/6850)（2026-05-22 提出，12 条评论，等待作者响应）。
- [#7155 RFC: 高风险 shell 命令确认层级](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)（P1, 已到 Rev 3，18 条评论，等待维护者最终裁决；[#9978](https://github.com/zeroclaw-labs/zeroclaw/issues/9978) 提出可参考 DeepSeek Harness 的设计）。
- [#8303 RFC: Goal mode v1](https://github.com/zeroclaw-labs/zeroclaw/issues/8303)（今日评论最多，范围已收敛，等待验收）。
- [#9323 执行树迭代预算所有权](https://github.com/zeroclaw-labs/zeroclaw/issues/9323)（ToolLoop.shared_budget 所有生产 root 均为 None，子代理/委托循环不受预算约束）。
- [#8692 维护者 RFC 决策队列](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)：累积决策持续增长，建议维护者评估每周固定评审时段，避免 #9487、#9598、#9810、#9880 等关键 RFC 互相阻塞。

**积压 PR（评审/作者响应超过 2 周）**
- [#9002 fix(gateway): keep agent turns alive after viewer disconnect](https://github.com/zeroclaw-labs/zeroclaw/pull/9002)（P1, needs-maintainer-review，2026-07-11）：观看者断开 WebSocket 不应取消 agent turn，影响真实使用稳定性，已搁置超一个月。
- [#8955 fix(telegram): batch media group attachments](https://github.com/zeroclaw-labs/zeroclaw/pull/8955)（P2, needs-maintainer-review，2026-07-10）：Telegram 相册消息被拆成独立 turn 的体验修复。
- [#9013 refactor(config)!: TodoWrite 显示配置从 daemon 迁入 zerocode](https://github.com/zeroclaw-labs/zeroclaw/pull/9013)（breaking change, size:XL，2026-07-12）：展示类配置不应由 daemon schema 持有。
- [#9424 fix(runtime): reject semantic-empty terminal completions](https://github.com/zeroclaw-labs/zeroclaw/pull/9424)（P1, in-progress, size:XL）：空/纯 think 终态响应不应报成功，涉及 Reliable 重试与 fallback，评审周期已近 3 周。
- [#9635 fix(config): 风险分类器解析 git 子命令越过全局选项](https://github.com/zeroclaw-labs/zeroclaw/pull/9635)（P1, needs-author-action）：`git -C <path> <verb>` 下 args.first() 读到的是全局选项而非子命令，导致风险误判，属安全相关。

**风险提示**：43 条待合并 PR 中，多条 P1 级安全/稳定性修复（#9002、#9424、#9635、#9968）均处于待评审或待作者响应状态。若评审吞吐不提升，v0.9.0 安全加固里程碑存在延期风险。

---

*数据来源：ZeroClaw GitHub 仓库（github.com/zeroclaw-labs/zeroclaw），统计窗口 2026-08-13 至 2026-08-14。*

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*