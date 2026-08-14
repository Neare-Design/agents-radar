# AI CLI 工具社区动态日报 2026-08-15

> 生成时间: 2026-08-14 23:14 UTC | 覆盖工具: 10 个

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

# AI CLI 工具横向对比分析报告（2026-08-15）

## 1. 生态全景

2026-08-15 的社区动态显示，AI CLI 工具已进入**功能高频迭代与稳定性修补并行**的阶段：Claude Code 日发两版、Codex 连续 5 个 alpha、Copilot 和 Qwen 同步发布正式版，新特性（Subagent fork、后台会话、GitLab MR、Web Shell 文件上传）落地速度极快。但稳定性问题同样集中爆发——Windows 性能回归、会话挂起/假死、上下文压缩丢失在多个工具中同时出现，工程化成熟度仍是共同短板。社区关注点高度趋同：MCP 生态、跨会话记忆、Agent 协作、企业治理成为跨工具高频议题。值得注意的是，Gemini CLI 的 SSR Agent 自动修复 PR 和 CodeWhale 的 Dependabot 批量依赖升级，预示着 **AI 自动化正在渗透开发工具自身的维护流程**。总体判断：AI CLI 正从"单机会话"走向"多 Agent 协作 + 企业级治理 + 跨平台稳定"的下一阶段。

## 2. 各工具活跃度对比

数据口径：GitHub Issues / PR / Releases 过去 24 小时动态。

| 工具 | 活跃 Issues | PR 动态 | Releases | 活跃度特征 |
|---|---|---|---|---|
| Claude Code | 50（精选 10） | 4 | 2 正式版（v2.1.232/233） | 功能迭代最快，社区反馈量大 |
| OpenAI Codex | 50 更新（精选 10） | 50 更新（重点 10） | 5 alpha（rust-v0.148.0-alpha.14~18） | Issue/PR 双高，但以预发布和缺陷修复为主 |
| Gemini CLI | 10 精选 | 10（SSR Agent 批量提交） | 1 nightly | 自动化修复占比高，核心问题聚焦子代理 |
| GitHub Copilot CLI | 31 | 3 | 2（v1.0.80 + v1.0.80-1） | 企业模型治理和 MCP 回归问题突出 |
| Kimi Code CLI | 4 | 0 | 无 | 低活跃，但记忆系统讨论深入（39 评论） |
| OpenCode | 10 精选 | 10 | 未提及 | 48-bit ID 时间戳回绕等底层缺陷受关注 |
| Pi | 10 精选 | 10 | v0.84.2 | 模型适配广，TUI 性能问题集中 |
| Qwen Code | 10 精选 | 10 | v0.21.12 + preview | Web Shell 迭代核心，CI 稳定性受关注 |
| DeepSeek TUI (CodeWhale) | 50（精选 10） | 18（重点 10） | v0.9.8（品牌统一） | 品牌迁移期，工程修复密度高 |
| Grok Build | 0 | 0 | 无 | 24h 无活动 |

**阅读提示**：Claude Code 和 OpenAI Codex 的 Issue/PR 活跃度显著领先；Kimi 和 Grok 处于低活跃状态，但 Kimi 的讨论深度（记忆系统 39 条评论）值得关注。

## 3. 共同关注的功能方向

| 方向 | 涉及工具 | 具体诉求 |
|---|---|---|
| **Windows / WSL 稳定性** | Claude Code、Codex、Copilot CLI、Pi | Git Bash 权限误报（Claude #86619）、CPU 忙循环与系统级卡顿（Codex #38547/#38554/#38583）、OOM 崩溃（Copilot #4499）、WSL 登录挂起（Pi #6187） |
| **MCP 工程化落地** | Claude Code、Copilot CLI、Codex、OpenCode | MCP_TIMEOUT 上限被忽略（Claude #16837）、OAuth issuer 校验回归（Copilot #4480/#4439）、MCP 协议发现指标（Codex PR #38634）、per-tool 执行超时（OpenCode PR #36869） |
| **上下文 / 记忆 / 压缩** | Kimi、Claude Code、Codex、Gemini、Pi | 跨会话记忆系统（Kimi #1283，39 评论）、图像处理失败浪费大量 token（Claude #60334）、压缩丢失操作连续性（Codex #29356）、Context 缓存失效（OpenCode #37489）、实验性 append compaction（Pi PR #8120） |
| **Agent 协作与子代理** | Claude Code、Gemini、Copilot、OpenCode、CodeWhale | Subagent fork 默认开启（Claude v2.1.232）、agents 递归调用（Gemini PR #28738）、MAX_TURNS 误报成功（Gemini #22323）、autopilot 子任务冻结（Copilot #4306）、多 subagent 渲染卡顿（OpenCode #42657）、32 字段 agent schema 过载（CodeWhale #5324） |
| **企业治理与可观测性** | Claude Code、Copilot、Codex、Pi | Admin API 不返回订阅用户（Claude #27780）、企业策略误禁用模型（Copilot #4422/#4390）、429 限流（Pi #7850）、OTLP 遥测 headers 缺失（Claude #82092） |
| **多模型 / Provider 兼容** | OpenCode、Pi、CodeWhale、Qwen | 自定义 provider 模型自动发现（OpenCode #27553）、GLM/Kimi 工具调用 422（OpenCode #42616）、第三方服务预制模板（CodeWhale #5350）、音频转录桥接（Qwen PR #8332） |

## 4. 差异化定位分析

- **Claude Code**：功能最前沿、迭代最激进的头部工具。Subagent fork、后台会话、GitLab MR 等新能力当天落地，面向深度使用 Agent 的专业开发者；但新版本引入的 Windows 回归也说明其快速迭代的代价。
- **OpenAI Codex**：OpenAI 官方 CLI，Rust 核心 + 桌面 App，模型通道与沙箱整合深。当前最大短板是 Windows 平台性能回归（26.810.x 系列被集中投诉）和长会话上下文压缩可靠性。
- **Gemini CLI**：与 Gemini 模型绑定最紧，工程文化最明显——SSR Agent 自动修复 PR 维持高合入效率，同时在建组件级评估体系（#24353）。子代理行为不透明是当前主要痛点。
- **GitHub Copilot CLI**：GitHub 生态桥头堡，多模型（Anthropic、GPT、Kimi）+ autopilot 多 agent 模式，企业治理功能最丰富但问题也最突出（模型被误禁用、MCP OAuth 回归）。定位是 IDE 和企业工作流的自然延伸。
- **Kimi Code CLI**：轻量聚焦，社区规模小，但围绕记忆系统的讨论（#1283 39 条评论）显示出对跨会话上下文的强烈需求；当前处于功能储备期。
- **OpenCode**：开源、provider 中立，强调 OpenAI-compatible 适配和配置自动化（动态模型发现 PR #42660），但中继层对 GLM/Kimi 等非 OpenAI 模型的工具调用兼容性仍在补课。
- **Pi**：独立开发者（badlogic）驱动的精品工具，模型接入速度极快（xAI Grok 4.6、DeepSeek V4、Kimi、Anthropic Vertex、Bedrock Mantle 同批落地），TUI 性能和跨平台细节打磨到位，但生态规模和团队资源有限。
- **Qwen Code**：阿里系，以 Web Shell 和渠道集成（钉钉）形成差异化，代码审查工作流（/review 轮次感知收敛、/audit 遗留代码审计）和端到端基准验证（SWE-bench → Terminal-Bench）是突出特色。
- **CodeWhale（原 DeepSeek TUI）**：DeepSeek 系，Rust 工程质量见长（PTY 泄漏、状态写入竞态等深层修复），本地模型路由（DS4）和 Dependabot 批量依赖升级体现其技术路线；品牌迁移期带来一定的升级阵痛。

## 5. 社区热度与成熟度

**最活跃梯队（每日多版本，双高 Issue/PR）**：Claude Code、OpenAI Codex、GitHub Copilot CLI。三者均有企业级功能布局（GitLab MR、Admin API、模型治理），是当前 AI CLI 的第一梯队。

**高活跃梯队（持续迭代，专项突破）**：Gemini CLI、Qwen Code、Pi、OpenCode、CodeWhale。Gemini 在自动化维护和评估体系上领先；Qwen 在 Web Shell 和发布验证上投入明确；Pi 和 OpenCode 是社区驱动的多模型探索者；CodeWhale 在品牌重塑期仍保持高密度修复。

**低活跃梯队**：Kimi Code（4 Issues、0 PR）、Grok Build（无活动）。前者在记忆系统讨论上积累了一定的社区共识，后者暂无可见进展。

**成熟度判断**：Claude Code 和 Copilot CLI 在企业功能完备性上领先；Codex 和 Gemini 有强大平台支撑，但稳定性口碑受 Windows/桌面端问题拖累；Pi、OpenCode、CodeWhale 迭代速度不输大厂，但社区基数和生态资源规模较小，更适合技术尝鲜者。

## 6. 值得关注的趋势信号

1. **Windows 成为竞争分水岭**：Codex 26.810.x 被集中报告 CPU 忙循环、系统卡顿和鼠标滞后；Claude Code 在 Git Bash 出现权限误报；Copilot 在 Windows 上 OOM；Pi 在 WSL 登录挂起。**参考价值**：Windows 用户在选型时应重点核查目标工具在 Windows 原生的真实反馈，必要时暂避已知回归版本（如 Codex 26.810.x）。

2. **上下文/记忆是下一代核心战场**：Kimi 记忆系统 39 条讨论、Codex 压缩丢操作、Pi 实验性 append compaction（命中 provider 缓存）、Qwen `/compress` 状态栏不刷新——跨会话上下文生产可用性正在成为刚需。**参考价值**：长期项目应优先选择记忆系统成熟或压缩策略可配置的工具，避免 token 浪费和"失忆"。

3. **MCP 从概念进入工程化深水区**：Copilot 的 OAuth issuer 回归（跨 1.0.79/1.0.80 两个版本）、Claude 的 MCP_TIMEOUT 上限被忽略、Codex 增加协议发现指标——说明 MCP 的认证、超时、可观测性开始被严肃对待。**参考价值**：使用 MCP 生态时需固定版本并测试 OAuth 流程；企业选型要考察协议的兼容性和容错能力。

4. **子代理/多 Agent 协作成为标配，但透明度不足**：Claude 默认开启 subagent fork、Gemini 允许 agents 调用 agents、Copilot autopilot 多 agent、OpenCode 支持并发 subagent、CodeWhale 简化 agent schema——各工具都在推进 Agent 分工，但 MAX_TURNS 被误报为成功、子代理绕过权限、冻结无响应等问题普遍存在。**参考价值**：在无人值守场景（CI、autopilot）中，需要额外关注工具的终止语义和可观测性，不要盲目信任"success"状态。

5. **企业治理与成本可预期性诉求上升**：Claude Admin API 数据缺口、Copilot 企业模型被误禁用、Pi 组织 429 限流、Claude 图像处理失败浪费 70% 窗口 token——企业用户在模型治理、用量统计和成本控制上的要求正从"可选"变为"必需"。**参考价值**：技术决策者在评估工具时，应明确询问 Admin API 能力、模型目录同步机制和成本可见性，而不是只看编码效果。

6. **AI 开始维护开发工具自身**：Gemini CLI 的 SSR Agent 自动提交修复 PR（10 个 PR 中占多数），CodeWhale 的 Dependabot 批量升级 Rust 依赖，Qwen 的 CI 失败由机器人自动跟踪（#9143）——AI 自动化正在进入开发工具的开发流程。**参考价值**：这是一个积极信号，说明头部工具团队正在用 AI 提升维护效率，未来 Issue 响应和 Bug 修复速度可能进一步加快；开发者可以关注这些自动化机制是否真正提升了迭代质量。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区热点报告（截至 2026-08-15）

> 数据来源：github.com/anthropics/skills。以下 PR 均为 **open** 状态；Issue 按评论数排序。

---

## 1. 热门 Skills 排行

