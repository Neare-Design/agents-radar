# OpenClaw 生态日报 2026-08-15

> Issues: 500 | PRs: 500 | 覆盖项目: 12 个 | 生成时间: 2026-08-14 23:14 UTC

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

# OpenClaw 项目动态日报 — 2026-08-15

> 数据窗口：2026-08-14 至 2026-08-15 | 数据来源：[github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)

---

## 1. 今日速览

过去 24 小时项目活跃度极高，Issue 与 PR 更新均达 500 条，但新增/活跃 Issue（488）远超关闭数（12），PR 端则有 97 条被合并/关闭、403 条仍待处理，修复输出速度暂时无法消化持续的 bug 报告。社区讨论高度集中在消息静默丢失（#121058）、Gateway 内存泄漏（#91588）等老问题上，用户对「issue 被关闭但问题依旧复现」表现出明显不满。项目今日无新版本发布，大量高优先级修复仍停留在 open PR 阶段，整体健康度偏紧。

---

## 2. 版本发布

过去 24 小时无新版本发布，无 release 信息可汇报。

---

## 3. 项目进展

过去 24 小时共有 97 个 PR 被合并/关闭，修复管线保持运转，核心方向集中在 Gateway 稳定性、session 状态一致性和多通道消息投递可靠性。受数据窗口限制，以下为当前可见的关键 PR（多数处于待维护者 review 状态，尚未合并）：

