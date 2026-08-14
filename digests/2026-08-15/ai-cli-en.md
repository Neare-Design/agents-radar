# AI CLI Tools Community Digest 2026-08-15

> Generated: 2026-08-14 23:14 UTC | Tools covered: 10

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

## Cross-Tool Comparison

# Cross-Tool AI CLI Comparison Report — 2026-08-15

## 1. Ecosystem Overview

The AI CLI ecosystem is moving quickly: Claude Code shipped two stable releases, Qwen Code tagged eight releases, and OpenAI Codex published five alpha builds in the past 24 hours. Community attention, however, is concentrated less on new features and more on reliability — Windows performance regressions, agent hangs, false success reports, context/token waste, and MCP/provider interoperability dominate the discussions. Maintainers are responding with rapid patch PRs, but the gap between feature velocity and platform hardening remains visible. Grok Build is the only tracked tool with no activity, while Kimi Code CLI is release-dormant but still accumulating user demand around persistent memory.

## 2. Activity Comparison

Counts reflect issues and PRs highlighted in each community digest, not total repository activity.

| Tool | Tracked hot issues | Active PRs | Releases |
|---|---:|---:|---|
| Claude Code | 10 | 4 | 2 (v2.1.232, v2.1.233) |
| OpenAI Codex | 10 | 10 | 5 alphas (rust-v0.148.0-alpha.14–18) |
| Gemini CLI | 10 | 16 (10 highlighted + 6 SSR-agent PRs) | 1 nightly |
| GitHub Copilot CLI | 10 | 3 | 2 (v1.0.80, v1.0.80-1) |
| Kimi Code CLI | 4 | 0 | 0 |
| OpenCode | 10 | 10 | 0 |
| Pi | 10 | 12 (10 highlighted + 2 notable) | 1 (v0.84.2) |
| Qwen Code | 10 | 10 | 8 (stable, previews, nightly, e2e tags) |
| DeepSeek TUI / Codewhale | 10 | 17 (10 highlighted + 7 merged) | 1 (v0.9.8) |
| Grok Build | 0 | 0 | 0 |

## 3. Shared Feature Directions

