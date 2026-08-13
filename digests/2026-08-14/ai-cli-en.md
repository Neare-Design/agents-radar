# AI CLI Tools Community Digest 2026-08-14

> Generated: 2026-08-13 23:34 UTC | Tools covered: 10

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

# Cross-Tool AI CLI Comparison Report — 2026-08-14

## 1. Ecosystem Overview

The AI CLI developer-tool space is in a phase of rapid differentiation: Claude Code is consolidating around enterprise-grade session continuity, subagent forking, and model-behavior control, while OpenAI Codex is iterating quickly on Rust-based alpha builds but is straining on Windows sandbox reliability. Gemini CLI is investing heavily in behavioral evaluation infrastructure and provider polyglotism, whereas Qwen Code is pushing multi-agent fleet coordination and security hardening. Smaller/emerging tools such as OpenCode, Pi, Kimi Code, DeepSeek TUI (Codewhale), and Grok Build are competing on plugin flexibility, TUI performance, persistent memory, and local-provider support. Cross-cutting demand is converging on MCP reliability, persistent memory, Windows support, subagent observability, and session continuity.

## 2. Activity Comparison

*Counts reflect issues/PRs highlighted in the digest, not full repository totals.*

| Tool | Sampled Issues | Sampled PRs | Releases Today |
|---|---|---|---|
| Claude Code | 10 | 2 | v2.1.232, v2.1.231 |
| OpenAI Codex | 10 | 10 | 3 alpha builds (rust-v0.148.0-alpha.11–13) |
| Gemini CLI | 10 | 10 | v0.56.0-nightly.20260813 |
| GitHub Copilot CLI | 10 | 1 | v1.0.80-0 |
| Kimi Code CLI | 3 | 0 | None |
| OpenCode | 10 | 10 | v1.18.18 |
| Pi | 10 | 10 | None |
| Qwen Code | 10 | 10 | v0.21.11, v0.21.12-preview.1 |
| DeepSeek TUI / Codewhale | 10 | 10 | v0.9.7 |
| Grok Build | 0 | 0 | No activity |

Most active PR ecosystems today: **Codex, Gemini, OpenCode, Pi, Qwen, DeepSeek TUI**. Release velocity is highest at **Claude Code, Codex, Gemini, Copilot CLI, OpenCode, Qwen**.

## 3. Shared Feature Directions

| Direction | Tools | Specific Needs |
|---|---|---|
| **Persistent memory / cross-session context** | Kimi, Gemini CLI, OpenCode, Claude Code, DeepSeek TUI | Auto memory with redaction, user-defined memory, memory tables, memory fidelity, session continuity |
| **MCP reliability & auth** | Claude, Codex, Copilot CLI, OpenCode, Qwen, DeepSeek | OAuth refresh races, callback ports, per-server timeouts, scoped connectors, corrupted config surfacing, `nextCursor` correctness |
| **Multi-agent / subagent orchestration** | Claude, Codex, Gemini, Qwen, DeepSeek, Copilot | Forking, lifecycle controls, subagent hangs, MAX_TURNS reporting, supervised teammates, circuit breakers, activity visibility |
| **Windows / cross-platform reliability** | Codex, Qwen, DeepSeek, OpenCode, Pi, Copilot | Sandbox helpers, MSIX PowerShell, Ctrl+V paste, installer checksums, WSL MCP, Cygwin config paths, console flashing |
| **Context / cost management** | Claude, Pi, Kimi, Qwen | Prompt-cache waste, compaction before provider overflow, runaway token limits, placeholder-response rejection |
| **TUI / UX polish** | Copilot CLI, OpenCode, Pi, DeepSeek, Gemini | Markdown/LaTeX rendering, Vim mode, layout options, visual-line caching, CJK alignment, status visibility |
| **Security & supply-chain hardening** | OpenCode, Qwen, Gemini, Copilot | `curl|bash` integrity, SSRF guards, hook trust boundaries, A2A auth, checkpoint path traversal, SHA-pinned CI actions |
| **Session sync & resumability** | Claude, Codex, Kimi, DeepSeek | CLI/desktop history sync, configurable chat storage, silent drops, stuck resume states |

## 4. Differentiation Analysis

- **Claude Code** is positioning as the **enterprise workflow hub**: cross-surface session sync, subagent forking with full prompt-cache inheritance, desktop integration, and MCP OAuth robustness. Its community is most focused on model behavior and documentation accuracy.
- **OpenAI Codex** is the fastest-moving **Rust-native agent runtime**, but Windows sandbox/extension reliability is its clearest weak point. Guardian V2 context expansion and daemon lifecycle controls show a focus on governed, automatable execution.
- **Gemini CLI** is differentiating on **evaluation discipline**: component-level behavioral evals, tool-call failure summaries, and support for Claude models (Sonnet 4.5, Opus 4.8). Its pain points cluster around subagent reliability and Auto Memory privacy.
- **GitHub Copilot CLI** is tightly coupled to the **GitHub/VS Code Copilot ecosystem**, with custom-agent frontmatter, MCP registry policies, and `--ahp` multi-client sessions. It is quieter on PRs but has steady issue-driven demand.
- **OpenCode** emphasizes **plugin/provider flexibility**: hooks, memory plugins, fallback chains, MCP/WSL fixes, and legacy layout. Its V2 migration is causing coexistence risk, but contributor momentum is high.
- **Pi** is the **terminal-performance specialist**: visual-line caching, viewport-only rendering, SIGINT hygiene, and CJK width handling. It is also actively pursuing multi-provider resilience and extension-API hardening.
- **Qwen Code** is driving the **multi-agent fleet** direction with `/coordinate`, supervised teammates, live-session registries, and desktop/web-shell parity. Security hardening around hooks, Git mutations, and Local Control is unusually prominent.
- **DeepSeek TUI / Codewhale** is rebranding and maturing its architecture: crate decomposition, local DS4 provider setup, Auto-Review model-guardian escalation, and CJK/i18n polish. It serves local-model and Chinese-language users especially.
- **Kimi Code** is minimal but shows clear demand for persistent memory and streaming correctness; its small issue sample masks significant UX-reliability concerns.
- **Grok Build** has no observable activity today.

## 5. Community Momentum & Maturity