| PR | 内容 | 关联 Issue | 状态 |
|---|---|---|---|
| [#123878](https://github.com/openclaw/openclaw/pull/123878) | fix(security): 显式 fleet 配置下跳过未使用的默认 workspace，消除安全审计误报 | — | ✅ CLOSED |
| [#121081](https://github.com/openclaw/openclaw/pull/121081) | fix(gateway): 隧道启动前拒绝过期 worker bundle，防止恢复流程使用错误版本 | #121036 | 👀 ready for maintainer |
| [#121299](https://github.com/openclaw/openclaw/pull/121299) | fix(gateway): 配置热重载时仅对变更 agents 刷新 prepared-model 运行时快照，消除 O(N) 事件循环阻塞 | #120154 | 👀 ready for maintainer |
| [#121022](https://github.com/openclaw/openclaw/pull/121022) | fix(gateway): `config.patch`/`config.apply` 等待运行时实际生效后再返回成功，避免状态不一致 | #120408 | 👀 ready for maintainer |
| [#121167](https://github.com/openclaw/openclaw/pull/121167) | fix(beam): 仅将确认不活跃的会话标记为完成，防止活跃 coding session 被错误归档 | #121132 | 👀 ready for maintainer |
| [#121186](https://github.com/openclaw/openclaw/pull/121186) | fix(beam): 终态 mirror 上传失败时自动重试，避免远程会话虚挂 7 天 | #121176 | 👀 ready for maintainer |
| [#121204](https://github.com/openclaw/openclaw/pull/121204) | fix(discord)：恢复后旧 ambient 消息不再饿死实时 mention，修复回复昨日消息的问题 | #97435 | 👀 ready for maintainer |
| [#121195](https://github.com/openclaw/openclaw/pull/121195) | fix(agents): yield 的 requester 完成事件保证只消费一次，解决重复/丢失完成信号 | #121187 等 11 个关联 | 👀 ready for maintainer |
| [#120900](https://github.com/openclaw/openclaw/pull/120900) | feat(ui): 管理员可在 Control UI 审阅 install-policy 警告并确认继续安装插件 | — | 👀 ready for maintainer |

**判断**：尽管 0 release，但修复方向精准针对社区反馈最强烈的 session-state / message-loss / gateway 稳定性问题。PR 质量门禁（proof 标签、merge-risk 标注、maintainer review）执行严格，合入节奏偏保守。

---

## 4. 社区热点

### 🔥 #121058 — Silent reply failures still recurring（94 条评论）
[Issue #121058](https://github.com/openclaw/openclaw/issues/121058)

**热度断层第一**。用户报告 #116277 被关闭后，silent-reply 失败模式仍在持续，监控 cron 在 issue 关闭后仍不断记录新发生事件。94 条评论表明大量用户在同一问题上共鸣。背后的核心诉求是：**修复是否真正覆盖了根因？关闭 issue 的标准是什么？** 如果本项目引入 clawbot 自动关闭机制，这种「关闭后复发」会显著消耗社区信任。

### 🔥 #91588 — Gateway 内存泄漏 P0（24 条评论）
[Issue #91588](https://github.com/openclaw/openclaw/issues/91588)

RSS 从 350MB 增长到 15.5GB 后触发 OOM，导致 launchd-handoff 反复重启。创建于 6 月 9 日，至今 2 个月未关闭，已升级为 P0 并附带 5 个 impact 标签。用户提供了完整的复现路径和环境数据，等待维护者实质修复。

### 🔥 #121953 — DeepSeek cron 任务 stall（20 条评论）
[Issue #121953](https://github.com/openclaw/openclaw/issues/121953)

OpenClaw 给 cron 消息加的 `[cron:<jobId> <name>]` 前缀导致 DeepSeek API 边缘节点将请求降级到低优先级队列，任务卡顿数十秒至数分钟。这暴露了「为兼容 provider 而做的临时 hack 最终伤害了用户实际使用」的典型问题，社区希望找到更优雅的前缀设计或 provider 侧规避方案。

### 其他高热度话题
- [Issue #96834](https://github.com/openclaw/openclaw/issues/96834)：WhatsApp 图片消息楔住主队列约 3 分钟，multimodal 处理与 active_reply 状态机冲突（15 条评论）。
- [Issue #62505](https://github.com/openclaw/openclaw/issues/62505)：Coding Agent 完全无法完成任务，2026.4.2 前正常，典型的 regression（15 条评论）。

---

## 5. Bug 与稳定性

### P0（严重，需立即关注）

| Issue | 描述 | 创建时间 | Fix PR |
|---|---|---|---|
| [#91588](https://github.com/openclaw/openclaw/issues/91588) | Gateway 内存泄漏 RSS 350MB→15.5GB，OOM 反复重启 | 2026-06-09 | ❌ 无 |
| [#119270](https://github.com/openclaw/openclaw/issues/119270) | file 工具剥离目标路径开头的 `@`，导致写/删错文件（数据丢失） | 2026-08-04 | ❌ 无 |
| [#108435](https://github.com/openclaw/openclaw/issues/108435) | 升级 2026.7.1 后 gateway 无法启动（systemd/ollama/手动均失败） | 2026-07-15 | ❌ 无 |

### P1（高影响，数量庞大，选取代表）

| Issue | 描述 | 创建时间 | Fix PR |
|---|---|---|---|
| [#62505](https://github.com/openclaw/openclaw/issues/62505) | Coding Agent 完全不动，仅输出含糊状态（回归） | 2026-04-07 | ❌ 无 |
| [#121953](https://github.com/openclaw/openclaw/issues/121953) | DeepSeek cron 前缀导致请求被降级，stall 数十秒 | 2026-08-11 | ✅ [#121299](https://github.com/openclaw/openclaw/pull/121299)（相关，非直接） |
| [#96834](https://github.com/openclaw/openclaw/issues/96834) | WhatsApp 图片楔住主队列 ~3 分钟 | 2026-06-25 | ❌ 无 |
| [#87109](https://github.com/openclaw/openclaw/issues/87109) | macOS 空闲堆内存膨胀至 1GB+，cron 静默失败 | 2026-05-27 | ❌ 无 |
| [#86215](https://github.com/openclaw/openclaw/issues/86215) | Codex OAuth 刷新失败可楔住 agent 数小时，无告警 | 2026-05-24 | ❌ 无 |
| [#107244](https://github.com/openclaw/openclaw/issues/107244) | WhatsApp 群消息完全不进入 inbound 处理（LID 群），DM 正常 | 2026-07-14 | ❌ 无 |
| [#92186](https://github.com/openclaw/openclaw/issues/92186) | 自动可见回复模式下，较早完成的消息回复永远不会投递到 WhatsApp | 2026-06-11 | ❌ 无 |

### 稳定性观察

- 大量 P0/P1 issue 已存活 1–2 个月，多数标记 `clawsweeper:no-new-fix-pr`（长期无新修复 PR），修复周期较长。
- 8 月新开 issue 开始出现上周回归类报告（如 #121058、#121083），说明近期改动可能引入了新问题。
- `clawsweeper-recovery-stuck` 标签密集出现（#119270、#96834、#99910、#98435 等），session 恢复卡死是高频故障模式。

---

## 6. 功能请求与路线图信号

### 高讨论度功能请求

| Issue | 请求 | 评论/👍 | 状态 |
|---|---|---|---|
| [#10687](https://github.com/openclaw/openclaw/issues/10687) | 全动态模型发现（OpenRouter + 多 provider） | 10 评论 / 3 👍 | needs-product-decision |
| [#13219](https://github.com/openclaw/openclaw/issues/13219) | 按模型的 usage 日志，用于成本追踪 | 8 评论 / 1 👍 | needs-product-decision |
| [#68920](https://github.com/openclaw/openclaw/issues/68920) | HTTP endpoints 增加 lightContext/voice 模式，降低 TTFB | 6 评论 / 1 👍 | needs-product-decision |
| [#71142](https://github.com/openclaw/openclaw/issues/71142) | Control UI 上传大小限制可配置（现硬编码 5MB） | 8 评论 | needs-product-decision |
| [#88154](https://github.com/openclaw/openclaw/issues/88154) | Slack Modal 原生交互支持 | 7 评论 / 1 👍 | needs-product-decision |
| [#50093](https://github.com/openclaw/openclaw/issues/50093) | WhatsApp 重连后补发断线期间丢失的消息 | 13 评论 / 1 👍 | needs-product-decision |
| [#17840](https://github.com/openclaw/openclaw/issues/17840) | opt-in 反应（emoji reaction）触发 agent 轮次 | 6 评论 | needs-product-decision |

### 路线图信号判断

- 多数功能请求长期停在 `needs-product-decision`，说明维护者尚未将这些纳入明确路线图。结合 0 release 和大量 P0/P1 bug 积压，**下一版本的优先级大概率仍是稳定性修复而非新功能**。
- 两个值得注意的「低成本高收益」请求：**#68920**（HTTP TTFB 优化）与 **#71142**（上传限制可配置），均有明确使用场景，如果希望提升企业/开发者口碑，可考虑安排在稳定性修复之后。
- PR 侧已有 [#121135](https://github.com/openclaw/openclaw/pull/121135)（docs: SecretRef default alias 说明）和 [#121073](https://github.com/openclaw/openclaw/pull/121073)（memory_search 超时误报修复），说明维护者在补文档与边界情况，属于「收敛性迭代」。

---

## 7. 用户反馈摘要

### 高频痛点

1. **升级即回归**：多名用户报告从 2026.6.x/2026.7.x 升级后出现功能损坏，包括 Coding Agent 罢工（#62505）、gateway 无法启动（#108435）、memory embedding provider 报错（#90786）、会话存储迁移后为空（#94939）。用户常用「worked before, now fails」描述，**回归测试缺口明显**。
2. **静默失败最伤信任**：cron 任务在内存压力下静默失败（#87109）、WhatsApp 群消息永远不达（#107244）、回复在 dashboard 显示但用户收不到（#92186）——这类「看起来正常但实际不可用」的问题比显式报错更容易引发负面情绪。
3. **内存问题长期未解**：#91588 和 #87109 都提供了非常完整的复现数据和日志，但修复迟迟未到，用户投入大量时间配合排查却得不到闭环反馈。

### 正面信号

- [#73537](https://github.com/openclaw/openclaw/issues/73537) 中用户明确表达了对项目的感谢，称 OpenClaw 已成为家庭和业务助手日常工作流的一部分，并建设性地请求「生产就绪稳定性标签」——说明有一批真实用户将 OpenClaw 用于关键场景，这是项目最宝贵的资产。
- 多个 PR（如 #123871、#121047、#121073）注明「AI-assisted contribution prepared with Codex/Claude」，说明 AI 辅助贡献的协作模式已跑通，外部贡献门槛在降低。

---

## 8. 待处理积压

以下问题长期未关闭或未获得维护者实质响应，建议优先处理：

| 项目 | 类型 | 创建时间 | 滞留时间 | 严重度 | 链接 |
|---|---|---|---|---|---|
| #91588 Gateway 内存泄漏 | P0 bug | 2026-06-09 | 67 天 | 🔴 严重 | [链接](https://github.com/openclaw/openclaw/issues/91588) |
| #62505 Coding Agent 回归 | P1 bug | 2026-04-07 | 130 天 | 🟠 高 | [链接](https://github.com/openclaw/openclaw/issues/62505) |
| #121058 Silent reply 复发 | P1 bug | 2026-08-09 | 6 天（关联 #116277 更早） | 🟠 高 | [链接](https://github.com/openclaw/openclaw/issues/121058) |
| #50093 WhatsApp 断线补发 | P2 feature | 2026-03-19 | 149 天 | 🟡 中 | [链接](https://github.com/openclaw/openclaw/issues/50093) |
| #10687 动态模型发现 | P2 feature | 2026-02-06 | 190 天 | 🟡 中 | [链接](https://github.com/openclaw/openclaw/issues/10687) |
| PR #105025 Twilio RCS 通道 | P3 PR | 2026-07-12 | 34 天 | 🟡 中 | [链接](https://github.com/openclaw/openclaw/pull/105025) |
| PR #112811 MS Teams 多 bot 支持 | P2 PR | 2026-07-23 | 23 天 | 🟡 中 | [链接](https://github.com/openclaw/openclaw/pull/112811) |

**特别提醒**：

- #121058 虽然创建仅 6 天，但已积累 94 条评论，且涉及「关闭后复发」的流程信任问题，建议维护者公开回应根因分析与关闭标准，否则类似 issue 会持续以更高频次重开。
- 两个 P0 级 issue（#91588、#119270）均无 fix PR，其中后者（文件工具误删写）涉及数据丢失，需最高优先级处理。
- PR #121204（Discord 恢复后旧消息饿死实时 mention）虽是 #97435 的修复，但类似的「恢复后状态不一致」模式在 WhatsApp（#50093、#107244）、MCP（#98435）等多个通道反复出现，建议做一个跨通道的恢复流程专项治理。

---

**总结**：OpenClaw 项目社区活跃度高、贡献者参与积极，但当前处于「bug 报告速度 > 修复推出速度」的被动阶段。0 release 意味着所有修复尚未对普通用户生效，而 P0 级内存泄漏和数据丢失问题已滞留超过两个月。建议下一阶段优先收敛 P0/P1 修复并发布 patch 版本，恢复社区信心；功能请求可暂缓排期，优先处理回归测试与静默失败类问题。

*本日报由 AI 分析师生成，基于 2026-08-15 从 GitHub 拉取的数据。所有链接指向 openclaw/openclaw 仓库对应 issue/PR。*

---

## 横向生态对比

# 个人 AI 智能体开源生态横向对比分析报告

**报告日期**: 2026-08-15 | **数据窗口**: 2026-08-14 至 2026-08-15 | **覆盖项目**: 12 个

---

## 1. 生态全景

当前个人 AI 助手/自主智能体开源生态整体处于「**规模分化、质量分层**」阶段：以 OpenClaw 为核心的头部项目日活极高（24h 内 1000 条 Issue/PR 更新），但正处于「bug 报告速度 > 修复交付速度」的被动消化期，0 release 意味着大量修复尚未触达用户；与此同时，IronClaw、LobsterAI 等具备商业或企业背景的项目通过规范的 RC 发布线（IronClaw 1.2.0）和日级迭代（LobsterAI 2026.8.14）展示了更健康的交付节奏。跨项目共性问题高度集中：**多渠道投递可靠性、会话状态一致性、MCP 集成健壮性与 Windows/macOS 桌面体验**是普遍短板，而「升级即回归」「静默失败」是消耗社区信任的最主要因素。值得注意的积极信号是，AI 辅助贡献（Codex/Claude 标注的 PR）已常态化，外部首次贡献者开始提交跨仓库大型 PR，生态的协作门槛正在降低。

---

## 2. 各项目活跃度对比

| 项目 | Issue 动态 | PR 动态 | Release | 健康度评估 |
|---|---|---|---|---|
| **OpenClaw** | 488 新开/活跃，12 关闭 | 97 合并/关闭，403 待处理 | 无 | 🔴 偏紧：bug 积压严重，P0 滞留 67 天，0 release |
| **NanoBot** | 3 更新 | 6 合并，14 待合并 | 无 | 🟢 良好：修复闭环快，但 8 个 PR 冲突需整合 |
| **Hermes Agent** | 46 新开，4 关闭 | 4 合并，46 待合并 | 无 | 🟡 良好偏热：架构重构收尾，Windows/macOS 问题集中 |
| **PicoClaw** | 3 更新 | 0 合并，5 条 stale 关闭 | 无 | 🟡 中等：修复 PR 待合并，stale 机制恐误伤有效 PR |
| **NanoClaw** | 2 新开 | 3 关闭/合并，6 待合并 | 无 | 🟢 良好：CI/签名基础设施验证扎实，社区响应及时 |
| **NullClaw** | 0 | 1 合并 | 无 | 🟢 平稳：低频但无积压，backlog 健康 |
| **IronClaw** | 9 关闭 | 22 合并/关闭 | ✅ 1.2.0 | 🟢 优秀：发布线规范，吞吐量历史高位，XL PR 略多 |
| **LobsterAI** | 2 更新 | 22 合并/关闭 | ✅ 2026.8.14 | 🟢 优秀：日级迭代，大型功能合入（67 commit） |
| **Moltis** | 0 | 1 待合并 | 无 | 🟡 早期：功能储备阶段，社区互动弱 |
| **CoPaw (QwenPaw)** | 38 关闭（多为清理） | 15 合并/关闭 | 无 | 🟡 高迭代+回归压力：2.1.0 存在多会话串扰等正确性 Bug |
| **ZeptoClaw** | 0 | 0 | 无 | ⚪ 休眠 |
| **ZeroClaw** | 33 更新 | 50 更新 | 无 | 🟡 高活跃：安全 RFC 密集，决策队列积压明显 |

---

## 3. OpenClaw 在生态中的定位

**生态位：参照系与流量入口。** OpenClaw 当日 Issue+PR 更新量达 1000 条，是第二梯队（Hermes/ZeroClaw/CoPaw，各约 80-100 条）的 10 倍以上；其衍生/对标项目命名（PicoClaw、NanoClaw、NullClaw、IronClaw、ZeptoClaw、ZeroClaw、CoPaw）本身即说明其「类目定义者」地位。LobsterAI 直接修复 OpenClaw 技能管理缺陷（#2491/#2483），进一步印证 OpenClaw 是事实上的生态基座。

**技术路线差异：** OpenClaw 采用「Gateway 统一入口 + 多通道适配 + session/beam 语义」的架构，强调 prepared-model 运行时快照、config 热重载一致性与跨通道恢复流程——这是典型的**高复杂度单体型架构**，功能覆盖面最大，但相应的状态管理复杂度也最高（P0 内存泄漏、session 恢复卡死均源于此）。相比之下，IronClaw 走「发布列车 + 金丝雀测试 + 契约测试」的企业级工程路线，LobsterAI 走「商业产品 + 高频发版」路线，NanoBot 走「轻量 WebUI-first」路线。

**核心优势：** 真实用户规模与场景广度——社区反馈表明 OpenClaw 已被用于家庭与业务助手的关键工作流（#73537），且用户愿意提交完整复现路径（#91588 内存泄漏附全量环境数据）。这种「被真实世界锤打」的规模效应是其他项目无法复制的资产。

**核心风险：** 修复管线吞吐不足（403 个 PR 待处理，P0 文件误删/写 Bug 无 fix PR）、「issue 关闭但问题复发」正在消耗信任（#121058 获 94 条评论）。生态参照系的地位依赖社区信心，若不尽快发布 patch 版本收敛 P0/P1，用户可能向 IronClaw、LobsterAI 等交付更快的项目迁移。

---

## 4. 共同关注的技术方向

| 技术方向 | 涉及项目 | 具体诉求 |
|---|---|---|
| **多渠道消息可靠性** | OpenClaw、IronClaw、Hermes、CoPaw、ZeroClaw | WhatsApp 图片楔住主队列（OpenClaw #96834）、Telegram MP4 上传失败（IronClaw #7662）、Discord v10 API 对齐（Hermes #79564）、群消息不进入 inbound（OpenClaw #107244）、飞书会话被串扰取消（CoPaw #7011） |
| **会话状态一致性与恢复** | OpenClaw、NanoBot、CoPaw、IronClaw | 后台任务覆盖会话数据（NanoBot #5271，p0）、恢复后旧消息饿死实时 mention（OpenClaw #121204）、scroll 压缩导致 UI transcript 丢失（CoPaw #6951）、线程索引投影修复（IronClaw 1.2.0） |
| **MCP 集成健壮性** | PicoClaw、CoPaw、NanoBot、OpenClaw | MCP server 失败挂起整个 agent 循环（PicoClaw #3269）、FastMCP 结果重复写入（CoPaw #6958）、升级后 Tool not found（CoPaw #6405）、MCP SDK v2 迁移（NanoBot #5179）、大量 `clawsweeper-recovery-stuck` 标签（OpenClaw） |
| **Provider/模型适配层** | OpenClaw、Hermes、CoPaw、ZeroClaw、NanoBot | DeepSeek cron 前缀致降级（OpenClaw #121953）、Xiaomi MiMo 工具不暴露（Hermes #86403）、GLM-5.3 快速接入（Hermes #86433）、MiniMax 不支持 /models 端点（CoPaw #2303）、`finish_reason:"length"` 误判为成功（ZeroClaw #9421）、Anthropic 流式超时误用（NanoBot #5391） |
| **内存/资源治理** | OpenClaw | Gateway RSS 350MB→15.5GB OOM（#91588，67 天）、macOS 空闲堆膨胀 1GB+（#87109）——目前仅 OpenClaw 暴露此规模压力，但任何项目规模增长后都会遇到 |
| **桌面/终端体验与跨平台** | Hermes、NanoBot、CoPaw、ZeroClaw、NanoClaw | Windows 启动无重试（Hermes #73722）、RDP 缩放丢失（Hermes #84274）、Windows os.replace 访问拒绝（NanoBot #5382）、Windows 74 项测试失败（ZeroClaw #7462）、自动更新缺失（CoPaw #2846）、AVX2 不兼容 SIGILL（NanoClaw #3245） |
| **确定性自动化执行** | IronClaw、ZeroClaw、OpenClaw | 无人值守运行结果不可预测（IronClaw #6879，v1.3.0 epic）、Goal mode 有界目标执行（ZeroClaw #8303）、cron 静默失败（OpenClaw #87109） |
| **安全与授权管控** | ZeroClaw、NanoClaw、IronClaw、Hermes | 高风险 shell 命令确认（ZeroClaw #7155）、插件出口策略（ZeroClaw #9137）、签名审批器实弹验证（NanoClaw #3242/3244）、OAuth 诊断透传（IronClaw #7668）、浏览器子进程环境净化（Hermes #86371） |

---

## 5. 差异化定位分析

| 项目 | 功能侧重 | 目标用户 | 关键架构特征 |
|---|---|---|---|
| **OpenClaw** | 全功能个人 AI 中枢：多通道、coding session、beam 会话、技能/插件 | 个人重度用户、自托管家庭/业务助手 | Gateway 中心化，session/beam 状态机，prepared-model 快照 |
| **NanoBot** | 轻量对话助手 + 技能系统 + WebUI 体验打磨 | 开发者、追求开箱即用者 | Python 异步栈，WebUI 重度迭代（拖放会话/本地化/粒子背景） |
| **Hermes Agent** | TUI/Desktop 优先 + 深度平台厂商适配（Discord 全覆盖）+ 模型快速跟进 | 高端开发者、Discord 社区运营者 | god-file 分片架构，模块零侵入扩展，per-profile MCP |
| **PicoClaw** | 轻量自托管，多渠道基础能力 | 个人开发者、资源受限环境 | Go 栈（goroutine），MCP 集成，但代码质量门禁偏弱 |
| **NanoClaw** | 安装/供应链安全：预构建镜像、签名审批、CI 可靠性 | 容器化部署用户、安全敏感环境 | 供应链验证管线为核心竞争力 |
| **NullClaw** | 最小化部署：只读文件系统、路径可配置 | 容器/不可变基础设施用户 | SQLite 解耦工作区，部署灵活性优先 |
| **IronClaw** | 企业级自动化可靠性：scheduled 任务确定性、MCP OAuth、DB 压力基线 | 企业客户、营销/运营团队 | 发布列车（RC/金丝雀），epic 驱动（1.3.0），结构化 ask/reply |
| **LobsterAI** | 商业产品化：Team Edition、账户配额、多智能体 cowork UI | 团队用户、商业客户 | 日级发版，OpenClaw 生态兼容，商业化元素（广告/签到） |
| **Moltis** | 本地优先个人数据整合：日历/邮件/频道历史 | 隐私敏感用户 | 提供商无关持久化连接器，原子快照，不复制凭证 |
| **CoPaw (QwenPaw)** | 中文生态整合：QQ/OneBot、插件体系、桌面端 | 中文用户、AgentScope 生态开发者 | 技能动态化重构中，插件可注册 HTTP 路由 |
| **ZeroClaw** | 安全与协议合规：OpenAI 兼容端点、可插拔认证、命令策略 | 安全敏感组织、Open WebUI 生态用户 | RFC 驱动，安全预算原子化，egress 策略统一 |
| **ZeptoClaw** | — | — | 休眠，无动态 |

---

## 6. 社区热度与成熟度分层

**第一梯队（日更新 80+ 条）——大规模迭代/承压期：**
- **OpenClaw**：社区规模断层第一，但处于「被动消化 bug」阶段。严格的质量门禁（proof 标签、merge-risk 标注）是双刃剑——保证合入质量，却导致 403 个 PR 积压、0 release。
- **Hermes**：架构重构（god-file 分片）与功能战役（Discord parity）双轨推进，工程纪律良好，但平台兼容性（Windows/macOS）拖累整体质量评价。
- **ZeroClaw**：安全敏感型项目，RFC 文化浓厚，讨论质量高，但大量 5-7 月创建的 RFC 待决策，存在设计漂移风险。
- **CoPaw**：迭代速度快（15 个 PR 合入），但 2.1.0 回归类 Bug（会话串扰、MCP 重复写入、工具 404）集中，处于「高迭代 + 修回归」双轨。

**第二梯队（日更新 10-50 条）——质量巩固/商业加速期：**
- **IronClaw**：最成熟。1.2.0 发布线与 main 合并流程完整，QA bug bash 制度化，当前处于「前版本收尾 + 下一版本核心特性发力」的良性过渡期。XL PR 占比高是唯一隐患。
- **LobsterAI**：交付节奏最快（日级发版），功能合入力度大（Team Edition 67 commit），但商业元素（侧边栏广告）与用户体验的平衡是潜在社区摩擦点。
- **NanoBot**：典型的产品打磨期，WebUI 交互细节持续迭代，修复闭环快（当日 Bug 当日合入），8 个 PR 冲突反映并行开发热度。

**第三梯队（日更新 <10 条）——早期/工具化/休眠期：**
- **PicoClaw**、**NanoClaw**：规模较小但工程动作扎实（NanoClaw 的供应链安全验证值得关注）；PicoClaw 的 stale 自动关闭机制可能误伤有价值的 PR（#3279 实质修复被关）。
- **NullClaw**、**Moltis**：低频维护，前者无积压健康度高，后者处于功能储备阶段（持久化连接器）。
- **ZeptoClaw**：休眠。

---

## 7. 值得关注的趋势信号

1. **「静默失败」是信任的第一杀手**：OpenClaw #121058（94 评论）、#92186、#107244 与 ZeroClaw #9486（钱包地址被脱敏替换）等问题共同指向：**用户能容忍显式崩溃，不能容忍"看似正常实则不可用"**。智能体开发者应将「可观测的失败」作为一等项目，而非仅关注功能正确性。

2. **Issue 关闭标准的透明化成为社区治理议题**：OpenClaw「关闭后复发」引发信任危机，社区开始要求根因分析 + 验证闭环的公开回应。建议各项目引入「关闭条件模板」（复现路径、fix PR 链接、验证结果），避免 clawbot 式自动关闭进一步消耗信任。

3. **确定性自动化是下一轮竞争的制高点**：IronClaw 将「相同提示词无人值守结果不稳定」定义为结构性缺陷并投入整个 v1.3.0 epic；ZeroClaw 推 Goal mode；OpenClaw 用户受 cron 静默失败困扰。**「可预测的自动执行」正从"加分项"变为"准入门槛"**——这对所有做 scheduled task / background agent 的开发者是明确的信号。

4. **模型多样性倒逼 Provider 抽象层升级**：GLM-5.3、Xiaomi MiMo、DeepSeek、Anthropic 在同一天内出现在 6 个项目的 Bug/PR 中，且问题各有不同（工具协商缺失、前缀降级、超时语义误用、端点不兼容）。**「一次适配、处处可用」的幻想已破灭**，按 provider 语义化能力协商（tools/vision/context）将成为基础设施。OpenAI 兼容层（ZeroClaw #8603）是破局方向之一。

5. **Windows/桌面端是生态共同短板，也是增量市场**：ZeroClaw 74 项 Windows 测试失败、Hermes Windows 启动无重试、NanoBot Windows 文件锁崩溃、CoPaw 自动更新缺失——**四个活跃项目在同一天暴露 Windows 问题**，说明主流生态仍以 Linux/macOS 为默认，但用户基数正在向 Windows 迁移。优先补 Windows CI 覆盖的项目将获得差异化优势。

6. **供应链安全从"最佳实践"走向"标配"**：NanoClaw 完成签名审批器第二轮实弹测试、IronClaw 发布线包含容器镜像 curl 修复、ZeroClaw 建立插件 egree 策略基础——**AI 智能体的分发（预构建镜像、插件市场、agent 导出）正在复制软件供应链的安全成熟度曲线**。自托管 agent 的「可移植导出」需求（ZeroClaw #9986）与「本地优先数据自主」（Moltis）同源，指向用户对「agent 资产所有权」的重视。

7. **AI 辅助贡献常态化，外部贡献门槛显著降低**：OpenClaw 多个 PR 标注「AI-assisted contribution prepared with Codex/Claude」，CoPaw 收到首次贡献者的大型跨仓库 PR（#6940），NanoClaw 用户在提交 Issue 当天附上完整修复 PR（#3248/#3249）。**维护者需要建立面向 AI 辅助贡献的 review 规范**（AI 生成代码的审计清单、语义化提交要求），而非简单拒绝或全盘接受。

---

*本报告基于 2026-08-15 当日各项目 GitHub 动态生成，数据源及链接见各项目日报原文。*

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报 — 2026-08-15

## 1. 今日速览

过去 24 小时 NanoBot 项目整体活跃度较高：共发生 3 条 Issue 更新、22 条 PR 更新，其中 6 个 PR 已合并/关闭，14 个 PR 待合并；无新版本发布。今日合并的 PR 集中在 WebUI 体验打磨、Anthropic 流式超时 Bug 修复和技能系统增强上，同时有 2 个高价值 Bug（Anthropic 流式超时、文件归档异常）已闭环，项目健康度良好，迭代节奏稳健。值得注意的是，当前有 8 个 PR 被标记为冲突（conflict），并行开发的整合压力正在显现。

## 2. 版本发布

无新版本发布。

## 3. 项目进展

今日共 6 个 PR 被合并/关闭，主要推动了三个方向的进展：

### 3.1 稳定性修复（Anthropic 流式超时）
- **[PR #5392](https://github.com/HKUDS/nanobot/pull/5392)（已合并）** 修复 `NANOBOT_STREAM_IDLE_TIMEOUT_S` 被错误当作总超时应用的问题。此前该变量在 Anthropic 无回调流路径中被 `asyncio.wait_for` 用于限制 `get_final_message()` 的等待时间，导致长时但活跃的生成被意外终止；现改为仅作为“空闲不活跃”超时。对应 Issue #5391 已关闭。

### 3.2 WebUI 持续打磨
- **[PR #5393](https://github.com/HKUDS/nanobot/pull/5393)（已合并）** 从协作功能中拆分出纯 UI 改进：侧边栏层级结构更清晰、连接线优化、选项卡面板扁平化、文件夹展示改进，以及更平滑的会话过渡动画。
- **[PR #5395](https://github.com/HKUDS/nanobot/pull/5395)（已合并）** 完善会话分组交互：统一分组术语、全流程本地化、支持将活动会话拖入/拖出分组、简化删除确认样式，并引入共享形状规格统一 WebUI 控件观感。

### 3.3 技能与提供商功能增强
- **[PR #5018](https://github.com/HKUDS/nanobot/pull/5018)（已合并）** 修复 `skill_names` 输入被忽略的问题，现在支持显式指定技能进行上下文加载，使直接调用者可以预加载所需技能。
- **[PR #4689](https://github.com/HKUDS/nanobot/pull/4689)（已关闭）** 为 OAuth 提供商添加状态可见性和 Token 过期预警，覆盖 CLI、WebUI 和运行时会话。
- **[PR #5390](https://github.com/HKUDS/nanobot/pull/5390)（已关闭）** 标题为 “Agent/knowledge graph”，摘要为空，推测为实验性或未完整提交的 PR。

整体上，项目正在通过“修复+打磨+新功能”并行的策略稳步演进，尤其是 WebUI 的体验迭代频率显著加快，说明前端体验是当前重点投入方向之一。

## 4. 社区热点

> 注：本次数据中 PR 的评论数未提供（undefined），以下基于更新频率、标记状态和关联度判断。

### 4.1 Issue #5161——Pyright 严格检查重构引发连锁关注
- **[Issue #5161](https://github.com/HKUDS/nanobot/issues/5161)（开放，评论 1）** 提议收窄文件级 Pyright suppression，提升代码类型安全。该 Issue 已有一个专门 PR（#[5396](https://github.com/HKUDS/nanobot/pull/5396)）承接修复，并涉及 8 个工具模块的改动。这是一个典型的“质量基建”话题，虽然没有热烈讨论，但对项目长期健康度有重要意义。

### 4.2 多个 WebUI 功能 PR 进入冲突状态，并行开发热度高
- **[PR #5356](https://github.com/HKUDS/nanobot/pull/5356)（开放，conflict）** 改进设置流程。
- **[PR #5389](https://github.com/HKUDS/nanobot/pull/5389)（开放，conflict）** 拖放式会话组织。
- **[PR #5371](https://github.com/HKUDS/nanobot/pull/5371)（开放，conflict）** 隐藏助手操作直到回合结束。
- **[PR #5358](https://github.com/HKUDS/nanobot/pull/5358)（开放，conflict）** 会话协作 @mention。
- **[PR #5340](https://github.com/HKUDS/nanobot/pull/5340)（开放，conflict）** 粒子英雄背景。

大量 WebUI 相关 PR 在同一时期并行开发并产生合并冲突，说明社区对 WebUI 改进的热情很高。这些 PR 多来自不同贡献者（chengyongru、ZhouJ-sh、bingqilinweimaotai 等），需求集中在：界面交互丰富度、用户体验细节、以及更高效的会话管理。背后诉求是让 NanoBot 的 Web 界面达到接近商业产品的成熟度。

## 5. Bug 与稳定性

今日 2 个新 Bug 被报告并修复，1 个早期 Bug 的修复 PR 仍待合并。

| 严重程度 | Issue | 状态 | 修复 PR |
|---|---|---|---|
| 高 | **[#5391](https://github.com/HKUDS/nanobot/issues/5391)**：流式空闲超时被误用为总超时，导致 Anthropic 长时生成被错误终止 | 已关闭 | **[PR #5392](https://github.com/HKUDS/nanobot/pull/5392)** 已合并 |
| 高 | **[#5378](https://github.com/HKUDS/nanobot/issues/5378)**：文件容量归档失败时先修改会话再持久化，导致内存态与持久态不一致 | 已关闭 | 暂无直接对应 PR，需观察会话管理的后续修复 |
| 严重（p0） | **[PR #5271](https://github.com/HKUDS/nanobot/pull/5271)（开放，p0）**：修复过期的后台任务保存覆盖 `/new` 后的会话数据 | 待合并 | 该 PR 本身即修复，已标记 p0 优先级 |
| 中 | **[PR #5382](https://github.com/HKUDS/nanobot/pull/5382)（开放）**：修复 Windows 下 `os.replace()` 瞬时访问拒绝导致网关崩溃 | 待合并 | 该 PR 本身即修复（含重试逻辑） |

其中 [#5271](https://github.com/HKUDS/nanobot/pull/5271) 的 p0 优先级表明，会话数据一致性问题是当前稳定性领域的核心关切。Windows 平台上的文件锁问题（#5382）也提示跨平台兼容性测试可能需要加强。

## 6. 功能请求与路线图信号

今日无新增功能请求 Issue，但大量开放中的 PR 提供了明确的路线图信号：

- **WebUI 体验升级**：本地化 Agent 活动（[#5367](https://github.com/HKUDS/nanobot/pull/5367)）、拖放会话管理（[#5389](https://github.com/HKUDS/nanobot/pull/5389)）、设置流程改进（[#5356](https://github.com/HKUDS/nanobot/pull/5356)）、交互式粒子背景（[#5340](https://github.com/HKUDS/nanobot/pull/5340)）等，均有可能被纳入下一版本。
- **技能系统增强**：[#5309](https://github.com/HKUDS/nanobot/pull/5309) 允许市场技能覆盖内置技能，补齐了技能加载的一个关键缺口。
- **基础设施升级**：[#5179](https://github.com/HKUDS/nanobot/pull/5179) 将 MCP 集成迁移至 SDK v2，并保留 SSRF 验证等安全特性，属于必要技术债偿还，预计也会在后续版本落地。
- **全新终端产品方向**：[#4329](https://github.com/HKUDS/nanobot/pull/4329) 以 TypeScript/OpenTUI 重建 `nanobot agent` 终端，目前仍开放，若合并将大幅提升终端用户体验。

综合来看，WebUI 是当前功能需求最密集的领域，技能系统的生态建设也在稳步推进。

## 7. 用户反馈摘要

- **Anthropic 流式超时问题（Issue #5391）**：用户明确描述了“长时但活跃的生成”被 90 秒默认超时错误终止的现象，并定位到 `anthropic_provider.py:842-845` 的具体代码行。这说明用户对代码库有一定理解，同时反馈质量高，直接促成了快速修复。
- **文件归档失败导致会话状态丢失（Issue #5378）**：用户发现 `enforce_file_cap()` 在归档回调抛错时会污染内存会话，使后续保存无法恢复溢出前状态。这是一个隐蔽的状态一致性问题，需要更严谨的会话管理逻辑来避免。
- **Windows 平台会话保存崩溃（PR #5382）**：贡献者在日志中确认了两次 `os.replace()` 的 `[WinError 5] Access is denied` 错误，并提供了修复方案，反映出社区对 Windows 平台使用体验的关注在增加。

这些反馈共同指向：用户对 Nanobot 的稳定性和数据一致性有较高期待，同时积极的 Issue 报告和 PR 提交也体现了社区参与度在提升。

## 8. 待处理积压

以下 PR/Issue 开放时间较长或优先级较高，建议维护者关注：

| 类型 | 编号 | 说明 | 开放时长/优先级 |
|---|---|---|---|
| PR | **[#4145](https://github.com/HKUDS/nanobot/pull/4145)** | Weather Skill 修复，解决 #3958 | 已开放 75 天，包含新技能和测试，等待时间较长 |
| PR | **[#4329](https://github.com/HKUDS/nanobot/pull/4329)** | 原生 TypeScript 终端 UI，重大增强 | 已开放 63 天，涉及架构改动，需要仔细评审 |
| PR | **[#5152](https://github.com/HKUDS/nanobot/pull/5152)** | 修复子代理部分完成结果标记问题，含回归测试 | 已开放 18 天 |
| PR | **[#5179](https://github.com/HKUDS/nanobot/pull/5179)** | MCP SDK v2 迁移，涉及传输层和安全特性保留 | 已开放 16 天，优先级 p1 |
| Issue | **[#5161](https://github.com/HKUDS/nanobot/issues/5161)** | 收窄 Pyright suppression，代码质量改进 | 已开放 17 天，已有 PR #5396，建议加快评审 |
| PR | **[#5271](https://github.com/HKUDS/nanobot/pull/5271)** | 会话数据被后台任务覆盖，p0 优先级 | 已开放 9 天，优先级最高，应优先处理 |
| PR | **[#5396](https://github.com/HKUDS/nanobot/pull/5396)** | 收窄 Pyright suppression 的具体实现 | 创建当天即更新，建议尽快与 Issue #5161 联动 |

其中 [#5271](https://github.com/HKUDS/nanobot/pull/5271)（p0 会话数据一致性）和多个带 `conflict` 标记的 WebUI PR 是当前最需要维护者介入协调的事项。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目动态日报 — 2026-08-15

## 1. 今日速览

过去 24 小时项目活跃度处于高位：50 条 Issue 更新（46 新开/活跃、4 关闭）与 50 条 PR 更新（46 待合并、4 合并/关闭）齐头并进，无新版本发布。核心事件是架构级史诗 [#78647](https://github.com/NousResearch/hermes-agent/issues/78647)「All Gods Must Die」正式关闭，20 个 god-file 分片全部完成；同时 andrexibiza 发起的 Discord API v10 功能对齐战役（[#79564](https://github.com/NousResearch/hermes-agent/issues/79564) 元问题）批量涌入 15+ 个 Issue/PR 对，测试全绿、模块零侵入。Bug 侧，TUI/Dashboard 三个显示类问题（[#59591](https://github.com/NousResearch/hermes-agent/issues/59591)、[#66490](https://github.com/NousResearch/hermes-agent/issues/66490)、[#41480](https://github.com/NousResearch/hermes-agent/issues/41480)）确认关闭。整体呈「功能密集推进 + 平台稳定性修补」双轨态势，但 Windows/macOS 平台问题依然集中，是当前健康度的主要短板。

---

## 2. 版本发布

今日无新版本发布（最新 Releases 为空）。

---

## 3. 项目进展

今日合并/关闭的可见成果以 Bug 修复为主，另有大批高质量 PR 待合并：

- **god-file 分片史诗完结（架构里程碑）**：[#78647](https://github.com/NousResearch/hermes-agent/issues/78647) 关闭，20/20 god-file 全部完成分片，「只分片、不回退」成为仓库正式政策。
- **已关闭 Bug（对应修复落地）**：
  - [#59591](https://github.com/NousResearch/hermes-agent/issues/59591) Dashboard 恢复会话转录不完整，需主题切换强制刷新——渲染刷新机制修复。
  - [#66490](https://github.com/NousResearch/hermes-agent/issues/66490) TUI 在 Zellij 下 DEC 2026 同步输出导致帧重复。
  - [#41480](https://github.com/NousResearch/hermes-agent/issues/41480) iTerm2 流式输出时状态栏严重闪烁。
- **Discord Omniscience 战役批量就绪**（Part of [#79564](https://github.com/NousResearch/hermes-agent/issues/79564)，全部标注 Fixes 对应 Issue，测试全绿、新模块零侵入）：
  - M 系列：M1 消息投影（31/31）、M2 消息编辑/删除（22/22）、M3 反应、M5 投票投影 —— PR [#86449](https://github.com/NousResearch/hermes-agent/pull/86449)、[#86451](https://github.com/NousResearch/hermes-agent/pull/86451) 等。
  - T 系列：T1 线程生命周期（54/54）、T3 论坛帖子/标签（44/44）—— PR [#86454](https://github.com/NousResearch/hermes-agent/pull/86454)、[#86458](https://github.com/NousResearch/hermes-agent/pull/86458)。
  - A 系列：A1 频道 CRUD（32/32）、A2 权限覆盖、A3 角色（82/82）、A4 moderation、A5 guild 设置、A6 定时事件（25/25）—— PR [#86460](https://github.com/NousResearch/hermes-agent/pull/86460)、[#86462](https://github.com/NousResearch/hermes-agent/pull/86462)、[#86466](https://github.com/NousResearch/hermes-agent/pull/86466) 等。
  - R 系列：R1 限流契约（37/37）、R2 分页、R4 可靠性遥测 —— PR [#86468](https://github.com/NousResearch/hermes-agent/pull/86468) 等。
- **MCP 配置能力下沉到任意 profile**：[#86473](https://github.com/NousResearch/hermes-agent/pull/86473) 新增 per-profile `mcp.servers.*` 生命周期 RPC，Desktop 客户端（Bot 编辑器、Capabilities 页）可为非启动 profile 增删改查/测试/认证 MCP 服务器。
- **zai provider 跟进 GLM-5.3**：[#86433](https://github.com/NousResearch/hermes-agent/pull/86433) 复用 GLM-5.2 接线，保留 743B 基座与 1M 上下文，仅后训练差异。

---

## 4. 社区热点

- **[#78647](https://github.com/NousResearch/hermes-agent/issues/78647)（71 条评论，已关闭）**：今日绝对热点。「All Gods Must Die」史诗以 20/20 全灭收官，评论量碾压其他所有 Issue，社区围绕 god-file 分片与「扩展而非复制、设计统一接口」的工程原则讨论热烈，最终确立仓库长期重构政策。
- **[#79564](https://github.com/NousResearch/hermes-agent/issues/79564)（4 条评论，开放）**：Discord Feature Parity meta-issue，当前最大功能集群的枢纽，串联 15+ 子 Issue 与 PR，代表社区对 Discord 官方 API v10 / discord.py 2.7.1 对齐的强烈需求。
- **[#86403](https://github.com/NousResearch/hermes-agent/issues/86403)（Xiaomi MiMo v2.5 Pro 工具调用失效）**：评论虽少，但暴露第三方 provider 适配质量参差，涉及国产模型生态，信号价值高。

---

## 5. Bug 与稳定性

按严重程度排列：

**P2 高优**

- [#73722](https://github.com/NousResearch/hermes-agent/issues/73722) Desktop（Windows）启动遇瞬时 gateway 连接失败即 fatal：启动路径无重试（重连路径有），且瞬时 token 刷新失败被误报为「会话已过期」。P2 + Windows，建议优先。
- [#8751](https://github.com/NousResearch/hermes-agent/issues/8751)（4 月遗留）`agent/prompt_builder.py` 多处因无权限目录抛 `PermissionError`，P2 且已存活 4 个月，8 月 14 日仍有更新，社区持续关注。
- [#86411](https://github.com/NousResearch/hermes-agent/issues/86411) 显式 `terminal.cwd` 会在对话中途重新钉住工作目录，覆盖启动目录（#19214/#19242 确立的语义），属行为回归。
- [#86385](https://github.com/NousResearch/hermes-agent/issues/86385) macOS 更新后 Screen Recording 授权死循环：旧 cdhash 签名授权失效后开关仍显示 ON，用户无法重新授权，属签名策略迁移的信任链问题。

**P3 中优**

- [#86445](https://github.com/NousResearch/hermes-agent/issues/86445) Windows LSP 二进制探测误选 POSIX shim → WinError 193。**已有修复 PR [#86456](https://github.com/NousResearch/hermes-agent/pull/86456)**（Windows 下优先探测 `.cmd`/`.exe` 后缀）。
- [#84274](https://github.com/NousResearch/hermes-agent/issues/84274) Windows RDP 重连后 UI 缩放重置为 100%，`zoom-state.json` 未丢但渲染未重新应用。
- [#73495](https://github.com/NousResearch/hermes-agent/issues/73495) Desktop Cloud 冷启动可能隐藏全部 hosted agents，需 Portal 重新登录才能恢复。
- [#86403](https://github.com/NousResearch/hermes-agent/issues/86403) Xiaomi MiMo v2.5 Pro 下启用工具（17/26）不暴露给模型，核心工具（terminal/read_file/web_search 等）全部不可用。
- [#86452](https://github.com/NousResearch/hermes-agent/issues/86452) / [#86393](https://github.com/NousResearch/hermes-agent/issues/86393) 两条「duplicate」标记的噪音类问题：无 MCP 服务器时后台发现线程反复 WARNING；Kanban 运行时 `TERMINAL_CWD` 被误报为废弃 `.env` 设置，均影响 CLI 日常体验。

---

## 6. 功能请求与路线图信号

- **Discord REST v10 全覆盖**：Omniscience 战役按 Phase 2A/2B/2E/2G 分层推进，覆盖消息、线程、论坛、频道、角色、审核、限流、分页全表面。对应 PR 全部测试全绿且为新增模块，风险极低，几乎确定进入下一版本。
- **Per-profile MCP 管理**（[#86473](https://github.com/NousResearch/hermes-agent/pull/86473)）：当前仅 `mcp.catalog`（list）走 gateway，本次补齐全生命周期 RPC，指向「MCP 配置能力中心化、任意 profile 可管理」的路线，是 Desktop 多 profile 场景的刚需。
- **浏览器工具链加固**：[#86371](https://github.com/NousResearch/hermes-agent/pull/86371)（净化 browser-use 子进程 `PYTHONPATH`/`PYTHONHOME`）、[#86472](https://github.com/NousResearch/hermes-agent/pull/86472)（回收空闲多路复用会话）、[#86374](https://github.com/NousResearch/hermes-agent/pull/86374)（slash_worker PATH 补全）—— 浏览器/桌面派生进程的环境与资源管理是近期工具链重点。
- **新模型快速跟进**：[#86433](https://github.com/NousResearch/hermes-agent/pull/86433) GLM-5.3 复用既有接线，说明 zai provider 的模型扩展成本已很低，未来新模型接入将更频繁。

---

## 7. 用户反馈摘要

- **Windows 平台体验显著落后**：[#73722](https://github.com/NousResearch/hermes-agent/issues/73722)（启动无可重试路径）、[#84274](https://github.com/NousResearch/hermes-agent/issues/84274)（RDP 缩放丢失）、[#86445](https://github.com/NousResearch/hermes-agent/issues/86445)（LSP 二进制误选）均来自 Windows 用户，集中在启动健壮性、显示状态恢复与工具链路径解析三类问题上。
- **macOS 更新信任链断裂反噬**：[#86385](https://github.com/NousResearch/hermes-agent/issues/86385) 用户对「授权开关显示 ON 却无法重新授权」的死锁状态明显不满，这属于签名策略更新（#73681）带来的存量用户迁移成本。
- **噪音日志磨损信任**：无 MCP 配置的用户被 [#86452](https://github.com/NousResearch/hermes-agent/issues/86452) 的 WARNING 刷屏；Kanban 用户被 [#86393](https://github.com/NousResearch/hermes-agent/issues/86393) 的误导性废弃告警干扰——两者虽不影响功能，但频繁出现会降低 CLI 用户对日志质量的信任。
- **第三方模型适配期待更高**：Xiaomi MiMo 场景（[#86403](https://github.com/NousResearch/hermes-agent/issues/86403)）中「配置里已启用但会话中完全不出现工具」是典型的 provider 能力协商缺失，用户期望是开箱即用而非逐家调试。
- **正面反馈**：TUI 在 Zellij/iTerm2 的显示问题（[#66490](https://github.com/NousResearch/hermes-agent/issues/66490)、[#41480](https://github.com/NousResearch/hermes-agent/issues/41480)）确认修复；god-file 分片史诗获得社区高热度讨论，重构方向获得认可。

---

## 8. 待处理积压

- **[#8751](https://github.com/NousResearch/hermes-agent/issues/8751)**（2026-04-13 创建，P2）：父目录 `PermissionError` 崩溃，无对应 fix PR，已存活 4 个月且为权限类可复现问题，建议纳入下次迭代。
- **[#73722](https://github.com/NousResearch/hermes-agent/issues/73722)**（2026-07-29 创建，P2）：Desktop Windows 启动路径无重试，与 [#73495](https://github.com/NousResearch/hermes-agent/issues/73495)（Cloud 冷启动丢 agent）同属 Desktop 连接健壮性问题，建议合并修复。
- **[PR #68499](https://github.com/NousResearch/hermes-agent/pull/68499)**（2026-07-21 创建）：委派生命周期与任务结果分离，涉及 gateway/TUI/Desktop 三方渲染与 fail-closed 传播，改动面大（含 `sweeper:blast-broad`），已搁置近 4 周，需要维护者明确 review 结论。
- **[PR #77050](https://github.com/NousResearch/hermes-agent/pull/77050)**（2026-08-02 创建）：委派产物远程可读路径 + Docker 缓存挂载，属于委派链路的深水区修复，与 #68499、#83485 同属委派体系改进，建议合并评审避免 PR 堆积。

---

*数据来源：Hermes Agent GitHub 仓库（github.com/nousresearch/hermes-agent），统计窗口 2026-08-14 至 2026-08-15。*

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报 2026-08-15

## 1. 今日速览
过去 24 小时，PicoClaw 项目共更新 3 条 Issues 和 9 条 Pull Requests，无新版本发布。核心动态是 Issue [#3269](https://github.com/sipeed/picoclaw/issues/3269)（MCP 服务器连接失败导致 agent 循环挂起）受到关注，并已出现对应修复 PR [#3337](https://github.com/sipeed/picoclaw/pull/3337)。与此同时，大量标记为 `[stale]` 的旧 PR/Issue 被自动清理（5 条 PR、2 条 Issue 关闭），表明项目当前以稳定性修复和积压清理为主要节奏。整体活跃度中等，但多个月份久远的 PR 仍待合并，合并效率有待提升。

## 2. 版本发布
今日无新版本 Release。

## 3. 项目进展
今日没有明确的 PR 合并记录；5 条 PR 被关闭，其中多数为 `[stale]` 自动关闭（[#3303](https://github.com/sipeed/picoclaw/pull/3303)、[#3283](https://github.com/sipeed/picoclaw/pull/3283)、[#3279](https://github.com/sipeed/picoclaw/pull/3279)、[#3271](https://github.com/sipeed/picoclaw/pull/3271)、[#3270](https://github.com/sipeed/picoclaw/pull/3270)），并非功能合并。真正值得关注的是以下待合并 PR：

- [PR #3337](https://github.com/sipeed/picoclaw/pull/3337) — 修复 MCP 连接失败导致 agent 循环挂起的问题，直接对应 #3269，是今日最重要的稳定性修复。
- [PR #3319](https://github.com/sipeed/picoclaw/pull/3319) — 修复 exec 工具忽略单次调用超时参数、以及布尔选项类型声明错误的问题。
- [PR #3200](https://github.com/sipeed/picoclaw/pull/3200) — 为模型页面增加可配置的默认 fallback 链，提升模型路由灵活性。
- [PR #3222](https://github.com/sipeed/picoclaw/pull/3222) — 重构 DeltaChat 实现，精简约 200 行代码并更新文档。

这些 PR 若能及时合并，将修复若干稳定性问题并改善多通道体验。但需警惕 stale 机制可能误关有价值的待合并 PR。

## 4. 社区热点
最热门的讨论集中在 [Issue #3269](https://github.com/sipeed/picoclaw/issues/3269) — MCP server 连接失败导致 agent 循环挂起，共 5 条评论、1 个 👍，用户反馈 PicoClaw 聊天界面在 MCP 故障时完全停止响应，影响直接且严重。该 Issue 同时催生了修复 PR #3337，形成“报告-修复”的良性循环。

另外两个讨论点分别是 [Issue #3307](https://github.com/sipeed/picoclaw/issues/3307)（Telegram 等渠道的 session 管理功能）和 [Issue #3308](https://github.com/sipeed/picoclaw/issues/3308)（并发风险、goroutine 泄漏等代码审查），但均已被 stale 机制关闭，反映出社区对跨渠道会话管理和代码质量的关注未能得到维护者及时回应。

## 5. Bug 与稳定性
- **[严重]** [Issue #3269](https://github.com/sipeed/picoclaw/issues/3269) — MCP server 连接失败时 agent loop 挂起，PicoClaw 聊天界面停止回复用户。已有对应修复 PR [#3337](https://github.com/sipeed/picoclaw/pull/3337) 提交，等待合并。
- **[中等]** [PR #3319](https://github.com/sipeed/picoclaw/pull/3319) — exec 工具同步执行时忽略单次调用的 `timeout` 参数，且 `background`/`pty` 被错误声明为字符串而非布尔值。修复已提交，待合并。
- **[中等]** [PR #3279](https://github.com/sipeed/picoclaw/pull/3279) — seahorse 的 `partsToReadableContent` 可能导致 tool-call 格式泄漏到 LLM 摘要，污染用户上下文。该 PR 已被 stale 关闭，需要维护者确认是否仍要修复。

## 6. 功能请求与路线图信号
- [Issue #3307](https://github.com/sipeed/picoclaw/issues/3307) 请求为 Telegram 等聊天渠道增加 session 列出/切换/删除命令，以对齐 Web UI 的会话管理能力。该 issue 被 stale 关闭，但需求真实，若未来支持多渠道会话管理可重新开启。
- [PR #3200](https://github.com/sipeed/picoclaw/pull/3200) 的可配置模型 fallback 链是提升生产环境容错能力的重要信号，当前待合并。
- 被关闭的 [PR #3270](https://github.com/sipeed/picoclaw/pull/3270)（DashScope TTS + 微信音频发送）和 [PR #3283](https://github.com/sipeed/picoclaw/pull/3283)（钉钉图片消息）表明社区有扩展多模态和 IM 平台能力的需求，但可能因优先级或资源限制未被接纳，建议在路线图中重新评估。

## 7. 用户反馈摘要
- 在 [Issue #3269](https://github.com/sipeed/picoclaw/issues/3269) 中，用户报告了真实痛点：MCP server 一旦不可达，整个 agent 循环会卡死，聊天界面完全无响应，直接影响生产可用性。
- 在 [Issue #3307](https://github.com/sipeed/picoclaw/issues/3307) 中，用户指出 Web UI 与 Telegram 等渠道的功能不对等，明确表达了“至少能列出、切换、删除会话”的诉求，说明跨渠道一致性是用户关注方向。
- 在 [Issue #3308](https://github.com/sipeed/picoclaw/issues/3308) 中，用户主动进行代码审查，指出并发风险、goroutine 泄漏和内存/速度优化点，反映出部分用户具有较高技术参与度，同时也暗示项目需要加强代码评审与性能加固。

## 8. 待处理积压
以下重要 PR/Issue 长期未合并或未得到维护者响应，建议关注：

- [Issue #3269](https://github.com/sipeed/picoclaw/issues/3269) — 严重 BUG，已有修复 PR #3337，需尽快 review 和合并。
- [PR #3337](https://github.com/sipeed/picoclaw/pull/3337) — 修复 #3269 的 PR，创建于 2026-08-14，处于待合并状态。
- [PR #3222](https://github.com/sipeed/picoclaw/pull/3222) — DeltaChat 重构 PR，自 2026-07-03 起已开放超 6 周。
- [PR #3200](https://github.com/sipeed/picoclaw/pull/3200) — 模型 fallback chain 功能 PR，自 2026-07-01 起开放超过 6 周。
- [PR #3319](https://github.com/sipeed/picoclaw/pull/3319) — exec 工具修复 PR，自 2026-08-07 起已等待 1 周。
- 另有一批被 stale 标记并关闭的 PR（如 [#3279](https://github.com/sipeed/picoclaw/pull/3279)、[#3270](https://github.com/sipeed/picoclaw/pull/3270)、[#3283](https://github.com/sipeed/picoclaw/pull/3283)），如果维护者仍认可其价值，请及时取消 stale 或重新开启，避免有价值的功能与修复流失。

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报 — 2026-08-15

## 1. 今日速览

过去 24 小时 NanoClaw 保持较高的维护活跃度：共产生 2 条新 Issue、9 条 PR 更新，其中 3 条已关闭/合并，6 条待合并。新版本发布为 0，整体处于「集中修复 + 基础设施验证」的迭代节奏。值得关注的是，核心团队完成了签名审批器（signature approver）的第二次实弹测试并关闭测试 PR，CI 可靠性有所增强。两条新报告的 Bug（Node 版本判断、AVX2 兼容性）均属开箱即用体验问题，其中一条已有配套修复 PR。整体项目健康度良好，社区侧讨论热度偏低，但工程侧响应及时。

## 3. 项目进展

今日关闭/合并的 3 条 PR 中，1 条为实质代码修复，2 条为核心团队内部流程验证（不合并）：

- **[#3243 [CLOSED] verify-agent-image: arming auto-merge is not a verdict](https://github.com/nanocoai/nanoclaw/pull/3243)**（core-team，已合入）— 修复 CI 中「启用自动合并」步骤的失败被误判为镜像验证失败的问题。该步骤在 draft PR、`allow_auto_merge` 关闭等场景下必然失败，与镜像本身质量无关。修复后验证逻辑决策更准确，避免了针对镜像签名审批的误报。
- **[#3242 [CLOSED] DO NOT MERGE — live-fire test of the signature approver](https://github.com/nanocoai/nanoclaw/pull/3242)**（core-team，已关闭不合并）— 签名审批器实弹测试第一轮，验证 verify → approve-agent-image → cosign 独立验证 → 审批评论的完整链路，按计划关闭。
- **[#3244 [CLOSED] DO NOT MERGE — live-fire test of the signature approver (take 2)](https://github.com/nanocoai/nanoclaw/pull/3244)**（core-team，已关闭不合并）— 在第一轮基础上，借助 #3243 的修复确保 verify 在 draft PR 上也能通过，使审批器在测试中正确触发并独立复核。测试完成，按计划关闭。

整体而言，今日并无大的 Feature 合入，但 CI 基础设施的稳定性和容器镜像审批自动化程度得到实质提升，为后续更安全的自动化合并流程奠定了基础。

## 4. 社区热点

从当前数据看，今日所有 Issue/PR 的评论数均为 0，未出现高热度集中讨论。相对值得关注的是以下两条新 Issue，它们均直接命中用户开箱即用环节：

- **[#3245 Prebuilt agent image: Bun binary requires AVX2 — SIGILL on CPUs without it](https://github.com/nanocoai/nanoclaw/issues/3245)** — 默认安装向导推荐的预构建镜像在 Intel Tremont/Elkhart Lake（如 Celeron J6413/N5105）等无 AVX2 指令集的 CPU 上直接 SIGILL 崩溃。该问题影响面较广（涉及一大类低功耗/嵌入式平台），是安装失败类的硬阻塞。目前尚无对应修复 PR。

- **[#3248 [bug] setup.sh's "Node missing or too old" branch cannot handle too old](https://github.com/nanocoai/nanoclaw/issues/3248)** — 安装脚本对「Node 太旧」分支的处理存在逻辑漏洞：`install-node.sh` 会在检测到任意 Node 时短路，导致 setup.sh 中“Node 版本过旧”的分支实际无法生效。用户 glifocat 在提交 Issue 的同一天附上了修复 PR #3249，属于典型的高质量社区贡献。

两条 Issue 共同反映了用户对「在不同硬件/系统环境下平滑安装」的强烈诉求。

## 5. Bug 与稳定性

| 严重程度 | Issue | 描述 | 修复状态 |
|---|---|---|---|
| 🔴 高 | [#3245](https://github.com/nanocoai/nanoclaw/issues/3245) | 预构建镜像要求 AVX2，在不支持的 CPU 上直接 SIGILL | 暂无修复 PR，需跟进 |
| 🟠 中高 | [#3248](https://github.com/nanocoai/nanoclaw/issues/3248) | setup.sh 对「Node 已存在但过旧」的场景处理失效 | 已有修复 PR [#3249](https://github.com/nanocoai/nanoclaw/pull/3249)，待合并 |
| 🟡 中 | [#3247](https://github.com/nanocoai/nanoclaw/pull/3247)（PR 即修复） | 非法 cron 表达式（如 `0 21-5 * * *`）导致每次调度扫描都重复报错 | PR 待合并 |
| 🟡 中 | [#3246](https://github.com/nanocoai/nanoclaw/pull/3246)（PR 即修复） | 孤儿容器清理在 Windows 上因 shell 引号问题静默失效 | PR 待合并 |
| 🟢 低 | [#3230](https://github.com/nanocoai/nanoclaw/pull/3230)（PR 即修复） | 技能移除文档仍指向已退役的数据/环境镜像 | PR 待合并 |

两条新 Bug 中，#3245 影响面更大且暂无修复方案；#3248 的严重度中等但已快速获得社区贡献的修复 PR，体现了良好的社区协作节奏。

## 6. 功能请求与路线图信号

今日新增的 Issue/PR 均为 Bug 修复，无明确的新功能请求。值得关注的路线图信号来自两个已存在约一个月的 Dial 频道集成 PR，它们在今日有新的更新：

- **[#3050 feat(setup): add Dial to the channel picker + wizard/skills](https://github.com/nanocoai/nanoclaw/pull/3050)**
- **[#3041 feat(channels): add Dial channel adapter (SMS + AI voice calls)](https://github.com/nanocoai/nanoclaw/pull/3041)**

两者均由 OmriBenShoham 提交，分别覆盖安装向导侧的频道选择和运行时侧的频道适配器。Dial 支持短信与 AI 语音通话，属于沟通渠道扩展方向。结合作者仍在持续更新，该功能有一定概率进入下一版本，但需维护者推动合并流程。

## 7. 用户反馈摘要

由于当前各 Issue/PR 的评论区均为空，此处基于 Issue 描述提炼间接反馈：

- **安装流程的健壮性不足**（#3248）：用户期望 setup.sh 能在 Node 已安装但版本过低时给出清晰引导，而不是因脚本内部短路而跳过版本检测。这说明安装脚本需要覆盖更多“已存在但版本不合格”的环境状态。
- **官方镜像的硬件兼容性影响真实部署**（#3245）：用户使用默认推荐的 hardened 镜像时，在老款 Intel 处理器上直接崩溃。对于低功耗设备用户而言，这是严重的使用障碍，期望项目能提供 baseline 构建或在不支持 AVX2 的环境下给出可操作的降级方案。

## 8. 待处理积压

以下 PR 已有较长时间未获得合并，需要维护者关注：

- **[#3050 feat(setup): add Dial to the channel picker + wizard/skills](https://github.com/nanocoai/nanoclaw/pull/3050)** — 创建于 2026-07-14，已开放一个月有余，今日仍有更新，但尚未进入合并评审流程。
- **[#3041 feat(channels): add Dial channel adapter (SMS + AI voice calls)](https://github.com/nanocoai/nanoclaw/pull/3041)** — 同样创建于 2026-07-14，与 #3050 为同一功能的两部分。长时间未合并可能会增加社区贡献者的维护成本，建议尽快安排评审。

今日新 Issue 均为新创建，暂无长期无响应的积压问题。

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

# NullClaw 项目日报 — 2026-08-15

## 1. 今日速览

过去 24 小时项目活跃度较低：Issues 无新增、无关闭；仅 1 条 PR 被关闭（未标记为待合并，状态为已合并/关闭），该 PR 为 SQLite 内存数据库路径可配置功能。无新版本发布。整体来看，项目处于低频维护期，但本次 PR 为解决只读工作区部署场景提供了关键灵活性，属于值得关注的功能增强。社区讨论氛围平淡，无热点议题。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

### 合并/关闭 PR：SQLite 内存数据库路径可配置
- **链接**: [nullclaw/nullclaw PR #986](https://github.com/nullclaw/nullclaw/pull/986)
- **状态**: 已关闭（未标记待合并，视为已合并） | 作者: gently-whitesnow | 更新于 2026-08-14

**核心变更**:
- 为 SQLite 主存引擎新增 `memory.database_path` 配置项。
- 当该配置为空时，保持原有默认位置 `<workspace>/memory.db` 不变（完全向后兼容）。
- 相对路径会基于工作区解析，同时支持绝对路径，以适应只读工作区部署（例如容器或挂载只读文件系统的场景）。
- 为配置项补充了文档说明。

**项目意义**: 这一改动解除了 SQLite 引擎对工作区可写目录的硬性依赖，使 NullClaw 在只读/受限部署环境下能够将数据库放置在其他可写位置，是部署灵活性和可运维性的有效提升。同时，默认值保持不变，没有破坏现有用户的配置习惯。

## 4. 社区热点

今日无高讨论度 Issues 或 PR。唯一更新的 PR #986 没有评论数据，也未获得点赞，但作为当日唯一活动项，仍可视为社区关注的技术方向（部署灵活性）。由于讨论量极低，无法基于评论内容做进一步热点分析。

## 5. Bug 与稳定性

今日无新增 Bug、崩溃、回归报告。项目稳定性表现平稳，未收到需要优先处理的不稳定反馈。

## 6. 功能请求与路线图信号

PR #986 本身是一个显性的功能增强请求，来自对只读工作区部署的支持需求。这暗示用户群体中存在以下趋势：
- 对容器化、不可变基础设施部署的兴趣增加；
- 希望将状态数据（如 memory.db）与代码/工作区分离开来，以便管理持久化；
- 对配置项灵活性的诉求（路径可配置）高于简单默认值。

结合该 PR 的快速合并（创建当天即关闭），可以推测维护者愿意快速响应这类提升部署能力的改动，后续版本可能继续围绕“部署模式适配”推出更多配置项。

## 7. 用户反馈摘要

由于今日无 Issue 评论、PR 评论或讨论数据，无法提炼直接的用户反馈。从 PR #986 的变更内容推断，真实场景中的痛点包括：
- 只读文件系统部署下，SQLite 引擎无法创建或写入 `memory.db`；
- 需要更多数据库路径控制权，以便将数据与工作区分离。

未见对项目当前功能的不满反馈（可能是由于活跃度低，而非没有问题）。

## 8. 待处理积压

当前积压数量为 0。在过去的 24 小时内，没有长期未响应的 Issue 或 PR 需要提醒维护者关注。项目 backlog 处理情况健康，未见被忽视的僵尸任务。

---

**备注**: 本期日报基于 2026-08-15 当年 24 小时数据快照生成，所有日期与链接均依据提供的数据标注（快照时间显示为 2026-08-15）。由于项目活动量较小，部分章节（如社区热点、用户反馈）内容较为简略，属正常现象。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报 — 2026-08-15

## 今日速览

IronClaw 在过去 24 小时保持高压迭代节奏：**1.2.0 稳定版发布后的合并回主线工作基本完成**（#7657），发布修复已同步到 main；与此同时，**1.3.0 自动化可靠性 epic（#6879）进入密集设计+实现阶段**，今日开出 4 个关联子 issue（#7644-#7647）并有 2 个对应 PR（#7650、#7651）提交，是当前最集中的开发主线。QA bug bash 同步推进，Slack UI 状态误报（#7660）、Telegram MP4 上传失败（#7662）、扩展状态跨用户泄漏（#7659）三个 P2 bug 已归档。宏观上项目健康度良好：24 小时内 9 个 issue 关闭、22 个 PR 合并/关闭，吞吐量处于历史高位，但大型 PR（XL）占比高，存在 Review 队列积压风险。

## 版本发布

### [ironclaw-v1.2.0](https://github.com/nearai/ironclaw/releases) — 1.2.0 · 2026-08-13

由 `1.2.0-rc.3` 稳定推广而来，包含 RC1 完整功能集以及 RC2/RC3 中验证的修复。

**本次发布关键修复（来自 RC3）**：
- 运行时容器镜像现安装 `curl`，使容器内 HTTP 健康检查可执行，修复 Orchestrator 探活失败问题。

**随发布线一并合入的修复**（经 #7657 前向移植确认）：
- 线程索引投影修复（thread-index projection repair）
- Windows 文件系统/冒烟测试可靠性修复
- Windows JSON 输出格式修复

**迁移注意事项**：
- 从 1.0/1.1 升级需执行状态保留迁移，1.2 发布线已在合并回 main 时验证该迁移以及后端/领域契约覆盖（见 [#7657](https://github.com/nearai/ironclaw/pull/7657)）。
- 生产环境部署 1.2.0 时请确认运行时容器镜像已更新（含 `curl`），否则 orchestrator HTTP 健康检查将无法执行。

## 项目进展

今日共 **22 个 PR 合并/关闭**，以下为关键合并（均已合回 main）：

| PR | 内容 | 影响 |
|---|---|---|
| [#7657](https://github.com/nearai/ironclaw/pull/7657) | **合并 1.2.0 发布线回 main** | 将 1.2.0 修复、迁移、升级金丝雀测试全部同步至主干，消除发布-主干漂移 |
| [#7668](https://github.com/nearai/ironclaw/pull/7668) | **扩展提供商认证诊断** | GitHub 等 provider 运行时 401 错误保留稳定错误码与诊断信息，经由 WASM/ABI/capability/durable gate/deny-resume 全链路传递给模型，避免泛化为 re-auth 上下文 |
| [#7665](https://github.com/nearai/ironclaw/pull/7665) | **支持 origin-scoped 托管 MCP OAuth** | 允许 MKT1 场景中 HTTPS `/mcp` 端点采用 RFC 9728 同源授权形状，并完整保留 OAuth resource/metadata URL 至 DCR、token exchange、refresh 链路 |
| [#7652](https://github.com/nearai/ironclaw/pull/7652) | **生产 DB 写入压力测量** | 测量真实 agent turn（10 次内置工具调用、11 次模型尝试）的 DB 写入与长时间空闲进程的写入模式——为 #7591 epic 提供基线 |
| [#7666](https://github.com/nearai/ironclaw/pull/7666) | **扩展卡片/安装结果"说真话"** | 修复 QA #7660 中扩展卡片与安装结果误导性问题；设备链接安装引导用户走 Web UI link 步骤 |
| [#7658](https://github.com/nearai/ironclaw/pull/7658) | **Telegram 2FA 门控识别 + 登录码位置提示** | 修复迁移 DC 上的 2FA 账户扫码失败，并让用户明确知道登录码发送位置 |
| [#7655](https://github.com/nearai/ironclaw/pull/7655) | **CI 集成覆盖率下限重新固定** | 将两个失败 crate 的覆盖率下限对齐至 main 实际观测值，恢复 CI 绿线 |

**整体评估**：1.2.0 发布余波及发布线合主已接近尾声；1.3.0 自动化可靠性从设计走向实现（#7650/#7651），同时"unbound-turns"大型特性列车（#7562 + #7634）持续推进，已完成 71 条款设计文档一致性审计。项目正处于**前一个版本收尾与下一版本核心特性发力并行**的过渡期。

## 社区热点

### 1. 自动化运行可靠性 Epic（#6879） — 最长链讨论

[Issue #6879](https://github.com/nearai/ironclaw/issues/6879) 是当前最受关注的 epic，今日衍生出 4 个新子 issue（#7644-#7647），并有 2 个对应实现 PR（#7650、#7651）。问题核心：**相同提示词在无人值守运行时结果不稳定，尤其在 DeepSeek V4 Flash 等小模型上**。审计发现这是结构性缺陷——触发的执行在交互式聊天中作为普通对话轮次进行，而非确定性自动化运行。

**背后诉求**：用户和企业客户希望 IronClaw 的 scheduled/automation 运行具有**可预测性**，而非模型噪声驱动的"抽奖"。这已成为 v1.3.0 的最优先路线图项。

### 2. 可插拔内存系统（#7664） — 新架构讨论

[Issue #7664](https://github.com/nearai/ironclaw/issues/7664) 提出将内存后端（native/mem0）从编译期工厂 arm 改为**运行时配置绑定**，以 Mnesis Core 作为首个外部消费者。对应 [PR #7661](https://github.com/nearai/ironclaw/pull/7661) 已提交（XL），映射 IronClaw 内存逻辑到 MCP 协议。

**背后诉求**：社区用户对持久记忆能力有强烈需求，但目前 memory 后端固定在编译期内置选项，未开放给第三方。该 issue 打开了"外部记忆系统可插拔"的可能性。

### 3. WebUI 结构化 Ask User 卡片（#7653） — 新交互模式

[Issue #7653](https://github.com/nearai/ironclaw/issues/7653) 提议在 WebUI 实现 OMP 风格的 `ask` 工具卡片交互，复用终端 `LoopCompletionKind::AskUserReply` 机制。这是"让 agent 在 Web 界面向用户提问"的能力补全。

## Bug 与稳定性

按严重程度排列（P2 为 bug bash 定义，均来自 Railway qa-testing 实例）：

| 严重度 | Issue | 问题描述 | 状态 |
|---|---|---|---|
| **P2（数据隔离）** | [#7659](https://github.com/nearai/ironclaw/issues/7659) | 当前用户可见其他用户安装的扩展，扩展状态疑似跨用户泄漏 | 🟡 无 fix PR |
| **P2（功能失效）** | [#7662](https://github.com/nearai/ironclaw/issues/7662) | Telegram 上传 MP4 附件失败，报 `invalid_value (attachments.mime_type)`，尽管文件识别为 video/mp4 | 🟡 无 fix PR |
| **P2（UI 误导）** | [#7660](https://github.com/nearai/ironclaw/issues/7660) | Slack 已连接且功能正常，但 UI 仍显示 "Finish Setup" 徽章和 "Reconnect" 按钮 | 🟡 无专门 fix PR（#7666 仅对扩展卡片同理问题做了修复） |
| **P2（登录异常）** | [#7667](https://github.com/nearai/ironclaw/issues/7667) | Telegram 手机模式登录时 `PHONE_MIGRATE_1` 重新发送成功，但用户未收到登录码；且 UI 提示未反映 `sentCode.type_` | 🟢 已有 [PR #7658](https://github.com/nearai/ironclaw/pull/7658) 修复 2FA 门控与登录码位置提示（该 PR 已合入） |
| **P2（数据损坏）** | [#6869](https://github.com/nearai/ironclaw/issues/6869) | 生成的 DOCX 损坏无法用 Word 打开（用户反馈） | ✅ 已关闭（已解决） |

**稳定性评估**：QA bug bash 报告的问题多数为 UI 状态一致性和边缘功能缺陷，暂无 P0/P1 级别的核心链路故障。Telegram 登录问题已通过 #7658 合入修复；本轮约 **4/5 的 QA 问题仍未关联 fix PR**，需关注后续补丁覆盖。

## 功能请求与路线图信号

### 可能纳入 v1.3.0（已有实现 PR 支撑）

| 请求/特性 | 状态 | 对应 PR | 说明 |
|---|---|---|---|
| **自动化确定性无结果抑制**（[#7647](https://github.com/nearai/ironclaw/issues/7647)） | 已提交 | [#7651](https://github.com/nearai/ironclaw/pull/7651) | 强制 trigger 显式选择 deliver 或 suppress，模型返回 `[SILENT]` 实现静默交付 |
| **自动化语义执行结果持久化**（[#7650](https://github.com/nearai/ironclaw/pull/7650) 关联） | 已提交 | [#7650](https://github.com/nearai/ironclaw/pull/7650) | 运行成功后再评估结构化结果，移除隐藏 reconciler，引入异步 judge 任务 |
| **自动化预检 grants + 通行审批租赁**（[#7646](https://github.com/nearai/ironclaw/issues/7646)） | 规划中 | — | 在 armed 前验证创建者的 grants，防止无人值守执行中途失败 |
| **结构化自动化验证后再 arm**（[#7644](https://github.com/nearai/ironclaw/issues/7644)） | 规划中 | — | 创建时即验证结构化执行契约可满足 |
| **每自动化 LLM 模型固定**（[#7645](https://github.com/nearai/ironclaw/issues/7645)） | 规划中 | — | 避免默认模型变化导致自动化行为漂移 |
| **可插拔内存（MCP）**（[#7664](https://github.com/nearai/ironclaw/issues/7664)） | 已提交 | [#7661](https://github.com/nearai/ironclaw/pull/7661) | 基于配置绑定内存后端，首个外部 consumer 为 Mnesis Core |
| **WebUI 结构化 Ask User 卡片**（[#7653](https://github.com/nearai/ironclaw/issues/7653)） | 规划中 | — | OMP 风格 `ask` 工具，非阻塞可恢复 |

### 前端体验类（均已设计完毕，待实施）

- **共享 SearchField 组件**（[#7569](https://github.com/nearai/ironclaw/issues/7569)）— 已关闭，设计完成
- **共享 InlineNotice 组件**（[#7639](https://github.com/nearai/ironclaw/issues/7639)）— 统一 info/success/warning/danger 反馈条
- **全局 toast 代替 window.alert**（[#7638](https://github.com/nearai/ironclaw/issues/7638)）
- **类型化设计系统组件边界**（[#7637](https://github.com/nearai/ironclaw/issues/7637)）— 补齐显式 prop types

## 用户反馈摘要

- **营销团队明确表达 per-user 模型选择需求**：在 2026-07-23 Champions weekly check-in 中，Jeremy Koch（marketing）提出当前模型选择仅管理员可控（[#7183](https://github.com/nearai/ironclaw/issues/7183)）。该 issue 今日已关闭，但这是一个**强真实用户诉求**——让非管理员用户能自主切换 LLM 模型。
- **DOCX 生成质量差有损信任度**：用户 Davin Basi 的反馈（[#6869](https://github.com/nearai/ironclaw/issues/6869)）指出，ChatGPT 和 Claude 都能轻松完成带标记的 NDA DOCX 生成，IronClaw 连续两次失败并出现协议违规报错——此类与竞品对比的负面反馈需要产品侧重点重视，不过该 issue 今日已关闭（已解决）。
- **无人值守运行的不可靠严重损害自动化信心**（[#6879](https://github.com/nearai/ironclaw/issues/6879)）：用户观察到相同存储的提示词在自动化中"有时候成功有时什么都没有"，本质上反映了 **IronClaw 目前缺少自动驾驶级别的确定性执行契约**，这是当前 v1.3.0 重点解决的问题。
- **QA 测试者的直接痛点**：Slack 连接状态 UI 与真实状态不一致（#7660）、扩展页泄露他人安装状态（#7659），这些会让用户**质疑数据隔离可靠性与产品成熟度**。

## 待处理积压

### 长期未合并 PR

| PR | 创建日期 | 天数 | 备注 |
|---|---|---|---|
| [#7255](https://github.com/nearai/ironclaw/pull/7255) APDD 治理框架评估 | 08-05 | 10 天 | Docs-only，无阻塞风险；建议 review 后合并或明确关闭 |
| [#7378](https://github.com/nearai/ironclaw/pull/7378) doc-fact 契约测试 | 08-07 | 8 天 | 与 #7379 同属 doc-truth 序列 3/5 |
| [#7379](https://github.com/nearai/ironclaw/pull/7379) docs-live 分支发布机制 | 08-07 | 8 天 | docs-truth 序列 4/5，解决文档与版本漂移 |
| [#7456](https://github.com/nearai/ironclaw/pull/7456) 持久存储 profile 无关化 | 08-10 | 5 天 | 涉及 Reborn 存储根目录重构，风险中等 |

### 长期未关闭 Issue

- **[#6879](https://github.com/nearai/ironclaw/issues/6879)（自动化可靠性 epic）**：自 07-29 发起，已完成审计并拆分为 4 个子任务实施中，预计随 v1.3.0 迭代收尾。
- **[#7624](https://github.com/nearai/ironclaw/issues/7624)（ACP harness executor）**：为 pluggable loops 的 v0 工作项，已有对应 [PR #7648](https://github.com/nearai/ironclaw/pull/7648)，建议关注 review 进度。

### 维护提醒

大型 PR（size: XL）在当前 PR 队列中占比显著（#7562、#7634、#7648、#7650、#7661、#7663 等），且多数为 same-owner 连续提交。建议维护者：
1. 确保 XL PR 拆分为可独立 review 的提交序列（#7634 已在做）；
2. 关注 #7562/#7634 这条 unbound-turns 特性列车，其多阶段 merge 需要优先处理，避免后续 PR 产生过多 merge conflict。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报 — 2026-08-15

## 1. 今日速览

过去24小时内，LobsterAI 项目整体呈现**高活跃度、快速迭代**态势：共产生 2 条 Issue 更新和 27 条 PR 更新，合并/关闭 22 条 PR，并发布 1 个新版本 `2026.8.14`。从 PR 内容看，本轮更新集中在多智能体协作（cowork）UI 交互优化、OpenClaw 技能管理修复、账户体系视觉打磨及依赖升级，其中 `Release: 2026.7.30` PR（#2498）展示了 67 个 commit、264 个文件变更的大规模合并，标志着 Team Edition 账户与配额流、Skills/Connectors 体验刷新等重大功能正式合入主干。社区侧用户对新版本（v4pro）的期待呼声较高（Issue #2489），且针对侧边栏广告的永久隐藏需求已有对应 PR 被提出，反映出商业化元素与用户体验之间的张力正在成为社区关注点。整体项目健康度良好，维护者响应及时，版本发布节奏稳定。


## 2. 版本发布

### LobsterAI 2026.8.14

**发布日期**：2026-08-14

**主要更新内容**：
- **侧边栏（sidebar）增强**：新增签到（check-in）与横幅轮播（banner carousel）支持，见 PR [#2411](https://github.com/netease-youdao/LobsterAI/pull/2411)
- **多智能体任务过滤**：侧边栏新增多智能体任务活动过滤器，见 PR [#2418](https://github.com/netease-youdao/LobsterAI/pull/2418)

**破坏性变更**：从 Release Notes 摘要来看，未披露明显的破坏性变更。

**迁移注意事项**：由于侧边栏交互与过滤逻辑发生变化，自定义了侧边栏布局或依赖旧版任务列表行为的用户，建议关注相关配置是否仍然有效。完整变更日志请参考官方 Release 页面。


## 3. 项目进展

今日合并/关闭的 22 条 PR 涉及多个功能模块的修复与优化，按其影响面分类如下：

### 🚀 大型版本合并
- **[#2498](https://github.com/netease-youdao/LobsterAI/pull/2498) — Release: 2026.7.30**：将 `release/2026.7.30` 分支的最终变更合入 `main`。该分支领先 `origin/main` 67 个 commit，变更 264 个文件（+24,736/-4,253），引入 Team Edition 账户与配额流程、刷新 Skills 与 Connectors 体验，是本次更新中体量最大的一次合并。

### 🐛 功能修复
- **[#2499](https://github.com/netease-youdao/LobsterAI/pull/2499) — cowork 回合折叠逻辑修复**：修复了回合在等待回复时（如 `sessions_yield` 之后）被过早折叠成空行的问题，现在仅在存在答案块后才折叠。
- **[#2491](https://github.com/netease-youdao/LobsterAI/pull/2491) 与 [#2483](https://github.com/netease-youdao/LobsterAI/pull/2483) — OpenClaw 技能条目 key 修复**：修复目录名与 frontmatter name 不匹配时，UI 技能开关静默失效的问题。两条 PR 分别从不同角度修复了同一问题，说明团队对 OpenClaw 技能管理正确性的重视。
- **[#2493](https://github.com/netease-youdao/LobsterAI/pull/2493) — 会话导出图片与卡片切换 UI 修复**。
- **[#2497](https://github.com/netease-youdao/LobsterAI/pull/2497) — i18n 文案优化**：改进 cowork goal 与 steer 的文案表述。

### ✨ 新功能
- **[#2490](https://github.com/netease-youdao/LobsterAI/pull/2490) — cowork 浏览器注解附件预览**：将浏览器注解截图渲染为用户消息中的编号附件卡片，并在独立工件面板中打开，替代通用图片预览弹窗。
- **[#2495](https://github.com/netease-youdao/LobsterAI/pull/2495) — 排版字体大小提升**：增大默认 UI/代码字体大小，并提供一次性迁移机制。

### 🎨 UI 优化
- **[#2494](https://github.com/netease-youdao/LobsterAI/pull/2494) 与 [#2492](https://github.com/netease-youdao/LobsterAI/pull/2492) — 账户积分图标样式对齐**：替换图标与统一颜色，保持明暗主题下的视觉一致性。
- **[#2496](https://github.com/netease-youdao/LobsterAI/pull/2496) — cowork 徽章弹层视口适配**：确保徽章弹层不超出视口且位于后续消息之上。

**整体评估**：项目今日不仅在快速修复缺陷（技能 key 不匹配、折叠逻辑误判），还完成了大型功能分支的合并，并持续打磨 UI 细节，整体向前迈进了至少一个大版本的功能体量。


## 4. 社区热点

### 热点一：用户急切期待新版发布
- **Issue [#2489](https://github.com/netease-youdao/LobsterAI/issues/2489) — “快更新v4pro！”**：创建于 2026-08-14，已有 1 条评论，👍 0。该 Issue 标题直白，表达了用户对 v4pro 版本的迫切期待，虽未给出详细反馈，但反映了用户群体对项目迭代速度的关注。

### 热点二：侧边栏广告横幅的永久隐藏需求
- **PR [#2374](https://github.com/netease-youdao/LobsterAI/pull/2374)（OPEN）— feat: add permanent setting to hide sidebar ad banner**：该 PR 已开放 24 天，针对 Issue #2342 提出在 **Settings → General** 中新增永久隐藏侧边栏广告横幅的开关。评论数为 undefined（可理解为尚未有评论），但此类需求直指商业化与用户体验的平衡点，具有较高的用户共鸣度。

**分析**：当前社区的活跃讨论集中在两个方向——（1）对新版本的期待（v4pro），（2）对应用内广告控制权的诉求。前者表明用户对项目信心较高、期望快速获得新能力；后者则提示团队在商业化推进过程中需要提供更灵活的用户控制选项。


## 5. Bug 与稳定性

今日报告的 Bug 不多，但已发现的问题均已得到快速响应：

### 中等严重程度
- **技能切换静默失效（已修复）**：OpenClaw 中，当技能目录名与 frontmatter name 不一致时，`skills.entries` 覆盖无效，导致用户在 UI 中切换技能时静默失败。已由 PR [#2491](https://github.com/netease-youdao/LobsterAI/pull/2491) 修复（同时 #2483 提供了并行的修复方案，已合并）。该问题直接影响用户对技能启停的控制，修复及时。

### 轻度问题
- **cowork 回合折叠显示异常（已修复）**：回合在等待父进程恢复时被折叠为空行，容易误读为失败状态。已由 PR [#2499](https://github.com/netease-youdao/LobsterAI/pull/2499) 修复。
- **会话导出图片与卡片切换 UI 异常（已修复）**：已由 PR [#2493](https://github.com/netease-youdao/LobsterAI/pull/2493) 修复。

**结论**：今日无新增严重 Bug 或崩溃报告，既有问题均在当天完成修复，项目稳定性表现良好。


## 6. 功能请求与路线图信号

- **永久隐藏侧边栏广告**：用户此前只能临时关闭单条横幅，无法永久禁用。PR [#2374](https://github.com/netease-youdao/LobsterAI/pull/2374) 提出了用户级设置项，若被合并，将纳入下一版本。该信号表明项目正在逐步完善商业化场景下的用户控制力。
- **安全模块测试覆盖**：Issue [#1154](https://github.com/netease-youdao/LobsterAI/issues/1154) 提出为 `commandSafety` 与 `coworkMemoryJudge` 补充 Vitest 单元测试，这两个模块分别负责危险命令检测和记忆写入质量管理。虽然此 Issue 已标记为 stale，但安全相关测试的补强对 AI 助手项目至关重要，建议维护者重新评估其优先级。
- **v4pro 版本发布**：Issue [#2489](https://github.com/netease-youdao/LobsterAI/issues/2489) 虽未说明具体功能诉求，但结合近期 Team Edition 与 Skills 体验的合并（PR #2498），可推断用户对高级版本能力存在较高期待。


## 7. 用户反馈摘要

- **对新版本迭代速度的期待**：用户 `nimamasl114514` 在 Issue [#2489](https://github.com/netease-youdao/LobsterAI/issues/2489) 中直接催促“快更新v4pro！”，虽内容简短，但映射出部分用户对功能更新交付周期的敏感度高，也可能是对某些已承诺功能（如 Skills/Connectors 刷新）的等待。
- **对广告展示的控制诉求**：PR [#2374](https://github.com/netease-youdao/LobsterAI/pull/2374) 的提出表明有用户对侧边栏广告的反复出现感到困扰，希望得到“一次设置、永久生效”的控制能力，而非每次手动关闭。
- **功能使用中的细节痛点**：从已合并的 PR 可间接推断用户反馈的问题点：技能开关“看似生效实则无效”（#2483/#2491）、cowork 回合等待期被误判为失败（#2499）、字体大小偏小（#2495）等。这些反馈虽未直接晒出评论，但修复的及时性说明维护者在持续跟踪用户体验细节。


## 8. 待处理积压

以下为长期未获响应或价值较高但已 stale 的 Issue/PR，提醒维护者关注：

### 高优先级
- **PR [#1153](https://github.com/netease-youdao/LobsterAI/pull/1153)（OPEN，stale）— 修复 Gemini /v1 路径 URL 拼接错误**：`buildOpenAIChatCompletionsURL` 对 Google Gemini 以 `/v1` 结尾的 baseURL 处理时多切了一个字符，导致 URL 缺少分隔符。该 PR 创建于 2026-03-31，已积压 4 个半月。问题直接影响 Gemini 接入的可用性，建议尽快 review 或关闭并给出替代方案。

### 中优先级
- **Issue [#1154](https://github.com/netease-youdao/LobsterAI/issues/1154)（OPEN，stale）— 为 commandSafety 和 coworkMemoryJudge 补充 Vitest 单元测试**：涉及危险命令检测与记忆质量门卫两大核心安全模块的测试覆盖。无测试意味着误判可能被静默引入，建议结合安全策略的优先级重新评估。
- **PR [#1155](https://github.com/netease-youdao/LobsterAI/pull/1155)（OPEN，stale）— 会话内页内搜索（Ctrl+F）**：功能设计完整（快捷键、精确高亮、TreeWalker 等），属于日常工作流高频刚需，但已 stale 超过 4 个月。若产品路线图无冲突，建议评估合入或关闭并说明原因。
- **PR [#2460](https://github.com/netease-youdao/LobsterAI/pull/2460) 与 [#2465](https://github.com/netease-youdao/LobsterAI/pull/2465) — 依赖升级（rimraf 与 vite）**：dependabot 创建的依赖升级 PR，分别将 rimraf 升至 6.1.3、vite 升至 8.2.1。均处于 OPEN 状态，涉及构建工具链与开发依赖，建议在 CI 通过的前提下尽早合并，避免因依赖过旧积累技术债。

---

**日报总结**：LobsterAI 项目在 2026-08-15 展现出强劲的迭代动能，核心功能（Team Edition、Skills/Connectors）正在加速落地，UI/UX 细节也在同步打磨。安全相关测试覆盖和部分长期积压的 PR 是当前值得关注的风险点。建议社区维护者重点跟进 #1153、#1154 与 #1155 三件积压事项，以进一步提升项目健康度。

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

## Moltis 项目动态日报 — 2026-08-15

### 今日速览
过去24小时，Moltis 项目活跃度整体平稳，社区侧动静较少：未出现新开/关闭的 Issue，也未发布新版本。开发侧的唯一动态是待合并 PR #1190 于昨日有内容更新，说明持久化连接器功能仍在推进中。总体来看，项目当前处于功能开发与集成能力储备阶段，外部社区互动较弱。

### 项目进展
- 过去24小时无 PR 合并或关闭。
- 当前积压的待合并 PR：
  - **[PR #1190] Add durable calendar, channel, and email connectors** — 作者：penso，更新于 2026-08-14  
    - 推出提供商无关的连接器持久化、原子快照、调度、投影和有界本地全文搜索  
    - 增加只读 CalDAV、Gmail、Himalaya v2 及可复用频道历史数据集，采用提供商自有 Schema，不复制用户凭证  
    - 引入提供商作用域信任模型，以安全方式连接外部服务  
    - 链接：https://github.com/moltis-org/moltis/pull/1190  

若该 PR 成功合并，将显著拓宽 Moltis 对日历、邮件和频道历史数据的本地接入能力，是向“本地优先数据整合”方向迈出的实质性一步。

### 社区热点
当前社区焦点集中在上述 [PR #1190](https://github.com/moltis-org/moltis/pull/1190) 上。虽然未报告评论及反应数据，公开讨论热度尚不明确，但该 PR 涉及的核心诉求非常清晰：用户在 Moltis 本地统一管理日历、邮件和频道信息，并保持对数据的自主控制权，同时避免第三方服务密码被复制或滥用。

### Bug 与稳定性
过去24小时内没有新的 Bug、崩溃或回归问题报告，项目稳定性数据面保持干净。未发现需要紧急响应的稳定性风险。

### 功能请求与路线图信号
从 [PR #1190](https://github.com/moltis-org/moltis/pull/1190) 的内容可捕捉到明确的路线图信号，下一版本可能纳入：
- 持久化连接器（Calendar、Gmail、Channel History）
- 本地全文搜索与原子快照机制
- 提供商中立的数据模型与信任边界设计

这些特性与 Moltis“本地优先的 AI 助手”定位高度一致，预示项目正在从基础的对话/记忆能力向多维个人数据整合演进。

### 用户反馈摘要
过去24小时没有新的 Issue 评论或用户反馈流入，因此无法提炼出新的用户痛点、使用场景或满意/不满意信号。建议后续持续观察 PR #1190 合并后的用户反馈。

### 待处理积压
- **[PR #1190](https://github.com/moltis-org/moltis/pull/1190)** 已开放 4 天，目前仍处于待合并状态。由于功能覆盖面较广，建议维护者尽快安排审查，避免长时间占用开发分支并增加后续合并冲突的潜在风险。

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

好的，这是 2026-08-15 的 CoPaw 项目动态日报。

---

# CoPaw 项目动态日报 — 2026-08-15

## 1. 今日速览

过去 24 小时 CoPaw 项目活跃度处于**高位**：跟踪到 50 条 Issue 更新与 41 条 PR 更新，其中 38 个 Issue 被关闭（多为旧 Issue 集中清理），15 个 PR 被合并/关闭。功能迭代速度较快，多个新 PR 集中在技能系统动态化（#7033）、子代理会话分组（#7035）、媒体下载（#7036）与 Computer Use（#7037）方向。**未发布新版本**，当前关注焦点集中在 2.1.0 的稳定性回归（MCP 重复数据、工具调用 404、多会话串扰）与插件生态兼容性上。⚠️ 注意：项目当前在 GitHub 数据中大量使用 **QwenPaw** 作为产品名（仓库为 CoPaw），下文按原始标题引用。

---

## 2. 版本发布

无新版本发布。

---

## 3. 项目进展

今日关闭（部分已合并）的 PR 主要覆盖以下方向：

- **技能系统动态化（Skill System）**：`#7029` / `#7031` 引入了动态技能加载、自动卸载（AutoUnloadHook）与 frontmatter 修复的中英文两个版本，均被关闭。随后 #7033 提交了该功能的新版本（OPEN），显示该功能仍在迭代中。
- **OneBot 渠道媒体本地化**（`#6715`）：PR 已关闭/合并，将 OneBot 入站图片、音频、视频与文件解析对齐 AgentScope 2.0 的本地 DataBlock 管线，修复了外部引用导致的媒体处理一致性。
- **插件渠道交互式配置**（`#6943`）：恢复对插件渠道 `get_configurator()` 的支持，并允许插件注册 HTTP 路由。
- **文档**：`#2105` 为 README 增加 Whisper 安装说明（关闭/合并）。

整体来看，项目正在围绕**渠道数据处理管线**（OneBot）与**插件体系建设**补齐基础设施，同时技能系统的重写已进入第二轮提 PR 阶段。风险点在于多个“动态技能加载”PR（#7029、#7031、#7033）反复提交，说明该功能实现方案仍在摇摆中。

---

## 4. 社区热点

今日评论数最高的 Issue 集中于以下几类：

| Issue | 标题 | 评论数 | 状态 | 链接 |
|---|---|---|---|---|
| #3045 | 自动获取模型为什么不可用 | 8 | 已关闭 | https://github.com/agentscope-ai/QwenPaw/issues/3045 |
| #2418 | 能否新增 skills-hub 管理页面 | 7 | 已关闭 | https://github.com/agentscope-ai/QwenPaw/issues/2418 |
| #2846 | 桌面端增加自动更新 + 任务栏图标问题 | 6 | 已关闭 | https://github.com/agentscope-ai/QwenPaw/issues/2846 |
| #7010 | 缺少真正的后台/守护模式（SSH 启动卡住） | 6 | 已关闭 | https://github.com/agentscope-ai/QwenPaw/issues/7010 |
| #6405 | 升级 2.0 后 MCP 工具提示 Tool not found | 6 | 已关闭 | https://github.com/agentscope-ai/QwenPaw/issues/6405 |

**需求信号解读**：

- **部署体验是最大痛点**：Windows 自动更新（#2846、#3464）与后台守护模式（#7010）均获得较高讨论，说明用户已从“试用”走向“生产环境长期部署”，对安装/更新/进程管理提出了基础设施级要求。
- **技能与 MCP 的易用性**：持续有用户提出“技能中心/Skills Hub”（#2418）与“MCP 工具失效”（#6405）的问题，反应出技能/工具生态是用户高频使用场景，但当前配置与排查门槛较高。

---

## 5. Bug 与稳定性

按严重程度排列：

**🔴 严重（会话正确性 / 数据安全）**

- **`#7011` Console stop 请求可取消活动中的飞书会话（2.1.0，OPEN）**：在不同 UI 会话间发生 session identity 串扰，导致 Console UI 的停止请求误取消飞书渠道正在进行的对话。涉及多会话隔离，属于会话状态机缺陷。  
  https://github.com/agentscope-ai/QwenPaw/issues/7011

- **`#6958` 调用 FastMCP 编写的 MCP 时 tool result 文件写两份重复数据（OPEN）**：当返回数据超过截断阈值时，result 文件中同一份数据以非结构化与格式化两行重复存在，影响下游解析。已有对应修复 PR `#6969`（OPEN）。  
  Issue: https://github.com/agentscope-ai/QwenPaw/issues/6958 | PR: https://github.com/agentscope-ai/QwenPaw/pull/6969

**🟠 中等（功能不可用 / 功能缺失）**

- **`#7016` 流式会话时工具调用 404（OPEN）**：前端持续轮询 `/api/tool-calls/.../{tool_call_id}/offload` 接口，但返回 `Tool call not found`，影响流式响应下的工具结果回归。  
  https://github.com/agentscope-ai/QwenPaw/issues/7016

- **`#7025` QwenPaw Creator 插件导致所有插件失效（OPEN）**：安装 Creator 插件后，现有插件全部无法工作。属于插件隔离/依赖管理问题。  
  https://github.com/agentscope-ai/QwenPaw/issues/7025

- **`#6951` scroll 压缩后重新进入会话，压缩前聊天记录不可见（CLOSED）**：压缩策略只影响模型输入，但 UI 将压缩后的 `AgentState.context` 作为聊天记录展示，导致用户丢失可见的完整 transcript。  
  https://github.com/agentscope-ai/QwenPaw/issues/6951

**🟡 中低（兼容性 / 体验问题）**

- **`#6972` Chrome 扩展 WebSocket 连接在发送 tab.create 命令后断开（CLOSED）**：browser 工具对 Chrome 扩展 JSON-RPC 协议处理存在缺陷。  
  https://github.com/agentscope-ai/QwenPaw/issues/6972

- **`#6612` QwenPaw 2.0.1 与 agentscope 2.0.4.post1 不兼容（CLOSED）**：proactive 子系统崩溃 + 工具权限死锁。已有关联 PR `#6908`（升级 agentscope 至 2.0.6）在推进中。  
  Issue: https://github.com/agentscope-ai/QwenPaw/issues/6612 | PR: https://github.com/agentscope-ai/QwenPaw/pull/6908

- **`#6197` 桌面版启动时 nvidia-smi 卡死导致应用挂起（CLOSED）**：冻结二进制在 nvidia-smi 无响应时未做超时保护，导致启动阻塞。  
  https://github.com/agentscope-ai/QwenPaw/issues/6197

- **`#6806` Creator 插件在 Windows 上无法保存任何模型配置（CLOSED）**：每次保存返回 Internal Server Error，当时处于 OPEN 状态，今日已关闭。  
  https://github.com/agentscope-ai/QwenPaw/issues/6806

- **`#2303` MiniMax provider 的 check_connection 调用不支持的 /models 端点导致 404（CLOSED）**：在 Anthropic 兼容实现中假设了完整 Anthropic API 形态，导致连接检查误判。  
  https://github.com/agentscope-ai/QwenPaw/issues/2303

---

## 6. 功能请求与路线图信号

当前新功能请求集中在以下方向，结合已有 PR 判断后续版本可能包含：

| 方向 | 代表性 Issue / PR | 状态 | 纳入判断 |
|---|---|---|---|
| 动态技能管理（加载/卸载/状态） | PR #7033；Issue #2418（skills-hub 页面） | PR OPEN | 高概率，已在实现中 |
| 会话管理增强（删除单条消息、会话拆分） | Issue #4001（OPEN）、#4436（OPEN） | Issue 讨论中 | 中概率，社区呼声高 |
| 按会话维度切换模型 | PR #5992（per-session model overrides） | OPEN（7/12 创建） | 中概率，等待评审时间较长 |
| 自动更新与桌面端体验 | Issue #2846（CLOSED）、#3464（CLOSED） | 已关闭 | 低概率，具体计划不明 |
| 内置本地 GGUF 模型运行 | Issue #6433（CLOSED） | 已关闭 | 低概率，实现成本高 |
| 后台守护 / Daemon 模式 | Issue #7010（CLOSED） | 已关闭 | 低概率，需重新评估 |
| 子代理会话分组 | PR #7035 | OPEN | 高概率，已进入实现 |

**路线图信号**：项目当前的重心在技能系统重构（动态生命周期）、Console UI 增强（子代理分组、媒体下载）、以及 Computer Use 的窗口观测能力（PR #7037）。值得关注的是 PR #6940（DataPaw app runtime，first-time contributor），这是一个全新的“数据分析工作区”方向，暂时未合并，需关注后续维护者反馈。

---

## 7. 用户反馈摘要

- **“每次都要卸载后再更新很麻烦”**（#2846、#3464）：多位用户反复抱怨 Windows 端更新体验差，且任务栏显示 Python 图标而非产品图标，影响使用观感。
- **“通过 SSH 启动命令一直卡住不返回”**（#7010）：qwenpaw app 缺少守护模式，`nohup` 也无法解决。对于服务器部署场景是刚需。
- **“Tool result 文件夹里写两份重复数据”**（#6958）：有用户直接指出 FastMCP 返回的 `content` 与 `structuredContent` 被同时写入，说明技术用户会深度检查数据文件，对数据写入逻辑有明确预期。
- **“mcp 工具总是提示 Tool not found”**（#6405）：升级 2.0 后，MCP 工具命名空间变化（`[mcp-key]__[tool_name]`）但调用端未对齐，导致用户侧无法正常使用，反应用户对版本升级的兼容性容忍度较低。
- **“自动获取模型为什么不可用”**（#3045）：用户对“自动获取模型”功能的预期与实际可用性不一致，说明这一能力在当前构建中并未达到开箱即用的标准。
- **UI 文案错别字**（#7040）：“Stopp Running” 等文案错误被用户专门开 Issue 指出，虽然是小问题，但频繁出现会损害对项目整体质量的信任。
- **正面反馈**：PR #6940 截图展示了 DataPaw 数据分析界面，有外部用户愿意以 first-time contributor 身份提交大型 PR（跨仓库 infra），说明项目对技术用户有一定吸引力，社区有向上贡献意愿。

---

## 8. 待处理积压

以下 Issue / PR 长期处于开放状态，建议维护者优先关注：

- **PR #5992 —— per-session model overrides**（7/12 创建，已开放 1 个月以上）：实现“单会话覆盖模型”，功能设计完整且 opt-in，不影响默认行为，但长期无人 review。  
  https://github.com/agentscope-ai/QwenPaw/pull/5992

- **PR #6940 —— DataPaw app runtime**（first-time contributor，ready-for-human-review）：大型新功能 PR，涉及独立运行时与数据分析工作区，已等待 3 天无 review 迹象。作为 first-time contribution 更需要维护者及时响应，避免打击社区参与意愿。  
  https://github.com/agentscope-ai/QwenPaw/pull/6940

- **Issue #4001 —— 支持在对话中手动删除单条消息**（5/2 创建，OPEN）：需求明确、场景具体（误发、隐私、整理），是高频请求但始终未排期。  
  https://github.com/agentscope-ai/QwenPaw/issues/4001

- **Issue #4436 —— 支持将部分对话转移至新会话（会话拆分）**（5/16 创建，OPEN）：长上下文管理相关需求，当前已有 scroll 压缩但用户仍需要显式控制。  
  https://github.com/agentscope-ai/QwenPaw/issues/4436

- **PR #6302 —— provider 发现、模型元数据、路由与 agent 控制统一**（7/21 创建，OPEN）：对模型系统架构级重构，改动范围大，建议与 #5992 结合评估，避免方案冲突。  
  https://github.com/agentscope-ai/QwenPaw/pull/6302

---

**项目健康度总结**：当前 CoPaw/QwenPaw 处于“高迭代 + 集中修回归”的双轨阶段。一方面新功能 PR 活跃度高，社区贡献者（含 first-time contributor）数量可观；另一方面 2.1.0 相关的会话隔离、MCP 重复写入、工具调用 404 等数据一致性/会话正确性 Bug 需要尽快修复，以避免信任流失。维护者 review 队列存在积压（#5992、#6940），建议加快响应速度。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目动态日报 — 2026-08-15

## 1. 今日速览

ZeroClaw 今日保持高活跃度：过去 24 小时共产生 33 条 Issue 动态与 50 条 PR 动态，但无新版本发布。项目当前处于高度密集的 RFC 评审期，安全架构、并发控制与平台兼容性是讨论最集中的主题，多条 p1 级 Bug 有对应修复 PR 在途。整体看，项目处于功能迭代与架构收口并行的深度开发阶段，社区讨论质量较高，维护者响应及时，但大量 RFC 仍停留在待审查状态，决策队列存在一定积压。

---

## 2. 版本发布

过去 24 小时无新版本发布。

---

## 3. 项目进展

今日无大型 PR 合并或关闭事件。但多条关键修复与新功能 PR 正在密集推进，主要信号如下：

- **修复 OpenAI 兼容接口终态响应误判**（[PR #9999](https://github.com/zeroclaw-labs/zeroclaw/pull/9999)）：将 `finish_reason: "length"` 分类为输出 token 上限终止，并拒绝不完整的非流式文本，直接呼应 [#9421](https://github.com/zeroclaw-labs/zeroclaw/issues/9421) 的 S1 级 Bug。
- **安全预算原子化**（[PR #9996](https://github.com/zeroclaw-labs/zeroclaw/pull/9996)）：提交前原子预留 action-budget 容量，修复并行调用可绕过 `max_actions_per_hour` 的竞态问题。
- **Discord 角色授权**（[PR #9970 对应 Issue](https://github.com/zeroclaw-labs/zeroclaw/issues/9970)）：计划为 Discord 频道新增 `allowed_role_ids`，实现按角色而非仅按用户 ID 授权。
- **Telegram 模型选择器**（[PR #9997](https://github.com/zeroclaw-labs/zeroclaw/pull/9997)）：新增 provider 分组、分页的 Telegram 内联键盘 `/model` 选择器，对应 [#9895](https://github.com/zeroclaw-labs/zeroclaw/issues/9895)。
- **ZeroCode 复制菜单**（[PR #9994](https://github.com/zeroclaw-labs/zeroclaw/pull/9994)）：为转录消息与代码块添加右键复制菜单。

另有大量 PR 处于 `needs-author-action` 状态，等待作者响应维护者反馈，包括 [#9137](https://github.com/zeroclaw-labs/zeroclaw/pull/9137)（插件出口策略基础）、[#9839](https://github.com/zeroclaw-labs/zeroclaw/pull/9839)（阻断不可逆破坏性命令）、[#9713](https://github.com/zeroclaw-labs/zeroclaw/pull/9713)（暴露历史截断 token 统计）等，提交者需尽快跟进。

---

## 4. 社区热点

| 排名 | Issue/PR | 评论数 | 关注点 |
|---|---|---|---|
| 1 | [#8303 RFC: Goal mode v1](https://github.com/zeroclaw-labs/zeroclaw/issues/8303) | 22 | 有界前台目标执行模式（bounded foreground Matrix work） |
| 2 | [#7155 RFC: 高风险 shell 命令确认机制](https://github.com/zeroclaw-labs/zeroclaw/issues/7155) | 20 | 每执行确认 + allow/ask/deny 命令策略 |
| 3 | [#8603 RFC: Chat Completions 兼容接口](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) | 19 | 暴露 OpenAI Chat Completions 协议，对接 Open WebUI / LobeChat / Continue.dev 等 |
| 4 | [#7141 RFC: 可插拔入站认证](https://github.com/zeroclaw-labs/zeroclaw/issues/7141) | 16 | OIDC + 规范化主体（canonical principals） |
| 5 | [#7462 Bug: Windows 74 项测试失败](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) | 15 | 仅 Linux CI 导致 Windows 回归未被发现 |

**分析**：社区讨论高度集中在三个方面——**安全控制面**（命令策略、认证、安全决策管线）、**协议兼容性**（Chat Completions 接入，将大幅拓宽客户端生态）、**跨平台工程质量**（Windows 测试失败长期未被 CI 捕获，引发信任担忧）。这些讨论对应了项目从“能用”走向“可信、可集成”的关键路径。

---

## 5. Bug 与稳定性

### S1 — 工作流受阻

- **不完整终态响应被误报为成功**（[#9421](https://github.com/zeroclaw-labs/zeroclaw/issues/9421)，p1，in-progress）：provider 可结束回合而不提供可信最终答案，但 runtime/委派层仍向上报告成功。**已有修复 PR：[#9999](https://github.com/zeroclaw-labs/zeroclaw/pull/9999)**，对 OpenAI 兼容 `finish_reason: "length"` 进行正确分类。

### S2 — 功能降级

- **高熵检测器误删 Solana 钱包地址**（[#9486](https://github.com/zeroclaw-labs/zeroclaw/issues/9486)，p2，accepted/no-stale）：Telegram 出站消息中所有钱包地址被替换为 `[REDACTED_HIGH_ENTROPY_TOKEN]`，且 `high_entropy_tokens=false` 在 channel 路径上无效。这将直接破坏 Solana MCP 服务器的核心可用性。**暂无对应 fix PR**。
- **Windows 74 项测试失败**（[#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462)，p1，accepted/no-stale)：Unix-only 命令、路径语义与控制台编码问题导致 Windows 上大量失败，CI 不覆盖。**暂无对应 fix PR**。

### S3 — 轻微问题

- **回退模型（无视觉）错误信息误导**（[#9983](https://github.com/zeroclaw-labs/zeroclaw/issues/9983)）：错误未说明是回退模型不支持视觉，而非请求本身失败。
- **cron 自定义 shell 测试 ETXTBSY 竞态**（[#9965](https://github.com/zeroclaw-labs/zeroclaw/issues/9965)，p1，accepted）：导致无关 PR 红检。**暂无对应 fix PR**。

### 其他已接受 Bug（部分含 PR）

| Issue | 问题 | 修复 PR |
|---|---|---|
| [#9919](https://github.com/zeroclaw-labs/zeroclaw/issues/9919) | builder-only 工厂静默降级 Qdrant → MarkdownMemory | 待确认 |
| [#9759](https://github.com/zeroclaw-labs/zeroclaw/issues/9759) | Quickstart 可配置重复 webhook 端口 | 待确认 |
| [#9713 对应 Issue #9619](https://github.com/zeroclaw-labs/zeroclaw/issues/9619) | 历史截断仅报结构计数、未报 token 数 | PR #9713 已就绪 |

---

## 6. 功能请求与路线图信号

### 已进入 RFC 流程的高优先级方向

- **Goal mode v1**（[#8303](https://github.com/zeroclaw-labs/zeroclaw/issues/8303)）：有界前台目标执行，跨多轮 agent turn 持久跟踪用户目标，是产品从“对话机器人”迈入“任务执行体”的关键。
- **Chat Completions 兼容层**（[#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)）：开放 OpenAI 协议后，可接入 Open WebUI、Continue.dev、Aider 等主流生态，属于高杠杆集成方向。
- **终端确认 + 命令策略**（[#7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)）：已收窄为 shell 策略契约，具备落地条件。
- **统一能力/包/配置/运行时目录**（[#9346](https://github.com/zeroclaw-labs/zeroclaw/issues/9346)）：产品级目录的收口设计，为插件体系铺路。

### 已有对应实现 / 接近实现的请求

- **Telegram 模型选择器**（[#9895](https://github.com/zeroclaw-labs/zeroclaw/issues/9895)）→ PR [#9997](https://github.com/zeroclaw-labs/zeroclaw/pull/9997) 已提交
- **Discord 按角色授权**（[#9970](https://github.com/zeroclaw-labs/zeroclaw/issues/9970)）→ 已有明确设计
- **Agent 可移植导出**（[#9986](https://github.com/zeroclaw-labs/zeroclaw/pull/9986)）→ 已提交 PR
- **Harness 评估框架**（[#9967](https://github.com/zeroclaw-labs/zeroclaw/issues/9967)）→ tracker 已建立，为 [zeroclaw eval #7065](https://github.com/zeroclaw-labs/zeroclaw/issues/7065) 铺路
- **外部不可见文本本地化**（[#9972](https://github.com/zeroclaw-labs/zeroclaw/issues/9972)）→ cleanup tracker 已建立

### 值得关注的新信号

- **运行时归属的会话与传输适配层**（[#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)，14 评论）：标准化 WebSocket、ACP、webhook 等入口到 `InboundAction`，是架构收敛的重要方向。
- **产品遥测（staged opt-in）**（[#9621](https://github.com/zeroclaw-labs/zeroclaw/issues/9621)）：争议较大，但有助项目做出数据驱动的取舍决策。

---

## 7. 用户反馈摘要

- **金融场景用户直接受害**（[#9486](https://github.com/zeroclaw-labs/zeroclaw/issues/9486)）：Solana MCP 用户无法向 Telegram 输出任何钱包地址，且关闭高熵过滤无效。原话场景：“Agent: Твой кошелёк (mainnet): [REDACTED_HIGH_ENTROPY_TOKEN]”——对 DeFi 场景属于阻断级缺陷。
- **Windows 开发者被排除在质量保障之外**（[#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462)）：中文 Windows 11 + GBK 控制台下 74 项测试失败；提交者指出 CI 仅在 Linux 运行导致问题长期潜伏。
- **移动端管理体验不佳**（[#9895](https://github.com/zeroclaw-labs/zeroclaw/issues/9895)）：多个 provider/model 路由配置下，文本命令在手机上难以使用，用户明确期望内联键盘分页选择器。
- **错误信息误导影响信任**（[#9983](https://github.com/zeroclaw-labs/zeroclaw/issues/9983)）：有视觉的 provider 回退到无视觉 provider 时，用户以为“所有请求失败”，实际只是视觉功能不支持。
- **自托管用户对可移植性有强需求**（[#9986](https://github.com/zeroclaw-labs/zeroclaw/pull/9986)）：PR 作者明确表达“Moving an agent between installs is …”的诉求，期望 `zeroclaw agents export` 支持跨安装迁移。

---

## 8. 待处理积压

### 长期未决的 RFC（创建超过 60 天，等待决策）

| Issue | 主题 | 状态 |
|---|---|---|
| [#6971](https://github.com/zeroclaw-labs/zeroclaw/issues/6971) | 安全态势与凭证边界 | `needs-maintainer-review` |
| [#6954](https://github.com/zeroclaw-labs/zeroclaw/issues/6954) | 内部发起 agent turn 的溯源/回复契约 | `needs-maintainer-review` |
| [#7141](https://github.com/zeroclaw-labs/zeroclaw/issues/7141) | 可插拔入站认证 | `accepted, in-progress` |
| [#7142](https://github.com/zeroclaw-labs/zeroclaw/issues/7142) | 安全决策管线与限制性覆盖 | `accepted` |
| [#7897](https://github.com/zeroclaw-labs/zeroclaw/issues/7897) | 安全配置热加载（免 daemon 重载） | `accepted, no-stale` |
| [#7065](https://github.com/zeroclaw-labs/zeroclaw/issues/7065) | zercolaw eval 评估框架 | `accepted, in-progress` |

### 需要维护者重点关注的 PR

| PR | 风险 | 备注 |
|---|---|---|
| [#9137](https://github.com/zeroclaw-labs/zeroclaw/pull/9137) 插件共用 egress 策略基础 | high, XL | 依赖 #9580，牵涉面广，决定外部流量策略的统一 |
| [#9580](https://github.com/zeroclaw-labs/zeroclaw/pull/9580) 加固内置 HTTP egress | high, L | 安全基座，被 #9137 依赖，长期处于 `needs-author-action` |
| [#9126](https://github.com/zeroclaw-labs/zeroclaw/pull/9126) 插件类型化实例配置验证 | high, XL | 插件体系的质量关卡，长期未合并 |
| [#9002](https://github.com/zeroclaw-labs/zeroclaw/pull/9002) viewer 断开不取消 agent turn | high, M | 影响 WebSocket 用户体验，`needs-maintainer-review` |

### 维护者决策队列

[#8692 Tracker: Maintainer decision queue for RFCs](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) 专门用于跟踪待维护者决策的 RFC 队列，当前仍有多条 5–7 月创建的 RFC 悬而未决，建议加快进度以降低长时间开放 RFC 带来的设计漂移风险。

---

**项目健康度总评**：活跃度极高，安全敏感度高，社区反馈回路畅通；主要风险在于大量重要 RFC/PR 积压等待决策，以及 Windows 平台质量保障缺位。整体处于“功能快速扩张 + 架构讨论集中爆发”的阶段，建议维护者优先处理 p1 级 Bug 对应 PR（如 #9999）和决策队列积压。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*