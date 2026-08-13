# AI CLI 工具社区动态日报 2026-08-14

> 生成时间: 2026-08-13 23:34 UTC | 覆盖工具: 10 个

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

# AI CLI 工具社区横向对比分析报告

**报告日期**：2026-08-14  
**数据来源**：GitHub 各仓库 Issues / PR / Releases（统计窗口 24 小时）  
**覆盖工具**：Claude Code、OpenAI Codex、Gemini CLI、GitHub Copilot CLI、Kimi Code CLI、OpenCode、Pi、Qwen Code、DeepSeek TUI（CodeWhale）、Grok Build

---

## 1. 生态全景

AI CLI 工具已从"单轮代码补全"演进为 **多代理协作平台**，Subagent、MCP、跨端会话同步成为头部工具的标配能力。各厂商以周级甚至日级频率发布新版本（Claude Code 单日 2 个维护版、Codex 单日 3 个 alpha），但快速迭代也伴随稳定性投诉集中爆发。社区共识正从"功能丰富度"转向"**可信任的执行力**"——挂起、误报成功、失控生成、缓存浪费等问题被高频吐槽。生态呈现"全家桶 + 开源补位"格局：Anthropic/OpenAI/Google/GitHub 依托模型与 IDE 生态构筑壁垒，OpenCode、Pi、CodeWhale 等开源项目则在多 Provider 聚合、终端体验、本地化场景中差异化突围。

---

## 2. 各工具活跃度对比

> 注：以下 Issue / PR 数量为各日报精选活跃项，非全量数据；Release 数为统计窗口内发布数量。

| 工具 | Release 数 | 精选活跃 Issues | 精选 PR | 最热议题（热度信号） | 活跃度观感 |
|------|-----------|----------------|---------|---------------------|-----------|
| **Claude Code** | 2（v2.1.231 / v2.1.232） | 10 | 2 | CLI ↔ 桌面会话同步 #28791（123 👍 / 34 评论） | 社区体量大、需求层次高，PR 合入节奏平缓 |
| **OpenAI Codex** | 3（0.148.0-alpha.11/12/13） | 10 | 10 | Windows 扩展加载失败 #37458（53 评论） | 高频迭代、机器人驱动 PR 占比高（14/20） |
| **Gemini CLI** | 1（v0.56.0-nightly） | 10 | 10 | Subagent 误报 GOAL 成功 #22323（12 评论） | 评估体系投入大，Subagent 可靠性问题集中 |
| **GitHub Copilot CLI** | 1（v1.0.80-0） | 10 | 1 | 自定义 Agent reasoning effort #2904（20 👍） | 讨论持续但维护节奏偏慢，PR 显著偏少 |
| **Kimi Code CLI** | 0 | 3（全部活跃） | 0 | 跨会话 Memory System #1283（38 评论） | 热度集中在单个长期需求，整体活跃度低 |
| **OpenCode** | 1（v1.18.18） | 10 | 10 | 保留旧版布局 #37012（41 👍 / 37 评论） | 开源社区活跃，V1/V2 过渡引发大量回归反馈 |
| **Pi** | 0（未提及） | 10 | 10 | auto-compaction 失效 #6879（19 评论 / 17 👍） | 精品型社区，Issue-PR 闭环效率极高（当天修复） |
| **Qwen Code** | 2（v0.21.11 / v0.21.12-preview.1） | 10 | 10 | 多代理协调 RFC #8718（9 评论） | 快速迭代，多代理与 Web Shell 双线推进 |
| **DeepSeek TUI（CodeWhale）** | 1（v0.9.7） | 10 | 10 | 多 Agent 大任务稳定性 #1425 等（各 5~11 评论） | 架构转型期（品牌迁移 + crate 拆解），长尾问题多 |
| **Grok Build** | 0 | 0 | 0 | — | 统计窗口内无活动 |

---

## 3. 共同关注的功能方向

### 3.1 多代理 / 子代理可靠性（7 个工具共同痛点）

| 工具 | 具体诉求 |
|------|---------|
| **Gemini CLI** | MAX_TURNS 触顶后误报 GOAL 成功（#22323）；generalist 代理无限挂起（#21409）；subagent 未经许可被调用（#22093） |
| **OpenAI Codex** | spawn_agent 拒绝新模型 gpt-5.6-luna（#34700）；子代理日志膨胀至 145 GiB（#31198） |
| **DeepSeek TUI** | 300 万字文本切 10 个子 Agent 并行，超时中断（#1425）；agent schema 过于复杂致模型频繁出错（#5324） |
| **Claude Code** | Subagent forking 默认开启（v2.1.232），降低子代理上下文割裂 |
| **Copilot CLI** | Task 工具 multiplier guard 静默降级子代理模型（#3565） |
| **Qwen Code** | 后台 Agent 恢复与 activeWork 追踪（#8586）；/coordinate 多代理工作流（v0.21.11） |
| **Pi** | Codex 后端需处理 `end_turn: false` 语义（#7689） |

**共性判断**：子代理机制已大规模铺开，但**状态透出、可信完成判定、资源回收**仍是全行业短板。

### 3.2 MCP 生态稳定性（7 个工具）

| 工具 | 具体诉求 |
|------|---------|
| **Copilot CLI** | Atlassian OAuth 版本回归（#4480）；CI 中 MCP Registry 策略返回 403（#4346）；服务器名大小写碰撞误判（#4478） |
| **Claude Code** | MCP OAuth 重定向 URI 修复（v2.1.231）；连接器 30 秒超时（#86502） |
| **OpenAI Codex** | MCP 服务器级 OAuth 回调端口（#38448）；本地 MCP HTTPS 的 rustls 回退（#38436） |
| **Gemini CLI** | MCP 配置损坏不应被当作空配置（#28787） |
| **Qwen Code** | MCP 2026 客户端首片（#8992）；桌面端 MCP OAuth 流程被外链问题阻塞（#9108） |
| **DeepSeek TUI** | 无分页信息时省略 `nextCursor`，避免被严格客户端拒绝（#5336） |
| **OpenCode** | 并行 spawn MCP server 的 "Connection closed" 竞态（#42431） |

**共性判断**：MCP 正从"能连上"走向"**企业级可用**"，认证、超时、配置容错、并发竞态是四座大山。

### 3.3 Windows 平台体验（6 个工具）

| 工具 | 具体诉求 |
|------|---------|
| **OpenAI Codex** | 扩展 "couldn't load its resources"（#37458，53 评论）；MSIX 版 PowerShell 致沙箱启动失败（#35871）；插件缓存哈希路径持久化（#25285） |
| **Claude Code** | Dispatch 永久卡死无法重置 QR（#67682）；内嵌浏览器被广告页拖垮并终止所有会话（#86234） |
| **Qwen Code** | 安装时 Get-FileHash 失败（#7118）；Ctrl+V 粘贴回归（#9061）；桌面启动弹终端窗口（#9043） |
| **Copilot CLI** | Windows socket 10013 错误（#4463） |
| **OpenCode** | WSL 模式下本地 MCP 命令引用 Linux 可执行文件（#42429） |
| **DeepSeek TUI** | Windows / Cygwin 配置路径碎片化 + 静默迁移失败（#2369） |

**共性判断**：Windows 是第一大桌面开发者系统，但**沙箱、终端、安装器、文件路径**四类适配问题集中拖后腿。

### 3.4 会话 / 上下文 / 记忆管理（8 个工具）

| 工具 | 具体诉求 |
|------|---------|
| **Claude Code** | CLI ↔ 桌面应用会话历史同步（#28791，123 👍 断层第一）；切账户后历史丢失（#48511） |
| **OpenAI Codex** | 子代理日志膨胀至 145 GiB（#31198）；上下文压缩时保留客户端开发者消息（#38445）；分页线程回滚（#38440） |
| **Copilot CLI** | 点击停止导致整个会话被删除（#4477）；权限事件在恢复后重放（#4469） |
| **Gemini CLI** | bugreport 不包含子代理上下文（#21763） |
| **Kimi Code** | 跨会话 Memory System 连续半年高热度（#1283，38 评论） |
| **Pi** | auto-compaction 在 context 超限后不触发（#6879）；大会话恢复刷屏（#8079） |
| **Qwen Code** | 大会话恢复超时保护（#8678）；live-session registry（#8969） |
| **DeepSeek TUI** | 重启后遗忘记忆（#2492） |

**共性判断**：会话是用户最重要的工作资产。**持久化、跨端互通、恢复性能、记忆写入透明度**是下一代 CLI 的分水岭。

### 3.5 模型行为可控性与透明度（6 个工具）

| 工具 | 具体诉求 |
|------|---------|
| **Copilot CLI** | 自定义 Agent 支持 reasoning effort（#2904，20 👍）；`explore` 工具硬编码 gpt-5.4-mini（#3954）；模型数组语法兼容 VS Code（#2133） |
| **Claude Code** | 冗长注释无视停止指令（#65961，110 👍）；覆盖用户明确的代词设定（#52477） |
| **Kimi Code** | 单步生成 88k 乱码 token，无熔断机制（#2597） |
| **OpenCode** | 保留 AI SDK 返回的真实模型 ID（#42433）；修复 xAI 过高 reasoning effort（v1.18.18） |
| **Qwen Code** | Gemini 2.5 在 Vertex AI 上因 `thinking_level` 参数 400（#9019） |
| **Pi** | Anthropic 服务端拒答需 fallback 机制（#8017） |

**共性判断**：开发者要求"**最明确的配置优先，隐式逻辑不得覆盖**"，同时要求异常生成可熔断、模型路由可审计。

### 3.6 成本与资源效率（4 个工具）

| 工具 | 具体诉求 |
|------|---------|
| **Claude Code** | 并行工具调用后提示缓存完全重建，74% cache_creation 浪费（#63930） |
| **Pi** | compaction 依赖 Provider 溢出才触发，超长任务被迫多轮重试（#6879） |
| **Copilot CLI** | Task 工具静默降级导致模型选择不可预期，变相浪费（#3565） |
| **Kimi Code** | 失控生成 88k token 消耗大量计算资源（#2597） |

**共性判断**：成本焦虑已从"API 价格"转向"**工具链层面的浪费**"——缓存失效、重试风暴、失控生成是三个可优化的真金白银区域。

### 3.7 安全与供应链（6 个工具）

| 工具 | 具体诉求 |
|------|---------|
| **OpenCode** | 升级脚本 curl\|bash 无完整性校验（#42434）；webfetch SSRF（#42435）；上下文修剪静默丢指令（#42437） |
| **Claude Code** | 安全过滤器误伤 USB 枚举、固件加固等合法开发流程（#71871 / #71865 / #71861） |
| **Gemini CLI** | Auto Memory 先发送后脱敏（#26525）；A2A 服务器无认证强制 + checkpoint 路径穿越（#28699） |
| **Qwen Code** | HTTP hooks 四个信任边界漏洞（#8396）；跨工作树 Git 变更防护（#8687） |
| **DeepSeek TUI** | 无隔离测试可读真实 `~/.codewhale` 的信任漏洞（#5359 / #5368） |
| **Pi** | Anthropic 服务端拒绝需安全 fallback（#8017） |

**共性判断**：供应链完整性与模型上下文安全成为新增攻击面，社区对“**默认安全、显式降级**”的呼声增强。

---

## 4. 差异化定位分析

| 工具 | 核心定位 | 技术路线 | 目标用户 | 关键优势 | 当前最大短板 |
|------|---------|---------|---------|---------|-------------|
| **Claude Code** | AI 编码代理事实标准之一 | TypeScript/Node，深度绑定 Claude 模型与 MCP | 中大型团队、Claude 重度用户 | 生态最完善（Subagent forking、提示缓存、会话引用）；社区规模最大 | 模型行为遵从度（注释、代词）；成本随缓存失效上升 |
| **OpenAI Codex** | 多代理 + 安全审查标杆 | Rust，Guardian 安全层，Azure/OpenAI 生态 | 企业级、Security 敏感团队 | 迭代速度（3 alpha/日）、Guardian V2 上下文感知、模型生命周期管理 | Windows 平台表现拖后腿；机器人 PR 多但稳定性口碑受损 |
| **Gemini CLI** | 多模型网关 + 行为评估体系 | TypeScript，可接入 Claude/Gemini，重视 evals | 看重评估与可观测性的工程团队 | 行为评估 EPIC（76 个 eval、6 个模型）、跨模型支持 | Subagent 可信度问题（误报成功、挂起）直接削弱核心体验 |
| **Copilot CLI** | GitHub 生态的 Agent 入口 | TypeScript，Agent 定义与 VS Code Copilot 共享 | GitHub 企业用户、CI/CD 自动化 | 与 Copilot Chat 配置兼容；GitHub 原生集成 | 模型选择硬编码/静默降级；MCP OAuth 故障频发；PR 维护节奏慢 |
| **Kimi Code** | 轻量 Moonshot 生态 CLI | 未披露，Agent 能力插件化 | Kimi API 用户、轻量自动化场景 | Memory 诉求清晰（#1283） | 活跃度低、基础设施（流式/日志）稳定性不足 |
| **OpenCode** | 开源社区的"瑞士军刀" | TypeScript，插件机制 + 多 Provider（含 GitHub Copilot、Kimi） | 开源爱好者、插件生态依赖者、多模型混用者 | 插件生态丰富、V2 性能优化、安全响应快速 | V1/V2 兼容性混乱；桌面端回归频繁；供应链安全报告密集 |
| **Pi** | 终端体验极致主义多后端代理 | TypeScript（单体 repo），支持 Codex/Gemini/Kimi/Anthropic/Bedrock 等多后端 | TUI 重度用户、多模型比较用户 | Issue-PR 闭环效率极高（当天修复）；终端卫生、CJK 细节打磨；创始人参与设计 | 社区规模较小；auto-compaction 等核心机制尚不稳定 |
| **Qwen Code** | 阿里生态的多代理工作台 | TypeScript/Node，Agent Plugins + /coordinate + Web Shell | 通义模型用户、多代理自动化、Web 桌面环境 | 多代理原生工作流、MCP 2026 先行、daemon 健康架构 | Windows 安装/输入体验缺陷；Vertex 等第三方云认证兼容性 |
| **DeepSeek TUI（CodeWhale）** | 中文用户优先的多模态 TUI | Rust TUI，强调本地/多模态/大文本处理 | 中文开发者、长文本分析、本地模型用户 | 品牌与架构焕新（v0.9.7）、本地 DS4 集成、画中画等特色功能 | 多 Agent 稳定性、中文输入法、记忆持久化三大历史债待还 |

**一句话概括**：  
Anthropic/OpenAI/Google/GitHub 在"模型 + 生态"上角力，开源派（OpenCode/Pi/CodeWhale）则在"终端体验 + 多 Provider + 本地化"上蚕食缝隙市场，Kimi 和 Grok 仍处于边缘试探期。