**① skill-creator 评估链路修复** · [PR #1298](https://github.com/anthropics/skills/pull/1298) · open  
- 功能：修复 `run_eval.py` 始终报 0% recall 的核心问题，将评估产物安装为真实 skill，并修复 Windows 流读取、触发检测与并行 worker。  
- 热点：关联 #556/#1169 等多次复现，直接影响 skill 描述的自动优化循环，是当前开发工具链中最受关注的 PR。

**② document-typography 文档排版技能** · [PR #514](https://github.com/anthropics/skills/pull/514) · open  
- 功能：新增排版质量控制技能，解决 AI 生成文档的孤字、寡行、标题滞留页尾、编号错位等共性问题。  
- 热点：讨论集中在"用户很少主动要求但几乎所有生成文档都受影响"的高频痛点。

**③ PDF 技能大小写引用修复** · [PR #538](https://github.com/anthropics/skills/pull/538) · open  
- 功能：修正 `skills/pdf/SKILL.md` 中 8 处大小写不一致的文件引用（`REFERENCE.md`→`reference.md`、`FORMS.md`→`forms.md`）。  
- 热点：在大小写敏感文件系统上会直接导致 skill 资源加载失败，社区反馈强烈。

**④ ODT 文档技能** · [PR #486](https://github.com/anthropics/skills/pull/486) · open  
- 功能：新增 OpenDocument 格式（.odt/.ods）创建、填充、读取及 ODT→HTML 转换。  
- 热点：响应开源/ISO 标准文档格式需求，与现有 docx/pdf 技能形成互补。

**⑤ frontend-design 技能可执行性改进** · [PR #210](https://github.com/anthropics/skills/pull/210) · open  
- 功能：修订前端设计技能，使指令更清晰、可操作、内部一致，确保 Claude 能在单次对话内执行。  
- 热点：讨论核心是"技能指令不应是文档，而应是可执行的行为约束"。

**⑥ 技能质量/安全分析器** · [PR #83](https://github.com/anthropics/skills/pull/83) · open  
- 功能：新增 `skill-quality-analyzer` 和 `skill-security-analyzer` 两个元技能，从结构、文档、资源、安全等维度评估其他技能。  
- 热点：社区对技能生态的自我治理与质量门槛需求明显，与安全信任问题（#492）相互呼应。

**⑦ DOCX 修订 ID 冲突修复** · [PR #541](https://github.com/anthropics/skills/pull/541) · open  
- 功能：修复 DOCX 技能添加修订时 `w:id` 与已有书签/评论/移动区间冲突导致的文档损坏。  
- 热点：涉及 OOXML 共享 ID 空间的底层规范，是文档类技能的高价值稳定性修复。

**⑧ testing-patterns 测试模式技能** · [PR #723](https://github.com/anthropics/skills/pull/723) · open  
- 功能：新增综合测试技能，覆盖 Testing Trophy 模型、单元测试、React 组件测试、测试命名与边界条件。  
- 热点：社区对测试生成与测试最佳实践的需求显著，该 PR 提供了完整的测试栈指导。

---

## 2. 社区需求趋势

- **安全与信任边界**：[#492](https://github.com/anthropics/skills/issues/492)（43 评论）指出社区技能在 `anthropic/` 命名空间下分发，可能诱导用户授予过高权限。期望官方明确社区/Official 标识与审查机制。
- **组织级技能共享**：[#228](https://github.com/anthropics/skills/issues/228)（👍 8）希望支持组织内直接共享技能库，当前需手动下载上传，协作成本高。
- **技能开发工具链可靠性**：[#556](https://github.com/anthropics/skills/issues/556)（👍 7）与 [#1169](https://github.com/anthropics/skills/issues/1169) 反映 `skill-creator` 评估脚本在真实触发和 Windows 环境下不可用，跨平台兼容性是显著短板。
- **上下文窗口资源效率**：[#1487](https://github.com/anthropics/skills/issues/1487) 指出 `claude-api` 技能一次性注入约 156k tokens，导致上下文耗尽。技能需要更轻量的按需加载设计。
- **新技能方向**：
  - 测试生成/测试模式（对应 [PR #723](https://github.com/anthropics/skills/pull/723)）
  - 企业平台集成：ServiceNow（[PR #568](https://github.com/anthropics/skills/pull/568)）、SAP RPT（[PR #181](https://github.com/anthropics/skills/pull/181)）
  - 代理治理/安全模式（[#412](https://github.com/anthropics/skills/issues/412)）
  - 记忆压缩（[#1329](https://github.com/anthropics/skills/issues/1329)）
  - 推理质量门控（[#1385](https://github.com/anthropics/skills/issues/1385)）
  - 文档格式扩展：ODT（[PR #486](https://github.com/anthropics/skills/pull/486)）、排版控制（[PR #514](https://github.com/anthropics/skills/pull/514)）

---

## 3. 高潜力待合并 Skills

以下 PR 讨论活跃、内容完整且更新较近，存在近期落地可能：

- **ServiceNow 平台技能** · [PR #568](https://github.com/anthropics/skills/pull/568) · 更新至 2026-08-12  
  覆盖 ITSM、ITOM、SecOps、ITAM/SAM、FSM、CSDM 等全平台，企业级需求明确。

- **Pyxel 复古游戏开发技能** · [PR #525](https://github.com/anthropics/skills/pull/525) · 更新至 2026-07-15  
  基于 `pyxel-mcp`，提供"编写→运行→截图→迭代"的完整游戏开发工作流。

- **self-audit 交付审计技能** · [PR #1367](https://github.com/anthropics/skills/pull/1367) · 更新至 2026-07-02  
  先做机械文件验证，再做四维推理审计，与 #1385 提案形成完整质量门控体系。

- **plan-file-hygiene 规划文件卫生技能** · [PR #1479](https://github.com/anthropics/skills/pull/1479) · 更新至 2026-07-27  
  解决规划工件持续积累、无生命周期管理的问题，聚焦 Agent 长期运行后的上下文污染。

- **testing-patterns 测试模式技能** · [PR #723](https://github.com/anthropics/skills/pull/723) · 更新至 2026-04-21  
  内容完整度高，社区对测试类技能需求强，是近期最有潜力的新增技能之一。

- **ODT 文档技能** · [PR #486](https://github.com/anthropics/skills/pull/486) · 更新至 2026-04-14  
  创建较早但讨论热度持续，文档格式扩展是长期需求。

---

## 4. Skills 生态洞察

当前社区最集中的诉求是完善 Skills 的**"生产级基础设施"**——既包括 skill-creator 评估/优化工具链的可靠性与跨平台兼容性，也包括命名空间信任、组织级共享和上下文窗口效率等生态治理问题。

---

# Claude Code 社区动态日报

**日期：2026-08-15** | 数据来源：github.com/anthropics/claude-code


## 今日速览

过去 24 小时内 Claude Code 连续发布 v2.1.232 与 v2.1.233 两个版本，Subagent fork 机制正式默认开启、会话后台运行、GitLab MR 支持等多项体验升级落地。社区侧，Windows Git Bash 上的权限误报问题成为最新焦点，而长期高赞的「Enter 键换行」诉求仍以 147 👍 居需求榜首；同时一大批 cyber 安全过滤误报 Issue 被集中关闭，表明官方正在清理积压。

## 版本发布

### v2.1.233

- **GitLab MR 支持**：`--worktree` 标志与 `claude agents` 视图新增 GitLab Merge Request URL 支持（MR 显示为 `!N` 格式）。
- **用户身份转发**：Anthropic upstream 上的 apps gateway 新增可选的 `forward_user_identity` 设置，将登录用户身份以 header 形式发送至代理后端。

### v2.1.232

- **Subagent forking 正式默认开启**：`subagent_type: "fork"` 的子代理现在默认继承完整对话与 prompt cache。
- **后台运行**：交互式会话中非 teammate 的 agent 生成默认改为后台执行，不阻塞主流程。
- **@ 提及会话**：在 prompt 中输入 `@` 可按名称提及另一个 Claude 会话。

## 社区热点 Issues（10 个）

### 1. API 图像处理失败造成大量 token 浪费
[#60334](https://github.com/anthropics/claude-code/issues/60334) · CLOSED · 评论 73 · 👍 19

用户反馈：无图片场景下频繁出现 "an image in the conversation could not be processed and was removed" 错误，约烧掉 5 小时窗口的 70%。评论数居所有 Issue 之首，说明这是一个覆盖面极广的体验问题。

### 2. Enter 键应插入换行而非发送消息
[#2054](https://github.com/anthropics/claude-code/issues/2054) · OPEN · 评论 28 · 👍 147

CJK 用户的核心痛点：输入法中 Enter 用于确认候选词，经常误触发送不完整消息。147 个 👍 使其成为社区最高赞功能请求之一，已持续开放超过一年，官方尚未给出明确方案。

### 3. 桌面 App 增加取消归档会话功能
[#30869](https://github.com/anthropics/claude-code/issues/30869) · CLOSED · 评论 29 · 👍 57

用户希望桌面端支持将已归档的 Claude Code 会话恢复/取消归档，当前只能归档无法反向操作。属于高频工作流缺失功能。

### 4. Windows Git Bash 静态分析误报引发无休止权限弹窗（新）
[#86619](https://github.com/anthropics/claude-code/issues/86619) · OPEN · 评论 8 · 👍 9

2.1.232 起在 Windows Git Bash 上对只读 cd-compound 命令产生静态分析误报，触发无法抑制的权限确认弹窗，已在两台独立机器上复现。这是昨日新提交、与最新版本直接相关的 Bug，建议持续跟进。

### 5. MCP_TIMEOUT 超过 60 秒被忽略
[#16837](https://github.com/anthropics/claude-code/issues/16837) · OPEN · 评论 15 · 👍 16

用户在 Linux 平台设置 `MCP_TIMEOUT` 大于 60 秒时配置不生效，MCP 调用仍按 60 秒超时处理。长时间运行的 MCP 工具调用场景下影响明显。

### 6. Analytics Admin API 不返回订阅/OAuth 用户
[#27780](https://github.com/anthropics/claude-code/issues/27780) · OPEN · 评论 26 · 👍 23

企业用户无法通过 Admin API 获取订阅/OAuth 用户的分析数据，导致用量统计不完整，影响团队成本管理。

### 7. Apps gateway OTLP telemetry 因缺少 headers 被拒
[#82092](https://github.com/anthropics/claude-code/issues/82092) · OPEN · 评论 13 · 👍 5

Claude Desktop 的遥测数据 flush 因 `otlpHeaders` 缺失被网关拒绝（`missing_token`），导致桌面端 telemetry 全部失效。涉及可观测性基础设施的配置缺陷。

### 8. VSCode 扩展：增加 "Background Tasks" 面板
[#75863](https://github.com/anthropics/claude-code/issues/75863) · OPEN · 评论 6 · 👍 8

社区希望 VSCode 扩展与桌面 App 对齐，增加后台任务面板以查看/manage 并行 agent 任务，IDE 集成完善度是当前重点诉求之一。

### 9. Web/App 界面提示建议无法关闭
[#66117](https://github.com/anthropics/claude-code/issues/66117) · OPEN · 评论 9 · 👍 10

用户希望在 Claude.ai web/app 界面中禁用 prompt 建议，目前没有开关选项，影响专注编码场景。

### 10. 浏览器自动化工具与 Web Sandbox 代理不兼容
[#11791](https://github.com/anthropics/claude-code/issues/11791) · OPEN · 评论 11 · 👍 16

Playwright/Puppeteer/Selenium 因 sandbox 安全代理不支持 HTTPS CONNECT 隧道而无法运行。属于架构性限制，社区呼吁官方至少补充文档说明。

**其他值得注意的动态**：sworrl 用户批量提交/关闭了 20+ 条 cyber/aup 安全过滤误报 Issue（涉及无人机固件逆向分析被拦截等），大多数已标记为 duplicate/stale 并关闭，建议关注官方后续是否会调整安全过滤策略的发布说明。

## 重要 PR 进展（全部 4 条）

### 1. 保留 Python 探测错误诊断信息
[#86746](https://github.com/anthropics/claude-code/pull/86746) · OPEN

修复 `sg-python.sh` 将 stderr 重定向到 `/dev/null` 的问题。此前当 `python3`/`python`/`py -3` 全部失败时用户只能看到泛化错误；此 PR 保留探测错误信息，便于定位 Python 环境问题。

### 2. 为 CLI 添加 bash/zsh/fish 自动补全
[#86626](https://github.com/anthropics/claude-code/pull/86626) · OPEN

新增 `completions/` 目录，提供 bash（兼容 macOS 自带 3.2）、zsh、fish 三套补全脚本及安装说明，且与已安装 CLI 保持同步。目前 PR 仍为草稿状态。

### 3. 新增 pylint CI 工作流
[#83890](https://github.com/anthropics/claude-code/pull/83890) · OPEN

提交了 `pylint.yml`，计划为仓库引入 pylint 静态检查。已开放约 10 天，无评论反馈，关注度较低。

### 4. 为 Claude Code 补充缺失的 source
[#41611](https://github.com/anthropics/claude-code/pull/41611) · OPEN

个人开发者提交的文档/源码补充 PR，已开放近 5 个月，无实质讨论，疑为低优先级。

## 功能需求趋势

从全部 50 条活跃 Issue 中提炼出以下社区重点关注方向：

| 方向 | 代表 Issue | 热度信号 |
|---|---|---|
| **输入交互与 CJK 支持** | #2054 Enter 换行（👍147） | 最高赞需求，持续发酵超一年 |
| **安全过滤误报（cyber/aup）** | #71986/#71896 等 20+ 条 | 同一用户批量反馈，规模集中 |
| **IDE/桌面端集成完善** | #75863 VSCode 背景任务、#30869 取消归档 | 桌面端与 VSCode 功能对齐诉求 |
| **MCP 配置灵活性** | #16837 MCP_TIMEOUT 上限问题 | 被忽略的超时配置 |
| **可观测性与成本管理** | #27780 Admin API 不完整、#82092 OTLP headers、#60334 token 浪费 | 企业级管理能力缺口 |
| **沙箱与代理兼容性** | #11791 浏览器自动化不兼容 | 架构性限制，推进缓慢 |
| **界面细节控制** | #66117 关闭 prompt 建议、#72707 长 prompt 无法折叠 | 细致体验打磨需求 |

## 开发者关注点

1. **新版本稳定性**：2.1.232 引入的 Windows Git Bash 静态分析误报（#86619）是昨日最值得警惕的新问题，影响日常操作且无法通过配置抑制。升级用户需留意。
2. **token 浪费与成本**：#60334 中图像处理失败大量消耗窗口额度，加上 Max 20x 升级后周限额未同步生效（#79773）的反馈，成本可预期性成为企业用户的突出焦虑。
3. **安全过滤误报成规模出现**：sworrl 一人提交的 20+ 条 cyber/aup 误报 Issue 横跨固件逆向、解密工具、普适 web 开发等场景，且均被标记为「session-halted」，此类误杀对合法开发工作的阻断成本极高——好在多数已关闭，官方或已有治理动作。
4. **长时间未解决的高赞需求**：#2054（Enter 换行）与 #16837（MCP_TIMEOUT）已开放超过一年/半年，高赞高评论但无明确排期，CJK 用户与 MCP 重度用户的耐心在被持续消耗。
5. **PR 社区参与度偏低**：当前 4 条 PR 全部无评论，社区贡献主要集中在 Issue 侧；补全脚本（#86626）是其中最有实用潜力的一个。

---

*本日报由 AI 技术分析师基于 GitHub 公开数据自动整理生成，不构成官方立场。*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报（2026-08-15）

## 今日速览

- 过去 24 小时，OpenAI 密集发布了 `rust-v0.148.0-alpha.14` 至 `alpha.18` 共 5 个预发布构建，均为测试通道更新。
- Windows 平台性能问题成为社区焦点：最新版 `26.810.4967` 被集中报告引发系统级 CPU 忙循环、整机卡顿和鼠标滞后，与之前 `26.803.x` 版本形成明显回归。
- 一批以稳定性为目标的 PR 合并，涵盖 Windows 沙箱安全加固、TUI 启动输入处理、gRPC code-mode 通知优化等。

## 版本发布

**rust-v0.148.0-alpha.14 → alpha.18**（共 5 个）

- 均为 Rust 版 Codex CLI 的 alpha 预发布版本，官方暂未提供详细变更日志，适合关注新特性验证的开发者跟踪。
- 查看全部 Release：[github.com/openai/codex/releases](https://github.com/openai/codex/releases)

## 社区热点 Issues

过去 24 小时有 50 条 Issues 更新，以下是最值得关注的 10 条：

**1. Codex App 在 Windows 11 Pro 上频繁冻结/卡顿**
[Issue #20214](https://github.com/openai/codex/issues/20214) · 84 👍 · 101 评论
长期未解决的高热度问题。用户报告在系统资源充足的情况下，App 仍然频繁冻结，且问题从 4 月持续至今。

**2. Windows 空转时 Electron 主进程 CPU 忙循环（26.810.4967 回归）**
[Issue #38547](https://github.com/openai/codex/issues/38547) · 5 👍 · 11 评论
用户确认更新到 `26.810.4967.0` 后，App 在完全空闲状态下进入持续 CPU 忙循环，回滚到 `26.803.10989.0` 后消失。该版本疑似存在严重回归。

**3. 新版本使整台 PC 卡顿——退出 Codex 立即恢复**
[Issue #38554](https://github.com/openai/codex/issues/38554) · 3 👍 · 7 评论
同样指向 `26.810.4967.0`：安装该更新后整个系统出现卡顿（stutter），完全退出 Codex 后症状立刻消失。

**4. Windows 11 系统级鼠标滞后与 ~10% 空闲 CPU**
[Issue #38583](https://github.com/openai/codex/issues/38583) · 6 👍 · 10 评论
ChatGPT/Codex 更新到 `26.813.12317` 后，在空闲状态下造成持久系统级鼠标滞后和约 10% CPU 占用，影响整个桌面体验。

**5. Windows 桌面造成输入延迟**
[Issue #28855](https://github.com/openai/codex/issues/28855) · 20 👍 · 17 评论
App 在启动后和运行期间造成间歇性系统输入延迟，鼠标和键盘输入出现明显滞后，问题在清理日志、禁用插件后依然存在。

**6. macOS 版本极度不稳定：高 CPU 占用、反复崩溃**
[Issue #38637](https://github.com/openai/codex/issues/38637) · 0 👍 · 4 评论
`26.810.41047` 在 macOS arm64 上短短几分钟内即崩溃，打开长对话几乎不可能，用户要求回滚。

**7. Windows taskkill.exe/conhost.exe 清理风暴耗尽 WMI**
[Issue #34260](https://github.com/openai/codex/issues/34260) · 11 👍 · 35 评论
桌面端进入无界进程清理循环，数百个 `taskkill.exe` 与 `conhost.exe` 残留进程反复查询 WMI，耗尽配额，拖垮整个系统。

**8. 上下文压缩丢失操作连续性**
[Issue #29356](https://github.com/openai/codex/issues/29356) · 1 👍 · 21 评论
长时间任务中自动上下文压缩会丢失关键操作步骤，导致 Codex 在任务中途"失忆"。用户建议保留最后 5 个操作步骤的原文。

**9. Windows 沙箱无法启动 MSIX Store 版 PowerShell**
[Issue #35871](https://github.com/openai/codex/issues/35871) · 3 👍 · 14 评论
当解析到的 shell 是 MSIX（Microsoft Store）版 PowerShell 7 时，沙箱以 `CreateProcessAsUserW failed: 5 (Access denied)` 失败。Windows 拒绝在受限令牌下启动打包应用。

**10. Android 远程连接卡在 "Waiting for desktop…"**
[Issue #22733](https://github.com/openai/codex/issues/22733) · 19 👍 · 16 评论
从 ChatGPT Android 应用发起远程 Codex 会话时一直卡在等待界面。该问题自 5 月报告，至今仍有大量用户受影响。

## 重要 PR 进展

过去 24 小时有 50 条 PR 更新，以下 10 个 PR 值得关注：

**1. Windows 沙箱强制执行 deny-read 规则**
[PR #38660](https://github.com/openai/codex/pull/38660)
确保 Windows 沙箱在每条执行路径和 setup 刷新时保留托管的文件系统 deny 规则；不支持的策略应 fail closed，而不是在未受保护的情况下运行命令。

**2. Code Mode 类型中解析本地 JSON Schema 引用**
[PR #38664](https://github.com/openai/codex/pull/38664)
修复 Code Mode 渲染时将文档内 `$ref` 解析为 `unknown` 的问题，使生成的 TypeScript 声明能正确呈现输入和结构化输出类型。

**3. gRPC code-mode 通知不再截断**
[PR #38645](https://github.com/openai/codex/pull/38645)
移除此前 1,024 字节的通知长度限制和截断后缀，包含超长多字节文本的通知也能完整送达会话。

**4. 加固 TUI 启动输入处理**
[PR #38641](https://github.com/openai/codex/pull/38641)
避免终端探测等启动工作残留的按键或部分控制序列在交互界面出现前被误选中或执行，同时保留用户有意输入的 typeahead。

**5. 添加 MCP 协议发现指标**
[PR #38634](https://github.com/openai/codex/pull/38634)
为 MCP 客户端协议发现增加计数器和耗时统计，按 legacy/auto 模式标记，并分类为 modern/legacy/failure 结果。

**6. 移除 gRPC code-mode 打开会话数限制**
[PR #38630](https://github.com/openai/codex/pull/38630)
允许 gRPC code-mode host 注册超过原 `MAX_IN_FLIGHT_REQUESTS` 的并发会话；in-flight 请求、控制请求和 active cells 的既有限制保持不变。

**7. 权限配置文件快照移入协议**
[PR #38651](https://github.com/openai/codex/pull/38651)
将 `PermissionProfileSnapshot` 定义为协议模型并从 `core-api` 重新导出，核心权限状态直接存储快照，同时保持对具体 `PermissionProfile` 的约束应用。

**8. 无链接时跳过终端超链接布局计算**
[PR #38657](https://github.com/openai/codex/pull/38657)
当提供的行中不包含超链接元数据时提前返回，避免不必要的段落布局计算，降低终端渲染开销。

**9. Guardian v2 风险分类可配置化**
[PR #38628](https://github.com/openai/codex/pull/38628)
`features.guardianv2` 现在可以是布尔开关，也可以配置分类器指令、审查阈值、推理力度以及 action/instruction token 上限。

**10. Thai 组合字符逐个删除**
[PR #38662](https://github.com/openai/codex/pull/38662)
将 Thai 语元音和声调符号视为独立退格边界，允许用户逐个删除组合标记，而不是整个字素簇。

## 功能需求趋势

从近期 Issues 中可以提炼出社区最关注的三个功能方向：

- **会话与上下文管理的精细化控制**：用户要求在长时间任务中保留关键操作步骤（[#29356](https://github.com/openai/codex/issues/29356)）、跨工作区进行经过清理的任务交接（[#34582](https://github.com/openai/codex/issues/34582)）、添加 `/cd` 命令切换会话工作目录（[#38585](https://github.com/openai/codex/issues/38585)）。这反映出 Codex 在复杂、长路径任务中的上下文保持能力距离生产级仍有差距。

- **Windows/WSL 执行环境的灵活选择**：开发者希望在 Windows 原生与 WSL 环境间按项目和聊天切换（[#36098](https://github.com/openai/codex/issues/36098)），并在 Chrome 侧边栏中选择本地 Codex 项目（[#32610](https://github.com/openai/codex/issues/32610)）。跨环境开发工作流已成为刚需。

- **更稳定的远程连接与模型通道**：Android 远程会话长时间卡在等待界面（[#22733](https://github.com/openai/codex/issues/22733)），以及 GPT-5.6 在 Amazon Bedrock 上不支持 Ultra 推理（[#37160](https://github.com/openai/codex/issues/37160)），表明多端、多模型的接入可靠性仍是重要痛点。

## 开发者关注点

- **Windows 性能回归是当前最大痛点**：26.810.x 系列被多个独立报告指向 CPU 忙循环、系统级鼠标/输入滞后、整机卡顿等问题（[#38547](https://github.com/openai/codex/issues/38547)、[#38554](https://github.com/openai/codex/issues/38554)、[#38583](https://github.com/openai/codex/issues/38583)）。社区情绪明显，已有用户要求回滚版本，希望官方尽快确认回归来源并修复。

- **进程与资源清理机制需要重设计**：taskkill/conhost 清理风暴（[#34260](https://github.com/openai/codex/issues/34260)）、`logs_2.sqlite` 无限增长（[#35823](https://github.com/openai/codex/issues/35823)）、Git 轮询引发的句柄泄漏（[#35775](https://github.com/openai/codex/issues/35775)）——多起报告指向底层的进程生命周期和存储管理存在系统性缺陷。

- **上下文压缩可靠性不足**：有用户报告压缩成功率约 85% 会断开连接（[#31375](https://github.com/openai/codex/issues/31375)），即使成功也会丢失重要的推理过程和操作步骤（[#29356](https://github.com/openai/codex/issues/29356)）。这对长跑任务影响极大。

- **沙箱与权限体系在 Windows 上兼容性待加强**：MSIX PowerShell 启动失败（[#35871](https://github.com/openai/codex/issues/35871)）以及 Full access 模式触发严重 UI 滞后（[#38666](https://github.com/openai/codex/issues/38666)），说明 Windows 沙箱在特权模型和进程隔离上仍需工程投入。

---

*本日报数据来源于 [github.com/openai/codex](https://github.com/openai/codex) 公开 Issues、PRs 和 Releases，基于过去 24 小时的更新生成。*

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 — 2026-08-15

## 今日速览

昨日发布 v0.56.0-nightly.20260814 版本，重点修复了容量错误的静默重试机制与 e2e 测试稳定性。社区方面，Subagent 达到 MAX_TURNS 却被错误报告为成功（#22323）成为最热门 Issue 并已有对应 PR 修复；同时，一批由 SSR Agent（自动修复机器人）提交的小型修复 PR 集中合入，涵盖 TUI 挂起、子代理上下文丢失、MessageBus 静默失败等多项稳定性问题。

## 版本发布

**v0.56.0-nightly.20260814.gc0d192452**
- 测试：稳定文件系统交互的 e2e 测试（#28793）
- 核心：实现上下文感知的静默重试与容量错误的可用性 TTL（#28761）

来源：https://github.com/google-gemini/gemini-cli/releases

## 社区热点 Issues

本期挑选 10 个最受关注或讨论最激烈的 Issue：

1. **[#22323] Subagent 达到 MAX_TURNS 后恢复被误报为 GOAL 成功**（评论 12，👍 2）
   `codebase_investigator` 子代理明明因达到最大轮次而中断，却对外报告 `status: "success"` 和 `Termination Reason: "GOAL"`，掩盖了真实的截断原因。此问题直接误导用户判断任务是否真正完成，社区反应强烈，已有对应 PR #28815 提出修复。https://github.com/google-gemini/gemini-cli/issues/22323

2. **[#21409] Generalist agent 无限挂起**（评论 8，👍 8）
   当 CLI 委托给 generalist agent 时，即使简单如创建文件夹也会永久挂起，用户最长等待一小时。该 Issue 是当前获 👍 最多的未解决问题，社区普遍认为这严重影响日常使用，需要优先处理。https://github.com/google-gemini/gemini-cli/issues/21409

3. **[#25166] Shell 命令执行完成后卡在 "Waiting input"**（评论 4，👍 3）
   Gemini 执行完简单 CLI 命令后仍显示命令活跃并等待输入，导致会话挂起。该问题频繁复现，涉及非常简单的命令，说明 shell 执行状态机存在缺陷。https://github.com/google-gemini/gemini-cli/issues/25166

4. **[#21983] Browser subagent 在 Wayland 下失败**（评论 4，👍 1）
   Browser Agent 在 Wayland 环境下无法正常工作，以 `Termination Reason: GOAL` 结束，但实际并未完成目标，暴露了跨显示服务器的兼容性问题。https://github.com/google-gemini/gemini-cli/issues/21983

5. **[#19873] 利用模型原生 bash 能力：零依赖 OS 沙箱与执行后意图路由**（评论 8，👍 1）
   提出 Gemini 3 模型天然擅长 POSIX 工具链，建议通过零依赖沙箱让模型以原生 bash 方式操作，同时保证安全性和 UX。该方向可能大幅提升代码库探查效率，是值得关注的架构级增强提案。https://github.com/google-gemini/gemini-cli/issues/19873

6. **[#24353] 组件级评估体系（EPIC）**（评论 7）
   追踪 76 个行为评估测试在 6 个 Gemini 模型上的运行情况，目标是建立更细粒度的评估体系，被视为提升整体质量的关键基础设施。https://github.com/google-gemini/gemini-cli/issues/24353

7. **[#22745] AST 感知的文件读取、搜索与代码库映射评估（EPIC）**（评论 7，👍 1）
   探讨利用抽象语法树（AST）精确读取方法边界、减少 token 噪声、优化导航，可能为代码库分析工具带来质的提升。https://github.com/google-gemini/gemini-cli/issues/22745

8. **[#21968] Gemini 不主动使用 skills 和 sub-agents**（评论 6）
   用户反映模型几乎不会主动调用自定义 skills 和子代理，即使描述明确相关。这是 agent 自主性的核心痛点，直接影响自定义工作流的效果。https://github.com/google-gemini/gemini-cli/issues/21968

9. **[#26522] Auto Memory 无限重试低信号会话**（评论 5）
   Auto Memory 只将成功读取的会话标记为已处理，导致低价值会话被反复重试，浪费资源。https://github.com/google-gemini/gemini-cli/issues/26522

10. **[#26525] Auto Memory 需添加确定性脱敏并减少日志**（评论 4）
    自动记忆服务将本地转录内容发送给模型，但脱敏发生在内容进入模型上下文之后；此外服务可能记录敏感信息，存在隐私风险。https://github.com/google-gemini/gemini-cli/issues/26525

## 重要 PR 进展

以下 10 个 PR 反映了当前最值得关注的修复与功能开发：

1. **[#28815] 修复 Subagent 恢复时保留原始终止原因**（SSR Agent 修复 #22323）
   当子代理在最终恢复轮次调用 `complete_task` 时，不再将 `MAX_TURNS`/`TIMEOUT` 覆盖为 `GOAL` 成功。https://github.com/google-gemini/gemini-cli/pull/28815

2. **[#28812] 修复 TUI 在裸 Linux 终端无限挂起**（SSR Agent 修复 #21477）
   通过为 `getProcessInfo()` 等调用添加执行超时，解决 CLI 在初始化阶段卡死的问题。https://github.com/google-gemini/gemini-cli/pull/28812

3. **[#28817] 保留执行中子代理工具调用的 hook 状态**（SSR Agent 修复 #22589）
   修复非根调度器（子代理）的 `Executing` 状态工具调用被过滤掉的问题，确保 hook 能正确追踪。https://github.com/google-gemini/gemini-cli/pull/28817

4. **[#28816] 修复 MessageBus.request 发布失败时的静默挂起**（SSR Agent 修复 #22588）
   浮动 Promise 未注册失败处理，导致 `publish()` 拒绝时请求静默挂起 60 秒，现已修复。https://github.com/google-gemini/gemini-cli/pull/28816

5. **[#20916] 修复 ShellExecutionService 的 PTY 文件描述符泄漏**
   修复 PTY 主文件描述符未正确关闭的问题，避免 macOS 长会话中 `kern.tty.ptmx_max` 耗尽（上限 511）。https://github.com/google-gemini/gemini-cli/pull/20916

6. **[#27154] 修复 PTY 内存泄漏：同步删除活跃条目**
   此前 `activePtys.delete()` 嵌套在 Promise `.then()` 中，若后台日志流异常则永远不执行，导致内存和 fd 持续泄漏。https://github.com/google-gemini/gemini-cli/pull/27154

7. **[#28738] 允许 agents 调用 agents**
   通过 `tools:` frontmatter 让子代理可以委派给其他子代理或递归调用自身，解决了 #22092，具有重要意义的能力扩展。https://github.com/google-gemini/gemini-cli/pull/28738

8. **[#25378] 修复 Windows 上 ripgrep 的 spawn EFTYPE 错误**
   当下载的二进制与主机架构不匹配（如 ARM 与 x64）时，`grep_search` 在 Windows 上失败，此 PR 增加架构检测与降级方案。https://github.com/google-gemini/gemini-cli/pull/25378

9. **[#27588] 支持 WSL2 剪贴板图片粘贴**
   在 WSL 环境中通过 PowerShell interop 读取 Windows 剪贴板并保存 PNG，补齐了 WSL2 下缺失的图片粘贴能力。https://github.com/google-gemini/gemini-cli/pull/27588

10. **[#28597] 先加载环境变量再解析设置占位符**
    修复 settings 加载顺序的竞态条件——此前 `.env` 尚未加载就展开 `process.env` 占位符，导致配置解析错误。https://github.com/google-gemini/gemini-cli/pull/28597

## 功能需求趋势

从近期 Issues 和 PR 中可提炼出社区最关注的几个方向：

- **Agent 自主性与协作**：子代理之间能否互相调用、模型是否会主动使用 skills/sub-agents、Agent 的自我认知（CLI 参数、快捷键、自我执行能力）是社区最高频的讨论话题。
- **记忆系统可靠性与安全**：Auto Memory 的重试逻辑、确定性脱敏、无效补丁隔离等议题集中出现，说明记忆功能已进入精细化打磨阶段。
- **终端与交互稳定性**：TUI 挂起、shell 卡死、终端 resize 闪烁、外部编辑器退出后的界面损坏等问题持续困扰用户，稳定性成为普遍诉求。
- **评估与质量基础设施**：组件级评估体系（EPIC #24353）、行为测试策略调整（#28818）等显示社区对可量化的质量保证体系有明确需求。
- **AST 感知工具链**：利用 AST 精确读取、搜索和映射代码库是新兴的探索方向，可能成为未来代码分析效率提升的关键。

## 开发者关注点

- **挂起与卡死问题频发**：Generalist agent 挂起、Shell 等待输入、TUI 初始化卡死等多类挂起问题频繁出现，是当前最影响体验的痛点。
- **子代理行为不透明**：MAX_TURNS 被误报为 GOAL 成功、bug 报告不包含子代理上下文、subagent 轨迹无法通过 `/chat share` 分享——开发者希望更透明地观察子代理的执行过程。
- **技能与子代理利用率不足**：模型不会主动调用已配置的 skills 和 sub-agents，导致自定义工作流形同虚设，用户呼吁改善 agent 的自主决策能力。
- **权限与安全边界**：包括子代理绕过权限设置（#22093）、破坏性 git 操作（#22672）、Auto Memory 的隐私脱敏（#26525）——开发者既希望 agent 更强大，也要求更强的安全护栏。
- **跨平台兼容性**：Wayland 下浏览器 agent 失败、Windows ripgrep 架构错误、WSL2 剪贴板缺失——Linux 与 Windows 生态的兼容问题仍待解决。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报 — 2026-08-15

## 今日速览

昨日连续发布 v1.0.80 与修复版 v1.0.80-1，更新模型配置并修复若干问题。社区热度聚焦两件事：一是 Atlassian/GitLab MCP OAuth 在 1.0.79/1.0.80 出现 RFC 8414 issuer 校验回归（#4480、#4439、#4490），二是企业账户下 Claude 系列模型被误禁用或从目录中消失（#4422、#4390），多个 issue 已发酵一周以上仍未解决。

## 版本发布

### v1.0.80（2026-08-14）
- 更新模型配置（Update model configurations）
- 发布链接：https://github.com/github/copilot-cli/releases

### v1.0.80-1（2026-08-14）
- 修复补丁版本（Fixes and changes）
- 发布链接：https://github.com/github/copilot-cli/releases

## 社区热点 Issues

话题集中在 MCP OAuth 回归、企业模型可用性和 autopilot 稳定性三个方向。以下按关注度排序：

### 1. Reasoning effort 'medium' 不被 claude-haiku-4.5 支持
- **Issue #4345**（6 评论，4 👍）
- 当 `copilot_cli_opus_medium_effort_default` 与 `copilot_cli_gpt_5_4_mini_for_explore` 两个服务端 feature flag 同时启用时，sub-agent 执行反复报错，阻断任务流。
- 影响面：模型配置组合下的高频崩溃，严重影响依赖 sub-agent 的复杂 workflow。
- https://github.com/github/copilot-cli/issues/4345

### 2. 组织已启用模型缺失于模型目录（Claude Sonnet 5/Opus 5、Kimi K3）
- **Issue #4390**（6 评论，4 👍）
- Copilot Business 组织在设置中显式启用的模型未出现在 CLI 有效模型目录中。选择 `claude-sonnet-5` 时提示 "This model is disabled by your..."。
- 影响面：企业用户无法使用已授权的模型，涉及 Anthropic 全系和 Kimi K3。
- https://github.com/github/copilot-cli/issues/4390

### 3. Atlassian MCP OAuth 失败——1.0.79 回归
- **Issue #4480**（4 评论，6 👍）
- 升级至 1.0.79 后，连接 Atlassian 远程 MCP 服务器在 OAuth 发现阶段失败：`Incompatible authorization server: authorization server advertised an issuer that does not match the URL its metadata was discovered from`。1.0.71 正常。
- 社区反响：获得今日最高 👍 数，且 1.0.80 中仍有同类报告（见 #4490），说明修复未彻底。
- https://github.com/github/copilot-cli/issues/4480

### 4. 个人企业账号下所有 Claude 模型不可用
- **Issue #4422**（3 评论，3 👍）
- 昨天还能用的 Claude sonnet 5/4.8 等模型，今天全部报 "This model is disabled"；回滚 CLI 版本无效，GitHub 设置中显示已启用。
- 影响面：个人企业账户大面积模型不可用，已持续 5 天。
- https://github.com/github/copilot-cli/issues/4422

### 5. GitLab MCP OAuth 同样遭遇 RFC 8414 issuer 校验失败
- **Issue #4439**（3 评论，2 👍）
- Copilot CLI 1.0.79 对 GitLab Self-Managed MCP server 的 OAuth 2.0 Dynamic Client Registration 认证失败，报同样的 RFC 8414 §3.3 issuer 不匹配。
- 与 #4480 同源，说明这是通用 MCP OAuth 回归而非 Atlassian 特有问题。
- https://github.com/github/copilot-cli/issues/4439

### 6. Autopilot 模式子任务冻结无响应
- **Issue #4306**（3 评论，2 👍）
- `/fleet use` 多 agent 循环场景下，会话运行至某点出现子任务冻结，不再产生任何输出或 prompt。
- 影响面：长时间 autopilot 任务的可靠性问题，已报告两周以上。
- https://github.com/github/copilot-cli/issues/4306

### 7. MCP registry 策略返回 403，CI 中非默认 MCP 服务器全部不可用
- **Issue #4346**（2 评论，3 👍）
- GitHub Actions 中使用内置 `GITHUB_TOKEN` 认证（copilot-requests: write）时，MCP 全部服务器策略拉取返回 403，导致 CI 中所有非默认 MCP 服务器被阻断。
- 影响面：官方推荐的 PAT-less Actions 集成方案在 MCP 场景下不可用。
- https://github.com/github/copilot-cli/issues/4346

### 8. 请求支持 protobuf OTLP 导出
- **Issue #2934**（2 评论，6 👍）
- Copilot CLI 的 OpenTelemetry 导出仅支持 `application/json`，`OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf` 被静默忽略。
- 虽然评论区关注不多，但 6 个 👍 说明社区对可观测性标准化有明确需求，且该 issue 已持续近 4 个月。
- https://github.com/github/copilot-cli/issues/2934

### 9. v1.0.79 autopilot 模式致命 OOM 崩溃
- **Issue #4499**（0 评论）
- `copilot.exe` 在长时 autopilot 会话中崩溃：`FATAL ERROR: Committing semi space failed`。崩溃时 V8 堆仅用 607 MB / 4.3 GB，是宿主内存提交失败而非堆限制。
- 影响面：Windows 上长会话稳定性的严重问题，值得关注。
- https://github.com/github/copilot-cli/issues/4499

### 10. 多会话/VS Code 打开时插件更新失败（Access denied）
- **Issue #4488**（1 评论）
- 当其他 Copilot CLI 会话或 VS Code 窗口打开时（即使插件未被使用），插件更新被文件锁阻断，报 "Access is denied"。
- 影响面：日常开发中多窗口并行的场景非常普遍，此问题会频繁触发。
- https://github.com/github/copilot-cli/issues/4488

## 重要 PR 进展

过去 24 小时内共 3 个 PR，均来自维护者 mrecachinas，围绕 PR 自动化迁移：

### 1. PR #4449 — 将 PR 自动化迁移出 pull_request_target（已合并）
- 将 invalid-label 自动化从 `pull_request_target` 迁移，直接使用 issue-scoped token 关闭无效 issue，用无权限的 `pull_request` 信号处理可合并 PR，权限操作移至受信任的 workflow-run 元数据。
- 安全性改进：消除 `pull_request_target` 的权限提升风险。
- https://github.com/github/copilot-cli/pull/4449

### 2. PR #4497 — 处理 fork PR 关联缺失场景（开启）
- 当 GitHub 未填充 workflow run 的 PR 关联时，writer 改用受信任的 workflow-run 元数据搜索，要求恰好匹配一个 open PR 才继续。
- 补全 #4449 迁移后的边界情况。
- https://github.com/github/copilot-cli/pull/4497

### 3. PR #4496 — 验证 PR workflow 迁移的临时 canary（已关闭）
- 仅含文档文件的临时 PR，用于验证 fork 来源 PR 的自动化行为，验证后即关闭。
- https://github.com/github/copilot-cli/pull/4496

## 功能需求趋势

从全部 31 条 issues 中提炼出以下社区关注方向：

**1. 新模型支持与模型配置（最高频）**
- GPT-5.6 `reasoning.mode` 参数支持（#4495）
- claude-haiku-4.5 的 reasoning effort 兼容（#4345）
- 新模型启用后本地缓存不刷新（#4494）

**2. MCP 生态兼容性**
- OAuth 元数据发现回归（#4480、#4439、#4490）
- tools/list 分页未遵循（#4006）
- MCP server 名称大小写冲突（#4478）

**3. 企业策略与模型治理**
- 企业策略误禁用模型（#4422、#4481）
- 启用模型未同步至 CLI 目录（#4390）

**4. 插件生态**
- 插件依赖声明与自动安装机制（#4487）
- 插件更新文件锁冲突（#4488）

**5. 会话生命周期与稳定性**
- 停止操作导致整个会话丢失（#4477）
- `/restart` 在 `-w` 会话中冲突（#4493）
- 会话恢复后 agent 未被自动选中（#4489）

**6. 可观测性与权限配置**
- protobuf OTLP 导出（#2934）
- `allowed_directories` 对 shell 命令不生效（#4482）

## 开发者关注点

**痛点与高频反馈：**

- **MCP OAuth 回归最受关注**：#4480 获得今日最高 👍（6），且 #4439、#4490 为同类问题，横跨 1.0.79 和 1.0.80 两个版本，用户已多次报告 "worked in 1.0.78"——回归时间清晰，修复优先级应提高。

- **企业模型可用性问题持续发酵**：Claude 全部模型被禁（#4422）和启用模型未同步（#4390）两个问题已分别持续 6 天和 9 天，涉及企业付费用户核心使用场景。

- **autopilot 稳定性依然是信任瓶颈**：子任务冻结（#4306，持续 16 天）和 OOM 崩溃（#4499）都在长时无人值守场景中出现，这会直接影响用户对 agent 自治能力的信任。

- **权限配置语义不统一**：`allowed_directories` 对 shell 命令不生效（#4482）与 `add-dir` 行为不一致，用户需手动绕过。

- **跨会话状态管理粗糙**：会话恢复丢 agent（#4489）、停止操作丢整个会话（#4477）、插件更新互相锁文件（#4488），三个问题都反映多会话场景下的状态管理仍需打磨。

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报

**日期：2026-08-15** | 数据来源：[github.com/MoonshotAI/kimi-cli](https://github.com/MoonshotAI/kimi-cli)

---

## 1. 今日速览

过去 24 小时无新版本发布或 PR 合并，社区讨论集中在**记忆系统**这一主题上——#1283 已累计 39 条评论，且 #1478 也在持续跟进，开发者对跨会话上下文持久化的呼声依旧强烈。此外，#2269 提出的多设备会话交接成为新的讨论方向，#1136 的 Shell 工具增强已完成并关闭。

---

## 2. 版本发布

**无**。过去 24 小时无新 Release（最新版本状态保持不变）。

---

## 3. 社区热点 Issues

> 注：过去 24 小时共有 4 个 Issue 有更新，全部列出如下（当前无更早时间窗内的其他活跃 Issue）。

### #1283 [增强] 记忆系统：跨会话持久化上下文 —— **社区焦点**
- **作者**: CatKang | 创建: 2026-02-27 | 更新: 2026-08-14 | 评论: **39** | 👍: 0
- **链接**: https://github.com/MoonshotAI/kimi-cli/issues/1283
- **内容**: 建议实现完整的记忆系统，涵盖 AI 自动管理的记忆（笔记）与用户手动定义的指令记忆，使 CLI 能在会话间记住项目模式、上下文和用户偏好。
- **重要性**: 39 条评论是当前所有 Issue 中最高的，且 0 👍 说明开发者并非以投票方式表达支持，而是以深度讨论的方式参与具体方案设计。该 Issue 直接决定 Kimi CLI 在大型项目中的实用上限，是社区长期关注的核心功能。

### #2269 [功能请求] 远程控制 / 多设备会话交接
- **作者**: lucianalima777 | 创建: 2026-05-13 | 更新: 2026-08-14 | 评论: 6 | 👍: 1
- **链接**: https://github.com/MoonshotAI/kimi-cli/issues/2269
- **内容**: 希望支持在一个设备上启动会话，在另一台设备（笔记本 / Web / 手机）上无缝继续或远程控制，面向多环境工作流的开发者。
- **重要性**: 这是较新的讨论方向（创建于 5 月），表明社区已不满足于单机 CLI 体验，而是开始探索云/多端协同场景。6 条评论说明仍处于早期讨论阶段，但 👍 数相对较高，说明存在现实需求。

### #1478 [增强] 能否优化记忆层？大项目使用很痛苦
- **作者**: hahy36 | 创建: 2026-03-17 | 更新: 2026-08-14 | 评论: 2 | 👍: 0
- **链接**: https://github.com/MoonshotAI/kimi-cli/issues/1478
- **内容**: 中文反馈，询问记忆层优化，并称参考文档中未找到相关说明（仅看到 agent.md）。作者附带了一份其他工具的目录结构（~/.openclaw/workspace/ 包含 SOUL.md、USER.md、MEMORY.md 等）作为参考。
- **重要性**: 与 #1283 形成互补，体现了**实际用户在大规模项目开发中的痛点**。中文用户社区的反馈也被官方同步关注到，该 Issue 与 #1283 共同构成了记忆功能需求的双重证据。

### #1136 [已关闭] 增强 Shell 工具：版本感知的 PowerShell 上下文
- **作者**: QIN2DIM | 创建: 2026-02-13 | 更新: 2026-08-14 | 评论: 0 | 👍: 0
- **链接**: https://github.com/MoonshotAI/kimi-cli/issues/1136
- **状态**: **CLOSED（已关闭）**
- **内容**: 问题描述列举了在 Kimi K2.5 上测试时 Shell 工具的三个关键问题，尤其是在 Windows 上首轮命令生成时（pass-1）出现歧义，并提出了增强方案。
- **重要性**: 该 Issue 已关闭，**说明相关修复/增强已合入主线**，对 Windows 用户而言是实质性改进。虽处于关闭状态，但其更新记录（8月14日）仍值得关注，建议开发者升级后验证 Windows 下的 Shell 交互体验。

---

## 4. 重要 PR 进展

**无**。过去 24 小时暂无公开 PR 的创建、更新或合并动态。建议关注 #1136 关闭后对应的代码变更（可查看 Shell 工具相关提交记录）。

---

## 5. 功能需求趋势

从当前活跃 Issue 中可以提炼出以下社区最关注的功能方向：

| 方向 | 关联 Issue | 热度/状态 |
|------|-----------|----------|
| **记忆系统 / 跨会话上下文持久化** | #1283, #1478 | **极高**（39 条讨论持续发酵，形成显性刚需） |
| **多设备协同 / 远程会话交接** | #2269 | 中（新方向，处于讨论早期） |
| **Shell 工具跨平台增强（Windows）** | #1136 | 高（已完成并关闭，修复落地） |
| **大型项目管理体验** | #1283, #1478 | 高（记忆系统最直接的应用场景） |

---

## 6. 开发者关注点

- **记忆系统缺失主导社区情绪**：#1283 的 39 条评论和 #1478 的“很痛苦”表述，表明开发者对“会话间遗忘”的容忍度正在降低。多数人希望 CLI 具备类似 **MEMORY.md 分层记忆**（长期 + 短期）的能力，而非仅依赖单文件 agent.md。
- **大项目使用体验成核心痛点**：中文用户社群的反馈（#1478）直指大型项目开发中因缺乏记忆导致的上下文丢失，这成为影响生产力的重要因素。
- **多设备工作流处于早期探索阶段**：#2269 获赞最多（1 👍），说明跨设备场景虽小众但具备潜在用户群，社区正在观望官方态度。
- **Windows 平台体验受关注**：#1136 的关闭是一个利好信号，但开发者仍期待更多针对 Windows PowerShell/SHELL 的细节优化，Kimi K2.5 用户可优先测试最新版本。

---

*本日报基于 2026-08-15 当日 GitHub 公开数据自动生成，仅供参考。*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报（2026-08-15）

## 今日速览

- 昨日出现严重的 48-bit ID 时间戳回绕问题（#42608），大量既有会话在 `2026-08-14 12:39:55 UTC` 后无法继续处理新消息，很可能是近期多起“会话无响应”报告的根因。
- TUI/Desktop 性能和 UI 回归问题集中爆发，包括多 subagent 下 97% CPU 卡顿、新布局隐藏 Plan/Build 切换入口。
- PR 侧亮点是 custom provider 动态模型发现（#42660）和 per-tool 执行超时（#36869），直指配置繁琐和工具挂起两大痛点。

## 社区热点 Issues

### 1. #42608 [CLOSED] 48-bit ID 时间戳回绕导致旧会话失效
https://github.com/anomalyco/opencode/issues/42608

所有创建于 `2026-08-14 12:39:55 UTC` 之前的会话都停止处理 prompt，是当日“agent 不再响应”问题激增的可能根因。虽然已关闭，但影响面极大，获 👍 3。

### 2. #42605 [OPEN] 会话保持打开但 agent 不处理后续消息
https://github.com/anomalyco/opencode/issues/42605

桌面端 agent 完成任务并提问后，用户发送新消息无任何响应。与 #42608 高度相关，社区正在追踪是否同源。

### 3. #36997 [OPEN] Desktop v1.18.1 新布局隐藏 Plan/Build agent 切换 UI
https://github.com/anomalyco/opencode/issues/36997

新布局下用户完全看不到当前 agent 是 Plan 还是 Build，Tab 键切换也异常。评论 12、👍 6，是当前 UI 回归中热度最高的问题。

### 4. #42657 [OPEN] 多 subagent 会话导致 TUI 卡顿，渲染线程 97% CPU
https://github.com/anomalyco/opencode/issues/42657

2-4 个并发 subagent 时输入延迟 1-3 秒，Warp、Windows Terminal、WezTerm 中均复现，指向 TUI 渲染层或事件循环瓶颈。

### 5. #41518 [OPEN] gpt-5.6-luna 经 OpenCode Go 中继返回 403 “region not available”
https://github.com/anomalyco/opencode/issues/41518

通过 opencode.ai 的 “OpenCode Go” 中继访问模型被区域限制阻断，错误信息不足以判断是账号、网络还是上游策略问题。

### 6. #38791 [OPEN] 导入会话消息 ID 不可按时间排序时，run loop 永不退出
https://github.com/anomalyco/opencode/issues/38791

`SessionPrompt.runLoop` 把消息 ID 当纯字符串比较大小，而该逻辑依赖 ID 内嵌时间戳。第三方导入的会话可能死循环直到 provider 400，对迁移用户是隐性坑。

### 7. #37489 [OPEN] 切换模式/压缩时上下文缓存失效引发性能问题
https://github.com/anomalyco/opencode/issues/37489

本地 LLM（vLLM/Ollama）场景下，模式切换或 compaction 后上下文缓存频繁失效，等待时间显著增加。评论 5，已获得 👍 1。

### 8. #40568 [CLOSED] Go 模型没有 websearch 工具，需设置未文档化环境变量
https://github.com/anomalyco/opencode/issues/40568

Go 路由默认没有 `websearch` 工具，设置 `OPENCODE_ENABLE_EXA=1` 后才出现，且该变量未写入官方文档。反映功能开关透明度不足。

### 9. #27553 [OPEN] 自动发现 OpenAI-compatible provider 的模型列表
https://github.com/anomalyco/opencode/issues/27553

LiteLLM、LM Studio、Ollama 等自定义 provider 都有 `/v1/models` 接口，但当前必须在 `opencode.json` 手动列出每个模型。获 👍 4，是配置体验最集中的需求。

### 10. #42616 [CLOSED] Zen Go Anthropic 端点：所有 GLM 模型带 tools 请求均失败
https://github.com/anomalyco/opencode/issues/42616

非空 `tools` 数组在 glm-5.3/5.2/5.1 上全部触发 422 `web_search` translation error。同类问题也影响 Kimi（#41120），说明中继层对非 OpenAI 模型工具调用兼容性仍需加固。

## 重要 PR 进展

### 1. #42660 [OPEN] feat(provider): add dynamic model discovery for custom providers
https://github.com/anomalyco/opencode/pull/42660

自动读取 OpenAI-compatible provider 的 `/v1/models` 并生成模型列表，一次性关闭 6 个相关 issue。这是配置体验的重要增强。

### 2. #42656 [CLOSED] refactor(protocol): move worktree routes out of experimental namespace
https://github.com/anomalyco/opencode/pull/42656

将 worktree API 从 `/api/experimental/project/:projectID/worktree` 提升为 `/api/worktree/:projectID`，减少后续 breaking change 风险。

### 3. #36869 [CLOSED] feat(opencode): per-tool execution timeout with abort + session recovery
https://github.com/anomalyco/opencode/pull/36869

为内置工具和 MCP 工具增加独立执行超时、中止和会话恢复能力，解决单个工具长时间挂起拖死 agent loop 的问题。

### 4. #36861 [CLOSED] fix(session): recover cache tokens from openai-compatible metadata usage fallback
https://github.com/anomalyco/opencode/pull/36861

当 provider 未按标准 usage 返回 cache token 时，从 metadata 中提取并回填，修复自定义 baseURL 场景下的 token/cost 统计错误。

### 5. #36870 [CLOSED] fix(core): load documented provider packages
https://github.com/anomalyco/opencode/pull/36870

支持文档中 provider/model 的 `package` 和 `settings` 字段，并将 `settings.baseURL` 正确映射到 catalog URL，避免生成 `undefined/...` 路由。

### 6. #36863 [CLOSED] feat(opencode): make webfetch response size limit configurable via env
https://github.com/anomalyco/opencode/pull/36863

新增 `OPENCODE_WEBFETCH_MAX_SIZE` 环境变量，允许用户调整 webfetch 抓取响应的大小上限。

### 7. #36862 [CLOSED] fix(desktop): validate openExternal URLs by protocol
https://github.com/anomalyco/opencode/pull/36862

对 `shell.openExternal` 的 URL 做协议白名单校验，防止 `file://`、`javascript:` 等危险协议被打开，属于安全性加固。

### 8. #36860 [CLOSED] fix(opencode): strip MiniMax trailing tool_call leak suffix from assistant text
https://github.com/anomalyco/opencode/pull/36860

清理 MiniMax 模型在普通 assistant 文本末尾追加的序列化 tool_call 残留标记，避免下游解析和渲染异常。

### 9. #36943 [CLOSED] fix(core): keep interrupted sessions stopped
https://github.com/anomalyco/opencode/pull/36943

修复中断后会话被旧 wake 重新唤醒的问题，保证“已中断会话保持停止”的语义，减少重复响应。

### 10. #36898 [CLOSED] fix(cli): handle descendant permission asks
https://github.com/anomalyco/opencode/pull/36898

修复 headless `opencode run` 只处理根会话权限请求，导致 Task 子会话权限请求永久阻塞的问题。

## 功能需求趋势

- **会话稳定性与可恢复性**：多个 issue 聚焦“会话无响应”“run loop 不退出”“历史会话静默失效”，社区对可靠的会话恢复和错误提示需求强烈。
- **Provider/模型兼容性**：OpenCode Go/Zen 中继上的区域 403、GLM/Kimi 工具调用失败、DeepSeek `reasoning_content` 回传错误等，说明中继层需要更完善的多模型适配。
- **性能优化**：TUI 多 subagent 卡顿、上下文缓存失效、大输出渲染慢是开发者最常抱怨的性能点。
- **配置自动化**：自动发现模型、OAuth 回调地址可配置、`/approve` 运行时权限切换、websearch 开关文档化，都指向“减少手工配置、增加运行时控制”。
- **桌面端/TUI 体验**：新布局隐藏核心切换、主题无法感知外部变化、WSL mirrored 网络下 sidecar 启动失败，桌面端细节问题开始集中暴露。

## 开发者关注点

- **“会话正常但无响应”是高频词**：多个 issue 都报告 agent 完成任务或提问后，新消息被静默忽略。开发者希望有明确报错或自动恢复机制，而不是长期挂起。
- **付费/配额体验不佳**：购买 Go / credit 后余额不显示、免费模型 24 小时 quota 未重置、403 区域限制说明不清，直接影响付费用户信任。
- **配置发现成本高**：部分功能依赖未文档化 env var，自定义 provider 需要手工维护模型列表，开发者希望配置能自动发现并补齐文档。
- **工具调用兼容性仍是长尾痛点**：通过 Anthropic/OpenAI 兼容端点访问非 OpenAI 模型时，`tool_call`、`reasoning_content` 等字段差异导致大量请求失败。
- **安全与权限意识增强**：`openExternal` URL 校验、headless 权限请求处理等 PR 显示社区开始关注桌面安全边界和 CLI 自动化权限流程。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi 社区动态日报 2026-08-15

## 今日速览

v0.84.2 发布，带来全屏会话搜索与可配置默认工具；社区焦点集中在 Windows/WSL 兼容性、GitHub Copilot 登录限流与 TUI 性能问题上。PR 侧有多项新模型适配（xAI、DeepSeek、Kimi）以及扩展/终端修复落地。

---

## 版本发布

### [v0.84.2](https://github.com/earendil-works/pi/releases/tag/v0.84.2)
- **全屏会话搜索**：在全屏视口下搜索并跳转匹配项，详见 [TUI Fullscreen Viewport 键位文档](https://github.com/earendil-works/pi/blob/v0.84.2/packages/coding-agent/docs/keybindings.md#tui-fullscreen-viewport)。
- **可配置默认工具**：支持选择启动时加载的默认工具集。

---

## 社区热点 Issues（10 个）

1. **[#7547] [Windows] 你如何在 Windows 上使用 Pi？遇到了哪些问题？**  
   27 条评论。作者发起讨论帖，收集 Windows 下各种运行方式（原生/WSL/模拟器）的痛点和优先级，说明 Windows 开发者基数大但官方支持路径尚不明确。  
   https://github.com/earendil-works/pi/issues/7547

2. **[#6187] WSL 中 Pi 登录挂起：GitHub Copilot 设备授权完成后无响应**  
   26 条评论。WSL 下浏览器授权成功后客户端检测不到完成状态，登录一直卡住，是 WSL 用户的高频阻塞问题。  
   https://github.com/earendil-works/pi/issues/6187

3. **[#5223] Anthropic provider 修改 thinking blocks，导致 Opus 4.8 自适应思考多轮对话 400**  
   17 条评论，6 👍。多轮会话中 provider 篡改最新 assistant 消息的 `thinking` 块，Claude Opus 4.8 用户受影响较大，已关闭。  
   https://github.com/earendil-works/pi/issues/5223

4. **[#6665] TUI 流式输出时占满单核：Intl.Segmenter 未缓存 + 逐 chunk Markdown 重建**  
   12 条评论，3 👍。长会话下 TUI 占用 100% CPU，已定位到分词未缓存和 Markdown 重复构建，官方标记 `inprogress`。  
   https://github.com/earendil-works/pi/issues/6665

5. **[#5023] 终端无故滚动到会话开头**  
   12 条评论。模型输出时终端随机跳转，再快速回到底部，虽无法稳定复现但多位用户反馈，影响会话阅读。  
   https://github.com/earendil-works/pi/issues/5023

6. **[#7850] 启用大量模型的 GitHub Copilot 组织登录报 429 限流**  
   9 条评论，7 👍（本期最高赞）。设备授权成功但 Copilot 登录返回 429，组织启用 20+ 模型时必现，企业用户痛点明显。  
   https://github.com/earendil-works/pi/issues/7850

7. **[#8096] Z.AI Coding Plan 默认模型引用已下架模型**  
   5 条评论。`defaultModelPerProvider` 仍指向 `glm-5.1`，但 models.dev 的目录已移除该模型，导致新配置失效。反映模型目录同步滞后。  
   https://github.com/earendil-works/pi/issues/8096

8. **[#8092] pnpm 安装的扩展依赖解析失败（jiti + 隔离 node_modules）**  
   5 条评论。pnpm 的隔离布局下 jiti 无法正确解析扩展依赖，影响使用 pnpm 的扩展开发者，已被 PR #8112 修复。  
   https://github.com/earendil-works/pi/issues/8092

9. **[#8036] Edit 工具渲染大 diff 时崩溃 TUI，会话恢复也失败**  
   2 条评论。一次编辑产生约 14.5 MB 的 diff，执行成功但渲染崩溃，恢复会话时再次崩溃。大文件长行场景下风险较高。  
   https://github.com/earendil-works/pi/issues/8036

10. **[#7761] TUI 提示 "Copied!" 但剪贴板为空（VTE/GNOME Terminal）**  
    3 条评论。`copySelectionToClipboard()` 只写 OSC 52，VTE 终端会忽略该序列。已由 PR #8110 修复。  
    https://github.com/earendil-works/pi/issues/7761

---

## 重要 PR 进展（10 个）

1. **[#8146] fix(ai): 将 Baseten DeepSeek V4 Flash 最大输出限制为 384k tokens**  
   修正 models.dev 上报的 1,048,576 输出上限与实际 Baseten 服务不一致的问题，避免请求失败。  
   https://github.com/earendil-works/pi/pull/8146

2. **[#8124] feat(ai): xAI 模型改走 Responses API，默认模型升级为 Grok 4.6**  
   同时统一发送 Pi 的 user-agent，默认从 Grok 4.5 迁移到 4.6。  
   https://github.com/earendil-works/pi/pull/8124

3. **[#8120] feat(coding-agent): 实验性 append compaction**  
   设置 `PI_EXPERIMENTAL=1` 时启用，复用 system prompt、工具和路由会话，使压缩前缀能命中 provider 的 prompt 缓存。  
   https://github.com/earendil-works/pi/pull/8120

4. **[#8119] fix: 跟踪 Kimi cached tokens**  
   将 Kimi Chat Completions 顶层 `usage.cached_tokens` 正确计入缓存读取 token，修复使用量统计偏差。  
   https://github.com/earendil-works/pi/pull/8119

5. **[#8112] fix(coding-agent): jiti 导入前先 realpath 扩展入口**  
   关闭 #8092，解决 pnpm 隔离 node_modules 布局下扩展依赖无法解析的问题。  
   https://github.com/earendil-works/pi/pull/8112

6. **[#8110] fix(tui): 选区复制改走宿主剪贴板，让 "Copied!" 提示真实可靠**  
   修复 VTE/Terminal.app/tmux 等忽略 OSC 52 的终端中复制无效的问题，关闭 #7761。  
   https://github.com/earendil-works/pi/pull/8110

7. **[#8011] fix: 修复单个 edit 对象输入**  
   部分模型只输出单个 edit 对象而非数组时，参数规范化失败导致工具崩溃，现已支持两种形式。  
   https://github.com/earendil-works/pi/pull/8011

8. **[#8123] fix(extensions): registerFlag 类型不匹配**  
   修复 `boolean` flag 允许 `default: "false"` 字符串导致省略 flag 仍为 truthy 的问题，增加运行时检查。  
   https://github.com/earendil-works/pi/pull/8123

9. **[#5262] feat(ai): 新增 Anthropic Vertex provider**  
   以 thin adapter 方式接入 Google Cloud Vertex AI 上的 Claude，复用现有 Anthropic Messages 流式链路。  
   https://github.com/earendil-works/pi/pull/5262

10. **[#6216] feat: 新增 Amazon Bedrock Mantle OpenAI Responses provider**  
    基于 OpenAI Bedrock Provider 实现，为 AWS 用户提供 Bedrock Mantle 接入能力。  
    https://github.com/earendil-works/pi/pull/6216

---

## 功能需求趋势

- **新模型/提供商扩展**：社区持续要求接入新模型，本期涉及 xAI Grok 4.6、Baseten DeepSeek V4、SiliconFlow、Anthropic Vertex、Bedrock Mantle、Kimi Coding 等；同时多起 Issue 暴露了模型目录数据同步滞后的问题。
- **Windows/WSL 支持**：#7547 发起 Windows 使用方式收集，#6187 WSL 登录挂起、#8047 Windows 测试套件失败，说明 Windows 仍是重点但未完善的方向。
- **TUI 性能与稳定性**：全核占用、随机滚动、大 diff 崩溃、剪贴板失效等终端问题集中反馈，长会话/大输出场景下的渲染稳定性是核心诉求。
- **扩展系统与自动化**：扩展依赖解析、flag 类型安全、事件钩子、脚本化 CLI/env 参数（#8114）等需求，显示开发者希望 Pi 更好地融入现有工程化工作流。
- **模型兼容性精确化**：`strict: null` 导致可选参数变必选、`thinkingLevelMap` 被丢弃、reasoning-only 响应绕过重试、代理/WebSocket 故障恢复等，社区希望严格遵循各 provider 语义。
- **配置灵活性**：按模型设置 compaction（#8133）、skills 在输入中自动补全（#8144）、autocomplete 弹出位置（#8132）、/tmp 文件创建规则（#8145）等，用户希望更多可配置项。

---

## 开发者关注点

- **Copilot 登录限流（429）** 是本期最高赞话题（#7850、#8010），企业组织启用大量模型时容易出现。
- **WSL 认证流程不健全**：设备授权完成后客户端无法感知（#6187），登录体验断裂。
- **TUI 假死/崩溃**：全核占用（#6665）、随机滚动（#5023）、大 diff 崩溃（#8036）直接损害长时间使用体验。
- **剪贴板兼容性**：OSC 52 在 VTE、Terminal.app、tmux 等环境不生效（#7761），提示信息误导用户。
- **pnpm 扩展安装的依赖解析问题**（#8092）对现代 JS 工具链的扩展作者不友好。
- **模型目录同步滞后**：默认模型指向已失效的 `glm-5.1`（#8096），说明 models.dev 数据更新需要自动化保障。
- **网络/代理故障恢复**：HTTP 代理下工具调用后挂起（#8134）、WebSocket 瞬时失败后固定走 SSE（#8125），容错行为需要更稳健。
- **兼容层语义失准**：`strict: null`、`thinkingLevelMap`、reasoning-only 完成等场景下，行为与 provider 文档不一致，需要精细修复。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报

**日期：2026-08-15** ｜ **数据来源：** [QwenLM/qwen-code](https://github.com/QwenLM/qwen-code)

---

## 1. 今日速览

- **v0.21.12 正式版发布**，重点改进了 Web Shell 的会话与文件交互体验，支持通过拖拽或 @ 面板上传工作区文件，并带有进度追踪。
- **Web Shell 成为当前迭代的核心**：媒体引用端到端支持、侧边栏会话管理、Electron 桌面宿主评估等多个 PR/Issue 同时活跃推进。
- **架构治理与 CI 稳定性是社区第二大关切**：`utils/` 目录循环依赖、CI 多次失败、autofix 安全隔离等议题获得了密集讨论与快速响应。

---

## 2. 版本发布

### v0.21.12（正式版）
- **亮点**：
  - Web Shell 支持通过拖拽或 @ 文件面板上传工作区文件，并显示上传进度（[#8874](https://github.com/QwenLM/qwen-code/pull/8874)）。
  - 自动修复（autofix）评审中实现了 diff 增长制动器，限制无限膨胀的修复循环。
- 另含多个 preview 迭代（v0.21.12-preview.3 / preview.4），主要包含 Web Shell 修复与文件上传功能。

### 验证与基准版本
- `dsw-eas-tb-e2e-20260814-r6`：完成 Release → Actions → DSW SWE-bench Verified 500 → Publisher → Terminal-Bench 2.0 89 的全链路验证；基准版本 v0.21.2。
- `dsw-eas-tb-e2e-20260814-r1/r2/r3`：隔离环境下的端到端验证，其中 r1 报告 1 个基础设施错误（未解决）。

---

## 3. 社区热点 Issues

> 以下挑选了评论数最多、讨论最活跃的 10 个 Issue，覆盖用户反馈、Bug 报告与架构讨论。

### #8957 [回归] Qwen Code 自 0.21.2 起加载图片即崩溃
- **作者**：nicman23 ｜ 评论 12 ｜ 优先级 P2 ｜ 状态：待补充信息
- **摘要**：自 0.21.2 起，读取图片时 Qwen Code 立即崩溃，0.21.1 为最后一个正常版本。
- **社区反应**：问题已持续 3 天仍为 OPEN，评论数最高，说明影响面较大，用户期望尽快定位回归源。
- **链接**：[Issue #8957](https://github.com/QwenLM/qwen-code/issues/8957)

### #8678 [已关闭] 大型恢复超时时保留当前会话
- **作者**：doudouOUC ｜ 评论 9 ｜ 优先级 P1 ｜ 状态：已关闭（部分解决）
- **摘要**：请求级恢复超时、晚到结果安全、附件身份隔离等多项验收标准落地。
- **社区反应**：P1 会话管理问题，最终判定为"部分解决并被取代"，说明核心问题已缓解。
- **链接**：[Issue #8678](https://github.com/QwenLM/qwen-code/issues/8678)

### #8051 [跟踪] 限制多工作区守护进程的资源占用
- **作者**：doudouOC ｜ 评论 9 ｜ 优先级 P2 ｜ 状态：待分类
- **摘要**：`qwen serve` 多工作区守护进程目前仅限制数量，但请求体字节、WebSocket 组装等内存占用不受控，需按字节上限管理。
- **社区反应**：长期跟踪（7/29 创建，至今 15 天），反映服务端资源治理是持续关注点。
- **链接**：[Issue #8051](https://github.com/QwenLM/qwen-code/issues/8051)

### #9143 主分支 CI 失败（E2E Tests）
- **作者**：qwen-code-dev-bot ｜ 评论 7 ｜ 优先级 P3 ｜ 状态：待 Agent 处理
- **摘要**：E2E Tests 工作流在报告任何测试结果前失败，已在提交 `c5bf2224` 层面单独跟踪。
- **社区自动报告**：机器人提交的自动化 CI 失败跟踪，高频出现说明流水线仍有不稳定因素。
- **链接**：[Issue #9143](https://github.com/QwenLM/qwen-code/issues/9143)

### #9002 Python SDK 拒绝 `permission_mode="auto"`
- **作者**：shenyankm ｜ 评论 6 ｜ 优先级 P3 ｜ 状态：待处理
- **摘要**：CLI 已支持 `permission_mode="auto"`，但 Python SDK 在客户端校验阶段即抛 `ValidationError`，选项不一致。
- **社区反应**：SDK 与 CLI 行为不一致，属集成类问题，影响自动化脚本用户。
- **链接**：[Issue #9002](https://github.com/QwenLM/qwen-code/issues/9002)

### #6806 `/compress` 后状态栏上下文百分比不刷新
- **作者**：qwen-code-dev-bot ｜ 评论 5 ｜ 优先级 P2 ｜ 状态：待补充信息
- **摘要**：执行 `/compress` 或 `/compress-fast` 后，状态栏（footer）显示的上下文使用率不更新，仍停留在压缩前数值。
- **社区反应**：TUI 细节问题，欢迎 PR，属于易复现的体验 Bug。
- **链接**：[Issue #6806](https://github.com/QwenLM/qwen-code/issues/6806)

### #8582 [安全] 只读 Shell 分类器可被命令替换绕过
- **作者**：yiliang114 ｜ 评论 5 ｜ 优先级 P1 ｜ 已关闭
- **摘要**：AST 分类器和运行时替换门均可被行延续或 `${var@P}` 绕过，导致"只读"命令实际执行任意代码。
- **社区反应**：安全类 P1 问题，已关闭说明已修复，但讨论密度反映社区对 Shell 安全的敏感度。
- **链接**：[Issue #8582](https://github.com/QwenLM/qwen-code/issues/8582)

### #8871 ACP 子进程报 "Unknown argument: acp"
- **作者**：aspnetdb ｜ 评论 5 ｜ 优先级 P2 ｜ 状态：待补充信息
- **摘要**：`qwen serve --http-bridge=true` 时主进程以 `--acp` 参数启动 ACP 子进程，但子进程无法解析该参数，导致 401 认证失败。
- **社区反应**：服务模式下的集成断裂问题，阻碍 ACP 工作流使用。
- **链接**：[Issue #8871](https://github.com/QwenLM/qwen-code/issues/8871)

### #9026 无头模式安静结束导致 `NO_TOOL_RESULT_PROGRESS` 硬失败
- **作者**：ram-centific ｜ 评论 4 ｜ 优先级 P2 ｜ 状态：OPEN
- **摘要**：模型在工具结果后以有效 `finish_reason` 安静结束（无文本、无后续调用）时，`InvalidStreamError` 被触发，重复消耗 4 次重试预算后终止。
- **社区反应**：核心流处理逻辑 Bug，已提交对应修复 PR #9196，形成闭环。
- **链接**：[Issue #9026](https://github.com/QwenLM/qwen-code/issues/9026)

### #9146 `utils/` 应成为叶子层 — 107 个向上导入造成循环依赖
- **作者**：yiliang114 ｜ 评论 4 ｜ 优先级 P2 ｜ 状态：OPEN（创建于 8/14）
- **摘要**：`packages/core` 与 `packages/cli` 中的 `utils/` 目录被 51 个文件的 107 个向上导入依赖，需重构使其成为纯叶子层。
- **社区反应**：新提交的架构重构 Issue，体现社区对代码可维护性的持续投入。
- **链接**：[Issue #9146](https://github.com/QwenLM/qwen-code/issues/9146)

---

## 4. 重要 PR 进展

> 按讨论热度与影响范围选取 10 个 PR，涵盖新功能、Bug 修复与架构改进。

### #9196 fix(core): 接受重试耗尽后的安静工具结果完成
- **作者**：yiliang114 ｜ 创建：8/14 ｜ 状态：OPEN
- **内容**：修复 #9026 — 合法安静结束不应触发 `NO_TOOL_RESULT_PROGRESS` 重试循环，节省无意义的 4 次重试预算。
- **意义**：直接解决无头（headless）模式下稳定性的根因问题。
- **链接**：[PR #9196](https://github.com/QwenLM/qwen-code/pull/9196)

### #8938 feat(core): 拒绝上游 fail-fast 占位响应
- **作者**：yiliang114 ｜ 创建：8/11 ｜ 状态：OPEN，自报评审
- **内容**：防止上游模型端点返回 HTTP 200 + 正常 finish reason，但整段正文仅为 `(request timed out)` 类占位文本的情况。
- **意义**：提高模型 API 异常响应的防御能力，减少脏数据进入会话。
- **链接**：[PR #8938](https://github.com/QwenLM/qwen-code/pull/8938)

### #9127 feat: 端到端支持会话媒体引用
- **作者**：ytahdn ｜ 创建：8/14 ｜ 状态：OPEN
- **内容**：在守护进程、ACP 桥、TypeScript SDK、Web Shell 全链路支持图片等媒体的会话级引用（媒体 ID + 元数据），避免重复上传。
- **意义**：Web Shell 媒体处理的一次重大架构升级，与今日 v0.21.12 的文件上传功能形成互补。
- **链接**：[PR #9127](https://github.com/QwenLM/qwen-code/pull/9127)

### #9122 feat(web-shell): 改进侧边栏会话管理
- **作者**：ytahdn ｜ 创建：8/14 ｜ 状态：OPEN
- **内容**：会话详情悬停预览、会话文件夹最多预览 5 行后展开、长标题溢出滚动、运行中会话视觉标识等。
- **意义**：提升 Web Shell 日常使用的可浏览性与可管理性。
- **链接**：[PR #9122](https://github.com/QwenLM/qwen-code/pull/9122)

### #8403 feat(audit): 添加遗留代码审计工作流
- **作者**：wenshao ｜ 创建：8/3 ｜ 状态：OPEN（autofix 接管）
- **内容**：实现 `/audit <directory> [--effort low|medium|high]` 命令，允许对无 diff 的存量模块进行审计，含确定性 CLI 参数解析与文件系统规划。
- **意义**：将审查能力从 PR 扩展到存量代码库，适用于技术债治理。
- **链接**：[PR #8403](https://github.com/QwenLM/qwen-code/pull/8403)

### #8332 feat(cli): 添加附件音频桥
- **作者**：DragonnZhang ｜ 创建：8/1 ｜ 状态：OPEN（autofix 接管）
- **内容**：当主模型不支持音频时，通过配置的批量语音模型将用户附件（@ 引用、ACP 音频提示）转录后，替换为显式不可信的机器转录文本。
- **意义**：扩展多模态输入能力，降低对单一模型能力的依赖。
- **链接**：[PR #8332](https://github.com/QwenLM/qwen-code/pull/8332)

### #9096 feat(review): 将散文式 gh 命令合并为平台支持的子命令
- **作者**：wenshao ｜ 创建：8/13 ｜ 状态：OPEN
- **内容**：将 `/review` skill 中的 `gh repo view`、`gh pr view --json headRefOid` 等散文式命令改为平台支持的确定性子命令。
- **意义**：提升 `/review` 的确定性与可测试性，减少 LLM 自由调用 `gh` 的风险。
- **链接**：[PR #9096](https://github.com/QwenLM/qwen-code/pull/9096)

### #9027 feat(cli): 纯散文 `/review` 评论，严重度标记跟随 review.attribution
- **作者**：wenshao ｜ 创建：8/12 ｜ 状态：OPEN（autofix 接管）
- **内容**：PR 评论改用人话表达，不再使用模板语气；严重度标记遵循既有属性配置，双层文本结构。
- **意义**：提升 `/review` 输出的可读性，更接近人类 reviewer 的表达方式。
- **链接**：[PR #9027](https://github.com/QwenLM/qwen-code/pull/9027)

### #9118 feat(review): 采用轮次感知的收敛姿态
- **作者**：wenshao ｜ 创建：8/14 ｜ 状态：OPEN
- **内容**：随着同一 PR 的评审轮次累积，`/review` 自动提高发布门槛，避免循环扩 diff，默认收敛。
- **意义**：针对多轮 review 循环的机制性改进，直接回应了 #9155 等 "七轮评审" 类痛点。
- **链接**：[PR #9118](https://github.com/QwenLM/qwen-code/pull/9118)

### #9171 fix(devx): 单元测试构建前置缺失时给出可操作报错
- **作者**：yiliang114 ｜ 创建：8/14 ｜ 状态：OPEN（自报评审）
- **内容**：为 `packages/cli` 单测添加 vitest `globalSetup` 守卫，检查 12 个 workspace 包的 `dist/` 输出，缺失时给出明确指引。
- **意义**：改善开发体验，避免因构建前置缺失而产生迷惑性测试失败。
- **链接**：[PR #9171](https://github.com/QwenLM/qwen-code/pull/9171)

### #9082 fix(ci): release 分支改为 force-push 以支持重试替换
- **作者**：qwen-code-dev-bot ｜ 创建：8/13 ｜ 状态：OPEN
- **内容**：发布任务将 `release/<tag>` 分支改为强制推送，避免上一次失败留下的陈旧分支阻塞重试。
- **意义**：直接回应 #9137 等发布失败问题，提升发布管道鲁棒性。
- **链接**：[PR #9082](https://github.com/QwenLM/qwen-code/pull/9082)

---

## 5. 功能需求趋势

综合当前活跃的 Issues 和 PRs，社区关注的功能方向集中于以下六点：

### ① Web Shell 体验持续深化
- **文件上传**：拖拽 / @ 面板上传（v0.21.12 已发布）
- **会话管理**：侧边栏预览、hover 详情、媒体引用端到端支持
- **桌面化**：提案评估 Electron 作为独立桌面宿主（[#9168](https://github.com/QwenLM/qwen-code/issues/9168)），与现有 Tauri 应用并行
- **导出能力**：要求将 HTML 导出渲染统一为 WebShellTranscript 只读 UI（[#9186](https://github.com/QwenLM/qwen-code/issues/9186)）

### ② 代码审查与自动化治理
- 新的 `/audit` 遗留代码审计工作流（[#8403](https://github.com/QwenLM/qwen-code/pull/8403)）
- `/review` 机制持续演进：轮次感知收敛、纯散文评论、gh 命令确定性化
- **终端渲染验证**：capture-tui 像素级证据链（[#8894](https://github.com/QwenLM/qwen-code/pull/8894)）
- **CI 稳定性**：多个 CI 失败自动化跟踪与修复（[#9143](https://github.com/QwenLM/qwen-code/issues/9143)、[#9082](https://github.com/QwenLM/qwen-code/pull/9082)）

### ③ 架构可维护性重构
- `utils/` 层循环依赖治理（[#9146](https://github.com/QwenLM/qwen-code/issues/9146)）
- ACP 集成解耦 serve 内部实现（[#8084](https://github.com/QwenLM/qwen-code/issues/8084)）
- 守护进程资源按字节限制（[#8051](https://github.com/QwenLM/qwen-code/issues/8051)）

### ④ 多模态与第三方模型扩展
- **音频桥**：不支持音频的模型通过转录桥接（[#8332](https://github.com/QwenLM/qwen-code/pull/8332)）
- **新模型提供商**：Kimi 与小米 MiMo 一键接入（[#8368](https://github.com/QwenLM/qwen-code/pull/8368)）

### ⑤ 新渠道与平台集成
- **钉钉渠道**：DingTalk Workspace 原生支持，含群聊、At 提醒、待办同步（[#9049](https://github.com/QwenLM/qwen-code/pull/9049)）
- 频道会话管理体系化：Channel policy、session isolation、workspace ownership（[#8845](https://github.com/QwenLM/qwen-code/issues/8845)）

### ⑥ 智能体自治能力
- 会话媒体引用与端到端状态同步（[#9127](https://github.com/QwenLM/qwen-code/pull/9127)）
- 目标（Goal）v3 控制平面采用（[#9087](https://github.com/QwenLM/qwen-code/pull/9087)）

---

## 6. 开发者关注点

### 高优先级痛点

- **回归 Bug 响应速度**：#8957（图片加载崩溃）持续 3 天未关闭，显示回归问题的定位与修复效率仍需提升；用户对 0.21.x 系列的稳定性存在顾虑。
- **CI 不稳定**：过去 24 小时内出现至少 3 个 CI 失败跟踪 Issue（#9143、#9159、#9160），E2E 测试在无结果阶段即失败，影响社区对 main 分支健康度的信任。
- **SDK/CLI 行为不一致**：#9002 暴露了 Python SDK 与 CLI 在 `permission_mode` 配置上的分歧，这类基础能力的不一致对自动化用户影响明显。
- **长期内存增长**：#2128 已有近半年未解决（3/5 创建），UI History 无上限累积是长会话场景下的稳定隐患。

### 高频观察

- **发布流程可靠性**：v0.21.12-preview.2 发布 workflow 失败（#9137），社区机器人连续提交 E2E 验证版本（r1-r6），说明发布管道正在承受较大压力。
- **安全敏感度上升**：#8582 这类 shell 注入绕过问题得到 P1 优先级快速处理，#9089 进一步提出 PAT 作业与不可信分支代码的主机隔离需求。
- **多轮评审疲劳**：#9155（PR #9040 第 7 轮评审）、#9118（轮次感知收敛）等动作表明，社区正在积极优化 review 长尾问题，降低维护者负担。
- **开发体验细节**：#6806 状态栏刷新、#8827 终端 teardown 加固、#9171 构建前置提示等，说明开发者对"小但磨人"的 DX 问题同样敏感。

---

*本日报由 Qwen Code 社区数据自动生成，仅供技术参考。完整数据请见 [QwenLM/qwen-code](https://github.com/QwenLM/qwen-code)。*

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报 — 2026-08-15

> 数据源：github.com/Hmbown/DeepSeek-TUI（仓库活动已迁移至 **Hmbown/CodeWhale**，原仓库名已成为历史）

## 一、今日速览

v0.9.8 正式发布，项目以 **CodeWhale** 品牌统一对外，旧 `deepseek-tui` npm 包停止维护。过去 24 小时社区焦点集中在 agent 工具 schema 过载（#5324）、`/dryrun` 请求预览（#1004）和 Web UI P0 损坏（#5370）；同时 8 个稳定性修复 PR 密集合入，session 数据竞争、webhook panic、CI 红线等问题被快速解决。Dependabot 也在批量推进 Rust 依赖升级。

## 二、版本发布

### v0.9.8（过去 24 小时）

- **品牌统一**：CodeWhale 正式成为 Shannon Labs 的公开产品名；`codewhale` 命令、npm 包、release 资产名称统一为小写技术标识。
- **弃用通知**：旧 npm 包 `deepseek-tui` 已弃用，不再接收后续 release。
- **升级提示**：来自 v0.8.x 旧版 `deepseek` / `d…` 的用户需切换到 `codewhale` 入口。  
  完整变更说明见 Release 页面。

## 三、社区热点 Issues（10 个）

1. **#3192 [OPEN] 接入 agentclientprotocol/registry**  
   社区讨论度最高（13 条评论）。目标是让 Zed 能通过 ACP registry 直接发现并安装 CodeWhale，IDE 集成诉求强烈。  
   [https://github.com/Hmbown/CodeWhale/issues/3192](https://github.com/Hmbown/CodeWhale/issues/3192)

2. **#5324 [OPEN] agent 工具：简化 32 字段 schema**  
   项目所有者 Hmbown 提出。模型侧 `agent` 工具携带 32 属性 JSON schema、0 必填字段、8 个 action 混合使用，是模型高频报错的主因。8 条评论，已催生配套 PR #5369。  
   [https://github.com/Hmbown/CodeWhale/issues/5324](https://github.com/Hmbown/CodeWhale/issues/5324)

3. **#1004 [OPEN] /dryrun：预览下一次补全请求而不发送**  
   9 条评论。针对 V4 Pro 长上下文场景，开发者希望在真正发送前看到完整请求结构，降低长 turn 试错成本。  
   [https://github.com/Hmbown/CodeWhale/issues/1004](https://github.com/Hmbown/CodeWhale/issues/1004)

4. **#5370 [OPEN][P0] Web UI 完全损坏，需对照重建**  
   Hmbown 报告公共 Web UI “totally broken”，需区分 codewhale.net 与托管 cwc 应用两条产品线。评论不多但优先级极高。  
   [https://github.com/Hmbown/CodeWhale/issues/5370](https://github.com/Hmbown/CodeWhale/issues/5370)

5. **#5374 [OPEN] macOS 下 agent 输出文本乱码**  
   用户贴图显示输出区文字完全不可读，严重影响核心使用。4 条评论，高可见度 TUI 缺陷。  
   [https://github.com/Hmbown/CodeWhale/issues/5374](https://github.com/Hmbown/CodeWhale/issues/5374)

6. **#5322 [OPEN] 宽终端回归：输出区不再自动填充**  
   v0.8.65 可自动扩展，v0.9.5 起被 max width 限制，宽屏下大量留白。3 条评论，典型用户可见回归。  
   [https://github.com/Hmbown/CodeWhale/issues/5322](https://github.com/Hmbown/CodeWhale/issues/5322)

7. **#5350 [OPEN] 第三方模型配置预制模板**  
   中文用户提出：OpenCode Zen/Go、Agnes、美团 Sensenova 等第三方服务需手工填写 Base URL、模型名与密钥，且保存后常卡在 `not checked` / `cache failed`。希望内置模板、文档说明与「测试连接」按钮。  
   [https://github.com/Hmbown/CodeWhale/issues/5350](https://github.com/Hmbown/CodeWhale/issues/5350)

8. **#4326 [OPEN] 取消 32-worker 风暴后 RSS 不回落**  
   6 条评论。32-worker PTY benchmark 后内存只升不降，需要区分分配器高水位保留与真实运行时泄漏，并约束取消后状态。  
   [https://github.com/Hmbown/CodeWhale/issues/4326](https://github.com/Hmbown/CodeWhale/issues/4326)

9. **#5373 [CLOSED] 输出 token 上限被钳制，导致长任务截断**  
   CodeWhale 对 deepseek-v4-flash/pro 请求 65,536 输出 token，而目录文档标注上限 384,000；Terminal-Bench 任务因此直接崩掉。  
   [https://github.com/Hmbown/CodeWhale/issues/5373](https://github.com/Hmbown/CodeWhale/issues/5373)

10. **#5293 [CLOSED] 默认拒绝（deny-by-default）批准选择需可配置**  
    v0.9.4 起权限对话框默认高亮改为 deny，可能让用户误拒本要确认的操作。5 条评论、1 👍，安全默认与既有交互习惯的冲突。  
    [https://github.com/Hmbown/CodeWhale/issues/5293](https://github.com/Hmbown/CodeWhale/issues/5293)

## 四、重要 PR 进展（10 个）

1. **#5365 feat(provider): 本地 DS4 一等公民设置** — 已合并  
   让 DwarfStar（DS4）成为本地 DeepSeek 路由，复用 OpenAI 兼容传输；`/setup provider ds4` 等命令直接打开预填 loopback 预设。  
   [https://github.com/Hmbown/CodeWhale/pull/5365](https://github.com/Hmbown/CodeWhale/pull/5365)

2. **#5353 feat(tui): Auto-Review 模型守护层** — 已合并  
   v0.9.8 双模式审查：确定性兜底不可绕过，fallback 升级为一次性模型守护；对齐 Codex `auto_review` 语义、Kimi 模式词汇、fail-closed 默认值。  
   [https://github.com/Hmbown/CodeWhale/pull/5353](https://github.com/Hmbown/CodeWhale/pull/5353)

3. **#5358 feat(engine): 审查拒绝理由 + 回合断路器** — 已合并  
   Auto-Review 的 Block 不再以裸 `permission_denied` 返回；增加拒绝理由和断路器，避免模型反复改写同一动作直至步数耗尽。  
   [https://github.com/Hmbown/CodeWhale/pull/5358](https://github.com/Hmbown/CodeWhale/pull/5358)

4. **#5369 fix(tools): Moonshot schemas 降级而非拒绝** — 已合并  
   针对模型无法处理复杂条件 schema 的问题，将 Moonshot 的 schema 降级为兼容形态，而不是直接拒绝调用。  
   [https://github.com/Hmbown/CodeWhale/pull/5369](https://github.com/Hmbown/CodeWhale/pull/5369)

5. **#5382 fix(state): 序列化 session-index 写入** — 已合并  
   修复 #5380。`append_thread_name` 在锁外 append+rewrite+rename，并发 StateStore 克隆可导致静默数据丢失；改为同步化写入。  
   [https://github.com/Hmbown/CodeWhale/pull/5382](https://github.com/Hmbown/CodeWhale/pull/5382)

6. **#5381 fix(hooks): webhook 客户端构建失败不再 panic** — 已合并  
   修复 #5379。去掉 fallback HTTP client 的 `.expect()`，改为返回错误，避免 TLS 等环境问题直接崩溃宿主进程。  
   [https://github.com/Hmbown/CodeWhale/pull/5381](https://github.com/Hmbown/CodeWhale/pull/5381)

7. **#5376 fix(tui): 内部运行时事件不进入 session peek** — 已合并  
   修复 #5375。真实构造函数产生的 PROJECTION 等内部事件不再污染用户态 peek 视图，并补充回归测试。  
   [https://github.com/Hmbown/CodeWhale/pull/5376](https://github.com/Hmbown/CodeWhale/pull/5376)

8. **#5364 feat(tui): Markdown 引用块 quote rail 渲染** — 已合并  
   TUI 字幕新增 `Block::Quote`，用醒目引用栏替代纯文本 `>`，支持嵌套、内联格式、换行与正确选择复制。  
   [https://github.com/Hmbown/CodeWhale/pull/5364](https://github.com/Hmbown/CodeWhale/pull/5364)

9. **#5339 fix(engine): 抑制子进程的 shell 补全事件** — 已合并  
   修复 #5325。子进程 owned 的后台 shell 补全不再流入父模型流；保留父进程未 owned 补全与任务/状态可见性。  
   [https://github.com/Hmbown/CodeWhale/pull/5339](https://github.com/Hmbown/CodeWhale/pull/5339)

10. **#5384 test(cli): 重新固定 provider-count 断言** — 待合并  
    修复 #5383。v0.9.8 注册表新增 Gemini 等 backend，provider 数从 43 → 45；CLI 测试断言落后导致 main 变红，仅需更新两个整数。  
    [https://github.com/Hmbown/CodeWhale/pull/5384](https://github.com/Hmbown/CodeWhale/pull/5384)

> 另：Dependabot 正批量更新 Rust 依赖 — rusqlite 0.40.2（#5391）、rmcp 3.1.2（#5390）、thiserror 2.0.20（#5389）、ratatui 0.30.2（#5388）、tower-http 0.7.0（#5387）均处于 open 状态。

## 五、功能需求趋势

从 50 个活跃 Issue 与 18 个 PR 中提炼出 6 个社区强关注方向：

1. **模型/服务商可扩展性**：NVIDIA NIM、Moonshot、DS4 本地路由、第三方兼容服务商预制模板；要求「填密钥即可用」并修复 `not checked` / `cache failed` 状态。
2. **Agent 工具链成熟**：简化 32 字段 agent schema（#5324）、`/dryrun` 请求预览（#1004）、子代理显示身份统一（#5287）、解决过期写声明阻塞新子代理（#5372）。
3. **IDE 与生态集成**：通过 ACP registry 被 Zed 直接安装（#3192）、VSCode 市场非官方扩展的版权治理（#2327）、Kimi 级插件系统与联邦市场（#5311）。
4. **TUI 体验回归修复**：宽终端填充（#5322）、Markdown 引用块（#5364）、TUI 更新提示（#5053）、非英文路由可点击控件（#5290）。
5. **性能与内存治理**：32-worker 取消后 RSS 回落（#4326）、输出 token 上限对齐 models.dev 目录（#5373）。
6. **数据可靠性**：session-index 写入同步（#5380）、压缩生存契约发布（#4394）、webhook 构建失败不崩溃（#5379）。

## 六、开发者关注点（痛点 / 高频需求）

- **Agent 工具 schema 过载**：32 字段、0 必填、8 actions 混用，是模型报错与幻觉的高频来源。
- **长任务被 token 上限截断**：客户端请求 65,536 输出 token，而目录允许 384,000；真实任务与 benchmark 均受影响。
- **并发状态写入静默丢失**：StateStore 在锁外写 JSONL + rename，多实例克隆下存在竞态窗口。
- **品牌迁移阵痛**：`deepseek-tui` 弃用后，命令、配置路径、文档同步改名；升级后 doctor 的 first-run / update checkpoint 卡在 `needs action`（#5340）。
- **第三方配置流程繁琐**：无内置模板、无文档提示、保存后状态异常，是新手上手的主要障碍。
- **TUI 渲染回归**：macOS 输出乱码、宽屏留白、session peek 混入内部事件，直接影响核心可读性。

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

过去24小时无活动。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*