# AI CLI 工具社区动态日报 2026-08-18

> 生成时间: 2026-08-17 23:16 UTC | 覆盖工具: 10 个

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

# AI CLI 工具横向对比分析报告（2026-08-18）

## 1. 生态全景

当前 AI CLI 工具处于"功能竞赛与稳定性还债并行"阶段：十款主流工具中 9 款在 24 小时内有实质动态，Claude Code、OpenAI Codex、Gemini CLI、Qwen Code 四家均有版本发布或预发布版本。社区讨论焦点高度趋同——MCP 生产级治理、子代理/多智能体可靠性、上下文压缩与内存治理、Windows 平台体验成为跨工具共性议题。与此同时，稳定性问题集中爆发：多个工具同日报出 10GB 级内存膨胀、OOM、会话数据丢失等严重缺陷，说明功能迭代速度已明显快于架构稳定性建设。值得注意的是，"AI 维护 AI" 的模式开始浮现（Gemini SSR Agent 批量提交修复 PR、Claude Code 与 Qwen Code 的自动化 review 管线），开源维护方式正在被 AI 工具自身重塑。

## 2. 各工具活跃度对比

| 工具 | Issues（日报精选） | PRs（日报精选） | Release | 活跃度 |
|---|---|---|---|---|
| Claude Code | 10 | 10 | v2.1.234 | 高 |
| OpenAI Codex | 10 | 10 | rust-v0.148.0-alpha.21 | 高 |
| Gemini CLI | 10 | 10 | v0.56.0-nightly | 高 |
| GitHub Copilot CLI | 10（总更新 29） | 1（存疑/疑似恶意） | 无 | 中 |
| Kimi Code CLI | 0 | 1 | 无 | 低 |
| OpenCode | 10 | 10 | 无 | 中高 |
| Pi | 10（总 50） | 10（总 34） | 无 | 中 |
| Qwen Code | 10 | 10 | v0.21.13 | 高 |
| DeepSeek TUI | 12 | 11 | v0.9.9 | 中 |
| Grok Build | 0 | 0 | 无 | 休眠 |

> 注：Issues/PRs 数为日报精选口径；Pi 底层实际更新量为 50 Issues / 34 PRs。Copilot CLI 当日唯一 PR（#4510）试图删除 README 中的 Copilot CLI 文档，需警惕。

## 3. 共同关注的功能方向

**① 非中断式任务/消息队列**
- Claude Code：#50246 消息队列模式（198 👍，社区最高呼声），排队指令不打断当前工作流
- OpenAI Codex：PR #39092 新增 `codex queue --thread --message`，向既有会话提交消息
- Pi：#8166 `triggerTurn: false` 插入自定义消息破坏 tool_calls 相邻性，暴露消息队列约束

**② 子代理/多智能体可靠性**
- Gemini CLI：#22323 子代理 MAX_TURNS 后谎报成功、#21409 generalist agent 永久挂起、#21968 模型不主动使用 skills
- OpenAI Codex：#15723 后台子代理完成不唤醒调用代理、#13491 子代理继承父代理意图导致递归委托
- Claude Code：#28300 跨机器 Agent-to-Agent 协议
- DeepSeek TUI：#1425 大文本分片启动 10 个子代理后超时卡死
- Qwen Code：#9290 agent-team 会话稳定性问题

**③ MCP 生产级治理**
- Copilot CLI：#4480/#4439 1.0.79 引入 OAuth 元数据回归（Atlassian/GitLab 均受影响）
- OpenAI Codex：#17265 路由 MCP OAuth token 不自动刷新
- Gemini CLI：PR #28863 扩展可绕过用户同意注入环境变量
- Claude Code：#69087 MCP 表单对话框在 TUI 中被裁剪不可滚动
- OpenCode：#33027 MCP 工具已连接但未暴露给 Agent

**④ 上下文压缩/内存治理/会话膨胀**
- Pi：#6879 上下文越过 100% 仍不触发压缩直至 API 400
- Copilot CLI：#4506 内存看门狗在 23% 上下文占用时循环压缩直至 OOM
- Qwen Code：#9320 `/compress-fast` 后上下文丢失
- Claude Code：#87238/#87319 per-tool 进程膨胀至 11.6GB、后台 Bash 进程泄漏 10.8GB
- OpenAI Codex：#34268 多代理 fork 导致会话存储膨胀至 110 GiB

**⑤ Windows 与 ARM64 平台体验**
- Claude Code：#80444 GPU 崩溃致应用不可启动、#86298 跨会话消息静默丢失
- Qwen Code：#9061 Windows Ctrl+V 粘贴回归（P1）
- OpenCode：#19130 Windows ARM64 TUI 初始化失败
- OpenAI Codex：#38754 Windows 下 MCP server 重复拉起不回收
- DeepSeek TUI：#1829 SSH 出站被沙箱阻断（exit 255）

**⑥ 用量与成本可观测性**
- Claude Code：#33978 内置 `claude usage` 命令（整合 10+ 个零散请求）
- Copilot CLI：#4504 `account.getQuota` 返回错误 resetDate
- Pi：#7995 openai-responses 缺 Anthropic 缓存格式致成本增加 2.5 倍
- DeepSeek TUI：#5470 DeepSeek V4 分时段定价按轮次解析

**⑦ 安全与权限收敛**
- Gemini CLI：#28740 eval-pr 工作流供应链 RCE 漏洞
- OpenAI Codex：#39083 Windows 沙箱提权绕过（junction/reparse point）
- DeepSeek TUI：#5491 审批决策执行前持久化，防止恢复后误执行
- Copilot CLI：#4503 SDK server 未认证即报告 ready
- Claude Code：#87395 插件可被模型自调用陷入无限循环

## 4. 差异化定位分析

| 工具 | 功能侧重 | 目标用户 | 技术路线/独特资产 | 当日暴露短板 |
|---|---|---|---|---|
| Claude Code | 全功能工作台（插件/hooks/桌面/IDE） | 企业研发团队、重度自动化用户 | 成熟脚本 API、插件生态、跨平台桌面；社区需求最热（198👍） | Windows 与内存稳定性 |
| OpenAI Codex | 模型能力优先 + 远程/多端协同 | 追新模型能力的开发者、远程办公者 | Rust 核心、Desktop/Mobile 同步、GPT-5.6 872k 上下文、alpha 高频迭代 | 预发布阶段、Mac 僵尸进程、会话存储膨胀 |
| Gemini CLI | 子代理编排 + 评估体系建设 | 信赖智能体委托的开发者 | SSR Agent 自动修复、76 项 behavioral eval、Auto Memory、nightly 构建 | 子代理挂起/谎报成功，高优 issue 超 5 个月未合入 |
| GitHub Copilot CLI | GitHub/Copilot 企业生态入口 | Copilot Business/Enterprise 组织 | 与组织模型目录、配额、Slack/ACP 深度集成 | 1.0.79 OAuth 回归、社区 PR 流近乎停滞 |
| Kimi Code | 极简脚本化/非交互调用 | 自动化/CI 集成开发者 | `--starting-prompt`（-s）参数化调用 | 社区活跃度极低，PR 评审周期长达半年 |
| OpenCode | 模型中立网关 + 插件 API 扩展 | 多模型用户、插件开发者 | 统一推理网关、Azure/Bedrock/Vertex 适配器、session request hook | 网关端点 410/403 频发、Windows 缺口 |
| Pi | 本地推理/成本敏感 + 事件化扩展 | 高级用户、vLLM/llama.cpp 本地推理用户 | provider 兼容层、缓存控制（#7995）、实验性追加压缩、事件契约标准化 | 压缩触发不可靠、大 diff 崩溃 TUI |
| Qwen Code | 中国生态 + Web Shell + 服务化 | 中国开发者、阿里云生态 | Web Shell 拖放附件、`qwen serve` daemon、微信渠道、对话 fork | Windows Ctrl+V 回归、压缩算法信任危机 |
| DeepSeek TUI | Rust TUI + shell 工具韧性 | DeepSeek/OpenCode API 用户、TUI 爱好者 | exec 流软失败、诚实定价标注、文档中文化 EPIC | main 分支双平台 CI 全红、配置路径碎片化 |
| Grok Build | — | — | — | 24 小时无任何动态 |

## 5. 社区热度与成熟度

**高活跃梯队**：Claude Code（最强需求 198👍、稳定版本 2.1.234 仍保持高频迭代）、OpenAI Codex（195👍 次热、alpha 版本快速推进）、Gemini CLI（SSR Agent 批量提交 PR，修复速度快）、Qwen Code（正式版 v0.21.13 + SWE-bench 500 全量验证）。

**中活跃梯队**：Copilot CLI（用户反馈活跃但维护响应弱，29 条更新仅 1 条 PR）、OpenCode（10+10 但无 release、架构重构中）、Pi（底层 50 Issues/34 PRs，偏技术细节打磨）、DeepSeek TUI（发版积极但 CI 门禁全红）。

**低活跃/休眠**：Kimi Code（唯一 PR 历时半年才关闭）、Grok Build（无动态）。

**成熟度判断**：Claude Code 与 Copilot CLI 已进入稳定 1.x/2.x 线，但当日均出现明显回归（Windows GPU 崩溃、OAuth 元数据破坏），说明用户规模扩大后质量挑战陡增；Codex、Gemini、Qwen、DeepSeek TUI 处于快速迭代期，版本节奏快但稳定性欠账较多；Pi 与 OpenCode 处于架构整理期（共享数据层重构、扩展事件语义标准化），是为下一阶段打基础的信号。

## 6. 值得关注的趋势信号

**① 异步代理工作流将成标配**。消息队列、后台子代理、排队指令在 Claude Code、Codex、Pi 三个仓库同时出现，交互范式正从"同步轮询"转向"异步任务编排"，决策者应关注工具对后台任务可见性和取消能力的支持。

**② MCP 进入"生产可用"治理时代**。OAuth 自动刷新、策略继承、进程生命周期、序列化边界是当前最高频的 MCP 议题，且半数与版本回归相关（Copilot 1.0.79 教训：升级前务必核对 changelog）。

**③ 上下文管理能力正在取代模型能力成为核心分水岭**。自动压缩触发不可靠、10GB+ 内存膨胀、110GiB 会话存储、缓存格式缺失导致 2.5 倍成本差——长会话用户应把"压缩可靠性 + 内存治理"纳入选型硬指标。

**④ Windows/ARM64 是尚未被占领的差异化市场**。主流的 macOS/Linux-first 团队普遍在 Windows 上出现 GPU 崩溃、粘贴回归、路径解析缺陷。面向企业 Windows 环境的用户，当前选择稀少，这既是风险也是机会。

**⑤ 子代理可信度决定多智能体能否落地**。谎报成功（Gemini #22323）、递归委托（Codex #13491）、绕过权限配置（Gemini #22093）等问题说明：在多智能体普及前，状态上报真实性、意图隔离、权限边界必须优先解决。

**⑥ 企业选型把成本可观测性纳入硬指标**。内置用量命令、分时段定价、配额 API 正确性、缓存透传成为跨工具共性需求。企业用户在采用前应确认工具是否提供透明的成本追踪能力。

**⑦ "AI 修复 AI" 的开源维护模式浮现**。Gemini 的 SSR Agent 批量产出修复 PR、Claude Code 与 Qwen Code 用自动化管线维护自身仓库（autofix、duplicate 检查、hook 断言）——开源项目的维护节奏正在被 AI 自动化重塑，社区贡献者需要适应与自动化 Agent 协作的新常态。

**⑧ 中国生态成为独立增长极**。微信渠道集成（Qwen Code）、文档中文化 EPIC（DeepSeek TUI）、中文翻译质量（Codex #31963）等信号显示，中文开发者群体对 AI CLI 工具的需求正从"能用"走向"好用"，具备本地化能力的工具将在该市场获得先发优势。

---

**对开发者的参考建议**：升级前关注目标版本的已知回归（Copilot 1.0.79 是当日最典型案例）；长会话/重负载场景优先选择压缩与内存治理成熟度高的工具；Windows 用户在选型前核实目标工具的平台支持状态；自动化/CI 场景优先支持非交互参数的工具（如 Kimi `--starting-prompt`）；安全敏感环境重点考察工具的审批持久化与权限模型。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区热点报告

