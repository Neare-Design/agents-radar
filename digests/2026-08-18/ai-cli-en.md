# AI CLI Tools Community Digest 2026-08-18

> Generated: 2026-08-17 23:16 UTC | Tools covered: 10

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

# Cross-Tool Comparison Report — AI CLI Developer Tools
**Snapshot date: 2026-08-18 · Window: last 24 hours per project digest**

---

## 1. Ecosystem Overview

The AI CLI landscape entered a hardening phase: the most prominent activity across all eight active projects is reliability work — subagent truthfulness, context/compaction management, MCP lifecycle fixes, and TUI input regressions — rather than new model capabilities. Release cadence remains healthy for the big vendors (Claude Code v2.1.234, Qwen Code v0.21.13, Codex alpha, Gemini nightly), while open-source challengers like OpenCode iterate fastest on feature surface. The highest-community-voted requests across trackers are about control and UX, not raw model power: queueing instead of interrupting, disabling auto-resolve, and honest completion reporting. Overall, the ecosystem is converging on "agents as durable, always-available infrastructure," but operational trust has not yet caught up with capability.

---

## 2. Activity Comparison

| Tool | Issue activity (24h) | PR activity (24h) | Release status |
|---|---|---|---|
| Claude Code | 10 hot issues¹ | 10 PRs | ✅ v2.1.234 |
| OpenAI Codex | 10 hot issues¹ | 10 PRs | ✅ rust-v0.148.0-alpha.21 |
| Gemini CLI | 10 hot issues¹ | 10 PRs | ✅ v0.56.0-nightly |
| GitHub Copilot CLI | 29 issues updated | 1 PR | — |
| Kimi Code CLI | 0 issues updated | 1 PR (closed) | — |
| OpenCode | 50 issues updated | 50 PRs updated | — |
| Pi | 10 hot issues¹ | 10 PRs | — (model catalog updates) |
| Qwen Code | 10 hot issues¹ | 10 PRs | ✅ v0.21.13 + nightly |
| DeepSeek-TUI (CodeWhale) | 10 hot issues¹ | 10 PRs | ✅ v0.9.9 release PR closed |
| Grok Build | 0 | 0 | — |

¹ Digest curates top 10; total tracker activity may be higher. Copilot and OpenCode report direct totals.

**Interpretation:** OpenCode has the busiest tracker (50/50) with no release — a heavy V2 refactor phase. Copilot shows high issue inflow but near-zero PR output, suggesting a maintainer backlog. Claude, Codex, Gemini, and Qwen all shipped code in the window. Kimi and Grok are effectively dormant in public channels.

---

## 3. Shared Feature Directions