---

## 5. 社区热度与成熟度

### 第一梯队：高活跃、强需求（每日产生高赞 Issue 与密集 PR）

| 工具 | 证据 |
|------|------|
| **Claude Code** | 123 👍 的头部需求断层领先；10 个精选 Issue 覆盖功能、成本、安全、文档全维度，社区成熟度最高 |
| **OpenAI Codex** | 24h 3 个 alpha + 10 条 PR，自动化合并流水线成熟（20 PR 中 14 条来自机器人）；但 Windows 问题 53 评论 / 26 次崩溃显示质量口碑在承压 |
| **Gemini CLI** | 10 条 PR（5 合并 + 5 审查），EPIC 级评估体系建设中；Subagent 高优先级 bug 密集，处于"快速补课"阶段 |
| **OpenCode** | 41 👍 的布局争议 + 10 条 PR + 多份安全报告，社区参与度高，且对回归和安全问题响应迅速 |

### 第二梯队：稳定迭代、局部高热

| 工具 | 证据 |
|------|------|
| **Qwen Code** | 双版本发布、10 条 PR、多代理立项完整，处于功能扩展快车道；但 Windows 回归与认证问题分走注意力 |
| **Pi** | 19 评论的热点 + 10 条 PR，且修复当日闭环（#8079/#8080 → #8082），社区"小而精"、执行效率高 |
| **Copilot CLI** | 长期 Issue 热度延续（#2904 4 个月 20 👍），但 PR 仅 1 条，迭代效率明显不及前两者 |
| **DeepSeek TUI** | 40+ 活跃 Issue，10 条 PR，品牌与架构转型期活跃度高；问题分散（中文输入、多代理、i18n），说明尚未收敛核心体验 |

### 第三梯队：低活跃 / 观望期

| 工具 | 证据 |
|------|------|
| **Kimi Code CLI** | 仅 3 个活跃 Issue、0 PR、0 Release，社区讨论集中但维护响应不足 |
| **Grok Build** | 统计窗口内完全无活动，或处于早期孵化阶段 |

---

## 6. 值得关注的趋势信号

**1. "子代理信任危机"是全行业头号工程挑战**  
7 个工具同时出现误报成功、挂起、资源失控等问题。参考信号：Gemini CLI #22323 的"MAX_TURNS 误报 GOAL"会污染自动化决策链。  
→ **对开发者的价值**：在关键流程中不要无条件信任子代理的 `success` 状态，需设置独立验收条件；为选用工具设定"子代理可观测性"评估标准。

**2. MCP 进入"基础设施期"，协议细节决定成败**  
OAuth 端口、回调 URI、`nextCursor` 序列化、配置损坏兜底……这些问题不解决，MCP 生态无法承载企业级工作流。  
→ **对开发者的价值**：评估 AI CLI 时，关注其 MCP 客户端对"认证失败、并发竞态、配置异常"的处理成熟度，而不只是"支持多少个 MCP server"。

**3. Windows 与跨平台适配是差异化竞争窗口**  
Codex、Claude Code、Qwen Code 在 Windows 上集中翻车（沙箱、安装器、剪贴板、内嵌浏览器），而 OpenCode 已在 WSL MCP 方向修复。  
→ **对开发者的价值**：Windows / WSL 用户应优先选择对平台问题有快速修复记录的工具（如 OpenCode PR #42429）；多平台团队需建立各自的回归测试矩阵。

**4. 会话持久化与记忆机制成为"第二存储层"**  
跨端同步（Claude Code #28791）、记忆系统（Kimi #1283）、日志膨胀（Codex #31198）、重启失忆（CodeWhale #2492）本质上都是"会话即数据"的工程化问题。  
→ **对开发者的价值**：开始将"会话历史"视为需要备份、迁移、审计的数据资产；留意工具是否支持导出/同步/压缩保留策略。

**5. 模型行为可控性成为选型硬指标**  
Copilot CLI 的硬编码模型、Claude Code 的不遵从指令、Kimi 的失控生成，指向同一诉求：**用户配置优先级必须高于模型/工具隐式逻辑**。  
→ **对开发者的价值**：在自动化脚本中，基于"行为确定性不足"预设防护——设置输出 token 上限、超时熔断、模型选择显式断言，而非依赖工具默认策略。

**6. 供应链安全与上下文安全同步升温**  
OpenCode 的三连报（curl\|bash 无校验、SSRF、上下文修剪丢指令）与 Gemini 的 Auto Memory 先发送后脱敏，揭示新攻击面。  
→ **对开发者的价值**：审查 AI CLI 的安装/升级链路是否签名；对涉及私有代码/文档的工具，优先选择本地可审计、可禁用远程记忆的选项。

**7. 资本与生态正在分野："全家桶" vs "开源组合"**  
Anthropic/OpenAI/Google/GitHub 依托模型、IDE、CI 集成构筑闭环；OpenCode、Pi、CodeWhale 则靠多 Provider、插件、终端体验争取用户。  
→ **对开发者的价值**：技术选型本质是选择生态绑定——追求深度集成选全家桶，追求灵活性与透明度选开源组合；建议在"模型可替换性"上留有余地。

---

*本报告基于 2026-08-14 各工具社区动态日报自动聚合分析生成，数据口径以各日报说明为准。*

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区热点报告（数据截止 2026-08-14）

> 数据来源：github.com/anthropics/skills 官方仓库 Issues / PRs

---

## 一、热门 Skills 排行

> 以下按社区讨论热度排序，当前状态均为 **Open**（尚未合并）。

### 1. skill-creator 评估链路修复（PR #1298）
- **功能**：修复 `run_eval.py` 对所有 Skill 描述一律报 0% recall 的致命缺陷；同时修复 Windows 流读取、触发检测与并行 worker 问题。
- **讨论热点**：该问题已被 10+ 用户独立复现（关联 Issue #556 / #1169），社区共识是「描述优化循环一直在对噪声做优化」，属基础设施级 bug，影响面极大。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/1298)

### 2. document-typography 文档排版技能（PR #514）
- **功能**：对 AI 生成文档做排版质量控制——孤行文字（1-6 词溢出到下一行）、段首孤儿标题（标题滞留页尾）、编号错位等。
- **讨论热点**：直击「所有 Claude 生成文档的通病」，用户很少主动要求排版质量，因此需要 Skill 兜底，属于普适性刚需。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/514)

### 3. ODT 文档技能（PR #486）
- **功能**：OpenDocument 格式（.odt/.ods）的创建、模板填充、读取及 ODT→HTML 转换。
- **讨论热点**：补齐了 office 文档生态中 Docx/PDF 之外的格式空白，与 LibreOffice/ISO 标准格式诉求相关。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/486)

### 4. frontend-design 技能重构（PR #210）
- **功能**：重写 frontend-design 技能，提升指令清晰度、可执行性与内部一致性，确保每条指令能在单次对话内被 Claude 实际执行。
- **讨论热点**：聚焦「技能应是操作手册而非教学文档」的元问题——与 Issue #202 对 skill-creator 的批评同源。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/210)

### 5. self-audit 自我审计技能（PR #1367，v1.3.0）
- **功能**：交付前先做机械性文件核查（验证所有声称的输出文件是否存在），再按损害严重度进行四维推理审计；宣称通用、跨技术栈。
- **讨论热点**：AI 输出可靠性治理的落地尝试，与提案 Issue #1385（Reasoning Quality Gate Pipeline）互相呼应，讨论持续活跃。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/1367)

### 6. testing-patterns 测试模式技能（PR #723）
- **功能**：覆盖完整测试栈——Testing Trophy 模型、单元测试（AAA 模式/命名/纯函数/边界）、React 组件测试（Testing Library）等。
- **讨论热点**：社区对「测试生成」类技能的稳定需求，内容体系完整度高。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/723)

### 7. ServiceNow 平台技能（PR #568）
- **功能**：覆盖 ITSM、ITOM、ITAM/SAM、FSM、HRSD/CSM、SPM、漏洞响应、安全事件响应与 IntegrationHub 的宽幅平台助手。
- **讨论热点**：企业级平台集成方向关注度最高的 PR，**2026-08-12 仍有更新**，是目前最活跃的待合并 PR 之一。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/568)