**数据截止**：2026-08-18 | **数据来源**：[anthropics/skills](https://github.com/anthropics/skills)


## 一、热门 Skills 排行（评论/关注度 Top Skills）

### 1. skill-creator 评估链路修复（PR #1298）
- **功能**：修复 `run_eval.py` 在所有场景下误报 `recall=0%` 的严重问题（关联 Issue #556，已有 10+ 独立复现）。同时修复 Windows 流读取、触发检测与并行 worker 缺陷。
- **社区关注点**：skill-creator 的自动评估循环正在“基于噪声做优化”，直接影响 Skill 描述质量的迭代闭环。
- **状态**：Open（创建于 2026-06-10，持续更新中）
- [GitHub PR #1298](https://github.com/anthropics/skills/pull/1298)

### 2. document-typography 排版质量 skill（PR #514）
- **功能**：新增文档排版质量控制 skill，解决 AI 生成文档中常见的孤词换行（1-6 个词溢出到下一行）、寡妇段落（章节标题悬在页底）及编号错位等排版问题。
- **社区关注点**：所有 Claude 生成的文档都受影响，用户极少主动要求排版优化，需要 skill 自动兜底。
- **状态**：Open（2026-03-04 创建，2026-03-13 更新）
- [GitHub PR #514](https://github.com/anthropics/skills/pull/514)

### 3. ODT 文档技能（PR #486）
- **功能**：支持创建、填充、读取 OpenDocument 格式（.odt, .ods），并可将 ODT 解析为 HTML。触发词覆盖 `ODT / ODS / ODF / OpenDocument / LibreOffice` 等。
- **社区关注点**：补全了文档处理矩阵中缺失的开源标准格式支持，与现有 docx/pdf 技能形成互补。
- **状态**：Open（2026-03-01 创建，2026-04-14 更新）
- [GitHub PR #486](https://github.com/anthropics/skills/pull/486)

### 4. frontend-design 技能重构（PR #210）
- **功能**：系统修订 frontend-design skill，确保每条指令都可在单次对话中执行，且指导内容足够具体以约束模型行为，解决原版偏理论、可操作性不足的问题。
- **社区关注点**：Skill 设计哲学讨论——如何从“文档”变成“可执行的操作指南”。
- **状态**：Open（2026-01-05 创建，2026-03-07 更新）
- [GitHub PR #210](https://github.com/anthropics/skills/pull/210)

### 5. 元技能：质量与安全分析器（PR #83）
- **功能**：新增两个元技能并加入 marketplace——`skill-quality-analyzer`（从结构、文档、示例等五个维度评估 Skill 质量）与 `skill-security-analyzer`（安全审计）。
- **社区关注点**：社区开始对 Skill 本身做质量管控，是生态成熟的标志。
- **状态**：Open（2025-11-06 创建，2026-01-07 更新）
- [GitHub PR #83](https://github.com/anthropics/skills/pull/83)

### 6. skill-creator 系列批量修复（PR #538 / #539 / #541）
- **功能**：三个高关注度修复：
  - #538：pdf skill 中 8 处大小写敏感的文件引用错误，在 Linux/macOS 上直接导致 skill 加载失败
  - #539：检测 description 字段中未加引号的 YAML 特殊字符（如 `:`），避免静默解析失败
  - #541：修复 DOCX 技能添加修订时 `w:id` 与书签 ID 冲突导致的文档损坏
- **社区关注点**：Skills 生态正在经历一次“工程质量补课”，跨平台兼容与边界条件处理成为焦点。
- **状态**：均 Open
- [PR #538](https://github.com/anthropics/skills/pull/538) | [PR #539](https://github.com/anthropics/skills/pull/539) | [PR #541](https://github.com/anthropics/skills/pull/541)


## 二、社区需求趋势（Issues 洞察）

| 需求方向 | 代表 Issues | 热度信号 |
|---------|------------|---------|
| **安全与信任边界** | #492（43 评论）：社区 skill 在 `anthropic/` 命名空间下分发，导致信任边界滥用，用户可能向非官方 skill 授予过高权限 | ⭐⭐⭐ 已升级为生态治理问题，讨论持续至 7 月 |
| **组织级共享与协作** | #228（16 评论，8 👍）：企业用户要求 Skill 在 org 内直接共享，当前手动下载/上传流程效率过低 | ⭐⭐⭐ 企业采用的核心阻碍 |
| **评估与质量工具链** | #556（12 评论，7 👍）：`run_eval.py` 全链路不触发 skill，0% 触发率的严重缺陷 | ⭐⭐⭐ 直接影响 skill 迭代闭环 |
| **记忆与上下文管理** | #1329 / #1487：compact-memory 符号化记忆提案；claude-api skill 单次注入 156k tokens 撑爆上下文 | ⭐⭐ 长时运行与上下文窗口的矛盾凸显 |
| **安全与治理模式** | #412（6 评论）：agent-governance 提案——策略执行、威胁检测、信任评分与审计追踪 | ⭐⭐ Agent 规模化落地的治理空白 |
| **互操作与基础设施** | #16 / #29：Skills 以 MCP 形式暴露 API；Bedrock 使用支持 | ⭐ 用户期待更开放的集成生态 |
| **插件体系去重** | #189（6 评论，9 👍）：document-skills 与 example-skills 插件安装相同内容导致 Skill 重复注入，浪费上下文窗口 | ⭐⭐ 生态膨胀带来的副作用 |


## 三、高潜力待合并 Skills（近期可能落地）

| Skill | PR | 亮点 | 潜在价值 |
|-------|-----|------|---------|
| **self-audit 自审计** | [#1367](https://github.com/anthropics/skills/pull/1367) | 机械式文件校验 → 四维推理审计（按危害优先级排序），技术栈无关，通用性强 | 填补 AI 交付质量门禁空白，与 #1385 提案构成三关卡流程 |
| **testing-patterns 测试模式** | [#723](https://github.com/anthropics/skills/pull/723) | 覆盖 Testing Trophy、单元测试 AAA 模式、React Testing Library 等完整测试栈 | 开发者在测试生成上的刚需，覆盖面广 |
| **ServiceNow 平台技能** | [#568](https://github.com/anthropics/skills/pull/568) | 覆盖 ITSM、ITOM、ITAM/SAM、FSM、HRSD、CSM、SPM/PPM、SecOps 等全模块 | 企业级平台助手，近期仍持续更新（8/12），作者活跃 |
| **pyxel 复古游戏开发** | [#525](https://github.com/anthropics/skills/pull/525) | 基于 MCP 的服务端，支持 write → run_and_capture → inspect 迭代闭环 | 创意开发场景的差异化 skill，具备生态联动价值 |
| **document-typography** | [#514](https://github.com/anthropics/skills/pull/514) | AI 文档排版质量控制，解决孤词/寡妇段落/编号错位 | 文档类 skill 的必要补充，讨论热度高 |


## 四、生态洞察

> **一句话总结**：当前社区最集中的诉求是 **Skills 的工程化质量保障**——包括评估工具链修复（#556/#1298）、安全信任边界治理（#492）、跨平台兼容（#538/#1099/#1050）与上下文效率优化（#189/#1487），标志着 Claude Code Skills 生态正从“数量扩张”进入“质量与治理”阶段。

---

# Claude Code 社区动态日报 — 2026-08-18

## 今日速览

Claude Code 发布 v2.1.234，引入项目目录短名称自定义与清除选区快捷键。社区方面，消息队列模式请求（#50246）以 198 👍 成为最热门讨论，跨机器 Agent 协作（#28300）与内置用量分析（#33978）同样备受关注；与此同时，Windows 桌面端崩溃与多起内存泄漏报告使稳定性问题成为当日焦点。

## 版本发布

**v2.1.234**
- 新增可选环境变量 `CLAUDE_CODE_PROJECT_DIR_NAME`：为每个会话分配独立配置目录的主机，可为按项目的会话记录目录指定短名称
- 新增 `selection:clear` 键位绑定动作：可将快捷键绑定为清除应用内选区，完善 TUI 键盘操作

---

## 社区热点 Issues（10 条）

### 1. 消息队列模式 — 社区最热功能请求
[#50246](https://github.com/anthropics/claude-code/issues/50246) · [CLOSED] · 60 评论 · 👍 198

**请求**：当 Claude 正在执行任务时，唯一的操作是打断它。提案新增“消息队列模式”，让用户在不打断当前工作流的前提下排队后续指令，由 Claude 在适当时机逐个处理。这是目前社区呼声最高的增强请求，收到近 200 个赞，说明“非中断式跟随指令”是大量用户的刚需。

### 2. Windows 桌面应用 GPU 进程崩溃
[#80444](https://github.com/anthropics/claude-code/issues/80444) · [OPEN] · 39 评论 · 👍 5

**问题**：桌面应用 1.24012.1 在 Windows 上通过应用内浏览器页签触发致命 GPU 进程崩溃（0x060C201E），崩溃后 MSIX 包不可启动（appxState=2），必须执行“修复”才能恢复。已在两个 NVIDIA 驱动版本上复现。属于严重平台稳定性缺陷，影响面大。

### 3. 跨机器多智能体协作（Agent-to-Agent 协议）
[#28300](https://github.com/anthropics/claude-code/issues/28300) · [OPEN] · 38 评论 · 👍 0

**请求**：希望引入 Agent 间协议，支持分布在多台机器上的多个 Claude Code 实例协同完成现代软件工程任务。社区讨论了握手协议、安全边界、任务分割方式等设计问题，体现对分布式 AI 开发工作流的强烈兴趣。

### 4. 内置用量分析命令 `claude usage`
[#33978](https://github.com/anthropics/claude-code/issues/33978) · [OPEN] · 20 评论 · 👍 10

**请求**：提议新增内置的 token 用量与成本分析命令，整合社区中 10+ 个零散的用量统计请求。用户希望无需依赖第三方工具即可追踪花费、估算成本。评论中大量开发者贴出自己的用量痛点和统计需求。

### 5. Windows 桌面端跨会话消息静默丢失（回归）
[#86298](https://github.com/anthropics/claude-code/issues/86298) · [OPEN] · 13 评论 · 👍 1

**问题**：自桌面应用 1.28929.0 起，跨会话消息被静默丢弃——消息被挂起等待一个 UI 永远不会提供的审批，约 5 分钟后过期。报告人整理了与 #86212、#85888 等相近 issue 的关联，属于关键回归，严重影响 Windows 用户的日常流程。

### 6. Esc 退出 `/btw` 模式误拒绝待定工具调用
[#64568](https://github.com/anthropics/claude-code/issues/64568) · [OPEN] · 10 评论 · 👍 9

**问题**：在 `/btw` 模式下，当存在待处理的工具使用/权限提示时，按 Esc 本应退出该模式，却会被路由到待定提示并拒绝工具调用。用户意图仅是离开模式，结果却意外拒绝了工具执行。TUI 模式状态管理存在冲突。

### 7. Bash-tool `grep` 模拟导致灾难性回溯（OOM）
[#82179](https://github.com/anthropics/claude-code/issues/82179) · [OPEN] · 4 评论 · 👍 0

**问题**：Bash 工具将 `grep` 替换为内嵌 ugrep 模拟（通过 `exec -a ugrep`），当模式组合 `-o` 与带边界量词的选择结构时发生灾难性回溯，对 20 KB 文件造成 6.6 GB RSS 并触发 OOM。属于安全/稳定性隐患，需优化正则引擎或回退策略。

### 8. Per-tool 辅助进程内存膨胀至 11.6 GB 后 OOM
[#87238](https://github.com/anthropics/claude-code/issues/87238) · [CLOSED] · 3 评论 · 👍 0

**问题**：正常交互期间，临时 per-tool-call 辅助进程在约 2 分钟内匿名 RSS 膨胀至 11.6 GB，在 12 GB cgroup 上限被内核 OOM 杀死。慢速沙箱 Bash 命令场景下触发，需排查内存分配泄漏。同日另有 #87319（后台 Bash 进程泄漏至 10.8 GB）报出类似问题。

### 9. MCP 表单对话框在 TUI 中被裁剪且不可滚动
[#69087](https://github.com/anthropics/claude-code/issues/69087) · [OPEN] · 3 评论 · 👍 2

**问题**：全屏 TUI 下，MCP 引导表单对话框内容超出视口，操作按钮位于可视区域之下，且无法滚动访问。影响 MCP 工具的交互可用性，全屏模式下无法正常完成表单提交。

### 10. 持久语音对话功能请求
[#83434](https://github.com/anthropics/claude-code/issues/83434) · [OPEN] · 1 评论 · 👍 0

**请求**：一位急救护理员（Advanced Care Paramedic）提交在移动中通过语音使用 Claude Code 的场景——希望获得真正的双向语音对话且闲置不断开连接。展示出非桌面场景对 Claude Code 的扩展需求，覆盖移动/语音助手使用形态。

---

## 重要 PR 进展（10 条）

### 1. 修复 ralph-wiggum 插件可被模型自调用
[#87395](https://github.com/anthropics/claude-code/pull/87395) · [CLOSED]

**修复**：`/ralph-loop` 和 `/cancel-ralph` 命令原本在 frontmatter 中使用不支持的 `hide-from-slash-command-tool` 字段，导致无实际效果——模型可自行调用 `/ralph-loop` 并陷入无限循环。改用 `disable-model-invocation` 正确阻止模型自我触发，并附带了复现案例。

### 2. 限制插件开发工具 frontmatter 解析范围
[#84004](https://github.com/anthropics/claude-code/pull/84004) · [CLOSED]

**修复**：基于 `sed` 的范围表达式会在后续每个 `---` 行重新开始匹配。如果设置文件的 Markdown 正文包含水平分隔线（`---`），这些文本可能被错误解析为 frontmatter 内容。改为仅解析开头的 YAML frontmatter 块，并拒绝缺少开始/结束标记的文件。

### 3. 脚本顶层失败现在能正确传播
[#84003](https://github.com/anthropics/claude-code/pull/84003) · [CLOSED]

**修复**：duplicate-maintenance 脚本此前使用 `.catch(console.error)`，导致启动和 API 故障被记录但脚本仍以成功状态退出。现在脚本会以失败状态退出，同时保留错误日志并等待输出刷新完毕。

### 4. 严格校验受限制 `gh` wrapper 的标志值
[#83999](https://github.com/anthropics/claude-code/pull/83999) · [CLOSED]

**修复**：在受限 `gh` 包装器中，`skip_next=true` 在输入结束时残留，导致 `gh issue list --limit` 这类不完整命令被原样转发，绕过参数验证。现在拒绝缺少值的取值标志，防止绕过安全限制。

### 5. 校验标签选项值
[#83995](https://github.com/anthropics/claude-code/pull/83995) · [CLOSED]

**修复**：`--add-label` / `--remove-label` 缺少值时，`set -u` 会导致脚本以隐晦的 `$2: unbound variable` 错误中止；此外后续选项还可能被误吞为标签名。现在先校验标签名再读取下一个位置参数。

### 6. 拒绝自引用重复 issue
[#83993](https://github.com/anthropics/claude-code/pull/83993) · [CLOSED]

**修复**：`comment-on-duplicates.sh` 此前接受相同的 base 和 duplicate issue 编号，发布一条自引用重复评论并返回成功，该评论还可能被自动化流程消费。现在脚本会拒绝 self-referential duplicates。

### 7. 断言 hook 的预期决策
[#83992](https://github.com/anthropics/claude-code/pull/83992) · [CLOSED]

**修复**：`test-hook.sh` 原来把 allow 和 deny 都视为成功执行，无法捕获“本应拒绝却放行”的 hook。新增可选 `--expect allow|deny|ask` 标志，可显式断言期望的 hook 决策，增强测试有效性。

### 8. 报告缺失的 `jq` 依赖
[#83990](https://github.com/anthropics/claude-code/pull/83990) · [CLOSED]

**修复**：`test-hook.sh` 将 `jq empty` 的一切失败都当作无效 JSON。当 `jq` 未安装时，shell 错误被静默吞掉，合法输入也会被误报为格式错误。现在在首次使用前检查 `jq` 存在与否，并明确报告缺失依赖。

### 9. validate-settings.sh 无匹配键时不应中止
[#79131](https://github.com/anthropics/claude-code/pull/79131) · [OPEN]

**修复**：当没有 frontmatter 键匹配小写字段模式时，`grep` 返回 1，与 `set -euo pipefail` 叠加导致脚本无任何诊断信息直接中止；被跳过的键也从不报告。现在脚本在无匹配时能正常退出并给出提示。

### 10. 添加容器隔离示例与 guard hook
[#30692](https://github.com/anthropics/claude-code/pull/30692) · [CLOSED]

**功能**：新增 `examples/container/`，提供在 Podman/Docker 容器中运行 Claude Code 的完整示例（替代内建沙箱）。包含 `guard-destructive-git` PreToolUse hook，可拦截强制推送、硬重置、分支删除、`rm -rf` 和 PR 合并等危险操作，单独使用无需容器环境。

---

## 功能需求趋势

从全部 Issues 中提炼出社区最关注的功能方向：

### 1. 非中断式任务管理
消息队列模式（#50246）大热说明用户强烈需要“想不打断就不打断”的交互——排队指令、延后处理、后台任务过多时的可见性与控制（#60095）也同属此类。

### 2. 多智能体协作与编排
从 Agent-to-Agent 跨机器协议（#28300）到子代理/后台任务生命周期管理（#60095），社区正推动 Claude Code 从单一助手向多智能体系统演进。

### 3. 用量与成本可观测性
内置 `claude usage` 命令（#33978）整合了 10+ 个独立请求，token 计数、成本追踪、会话限额透明度（#71594）成为企业级用户的普遍诉求。

### 4. 语音与移动场景支持
持续语音对话（#83434）、语音模式稳定性（#72540）表明部分用户将 Claude Code 当作“随身助手”使用，移动化/口语化交互是明确的潜在方向。

### 5. 更细粒度的键盘与 TUI 控制
新增 `selection:clear` 键位动作、Esc 模式冲突修复（#64568）、TUI 滚动缓冲不足（#72529）、表单对话框裁剪（#69087）——围绕键盘操作与界面排布的精细化改进持续出现。

### 6. MCP 生态整合质量
MCP 指令注入上下文（#48680）、表单交互缺陷（#69087）显示 MCP 在日常工作流中的渗透率在提升，相应交互细节和质量问题正成为新焦点。

---

## 开发者关注点

### 高频痛点

- **内存泄漏/OOM 问题集中爆发**：当日至少 3 条高严重度报告——grep 模拟灾难性回溯（#82179）、per-tool 进程膨胀至 11.6 GB（#87238）、后台 Bash 进程泄漏 10.8 GB（#87319）。Linux/macOS 用户对沙箱进程生命周期管理提出质疑。
- **Windows 平台稳定性堪忧**：GPU 崩溃导致应用不可启动（#80444）、跨会话消息静默丢弃（#86298）、会话恢复报错（#72539）、映射网络驱动器会话列表为空（#78461）——Windows 桌面端在 8 月中旬出现多起回归。
- **模型指令遵循不一致**：明确接受完成条件却提前停止（#86261）、将工具调用渲染为字面文本（#63580）、技能参数误替换 `$0`（#87201），开发者对模型行为的不可预期性持续不满。

### 开发与脚本建议

- PR 集中修复了插件开发脚本（frontmatter 解析、hook 决策断言、依赖检查）和维护脚本（`gh` 参数校验、重复 issue 判断）的一系列边界条件问题，提示社区工具链存在测试覆盖不足。
- 多个脚本修复（#83999、#83995）都涉及 `set -u` / `pipefail` 下的“静默失败”或“非预期中止”，建议开发者在 bash 脚本中显式处理参数缺失场景。

### 平台与生态

- 关于 VSCode 扩展的多个 issue（#63580、#72261、#78461）表明 IDE 集成仍是相对薄弱环节，尤其是 Windows 路径解析和设置透传。
- 桌面环境默认值被旧会话“静默打败”（#87398）、跨会话审批丢失（#86298）等新出现的问题，显示会话状态的持久化逻辑正成为稳定性短板。

---

*数据来源：[github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) · 2026-08-18*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报（2026-08-18）

## 今日速览

昨日社区讨论热度集中在 **MCP 认证与进程管理**、**TUI 与子代理导航体验** 以及 **Windows 平台稳定性** 三大方向。最受关注的 Issue 是“禁用 60 秒自动解析问答”的配置请求（78 条评论、195 个 👍），而 PR 侧则密集合并了一批 TUI、MCP 策略与远程工作区相关改进。另发布了一个 CLI 预发布版本 `rust-v0.148.0-alpha.21`。


## 版本发布

### rust-v0.148.0-alpha.21
- **版本号**：0.148.0-alpha.21
- **发布说明**：仅标注 “Release 0.148.0-alpha.21”，未附带额外变更描述。
- **链接**：https://github.com/openai/codex


## 社区热点 Issues（10 个）

1. **#28969 Add setting to disable the auto-resolve in 60 seconds for questions**
   - 热度：78 评论 / 195 👍（本期最高）
   - 核心诉求：用户希望增加配置项，关闭/调整 60 秒自动解析问题的行为。
   - 链接：https://github.com/openai/codex/issues/28969

2. **#17265 Codex does not auto-refresh routed MCP OAuth tokens even when a refresh token is stored**
   - 热度：31 评论 / 57 👍
   - 影响：MCP access token 过期后，工具调用开始失败，且刷新 token 已持久化却未自动使用。
   - 链接：https://github.com/openai/codex/issues/17265

3. **#37403 [macOS][regression] Desktop cannot resume Remote Control / CLI thread: `already has an active writer` after latest update**
   - 热度：21 评论 / 17 👍
   - 影响：桌面端远程控制/CLI 线程恢复被回归 bug 阻断，影响 macOS 用户远程工作流。
   - 链接：https://github.com/openai/codex/issues/37403

4. **#25744 Codex for macOS accumulates Computer Use / MCP helper processes and unreaped zombie children**
   - 热度：19 评论 / 3 👍
   - 影响：长时运行会话导致 HID 延迟和 WindowServer/TCC 停滞，属资源泄漏。
   - 链接：https://github.com/openai/codex/issues/25744

5. **#15723 Background subprocesses/subagents do not wake the calling agent on completion**
   - 热度：18 评论 / 8 👍
   - 影响：后台子代理完成后不唤醒调用代理，导致多代理协作流程卡顿。
   - 链接：https://github.com/openai/codex/issues/15723

6. **#17793 Backspace seems to delete more than one character, making prompting harder**
   - 热度：16 评论 / 5 👍
   - 影响：TUI 输入退格一次删除多字符，直接影响日常 prompting 体验。
   - 链接：https://github.com/openai/codex/issues/17793

7. **#23418 Mobile remote-created worktree thread is not associated with project in Codex Desktop sidebar**
   - 热度：12 评论 / 4 👍
   - 影响：移动端远程创建的线程无法出现在桌面侧边栏项目中，影响跨端工作流一致性。
   - 链接：https://github.com/openai/codex/issues/23418

8. **#13491 Forked Worker Inherits Parent User Intent and Misinterprets It as Direct Instruction（递归委托）**
   - 热度：10 评论 / 11 👍
   - 影响：子代理继承父代理的用户意图并误判为直接指令，导致递归委托，值得关注。
   - 链接：https://github.com/openai/codex/issues/13491

9. **#34268 Multi-agent V2 full-history forks duplicate historical compaction snapshots… >100 GiB session storage growth**
   - 热度：9 评论 / 6 👍
   - 影响：多代理 V2 全历史 fork 导致会话存储膨胀至 110 GiB，属严重资源问题。
   - 链接：https://github.com/openai/codex/issues/34268

10. **#38754 [Windows] Local stdio MCP servers are repeatedly spawned and not reaped within a single task**
    - 热度：7 评论 / 2 👍
    - 影响：Windows 上每轮对话都会重复拉起 MCP server 且不回收，进程泄漏明显。
    - 链接：https://github.com/openai/codex/issues/38754

> 此外 #31963（zh-CN 翻译将 xhigh/ultra 均渲染为“极高”）、#33282（Windows worktree 不继承自动批准模式）、#33599（node_repl MCP 工具静默失败）也值得关注。


## 重要 PR 进展（10 个）

1. **#39094 Add an agents overview dashboard to the TUI**（新功能）
   - 新增 `/agents` 命令，展示全部根会话及子代理状态，支持搜索、导航、按项目/状态分组。
   - 链接：https://github.com/openai/codex/pull/39094

2. **#39092 Add a command to queue messages for existing sessions**（新功能）
   - 新增 `codex queue --thread <THREAD> --message <TEXT>`，可通过 app-server API 向既有会话提交消息。
   - 链接：https://github.com/openai/codex/pull/39092

3. **#39102 Raise the GPT-5.6 maximum context window**（模型支持）
   - 将 `gpt-5.6-sol/terra/luna` 上下文窗口上限提升至 872,000 tokens，并同步 Bedrock 条目。
   - 链接：https://github.com/openai/codex/pull/39102

4. **#39101 Update rmcp to 3.1.2**（依赖升级）
   - 升级 MCP Rust 库 `rmcp` 至 3.1.2，移除本地兼容层，支持原生 JSON-RPC 解码与 OAuth protected-resource 元数据。
   - 链接：https://github.com/openai/codex/pull/39101

5. **#39091 Make codex-otel OTLP HTTP exporters proxy-aware**（可观测性）
   - 所有 OTLP/HTTP 导出链路（logs/traces/metrics/Statsig）接入代理感知传输层，保留 TLS 与超时配置。
   - 链接：https://github.com/openai/codex/pull/39091

6. **#39088 Harden TUI subagent navigation**（TUI）
   - 统一 `/subagents` 导航、避免覆盖子代理设置、仅向活动线程路由通知/审批。
   - 链接：https://github.com/openai/codex/pull/39088

7. **#39083 Harden Windows sandbox provisioning against reparse points**（安全）
   - 防止提权配置通过 junction/reparse point 将 ACL 应用到非预期目录，修复潜在安全风险。
   - 链接：https://github.com/openai/codex/pull/39083

8. **#39082 Prompt for project trust in remote TUI workspaces**（远程）
   - 远程 TUI 工作区在启动线程前查询项目配置层并显示信任提示，避免未授权配置加载。
   - 链接：https://github.com/openai/codex/pull/39082

9. **#39079 Apply user MCP policy to selected executor plugins**（MCP）
   - 将用户 MCP 策略（enablement、allow/deny、审批模式）应用到 executor-plugin 根，同时保留插件侧更严格策略。
   - 链接：https://github.com/openai/codex/pull/39079

10. **#39087 Read plugin authentication state from AuthManager**（架构）
    - `PluginsManager` 改为共享 `AuthManager` 状态，保证插件发现/启动/CLI 配置的认证模式一致。
    - 链接：https://github.com/openai/codex/pull/39087

> 补充：#39093（app-server 测试迁移至共享 HTTP 客户端）仍在开放中，建议关注。


## 功能需求趋势

从全部 Issue 中提炼出以下产品方向：

1. **MCP 生态成熟度** — 包括 OAuth 自动刷新、MCP 进程生命周期管理、策略继承/覆盖、工具附加可靠性，是当前最集中的功能诉求。
2. **远程开发体验** — 移动端远程控制与桌面端的项目/线程关联、远程 TUI 项目信任提示、remote compact 稳定性是社区明显关注的工作流场景。
3. **Windows 平台追赶** — 多起 Windows 专属问题（进程泄漏、沙箱凭据、DPAPI 错误、磁盘读循环）表明该平台体验仍落后于 macOS/Linux。
4. **TUI/终端体验细节** — 更精细的导航（/agents、/subagents）、退格行为、重放缓冲区、终端渲染效率成为持续改进方向。
5. **模型上下文与行为控制** — 社区希望更大的上下文窗口（如 GPT-5.6 872k），也关注模型在大型代码库上过度“自建验证/治理层”的行为。
6. **可观测性与审计** — Opt-in OTel 日志、OTLP 代理支持、追踪上下文保留等需求，说明开发者在真实生产中监控 agent 行为。


## 开发者关注点

- **MCP 模块“又爱又恨”**：MCP 相关 Issue 占比最高，认证刷新、进程堆积、工具附加失败等问题直接影响实际可用性。
- **多设备/远程工作流不一致**：移动端远程创建的工作树、桌面端会话关联、远程控制恢复失败等问题，说明多端同步仍是短板。
- **资源泄漏问题规模化浮现**：macOS 僵尸进程、Windows MCP 重复拉起、会话存储 110 GiB 膨胀——稳定性和资源管理成为高频痛点。
- **自动化任务可靠性受质疑**：#38350 中定时任务在成功运行后被自动禁用且未经授权，直接影响用户对 Automations 功能的信任。
- **模型行为治理需求上升**：GPT-5.6 在成熟代码库上过度“自我强化验证流程”（#39059），以及子代理意图继承混乱（#13491），反映出用户对 agent 行为可控性的关注。
- **社区治理与激励**：#37585 提出为高质量 bug 报告提供额外使用额度；#39089 亦在澄清外部贡献政策，显示社区在积极参与治理讨论。

---
*本日报基于 github.com/openai/codex 公开数据整理，仅反映 2026-08-17 至 2026-08-18 的社区动态。*

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 — 2026-08-18

## 今日速览

昨日社区动态集中于 **子代理（Subagent）可靠性** 与 **SSR Agent 批量修复** 两大主线：多个长期悬而未决的高优 issue（如 Subagent 在 MAX_TURNS 后误报成功、generalist agent 挂起）获得了由 SSR Agent 提交的针对性 PR（如 #28815、#28812），显示 Google 内部自动化修复系统正加速推进。此外，**Auto Memory 安全性** 引发新一轮讨论，出现多条关于 redaction 与低信号重试的 issue（#26522、#26525）。

## 版本发布

**v0.56.0-nightly.20260817.g9a15c45fb**

本次更新仅包含一个提交：由 `joneba-google` 提交的 [PR #28813](https://github.com/google-gemini/gemini-cli/pull/28813)，为 `packages/cli` 的 tsconfig 添加 `composite` 标志，修复 SSR Agent 在此前 issue #21911 中暴露出的 TypeScript 项目引用问题。属于常规夜间构建迭代，不涉及面向用户的功能变更。

## 社区热点 Issues

挑选近 24 小时内讨论最活跃、优先级最高或对社区影响最大的 10 个 Issue：

1. **[#22323] Subagent 在 MAX_TURNS 后用尽后误报 GOAL 成功**（P1, 12 评论）
   作者 `matei-anghel` 发现 `codebase_investigator` 子代理在未执行任何分析就触达最大 turn 限制时，仍返回 `status: "success"` 与 `Termination Reason: "GOAL"`，掩盖了中断本质。该问题已在 [PR #28815](https://github.com/google-gemini/gemini-cli/pull/28815) 中提交修复方案。
   🔗 https://github.com/google-gemini/gemini-cli/issues/22323

2. **[#21409] Generalist agent 一调用就永久挂起**（P1, 8 👍, 8 评论）
   用户反映当 CLI 委托给 generalist agent 时（哪怕只是创建文件夹），会无限期挂起且无法取消。用户通过指示模型不使用 subagent 可规避此问题。这是社区反馈最强烈的高优 bug 之一，已被标记为 `status/need-retesting`。
   🔗 https://github.com/google-gemini/gemini-cli/issues/21409

3. **[#19873] 利用模型 bash 亲和性：零依赖 OS 沙箱与执行后意图路由**（P2, 8 评论）
   一个架构级 enhancement 提案：利用 Gemini 3 模型天然擅长 POSIX 工具链的特性，通过 OS 级沙箱提升安全性与执行效率。反映了社区对“模型原生能力”与“安全隔离”结合的前沿探索。
   🔗 https://github.com/google-gemini/gemini-cli/issues/19873

4. **[#24353] 组件级评估体系（EPIC）**（P1, 7 评论）
   作为行为评估体系的大型 EPIC，当前已有 76 个 behavioral eval 测试，覆盖 6 种 Gemini 模型。社区正在推动更细粒度的组件评估基础设施，这直接关系到 CLI 的长期稳定性。
   🔗 https://github.com/google-gemini/gemini-cli/issues/24353

5. **[#22745] AST 感知的文件读取/搜索/代码库映射影响评估（EPIC）**（P2, 7 评论）
   探讨是否值得引入 AST 感知工具：单次调用即可精确读取方法边界、减少 token 噪声和错误读取往返。该 EPIC 下有多个关联子任务。
   🔗 https://github.com/google-gemini/gemini-cli/issues/22745

6. **[#21968] Gemini 不会主动使用 skills 和 sub-agents**（P2, 6 评论）
   社区反馈：即使配置了明确的 gradle/git 自定义技能，Gemini 在相关场景下仍不主动触发。该 issue 与另一个现象一致——模型工具调用策略需要更智能地匹配用户自定义资产。
   🔗 https://github.com/google-gemini/gemini-cli/issues/21968

7. **[#26522] Auto Memory 对低信号会话无限重试**（P2, 5 评论）
   指出 Auto Memory 后台提取代理只将 `read_file` 成功的会话标记为已处理——如果代理因低信号拒绝读取某会话，该会话会被反复重新提取，浪费 token 与计算资源。
   🔗 https://github.com/google-gemini/gemini-cli/issues/26522

8. **[#26525] Auto Memory 需确定性 redaction 并减少日志**（P2, 4 评论）
   安全相关问题：Auto Memory 会把本地会话内容送入模型上下文之后才提示 redaction，此外其服务日志可能记录已存在的技能内容。社区关注隐私与最小化数据暴露。
   🔗 https://github.com/google-gemini/gemini-cli/issues/26525

9. **[#25166] Shell 命令执行完成后卡在 "Waiting input"**（P1, 3 👍, 4 评论）
   即使执行最简单的 CLI 命令，Gemini 偶尔会在命令结束后挂起，界面仍显示进程活跃。属于核心体验的 P1 bug，影响高频 shell 操作的用户。
   🔗 https://github.com/google-gemini/gemini-cli/issues/25166

10. **[#22093] v0.33.0 之后子代理绕过权限配置自动执行**（P2, 3 评论）
    用户报告配置文件中 agents 已设为 disabled，但升级后 generalist 等子代理仍被自动调用。对权限控制机制的可信度提出了质疑，与 #21968 形成“过度使用”与“不足使用”两个极端。
    🔗 https://github.com/google-gemini/gemini-cli/issues/22093

## 重要 PR 进展

1. **[PR #28815] 修复 Subagent 恢复时原始终止原因被覆盖**（P1, 对应 #22323）
   当子代理在最终宽限回合调用 `complete_task` 时，原先的 `MAX_TURNS`/`TIMEOUT` 终止原因被 `GOAL` 覆盖。SSR Agent 修复了 `LocalAgentExecutor` 中的属性合并逻辑，保留真实终止原因。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28815

2. **[PR #28812] 为 TUI 初始化添加执行超时，防止无限挂起**（P1, 对应 #21477）
   在裸 Linux 终端中，TUI 初始化时 `getProcessInfo()` 依赖 `execAsync` 执行 `ps` 命令可能无限等待。该 PR 为相关调用增加超时保护，防止卡死在 "Initializing..."。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28812

3. **[PR #28816] 修复 MessageBus.request 发布失败时的静默挂起**（P2, 对应 #22588）
   此前 `this.publish()` 是一个无失败处理的浮动 Promise，reject 时会导致请求静默挂起 60 秒。该 PR 将发布失败纳入错误处理，避免长时等待无响应。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28816

4. **[PR #28863] 扩展环境变量变更需用户确认 + 运行时敏感变量清洗**（安全相关, 新提交）
   解决扩展更新可绕过用户同意、向 MCP server 注入未授权环境变量的问题。通过将 MCP 环境配置纳入同意字符串生成，并消毒自定义环境变量。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28863

5. **[PR #28864] 将 cli_help 子代理输出格式化为 markdown**（对应 #19463）
   修复 CLI 回答中泄露内部思考与模型独白的问题，改进 `cli_help` 子代理结果的呈现格式。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28864

6. **[PR #28819] 修复个人账户展示误导性的企业错误信息**（P2, 对应 #24587）
   个人账户用户选择不可用模型时，此前显示企业专属错误文案，现改为更准确的提示。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28819

7. **[PR #28834] 消除工作区扫描中瞬态子目录的虚假 ENOENT 警告**（社区贡献）
   BFS 目录遍历时遇到的锁文件目录在 `readdir` 与递归下降之间消失导致警告噪音。该修复将非根目录的 ENOENT 视为正常竞态。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28834

8. **[PR #28740] 修复 eval-pr 工作流中的供应链 RCE 漏洞**（安全, 对应 #28336）
   解决 `pull_request_target` 上下文中不可信 fork 代码可被以特权执行的问题，将构建与执行拆分为安全的 `workflow_run` 两阶段。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28740

9. **[PR #28744] ACP 恢复会话前不启动全新聊天，避免污染会话文件**（P1, 对应 #28693）
   修复 `loadSession` 路径中 `initializeSessionConfig` 内部导致会话文件被写污染的 init 调用。部分解决 #28693，仍有另一处 fresh-chat 待修。
   🔗 https://github.com/google-gemini/gemini-cli/pull/28744

10. **[PR #28624] 防止布尔 thought 部分泄漏为 "[Thought: true]" 文本**（P2, 对应 #23525）
    修复内部 thought parts 中布尔 `thought: true` 字段进入文本表示层，避免在输出中显示无意义的 `[Thought: true]` 片段。
    🔗 https://github.com/google-gemini/gemini-cli/pull/28624

## 功能需求趋势

综合近期 Issues 与 PRs，当前社区最关注的功能方向可归纳为以下几类：

1. **Subagent 行为智能化与可靠性**（#21409、#21968、#22093、#22323）
   - 模型自主调用子代理/技能的判断力不足（要么过度调用、要么几乎不调用）
   - Subagent 执行状态上报不准确、恢复逻辑掩盖真实结束原因
   - 权限系统的可信度（配置 disabled 后被绕过）

2. **Auto Memory 系统的成熟化**（#26516、#26522、#26523、#26525）
   - 需要更智能的会话筛选（低信号不要反复重试）、更强的隐私保护（进入模型上下文前完成确定性 redaction）
   - 无效 memory patch（malformed / 路径逃逸）需要 surface 或隔离，而非静默跳过

3. **AST 感知的代码理解能力**（#22745、#22746）
   - 社区与维护者均在探索：基于 AST 的精确文件读取、方法边界识别、代码库映射
   - 目标：减少 token 开销、降低错误读取与对齐的往返次数

4. **安全与权限的加固**（#19873、#28740、#28863）
   - 从 CI/CD 供应链到 MCP server 环境变量注入，安全修复持续高优
   - 长期方向：模型偏向原生 bash 操作，需要 OS 级沙箱兼顾能力与安全

5. **交互体验修复**（#25166、#22465、#22466）
   - Shell 执行完成后挂起、交互式提示卡死、`\n` 转义行为异常等高频体验问题仍被反复反馈

## 开发者关注点

- **“挂起”是最核心痛点**：无论是 generalist agent 挂起、shell 命令结束后 “Waiting input” 卡死、还是 TUI 初始化时的无限等待，多个 P1 bug 都指向同类型的可靠性问题。开发者在等待期间无法取消或干预，严重影响日常使用。
- **Subagent 的状态报告中“谎报成功”**：MAX_TURNS 被吞没、以 GOAL 形式上报的 bug（#22323）引发了对子代理执行过程透明度的质疑，#22598 提出的“subagent 轨迹应在 /chat share 中可见”诉求与之呼应。
- **自定义资产（技能/子代理）不被主动使用**：配置了 gradle/git 等 skills 却几乎不会被模型主动触发的反馈，提示工具选择策略仍需改进。
- **对自动化修复速度的关注**：SSR Agent 批量提交的 PR（#28812-#28820）表明 Google 在将 issue 修复流程自动化，社区 PR 作者被标记为 `status/pr-nudge-sent` 的条目也在增多，但部分高优 bug（如 #21409）从创建至今已超 5 个月仍未合入修复，开发者希望能加快闭环速度。
- **隐私与安全敏感度上升**：Auto Memory 在模型上下文后 redaction、扩展环境变量注入、fork PR 供应链攻击等安全问题获得了较多关注，开发者对 CLI 在后台读取内容与执行操作的态度趋于谨慎。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报 — 2026-08-18

## 今日速览

过去 24 小时无新版本发布，但社区保持活跃：共收到 29 条 Issue 更新和 1 条 PR。最值得关注的是 **1.0.79 版本疑似引入 MCP OAuth 认证回归**（Atlassian、GitLab 均受影响），以及一个**内存压力看门狗循环压缩导致 OOM** 的严重稳定性缺陷（#4506）。此外，出现了一条**试图删除 README 中 Copilot CLI 文档的 PR**（#4510），建议维护者警惕。

---

## 版本发布

过去 24 小时内无新版本发布。

---

## 社区热点 Issues（Top 10）

### 1. SHIFT+ENTER 应换行，却执行了 prompt — 讨论持续升温
**#1481 | 状态：已关闭 | 评论 28 | 👍 17**
用户在大多数聊天应用中习惯用 `SHIFT+ENTER` 换行，而 Copilot CLI 用 `CTRL+ENTER` 换行，`SHIFT+ENTER` 会直接执行 prompt。该 Issue 虽于 2 月创建并已关闭，但仍在被讨论——长期未解决的键盘交互问题，用户普遍认为不符合直觉，建议重开或至少文档化该行为。
🔗 [github/copilot-cli Issue #1481](https://github.com/github/copilot-cli/issues/1481)

### 2. 组织已启用的模型在目录中缺失（Claude Sonnet 5/Opus 5、Kimi K3）
**#4390 | 状态：开放 | 评论 8 | 👍 7**
Copilot Business 组织明确启用的 Anthropic（Claude Sonnet 5/Opus 5）和 Kimi K3 模型在 Copilot CLI 中不可用，选择时提示"模型已被组织禁用"。这会直接阻碍企业用户使用 CLI 访问已购买的模型服务，影响面较大。
🔗 [github/copilot-cli Issue #4390](https://github.com/github/copilot-cli/issues/4390)

### 3. Atlassian MCP OAuth 失败 — 1.0.79 回归
**#4480 | 状态：开放 | 评论 5 | 👍 6**
连接 `https://mcp.atlassian.com/v1/mcp` 时，OAuth 发现阶段报 "Incompatible authorization server"（RFC 8414 §3.3），**1.0.71 正常，1.0.79 回归**。这证实了 1.0.79 在 MCP OAuth 元数据处理上引入了破坏性变更，影响所有 OAuth 保护的远程 MCP 服务。
🔗 [github/copilot-cli Issue #4480](https://github.com/github/copilot-cli/issues/4480)

### 4. GitLab MCP OAuth 元数据被拒 — RFC 8414 issuer 不匹配
**#4439 | 状态：已关闭 | 评论 5 | 👍 3**
GitLab Self-Managed MCP 服务器在 OAuth 2.0 Dynamic Client Registration 下认证失败，CLI 报告 RFC 8414 issuer 不匹配。与 #4480 同样涉及 1.0.79 的 OAuth 元数据解析，基本可以确定是同一处回归。
🔗 [github/copilot-cli Issue #4439](https://github.com/github/copilot-cli/issues/4439)

### 5. SDK server 未认证就报告 ready，Slack 会话创建失败
**#4503 | 状态：已关闭 | 评论 5**
通过 Slack DM 调用 Copilot 时，SDK server 在缺少 `COPILOT_SDK_AUTH_TOKEN` 的环境中启动并报告 ready，随后 Slack 会话创建报"无法创建会话"。暴露了 SDK server 生命周期中**认证状态检查缺失**的问题，对 Slack/Chat 集成场景影响大。
🔗 [github/copilot-cli Issue #4503](https://github.com/github/copilot-cli/issues/4503)

### 6. 内存看门狗在 23% 上下文占用时强制压缩，循环至 OOM
**#4506 | 状态：开放 | 评论 0**
严重稳定性缺陷：进程内存过高触发内存压力看门狗，强制压缩对话——但当时上下文仅使用 **23%**（400k 窗口），压缩后仅回收 **0.003%** token，随后继续循环压缩直至 OOM。长会话用户可能遭遇极端内存崩溃，需紧急排查。
🔗 [github/copilot-cli Issue #4506](https://github.com/github/copilot-cli/issues/4506)

### 7. 希望支持滚动当前会话历史
**#4313 | 状态：开放 | 评论 5**
用户希望用鼠标滚轮或 PageUp/PageDown 滚动当前会话历史，目前不支持，长对话回溯只能靠翻页键或搜索。交互效率问题，社区讨论积极。
🔗 [github/copilot-cli Issue #4313](https://github.com/github/copilot-cli/issues/4313)

### 8. MCP 结构化响应中的 BigInt 导致任务中断
**#4211 | 状态：开放 | 评论 4 | 👍 2**
当 MCP server 在 `structuredContent` 中返回大整数时，CLI 无法序列化并汇报 `TypeError: Do not know how to serialize a BigInt`，所有进行中的任务被中止。影响依赖大数字（如时间戳、ID）的 MCP 工具。
🔗 [github/copilot-cli Issue #4211](https://github.com/github/copilot-cli/issues/4211)

### 9. account.getQuota 返回错误 resetDate
**#4504 | 状态：开放 | 评论 0**
JSON-RPC `account.getQuota` 响应的 `resetDate` 返回的是**请求时间戳**，而非真实的配额重置时间。直接影响依赖配额统计的自动化工具，属于 API 数据正确性缺陷。
🔗 [github/copilot-cli Issue #4504](https://github.com/github/copilot-cli/issues/4504)

### 10. --no-alt-screen 被静默移除，且无替代方案
**#4509 | 状态：开放 | 评论 0 | 👍 1**
用户发现原本可用的 `--no-alt-screen` 选项被移除，没有任何弃用说明或替代方案。alt-screen 全屏模式自 3 月起已有多起投诉（#1799、#2334），移除逃生通道会加剧用户不满。
🔗 [github/copilot-cli Issue #4509](https://github.com/github/copilot-cli/issues/4509)

---

## 重要 PR 进展

### 唯一 PR：#4510 试图删除 README 中的 Copilot CLI 文档
**#4510 | 状态：开放 | 创建/更新：2026-08-17**
该 PR 删除了 README 中关于 Copilot CLI 的安装说明与使用文档。作者为 `prioritizedprotection086`，无相关描述和评论。这是过去 24 小时唯一的 PR，**建议社区与维护者重点关注，核实是否为恶意/误操作**——若无正当理由，应关闭。
🔗 [github/copilot-cli PR #4510](https://github.com/github/copilot-cli/pull/4510)

---

## 功能需求趋势

### 1. MCP 生态治理成为绝对主线
围绕 MCP 的 Issue 数量最多，且方向分散：**认证兼容**（#4439、#4480）、**序列化**（#4211）、**策略隔离**（#4512）、**数据冗余**（#4515）、**生命周期清理**（#4461）、**缓存 key 设计**（#4513）。这表明 MCP 已进入"功能可用→生产可用"的打磨阶段。

### 2. 会话生命周期与稳定性
长会话场景下问题集中爆发：内存看门狗循环压缩（#4506）、恢复后连接 ID 失效（#4505）、远程会话无法本地恢复（#4514）、Docker MCP 容器残留（#4461）。社区对"数天级长会话"的需求真实存在，需系统性解决状态管理与资源回收。

### 3. 终端交互标准化与可访问性
键盘快捷键（#1481）、历史滚动（#4313）、alt-screen 回归（#4509）、主题切换异常（#4485）、会话选择器对比度（#4455）——用户对终端 UI 的细节体验要求越来越高，且多次反馈未解决，交互层需要一次集中治理。

### 4. 模型与代理灵活性
新模型支持（#4390）、自定义 agent 的 model 配置失效（#2950）、自动模型选择的推理级别冲突（#4459）、子代理工具继承误报（#4457）——模型生态正在扩大，但配置传递链路还不稳定。

### 5. 非交互模式配置一致性
`interactive` 与 `non-interactive` 模式行为分叉：`enabledPlugins`（#4507）、`contextTier`（#4275）在两种模式下表现不一致，影响 ACP/CI 集成场景。

---

## 开发者关注点

- **1.0.79 版本质量存疑**：至少两个独立 Issue 指向 MCP OAuth 认证回归，建议需要接远程 MCP 的用户暂缓升级。
- **长会话稳定性是核心痛点**：内存压缩循环、连接 ID 失效、远程恢复失败——高频出现，亟需维护者优先修复。
- **配置行为在交互/非交互模式下不一致**：插件开关、上下文层级会话选项在 `copilot -p` 下失效，自动化用户受影响明显。
- **高度关注终端 UI 交互细节**：alt-screen 移除、SHIFT+ENTER 换行、历史滚动等高频投诉已持续数月，用户希望得到正面回应而非静默更改。
- **企业功能落地不足**：组织模型目录、配额 API 数据正确性、SDK 认证检查等企业相关功能存在明显缺陷，影响组织级采用信心。

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报
**日期：2026-08-18**

## 今日速览

过去 24 小时，Kimi Code CLI 仓库没有新版本发布，也没有 Issue 更新，仅 1 条 PR（#864）在 8 月 17 日被标记为 CLOSED。该 PR 新增 `--starting-prompt`（`-s`）标志，让开发者可以在启动 CLI 时直接传入提示词，而无需先进入交互界面再手工输入。这项功能从 2 月提交到 8 月关闭，历时半年，反映出社区对脚本化/非交互式调用的持续需求。

## 版本发布

无新版本发布。仓库在过去 24 小时内没有 release 输出。

## 社区热点 Issues

> 说明：过去 24 小时内仓库没有新增或更新的 Issue。以下列出与今日 PR #864 直接相关、值得关注的 Issue 作为背景补充。

1. **Issue #887 —— `--starting-prompt` 功能的直接需求源（已关闭）**  
   PR #864 的标题明确说明会 "closes #887"，对应的需求正是新增 `--starting-prompt` 标志。这意味着用户曾通过 Issue 明确提出：需要在 CLI 启动时传入自定义提示词，而非进入交互界面后再输入。该 Issue 随 PR 关闭而关闭，功能已进入交付通道。  
   🔗 https://github.com/MoonshotAI/kimi-cli/issues/887

2. **Issue #785 —— 同一话题的早期讨论参考**  
   该 Issue 评论区的一条讨论（comment-3837789973）被 PR #864 引用，虽为 "tangentially-related"（间接相关），但很可能是 PR 设计思路的来源之一。这说明社区更早就在探索类似场景（例如通过 shell 脚本调用 Kimi CLI），用户群对自动化工作流有明确的诉求。  
   🔗 https://github.com/MoonshotAI/kimi-cli/issues/785

> 受数据源限制（过去 24 小时无 Issue 更新），其余 8 个「热点 Issue」槽位本期暂缺，建议关注后续日报。

## 重要 PR 进展

1. **PR #864 —— feat: `--starting-prompt` 标志，实现「无需退出即可发起提示」[CLOSED]**  
   - **作者**：stebbins  
   - **创建**：2026-02-02 | **最近更新**：2026-08-17 | **状态**：CLOSED  
   - **功能内容**：新增 `--starting-prompt`（短标志 `-s`）参数，使用户可以在命令行中直接指定起始提示词，无需先启动 CLI 再手动输入。该设计对自动化脚本、CI/CD 管道等场景有直接价值。  
   - **关联 Issue**：关闭 #887；引用 #785 上的一段讨论。  
   - **社区反应**：从 2 月到 8 月的长周期评审说明该功能经过了充分的讨论与打磨。评论数据未完整记录，但多个关联 Issue 的出现表明社区对此需求有真实而具体的用例。  
   - **点评**：这是过去 24 小时内唯一更新的 PR。CLOSED 状态往往意味着即将合并或已合入主干。如果落地，`kimi --starting-prompt "你的提示词"` 这类命令将显著提升 CLI 的可编程性和自动化接入能力。  
   🔗 https://github.com/MoonshotAI/kimi-cli/pull/864

> 其余 9 个 PR 槽位因数据源限制暂缺。

## 功能需求趋势

- **非交互式/参数化调用**：`--starting-prompt` 表明，Kimi CLI 用户希望将其嵌入脚本和自动化流程中，而不仅限于交互式终端。参数化调用正成为 AI CLI 工具的核心能力要求。
- **会话保持与上下文管理**：「prompt without exit」暗示开发者希望在保持 CLI 进程不中断的前提下注入新提示词，指向更精细的会话控制诉求。
- **Unix 风格 CLI 设计**：短标志 `-s` 的命名方式说明用户期待 Kimi CLI 遵循主流命令行工具的习惯，支持高度组合化的调用方式。

## 开发者关注点

- **需求落地周期较长**：一个 PR 从 2 月到 8 月才走到 CLOSED，提示维护团队当前的处理节奏偏慢。急需某项功能的开发者可能需要提前规划，或自行基于源码构建临时方案。
- **好的 Issue 是功能落地的催化剂**：#887、#785 这类包含具体使用场景的 Issue，能有效帮助维护者理解需求，并推动 PR 引用与实现。
- **期待合并后的生态扩展**：一旦 `--starting-prompt` 正式可用，预计社区将涌现更多基于 Kimi CLI 的脚本工具、IDE 任务配置，甚至轻量级 AI 网关封装。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报 2026-08-18

## 今日速览

过去 24 小时无新版本发布，社区讨论集中在 Windows 平台稳定性、模型网关/端点异常，以及 MCP 工具集成与会话管理体验上。多个 PR 修复了 Azure DeepSeek 适配器、会话摘要回归等关键问题，插件 API 正在向完整化方向演进。

## 社区热点 Issues（10 个）

**1. Windows ARM64 原生 TUI 初始化失败**（#19130，OPEN）
- 在 Windows 11 ARM64 上，原生 ARM64 二进制可运行非交互命令，但 TUI 初始化报错 `bun:ffi dlopen TinyCC error`。
- 获得 12 个赞与 18 条评论，反映 ARM64 原生支持是 Windows 用户的核心诉求。
- 链接：https://github.com/anomalyco/opencode/issues/19130

**2. [2.0] 端点错误：Legacy 推理端点报 410 Gone**（#43105，CLOSED）
- 使用 `https://opencode.ai/inference/v1` 时所有 CLI 均报 `status 410 · non-retryable Gone: Legacy inference endpoint retired`，但在 opencode2 beta 中正常，多位用户确认同样问题。
- 链接：https://github.com/anomalyco/opencode/issues/43105

**3. 功能需求：Plan Mode 自动切换 Build 模式**（#7801，OPEN）
- 用户希望 Plan 模式在提问后自动切换到 Build 模式，减少手动切换步骤。
- 获得 32 个赞，是近期呼声最高的功能之一。
- 链接：https://github.com/anomalyco/opencode/issues/7801

**4. Big Pickle 模型提前停止响应**（#22861，CLOSED）
- 用户要求 Big Pickle 描述实现方案时，模型在固定位置提前停止；再次请求依然在同一位置截断，影响长文本生成连续性。
- 链接：https://github.com/anomalyco/opencode/issues/22861

**5. ChatGPT OAuth 拒绝 EU 工作区使用 GPT-5.6 模型**（#40243，CLOSED）
- EU 数据驻留工作区通过 OAuth 认证后，GPT-5.6 模型无法调用，但官方 Codex CLI 可以。直接影响企业级 EU 合规用户的模型可用性。
- 链接：https://github.com/anomalyco/opencode/issues/40243

**6. MCP 工具已连接但未暴露给 Agent**（#33027，OPEN）
- MCP 服务器 `pdfrag` 成功连接并通过 `tools/list` 暴露 6 个工具，但这些工具未出现在 Agent 可用列表中，说明 MCP 集成深度仍需加强。
- 链接：https://github.com/anomalyco/opencode/issues/33027

**7. 功能需求：已归档会话的取消归档/恢复**（#24153，OPEN）
- 归档会话目前是单向操作，用户希望支持还原，改善长会话管理效率，获得 11 个赞。
- 链接：https://github.com/anomalyco/opencode/issues/24153

**8. Windows 路径引用与外部目录权限不生效**（#36681，OPEN）
- Windows 下配置 `external_directory` 权限无效，且缺少官方文档说明 Windows 路径写法。
- 链接：https://github.com/anomalyco/opencode/issues/36681

**9. 除特定免费模型外全部报 Forbidden 错误**（#43054，OPEN）
- 除 `hy3-free` 与 `deepseek flash free` 外，其他模型请求均被拒绝，报错体为 `Forbidden: {"model":"big-pickle"}`，疑似网关默认模型配置异常。
- 链接：https://github.com/anomalyco/opencode/issues/43054

**10. Go Gateway 模型列表与实际部署不匹配**（#42962，CLOSED）
- `GET /zen/go/v1/models` 列出的部分模型在 POST chat/completions 时返回 503/400，list ≠ deployment。
- 链接：https://github.com/anomalyco/opencode/issues/42962

## 重要 PR 进展（10 个）

**1. refactor(app): 使用共享服务器数据层**（#43017，OPEN）
- 将应用消费方迁移到共享服务器数据层，移除重复的 app 同步、session reducers 和遗留缓存，是一次重要的架构层整理。
- 链接：https://github.com/anomalyco/opencode/pull/43017

**2. fix(provider): 选择 Azure DeepSeek 适配器**（#43135，OPEN，Closes #43106）
- 为 Azure 上的 DeepSeek-V4 部署选择专用 `deepseek()` AI SDK 适配器，替代通用 Azure chat/responses 适配器，以支持 `reasoningEffort` 等参数。
- 链接：https://github.com/anomalyco/opencode/pull/43135

**3. feat(ai): 支持 Vertex request labels**（#43129，CLOSED）
- 为 Vertex Gemini provider 暴露计费标签，并添加到请求体中，不影响标准 Gemini API 路由。
- 链接：https://github.com/anomalyco/opencode/pull/43129

**4. feat(plugin): 添加 session request hook**（#37549，CLOSED）
- 新增 `ctx.session.hook("request", ...)` API，允许插件在请求身份验证和签名前修改模型 headers 与 JSON body，同时覆盖 HTTP 与 WebSocket。
- 链接：https://github.com/anomalyco/opencode/pull/37549

**5. fix(opencode): 恢复会话差异摘要**（#37542，CLOSED）
- 修复 #30127 引入的回归，恢复 session-level diff 摘要能力（Closes #30877 / #32852 / #17797）。
- 链接：https://github.com/anomalyco/opencode/pull/37542

**6. fix(tui): 保留系统终端调色板颜色**（#37537，CLOSED）
- 从检测到的终端调色板直接生成 V2 系统主题，保留字面 ANSI 色相，避免 TUI 颜色被合成为更暗颜色。
- 链接：https://github.com/anomalyco/opencode/pull/37537

**7. fix(opencode): 清理 Bedrock 文档名称**（#37535，CLOSED，Fixes #37191）
- Bedrock 拒绝包含非法字符的文档名。该 PR 在发送前清理 MCP 二进制附件生成的合成文件名。
- 链接：https://github.com/anomalyco/opencode/pull/37535

**8. fix(core): 恢复外部目录默认值**（#37530，CLOSED）
- 默认允许外部访问技能（skill）和引用（reference）目录，同时保留精确拒绝与托管 shell 输出的可读性。
- 链接：https://github.com/anomalyco/opencode/pull/37530

**9. fix(core): 目录加载前刷新 console 认证**（#37517，CLOSED）
- 在冷启动 V2 时先刷新 Console 凭据，再加载目录，避免发送过期令牌到遗留 Zen 服务。
- 链接：https://github.com/anomalyco/opencode/pull/37517

**10. fix: 不为 `session list` 启动完整实例**（#37477，CLOSED，Closes #37435）
- `session list` 之前会加载完整实例来查询数据库，该 PR 消除此不必要的性能开销。
- 链接：https://github.com/anomalyco/opencode/pull/37477

## 功能需求趋势

- **Windows 平台体验优化**：包含 ARM64 原生 TUI、路径语法、Cmdlet 权限、npm 全局安装崩溃、Grep 工具兼容性等，是当日反馈最集中的方向。
- **模型网关/端点稳定性**：包括 Legacy 端点 410、上游请求失败、模型列表与实际部署不一致、OAuth 与模型可用性冲突等。
- **MCP 工具链完善**：MCP 工具连接后未暴露给 Agent 的问题，说明 MCP 集成仍停留在协议层，尚未完全打通 Agent 工具调用链路。
- **会话管理增强**：归档会话恢复、压缩（Summarization）稳定性、分叉会话的推理状态清理等。
- **Agent 自主性**：Plan/Build 模式自动切换、速率限制自动暂停/恢复、会话循环命令等。
- **插件生态扩展**：会话请求钩子、Web/桌面 UI 插件表面等，插件 API 正在向 TUI 之外的全界面覆盖演进。

## 开发者关注点

- **Windows 是重灾区**：从原生 ARM64、路径解析、权限配置到 npm 安装，Windows 用户遇到的问题种类多、复现率高，需官方系统性地修复与补充文档。
- **端点/网关错误 410/403/503 频发**：官方推理端点的下线与网关模型“名不副实”，直接影响用户生产可用性。
- **模型适配器选择逻辑**：Azure DeepSeek 适配器缺失、OpenAI OAuth 对 EU 工作区的拒绝，表明多云模型兼容性仍需打磨。
- **会话数据安全**：`opencode run --continue` 会把 prompt 注入到另一个正在活跃使用的会话中，社区对会话隔离与并发安全表达了明确担忧。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi 社区动态日报 — 2026-08-18

## 今日速览

本周社区讨论焦点集中在**上下文自动压缩（auto-compaction）触发不可靠**的问题：在 agentic 长会话中，上下文已越过 100% 阈值仍未触发压缩，直至 API 以 400 错误拒绝请求。与此同时，一批围绕**模型/供应商兼容性**（Anthropic refusal 回退、OpenRouter 缓存、Qwen/GLM 目录对齐）和 **TUI 渲染稳定性**的 PR 密集合并，表明项目正在同时消化长期累积的技术债与新一轮模型生态变化。

---

## 社区热点 Issues（10 条）

### 1. 自动压缩在上下文超过 100% 后依然不触发，直到 provider 报错
**#6879** · [open] · 评论 18 · 👍 17
> 在 gpt-5.6-sol 的一个 agentic turn 运行超过 2 小时后，上下文页脚越过压缩阈值继续增长；直到 API 在 373k tokens 处拒绝请求，压缩才被触发。需要在每个 agent 步骤后检查上下文水位。

**关注点**：这是当前最严重、社区共鸣最高的可靠性问题。压缩机制是长会话的生命线，当前实现存在滞后风险，直接影响所有重度用户。
🔗 https://github.com/earendil-works/pi/issues/6879

### 2. Linux 下配置目录不符合 XDG Base Directory Spec
**#534** · [closed] · 评论 15 · 👍 39
> 在 Linux 上，`pi` 的配置文件夹直接位于 `$HOME` 下，而现代工具应遵循 XDG 规范（`~/.config/pi`）。

**关注点**：虽已关闭，但 39 个 👍 是本期最高。说明 Linux 用户对系统集成规范有很强的诉求，关闭原因可能是已通过其他方式处理或明确不采纳。
🔗 https://github.com/earendil-works/pi/issues/534

### 3. 提示编辑器在超大文本下移动光标性能极差
**#8029** · [open] · 评论 9
> 在约 7000 行的提示文本框中，单次按 ↑ 键需要 1650ms，性能随文本量线性恶化。

**关注点**：这是日常编辑体验中的明显退化。对粘贴大段代码或日志后需要微调的用户来说，卡顿感会非常突出。
🔗 https://github.com/earendil-works/pi/issues/8029

### 4. 支持在 prompt 命令中传递视频/音频内容
**#3200** · [open] · 评论 8 · 👍 5
> 现有的 `prompt` RPC 仅支持 `images`，建议扩展为同时支持 video/audio，以便 Gemma 4、GPT-4o 等多模态模型使用。

**关注点**：多模态能力是模型侧的趋势，Pi 的工具层需要同步跟上。该 issue 已开放 4 个月仍在讨论，说明实现路径可能受限于 RPC 协议设计和传输体积。
🔗 https://github.com/earendil-works/pi/issues/3200

### 5. openai-responses 缺少 Anthropic 缓存格式支持 — 实测成本增加 2.5 倍
**#7995** · [open] · 评论 4
> 由 OpenRouter 的 870 次基准测试发现：`openai-responses` 实现中没有任何 Anthropic 风格的 prompt-caching 支持，`cache_control` 从未出现，导致经 OpenRouter responses 访问 Claude 模型时成本显著增加。

**关注点**：缓存控制直接影响用户的 API 账单，2.5 倍的成本差异足以让团队重估 provider 选择。属于投入产出比很高的修复方向。
🔗 https://github.com/earendil-works/pi/issues/7995

### 6. Edit 工具渲染大 diff 时导致 TUI 崩溃
**#8036** · [open] · 评论 4
> 对一个包含超长物理行的 HTML 文件执行编辑后，diff 结果达到约 14.5 MB，TUI 立即崩溃；恢复会话时再次崩溃。

**关注点**：大 diff 场景在真实项目中并不罕见（如生成器产物、打包文件），崩溃会中断工作流并破坏会话恢复信心。与 #8028（V8 字符串长度限制）属于同一类渲染健壮性问题。
🔗 https://github.com/earendil-works/pi/issues/8036

### 7. 自定义消息插入工具批次中，破坏 tool_calls→tool 相邻性（DeepSeek 400）
**#8166** · [open] · 评论 3
> 扩展在工具调用过程中调用 `pi.sendMessage(..., { triggerTurn: false })` 注入自定义消息，导致后续轮次 API 报错 "Messages with role 'tool' must be a response to a preceding message with 'tool_calls'"（DeepSeek 400），会话此后每个轮次都失败。

**关注点**：这是一个扩展机制与 API 约束之间的边界情况。`triggerTurn: false` 是官方支持的参数，说明核心消息队列需要保证 tool_calls 与 tool 结果的严格相邻性。
🔗 https://github.com/earendil-works/pi/issues/8166

### 8. detectInstallMethod 将非 pnpm 安装误判为 pnpm
**#7756** · [open] · 评论 3
> 只要路径包含 `/pnpm/` 就会被标记为 pnpm 安装；共享 `PNPM_HOME` bin 目录的安装方式被错误识别，然后被 `isManagedByGlobalPackageManager` 拒绝。

**关注点**：影响通过 `$PNPM_HOME/global-nub` 这类环境安装的用户，错误信息 "not managed by ..." 有误导性。属于安装/升级体验中的 edge case，但定位成本低。
🔗 https://github.com/earendil-works/pi/issues/7756

### 9. openai-completions: reasoning_details 只能回放加密条目，无法回放签名文本
**#7994** · [open] · 评论 3
> 由 OpenRouter 基准测试发现：`openai-completions` 实现只解析 `reasoning.encrypted` 条目，OpenRouter 返回的已签名 `reasoning.text` / `reasoning.summary` 被丢弃，导致后续 assistant replay 缺失 reasoning_details。

**关注点**：与 #7995 同属 OpenRouter API surface 兼容性问题。推理内容回放影响多轮调用的上下文完整性和可审计性，对依赖 reasoning 的流程是有害的。
🔗 https://github.com/earendil-works/pi/issues/7994

### 10. Anthropic refusal 需要服务端回退支持
**#8017** · [closed] · 评论 3
> 当 Anthropic 的 classifier 认为 Pi 正在执行非法操作时，压缩（compaction）可能失败。建议实现 Anthropic 官方文档中的 `allowed_fallback_models` 机制。

**关注点**：压缩本是为了维持会话，但如果压缩请求本身被拒绝，整个会话将卡死。这个 issue 催生了 PR #8258（下方详述），属于快速闭环的案例。
🔗 https://github.com/earendil-works/pi/issues/8017

---

## 重要 PR 进展（10 条）

### 1. 修复 Anthropic refusal 错误并支持 fallback 模型
**#8258** · [closed] · 作者: cristinaponcela
> 在 `claude-fable-5` 上复现了压缩失败：Anthropic 返回 `stop_reason: "refusal"`。PR 将 `allowed_fallback_models` 元数据写入生成的模型注册表，并在 refusal 场景自动切换 fallback。

**意义**：直接解决 #8017，是压缩可靠性链条上的重要一环。让会话在模型拒绝时能够自动降级而不是卡死。
🔗 https://github.com/earendil-works/pi/pull/8258

### 2. 修复嵌套 Markdown 技能无法加载的问题
**#8255** · [closed] · 作者: cristinaponcela
> `~/.agents/skills/third-party/child-skill.md` 这类嵌套 Markdown 技能此前被静默跳过，因为只递归发现 `SKILL.md` 目录而忽略了根处理路径之外的 Markdown 文件。

**意义**：解决 #6479。技能发现机制更完整，第三方技能目录（如按组织/来源分组）现在可以正常工作。
🔗 https://github.com/earendil-works/pi/pull/8255

### 3. 实验性追加压缩（append compaction）
**#8120** · [closed] · 作者: vegarsti
> 当 `PI_EXPERIMENTAL=1` 时启用追加压缩：复用当前系统提示、工具、转换上下文和路由会话，使压缩前缀能够直接命中 provider 的 prompt 缓存；独立压缩仍是默认模式。

**意义**：对 #6879 等压缩问题的积极探索。如果复用缓存有效，可以显著降低压缩后的 token 成本与模型响应延迟。
🔗 https://github.com/earendil-works/pi/pull/8120

### 4. 对齐 Qwen Token Plan 模型目录
**#8240** · [closed] · 作者: sunner
> `qwen-token-plan` 和 `qwen-token-plan-cn` 现在共享同一文本模型 allowlist，统一暴露 8 个当前模型（含 `deepseek-v4-pro-0813`、`deepseek-v4-flash-0731`）；`-individual` 保持独立的 7 模型目录。

**意义**：模型目录碎片化影响用户选择，统一后国内/国际版行为一致，同时将已废弃模型清理出列表。
🔗 https://github.com/earendil-works/pi/pull/8240

### 5. 修复 TUI 在长会话中内容变化导致的全屏闪烁
**#8253** · [closed] · 作者: wlynxg
> 差分渲染只处理可见视口，因此视口上方任何变化（如长会话中工具结果更新）都会清屏并重印每一行。现在只清除屏幕上的受保护区域。

**意义**：10k+ 行会话中的工具输出更新不再整屏闪烁，滚动位置与内容对比的稳定性大幅提升，直接改善重度用户的视觉体验。
🔗 https://github.com/earendil-works/pi/pull/8253

### 6. openai-completions：补全 reasoning_details 回放（含签名文本）
**#8246** · [open] · 作者: cristinaponcela
> 在合成 OpenRouter openai-completions 流上复现：来自 `delta.reasoning_details` 的已签名 `reasoning.text` / `reasoning.summary` 此前被丢弃。现在保留消息级别的 `reasoning_details`，使 assistant replay 可以完整带回推理内容。

**意义**：对应 #7994，补上了推理内容在 multi-turn 调用中的一个漏洞，对依赖推理过程审计的工作流有帮助。
🔗 https://github.com/earendil-works/pi/pull/8246

### 7. Bedrock 响应现在包含 Smithy 原始响应头
**#8243** · [closed] · 作者: cristinaponcela
> 通过在 Bedrock deserialize middleware 中捕获原始 Smithy HTTP 响应，现在 `onResponse` / `after_provider_response` 能拿到真实的网关响应头（如 `x-bifrost-provider`），此前只有 `$metadata` 派生头。

**意义**：网关类投递的头信息对调试和可观测性有用，例如识别实际路由到哪个上游模型。修复 #8234。
🔗 https://github.com/earendil-works/pi/pull/8243

### 8. 扩展系统新增 `session_compact_failed` 事件
**#8241** · [closed] · 作者: cristinaponcela
> 压缩失败此前只产生内部 `compaction_end errors`，扩展只能看到 `session_before_compact` 而无法得知失败原因。新增扩展可见的 `session_compact_failed` 事件，携带原有失败负载。

**意义**：让第三方扩展可以感知压缩失败并做出反应（如告警、切换配置），是对 #6879 一类问题的可观测性补强。
🔗 https://github.com/earendil-works/pi/pull/8241

### 9. 示例扩展改用 `agent_settled` 而非 `agent_end`
**#8242** · [closed] · 作者: cristinaponcela
> `agent_end` 在每次底层 run 结束后立即触发，而此时 Pi 可能还在重试、压缩或排队后续 follow-up。将 `notify.ts` 等示例切换到 `agent_settled`，该事件只在 Pi 确定不再继续时触发。

**意义**：修正扩展事件语义，避免通知类扩展在会话真正空闲前误报 "Ready for input"。修复 #7350。
🔗 https://github.com/earendil-works/pi/pull/8242

### 10. 泛化 openai-completions 的 thinking token budget 字段名
**#8275** · [closed] · 作者: bnsd55
> 在 #7638 的基础上，增加 `compat.thinkingTokenBudgetField` 配置项，可指定 `thinking_token_budget`（vLLM）、`thinking_budget`（Qwen/SGLang）或 `thinking_budget_tokens`（llama.cpp），并补充到 compat 文档。

**意义**：同一套截断逻辑现在可以适配三种主流推理服务，属于本地推理用户可感知的兼容性改进。
🔗 https://github.com/earendil-works/pi/pull/8275

---

## 功能需求趋势

从近 24 小时的 Issues/PR 中可以提炼出以下社区关注方向：

1. **上下文管理可靠性优先** — 自动压缩触发策略（#6879）、本地 provider 在工具轮次间依然可能溢出（#8229）、追加压缩实验（#8120）。压缩不是新功能，但是当前最痛的点。
2. **多模态支持外延** — 提示命令中的视频/音频（#3200）、剪贴板图片粘贴（#2144）、GLM 视觉模型入目录（#8220）。模型生态已多模态化，工具链在跟上。
3. **供应商/模型目录即时性** — 小米废弃模型清理（#8187）、Qwen Token Plan 对齐（#8194）、GLM-4.6V 入编（#8220）、Bedrock 工具 schema 校验（#8279）。厂商模型迭代节奏快，内置目录需要更快同步。
4. **缓存控制全面化** — OpenRouter 的 Anthropic 缓存格式缺失（#7995）、`allowEmptySignature` 兼容（#7996）。控制成本成为企业用户刚需。
5. **扩展生态的事件语义标准化** — `agent_settled` 替代 `agent_end`（#7350）、压缩失败通知扩展（#8175）、自定义消息与工具批次顺序（#8166）。插件作者对事件契约的精确性要求越来越高。
6. **TUI 性能与渲染鲁棒性** — 大文本编辑器（#8029）、大 diff 崩溃（#8036）、V8 字符串上限（#8028）、视口上方变化整屏闪烁（#8253）。都是“内容变大后不要崩、不要卡、不要闪”的朴素需求。

---

## 开发者关注点

| 痛点/需求 | 相关 Issue/PR | 说明 |
|---|---|---|
| **上下文超限导致会话报废** | #6879, #8229 | 压缩触发时机滞后，API 拒绝错误出现后轻则重试重则丢会话。开发者建议“每 agent 步骤后检查”而非依赖 provider 错误。 |
| **大型内容场景下的 TUI 稳定性** | #8029, #8036, #8028 | 几千行编辑器输入、十几 MB diff 结果都能击穿 TUI。渲染层（尤其是 fullRender）需要做防御性截断或分块。 |
| **成本控制依赖缓存机制** | #7995, #7996 | OpenRouter 路径下 Claude 因缓存缺失成本涨 2.5 倍，开发者对 `cache_control` 透传有极高敏感度。 |
| **扩展编程契约不够严格** | #8166, #7350, #8241 | `triggerTurn: false` 插入消息会破坏 tool_calls 相邻性；`agent_end` 误报空闲。扩展开发者需要更明确的事件语义与消息队列保证。 |
| **安装与模型目录“脏数据”** | #7756, #8187, #8194 | pnpm 误判、已废弃模型仍在列表中出现并导致选择后失败。小型但影响信任感的细节问题。 |
| **Linux 桌面集成** | #534, #8276 | 配置目录不符合 XDG、SELinux 下容器需要 `:Z` 标记。Linux 用户的系统集成期望在提升。 |
| **推理细节的往返一致性** | #7994, #8246 | 多轮调用中 `reasoning_details` 若被丢弃，会影响依赖推理输出的下游消费者（如审计、评估）。 |

---

*本日报由 GitHub 数据自动整理生成，基于 2026-08-17 的 50 条 Issues 与 34 条 PRs。*

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报 — 2026-08-18

## 今日速览

昨日发布正式版 **v0.21.13**，核心亮点为 Web Shell 编辑器支持拖放/粘贴附带图片的命名文本文件，以及对话分支（fork）能力。社区方面，Windows 平台 Ctrl+V 粘贴回归、`/compress-fast` 后丢失上下文等稳定性问题引发热议；同时微信（Weixin）渠道集成、服务端资源治理和 `qwen serve` 文件权限可配置化成为 PR 与 Issue 的活跃方向。

---

## 版本发布

### v0.21.13
昨日发布的正式版本，主要更新包括：

- **Web Shell composer**：支持将文本文件作为命名附件拖放、粘贴到编辑器中，与图片附件共存（[#9180](https://github.com/QwenLM/qwen-code/pull/9180)）
- **对话分支（Fork）**：用户可从任意一条 Assistant 回复开始分支对话
- 另有 `v0.21.11-nightly` 系列 nightly 构建和 DSW EAS benchmark 全量验证（SWE-bench Verified 500 + Terminal-Bench 2.0 89 项，全部通过）

---

## 社区热点 Issues（10 条）

### 1. Ctrl+V 粘贴在 Windows CLI 完全失效（回归）— P1
**Issue #9061** | 评论 6 | 👍 0  
自 0.21.x 某个版本起，Windows 下 Qwen Code CLI 的 Ctrl+V 粘贴完全无响应；降级到 0.21.0 即恢复正常。用户确认系统剪贴板正常、PowerShell 内粘贴正常，属 CLI 自身回归。这是当前 Windows 用户最紧迫的阻断问题之一。  
🔗 [github.com/QwenLM/qwen-code/issues/9061](https://github.com/QwenLM/qwen-code/issues/9061)

### 2. Qwen Autofix 事件风暴浪费 runner 容量 — P1
**Issue #9296** | 评论 4 | 👍 0  
对 2026-08-16 约 500 次 autofix 运行的分析发现：59% 被取消（294/500），根因包括**已关闭/已合并 PR 仍触发 autofix**（P0）、重复寻址导致同一 PR 并发跑多个任务等。直接影响 CI 成本和响应速度。  
🔗 [github.com/QwenLM/qwen-code/issues/9296](https://github.com/QwenLM/qwen-code/issues/9296)

### 3. 微信渠道 64 位 message_id 精度丢失 — P1
**Issue #9307** | 评论 4 | 👍 0  
Weixin `getupdates` 响应中的 `message_id` 可能超过 `Number.MAX_SAFE_INTEGER`，当前 `Response.json()` 按 number 解析导致精度舍入，后续转字符串后消息 ID 错误。影响消息去重与追踪。  
🔗 [github.com/QwenLM/qwen-code/issues/9307](https://github.com/QwenLM/qwen-code/issues/9307)

### 4. `/compress-fast` 后对话上下文丢失
**Issue #9320** | 评论 5 | 👍 0  
用户实测：102k 上下文经 `/compress-fast` 压至 87k token 后，启动新 llama-server 恢复会话，模型似乎丢失了部分早期上下文。压缩算法的忠实性问题引发多位用户跟帖确认。  
🔗 [github.com/QwenLM/qwen-code/issues/9320](https://github.com/QwenLM/qwen-code/issues/9320)

### 5. 取消 prompt 后内容未恢复到输入框
**Issue #8316** | 评论 9 | 👍 0  
用户发送 prompt 后按 Ctrl+C 取消，已输入的内容不会恢复到输入框，必须重新打字。长时间未修复，社区呼声较高。  
🔗 [github.com/QwenLM/qwen-code/issues/8316](https://github.com/QwenLM/qwen-code/issues/8316)

### 6. 消息被重复投递（Qwen Desktop Code）
**Issue #9324** | 评论 7 | 👍 0  
用户反馈 Qwen Desktop Code（Qwen 3.8 Max）经常在 thinking 中表示“收到了多条相同消息”，并中断当前任务。可能与 session 同步或消息确认机制有关，影响对话连贯性。  
🔗 [github.com/QwenLM/qwen-code/issues/9324](https://github.com/QwenLM/qwen-code/issues/9324)

### 7. `/compress` 后状态栏上下文百分比不刷新
**Issue #6806** | 评论 6 | 👍 0  
执行 `/compress` 或 `/compress-fast` 后，底部状态栏的 context usage 百分比仍停留在压缩前数值，需等下一个模型请求完成后才更新。属于 UI 反馈滞后问题，welcome-pr 标记。  
🔗 [github.com/QwenLM/qwen-code/issues/6806](https://github.com/QwenLM/qwen-code/issues/6806)

### 8. VP 模式内容底部对齐缺失
**Issue #9300** | 评论 6 | 👍 0  
`useTerminalBuffer: true`（默认）下，VP 模式最后一条消息与输入框之间存在大片空白，未实现底部对齐。新引入的渲染回归，影响阅读连贯性。  
🔗 [github.com/QwenLM/qwen-code/issues/9300](https://github.com/QwenLM/qwen-code/issues/9300)

### 9. 测试合约固定缺口（PR #9096 复审后续）
**Issue #9194** | 评论 10 | 👍 0  
自动化复审在第 5-6 轮发现一类测试硬化问题：部分测试未充分固定所声明的行为合约——生产代码发生突变后测试套件仍可能通过。属于非阻断但需修复的测试鲁棒性缺口。  
🔗 [github.com/QwenLM/qwen-code/issues/9194](https://github.com/QwenLM/qwen-code/issues/9194)

### 10. 多工作区 daemon 资源使用无上限
**Issue #8051** | 评论 9 | 👍 0  
`qwen serve` 多工作区 daemon 目前仅按数量限制工作区和会话，但**未限制请求体、WebSocket 缓冲等实际字节占用**，可能被单个大请求拖垮。社区建议按字节维度补充资源约束。  
🔗 [github.com/QwenLM/qwen-code/issues/8051](https://github.com/QwenLM/qwen-code/issues/8051)

---

## 重要 PR 进展（10 个）

### 1. Web Shell：拖放/粘贴文件作为命名附件
**PR #9180** （昨日合入）  
为 Web Shell composer 增加拖放、粘贴纯文本文件作为命名附件的能力（与图片附件并存），已随 v0.21.13 发布。  
🔗 [github.com/QwenLM/qwen-code/pull/9180](https://github.com/QwenLM/qwen-code/pull/9180)

### 2. 导出 HTML 查看器增加全局展开/折叠控制
**PR #9367**  
为 `/export` HTML 模板的 `ChatViewer` 增加可选的“Expand all / Collapse all”全局工具栏，支持一键切换所有可折叠区域。  
🔗 [github.com/QwenLM/qwen-code/pull/9367](https://github.com/QwenLM/qwen-code/pull/9367)

### 3. `qwen serve` 新文件模式可配置化
**PR #9364**  
新增 `QWEN_SERVE_NEW_FILE_MODE` 环境变量，支持 `owner`（默认，0600）和 `system`（遵循 umask）两种策略，回应 Issue #9250。  
🔗 [github.com/QwenLM/qwen-code/pull/9364](https://github.com/QwenLM/qwen-code/pull/9364)

### 4. 定时任务支持绑定已有会话
**PR #9361**  
`POST /scheduled-tasks` 和 workspace 限定端点新增可选 `sessionId`，传入时复用现有会话而非新建独立任务会话，落地 Issue #8906 的请求。  
🔗 [github.com/QwenLM/qwen-code/pull/9361](https://github.com/QwenLM/qwen-code/pull/9361)

### 5. 微信 typing 指示器保活
**PR #9358**  
修复微信渠道单次 `TYPING` 在长时间任务中过期的问题——每 4 秒重发 `TYPING`，任务结束时发送 `CANCEL`。回应 Issue #9353。  
🔗 [github.com/QwenLM/qwen-code/pull/9358](https://github.com/QwenLM/qwen-code/pull/9358)

### 6. 过滤模型无法消费的图片媒体
**PR #9295**  
对模型端点无法消费的 MIME 类型（如 `image/heic`、`image/tiff`）或无法解码的图片，不再原样以 data URI 转发，避免请求校验失败。修复 Issue #9291。  
🔗 [github.com/QwenLM/qwen-code/pull/9295](https://github.com/QwenLM/qwen-code/pull/9295)

### 7. 限制 Web Shell daemon transcript 保留量，防止 OOM
**PR #9303**  
加载会话后立即释放原始 replay 快照；replay 重建与实时增长共用同一 block 上限。针对渲染进程 OOM 崩溃的修复。  
🔗 [github.com/QwenLM/qwen-code/pull/9303](https://github.com/QwenLM/qwen-code/pull/9303)

### 8. 简化 CI 自检自愈逻辑
**PR #9327**  
将 #9220 引入的 checkout 自愈从约 60 行路径守护精简回约 15 行的“擦除并重试”核心逻辑，降低维护复杂度。  
🔗 [github.com/QwenLM/qwen-code/pull/9327](https://github.com/QwenLM/qwen-code/pull/9327)

### 9. review body 受 GitHub 65,536 字符限制约束
**PR #9247**  
`compose-review` 在返回前测量 review body 长度，超限时按优先级裁剪——先丢弃中文翻译（英文在上，不损失内容），再降级冗余段落。  
🔗 [github.com/QwenLM/qwen-code/pull/9247](https://github.com/QwenLM/qwen-code/pull/9247)

### 10. verifier 探针迁移至私有 scratch worktree
**PR #9221**  
将 review Step 4 verifier 的写操作（写探针、执行、恢复）从共享 review worktree 隔离到私有工作树，避免污染其他 agent 的 `working_dir`。  
🔗 [github.com/QwenLM/qwen-code/pull/9221](https://github.com/QwenLM/qwen-code/pull/9221)

---

## 功能需求趋势

从当前活跃 Issues 与 PR 中可提炼出五大社区关注方向：

1. **会话与上下文管理（最高热度）**：`/compress` 压缩质量与状态刷新、上下文丢失、取消后恢复输入、会话 fork 等话题密集，说明长会话场景下用户对上下文可靠性和可恢复性的要求极高。

2. **Web Shell 与 UI 统一化**：多个 Issue/PR 指向将聊天面板、导出 HTML、跨宿主 transcript 统一到 Web Shell 组件（如 #5883、#9354），并增强导出内容的可读性（thinking 展示、展开/折叠）。

3. **服务端资源治理（daemon/serve）**：#8051、#8091、#9250 等持续追踪 `qwen serve` 的字节级资源上限、文件权限可配置性，以及相关功能拆分为可评审的小 PR 落地。

4. **微信（Weixin）渠道集成完善**：已出现 64 位消息 ID、typing 保活、文件发送、长任务稳定性等多个 Issue/PR，说明该渠道正在快速补齐生产可用性。

5. **自动化评审/Autofix 效率与稳定性**：#9296 事件风暴、#9194 测试固定缺口以及多个 autofix/takeover 相关 PR，表明 Qwen Code 自身的 CI/自动审阅管线正在经历“吃自己的狗粮”式硬化。

---

## 开发者关注点

| 痛点/高频需求 | 相关链接 |
|---|---|
| **Windows CLI 粘贴回归**：Ctrl+V 在 0.21.x 失效，属阻断级 Bug，影响日常使用 | [#9061](https://github.com/QwenLM/qwen-code/issues/9061) |
| **上下文压缩后丢失或状态不刷新**：用户对压缩算法的“无损性”存疑 | [#9320](https://github.com/QwenLM/qwen-code/issues/9320)、[#6806](https://github.com/QwenLM/qwen-code/issues/6806)、[#9309](https://github.com/QwenLM/qwen-code/issues/9309) |
| **取消 prompt 后内容不恢复**：输入内容丢失，必须重打，影响长 prompt 编辑体验 | [#8316](https://github.com/QwenLM/qwen-code/issues/8316) |
| **新版终端交互方式改变**：v0.19 之后 UI 重写导致“无法复制选中文本”等回退 | [#9315](https://github.com/QwenLM/qwen-code/issues/9315) |
| **agent-team 会话稳定性**：错误/不完整的 teammate tab 会导致整个交互会话崩溃 | [#9290](https://github.com/QwenLM/qwen-code/issues/9290)、[#9283](https://github.com/QwenLM/qwen-code/issues/9283) |
| **daemon 资源不受限**：按数量限制不够，需要按字节/内存上限保护 | [#8051](https://github.com/QwenLM/qwen-code/issues/8051)、[#8091](https://github.com/QwenLM/qwen-code/issues/8091) |

---

*本日报由 QwenLM/qwen-code GitHub 仓库数据自动生成，覆盖 2026-08-17 至 2026-08-18 的社区动态。*

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报 — 2026-08-18

> 数据来源：github.com/Hmbown/DeepSeek-TUI（仓库数据指向 Hmbown/CodeWhale）

## 今日速览

昨日（8月17日）是社区高度活跃的一天：**v0.9.9 正式发版**（release PR #5476 已合并），核心修复包括 shell 工具在磁盘/句柄耗尽时不再卡死会话（#5465）、DeepSeek V4 分时段定价（#5470）、web 工具噪声结果压缩（#5474）。与此同时，**文档中文本地化史诗级 Issue（#5482）** 与 **agent 工具 32 字段 schema 简化（#5324）** 成为社区讨论焦点，测试可靠性（#5056、#5403）和配置迁移问题（#2369）持续发酵。

## 社区热点 Issues

1. **[#5424] v0.9.7: Codewhale TUI 崩溃（CLOSED）** — 用户报告等待输出约一分钟后程序自动退出，`codewhale --continue` 可复现。由 Hixac 报告，7 条评论，已关闭说明已定位或修复。
   https://github.com/Hmbown/CodeWhale/issues/5424

2. **[#2369] CodeWhale 配置路径跨平台碎片化 + 静默迁移 Bug（OPEN）** — Windows/Cygwin 下配置文件可能解析到不同的 home 目录，遗留迁移还会产生静默错误。8 条评论，是当前评论数最高的 open issue，长期未解决值得关注。
   https://github.com/Hmbown/CodeWhale/issues/2369

3. **[#5056] 测试可靠性：flaky verifier 后台测试、/workspace 敏感 fixtures（OPEN）** — 维护者 Hmbown 自报：`run_verifiers_background_*` 在整套并行测试下仍不稳定，还有 12 个未分类的 `#[ignore]` 测试。8 条评论，社区持续关注测试基建质量。
   https://github.com/Hmbown/CodeWhale/issues/5056

4. **[#5324] agent 工具 32 字段 JSON schema 简化（CLOSED）** — 模型面向的 `agent` 工具 schema 有 32 个属性且零必填字段，同时承载 8 个动作，运行时解析器还接受一堆别名，容易让模型出错。已关闭，改动可能已并入 v0.9.9。
   https://github.com/Hmbown/CodeWhale/issues/5324

5. **[#1425] 300 万字小说分析：子 agent 超时会话卡死（OPEN）** — 中文用户报告：AI 将小说切片为 10 份并启动 10 个子 agent，但 `agent_wait` 等待超时导致会话中断。7 条评论，涉及大文本处理、子 agent 调度与超时策略，中文社区反馈强烈。
   https://github.com/Hmbown/CodeWhale/issues/1425

6. **[#5123] agent spawn 旋钮过多：labeled builder 只读且自我 BLOCKED（OPEN）** — 0.9.4 实测（dogfood）：delegate builder 被打上 `builder` / `gates-shell-writer` 标签，但实际工具合约是只读的，导致无法执行分配的门禁任务。7 条评论，暴露权限模型与标签不一致的问题。
   https://github.com/Hmbown/CodeWhale/issues/5123

7. **[#1829] SSH 连接失败 exit code 255（OPEN）** — 中文用户报告：Windows 10 下 TUI 内置 shell 无法 SSH 到腾讯云服务器（TCP 22 出站疑似被沙箱阻断），本地终端正常。6 条评论，涉及沙箱网络策略。
   https://github.com/Hmbown/CodeWhale/issues/1829

8. **[#1651] VS Code 崩溃：YOLO Agent 运行测试脚本时退出（OPEN）** — 在 VS Code 集成终端中使用 DeepSeek v4-pro/v4-flash 模型时，YOLO Agent 后台执行测试脚本导致 VS Code 意外退出。6 条评论，IDE 集成稳定性问题。
   https://github.com/Hmbown/CodeWhale/issues/1651

9. **[#5374] agent 书写文本乱码（CLOSED）** — macOS 用户反馈 agent 书写时文本全部乱码（附截图），5 条评论，已关闭。
   https://github.com/Hmbown/CodeWhale/issues/5374

10. **[#5482] EPIC：文档全面中文本地化（OPEN）** — 新建 Issue，指出 CodeWhale 有大量中文用户但 `docs/` 仍是英文为主，机器翻译有误且源文档已过时。1 条评论但方向重要，配合 #5290、#5337 等形成 i18n 主线。
    https://github.com/Hmbown/CodeWhale/issues/5482

11. **[#5403] main 分支双平台 CI 全红（OPEN）** — 4 次完成的 CI 运行在 macOS 和 Windows 上均失败（plugin_e2e_acceptance 和 NSIS provisioning），3 条评论，当前 release 门禁的拦路虎。
    https://github.com/Hmbown/CodeWhale/issues/5403

12. **[#5350] 第三方模型配置简化：预制模板（OPEN）** — 中文用户建议为 OpenCode Zen、Agnes、美团 Sensenova 等第三方兼容服务商内置预制模板，一键填入 Base URL 和模型列表，并增加「测试连接」按钮。4 条评论，代表配置体验类需求。
    https://github.com/Hmbown/CodeWhale/issues/5350

## 重要 PR 进展

1. **[#5476] release: 0.9.9（CLOSED）** — v0.9.9 正式发布，主题为“truth-and-resilience”：shell 工具在磁盘/描述符耗尽时不再卡死会话（#5465），未验证的上下文窗口/输出上限/遥测默认值被如实标注，避免误导。
   https://github.com/Hmbown/CodeWhale/pull/5476

2. **[#5465] exec 流创建失败必须软失败，绝不卡死 shell 工具（CLOSED）** — 修复主人自己的 0.9.9 会话被内存 thrash 搞挂的问题：macOS 主机内存耗尽后，所有 `bash` 调用（含 `echo ok`）都返回 “Failed to create streaming shell output”。核心稳定性修复。
   https://github.com/Hmbown/CodeWhale/pull/5465

3. **[#5470] DeepSeek V4 分时段定价按轮次解析（CLOSED）** — 将 V4-Pro / V4-Flash 的单一费率改为按 UTC 小时的 peak/off-peak 分级定价，按每轮请求的实际时间计算费用，修正成本显示。
   https://github.com/Hmbown/CodeWhale/pull/5470

4. **[#5474] 压缩所有嘈杂 web 工具结果（CLOSED）** — 对 `Web`、`web_search`、`web.run`、`fetch_url` 全部应用现有 noisy-result 软限制，普通工具（如 `read_file`）保留硬限制，大幅节省上下文。
   https://github.com/Hmbown/CodeWhale/pull/5474

5. **[#5475] 修复自有 direct model 大小写解析（CLOSED）** — 修复小写保存的选择器（如 `glm-5.2`）被错误归类为其他提供商的 bare wire id 的问题，保持精确匹配的权威性，仅当唯一所有者匹配时才做大小写折叠回退。社区贡献（h3c-hexin）。
   https://github.com/Hmbown/CodeWhale/pull/5475

6. **[#5473] perf(skills): 保持配置的技能提示词稳定（OPEN）** — 配置的 skills 根目录下的原生技能在模型目录中只显示名称和描述，替换物理路径为 `<configured-skills>`，减少模型暴露的易变信息（h3c-hexin）。
   https://github.com/Hmbown/CodeWhale/pull/5473

7. **[#5491] fix(tui): 执行前持久化审批结果（OPEN）** — 在每个审批请求执行前将会话级日志持久化，无法持久化则拒绝执行并拒绝过期决策，会话恢复时重建已关闭/中断的审批状态。直接对应 #5360。
   https://github.com/Hmbown/CodeWhale/pull/5491

8. **[#5402] 修复实时定价不可验证时的会话成本显示（CLOSED）** — 修复 #5241：实时定价无法验证时（含 `https://api.codewhale.net/session` 返回 503 的路径），成本不再永远显示 `unverified_live_pricing`，改为诚实标注路径。
   https://github.com/Hmbown/CodeWhale/pull/5402

9. **[#5480] 显示并打开 /rc 实时会话链接 + 稳定 device id（CLOSED）** — `/rc` banner 现在会显示、打印并打开实时 web 会话链接；解析可选的 `runner.runUrl` / `runner.computerUrl`；停止每次 `/rc` 都生成新“computer”的行为。
   https://github.com/Hmbown/CodeWhale/pull/5480

10. **[#5486] 紧凑行隐藏会话指标条（OPEN）** — 宽度低于 60 列时，phase strip 会隐藏工作细节/缓存 chip/状态 toast，但会话指标条仍在绘制，59 列下行渲染拥挤。此 PR 让紧凑模式一致地隐藏 SessionMetrics。
    https://github.com/Hmbown/CodeWhale/pull/5486

11. **[#5488] 将 docs shell 移至字典主干（OPEN）** — `app/[locale]/docs/layout.tsx` 中的 5 个字符串仍是 `isZh` 三元表达式，导致 ja/vi/ko 等 8 个部分本地化路由只能看英文。此 PR 将 docs 布局的文案接入 #4934 的字典路径，配合 #5337 的 i18n 主线。
    https://github.com/Hmbown/CodeWhale/pull/5488

12. **[#5489] rustdoc 中包裹裸 URL（OPEN）** — 两个 prose URL 触发 `rustdoc::bare-urls` -D warnings，导致 0.9.9 精确头 CI 的强制 Documentation 任务失败。0.9.9 tag 本身不受影响（构建不跑 rustdoc）。
    https://github.com/Hmbown/CodeWhale/pull/5489

## 功能需求趋势

- **配置与迁移可靠性**（#2369、#5350）：跨平台配置路径不一致、第三方模型配置繁琐是两大高频痛点。社区希望内置预制模板、一键测试连接、自动修复迁移，降低新手配置成本。
- **子代理 / 多代理稳定性**（#1425、#5123）：大文本分片处理、子 agent 超时、权限标签与实际工具合约不一致等问题表明，子代理调度和授权模型亟需打磨。
- **上下文与成本管理**（#5239、#5241、#5055）：1M 上下文窗口下仍按 128K 触发压缩引发质疑；实时定价不可验证导致成本显示失效；DeepSeek Pro 的 effort 映射需集中管理——社区对透明、可控的上下文/成本策略需求强烈。
- **本地化 / i18n**（#5290、#5337、#5482、#5350）：除中英文外，日/越/韩/西等 8 个部分本地化路由的交互控件失效；文档全量中文化被提上议程；i18n 字典主干的推进成为 web 端持续主线。
- **插件系统与生态**（#5311、#5102、#4170）：社区希望 Codewhale 具备 Kimi 级别的插件市场、MCP capability 元数据以及原生的截图/图像查看工具，构建完整的插件产品体验。
- **测试基建稳定性**（#5056、#5355、#5403）：并行加载 flake、跨平台 CI 全红、12 个未分类 ignore 测试——测试可靠性正在消耗维护者注意力，是发版门禁的最大瓶颈。

## 开发者关注点

- **稳定性与崩溃修复**：v0.9.7 的 TUI 崩溃（#5424）、VS Code 在 YOLO Agent 运行测试时退出（#1651）、大文本处理中的会话卡死（#1425）、shell 在磁盘耗尽后完全不可用（#5465）——这些是开发者反馈最多的痛点类别。
- **配置路径与跨平台问题**：Windows/Cygwin 下的配置路径碎片化（#2369）和 SSH 出站被沙箱阻断（#1829）表明跨平台兼容性仍需加强，尤其对 Windows 用户。
- **模型与成本透明度**：实时定价 503 导致成本不可见（#5241）、V4 分时段定价硬编码无日期标注（#5055）、1M 上下文却 128K 就压缩（#5239）——开发者希望模型行为可预期、成本可核算。
- **CI/CD 门禁健康**：main 分支双平台全红（#5403）、rustdoc 链接检查失败阻塞 CI（#5489），说明 release 工程的自动化质量直接影响社区交付信心。
- **中文社区活跃度上升**：多个中文 Issue（#1425、#1829、#5350）和文档本地化 epic（#5482）的出现，表明中文用户群体正在快速增长，官方需重视本地化体验。

---
*本日报由 AI 自动生成，数据截止 2026-08-18 00:00 UTC。*

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

过去24小时无活动。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*