- **Claude Code** has the strongest and most engaged community: the top feature request (#28791) has 123 👍 and the verbosity issue has 110 👍. It is the most mature on release cadence and ecosystem integration.
- **OpenAI Codex** has very high issue engagement (53 comments on Windows resource loading, 36 👍 on subagent model compatibility) and constant alpha releases — rapid iteration with rough edges.
- **Gemini CLI** shows disciplined momentum: p1-class subagent correctness issues, eval epics, and security-relevant PRs. Community is smaller but technically focused.
- **OpenCode** and **Pi** have healthy contributor pipelines (10 PRs each). Pi’s community is particularly performance- and correctness-oriented; OpenCode is feature- and plugin-oriented.
- **Qwen Code** is rapidly shipping major architecture (Agent Plugins, multi-agent workflows, live-session registry) and addressing security in parallel — one of the most aggressive roadmaps.
- **DeepSeek TUI** is transitioning to Codewhale with active PR work, but legacy migration and Windows/Cygwin fragmentation are ongoing drags.
- **Copilot CLI** has broad user demand but slower PR throughput; its feature-request surface is consolidating around MCP auth and custom-agent configuration.
- **Kimi and Grok** are peripheral in this digest: Kimi has emergent community needs, Grok is dormant.

## 6. Trend Signals

1. **Multi-agent orchestration is becoming a default, not a feature**. Claude enables subagent forking by default; Qwen ships `/coordinate`; Codex, Gemini, and Copilot are all chasing subagent lifecycle and observability fixes.
2. **Persistent memory is the next competitive battleground.** Kimi’s #1283, Gemini’s Auto Memory, OpenCode’s `agent_memory`, Claude’s memory-fidelity bugs, and DeepSeek’s cross-session requests all point to users expecting stateful assistants.
3. **MCP is table stakes, but authentication and transport are still immature.** OAuth callback-port configs, refresh races, timeouts, corrupted-config handling, and strict-client `null` fields are recurring across at least six tools.
4. **Windows support is a trust differentiator.** Codex, Qwen, DeepSeek, Pi, Copilot, and OpenCode all have Windows-specific breakage; tools that stabilize Windows sandbox/installer/CLI behavior will win enterprise and hybrid-dev users.
5. **Cost and context efficiency are rising in priority.** Prompt-cache waste (Claude), late compaction (Pi), runaway token generation (Kimi), and placeholder responses (Qwen) all indicate users are hitting real cost ceilings in long agentic sessions.
6. **Security scrutiny is moving from the model to the toolchain.** Reports on `curl|bash` upgrades, SSRF in webfetch, hook trust boundaries, Git worktree mutations, and supply-chain action pinning show the community is now auditing AI CLI infrastructure itself.
7. **TUI quality and developer ergonomics matter for retention.** Vim mode, LaTeX rendering, layout flexibility, CJK alignment, and fast large-session rendering are recurring asks — terminal UX is a product surface, not an implementation detail.

**For developers and decision-makers:** prioritize MCP auth resilience, subagent observability, Windows hardening, context-cost guardrails, and persistent memory when selecting or building AI CLI tooling.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights Report
*Data snapshot: 2026-08-14 | Source: github.com/anthropics/skills*

---

## 1. Top Skills Ranking

The following PRs represent the most-attended Skill activity in the repository, ranked by discussion volume and cross-referenced issue engagement.

### #1298 — skill-creator: run_eval.py 0% recall fix *(Open)*
**Function:** Fixes the skill-creator evaluation pipeline (`run_eval.py`, `run_loop.py`, `improve_description.py`) so skill descriptions are scored against real trigger signals instead of noise. Addresses the Windows subprocess reading bug, trigger detection, and parallel worker issues.
**Discussion highlights:** This PR is the culmination of a long-running bug saga (#556 with 12 comments, #1169) — the eval loop reporting `recall=0%` for every description. The fix installs the eval artifact as a real skill, which is the root-cause correction.
**Status:** Open; directly resolves the most-voted tooling bug in the repo.
🔗 https://github.com/anthropics/skills/pull/1298

### #514 — document-typography skill *(Open)*
**Function:** A typographic quality-control skill preventing orphan words, widow paragraph headers, and numbering misalignment in AI-generated documents.
**Discussion highlights:** Positions typography as a universal post-processing concern — "these issues affect every document Claude generates." Broad applicability across all document-producing skills.
**Status:** Open since March 2026.
🔗 https://github.com/anthropics/skills/pull/514

### #723 — testing-patterns skill *(Open)*
**Function:** A comprehensive testing skill covering the Testing Trophy model, unit-testing patterns (AAA, naming, edge cases), React component testing with Testing Library, and what *not* to test.
**Discussion highlights:** Responds to demand for structured code-quality guidance; the most complete testing-oriented submission in the repository.
**Status:** Open; candidate for merging given the maturity of content.
🔗 https://github.com/anthropics/skills/pull/723

### #568 — ServiceNow platform skill *(Open)*
**Function:** A broad ServiceNow platform assistant spanning ITSM, ITOM, ITAM/SAM Pro, FSM, HRSD/CSM, SPM/PPM, Vulnerability Response, Security Incident Response, CSDM, and IntegrationHub.
**Discussion highlights:** The most ambitious enterprise-platform skill submitted; updated as recently as 2026-08-12, indicating active maintainer engagement.
**Status:** Open; likely near review.
🔗 https://github.com/anthropics/skills/pull/568

### #486 — ODT skill (OpenDocument) *(Open)*
**Function:** Create, fill, read, and convert OpenDocument Format files (.odt, .ods), including template filling and ODT-to-HTML parsing.
**Discussion highlights:** Directly addresses the document-format gap beyond DOCX/PDF; triggered by any mention of "ODT/ODS/ODF/OpenDocument/LibreOffice."
**Status:** Open since March 2026.
🔗 https://github.com/anthropics/skills/pull/486

### #1367 — self-audit skill *(Open)*
**Function:** A universal delivery-audit skill: mechanical verification of claimed output files, followed by a four-dimension reasoning-quality gate ordered by damage severity.
**Discussion highlights:** Tied to proposal #1385 (Reasoning Quality Gate Pipeline); part of a push for systematic output verification across any tech stack or model.
**Status:** Open; updated July 2026.
🔗 https://github.com/anthropics/skills/pull/1367

### #83 — skill-quality-analyzer & skill-security-analyzer *(Open)*
**Function:** Two meta-skills: a five-dimension quality analyzer for SKILL.md files (structure, documentation, examples, resources…) and a security analyzer for skills.
**Discussion highlights:** The only PR directly addressing skill *security* analysis — especially salient given the 43-comment trust-boundary issue #492.
**Status:** Open since November 2025.
🔗 https://github.com/anthropics/skills/pull/83

### #525 — pyxel skill (retro game development) *(Open)*
**Function:** Wraps the pyxel-mcp server for the Pyxel retro/pixel-art/8-bit game engine; covers the write → run_and_capture → inspect → iterate workflow.
**Discussion highlights:** The most creative-domain-specific skill in the top tier; demonstrates MCP-server-as-skill integration.
**Status:** Open; updated July 2026.
🔗 https://github.com/anthropics/skills/pull/525

---

## 2. Community Demand Trends

Distilled from the Issues list (sorted by comments):

1. **Skill evaluation & quality tooling is the #1 pain point.** Issues #556 (12 comments) and #1169 document that `run_eval.py` reports 0% trigger rates universally, making the description-optimization loop meaningless. Three separate PRs (#1298, #1099, #1050) attack this from different angles — a clear signal that meta-tooling reliability is the community's most urgent need.

2. **Security & trust boundary concerns are rising.** Issue #492 (43 comments — the most-discussed issue in the repo) exposes that community skills under the `anthropic/` namespace impersonate official skills, creating an elevated-permission trust risk. This is the highest-attention conversation in the repository and directly motivates the security-analyzer submission (#83).

3. **Skill distribution & sharing friction.** Issue #228 (16 comments, 8 👍) requests org-wide skill sharing in Claude.ai; #189 (6 comments, 9 👍) reports duplicate skills across `document-skills` and `example-skills` plugins. The community wants better packaging, deduplication, and organizational distribution.

4. **Document-format coverage remains the strongest vertical.** DOCX (#541, #12), PDF (#538), ODT (#486), and typography (#514) form a dense cluster. The demand is for *correct and safe* document manipulation — whitespace corruption (#12) and tracked-change ID collisions (#541) show users are hitting real-world file-corruption bugs.

5. **New frontier: governance, memory & verification.** Proposals for agent-governance (#412), compact-memory symbolic state (#1329), and a reasoning-quality-gate pipeline (#1385) indicate the community is pushing Skills beyond task automation toward agent safety, state management, and output verification.

---

## 3. High-Potential Pending Skills

These open PRs have active discussion and recent updates, making them the most likely candidates to land soon.

- **#568 — ServiceNow platform skill** *(updated 2026-08-12)* — Broadest enterprise skill in the queue; recent activity suggests active review cycles.
  🔗 https://github.com/anthropics/skills/pull/568

- **#1538 — Agent Skills spec compliance fix** *(updated 2026-08-12)* — Brings the `template` and `docx` skills back under the official spec; low-risk, correctness-focused fix.
  🔗 https://github.com/anthropics/skills/pull/1538

- **#1479 — plan-file-hygiene skill** *(updated 2026-07-27)* — Addresses issue #1417 on planning-artifact lifecycle management; direct issue-to-PR linkage increases merge probability.
  🔗 https://github.com/anthropics/skills/pull/1479

- **#1367 — self-audit skill** *(updated 2026-07-02)* — Complements the open proposal #1385; a well-specified, model-agnostic quality gate.
  🔗 https://github.com/anthropics/skills/pull/1367

- **#525 — pyxel skill** *(updated 2026-07-15)* — Mature MCP-based skill with clear workflow; author is the pyxel-mcp maintainer.
  🔗 https://github.com/anthropics/skills/pull/525

- **#723 — testing-patterns skill** *(updated 2026-04-21)* — High-content-value skill; the most complete testing submission to date.
  🔗 https://github.com/anthropics/skills/pull/723

- **#514 — document-typography skill** *(updated 2026-03-13)* — Universal applicability to every document-generation workflow; strong candidate for inclusion.
  🔗 https://github.com/anthropics/skills/pull/514

- **#1298 — skill-creator eval fix** *(updated 2026-06-23)* — Directly resolves the most-voted bug cluster (#556, #1169); high community pressure to merge.
  🔗 https://github.com/anthropics/skills/pull/1298

---

## 4. Skills Ecosystem Insight

**The community's most concentrated demand is trust infrastructure — fixing the skill-creator evaluation pipeline, building quality/security-audit meta-skills, and addressing namespace impersonation — while document-format and enterprise-platform skills (ODT, DOCX/PDF reliability, ServiceNow, SAP) represent the strongest vertical growth areas.**

---

# Claude Code Community Digest — 2026-08-14

## Today's Highlights
v2.1.232 shipped with subagent forking enabled by default (forked subagents now inherit the full conversation and prompt cache, and non-teammate agent spawns run in the background), plus v2.1.231 fixed an MCP OAuth redirect-URI bug affecting pre-registered clients like Slack. Community energy is concentrated on model behavior (persistent verbosity, memory overriding) and prompt-cache cost inefficiency, while the long-running request to sync conversation history between CLI and desktop remains the most-backed open feature.

## Releases

**v2.1.232**
- Subagent forking is now on by default: a `subagent_type: "fork"` subagent inherits the full conversation and prompt cache; non-teammate agent spawns in interactive sessions now run in the background by default.
- Type `@` in the prompt to mention another Claude session by name.

**v2.1.231**
- Fixed MCP OAuth sign-in failing with a redirect URI mismatch for servers using a pre-registered OAuth client (e.g., Slack).

## Hot Issues

1. **[#28791 — Sync conversation history between CLI and Claude Code desktop app](https://github.com/anthropics/claude-code/issues/28791)** · 34 comments · 123 👍
   The most-upvoted open feature request. Users want seamless history continuity between terminal sessions and the desktop app. High engagement signals this is the top workflow-integration gap.

2. **[#65961 — Claude verbose code comments by default, ignores instructions to stop](https://github.com/anthropics/claude-code/issues/65961)** · 11 comments · 110 👍
   Strong community consensus that the model over-generates comments despite explicit direction. Heavy 👍 count makes this a priority model-behavior signal.

3. **[#52477 — Claude overrode explicit pronouns in user memory, defaulted to male bias](https://github.com/anthropics/claude-code/issues/52477)** · 12 comments
   A memory-fidelity bug where Claude ignored user-specified pronouns, reverting to a male default. Raises concerns about memory reliability and bias handling.

4. **[#63930 — Prompt cache fully re-created after turns with many parallel tool calls](https://github.com/anthropics/claude-code/issues/63930)** · 10 comments
   Since ~v2.1.154, cache is invalidated mid-session, with **74% of cache writes wasted** on Opus 4.8. A significant cost issue for heavy parallel-tool users, with a clear repro attached.

5. **[#86234 — Claude Desktop (Windows) crashes on ad-heavy pages in in-app Browser pane](https://github.com/anthropics/claude-code/issues/86234)** · 3 comments
   A full publisher ad stack in the in-app browser pane crashes the entire desktop app, killing all hosted Claude Code sessions. Reproduced on two machines — a severe stability bug for Windows desktop users.

6. **[#86502 — MCP connectors timeout at 30s despite fast endpoint response](https://github.com/anthropics/claude-code/issues/86502)** · 2 comments
   Two claude.ai-scoped MCP connectors fail with 30s timeouts in the CLI while working fine in the Claude app on the same account. Points to a scoping or transport bug in CLI MCP handling.

7. **[#67682 — Dispatch permanently stuck, never resets to QR pairing on Windows 11](https://github.com/anthropics/claude-code/issues/67682)** · 5 comments
   Remote dispatch pairing is unrecoverable once stuck — "Can't reach your desktop"/"Asleep" persists on mobile. Blocks Cowork/desktop workflows on Windows.

8. **[#71861 / #71865 / #71871 — Cybersecurity safety-filter false positives on drone/USB enumeration](https://github.com/anthropics/claude-code/issues/71861)** · 3 comments each
   Legitimate embedded-systems work (USB device enumeration, FOSS drone tooling, defensive firmware hardening) is being blocked by the cybersecurity filter. Frustrating for hardware developers; all three closed, so the maintainers likely considered them expected behavior.

9. **[#67938 — Weekly usage limit banner persists despite being at 31% usage](https://github.com/anthropics/claude-code/issues/67938)** · 2 comments
   A misleading TUI banner shows a weekly limit warning far below the actual threshold — a small UX bug, but one that erodes trust in usage reporting.

10. **[#52601 and the coygeek documentation batch](https://github.com/anthropics/claude-code/issues/52601)** · up to 7 comments each
    A series of ~20 documentation issues (most closed as stale) found settings paths, worktree behavior, auth token handling, hook inputs, and env vars documented incorrectly or not at all. Demonstrates ongoing drift between docs and actual CLI behavior.

## Key PR Progress

PR activity was light in the last 24 hours — only two PRs were updated:

1. **[#86537 — Fix duplicated word in CHANGELOG.md](https://github.com/anthropics/claude-code/pull/86537)** (open)
   Documentation-only typo fix ("to to" in the `CLAUDE_BASH_NO_LOGIN` entry). Minor, but keeps the changelog clean.

2. **[#60280 — chore(ci): SHA-pin remaining actions/checkout and actions/github-script](https://github.com/anthropics/claude-code/pull/60280)** (closed)
   Follow-up to #56784, SHA-pinning third-party GitHub Actions across six workflows (auto-close-duplicates, backfill-duplicate-comments, claude-dedupe-issues, claude-issue-triage, etc.). Supply-chain hygiene for the issue-triage bot infrastructure.

## Feature Request Trends

- **Cross-surface session sync** — The dominant request: unified conversation history between CLI, desktop app, and mobile (see #28791). Users increasingly treat Claude Code as a multi-device tool and expect state to follow them.
- **Model behavior control** — Two high-👍 requests (#65961 verbosity, #52477 memory pronouns) both ask for stricter adherence to explicit user instructions, especially persistent memory and comment style.
- **Documentation accuracy** — A large wave of stale-doc issues (settings paths, auth precedence, hook inputs, worktree behavior) shows users depend on docs as a source of truth and are actively filing when they drift.
- **MCP reliability and auth** — Between the Slack OAuth fix in v2.1.231 and #86502's timeouts, MCP connector robustness remains a recurring theme: auth flows, timeouts, and scoped connectors.

## Developer Pain Points

- **Prompt-cache cost waste** — #63930's 74% wasted `cache_creation` on parallel tool calls is the clearest cost complaint: users are effectively paying multiple times for the same context in long sessions.
- **Ignored instructions / verbosity** — "I told it to stop commenting and it kept commenting" is a common frustration, amplified by the 110 👍 on #65961.
- **Desktop stability** — The Windows crash on ad-heavy pages (#86234) and stuck Dispatch pairing (#67682) show the desktop surface still has reliability gaps that nuke entire sessions.
- **MCP connection timeouts** — 30s hard timeouts on healthy endpoints (#86502) are particularly confusing when the same connectors work in the Claude app.
- **Stale, incorrect docs** — The volume of closed-as-stale doc issues suggests users hit real walls (wrong settings paths, missing env vars) before discovering the shipped behavior differs from the docs.

*Digest covers the 24-hour window ending 2026-08-14, based on the anthropics/claude-code repository.*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-08-14

## Today’s Highlights  
The Codex repo shipped a new burst of `0.148.0-alpha` Rust builds and merged a large batch of infrastructure-focused PRs around Guardian review context, MCP configuration, and Windows sandbox reliability. Community discussion remains dominated by Windows sandbox/extension breakage and subagent lifecycle bugs, while longer-running feature requests for TUI/Vim improvements and configurable chat storage continue to gain traction.

## Releases  
Three new alpha builds were published in the last 24 hours:

- **rust-v0.148.0-alpha.11** — https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.11  
- **rust-v0.148.0-alpha.12** — https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.12  
- **rust-v0.148.0-alpha.13** — https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.13  

No detailed changelogs were included in the provided data.

## Hot Issues  

1. **Codex extension fails to load resources on Windows**  
   [#37458](https://github.com/openai/codex/issues/37458) — 53 comments / 11 👍  
   The Codex panel fails to start in VS Code on Windows with “The extension couldn’t load its resources.” This is one of the highest-engagement issues in the digest and blocks core extension usage for affected users.

2. **Subagent spawn rejects `gpt-5.6-luna` with `multi_agent_v2` enabled**  
   [#34700](https://github.com/openai/codex/issues/34700) — 15 comments / 36 👍  
   Windows App and CLI users report that `spawn_agent` fails for the newer model when multi-agent mode is on. High 👍 count indicates broad interest in new-model/subagent compatibility.

3. **Feature Request: make “Chats” project directory configurable**  
   [#19909](https://github.com/openai/codex/issues/19909) — 17 comments / 35 👍  
   Codex App stores chat data under `~/Documents/Codex`, which is problematic for iCloud-synced setups. Long-running popular request for user-controlled storage paths.

4. **TUI: support Markdown math rendering for inline and block LaTeX**  
   [#18906](https://github.com/openai/codex/issues/18906) — 15 comments / 22 👍  
   Terminal UI users want proper LaTeX rendering for technical documentation and math-heavy responses.

5. **Windows sandbox fails when resolved shell is MSIX PowerShell 7**  
   [#35871](https://github.com/openai/codex/issues/35871) — 13 comments / 3 👍  
   `CreateProcessAsUserW failed: 5 (Access is denied.)` occurs when the sandbox tries to launch the Microsoft Store build of `pwsh`. Important edge case for Windows users with Store-installed PowerShell.

6. **`browser.tabs.finalize()` silently terminates the entire Codex Desktop app**  
   [#35210](https://github.com/openai/codex/issues/35210) — 12 comments  
   Desktop browser-use API call can crash the whole app on Windows instead of finalizing a tab. Critical for anyone relying on browser automation.

7. **Windows Computer Use fails with `EPERM lstat` on Codex runtime**  
   [#37029](https://github.com/openai/codex/issues/37029) — 12 comments / 3 👍  
   Computer Use cannot get past app selection on Windows 11 due to a filesystem permission error targeting the Codex runtime directory.

8. **`codex-windows-sandbox-setup.exe` not found after clean install**  
   [#30829](https://github.com/openai/codex/issues/30829) — 10 comments  
   CLI setup can fail to locate the sandbox helper after clean installs, likely related to bin junction handling on Windows.

9. **Volatile plugin cache hash paths persist in Windows sessions, breaking skills**  
   [#25285](https://github.com/openai/codex/issues/25285) — 10 comments / 1 👍  
   Old threads reference absolute `SKILL.md` paths under plugin cache hash directories; after cache updates, older sessions lose skill-loading ability.

10. **macOS App OOM-crashes from `external-agent-import` parsing 1.73 GB**  
    [#36523](https://github.com/openai/codex/issues/36523) — 6 comments / 1 👍  
    Marked P0 regression: Codex/ChatGPT macOS app crashes at startup when it tries to import history from Claude Desktop’s app-support directory, causing V8 heap OOM.

## Key PR Progress  

1. **Embed Windows sandbox setup manifest in Bazel builds**  
   [#38450](https://github.com/openai/codex/pull/38450)  
   Fixes missing `asInvoker` manifest for the Windows sandbox setup helper when built via Bazel.

2. **Support per-server MCP OAuth callback ports**  
   [#38448](https://github.com/openai/codex/pull/38448)  
   Adds `oauth.callback_port` to MCP server config and preserves it through plugin/skill metadata serialization.

3. **Add running-task exit choices to local daemon sessions**  
   [#38447](https://github.com/openai/codex/pull/38447)  
   On `Ctrl-C` with an empty composer, users can cancel the running task, exit and leave the task running, or stop the daemon.

4. **Retain client developer messages across context compaction**  
   [#38445](https://github.com/openai/codex/pull/38445)  
   Preserves annotated client-authored developer instructions after compaction when `retain_client_developer_messages` is enabled.

5. **Expose model upgrade retirement times**  
   [#38449](https://github.com/openai/codex/pull/38449)  
   Parses optional `retirement_at` metadata from model upgrade info and exposes it as nullable Unix timestamps.

6. **Give Guardian V2 full tool action context**  
   [#38441](https://github.com/openai/codex/pull/38441)  
   Exposes the original pre-hook `ToolPayload` so Guardian can assess the full requested action rather than only a tool name and call ID.

7. **Add structured telemetry for response retries**  
   [#38452](https://github.com/openai/codex/pull/38452)  
   Emits trace-safe `codex.retry` events with attempt, delay, retry layer, and operation across HTTP, sampling, compaction, and connection recovery.

8. **App-server support for reverting paginated threads**  
   [#38440](https://github.com/openai/codex/pull/38440)  
   Adds experimental `thread/revert`, replacing durable history with the prefix before a given `beforeTurnId` while preserving the thread ID.

9. **Resolve local MCP refs in Code Mode tool schemas**  
   [#31901](https://github.com/openai/codex/pull/31901)  
   Resolves JSON Pointer `$ref` values in MCP tool schemas when rendering TypeScript declarations, supporting both `$defs` and `definitions`.

10. **exec-server: start managed network proxy on executor**  
    [#31453](https://github.com/openai/codex/pull/31453)  
    Moves HTTP/SOCKS proxy ownership to the remote executor and sends sanitized managed-network policy to child processes, including fail-closed behavior for MITM/credential injection.

## Feature Request Trends  

- **TUI/Vim mode remains a top priority**  
  Repeated requests include starting Vim mode in Insert mode by default ([#21850](https://github.com/openai/codex/issues/21850)), proper `c*` change operations ([#32745](https://github.com/openai/codex/issues/32745)), and basic Vim keybindings ([#33296](https://github.com/openai/codex/issues/33296)).

- **Configurable local storage locations**  
  Users want control over Codex App data directories, especially to avoid iCloud-synced `~/Documents` ([#19909](https://github.com/openai/codex/issues/19909)).

- **Richer TUI rendering**  
  Markdown math/LaTeX rendering support is requested for technical workflows ([#18906](https://github.com/openai/codex/issues/18906)).

- **MCP and plugin configuration flexibility**  
  PR momentum around per-server OAuth callback ports, local schema refs, and curated plugin catalogs indicates growing demand for more configurable MCP integrations.

- **Better subagent lifecycle controls**  
  Users want completed subagents to stop counting against limits ([#22779](https://github.com/openai/codex/issues/22779)) and stale subagents to not appear stuck after app restarts ([#38408](https://github.com/openai/codex/issues/38408)).

## Developer Pain Points  

- **Windows sandbox reliability is a recurring theme**  
  Multiple issues involve missing sandbox helpers, MSIX PowerShell failures, broken auto-upgrade launchers, and setup manifest problems ([#30829](https://github.com/openai/codex/issues/30829), [#35871](https://github.com/openai/codex/issues/35871), [#38039](https://github.com/openai/codex/issues/38039), [#38450](https://github.com/openai/codex/pull/38450)).

- **Extension startup and resource loading fragility**  
  VS Code sidebar/webview failures and CSP-blocked fonts block users repeatedly ([#37458](https://github.com/openai/codex/issues/37458), [#37517](https://github.com/openai/codex/issues/37517)).

- **Session and resume failures**  
  Persisted state can become impossible to replay: NUL bytes in `function_call.name`, 404s on `/compact`, and enormous subagent logs ([#24369](https://github.com/openai/codex/issues/24369), [#38323](https://github.com/openai/codex/issues/38323), [#31198](https://github.com/openai/codex/issues/31198)).

- **Subagent state is often confusing or wrong**  
  Completed subagents still count against thread limits, and old subagents can remain “running” after restart ([#22779](https://github.com/openai/codex/issues/22779), [#38408](https://github.com/openai/codex/issues/38408)).

- **macOS startup and auth flow regressions**  
  V8 OOM crashes during external agent import and desktop MFA enrollment dead-ends cause significant onboarding friction ([#36523](https://github.com/openai/codex/issues/36523), [#34934](https://github.com/openai/codex/issues/34934)).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-08-14

## 1. Today's Highlights

The project shipped nightly `v0.56.0-nightly.20260813`, focused on hardening the behavioral evaluation infrastructure with a new tool-call formatter and integrated failure summaries. A critical capacity-exhaustion retry regression was addressed in PR #28790, while the team also merged support for Claude Sonnet 4.5 and Opus 4.8 model definitions. Community attention remains concentrated on subagent reliability: the long-standing generalist-agent hang (#21409) and misreported MAX_TURNS recovery (#22323) continue to draw engagement.

## 2. Releases

**v0.56.0-nightly.20260813.g1ac337739** — Nightly release with:
- **Eval validation** (#28344): New validation tooling for behavioral eval authoring.
- **Tool call formatter + failure summaries** (#28305): Improves eval output readability on tool-call failures, which should make debugging eval regressions faster.
- Changelog for v0.55.1.

## 3. Hot Issues

1. **[#22323 — Subagent recovery after MAX_TURNS is reported as GOAL success](https://github.com/google-gemini/gemini-cli/issues/22323)** *(p1, 12 comments)*: `codebase_investigator` reports `status: "success"` even after hitting the max turn limit before doing any analysis. This actively hides interruption from users; the community flagged it as a correctness issue that undermines trust in agent reporting.

2. **[#21409 — Generalist agent hangs](https://github.com/google-gemini/gemini-cli/issues/21409)** *(p1, 8 comments, 8 👍)*: Deferring to the generalist agent hangs indefinitely, with users reporting waits of up to an hour for trivial tasks like folder creation. Workaround: instructing the model never to use subagents.

3. **[#24353 — Robust component-level evaluations](https://github.com/google-gemini/gemini-cli/issues/24353)** *(p1 epic, 7 comments)*: Tracks evolution of the behavioral eval suite (76 tests across 6 supported Gemini models) toward more granular component-level coverage. Signals the team's investment in regression prevention.

4. **[#22745 — Assess impact of AST-aware file reads, search, and mapping](https://github.com/google-gemini/gemini-cli/issues/22745)** *(p2 epic, 7 comments)*: Investigates whether AST-aware tools can reduce token noise and turn counts vs. naive file reads. Directly related to the codebase_investigator quality issues seen elsewhere.

5. **[#21968 — Gemini does not use skills and sub-agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)** *(p2, 6 comments)*: Anecdotal but heavily endorsed report that custom skills/sub-agents are only invoked when explicitly instructed, despite relevant descriptions (e.g., `gradle`/`git` skills).

6. **[#26522 — Auto Memory retries low-signal sessions indefinitely](https://github.com/google-gemini/gemini-cli/issues/26522)** *(p2, 5 comments)*: Memory extraction agent never marks low-signal sessions as processed, causing repeated re-scans. Resource-waste bug in the background memory pipeline.

7. **[#26525 — Add deterministic redaction and reduce Auto Memory logging](https://github.com/google-gemini/gemini-cli/issues/26525)** *(p2 security, 4 comments)*: Auto Memory sends transcript content to the extraction model *before* prompt-based redaction occurs, and logs existing skills — a privacy gap flagged by the community.

8. **[#25166 — Shell command execution stuck with "Waiting input" after completion](https://github.com/google-gemini/gemini-cli/issues/25166)** *(p1, 4 comments, 3 👍)*: Simple CLI commands hang while showing as active/"Awaiting user input" even after finishing. High frequency for a core shell-integration flow.

9. **[#20079 — Symlinked agent files not recognized](https://github.com/google-gemini/gemini-cli/issues/20079)** *(p2, 4 comments)*: `~/.gemini/agents/filename.md` symlinks are silently ignored. Common pattern for dotfile-managed setups (chezmoi, stow, etc.), making this a friction point for power users.

10. **[#21983 — Browser subagent fails on Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)** *(p1, 4 comments)*: Browser agent fails under Wayland compositors despite reaching `Termination Reason: GOAL`. Linux desktop users increasingly affected as Wayland becomes default.

## 4. Key PR Progress

1. **[#28790 — Context-aware silent retries and availability TTL for capacity errors](https://github.com/google-gemini/gemini-cli/pull/28790)** *(p1, closed, size/l)*: Fixes critical capacity-exhaustion retry regression (#28761). Non-interactive runs now back off/retry automatically with up to 2 silent retries; key for CI automation reliability.

2. **[#28803 — Add Claude Sonnet 4.5 and Opus 4.8 model definitions](https://github.com/google-gemini/gemini-cli/pull/28803)** *(closed, size/xl)*: Adds model constants, alias resolution, policy-chain fallbacks for both Claude models. Broadens BYO-model support beyond Gemini.

3. **[#28801 — Rollback entire multi-turn request on cancellation/abort](https://github.com/google-gemini/gemini-cli/pull/28801)** *(closed, size/m)*: Prevents aborted tool-call turns from leaving chat history in an un-responded state, which corrupted subsequent unrelated requests.

4. **[#28804 — Eval tools expansion](https://github.com/google-gemini/gemini-cli/pull/28804)** *(open, size/l)*: Adds behavioral evals for `read_many_files`, `get_internal_docs`, `list_mcp_resources`, and `read_mcp_resource`. Directly advances the component-level eval epic (#24353).

5. **[#28789 — Fix vscode-ide-companion stop() hang and keep-alive failure threshold](https://github.com/google-gemini/gemini-cli/pull/28789)** *(open, size/xl)*: Fixes `IdeServer.stop()` hanging when streaming MCP sessions are open, plus a resource leak in the keep-alive ping loop (#28785). Important for VS Code extension stability.

6. **[#28787 — Don't treat corrupt MCP enablement config as empty](https://github.com/google-gemini/gemini-cli/pull/28787)** *(p1, open)*: JSON parse failures were silently collapsed into `{}`, defaulting every MCP server to enabled. Now surfaces the corruption instead of running unintended servers — a security-relevant fix.

7. **[#28699 — Enforce authentication and stop checkpoint path traversal on A2A server](https://github.com/google-gemini/gemini-cli/pull/28699)** *(open, size/l)*: Custom REST routes bypass `UserBuilder` auth entirely; also fixes checkpoint path traversal. Both issues are serious for multi-user A2A deployments.

8. **[#28792 — Normalize git environment and resolve workspace state mismatch](https://github.com/google-gemini/gemini-cli/pull/28792)** *(closed, size/l)*: Standardizes Git subprocess env config and fixes a workspace-trust initialization mismatch. Predictable, non-interactive internal Git execution.

9. **[#28624 — Prevent boolean thought parts leaking as `[Thought: true]` text](https://github.com/google-gemini/gemini-cli/pull/28624)** *(open, size/m)*: Filters internal boolean `thought` fields from model-thought text rendering. Fixes #23525; cosmetic but user-visible noise.

10. **[#28679 — Improve Vertex AI 401 error message for standard API keys](https://github.com/google-gemini/gemini-cli/pull/28679)** *(open, size/s)*: Replaces an opaque 401 with actionable guidance when users mistakenly use a Gemini API key with the Vertex AI auth type. Small DX win for auth misconfiguration.

Also noteworthy: **[#28797 — Workflow context probe for security research](https://github.com/google-gemini/gemini-cli/pull/28797)** (open, size/s) adds an inert probe logging workflow context during `npm ci` as part of a Google OSS-VRP submission. Reviewers should inspect this carefully before merging given CI-supply-chain sensitivities.

## 5. Feature Request Trends

- **AST-aware code understanding**: Two linked epics (#22745, #22746) propose AST-aware reads, search, and codebase mapping to cut token noise and misaligned reads — directly targeting codebase_investigator quality.
- **Behavioral eval expansion**: Continuous investment in evals (#24353) with new coverage for skills, web fetch, MCP resources, and multi-file batch reads (PRs #28804, #28788).
- **Subagent observability**: Community requests to expose subagent trajectories via `/chat share` (#22598) and include subagent context in `/bug` reports (#21763) — users want visibility into what agents actually do.
- **Browser agent resilience**: Requests for automatic session takeover and lock recovery (#22232) and proper `settings.json` override support (#22267).
- **Agent self-awareness**: Users want the CLI to accurately explain its own flags, hotkeys, and mechanics (#21432) and to proactively use configured skills (#21968).

## 6. Developer Pain Points

- **Subagent reliability is the #1 friction point**: Hangs (#21409), misreported termination reasons (#22323), permission bypasses (#22093), and crashes in output hooks (#22186) all point to agent orchestration being the most fragile area.
- **Shell integration bugs**: Commands stuck in "Waiting input" after completion (#25166) and interactive prompts (e.g., `vite` scaffolding) hanging (#22465) disrupt the core coding loop.
- **Memory/Auto Memory privacy concerns**: Transcript content sent to models before redaction (#26525), invalid patches silently skipped (#26523), and endless low-signal retries (#26522) show the memory subsystem needs hardening.
- **Configuration and environment friction**: Symlink agents ignored (#20079), corrupt MCP configs silently enabling servers (#28787), browser agent ignoring `settings.json` (#22267), and Wayland failures (#21983) frustrate users with non-default setups.
- **Scale ceilings**: 400 errors with >128 tools (#24246) and model-created tmp scripts scattered across workspaces (#23571) show agent behavior degrades as project complexity grows.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-08-14

## Today's Highlights
v1.0.80-0 shipped with a new `--enable-mcp-server` flag and clearer multi-client session indicators in `--ahp` mode. The issue tracker remains dominated by MCP authentication/reliability problems, especially around OAuth refreshes and CI token policy. Custom-agent model/reasoning-effort configuration is still the most active feature request, with related bugs continuing to surface.

## Releases
**v1.0.80-0**
- Added `--enable-mcp-server` to re-enable MCP servers that were disabled in settings for the current run.
- Sessions shared with another CLI now display this in `--ahp` mode: a joined session row leads with `2 clients` (or more) when others are attached, both in the Sessions tab and related views.

No other release changes were provided in the last 24 hours.

## Hot Issues
Selected 10 noteworthy issues from the 27 updated in the last 24 hours:

1. **[#2904: Custom Agent YAML Frontmatter Should Support Reasoning Effort](https://github.com/github/copilot-cli/issues/2904)**  
   Open since April with high community interest (20 👍, 6 comments). Users want per-agent reasoning-effort configuration instead of only a global `--effort` flag. This is the umbrella issue behind several newer duplicates.

2. **[#4345: Reasoning effort 'medium' is not supported for model 'claude-haiku-4.5'](https://github.com/github/copilot-cli/issues/4345)**  
   Closed but representative: when feature flags activate `medium` effort defaults, sub-agent execution fails because `claude-haiku-4.5` does not support that setting. A similar fresh report exists at [#4473](https://github.com/github/copilot-cli/issues/4473).

3. **[#2133: Custom agent frontmatter `model` field rejects array syntax](https://github.com/github/copilot-cli/issues/2133)**  
   Incompatibility with VS Code Copilot Chat: agents that use `model:` as an array parse in Chat but fail to load in Copilot CLI. Community asks for parity with the editor tooling.

4. **[#3954: `explore` tool hardcodes model to `gpt-5.4-mini`, ignoring custom/DeepSeek API configuration](https://github.com/github/copilot-cli/issues/3954)**  
   Users with custom API endpoints report the `explore` tool bypasses configured models and sends `gpt-5.4-mini` to the endpoint, breaking non-GPT setups.

5. **[#4346: MCP registry policy fetch returns 403 for Actions GITHUB_TOKEN](https://github.com/github/copilot-cli/issues/4346)**  
   Blocks all non-default MCP servers in GitHub Actions when using the documented PAT-less setup. Important for CI users who adopted the new `copilot-requests: write` workflow.

6. **[#4480: Atlassian MCP OAuth fails with "Incompatible authorization server" on 1.0.79](https://github.com/github/copilot-cli/issues/4480)**  
   A regression from 1.0.71: connecting to `https://mcp.atlassian.com/v1/mcp` fails during OAuth discovery due to issuer mismatch. Fresh triage report with no comments yet.

7. **[#4472: Concurrent MCP tool calls during token refresh cancel in-flight calls](https://github.com/github/copilot-cli/issues/4472)**  
   Each concurrent refresh spins up a new `rmcp::service`, causing "transport closed before the tool responded." This is a meaningful reliability issue for OAuth-protected Streamable HTTP MCP servers.

8. **[#4469: Orphaned `permission.requested` event replays on every session resume](https://github.com/github/copilot-cli/issues/4469)**  
   A directory-access prompt from a command run 10 days earlier keeps reappearing on every resume and cannot be permanently dismissed. Long-lived session users are hit hard.

9. **[#4467: Long-running agent sessions exhaust event storage and appear cancelled](https://github.com/github/copilot-cli/issues/4467)**  
   Sessions spawning many subagents can exhaust the remote session event store, making live sessions appear inactive or cancelled. This undermines trust in session status for heavy agent use.

10. **[#4477: Session and prompt lost when stopping an action or hitting the stop button](https://github.com/github/copilot-cli/issues/4477)**  
   Users report the entire session, including the original prompt and edits, is deleted after Stop. Data-loss bugs like this typically get rapid community attention.

## Key PR Progress
Only one pull request was updated in the last 24 hours:

- **[#4476: docs: document proposed custom-agent effort frontmatter (Option A)](https://github.com/github/copilot-cli/pull/4476)**  
  Closed. Adds documentation for a dedicated `effort` frontmatter field for custom agents, parallel to `model`. This targets issue #2904 with **Option A** and expands the "Custom Agents" reference section in the README. No review comments or merge details were provided in the snapshot.

The 24-hour PR window was otherwise quiet; no other code PRs were updated.

## Feature Request Trends
The most consistent feature directions across recent issues:

- **Per-agent model and reasoning-effort control**  
  Custom agent frontmatter should support `effort` and array-based `model` declarations, and subagent model overrides must not be silently ignored by the parent session. (#2904, #2133, #4462, #3565)

- **MCP server resilience and authentication**  
  Users increasingly run remote MCP servers and need robust OAuth refresh, retry/backoff on transient 5xx errors, case-insensitive server-name collision detection, and support for Actions `GITHUB_TOKEN` in registry policy fetches. (#4346, #4472, #4464, #4466, #4478, #4480)

- **Session lifecycle tooling**  
  Developers want an official way to list running sessions with status — similar to Claude Code’s `claude agents --json` — plus better restore/archive behavior and protection against session loss. (#4470, #4474, #4477)

- **Permissions and prompt suppression**  
  `allowed_directories` entries should suppress shell "outside allowed directory" prompts, and stale permission events should not replay across session resumes. (#4482, #4469)

## Developer Pain Points
Recurring frustrations in the last 24 hours:

- **Silent model downgrades/overrides**: Subagents and built-in tools ignore configured models, leading to confusing behavior and failed calls. (#3565, #3954, #4462)
- **Reasoning-effort incompatibility**: The CLI applies `medium` effort to models that do not support it, producing hard failures during sub-agent execution. (#4345, #4473)
- **MCP auth friction**: Remote MCP users face repeated interactive sign-ins, socket errors on Windows, OAuth issuer mismatches, and canceled tool calls during refresh. (#4464, #4463, #4480, #4472)
- **Session/event data loss**: Stop actions delete sessions, long-running agents exhaust event stores, and stale permission events persist indefinitely. (#4477, #4467, #4469)
- **Windows-specific resource leaks**: `--server --stdio` does not release extension-host processes; sessions accumulate four processes each until server exit. (#4468)
- **Plugin/skill UI confusion**: `/plugins` displays identical checkmarks for enabled and disabled skills, and disabled state is not persisted. (#4471)

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-08-14

## Today's Highlights
No new releases or PR updates landed in the last 24 hours, but three long-running issues saw renewed attention. The most notable developments are continued demand for a persistent memory system, a serious ACP/print streaming hang that can silently drop conversation data, and a reported runaway generation that produced 88k tokens of gibberish in a single LLM step.

## Releases
No new versions were published in the last 24 hours.

## Hot Issues
Only 3 issues were updated in the last 24 hours. All are listed below.

### 1. Feature Request: Memory System — Persistent context across sessions  
**MoonshotAI/kimi-cli Issue #1283** | Author: CatKang | Comments: 38 | Updated: 2026-08-13  
URL: https://github.com/MoonshotAI/kimi-cli/issues/1283  

This long-running enhancement request asks for both automatic memory (AI-managed notes) and manual memory (user-defined persistent instructions). With 38 comments, it remains one of the most active feature discussions in the repo, indicating strong community demand for continuity across CLI sessions — not just file-based context, but stored project patterns and preferences.

### 2. ACP/print streaming response hangs silently: no idle timeout, replaced wheel partial does not fall off the wire  
**MoonshotAI/kimi-cli Issue #2598** | Author: ai-agent-workbench | Comments: 1 | Updated: 2026-08-13  
URL: https://github.com/MoonshotAI/kimi-cli/issues/2598  

In `kimi acp` mode, a stream can deliver all content deltas but never receive a terminal `[DONE]`/finish frame, leaving the CLI waiting indefinitely with no configurable idle timeout. Worse, if the user sends a new message, the stuck turn is silently replaced and the already-streamed answer is never written to `wire.jsonl`. This is a correctness and observability issue for agentic workflows and automation, not just interactive use.

### 3. Runaway garbled generation — 88k tokens of gibberish in one LLM step  
**MoonshotAI/kimi-cli Issue #2597** | Author: kdp123 | Comments: 1 | Updated: 2026-08-13  
URL: https://github.com/MoonshotAI/kimi-cli/issues/2597  

A single LLM step ran for ~53 minutes and emitted 88,114 tokens of incoherent, repetitive output. This points to a lack of step-level token/safety limits and termination handling in the CLI. For developers relying on long-running autonomous sessions, this is a critical reliability concern.

## Key PR Progress
No pull requests were updated in the last 24 hours. There is no new PR activity to report.

## Feature Request Trends
- **Persistent memory / cross-session context** is the clearest requested direction, centered on Issue #1283. The community wants the CLI to remember user preferences, project patterns, and important context automatically.
- **Reliable streaming semantics** are emerging as a de facto requirement: proper end-of-stream detection, idle timeouts, and guaranteed persistence of partial responses.
- **Generation guardrails** are implied by the runaway-token issue — users want limits on max tokens per step and protection against degenerate loops.

## Developer Pain Points
- **Silent hangs and lost data**: The ACP/print hang (#2598) is especially painful because the UI looks successful while logs and wire data are incomplete. Developers cannot trust automation output without explicit finish frames or timeouts.
- **No escape from runaway generations**: Issue #2597 shows that a broken generation can consume enormous time and token cost without being stopped, suggesting the CLI needs built-in safeguards.
- **Lack of session continuity**: The popularity of the memory-system request highlights that developers want the CLI to behave less like a fresh sandbox and more like a persistent coding assistant.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-08-14

Source: [`github.com/anomalyco/opencode`](https://github.com/anomalyco/opencode)

## Today's Highlights
OpenCode shipped **v1.18.18** with targeted provider fixes for Kimi system-prompt selection and xAI `xhigh` reasoning effort. Community attention is centered on the legacy-layout demand ([#37012](https://github.com/anomalyco/opencode/issues/37012)), a clipboard regression in VS Code Server/Docker ([#41470](https://github.com/anomalyco/opencode/issues/41470)), and a wave of security reports around `curl|bash` upgrades and context pruning. Several contributor PRs also landed to improve plugin auto-update reliability, MCP/WSL behavior, and response-model metadata preservation.

## Releases

- [v1.18.18](https://github.com/anomalyco/opencode/releases/tag/v1.18.18)
  - **Core bugfixes**
    - Select the Kimi system prompt correctly for official Moonshot and Kimi providers.
    - Fix `xhigh` reasoning effort for xAI models.

No new features were listed in this patch release.

## Hot Issues

1. [Keep legacy layout option (#37012)](https://github.com/anomalyco/opencode/issues/37012) — 37 comments, 41 👍. The most active discussion this week: users want to keep the old layout because it provides quicker access to options from the main window and supports a workspace-based workflow. The new layout requires more navigation.

2. [“Copied to clipboard” doesn’t work (#41470)](https://github.com/anomalyco/opencode/issues/41470) — 15 comments. Copying inside VS Code Server/Docker shows a success message, but the text never reaches the system clipboard, breaking remote development workflows.

3. [Regression: plugin provider.models() hook no longer populates custom providers (#25630)](https://github.com/anomalyco/opencode/issues/25630) — 15 comments, 6 👍. After PR #25167, plugins can no longer add models for user-declared custom providers, impacting users with private model catalogs.

4. [Error 429 FreeUsageLimitError on Zen models (#42029)](https://github.com/anomalyco/opencode/issues/42029) — 5 comments. Users are hitting free-tier rate limits on Zen models despite reporting little or no usage. Related reports also mention DeepSeek V4 Flash and mimo models ([#42074](https://github.com/anomalyco/opencode/issues/42074)).

5. [GitHub Copilot provider shows zero models (#42083)](https://github.com/anomalyco/opencode/issues/42083) — 5 comments. Authentication succeeds, but `opencode models github-copilot` returns “Provider not found” and no Copilot models appear in the TUI model picker.

6. [Desktop app: provider/model/MCP fail to load on startup (#40516)](https://github.com/anomalyco/opencode/issues/40516) — 4 comments. A regression affecting roughly 80% of startup attempts for some organizations; `v1.18.4` is the last known-good version.

7. [Security: `opencode upgrade` uses curl|bash without integrity verification (#42434)](https://github.com/anomalyco/opencode/issues/42434) — 3 comments. Flags missing checksum/signature validation in the upgrade path, raising supply-chain and TOCTOU concerns.

8. [Security: context pruning silently drops instruction-bearing content (#42437)](https://github.com/anomalyco/opencode/issues/42437) — 2 comments. Compaction is currently seen as a cost optimization, but it can remove constraints/instructions and change agent behavior.

9. [Security: webfetch can reach loopback/private addresses — SSRF (#42435)](https://github.com/anomalyco/opencode/issues/42435) — 2 comments. `tool/webfetch.ts` has no private-address guard, and the earlier guard PR #40851 was closed unmerged.

10. [opencode2 mutates shared V1 database and breaks opencode 1.x coexistence (#42260)](https://github.com/anomalyco/opencode/issues/42260) — 2 comments. The V2 beta migrates the shared database schema, breaking `/move` in V1 and leaving sessions trapped in worktrees.

## Key PR Progress

- [feat(core): add sessionID to agent-invoked Shell.create before hook (#40668)](https://github.com/anomalyco/opencode/pull/40668) — Allows plugins to attribute agent shell commands to the originating session. Closes #40657.

- [fix(opencode): preserve response model metadata (#42433)](https://github.com/anomalyco/opencode/pull/42433) — Keeps the AI SDK’s structured `response.modelId` on assistant turns instead of only persisting the requested alias. Closes #42420.

- [feat(memory): add agent_memory table and memory-tools plugin (#42425)](https://github.com/anomalyco/opencode/pull/42425) — Adds an `agent_memory` table plus a plugin for cloud backup/restore of AgentMemory via Supabase. Closes #41998.

- [fix(opencode): plugin auto-update with temp residue cleanup (#42427)](https://github.com/anomalyco/opencode/pull/42427) — Fixes `@latest` plugin auto-update stalls and cleans temporary npm residue after installs. Closes #16608.

- [some experimental perf improvements (#40427)](https://github.com/anomalyco/opencode/pull/40427) — Reduced v2-only performance series: faster session-route loading, lower baseline overhead, and related optimizations.

- [feat(processor): add model fallback chain when retries are exhausted (#42424)](https://github.com/anomalyco/opencode/pull/42424) — Automatically tries configured fallback models once the primary model exhausts retry attempts. Closes #10287.

- [fix(mcp): retry failed MCP connections to handle parallel spawn race condition (#42431)](https://github.com/anomalyco/opencode/pull/42431) — Reduces intermittent “Connection closed” errors when MCP servers are spawned with `concurrency: "unbounded"`. Closes #41996.

- [fix(skill): ensure plugin config hooks run before skill discovery (#42430)](https://github.com/anomalyco/opencode/pull/42430) — Ensures plugins such as superpowers can register custom skill paths before discovery runs. Closes #28646.

- [fix(desktop): wrap MCP commands with wsl.exe when WSL mode is enabled (#42429)](https://github.com/anomalyco/opencode/pull/42429) — Fixes MCP `local` commands referencing Linux executables on Windows/WSL desktop setups. Closes #28159.

- [fix(provider): add kimi-for-coding custom handler and fix model detection for k2p6 (#42428)](https://github.com/anomalyco/opencode/pull/42428) — Properly detects and routes Kimi K2.6 (`k2p6`) through the `kimi-for-coding` provider path. Closes #23933.

## Feature Request Trends

- **Layout and workspace flexibility** — The strongest signal is demand for the legacy layout and richer workspace flows: [keep legacy layout (#37012)](https://github.com/anomalyco/opencode/issues/37012), [workspace flows in new layout (#38790)](https://github.com/anomalyco/opencode/pull/38790), and [right sidebar for background subagents (#42369)](https://github.com/anomalyco/opencode/issues/42369).

- **Plugin lifecycle management** — Users want manual plugin updates and cleaner synchronization between config and `package.json`: [manual `opencode plugin update` (#18544)](https://github.com/anomalyco/opencode/issues/18544), [stale plugin deps never cleaned (#30526)](https://github.com/anomalyco/opencode/issues/30526), and [auto-update temp residue cleanup (#42427)](https://github.com/anomalyco/opencode/pull/42427).

- **Explicit SDK contracts for automation** — Developers working with plugins/proxies want structured metadata and machine-readable contracts: [structured Retryable.action (#37083)](https://github.com/anomalyco/opencode/issues/37083), [preserve response.modelId (#42420)](https://github.com/anomalyco/opencode/issues/42420), and [sessionID in Shell.create hooks (#40668)](https://github.com/anomalyco/opencode/pull/40668).

- **Memory and session controls** — Requests for persistent memory and safer session actions are gaining traction: [agent_memory table (#42425)](https://github.com/anomalyco/opencode/pull/42425) and [confirmation dialog before archiving (#42423)](https://github.com/anomalyco/opencode/pull/42423).

- **TUI/UX state visibility** — Users want more visibility into running work: [background activities sidebar (#42369)](https://github.com/anomalyco/opencode/issues/42369), [unified task-state colors/icons (#42426)](https://github.com/anomalyco/opencode/pull/42426), and [optimistic prompt rendering (#36757)](https://github.com/anomalyco/opencode/pull/36757).

## Developer Pain Points

- **Provider/model discovery regressions** — Repeated breakage in provider plugins and model lists: [plugin provider.models() regression (#25630)](https://github.com/anomalyco/opencode/issues/25630), [GitHub Copilot zero models (#42083)](https://github.com/anomalyco/opencode/issues/42083), and [Kimi K2.6 detection fixes (#42428)](https://github.com/anomalyco/opencode/pull/42428).

- **Remote, container, and desktop friction** — Common environment-specific issues keep surfacing: [clipboard broken in VS Code Server (#41470)](https://github.com/anomalyco/opencode/issues/41470), [desktop startup load failures (#40516)](https://github.com/anomalyco/opencode/issues/40516), [WSL MCP command handling (#42429)](https://github.com/anomalyco/opencode/pull/42429), and [Windows console flashing on subprocess spawn (#42440)](https://github.com/anomalyco/opencode/issues/42440).

- **Rate-limit and retry reliability** — Free-tier and proxy retry behavior is a source of recurring frustration: [429 FreeUsageLimitError (#42029)](https://github.com/anomalyco/opencode/issues/42029), [unlimited usage exploit via VPN rotation (#34344)](https://github.com/anomalyco/opencode/issues/34344), and [infinite retry loop (#29143)](https://github.com/anomalyco/opencode/issues/29143).

- **Security and trust in tooling** — Several security-focused reports landed in the last 24 hours: [`curl|bash` upgrade without integrity checks (#42434)](https://github.com/anomalyco/opencode/issues/42434), [webfetch SSRF (#42435)](https://github.com/anomalyco/opencode/issues/42435), and [context pruning integrity (#42437)](https://github.com/anomalyco/opencode/issues/42437).

- **V2 migration/coexistence risk** — Users testing V2 are hitting shared-state problems: [opencode2 mutates V1 database (#42260)](https://github.com/anomalyco/opencode/issues/42260), [V2 missing todowrite/todoread tools (#42421)](https://github.com/anomalyco/opencode/issues/42421), and [Build agent stuck in Plan mode after revert (#42439)](https://github.com/anomalyco/opencode/issues/42439).

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-08-14

## Today's Highlights

Terminal hygiene and TUI performance dominate this digest: frankieyep's PR #8082 fixes SIGINT raw-mode corruption and large-session resume floods, while affanali2k3's visual-lines caching (PR #8066) tackles linear keystroke latency in big prompt buffers. The most-engaged open bug remains #6879 (auto-compaction not triggering until provider overflow, 19 comments, 17 👍), and one PR (#7993) was withdrawn with the note "Sorry, this was an agent gone wild." No new releases shipped in the last 24 hours.

## Releases

None in the last 24 hours.

## Hot Issues

1. **#6879 — Auto-compaction never triggers after context grows past 100% until provider overflow** — [earendil-works/pi#6879](https://github.com/earendil-works/pi/issues/6879)  
   The most-engaged issue this week (19 comments, 17 👍). A 2-hour agentic turn on gpt-5.6-sol climbed past the compaction threshold until the API rejected the request at 373k tokens. Broad support for checking compaction after every agentic step, not only at turn boundaries.

2. **#7836 — Edit fuzzy match misses lines with whitespace-length differences** — [earendil-works/pi#7836](https://github.com/earendil-works/pi/issues/7836)  
   `normalizeForFuzzyMatch` doesn't collapse whitespace runs or strip leading whitespace, so content-identical `oldText` fails to match. Especially punishing for small models that emit slightly different indentation.

3. **#8029 — Very slow performance moving in prompt editor** — [earendil-works/pi#8029](https://github.com/earendil-works/pi/issues/8029)  
   A single arrow keypress took 1,650 ms with ~7,000 lines in the prompt buffer. Paired with PR #8066, which fixes it via visual-lines caching.

4. **#7791 — Global Undici dispatcher inherits 16 KiB maxHeaderSize** — [earendil-works/pi#7791](https://github.com/earendil-works/pi/issues/7791)  
   Pi's global `EnvHttpProxyAgent` doesn't set `maxHeaderSize`, so global `fetch` rejects valid responses with `UND_ERR_HEADERS_OVERFLOW`. Now closed.

5. **#7779 — Allow trusted Unix users to share PI_CODING_AGENT_DIR** — [earendil-works/pi#7779](https://github.com/earendil-works/pi/issues/7779)  
   `auth.json` and `models-store.json` are written with mode `0600`, so the first user to create them locks out all other users on shared machines.

6. **#7829 — Invalid settings.json silently ignored; misleading 'bash not found' on Windows** — [earendil-works/pi#7829](https://github.com/earendil-works/pi/issues/7829)  
   Unescaped backslashes in a Windows `shellPath` produce invalid JSON that's silently dropped, then Pi reports "bash not found" instead of the real parse error — a diagnostic dead end.

7. **#8079 — Resuming a large session floods the terminal with entire history replay** — [earendil-works/pi#8079](https://github.com/earendil-works/pi/issues/8079)  
   A 759 KB session (~6,300 rendered lines) emitted ~844 KB of output over 18 seconds on resume, polluting scrollback. Fixed by PR #8082's viewport-only rendering.

8. **#8031 — openai-codex mid-stream termination restarts the entire response** — [earendil-works/pi#8031](https://github.com/earendil-works/pi/issues/8031)  
   On failures after substantial streaming, Pi retries the whole request while keeping the failed partial output visible, duplicating content and burning tokens.

9. **#8055 — Ambiguous-width chars counted as 1 column break CJK table alignment** — [earendil-works/pi#8055](https://github.com/earendil-works/pi/issues/8055)  
   Characters like ① ± … € render 2 columns wide on CJK terminals but are counted as 1, misaligning TUI table borders. An i18n edge case affecting a significant share of users.

10. **#8000 — @ file autocomplete ranks deep nested matches above direct children** — [earendil-works/pi#8000](https://github.com/earendil-works/pi/issues/8000)  
    Typing `@~/<dir>/pro` surfaces deep `projects` matches while the direct child the user almost certainly wants never appears. A basename-tie ranking issue in the autocomplete.

## Key PR Progress

1. **#8082 — fix(tui): render only the visible viewport in fullRender; restore terminal on SIGINT** (closed) — [earendil-works/pi#8082](https://github.com/earendil-works/pi/pull/8082)  
   Two terminal-hygiene fixes verified with a pty harness: large-session resumes no longer replay history, and SIGINT restores echo, cursor, bracketed paste, and window title.

2. **#8066 — fix(tui): add visual line caching** (open) — [earendil-works/pi#8066](https://github.com/earendil-works/pi/pull/8066)  
   Fixes #8029 by caching visual-line computations, invalidating only on width or text change; also extracts a shared `VisualLine` type.

3. **#8084 — fix(coding-agent): don't swallow the prompt after boolean extension flags** (closed) — [earendil-works/pi#8084](https://github.com/earendil-works/pi/pull/8084)  
   Boolean flags like `--plan` consumed the next CLI argument; `pi -p --plan "prompt"` silently started an empty session and exited 0.

4. **#8086 — fix(ai): fall back to legacy Gemini tool schema** (closed) — [earendil-works/pi#8086](https://github.com/earendil-works/pi/pull/8086)  
   Some generativelanguage endpoints reject `parametersJsonSchema` and other modern JSON Schema fields; this PR falls back to the legacy Schema message.

5. **#8085 — feat(tui): cancel active mouse selection with Escape** (open) — [earendil-works/pi#8085](https://github.com/earendil-works/pi/pull/8085)  
   Adds standard editor behavior: press Escape mid-drag to clear a selection without auto-copying — useful for people who "click and select like maniacs while reading."

6. **#8070 — fix(coding-agent): validate extension flag defaults** (open) — [earendil-works/pi#8070](https://github.com/earendil-works/pi/pull/8070)  
   Models `registerFlag()` as a discriminated union so a boolean flag with `default: "false"` can no longer silently return a truthy string.

7. **#7984 — fix(coding-agent): update grok-mermaid to 0.2.3** (open) — [earendil-works/pi#7984](https://github.com/earendil-works/pi/pull/7984)  
   Resolves #7832, substantially improving mermaid rendering (classes ignored for now); includes before/after screenshots.

8. **#6216 — feat: Add Amazon Bedrock Mantle OpenAI Responses provider** (open) — [earendil-works/pi#6216](https://github.com/earendil-works/pi/pull/6216)  
   New provider for Bedrock Mantle's OpenAI Responses API via OpenAI's Bedrock Provider; supersedes an earlier attempt.

9. **#8067 — Use APP_NAME in user-facing messages** (closed) — [earendil-works/pi#8067](https://github.com/earendil-works/pi/pull/8067)  
   Replaces remaining hardcoded "pi" strings with `APP_NAME` so rebranded builds look consistent; no output change upstream.

10. **#8057 — fix(examples): todo renderResult returns undefined on validation errors** (open) — [earendil-works/pi#8057](https://github.com/earendil-works/pi/pull/8057)  
    A failed schema validation produces `details = {}` (truthy), skipping all guards and returning `undefined` from `renderResult`, which crashes the TUI.

Also spotted: **#7993** ("fix(coding-agent): compact between tool turns") was closed by its author with *"Sorry, this was an agent gone wild. Please ignore this."* — a reminder that agent-generated PRs now require human/CI filtering.

## Feature Request Trends

- **Provider resilience & compatibility** — the strongest theme: Anthropic refusal server-side fallback (#8017), Codex `end_turn: false` handling (#7689), Kimi top-level `cached_tokens` tracking (#8075), and a new Bedrock Mantle provider (PR #6216). The Gemini legacy-schema fallback (PR #8086) shows the same drive toward provider-agnostic robustness.
- **Startup and rendering performance budgets** — #7739 asks for a startup-time budget targeting jcode-comparable latency/memory; #4254 (shared jiti singleton with `moduleCache`) targets extension-load dominance. Cold start is clearly a key competitive metric.
- **Extension API hardening** — per-tool opt-out of argument validation (#7607), an immutable pre-execute tool admission hook (#7092), validated extension flag types (#8070), and correct package metadata in `extensionsOverride` (#8078) show demand for a more predictable extension surface.
- **HTML export parity** — #8041 asks for mermaid and LaTeX rendering in HTML exports to match the TUI, following up on earlier attempt #7956.

## Developer Pain Points

- **Terminal hygiene** — raw-mode corruption on SIGINT (#8080), broken kitty keyboard protocol after `/exit` (#5065), and scrollback flooding on resume (#8079) make terminal restoration a recurring frustration.
- **Silent failures and misleading diagnostics** — invalid `settings.json` surfaces as "bash not found" on Windows (#7829); unknown slash commands like `/exit` are silently sent to the model as chat messages (#8081); 0600 permission locks fail only for the second user on shared systems (#7779).
- **Context/compaction anxiety** — #6879's high engagement shows users hit context limits in long agentic runs and expect proactive compaction before provider overflow, not after.
- **Provider edge-case whack-a-mole** — mid-stream termination duplicating output on openai-codex (#8031), Undici's 16 KiB header ceiling (#7791), and version-to-version catalog drift (qwen3.8-max-preview vs qwen3.8-max, #8083) are the cost of a multi-provider architecture.
- **Unnecessary permission prompts** — #7787: the default guideline "Inspect PI_* environment variables…" leads models to run env-style inspection during ordinary tasks, triggering permission prompts users didn't ask for.
- **Windows support gaps** — 31 failing server tests due to Unix-socket binding on Windows (#8047) plus the settings.json parsing issue (#7829) indicate Windows remains a second-class platform.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-08-14

## Today's Highlights

Qwen Code published two releases: v0.21.11 with Agent Plugins v1 and native `/coordinate` multi-agent workflows, plus v0.21.12-preview.1 with web-shell session and upload fixes. Community activity is heavily focused on the multi-agent fleet rollout, Windows regressions, and a series of security-hardening PRs around hooks, Git mutations, and release workflows. Note: the v0.21.11 pipeline quarantined its non-production SWE-bench Verified run, so that benchmark should not yet be treated as a reliable signal.

## Releases

- [v0.21.12-preview.1](https://github.com/QwenLM/qwen-code/releases/tag/v0.21.12-preview.1): `fix(web-shell): preserve standalone session target` and `feat(web-shell): support workspace file uploads` via [PR #9038](https://github.com/QwenLM/qwen-code/pull/9038).
- [v0.21.11](https://github.com/QwenLM/qwen-code/releases/tag/v0.21.11): Adds Agent Plugins v1 to extend agent capabilities ([#8834](https://github.com/QwenLM/qwen-code/pull/8834)) and enables native multi-agent workflows with read-only teammates via the `/coordinate` command ([#8804](https://github.com/QwenLM/qwen-code/pull/8804)). The release notes also mark SWE-bench Verified as **QUARANTINED** for the non-production E2E run, so benchmark numbers should be interpreted cautiously.

## Hot Issues

1. [#8718 RFC: Native coordination for independent Qwen sessions](https://github.com/QwenLM/qwen-code/issues/8718) — closed, 9 comments. The umbrella issue for the multi-agent fleet: a leader dispatches workers, observes correlated state, and collects structured results. High engagement because it defines the architecture behind `/coordinate`.

2. [#8678 fix(serve): Preserve the current session when a large restore times out](https://github.com/QwenLM/qwen-code/issues/8678) — open, 8 comments. A daemon reliability gap; the first PR landed in [#8691](https://github.com/QwenLM/qwen-code/pull/8691), but the full restore/recovery behavior remains unresolved.

3. [#7118 Windows standalone installer fails when powershell.exe cannot resolve Get-FileHash](https://github.com/QwenLM/qwen-code/issues/7118) — open, 7 comments, 3 👍. Long-standing Windows installation friction; users must fall back to `--method npm`.

4. [#9019 Gemini 2.5 models are unusable on Vertex AI: thinkingLevel is always sent](https://github.com/QwenLM/qwen-code/issues/9019) — open, 5 comments. Every request fails with a 400 because the unsupported `thinking_level` placeholder is always included, blocking Gemini 2.5 on Vertex.

5. [#9025 Keyless Vertex AI is not inferred from the environment](https://github.com/QwenLM/qwen-code/issues/9025) — open, 5 comments. Headless ADC runs exit before authenticating because `vertex-ai` auth type is not selected from env-only configuration.

6. [#9002 SDK Python rejects permission_mode="auto" although the CLI supports it](https://github.com/QwenLM/qwen-code/issues/9002) — open, 5 comments. Client-side SDK validation rejects a valid CLI mode, complicating SDK automation.

7. [#8586 Track activeWork and background Agent recovery](https://github.com/QwenLM/qwen-code/issues/8586) — open, 4 comments. Requests an explicit `activeWork` fact in daemon deep health plus recovery for background agents that outlive the foreground prompt.

8. [#8841 feat(cli): supervised teammate runtime — fleet MVP](https://github.com/QwenLM/qwen-code/issues/8841) — closed, 4 comments. The fleet MVP stage; shows the multi-agent roadmap is moving from design into staged delivery.

9. [#9108 Desktop: remaining Web Shell external links can still fail silently; MCP OAuth cannot complete](https://github.com/QwenLM/qwen-code/issues/9108) — open, 3 comments. The desktop webview still drops `target="_blank"` requests in several surfaces, breaking MCP OAuth and other browser-opening flows.

10. [#9061 Ctrl+V paste completely unresponsive in CLI on Windows — regression since 0.21.x](https://github.com/QwenLM/qwen-code/issues/9061) — open, 3 comments. Clipboard paste is broken in the Windows CLI; downgrading to 0.21.0 restores it. High-impact terminal regression.

## Key PR Progress

1. [#8969 feat(core): add a live-session registry and `qwen sessions ps`](https://github.com/QwenLM/qwen-code/pull/8969) — Adds a lightweight registry of running sessions, making live session discovery and monitoring much easier.

2. [#8848 feat(web-shell): redesign Channel policy and workspace management](https://github.com/QwenLM/qwen-code/pull/8848) — Exposes shared Channel access, session isolation, and workspace ownership controls across all manageable adapters.

3. [#8992 feat(mcp): add MCP 2026 core and WebShell Apps host](https://github.com/QwenLM/qwen-code/pull/8992) — First MCP 2026 client slice and an Apps host for daemon-backed WebShell sessions, including `ui://` tool metadata preservation.

4. [#8938 feat(core): reject upstream fail-fast placeholder responses](https://github.com/QwenLM/qwen-code/pull/8938) — Detects HTTP 200 placeholder bodies such as `(request timed out)` so the agent doesn't treat them as real model output.

5. [#9111 fix(desktop): open remaining external links through the shell opener](https://github.com/QwenLM/qwen-code/pull/9111) — Fixes the remaining silent-drop link surfaces in the desktop webview, addressing [#9108](https://github.com/QwenLM/qwen-code/issues/9108).

6. [#9112 fix(install): avoid Get-FileHash for Windows checksums](https://github.com/QwenLM/qwen-code/pull/9112) — Replaces PowerShell `Get-FileHash` with an inline .NET SHA-256 stream, targeting the installer issue [#7118](https://github.com/QwenLM/qwen-code/issues/7118).

7. [#8687 feat(daemon): guard cross-worktree Git mutations](https://github.com/QwenLM/qwen-code/pull/8687) — Blocks model-issued `run_shell_command` Git mutations that escape the session worktree via `-C`, `--work-tree`, or `--git-dir`.

8. [#8396 fix(hooks): close four trust-boundary holes in hook execution](https://github.com/QwenLM/qwen-code/pull/8396) — Removes HTTP redirect following and closes SSRF/trust-boundary gaps in repository-controlled hook configuration.

9. [#9106 feat: consolidate Local Control into one daemon-owned implementation](https://github.com/QwenLM/qwen-code/pull/9106) — Merges two parallel implementations of LAN-pairing Local Control into one daemon-based security model.

10. [#9098 feat(cli): enable dynamic workflows from a settings key](https://github.com/QwenLM/qwen-code/pull/9098) — Adds `tools.workflowsEnabled` so dynamic workflows can be enabled through configuration instead of an undocumented environment variable.

## Feature Request Trends

- **Multi-agent fleet and coordination** is the dominant direction: RFC [#8718](https://github.com/QwenLM/qwen-code/issues/8718), stage issues [#8840](https://github.com/QwenLM/qwen-code/issues/8840)–[#8843](https://github.com/QwenLM/qwen-code/issues/8843), and background-agent recovery in [#8586](https://github.com/QwenLM/qwen-code/issues/8586). Users want supervised teammates, persistence, recovery, and terminal attach.

- **Omni multimodal integration** remains a major experimental track: issues around policy-driven media degradation, memory recall, quarantine/GC, and cross-session reuse ([#8197](https://github.com/QwenLM/qwen-code/issues/8197), [#8186](https://github.com/QwenLM/qwen-code/issues/8186)–[#8190](https://github.com/QwenLM/qwen-code/issues/8190)).

- **Daemon and session observability** is a recurring ask: `activeWork` tracking, session restore safety, live-session registry, and `qwen sessions ps`.

- **Web-shell/desktop parity** is increasingly requested: Channel policies and workspace ownership ([#8845](https://github.com/QwenLM/qwen-code/issues/8845)), external-link reliability ([#9108](https://github.com/QwenLM/qwen-code/issues/9108)), and artifact path consistency ([#9083](https://github.com/QwenLM/qwen-code/issues/9083)).

- **Configuration and SDK ergonomics**: users want env-only Vertex auth inference, SDK parity for `permission_mode="auto"`, and a supported settings key for dynamic workflows.

## Developer Pain Points

- **Windows reliability**: installer SHA-256 failure ([#7118](https://github.com/QwenLM/qwen-code/issues/7118)), Ctrl+V paste regression ([#9061](https://github.com/QwenLM/qwen-code/issues/9061)), and the Desktop runtime Terminal issue ([#9043](https://github.com/QwenLM/qwen-code/issues/9043)) all indicate Windows-specific testing gaps.

- **Vertex AI / GCP friction**: unsupported `thinkingLevel` on Gemini 2.5 ([#9019](https://github.com/QwenLM/qwen-code/issues/9019)) and missing keyless/ADC environment detection ([#9025](https://github.com/QwenLM/qwen-code/issues/9025)) block headless and Vertex workflows.

- **File/artifact handling is too trusting**: `record_artifact` reports success for files that later appear "missing" ([#9083](https://github.com/QwenLM/qwen-code/issues/9083)); `read_file` sends non-image bytes to the model based only on a `.png` extension ([#9088](https://github.com/QwenLM/qwen-code/issues/9088)), causing raw 400 aborts.

- **Headless/tool-loop hard failures**: `NO_TOOL_RESULT_PROGRESS` aborts quiet turns after tool results ([#9026](https://github.com/QwenLM/qwen-code/issues/9026)), and compression side-queries can exceed small context windows ([#7960](https://github.com/QwenLM/qwen-code/issues/7960)).

- **CLI/SDK inconsistencies and TUI polish issues** add daily friction, such as `/statusline` clipping in short terminals ([#9037](https://github.com/QwenLM/qwen-code/issues/9037)).

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-08-14

## Today's Highlights

The v0.9.7 release formally introduces **Codewhale** as the public product name and deprecates the legacy `deepseek-tui` npm package. Maintainers and contributors are already moving toward v0.9.8, with notable PRs landing for Auto-Review model-guardian escalation, simplified model-facing schemas, local DS4 provider setup, and TUI test isolation. Community activity remains concentrated on long-running agent reliability, Windows/Cygwin environment quirks, and i18n/CJK polish.

## Releases

### v0.9.7

[Codewhale v0.9.7](https://github.com/Hmbown/CodeWhale/releases)

Codewhale is now the public product from Shannon Labs. The `codewhale` command, npm package, and release-asset names remain lowercase technical identifiers. The legacy npm package `deepseek-tui` is deprecated and receives no further releases; the release notes also begin covering the migration path for v0.8.x legacy users.

## Hot Issues

1. **[#998 文案展示不全](https://github.com/Hmbown/CodeWhale/issues/998)** — UI text is truncated in several places; users request hover tooltips for full content. High engagement: 11 comments. Indicates growing demand for CJK layout and completeness polish.

2. **[#1004 /dryrun — preview next chat completion request](https://github.com/Hmbown/CodeWhale/issues/1004)** — A feature request to preview the exact request payload before sending. Especially valuable for DeepSeek V4 Pro users with long system prompts, cached repo files, and multi-step tool definitions.

3. **[#5324 Simplify the 32-field agent tool schema](https://github.com/Hmbown/CodeWhale/issues/5324)** — Maintainer-opened issue: the model-facing `agent` tool has 32 properties, zero required fields, and 8 actions in one schema, causing models to error. Directly related to the Moonshot schema PR below.

4. **[#2369 CodeWhale config paths fragmented across OS/Cygwin](https://github.com/Hmbown/CodeWhale/issues/2369)** — Config and secret paths resolve differently across Windows, Cygwin, and legacy environments, plus a silent migration bug. High reliability impact for affected users.

5. **[#1425 Large-text sessions hang on agent_wait timeout](https://github.com/Hmbown/CodeWhale/issues/1425)** — Analyzing a 3M+ character novel spawned 10 subagents, which all showed as `Running`, but `agent_wait` timeouts kept interrupting the session. Highlights multi-agent orchestration stability gaps.

6. **[#1482 NVIDIA NIM returns 404](https://github.com/Hmbown/CodeWhale/issues/1482)** — API calls to NVIDIA NIM fail with `404 page not found`; user environment shows legacy v0.8.29. Suggests provider-version compatibility friction.

7. **[#1651 VS Code crashes while YOLO Agent runs test scripts](https://github.com/Hmbown/CodeWhale/issues/1651)** — VS Code exits unexpectedly when the autonomous YOLO Agent executes tests with DeepSeek v4-pro/v4-flash. Important for agent autonomy and IDE stability.

8. **[#1829 SSH fails with exit code 255 inside shell sandbox](https://github.com/Hmbown/CodeWhale/issues/1829)** — TCP 22 outbound appears blocked by the TUI shell sandbox. Legitimate remote workflows are being blocked; users expect configurable network sandboxing.

9. **[#1732 Merging and saving analysis reports is extremely slow](https://github.com/Hmbown/CodeWhale/issues/1732)** — Low cache hit rate and slow local document saving during merged analysis reports. A recurring performance complaint for large output workloads.

10. **[#5316 EPIC-005: CodeWhale TUI crate decomposition](https://github.com/Hmbown/CodeWhale/issues/5316)** — Umbrella tracking issue for decomposing the TUI into smaller crates. Signals a major architectural refactor and better modularity ahead.

## Key PR Progress

1. **[#5368 fix(tui): confine unguarded tests to the isolated state root](https://github.com/Hmbown/CodeWhale/pull/5368)** — Fixes the four machine-state-dependent tests from #5359. Adds regression coverage for lock-holder trust, settings paths, and display-probe isolation.

2. **[#5369 fix(tools): degrade Moonshot schemas instead of refusing conditionals](https://github.com/Hmbown/CodeWhale/pull/5369)** — A prerequisite for the #5324 schema simplification work. Prevents schema-generated refusals for Moonshot models.

3. **[#5358 feat(engine): auto-review denial rationale + turn circuit breaker](https://github.com/Hmbown/CodeWhale/pull/5358)** — First P0 slice of #5352. Denials now include rationale, and a circuit breaker prevents the model from rephrasing the same denied action until the step budget is exhausted.

4. **[#5364 feat(tui): render markdown blockquotes with a quote rail](https://github.com/Hmbown/CodeWhale/pull/5364)** — Adds proper quote-rail rendering for `>` blockquotes in the TUI transcript, with nesting, inline formatting, wrapping, and selection-copy support.

5. **[#5365 feat(provider): add first-class local DS4 setup](https://github.com/Hmbown/CodeWhale/pull/5365)** — Makes DwarfStar (DS4) a first-class local DeepSeek route with a prefilled keyless loopback preset, reusing the OpenAI-compatible transport.

6. **[#5339 fix(engine): suppress child-owned shell completions](https://github.com/Hmbown/CodeWhale/pull/5339)** — Filters child-owned background shell completion events out of the parent model stream while preserving parent completions and task/status visibility. Closes #5325.

7. **[#5353 feat(tui): model guardian tier for Auto-Review](https://github.com/Hmbown/CodeWhale/pull/5353)** — Turns Auto-Review into a true two-layer mode for v0.9.8: deterministic non-bypassable floor plus one-shot model-guardian escalation instead of silent blocking.

8. **[#5333 feat(tui): pin host terminal window as an always-on-top mini window](https://github.com/Hmbown/CodeWhale/pull/5333)** — Harvested from community PR #5318. Adds Windows PiP-style “shrink and pin on top” behavior via context menu or `/pin`.

9. **[#5336 fix(mcp): omit nextCursor when there are no further pages](https://github.com/Hmbown/CodeWhale/pull/5336)** — Fixes invalid `"nextCursor": null` in MCP responses. Strict clients such as Claude Code reject the `null` shape; the field is now omitted when no pages remain.

10. **[#5354 chore(ci): refresh the source-structure budget on main](https://github.com/Hmbown/CodeWhale/pull/5354)** — Small but important CI fix: refreshes the stale source-structure budget that was failing main’s Lint gate, unblocking open contributor PRs.

## Feature Request Trends

- **Localization and CJK input polish**  
  Users keep requesting better Chinese/Chinese-traditional support, including truncated text hover tips, full i18n coverage, and Chinese IME compatibility.  
  See [#998](https://github.com/Hmbown/CodeWhale/issues/998), [#790](https://github.com/Hmbown/CodeWhale/issues/790), [#2323](https://github.com/Hmbown/CodeWhale/issues/2323), [#1675](https://github.com/Hmbown/CodeWhale/issues/1675).

- **Cross-session memory and persistence**  
  Users want the TUI to remember prior sessions across restarts, and to actively reload written memory.  
  See [#2492](https://github.com/Hmbown/CodeWhale/issues/2492).

- **Long-running agent orchestration reliability**  
  Repeated requests for better timeout handling, cancellation, pause/resume, and circuit breaking for multi-agent workloads.  
  See [#1425](https://github.com/Hmbown/CodeWhale/issues/1425), [#1917](https://github.com/Hmbown/CodeWhale/issues/1917), [#5358](https://github.com/Hmbown/CodeWhale/pull/5358).

- **Provider flexibility and automatic failover**  
  Users want more provider options beyond the default path, including NVIDIA NIM fixes, local DS4 support, and automatic profile switching on rate limits.  
  See [#1482](https://github.com/Hmbown/CodeWhale/issues/1482), [#5365](https://github.com/Hmbown/CodeWhale/pull/5365), [#855](https://github.com/Hmbown/CodeWhale/issues/855).

- **TUI output UX improvements**  
  Requested features include clickable file previews, better output previews, `/dryrun` request previews, a built-in `tui_help` tool, and configurable keymaps.  
  See [#2342](https://github.com/Hmbown/CodeWhale/issues/2342), [#1682](https://github.com/Hmbown/CodeWhale/issues/1682), [#1004](https://github.com/Hmbown/CodeWhale/issues/1004), [#1708](https://github.com/Hmbown/CodeWhale/issues/1708), [#436](https://github.com/Hmbown/CodeWhale/issues/436).

## Developer Pain Points

- **Windows/Cygwin environment fragmentation**  
  Config paths diverge, shell command style does not match PowerShell/cmd, the default `.exe` launch looks worse than Windows Terminal, and Chinese IME misbehaves.  
  See [#2369](https://github.com/Hmbown/CodeWhale/issues/2369), [#1754](https://github.com/Hmbown/CodeWhale/issues/1754), [#1854](https://github.com/Hmbown/CodeWhale/issues/1854), [#2323](https://github.com/Hmbown/CodeWhale/issues/2323).

- **Sandbox restrictions block legitimate workflows**  
  The shell sandbox currently blocks SSH outbound TCP 22 and breaks SwiftPM tests via `sandbox-exec`.  
  See [#1829](https://github.com/Hmbown/CodeWhale/issues/1829), [#2617](https://github.com/Hmbown/CodeWhale/issues/2617).

- **Long-running sessions are still fragile**  
  Large report merges are slow, subagent waits can hang or interrupt sessions, and autonomous agent runs can crash VS Code.  
  See [#1732](https://github.com/Hmbown/CodeWhale/issues/1732), [#1425](https://github.com/Hmbown/CodeWhale/issues/1425), [#1651](https://github.com/Hmbown/CodeWhale/issues/1651).

- **Upgrade and migration state can get stuck**  
  `codewhale doctor` can permanently report `first-run` / `update checkpoint` as `needs action`, and legacy config migrations can silently mis-resolve paths.  
  See [#5340](https://github.com/Hmbown/CodeWhale/issues/5340), [#2369](https://github.com/Hmbown/CodeWhale/issues/2369).

- **Model-facing schema complexity causes real breakage**  
  Oversized tool schemas and invalid MCP shapes are causing models to error or clients to reject responses.  
  See [#5324](https://github.com/Hmbown/CodeWhale/issues/5324), [#5336](https://github.com/Hmbown/CodeWhale/pull/5336).

- **Tests pass in CI but fail on real developer machines**  
  Some TUI tests read `~/.codewhale` and display state, so they fail only on dev boxes with real local configuration.  
  See [#5359](https://github.com/Hmbown/CodeWhale/issues/5359), [#5368](https://github.com/Hmbown/CodeWhale/pull/5368).

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

No activity in the last 24 hours.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*