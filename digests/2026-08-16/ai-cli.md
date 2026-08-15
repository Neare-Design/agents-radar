# AI CLI 工具社区动态日报 2026-08-16

> 生成时间: 2026-08-15 23:14 UTC | 覆盖工具: 10 个

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [Kimi Code CLI](https://github.com/MoonshotAI/kimi-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/badlogic/pi-mono)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [DeepSeek TUI](https://github.com/Hmbown/DeepSeek-TUI)
- [Grok Build](https://github.com/xai-org/grok-build)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## 横向对比

## AI CLI 工具横向对比分析报告（2026-08-16）

### 1. 生态全景

当前 AI CLI 工具已从“单轮对话助手”全面转向 **agentic 自动化平台**，核心竞争点聚焦在会话管理、工具生态（MCP）、权限安全与成本控制。过去 24 小时，除 Grok Build 外，8 个主流工具保持高频更新，其中 OpenAI Codex、Gemini CLI、Qwen Code 等进入“日更/夜更”节奏，但版本回归与稳定性问题也随之密集暴露。社区需求的共性明显：**用户在追求更强自主性的同时，对可靠性、可观测性和安全边界提出了更高要求**。整体呈现“能力快速扩张、工程质量承压”的发展态势。

### 2. 各工具活跃度对比

> 注：Issues/PR 数量为各日报中列出的热点/重要项数量，非仓库全量数据。

| 工具 | Issues 动态 | PR 动态 | Release |
|------|------------|---------|---------|
| Claude Code | 10 个热点（含 197👍 会话续跑） | 3 条（全部列出） | 无 |
| OpenAI Codex | 10 个热点 | 10 条 | `rust-v0.148.0-alpha.19` |
| Gemini CLI | 50 条更新 | 17 条更新 | `v0.56.0-nightly.20260815` |
| GitHub Copilot CLI | 17 条更新 | 2 条 | `v1.0.81-0` |
| Kimi Code CLI | 5 条更新 | 2 条 | 无 |
| OpenCode | 10 个热点（TOP 10） | 10 个（TOP 10） | 无 |
| Pi | 10 个热点（TOP 10） | 10 个（TOP 10） | 无 |
| Qwen Code | 10 个热点（TOP 10） | 10 个（TOP 10） | `v0.21.11-nightly.20260815` |
| DeepSeek TUI | 10 个热点（TOP 10） | 10 个（TOP 10） | 无（v0.9.8 修复密集合并） |
| Grok Build | 无活动 | 无活动 | 无 |

**解读**：Gemini CLI 的 Issue/PR 更新量断层领先，且 P1 bug 密集；Codex、OpenCode、Pi、Qwen、DeepSeek TUI 均保持每日 10+ 量级活跃；Claude Code 社区反馈热度高，但官方当日无发布，且有大量 issue 被标记 stale；Kimi 活跃度相对较低。

### 3. 共同关注的功能方向

**1. 会话生命周期与上下文管理**
- **Claude Code**：`#13354` 要求会话 token 上限后无缝 Continue（197👍，所有工具中最高热度）。
- **Kimi Code**：`#1283`、`#1478` 长期呼吁跨会话记忆系统，用户引用 OpenClaw 的 SOUL/MEMORY 文件体系作为参考。
- **Pi**：`#6879` 指出 auto-compaction 超过 100% 仍不触发，多个 PR 修复压缩时机与 token 统计。
- **Codex / Qwen / Copilot**：分页历史丢失、sessionRotation 生命周期限制、`/restart` 在 worktree 会话失败等问题，均指向“长任务会话的可靠恢复与状态隔离”。

**2. 认证与 OAuth 稳定性**
- **Claude Code**：`#54443`、`#61912` 反映 OAuth 刷新失败导致多会话强制 `/login` 和 401 死循环。
- **Copilot CLI**：Atlassian MCP OAuth 在 1.0.79/1.0.80 连续回归（`#4480`/`#4490`）。
- **Gemini CLI**：改进 Vertex AI 认证错误提示，避免将普通 401 值误判为认证失败。
- **OpenCode**：订阅支付成功但工作区显示“余额不足”（`#37790`），本质是账户状态同步问题。
- 结论：认证链路的容错性和状态一致性已成为生产环境的首要事故源。

**3. MCP 生态可靠性**
- **Claude Code**：`#66084` MCP `tools/list_changed` 不刷新工具索引；macOS filesystem MCP server 不可用（`#80094`）。
- **Copilot CLI**：MCP initialize 固定 60 秒超时且无重试（`#4421`），CI 中 MCP registry 返回 403（`#4346`）。
- **Gemini CLI**：hooks 引擎新增 MCP 工具处理器支持（PR `#38705`）。
- 趋势：MCP 已成为跨工具的标准生态接口，但认证、超时、动态更新等“最后一公里”问题远未解决。

**4. 权限模型与安全边界**
- **Claude Code**：`--permission-mode dontAsk` 无条件拒绝 Write/Edit，与文档冲突（`#74567`）；内置安全过滤误伤合法开发任务。
- **Gemini CLI**：`#22093` 用户禁用 agents mode 后 sub-agent 仍自动运行；`#22672` 模型倾向使用 `git reset --force` 等危险命令。
- **OpenCode**：`permission.ask` 配置运行时未生效（`#32787`），Agent 绕过确认直接执行工具。
- **Pi**：Windows 上 bash 工具可无确认执行 `taskkill /F /IM node.exe` 杀死宿主进程（`#8170`）。
- **Qwen Code**：autofix PAT 任务与不受信任分支共用 runner，需要架构级隔离（`#9089`）。
- 核心矛盾：工具能力越强，用户对“可预测的权限边界”和“可配置的安全策略”需求越紧迫。

**5. 成本与配额透明度**
- **Codex**：`/status` 命令只显示模糊用量，用户要求展示计划类型、余额、重置时间（`#15281`，22👍）。
- **Kimi Code**：`#2604` 用户自建 JSONL 记账，发现每周配额疑似缩减 3–5 倍；`#2603` 指出压缩策略应基于 token 预算而非模型窗口。
- **OpenCode**：`#24879` 要求新增固定 Pro 层级；`#37790` 订阅支付后状态不同步。
- **Pi**：修正 `tokens.total` 误含缓存 token 导致的统计失真（PR `#8165`）。
- **Qwen Code**：`#9230` 前缀缓存命中率约 0%，`enableCacheSharing` 默认关闭造成推理成本浪费。
- 信号：开发者已开始用“计量脚本”核验服务商行为，配额透明性直接影响产品信任。

### 4. 差异化定位分析

| 工具 | 功能侧重 | 目标用户 | 技术路线 / 特色 |
|------|----------|----------|-----------------|
| **Claude Code** | 深度 IDE 集成、安全策略、插件生态 | 企业级开发者、Claude 生态重度用户 | 绑定 VS Code 和 Desktop，强调可管理性与企业合规；当前迭代节奏偏保守 |
| **OpenAI Codex** | 代理型 CLI、TUI + 桌面端 | OpenAI API 用户、追求自动化的开发者 | Rust 重写、Electron 桌面端；快速发版但 Windows 性能回归显著 |
| **Gemini CLI** | Sub-agent 架构、评估体系（Evals） | Google 生态开发者、需要可审计 agent 行为的团队 | 强调行为可评估、可追溯；安全修复积极（SSRF、Node EOL） |
| **GitHub Copilot CLI** | GitHub 原生集成、MCP、Codespaces | GitHub 重度用户、CI/云端开发 | 与 GitHub 产物深度绑定，更新节奏稳定；MCP 兼容性是当前短板 |
| **Kimi Code** | 多模型支持、订阅制 | Moonshot 模型用户、自建推理后端开发者 | 轻量 CLI，但记忆系统缺失和配额透明度问题阻碍大型项目落地 |
| **OpenCode** | 多模型聚合、TUI/Docker 工作区 | 追求模型选择自由的开发者、开源社区 | 插件系统 + 云端订阅双轨；社区对免费策略与计费公平性敏感 |
| **Pi** | 上下文压缩、扩展系统、TUI 深度定制 | 极客/高级用户、自托管模型玩家 | 社区驱动，架构开放；注重长会话稳定性与终端渲染细节 |
| **Qwen Code** | WebShell、自动化 Code Review、多渠道 | 通义模型用户、企业 CI/CD 团队 | 与阿里云生态绑定；`/review` 工具链和 CI 自愈是差异化亮点 |
| **DeepSeek TUI** | 轻量 TUI、第三方模型模板、i18n | DeepSeek 用户、macOS 终端爱好者 | Rust/CodeWhale 架构，社区贡献活跃；产品成熟度仍在早期 |
| **Grok Build** | —（当前无动态） | — | 需观察后续定位，暂无有效对比数据 |

**关键差异总结**：Claude Code 走“企业安全合规”路线，Codex 走“高频迭代代理平台”路线，Gemini CLI 强调“可评估的 agent 质量”，Copilot CLI 绑定 GitHub 生态，而 Qwen、Kimi、DeepSeek 则依托模型厂商优势，在垂直场景（中文、IDE、TUI）建立差异化。Pi 和 OpenCode 则是典型的社区驱动型通用工具。

### 5. 社区热度与成熟度

**第一梯队：高活跃、快速迭代**
- **Gemini CLI**：50 Issues + 17 PR 为当日之最，P1 bug 集中于 sub-agent 可靠性，nightly 发布如期推进，处于功能与质量的双高压力期。
- **OpenAI Codex**：10 PR/日 + alpha 版本发布，官方修复动作密集，但 26.810.x 系列 Windows 性能回归说明快速发版的“副作用”明显。
- **Qwen Code**：同样保持“日更 + 快速 fix”节奏，围绕 `/review` 工具链的多个 issue 在 24 小时内就有对应 PR，响应速度突出。
- **OpenCode / Pi / DeepSeek TUI**：均为 10+ Issue、10+ PR 的活跃状态，且社区贡献者（非官方）参与度高，如 Pi 的上下文压缩修复、DeepSeek TUI 的 macOS 乱码修复。

**第二梯队：高热度、修复节奏偏慢**
- **Claude Code**：社区基数大、需求声音强（`#13354` 197👍），但当日无版本发布，多个认证/会话类 issue 被关闭或标记 stale，用户对官方响应速度存在焦虑。

**第三梯队：稳定迭代 / 低活跃**
- **Copilot CLI**：17 Issue、2 PR，相对平静，但有 MCP OAuth 反复回归和 Codespaces 更新链路问题，属于“存量问题持续发酵”。
- **Kimi Code**：5 Issue、2 PR，热度集中在记忆系统和配额质疑两个长期/突发议题，总体活跃度偏低。
- **Grok Build**：无活动，尚无法评估。

### 6. 值得关注的趋势信号

**1. “可靠性”正在取代“能力”成为竞争焦点**
- 典型证据：Gemini 子代理误报 GOAL 成功、Claude Code 模型编造对话轮次、Pi 压缩超过 100% 才触发、Qwen `/review` 管道因内部 schema 不匹配失败。
- 对开发者：在引入 AI CLI 进入自动化流程前，应设计独立的审计与验证环节，不能盲信 agent 的“成功”返回。

**2. 安全与权限从“外围配置”升级为“核心架构问题”**
- SSRF 修复（Gemini）、危险命令未确认执行（Pi）、权限规则绕过（OpenCode）、PAT 与不确定代码共享 runner（Qwen）——这些不是小 bug，而是 agent 滥用风险的真实案例。
- 对开发者：评估工具时应考察其权限默认值、沙箱能力、危险操作确认机制，而非仅看模型效果。

**3. 成本可观测性成为工具链的“标准配置”需求**
- 配额缩减未公告（Kimi）、`/status` 信息不完整（Codex）、订阅支付后状态不同步（OpenCode）、缓存命中率不可见（Qwen）——用户已用自建脚本精确计量，倒逼工具方提供透明数据。
- 对开发者：在预算敏感项目中，优先选择提供余额、缓存命中、token 明细等可编程接口的工具。

**4. 桌面/终端体验的跨平台稳定性仍是最大短板**
- Windows 鼠标卡顿与 CPU 忙循环（Codex）、OOM 崩溃（Copilot）、输入法失效（Qwen）、SSE UTF-8 乱码（DeepSeek）、焦点抢占（Claude Code）——大量基础体验问题仍未根治。
- 对开发者：如果所在团队使用 Windows/Linux 混合环境，需为工具切换预留兼容性测试时间。

**5. MCP 生态进入“整合期”，但生产级可靠性不足**
- 工具索引不刷新、OAuth 回归、初始化超时、403 权限问题并存。MCP 作为统一协议已获共识，但各工具实现质量参差。
- 对开发者：在关键链路中应避免单一 MCP 服务器单点故障，并关注工具对超时、重试、动态发现的支撑程度。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

⚠️ Skills 摘要生成失败。

---

# Claude Code 社区动态日报 — 2026-08-16

## 今日速览

过去 24 小时无新版本发布，社区讨论焦点集中在长期未决的**会话限制续跑**功能（#13354，197 👍）与 **OAuth/认证稳定性**问题。#86870 是当天唯一值得关注的新 PR，尝试修复安全策略在授权研究场景下的误报阻断问题。此外，多起模型安全过滤误报的 issue 被集中关闭，暴露了内置策略对合法开发任务的干扰风险。

## 社区热点 Issues

以下 10 个 Issue 最值得关注：

**1. [enhancement] 会话达到上限后支持 Continue（#13354）**  
78 条评论、197 👍 的超级热门请求。用户希望在会话 token 上限耗尽后能无缝续接上下文，而非被迫开新会话。社区反应强烈，属 CLI 工作流痛点 TOP 1。  
https://github.com/anthropics/claude-code/issues/13354

**2. [bug] Windows 版 Claude Desktop 反复崩溃，需 Repair 修复（#85199）**  
23 条评论。MSIX/Store 安装版本会重复触发标题栏渲染异常导致空白屏，用户只能通过“高级选项 → 修复”恢复，属于高频阻断问题。  
https://github.com/anthropics/claude-code/issues/85199

**3. [bug] OAuth 刷新返回 400，多会话被强制 /login（#54443，已关闭）**  
15 条评论。服务器端提前返回 401 后，本地 refresh 请求被拒，导致并发会话同时掉线。该 issue 已关闭但引发的认证可靠性讨论仍在继续。  
https://github.com/anthropics/claude-code/issues/54443

**4. [bug] MCP tools/list_changed 不刷新工具索引（#66084）**  
8 条评论。deferred-tool / ToolSearch 索引在交互式会话中不随工具变更更新，被标记为已复现（2.1.165 仍在）。对 MCP 重度用户影响明显。  
https://github.com/anthropics/claude-code/issues/66084

**5. [bug] AskUserQuestion 对话框抢占输入焦点（#45374，已关闭）**  
7 条评论。VS Code 扩展中弹窗会抢走正在输入消息的键盘焦点，击键被误判为选项快捷键。IDE 集成体验的高频痛点。  
https://github.com/anthropics/claude-code/issues/45374

**6. [bug] OAuth 刷新遇瞬态 5xx 时损坏凭证，形成 401 死循环（#61912，已关闭）**  
7 条评论。与 #54443 同属认证路径缺陷——上游 5xx 未做容错，导致跨会话持续性 401。强烈暗示 OAuth client 状态机需加固。  
https://github.com/anthropics/claude-code/issues/61912

**7. [bug] macOS 版 filesystem MCP server 完全不可用（#80094）**  
6 条评论。新老两代包均受影响：新模式 schema 从未下发、旧模式注册即被丢弃。文件系统类 MCP 工具在 Desktop 上形同虚设。  
https://github.com/anthropics/claude-code/issues/80094

**8. [bug] VS Code 多会话输入框焦点 ping-pong（#71809，已关闭）**  
6 条评论。同时打开多个会话 Tab 时，输入框焦点在会话间快速跳动，导致无法正常输入。IDE 多任务场景的严重体验问题。  
https://github.com/anthropics/claude-code/issues/71809

**9. [bug] 模型编造整个对话轮次（#70148，已关闭）**  
5 条评论。高延迟下中断工具调用后，模型虚构了用户消息与工具结果。这一行为若未被发现将污染整个对话历史，属高危害置信度问题。  
https://github.com/anthropics/claude-code/issues/70148

**10. [bug] --permission-mode dontAsk 无条件拒绝 Write/Edit（#74567）**  
3 条评论但价值极高。dontAsk 模式无视 permissions.allow 规则，导致 headless 场景下根本无法写入文件，与文档描述直接冲突。  
https://github.com/anthropics/claude-code/issues/74567

## 重要 PR 进展

当日 PR 数量较少（共 3 条），以下为全部内容：

**1. fix: 安全研究场景下阻止误报 CVP 状态变更（#86870，OPEN）**  
新增任务上下文检查机制（`is_authorized_lab()`），在安全引导 hook 的 `cap_diff_for_prompt()` 中识别 CVS 状态与教学实验环境，避免经过授权的安全研究被错误标记。对安全过滤器的可配置性有积极意义。  
https://github.com/anthropics/claude-code/pull/86870

**2. 在项目级启用 frontend-design 插件（#84600，CLOSED）**  
注册官方 marketplace 并在 `.claude/settings.json` 中启用 frontend-design 技能，实现仓库级自动加载。是实践 Claude Code 插件按项目分发的简单参考用例。  
https://github.com/anthropics/claude-code/pull/84600

**3. Claude：自动盘点物资库存（#82981，OPEN）**  
标题与描述为西班牙语，内容不完整。可能利用 Claude Code 实现 ERP/进销存类自动化工作流，但缺乏技术细节，不建议深入跟进。  
https://github.com/anthropics/claude-code/pull/82981

## 功能需求趋势

综合所有 Issues，社区当前最关注的功能方向为：

- **会话生命周期管理**：#13354 高居热度榜首，期望会话上限后能继续而非重启，“上下文延续”被视为自动化任务的基础能力。
- **认证与账户稳定性**：#54443、#61912、#79808 三箭齐发，OAuth 刷新路径的容错性成为高频词，用户对掉线后强制 /login 的流程非常不满。
- **IDE 集成完善度**：#45374、#71809、#57691 显示 VS Code 端的焦点管理、滚动约束和会话 Tab 复用仍是体验短板。
- **MCP 生态可靠性**：#66084、#80094 说明工具变更通知与 filesystem server 的基础质量还未达标，影响 MCP 在 Desktop/CLI 的推广。
- **权限模型的可预测性**：#74567 的 dontAsk 行为与文档不符、#58614 的 Windows 8.3 短文件名绕过允许规则，表明权限系统需要更可调试的设计。
- **模型安全滤镜的降噪**：sworrl 用户批量提交的 cyber/AUP 误报系列（#72074–#72105）虽已关闭，但揭示安全策略会误伤固件逆向、隐私保护等合法开发操作，社区呼吁提供更细粒度的豁免配置。

## 开发者关注点

- **认证掉线循环是最大生产事故源**：多个 Issue 指向 OAuth 刷新失败后的持续性 401，且修复状态不明（相关 issue 被标记 stale）。建议受影响用户监控官方版本更新。
- **“输入焦点被抢”类问题反复出现**：无论是 AskUserQuestion 弹窗还是多会话焦点 ping-pong，都打断了开发者“心流”状态，IDE 集成团队需优先处理。
- **模型编造消息属于高危信号**：#70148 暗示在延迟波动下存在上下文完整性问题，建议在关键任务中使用外部审计或日志记录。
- **安全过滤误报形成新噪声**：被误伤的固件分析、无人机 SDK 集成等案例说明默认策略过于激进，开发者需要在安全性与可用性之间期待一个可配置的中间层。
- **无新版本发布与 issue 大范围 stale**：大量 2026 年 4–6 月的 bug 被批量关闭/标记过期，社区可能对回复节奏产生焦虑，值得关注官方后续的版本节奏声明。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报 — 2026-08-16

## 今日速览

过去 24 小时 Codex 社区的核心话题仍是 Windows 桌面客户端性能问题——大量新 Issue 报告系统级鼠标卡顿、空闲 CPU 忙循环和崩溃转储无限增长，且集中在 26.810.x 新版本上。与此同时，官方在修复层面动作频繁：连续合并了存储诊断、分页历史、MCP 钩子支持等多个 PR，并发布 `rust-v0.148.0-alpha.19` 版本。

## 📦 版本发布

**rust-v0.148.0-alpha.19**  
- 发布了 `0.148.0-alpha.19`。该版本为 alpha 通道的最新快照，官方尚未提供详细变更说明，社区可重点关注后续更新日志。  
🔗 [github.com/openai/codex/releases](https://github.com/openai/codex/releases)

---

## 🔥 社区热点 Issues（10 条）

### 1. #20214 — Codex App 在 Windows 11 Pro 上频繁卡顿/掉帧
**状态**：Open | **评论** 104 | **👍** 85 | **标签**：bug, windows-os, app, performance  
这是当前社区反馈最激烈的问题。用户报告即便 CPU（Ryzen 5 5600）和 32GB 内存资源充足，Codex 桌面应用在 Windows 11 上依然频繁无响应、动画掉帧，严重影响日常使用。85 个赞和 104 条评论表明该问题覆盖面极广，官方需优先定位渲染线程或 Electron 主进程的性能瓶颈。  
🔗 [Issue #20214](https://github.com/openai/codex/issues/20214)

### 2. #3550 — 将 Codex 聊天范围限定到 VS Code 项目/工作区
**状态**：Closed | **评论** 34 | **👍** 79 | **标签**：enhancement, extension  
虽已关闭，但该请求获得 79 个 👍，是社区强烈的 IDE 集成诉求。用户希望 Recent Tasks 列表按工作区隔离，避免跨项目会话混杂。关闭状态可能与官方已规划或部分实现有关，值得跟踪后续 VS Code 扩展更新。  
🔗 [Issue #3550](https://github.com/openai/codex/issues/3550)

### 3. #38546 — [Windows] 非管理员运行时引发系统级鼠标卡顿
**状态**：Open | **评论** 25 | **👍** 10 | **标签**：bug, windows-os, app, performance  
8 月 14 日新提交的 Issue，报告 ChatGPT/Codex 桌面应用在未提权运行时导致整个操作系统的鼠标指针周期性停顿。与 #20214、#38750 等问题高度相似，可能指向同一个 Electron 主进程忙循环根因。  
🔗 [Issue #38546](https://github.com/openai/codex/issues/38546)

### 4. #28109 — 大型 sessions 目录导致 Windows 输入短暂冻结
**状态**：Closed | **评论** 22 | **👍** 14 | **标签**：bug, windows-os, app, session, performance  
应用打开或运行时会间歇性出现 1-2 秒的系统输入停止。用户定位到 sessions 目录过大的场景，暗示启动时扫描/序列化大量会话文件阻塞了主线程。问题已关闭，推测已有修复或缓解方案。  
🔗 [Issue #28109](https://github.com/openai/codex/issues/28109)

### 5. #25921 — Crashpad 待处理转储无限制增长，每天超 5GB
**状态**：Open | **评论** 17 | **👍** 8 | **标签**：bug, app, performance  
macOS 用户观察到 `~/Library/Application Support/com.openai.codex/web/Crashpad/pending` 目录以每日 5GB 的速度膨胀（峰值 54,504 个文件）。该问题既浪费磁盘，也暗示存在持续崩溃/重复转储的底层缺陷，与 Windows 端的磁盘膨胀问题遥相呼应。  
🔗 [Issue #25921](https://github.com/openai/codex/issues/25921)

### 6. #38547 — Windows 26.810.4967 空闲时主进程 CPU 忙循环
**状态**：Closed | **评论** 16 | **👍** 7 | **标签**：bug, windows-os, app, app-server, performance  
从 26.803.10989 升级到 26.810.4967 后，应用在完全空闲时 Electron 主进程持续占用 CPU。该版本回归与 #38716 等新报告版本一致，指向 26.810.x 系列的编程回归，现已关闭，可能已有热修或回滚方案。  
🔗 [Issue #38547](https://github.com/openai/codex/issues/38547)

### 7. #38750 — [Windows] 空闲时系统卡顿，退出应用立即恢复
**状态**：Open | **评论** 9 | **标签**：bug, windows-os, app, performance  
8 月 15 日新增报告，版本 26.810.50856。即使没有任何 Codex 任务运行，仅保持应用开启就会拖慢整个 Windows 交互。这是 26.810 系列性能问题的又一个独立复现，社区已出现多条“退出即恢复”的验证反馈。  
🔗 [Issue #38750](https://github.com/openai/codex/issues/38750)

### 8. #15281 — 在 CLI /status 命令中展示完整用量/配额数据
**状态**：Open | **评论** 8 | **👍** 22 | **标签**：enhancement, TUI, rate-limits  
用户希望 `/status` 不再只显示模型名和模糊的用量百分比，而是展示计划类型、余额、重置时间等完整信息。22 个 👍 说明这是 CLI 用户的普遍需求，与 #24080、#19555 形成同一需求簇。  
🔗 [Issue #15281](https://github.com/openai/codex/issues/15281)

### 9. #38716 — [Windows][26.810.6296] Electron 主进程忙循环导致鼠标卡顿
**状态**：Open | **评论** 7 | **👍** 3 | **标签**：bug, windows-os, app, performance  
该报告将问题精确定位到 Electron 主进程，并再次验证“完全退出应用即恢复”的特征。26.810.6296 是最新触发版本，说明前述问题在 26.810.x 系列中持续存在，尚未彻底修复。  
🔗 [Issue #38716](https://github.com/openai/codex/issues/38716)

### 10. #35470 — Codex 将同一图片复制 150,000 次，消耗 400 GiB 磁盘
**状态**：Open | **评论** 5 | **标签**：bug, windows-os, CLI, context, subagent, session, performance  
极端但最能体现“磁盘失控”的案例：CLI 0.145.0 在 Windows 上执行任务时反复复制项目图片文件，最终产生约 40 万次文件操作、占用约 400 GiB 空间。这直接揭示了会话上下文打包或临时文件管理存在严重的去重缺陷。  
🔗 [Issue #35470](https://github.com/openai/codex/issues/35470)

---

## 🛠️ 重要 PR 进展（10 条）

### 1. #38795 — 为 `codex doctor` 增加存储诊断
**状态**：Closed | **标签**：storage, diagnostics  
新增对 `CODEX_HOME` 和当前 worktree 可用空间的检查（低于 5 GiB 告警、低于 1 GiB 报错）；Windows 上额外检测 worktree 是否位于可信 Dev Drive 并提供修复建议。直接回应当前社区的多起磁盘膨胀问题。  
🔗 [PR #38795](https://github.com/openai/codex/pull/38795)

### 2. #38774 — 持久 exec 线程改用分页历史
**状态**：Closed | **标签**：exec, session, pagination  
`codex exec` 启动持久线程时请求分页历史，临时线程保持原有逻辑；当线程存储不支持分页时自动回退旧版历史加载。该 PR 直接修复 Issue #35746 中“分页历史丢失 rollout 记录”的问题，并相应增加分页恢复的测试覆盖。  
🔗 [PR #38774](https://github.com/openai/codex/pull/38774)

### 3. #38743 — 将 TUI 应用目录状态限定到当前上下文
**状态**：Closed | **标签**：TUI, context, state  
修复应用目录数据和进行中的请求可能跨账户、工作区、线程存活的问题。现在切换上下文时会自动失效缓存的应用数据、关闭应用选择器、丢弃过期的应用请求，避免在当前 TUI 会话中显示陈旧应用。  
🔗 [PR #38743](https://github.com/openai/codex/pull/38743)

### 4. #38785 — 保持活动回合中的模型设置稳定
**状态**：Closed | **标签**：model, turn, settings  
线程设置可能在一次回合进行中（包括采样请求之间）被修改，导致模型配置中途变化。该 PR 让模型配置更新推迟到下一回合生效，确保单次回合内采样一致性。  
🔗 [PR #38785](https://github.com/openai/codex/pull/38785)

### 5. #38788 — TUI 启动时显示恢复/分叉状态
**状态**：Closed | **标签**：TUI, UX  
在 TUI 启动加载会话时，于编辑器上方显示“Resuming session…”或“Forking session…”的灰色状态提示；会话解析完成后自动更新或清除，并保持编辑器高度不跳动。改善了启动阶段的可预期性和反馈。  
🔗 [PR #38788](https://github.com/openai/codex/pull/38788)

### 6. #38705 — 为 hooks 引擎增加 MCP 工具处理器支持
**状态**：Closed | **标签**：hooks, MCP  
支持发现同步 `mcp_tool` 钩子处理器，通过注入的 executor 调用对应 MCP 服务器和工具；同时支持嵌套的钩子事件占位符展开，并在调用完成后将工具输出回灌到钩子运行时。扩展了 Codex hooks 的生态接入能力。  
🔗 [PR #38705](https://github.com/openai/codex/pull/38705)

### 7. #38701 — 权限请求统一走 Guardian 审批
**状态**：Closed | **标签**：permissions, guardian  
将 `request_permissions` 调用建模为共享审批动作，所有权限申请统一转换为 Guardian 审批请求。自动评审期间保留回合取消能力，并保持现有 TUI 交互链路不变。这是权限系统的一次架构收敛。  
🔗 [PR #38701](https://github.com/openai/codex/pull/38701)

### 8. #38806 — 为 code-mode gRPC 监听器增加健康端点
**状态**：Closed | **标签**：infrastructure, grpc  
新增 `GET /healthz` 返回 200 OK，支持 HTTP/1.1 与 HTTP/2；其他请求仍强制 HTTP/2，避免 gRPC 方法被 HTTP/1.1 暴露。同时增加了 TCP 监听器的集成测试覆盖。  
🔗 [PR #38806](https://github.com/openai/codex/pull/38806)

### 9. #38690 — 在 exec-server 中继间传播请求追踪上下文
**状态**：Closed | **标签**：observability, tracing  
为 relay 帧新增 W3C `traceparent` 与 `tracestate` 字段，从 JSON-RPC 请求复制追踪上下文到数据帧，并支持跨多个 Noise 记录分片的加密请求正确携带链路信息。便于分布式排查跨组件调用问题。  
🔗 [PR #38690](https://github.com/openai/codex/pull/38690)

### 10. #38704 — 修正粘贴文本中的 CRLF 行尾规范化
**状态**：Closed | **标签**：TUI, bugfix, windows  
修复 Windows 上向 TUI 粘贴文本时 CRLF 被错误拆成两行的 bug。现在先归一化 CRLF 对，再处理残留的裸回车，保证每行粘贴文本只产生一个换行。  
🔗 [PR #38704](https://github.com/openai/codex/pull/38704)

---

## 📊 功能需求趋势

1. **Windows 桌面客户端稳定性与性能（压倒性热点）**  
   超过一半的热门 Issue 与 Windows 端卡顿、CPU 忙循环、鼠标系统级停滞相关。用户对 26.810.x 系列的 Electron 主进程性能回归非常敏感，且大量报告相互印证为共性问题，推测官方正在统一排查底层渲染或 IPC 调度模块。

2. **本地存储与磁盘空间管理**  
   Crashpad 转储无限制增长（#25921）、会话 rollout 文件膨胀（#34337）、子代理 JSONL 历史无限持久化（#30779）、异常文件复制（#35470）共同构成了“磁盘空间失控”问题群。社区需要更细粒度的存储配额、清理策略与诊断工具（如 #38795 存储诊断 PR 的落地）。

3. **用量与速率限制可观测性**  
   `#15281`、`#24080`、`#19555`、`#20310` 四条 Issue 从不同角度呼吁在 CLI/TUI 中展示计划余额、重置时间、信用额度等完整用量数据，形成明显的功能需求簇。类似 Claude Code 的终端用量展示已成为社区对标对象。

4. **工作区/上下文隔离**  
   VS Code 聊天按项目隔离（#3550）、TUI 应用目录状态按上下文隔离（#38743）等诉求表明，用户希望 Codex 更严格地区分不同项目的会话和状态，减少跨项目串扰。

5. **MCP 生态扩展**  
   除性能问题外，MCP 相关讨论也在增加：Windows 上 MCP 套件重复累积（#34614）、hooks 引擎支持 MCP 工具处理器（#38705），说明开发者正在把 Codex 接入更多自定义工具链，同时需要更可靠的 MCP 生命周期管理。

---

## 🧑‍💻 开发者关注点

- **“退出即恢复”成为 Windows 性能问题的铁证**：多个 Issue（#38546、#38750、#38716）都描述“完全退出 Codex 后系统立即恢复流畅”，用户已通过这一手段把问题锁定在应用本体，而非系统环境。高频复现路径急需官方定位 Electron 主进程的空闲忙循环。

- **磁盘膨胀是跨平台顽疾**：macOS 的 Crashpad 转储（#25921）与 Windows 的 400GiB 图片复制（#35470）性质不同但结果相似——本地磁盘被 Codex 以“不可控”的方式消耗。用户期待官方提供存储占用视图与一键清理机制，而非依赖手动删除目录。

- **新版本更新即“踩雷”**：26.810.x 系列连续多个版本（4967、50856、6296）触发同类回归，开发者对快速迭代但缺少充分回归测试表示不满，希望官方在发布前覆盖“空闲主进程 CPU 占用”“大型会话目录加载”等场景。

- **CLI 用户希望获得与定价相关的透明信息**：多位用户将 Codex CLI 与 Claude Code 对比，认为缺少剩余额度、重置时间、计划类型等关键信息让成本控制变得困难。这已成为 CLI 工具链的“标准配置”需求。

- **会话历史持久化需要更可靠的设计**：分页历史丢失记录（#35746）、rollout 文件未索引（#31433）、子代理历史无限膨胀（#30779）等问题集中在会话层，开发者期待官方梳理出一条“写入—索引—清理”的完整链路，而不是靠用户定期手动删除 `.codex` 目录来维持磁盘空间。

---

*本日报基于 github.com/openai/codex 公开数据自动生成，所有 Issue/PR 链接均指向 GitHub 原始页面。数据观测窗口：2026-08-15 至 2026-08-16。*

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

### 今日速览

过去 24 小时，Gemini CLI 社区共更新 50 条 Issue 和 17 条 PR。核心矛盾集中在 sub-agent 的可靠性上：状态误报、无限挂起、未授权自动执行等问题集中爆发。与此同时，安全修复（SSRF、Node 20 EOL）和评估体系扩展是 PR 侧的主旋律，新 nightly v0.56.0 如期发布。

---

### 版本发布

**v0.56.0-nightly.20260815.g2a87e7be1**

仅包含一项变更：由 SSR Agent 提交的测试基建修复——将 `a2a-server` 测试中对 `process.env` 的直接操作迁移至 Vitest 的 `vi.stubEnv`，以符合项目测试规范。

- 对应 PR：[#28811](https://github.com/google-gemini/gemini-cli/pull/28811)
- 完整变更：[Compare v0.56.0-nightly.20260814...v0.56.0](https://github.com/google-gemini/gemini-cli/compare/v0.56.0-nightly.20260814.gc0d192452...v0.56.0)

---

### 社区热点 Issues

以下 10 个 Issue 在过去 24 小时内讨论最集中或影响面最大：

**#22323 子代理在 MAX_TURNS 后被误报为 GOAL 成功** [P1/Bug]  
链接：[Issue #22323](https://github.com/google-gemini/gemini-cli/issues/22323)  
`codebase_investigator` 子代理实际已达最大轮次限制、未做任何分析，却在恢复后返回 `status: "success"` 和 `Termination Reason: "GOAL"`。这会让自动化流程误判任务成功，掩盖真实中断。12 条评论，是目前讨论度最高的 agent 可靠性 bug。

**#21409 Generalist 子代理无限挂起** [P1/Bug]  
链接：[Issue #21409](https://github.com/google-gemini/gemini-cli/issues/21409)  
只要委托给 generalist agent，哪怕只是创建文件夹这类简单操作，都会无限挂起，有用户最长等待了 1 小时。8 条评论、8 个 👍，社区受影响面很广。

**#25166 Shell 命令执行完成后仍显示 “Waiting input”** [P1/Bug]  
链接：[Issue #25166](https://github.com/google-gemini/gemini-cli/issues/25166)  
简单命令已执行完毕，但 CLI 仍卡在 “Awaiting user input”。4 条评论、3 个 👍，直接中断日常交互流，反馈精确且可复现。

**#22093 自 v0.33.0 起子代理未被授权即自动运行** [P2/Bug]  
链接：[Issue #22093](https://github.com/google-gemini/gemini-cli/issues/22093)  
用户已在所有配置中禁用 agents mode，但 upgrade 后 generalist 等 subagent 仍然被自动调用。这触及权限边界和用户预期，是信任度方面的严重问题。

**#26525 Auto Memory 缺少确定性脱敏机制** [P2/Security]  
链接：[Issue #26525](https://github.com/google-gemini/gemini-cli/issues/26525)  
Auto Memory 会读取本地 transcript 并发送给模型，但脱敏发生在内容进入模型上下文之后；同时服务日志可能记录已有 skill 内容。社区关注隐私泄露风险，要求确定性 redaction。

**#22745 评估 AST 感知文件读取/搜索/映射的价值** [P2/Feature]  
链接：[Issue #22745](https://github.com/google-gemini/gemini-cli/issues/22745)  
EPIC 跟踪：利用 AST 感知工具精确定位方法边界、减少错误读取和 token 消耗。7 条评论，代表社区对代码理解效率的前瞻性诉求。

**#21968 Gemini 不会主动使用技能和子代理** [P2/Bug]  
链接：[Issue #21968](https://github.com/google-gemini/gemini-cli/issues/21968)  
用户反馈即使已经配置了 gradle、git 等自定义 skills，模型仍需被明确指示才使用。6 条评论，直接关系 agent 自主性与自动化效率。

**#19873 零依赖 OS 沙箱 + 执行后意图路由** [P2/Enhancement]  
链接：[Issue #19873](https://github.com/google-gemini/gemini-cli/issues/19873)  
Gemini 3 模型天然擅长以 bash 方式工作，但直接放开有安全隐患。提议通过轻量沙箱和执行后意图路由，在安全与效率之间取得平衡。8 条评论，体现社区对 “能力释放 vs 安全限制” 的深层思考。

**#24246 工具数超过 128 后触发 400 错误** [P2/Bug]  
链接：[Issue #24246](https://github.com/google-gemini/gemini-cli/issues/24246)  
当可用工具数量超出上限时出现 400 错误，用户期待 agent 能按场景智能裁剪工具范围。这是工具生态扩大后的必然问题。

**#22672 代理应停止/劝阻破坏性行为** [P2/Enhancement]  
链接：[Issue #22672](https://github.com/google-gemini/gemini-cli/issues/22672)  
模型在复杂 git 操作或数据库维护中，偶尔使用 `git reset`、`--force` 等危险命令，而存在更安全的替代方案。3 条评论、1 个 👍，属于信任与安全基座问题。

---

### 重要 PR 进展

以下 10 个 PR 在过去 24 小时内有实质进展，覆盖核心修复、安全加固和评估扩展：

**#28828 静默替换预览模型时增加警告** [P1/Core]  
链接：[PR #28828](https://github.com/google-gemini/gemini-cli/pull/28828)  
当用户请求 `gemini-3.1-pro-preview` 但账号无预览权限时，Config 会静默降级为 `auto-gemini-2.5`，无任何提示。此 PR 增加显式警告。

**#28815 修复 #22323：保留子代理恢复时的原始终止原因** [P1/Agent]  
链接：[PR #28815](https://github.com/google-gemini/gemini-cli/pull/28815)  
解决子代理在 MAX_TURNS/TIMEOUT 后，因 `complete_task` 调用而把终止原因覆盖为 GOAL 的问题，确保中断原因不被隐藏。

**#28812 为 TUI 初始化添加执行超时** [P1/Core]  
链接：[PR #28812](https://github.com/google-gemini/gemini-cli/pull/28812)  
修复裸 Linux 终端下 TUI 无限卡在 “Initializing...” 的问题（`getProcessInfo()` 依赖 `ps` 命令，无超时保护）。

**#28725 修复 web-fetch 的 SSRF 漏洞** [P2/Security]  
链接：[PR #28725](https://github.com/google-gemini/gemini-cli/pull/28725)  
CVSS 8.6 严重漏洞：恶意域名可绕过 DNS 保护指向 `169.254.169.254` 等内网地址。修复 DNS 解析绕过路径。

**#28726 沙箱 Dockerfile 升级至 node:22-slim** [P1/Security]  
链接：[PR #28726](https://github.com/google-gemini/gemini-cli/pull/28726)  
Node 20 已 EOL，不再获得 CVE 修复。将 Sandbox 及相关 Cloud Run 镜像统一升级到 Node 22。

**#28827 避免将包含 “401” 的普通值误判为认证失败** [P2/Core]  
链接：[PR #28827](https://github.com/google-gemini/gemini-cli/pull/28827)  
`isAuthenticationError` 会把端口号、退出码等包含 “401” 的值误判为认证失败，导致误报。此 PR 增加上下文识别。

**#28679 改进 Vertex AI 认证错误提示** [P2/Auth]  
链接：[PR #28679](https://github.com/google-gemini/gemini-cli/pull/28679)  
当用户以 vertex-ai 认证类型提供的是标准 Gemini API Key 时，给出更明确的配置引导，而非笼统的失败提示。

**#28823 新增任务依赖/错误恢复行为评估** [Evals]  
链接：[PR #28823](https://github.com/google-gemini/gemini-cli/pull/28823)  
为 tracker 的依赖添加、可视化、文件 404 路径恢复和 shell 失败重试增加 behavioral evals，提升回归保障。

**#28824 新增多工具链与安全边界评估** [Evals]  
链接：[PR #28824](https://github.com/google-gemini/gemini-cli/pull/28824)  
覆盖多工具链执行流程、大文件上下文安全处理、以及敏感文件/目录的安全边界强制。

**#28821 版本发布：bump 至 nightly.20260815** [Release]  
链接：[PR #28821](https://github.com/google-gemini/gemini-cli/pull/28821)  
自动版本号更新，对应 v0.56.0-nightly.20260815.g2a87e7be1。

---

### 功能需求趋势

从全部 Issue 中可以提炼出四个显著方向：

1. **Agent 可靠性治理**  
   围绕 MAX_TURNS 状态误报、挂起、未授权自动执行、破坏性命令等问题，社区的核心诉求是：agent 行为可预测、可追溯、受约束。

2. **安全与隐私加固**  
   从 Auto Memory 脱敏、SSRF 漏洞到 Node EOL 升级，安全不再只是外围话题，而是成为基础能力的一部分被持续审视。

3. **评估体系（Evals）扩展**  
   多个 PR 在补充 behavioral evals（任务依赖、多工具链、安全边界），社区正通过系统化评估来巩固 agent 行为质量。

4. **代码理解效率优化**  
   AST 感知文件读取/搜索/映射（[#22745](https://github.com/google-gemini/gemini-cli/issues/22745)）等方向，说明社区开始关注 token 成本与上下文利用效率。

---

### 开发者关注点

- **Sub-agent 卡死与误报是最大痛点**：多个 P1 Issue 指向挂起或不准确的成功状态，严重影响自动化信任度。
- **权限边界不容侵犯**：[#22093](https://github.com/google-gemini/gemini-cli/issues/22093) 表明用户对 agent 绕过配置自动执行非常敏感，禁用后必须严格禁用。
- **工具规模受限**：超过 128 个工具即报错，开发者希望 agent 能按场景智能裁剪工具集，而不是简单全量加载。
- **模型“不主动”使用 skills**：用户反馈模型极少自发调用自定义技能和子代理，需要显式指导，削弱了低阶自动化的体验。
- **终端交互稳定性**：命令执行后卡在 “Waiting input”、外部编辑器退出后界面损坏（[#24935](https://github.com/google-gemini/gemini-cli/issues/24935)）等细节问题，仍是高频干扰。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报 — 2026-08-16

## 今日速览

今日发布补丁版本 v1.0.81-0（更新模型配置）。社区最集中讨论的仍是 MCP 生态的稳定性与兼容性问题——Atlassian MCP OAuth 回归在 1.0.79/1.0.80 上持续发酵；此外 Codespaces 默认携带的旧版本 1.0.3 引发的更新失败、以及 Windows autopilot 模式下的 OOM 崩溃也获得较多关注。

## 版本发布

- **v1.0.81-0**（Improved）：更新模型配置。
  [查看发布说明](https://github.com/github/copilot-cli/releases)

## 社区热点 Issues

过去 24 小时共更新 17 条 Issue，以下 10 条最值得关注：

1. **NixOS 上 Bash 工具全面失效（≥1.0.49）**
   `area:platform-linux` | 👍 9 | 💬 4
   任何命令执行都会报 `Failed to start bash process`，在 NixOS 上自 1.0.49 开始可稳定复现，1.0.50 亦然，至今未定位根因。高赞说明受影响面较大。
   [Issue #3392](https://github.com/github/copilot-cli/issues/3392)

2. **Atlassian MCP OAuth 在 1.0.79 失败（1.0.71 回归，已关闭）**
   `area:authentication, area:mcp` | 👍 6 | 💬 4
   连接 `mcp.atlassian.com` 时 OAuth 发现阶段报 RFC 8414 §3.3 issuer 不匹配。该问题被标为已关闭，但同类问题在 1.0.80 上再次被报告（见下条）。
   [Issue #4480](https://github.com/github/copilot-cli/issues/4480)

3. **Atlassian MCP OAuth 在 1.0.80 再次损坏**
   `area:authentication, area:mcp` | 刚创建
   与 #4480 相同的错误信息，但版本为 1.0.80（1.0.78 正常），发布者跟踪链路明显出现遗漏，需警惕。
   [Issue #4490](https://github.com/github/copilot-cli/issues/4490)

4. **MCP initialize 握手固定 60 秒超时且无重试**
   `area:mcp` | 💬 1
   npx 启动的 stdio MCP 服务器约 29% 的会话在初始化阶段超时，日志仅记录一次失败后整个会话不再尝试拉起该服务器。严重影响使用 npx 类型 MCP 的开发者。
   [Issue #4421](https://github.com/github/copilot-cli/issues/4421)

5. **MCP registry 策略在 GitHub Actions 中返回 403**
   `area:authentication, area:non-interactive, area:mcp` | 👍 3（已关闭）
   使用文档推荐的无 PAT 方案（`GITHUB_TOKEN` + `copilot-requests: write`）时，非默认 MCP 服务器全部被 403 拒绝，CI 场景不可用。虽已关闭，但属高频痛点。
   [Issue #4346](https://github.com/github/copilot-cli/issues/4346)

6. **v1.0.79 autopilot 模式 OOM 崩溃（Windows）**
   `area:non-interactive, area:platform-windows`
   崩溃时 V8 堆仅用 0.6 GB / 4.3 GB，报 `FATAL ERROR: Committing semi space failed`——属于系统内存提交失败而非堆上限，长时运行时触发概率高，性质与常见 JS OOM 不同。
   [Issue #4499](https://github.com/github/copilot-cli/issues/4499)

7. **Codespaces 预装 1.0.3 且 `copilot update` 无法生效**
   `area:platform-linux, area:installation`
   新 Codespace 内 `copilot` 为 1.0.3，运行 update 显示下载 1.0.80 后二进制未被替换，需手动 sudo 安装。会直接影响用户在云环境中的上手体验。
   [Issue #4501](https://github.com/github/copilot-cli/issues/4501)

8. **新启用模型需清除本地缓存才可见**
   `area:models, area:configuration`
   在 GitHub 设置中启用新模型（如 Sonnet 5）后，CLI 与 VS Code 仍不可见，手动重置本地 Copilot 状态后恢复。模型目录缓存刷新机制缺失。
   [Issue #4494](https://github.com/github/copilot-cli/issues/4494)

9. **`/restart` 在 `-w`（worktree）会话中失败**
   `area:sessions`
   `copilot -w` 启动的会话内执行 `/restart` 时，worktree 选项与既有 session ID 冲突，无法恢复。直接影响启用 worktree 的用户日常使用。
   [Issue #4493](https://github.com/github/copilot-cli/issues/4493)

10. **BYOK autopilot 补全提示破坏 prompt caching**
    `area:non-interactive, area:models`
    在自动完成 nudge 轮次，CLI 会从内部状态重建整个输入数组而非 byte-for-byte 重发，导致 BYOK 场景下 prompt cache 全部失效，增加成本与延迟。
    [Issue #4500](https://github.com/github/copilot-cli/issues/4500)

## 重要 PR 进展

过去 24 小时共有 2 个 PR 更新：

1. **处理 fork PR 关联缺失的无效标签写入器**
   `mrecachinas` 提交，针对 fork PR 工作流中 GitHub 未填充 PR 关联时，写入器现在会基于可信工作流元数据搜索并仅要求恰有一个匹配的打开 PR 后写入标签，避免误关。
   [PR #4497](https://github.com/github/copilot-cli/pull/4497)

2. **将 PR 自动化从 `pull_request_target` 迁移**
   `mrecachinas` 提交，已合并关闭。用 issue-scoped token 直接关闭无效 issue，改用无权限的 `pull_request` 事件处理 PR，消除对 `pull_request_target` 的依赖，提升仓库安全性。
   [PR #4449](https://github.com/github/copilot-cli/pull/4449)

## 功能需求趋势

综合近期议题，社区关注方向主要有：

- **MCP 生态稳定性**：OAuth 认证回归、初始化超时、CI 场景权限受限，多个 Issue 集中指向 MCP 在真实生产环境中的可靠性不足。
- **模型管理与新模型支持**：对 GPT-5.6 `reasoning.mode` 参数支持（[#4495](https://github.com/github/copilot-cli/issues/4495)）、contextTier 会话级配置（[#4275](https://github.com/github/copilot-cli/issues/4275)）、新模型缓存刷新问题的诉求交织出现。
- **会话生命周期管理**：`/spawn` 语义矛盾（[#4491](https://github.com/github/copilot-cli/issues/4491)）、Done 会话无法取消归档（[#4502](https://github.com/github/copilot-cli/issues/4502)）、worktree 会话 restart 失败等，用户迫切需要更可靠的会话恢复与控制能力。
- **可观测性与协议标准**：OTLP protobuf 导出（[#2934](https://github.com/github/copilot-cli/issues/2934)）等标准协议支持仍在推进。

## 开发者关注点

高频痛点集中在：

- **MCP 认证与启动可靠性**：Atlassian OAuth 在多个版本反复回归、60 秒硬编码初始化超时无重试，是当前影响 MCP 实际部署的最大阻力。
- **更新链路不畅**：Codespaces 内旧版本无法自动更新，用户被迫手动干预，影响新功能获取与安全修复。
- **平台特定缺陷**：NixOS 的 bash 工具失效长期未解决；Windows autopilot 的 OOM 崩溃定位困难，稳定性问题突出。
- **缓存与状态不同步**：模型目录缓存不刷新、BYOK prompt cache 被破坏，影响模型可见性与使用成本。
- **会话操作语义不清晰**：`/spawn` 可注入已存在会话、`/restart` 在 worktree 模式下冲突等，提示面向会话的操作命令仍需更严格的契约校验。

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报 — 2026-08-16

数据来源：github.com/MoonshotAI/kimi-cli  
统计区间：2026-08-15 更新记录  
报告生成日期：2026-08-16

---

## 1. 今日速览

过去 24 小时 Kimi Code CLI 无新版本发布，社区讨论重心集中在两个方向：一是“跨会话记忆系统”长期未落地导致的体验痛点（#1283、#1478），二是订阅用户对配额不透明、压缩策略失效的质疑（#2604、#2603）。PR 侧则有 2 个来自外部贡献者的工具链修复更新，整体社区活跃度集中在问题反馈与功能期待上。

---

## 2. 版本发布

过去 24 小时内无新 Release。

---

## 3. 社区热点 Issues

> 说明：本次统计区间内更新的 Issue 共 5 条，全部列入。因当日数据样本较少，未强行凑满 10 条。

### #1283 [OPEN] [enhancement] 功能请求：记忆系统 —— 跨会话持久上下文

- **作者**：CatKang
- **创建时间**：2026-02-27 | **最后更新**：2026-08-15 | **评论数**：40
- **为什么重要**：这是一条自 2 月起持续发酵、累计 40 条评论的高热度需求。用户期望实现“自动记忆（AI 管理的笔记）”与“手动记忆（用户自定义指令）”双轨机制，让 CLI 在跨会话中记住项目模式与用户偏好。
- **社区反应**：评论持续增加但产品侧未见明确排期，属社区呼声最高、悬置最久的功能请求。
- **链接**：[MoonshotAI/kimi-cli Issue #1283](https://github.com/MoonshotAI/kimi-cli/issues/1283)

---

### #1478 [OPEN] [enhancement] 能否优化记忆层？参考文档里看不到相关设计，搞大项目很痛苦

- **作者**：hahy36
- **创建时间**：2026-03-17 | **最后更新**：2026-08-15 | **评论数**：3
- **为什么重要**：与 #1283 完全同频，直接指向大型项目中的“记忆断裂”问题。用户明确反馈文档里只看到 `agent.md`，没有记忆层说明；还参考了 OpenClaw 的 `SOUL.md / USER.md / MEMORY.md` 文件体系，希望 Kimi 也有类似机制。
- **社区反应**：虽然评论数不多，但和 #1283 互相印证，说明“记忆系统”是真实且被反复提及的缺口。
- **链接**：[MoonshotAI/kimi-cli Issue #1478](https://github.com/MoonshotAI/kimi-cli/issues/1478)

---

### #2604 [OPEN] 每周有效配额疑似未经公告缩减约 3–5 倍（含前后计量数据）

- **作者**：tobiu
- **创建时间**：2026-08-15 | **最后更新**：2026-08-15 | **评论数**：2
- **为什么重要**：订阅用户自建 wire-level JSONL 记账脚本，逐日记录原始 token 量（fresh input + cache reads + output），发现同一档位（Vivace）每周可用额度从 7 月中旬起大幅下降。这直接牵涉商业诚信问题，风险等级高。
- **社区反应**：刚发布即获 2 条评论，属于典型的“价格/配额透明度”高敏话题，可能会有持续追踪。
- **链接**：[MoonshotAI/kimi-cli Issue #2604](https://github.com/MoonshotAI/kimi-cli/issues/2604)

---

### #2603 [OPEN] 配额感知压缩：订阅制下应基于 Token 预算触发压缩，而非只依赖模型最大上下文窗口

- **作者**：salim4n
- **创建时间**：2026-08-15 | **最后更新**：2026-08-15 | **评论数**：0
- **为什么重要**：指出了 K3 1M 上下文窗口的实际副作用——`max_context_size = 1048576`，默认 `reserved_context_size = 50000`，导致压缩在真实会话里“几乎永远不会触发”。用户烧的是订阅配额，而不是模型窗口。
- **社区反应**：一条新提出但几乎没有延时的 Issue，表达精准，预计会与 #2604 合并为“配额管理”主题。
- **链接**：[MoonshotAI/kimi-cli Issue #2603](https://github.com/MoonshotAI/kimi-cli/issues/2603)

---

### #1155 [CLOSED] openai_legacy provider 丢弃 reasoning 内容，导致 APIEmptyResponseError

- **作者**：rongou
- **创建时间**：2026-02-14 | **最后更新**：2026-08-15 | **评论数**：0
- **为什么重要**：对接 OpenAI 兼容服务器（sglang / vllm）时，reasoning 内容因 `reasoning_key` 未传入 `OpenAILegacy` 构造器而被丢弃，进而引发 `APIEmptyResponseError`。这是自建推理后端用户的核心兼容性故障。
- **社区反应**：此 Issue 已关闭，但 8 月 15 日仍有更新，说明相关修复可能刚合入或即将合入。对使用非 Moonshot 推理后端的开发者仍是重要观察点。
- **链接**：[MoonshotAI/kimi-cli Issue #1155](https://github.com/MoonshotAI/kimi-cli/issues/1155)

---

## 4. 重要 PR 进展

> 说明：本次统计区间内更新的 PR 共 2 条，全部列入。因当日数据样本较少，未强行凑满 10 条。

### #2524 [OPEN] fix(tools): count StrReplaceFile replacements against the running content

- **作者**：Sreekant13
- **创建时间**：2026-07-20 | **最后更新**：2026-08-15 | **评论数**：未公开
- **功能/修复内容**：修复 `StrReplaceFile` 编辑计数逻辑——原实现按“原始文件内容”统计替换次数，导致链式编辑中由上一次编辑产生的 `old` 字符串无法被正确计数。修复后按“实时内容”计数，提升 agent 编辑文件时的正确性与可观测性。
- **链接**：[MoonshotAI/kimi-cli PR #2524](https://github.com/MoonshotAI/kimi-cli/pull/2524)

---

### #2506 [CLOSED] fix(kosong): raise a clear error on circular $ref in deref_json_schema

- **作者**：Sreekant13
- **创建时间**：2026-07-18 | **最后更新**：2026-08-15 | **评论数**：未公开
- **功能/修复内容**：修复 `kosong.utils.jsonschema.deref_json_schema` 在递归展开 JSON Schema 局部 `$ref` 时遇到循环引用可能死循环或堆栈溢出的问题，改为主动抛出明确的错误信息。属于 JSON Schema 解引用链路的健壮性修复。
- **链接**：[MoonshotAI/kimi-cli PR #2506](https://github.com/MoonshotAI/kimi-cli/pull/2506)

---

## 5. 功能需求趋势

> 基于本次统计区间覆盖的 5 条 Issue 与 2 条 PR 样本进行提炼。

### 1. 记忆系统 / 跨会话持久上下文 — 最高优先级诉求

- 代表性 Issue：#1283、#1478
- 社区期望：不仅要有自动记录（AI 管理的笔记），还要有用户可显式控制的记忆文件，并在官方文档中提供完整说明。用户不只提概念，更直接引用了其他工具的实现方式作为参考。
- 现状：连续数月未得到满足，属于“需求明确、实现缺位”的典型状态。

### 2. 订阅额度 / Token 预算管理 — 新出现的并发焦点

- 代表性 Issue：#2603、#2604
- 社区期望：
  - 压缩策略从“模型窗口导向”转为“配额导向”；
  - 额度变动需透明、有公告，而非“后台悄悄调整”。
- 现状：两条高信号 Issue 均在 8 月 15 日集中出现，短时间形成“配额管理”套餐反馈。

### 3. 推理后端兼容性完善

- 代表性 Issue：#1155
- 社区期望：使用 OpenAI 兼容服务（如 sglang、vllm）时，reasoning/thinking 字段需正确透传，不能因 provider 适配导致空响应。
- 现状：该 Issue 已关闭，但结合过去对多 provider 支持的诉求，仍是一个长期应被测试覆盖的方向。

---

## 6. 开发者关注点

- **大型项目的“记忆断裂”是清晰痛点**：#1478 的表述尤为直接——“搞大项目的时候很痛苦”。开发者需要的不只是单次会话能力，而是跨会话累积的项目上下文、用户偏好与决策历史。当前文档只提供 `agent.md`，与真正的记忆系统落差明显。

- **额度变化缺少透明化机制**：#2604 是典型的协议型反馈——用户精确计量了 API 调用，发现配额大幅缩减却无公告。这会直接影响社区对订阅定价体系的信任，需要官方明确是“规则调整”还是“计量回归”。

- **1M 上下文窗口带来的“假舒适区”**：#2603 提示核心风险：窗口够大，反而让压缩机制形同虚设。开发者在 agentic 工作流中频繁跑满配额，不得不自行控制上下文长度。这本质上是产品策略上的“窗口大小”与“成本预算”之间的错配。

- **工具链精确性开始被关注**：#2524 与 #2506 虽是小修复，但反映社区贡献者正在打磨 agent 文件编辑与 JSON Schema 处理的基础可靠性。此类 PR 的审核与合并速度，会影响外部贡献者的持续参与意愿。

---

**总结**：本次 24 小时窗口内，Kimi Code CLI 社区没有新产出版本，但“记忆系统”与“配额管理”两股声音明显增强，前者是长期悬置的老需求，后者是当天新出现的高热度议题。外部贡献者的 PR 质量在线，但后续语义与文档层面的跟进仍有待官方推进。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报

**日期：2026-08-16** | 数据来源：github.com/anomalyco/opencode

---

## 今日速览

过去 24 小时无新版本发布，社区热点集中在 **grok-4.5 模型持续故障**与**订阅计费/余额显示不一致**两大问题上，多个相关 Issue 引发大量讨论。开发侧，**Docker 工作区蓝图**与**插件事件订阅选择**两个新功能 PR 正在推进，另有多个性能与内存修复合入。

---

## 社区热点 Issues（TOP 10）

### 1. OpenCode Go 订阅支付成功但工作区显示“余额不足”
- **Issue #37790** · 💬 14 评论
- 用户通过 Stripe 成功支付 OpenCode Go 订阅，但工作区仍显示 "Insufficient balance"，导致无法使用服务。
- https://github.com/anomalyco/opencode/issues/37790

### 2. 官网宣称 100% 免费，为何实际要求订阅？
- **Issue #42143** · 💬 10 评论 · 👍 1
- 用户质疑官网“免费”宣传与实际订阅门槛的差异，反映文档与产品策略之间存在认知缺口。
- https://github.com/anomalyco/opencode/issues/42143

### 3. 功能建议：Go Pro 层级（$20）与首月折扣
- **Issue #24879** · 💬 11 评论 · 👍 11
- 用户希望新增固定额度的 Pro 订阅层级，避免频繁触达月度上限后被迫使用难以预算的按量付费。
- https://github.com/anomalyco/opencode/issues/24879

### 4. Plan Mode 下 Question 工具应自动切换至 Build 模式
- **Issue #7801** · 💬 10 评论 · 👍 31（今日最高赞需求）
- 这是社区呼声最高的功能请求：当 Plan Mode 使用 Question 工具时，应能自动切换到 Build 模式，减少手动操作摩擦。👍 数远超其他 Issue。
- https://github.com/anomalyco/opencode/issues/7801

### 5. grok-4.5 自 8 月 2 日起通过 OpenCode Go 调用持续报错
- **Issue #40206** · 💬 9 评论
- Windows 11 + opencode v1.18.11，通过 OpenAI Chat Completions API 调用 grok-4.5 始终返回 500，其他模型正常。
- https://github.com/anomalyco/opencode/issues/40206

### 6. grok-4.5 在 OpenCode Go 返回 HTTP 503
- **Issue #40886** · 💬 3 评论
- 官方文档推荐的 `/zen/go/v1/chat/completions` 端点上调用 grok-4.5 持续 503，而 deepseek-v4-flash 工作正常，进一步说明问题指向模型侧而非 API 网关。
- https://github.com/anomalyco/opencode/issues/40886

### 7. “Failed to fetch”错误导致无法发送 Prompt
- **Issue #42329** · 💬 4 评论
- 用户报告在长时间使用后发送 prompt 出现 "Failed to fetch" 错误，重启后仅能发送 0-1 条新消息便复发。该问题在上次更新后出现。
- https://github.com/anomalyco/opencode/issues/42329

### 8. tui.json 禁用鼠标后滚轮行为异常
- **Issue #35295** · 💬 4 评论
- 设置 `"mouse": false` 后，滚轮事件回退为方向键行为，意外触发 prompt 历史导航而非消息视口滚动。TUI 输入处理存在逻辑缺陷。
- https://github.com/anomalyco/opencode/issues/35295

### 9. 项目目录移动后旧路径残留
- **Issue #34737** · 💬 4 评论
- 项目从 C 盘移至 D 盘并删除原目录后，OpenCode 仍打开已删除的旧路径。项目路径管理缺少校验与迁移机制。
- https://github.com/anomalyco/opencode/issues/34737

### 10. Cloudflare 环境变量导致 Provider 列表崩溃
- **Issue #42739** · 💬 4 评论
- 当存在 Cloudflare 环境变量但缺少 `CLOUDFLARE_API_TOKEN` 时，`opencode` 启动即崩溃，TUI 无法初始化，提示 "Unexpected server error"。
- https://github.com/anomalyco/opencode/issues/42739

> 另注：`#42799`（OpenCode 服务器 500 / ResourceExhausted）、`#42750`/`#42757`（Endpoint 不可用）等服务器端异常也在集中上报，可能与近期 grok-4.5 故障存在关联。

---

## 重要 PR 进展（TOP 10）

### 1. feat(core): 添加 Docker 蓝图工作区（OPEN）
- **PR #42831** — 全新工作区提供商：基于不可变蓝图快照的本地 Docker 容器化工作区，支持 SDK 级别的 workspace forking，子代理进入隔离子容器；空闲容器自动停止、按需唤醒。
- https://github.com/anomalyco/opencode/pull/42831

### 2. feat(plugin): 选择性事件订阅（OPEN）
- **PR #42830** — 为插件新增 `ctx.event.subscribe(type)` 按类型订阅能力，替代现有的全量通配符订阅，并通过 `EventManifest.Server` 解析事件类型，减少不必要的跨进程事件分发。
- https://github.com/anomalyco/opencode/pull/42830

### 3. fix(core): 批处理流式 Session 增量（CLOSED）
- **PR #42826** — 当前服务器将每个文本/推理/工具输入片段都作为单独公共事件发布，实测负载过高。该 PR 合入后将流式增量批量发布，显著降低事件风暴。
- https://github.com/anomalyco/opencode/pull/42826

### 4. fix(app): 释放虚拟化时间线元素（CLOSED）
- **PR #42825** — 修复渲染器堆快照中 `Virtualizer2.elementsCache` 残留已删除时间线行的问题。一次长会话中约 37,500 个游离 DOM 节点被错误保留，该修复可明显降低内存占用。
- https://github.com/anomalyco/opencode/pull/42825

### 5. fix(app): 各处统一使用树形目录选择器（CLOSED）
- **PR #42820** — 移除旧的扁平目录选择器，统一使用树形目录选择器，改善 Web UI 中项目添加体验（24 个目录选择器相关测试通过）。
- https://github.com/anomalyco/opencode/pull/42820

### 6. refactor(core): 改用数值事件时间戳（CLOSED）
- **PR #42828** — V2 事件 `created` 字段改为以 epoch 毫秒数值存储/传输，仅在映射到 Session 领域模型时转换 DateTime。减少序列化往返损耗，为事件流性能优化打基础。
- https://github.com/anomalyco/opencode/pull/42828

### 7. fix(tui): 同步模型收藏列表（CLOSED）
- **PR #37172** — 模型收藏改为存储在托管 CLI 配置中，支持跨进程实时同步，修复同一机器上多个 TUI 实例收藏不同步的问题（Fixes #37053）。
- https://github.com/anomalyco/opencode/pull/37172

### 8. fix(server): 修复 bwrap PID 命名空间下的 SSE 事件丢失（CLOSED）
- **PR #37156** — 修复 `opencode serve` 在 bwrap `--unshare-pid` 沙箱内运行时分块 SSE 流停滞的问题。沙箱化部署场景下的事件流可靠性显着提升（Closes #37128）。
- https://github.com/anomalyco/opencode/pull/37156

### 9. fix(opencode): 停止重复空工具循环（CLOSED）
- **PR #37110** — 连续三次空结果/无匹配的 discovery 工具循环将自动终止，即使模型每次更改查询参数也不例外。避免 token 浪费与无效调用（Closes #31942）。
- https://github.com/anomalyco/opencode/pull/37110

### 10. feat(app): 添加模型覆盖设置（CLOSED）
- **PR #37087** — 新增 General 设置，可固定当前会话的模型覆盖，避免切换会话后模型选择被重置（Closes #23666）。
- https://github.com/anomalyco/opencode/pull/37087

> 其他值得关注的合并：`#37104`（GitLab token 传递与模型发现修复）、`#37097`（shell 命令运行时实时显示输出）、`#37088`（异步任务隐藏背景提示）、`#37058`（xAI 跨进程 OAuth 刷新单飞）、`#37184`（波斯语本地化翻译）。

---

## 功能需求趋势

从近期 Issues 提炼出社区最关注的五大方向：

1. **订阅与计费体系优化**
   - 新增 Pro 固定层级（#24879）、订阅状态与余额同步（#37790）、免费与付费策略透明化（#42143）——付费体验正成为社区最大的呼声群体。

2. **TUI / 终端交互细节打磨**
   - 跨行链接可点击性（#35649、#42805）、鼠标滚轮与禁用鼠标的行为一致性（#35295）、v2 子代理行点击交互回归（#42754）。终端体验是 TUI 用户的核心痛点。

3. **模型服务稳定性与多模型支持**
   - grok-4.5 连续多日故障（#40206、#40886、#42802）、MiMo V2.5 多模态输入未真实生效（#40642）、DeepSeek token 消耗异常（#32911）表明用户对模型提供方的接入质量高度敏感。

4. **工作区与项目管理**
   - Docker / Incus 容器化工作区（PR #42831、#42829）、项目目录迁移后路径残留（#34737）、Web UI 项目浏览体验（#42784）——用户期待更灵活的隔离与更健壮的路径管理。

5. **Agent 权限与自动化**
   - Plan Mode 自动切换（#7801，31👍）、Agent 权限规则运行时未生效（#32787）、headless 模式资源泄漏（#37671）——自动化与安全边界需要同时补齐。

---

## 开发者关注点（痛点与高频需求）

- **🔴 grok-4.5 模型持续异常**：自 8 月 2 日起 500/503 错误密集上报，涉及 OpenCode Go 与 Zen 两种接入方式，影响范围较大，是目前最集中的稳定性问题。
- **🔴 订阅支付后服务未激活**：Stripe 扣款成功但工作区仍提示余额不足，且无自助修复入口，对使用者信任影响严重。
- **🟠 服务器端点可用性**：Endpoint is unavailable、Failed to fetch、服务器 500 / ResourceExhausted 等错误近期集中出现，可能与后端容量或上游依赖有关。
- **🟠 资源消耗与泄漏**：headless 命令加载 OpenTUI 库并泄漏 13.1 MiB 临时文件/进程；虚拟化时间线 DOM 节点大量残留；DeepSeek token 过度计费——性能和成本控制是开发者刚需。
- **🟠 权限规则形同虚设**：`permission.ask` 等配置在运行时未被真正执行，Agent 绕过确认直接执行工具操作，是安全相关的高危待修复点。
- **🟡 长链接在终端无法点击**：跨行 URL 无法识别为可点击超链接，阻碍工作流效率，TUI 侧 Markdown/OSC 8 处理有待完善。

---

*本日报基于 GitHub 公开数据整理，仅代表社区讨论动态，不构成官方立场。*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi 社区动态日报 — 2026-08-16

## 今日速览

过去 24 小时，Pi 社区的核心热度集中在**上下文压缩（auto-compaction）可靠性**上，多个 PR 针对压缩时机不当导致的崩溃、token 统计偏差等问题展开修复。与此同时，TUI 渲染体验（光标闪烁、滚动配置）与模型供应商支持（DeepSeek、xAI、Mermaid 渲染）也有明显进展。最受关注的 Issue 是 [`#6879`](https://github.com/earendil-works/pi/issues/6879)：自动压缩在上下文超过 100% 后仍不触发，收获 21 条讨论和 17 个 👍。

---

## 社区热点 Issues

### 1. auto-compaction 超过 100% 仍不触发，直到 API 拒绝请求
[`#6879`](https://github.com/earendil-works/pi/issues/6879) | OPEN | 👍 17 | 💬 21

单次 agentic turn 运行超过 2 小时，footer 越过压缩阈值后继续增长，直到 API 在 373k tokens 拒绝请求才触发压缩。作者建议在每一步 agent 动作后都检查上下文占比。高赞、高讨论量，是当前上下文管理方向最核心的 bug。

### 2. WSL 下 GitHub Copilot 设备授权后登录挂起
[`#6187`](https://github.com/earendil-works/pi/issues/6187) | CLOSED | 💬 27

浏览器完成设备授权后，WSL 内的 pi 客户端检测不到注册状态，一直卡在等待登录。评论数最高，说明 WSL 用户群体广泛，此类环境兼容性问题影响面大。

### 3. 响应随机截断 "Response was truncated before completion."
[`#7855`](https://github.com/earendil-works/pi/issues/7855) | CLOSED | 💬 5

任何 OpenAI 兼容 API（包括本地 VLLM）都可能随机出现红色报错 “Response was truncated before completion.”，需要用户手动提示继续。影响面广，且问题定位困难。

### 4. openai-codex-responses 把可选工具参数物化为必选
[`#8105`](https://github.com/earendil-works/pi/issues/8105) | CLOSED | 💬 4

`buildRequestBody` 序列化工具时携带 `strict: null`，在 gpt-5.6-sol 下导致可选参数变成必选，调用方必须提交所有属性。属于模型兼容性细节问题。

### 5. 全屏 TUI 鼠标滚轮步长硬编码为 1 行
[`#7765`](https://github.com/earendil-works/pi/issues/7765) | CLOSED | 👍 2 | 💬 5

`wheelScrollLines` 默认值被硬编码为 1，用户无法在`pi-tui`中配置滚轮速度。功能请求虽小，但直接影响全屏模式下的浏览效率。

### 6. 请求新增 `pi completion` shell 补全生成器
[`#4776`](https://github.com/earendil-works/pi/issues/4776) | CLOSED | 👍 5 | 💬 4

希望增加 `pi completion <bash|zsh|fish>` 子命令，输出补全脚本供用户 source。5 个 👍 表明社区对 CLI 完善度有明确需求。

### 7. bash 工具注入的 PI_* 指导语导致不必要的权限提示
[`#7787`](https://github.com/earendil-works/pi/issues/7787) | OPEN | 💬 3

默认 `exposeSessionEnvironment: true` 时，Pi 会给每个会话注入“检查 PI_* 环境变量”的指导语，模型在普通任务中也会执行 `env`，触发额外权限确认。对应 PR [`#8148`](https://github.com/earendil-works/pi/pull/8148) 已提交修复。

### 8. TUI `fullRender` 因 V8 字符串长度限制崩溃
[`#8028`](https://github.com/earendil-works/pi/issues/8028) | OPEN | 💬 2

视频生产 agent 读取大量图片后，渲染输出超过 V8 字符串上限，触发 `RangeError: Invalid string length`。图像密集型任务容易踩中。

### 9. 流式输出时输入框光标异常闪烁
[`#8003`](https://github.com/earendil-works/pi/issues/8003) | OPEN | 💬 2

AI 生成过程中，输入框光标以异常频率闪烁，输入时更明显。PR [`#8155`](https://github.com/earendil-works/pi/pull/8155) 正在尝试修复。

### 10. Windows 上 bash 工具可执行 `taskkill` 杀死 pi 自身宿主
[`#8170`](https://github.com/earendil-works/pi/issues/8170) | CLOSED | 💬 2

模型生成的 `taskkill /F /IM node.exe` 在 Windows 上无确认执行，直接杀死 pi-web 的 Node.js 宿主进程。属于安全边界问题，值得关注。

---

## 重要 PR 进展

### 1. DRAFT：包含新 harness 的 dev 分支
[`#8076`](https://github.com/earendil-works/pi/pull/8076) | OPEN

新 harness 的开发分支，可能预示着 pi 核心架构的重要演进，建议关注后续走向。

### 2. 升级 Mermaid 终端渲染（grok-mermaid → lovely-mermaid）
[`#8158`](https://github.com/earendil-works/pi/pull/8158) | OPEN

Closes [`#8157`](https://github.com/earendil-works/pi/issues/8157) 和 [`#7832`](https://github.com/earendil-works/pi/issues/7832)。lovely-mermaid 修复了原始 grok-mermaid 移植版的诸多边角问题，提升终端图表渲染质量。

### 3. 在安全 turn 边界触发压缩
[`#8153`](https://github.com/earendil-works/pi/pull/8153) | CLOSED

新增运行级 boundary-compaction 请求 API，在完整 turn 之间重建 live context，保留最近原生消息尾部，避免在 provider turn 中途压缩。

### 4. 禁止从尾部 assistant 消息继续（修复压缩后崩溃）
[`#8164`](https://github.com/earendil-works/pi/pull/8164) | CLOSED

静默溢出压缩遇到已完成 turn（stopReason 'stop'）时，使用 `agent.continue()` 重试会导致 “Cannot continue from message role: assistant” 崩溃。修复为仅在 turn 因错误中断时重试，避免误用。

### 5. tokens.total 仅统计计费 token
[`#8165`](https://github.com/earendil-works/pi/pull/8165) | CLOSED

此前 `tokens.total` 包含缓存 token（按输入价格 1/120 计费），导致压缩预算和状态统计失真。修复后 total = input + output，缓存单独报告。

### 6. 移除 OpenAI 请求中无效的 session_id header
[`#8149`](https://github.com/earendil-works/pi/pull/8149) | CLOSED

OpenAI Responses 请求携带 `session_id` HTTP header，HTTP/1 代理会拒绝含下划线的 header，生产环境出现 Envoy `400 http1.unexpected_underscore`。修复为不发送该 header。

### 7. 限制 bash 工具 PI_* 指导语的使用范围
[`#8148`](https://github.com/earendil-works/pi/pull/8148) | CLOSED

修复 [`#7787`](https://github.com/earendil-works/pi/issues/7787)。将 PI_* 环境变量指导语限定在“会话相关问题”范围内，避免模型在无关任务中主动执行 `env` 触发权限提示。

### 8. DeepSeek V4 Flash 在 opencode/opencode-go 暴露 low thinking level
[`#8181`](https://github.com/earendil-works/pi/pull/8181) | CLOSED

`DEEPSEEK_V4_FLASH_THINKING_LEVEL_MAP` 之前只应用于 `deepseek/deepseek-v4-flash`，通过 opencode 供应商接入时回退到 `low: null`。此修复统一了推理级别映射。

### 9. 抑制扩展 widget 渲染失败导致的崩溃
[`#8151`](https://github.com/earendil-works/pi/pull/8151) | CLOSED

第三方扩展 `@marckrenn/pi-sub-bar` 在 `/reload` 后 widget registration 残留，渲染失败导致清理异常。修复为容纳渲染失败并正确销毁 ctx 持有的 widget。

### 10. xAI 模型默认走 Responses API，默认模型切换为 Grok 4.6
[`#8124`](https://github.com/earendil-works/pi/pull/8124) | OPEN

默认使用 Responses API 替代 Completions，同时将 xAI 默认模型从 Grok 4.5 升级到 Grok 4.6，并发送 Pi 的 user agent。

---

## 功能需求趋势

从近期 Issues 和 PR 中，社区最关注的方向可归纳为四类：

1. **上下文压缩与内存管理**：压缩触发时机不可靠（[`#6879`](https://github.com/earendil-works/pi/issues/6879)）、压缩边界安全（[`#8153`](https://github.com/earendil-works/pi/pull/8153)）、压缩失败后恢复（[`#8164`](https://github.com/earendil-works/pi/pull/8164)）、压缩事件对扩展透明（[`#8175`](https://github.com/earendil-works/pi/issues/8175)）——这是当前最热的工程方向。

2. **TUI 交互体验打磨**：鼠标滚轮步长可配置（[`#7765`](https://github.com/earendil-works/pi/issues/7765)）、thinking 块折叠/固定高度滚动（[`#8171`](https://github.com/earendil-works/pi/issues/8171)）、光标闪烁修复（[`#8003`](https://github.com/earendil-works/pi/issues/8003)）、Windows Terminal 快捷键冲突文档化（[`#8183`](https://github.com/earendil-works/pi/issues/8183)）。

3. **扩展系统能力补全**：UI 对话框事件（[`#7147`](https://github.com/earendil-works/pi/issues/7147)）、模型选择前可取消钩子（[`#8169`](https://github.com/earendil-works/pi/issues/8169)）、压缩失败对扩展可见（[`#8175`](https://github.com/earendil-works/pi/issues/8175)）、快捷键使用扩展命令上下文（[`#8180`](https://github.com/earendil-works/pi/issues/8180)）。

4. **模型与供应商扩展**：DeepSeek V4 Flash 推理级别补充（[`#8182`](https://github.com/earendil-works/pi/issues/8182)）、LLMTR 内置供应商（[`#8178`](https://github.com/earendil-works/pi/issues/8178)）、llama.cpp router 模式模型列表支持（[`#8167`](https://github.com/earendil-works/pi/issues/8167)）、xAI 默认模型升级（[`#8124`](https://github.com/earendil-works/pi/pull/8124)）。

---

## 开发者关注点

- **压缩机制是最大痛点**：上下文超过阈值后不自动压缩、压缩后 token 统计偏差（[`#8165`](https://github.com/earendil-works/pi/pull/8165)）、压缩后 tool-result 角色损坏导致 422（[`#8168`](https://github.com/earendil-works/pi/issues/8168)）——这些问题直接影响长会话稳定性。
- **TUI 渲染稳定性**：全屏渲染崩溃（[`#8028`](https://github.com/earendil-works/pi/issues/8028)）、光标闪烁（[`#8003`](https://github.com/earendil-works/pi/issues/8003)）、隐藏 thinking 块残留空白行（[`#8154`](https://github.com/earendil-works/pi/issues/8154)）反映出终端渲染层需要更多鲁棒性。
- **供应商规格不准确**：Baseten 对 DeepSeek V4 Flash 的实际输出上限为 384k 而非 models.dev 记录的 1M（[`#8146`](https://github.com/earendil-works/pi/pull/8146)）、Codex 可选参数被物化（[`#8105`](https://github.com/earendil-works/pi/issues/8105)）——开发者需要更可靠的模型元数据来源。
- **扩展开发体验**：widget 渲染失败会拖垮整个进程（[`#8151`](https://github.com/earendil-works/pi/pull/8151)）、压缩失败对扩展完全静默（[`#8175`](https://github.com/earendil-works/pi/issues/8175)）——扩展生态开发者希望获得更清晰的生命周期和错误传播机制。
- **安全边界**：bash 工具在 Windows 上可无确认执行 `taskkill /F /IM node.exe` 杀死宿主进程（[`#8170`](https://github.com/earendil-works/pi/issues/8170)），模型生成的危险命令需要更严格的确认策略。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报（2026-08-16）

> 数据来源：github.com/QwenLM/qwen-code ｜ 统计窗口：2026-08-15 更新内容

## 1. 今日速览

- 发布 nightly 版本 `v0.21.11-nightly.20260815.c396fe3d12`，核心变更为 autofix 默认拒绝型 footprint gate，并完成 DSW EAS SWE-bench Verified + Terminal-Bench 2.0 多轮发布链路冒烟验证。
- 社区焦点集中在 `/review` 子命令的可靠性：并发 worktree 竞争、schema 不兼容、重叠检测漏报等问题密集出现，团队正通过多个 fix PR 快速响应。
- 用户侧痛点依旧集中在 Web Shell 工件加载失败、中文输入法失效、长时间运行 OOM、文件权限硬编码 0600 等问题，多个 P2 级 bug 等待处理。

## 2. 版本发布

### v0.21.11-nightly.20260815.c396fe3d12
- 链接：https://github.com/QwenLM/qwen-code/releases/tag/v0.21.11-nightly.20260815.c396fe3d12
- 主要变更：
  - `feat(autofix)`: deny-by-default footprint gate and positional window censuses（PR #9156）
  - `fix(web-shell)`: Terminal-Bench proxy-prelude 修复后的端到端冒烟验证
- 发布验证：包含 DSW EAS SWE-bench Verified + Terminal-Bench 2.0 冒烟测试 r1–r5，Benchmark-Qwen-Ref 指向 v0.21.12，表明该 nightly 主要在验证 v0.21.12 正式版的发布与基准测试链路。

## 3. 社区热点 Issues

1. [#7427] **Web Shell 工件面板自动刷新时刷屏 “Load artifacts failed: Failed to fetch”**（5 评论）
   工件列表在 `promptStatus` 变为 idle 时自动刷新，每次刷新都触发失败 toast，用户界面被持续打扰。
   https://github.com/QwenLM/qwen-code/issues/7427

2. [#9250] **qwen serve 新建文件硬编码 0600 权限，忽略 umask 且不可配置**（4 评论）
   所有文本写入工具创建新文件均使用 0600 模式，与常规用户期望不符，且没有配置或环境变量可覆盖，影响多用户协作场景。
   https://github.com/QwenLM/qwen-code/issues/9250

3. [#9089] **autofix PAT 任务与不受信任的分支代码共享主机，亟需 runner 级隔离**（4 评论，P1/安全）
   安全审查发现，携带 PAT 的 CI 任务运行在复用 runner 上，无法从 Actions 步骤内部彻底封堵攻击面，属于架构级安全隐患。
   https://github.com/QwenLM/qwen-code/issues/9089

4. [#9230] **follow-up 建议侧查询破坏服务器端前缀缓存，`enableCacheSharing` 默认关闭**（3 评论）
   主会话 prompt 缓存命中率约为 0%，LLM 服务器被迫反复全量预填充，缓存共享开关默认关闭，造成显著推理成本浪费。
   https://github.com/QwenLM/qwen-code/issues/9230

5. [#9198] **长时间运行后 OOM：服务器 1T 内存仍被耗尽**（3 评论）
   用户连续运行一周后 OOM，且退出后 tmux 终端按键错乱。疑似会话状态累积泄漏，属于稳定性问题。
   https://github.com/QwenLM/qwen-code/issues/9198

6. [#5966] **0.19.3 UI 中文输入法完全失效**（4 评论）
   中文用户无法在界面中直接输入中文，只能输入拼音且无报错。该问题长期未定位，社区关注度高。
   https://github.com/QwenLM/qwen-code/issues/5966

7. [#9200] **相同任务、相同本地模块，结果相同但过程差异巨大，用户表达强烈不满**（4 评论）
   用户对比多份日志，发现 Qwen Code 多次执行同一任务时思考和调用过程不稳定，体验一致性受质疑。
   https://github.com/QwenLM/qwen-code/issues/9200

8. [#9219] **`/review` presubmit 重叠匹配仅按精确行比对，多行区间和语义重复均会漏报**（4 评论）
   导致重复评论未被识别，自动审查在 PR #9204 中实际漏掉重复项，降低 review 结果可信度。
   https://github.com/QwenLM/qwen-code/issues/9219

9. [#9205] **并发 review 同一 PR 时争夺固定 worktree 路径，工作树运行中被删**（3 评论）
   固定路径 `.qwen/tmp/review-pr-<n>` 导致两个会话互相踩踏，cleanup 记录显示 5 次未凭证删除。
   https://github.com/QwenLM/qwen-code/issues/9205

10. [#9011] **`ask_user_question` 静默返回 “User declined”，既不展示问题也不给出取消原因**（3 评论）
    交互工具吞掉真实状态，用户被误导以为主动拒绝，影响自动化流程正确性。
    https://github.com/QwenLM/qwen-code/issues/9011

## 4. 重要 PR 进展

1. [#9220] **fix(ci): 复用 review runner 上自愈失败的 checkout**
   首次 checkout 失败后自动清理并重试，避免 runner 工作区永久损坏导致 CI 任务卡死。
   https://github.com/QwenLM/qwen-code/pull/9220

2. [#9228] **fix(ci): 将 serve-ab 自托管 wipe 范围收窄到 A/B checkout 目录**
   之前会删除整个共享工作区（含约 900MB 的根 `.git` 历史），修复后仅清理必要目录，大幅减少 runner 重复拉取成本。
   https://github.com/QwenLM/qwen-code/pull/9228

3. [#8467] **feat(web-shell): 增加 Git diff 来源与现有分支切换**
   扩展 Web Shell Git 工具，支持 Uncommitted、Unstaged、Staged、Committed、Branch comparison 等多来源，并加入可搜索的分支/提交选择器。
   https://github.com/QwenLM/qwen-code/pull/8467

4. [#9087] **feat(web-shell): 采用标准 Goal v3 控制平面**
   支持首条消息前创建/编辑/暂停/恢复/替换/清除 Goal，WebShell 在 composer 行内展示当前 Goal，不再强制通过模型路由命令。
   https://github.com/QwenLM/qwen-code/pull/9087

5. [#8927] **feat(channels): 通过 sessionRotation 限制通道会话生命周期**
   可配置 `maxTurns` 或时间上限，超过上限后同路由下一条消息自动开启新会话，适合高频渠道防串话。
   https://github.com/QwenLM/qwen-code/pull/8927

6. [#9222] **fix(review): 标准化 last-gate 输入并支持行中片段锚定**
   解决 `/review` 管道最后阶段因 schema 不匹配拒绝自身产物的问题，并补齐文档/工具链缺口，避免数小时审查在终点线失败。
   https://github.com/QwenLM/qwen-code/pull/9222

7. [#9215] **fix(review): 为重复丢弃的 Suggestion 提供独立 compose state**
   已确认但因 prior round 或并发 reviewer 已发布而未重新提交的建议，现在会显式记录来源与正文，审计可追溯性增强。
   https://github.com/QwenLM/qwen-code/pull/9215

8. [#9211] **fix(review): 为 PR review worktree 增加租约锁**
   并发会话破坏固定 worktree 路径的问题通过租约锁解决，任何破坏性操作前先校验持有权，杜绝运行中目录被删。
   https://github.com/QwenLM/qwen-code/pull/9211

9. [#9191] **feat(review): 跨 rebase 转移按文件内容审查结论**
   rebase/force-push 后不再退化为全量 review，而是基于文件内容对保留历史结论，显著提升增量审查效率。
   https://github.com/QwenLM/qwen-code/pull/9191

10. [#9007] **fix(serve): 按字节限制 ACP HTTP pre-attach 缓冲区**
    防止超大预附加内容拖垮服务内存或触发异常，为 ACP 服务器增加可预期的资源边界。
    https://github.com/QwenLM/qwen-code/pull/9007

## 5. 功能需求趋势

- **Web Shell 体验增强**：Git diff 来源扩展、Goal v3 控制、会话重命名/保留、HTML 导出改用 WebShellTranscript 渲染，说明终端 UI 正在从“可用”走向“精细化管理”。
- **审查与 CI 自动化可靠性**：多 issue 聚焦 `/review` 的 schema 一致性、worktree 隔离、重叠检测、跨 rebase 结论迁移；CI 侧强调自愈 checkout 与定向清理，自动化基础设施仍是团队当前重点投入方向。
- **性能与资源可配置性**：前缀缓存命中率问题、1T 内存仍 OOM、文件权限可配置化，反映出用户对运行成本和系统资源透明度的诉求越来越高。
- **多平台通道扩展**：DingTalk 文件投递、会话轮换、音频附件桥接，显示 Qwen Code 正在从单机 IDE 工具走向多渠道自动化代理平台。

## 6. 开发者关注点

- **`/review` 工具链的“最后一公里”问题集中爆发**：多个 issue 显示审查管道在 final gate 因内部 schema 不匹配而失败，且并发、重叠、清理逻辑存在边界漏洞，团队修复速度很快但暴露面仍较广。
- **主分支 CI 连续多日失败**：`#9241`、`#9239`、`#9237` 等 P1 E2E 测试失败均发生在 “before any test result”，说明基础设施层问题优先于测试逻辑本身，当前已被 autofix 接管。
- **中文用户与长期运行用户受影响最深**：输入法失效、OOM、终端乱码在多 issue 中反复出现，且部分问题停留在“需要信息”状态，尚无明确修复版本。
- **权限与缓存策略需要更开放**：开发者希望 new-file mode、缓存共享开关等行为可配置，而不是被硬编码或默认关闭，以适配不同的服务端部署环境。

---
*本日报由 AI 技术分析师自动整理，仅供参考。*

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报（2026-08-16）

> 数据来源：`github.com/Hmbown/DeepSeek-TUI`（Issue / PR 页面位于 Hmbown/CodeWhale）

## 今日速览

过去 24 小时没有新 Release。v0.9.8 相关修复与合并密集推进：CI 红色状态逐步转绿，macOS 下 agent 输出乱码、宽屏终端输出区回退等问题均已有对应修复；第三方模型预制模板和长上下文模型工具结果预算配置也正式落地。社区层面，“Constitution”中文译名在讨论三周后最终定为“宪章”，TUI crate 拆分 EPIC 仍在持续跟踪。

---

## 社区热点 Issues

### 1. [#4949][CLOSED] “Constitution” 中文翻译讨论：宪法 / 协作准则 / 宪章？
**链接**：[Hmbown/CodeWhale Issue #4949](https://github.com/Hmbown/CodeWhale/issues/4949)

PR #4908 作者将“Constitution”中文译名从“协作准则”改回“宪法”，引发对中文语境敏感性和准确性的讨论。Issue 共收到 17 条评论，最终以“宪章”作为定稿译名，Web 端已由 PR #5397 跟进。这是本地化文案治理的重要社区决策。

### 2. [#5316][OPEN] EPIC-005：CodeWhale TUI Crate 分解（伞形 Issue）
**链接**：[Hmbown/CodeWhale Issue #5316](https://github.com/Hmbown/CodeWhale/issues/5316)

作为 TUI crate 分解的顶层跟踪 Issue，汇总所有子 EPIC、FEAT 和 PR。这是当前架构演进的主干，对关注 TUI 模块化方向的同学很重要。

### 3. [#5374][OPEN] [bug] macOS 上 agent 输出文字乱码
**链接**：[Hmbown/CodeWhale Issue #5374](https://github.com/Hmbown/CodeWhale/issues/5374)

用户反馈 macOS 下 agent 书写时文本全部损坏。根因被定位到 Chat Completions SSE 在 HTTP/2 DATA 帧边界分割多字节 UTF-8 字符时使用了错误的 lossy 解码。对应修复见 PR #5404。

### 4. [#5350][OPEN] [enhancement] 简化第三方模型配置，增加预制模板
**链接**：[Hmbown/CodeWhale Issue #5350](https://github.com/Hmbown/CodeWhale/issues/5350)

配置 OpenCode Zen、OpenCode Go、Agnes、美团 Sensenova 等第三方服务时，需要手填 Base URL、模型名和密钥环境变量，且模型列表容易卡在 `not checked` / `cache failed`。社区建议内置预制模板、嵌入官方文档、增加“测试连接”按钮，PR #5406 已实现。

### 5. [#5367][OPEN] [enhancement] 自托管长上下文模型的 read / tool-result 大小限制应可配置
**链接**：[Hmbown/CodeWhale Issue #5367](https://github.com/Hmbown/CodeWhale/issues/5367)

自托管 DeepSeek V4 等长上下文模型时，`read` 50 KiB、隐藏 `read_file` 16 KiB、tool-result 12000 字符的保守上限会导致读取 64 KiB 文件需要约 20 次额外 read。社区希望限制可在 model 或 HarnessProfile 级别配置，PR #5405 已实现。

### 6. [#5337][OPEN] Web：完成 #4934 字典 spine，移除 isZh 分支并内联 { en, zh } 模块
**链接**：[Hmbown/CodeWhale Issue #5337](https://github.com/Hmbown/CodeWhale/issues/5337)

Web 端 i18n 重构尚未彻底：只有路由层使用统一字典路径，大量页面 body 仍保留 `isZh` 分支。该 Issue 推动 Web 端本地化走向统一数据结构，减少维护成本。

### 7. [#5322][CLOSED] [bug] 回归：输出区无法填满宽终端（v0.8.65 正常）
**链接**：[Hmbown/CodeWhale Issue #5322](https://github.com/Hmbown/CodeWhale/issues/5322)

v0.9 将 transcript/output 区域限制在最大宽度，导致宽屏显示器下文本拥挤、右侧留白。PR #5400 将 `session_shell_area` 恢复为 identity 布局，重新支持宽屏铺满，已关闭。

### 8. [#5241][OPEN] Pricing endpoint 503，所有会话显示 unverified_live_pricing
**链接**：[Hmbown/CodeWhale Issue #5241](https://github.com/Hmbown/CodeWhale/issues/5241)

从 0.8.67 升级到 0.9.3 后，成本展示失效，所有 provider 均返回 `unverified_live_pricing`，原因指向 `https://api.codewhale.net/session` 返回 503。PR #5402 提供了修复，不再让会话成本永久处于未验证状态。

### 9. [#5410][OPEN] [enhancement] 允许在 bwrap sandbox 中配置额外根路径
**链接**：[Hmbown/CodeWhale Issue #5410](https://github.com/Hmbown/CodeWhale/issues/5410)

有开发者使用 Zig + CodeWhale + deepseek，启用 bwrap sandbox 后出现 `/dev/null` 重定向被禁止、链接系统库失败等问题。该 Issue 建议允许用户配置额外 sandbox root，以兼容本地工具链。属于 sandbox 安全边界与可用性之间的平衡需求。

### 10. [#5060][CLOSED] [workflow-runtime] 工作流搜索重新硬编码 16 worker 上限，未读取 Fleet 并发 seam
**链接**：[Hmbown/CodeWhale Issue #5060](https://github.com/Hmbown/CodeWhale/issues/5060)

`WORKFLOW_SEARCH_MAX_CONCURRENT: u16 = 16` 被硬编码，未使用 Fleet pool / admission config 的实时并发限制。社区建议读取实际并发上限，并以 16 作为 fallback，同时在 run receipts 中暴露解析结果，便于运维判断搜索边界。

---

## 重要 PR 进展

### 1. [#5407][OPEN] v0.9.8：完成 assigned cut
**链接**：[Hmbown/CodeWhale PR #5407](https://github.com/Hmbown/CodeWhale/pull/5407)

将 `codex/v098-final-20260814` 分支的 v0.9.8 收尾内容合入 main，tag 目标为 `d30effc82492704caa1b00c342e032703427cead`。同时保持与 #5322/#5400 的 session-shell geometry 一致，是版本管理的关键合并。

### 2. [#5406][OPEN] feat(tui)：第三方模型预制模板和测试连接（#5350）
**链接**：[Hmbown/CodeWhale PR #5406](https://github.com/Hmbown/CodeWhale/pull/5406)

实现 #5350：内置 OpenCode Zen、OpenCode Go、Agnes、SenseNova 预制模板，用户只需填写 API key；为首次接入的模型路由增加“测试连接”按钮并自动刷新状态，提升第三方模型配置体验。

### 3. [#5405][OPEN] feat(tui)：可配置的 read / tool-result 模型可见预算（#5367）
**链接**：[Hmbown/CodeWhale PR #5405](https://github.com/Hmbown/CodeWhale/pull/5405)

为自托管长上下文模型开放 `read`、隐藏 `read_file`、tool-result context/wire 限制配置，支持按 model / HarnessProfile 调整，解决长上下文模型因保守上限导致的额外读取开销。

### 4. [#5404][OPEN] fix(client)：SSE UTF-8 跨 HTTP/2 DATA 分割时 fail closed（#5374）
**链接**：[Hmbown/CodeWhale PR #5404](https://github.com/Hmbown/CodeWhale/pull/5404)

修复 macOS 下 DeepSeek Flash streaming 输出乱码问题。核心改动是 Chat Completions SSE 解码不再使用 `String::from_utf8_lossy` 拼接未终止的多字节字符，而是遇到跨帧分割时 fail closed，保证输出文本可读。

### 5. [#5402][OPEN] fix(tui)：live pricing 无法验证时恢复会话成本（#5241）
**链接**：[Hmbown/CodeWhale PR #5402](https://github.com/Hmbown/CodeWhale/pull/5402)

修复 #5241：当 live pricing 无法验证，包括 `api.codewhale.net/session` 返回 503 `control_plane_not_attached` 时，不再让会话成本永久停留在 `unverified_live_pricing`，而是采用诚实路径恢复成本展示。

### 6. [#5401][OPEN] fix：CodeQL Highs（#107、#88–#106）并准备 GHSA-8hp3 / GHSA-3mgh
**链接**：[Hmbown/CodeWhale PR #5401](https://github.com/Hmbown/CodeWhale/pull/5401)

安全修复切片：修复 `scripts/catalog_models_dev.py` 明文日志泄漏，以及 CodeQL 报告中 #88–#106 的 High 级别问题；同时为 GHSA-8hp3 和 GHSA-3mgh 做准备。该 PR 不触碰 v0.9.8 tag、不发布 crates/npm/Homebrew。

### 7. [#5400][CLOSED] fix(tui)：transcript 铺满终端宽度（#5322）
**链接**：[Hmbown/CodeWhale PR #5400](https://github.com/Hmbown/CodeWhale/pull/5400)

关闭 #5322。恢复 v0.8.65 行为，`session_shell_area` 变为 identity，transcript 和 composer 在宽终端 / tmux 下填满宿主宽度，不再被 v0.9 的侧边 gutter 压缩。

### 8. [#5399][CLOSED] fix(tui)：v0.9.8 稳定性修复
**链接**：[Hmbown/CodeWhale PR #5399](https://github.com/Hmbown/CodeWhale/pull/5399)

在 main 上重建缺失的 v0.9.8 Rust 稳定性修复，包括：默认直接子代理改为 turn-owned、compaction 质量改进、Blue Stage web 修复。不包含版本号提升、tag、release 或无关功能。

### 9. [#5395][CLOSED] fix(CI)：取消 cancel-in-progress 导致并发 main push 相互取消
**链接**：[Hmbown/CodeWhale PR #5395](https://github.com/Hmbown/CodeWhale/pull/5395)

此前 main 分支 CI 在缺少 `pull_request.number` 时共享同一个 concurrency group，且 `cancel-in-progress: true` 会让后续 push 取消前一次运行，导致失败断言从未变红。该 PR 修复后，并发 main 推送可以完整跑完，CI 结果更可靠。

### 10. [#5394][CLOSED] fix：修复 v0.9.8 provider-count 断言和 Google ModelRegistry drift
**链接**：[Hmbown/CodeWhale PR #5394](https://github.com/Hmbown/CodeWhale/pull/5394)

解决 #5383 及 Lint provider-registry drift：v0.9.8 将 registry 数量从 43 提升到 45、catalog 从 38 提升到 40，同时更新 Google ModelRegistry 漂移，让 main 的 CLI provider 断言恢复绿色。

---

## 功能需求趋势

- **第三方模型接入体验**：社区明显希望减少手填 Base URL / 模型名 / 密钥的流程，要求预制模板、官方文档嵌入和“测试连接”能力。代表：#5350、PR #5406。
- **长上下文 / 自托管模型资源预算可配置**：DeepSeek V4 等模型需要更大单次 read / tool-result 上限，同时不牺牲安全边界。代表：#5367、PR #5405。
- **沙箱与安全边界的可塑性**：bwrap sandbox 需要支持额外 root 路径，以兼容本地工具链；同时 CodeQL High 与 GHSA 安全修复被集中处理。代表：#5410、PR #5401。
- **国际化文案治理**：“Constitution”中文译名从“宪法”到“宪章”的讨论，以及 Web 端 i18n 字典 spine 重构，显示社区对中文文案准确性和多语言一致性的关注。代表：#4949、#5337、PR #5397。
- **架构与可靠性治理**：TUI crate 分解、工作流搜索并发上限去除硬编码、CI 并发取消问题修复，都指向社区对代码结构可维护性和 CI 可观测性的重视。代表：#5316、#5060、PR #5395。

---

## 开发者关注点

- **macOS 兼容性问题集中**：agent 输出乱码、PTY acceptance 卡住、`agy_credentials` 测试因 `/var` symlink 失败等，说明 macOS 路径仍是稳定性短板。
- **CI 经常性红色**：v0.9.8 provider-count 断言过期、Lint 被 clippy 缺陷阻塞、并发 main push 互相取消，都是开发者高频吐槽点。
- **成本展示和计费可见性**：升级后出现 `unverified_live_pricing` 且服务端 503，影响用户对实际消耗的感知。
- **终端渲染回归**：宽屏输出区被限制最大宽度，被认为是 v0.9 的体验回退，开发团队已通过恢复 identity 布局解决。
- **第三方模型配置成本仍然偏高**：尤其是在自托管和国内服务商场景下，用户希望“只填密钥即可用”的开箱体验。

> 以上链接均指向 Hmbown/CodeWhale。数据更新截止 2026-08-15，报告日期为 2026-08-16。

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

过去24小时无活动。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*