### 8. 技能质量/安全分析器（PR #83）
- **功能**：新增两个元技能——`skill-quality-analyzer`（按结构文档 20%、示例、资源等五维评估技能质量）和 `skill-security-analyzer`（安全审计）。
- **讨论热点**：直接回应社区对 Skill 质量参差与安全边界的担忧（尤其关联高热度 Issue #492 的安全信任问题）。
- **状态**：Open · [GitHub](https://github.com/anthropics/skills/pull/83)

> 另有 **pyxel 复古游戏开发技能**（PR #525，Pyxel 作者亲自提交）、**PDF/DOCX/Skill-creator 多项修复**（PR #538/#541/#539）、**plan-file-hygiene 规划文件治理**（PR #1479）等也获得持续关注。

---

## 二、社区需求趋势（来自 Issues）

| 需求方向 | 代表 Issue | 热度 |
|---|---|---|
| **安全与信任边界** | [#492：社区技能冒用 `anthropic/` 命名空间，构成信任边界滥用](https://github.com/anthropics/skills/issues/492) | 43 评论，全仓最高 |
| **组织级技能共享/分发** | [#228：支持 org-wide skill sharing，建立共享技能库或直达链接](https://github.com/anthropics/skills/issues/228) | 16 评论，👍 8 |
| **技能创建工具的可靠性** | [#556：run_eval.py 0% 触发率](https://github.com/anthropics/skills/issues/556) / [#202：skill-creator 应改为操作型指令](https://github.com/anthropics/skills/issues/202) | 12+8 评论 |
| **AI 智能体安全治理** | [#412：agent-governance——策略执行/威胁检测/信任评分/审计追踪](https://github.com/anthropics/skills/issues/412) | 已关闭但方向明确 |
| **长期记忆与上下文压缩** | [#1329：compact-memory 符号化记忆，降低长任务上下文消耗](https://github.com/anthropics/skills/issues/1329) | 9 评论 |
| **推理质量门禁** | [#1385：任务前校准 → 对抗评审 → 交付验证三闸流水线](https://github.com/anthropics/skills/issues/1385) | 4 评论 |
| **技能与 MCP 协议互通** | [#16：将 Skills 暴露为 MCP API](https://github.com/anthropics/skills/issues/16) | 4 评论 |
| **平台兼容与稳定性** | [#29：AWS Bedrock 使用支持](https://github.com/anthropics/skills/issues/29) / [#189：插件重复安装致技能冗余](https://github.com/anthropics/skills/issues/189) / [#1487：claude-api 技能一次注入 ~156k token 撑爆上下文](https://github.com/anthropics/skills/issues/1487) | — |

**趋势解读**：社区需求正从「增加更多技能」转向「治理已生态」——安全信任、质量审计、记忆压缩、组织级分发成为新焦点；企业平台集成（ServiceNow、SAP、SharePoint）与测试/文档生成仍是稳定基本面。

---

## 三、高潜力待合并 Skills（近期可能落地）

1. **ServiceNow 平台技能（PR #568）** — 更新至 8/12，活跃度最高，企业服务管理覆盖面广。
   [GitHub](https://github.com/anthropics/skills/pull/568)

2. **skill-creator 评估修复（PR #1298）** — 直接解决 #556/#1169 两个高赞 bug，属基础设施修复，合并优先级应最高；另有 #1099、#1050 同向修复 Windows 兼容。
   [GitHub](https://github.com/anthropics/skills/pull/1298)

3. **self-audit 自我审计（PR #1367）** — 作者另有配套提案 #1385，体系化思路完整，讨论持续发酵。
   [GitHub](https://github.com/anthropics/skills/pull/1367)

4. **skill-quality-analyzer / skill-security-analyzer（PR #83）** — 呼应 #492 安全议题，若合并将填补官方技能质量评估工具空白。
   [GitHub](https://github.com/anthropics/skills/pull/83)

5. **document-typography（PR #514）** — 普适性排版刚需，实现简单、收益直观。
   [GitHub](https://github.com/anthropics/skills/pull/514)

6. **testing-patterns（PR #723）** — 测试方向需求稳定，内容量大、审查周期可能较长。
   [GitHub](https://github.com/anthropics/skills/pull/723)

7. **pyxel 游戏开发（PR #525）** — 生态作者亲自提交，7/15 仍有更新，质量有保障，细分领域标杆。
   [GitHub](https://github.com/anthropics/skills/pull/525)

---

## 四、Skills 生态洞察

**社区当前最集中的诉求是「Skill 生态的工程化治理」——一方面迫切要求官方修复 skill-creator 评估链路等基础设施缺陷，另一方面集中涌现对 AI 输出质量审计、安全防护与治理类 Skills（self-audit、agent-governance、quality/security analyzer）的强需求，同时企业级平台集成（ServiceNow、SAP）与组织级共享分发成为新的增长点。**

---

## Claude Code 社区动态日报
**📅 2026-08-14**

---

### 1. 今日速览

- 发布两个维护版本：**v2.1.232** 正式默认开启 Subagent forking（子代理继承完整对话与提示缓存），**v2.1.231** 修复 MCP OAuth 重定向 URI 不匹配问题。
- 社区最热议题：CLI 与桌面应用**对话历史同步**（#28791）以 123 👍 居首；模型**冗长注释行为**（#65961，110 👍）与**提示缓存被反复重建**（#63930）引发对效率与成本的集中吐槽。
- PR 侧相对平静，24 小时内仅 2 条，以文档修正与 CI 安全加固为主。

---

### 2. 版本发布

#### v2.1.232
- **Subagent forking 默认开启**：`subagent_type: "fork"` 的子代理现在自动继承完整对话上下文与提示缓存；交互式会话中，非 teammate 的 agent 生成默认转入后台执行。
- **会话快速引用**：输入 `@` 可按名称提及另一个 Claude 会话。

#### v2.1.231
- **MCP OAuth 修复**：解决使用预注册 OAuth 客户端（如 Slack）的服务器登录失败问题，根因为重定向 URI 不匹配。

---

### 3. 社区热点 Issues

#### 🔥 #28791【功能请求】CLI 与桌面应用同步会话历史
- **123 👍 / 34 评论**，当前热度最高需求。用户希望在 CLI 与 Claude Code 桌面应用之间无缝共享会话历史，解决两端数据隔离导致的上下文断裂。
- 🔗 https://github.com/anthropics/claude-code/issues/28791

#### 🔥 #65961【Bug】模型默认生成冗长注释，无视停止指令
- **110 👍 / 11 评论**。开发者普遍反映 Claude 即使被明确指示，仍持续输出注释掉的冗长代码，指令遵循度堪忧。
- 🔗 https://github.com/anthropics/claude-code/issues/65961

#### 💸 #63930【Bug】并行工具调用后提示缓存被完全重建，74% 写入被浪费
- 10 评论。Opus 4.7→4.8 切换后，多并行工具调用导致 `cache_read` 坍缩至 system+tools 地板值，整段对话反复从零缓存，**token 成本显著上升**。
- 🔗 https://github.com/anthropics/claude-code/issues/63930

#### ⚖️ #52477【模型行为】Claude 覆盖用户记忆中的明确代词设定
- 12 评论。用户已在记忆明确代词偏好的情况下，模型仍默认采用男性代词，涉及模型对用户显式配置的尊重问题。
- 🔗 https://github.com/anthropics/claude-code/issues/52477

#### 🪟 #67682【Bug】Windows 11 Dispatch 永久卡死，无法重置 QR 配对
- 5 评论。Dispatch 状态卡在异常态，手机端持续显示“Can't reach your desktop / Asleep”，需手动介入才能恢复，影响 Cowork 远程使用。
- 🔗 https://github.com/anthropics/claude-code/issues/67682

#### 💥 #86234【Bug】Windows 桌面内嵌浏览器打开重广告页导致整个应用崩溃
- 3 评论（最新 issue）。广告堆栈（prebid、GAM 等）即可拖垮主进程，并**连带终止所有托管中的 Claude Code 会话**，已在两台独立机器复现。
- 🔗 https://github.com/anthropics/claude-code/issues/86234

#### 🔌 #86502【Bug】MCP 连接器 30 秒超时，端点响应其实很快
- 2 评论（最新 issue）。两个自定义 claude.ai 域 MCP 连接器在 CLI 中超时，同一账号在 Claude App 中正常，疑似 CLI 侧连接器处理逻辑缺陷。
- 🔗 https://github.com/anthropics/claude-code/issues/86502

#### 👤 #48511【Bug】桌面应用切换账户后全部会话历史丢失
- 5 👍 / 4 评论。配额用尽切换账号后，历史会话在 Cowork 与本地 Code 模式下均消失，数据持久化设计存在缺口。
- 🔗 https://github.com/anthropics/claude-code/issues/48511

#### 🛡️ #71871 / #71865 / #71861【安全误报】网络安全过滤器误伤嵌入式开发
- 三份联锁报告（各 3 评论）：使用开源工具进行 USB 无人机枚举、固件加固等**合法硬件开发流程**被安全模块阻断，开发者质疑规则粒度过粗。
- 🔗 https://github.com/anthropics/claude-code/issues/71871 ｜ https://github.com/anthropics/claude-code/issues/71865 ｜ https://github.com/anthropics/claude-code/issues/71861

#### 📄 #52601【文档缺陷】Settings 文档路径与实现不一致（批量 stale 问题代表）
- 同一用户 `coygeek` 集中提交的 20+ 文档类 issue 之一，指出 `/config` 偏好被错误描述为存储在 `~/.claude.json`，实际应为 `~/.claude/settings.json`。此类 stale 文档数量庞大，已引起维护者注意并批量关闭。
- 🔗 https://github.com/anthropics/claude-code/issues/52601

---

### 4. 重要 PR 进展

过去 24 小时 PR 列表极短（仅 2 条），反映当日合入节奏平缓：

- **#86537 · Fix duplicated word in CHANGELOG.md**
  修正 CHANGELOG 中 `CLAUDE_BASH_NO_LOGIN` 条目里重复的 "to to" 字样，纯文档校正，无功能变更。
  🔗 https://github.com/anthropics/claude-code/pull/86537

- **#60280 · chore(ci): SHA-pin 剩余 actions/checkout 与 actions/github-script**
  延续 #56784 的 CI 安全加固，将 6 个工作流中第三方 action 从 tag 引用切换为精确 SHA（如 `checkout@v4` → `34e114...`），涉及 `claude-issue-triage`、`backfill-duplicate-comments` 等流程。
  🔗 https://github.com/anthropics/claude-code/pull/60280

---

### 5. 功能需求趋势

从 50 条活跃 Issues 中提炼出社区最关心的五个方向：

| 方向 | 代表 Issue | 热度信号 |
|------|-----------|---------|
| **跨端会话同步** | CLI ↔ 桌面应用历史互通（#28791） | 123 👍，断层第一 |
| **模型行为一致性** | 注释风格控制（#65961）、代词尊重（#52477） | 合计 120+ 👍 |
| **成本与缓存效率** | 提示缓存重建（#63930） | 直击 Opus 4.8 使用成本 |
| **桌面应用稳定性** | Windows 崩溃（#86234）、Dispatch 卡死（#67682）、切号丢历史（#48511） | 多平台连环反馈 |
| **MCP 生态成熟度** | OAuth 修复、连接器超时（#86502） | 与 v2.1.231 修复直接呼应 |

---

### 6. 开发者关注点

- **成本焦虑加剧**：提示缓存反复重建导致 74% 的 `cache_creation` 令牌被浪费，重度并行工具调用场景下费用肉眼可见地上升。
- **指令遵从度不足**：冗长注释等问题表明模型对用户风格的长期记忆遵守不足，开发者期望更强的显式指令控制力。
- **数据持久化痛点**：账户切换即丢失会话历史，CLI/Desktop 间无法共享上下文，跨环境工作流被割裂。
- **安全误报干扰合法工作流**：硬件枚举、固件分析等嵌入式开发场景被安全过滤器误伤，规则需要更好的领域感知能力。
- **文档与实现脱节**：以 `coygeek` 为代表的大量 stale 文档反馈进入集中处理阶段，社区在推动文档质量向实现看齐。
- **Windows 平台体验亟待补强**：桌面应用内嵌浏览器的健壮性、Dispatch 状态机恢复均为 Windows 特有痛点，影响远程协作可靠性。

---

*本日报由数据源自动整理，链接均可直达 GitHub 原始 Issue / PR。*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报

**日期：2026-08-14** | 数据来源：github.com/openai/codex

---

## 1. 今日速览

今日 Codex 仓库共发布 3 个 Rust 预发布版本（0.148.0-alpha.11/12/13），社区讨论热度集中在 **Windows 平台扩展加载失败**（Issue #37458，53 条评论）与 **gpt-5.6-luna 模型兼容性**问题（Issue #34700，36 👍）。PR 侧以 **Guardian 安全审查**与 **MCP 协议增强**为主线，另有 10 余项由自动化机器人提交的功能改进正在合入。

---

## 2. 版本发布

过去 24 小时发布了 3 个 `rust` 分支预发布版本，均为 0.148.0-alpha 系列的迭代：

- **rust-v0.148.0-alpha.13** — [Release 页面](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.13)
- **rust-v0.148.0-alpha.12** — [Release 页面](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.12)
- **rust-v0.148.0-alpha.11** — [Release 页面](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.11)

> 说明：release notes 仅为模板占位文本，具体变更需查看 commits 与关联 PR。建议关注 0.148 主线中关于 Windows 沙箱与子代理的修复合入情况。

---

## 3. 社区热点 Issues（10 条精选）

### 🔥 最热讨论

#### 1. [#37458] Codex 扩展加载失败: "The extension couldn't load its resources"
- 状态：已关闭 | 评论：53 | 👍：11 | [链接](https://github.com/openai/codex/issues/37458)
- **重要性**：近一周社区最热话题。Windows + VS Code 环境下扩展面板无法启动，连带 Remote-SSH 场景同样复现（见 #37517）。影响面广，直接阻断用户使用。
- **社区反应**：53 条评论中用户反馈了多种绕行方案，官方已关闭该问题，疑似已定位到 CSP 字体资源阻塞（相关修复见 PR 讨论）。

#### 2. [#34700] spawn_agent 拒绝 gpt-5.6-luna（多代理模式）
- 状态：开放 | 评论：15 | 👍：36 | [链接](https://github.com/openai/codex/issues/34700)
- **重要性**：`multi_agent_v2` 开启时，`spawn_agent` 无法识别新模型 `gpt-5.6-luna`。这是新模型发布后常见的兼容性滞后，36 👍 说明大量 Pro/Enterprise 用户遇到。
- **社区反应**：多用户确认在 CLI 0.145.0 与 Windows App 26.715.9868.0 上均复现。

#### 3. [#19909] 功能请求：让 "Chats" 项目目录可配置
- 状态：开放 | 评论：17 | 👍：35 | [链接](https://github.com/openai/codex/issues/19909)
- **重要性**：Codex App 默认将聊天存储写入 `~/Documents/Codex`，该目录在 macOS 上常被 iCloud Drive 同步，导致存储混乱与性能问题。35 👍 的高赞说明这是 App 用户普遍痛点。
- **社区反应**：用户建议应支持自定义路径或迁移至 `~/Library/Application Support`。

#### 4. [#18906] TUI 支持 Markdown 数学渲染（内联/块级 LaTeX）
- 状态：开放 | 评论：15 | 👍：22 | [链接](https://github.com/openai/codex/issues/18906)
- **重要性**：终端 UI 无法渲染 LaTeX 公式，影响技术文档的阅读体验。22 👍 表明科研/学术用户对该功能有真实需求。

#### 5. [#21850] TUI Vim 模式：默认以插入模式启动
- 状态：开放 | 评论：6 | 👍：20 | [链接](https://github.com/openai/codex/issues/21850)
- **重要性**：目前启用 Vim 模式后总是进入 Normal 模式，用户希望新增配置项以默认进入 Insert 模式。20 👍 反映 Vim 用户群体对 TUI 编辑体验的精细化要求。

### 🐛 高影响 Bug

#### 6. [#36523] [P0 回归] macOS App 启动时 OOM 崩溃
- 状态：开放 | 评论：6 | 👍：1 | [链接](https://github.com/openai/codex/issues/36523)
- **重要性**：`external-agent-import` 每次启动解析 Claude Desktop 目录下 1.73 GB 历史数据，导致 V8 堆 OOM。该机器此前 26 小时内崩溃 26 次，属于严重的资源管理回归。

#### 7. [#35871] Windows 沙箱：MSIX 版 PowerShell 导致 CreateProcessAsUserW 失败
- 状态：开放 | 评论：13 | 👍：3 | [链接](https://github.com/openai/codex/issues/35871)
- **重要性**：Windows 沙箱在解析到 Microsoft Store 安装的 pwsh（MSIX 打包）时，受限令牌拒绝启动，抛出 `CreateProcessAsUserW failed: 5`。这影响所有使用 Store 版 PowerShell 的 Windows 用户。

#### 8. [#31198] 子代理会话日志膨胀至 145 GiB
- 状态：开放 | 评论：6 | 👍：0 | [链接](https://github.com/openai/codex/issues/31198)
- **重要性**：`compacted.replacement_history` 被反复写入子代理 JSONL 日志，单个长父线程产生 145 GiB 的磁盘占用。这是会话持久化设计的严重缺陷。

#### 9. [#35210] browser.tabs.finalize() 静默终止整个 Codex Desktop
- 状态：开放 | 评论：12 | 👍：0 | [链接](https://github.com/openai/codex/issues/35210)
- **重要性**：Windows 桌面版在调用 `browser.tabs.finalize()` 关闭浏览器标签时，整个应用被静默终止，属于数据丢失级别的严重 Bug。

#### 10. [#25285] Windows 插件缓存哈希路径持久化导致会话技能丢失
- 状态：开放 | 评论：10 | 👍：1 | [链接](https://github.com/openai/codex/issues/25285)
- **重要性**：会话中持久化了易变的 `SKILL.md` 绝对路径（含缓存哈希目录）。插件缓存更新后旧路径失效，历史线程无法加载技能，代表了一类缓存路径设计的通病。

---

## 4. 重要 PR 进展（10 条精选）

#### 1. [#38440] App-server 支持分页线程回滚（thread/revert）
- [PR 链接](https://github.com/openai/codex/pull/38440)
- **内容**：新增实验性 `thread/revert` 请求，可将分页线程的持久化历史回滚至指定 `beforeTurnId`。合入后用户可更灵活地管理长会话。

#### 2. [#38448] MCP 服务器级 OAuth 回调端口
- [PR 链接](https://github.com/openai/codex/pull/38448)
- **内容**：允许为每个 MCP 服务器单独配置 `oauth.callback_port`，并支持插件声明与技能依赖元数据中传递该配置。MCP 集成灵活性显著提升。

#### 3. [#38441] Guardian V2 获得完整工具动作上下文
- [PR 链接](https://github.com/openai/codex/pull/38441)
- **内容**：向工具生命周期贡献者暴露原始的 `ToolPayload`（包含请求动作与对话上下文），使安全审查不再仅凭工具名称和 call ID 判断风险。

#### 4. [#38445] 上下文压缩时保留客户端开发者消息
- [PR 链接](https://github.com/openai/codex/pull/38445)
- **内容**：解决 `retain_client_developer_messages` 启用时，压缩后客户端编写的开发者指令丢失的问题，保证长会话中用户自定义指令持续生效。

#### 5. [#38449] 暴露模型升级退役时间
- [PR 链接](https://github.com/openai/codex/pull/38449)
- **内容**：解析模型升级元数据中的 `retirement_at` 字段，并通过 `model/list` 的 `upgradeInfo.retirementAt` 以可空 Unix 时间戳输出。提升模型生命周期透明度。

#### 6. [#38452] 响应重试的结构化遥测
- [PR 链接](https://github.com/openai/codex/pull/38452)
- **内容**：在重试延迟前发出 `codex.retry` 追踪事件，覆盖 HTTP 请求、采样流、远程压缩和连接恢复。为社区排查间歇性故障提供关键数据。

#### 7. [#38450] Bazel 构建中嵌入 Windows 沙箱配置清单
- [PR 链接](https://github.com/openai/codex/pull/38450)
- **内容**：修复 `rules_rust` 丢弃二进制链接器指令的问题，确保 Bazel 构建的 Windows 沙箱辅助程序携带 `asInvoker` 清单，解决提权提示问题。

#### 8. [#38436] 本地 MCP HTTP 请求增加 rustls 回退
- [PR 链接](https://github.com/openai/codex/pull/38436)
- **内容**：当平台 TLS 后端无法与 HTTPS 端点协商协议版本时，对可重放请求使用 rustls 重试一次。改善本地 MCP 服务器的连接可靠性。

#### 9. [#38447] 本地守护进程会话增加"运行中任务退出"选项
- [PR 链接](https://github.com/openai/codex/pull/38447)
- **内容**：在本地守护进程中按 Ctrl-C 且编辑器为空时，显示菜单支持用户选择：取消任务留在 Codex、退出但保持任务运行、或停止守护进程。

#### 10. [#38446] 全历史子代理的当前时间提醒刷新
- [PR 链接](https://github.com/openai/codex/pull/38446)
- **内容**：在将父历史复制到全历史子代理时，排除旧的当前时间提醒消息，保留子代理自身的新提醒，避免提醒消息累积污染上下文。

> 另有 [#31453 exec-server: 在 executor 上启动托管网络代理](https://github.com/openai/codex/pull/31453) 等长线 PR 仍在推进。

---

## 5. 功能需求趋势

从今日活跃 Issues 中可提炼出以下社区重点关注方向：

| 方向 | 代表 Issue / PR | 热度信号 |
|------|----------------|---------|
| **TUI 编辑器能力补全** | Vim 模式增强（#21850、#32745、#33296），/copy 指定消息（#24073），LaTeX 渲染（#18906） | 3 个 Vim 相关请求同时活跃，用户希望 TUI 达到可用级编辑器标准 |
| **Windows 平台稳定性** | Windows 沙箱系列（#35871、#30829、#28457）、扩展加载失败（#37458）、缓存路径问题（#25285） | 30 条展示 Issues 中近半数与 Windows 相关，是当前最大短板 |
| **会话/上下文管理** | 日志膨胀（#31198）、子代理计数（#22779）、客户端消息保留（#38445）、线程回滚（#38440） | 长会话场景下，持久化与压缩成为高频改进主题 |
| **新模型兼容性** | gpt-5.6-luna 拒绝（#34700）、Unknown model（#37910） | 模型发布节奏与工具链适配存在明显时间差 |
| **安全审查（Guardian）增强** | Guardian V2 上下文（#38441）、Node REPL 指导（#38427、#38432）、审批策略保留（#38439） | 多 PR 并行推进，安全侧持续加码 |
| **MCP 集成深度** | OAuth 回调端口（#38448）、rustls 回退（#38436）、schema $ref 解析（#31901） | MCP 协议支持正从"可用"走向"企业级" |

---

## 6. 开发者关注点

### 高频痛点 Top 3

1. **Windows 沙箱反复踩坑**
   - 多个独立问题指向同一根因：沙箱辅助程序在特定 Windows 环境下无法正确解析或启动（MSIX 携带、bin junction、独立启动器资源缺失）。开发者对沙箱稳定性的信任度正在下降，建议官方优先合并 #38450 类修复并补充安装包自检逻辑。

2. **应用启动/资源加载失败阻塞入口**
   - 扩展 "couldn't load its resources" 与 macOS OOM 崩溃（#36523）均发生在用户打开工具的第一步，属于"零容忍"级别问题。尤其是 #36523 在 26 小时内崩溃 26 次，必须视为 P0 回归。

3. **子代理与长会话的资源失控**
   - 145 GiB 日志膨胀（#31198）、已完成子代理仍占用线程配额（#22779）、重启后子代理卡在 running 状态（#38408）——子代理机制在持久化与清理方面仍有明显设计缺口。

### 值得注意的信号

- **自动化 PR 密度高**：今日 20 条展示 PR 中有 14 条来自 `copyberry[bot]`，说明 OpenAI 内部正在大量使用机器人驱动的代码合并流水线，社区反馈转化为修复的效率有望提升。
- **模型生命周期管理需求上升**：开发者希望提前知晓模型退役时间（#38449），并在模型升级后获得平滑的兼容性保障，而不是被动等待 hotfix。
- **TUI 用户群体持续扩大**：多个 Vim 模式 issue 保持活跃（#21850、#32745、#33296），且点赞数不低，建议官方产品路线图将 TUI 编辑器体验列为独立优化项。

---

*本日报由 AI 自动聚合生成，数据覆盖过去 24 小时 openai/codex 仓库的动态。链接均为 GitHub 原始地址，点击可查看完整讨论。*

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 · 2026-08-14

> 数据来源：github.com/google-gemini/gemini-cli ｜ 统计窗口：2026-08-13 至 2026-08-14

## 1. 今日速览

- 昨日发布 v0.56.0-nightly，主要推进评估验证（eval validation）与工具调用格式化能力，是持续强化行为评估体系的一环。
- Subagent 可靠性成为社区最集中的痛点：`MAX_TURNS` 误报「成功」、generalist 代理挂起、shell 命令卡死等 4 个 p1 Bug 在 24 小时内持续收到高频讨论。
- 多个关键修复 PR 合并（容量错误静默重试、多轮取消回滚、Claude 新模型定义），同时 A2A 服务器认证与路径穿越漏洞修复仍在审查中。

## 2. 版本发布

**v0.56.0-nightly.20260813.g1ac337739**

- **Feat/eval validate**（PR [#28344](https://github.com/google-gemini/gemini-cli/pull/28344)）：新增评估结果的验证逻辑。
- **feat(evals): tool call formatter + failure summaries**（PR [#28305](https://github.com/google-gemini/gemini-cli/pull/28305)）：为工具调用添加格式化器，并整合失败摘要，便于定位评估中的工具执行问题。
- Changelog for v0.55.1（由 gemini-cli-robot 自动生成）。

## 3. 社区热点 Issues

以下 10 个 Issue 在过去 24 小时内讨论最活跃、优先级最高或最能反映社区诉求：

### 🔴 高优先级 Bug（p1）

**1. Subagent 在 MAX_TURNS 后误报 GOAL 成功 — #22323**
[Issue #22323](https://github.com/google-gemini/gemini-cli/issues/22323) ｜ 评论 12 ｜ 👍 2
`codebase_investigator` 子代理在未完成任何分析就已触顶 turn 上限时，仍会报告 `status: "success"`、`Termination Reason: "GOAL"`，掩盖了真实的中断原因。这是当前评论数最高的 Issue，直接破坏了对子代理执行结果的信任。

**2. Generalist 代理挂起（hang）— #21409**
[Issue #21409](https://github.com/google-gemini/gemini-cli/issues/21409) ｜ 评论 8 ｜ 👍 8
用户反馈一旦 `gemini-cli` 委派给 generalist agent，即使创建文件夹这类简单操作也会无限期挂起，最久等待 1 小时无响应。社区给出的 workaround 是手动指示模型不要使用 subagent，说明该问题已严重影响日常使用。

**3. Shell 命令执行完成后卡在 “Waiting input” — #25166**
[Issue #25166](https://github.com/google-gemini/gemini-cli/issues/25166) ｜ 评论 4 ｜ 👍 3
简单 CLI 命令执行完毕但界面仍显示「等待输入」，命令状态保持 active。该问题高频复现，严重阻塞 shell 工具链路。

**4. Browser subagent 在 Wayland 环境失败 — #21983**
[Issue #21983](https://github.com/google-gemini/gemini-cli/issues/21983) ｜ 评论 4 ｜ 👍 1
浏览器子代理在 Wayland 下因会话锁等环境问题导致 `Termination Reason: GOAL` 之前即失败。Linux 桌面用户受影响面较大。

**5. get-shit-done 输出 hook 在收尾时崩溃 — #22186**
[Issue #22186](https://github.com/google-gemini/gemini-cli/issues/22186) ｜ 评论 3
当输出 hook 即将打印用户摘要时 Gemini CLI 反复崩溃，定位为 hook 生命周期管理缺陷。

**6. Bugreport 不包含子代理上下文 — #21763**
[Issue #21763](https://github.com/google-gemini/gemini-cli/issues/21763) ｜ 评论 2
`/bug` 报告只包含主会话信息，子代理内部发生的问题无法通过 bugreport 复现，导致问题诊断成本极高。

### 🟠 功能与评估

**7. 组件级评估体系（EPIC）— #24353**
[Issue #24353](https://github.com/google-gemini/gemini-cli/issues/24353) ｜ 评论 7
EPIC 追踪「行为评估」体系的演进，目前已有 76 个 behavioral eval 测试、覆盖 6 个 Gemini 模型，目标是构建更稳健的组件级评估管道。

**8. AST 感知的文件读取 / 搜索 / 代码库映射（EPIC）— #22745**
[Issue #22745](https://github.com/google-gemini/gemini-cli/issues/22745) ｜ 评论 7
评估通过 AST 感知工具提升方法级读取精度、减少无效回合与 token 噪声，并计划探索更智能的代码库导航能力。

### 🟡 安全与权限

**9. Auto Memory 需确定性脱敏并减少日志 — #26525**
[Issue #26525](https://github.com/google-gemini/gemini-cli/issues/26525) ｜ 评论 4
当前 Auto Memory 会将本地 transcript 发送给模型后再提示脱敏，内容已经暴露在模型上下文中，且服务可能记录已有 skill 数据。安全边界受到社区关注。

**10. Subagent 在 v0.33.0 后未经许可被调用 — #22093**
[Issue #22093](https://github.com/google-gemini/gemini-cli/issues/22093) ｜ 评论 3
用户配置中 agents 全部 disabled，但 subagent（如 generalist）仍被自动启用并执行。权限模型的行为变更引发了信任问题。

## 4. 重要 PR 进展

### ✅ 已合并 / 已关闭

**1. 容量错误（capacity errors）的上下文感知静默重试 — #28790（已关闭）**
[PR #28790](https://github.com/google-gemini/gemini-cli/pull/28790) ｜ 关闭 #28761
为非交互式 CLI 运行引入指数退避与最多 2 次静默重试，并实现可用性 TTL，修复关键容量耗尽重试回归。

**2. 新增 Claude Sonnet 4.5 与 Opus 4.8 模型定义 — #28803（已关闭）**
[PR #28803](https://github.com/google-gemini/gemini-cli/pull/28803)
添加新模型常量、别名解析与策略链回退，更新显示名称与描述，为跨模型工作流提供支持。

**3. 取消/中止时回滚整个多轮请求 — #28801（已关闭）**
[PR #28801](https://github.com/google-gemini/gemini-cli/pull/28801)
修复多轮工具调用被中止后聊天历史残留在未响应状态的问题，避免后续无关请求（如「Hello」）出现异常。

**4. 规范 git 环境并解决 workspace 状态不一致 — #28792（已关闭）**
[PR #28792](https://github.com/google-gemini/gemini-cli/pull/28792)
统一 Git 子进程的环境配置，并修复 workspace 信任评估中的状态初始化问题，确保内部 Git 工具可预测地非交互执行。

**5. 保留 functionCall 中的 thoughtSignature，修复 400 错误 — #28586（已关闭）**
[PR #28586](https://github.com/google-gemini/gemini-cli/pull/28586)
修复 v0.53.0 引入的并行工具调用 400 Bad Request 回归——`thoughtSignature` 此前被意外剥离。

### 🔵 审查中 / 进行中

**6. A2A 服务器：强制认证并阻止 checkpoint 路径穿越 — #28699（开启）**
[PR #28699](https://github.com/google-gemini/gemini-cli/pull/28699)
自定义 REST 路由绕过 `UserBuilder` 导致可无凭证访问，同时存在 checkpoint 路径穿越风险，属于安全关键修复。

**7. vscode-ide-companion：解决 stop() 挂起与 keep-alive 阈值 — #28789（开启）**
[PR #28789](https://github.com/google-gemini/gemini-cli/pull/28789)
修复 `IdeServer.stop()` 在 MCP 流式会话打开时无限挂起，并修复 keep-alive 间歇性失败导致的资源泄漏。

**8. MCP 配置损坏不应被当作空配置 — #28787（开启）**
[PR #28787](https://github.com/google-gemini/gemini-cli/pull/28787)
JSON 解析失败时不再折叠为 `{}`，避免 `isFileEnabled()` 默认启用所有未配置的 MCP server。

**9. WSL2 剪贴板图片粘贴支持 — #27588（开启）**
[PR #27588](https://github.com/google-gemini/gemini-cli/pull/27588)
通过 PowerShell interop 读取 Windows 剪贴板图片，补全 WSL 环境下的图片粘贴能力。

**10. 行为评估工具扩展：read_many_files / MCP 资源 / 内部文档 — #28804（开启）**
[PR #28804](https://github.com/google-gemini/gemini-cli/pull/28804)
为批量文件读取、CLI 内部文档查询、MCP 资源发现与读取新增 behavioral evals，扩大评估覆盖范围。

## 5. 功能需求趋势

从近 24 小时更新的 50 条 Issue 中提炼出以下 5 个最集中的功能方向：

1. **Subagent 行为可观测性与可靠性**（约 15 条相关 Issue）
   - 误报成功/GOAL、挂起、上下文不可见、不受控制地被调用，均指向子代理的执行生命周期需要更强的状态透出与控制能力。
   - 呼声包括：`/chat share` 展示 subagent 轨迹、bugreport 携带子代理上下文、settings.json 对 browser agent 的覆盖生效。

2. **评估体系工程化**（#24353、#22745、#28804、#28344）
   - 社区与官方都在推动 behavioral evals 从「测试数量」走向「组件级评估」，并引入 AST 感知工具来提升代码理解类任务的评测质量。

3. **安全与隐私加固**（#26525、#26522、#28699）
   - Auto Memory 的内容脱敏时机、无效 patch 的隔离、A2A 服务器的认证强制，反映出多智能体与记忆机制引入后，安全边界成为重点关切。

4. **平台兼容性补齐**（#21983、#25378、#27588）
   - Wayland 下的 browser agent、Windows ripgrep 架构匹配、WSL2 剪贴板图片粘贴持续被社区提及，跨平台体验仍是短板。

5. **MCP 与 IDE 生态稳定性**（#28787、#28789、#24246）
   - MCP 配置损坏后的静默兜底、工具数量过大（>128/400）导致的 400 错误、IDE companion 的挂起修复，均围绕「外部工具集成后的系统韧性」展开。

## 6. 开发者关注点

- **「挂起」比「报错」更消耗信任**：无论是 generalist agent 的无限 hang 还是 shell 命令假死，用户普遍反馈需要手动取消/等待数十分钟，这比功能缺失更影响采用意愿。
- **误报成功需要零容忍**：#22323 中 subagent 明明触顶 MAX_TURNS 却报告 GOAL success，开发者认为这会污染自动化流程的决策依据，属于「必须尽快修复」级别的问题。
- **权限模型变更需要更透明**：v0.33.0 之后 subagent 默认行为变化导致未经许可调用，用户的预期是显式 opt-in，回归事件损伤信任。
- **新模型支持是持续兴奋点**：Claude Sonnet 4.5 / Opus 4.8 模型定义的合并获得积极关注，开发者期待 Gemini CLI 作为通用 Agent 入口能灵活接入更多模型。
- **小型质量改进同样受关注**：`\n` 转义错误、terminal resize 闪烁、Session Browser 重命名（#28805，葡萄牙语反馈）等细节问题被持续提出，说明社区用户已开始对整体 UX 精雕细琢。

---
*本日报基于 2026-08-13 至 2026-08-14 的公开 GitHub 数据自动分析生成，供技术开发者快速了解社区动态。*

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报

**日期：2026-08-14**


## 1. 今日速览

今日社区动态集中在**模型配置与控制**领域：新版本 v1.0.80-0 新增了 `--enable-mcp-server` 参数，同时多个 Issue 围绕自定义 Agent 的 reasoning effort 支持、模型硬编码/静默降级问题展开激烈讨论。此外，**MCP 生态的稳定性问题（OAuth 认证、并发、重试机制）** 成为今日发帖量最大的主题，值得关注。


## 2. 版本发布

### v1.0.80-0

**新增内容：**
- 新增 `--enable-mcp-server` 参数，可在当前会话中重新启用设置在设置中禁用的 MCP 服务器
- 会话共享提示改进：在 `--ahp` 模式下，当其他客户端加入共享会话时，Sessions 标签页中对应行会显示 `2 clients`（或更多）标注

> 链接：[Release v1.0.80-0](https://github.com/github/copilot-cli/releases)


## 3. 社区热点 Issues

### #2904 自定义 Agent 的 YAML Frontmatter 应支持 Reasoning Effort
**作者**: brian-kelley-intel | 👍 20 | 💬 6 | 更新于 08-13

自定义 Agent（`.agent.md`）可以通过 `model` 字段指定模型，但**无法为单个 Agent 单独设置推理强度（reasoning effort）**，目前只能通过全局 `--effort` 标志配置。该 Issue 已开放近 4 个月且持续获得关注，是社区对 Agent 精细化控制的核心诉求。

> 链接：https://github.com/github/copilot-cli/issues/2904

### #4345 / #4473 claude-haiku-4.5 不支持 'medium' Reasoning Effort
**作者**: indeherb / philtillman | 👍 4 | 💬 5 | 更新于 08-13

多个用户报告同一问题：当 CLI 内部将子任务路由到 `claude-haiku-4.5` 时，会默认施加 `medium` 推理强度，导致执行报错。**同一问题出现两个独立 Issue**（#4345 已关闭，#4473 于今日新开），说明该 bug 可能仍未被彻底修复，影响面较大。

> 链接：https://github.com/github/copilot-cli/issues/4345 ｜ https://github.com/github/copilot-cli/issues/4473

### #2133 自定义 Agent `model` 字段不支持数组语法——与 VS Code Copilot Chat 不兼容
**作者**: deyil | 👍 7 | 💬 4 | 更新于 08-13

VS Code Copilot Chat 支持 `model` 字段使用**数组**语法，但 Copilot CLI 解析同一格式时报错并拒绝加载 Agent。这一跨工具兼容性问题已存在 5 个月，持续获得社区共鸣。

> 链接：https://github.com/github/copilot-cli/issues/2133

### #3954 `explore` 工具硬编码模型为 `gpt-5.4-mini`，忽略自定义/DeepSeek API 配置
**作者**: Aferrara3 | 👍 3 | 💬 3 | 更新于 08-13

用户配置的自定义模型（如 DeepSeek 端点）在 `explore` 工具中被忽略，CLI 硬编码使用 `gpt-5.4-mini` 发起请求，导致 API 调用失败。对于依赖自定义模型的企业用户，此问题影响严重。

> 链接：https://github.com/github/copilot-cli/issues/3954

### #3565 Task 工具通过 multiplier guard 静默降级子代理模型
**作者**: ReefProctor | 👍 1 | 💬 1 | 更新于 08-13

当子代理请求的模型成本倍率高于会话模型时，Task 工具会**静默降级**，且无论 frontmatter 声明还是显式 `model` 覆盖都无法阻止。该问题已标记为已关闭，但仍被引用讨论，说明模型控制逻辑仍需完善。

> 链接：https://github.com/github/copilot-cli/issues/3565

### #4346 MCP Registry 策略获取在 CI 中返回 403，阻断所有非默认 MCP 服务器
**作者**: ben-ogp | 👍 3 | 💬 1 | 更新于 08-13

在 GitHub Actions 中使用内置 `GITHUB_TOKEN`（官方文档推荐的 PAT-less 方式）认证时，MCP 服务器的策略获取返回 403，导致 CI 中**无法使用任何非默认 MCP 服务器**。对 CI/CD 场景是关键的阻断性问题。

> 链接：https://github.com/github/copilot-cli/issues/4346

### #4480 Atlassian MCP OAuth 在 1.0.79 上失败——1.0.71 的回归
**作者**: jfrost-fabric | 👍 0 | 💬 0 | 新发于 08-13

升级到 1.0.79 后，连接 Atlassian 远程 MCP 服务器（`mcp.atlassian.com`）在 OAuth 发现阶段报错 `Incompatible authorization server`，之前版本工作正常。**明确的版本回归问题**，值得维护者优先排查。

> 链接：https://github.com/github/copilot-cli/issues/4480

### #4478 MCP 服务器碰撞检测大小写敏感，跨配置作用域时误判
**作者**: cherchyk | 👍 0 | 💬 0 | 新发于 08-13

MCP 服务器可从多个作用域发现（用户配置、插件等），但服务器名碰撞检测**大小写敏感**，导致 `MCPBrowser` 和 `mcpbrowser` 被当作两个独立服务器重复处理。配置管理的边界情况 bug。

> 链接：https://github.com/github/copilot-cli/issues/4478

### #4477 停止操作或点击停止按钮时，会话和提示词被删除
**作者**: daveroama | 👍 0 | 💬 0 | 新发于 08-13

用户在执行过程中点击停止按钮，**整个会话（含原始提示词和编辑内容）被删除**，且已多次复现。对交互式工作流影响极大，属于严重的数据丢失问题。

> 链接：https://github.com/github/copilot-cli/issues/4477

### #4469 孤儿 `permission.requested` 事件在每次会话恢复时重放，目录访问提示无法消除
**作者**: grahamgreen | 👍 0 | 💬 0 | 更新于 08-13

一个长期运行的会话在每次恢复时都弹出"允许目录访问"提示，引用的是 10 天前已完成命令中的路径，批准后仍反复出现。**权限事件的持久化/清理机制存在缺陷**，对长期会话用户极为恼火。

> 链接：https://github.com/github/copilot-cli/issues/4469


## 4. 重要 PR 进展

### #4476 docs: 为自定义 Agent effort frontmatter 补充文档（Option A）
**作者**: romanstetsenko | 状态: 已关闭 | 更新于 08-13

该 PR 为 Issue #2904 的 **Option A 方案**（专用 `effort` 字段，与 `model` 字段并列）编写了文档，添加了 "Custom Agents" 参考章节，涵盖现有 frontmatter 字段（`name`、`description`、`model`）以及新增的 `effort` 字段说明。虽然此 PR 仅为文档，但它**反映了官方对 Agent 级别推理强度支持的推进方向**，对关注 #2904 的社区用户是积极信号。

> 链接：https://github.com/github/copilot-cli/pull/4476

**说明**：今日 PR 数量较少，可能与发布窗口或维护节奏有关。如有更多 PR 合入，将在后续日报中跟进。


## 5. 功能需求趋势

结合近期 Issue 与 PR，社区最关注的功能方向如下：

| 方向 | 具体诉求 | 代表 Issue/PR |
|------|---------|--------------|
| **精细化的模型控制** | 自定义 Agent 支持独立的 reasoning effort、模型数组语法，子代理模型不被静默覆盖 | #2904, #2133, #3565, #4476 |
| **MCP 生态稳定性** | OAuth 认证的可靠性、并发调用时的 token 刷新同步、5xx 自动重试、服务器名冲突检测 | #4463, #4464, #4466, #4472, #4478, #4480 |
| **会话管理增强** | 提供列出/监控所有运行中会话的命令（类似 Claude Code 的 `agents --json`）、会话恢复/停止不丢数据 | #4470, #4477, #4474, #4467 |
| **权限系统改进** | `allowed_directories` 在 shell 命令中生效、权限事件不残留重放 | #4482, #4469, #4237 |

**值得注意的趋势**：
- **MCP 已成为社区关注的核心领域**，今日 27 个活跃 Issue 中有近 1/3 与 MCP 相关，从认证、并发、重试到配置管理都有覆盖。
- **自定义 Agent 的模型控制能力**是持续高热度的诉求，#2904 的点赞数和讨论数在长期 Issue 中排名靠前，且有对应 PR 开始推进。


## 6. 开发者关注点

### 高频痛点与反馈

1. **模型配置被忽略或覆盖（今日最高频）**
   - `explore` 工具硬编码 `gpt-5.4-mini`（#3954）
   - code-review 子代理的 `gpt-5.6-luna` 被替换为 `gpt-5.6-sol`（#4462）
   - claude-haiku-4.5 被强制施加不支持的 `medium` reasoning effort（#4345, #4473）
   - Task 工具的 multiplier guard 静默降级模型（#3565）
   
   *开发者期望：模型选择应遵循"最明确的配置优先"，不能被隐式逻辑覆盖，即使需要降级也应显式告知。*

2. **MCP OAuth 和网络问题频发**
   - 多个 Issue 报告 OAuth 失败：Atlassian 回归（#4480）、Windows socket 10013（#4463）、Entra AD scope 复用 bug（#4464）、并发 token 刷新导致传输中断（#4472）
   
   *开发者期望：OAuth 流程需要更健壮的异常处理和重试机制，尤其是在 token 刷新和并发场景下。*

3. **会话数据丢失和状态混乱**
   - 停止操作导致整个会话被删除（#4477）
   - 长会话超时后被静默归档，无恢复入口（#4474）
   - 权限事件在恢复后反复重放（#4469）
   
   *开发者期望：会话是重要工作资产，停止/恢复/归档操作都应可预期、可恢复。*

4. **企业/CI 场景的配置硬伤**
   - `GITHUB_TOKEN` 在 CI 中无法获取 MCP Registry 策略（#4346）
   - 配置了 `allowed_directories` 但 shell 命令仍弹目录访问提示（#4482）
   - 长会话事件存储耗尽导致会话状态错乱（#4467）
   
   *开发者期望：文档中承诺的配置行为应与实际行为一致，CI 中的 MCP 使用应有明确的官方支持路径。*

5. **跨工具兼容性**
   - VS Code Copilot Chat 支持的 `model` 数组语法在 CLI 中被拒绝（#2133）
   - Copilot App 1.1.8 仍受旧版 CLI 策略门控（#4481）
   
   *开发者期望：同一生态内的工具应共享 Agent 定义格式，避免同一份配置在不同工具间互相冲突。*


*本日报由 AI 工具分析 GitHub 公开数据自动生成，仅供参考。*


</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报（2026-08-14）

## 今日速览

过去 24 小时，Kimi Code CLI 仓库无新版本发布、无新 PR 更新，共有 3 个 Issue 处于活跃状态。值得关注的是：长期热议的**跨会话 Memory System** 功能请求仍在持续讨论；新增两个高影响 bug —— ACP/print 模式流式响应静默挂死（#2598）以及单次 LLM 步骤生成 8.8 万 token 乱码的失控问题（#2597）。社区侧重点集中在会话记忆、流式稳定性与生成质量上。

## 版本发布

无新增版本。

## 社区热点 Issues

以下为过去 24 小时更新的全部 3 个 Issue（按关注度排序）：

### 1. [enhancement] Feature Request: Memory System - Persistent context across sessions（#1283）
- **作者**: CatKang  
- **创建**: 2026-02-27  
- **更新**: 2026-08-13  
- **评论**: 38  
- **链接**: [MoonshotAI/kimi-cli Issue #1283](https://github.com/MoonshotAI/kimi-cli/issues/1283)

**为什么重要**：这是仓库中最具人气的功能请求之一，已持续近半年。社区强烈希望 CLI 能够跨会话记住项目上下文、代码模式与用户偏好，实现「自动记忆 + 手动手册」的双层机制。38 条评论表明该需求覆盖面广，长期未得到满足。

**社区反应**：评论多集中在记忆的存储方式、隐私控制与调用透明度上，部分开发者呼吁优先实现手动记忆以避免 AI 自主写库引发污染。

---

### 2. ACP/print 流式响应静默挂死（#2598）
- **作者**: ai-agent-workbench  
- **创建**: 2026-08-09  
- **更新**: 2026-08-13  
- **评论**: 1  
- **链接**: [MoonshotAI/kimi-cli Issue #2598](https://github.com/MoonshotAI/kimi-cli/issues/2598)

**为什么重要**：在 ACP 模式下，模型内容已全部流式返回但终端帧（`[DONE]`/finish）不出现，导致连接静默挂起；且发送下一条消息时，挂死轮被顶替，已流式数据从未写入 `wire.jsonl`。0.31.1 仅处理了 Esc 场景，目前无空闲超时配置，会话会无限等待。该问题严重影响自动化工作流与日志审计。

**社区反应**：目前仅作者自述，但问题描述详尽，涉及协议层与轮次管理，可能影响多个 ACP 用户。作者已在 issue 中提供复现路径与环境信息，等待官方确认。

---

### 3. Runaway garbled generation — 单步生成 88k 乱码 token（#2597）
- **作者**: kdp123  
- **创建**: 2026-08-08  
- **更新**: 2026-08-13  
- **评论**: 1  
- **链接**: [MoonshotAI/kimi-cli Issue #2597](https://github.com/MoonshotAI/kimi-cli/issues/2597)

**为什么重要**：正常交互会话中，模型单次 LLM 步骤失控运行 53 分钟，产生 88,114 个输出 token，内容为无意义的重复乱码。这暴露了 CLI 缺少生成长度上限、异常生成检测与中断控制机制，会消耗大量计算资源并污染会话历史。

**社区反应**：其他用户可能遇见过类似现象，但当前评论较少。该 bug 对资源消耗和用户体验的冲击极大，需优先处理。

## 重要 PR 进展

今日无新增或更新的 Pull Request。

## 功能需求趋势

从当前活跃 Issues 中可提炼出社区最关注的三个方向：

1. **跨会话持久上下文**：期望 CLI 具备记忆与学习能力，减少重复描述，提升长线任务效率（#1283）。
2. **流式传输可靠性**：要求提供空闲超时、异常终止检测、或强制取消机制，避免挂死拖垮整个会话（#2598）。
3. **生成质量管控**：需要 max tokens 限制、流式输出审计、异常生成熔断等保障措施，杜绝失控输出（#2597）。

## 开发者关注点

- **ACP 模式下日志不落盘**：流式数据丢失导致无法重放或审计，开发者希望 CLI 在异常时也至少保留 `wire.jsonl` 数据。
- **长任务失控无保护**：缺乏运行时防护，单个异常 step 可消耗近 1 小时计算资源，社区要求内置超时与 token 上限配置。
- **记忆机制透明度**：即便 Memory System 呼声高，开发者仍强调用户应能查看、编辑和删除记忆内容，防止模型误写入敏感信息。

> 数据来源：github.com/MoonshotAI/kimi-cli | 日报生成时间：2026-08-14

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报 (2026-08-14)

## 今日速览
OpenCode 发布修复版 **v1.18.18**，解决了 Kimi 官方 provider 的系统提示词选择错误和 xAI 模型高推理强度问题。社区方面，围绕「保留旧版布局」的讨论持续高热（#37012），同时一批安全报告（升级脚本无完整性校验、webfetch SSRF、上下文修剪丢指令）以及稳定性回归（桌面应用启动失败、无限重试循环）成为开发者关注焦点。

## 版本发布
### v1.18.18
- **Core 修复**：
  - 为官方 Moonshot / Kimi provider 正确选择 Kimi 系统提示词。
  - 修复 xAI 模型的 `xhigh` reasoning effort 异常。

https://github.com/anomalyco/opencode/releases/tag/v1.18.18

## 社区热点 Issues
以下 10 个 Issue 在过去 24 小时受到最多关注，覆盖布局回归、插件生态、安全、稳定性等问题：

1. **[FEATURE] keep legacy layout option**（#37012）  
   旧版布局呼声极高：37 条评论、41 👍。用户指出旧版可从一个主窗口快速访问几乎所有功能，新版需要多层导航；并且支持 workspace 使用方式。社区对交互效率下降有明显不满。  
   https://github.com/anomalyco/opencode/issues/37012

2. **[Regression] plugin provider.models() hook no longer populates custom providers**（#25630）  
   PR #25167 合并后，插件 `provider.models()` 钩子无法为用户自定义 provider 填充模型列表。直接影响依赖 models.dev 目录之外模型的插件生态，社区较多插件作者关注（15 评论，6 👍）。  
   https://github.com/anomalyco/opencode/issues/25630

3. **"Copied to clipboard" doesn't work**（#41470）  
   VS Code Server（Docker）环境下，OpenCode 会话复制显示成功但实际未写入系统剪贴板。该问题影响远程开发用户，且与编辑器集成体验直接相关。  
   https://github.com/anomalyco/opencode/issues/41470

4. **GitHub Copilot provider shows zero models**（#42083）  
   `opencode auth login -p github-copilot` 可成功认证，但模型选择器完全不显示 Copilot 模型，`opencode models github-copilot` 返回 “Provider not found”。官方集成处于不可用状态。  
   https://github.com/anomalyco/opencode/issues/42083

5. **Desktop app: provider/model/MCP fail to load on startup**（#40516）  
   v1.18.5 到 v1.18.13 的桌面版约 80% 启动无法加载 provider/model/MCP，只有降级到 v1.18.4 才正常。多个组织用户受影响，属于明显的版本回归。  
   https://github.com/anomalyco/opencode/issues/40516

6. **Infinite retry loop on consistent provider failure**（#29143）  
   当 provider 持续失败时 fallback 系统可能无限重试，不会切换到下一个配置的模型。社区建议将重试次数上限设为 5 次，以保护用户任务不被卡死。  
   https://github.com/anomalyco/opencode/issues/29143

7. **[SECURITY] "opencode upgrade" fetches a remote script and pipes it to bash**（#42434）  
   curl|bash 安装/升级路径无完整性校验，存在供应链/TOCTOU 风险（Medium）。用户在提交前需要手动确认，但脚本本身未做签名或哈希校验。  
   https://github.com/anomalyco/opencode/issues/42434

8. **[SECURITY] Context pruning silently drops instruction/constraint-bearing content**（#42437）  
   上下文修剪可能静默丢弃含指令/约束的内容，不仅影响成本，更影响任务完整性。被标记为 Medium-High，可能成为约束绕过路径。  
   https://github.com/anomalyco/opencode/issues/42437

9. **[SECURITY] webfetch can fetch loopback/private addresses — SSRF**（#42435）  
   `webfetch` 工具可请求本地/内网地址，已有关联 PR #40851 被关闭未合并。可被模型诱导发起对本地服务的 SSRF 请求。  
   https://github.com/anomalyco/opencode/issues/42435

10. **[2.0] opencode2 mutates shared V1 database and breaks opencode 1.x coexistence**（#42260）  
   2.0 版本迁移共享数据库 schema，导致 1.x 的 `/move` 命令失效、会话被困在 worktree 中。V2 与稳定版共存的关键兼容性问题。  
    https://github.com/anomalyco/opencode/issues/42260

## 重要 PR 进展
以下 10 个 PR 在过去 24 小时更新，涵盖性能、模型链路、MCP 稳定性、插件机制与桌面端适配：

1. **feat(processor): add model fallback chain when retries are exhausted**（#42424）  
   为主模型重试耗尽后自动启用 fallback 模型链，解决 #10287，提升生产环境容错能力。  
   https://github.com/anomalyco/opencode/pull/42424

2. **fix(opencode): preserve response model metadata**（#42433）  
   保留 AI SDK 返回的 `response.modelId`，修复 #42420。相比 #26091 更窄更安全，只保留结构化模型 ID，不引入任意响应头。  
   https://github.com/anomalyco/opencode/pull/42433

3. **fix(mcp): retry failed MCP connections to handle parallel spawn race condition**（#42431）  
   修复在 `concurrency: "unbounded"` 下并行 spawn MCP server 时出现 “Connection closed” 的竞态问题，增加重试机制。  
   https://github.com/anomalyco/opencode/pull/42431

4. **fix(provider): add kimi-for-coding custom handler and fix model detection for k2p6 (Kimi K2.6)**（#42428)  
   修复 Kimi K2.6 的模型 ID `k2p6` 检测及相关代码路径，补全 kimi-for-coding provider 自定义处理，与今日 v1.18.18 修复方向一致。  
   https://github.com/anomalyco/opencode/pull/42428

5. **fix(opencode): plugin auto-update with temp residue cleanup**（#42427）  
   修复插件 `@latest` 自动更新卡住的问题，增加 `fetchRegistryVersion()` 直接读取 npm registry 的 `dist-tags.latest`，并在安装后清理临时残留文件。  
   https://github.com/anomalyco/opencode/pull/42427

6. **feat(core): add sessionID to agent invoked Shell.create before input**（#40668）  
   在 `shell.create` before 钩子中注入 sessionID，使插件可将 agent 发起的 shell 命令归属到具体会话，有利于审计与流控。  
   https://github.com/anomalyco/opencode/pull/40668

7. **feat(memory): add agent_memory table and memory-tools plugin**（#42425）  
   新增 `agent_memory` 表和 `memory-tools` 插件，支持通过 Supabase 对 OpenCode AgentMemory 进行云端备份/恢复。  
   https://github.com/anomalyco/opencode/pull/42425

8. **fix(skill): ensure plugin config hooks run before skill discovery**（#42430）  
   修复插件的 `config()` 钩子（如 superpowers）在技能发现前未执行，导致 `config.skills.paths` 新增目录不生效的问题。  
   https://github.com/anomalyco/opencode/pull/42430

9. **fix(desktop): wrap MCP commands with wsl.exe when WSL mode is enabled**（#42429）  
   修复 Windows 桌面端在 WSL 模式下，`opencode.json` 中的本地 MCP 命令引用 Linux 可执行文件而无法运行的问题。  
   https://github.com/anomalyco/opencode/pull/42429

10. **[beta] some experimental perf improvements**（#40427）  
   面向 v2 的系列性能优化，包括 session 路由加载、渲染路径等改动，基准对比显示多个场景显著提速，适合 v2 用户提前验证。  
    https://github.com/anomalyco/opencode/pull/40427

## 功能需求趋势
从全部 Issue 中可以提炼出以下社区关注的功能方向：

- **布局与工作区体验**：要求保留旧版布局（#37012）、TUI 增加右侧栏展示后台子代理活动（#42369）、新布局中增加 workspace 选择流程（#38790）。
- **安全与供应链完整性**：升级/安装脚本需要校验（#42434）、`webfetch` 需要限制内网地址（#42435）、上下文修剪需要透明化并保留关键指令（#42437）。
- **模型与 Provider 支持**：GitHub Copilot provider 修复（#42083）、Kimi K2.6 检测与提示词修复（#42428）、响应模型 ID / 路由元数据透出（#42420、#26091）。
- **插件系统完善**：修复 `provider.models()` 钩子回归（#25630）、增加手动插件更新命令（#18544）、插件 package.json 与配置状态同步（#30526）、shell 钩子增加 sessionID（#40668）。
- **桌面与 IDE 集成**：VS Code Server 剪贴板问题（#41470）、WSL 模式下 MCP 命令封装（#42429）、桌面启动加载稳定性（#40516）、健康检查增加指数退避重试（#42422）。
- **V2/2.0 兼容性**：V1/V2 数据库共存与 schema 迁移保护（#42260）、V2 恢复 todowrite/todoread 工具（#42421）、Windows 子进程控制台闪烁（#42440）。

## 开发者关注点

- **回归问题密集**：多个报告直指版本回归，例如桌面应用 v1.18.5+ 启动加载失败、插件 `provider.models()` 回归、V2 迁移破坏 V1 数据库。开发者希望强化发布前回归测试。
- **安全审计明显升温**：同一研究者集中提交了升级脚本无完整性校验、SSRF、上下文修剪静默丢指令三份安全报告，且相关防护 PR 曾被关闭未合并，社区对安全问题解决流程有疑虑。
- **免费模型限流与滥用并存**：多个 429 FreeUsageLimitError 报告（#42029、#42074）与“通过 VPN 绕过免费模型限速”的漏洞报告（#34344）同时出现，说明配额策略既不够透明也容易被绕过。
- **跨环境体验是短板**：VS Code Server 剪贴板失效、Windows 控制台闪烁、WSL 下 MCP 执行失败等问题反复出现，远程/容器/Windows 场景需要更系统的适配与测试。
- **模型行为透明性需求上升**：AI SDK 返回的真实模型 ID、代理路由响应头等信息被丢弃，导致用户无法确认实际调用模型，也影响插件生态的监控和路由能力（#26091、#42420）。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi 社区动态日报 — 2026-08-14

## 今日速览

Pi 社区昨日迎来大幅活跃：长期存在的 **auto-compaction 触发失效问题**（#6879）获得 19 条评论成为最热 Issue，多条 TUI 终端遗留 bug（SIGINT 后 raw mode 未恢复、大会话重放刷屏、CJK 字符宽度错位）集中爆发并被快速修复。PR 方面，`frankieyep` 提交的 **TUI 终端卫生修复合并包**（#8082）直接解决了 3 个终端相关问题，是今日最值得关注的代码变更。此外，扩展系统、LLM 推理优化与多 Provider 适配是社区持续深耕的方向。

---

## 社区热点 Issues

### 1. auto-compaction 在 context 超限后直到 Provider 溢出才触发
**#6879** — 作者: alexanderkreidich | 评论: 19 | 👍: 17

**核心问题**：在一次 GPT-5.6 长时 agentic 回合中（持续 2 小时+），footer 显示 context 超过 100% 后 compaction 仍未触发，直到 API 在 373k tokens 处报错才被动压缩。

**为什么重要**：这是当前最严重的执行稳定性 bug——直接阻断超长 agentic 任务，且浪费大量 API 开销。19 条评论表明影响了较多用户，提议是在每个 agent 回合后检查压缩阈值而非仅依赖 Provider 溢出。社区讨论度最高的 Issue。

🔗 [earendil-works/pi Issue #6879](https://github.com/earendil-works/pi/issues/6879)

### 2. Edit 模糊匹配忽略空白差异
**#7836** — 作者: robjgray | 评论: 10 | 👍: 1

**核心问题**：`normalizeForFuzzyMatch` 未折叠连续空白或去除行首空白，导致文本内容相同但空白区域不一致时 Edit 的 oldText 匹配失败——小型模型尤受影响。

**为什么重要**：这直接降低了小模型在编辑任务上的成功率，是不需要改 LLM 侧就能显著改善编辑能力的纯工程修复点，已被标为 inprogress。

🔗 [earendil-works/pi Issue #7836](https://github.com/earendil-works/pi/issues/7836)

### 3. Prompt 编辑器大缓冲下移动光标极慢
**#8029** — 作者: affanali2k3 | 评论: 7 | 👍: 0

**核心问题**：prompt 输入框缓冲较大时（7000 行），单次按方向键耗时 1650ms，性能随行数线性恶化。

**为什么重要**：大上下文/长指令场景是 agent 使用常态，此性能瓶颈直接影响日常交互效率。作者已提交修复 PR #8066（视觉行缓存），社区正向验证中。

🔗 [earendil-works/pi Issue #8029](https://github.com/earendil-works/pi/issues/8029)

### 4. 流式 thinking 内容短暂闪现标题色
**#8060** — 作者: smithyyang | 评论: 3 | 👍: 0

**核心问题**：0.84.1 中，thinking 块流式输出时部分文本短暂变成主题中的 `mdHeading` 橘黄色，下一 chunk 到达后恢复正常。

**为什么重要**：渲染状态管理缺陷，视觉干扰虽短暂但复现稳定。属于 TUI 渲染层的边界条件问题，已被标记为 untriaged，值得关注。

🔗 [earendil-works/pi Issue #8060](https://github.com/earendil-works/pi/issues/8060)

### 5. TUI 对 Ambiguous-width 字符宽度计数错误致表格错位
**#8055** — 作者: Shallow-dusty | 评论: 3 | 👍: 0

**核心问题**：① ② ± × € 等 char 在 CJK 终端下占 2 列，但被 TUI 按 1 列计算，导致 markdown 表格/列表对齐崩坏。

**为什么重要**：大量 CJK 用户受影响，属于终端兼容性基础问题。修复需引入 wcwidth 类正确处理 ambiguous width，影响面广。

🔗 [earendil-works/pi Issue #8055](https://github.com/earendil-works/pi/issues/8055)

### 6. Codex 后端需处理 end_turn: false
**#7689** — 作者: mitsuhiko | 评论: 3 | 👍: 2

**核心问题**：Codex 后端可能返回 `response.completed` 且 `end_turn: false`，当前 Pi 未处理该语义，可能导致回合结束判定错误。

**为什么重要**：由知名开发者 mitsuhiko 提出，直接关系 Codex 后端任务循环正确性。社区关注度高（👍: 2），需要定义明确的回合管理协议。

🔗 [earendil-works/pi Issue #7689](https://github.com/earendil-works/pi/issues/7689)

### 7. Anthropic 服务端拒答需 fallback 机制
**#8017** — 作者: badlogic | 评论: 2 | 👍: 0

**核心问题**：当 Anthropic 分类器判定 Pi 操作"非法"时，compaction 会失败。作者引用官方文档，提议支持 server-side refusal fallback。

**为什么重要**：创始人 badlogic 亲自提出，这是企业场景下无法回避的可靠性问题，需要接入 Anthropic 的 refusal fallback API。

🔗 [earendil-works/pi Issue #8017](https://github.com/earendil-works/pi/issues/8017)

### 8. 未知斜杠命令（如 /exit）被静默发给模型
**#8081** — 作者: frankieyep | 评论: 1 | 👍: 0

**核心问题**：输错命令（如 /exit，肌肉记忆自 claude-code/codex）时，Pi 将未知斜杠命令当作正常聊天消息发给模型——浪费一次调用并污染 transcript。

**为什么重要**：交互设计缺陷，跨工具使用者的高频误操作场景。需发出警告而非静默发送，适合快速修复。

🔗 [earendil-works/pi Issue #8081](https://github.com/earendil-works/pi/issues/8081)

### 9. SIGINT 后 terminal 未恢复且窗口标题保留
**#8080** — 作者: frankieyep | 评论: 1 | 👍: 0

**核心问题**：交互模式下 `kill -INT` 后终端无回显、光标隐藏、bracketed paste 与 kitty 键盘协议未重置，需 `reset` 才能恢复。

**为什么重要**：终端卫生的基础 bug，直接影响用户 shell 体验。该用户在提交 Issue 的同时也提交了修复 PR #8082，效率极高。

🔗 [earendil-works/pi Issue #8080](https://github.com/earendil-works/pi/issues/8080)

### 10. 恢复大会话导致终端被历史刷屏
**#8079** — 作者: frankieyep | 评论: 1 | 👍: 0

**核心问题**：恢复 759 KB 会话时输出 844,716 字节（约 6300 行）到终端，耗时约 18 秒，退出后滚动缓存被污染。

**为什么重要**：大会话用户核心痛点。该问题已由 PR #8082 修复（仅渲染可见视口），此时 Issue 与 PR 联动的效率极高。

🔗 [earendil-works/pi Issue #8079](https://github.com/earendil-works/pi/issues/8079)

---

## 重要 PR 进展

### 1. fix(tui): 仅渲染可见视口；SIGINT 恢复终端
**#8082** — 作者: frankieyep | 状态: CLOSED

**核心内容**：一次性修复 3 个终端问题——(1) 恢复大会话时仅渲染视口内行而非全量重放，解决刷屏；(2) SIGINT 时恢复终端 raw mode/光标/标题；(3) 优化 fullRender 路径性能。

**为什么重要**：今日最关键的 PR，直接消除 #8079/#8080 两个高频痛点。已合入，显著改善大会话恢复与终端卫生。

🔗 [earendil-works/pi PR #8082](https://github.com/earendil-works/pi/pull/8082)

### 2. fix(ai): Gemini 工具 schema 兼容 fallback
**#8086** — 作者: d33disc | 状态: CLOSED

**核心内容**：当 generativelanguage 端点拒绝含 `parametersJsonSchema` 等 JSON Schema 字段的请求时，自动 fallback 到 legacy Gemini Schema 消息格式。

**为什么重要**：解决 Gemini 端点对 JSON Schema 字段的兼容性，避免 400 错误。对 Gemini 用户直接提升稳定性。

🔗 [earendil-works/pi PR #8086](https://github.com/earendil-works/pi/pull/8086)

### 3. fix(coding-agent): 布尔扩展 flags 不再吞掉 prompt
**#8084** — 作者: felixzsh | 状态: CLOSED

**核心内容**：布尔扩展 flag（如 --plan）在扩展未加载时误把下一参数当值吞掉；修复后 `pi -p --plan "prompt"` 可正常启动会话。

**为什么重要**：命令行参数解析的隐蔽 bug，修复后扩展 flag 与 CLI 交互行为符合直觉。

🔗 [earendil-works/pi PR #8084](https://github.com/earendil-works/pi/pull/8084)

### 4. feat(tui): Escape 可取消鼠标选择
**#8085** — 作者: pablasso | 状态: OPEN

**核心内容**：拖选文本中途按 Escape 可清除选择且不复制——对齐文本编辑器标准行为。

**为什么重要**：TUI 交互细节提升，避免误选导致的剪贴板污染。易用性改进，社区反馈积极。

🔗 [earendil-works/pi PR #8085](https://github.com/earendil-works/pi/pull/8085)

### 5. fix(tui): 视觉行缓存避免重复计算
**#8066** — 作者: affanali2k3 | 状态: OPEN

**核心内容**：缓存视觉行计算结果，仅在宽度或文本变化时失效，解决 #8029 中 7000 行缓冲下单次按键 1650ms 的延迟。

**为什么重要**：针对 prompt 编辑器性能瓶颈的根治方案，引入 VisualLine 类型统一多处内联类型，是性能与代码质量双提升。

🔗 [earendil-works/pi PR #8066](https://github.com/earendil-works/pi/pull/8066)

### 6. fix(coding-agent): 校验扩展 flag 默认值
**#8070** — 作者: acmerfight | 状态: OPEN

**核心内容**：`registerFlag()` 的 type 与 default 不一致时（如 boolean flag 配字符串 default）不会产生 truthy 字符串；同时拒绝不支持的 flag 类型。模型化为 discriminated union。

**为什么重要**：扩展 API 类型安全的前置保障，防坑设计。对扩展开发者友好。

🔗 [earendil-works/pi PR #8070](https://github.com/earendil-works/pi/pull/8070)

### 7. fix(examples): todo renderResult 校验失败时返回 undefined 至 TUI 崩溃
**#8057** — 作者: cyzlmh | 状态: OPEN

**核心内容**：todo 工具 schema 校验失败时 `details` 为 `{}`（真值空对象），renderResult 中 `switch(details.action)` 无 default 返回 undefined，导致交互模式 TUI 崩溃。修复为正确返回。

**为什么重要**：暴露了 MCP/扩展工具 renderResult 的容错不足，对工具开发者的错误处理有参考意义。

🔗 [earendil-works/pi PR #8057](https://github.com/earendil-works/pi/pull/8057)

### 8. fix(coding-agent): grok-mermaid 升级至 0.2.3
**#7984** — 作者: xl0 | 状态: OPEN

**核心内容**：升级 grok-mermaid 依赖（解决 #7832），mermaid 图表渲染效果显著改善（附 before/after 截图对比）。

**为什么重要**：mermaid 渲染是代码文档场景常用能力，视觉质量直接关系用户体验。若合入将提升图表呈现准确性。

🔗 [earendil-works/pi PR #7984](https://github.com/earendil-works/pi/pull/7984)

### 9. feat: 新增 Amazon Bedrock Mantle OpenAI Responses Provider
**#6216** — 作者: unexge | 状态: OPEN

**核心内容**：为 Amazon Bedrock Mantle 增加基于 OpenAI Responses API 的 Provider 支持，基于 openai-node 的 Bedrock Provider 实现。

**为什么重要**：扩展 Pi 对云厂商模型的接入面，AWS 用户可直接使用 Bedrock Mantle 托管模型。长时间未合入，可能等待架构决策。

🔗 [earendil-works/pi PR #6216](https://github.com/earendil-works/pi/pull/6216)

### 10. 用户可见消息统一使用 APP_NAME
**#8067** — 作者: mellson | 状态: CLOSED

**核心内容**：将若干硬编码字符串替换为 APP_NAME，使 rebrand 后的 Pi 衍生版本在用户界面中显示正确名称（Pi 本体输出不变）。

**为什么重要**：对基于 Pi 二次开发的团队友好，避免衍生版本 UI 露出"pi"字样。小改动大价值。

🔗 [earendil-works/pi PR #8067](https://github.com/earendil-works/pi/pull/8067)

---

## 功能需求趋势

### 1. Context / Compaction 管理机制升级
- **#6879**（auto-compaction 不触发）与 **#8017**（Anthropic 拒绝回退）共同指向同一方向：上下文管理需从"Provider 报错后补救"转向"主动预测 + 多级 fallback"。
- **#7993**（PR 尝试 tool turn 间压缩，作者声明误操作关闭）说明社区已在探索更细粒度的压缩时机。

### 2. TUI 终端体验与兼容性修复
- **#8055**（CJK 宽度）、**#7761**（VTE 剪贴板）、**#8080**（SIGINT 恢复）、**#8079**（会话重放）——终端卫生与显示兼容性成高频主题。
- 趋势：不同终端（VTE、kitty、CJK）的差异化行为需要系统化测试与适配。

### 3. 扩展系统能力精细化
- **#7607**（per-tool 跳过参数校验）、**#7092**（最终不可变执行前 hook）、**#8078**（扩展元数据解析顺序）、**#8070/#8084**（flag 类型安全）——扩展 API 从"能用"走向"可控、可审计、类型安全"。

### 4. 多 Provider 适配与生态对接
- **#8075**（Kimi 缓存 token 统计）、**#7689**（Codex end_turn）、**#8086**（Gemini schema）、**#6216**（Bedrock Mantle）——社区持续为各家模型/平台做适配，Provider 兼容层是关注核心。

### 5. 会话恢复体验优化
- **#8079**（恢复刷屏）、**#7960**（进度计数不一致）——会话恢复的可视化反馈与资源开销被明确提出，PR #8082 已先行解决一部分。

### 6. 特殊内容呈现支持
- **#8041**（HTML 导出支持 mermaid/LaTeX）——随着 agent 编写文档场景增多，导出能力需与 TUI 渲染能力对齐。

---

## 开发者关注点

- **上下文管理可靠性**：compaction 触发机制不可靠会造成真实经济损失和时间浪费；开发者期待"到达阈值即压缩"的确定性行为，而非依赖 Provider 的 overflow 错误。
- **终端卫生与恢复**：SIGINT/退出后 terminal raw mode 残留、剪贴板失效、CJK 终端显示错位是 TUI 用户高频吐槽点；不过社区反馈"问题提出 - 修复 PR 落地"的周期正在缩短（#8080/#8082 当天完成闭环）。
- **会话恢复的性能与体验**：大会话恢复产生大量不必要终端输出，非可见区渲染是纯浪费；视口渲染改进方向获得认可。
- **扩展 API 的类型安全与状态管理**：flag 类型歧义、hook 时机不明确、renderResult 容错缺失等细节问题频繁出现，开发者期待更严格的类型约束与更清晰的扩展生命周期。
- **Windows 支持仍有缺口**：Unix socket 绑定失败（#8047）、settings.json 解析报错（#7829）等问题虽非高频，但影响特定用户群。
- **模型推理的 token 计费准确性与兼容性**：Kimi 缓存 token 统计（#8075）、Gemini 工具 schema 兼容（#8086）反映开发者对"用对模型、花对钱"的精细化诉求。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报 — 2026-08-14

## 今日速览

昨日发布 v0.21.11 与 v0.21.12-preview.1：前者引入 Agent Plugins v1 和 `/coordinate` 原生多代理工作流，后者修复 Web Shell 会话保留并新增工作区文件上传。SWE-bench Verified E2E 验证被标记为 **QUARANTINED**（500/500 完成、0 解决），需关注是否存在回归或配置问题。社区讨论集中在 Windows 平台兼容性、Vertex AI 认证失败及工具调用可靠性上。

---

## 版本发布

### v0.21.12-preview.1
- **fix(web-shell)**：保留独立会话目标（[#9038](https://github.com/QwenLM/qwen-code/pull/9038)）
- **feat(web-shell)**：支持工作区文件上传

### v0.21.11
- **Agent Plugins v1**：支持插件机制扩展 Agent 能力（[#8834](https://github.com/QwenLM/qwen-code/pull/8834)）
- **`/coordinate` 命令**：启用原生多代理工作流，支持只读 teammate（[#8804](https://github.com/QwenLM/qwen-code/pull/8804)）
- 弱化工具错误展示效果（weaken-tool-error）
- **SWE-bench Verified**：E2E 验证状态 QUARANTINED，500/500 全部未解决

---

## 社区热点 Issues

1. **RFC: Native coordination for independent Qwen sessions**（[#8718](https://github.com/QwenLM/qwen-code/issues/8718)）— 9 评论  
   多代理协调顶层设计，讨论领导者如何派发多个独立 worker 并保持交互，是 Fleet 架构的 umbrella issue，已关闭。

2. **fix(serve): 大会话恢复超时保护**（[#8678](https://github.com/QwenLM/qwen-code/issues/8678)）— 8 评论，P1  
   恢复超时可能导致当前会话丢失。PR1（[#8691](https://github.com/QwenLM/qwen-code/pull/8691)）已合并，实现超时契约与可观测性。

3. **Windows standalone 安装时 Get-FileHash 失败**（[#7118](https://github.com/QwenLM/qwen-code/issues/7118)）— 7 评论，👍3  
   安装程序无法解析 Get-FileHash 导致 SHA-256 校验失败。已有对应修复 PR [#9112](https://github.com/QwenLM/qwen-code/pull/9112)。

4. **Windows CLI 中 Ctrl+V 粘贴完全无响应**（[#9061](https://github.com/QwenLM/qwen-code/issues/9061)）— 3 评论，P1  
   0.21.x 回归 bug，降级到 0.21.0 可恢复，影响 Windows 用户日常输入效率。

5. **Gemini 2.5 在 Vertex AI 上完全不可用**（[#9019](https://github.com/QwenLM/qwen-code/issues/9019)）— 5 评论  
   每次请求都因 `thinking_level` 参数不被模型支持而 400，发生在任何工具调用或流式输出之前。

6. **Keyless Vertex AI 无法从环境推断认证类型**（[#9025](https://github.com/QwenLM/qwen-code/issues/9025)）— 5 评论  
   纯环境变量的 keyless 配置不会自动选择 `vertex-ai` auth，导致无头运行启动即退出。

7. **Python SDK 拒绝 `permission_mode="auto"`**（[#9002](https://github.com/QwenLM/qwen-code/issues/9002)）— 5 评论  
   客户端侧校验先于 CLI 执行，直接拒绝 CLI 已支持的 `auto` 值，SDK/CLI 行为不一致。

8. **`read_file` 仅凭 `.png` 扩展名把非图片发送给模型 API**（[#9088](https://github.com/QwenLM/qwen-code/issues/9088)）— 3 评论  
   外部工具写入的 JSON 内容被当作 PNG 传给模型，原始 400 错误直接中止整个 turn。

9. **后台 Agent 恢复与 activeWork 追踪**（[#8586](https://github.com/QwenLM/qwen-code/issues/8586)）— 4 评论  
   提议为 daemon 增加深度健康检查和后台 Agent 恢复路径，属于 background automation roadmap。

10. **Desktop 外链仍静默失效，MCP OAuth 无法完成**（[#9108](https://github.com/QwenLM/qwen-code/issues/9108)）— 3 评论  
   Web Shell 中四类外部链接仍走不可靠的隐式新窗口路径，MCP OAuth 流程被阻塞。

---

## 重要 PR 进展

1. **fix(install): 避免 Get-FileHash，改用 .NET SHA-256**（[#9112](https://github.com/QwenLM/qwen-code/pull/9112)）  
   内联流式校验替代 PowerShell 外部命令，修复 Windows standalone 安装失败问题（对应 #7118）。

2. **feat(core): live-session registry + `qwen sessions ps`**（[#8969](https://github.com/QwenLM/qwen-code/pull/8969)）  
   运行中的会话注册到统一目录，退出时移除，可直接回答"本机有哪些 Qwen Code 会话在运行"。

3. **feat(mcp): MCP 2026 core 与 WebShell Apps host**（[#8992](https://github.com/QwenLM/qwen-code/pull/8992)）  
   首个 MCP 2026 客户端切片，支持 Apps 扩展、`ui://` 工具元数据保留，以及 HTML 资源校验。

4. **feat(core): 拒绝上游 fail-fast 占位符响应**（[#8938](https://github.com/QwenLM/qwen-code/pull/8938)）  
   防御上游模型接口返回 HTTP 200 但正文为 "(request timed out)" 等占位符的情况，避免静默污染对话。

5. **fix(desktop): 剩余外链统一走 shell opener**（[#9111](https://github.com/QwenLM/qwen-code/pull/9111)）  
   修复桌面 WebView 中四类链接受阻问题，与 #9108 对应，MCP OAuth 可正常完成。

6. **feat(cli): 通过 settings 键启用动态工作流**（[#9098](https://github.com/QwenLM/qwen-code/pull/9098)）  
   新增 `tools.workflowsEnabled` 开关，取代此前未文档化的环境变量。

7. **feat: 将 Local Control 统一到 daemon 单实现**（[#9106](https://github.com/QwenLM/qwen-code/pull/9106)）  
   手机接入 daemon 的 LAN 配对流程原有两套实现、两套安全模型，现收敛为 daemon-owned 单一实现。

8. **feat(daemon): 跨工作树 Git 变更防护**（[#8687](https://github.com/QwenLM/qwen-code/pull/8687)）  
   识别 `-C` / `--work-tree` / `--git-dir`，阻止模型通过 `run_shell_command` 逃逸 session 目录执行 Git 变更。

9. **fix(hooks): 关闭四个信任边界漏洞**（[#8396](https://github.com/QwenLM/qwen-code/pull/8396)）  
   HTTP hooks 不再跟随重定向，修复 URL 白名单与 DNS 级 SSRF 检查绕过等问题。

10. **feat(core): 隐私安全的工具结果边界诊断**（[#9039](https://github.com/QwenLM/qwen-code/pull/9039)）  
    在工具结果边界增加不含用户内容的诊断信息，便于排查工具调用异常。

---

## 功能需求趋势

- **原生多代理与 Fleet 架构**：`/coordinate`、teammate runtime、fleet stages 1A/1B/2/3 密集推进，多会话协调是当前 roadmap 的核心方向。
- **Web Shell 能力扩展**：工作区文件上传、Channel 策略管理、MCP 2026 集成、桌面端外链与 OAuth 完善。
- **后台自动化与守护进程健康**：activeWork 追踪、live-session registry、会话恢复超时保护，持续强化 daemon 的可观测性。
- **云服务认证与兼容性**：Vertex AI keyless 自动推断、Gemini 模型参数差异化处理，第三方云接入是高频诉求。
- **OMNI 多模态实验**：Policy 链路打通、Memory 召回与治理收尾（S4a~S6）并行推进，实验分支活跃。

---

## 开发者关注点

- **Windows 平台体验集中告急**：安装校验失败（#7118）、Ctrl+V 粘贴回归（#9061）、桌面端启动弹出终端窗口（#9043），Windows 用户受影响面较大。
- **无头/非交互模式稳定性**：`NO_TOOL_RESULT_PROGRESS` 硬失败（#9026）、keyless 认证不被自动推断（#9025），自动化场景需要更可靠的默认行为。
- **工具调用可靠性**：`record_artifact` 未验证 workspacePath 就返回成功（#9083）、`read_file` 仅凭扩展名判断图片类型（#9088），工具结果的可信度直接影响模型行为。
- **大上下文恢复与压缩**：会话恢复超时（#8678）、压缩 side-query 固定 `maxOutputTokens` 超出小窗口上下文导致 400（#7960），长会话场景仍是痛点。
- **供应链安全**：npm audit 报告 2 个高危漏洞（#8944）、hook 信任边界、CI 权限与 CODEOWNERS 卫生（#9008），安全加固需求持续上升。

---

> 数据来源：GitHub QwenLM/qwen-code Issues / Pull Requests，统计窗口 2026-08-13 ~ 2026-08-14。

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报 — 2026-08-14

> 数据来源：`Hmbown/CodeWhale`（原 DeepSeek-TUI）

---

## 1. 今日速览

- **v0.9.7 正式发布**，项目全面切换至 **Codewhale** 品牌，旧 npm 包 `deepseek-tui` 正式弃用，社区同步开启 v0.9.8 Auto-Review 双层架构开发。
- 核心贡献者 `Lstarsky0` 连续提交 4+ PR，集中解决测试隔离、schema 过度复杂化与 Auto-Review 拒因透传问题，工程治理显著提速。
- 40+ 个 Issue 在过去 24 小时持续活跃，**多 Agent 大型任务稳定性**与**中文输入法/国际化**仍是社区最高频痛点。

---

## 2. 版本发布

### v0.9.7
- **Codewhale** 正式成为 Shannon Labs 公开产品名称；`codewhale` 命令 / npm 包 / release 资产统一为小写技术标识。
- 旧 npm 包 `deepseek-tui` 弃用，不再接收任何新版本；v0.8.x 用户需按新名称迁移。
- 此前社区 PR #5318（终端窗口画中画）已由维护者以 #5333 完成 Harvest，并计划为 v0.9.7 纳入的社区贡献集成。

---

## 3. 社区热点 Issues

### 🔥 讨论度 / 关注度领先

- **[#998] 文案展示不全**（💬11 · 👍1）
  UI 文案在 TUI 界面中被截断，用户希望鼠标悬停时能展示完整提示。创建已 3 个月仍开放，反映 UI 细节打磨长期受社区关注。
  https://github.com/Hmbown/CodeWhale/issues/998

- **[#1004] 请求 `/dryrun` 预演功能**（💬9）
  在长上下文（长 system prompt、大量缓存文件、多步 thinking）中，开发者看不到“即将发送给模型的请求实际是什么”。社区希望增加 `/dryrun` 命令预览完整请求而不实际发送，对大模型调试有实际刚需。
  https://github.com/Hmbown/CodeWhale/issues/1004

- **[#5324] 简化 agent tool 的 32 字段 schema**（💬7）
  模型侧 `agent` 工具套着 32 属性 JSON schema 且零 required 字段，承担 8 种 action，导致模型频繁出错。已触发 #5369 等后续修复 PR，是当前架构治理的核心议题之一。
  https://github.com/Hmbown/CodeWhale/issues/5324

- **[#2369] 配置路径跨 OS / Cygwin 碎片化 + 静默迁移 Bug**（💬7）
  Windows 与 Cygwin 下配置/密钥路径解析规则不一致，旧版本迁移可能静默失败。跨平台可靠性问题，二次触发概率高。
  https://github.com/Hmbown/CodeWhale/issues/2369

- **[#894] 执行过程中图片显示混乱**（💬6）
  多模态内容渲染 Bug，图片在 TUI 中错乱，影响可视化调试。
  https://github.com/Hmbown/CodeWhale/issues/894

- **[#1425] 大文本处理会话中断卡死**（💬6）
  用户用 DeepSeek TUI 分析 300 万字小说时，AI 切分 10 个子 Agent 并行处理，但 `agent_wait` 超时导致会话中断。多 Agent 并发可靠性问题，社区关注度高。
  https://github.com/Hmbown/CodeWhale/issues/1425

- **[#1482] NVIDIA NIM 集成不工作**（💬6）
  调用 NIM 本地推理接口返回 `404 page not found`，影响私有化部署用户。
  https://github.com/Hmbown/CodeWhale/issues/1482

- **[#1732] 合并分析报告保存文档巨慢**（💬6）
  保存报告时缓存命中率极低、过程缓慢，长会话场景下性能损耗严重。
  https://github.com/Hmbown/CodeWhale/issues/1732

- **[#5316] EPIC-005：TUI Crate 拆解（伞形追踪）**（💬5）
  架构级重构追踪 Issue，标志 CodeWhale TUI 正从单体 crate 走向模块化拆分，涉及全部子 EPIC / FEAT / PR 汇总。
  https://github.com/Hmbown/CodeWhale/issues/5316

- **[#1651] VS Code 在 YOLO Agent 运行测试脚本时崩溃**（💬5）
  YOLO Agent 自主执行测试脚本导致 VS Code 直接退出，IDE 集成稳定性受质疑。
  https://github.com/Hmbown/CodeWhale/issues/1651

---

## 4. 重要 PR 进展

- **[#5368] fix(tui): 将无隔离测试约束到独立状态根**
  修复 #5359 的 4 个测试：解析了锁持有线程可读真实 `~/.codewhale` 的信任漏洞，并引入显示探测隔离。CI 绿但真机红的问题终于有解。
  https://github.com/Hmbown/CodeWhale/pull/5368

- **[#5369] fix(tools): Moonshot schema 降级替代条件拒绝**
  响应 #5324 的预处理。Moonshot 模型无法处理复杂条件 schema 时，自动降级而非拒绝，受严格 schema 约束的模型报错率有望下降。
  https://github.com/Hmbown/CodeWhale/pull/5369

- **[#5358] feat(engine): 自动审核拒绝理由 + 回合熔断**
  此前 Auto-Review 拒绝以裸 `permission_denied` 返回，模型只会换句话重试同一操作直至步数耗尽。现已将拒绝理由随 Block 决策透出，并加入熔断机制。
  https://github.com/Hmbown/CodeWhale/pull/5358

- **[#5364] feat(tui): Markdown 引用块渲染优化**
  TUI 会话中将 `>` 引用块渲染为真正的引用栏，支持嵌套、行内格式、自动换行，并保持选中/复制行为正确。纯前端体验改进。
  https://github.com/Hmbown/CodeWhale/pull/5364

- **[#5365] feat(provider): 本地 DS4 一等公民式配置**
  新增 `/setup provider ds4` 一键预填的无密钥 loopback 预设，无需新增协议适配器即可将本机 DwarfStar (DS4) 作为本地 DeepSeek 路由。
  https://github.com/Hmbown/CodeWhale/pull/5365

- **[#5339] fix(engine): 过滤子 shell owned 的补全事件**
  子进程后台 shell 补全事件不再混入父模型流，保留未归属补全与任务/状态可见性。修复 #5325 并附带回归测试。
  https://github.com/Hmbown/CodeWhale/pull/5339

- **[#5353] feat(tui): Auto-Review 双层模式（v0.9.8）**
  确定性规则层保持不可绕过，fallback 受阻时升级为一次性模型守护层（guardian），而非静默阻断。集成 Codex `auto_review` 语义与 Kimi 模式词汇。
  https://github.com/Hmbown/CodeWhale/pull/5353

- **[#5333] feat(tui): 主机终端窗口支持画中画固定置顶**（Harvest）
  签收社区 PR #5318：Windows 下 `/pin` 命令可将终端收缩为 640×400 且置顶的迷你窗口，再次触发恢复原尺寸/最大化状态。修复了原 PR 的 CI 失败与跨平台环境不稳定性。
  https://github.com/Hmbown/CodeWhale/pull/5333

- **[#5336] fix(mcp): 无更多分页时省略 nextCursor**
  修复 `tools/list` 与 `resources/list` 返回 `"nextCursor": null` 导致 Claude Code 等严格 MCP 客户端拒绝响应的问题。已合并。
  https://github.com/Hmbown/CodeWhale/pull/5336

- **[#5334] docs(i18n): 移除过时的繁中 partial-pack 声明**
  早前 #5143 已使 `zh-Hant.json` 与 `en.json` 完全对齐，但仍有 5 处界面声明繁中为“部分翻译”，含 2 处用户可见字符串。已清理。
  https://github.com/Hmbown/CodeWhale/pull/5334

---

## 5. 功能需求趋势

| 方向 | 代表性 Issue/PR | 热度 |
|---|---|---|
| **新推理服务支持** | #1482 NIM 集成、#5365 本地 DS4、#5369 Moonshot schema | 高 |
| **多 Agent 并发可靠性** | #1425 子 Agent 超时卡死、#5324 agent schema 简化 | 高 |
| **跨会话记忆** | #2492 重启后遗忘记忆 | 高 |
| **国际化 / 中文支持** | #790 i18n 覆盖、#2323 中文输入法、#1675 乱码 | 高 |
| **终端 UI / 可访问性** | #998 文案不全、#5364 引用渲染、#5333 画中画 | 中 |
| **IDE 集成稳定** | #1651 VS Code 崩溃 | 中 |
| **配置标准化** | #2369 路径碎片化、#436 可配置 keymap | 中 |
| **远程工作台** | #1984 CNB/Lighthouse/Feishu 一体化、#1990 US-first 云栈 | 中 |

---

## 6. 开发者关注点

- **稳定性第一**：300 万字文本会话中断、VS Code 崩溃、SSH 出站被沙箱阻断（#1829）等问题高频出现，多 Agent 场景尤其脆弱。
- **中文支持是硬需求**：#2323 中文输入法下模态框按键错乱、#1675 Agent 输出乱码、#998 文案截断，直接影响国内用户体验，且多个 Issue 长期 open。
- **记忆持久化缺位**：#2492 明确抱怨“重启失忆”“写入记忆不读取”，在长会话工作流中是关键体验短板。
- **配置与迁移信任度低**：#2369 揭示 Windows / Cygwin 路径规则分歧加静默迁移失败；#5340 还报告 `doctor` 升级后一直显示 `needs action`，凭证配置信任度受损。
- **主动预演与可视化诉求**：#1004 `/dryrun` 预演完整请求、#2342 点击输出文件打开预览、#1754 由 AI 自动选择 shell/lang，说明开发者希望模型行为更可预见、可干预。

---

*本日报基于 GitHub 公开数据自动聚合生成，仅供参考。数据快照时间为 2026-08-13，仓库 Hmbown/CodeWhale。*

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

过去24小时无活动。

</details>

---
*本日报由 [agents-radar](https://github.com/Neare-Design/agents-radar) 自动生成。*