**Non-interrupting, queue-based interaction.** Claude Code's top-voted UX request (message queue mode, #50246, 198 👍) was closed after sustained demand; Codex shipped `codex queue --thread` for async message injection (#39092); OpenCode closed a PR adding `/loop` for autonomous continuation (#37504). The complementary bug is also everywhere: Claude's Esc key mis-routed to tool prompts (#64568), and Pi's extension lifecycle fix for `agent_settled` (#8242) so "ready for input" actually means something.

**Multi-agent observability and truthful completion.** Gemini's P1 issue — subagents reporting `MAX_TURNS` as clean `GOAL` success (#22323, fixed in #28815) — is the starkest example of a cross-industry problem. Codex added a full `/agents` TUI dashboard (#39094), traced exec-server requests end-to-end (#39098), and still has background subagents not waking the parent (#15723). Claude's multi-machine A2A protocol ask remains a top open thread (#28300). DeepSeek simplified a 32-field agent-tool schema that models kept erroring on (#5324).

**MCP reliability and policy inheritance.** Every vendor has a different MCP gap: Copilot's strict OAuth metadata validation broke Atlassian and GitLab MCP servers (#4480, #4439); Codex cannot refresh routed MCP OAuth tokens (#17265, 57 👍); Gemini added consent checks for MCP server env changes (#28863); OpenCode has MCP tools connected but invisible to the agent (#33027); Claude's MCP dialogs clip in fullscreen TUI (#69087). The protocol is winning, but its lifecycle management is the industry's weakest link.

**Context/compaction reliability and cost visibility.** Pi's top issue is auto-compaction never triggering until API hard-reject (#6879, 17 👍), with an experimental append-compaction fix that preserves prompt caches (#8120). Copilot's memory watchdog compacts at 23% context usage, recovers 0.003% of tokens, and loops until OOM (#4506). Qwen reports context loss after `/compress-fast` + resume (#9320); OpenCode's compactor fires false usage-limit errors (#41990). Cost is the companion ask: Claude's `claude usage` consolidates 10+ issues (#33978), Pi flags a 2.5× cost penalty from missing Anthropic-style `cache_control` (#7995), and DeepSeek reworked V4 peak/off-peak pricing with honest fallback on endpoint 503s (#5470, #5402).

**Terminal input regressions and TUI polish.** Basic input bugs dominate cross-tool traffic: Codex backspace deletes multiple characters (#17793); Qwen Ctrl+V is broken on Windows (#9061); OpenCode's Windows ARM64 TUI crashes at init, open since March (#19130); Gemini's TUI hangs on "Initializing..." (#28812); Pi's prompt editor costs 1,650 ms/keypress at 7,000 lines (#8029); Copilot's Shift+Enter executes instead of inserting a newline (#1481, 17 👍). Models are converging; terminal UX is where products differentiate.

**Session portability and lifecycle management.** OpenCode users want unarchive/restore (#24153, 11 👍) and pause/resume; Codex has mobile-created threads disassociated from projects (#23418) and 100+ GiB session-fork bloat (#34268); Qwen is proposing a cross-host transcript contract (#9354); DeepSeek is making approval receipts durable before execution, fail-closed (#5491); Claude treats sessions as durable artifacts (persistent env defaults #87398, network-drive session lists #78461).

---

## 4. Differentiation Analysis

**Claude Code** is the most enterprise-integration-oriented: its PRs are largely plugin-developer tooling (`test-hook.sh --expect`, frontmatter parsing limits, duplicate-issue detection), hook-based guardrails (container examples, destructive-git protection), and permissions governance. Target: teams that need scriptable, governed agent infrastructure with audit hooks.

**OpenAI Codex** is the most platform-ambitious: Rust-native CLI, `/agents` dashboard, queue API, OTLP telemetry exports, remote/mobile→Desktop session continuity, and GPT-5.6 context-window overrides up to 872k tokens. Target: platform teams and power users building workflows around the agent rather than inside it.

**Gemini CLI** is the most correctness- and safety-focused: P1 issues center on truthful status reporting, silent hangs, and permission surprises; PRs add component-level evals, deterministic transcript redaction before model calls, extension env-var consent, and a supply-chain RCE fix in CI workflows. Target: Google Cloud/Workspace users and safety-conscious organizations.

**GitHub Copilot CLI** is the most conservative and policy-driven: no release, one unexplained PR (README doc removal), and an issue tracker centered on MCP OAuth regressions, org-enabled model catalog gaps, and SDK-server auth failures. Target: GitHub Enterprise teams where policy compliance outweighs feature velocity.

**OpenCode** is the most community-driven OSS builder: highest raw activity (50/50), a V2 beta unifying TUI/desktop/web on shared server data, declarative `/workflow` YAML pipelines, and a Plan→Build auto-switch request with 32 👍. Target: fast-moving OSS teams wanting a hackable, mode-based agent.

**Pi** is the most extension-author- and provider-compat-driven: heavy model catalog churn (Xiaomi, Qwen, ZAI/GLM), OpenRouter 870-trial benchmark fallout fixes, rich extension lifecycle events, and multimodal inputs. Target: developers building on the extension API and multimodal agentic workflows.

**Qwen Code** is the most infrastructure/service-oriented: daemon resource governance, scheduled tasks bound to existing sessions, Weixin channel integration with typing-indicator heartbeats, and a self-improving review/autofix pipeline validated against SWE-bench and Terminal-Bench. Target: teams running agents as a service with CI/review automation, especially in the Chinese ecosystem.

**DeepSeek-TUI (CodeWhale)** is the most "truth-and-resilience"-driven: honest labeling of unverified context windows and pricing, durable approval receipts, shell execution that fails soft instead of wedging sessions, and a Chinese-first i18n refactor. Target: cost-sensitive bilingual users and self-hosters.

---

## 5. Community Momentum & Maturity

- **Rapid, continuous shippers:** Qwen (stable + nightly + 10 PRs), Codex (10 PRs + alpha build), Gemini (10 PRs + nightly), and Claude (10 PRs + stable) all demonstrate healthy release cadence and responsive maintainers.
- **High energy, pre-release churn:** OpenCode leads raw volume (50/50) but is mid-refactor on V2 — architecture PRs (shared server data) outnumber user-facing fixes.
- **Maintainer backlog risk:** Copilot's 29 issues vs. 1 PR signals either triage debt or a deliberate slow lane; the lone PR (README doc removal) is unexplained and potentially a regression.
- **Maturity markers:** Claude's plugin-dev tooling (hook decision assertions, dependency fail-fast) indicates ecosystem governance; Gemini's eval expansion epic (#24353) shows regression-prevention investment; Qwen's mutation-verified test-pin review (#9194) is a sophisticated CI practice most tools lack.
- **Honest-but-red flags:** CodeWhale has red CI on both platforms (#5403) and flaky tests (#5056) during a release close-out. Kimi and Grok Build are effectively quiet, which may mean dormancy or activity outside public trackers.

---

## 6. Trend Signals

1. **The "always-available copilot" is the next UX frontier.** Message queues, non-interrupting input, and async handoffs are the top-voted UX asks (Claude #50246, Codex queue, OpenCode /loop). Expect queue/async primitives in every major CLI within a few quarters.

2. **Multi-agent is mainstream; multi-agent trust is not.** Subagents report false success (Gemini #22323), don't wake parents (Codex #15723), inherit wrong intent when forked (Codex #13491), and multiply session storage to 100+ GiB (#34268). Dashboards and lifecycle events are wave one; intent isolation and policy inheritance are wave two.

3. **MCP lifecycle is the industry's weakest integration link.** OAuth refresh, server process reaping, BigInt serialization, and policy inheritance each break in a different tool. The protocol won; the operational tooling around it is 12–18 months behind.

4. **Memory/context management is the #1 operational risk.** OOMs, compaction loops, false compaction, and context loss on resume appear in nearly every tracker. Tools that make compaction deterministic and cache-aware (Pi #8120, Qwen dashboards) will own the long-session workload.

5. **Windows remains systematically underserved.** GPU-crash bricking (Claude #80444), ARM64 TUI init crashes (OpenCode #19130), paste regressions (Qwen #9061), sandbox reparse-point issues (Codex #39083) — every vendor is patching Windows issues reactively rather than testing Windows natively.

6. **Cost transparency is becoming a first-class feature.** Usage analytics (#33978), honest fallback pricing (#5402), per-turn peak/off-peak tiers (#5470), and cache-control cost penalties (#7995) show enterprise procurement pressure is reaching the CLI layer.

7. **Security hardening is moving from docs to code.** OS sandboxing epics (Gemini #19873), container isolation examples (Claude #30692), supply-chain RCE fixes (Gemini #28740), env-var injection consent (#28863), and pre-redaction of transcripts before model calls (#26525) — security is now a feature, not a footnote.

8. **TUI quality is a competitive battleground.** The highest-reaction issues of the day are input bugs and rendering crashes, not model capability gaps. As model quality converges, terminal UX — keybindings, large-diff rendering, dialog handling — will decide which tool developers tolerate daily.

9. **Agents are becoming services, not sessions.** Daemon architectures (Qwen), SDK servers (Copilot #4503), remote control (Codex #37403), scheduled tasks (Qwen #9361), and queue APIs (#39092) all point to agents as always-on infrastructure with programmatic access, not just interactive REPLs. The CLI is becoming a façade over an agent platform.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights

*Source: github.com/anthropics/skills · Data as of 2026-08-18*

---

## 1. Top Skills Ranking

Most-watched PR activity, ranked from the comment-sorted PR list. All are currently **open**.

### 1. `skill-creator` evaluation loop fix — [PR #1298](https://github.com/anthropics/skills/pull/1298)
- **Skill/area:** `skill-creator` tooling
- **Functionality:** Fixes `run_eval.py` always reporting `recall=0%` by installing the eval artifact as a real skill; also fixes Windows stream reading, trigger detection, and parallel workers.
- **Discussion highlights:** Tied to [Issue #556](https://github.com/anthropics/skills/issues/556) (12 comments, 7 👍), with 10+ independent reproductions. The description-optimization loop is currently “optimizing against noise.”
- **Status:** open

### 2. Document typography skill — [PR #514](https://github.com/anthropics/skills/pull/514)
- **Skill/area:** document quality control
- **Functionality:** Prevents typographic issues in AI-generated documents: orphan word wrap, widow paragraphs, and numbering misalignment.
- **Discussion highlights:** Argues these defects affect nearly every generated document, but users rarely request typographic fixes, so a proactive skill is valuable.
- **Status:** open

### 3. PDF case-sensitivity fix — [PR #538](https://github.com/anthropics/skills/pull/538)
- **Skill/area:** `pdf` skill
- **Functionality:** Corrects 8 case-sensitive references in `skills/pdf/SKILL.md` (`REFERENCE.md` → `reference.md`, `FORMS.md` → `forms.md`).
- **Discussion highlights:** Breaks on case-sensitive filesystems; a small but important reliability fix for Linux/CI environments.
- **Status:** open

### 4. ODT / OpenDocument skill — [PR #486](https://github.com/anthropics/skills/pull/486)
- **Skill/area:** OpenDocument formats
- **Functionality:** Create, fill, read, and convert ODT/ODS/ODF files; includes template filling and ODT-to-HTML parsing.
- **Discussion highlights:** Fills a gap for open-source/ISO-standard document workflows and LibreOffice users.
- **Status:** open

### 5. Frontend-design skill clarity pass — [PR #210](https://github.com/anthropics/skills/pull/210)
- **Skill/area:** `frontend-design`
- **Functionality:** Revises the skill so every instruction is actionable within a single conversation and behavior is steered more concretely.
- **Discussion highlights:** Focused on removing vague prose and improving internal coherence of the skill’s instructions.
- **Status:** open

### 6. Skill quality + security analyzers — [PR #83](https://github.com/anthropics/skills/pull/83)
- **Skill/area:** meta-skills for skill authors
- **Functionality:** Adds `skill-quality-analyzer` and `skill-security-analyzer`, evaluating structure, documentation, examples, and security posture.
- **Discussion highlights:** Represents community demand for “skills about skills” — validation, quality scoring, and security review.
- **Status:** open

### 7. DOCX tracked-change ID collision fix — [PR #541](https://github.com/anthropics/skills/pull/541)
- **Skill/area:** `docx` skill
- **Functionality:** Prevents document corruption by avoiding `w:id` collisions between bookmarks and tracked changes in OOXML.
- **Discussion highlights:** Root cause is a shared ID space; the skill’s hardcoded low IDs were unsafe.
- **Status:** open

### 8. `skill-creator` YAML validation — [PR #539](https://github.com/anthropics/skills/pull/539)
- **Skill/area:** `skill-creator` tooling
- **Functionality:** Adds pre-parse validation to warn on unquoted `description` fields containing YAML special characters like `:`.
- **Discussion highlights:** Prevents silent frontmatter truncation before `yaml.safe_load()`.
- **Status:** open

---

## 2. Community Demand Trends

From top issues, the community’s strongest demands are:

- **Security and trust boundaries** — [Issue #492](https://github.com/anthropics/skills/issues/492) (43 comments, 2 👍) is the highest-attention issue: community skills distributed under the `anthropic/` namespace create impersonation/trust-boundary risk. [Issue #412](https://github.com/anthropics/skills/issues/412) also proposes an `agent-governance` skill.
- **Skill authoring and eval reliability** — [Issue #556](https://github.com/anthropics/skills/issues/556) (12 comments, 7 👍) documents `run_eval.py` never triggering skills. [Issue #202](https://github.com/anthropics/skills/issues/202) argues `skill-creator` is too documentation-like and not operational enough.
- **Context-window efficiency** — [Issue #1329](https://github.com/anthropics/skills/issues/1329) proposes a `compact-memory` skill; [Issue #1487](https://github.com/anthropics/skills/issues/1487) reports the `claude-api` skill injecting ~156k tokens. Demand is clearly moving toward compact, context-budget-safe skills.
- **Enterprise sharing and platform support** — [Issue #228](https://github.com/anthropics/skills/issues/228) (16 comments, 8 👍) asks for org-wide skill sharing; [Issue #29](https://github.com/anthropics/skills/issues/29) asks about AWS Bedrock; [Issue #1175](https://github.com/anthropics/skills/issues/1175) raises SharePoint Online security concerns.
- **Packaging and deduplication** — [Issue #189](https://github.com/anthropics/skills/issues/189) (9 👍) reports duplicate skills when installing `document-skills` and `example-skills` together.
- **Reasoning/output quality gates** — [Issue #1385](https://github.com/anthropics/skills/issues/1385) proposes a three-gate pipeline: pre-task calibration → adversarial review → delivery verification.

---

## 3. High-Potential Pending Skills

Active, open PRs that may land soon:

- **ServiceNow platform skill — [PR #568](https://github.com/anthropics/skills/pull/568)**  
  Broad ServiceNow assistant covering ITSM, ITOM, ITAM/SAM, FSM, SecOps, CSDM, SPM, and IntegrationHub. Recently updated 2026-08-12.

- **Pyxel retro game development — [PR #525](https://github.com/anthropics/skills/pull/525)**  
  Workflow for `pyxel-mcp`: write → run_and_capture → inspect → iterate. Niche but active; updated 2026-07-15.

- **Self-audit skill — [PR #1367](https://github.com/anthropics/skills/pull/1367)**  
  Mechanical file verification plus a four-dimension reasoning quality gate. Aligned with [Issue #1385](https://github.com/anthropics/skills/issues/1385).

- **Testing-patterns skill — [PR #723](https://github.com/anthropics/skills/pull/723)**  
  Comprehensive testing skill: trophy model, unit-test patterns, React Testing Library usage, and edge-case guidance.

- **UIZZE partner skill — [PR #1595](https://github.com/anthropics/skills/pull/1595)**  
  Adds UIZZE to Partner Skills in the README; a simple docs/partner addition likely to merge quickly.

---

## 4. Skills Ecosystem Insight

The community’s most concentrated demand is not for new domain skills but for making Skills themselves **trustworthy, reliable, and context-efficient** — through better evaluation tooling, security/governance controls, validation, and packaging hygiene.

---

# Claude Code Community Digest — 2026-08-18

## Today's Highlights
Claude Code shipped v2.1.234 with a configurable per-project transcript directory name and a new `selection:clear` keybinding. The most-liked request in the tracker — message queue mode (#50246, 198 👍) — was closed after four months of discussion, while a fresh wave of memory-leak/OOM reports (#87238, #87319, #82179) plus a Windows desktop crash (#80444) dominate the bug conversation.

## Releases
**v2.1.234** — [Release link](https://github.com/anthropics/claude-code/releases)
- Added optional `CLAUDE_CODE_PROJECT_DIR_NAME` environment variable so hosts that give each session its own config directory can use a short name for the per-project transcript directory.
- Added the `selection:clear` keybinding action, allowing a key to be bound to clear an in-app selection.

## Hot Issues

1. **[#50246 — Message queue mode](https://github.com/anthropics/claude-code/issues/50246)** *(closed, 60 comments, 198 👍)* — The community's most-wanted UX improvement: queue follow-up messages instead of forcing an interrupt mid-task. Closed after sustained demand; users will be watching for the eventual implementation.

2. **[#80444 — Windows desktop GPU crash leaves app unlaunchable](https://github.com/anthropics/claude-code/issues/80444)** *(open, 39 comments)* — Fatal GPU-process crash (0x060C201E) from the in-app Browser tab puts the MSIX package in an unrepairable state (`appxState=2`) until a manual Repair. Reproduced on two NVIDIA driver versions.

3. **[#28300 — Multi-agent collaboration across machines](https://github.com/anthropics/claude-code/issues/28300)** *(open, 38 comments)* — Request for an Agent-to-Agent protocol enabling distributed collaboration. Long-running thread with steady engagement signals real demand for multi-machine orchestration.

4. **[#33978 — Built-in usage analytics command](https://github.com/anthropics/claude-code/issues/33978)** *(open, 20 comments, 10 👍)* — Proposal for `claude usage`, consolidating 10+ open issues around token/cost visibility. A frequent companion ask to enterprise adoption.

5. **[#86298 — Windows cross-session messages silently dropped](https://github.com/anthropics/claude-code/issues/86298)** *(open, 13 comments)* — Regression since desktop app 1.28929.0: cross-session messages are held for an approval the UI never offers and expire after ~5 minutes. Severely disrupts multi-session workflows.

6. **[#64568 — Esc in /btw mode rejects pending tool prompt](https://github.com/anthropics/claude-code/issues/64568)** *(open, 10 comments, 9 👍)* — Pressing Esc to exit `/btw` mode instead declines the pending tool-use prompt. A classic modal-input-routing bug, reproduced on macOS.

7. **[#82179 — grep shim catastrophic backtracking → OOM](https://github.com/anthropics/claude-code/issues/82179)** *(open, reproduced)* — The Bash tool's ugrep-emulation shim hits catastrophic backtracking with `-o` + bounded quantifiers: 6.6 GB RSS on a 20 KB file. Highlights the risk of re-execing the CLI as a grep replacement.

8. **[#87238 — Per-tool helper leaks 11.6 GB, OOM-killed](https://github.com/anthropics/claude-code/issues/87238)** *(closed)* — Ephemeral per-tool-call helper process ballooned to 11.6 GB anon RSS in ~2 minutes during a sandboxed Bash command, hitting the cgroup ceiling. Closed quickly, but a worrying pattern.

9. **[#69087 — MCP elicitation dialog clipped in fullscreen TUI](https://github.com/anthropics/claude-code/issues/69087)** *(open, 2 👍)* — MCP form dialogs render action buttons below the viewport with no scrolling in fullscreen mode — a blocker for MCP-heavy TUI users on macOS.

10. **[#87185 — Raw markdown rendering root cause identified](https://github.com/anthropics/claude-code/issues/87185)** *(open)* — Sharp root-cause analysis: markdown detection only scans the first ~500 characters, so messages whose first markdown construct appears later render as raw text. Supersedes earlier intermittent-report analysis in #73322.

## Key PR Progress

1. **[#87395 — ralph-wiggum: use disable-model-invocation](https://github.com/anthropics/claude-code/pull/87395)** *(closed)* — Fixes the `/ralph-loop` self-invocation footgun: the plugin's `hide-from-slash-command-tool` frontmatter key was unsupported, so the model could loop without being asked. Now uses the proper `disable-model-invocation`.

2. **[#30692 — Container isolation example with guard hook](https://github.com/anthropics/claude-code/pull/30692)** *(closed)* — Adds `examples/container/` for running Claude Code in Podman/Docker with a `guard-destructive-git` PreToolUse hook catching force push, hard reset, `branch -D`, `rm -rf`, and PR merges.

3. **[#84004 — plugin-dev: limit frontmatter parsing](https://github.com/anthropics/claude-code/pull/84004)** *(closed)* — Parses only the opening YAML block instead of restarting at every `---`, so horizontal rules in Markdown bodies no longer corrupt settings parsing.

4. **[#83992 — plugin-dev: assert expected hook decision](https://github.com/anthropics/claude-code/pull/83992)** *(closed, fixes #83800)* — Adds `--expect allow|deny|ask` to `test-hook.sh`, letting tests catch hooks that allow operations they were meant to deny.

5. **[#79131 — validate-settings.sh: don't abort on no lowercase key match](https://github.com/anthropics/claude-code/pull/79131)** *(open)* — `grep` returning 1 under `set -euo pipefail` silently killed the script; also reports skipped mixed-case/hyphenated keys.

6. **[#72451 — Remove statsig.anthropic.com from init-firewall.sh](https://github.com/anthropics/claude-code/pull/72451)** *(closed)* — The hostname no longer resolves, which broke devcontainer startup when the firewall script failed on DNS lookup.

7. **[#29284 — docs: clarify excludedCommands requires :* suffix](https://github.com/anthropics/claude-code/pull/29284)** *(closed)* — Updates examples to use `"docker:*"` and documents that a bare `"docker"` only matches the command without arguments.

8. **[#83990 — plugin-dev: report missing jq dependency](https://github.com/anthropics/claude-code/pull/83990)** *(closed, fixes #83802)* — `test-hook.sh` previously suppressed the `jq` shell error and reported valid JSON as malformed; now fails fast with a clear dependency message.

9. **[#83993 — plugin-dev: reject self-referential duplicates](https://github.com/anthropics/claude-code/pull/83993)** *(closed)* — `comment-on-duplicates.sh` no longer proposes the triggering issue as a duplicate of itself, which previously posted a self-referential comment and returned success.

10. **[#83999 — scripts: validate gh flag values](https://github.com/anthropics/claude-code/pull/83999)** *(closed)* — Rejects value-taking flags missing their value (e.g., `gh issue list --limit`), which previously bypassed the wrapper's argument validation.

## Feature Request Trends

- **Non-interrupting interaction**: Message queue mode (#50246) and persistent voice conversations (#83434) both push toward a Claude Code that can accept input while busy — the "assistant as always-available copilot" direction.
- **Distributed agents**: Multi-machine Agent-to-Agent collaboration (#28300) remains a top open ask, indicating users want to scale beyond a single session.
- **Cost and usage visibility**: The `claude usage` consolidation request (#33978) reflects growing production/enterprise use where token spend matters.
- **Session portability**: Requests for persistent environment defaults (#87398) and reliable session lists across network drives (#78461) show sessions are now treated as durable artifacts.

## Developer Pain Points

- **Memory safety**: Multiple OOM reports this cycle — grep shim backtracking (#82179), per-tool helper leak (#87238), and a background Bash runner leaking to 10.8 GB (#87319). All involve helper processes ballooning under normal usage.
- **Windows desktop instability**: GPU crash bricking the app until repair (#80444) and silently dropped cross-session messages (#86298) are the two loudest Windows-specific complaints.
- **TUI input edge cases**: Esc mis-routed to tool permission prompts (#64568), clipped MCP dialogs (#69087), and whole messages rendering as raw markdown (#87185) all erode trust in the interactive UI.
- **Model instruction adherence**: Reports like #86261 (model restates an explicit finish condition but stops short, reproduced across five sessions) point to lingering reliability gaps in long-horizon task execution.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-08-18

## Today's Highlights

Today's Codex digest is dominated by a wave of TUI/session-hardening PRs, including a new `/agents` dashboard and a `codex queue` command for driving existing sessions asynchronously. Meanwhile, the most active community issue remains the request for a setting to disable Codex's 60-second auto-resolve behavior, and MCP OAuth-token refresh continues to be a top pain point. A new CLI build, `rust-v0.148.0-alpha.21`, was published.

## Releases

- **`rust-v0.148.0-alpha.21`** — The latest Rust-native Codex CLI build is `0.148.0-alpha.21`. The release feed includes no detailed changelog beyond the version bump.

## Hot Issues

Selected from the top commented/upvoted issues updated in the last 24 hours.

- **[#28969 – Add setting to disable auto-resolve in 60 seconds for questions](https://github.com/openai/codex/issues/28969)**  
  78 comments, 195 👍. The most active issue today. Users want explicit control over Codex's 60-second auto-resolve behavior, which currently proceeds with assumptions when no clarification is provided.

- **[#17265 – Codex does not auto-refresh routed MCP OAuth tokens even when a refresh token is stored](https://github.com/openai/codex/issues/17265)**  
  31 comments, 57 👍. Routed MCP servers expire access tokens and then fail on tool calls despite having a stored `refresh_token`. This is a major reliability gap for MCP-heavy workflows.

- **[#37403 – macOS Desktop cannot resume Remote Control / CLI thread: `already has an active writer`](https://github.com/openai/codex/issues/37403)**  
  21 comments. A regression after the August 7 Desktop update breaks the mobile remote-control-to-desktop workflow and makes existing threads unusable.

- **[#25744 – macOS accumulates Computer Use / MCP helper processes and unreaped zombie children](https://github.com/openai/codex/issues/25744)**  
  19 comments. Long-running sessions leak helper processes, causing HID lag and WindowServer/TCC stalls. The issue is especially painful for Computer Use and MCP automation.

- **[#15723 – Background subprocesses/subagents do not wake the calling agent on completion](https://github.com/openai/codex/issues/15723)**  
  18 comments. Subagents and background subprocesses finish their work but never notify the parent agent, causing sessions to stall or wait indefinitely.

- **[#17793 – Backspace seems to delete more than one character in the TUI](https://github.com/openai/codex/issues/17793)**  
  16 comments. A core text-input bug in the terminal UI that makes prompt editing unpredictable, especially in Kitty and other modern terminals.

- **[#23418 – Mobile remote-created worktree thread is not associated with project in Codex Desktop sidebar](https://github.com/openai/codex/issues/23418)**  
  12 comments. Threads created via ChatGPT mobile remote access exist locally and are openable, but don't appear under the correct project in Desktop, breaking project-based organization.

- **[#13491 – Forked Worker Inherits Parent User Intent and Misinterprets It as Direct Instruction](https://github.com/openai/codex/issues/13491)**  
  10 comments, 11 👍. Forked subagents can inherit the parent's original user prompt and treat it as a direct command, leading to recursive delegation and irrelevant work.

- **[#34268 – Multi-agent V2 full-history forks duplicate compaction snapshots and inline images, causing >100 GiB storage growth](https://github.com/openai/codex/issues/34268)**  
  9 comments. Long-running Desktop conversations with Ultra reasoning and multi-agent V2 can generate multiplicative session data under `$CODEX_HOME/sessions`, reaching over 100 GiB.

- **[#33282 – Codex Desktop `create_thread` does not inherit auto-approval mode for worktree tasks](https://github.com/openai/codex/issues/33282)**  
  9 comments. Permission inheritance is broken between Desktop and bundled CLI sessions, so worktree tasks don't respect the configured auto-approval mode.

## Key PR Progress

Notable PRs updated in the last 24 hours.

- **[#39094 – Add an agents overview dashboard to the TUI](https://github.com/openai/codex/pull/39094)**  
  Adds a full-screen `/agents` dashboard showing loaded root sessions, subagent status, search, navigation, and grouping by project/status. This is a major step toward making multi-agent execution observable.

- **[#39092 – Add a command to queue messages for existing sessions](https://github.com/openai/codex/pull/39092)**  
  Introduces `codex queue --thread <THREAD> --message <TEXT>` to submit messages via the `thread/queue/add` API. Useful for scripting, async handoffs, and external automation.

- **[#39102 – Raise the GPT-5.6 maximum context window](https://github.com/openai/codex/pull/39102)**  
  Proposes allowing `gpt-5.6-sol`, `gpt-5.6-terra`, and `gpt-5.6-luna` context-window overrides up to 872,000 tokens, with matching Bedrock metadata.

- **[#39101 – Update rmcp to 3.1.2](https://github.com/openai/codex/pull/39101)**  
  Upgrades the MCP runtime, removes the local compatibility layer for multi-round-trip tool results, and preserves response metadata on `input_required` SSE results.

- **[#39091 – Make codex-otel OTLP HTTP exporters proxy-aware](https://github.com/openai/codex/pull/39091)**  
  Routes OTLP logs, traces, metrics, and Statsig exporters through proxy-aware transports while preserving TLS/mTLS, signal timeouts, and enterprise CA bundles.

- **[#39098 – Trace exec-server requests from receipt through completion](https://github.com/openai/codex/pull/39098)**  
  Adds end-to-end tracing spans for exec-server requests, covering queueing, dispatch, response handling, and network policy callback outcomes.

- **[#39088 – Harden TUI subagent navigation](https://github.com/openai/codex/pull/39088)**  
  Makes `/subagents` the consistent entry point, reuses already-loaded subagent threads without overriding settings, and routes notifications/approvals only to the active thread.

- **[#39079 – Apply user MCP policy to selected executor plugins](https://github.com/openai/codex/pull/39079)**  
  Resolves MCP server policy directly from the effective user configuration for executor-plugin roots, including enablement, tool allow/deny lists, and approval modes.

- **[#39083 – Harden Windows sandbox provisioning against reparse points](https://github.com/openai/codex/pull/39083)**  
  Prevents elevated Windows sandbox provisioning from following directory junctions or symlinks under `CODEX_HOME`, avoiding ACLs being applied to unintended directories.

- **[#39082 – Prompt for project trust in remote TUI workspaces](https://github.com/openai/codex/pull/39082)**  
  Adds a trust prompt for remote workspaces when no project decision exists, querying the remote app server for config layers before starting a thread.

## Feature Request Trends

Distilled from recent issues and enhancement requests:

- **Configurable automation behavior**  
  Users increasingly want control over Codex's automatic actions: disabling auto-resolve ([#28969](https://github.com/openai/codex/issues/28969)), hiding/collapsing intermediate code snippets in TUI output ([#32817](https://github.com/openai/codex/issues/32817)), and opt-in OTel logging of agent responses ([#22230](https://github.com/openai/codex/issues/22230)).

- **Remote/mobile ↔ Desktop session continuity**  
  Requests around remote-created threads, project association, shared context between ChatGPT and Codex, and bidirectional task handoff appear frequently: [#23418](https://github.com/openai/codex/issues/23418), [#32519](https://github.com/openai/codex/issues/32519), [#28238](https://github.com/openai/codex/issues/28238).

- **Multi-agent observability and lifecycle management**  
  Users want better visibility and control over subagents: root cause of stalled workflows ([#15723](https://github.com/openai/codex/issues/15723)), isolation of forked worker intent ([#13491](https://github.com/openai/codex/issues/13491)), and storage bloat from multi-agent history forks ([#34268](https://github.com/openai/codex/issues/34268)).

- **MCP reliability and policy inheritance**  
  MCP-related issues continue to grow: OAuth token refresh ([#17265](https://github.com/openai/codex/issues/17265)), missing `node_repl` MCP tools in Desktop ([#33599](https://github.com/openai/codex/issues/33599)), and repeated spawning of Windows stdio MCP servers ([#38754](https://github.com/openai/codex/issues/38754)).

- **Platform stability and performance hygiene**  
  Issues asking for fewer process leaks and lower resource consumption are common: macOS zombie helpers ([#25744](https://github.com/openai/codex/issues/25744)), Windows desktop read-loop stutter ([#38518](https://github.com/openai/codex/issues/38518)), and DPAPI credential recovery failures ([#35841](https://github.com/openai/codex/issues/35841)).

## Developer Pain Points

Recurring frustrations visible across the last 24 hours of issue activity:

- **Lack of control over “smart” auto-decisions**  
  The 60-second auto-resolve behavior is the highest-signal pain point ([#28969](https://github.com/openai/codex/issues/28969)). Related issues include missing auto-approval inheritance ([#33282](https://github.com/openai/codex/issues/33282)) and recurring scheduled tasks disabling themselves without user authorization ([#38350](https://github.com/openai/codex/issues/38350)).

- **MCP connection lifecycle is fragile**  
  Token expiration and process reaping remain the top MCP complaints: routed OAuth tokens don't refresh ([#17265](https://github.com/openai/codex/issues/17265)), and Windows stdio MCP servers are spawned repeatedly without reaping ([#38754](https://github.com/openai/codex/issues/38754)).

- **Subagent execution can stall or misdirect work**  
  Background subagents don't wake the caller ([#15723](https://github.com/openai/codex/issues/15723)), and forked workers can inherit and misinterpret parent user intent ([#13491](https://github.com/openai/codex/issues/13491)).

- **Desktop regressions degrade host machines**  
  macOS remote-resume failures (`already has an active writer`) ([#37403](https://github.com/openai/codex/issues/37403)) and resource leaks causing system-wide input lag ([#25744](https://github.com/openai/codex/issues/25744)) are high-impact blockers for daily desktop users.

- **TUI/CLI input and display bugs erode trust**  
  Simple but disruptive issues like backspace deleting multiple characters ([#17793](https://github.com/openai/codex/issues/17793)), reasoning-effort mistranslations ([#31963](https://github.com/openai/codex/issues/31963)), and `/resume` filter reset behavior ([#36010](https://github.com/openai/codex/issues/36010)) remain top-of-mind for terminal-heavy users.

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-08-18

## Today's Highlights

Agent reliability and correctness are the central themes this week. A new nightly release ships an SSR Agent config fix, while several targeted PRs landed to fix silent hangs, misleading subagent termination reasons, and TUI initialization stalls. The community remains most vocal about subagents hanging or reporting false success, especially issue #22323 and #21409.

## Releases

- **v0.56.0-nightly.20260817.g9a15c45fb** — [nightly release](https://github.com/google-gemini/gemini-cli/releases/tag/v0.56.0-nightly.20260817.g9a15c45fb)  
  Includes `[SSR Agent] Issue Fix (21911): Add composite flag to packages/cli tsconfig` via [#28813](https://github.com/google-gemini/gemini-cli/pull/28813). No stable release in this window.

## Hot Issues

- [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — **Subagent recovery after MAX_TURNS reported as GOAL success**  
  P1, 12 comments. A subagent can report `status: "success"` even after hitting `MAX_TURNS` before doing any analysis. This is a correctness/trust issue that directly misleads users about whether work actually completed. PR #28815 now addresses it.

- [#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — **Generalist agent hangs forever**  
  P1, 8 comments, 8 👍. Users report indefinite hangs even for simple operations like folder creation. The common workaround is to disable subagents entirely, which is a significant adoption blocker.

- [#25166](https://github.com/google-gemini/gemini-cli/issues/25166) — **Shell command stuck at "Waiting input" after completion**  
  P1, 3 👍. Simple CLI commands finish but Gemini remains stuck with the shell command active. This breaks basic automation and interactive workflows.

- [#22093](https://github.com/google-gemini/gemini-cli/issues/22093) — **Subagents running without permission since v0.33.0**  
  Agents mode is disabled in configs, but subagents are still being invoked. This is a permission/trust regression, especially for users who only want MCP functionality.

- [#26522](https://github.com/google-gemini/gemini-cli/issues/26522) — **Auto Memory retries low-signal sessions indefinitely**  
  Sessions are repeatedly surfaced when the extraction agent decides not to read them. This causes unnecessary background work and endless retry loops.

- [#26525](https://github.com/google-gemini/gemini-cli/issues/26525) — **Deterministic redaction and reduced Auto Memory logging**  
  Transcript content is sent to the model before redaction happens, meaning secrets may already enter model context. The request is to redact deterministically before any model call.

- [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — **Gemini does not use skills and sub-agents enough**  
  Anecdotally, the model only uses custom skills/subagents when explicitly told to. This limits the value of user-defined skills and agent configurations.

- [#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — **Zero-Dependency OS Sandboxing & Post-Execution Intent Routing**  
  Enhancement epic proposing to let Gemini 3 use its native bash capabilities safely. Key direction for reconciling agent power with security.

- [#24353](https://github.com/google-gemini/gemini-cli/issues/24353) — **Robust component-level evaluations**  
  EPIC for expanding behavioral evals beyond current 76 tests and 6 supported Gemini models. Important for preventing regressions like the recent subagent status bugs.

- [#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — **AST-aware file reads, search, and codebase mapping**  
  EPIC investigating whether AST-aware tooling can reduce token noise, improve file navigation, and make codebase investigation more precise.

## Key PR Progress

- [#28815](https://github.com/google-gemini/gemini-cli/pull/28815) — **Preserve original termination reason during subagent recovery**  
  Fixes #22323: a subagent that hits `MAX_TURNS` or `TIMEOUT` and then calls `complete_task` should no longer be misreported as a clean `GOAL` success.

- [#28816](https://github.com/google-gemini/gemini-cli/pull/28816) — **Fix silent hang in `MessageBus.request` when publish fails**  
  A floating `publish()` promise could cause a 60-second silent hang. Now failures are registered and surfaced instead of stalling requests.

- [#28812](https://github.com/google-gemini/gemini-cli/pull/28812) — **Prevent indefinite TUI hang with execution timeouts**  
  Fixes bare-Linux-terminal "Initializing..." hangs caused by `execAsync` calls to Unix `ps`.

- [#28817](https://github.com/google-gemini/gemini-cli/pull/28817) — **Retain executing subagent tool calls in hook state**  
  Subagent tool calls in `Executing` status were being dropped before entering hook state. This improves hook observability and reliability.

- [#28863](https://github.com/google-gemini/gemini-cli/pull/28863) — **Consent for extension environment changes + sanitize runtime-altering env vars**  
  Adds consent checks for MCP server environment configuration and prevents unauthorized environment variable injection from extensions.

- [#28864](https://github.com/google-gemini/gemini-cli/pull/28864) — **Format `cli_help` subagent output as markdown**  
  Fixes leaked internal thoughts and model monologue when presenting `cli_help` results.

- [#28624](https://github.com/google-gemini/gemini-cli/pull/28624) — **Prevent boolean thought parts leaking as `[Thought: true]` text**  
  Fixes a rendering bug where internal thought parts with `thought: true` leaked into visible output.

- [#28740](https://github.com/google-gemini/gemini-cli/pull/28740) — **Prevent supply chain RCE in eval-pr workflows**  
  Fixes a critical security issue where untrusted fork code could execute in a privileged `pull_request_target` context.

- [#28744](https://github.com/google-gemini/gemini-cli/pull/28744) — **Don't start a fresh chat before resuming ACP sessions**  
  Avoids poisoning the session file by removing one of two fresh-chat starts on ACP load path.

- [#28834](https://github.com/google-gemini/gemini-cli/pull/28834) — **Suppress spurious ENOENT warning for transient subdirs**  
  Eliminates noisy `Warning: Could not read directory ... projects.json.lock: ENOENT` messages during workspace scans.

## Feature Request Trends

- **Agent observability and truthful reporting**  
  Users want to see subagent trajectories via `/chat share` ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598)), include subagent context in `/bug` reports ([#21763](https://github.com/google-gemini/gemini-cli/issues/21763)), and improve agent self-awareness of CLI flags/hotkeys ([#21432](https://github.com/google-gemini/gemini-cli/issues/21432)).

- **Security, consent, and sandboxing**  
  Strong push toward OS-level sandboxing ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)), discouraging destructive git/DB commands ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)), and sanitizing extension environment changes ([#28863](https://github.com/google-gemini/gemini-cli/pull/28863)).

- **Auto Memory hardening**  
  Requests include deterministic redaction ([#26525](https://github.com/google-gemini/gemini-cli/issues/26525)), stopping endless low-signal retries ([#26522](https://github.com/google-gemini/gemini-cli/issues/26522)), and quarantining invalid memory patches ([#26523](https://github.com/google-gemini/gemini-cli/issues/26523)).

- **AST-aware code intelligence**  
  EPICs exploring AST-aware file reads, search, and codebase mapping ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)) to reduce token usage and improve navigation.

- **More and better evals**  
  Component-level evaluations ([#24353](https://github.com/google-gemini/gemini-cli/issues/24353)) and always-pass steering evals ([#28818](https://github.com/google-gemini/gemini-cli/pull/28818)) signal growing investment in regression prevention.

## Developer Pain Points

- **Hangs and stuck states**  
  Generalist agent hangs ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), shell "Waiting input" stalls ([#25166](https://github.com/google-gemini/gemini-cli/issues/25166)), TUI initialization hangs ([#28812](https://github.com/google-gemini/gemini-cli/pull/28812)), and silent `MessageBus` request failures ([#28816](https://github.com/google-gemini/gemini-cli/pull/28816)).

- **Misleading completion status**  
  `MAX_TURNS` reported as `GOAL` success ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)) and missing subagent context in bug reports ([#21763](https://github.com/google-gemini/gemini-cli/issues/21763)) make failures hard to diagnose.

- **Agent permission/config surprises**  
  Subagents running even when agents are disabled ([#22093](https://github.com/google-gemini/gemini-cli/issues/22093)) and browser agent ignoring `settings.json` overrides ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)) erode trust in configuration.

- **Browser agent fragility**  
  Wayland failures ([#21983](https://github.com/google-gemini/gemini-cli/issues/21983)) and locked-profile fail-fast behavior ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232)) block reliable browser automation.

- **Tool/context noise**  
  400 errors with more than 128 tools ([#24246](https://github.com/google-gemini/gemini-cli/issues/24246)), scattered temp scripts ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)), `[Thought: true]` leaks ([#28624](https://github.com/google-gemini/gemini-cli/pull/28624)), and spurious ENOENT warnings ([#28834](https://github.com/google-gemini/gemini-cli/pull/28834)) add avoidable friction.

- **Auto Memory privacy and loop risks**  
  Secrets entering model context before redaction ([#26525](https://github.com/google-gemini/gemini-cli/issues/26525)) and indefinite low-signal retries ([#26522](https://github.com/google-gemini/gemini-cli/issues/26522)) are key background-process concerns.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-08-18

## Today’s Highlights

No new Copilot CLI release shipped in the last 24 hours, but the issue tracker is active with 29 updated issues. The most significant threads focus on OAuth/MCP regressions affecting enterprise MCP servers, missing organization-enabled models in the CLI model catalogue, and several long-running session reliability bugs. The only PR in the window is a notable but unexplained removal of Copilot CLI documentation from the README.

## Releases

No new releases in the last 24 hours.

## Hot Issues

1. **[#4480: Atlassian MCP OAuth fails with "Incompatible authorization server" on 1.0.79 — regression from 1.0.71](https://github.com/github/copilot-cli/issues/4480)**  
   Enterprise MCP connectivity is broken for Atlassian users. The CLI now rejects the authorization server’s advertised issuer, and the community notes this worked in 1.0.71. 6 👍, 5 comments.

2. **[#4439: Copilot CLI 1.0.79 rejects GitLab MCP OAuth metadata due to RFC 8414 issuer mismatch](https://github.com/github/copilot-cli/issues/4439)**  
   A second OAuth discovery regression against GitLab Self-Managed MCP servers. Closed, but it reinforces a pattern of strict OAuth metadata validation breaking real-world MCP servers. 3 👍, 5 comments.

3. **[#4390: Organization-enabled models missing from catalogue — Claude Sonnet 5/Opus 5 and Kimi K3](https://github.com/github/copilot-cli/issues/4390)**  
   Models explicitly enabled by Copilot Business organizations do not appear in the CLI model list. This blocks enterprise teams from using approved models and is one of the highest-reaction issues in this window. 7 👍, 8 comments.

4. **[#4503: SDK server reports ready without auth, then Slack session creation fails generically](https://github.com/github/copilot-cli/issues/4503)**  
   The SDK server starts and reports ready even when `COPILOT_SDK_AUTH_TOKEN` is missing, causing vague downstream failures when creating Slack sessions. This is a serious embedding/API reliability problem. 5 comments.

5. **[#4506: Memory-pressure watchdog force-compacts at 23% context usage, recovers 0.003% of tokens, then loops until OOM](https://github.com/github/copilot-cli/issues/4506)**  
   A long-running session hit a catastrophic compaction loop triggered by process memory rather than actual context pressure. This is a major stability issue for extended agentic workflows.

6. **[#4505: Resumed session retains stale connection item IDs after interrupted response](https://github.com/github/copilot-cli/issues/4505)**  
   Resuming a session after an interrupted response makes every prompt fail with `CAPIError: 400 input item ID does not belong to this connection`. `/fork` does not recover it, so the session is effectively stuck.

7. **[#4507: Repository-level enabledPlugins in .github/copilot/settings.json is ignored in non-interactive mode](https://github.com/github/copilot-cli/issues/4507)**  
   Plugin settings work interactively and in `copilot plugins list`, but not in `copilot -p` mode. This creates confusing inconsistencies for CI and automation users.

8. **[#4509: --no-alt-screen was silently removed with no replacement](https://github.com/github/copilot-cli/issues/4509)**  
   Users have reported alt-screen issues since March, and now the opt-out flag is gone entirely. This is a workflow regression for developers who need Copilot embedded in their terminal without fullscreen takeover. 1 👍.

9. **[#4211: Copilot CLI cannot handle BigInt in structured MCP responses](https://github.com/github/copilot-cli/issues/4211)**  
   MCP servers returning big numbers cause `TypeError: Do not know how to serialize a BigInt` and abort all ongoing tasks. This limits interoperability with real-world MCP tools. 2 👍, 4 comments.

10. **[#1481: SHIFT + ENTER should spawn a line break, but executes the prompt instead](https://github.com/github/copilot-cli/issues/1481)**  
    A long-running UX annoyance with high community engagement. Standard chat-app keyboard behavior remains unsupported, while `CTRL + ENTER` is used for line breaks. 17 👍, 28 comments.

## Key PR Progress

Only one PR was updated in the last 24 hours:

- **[#4510: Remove GitHub Copilot CLI documentation from README](https://github.com/github/copilot-cli/pull/4510)**  
  This open PR removes detailed Copilot CLI documentation, including installation instructions and usage guidelines, from the README. No comments or reactions are listed, so the motivation is unclear. If merged, it would be a significant documentation regression unless replacement docs are published elsewhere.

## Feature Request Trends

- **MCP ecosystem hardening**: Users are asking for more forgiving OAuth discovery, better structured-content handling, support for large numeric types, and clearer lifecycle behavior for Docker/stdio MCP servers. Representative issues: #4480, #4439, #4211, #4512, #4461.

- **Terminal UX and input improvements**: There is consistent demand for more conventional keyboard shortcuts, scrollable conversation history, better accessibility contrast, and opt-out mechanisms for alt-screen mode. Representative issues: #1481, #4313, #4509, #4455, #4485.

- **Plugin and marketplace management**: The community wants richer plugin dependency resolution, repo-level settings honored in all modes, and marketplace caches that respect branch/ref pins. Representative issues: #4487, #4507, #4513.

- **Long-running session reliability**: Developers need hot-reloadable instructions, sane memory-pressure compaction, recoverable session state after interrupted responses, and accurate consumption/AIC reporting. Representative issues: #4508, #4505, #4506, #4511.

- **Model/catalog parity**: Multiple requests center on making the CLI’s model surface match org policy, custom agent configuration, and ACP session configuration. Representative issues: #4390, #2950, #4275.

## Developer Pain Points

- **MCP OAuth regressions**: Recent versions have broken remote MCP authentication against major providers like Atlassian and GitLab, with strict RFC 8414 checks being the most common culprit.
- **Fail-closed MCP policy behavior**: When the managed MCP registry policy fetch fails, even locally defined `stdio` MCP servers are blocked, frustrating users who expect local configuration to remain usable.
- **Configuration parity gaps**: `enabledPlugins`, model selection, and context tier settings behave differently between interactive, non-interactive, and ACP server modes.
- **Silent behavior changes**: The removal of `--no-alt-screen` without a replacement continues to generate negative feedback, especially since alt-screen itself is reported as broken.
- **Long-running sessions degrade badly**: Memory watchdog compaction can loop until OOM, stale connection IDs can make resumed sessions unusable, and custom instructions are never re-read mid-session.

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-08-18

## Today's Highlights
No new releases or issue updates were recorded in the last 24 hours. The only notable activity is the closure of PR #864, which adds a `--starting-prompt` / `-s` flag to allow users to launch straight into a prompt without exiting the current session. This suggests continued focus on improving CLI ergonomics and session workflow.

## Releases
No new releases in the last 24 hours.

## Hot Issues
No issues were updated in the last 24 hours. This digest highlights the current absence of community-reported issues in the active window, rather than ranking non-existent items.

## Key PR Progress
- [PR #864](https://github.com/MoonshotAI/kimi-cli/pull/864) — **`feat: --starting-prompt flag to prompt without exit`** (CLOSED)  
  Adds a `--starting-prompt` / `-s` flag that lets users jump directly into a prompt without needing to exit the current CLI session. The PR closes [issue #887](https://github.com/MoonshotAI/kimi-cli/issues/887) and references a [related discussion on issue #785](https://github.com/MoonshotAI/kimi-cli/issues/785#issuecomment-3837789973). This is a practical improvement for developer workflows that require quick iterative prompting.

## Feature Request Trends
Based on the single PR in the update window, the main feature direction is **session-level prompt control**. The `--starting-prompt` flag addresses a previously reported request (issue #887) to streamline entering a prompt from the command line without a full restart. This aligns with a broader trend of reducing friction in CLI invocation and interactive session management.

## Developer Pain Points
With no issues updated in the last 24 hours, comprehensive pain-point analysis is not possible. The referenced issues (#887, #785) hint at recurring friction around **initial prompt entry and session exit behavior**, but the data available today is too limited to draw broader conclusions. Future digests will provide deeper trend analysis as activity resumes.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-08-18

**Data source:** github.com/anomalyco/opencode — 50 issues and 50 PRs updated in the last 24h; no releases published.

---

## 1. Today's Highlights

No new releases shipped in the last 24 hours, but a large batch of stale pull requests from mid-July was swept up by the `automated-pr-cleanup` bot, signaling a maintainer push to close out old work. The most active threads continue to be Windows platform reliability (ARM64 TUI init, ripgrep extraction, npm postinstall stubs) and a fresh wave of endpoint/authentication errors — most notably a 410 "Legacy inference endpoint retired" that has users confused about whether they're hitting a deprecated service. Two PRs stand out as strategically important: a shared server-data refactor for the desktop/web app (#43017) and a targeted fix for Azure DeepSeek adapter selection (#43135).

---

## 2. Releases

No new releases in the last 24 hours. Omitted.

---

## 3. Hot Issues

1. **[#19130 — Windows ARM64 native: OpenTUI fails to initialize with bun:ffi dlopen TinyCC error](https://github.com/anomalyco/opencode/issues/19130)** · 18 comments, 12 👍
   The single most-commented issue this cycle. Native ARM64 builds work fine for CLI commands but the TUI crashes at init. Open since March, making it one of the longest-running Windows blockers. Community reaction is frustration: ARM64 Windows is a growing segment, and a 5-month-old TUI-breaking bug is hard to justify.

2. **[#43105 — "Endpoint error": legacy inference endpoint retired with status 410](https://github.com/anomalyco/opencode/issues/43105)** · 15 comments
   Users pointing OpenAI-compatible clients at `https://opencode.ai/inference/v1` receive a hard 410 Gone. The confusing part: the same endpoint works in opencode2 (beta). This looks like a migration/messaging gap — the deprecation is firing for third-party CLIs while the beta client takes a different code path.

3. **[#7801 — [FEATURE]: Plan Mode + Question tool can auto switch to Build mode](https://github.com/anomalyco/opencode/issues/7801)** · 11 comments, 32 👍
   The highest-upvoted open feature request. Users want Plan mode's question tool to automatically transition the session to Build mode once a plan is approved, rather than forcing a manual mode flip. The 32 👍 signal strong demand for smoother plan-to-execute workflows.

4. **[#22861 — Bug: Big Pickle stops response early](https://github.com/anomalyco/opencode/issues/22861)** · 10 comments, 3 👍
   The Big Pickle model consistently truncates responses at the same logical spot, even when asked to continue. Reports like this indicate a streaming/stop-sequence bug rather than a prompt issue, and it's reproducible across sessions.

5. **[#40243 — ChatGPT OAuth rejects GPT-5.6 models for an EU-resident workspace, while official Codex CLI succeeds](https://github.com/anomalyco/opencode/issues/40243)** · 9 comments, 4 👍
   A GDPR/data-residency regression: OpenCode's OAuth flow fails for EU workspaces where the official Codex CLI works. Users are blocked because their projects disallow API keys. This is a compliance-sensitive issue that could push EU enterprises off the tool.

6. **[#33027 — [BUG] MCP tools connected but not exposed to agent](https://github.com/anomalyco/opencode/issues/33027)** · 8 comments, 3 👍
   MCP server connects successfully and its tools are visible via `tools/list`, but the agent never sees them. The `pdfrag` example suggests a filtering/namespace bug in MCP tool forwarding. Open since June — a serious gap for the MCP ecosystem promise.

7. **[#24153 — [FEATURE]: Add unarchive/restore for archived sessions](https://github.com/anomalyco/opencode/issues/24153)** · 8 comments, 11 👍
   Archiving is one-way: sessions vanish from the sidebar and are only dimly visible elsewhere. Users are asking for a restore path. The 11 👍 and steady comments indicate session lifecycle management is a real workflow pain.

8. **[#43102 — Opencode is unavailable: Upstream request failed, Endpoint is unavailable](https://github.com/anomalyco/opencode/issues/43102)** · 4 comments
   Two different models in a new session both fail with a generic upstream-unavailable error. The vagueness of the message is itself a complaint — no model name, no provider, no retry guidance. This may be related to the same backend instability driving #43105.

9. **[#41990 — Compact Bug: summarization triggers false "usage limit reached"](https://github.com/anomalyco/opencode/issues/41990)** · 4 comments, 3 👍
   When the conversation compactor runs, users get a spurious usage-limit error even though a brand-new chat works fine. This poisons long sessions and erodes trust in the compaction feature; it's closed, which is good, but worth confirming the fix shipped.

10. **[#43054 — Models other than hy3-free / deepseek flash free fail with "Forbidden: {model:big-pickle}"](https://github.com/anomalyco/opencode/issues/43054)** · 3 comments, 1 👍
    A bizarre error: any non-free model fails with a Forbidden whose body names `big-pickle`. Looks like a server-side gateway routing bug where model names are mangled or the free tier is the only valid route. New as of 2026-08-17 and could escalate quickly.

**Also notable:** [#42962](https://github.com/anomalyco/opencode/issues/42962) — the Go gateway advertises models via `/zen/go/v1/models` that then 503/400 on chat completion, meaning "list ≠ deployment"; and [#42880](https://github.com/anomalyco/opencode/issues/42880) — `.so` files generated at high velocity in `/tmp`, with a user submitting a ramdisk fstab workaround out of sheer desperation.

---

## 4. Key PR Progress

1. **[#43017 — [beta] refactor(app): use shared server data](https://github.com/anomalyco/opencode/pull/43017)** · OPEN
   Migrates the desktop/web app to the shared server-data layer from #42999, removing duplicated session reducers and legacy caches. This is a significant architectural cleanup for the V2 beta — centralizing location-scoped access and session authority should eliminate a whole class of sync bugs.

2. **[#43135 — fix(provider): select Azure DeepSeek adapter](https://github.com/anomalyco/opencode/pull/43135)** · OPEN
   Directly closes #43106. Azure-hosted DeepSeek-V4 deployments were falling back to the generic chat adapter and ignoring custom `reasoningEffort` variants. The fix routes Azure DeepSeek models through the dedicated `deepseek()` adapter. Small, targeted, clearly correct.

3. **[#43129 — feat(ai): support Vertex request labels](https://github.com/anomalyco/opencode/pull/43129)** · CLOSED
   Adds billing-label support to the Vertex Gemini provider, with test coverage for the request-shaping path. Tied to the broader protocol-correctness audit in #41932.

4. **[#42810 — refactor(core): simplify interrupt continuation](https://github.com/anomalyco/opencode/pull/42810)** · OPEN
   Replaces the multi-field continuation state machine with a three-line post-cleanup check in `SessionExecution`. A nice complexity reduction for how `session.interrupt?continue=true` resumes steering input.

5. **[#37549 — feat(plugin): add session request hook](https://github.com/anomalyco/opencode/pull/37549)** · CLOSED (automated cleanup)
   Adds `ctx.session.hook("request", ...)` for mutating model headers and JSON bodies before auth/signing, with support for both native HTTP and final WebSocket payloads. Powerful for provider middleware and request rewriting.

6. **[#37542 — fix(opencode): restore session diff summary](https://github.com/anomalyco/opencode/pull/37542)** · CLOSED
   Reintroduces the session-level diff summary removed in #30127, fixing #30877, #32852, and #17797. Three issues closed by one restoration PR — clearly a regression that users felt immediately.

7. **[#37537 — fix(tui): preserve system palette colors](https://github.com/anomalyco/opencode/pull/37537)** · CLOSED
   Generates the native V2 theme directly from the detected terminal palette and preserves ANSI hues instead of synthesizing darker approximations. Minor but visible polish for terminal aesthetics.

8. **[#37530 — fix(core): restore external directory defaults](https://github.com/anomalyco/opencode/pull/37530)** · CLOSED
   Re-allows access to discovered skill/materialized reference directories by default, and refreshes agent defaults when skill/reference state changes. Likely addresses the Windows permission confusion in #36681/#36696.

9. **[#37504 — feat(opencode): add session loop command](https://github.com/anomalyco/opencode/pull/37504)** · CLOSED
   Adds built-in `/loop` (alias `/proactive`) for autonomous session continuation, closing #23578. The original #23575 went stale; this revival is a win for automation-minded users.

10. **[#37499 — feat: add /workflow slash command for multi-step YAML pipelines](https://github.com/anomalyco/opencode/pull/37499)** · CLOSED
    Introduces `.opencode/workflows/*.yaml` pipeline definitions with a `/workflow` command to run them. This is a significant feature direction: declarative, multi-step automation as a first-class CLI citizen.

**Also worth watching:** [#37477](https://github.com/anomalyco/opencode/pull/37477) stops booting a full instance for `session list` (a real perf win for startup time), and [#37472](https://github.com/anomalyco/opencode/pull/37472) strips stray provider control tokens like `<|tool_call_begin|>` from malformed tool output.

---

## 5. Feature Request Trends

Distilled from all issues and PRs updated in the last 24h:

- **Mode automation & workflow pipelines.** The dominant trend. #7801 (Plan→Build auto-switch, 32 👍) is the highest-upvoted request, and the `/loop` (#37504) and `/workflow` (#37499) PRs show maintainers are actively shipping in this direction. Users want less manual mode-flipping and more declarative orchestration.

- **Session lifecycle management.** #24153 (unarchive/restore, 11 👍) and #43126 (pause/resume on rate-limit reset) both ask for first-class session state control — the ability to archive, restore, pause, and resume without losing context.

- **Plugin surface parity across clients.** #43132 requests a web/desktop plugin UI mirroring the TUI plugin API (`Plugin.define`, `ui.dialog`, `ui.slot`). V2's rich TUI plugin surface currently has no equivalent in the graphical apps.

- **Universal authentication/endpoints.** #43131 asks for Console login support across all CLIs, and the 410-endpoint confusion in #43105/#43101 shows users want a single, well-documented auth path that works everywhere — including third-party tools.

---

## 6. Developer Pain Points

- **Windows is the reliability bottleneck.** Six or more issues this cycle are Windows-specific: ARM64 TUI crashess (#19130), grep/glob broken by MSIX PowerShell PSModulePath (#40623), broken npm postinstall leaving 479-byte stubs (#41370), path/cmdlet permission configs ignored (#36681, #36696), and a global-install "Orphan text error" (#41595). The pattern is clear: Linux/macOS are fine, but Windows users keep hitting platform integration bugs.

- **Endpoint/auth instability is eroding trust.** Users hit a 410 on the legacy inference endpoint (#43105), EU-resident OpenAI workspaces get OAuth rejection (#40243), the Go gateway advertises non-deployable models (#42962), and non-free models return a bizarre `Forbidden: {model:big-pickle}` (#43054). When the official CLI contradicts the official Codex CLI, confidence drops.

- **Provider adapter gaps.** Azure DeepSeek models ignore `reasoningEffort` because the wrong adapter is selected (#43106), Big Pickle truncates responses (#22861), and Gemini/Vertex label support only just landed (#43129). Each provider feels like a bespoke integration with its own edge cases.

- **Session isolation and integrity.** `opencode run --continue` injects prompts into sessions actively in use by another running instance (#43133), the compaction feature falsely reports usage limits (#41990), and image-heavy conversations die with "failed to read request body" and lose history (#43119). Users are paying for long-lived sessions and getting data loss.

- **MCP tool visibility gaps.** #33027 (MCP tools connected but invisible to the agent) remains open since June, undermining the "bring your own tools" story that MCP is supposed to deliver.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-08-18

## Today's Highlights

The community's focus this week is reliability: a wave of fixes landed for context compaction failures (#6879, #8120, #8241), extension lifecycle events (#7350, #8242), and provider compatibility gaps surfaced by OpenRouter's 870-trial API benchmark (#7994, #7995, #8246). Model catalog maintenance also saw heavy churn, with Xiaomi, Qwen, and ZAI/GLM catalogs all updated within the last 24 hours.

## Hot Issues

1. **#6879 — Auto-compaction never triggers after context grows past 100%** — [earendil-works/pi#6879](https://github.com/earendil-works/pi/issues/6879)
   The most active issue (18 comments, 17 👍). A 2-hour agentic turn on gpt-5.6-sol blew past the compaction threshold to 373k tokens until the API hard-rejected the request. The community wants compaction checks after every agentic step, not only at the provider boundary. Directly motivates PR #8120.

2. **#534 — Config folder is out of place on Linux** — [earendil-works/pi#534](https://github.com/earendil-works/pi/issues/534)
   Highest 👍 count of the batch (39), 15 comments. Closed, but the demand is clear: users want XDG Base Directory compliance instead of config files dumped directly in `$HOME`.

3. **#8029 — Very slow performance moving in prompt editor** — [earendil-works/pi#8029](https://github.com/earendil-works/pi/issues/8029)
   Arrow-key latency grows linearly with buffer size: 7,000 lines in the prompt box costs 1,650 ms per keypress. A major quality-of-life regression for agent-generated large edits.

4. **#3200 — Support video/audio content in prompt command** — [earendil-works/pi#3200](https://github.com/earendil-works/pi/issues/3200)
   Extends the `prompt` RPC beyond `images:` to forward video/audio to multimodal models (Gemma 4, GPT-4o). 8 comments, 5 👍 — multimodal agent workflows are clearly in demand.

5. **#7995 — openai-responses: no cacheControlFormat 'anthropic' support — 2.5x cost penalty** — [earendil-works/pi#7995](https://github.com/earendil-works/pi/issues/7995)
   Filed on behalf of an OpenRouter engineer from an 870-trial benchmark. Missing Anthropic-style prompt caching in `openai-responses` makes Claude via OpenRouter 2.5x more expensive than necessary. Marked `inprogress`.

6. **#8036 — Edit tool crashes TUI when rendering large diff** — [earendil-works/pi#8036](https://github.com/earendil-works/pi/issues/8036)
   A successful edit containing a ~14.5 MB diff crashes the interactive TUI at render time and again on session resume. Part of a recurring crash class around large outputs (see #8028).

7. **#8166 — Custom message mid-tool-batch breaks tool_calls→tool adjacency (DeepSeek 400)** — [earendil-works/pi#8166](https://github.com/earendil-works/pi/issues/8166)
   An extension calling `pi.sendMessage(..., { triggerTurn: false })` between tool calls corrupts the message sequence, causing every subsequent turn to fail with a 400 from DeepSeek. Shows how brittle tool-loop message ordering is under extension interference.

8. **#8187 — Update xiaomi model catalog: remove deprecated mimo-v2 models** — [earendil-works/pi#8187](https://github.com/earendil-works/pi/issues/8187)
   Deprecated models still appear in `/model` and `--list-models`; selection fails after the provider shuts them down. Catalog drift is a recurring maintenance theme (#8194, #8220).

9. **#8028 — TUI fullRender crashes with RangeError when output exceeds V8 string limit** — [earendil-works/pi#8028](https://github.com/earendil-works/pi/issues/8028)
   A video-production agent reading many image frames eventually crashes the renderer with "Invalid string length". Another data point for renderer fragility.

10. **#7756 — detectInstallMethod mislabels non-pnpm installs under PNPM_HOME** — [earendil-works/pi#7756](https://github.com/earendil-works/pi/issues/7756)
    Installs that merely share `PNPM_HOME` for bins but aren't pnpm-managed get mislabeled, producing confusing "not managed" errors. A sharp edge case in install detection.

## Key PR Progress

1. **#8120 — feat(coding-agent): add experimental append compaction** — [earendil-works/pi#8120](https://github.com/earendil-works/pi/pull/8120)
   With `PI_EXPERIMENTAL=1`, append-mode compaction reuses the active system prompt, tools, and routing session so the compacted prefix keeps provider prompt caches warm. Standalone remains the default. Directly relevant to #6879.

2. **#8258 — fix(coding-agent/ai): anthropic refusal error and fallbacks** — [earendil-works/pi#8258](https://github.com/earendil-works/pi/pull/8258)
   Addresses #8017 by adding `allowed_fallback_models` metadata for Anthropic's API-level refusal fallbacks; reproduced live on `claude-fable-5` during compaction.

3. **#8255 — fix(coding-agent): load nested markdown skills** — [earendil-works/pi#8255](https://github.com/earendil-works/pi/pull/8255)
   Fixes #6479: standalone `.md` skills in subfolders like `~/.agents/skills/third-party/` were silently skipped. Makes third-party skill overlays actually usable.

4. **#8242 — fix(extension-examples): use agent_settled instead of end** — [earendil-works/pi#8242](https://github.com/earendil-works/pi/pull/8242)
   Fixes #7350: `agent_end` fires before retries, compaction, and queued follow-ups; examples now hook `agent_settled` for true "ready for input" semantics. Important for everyone building on the extension API.

5. **#8241 — fix(extensions): emit compaction failed for extensions** — [earendil-works/pi#8241](https://github.com/earendil-works/pi/pull/8241)
   Fixes #8175: extension handlers now receive a `session_compact_failed` event with the failure payload instead of only internal `compaction_end` errors.

6. **#8246 — feat(ai): openai completions reasoning details** — [earendil-works/pi#8246](https://github.com/earendil-works/pi/pull/8246)
   Addresses #7994: preserves signed `reasoning.text`/`reasoning.summary` entries from `delta.reasoning_details` so the next assistant replay keeps reasoning intact. One of several PRs from the OpenRouter benchmark fallout.

7. **#8275 — feat(ai): generalize openai-completions thinking token budget fields** — [earendil-works/pi#8275](https://github.com/earendil-works/pi/pull/8275)
   Adds `compat.thinkingTokenBudgetField` so vLLM (`thinking_token_budget`), Qwen/SGLang (`thinking_budget`), and llama.cpp (`thinking_budget_tokens`) all get proper budget clamping, plus compat docs.

8. **#8243 — fix(ai): bedrock response to include smithy headers** — [earendil-works/pi#8243](https://github.com/earendil-works/pi/pull/8243)
   Captures the raw Smithy HTTP response in a Bedrock deserialize middleware so gateway headers like `x-bifrost-provider` reach `onResponse`/`after_provider_response` handlers.

9. **#8253 — fix(tui): avoid full-screen flashing when content changes above the viewport** — [earendil-works/pi#8253](https://github.com/earendil-works/pi/pull/8253)
   In 10k+ line transcripts, differential rendering cleared and reprinted the entire screen when content above the viewport changed. Fixes visible flashing; a quiet but high-impact TUI improvement.

10. **#8262 — feat(coding-agent): dispatch hooks on every turn-start path** — [earendil-works/pi#8262](https://github.com/earendil-works/pi/pull/8262)
    `sendCustomMessage(triggerTurn: true)` and similar paths were skipping the `input` hook and `before_agent_start`. Adds a cancellable turn preflight so extensions can intercept all turns uniformly.

Also notable: #6216 (Amazon Bedrock Mantle OpenAI Responses provider, open since July), #8254 (Copilot policy login rate-limit prevention), #8257 (skip project-agent trust confirm for already-trusted projects), #8240 (Qwen Token Plan catalog alignment).

## Feature Request Trends

- **Multimodal agent inputs**: Video/audio passed through `prompt` (#3200), clipboard image paste (#2144), and vision models in built-in catalogs (#8220). Users increasingly expect Pi to behave as a full multimodal agent chassis.
- **Smarter context/compaction management**: Auto-compaction triggering (#6879), experimental append compaction (#8120), and rate-limit-aware session auto-resume (#8277). Context-window handling is the #1 pain point.
- **Provider catalog upkeep**: Xiaomi deprecations (#8187), Qwen token-plan alignment (#8194), GLM vision model (#8220). Users expect built-in catalogs to track upstream availability closely and remove dead models.
- **Platform/desktop integration**: XDG Base Directory on Linux (#534), SELinux container docs (#8276), tmux/terminal compatibility fixes (#8252, #8278). Linux-first polish is a steady drumbeat.

## Developer Pain Points

- **Compaction is unreliable**: #6879 and #8229 show both cloud and local providers can hit context overflow when compaction doesn't trigger at the right moment. The community wants aggressive post-step checks rather than waiting for provider rejection.
- **TUI crashes under large outputs**: Three distinct crash reports (#8036 diff rendering, #8028 V8 string limit, #8252 tmux 1-column width) — the renderer remains the most fragile component.
- **Provider API-compat gaps are expensive**: Missing `cache_control` (#7995) costs 2.5x on Claude via OpenRouter; reasoning-details round-trip (#7994) and thinking-budget field names (#8274) cause silent degradation across vLLM/SGLang/llama.cpp.
- **Extension event model is confusing**: `agent_end` vs `agent_settled` (#7350), missing compaction-failure events (#8175/#8241), and mid-tool-batch message injection breaking tool adjacency (#8166) — extension authors are surfacing lifecycle semantics bugs faster than core users.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-08-18

## Today's Highlights

Qwen Code released **v0.21.13**, adding Web Shell file attachments and conversation forking. Community activity remains concentrated on context-management reliability, daemon resource governance, and the review automation pipeline. Several PRs made progress on Weixin integration, scheduled sessions, and browser-side transcript retention.

## Releases

- **[v0.21.13](https://github.com/QwenLM/qwen-code/releases/tag/v0.21.13)** — latest stable release. Highlights include:
  - Web Shell composer now supports dragging, dropping, and pasting text files as named attachments alongside images ([#9180](https://github.com/QwenLM/qwen-code/pull/9180)).
  - Users can now fork conversations from any specific Assistant response.

- **[v0.21.11-nightly.20260817.195128a17a](https://github.com/QwenLM/qwen-code/releases/tag/v0.21.11-nightly.20260817.195128a17a)** — nightly build including `feat(autofix): deny-by-default footprint gate and positional window censuses` ([#9156](https://github.com/QwenLM/qwen-code/pull/9156)) and a web-shell fix.

- **DSW EAS benchmark releases** — several smoke/full validation runs (`dsw-eas-tb-smoke-*`, `dsw-eas-full-*`) are pinned to **v0.21.13**; full runs cover SWE-bench Verified (500) and Terminal-Bench 2.0 (89).

## Hot Issues

1. **[#9194 — chore(review): close the mutation-verified test-pin gaps from PR #9096](https://github.com/QwenLM/qwen-code/issues/9194)**  
   10 comments. Automated review found a class of tests that under-pin their stated contract: a production-code mutation can still pass the suite. Important for long-term CI/review reliability.

2. **[#8316 — Prompt not restored to input box when canceling (Ctrl+C)](https://github.com/QwenLM/qwen-code/issues/8316)**  
   9 comments. High-friction UX bug: users cancel a prompt to correct it, then lose the draft and must retype everything.

3. **[#8051 — tracking(serve): Bound multi-workspace daemon resource usage](https://github.com/QwenLM/qwen-code/issues/8051)**  
   9 comments. Count-only limits do not bound bytes held by request bodies, WebSocket assembly, or other memory-heavy daemon state. Community wants measurable resource caps.

4. **[#9324 — Messages delivered in multiple copies without user redirection](https://github.com/QwenLM/qwen-code/issues/9324)**  
   7 comments. Desktop Code users report the model receives the same message repeatedly and interrupts its current work. Needs maintainer triage info.

5. **[#6806 — Status line context usage percentage does not refresh after /compress](https://github.com/QwenLM/qwen-code/issues/6806)**  
   6 comments. The footer keeps showing pre-compression token counts until the next model request. Minor but visible context-management wart.

6. **[#9061 — Ctrl+V paste completely unresponsive in CLI on Windows](https://github.com/QwenLM/qwen-code/issues/9061)**  
   6 comments, P1. Regression since 0.21.x; downgrading to 0.21.0 restores paste. Likely caused by the new terminal interaction layer.

7. **[#9300 — VP mode: content not bottom-aligned](https://github.com/QwenLM/qwen-code/issues/9300)**  
   6 comments. Blank space appears between the last message and composer in default `useTerminalBuffer: true` mode.

8. **[#9354 — Establish cross-host chat transcript contract prevalidation](https://github.com/QwenLM/qwen-code/issues/9354)**  
   5 comments. Requests a stable read-only transcript contract across Web Shell, Tauri Desktop, VS Code, and future HTML export, with versioned schemas and safety boundaries.

9. **[#9320 — Lost context after /compress-fast and /rewind](https://github.com/QwenLM/qwen-code/issues/9320)**  
   5 comments. User reports compression from 102k to 87k tokens, then a new server resume loses context. Directly affects long-session workflows.

10. **[#9296 — Qwen Autofix: review-event storms and duplicate dispatch](https://github.com/QwenLM/qwen-code/issues/9296)**  
    4 comments. Analysis of ~500 autofix runs showed 59% cancelled and wasteful dispatches, including reviews on closed/merged PRs still starting runs.

## Key PR Progress

1. **[#9367 — feat(webui): add global expand/collapse control to exported HTML viewer](https://github.com/QwenLM/qwen-code/pull/9367)**  
   Adds an “Expand all / Collapse all” toolbar to `ChatViewer` and enables it in `/export` HTML templates — directly supports the popular `/export` enhancement requests.

2. **[#9364 — feat(daemon): make serve new-file mode configurable](https://github.com/QwenLM/qwen-code/pull/9364)**  
   Introduces `QWEN_SERVE_NEW_FILE_MODE` so `qwen serve` can create new files with umask-derived modes instead of hardcoded `0600`. Addresses issue [#9250](https://github.com/QwenLM/qwen-code/issues/9250).

3. **[#9361 — feat(scheduled-tasks): allow creating a task with an existing session](https://github.com/QwenLM/qwen-code/pull/9361)**  
   Adds optional `sessionId` to scheduled-task endpoints, with validation before binding to an existing live session.

4. **[#9358 — fix(weixin): keep typing indicator alive during long turns](https://github.com/QwenLM/qwen-code/pull/9358)**  
   Re-sends `TYPING` every 4 seconds because WeChat expires the one-shot typing state. Fixes pain point from [#9353](https://github.com/QwenLM/qwen-code/issues/9353).

5. **[#9295 — fix(core): omit image media the model endpoint cannot safely consume](https://github.com/QwenLM/qwen-code/pull/9295)**  
   Prevents invalid image MIME types such as HEIC/TIFF or undecodable bytes from being forwarded as data URIs, which causes request validation failures.

6. **[#9303 — fix(web-shell): bound daemon transcript retention to stop renderer OOM crashes](https://github.com/QwenLM/qwen-code/pull/9303)**  
   Releases the raw replay snapshot after injection and applies the same block cap to replay rebuilds as live growth. Important for long-running web-shell sessions.

7. **[#9342 — fix(review): clear the deferred-suggestion backlog from #9175](https://github.com/QwenLM/qwen-code/pull/9342)**  
   Clears 19 deferred findings accumulated across 15 review rounds — no Critical issues, roughly half are behavior fixes. Reduces long-standing review debt.

8. **[#9321 — feat(autofix): seed the takeover round counter with `/takeover from N`](https://github.com/QwenLM/qwen-code/pull/9321)**  
   Allows a takeover to start from a non-zero round, so `CRITICAL_ONLY_AFTER_ROUND` engages in the remaining rounds as intended.

9. **[#9247 — fix(review): budget the composed body against GitHub's review limit](https://github.com/QwenLM/qwen-code/pull/9247)**  
   Keeps review bodies within GitHub’s 65,536-character limit, trimming the Chinese translation first when overflow would occur.

10. **[#8927 — feat(channels): bound session lifetime with sessionRotation](https://github.com/QwenLM/qwen-code/pull/8927)**  
    Adds per-channel `sessionRotation` with `maxTurns` / `maxAge` bounds, preventing long-lived route sessions from persisting indefinitely.

## Feature Request Trends

- **Exportable, portable chat transcripts** — Requests to make `/export` HTML include thinking blocks and tool results ([#8208](https://github.com/QwenLM/qwen-code/issues/8208)), plus a broader cross-host transcript contract ([#9354](https://github.com/QwenLM/qwen-code/issues/9354)) and now a global expand/collapse control ([#9367](https://github.com/QwenLM/qwen-code/pull/9367)).

- **Daemon / `qwen serve` governance** — Users want bounded memory usage ([#8051](https://github.com/QwenLM/qwen-code/issues/8051), [#8091](https://github.com/QwenLM/qwen-code/issues/8091)), configurable new-file permissions ([#9250](https://github.com/QwenLM/qwen-code/issues/9250)), and session reuse for scheduled tasks ([#8906](https://github.com/QwenLM/qwen-code/issues/8906)).

- **Channel integration completeness** — Weixin requests include sending files ([#9352](https://github.com/QwenLM/qwen-code/issues/9352)), typing-indicator heartbeats ([#9353](https://github.com/QwenLM/qwen-code/issues/9353)), and safe handling of 64-bit message IDs ([#9307](https://github.com/QwenLM/qwen-code/issues/9307)).

- **Terminal/UI convergence** — A long-running proposal asks to consolidate the chat panel onto Web Shell across VS Code and desktop ([#5883](https://github.com/QwenLM/qwen-code/issues/5883)). Users also keep hitting text-selection/copy regressions in the newer UI ([#9315](https://github.com/QwenLM/qwen-code/issues/9315)).

- **Dynamic provider model lists** — Users want ModelStudio Token Plan / Coding Plan presets to fetch available models dynamically instead of using hardcoded recommendation lists ([#9368](https://github.com/QwenLM/qwen-code/issues/9368)).

## Developer Pain Points

- **Terminal interaction regressions** — Windows Ctrl+V paste breakage ([#9061](https://github.com/QwenLM/qwen-code/issues/9061)), Linux copy failure in newer versions ([#9315](https://github.com/QwenLM/qwen-code/issues/9315)), and lost prompt drafts after Ctrl+C ([#8316](https://github.com/QwenLM/qwen-code/issues/8316)) all point to friction in the rewritten interactive terminal layer.

- **Context/compression trust issues** — Token counts shown after `/compress` don’t refresh ([#6806](https://github.com/QwenLM/qwen-code/issues/6806)), compression can report inconsistent results ([#9309](https://github.com/QwenLM/qwen-code/issues/9309)), and resuming after compression can lose context ([#9320](https://github.com/QwenLM/qwen-code/issues/9320)).

- **Review/autofix pipeline noise** — High cancellation rates, duplicate dispatches, and expensive review rounds ([#9296](https://github.com/QwenLM/qwen-code/issues/9296)) plus under-pinned tests ([#9194](https://github.com/QwenLM/qwen-code/issues/9194)) create trust and efficiency problems for maintainers.

- **Daemon resource and file-permission surprises** — Unbounded daemon memory, hardcoded `0600` new-file modes, and renderer OOM from unbounded transcript retention are recurring operational headaches ([#8051](https://github.com/QwenLM/qwen-code/issues/8051), [#9250](https://github.com/QwenLM/qwen-code/issues/9250), [#9303](https://github.com/QwenLM/qwen-code/pull/9303)).

- **Multi-agent and session-state oddities** — Errored agent-team tabs can crash sessions ([#9290](https://github.com/QwenLM/qwen-code/issues/9290)), prompts contradict automatic delivery behavior ([#9283](https://github.com/QwenLM/qwen-code/issues/9283)), and duplicate message delivery interrupts active work ([#9324](https://github.com/QwenLM/qwen-code/issues/9324)).

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek-TUI / CodeWhale Community Digest — 2026-08-18

All links point to the Hmbown/CodeWhale tracker, the current development home of DeepSeek-TUI.

## Today's Highlights

The project is closing out v0.9.9 with a “truth-and-resilience” release: shell execution can no longer wedge an entire session on host resource exhaustion ([#5465](https://github.com/Hmbown/CodeWhale/pull/5465)), and unverified pricing/context defaults are now labeled honestly instead of silently accepted. At the same time, the i18n refactor continues with two more PRs moving web/docs locales onto the dictionary spine ([#5488](https://github.com/Hmbown/CodeWhale/pull/5488), [#5490](https://github.com/Hmbown/CodeWhale/pull/5490)), while CI remains red on both platforms ([#5403](https://github.com/Hmbown/CodeWhale/issues/5403)) and model/pricing rows were refreshed as of 2026-08-17 ([#5485](https://github.com/Hmbown/CodeWhale/pull/5485)).

## Releases

No standalone release appeared in the GitHub Releases feed during the last 24 hours. However, the **v0.9.9 release PR closed** ([#5476](https://github.com/Hmbown/CodeWhale/pull/5476)). It is described as a truth-and-resilience release: the shell tool can no longer wedge a session when the host runs out of disk/file descriptors, and unverified context windows, output ceilings, and telemetry defaults are now displayed honestly.

## Hot Issues

1. **CodeWhale Config Paths Fragmented Across OS and Cygwin (Plus Silent Migration Bug)** — [Issue #2369](https://github.com/Hmbown/CodeWhale/issues/2369)  
   Config and secret paths resolve differently on Windows/Cygwin, and a legacy migration can silently move data. Critical for Windows users; 8 comments.

2. **Test reliability: flaky verifier background tests, /workspace-sensitive fixtures, 12 untriaged #[ignore] tests** — [Issue #5056](https://github.com/Hmbown/CodeWhale/issues/5056)  
   Verifier background tests and workspace-sensitive fixtures still flake under full-suite parallelism. Maintainer-flagged reliability debt; 8 comments.

3. **agent tool: simplify the 32-field schema so models stop erroring on it** — [Issue #5324](https://github.com/Hmbown/CodeWhale/issues/5324)  
   The model-facing `agent` tool had 32 properties, zero required fields, and eight actions in one schema — a likely cause of model errors. Closed with simplification; 8 comments.

4. **v0.9.7: Codewhale TUI crashing** — [Issue #5424](https://github.com/Hmbown/CodeWhale/issues/5424)  
   User-reported crash after prompting and waiting ~1 minute. Closed; likely addressed in the 0.9.9 hardening work; 7 comments.

5. **执行大文本处理工程后会话中断卡死** — [Issue #1425](https://github.com/Hmbown/CodeWhale/issues/1425)  
   Processing a 3-million-character novel spawned 10 subagents, but `agent_wait` timeouts hung the session. Important for large-scale document workflows; 7 comments.

6. **Agent spawn surface has too many knobs — labeled builder runs read-only and self-BLOCKED** — [Issue #5123](https://github.com/Hmbown/CodeWhale/issues/5123)  
   Dogfooding bug: a delegate labeled `builder` was read-only and blocked from its assigned work. Exposes capability/contract mismatch in subagents; 7 comments.

7. **VS Code crashes or exits unexpectedly when YOLO Agent is running test scripts** — [Issue #1651](https://github.com/Hmbown/CodeWhale/issues/1651)  
   Autonomous background test execution can crash the IDE. Major stability concern for agent-heavy users; 6 comments.

8. **SSH 连接失败：exit code 255（疑似 DeepSeek TUI shell 沙箱 TCP 22 出站阻断）** — [Issue #1829](https://github.com/Hmbown/CodeWhale/issues/1829)  
   SSH/scp from the TUI shell sandbox fails with exit 255, likely due to outbound TCP 22 blocking. Breaks remote workflows for Windows users; 6 comments.

9. **Web: finish the #4934 dictionary spine — retire every isZh branch and inline { en, zh } module** — [Issue #5337](https://github.com/Hmbown/CodeWhale/issues/5337)  
   Architectural i18n cleanup tracking all remaining `isZh` branches and inline language pairs. This is currently driving multiple open PRs; 4 comments.

10. **简化第三方模型配置，增加预制模板** — [Issue #5350](https://github.com/Hmbown/CodeWhale/issues/5350)  
    Users want pre-built templates for OpenCode Zen, OpenCode Go, Agnes, and Sensenova, plus a “test connection” button and cache fixes; 4 comments.

## Key PR Progress

1. **release: 0.9.9** — [PR #5476](https://github.com/Hmbown/CodeWhale/pull/5476)  
   Release PR for v0.9.9: fixes the session-wedging shell bug, honest labeling of unverified context windows/telemetry, and multiple reliability improvements.

2. **fix(tui): exec stream creation must fail soft and never wedge the shell tool** — [PR #5465](https://github.com/Hmbown/CodeWhale/pull/5465)  
   Root-cause fix for the “Shell execution failed: Failed to create streaming shell output” failure after memory/resource exhaustion.

3. **fix(tui): DeepSeek V4 tiered peak/off-peak pricing resolved per turn** — [PR #5470](https://github.com/Hmbown/CodeWhale/pull/5470)  
   Replaces flat V4 price rows with UTC-hour peak/off-peak tiers resolved from each turn’s timestamp.

4. **fix(tui): restore session cost when live pricing is unverifiable (#5241)** — [PR #5402](https://github.com/Hmbown/CodeWhale/pull/5402)  
   Stops sessions from showing `unverified_live_pricing` forever when the pricing endpoint returns 503; falls back honestly instead of dropping cost entirely.

5. **fix(models): bring first-party model rows and pricing current as of 2026-08-17** — [PR #5485](https://github.com/Hmbown/CodeWhale/pull/5485)  
   Re-verified model catalog and price tables across DeepSeek, xAI, Z.ai, and other providers; keeps cost display accurate.

6. **feat(tui): show and open the live /rc session link; send a stable device id** — [PR #5480](https://github.com/Hmbown/CodeWhale/pull/5480)  
   The `/rc` banner now surfaces/opens the live web session URL and stops minting a new device ID per session.

7. **fix(tui): persist approval outcomes before execution** — [PR #5491](https://github.com/Hmbown/CodeWhale/pull/5491)  
   Implements durable, fail-closed approval receipts: execution is denied if the outcome cannot be persisted, and closed/interrupted approvals are reconstructed on resume.

8. **perf(context): compact all noisy web tool results** — [PR #5474](https://github.com/Hmbown/CodeWhale/pull/5474)  
   Applies the existing noisy-result soft limit to `Web`, `web_search`, `web.run`, and `fetch_url`, reducing context bloat from web tool output.

9. **fix(config): resolve owned direct model casing safely** — [PR #5475](https://github.com/Hmbown/CodeWhale/pull/5475)  
   Fixes lowercase saved selectors like `glm-5.2` being misclassified as foreign model IDs before the owning Z.ai row could match.

10. **feat(web): move the docs shell onto the dictionary spine (#5337)** — [PR #5488](https://github.com/Hmbown/CodeWhale/pull/5488)  
    Removes five `isZh` ternaries from the docs layout so ja/vi/ko/ru/uk/es/pt-BR/id locales can translate portal hero strings; sibling PR [#5490](https://github.com/Hmbown/CodeWhale/pull/5490) continues the same migration for shared components.

## Feature Request Trends

- **Simpler configuration, fewer knobs**  
  Users and maintainers want less surface area in agent/tool schemas ([#5324](https://github.com/Hmbown/CodeWhale/issues/5324)), subagent spawning ([#5123](https://github.com/Hmbown/CodeWhale/issues/5123)), and fleet config ([#5098](https://github.com/Hmbown/CodeWhale/issues/5098)). Third-party providers should be one-click templates rather than raw Base URL/API key setup ([#5350](https://github.com/Hmbown/CodeWhale/issues/5350)).

- **Chinese localization and full i18n coverage**  
  There is strong demand for localized docs and UI: fully localize documentation to Chinese ([#5482](https://github.com/Hmbown/CodeWhale/issues/5482)), finish the locale dictionary spine ([#5337](https://github.com/Hmbown/CodeWhale/issues/5337)), and fix non-English route interaction bugs ([#5290](https://github.com/Hmbown/CodeWhale/issues/5290)).

- **Honest, durable operational state**  
  Users want approval outcomes persisted before execution ([#5360](https://github.com/Hmbown/CodeWhale/issues/5360)), session costs that don’t disappear when live pricing is unavailable ([#5241](https://github.com/Hmbown/CodeWhale/issues/5241)), and no silent migration surprises ([#2369](https://github.com/Hmbown/CodeWhale/issues/2369)).

- **Plugins, MCP, and multimodal agent capabilities**  
  Feature requests continue around a complete plugin marketplace ([#5311](https://github.com/Hmbown/CodeWhale/issues/5311)), MCP capability metadata ([#4170](https://github.com/Hmbown/CodeWhale/issues/4170)), and first-class screenshot/image reading for agents ([#5102](https://github.com/Hmbown/CodeWhale/issues/5102)).

## Developer Pain Points

- **Configuration path and migration inconsistencies** across OSes, especially Windows/Cygwin ([#2369](https://github.com/Hmbown/CodeWhale/issues/2369)), plus silent config shadowing between global/workspace fleet files ([#5098](https://github.com/Hmbown/CodeWhale/issues/5098)).
- **Flaky tests and red CI** are a recurring cost: verifier background flakes ([#5056](https://github.com/Hmbown/CodeWhale/issues/5056)), known parallel-load fixtures ([#5355](https://github.com/Hmbown/CodeWhale/issues/5355)), and both-platform CI failures ([#5403](https://github.com/Hmbown/CodeWhale/issues/5403)).
- **Model-facing tool contracts are too complex** — 32-field schemas and excessive agent spawn options cause models to error or self-block ([#5324](https://github.com/Hmbown/CodeWhale/issues/5324), [#5123](https://github.com/Hmbown/CodeWhale/issues/5123)).
- **Crashes/hangs in real workflows**: TUI exits after long waits ([#5424](https://github.com/Hmbown/CodeWhale/issues/5424)), VS Code crashes during autonomous test runs ([#1651](https://github.com/Hmbown/CodeWhale/issues/1651)), and large-text multi-agent jobs freeze on `agent_wait` ([#1425](https://github.com/Hmbown/CodeWhale/issues/1425)).
- **Network and endpoint reliability**: sandboxed SSH outbound blocks ([#1829](https://github.com/Hmbown/CodeWhale/issues/1829)), flaky completions URL errors ([#4683](https://github.com/Hmbown/CodeWhale/issues/4683)), and pricing endpoint 503s breaking cost display ([#5241](https://github.com/Hmbown/CodeWhale/issues/5241)).
- **Context compression feels misaligned with model capabilities** — users ask why 1M-context models still compress at 128K ([#5239](https://github.com/Hmbown/CodeWhale/issues/5239)).

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

No activity in the last 24 hours.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*