**Session persistence and lifecycle continuity**  
Users across Claude Code (unarchive sessions), Kimi Code CLI (#1283, #1478), OpenCode (#42608, #38791), Pi (#7724), Qwen Code (#8678), and Codewhale (#5382) want durable, restorable, and portable sessions. Cross-device handoff, reliable restore, and no-silent-data-loss are common requirements.

**Context/memory/token economics**  
This is a broad cross-tool theme: Claude Code (image-processing token burn), OpenAI Codex (compaction loses continuity), Gemini CLI (AST-aware file reads), Copilot CLI (BYOK prompt-cache breakage), OpenCode (context cache invalidation), Pi (append compaction, cached-token accounting), Qwen Code (unbounded history), and Codewhale (`/dryrun` request preview) all point to a shared demand for cheaper, more transparent context handling.

**Subagent/agent reliability**  
Gemini CLI (#22323 false `GOAL` success), Copilot CLI (#4306 frozen subtasks), OpenCode (#42605 unresponsive sessions), and Claude Code’s new subagent-forking default show that subagent orchestration is moving from novelty to critical-path infrastructure. Truthful completion reporting and hang-free execution are now baseline expectations.

**MCP/provider interoperability hardening**  
Claude Code (MCP timeouts capped at 60s), Copilot CLI (Atlassian/GitLab MCP OAuth regressions, pagination), OpenCode (GLM/Kimi tool-call translation bugs), Pi (Kimi endpoint detection, SiliconFlow), and Qwen Code (SDK validation inconsistency) all reveal that provider/MCP compatibility is the largest integration risk across the ecosystem.

**Windows/WSL support is now a first-class concern**  
OpenAI Codex has multiple Windows desktop regressions (stutter, WMI exhaustion, CPU busy loops), Pi has WSL login hangs, Claude Code has a Git Bash permission-prompt regression, Gemini CLI has Windows `grep_search` failures, and Qwen Code is investing in Web Shell. Cross-platform reliability is no longer an afterthought.

## 4. Differentiation Analysis

| Tool | Primary focus | Target users | Technical approach |
|---|---|---|---|
| **Claude Code** | Enterprise-ready agentic coding with desktop/CLI/IDE surface | Claude subscribers, enterprise teams | Proprietary subagents, apps gateway, identity forwarding, GitLab MR integration |
| **OpenAI Codex** | ChatGPT-integrated coding agent, Windows desktop experience | ChatGPT/Codex users, broad consumer/prosumer base | Rust core, sandbox enforcement, gRPC protocol, rapid alpha releases |
| **Gemini CLI** | Agent reliability and orchestration | Gemini users, Google-ecosystem developers | Nightly releases, automated SSR-agent PRs, PTY/browser-agent fixes, skills/subagents |
| **GitHub Copilot CLI** | GitHub-native automation and enterprise governance | GitHub Copilot Business/Enterprise users | Tight coupling to Copilot model catalogue, autopilot workflows, MCP OAuth |
| **Kimi Code CLI** | Persistent memory and cross-device session continuity | Moonshot/Kimi users | Low release cadence, community-driven feature roadmap |
| **OpenCode** | Provider-agnostic multi-model CLI | Polyglot provider users, local-LLM users | Universal relay, provider-specific translation fixes, critical session-ID architecture |
| **Pi** | Power-user TUI with broad provider coverage | TUI enthusiasts, multi-provider users | Extension loading, provider adapters, append compaction, host-clipboard correctness |
| **Qwen Code** | Web Shell + daemon-based architecture, platform integrations | Qwen users, enterprise automation users | ACP protocol, session media refs, channel integrations (DingTalk), resource governance |
| **DeepSeek TUI / Codewhale** | Lightweight DeepSeek-first TUI, rebranding to Codewhale | DeepSeek API users, TUI-focused developers | Rust TUI, provider templates, agent-tool schema simplification, local DS4 setup |
| **Grok Build** | No activity | — | — |

## 5. Community Momentum & Maturity

**Claude Code** remains the most mature and highest-engagement community: 73 comments on the image-token issue and 147👍 on the Enter-key request demonstrate both scale and product-level attachment. Its ecosystem is broadening into enterprise governance, but the volume of open complaints is also growing.

**OpenAI Codex** has the loudest Windows pain of any tool, with 101 comments on the Windows freeze issue alone. The five-alpha-per-day cadence and 10 active PRs show an aggressive release machine, but stability regressions are clearly straining user trust.

**Gemini CLI** has excellent fix velocity — the SSR-agent PR system is landing many small reliability fixes rapidly. The community is smaller than Claude/Codex but well focused on agent correctness, subagent behavior, and PTY/resource leaks.

**GitHub Copilot CLI** is less PR-active but faces significant enterprise-facing issues: model catalogue confusion, MCP OAuth regressions, and autopilot stability. The community appears less vocal but strongly affected by governance and platform-policy gaps.

**OpenCode** is active at the PR level but shipped no release in 24h. A critical session-wedging bug (#42608) is the center of attention. Its multi-provider positioning attracts users who accept some rough edges in exchange for model flexibility.

**Pi** is rapidly iterating with strong feature momentum (new providers, compaction improvements, clipboard fixes) but remains visibly limited by Windows/WSL and TUI performance gaps. The maintainer’s open call for Windows feedback (#7547) suggests deliberate community-driven prioritization.

**Qwen Code** is one of the most release-active tools, with a broad surface: Web Shell, daemon governance, session media, and platform integrations. Its community is concerned about regression quality, especially image-load crashes and headless-mode false failures.

**DeepSeek TUI / Codewhale** is rebranding and shipping actively, with 17 PRs in the digest and a new v0.9.8 release. CI instability and rendering regressions indicate a project still maturing, but the feature set is expanding quickly.

**Kimi Code CLI** and **Grok Build** are the laggards. Kimi has high user desire but no release/PR activity; Grok Build has none.

## 6. Trend Signals

**Reliability is the new differentiator.** The most upvoted and active issues are not feature requests — they are false success reports, infinite hangs, silent session wedging, and resource leaks. Developers evaluating AI CLI tools should weight regression history and issue-resolution speed more heavily than feature lists.

**Windows/WSL support is a market opportunity.** The strongest single-tool complaint cluster is Windows desktop performance (Codex), followed by WSL login issues (Pi) and Git Bash regressions (Claude). Teams building on these tools should expect cross-platform caveats; tool vendors that solve Windows/WSL well will have a clear adoption advantage.

**Context and token transparency are becoming table stakes.** Users want to know what is sent to the model, why tokens are consumed, and how to preserve prompt caches. Features like `/dryrun`, AST-aware file reads, and byte-for-byte transcript reuse indicate a shift from “smartness” to “efficiency and auditability.”

**MCP and provider compatibility remain fragile.** OAuth regressions, timeout caps, pagination gaps, and provider-specific translation bugs are common across Copilot, Claude, OpenCode, and Pi. The ecosystem is still waiting for standardized MCP behaviour and better provider conformance layers.

**Enterprise governance is rising.** Claude Code’s identity forwarding and analytics API gaps, Copilot’s model-catalogue inconsistencies, and Qwen’s daemon resource governance all point to increasing enterprise deployment. Admin controls, usage accuracy, and policy enforcement are growing requirements.

**For developers and decision-makers:** investing in cross-platform reliability, transparent context/cost accounting, and honest agent completion reporting is likely to yield more long-term value than chasing the next model-specific feature.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights
*Data: github.com/anthropics/skills · 2026-08-15*

## 1. Top Skills Ranking

The most-attended PRs in the snapshot, ordered by comment activity. All are currently open.

1. **[#1298 — skill-creator eval reliability fix](https://github.com/anthropics/skills/pull/1298)**  
   Fixes `run_eval.py` always reporting `recall=0%`, which was breaking the description-optimization loop. Also addresses Windows subprocess/stream reading, trigger detection, and parallel workers.  
   **Status:** Open

2. **[#514 — document-typography skill](https://github.com/anthropics/skills/pull/514)**  
   Adds typographic quality control for AI-generated documents: orphan-word wrapping, widow paragraph headers, and numbering misalignment.  
   **Status:** Open

3. **[#538 — pdf skill case-sensitivity fix](https://github.com/anthropics/skills/pull/538)**  
   Corrects 8 case mismatches in `SKILL.md` references, making `reference.md` and `forms.md` resolve correctly on case-sensitive filesystems.  
   **Status:** Open

4. **[#486 — ODT skill](https://github.com/anthropics/skills/pull/486)**  
   New skill for OpenDocument workflows: creating, filling, reading, and converting `.odt`/`.ods` files, including ODT-to-HTML.  
   **Status:** Open

5. **[#210 — frontend-design skill clarity pass](https://github.com/anthropics/skills/pull/210)**  
   Revises the frontend-design skill for clarity, actionability, and internal coherence so Claude can execute every instruction within a single conversation.  
   **Status:** Open

6. **[#83 — skill-quality-analyzer + skill-security-analyzer](https://github.com/anthropics/skills/pull/83)**  
   Two meta-skills: one evaluates skill quality across structure, documentation, examples, and resources; the other assesses security.  
   **Status:** Open

7. **[#541 — docx tracked-change ID collision fix](https://github.com/anthropics/skills/pull/541)**  
   Prevents document corruption when tracked changes conflict with existing bookmark `w:id` values in OOXML.  
   **Status:** Open

8. **[#539 — skill-creator YAML validation](https://github.com/anthropics/skills/pull/539)**  
   Adds pre-parse validation to catch unquoted `description` fields containing YAML special characters, preventing silent truncation.  
   **Status:** Open

## 2. Community Demand Trends

From Issues, the strongest demand areas are:

- **Skill security and trust boundaries**  
  [#492](https://github.com/anthropics/skills/issues/492) highlights impersonation risk from community skills under the `anthropic/` namespace. [#1175](https://github.com/anthropics/skills/issues/1175) raises security and context-window concerns for SharePoint Online handling.

- **Skill lifecycle and organizational sharing**  
  [#228](https://github.com/anthropics/skills/issues/228) requests org-wide sharing. [#62](https://github.com/anthropics/skills/issues/62) reports disappearing skills. [#189](https://github.com/anthropics/skills/issues/189) reports duplicate skills from overlapping plugins.

- **Reliable skill-creator tooling**  
  [#556](https://github.com/anthropics/skills/issues/556) and [#1169](https://github.com/anthropics/skills/issues/1169) both report `recall=0%` in the eval loop. [#202](https://github.com/anthropics/skills/issues/202) asks for skill-creator best-practice updates.

- **Context-window efficiency**  
  [#1487](https://github.com/anthropics/skills/issues/1487) reports a bundled `claude-api` skill injecting ~156k tokens in one tool call. Lightweight, on-demand skill delivery is a clear need.

- **Agent governance, memory, and quality gates**  
  [#1329](https://github.com/anthropics/skills/issues/1329) proposes `compact-memory`; [#412](https://github.com/anthropics/skills/issues/412) proposes agent-governance safety patterns; [#1385](https://github.com/anthropics/skills/issues/1385) proposes a reasoning quality-gate pipeline.

## 3. High-Potential Pending Skills

These open PRs add new Skills and may land soon if discussion continues.

- **[document-typography](https://github.com/anthropics/skills/pull/514)** — typographic QC for generated documents.
- **[ODT skill](https://github.com/anthropics/skills/pull/486)** — OpenDocument creation, filling, conversion.
- **[testing-patterns](https://github.com/anthropics/skills/pull/723)** — comprehensive testing skill covering philosophy, unit tests, React components, and more.
- **[ServiceNow platform skill](https://github.com/anthropics/skills/pull/568)** — broad ServiceNow assistant covering ITSM, ITOM, ITAM/SAM, SecOps, FSM, SPM, CSDM, and IntegrationHub.
- **[Pyxel retro game development](https://github.com/anthropics/skills/pull/525)** — workflow for pixel-art/8-bit games using `pyxel-mcp`.
- **[self-audit](https://github.com/anthropics/skills/pull/1367)** — mechanical file verification plus four-dimension reasoning audit before delivery.
- **[plan-file-hygiene](https://github.com/anthropics/skills/pull/1479)** — lifecycle management for planning artifacts, addressing #1417.
- **[SAP-RPT-1-OSS predictor](https://github.com/anthropics/skills/pull/181)** — predictive analytics on SAP business data using SAP's open-source tabular foundation model.

## 4. Skills Ecosystem Insight

The community’s most concentrated demand is for trustworthy skill infrastructure — security/trust boundaries, reliable skill-creator evaluation, lifecycle management, and context-efficient execution — rather than merely additional domain-specific content.

---

# Claude Code Community Digest — 2026-08-15

## Today's Highlights
This week's releases ([v2.1.232](https://github.com/anthropics/claude-code/releases) and [v2.1.233](https://github.com/anthropics/claude-code/releases)) make subagent forking and background agent spawns the default behavior, and add GitLab merge request URL support plus an opt-in identity-forwarding setting for apps gateway. The community is most vocal about a long-running cost issue (image processing failures burning tokens, 73 comments) and a beloved UX request — an option to use Enter for newlines instead of sending (147 👍).

## Releases

**v2.1.233**
- **GitLab MR URL support** for the `--worktree` flag and the `claude agents` view, where MRs display as `!N`
- New opt-in **`forward_user_identity` apps gateway setting** on Anthropic upstreams that sends the signed-in user's identity as headers (for proxies behind the gateway)

**v2.1.232**
- **Subagent forking is now on by default**: a `subagent_type: "fork"` subagent inherits the full conversation and prompt cache
- **Non-teammate agent spawns in interactive sessions now run in the background by default**
- Type `@` in the prompt to mention another Claude session by name

## Hot Issues

1. **[#60334 — Image processing failures causing conversation token waste](https://github.com/anthropics/claude-code/issues/60334)** *(closed, 73 comments, 19 👍)* — Users report images being silently dropped with "could not be processed and was removed" errors, burning ~70% of a 5-hour window in one case. The high engagement signals real pain around cost transparency and error handling for image attachments.

2. **[#2054 — Insert a new line with Enter instead of sending](https://github.com/anthropics/claude-code/issues/2054)** *(open, 28 comments, 147 👍)* — The most-upvoted open request. CJK users in particular accidentally send incomplete messages because Enter confirms IME input. A configurable keybinding would resolve it; the community reaction is strongly positive.

3. **[#30869 — Unarchive Claude Code sessions in desktop app](https://github.com/anthropics/claude-code/issues/30869)** *(closed, 29 comments, 57 👍)* — Archived sessions can't be restored from the desktop UI, forcing users to lose context permanently. Likely shipped or slated; high upvote count shows session management is a priority surface.

4. **[#27780 — Analytics Admin API does not return subscription/OAuth users](https://github.com/anthropics/claude-code/issues/27780)** *(open, 26 comments, 23 👍)* — Enterprise admins can't get complete usage data for OAuth/subscription-based users. A governance blind spot blocking adoption for org-level auditing.

5. **[#16837 — MCP_TIMEOUT values longer than 60 seconds are ignored](https://github.com/anthropics/claude-code/issues/16837)** *(open, 15 comments, 16 👍)* — With a repro attached. MCP servers that need slow tool calls get cut off, forcing workarounds. Relevant to anyone building on MCP.

6. **[#82092 — Apps gateway serves OTLP endpoint without headers; Desktop telemetry rejected](https://github.com/anthropics/claude-code/issues/82092)** *(open, 13 comments)* — Every Desktop telemetry flush fails with `missing_token` because `otlpHeaders` aren't provided alongside the bearer-gated `otlpEndpoint`. Breaks observability for gateway operators.

7. **[#11791 — Browser automation tools incompatible with web sandbox proxy](https://github.com/anthropics/claude-code/issues/11791)** *(open, 11 comments, 16 👍)* — Playwright/Puppeteer/Selenium can't run in the web sandbox because the security proxy doesn't support HTTPS CONNECT tunneling. Fundamental architectural limitation; community wants documentation at minimum.

8. **[#86619 — Windows Git Bash: false-positive permission prompts since 2.1.232](https://github.com/anthropics/claude-code/issues/86619)** *(open, 8 comments, 9 👍, new)* — Static analysis on read-only `cd`-compound commands causes constant, unsuppressable permission prompts across two independent machines. A regression correlated with the auto-mode rollout; high visibility for Windows users.

9. **[#79773 — Max 20x upgrade not reflected in weekly limits](https://github.com/anthropics/claude-code/issues/79773)** *(open, 7 comments)* — Users who upgraded to Max 20x report limits still depleting at the Max 5x rate. Billing/limit discrepancies erode trust; notably few upvotes relative to severity.

10. **[#66117 — Option to disable prompt suggestions in Claude.ai web/app](https://github.com/anthropics/claude-code/issues/66117)** *(open, 9 comments, 10 👍)* — Users want to turn off UI-suggested prompts. Minor but recurring polish request reflecting broader "let me control my UI" sentiment.

## Key PR Progress

Four PRs were active in the last 24 hours; all are covered below.

1. **[#86746 — fix(security-guidance): preserve Python probe errors](https://github.com/anthropics/claude-code/pull/86746)** *(open, by aayush598)* — Fixes #86709 by keeping stderr from Python interpreter probes and reporting diagnostics when all candidates fail. Previously, stderr was redirected to `/dev/null`, leaving users with a generic error and no way to debug missing Python setups.

2. **[#86626 — feat: add shell completions (bash, zsh, fish) that stay in sync with the installed CLI](https://github.com/anthropics/claude-code/pull/86626)** *(open, by 5hal1n)* — Adds tab-completion scripts for `claude` under `completions/`, including support for stock macOS bash 3.2 (no bash-completion package required) plus install docs. A quality-of-life improvement for CLI-heavy workflows.

3. **[#83890 — Create pylint.yml](https://github.com/anthropics/claude-code/pull/83890)** *(open, by KrypticKode007)* — Adds a Pylint CI workflow. Small but useful for Python lint consistency in the repo; no description provided, so scope is minimal.

4. **[#41611 — add the missing source to claude code](https://github.com/anthropics/claude-code/pull/41611)** *(open since March, by tornikeo)* — Long-running PR adding a missing source reference. Still open after 5 months; either low priority or awaiting maintainer review.

## Feature Request Trends

- **Session lifecycle management**: Unarchiving sessions ([#30869](https://github.com/anthropics/claude-code/issues/30869)) and a dedicated Background Tasks panel in VS Code for parity with the Desktop app ([#75863](https://github.com/anthropics/claude-code/issues/75863)) show demand for richer session/agent management across surfaces.
- **Input & keybinding customization**: The Enter-key behavior request ([#2054](https://github.com/anthropics/claude-code/issues/2054)) dominates, alongside requests for collapsing long prompts ([#72707](https://github.com/anthropics/claude-code/issues/72707)) and disabling prompt suggestions ([#66117](https://github.com/anthropics/claude-code/issues/66117)) — a broad "let users control their own UI" theme.
- **Enterprise & admin control**: Analytics API completeness ([#27780](https://github.com/anthropics/claude-code/issues/27780)) and identity forwarding (in v2.1.233) indicate growing enterprise adoption with corresponding governance demands.

## Developer Pain Points

- **Cost/token burn**: Image processing failures silently removing images and wasting tokens ([#60334](https://github.com/anthropics/claude-code/issues/60334)) remains a top concern; users want clearer errors and better cost accounting.
- **False-positive permission & safety blocks**: Two clusters stand out — the new Windows Git Bash regression in 2.1.232 ([#86619](https://github.com/anthropics/claude-code/issues/86619)) and a large batch of closed cyber/AUP safety-filter false positives around legitimate drone firmware reverse engineering (e.g., [#71985](https://github.com/anthropics/claude-code/issues/71985), [#71992](https://github.com/anthropics/claude-code/issues/71992)). Legitimate embedded/security work is being halted.
- **Misconfigured infrastructure defaults**: The OTLP header gap breaking Desktop telemetry ([#82092](https://github.com/anthropics/claude-code/issues/82092)) and MCP timeouts capped at 60s ([#16837](https://github.com/anthropics/claude-code/issues/16837)) are integration-level frustrations.
- **Silent failures**: The workflow-backed code review PR commenting that reports `completed` while failing to post ([#84474](https://github.com/anthropics/claude-code/issues/84474)) is dangerous because it erodes trust in automation.
- **Billing/limit accuracy**: Weekly limits not reflecting plan upgrades ([#79773](https://github.com/anthropics/claude-code/issues/79773)) — recurring theme that limits and usage reporting must match plan entitlements exactly.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-08-15

## Today's Highlights
A wave of `rust-v0.148.0` alpha builds (alpha.14 through alpha.18) was published, though the release notes contain no substantive changelog. Community attention is focused on Windows desktop regressions: users report idle CPU busy loops, system-wide mouse stutter, and WMI exhaustion after recent updates. Maintainers also landed a rapid batch of PRs covering sandbox enforcement, gRPC code-mode fixes, TUI startup UX, and observability.

## Releases
Five new alpha releases were tagged in the last 24 hours:

- [rust-v0.148.0-alpha.14](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.14)
- [rust-v0.148.0-alpha.15](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.15)
- [rust-v0.148.0-alpha.16](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.16)
- [rust-v0.148.0-alpha.17](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.17)
- [rust-v0.148.0-alpha.18](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.18)

Release notes only contain placeholder text; no user-facing changes were documented.

## Hot Issues

1. [Codex App frequently freezes/stutters on Windows 11 Pro despite sufficient system resources (#20214)](https://github.com/openai/codex/issues/20214) — 101 comments / 84 👍. The highest-traffic open issue, showing sustained Windows desktop performance pain.
2. [Windows Desktop: unbounded taskkill.exe/conhost.exe cleanup storm exhausts WMI (#34260)](https://github.com/openai/codex/issues/34260) — 35 comments / 11 👍. A specific technical root cause for system-wide slowdowns on Windows.
3. [Context compaction loses operational continuity in long Codex tasks (#29356)](https://github.com/openai/codex/issues/29356) — 21 comments. Users want the last few operational steps preserved verbatim during compaction.
4. [Codex Desktop causes intermittent system input lag on Windows (#28855)](https://github.com/openai/codex/issues/28855) — 17 comments / 20 👍. Happens even with clean logs and disabled plugins.
5. [Android remote connection to Windows Codex stuck on “Waiting for desktop…” (#22733)](https://github.com/openai/codex/issues/22733) — 16 comments / 19 👍. Remote session startup remains broken for Android users.
6. [Windows sandbox: CreateProcessAsUserW fails with error 5 when resolved shell is MSIX pwsh (#35871)](https://github.com/openai/codex/issues/35871) — 14 comments. Packaged PowerShell fails under the sandbox’s restricted token.
7. [Codex Windows idle main-process CPU busy loop in Chrome plugin app-server hashing (#38547)](https://github.com/openai/codex/issues/38547) — 11 comments / 5 👍. New regression after updating to `26.810.4967.0`.
8. [[Windows 11] ChatGPT/Codex causes persistent system-wide mouse lag and ~10% CPU while idle (#38583)](https://github.com/openai/codex/issues/38583) — 10 comments / 6 👍. Another recent-update regression.
9. [Codex App exits after task completion when Browser Use session tears down (#36645)](https://github.com/openai/codex/issues/36645) — 10 comments. The teardown path can crash the app entirely.
10. [[Windows][26.810.4967.0] This update makes the entire PC stutter (#38554)](https://github.com/openai/codex/issues/38554) — 7 comments / 3 👍. Fully exiting Codex restores responsiveness immediately.

## Key PR Progress

1. [Resolve local JSON Schema refs in Code Mode types (#38664)](https://github.com/openai/codex/pull/38664) — Fixes `$ref` values being rendered as `unknown`, enabling correct TypeScript declarations.
2. [Enforce managed deny-read rules in the Windows sandbox (#38660)](https://github.com/openai/codex/pull/38660) — Makes unsupported sandbox policies fail closed instead of running without requested filesystem protection.
3. [Move permission profile snapshots into the protocol (#38651)](https://github.com/openai/codex/pull/38651) — Stores snapshots in core permission state while continuing to apply profile constraints.
4. [Canonicalize default namespaces in gRPC subscription filters (#38650)](https://github.com/openai/codex/pull/38650) — Treats missing/empty namespaces as aliases for `functions`, improving gRPC matching.
5. [Deliver gRPC code-mode notifications without truncation (#38645)](https://github.com/openai/codex/pull/38645) — Removes the 1,024-byte truncation limit for notification text.
6. [Remove the gRPC code-mode open session limit (#38630)](https://github.com/openai/codex/pull/38630) — Allows more concurrent open sessions while preserving existing in-flight/control limits.
7. [Add MCP protocol discovery metrics (#38634)](https://github.com/openai/codex/pull/38634) — Adds counters/durations for `legacy` and `auto` MCP discovery outcomes.
8. [Make Guardian v2 risk classification configurable (#38628)](https://github.com/openai/codex/pull/38628) — Adds configurable classifier instructions, thresholds, token limits, and transcript controls.
9. [Keep the composer editable during TUI startup (#38642)](https://github.com/openai/codex/pull/38642) — Shows a provisional composer while startup work runs, preserving text and cursor position.
10. [Harden TUI startup input handling (#38641)](https://github.com/openai/codex/pull/38641) — Prevents buffered terminal-probe input from accidentally confirming actions while preserving composer typeahead.

## Feature Request Trends

- **Environment/session portability:** Users want per-project/per-chat Windows/WSL execution environments ([#36098](https://github.com/openai/codex/issues/36098)), a `/cd` command to move a conversation to another working directory ([#38585](https://github.com/openai/codex/issues/38585)), and Chrome side panel project selection for new chats ([#32610](https://github.com/openai/codex/issues/32610)).
- **Context/task handoff:** Requests for repository-aware, sanitized task handoffs across workspaces ([#34582](https://github.com/openai/codex/issues/34582)) and more reliable context compaction that preserves operational continuity ([#29356](https://github.com/openai/codex/issues/29356)).
- **Model/platform parity:** Codex CLI users want Ultra reasoning for GPT-5.6 on Amazon Bedrock, which currently stops at Max ([#37160](https://github.com/openai/codex/issues/37160)).

## Developer Pain Points

- **Windows performance regressions dominate:** system-wide stutter, mouse lag, idle CPU busy loops, WMI exhaustion, and `taskkill.exe`/`conhost.exe` storms ([#20214](https://github.com/openai/codex/issues/20214), [#28855](https://github.com/openai/codex/issues/28855), [#34260](https://github.com/openai/codex/issues/34260), [#38547](https://github.com/openai/codex/issues/38547), [#38583](https://github.com/openai/codex/issues/38583), [#38554](https://github.com/openai/codex/issues/38554)).
- **Context compaction is unreliable:** it loses pre-compression reasoning, disconnects mid-operation, or diverts to unrelated plans ([#29356](https://github.com/openai/codex/issues/29356), [#31375](https://github.com/openai/codex/issues/31375)).
- **Remote connectivity gaps:** Android remote sessions get stuck on “Waiting for desktop…” ([#22733](https://github.com/openai/codex/issues/22733)).
- **Windows sandbox friction:** MSIX PowerShell fails under restricted tokens ([#35871](https://github.com/openai/codex/issues/35871)), and Codex CLI uninstall does not revert system sandbox changes ([#15343](https://github.com/openai/codex/issues/15343)).
- **Lifecycle/rate-limit issues:** app exits after Browser Use teardown ([#36645](https://github.com/openai/codex/issues/36645)), SQLite log files grow monotonically without vacuum ([#35823](https://github.com/openai/codex/issues/35823)), and usage limits reportedly do not reset on time ([#37442](https://github.com/openai/codex/issues/37442), [#38652](https://github.com/openai/codex/issues/38652)).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-08-15

## Today's Highlights
The latest nightly release introduces context-aware silent retries for capacity errors, a welcome step for users hitting availability throttling. Concurrently, a wave of automated "SSR Agent" PRs landed fixes for long-standing reliability bugs: subagent termination reporting, TUI hangs, and PTY leaks. Community attention remains concentrated on agent execution reliability, especially hangs, false success reports, and subagent behavior.

## Releases
**v0.56.0-nightly.20260814.gc0d192452** — [Release](https://github.com/google-gemini/gemini-cli/releases/tag/v0.56.0-nightly.20260814.gc0d192452)

- `test(e2e)`: stabilize file-system-interactive test on slow runners (by DavidAPierce in [#28793](https://github.com/google-gemini/gemini-cli/pull/28793))
- `fix(core)`: implement context-aware silent retries and availability TTL for capacity errors (by DavidAPierce in [#28761](https://github.com/google-gemini/gemini-cli/pull/28761))

No stable release in the last 24 hours.

## Hot Issues
1. **[#22323 — Subagent recovery after MAX_TURNS is reported as GOAL success, hiding interruption](https://github.com/google-gemini/gemini-cli/issues/22323)**  
   A `codebase_investigator` subagent reports `success` / `GOAL` even when it hit `MAX_TURNS` before doing real work. This masks actual failures and undermines trust in agent results. 12 comments, 2 👍.

2. **[#21409 — Generalist agent hangs](https://github.com/google-gemini/gemini-cli/issues/21409)**  
   The generalist agent hangs indefinitely on simple tasks like folder creation; users must work around it by instructing the model not to defer to subagents. 8 comments, 8 👍 — one of the most upvoted active bug reports.

3. **[#25166 — Shell command execution gets stuck with "Waiting input" after command completes](https://github.com/google-gemini/gemini-cli/issues/25166)**  
   Even simple CLI commands remain marked active after finishing, causing repeated hangs. 4 comments, 3 👍.

4. **[#19873 — Zero-Dependency OS Sandboxing & Post-Execution Intent Routing](https://github.com/google-gemini/gemini-cli/issues/19873)**  
   Proposal to let Gemini 3 use native bash/POSIX workflows safely through sandboxing and intent routing. A significant architecture-level enhancement for security and capability. 8 comments.

5. **[#24353 — Robust component level evaluations](https://github.com/google-gemini/gemini-cli/issues/24353)**  
   Epic tracking expansion of behavioral evals from 76 tests to broader component coverage across six Gemini model versions. Important for preventing regressions. 7 comments.

6. **[#22745 — Impact of AST-aware file reads, search, and mapping](https://github.com/google-gemini/gemini-cli/issues/22745)**  
   Investigates whether AST-aware tools can reduce token noise, cut turns from misaligned reads, and improve codebase navigation. 7 comments.

7. **[#21968 — Gemini does not use skills and sub-agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)**  
   Users report the model only uses custom skills/subagents when explicitly told to, even in obvious scenarios like Gradle/git commands. 6 comments.

8. **[#26522 — Stop Auto Memory from retrying low-signal sessions indefinitely](https://github.com/google-gemini/gemini-cli/issues/26522)**  
   Auto Memory can repeatedly surface low-signal sessions because they are only marked processed after a successful read, causing wasted extraction work. 5 comments.

9. **[#22232 — Enhance browser_agent resilience: Automatic session takeover and lock recovery](https://github.com/google-gemini/gemini-cli/issues/22232)**  
   `BrowserManager` fails fast on locked profiles instead of recovering or taking over. A pain point for persistent browser automation. 4 comments.

10. **[#21983 — Browser subagent fails in Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)**  
   Browser-agent sessions fail under Wayland despite reaching GOAL. Linux desktop users are impacted. 4 comments, 1 👍.

## Key PR Progress
1. **[#28815 — Preserve original termination reason during subagent recovery](https://github.com/google-gemini/gemini-cli/pull/28815)**  
   Fixes [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) so `MAX_TURNS`/`TIMEOUT` interruptions are not reported as successful `GOAL` completion.

2. **[#28817 — Retain executing subagent tool calls in hook state](https://github.com/google-gemini/gemini-cli/pull/28817)**  
   Fixes [#22589](https://github.com/google-gemini/gemini-cli/issues/22589) by keeping first-seen, no-approval subagent tool calls visible to hooks.

3. **[#28812 — Prevent indefinite TUI hang by adding execution timeouts](https://github.com/google-gemini/gemini-cli/pull/28812)**  
   Fixes [#21477](https://github.com/google-gemini/gemini-cli/issues/21477): bare Linux terminals no longer hang forever at "Initializing..." when `ps` is unavailable.

4. **[#28816 — Fix silent hang in MessageBus.request when publish fails](https://github.com/google-gemini/gemini-cli/pull/28816)**  
   Fixes floating `publish()` promises causing 60-second silent hangs when publish rejects.

5. **[#20916 — Prevent PTY file descriptor leak in ShellExecutionService](https://github.com/google-gemini/gemini-cli/pull/20916)**  
   Closes [#15945](https://github.com/google-gemini/gemini-cli/issues/15945): PTY master FDs were not being closed after exit, leading to system-wide PTY exhaustion on macOS.

6. **[#27154 — Prevent PTY memory leak by synchronously deleting active entries](https://github.com/google-gemini/gemini-cli/pull/27154)**  
   Fixes another PTY cleanup path where `.then()` ordering left `activePtys` entries and headless terminals uncollected.

7. **[#28738 — Allow agents to call agents](https://github.com/google-gemini/gemini-cli/pull/28738)**  
   Large feature PR fixing [#22092](https://github.com/google-gemini/gemini-cli/issues/22092) by letting subagents delegate to other subagents or recurse via `tools:` frontmatter.

8. **[#28597 — Load environment variables before resolving settings placeholders](https://github.com/google-gemini/gemini-cli/pull/28597)**  
   Fixes a load-order race where `.env` files were not available when `settings.json` placeholders were expanded.

9. **[#28603 — Upgrade sandbox Dockerfile to Node 22](https://github.com/google-gemini/gemini-cli/pull/28603)**  
   Resolves [#28584](https://github.com/google-gemini/gemini-cli/issues/28584) by moving off Node 20, which reached EOL 2026-04-30. Important security hardening for sandbox execution.

10. **[#28596 — Add --list-all-sessions option to list sessions across all workspaces](https://github.com/google-gemini/gemini-cli/pull/28596)**  
   Quality-of-life feature for users with many sessions scattered across project folders, grouped by workspace path.

Additional automated SSR Agent PRs closed smaller issues including `/clear` docs ([#28810](https://github.com/google-gemini/gemini-cli/pull/28810)), tsconfig composite ([#28813](https://github.com/google-gemini/gemini-cli/pull/28813)), strict-null test fixes ([#28814](https://github.com/google-gemini/gemini-cli/pull/28814)), steering eval policy ([#28818](https://github.com/google-gemini/gemini-cli/pull/28818)), account mismatch errors ([#28819](https://github.com/google-gemini/gemini-cli/pull/28819)), and privacy notice wording ([#28820](https://github.com/google-gemini/gemini-cli/pull/28820)).

## Feature Request Trends
- **AST-aware codebase navigation**  
  Multiple issues ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)) propose AST-aware file reads, search, and mapping to reduce token usage and improve navigation precision.
- **Deeper agent self-awareness and control**  
  Users want the CLI to correctly know its own flags, hotkeys, and capabilities ([#21432](https://github.com/google-gemini/gemini-cli/issues/21432)), and to use skills/subagents proactively ([#21968](https://github.com/google-gemini/gemini-cli/issues/21968)).
- **Agent delegation and recursion**  
  PR [#28738](https://github.com/google-gemini/gemini-cli/pull/28738) shows strong demand for subagents that can call other subagents.
- **Browser agent resilience**  
  Requests focus on automatic takeover of locked profiles, recovery, and respecting `settings.json` overrides ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232), [#22267](https://github.com/google-gemini/gemini-cli/issues/22267)).
- **Safety and intent routing**  
  There is a clear direction toward sandboxed bash execution and post-execution intent routing ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)), plus discouraging destructive commands ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)).
- **Memory system transparency**  
  Auto Memory/related issues request deterministic redaction, quarantine of invalid patches, and better inbox handling (#26522–#26525).
- **Session management UX**  
  `--list-all-sessions` ([#28596](https://github.com/google-gemini/gemini-cli/pull/28596)) and subagent trajectory sharing via `/chat share` ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598)) reflect a push for better observability.

## Developer Pain Points
- **Frequent agent hangs / indefinite blocking**  
  Generalist hangs ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), shell command "Waiting input" stuck states ([#25166](https://github.com/google-gemini/gemini-cli/issues/25166)), and TUI initialization hangs are prominent recurring issues.
- **False success reporting from subagents**  
  Terminated or interrupted subagents reporting `GOAL` success ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)) erodes confidence and makes debugging harder.
- **Resource leaks in long sessions**  
  PTY file descriptor and memory leaks ([#20916](https://github.com/google-gemini/gemini-cli/pull/20916), [#27154](https://github.com/google-gemini/gemini-cli/pull/27154)) cause system-level exhaustion on long-running workflows.
- **Permission and configuration regressions**  
  Subagents executing without permission since v0.33.0 ([#22093](https://github.com/google-gemini/gemini-cli/issues/22093)) and browser agent ignoring `settings.json` ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)) are high-frustration configuration bugs.
- **Platform-specific breakage**  
  Windows `grep_search` failures ([#25378](https://github.com/google-gemini/gemini-cli/pull/25378)), WSL2 clipboard paste gaps ([#27588](https://github.com/google-gemini/gemini-cli/pull/27588)), and Wayland browser failures ([#21983](https://github.com/google-gemini/gemini-cli/issues/21983)) show cross-platform friction.
- **Memory and privacy concerns**  
  Auto Memory sends transcript content to model context before redaction ([#26525](https://github.com/google-gemini/gemini-cli/issues/26525)), and invalid patches are silently skipped, complicating cleanup ([#26523](https://github.com/google-gemini/gemini-cli/issues/26523)).
- **Environment/settings load-order instability**  
  Settings placeholders resolving before `.env` is loaded caused subtle race conditions ([#28597](https://github.com/google-gemini/gemini-cli/pull/28597)).

Overall, the community is converging on one theme: **agent reliability**. The fix velocity this week is strong, but hangs, misreported outcomes, and subagent behavior remain the dominant developer frustrations.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-08-15

## 1. Today's Highlights

v1.0.80 shipped with model configuration updates, but the community is flagging several regressions in recent versions: MCP OAuth authentication is breaking for Atlassian and GitLab due to RFC 8414 issuer-mismatch handling, and enterprise users report missing/disabled Claude models despite organization settings. A fatal autopilot OOM crash and BYOK prompt-caching breakage are also drawing attention. Meanwhile, older requests around MCP pagination, OTLP protobuf export, and plugin dependency management remain open.

## 2. Releases

- **[v1.0.80](https://github.com/github/copilot-cli/releases)** — 2026-08-14  
  Release notes: *Update model configurations*.
- **[v1.0.80-1](https://github.com/github/copilot-cli/releases)** — 2026-08-14  
  Release notes: *Fixes and changes*.

No detailed changelog was provided in the data for either release.

## 3. Hot Issues

- **[#4345 — Reasoning effort 'medium' is not supported for model 'claude-haiku-4.5'](https://github.com/github/copilot-cli/issues/4345)**  
  Sub-agent execution fails when certain server-side feature flags are active. 6 comments, 4 👍. Highlights an ongoing model/reasoning configuration mismatch.

- **[#4390 — Enabled organization models missing from catalogue](https://github.com/github/copilot-cli/issues/4390)**  
  Claude Sonnet 5 / Opus 5 and Kimi K3 are enabled in Copilot Business but unavailable in the CLI. 6 comments, 4 👍. Enterprise model visibility remains a pain point.

- **[#4480 — Atlassian MCP OAuth fails with RFC 8414 issuer mismatch](https://github.com/github/copilot-cli/issues/4480)**  
  Regression from 1.0.71 to 1.0.79. 4 comments, 6 👍. Same failure is still being reported against 1.0.80 in [#4490](https://github.com/github/copilot-cli/issues/4490).

- **[#4422 — All Claude models disabled under CLI model selection](https://github.com/github/copilot-cli/issues/4422)**  
  Personal Enterprise accounts can no longer use Claude models even though they appear enabled in GitHub settings. 3 comments, 3 👍.

- **[#4439 — GitLab MCP OAuth metadata rejected with RFC 8414 issuer mismatch](https://github.com/github/copilot-cli/issues/4439)**  
  Second MCP OAuth regression report involving self-managed GitLab MCP servers. 3 comments, 2 👍.

- **[#4306 — Subtasks freeze and stop responding](https://github.com/github/copilot-cli/issues/4306)**  
  Autopilot sessions using multiple agents/skills eventually freeze during sub-agent execution. 3 comments, 2 👍. Impacts long-running automation workflows.

- **[#2934 — Support protobuf OTLP export](https://github.com/github/copilot-cli/issues/2934)**  
  Closed, but 6 👍. Users want `OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf` respected instead of silently forcing JSON export.

- **[#4346 — MCP registry policy fetch returns 403 for Actions GITHUB_TOKEN](https://github.com/github/copilot-cli/issues/4346)**  
  Blocks non-default MCP servers in CI when using the documented PAT-less Actions setup. 2 comments, 3 👍.

- **[#4499 — Fatal OOM "Committing semi space failed" in autopilot](https://github.com/github/copilot-cli/issues/4499)**  
  `copilot.exe` v1.0.79 crashes with V8 heap only ~607 MB / 4.3 GB used. Suggests a host-RAM commit failure rather than a normal heap limit.

- **[#4500 — BYOK autopilot nudge re-serializes transcript items, breaking prompt caching](https://github.com/github/copilot-cli/issues/4500)**  
  Prompt caching is defeated because the CLI rebuilds `input` arrays instead of resending prior items byte-for-byte.

## 4. Key PR Progress

Only 3 PRs were updated in the last 24 hours; all are listed below.

- **[#4497 — Handle fork PR associations in invalid-label writer](https://github.com/github/copilot-cli/pull/4497)**  
  Fixes invalid-label automation for fork PRs when GitHub does not populate the PR association. The writer falls back to trusted workflow-run metadata and requires exactly one open PR.

- **[#4496 — [canary] Verify pull request workflow migration](https://github.com/github/copilot-cli/pull/4496)**  
  Closed temporary canary PR used to validate fork-originated PR automation. Documentation-only; not intended for review.

- **[#4449 — Migrate pull request automation away from pull_request_target](https://github.com/github/copilot-cli/pull/4449)**  
  Closes invalid issues directly with issue-scoped write tokens and uses a no-permission `pull_request` signal for mergeable PR handling. Reduces the security surface of privileged GitHub Actions workflows.

## 5. Feature Request Trends

- **Model configuration and reasoning controls**  
  Requests include support for GPT-5.6 `reasoning.mode` ([#4495](https://github.com/github/copilot-cli/issues/4495)), configurable reasoning effort ([#4345](https://github.com/github/copilot-cli/issues/4345)), and better model-catalogue refresh behavior ([#4494](https://github.com/github/copilot-cli/issues/4494)).

- **MCP ecosystem hardening**  
  Users want spec-compliant MCP `tools/list` pagination ([#4006](https://github.com/github/copilot-cli/issues/4006)), case-insensitive server-name collision detection ([#4478](https://github.com/github/copilot-cli/issues/4478)), and reliable MCP auth in CI ([#4346](https://github.com/github/copilot-cli/issues/4346)).

- **Plugin dependency and lifecycle management**  
  [#4487](https://github.com/github/copilot-cli/issues/4487) requests a formal dependency model for marketplace plugins, including automatic installation of inter/intra marketplace dependencies. [#4488](https://github.com/github/copilot-cli/issues/4488) highlights update failures due to file locks.

- **Observability and caching**  
  [#2934](https://github.com/github/copilot-cli/issues/2934) asks for OTLP protobuf export, while [#4500](https://github.com/github/copilot-cli/issues/4500) emphasizes preserving transcript bytes to maintain BYOK prompt caching.

- **Session, worktree, and UX improvements**  
  Notable requests include `/restart` support for sessions started with `-w` ([#4493](https://github.com/github/copilot-cli/issues/4493)), restoring the selected agent when resuming sessions ([#4489](https://github.com/github/copilot-cli/issues/4489)), and clarifying the "No copilot-instructions.md found" message ([#4475](https://github.com/github/copilot-cli/issues/4475)).

## 6. Developer Pain Points

- **Model availability/catalogue confusion**  
  Users repeatedly see "model disabled" errors even when models are enabled in org settings ([#4390](https://github.com/github/copilot-cli/issues/4390), [#4422](https://github.com/github/copilot-cli/issues/4422)). Newly enabled models can remain unavailable until local state/cache is cleared ([#4494](https://github.com/github/copilot-cli/issues/4494)).

- **MCP OAuth churn across releases**  
  Atlassian and GitLab MCP OAuth both fail with the same RFC 8414 issuer mismatch after upgrades, with regressions reported between 1.0.71 and 1.0.80 ([#4480](https://github.com/github/copilot-cli/issues/4480), [#4439](https://github.com/github/copilot-cli/issues/4439), [#4490](https://github.com/github/copilot-cli/issues/4490)).

- **Session loss and agent lifecycle instability**  
  Stopping an action can delete the session and prompt ([#4477](https://github.com/github/copilot-cli/issues/4477)), `/restart` conflicts with worktree sessions ([#4493](https://github.com/github/copilot-cli/issues/4493)), and resumed sessions lose the previously selected agent ([#4489](https://github.com/github/copilot-cli/issues/4489)).

- **Permission and policy friction**  
  `allowed_directories` does not suppress path-outside prompts ([#4482](https://github.com/github/copilot-cli/issues/4482)), edit permission requests time out ([#4486](https://github.com/github/copilot-cli/issues/4486)), and org policy gating for the Copilot App remains confusing ([#4481](https://github.com/github/copilot-cli/issues/4481)).

- **Long-running session stability**  
  Autopilot sessions can freeze ([#4306](https://github.com/github/copilot-cli/issues/4306)) or crash with OOM even when V8 heap usage is low ([#4499](https://github.com/github/copilot-cli/issues/4499)).

- **False-positive safety/security flags**  
  Ordinary debugging operations have been rejected with `CAPI 422` as cybersecurity risks ([#4479](https://github.com/github/copilot-cli/issues/4479)).

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-08-15

## Today's Highlights
No new releases or pull requests landed in the last 24 hours. Community activity remains concentrated on persistent memory and cross-device workflows: [#1283](https://github.com/MoonshotAI/kimi-cli/issues/1283) continues to attract heavy discussion (39 comments), while [#1478](https://github.com/MoonshotAI/kimi-cli/issues/1478) reinforces user pain around the memory layer in large projects. A closed PowerShell shell enhancement ([#1136](https://github.com/MoonshotAI/kimi-cli/issues/1136)) also saw updated status.

## Releases
No new releases were published in the last 24 hours.

## Hot Issues
Only 4 issues were updated in the last 24 hours; all are listed below.

- [**#1283 – Feature Request: Memory System - Persistent context across sessions**](https://github.com/MoonshotAI/kimi-cli/issues/1283)  
  *Author: CatKang | Updated: 2026-08-14 | Comments: 39 | 👍: 0*  
  Proposes a comprehensive memory system with automatic (AI-managed notes) and manual (user-defined instructions) memory, enabling Kimi Code CLI to persist project patterns and user preferences across sessions. This is the most-discussed recent issue, indicating strong community interest in durable context handling.

- [**#2269 – Feature Request: Remote Control / Multi-Device Session Handoff**](https://github.com/MoonshotAI/kimi-cli/issues/2269)  
  *Author: lucianalima777 | Updated: 2026-08-14 | Comments: 6 | 👍: 1*  
  Requests the ability to start a CLI session on one device and seamlessly continue or remotely control it from another (laptop, web, or mobile). Important for users working across multiple environments; positive but modest engagement so far.

- [**#1478 – Can the memory layer be optimized? / 能否优化记忆层？**](https://github.com/MoonshotAI/kimi-cli/issues/1478)  
  *Author: hahy36 | Updated: 2026-08-14 | Comments: 2 | 👍: 0*  
  Reiterates the pain of working on large projects without a clear, documented memory layer — only `agent.md` is mentioned. The author references an external memory layout (`MEMORY.md`, daily memory files, etc.) as a useful reference. Aligns closely with the broader demand in #1283.

- [**#1136 – feat(shell): enhance shell tool with version-aware PowerShell context**](https://github.com/MoonshotAI/kimi-cli/issues/1136)  
  *Author: QIN2DIM | Updated: 2026-08-14 | Comments: 0 | 👍: 0*  
  Closed enhancement describing Windows-specific Shell tool issues while testing Kimi K2.5 (SGLang), including ambiguous shebang handling and degraded first-pass command generation. No comments, but its closed status may mean the change was implemented or superseded elsewhere.

## Key PR Progress
No pull requests were updated in the last 24 hours.

## Feature Request Trends
The most requested feature directions from the current issue set are:

- **Persistent memory / context management** (#1283, #1478) — Users want automatic and manual memory, documented behavior, and better context retention for large projects.
- **Cross-device session continuity** (#2269) — Support for handing off or remotely controlling Kimi sessions across laptop, web, and mobile.
- **Windows shell reliability** (#1136) — Version-aware PowerShell handling and improved command generation in the Shell tool.

## Developer Pain Points
- **Large-project context loss** — The memory layer is seen as under-optimized and poorly documented; users struggle when working on big codebases.
- **No cross-session persistence** — The absence of persistent memory means useful context, project patterns, and preferences are lost between sessions.
- **Multi-device workflow gaps** — Sessions are currently tied to a single device, making it hard to switch environments mid-task.
- **Windows PowerShell friction** — Ambiguous command generation in the shell tool degrades the agent's first-pass performance on Windows.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-08-15

## Today’s Highlights
No new releases landed in the last 24 hours. The dominant topic is a **session-wedging ID timestamp wraparound** (#42608) that appears to be the root cause behind multiple “agent stopped responding” reports, including #42605. On the PR side, the most notable progress is **dynamic model discovery for OpenAI-compatible providers** (#42660), which has been a long-requested configuration feature.

## Releases
None in the last 24 hours.

## Hot Issues

1. **[#42608 — 48-bit ID timestamp wraparound on 2026-08-14 wedges all pre-existing sessions](https://github.com/anomalyco/opencode/issues/42608)**  
   Critical bug: the 48-bit timestamp in the ID generator wrapped at `2026-08-14 12:39:55 UTC`, causing all pre-existing sessions to stop processing prompts silently. Likely the root cause behind #42605 and the recent spike of unresponsive sessions. 5 comments, 3 👍.

2. **[#36997 — Desktop App v1.18.1 new layout hides agent switching UI](https://github.com/anomalyco/opencode/issues/36997)**  
   The new desktop layout (`newLayoutDesigns: true`) removes the visible Plan/Build mode toggle, making it impossible to tell which agent is active. High community impact with 12 comments and 6 👍.

3. **[#42605 — Session remains open, but agent does not process subsequent prompts](https://github.com/anomalyco/opencode/issues/42605)**  
   Likely related to #42608. After finishing a task and asking the user a question, new messages produce no response. This is the most visible symptom of the wraparound issue. 4 comments.

4. **[#38791 — Run loop can never exit when message IDs are not time-sortable](https://github.com/anomalyco/opencode/issues/38791)**  
   `SessionPrompt.runLoop` compares message IDs as plain strings, which only works because OpenCode embeds timestamps. Imported sessions with non-chronological IDs loop until the provider rejects them. Important correctness issue for third-party session imports. 6 comments.

5. **[#41518 — gpt-5.6-luna via OpenCode Go relay returns 403 “not available in your region”](https://github.com/anomalyco/opencode/issues/41518)**  
   Users accessing `gpt-5.6-luna` through the OpenCode Go relay get a regional 403, causing friction for non-US developers. Highlights ongoing regional availability concerns with the proxy. 6 comments.

6. **[#37489 — Context cache invalidation when switching modes or during compaction](https://github.com/anomalyco/opencode/issues/37489)**  
   Local LLM users (vLLM/Ollama) see major performance degradation because context caches are invalidated on mode switches or compaction. Requested optimization for local inference workflows. 5 comments, 1 👍.

7. **[#42657 — TUI lag with multi-subagent sessions (97% CPU on render thread)](https://github.com/anomalyco/opencode/issues/42657)**  
   Running 2–4 concurrent subagents makes the TUI nearly unusable, with 1–3 second typing delay and frozen spinners. Reproduced across Warp, Windows Terminal, and WezTerm. 2 comments.

8. **[#42385 — DeepSeek V4 Flash Free returns FreeUsageLimitError on OpenCode Zen](https://github.com/anomalyco/opencode/issues/42385)**  
   The free model consistently fails with `FreeUsageLimitError` despite valid auth, even within quota. A recurring complaint about free-tier quota handling. 3 comments.

9. **[#42616 — Zen Go Anthropic endpoint: all GLM models fail tool requests with 422 `web_search` error](https://github.com/anomalyco/opencode/issues/42616)**  
   Any non-empty `tools` array fails for GLM models on the Anthropic-compatible route. Other models accept the same request, pointing to model-specific translation bugs. 2 comments.

10. **[#41120 — Kimi models fail tool calls with 400 “function name is invalid” via Anthropic route](https://github.com/anomalyco/opencode/issues/41120)**  
   Tool calls carrying `input_schema` are rejected for `kimi-k3` and `kimi-k2.7-code` on the Zen Go Anthropic endpoint. Another provider-specific compatibility gap. 2 comments.

## Key PR Progress

1. **[#42660 — Add dynamic model discovery for custom providers](https://github.com/anomalyco/opencode/pull/42660)**  
   New feature that automatically fetches `/v1/models` from OpenAI-compatible providers, eliminating manual model lists. Closes six related issues, including #13891 and #29308.

2. **[#42656 — Move worktree routes out of experimental namespace](https://github.com/anomalyco/opencode/pull/42656)**  
   Promotes worktree APIs from `/api/experimental/...` to stable top-level routes, a breaking API cleanup for the protocol layer.

3. **[#36869 — Per-tool execution timeout with abort + session recovery](https://github.com/anomalyco/opencode/pull/36869)**  
   Adds configurable per-tool timeouts for built-in and MCP tools, preventing wedged agent loops from hanging indefinitely. Related to several timeout issues.

4. **[#36870 — Load documented provider packages](https://github.com/anomalyco/opencode/pull/36870)**  
   Accepts provider/model `package` and `settings` fields and properly promotes `settings.baseURL` so OpenAI-compatible routes work without malformed `undefined/...` URLs.

5. **[#36796 — Wait for shell output capture](https://github.com/anomalyco/opencode/pull/36796)**  
   Fixes a race where shell output-capture fiber was not joined after process exit, preventing truncated or lost command output.

6. **[#36883 — Expose valid subagent IDs to the model](https://github.com/anomalyco/opencode/pull/36883)**  
   Previously the `subagent` tool did not list valid agent IDs, so models guessed names like `explorer` instead of the configured ID. Improves agent reliability.

7. **[#36861 — Recover cache tokens from openai-compatible metadata usage fallback](https://github.com/anomalyco/opencode/pull/36861)**  
   Custom OpenAI-compatible providers often report cache tokens via metadata (e.g. `prompt_tokens_details`). This PR restores accurate cache-token accounting.

8. **[#36863 — Make webfetch response size limit configurable](https://github.com/anomalyco/opencode/pull/36863)**  
   Adds `OPENCODE_WEBFETCH_MAX_SIZE` env variable, addressing issues with large pages failing during web fetch.

9. **[#36862 — Validate openExternal URLs by protocol in desktop app](https://github.com/anomalyco/opencode/pull/36862)**  
   Security fix: prevents `shell.openExternal` from accepting dangerous protocols like `file://` or `javascript:`. Closes #30613.

10. **[#36860 — Strip MiniMax trailing tool_call leak suffix](https://github.com/anomalyco/opencode/pull/36860)**  
   Fixes MiniMax models appending serialized tool-call markers (e.g. `]<\`]minimax...`) to plain assistant text, which broke downstream parsing.

## Feature Request Trends

- **Automatic model discovery**: The strongest recurring theme is auto-discovering models from OpenAI-compatible providers instead of manual config (#27553, addressed by #42660).
- **Runtime permission controls**: Users want an `/approve on|off` command to toggle permission approval mode per session, inspired by Claude Code (#41909).
- **Configurable network/auth servers**: Self-hosted and remote setups need configurable OAuth callback host (#33966).
- **Context cache and compaction performance**: Local LLM users want cache invalidation minimized during mode switches and compaction (#37489).
- **Provider-specific translation fixes**: Multiple requests/errors point to a broader need for better provider compatibility layers, especially for tool calls and thinking content.

## Developer Pain Points

- **Sessions becoming unresponsive**: The 48-bit ID wraparound and non-time-sortable message IDs are causing silent prompt drops and infinite loops — the most urgent issue this week.
- **Provider quirk whack-a-mole**: DeepSeek `reasoning_content`, GLM `web_search`, Kimi function-name validation, and MiniMax tool-call leaks show the cost of supporting many providers on both OpenAI and Anthropic routes.
- **Free-tier availability confusion**: Repeated `FreeUsageLimitError` and “429 rate limited” reports indicate quota/reset handling is unclear or inconsistent on Zen free models.
- **Desktop/WSL integration gaps**: WSL mirrored-networking failures and desktop layout regressions are disrupting daily workflows.
- **TUI performance under concurrency**: Multi-subagent sessions cause extreme render-thread CPU usage, making the interface nearly unusable in some environments.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-08-15

## Today's Highlights

Pi shipped **v0.84.2** with fullscreen transcript search and configurable default tools, while the community's attention centered on Windows/WSL reliability, Copilot login rate-limiting, and a wave of provider-compatibility fixes. Notable PRs landed for honest clipboard behavior in the TUI, pnpm-based extension loading, and substantial new provider work including ChatGPT OAuth image generation, SiliconFlow, and an xAI default bump to Grok 4.6.

## Releases

**v0.84.2**
- **Fullscreen transcript search** — search and navigate matches in fullscreen mode. [TUI Fullscreen Viewport keybindings](https://github.com/earendil-works/pi/blob/v0.84.2/packages/coding-agent/docs/keybindings.md#tui-fullscreen-viewport)
- **Configurable default tools** — choose which tools are active at startup.

## Hot Issues

1. **#7547 — [Windows] How do you use Pi on Windows? What issues are you seeing?** — *27 comments, 1 👍*  
   An open call for Windows developer feedback; high engagement signals Windows support is a top community priority. The maintainers are trying to figure out where to focus energy vs. what to delegate to extensions.  
   https://github.com/earendil-works/pi/issues/7547

2. **#6187 — Pi login hangs in WSL after browser-based GitHub Copilot device authorization** — *26 comments*  
   Device authorization completes in the browser but the WSL client never detects it, leaving users stuck at the login prompt. A long-running WSL pain point.  
   https://github.com/earendil-works/pi/issues/6187

3. **#5223 — Anthropic provider modifies thinking blocks in latest assistant message, causing 400 with Opus 4.8 adaptive thinking** — *17 comments, 6 👍*  
   Multi-turn conversations fail mid-session with `invalid_request_error` because `thinking`/`redacted_thinking` blocks in the latest assistant message are mutated. High 👍 count shows broad impact on heavy Claude users.  
   https://github.com/earendil-works/pi/issues/5223

4. **#6665 — TUI pins a full core while streaming: uncached Intl.Segmenter + per-chunk Markdown rebuild** — *12 comments, 3 👍*  
   Long sessions peg one core at ~100% during streaming; root cause identified as uncached grapheme segmentation plus per-chunk Markdown re-rendering. Reproducible with `pi -ne`.  
   https://github.com/earendil-works/pi/issues/6665

5. **#7850 — GitHub Copilot login fails with 429 (rate limiting) for organizations with many activated models** — *9 comments, 7 👍*  
   Organizations with 20+ available models hit `429 Too Many Requests` during Copilot login, blocking device authorization completion. The highest 👍 count of the day.  
   https://github.com/earendil-works/pi/issues/7850

6. **#5023 — Terminal scrolls to beginning without reason** — *12 comments, 2 👍*  
   Randomly, the terminal jumps to the start of the session and fast-scrolls to the end while the model is working — a disruptive TUI UX bug with no clear trigger.  
   https://github.com/earendil-works/pi/issues/5023

7. **#8092 — Extension loader fails to resolve declared dependencies of extensions installed with pnpm (jiti + isolated node_modules layout)** — *5 comments*  
   pnpm's isolated `node_modules` layout breaks jiti's resolver when loading extensions, forcing users back to git installs. Fixed by PR #8112.  
   https://github.com/earendil-works/pi/issues/8092

8. **#7761 — TUI copy shows "Copied!" but clipboard stays empty on VTE terminals** — *3 comments*  
   Double-click selection flashes "Copied!" but writes nothing to the system clipboard — the OSC 52 sequence is ignored on GNOME Terminal/Tilix. Addressed by PR #8110.  
   https://github.com/earendil-works/pi/issues/7761

9. **#8036 — Edit tool crashes TUI when rendering a large diff during execution and session resume** — *2 comments*  
   A ~14.5 MB diff from HTML files with very long lines crashes the interactive TUI, both during execution and on session resume. Stability bug for large-file workflows.  
   https://github.com/earendil-works/pi/issues/8036

10. **#7724 — Cold restore replays an overflow assistant removed by live recovery** — *2 comments*  
   After context overflow compaction and successful retry, reopening the session re-adds the failed/truncated assistant response to history — a correctness bug in the compaction recovery path.  
    https://github.com/earendil-works/pi/issues/7724

## Key PR Progress

1. **#8139 — feat(ai): add ChatGPT OAuth image generation** — New native image-generation transport reusing OpenAI Codex OAuth and Responses infra; enables image generation/editing via ChatGPT entitlement with no API key.  
   https://github.com/earendil-works/pi/pull/8139

2. **#8124 — feat(ai): route xAI models through Responses and default to Grok 4.6** — Switches xAI from completions to the Responses API and bumps the default model from Grok 4.5 to 4.6.  
   https://github.com/earendil-works/pi/pull/8124

3. **#8120 — feat(coding-agent): add experimental append compaction** — With `PI_EXPERIMENTAL=1`, append mode reuses the active system prompt, tools, and routing session so the compacted prefix can reuse provider prompt caches.  
   https://github.com/earendil-works/pi/pull/8120

4. **#8110 — fix(tui): route selection copy through the host clipboard so "Copied!" is truthful** — Replaces the bare OSC 52 write with host-clipboard routing; fixes the empty-clipboard bug on VTE terminals, Terminal.app, and tmux without OSC 52 passthrough.  
   https://github.com/earendil-works/pi/pull/8110

5. **#8112 — fix(coding-agent): realpath extension entries before jiti import (closes #8092)** — Resolves pnpm isolated `node_modules` layout by realpath-ing extension entries before jiti resolution.  
   https://github.com/earendil-works/pi/pull/8112

6. **#8119 — fix: track kimi cached tokens** — Treats Kimi's top-level `usage.cached_tokens` as cache-read input tokens instead of counting them as normal input; addresses #8075.  
   https://github.com/earendil-works/pi/pull/8119

7. **#8113 — feat(ai): add SiliconFlow provider** — New built-in OpenAI-compatible provider for SiliconFlow (`https://api.siliconflow.com/v1`), following the moonshot/minimax patterns.  
   https://github.com/earendil-works/pi/pull/8113

8. **#5262 — feat(ai): add Anthropic Vertex provider** — Thin adapter constructing an `AnthropicVertex` SDK client and injecting it into the existing Anthropic Messages streaming path; Claude on GCP Vertex AI as a built-in provider.  
   https://github.com/earendil-works/pi/pull/5262

9. **#6216 — feat: Add Amazon Bedrock Mantle OpenAI Responses provider** — New provider for Bedrock Mantle's OpenAI Responses API using OpenAI's Bedrock provider, superseding an earlier effort.  
   https://github.com/earendil-works/pi/pull/6216

10. **#8109 — fix(ai): detect api.kimi.com as a Moonshot endpoint** — Fixes `role 'developer' is not allowed` 400 errors by extending Moonshot compat detection to `api.kimi.com` (Kimi Coding).  
    https://github.com/earendil-works/pi/pull/8109

Also noteworthy: **#8146** caps Baseten DeepSeek V4 Flash output at 384k tokens to match service limits, and **#8123** fixes `registerFlag()` allowing `boolean` flags with string defaults that made omitted flags truthy.

## Feature Request Trends

- **Windows & WSL first-class support** — The #7547 feedback thread plus WSL login hangs (#6187) and Windows Unix-socket test failures (#8047) point to Windows as the biggest open platform gap.
- **Scriptability / CI-CD usage** — Requests to run Pi with only CLI args or env vars, no config file (#8114), mirror a desire for headless, pipeline-friendly operation.
- **Per-model configuration** — Compaction profiles keyed by model ID (#8133), thinking-level mapping fixes (#8135), and session-only model state for extensions (#8100) all push toward finer-grained per-model/per-session control.
- **Input UX improvements** — Skill-name autocomplete mid-prompt (#8144) and configurable command-autocomplete popup position (#8132) show continued polish demand on the TUI prompt.
- **Provider catalog accuracy** — Removed model references in Z.AI defaults (#8096), Baseten output cap mismatches (#8147), and Kimi endpoint detection (#8109) highlight the need for catalog upkeep aligned with models.dev.

## Developer Pain Points

- **Copilot login rate-limiting** — Multiple reports of `429 Too Many Requests` during Copilot login (#7850, #8010), especially in enterprise orgs with many enabled models; a blocking onboarding issue.
- **TUI performance and stability** — Full-core CPU pinning during streaming (#6665), random scroll jumps (#5023), crashes rendering large diffs (#8036), and misleading "Copied!" feedback (#7761) make the TUI the top source of UX frustration.
- **Authentication flow gaps** — WSL login hangs after device authorization (#6187) and Anthropic OAuth refresh crashes on undefined signal (#8131) indicate auth paths need hardening across environments.
- **Provider compatibility friction** — Anthropic thinking-block mutation breaking Opus 4.8 (#5223), reasoning-only responses bypassing retry (#8115), `strict: null` making optional tool params required (#8105), and WebSocket failures pinning sessions to SSE (#8125) show the cost of a multi-provider matrix.
- **Extension ecosystem rough edges** — pnpm-installed extensions failing to resolve dependencies (#8092) and `registerFlag()` type confusion (PR #8123) create friction for extension authors and users alike.
- **Proxy and transport issues** — Plain-HTTP providers hanging after the first tool call when behind a forward proxy (#8134) and transient WebSocket failures permanently downgrading sessions (#8125) point to networking edge cases needing attention.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-08-15

## Today's Highlights

Qwen Code shipped **v0.21.12**, adding Web Shell workspace file uploads via drag-and-drop or the `@` file panel with progress tracking, plus a diff-growth brake for autofix review loops to prevent runaway review rounds. Previews also fixed Web Shell standalone session target preservation. Meanwhile, community attention remains on the image-load crash regression ([#8957](https://github.com/QwenLM/qwen-code/issues/8957)) and headless-mode failures caused by quiet post-tool-result completions, now addressed in [PR #9196](https://github.com/QwenLM/qwen-code/pull/9196).

## Releases

New releases in the last 24 hours:

- **v0.21.12** — Latest stable release.
- **v0.21.12-preview.4** / **v0.21.12-preview.3** — Include [fix(web-shell): preserve standalone session target](https://github.com/QwenLM/qwen-code/pull/9038) and [feat(web-shell): support workspace file uploads](https://github.com/QwenLM/qwen-code/pull/8874).
- **v0.21.11-nightly.20260814.45c2e73080** — Same Web Shell fixes.
- **dsw-eas-tb-e2e-20260814-r1/r2/r3/r6** — End-to-end validation releases for SWE-bench Verified and Terminal-Bench 2.0.

A publish failure occurred for **v0.21.12-preview.2** ([#9137](https://github.com/QwenLM/qwen-code/issues/9137)).

## Hot Issues

1. **Image-load crash regression since v0.21.2** — [#8957](https://github.com/QwenLM/qwen-code/issues/8957)  
   Qwen Code crashes when reading images; v0.21.1 is cited as the last working version. 12 comments and high community visibility.

2. **Read-only shell classifier auto-approves hidden command substitution** — [#8582](https://github.com/QwenLM/qwen-code/issues/8582)  
   Security bug where line continuations and `${var@P}` evade read-only detection. Closed as P1; relevant to shell-safety boundaries.

3. **Preserve session when large restore times out** — [#8678](https://github.com/QwenLM/qwen-code/issues/8678)  
   Closed P1; request-scoped restore timeout and late-result safety were partially addressed but superseded by broader daemon work.

4. **Bound multi-workspace daemon resource usage** — [#8051](https://github.com/QwenLM/qwen-code/issues/8051)  
   Tracking issue: count-only workspace/session limits do not bound request bodies, WebSocket assembly, or other memory consumers.

5. **Headless runs fail with `NO_TOOL_RESULT_PROGRESS`** — [#9026](https://github.com/QwenLM/qwen-code/issues/9026)  
   Valid model completions after tool results are treated as stream errors, burning retries and aborting headless runs.

6. **ACP child process fails with "Unknown argument: acp"** — [#8871](https://github.com/QwenLM/qwen-code/issues/8871)  
   `qwen serve` spawns ACP with an unsupported flag, causing token auth failures and broken child-process behavior.

7. **Python SDK rejects `permission_mode="auto"`** — [#9002](https://github.com/QwenLM/qwen-code/issues/9002)  
   CLI supports the value but SDK client-side validation blocks it, creating an integration inconsistency.

8. **Unbounded memory growth in UI History** — [#2128](https://github.com/QwenLM/qwen-code/issues/2128)  
   Long-session memory never decreases because `useHistoryManager.history` grows without a limit. P1 performance concern.

9. **Core + CLI architecture review: 14 structural issues** — [#4063](https://github.com/QwenLM/qwen-code/issues/4063)  
   Community-authored review found 136 files directly import `@google/genai` types; includes P0 coupling concerns and dependency-direction issues.

10. **Status line context percentage not refreshed after `/compress`** — [#6806](https://github.com/QwenLM/qwen-code/issues/6806)  
   Footer shows stale token usage until the next model request; a UI correctness issue awaiting a welcome PR.

## Key PR Progress

1. **reject upstream fail-fast placeholder responses** — [#8938](https://github.com/QwenLM/qwen-code/pull/8938)  
   Adds defenses against upstream endpoints returning HTTP 200 with placeholder body text as a fast-fail signal.

2. **support session media references end-to-end** — [#9127](https://github.com/QwenLM/qwen-code/pull/9127)  
   Adds session-scoped media IDs across daemon, ACP bridge, TypeScript SDK, and Web Shell, avoiding repeated image uploads.

3. **accept quiet post-tool-result completions after retry exhaustion** — [#9196](https://github.com/QwenLM/qwen-code/pull/9196)  
   Fixes `NO_TOOL_RESULT_PROGRESS` false positives by tolerating valid silent finishes after tool results.

4. **plain-prose `/review` comments; severity markers follow attribution** — [#9027](https://github.com/QwenLM/qwen-code/pull/9027)  
   Makes review comments read naturally instead of template voice while preserving severity marking behavior.

5. **round-aware convergence posture for posted findings** — [#9118](https://github.com/QwenLM/qwen-code/pull/9118)  
   Raises the posting bar as review rounds accumulate, helping review→fix→re-review loops converge.

6. **Web Shell sidebar session management improvements** — [#9122](https://github.com/QwenLM/qwen-code/pull/9122)  
   Adds hover details, folder previews, overflow-fade titles, and clearer running-session states.

7. **DingTalk Workspace channel** — [#9049](https://github.com/QwenLM/qwen-code/pull/9049)  
   New built-in channel using existing DWS CLI profiles; supports DMs, mentions, document notifications, and todos.

8. **canonical Goal v3 controls in WebShell** — [#9087](https://github.com/QwenLM/qwen-code/pull/9087)  
   Adds create/inspect/edit/pause/resume/clear actions for goals without routing commands through the model.

9. **Kimi and Xiaomi MiMo auth providers** — [#8368](https://github.com/QwenLM/qwen-code/pull/8368)  
   First-class provider presets for `/auth`, including regional access choices.

10. **Bound ACP HTTP pre-attach buffers by bytes** — [#9007](https://github.com/QwenLM/qwen-code/pull/9007)  
   Moves ACP pre-attach buffer limits from count-based to byte-based, addressing a key daemon resource issue.

## Feature Request Trends

- **Daemon resource governance**: Requests to bound `qwen serve` memory/bytes, not just workspace/session counts ([#8051](https://github.com/QwenLM/qwen-code/issues/8051), [#9007](https://github.com/QwenLM/qwen-code/pull/9007)).
- **Web Shell and desktop evolution**: Sidebar management, Channel policy redesign, Electron host evaluation, and HTML export via `WebShellTranscript` ([#9122](https://github.com/QwenLM/qwen-code/pull/9122), [#8845](https://github.com/QwenLM/qwen-code/issues/8845), [#9168](https://github.com/QwenLM/qwen-code/issues/9168), [#9186](https://github.com/QwenLM/qwen-code/issues/9186)).
- **Architecture decoupling**: Repeated asks to remove ACP/serve coupling, make `utils/` a leaf layer, and reduce `@google/genai` dependency penetration ([#4063](https://github.com/QwenLM/qwen-code/issues/4063), [#8084](https://github.com/QwenLM/qwen-code/issues/8084), [#9146](https://github.com/QwenLM/qwen-code/issues/9146)).
- **Session media and memory lifecycle**: End-to-end media references and bounded long-session history ([#9127](https://github.com/QwenLM/qwen-code/pull/9127), [#2128](https://github.com/QwenLM/qwen-code/issues/2128)).
- **More platform integrations**: DingTalk Workspace, Kimi, Xiaomi MiMo, and continued channel/provider expansion ([#9049](https://github.com/QwenLM/qwen-code/pull/9049), [#8368](https://github.com/QwenLM/qwen-code/pull/8368)).

## Developer Pain Points

- **Regression risk around attachments/images**: The v0.21.2 image-load crash is the most commented issue, indicating insufficient attachment regression coverage ([#8957](https://github.com/QwenLM/qwen-code/issues/8957)).
- **Headless/non-interactive flakiness**: `NO_TOOL_RESULT_PROGRESS` aborts and retry exhaustion are a recurring source of automation failures ([#9026](https://github.com/QwenLM/qwen-code/issues/9026), [#9196](https://github.com/QwenLM/qwen-code/pull/9196)).
- **CLI/SDK inconsistency**: Option validation differences, such as `permission_mode="auto"`, break SDK users ([#9002](https://github.com/QwenLM/qwen-code/issues/9002)).
- **Long-session memory growth**: Unbounded UI history and daemon resources remain a concern for production users ([#2128](https://github.com/QwenLM/qwen-code/issues/2128), [#8051](https://github.com/QwenLM/qwen-code/issues/8051)).
- **Security boundary scrutiny**: Shell classifier bypasses and PAT-bearing runner isolation are receiving focused attention ([#8582](https://github.com/QwenLM/qwen-code/issues/8582), [#9089](https://github.com/QwenLM/qwen-code/issues/9089)).
- **CI/release pipeline fragility**: Main-branch E2E failures, release publish failures, and stale branch push issues keep interrupting delivery ([#9143](https://github.com/QwenLM/qwen-code/issues/9143), [#9137](https://github.com/QwenLM/qwen-code/issues/9137), [#9082](https://github.com/QwenLM/qwen-code/pull/9082)).

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-08-15

> **Note on naming:** The project formerly known as `DeepSeek-TUI` has shipped v0.9.8 under its new public identity **Codewhale** (Shannon Labs). The legacy `deepseek-tui` npm package is deprecated. Issue/PR links below follow the current `Hmbown/CodeWhale` repository.

---

## 1. Today's Highlights

v0.9.8 ships as the project formally rebrands to **Codewhale** under Shannon Labs, deprecating the legacy `deepseek-tui` npm package. The release is overshadowed by three red-main reports: provider-count assertion drift ([#5383](https://github.com/Hmbown/CodeWhale/issues/5383)), nine reasoning-effort tests failing on macOS/Windows ([#5377](https://github.com/Hmbown/CodeWhale/issues/5377)), and a P0 audit declaring the public web UI "totally broken" ([#5370](https://github.com/Hmbown/CodeWhale/issues/5370)). In parallel, two correctness fixes landed — serialized session-index writes to prevent silent data loss ([#5382](https://github.com/Hmbown/CodeWhale/pull/5382)) and a non-panicking webhook HTTP client builder ([#5381](https://github.com/Hmbown/CodeWhale/pull/5381)).

---

## 2. Releases

### [v0.9.8](https://github.com/Hmbown/CodeWhale/releases) — Codewhale rebrand
- **Codewhale** is now the public product from **Shannon Labs**; the `codewhale` command, npm package, and release-asset names remain lowercase technical identifiers.
- The legacy npm package `deepseek-tui` is **deprecated** and receives no further releases; v0.8.x legacy `deepseek`/`d…` users are expected to migrate to the `codewhale` package.
- The release-gate close-out also drove the known-issue basket in [#5355](https://github.com/Hmbown/CodeWhale/issues/5355) (parallel-load and config-fixture flakes) into the v0.9.8 tracking cycle.

---

## 3. Hot Issues

| # | Issue | Why it matters |
|---|-------|----------------|
| [#5370](https://github.com/Hmbown/CodeWhale/issues/5370) | **P0: web UI looks broken — audit and rebuild** | Maintainer-reported regression affecting both the public `web/` Next.js app (codewhale.net) and the managed CWC app; scoping the "look/features" gap against harness references is the top open P0. |
| [#5383](https://github.com/Hmbown/CodeWhale/issues/5383) | **main is red on v0.9.8: provider-count assertions** | Two CLI assertions still hold pre-release registry numbers (43 vs. 45 / 38 vs. 40); a quick repro surfaced by `Lstarsky0`, already fixed by PR [#5384](https://github.com/Hmbown/CodeWhale/pull/5384). |
| [#5377](https://github.com/Hmbown/CodeWhale/issues/5377) | **main red on macOS/Windows: reasoning-effort ladder tests** | Nine tests assert the pre-ladder vocabulary; not a flake — bisects to one commit and reproduces every run; fixed in [#5378](https://github.com/Hmbown/CodeWhale/pull/5378). |
| [#5324](https://github.com/Hmbown/CodeWhale/issues/5324) | **Agent tool: simplify the 32-field schema** | A 32-property JSON schema with zero required fields and 8 actions is error-prone for models; the community requested schema degradation/acceptance of conditionals (see PR [#5369](https://github.com/Hmbown/CodeWhale/pull/5369)). |
| [#5340](https://github.com/Hmbown/CodeWhale/issues/5340) | **doctor: `first-run` / `update checkpoint` stuck on `needs action`** | Survives a fresh onboarding wizard after v0.9.4 → v0.9.6; setup checklist can never complete — a workflow-blocking UX bug for upgraders. |
| [#5374](https://github.com/Hmbown/CodeWhale/issues/5374) | **Agent writing is corrupted on macOS** | User-reported text corruption during agent streaming ("the writing its weird"); a rendering bug that makes output unreadable, with a screenshot attached. |
| [#5322](https://github.com/Hmbown/CodeWhale/issues/5322) | **Output area doesn't fill wide terminals** | v0.8 filled terminal width; v0.9 caps at max width, leaving cramped text and unused whitespace on wide displays — a clear v0.8 → v0.9 regression. |
| [#1004](https://github.com/Hmbown/CodeWhale/issues/1004) | **`/dryrun` — preview the next chat completion request** | High-value workflow request: see the exact long system prompt, cached repo files, tool definitions, and multi-step thinking before paying for a V4 Pro turn. 9 comments, still open. |
| [#3192](https://github.com/Hmbown/CodeWhale/issues/3192) | **Register with agentclientprotocol/registry** | Community asks to list Codewhale in the ACP registry to make Zed installation one-click; 13 comments — the most-discussed issue this cycle. |
| [#4326](https://github.com/Hmbown/CodeWhale/issues/4326) | **Perf: bound RSS after 32-worker storm cancellation** | The 32-worker PTY benchmark shows high fan-out is usable, but RSS grows again after cancel; needs allocator high-water vs. real leak diagnosis. |

---

## 4. Key PR Progress

| # | PR | Description |
|---|----|-------------|
| [#5384](https://github.com/Hmbown/CodeWhale/pull/5384) | `test(cli): re-pin the provider-count assertions` | Closes [#5383](https://github.com/Hmbown/CodeWhale/issues/5383); two-integer fix updating registry (43→45) and catalog (38→40) assertions to v0.9.8. |
| [#5382](https://github.com/Hmbown/CodeWhale/pull/5382) | `fix(state): serialize session-index writes` | Closes [#5380](https://github.com/Hmbown/CodeWhale/issues/5380); moves `session_index.jsonl` append/compact/rename inside the `Arc<Mutex<Connection>>` lock to prevent silent data loss under cloned `StateStore`s. |
| [#5381](https://github.com/Hmbown/CodeWhale/pull/5381) | `fix(hooks): do not panic on webhook HTTP client failure` | Closes [#5379](https://github.com/Hmbown/CodeWhale/issues/5379); replaces the `.expect("build fallback HTTP client")` panic path with graceful error handling. |
| [#5378](https://github.com/Hmbown/CodeWhale/pull/5378) | `test(tui): re-pin the thinking-ladder assertions` | Closes [#5377](https://github.com/Hmbown/CodeWhale/issues/5377); nine test-only updates, no production changes. |
| [#5376](https://github.com/Hmbown/CodeWhale/pull/5376) | `fix(tui): keep internal runtime events out of the session peek` | Closes [#5375](https://github.com/Hmbown/CodeWhale/issues/5375); prevents internal runtime envelopes from leaking into user-visible session projections. |
| [#5365](https://github.com/Hmbown/CodeWhale/pull/5365) | `feat(provider): first-class local DS4 setup` | Makes DwarfStar (DS4) a first-class local DeepSeek route with a prefilled keyless loopback preset and OpenAI-compatible transport reuse — no new protocol adapter. |
| [#5353](https://github.com/Hmbown/CodeWhale/pull/5353) | `feat(tui): model guardian tier for Auto-Review` | Auto-Review becomes a true two-layer mode: non-bypassable deterministic floor + one-shot model guardian escalation on fallback holds (Codex reviewer semantics, Kimi vocabulary, fail-closed defaults). |
| [#5358](https://github.com/Hmbown/CodeWhale/pull/5358) | `feat(engine): auto-review denial rationale + turn circuit breaker` | First P0 slice of [#5352](https://github.com/Hmbown/CodeWhale/issues/5352); blocks now carry rationale through `permission_denied` and the circuit breaker stops re-phrase loops on the same denied action. |
| [#5369](https://github.com/Hmbown/CodeWhale/pull/5369) | `fix(tools): degrade Moonshot schemas instead of refusing conditionals` | Prerequisite for the [#5324](https://github.com/Hmbown/CodeWhale/issues/5324) schema cleanup; keeps the schema slice purely about net-negative accounting. |
| [#5364](https://github.com/Hmbown/CodeWhale/pull/5364) | `feat(tui): render markdown blockquotes with a quote rail` | Community PR from `SparkofSpike`: proper quote-rail rendering with nesting, inline formatting, wrapping, and correct selection-copy. |

Also merged this cycle: [#5339](https://github.com/Hmbown/CodeWhale/pull/5339) suppresses child-owned shell completion events (closes [#5325](https://github.com/Hmbown/CodeWhale/issues/5325)); [#5368](https://github.com/Hmbown/CodeWhale/pull/5368) confines unguarded tests to the isolated state root; routine dependency bumps via dependabot ([#5387](https://github.com/Hmbown/CodeWhale/pull/5387) tower-http 0.7.0, [#5388](https://github.com/Hmbown/CodeWhale/pull/5388) ratatui 0.30.2, [#5389](https://github.com/Hmbown/CodeWhale/pull/5389) thiserror 2.0.20, [#5390](https://github.com/Hmbown/CodeWhale/pull/5390) rmcp 3.1.2, [#5391](https://github.com/Hmbown/CodeWhale/pull/5391) rusqlite 0.40.2).

---

## 5. Feature Request Trends

- **Agent ecosystem integration** — listing in the [agentclientprotocol registry](https://github.com/Hmbown/CodeWhale/issues/3192) for one-click Zed install; a Kimi-level plugin system with federated marketplaces ([#5311](https://github.com/Hmbown/CodeWhale/issues/5311)).
- **Request transparency** — `/dryrun` to preview the exact outgoing completion payload before spending a V4 Pro turn ([#1004](https://github.com/Hmbown/CodeWhale/issues/1004)).
- **TUI UX hardening** — configurable deny-by-default approval selection ([#5293](https://github.com/Hmbown/CodeWhale/issues/5293)), proactive update notice + one-chord update-and-relaunch ([#5053](https://github.com/Hmbown/CodeWhale/issues/5053)), and fleet/session-name display identity for sub-agents ([#5287](https://github.com/Hmbown/CodeWhale/issues/5287)).
- **Provider onboarding ergonomics** — pre-built templates with fixed Base URLs, model lists, and inline docs for third-party compatible services ([#5350](https://github.com/Hmbown/CodeWhale/issues/5350)); first-class local DS4 setup landed in [#5365](https://github.com/Hmbown/CodeWhale/pull/5365).
- **Tool-schema simplification** — reduce the 32-field, zero-required agent schema so models stop erroring ([#5324](https://github.com/Hmbown/CodeWhale/issues/5324)).

---

## 6. Developer Pain Points

- **Red main / CI instability** — recurring assertion drift against shipped registry numbers and reasoning-vocabulary changes ([#5383](https://github.com/Hmbown/CodeWhale/issues/5383), [#5377](https://github.com/Hmbown/CodeWhale/issues/5377)), plus the parallel-load/config-fixture flake basket ([#5355](https://github.com/Hmbown/CodeWhale/issues/5355)).
- **Rendering regressions** — corrupted agent text on macOS ([#5374](https://github.com/Hmbown/CodeWhale/issues/5374)) and the wide-terminal output cap regression ([#5322](https://github.com/Hmbown/CodeWhale/issues/5322)).
- **Setup/doctor reliability** — `first-run`/`update checkpoint` permanently stuck on `needs action` after upgrading ([#5340](https://github.com/Hmbown/CodeWhale/issues/5340)); third-party model lists stuck on `not checked`/`cache failed` ([#5350](https://github.com/Hmbown/CodeWhale/issues/5350)).
- **Concurrency and data-safety bugs** — unsynchronized `session_index.jsonl` writes causing silent data loss ([#5380](https://github.com/Hmbown/CodeWhale/issues/5380)), stale write-claims from closed sessions blocking new sub-agents ([#5372](https://github.com/Hmbown/CodeWhale/issues/5372)), and output-token ceilings clamped below the documented catalogue limit killing turns ([#5373](https://github.com/Hmbown/CodeWhale/issues/5373)).
- **Performance introspection** — RSS not settling after cancelling a 32-worker storm, requiring allocator high-water vs. leak analysis ([#4326](https://github.com/Hmbown/CodeWhale/issues/4326)).
- **Third-party provider friction** — nVidia NIM returns 404 on API calls ([#1482](https://github.com/Hmbown/CodeWhale/issues/1482)) and manual Base URL/model/key configuration remains error-prone ([#5350](https://github.com/Hmbown/CodeWhale/issues/5350)).

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

No activity in the last 24 hours.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*