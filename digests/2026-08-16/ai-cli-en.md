# AI CLI Tools Community Digest 2026-08-16

> Generated: 2026-08-15 23:14 UTC | Tools covered: 10

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

# Cross-Tool AI CLI Comparison Report — 2026-08-16

## 1. Ecosystem Overview

The AI CLI ecosystem remains in a high-velocity hardening phase: 9 of 10 surveyed tools logged issue or PR activity in the last 24 hours, with Qwen Code (40 issues / 50 PRs), Pi (39 touched issues / 16 PRs), and Gemini CLI (10 PRs, including a CVSS 8.6 SSRF fix) leading raw volume. Release cadence is polarized — nightly/alpha channels (Codex, Gemini, Qwen) ship continuously, while Claude Code, OpenCode, Pi, Kimi, and DeepSeek TUI are in stabilization windows with no releases. Community feedback converges on reliability fundamentals: session continuity, compaction/token economics, OAuth stability, Windows desktop performance, and agent correctness dominate issue trackers across every vendor. Differentiation remains strong: provider-native CLIs (Claude, Gemini, Qwen, Kimi) compete against multi-provider agnostic agents (Pi, OpenCode, DeepSeek TUI) and GitHub-ecosystem tooling (Copilot CLI). The overall signal is that the industry is shifting from raw model access to operational trust — predictable sessions, transparent billing, and safe automation.

## 2. Activity Comparison

| Tool | Issues (24h) | PRs (24h) | Release Today |
|---|---|---|---|
| Claude Code | 10 tracked | 3 | None |
| OpenAI Codex | 10 tracked | 10 | `rust-v0.148.0-alpha.19` |
| Gemini CLI | 10 tracked | 10 | `v0.56.0-nightly.20260815` |
| GitHub Copilot CLI | 10 tracked | 2 | `v1.0.81-0` |
| Kimi Code CLI | 5 updated | 2 | None |
| OpenCode | 10 tracked | 10 | None |
| Pi | 39 touched | 16 | None |
| Qwen Code | 40 active | 50 active | `v0.21.11-nightly.20260815` |
| DeepSeek TUI | 10 tracked | 10 | None |
| Grok Build | 0 | 0 | None |

*"Tracked" = top hot issues listed in each digest; actual totals may be higher. Qwen and Pi counts are explicit in their digests.*

## 3. Shared Feature Directions

| Direction | Tools | Specific community needs |
|---|---|---|
| **Session continuity & persistent memory** | Claude Code, Kimi, Gemini, Codex, Copilot, OpenCode, Pi | Auto-continue after session limits (#13354); durable memory layer across sessions (#1283, #1478); un-archive/restart support (#4502, #4493); resume robustness and exclusive JSONL writers (#8177) |
| **Compaction & token-budget awareness** | Pi, Kimi, Gemini, Codex, Qwen | Trigger compaction on quota, not just max context (#2603); safe turn-boundary compaction (#8153); preserve review verdicts across rebase (#9191); compaction never fires on 1M-token windows (#6879) |
| **Auth/OAuth lifecycle reliability** | Claude Code, Copilot, Gemini, OpenCode, DeepSeek TUI | Refresh-failure loops and corrupted credentials (#54443, #61912); MCP OAuth issuer mismatch (#4480, #4490); single-flight refresh across processes (#37058); false 401 detection (#28827) |
| **Windows/platform stability** | Codex, Claude Code, Copilot, Gemini, Qwen | UI freezes and system-wide mouse stutter (#20214, #38716); host-RAM OOM (#4499); desktop crash-repair loops (#85199); Wayland browser subagents (#21983); Chinese IME failure (#5966) |
| **MCP ecosystem robustness** | Copilot, Claude Code, Gemini, Qwen | Configurable handshake timeout/retry (#4421, ~29% failure rate); stale tool index refresh (#66084); hard 400 beyond ~128 tools (#24246); MCP tool hooks in the hooks engine (#38705) |
| **Permission/safety precision** | Claude Code, Gemini, Copilot, Pi, Qwen, OpenCode | `dontAsk` must respect allowlists (#74567); subagents ignore disabled-agent config (#22093); false-positive filters blocking security research (#72102/72103, #86870); `/spawn` cross-session injection (#4491); `taskkill` killing its own host (#8170); PAT isolation in CI (#9089) |
| **Usage/cost transparency** | Codex, Kimi, Gemini, Copilot | Rate-limit/credit visibility in TUI (#24080); allowance regression without announcement (#2604); prompt-cache preservation (#4500); warning on silent model substitution (#28828) |

## 4. Differentiation Analysis

- **Provider-native CLIs** (Claude Code, Gemini CLI, Qwen Code, Kimi) are tightly coupled to their model ecosystems. Gemini is the most security-forward (SSRF fix, Node 22 sandbox, eval suite expansion); Qwen is the most CI/automation-oriented (`/review` pipeline, SWE-bench/Terminal-Bench release gates); Claude Code has the deepest IDE/TUI integration demand and most mature plugin/skill discussion.
- **OpenAI Codex** stands apart on architecture (Rust core, rollout-based session store, hooks/Guardian permission engine) and is absorbing heavy Windows desktop user pain — its most active issue (104 comments) is a stutter/freeze regression. Rapid alpha cadence indicates fast iteration but also regression churn.
- **Multi-provider agnostic agents** (Pi, OpenCode, DeepSeek TUI) compete on flexibility: Pi on compaction reliability and extension event surfaces; OpenCode on workspace providers (Docker/Incus blueprints) and evolving hosted-service billing; DeepSeek TUI on third-party provider onboarding templates and long-context model tuning. These tools live or die on provider-compatibility correctness (Pi's DeepSeek/xAI fixes, Kimi's `openai_legacy` reasoning-content bug).
- **Copilot CLI** is the GitHub-ecosystem play — its PR work is about workflow safety (fork PR associations, migrating away from `pull_request_target`), and its pain points reflect CI/MCP-registry/Actions integration friction rather than model capability.

## 5. Community Momentum & Maturity

- **Highest velocity:** Qwen Code (40/50 daily), OpenAI Codex (10 PRs + alpha release), Gemini CLI (10 PRs, nightly, security-critical work), OpenCode (10 PRs despite no release).
- **Stabilization-heavy but productive:** Pi (16 PRs / 39 issues, no release) and DeepSeek TUI (10 PRs, v0.9.8 stabilization) are aggressively hardening after feature churn.
- **Deep engagement on fewer threads:** Claude Code shows the strongest per-issue community signal (197 👍 / 78 comments on session continuity) but only 3 PRs — a mature, waiting-for-release profile. Copilot CLI is stable (v1.0.x) but regression-prone (MCP OAuth breaking twice); Kimi's community is small but persistent (memory feature at 40 comments).
- **Maturity read:** Claude Code is the most feature-stable; Codex and Gemini are deliberately shipping pre-release channels; Qwen is the most automated in its own development loop; Copilot's patch-only cadence is not keeping pace with its MCP ecosystem demands.

## 6. Trend Signals

1. **Session continuity is table stakes.** The single strongest demand across Claude Code, Kimi, Gemini, Copilot, OpenCode, and Pi is seamless resume, auto-continue, and persistent memory. Teams evaluating tools for long-running agent workloads should probe recovery/restart behavior first, not model quality.
2. **Compaction economics are broken at 1M-token scale.** With large context windows, compaction never fires — wasting quota and risking API-rejection crashes (Pi #6879, Kimi #2603). Expect tools to move to quota-aware, turn-boundary-safe compaction with explicit extension events.
3. **Auth is the weakest operational link.** OAuth refresh loops, issuer mismatches, and credential corruption appear in nearly every tracker. Any team running concurrent sessions or CI automation should test auth-failure recovery before committing.
4. **Windows desktop is the accident zone.** Codex's stutter/freeze cluster, Claude Desktop repair loops, and Copilot's host-RAM OOM all point to Electron/native-shell instability. CLI-only and headless workflows remain the predictable path in this ecosystem.
5. **Agent correctness and safety are converging.** Silent success misreporting (Gemini #22323), silent model substitution (#28825), cross-session message drops (Claude #86671), and self-inflicted host kills (Pi #8170) are eroding trust. The response is already visible: Gemini's behavioral eval suite and Qwen's SWE-bench/Terminal-Bench CI gates mark eval-driven quality as a differentiator.
6. **MCP adoption is outrunning MCP reliability.** Handshake timeouts (~29% failure, Copilot #4421), cross-version OAuth regressions, stale tool indexes, and the >128-tool ceiling (Gemini #24246) will drive demand for tool-scoping middleware and more robust MCP client lifecycle management.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights Report

**Data source:** github.com/anthropics/skills · **Snapshot:** 2026-08-16

---

## 1. Top Skills Ranking

The most-discussed Pull Requests (all currently open) reveal where community attention is concentrated — notably, three of the top slots are **bug fixes to the `skill-creator` tooling itself** rather than new Skills, indicating that the community's biggest pain point is the reliability of Skill development infrastructure.

| Rank | PR | Skill / Change | Discussion Highlights | Status |
|---|---|---|---|---|
| 1 | [#1298](https://github.com/anthropics/skills/pull/1298) | **fix(skill-creator): `run_eval.py` reports 0% recall** — installs the eval artifact as a real skill; fixes Windows stream reading, trigger detection, and parallel workers | The eval loop is "optimizing against noise": every skill description scores `recall=0%` regardless of content, with 10+ independent reproductions. Breaks the entire description-optimization loop (`run_loop.py`, `improve_description.py`). | Open |
| 2 | [#514](https://github.com/anthropics/skills/pull/514) | **document-typography** — typographic quality control for generated documents: orphan word wrap, widow paragraphs, numbering misalignment | Targets a universal, frequently-felt problem: "These issues affect every document Claude generates." Strong candidate for broad adoption. | Open |
| 3 | [#538](https://github.com/anthropics/skills/pull/538) | **fix(pdf): case-sensitive file references** — 8 mismatches (`REFERENCE.md` → `reference.md`, `FORMS.md` → `forms.md`) | Small but critical fix; breaks PDF skill on case-sensitive filesystems. Complements an existing skill rather than adding a new one. | Open |
| 4 | [#486](https://github.com/anthropics/skills/pull/486) | **ODT skill** — OpenDocument text creation, template filling, ODT→HTML parsing | Covers an ISO-standard format gap (.odt/.ods) with clear trigger definitions; discussion around scope of conversion fidelity. | Open |
| 5 | [#210](https://github.com/anthropics/skills/pull/210) | **Improve frontend-design skill** — clarity and actionability rewrite | Reframes the skill so every instruction is executable "within a single conversation" — a quality bar the community wants applied repo-wide. | Open |
| 6 | [#83](https://github.com/anthropics/skills/pull/83) | **skill-quality-analyzer + skill-security-analyzer** — meta-skills evaluating Skills across structure, documentation, security dimensions | Response to a governance vacuum: the community building its own quality/security review tooling for Skills. | Open |
| 7 | [#541](https://github.com/anthropics/skills/pull/541) | **fix(docx): tracked-change `w:id` collision with existing bookmarks** | Root-cause fix for document corruption — in OOXML, `w:id` is a shared ID space across bookmarks, tracked changes, comments, and move ranges. Preventive guidance for future Skill authors. | Open |
| 8 | [#539](https://github.com/anthropics/skills/pull/539) | **fix(skill-creator): warn on unquoted descriptions with YAML special characters** | Pre-parse frontmatter validation to prevent silent truncation of `description` fields containing `:` — a root cause of Skill trigger failures. | Open |

**Notable cluster:** PRs [#1099](https://github.com/anthropics/skills/pull/1099) and [#1050](https://github.com/anthropics/skills/pull/1050) independently patch the same Windows `run_eval.py` subprocess breakage (`[WinError 10038]`, `claude.cmd` PATHEXT), underscoring how widely the skill-creator tooling failures are being hit.

---

## 2. Community Demand Trends

Distilled from the most-commented Issues (sorted by engagement):

- **Security & trust boundaries are the #1 community concern.** Issue [#492](https://github.com/anthropics/skills/issues/492) (43 comments — the highest in the dataset) warns that community skills under the `anthropic/` namespace impersonate official ones, enabling a trust-boundary vulnerability where users grant elevated permissions unintentionally. Expect demand for provenance/verification tooling.
- **Org-wide skill sharing and lifecycle management.** Issue [#228](https://github.com/anthropics/skills/issues/228) (16 comments, 8 👍) asks for direct organizational sharing instead of manual `.skill` file transfers. Companion issues: [#189](https://github.com/anthropics/skills/issues/189) (duplicate skills from overlapping plugins) and [#62](https://github.com/anthropics/skills/issues/62) (skill disappearances after file renames).
- **Reliable skill-development tooling.** Issues [#556](https://github.com/anthropics/skills/issues/556) (12 comments, 7 👍) and [#1169](https://github.com/anthropics/skills/issues/1169) document the `run_eval.py` 0% trigger-rate bug; [#202](https://github.com/anthropics/skills/issues/202) demands `skill-creator` be rewritten as an operational skill rather than human-oriented documentation. The community is actively **building meta-tools to build Skills better**.
- **Context-window efficiency and safety.** Issue [#1487](https://github.com/anthropics/skills/issues/1487) reports `claude-api` eagerly injecting ~156k tokens in a single tool call; [#1175](https://github.com/anthropics/skills/issues/1175) raises security/context concerns for SharePoint Online handling.
- **New Skill directions proposed:** agent governance & safety patterns ([#412](https://github.com/anthropics/skills/issues/412)), compact symbolic memory for long-running agents ([#1329](https://github.com/anthropics/skills/issues/1329)), and a three-gate reasoning quality pipeline ([#1385](https://github.com/anthropics/skills/issues/1385)).

---

## 3. High-Potential Pending Skills

Active, non-merged PRs that are strong candidates to land soon:

- **[document-typography](https://github.com/anthropics/skills/pull/514)** — typographic quality control for generated documents (orphans, widows, numbering). Universal applicability; fixes a problem in every AI-generated document.
- **[ODT skill](https://github.com/anthropics/skills/pull/486)** — OpenDocument creation, template filling, and ODT→HTML parsing. Fills an ISO-standard format gap alongside existing docx/pdf skills.
- **[testing-patterns](https://github.com/anthropics/skills/pull/723)** — comprehensive testing stack coverage: Testing Trophy model, unit testing (AAA), React Testing Library, and what-not-to-test guidance.
- **[ServiceNow platform skill](https://github.com/anthropics/skills/pull/568)** — broad enterprise coverage: ITSM, ITOM, ITAM/SAM, FSM, SPM, CSDM, IntegrationHub, and security response. Updated as recently as 2026-08-12 (near snapshot date — actively maintained).
- **[pyxel skill](https://github.com/anthropics/skills/pull/525)** — retro/pixel-art game development via the pyxel-mcp server: write → run_and_capture → inspect → iterate workflow.
- **[SAP-RPT-1-OSS predictor](https://github.com/anthropics/skills/pull/181)** — predictive analytics on SAP business data via SAP's open-source tabular foundation model.
- **[self-audit](https://github.com/anthropics/skills/pull/1367)** — mechanical output-file verification followed by a four-dimension reasoning quality gate; universal across tech stacks. Companion to proposal [#1385](https://github.com/anthropics/skills/issues/1385).
- **[plan-file-hygiene](https://github.com/anthropics/skills/pull/1479)** — addresses planning-artifact accumulation with no lifecycle; built directly from community framing in issue [#1417](https://github.com/anthropics/skills/issues/1417).

---

## 4. Skills Ecosystem Insight

The community's most concentrated demand is for **trustworthy, production-grade Skill infrastructure** — fixing and governing the skill-creator eval tooling, namespace security, and distribution lifecycle — before broad investment in new domain Skills will feel safe.

---

# Claude Code Community Digest — 2026-08-16

## Today's Highlights

No new release shipped in the last 24 hours. The most active thread is the long-running request for automatic continuation after session limits are reached ([#13354](https://github.com/anthropics/claude-code/issues/13354)), now at 78 comments and 197 👍. Meanwhile, OAuth refresh failures and safety-filter false positives remain the dominant bug themes, and a new PR ([#86870](https://github.com/anthropics/claude-code/pull/86870)) targets false-positive security status changes during authorized research.

## Releases

No new releases in the last 24 hours.

## Hot Issues

- [#13354 — Feature: Continue when the session limit reached](https://github.com/anthropics/claude-code/issues/13354)  
  The strongest community signal this week. Users want a seamless way to continue work after hitting session limits rather than losing context. 78 comments and 197 👍 make it the most actively discussed issue in the window.

- [#85199 — Claude Desktop repeatedly crashes and requires “Advanced Options → Repair” on Windows](https://github.com/anthropics/claude-code/issues/85199)  
  A Windows stability complaint with 23 comments. Desktop users report recurring crashes that force manual repair, which is a serious trust issue for the packaged app.

- [#54443 — OAuth refresh returns 400 after early 401; concurrent sessions forced to /login](https://github.com/anthropics/claude-code/issues/54443)  
  Auth sessions expire before the local `expiresAt` time, and refresh fails with HTTP 400, forcing repeated `/login`. High-impact for teams running multiple concurrent sessions.

- [#66084 — tools/list_changed doesn't refresh the deferred-tool / ToolSearch index](https://github.com/anthropics/claude-code/issues/66084)  
  MCP tool discovery remains stale in interactive sessions, even on recent versions. This affects any workflow relying on dynamically added MCP tools.

- [#45374 — AskUserQuestion dialog steals focus and captures keystrokes in VS Code](https://github.com/anthropics/claude-code/issues/45374)  
  A UX bug that interrupts users while typing. The dialog hijacks input, causing keystrokes to be interpreted as dialog responses. 7 👍 from affected developers.

- [#61912 — OAuth refresh corrupts credentials during transient upstream 5xx → persistent 401 loop](https://github.com/anthropics/claude-code/issues/61912)  
  A related auth failure: transient Cloudflare 5xx responses during token refresh can corrupt local credential state, causing persistent 401 loops across sessions.

- [#80094 — Claude Desktop macOS: filesystem MCP server unusable in both package generations](https://github.com/anthropics/claude-code/issues/80094)  
  The filesystem MCP server is broken in both packaging variants on macOS: the new schema is never dispatched and the old schema is dropped. This significantly limits desktop MCP usage.

- [#74567 — --permission-mode dontAsk denies Write/Edit regardless of --allowedTools](https://github.com/anthropics/claude-code/issues/74567)  
  Critical for headless agents. `dontAsk` is documented to auto-approve allowlisted tools, but Write/Edit are unconditionally denied, leaving no reliable scoped-write path for automation.

- [#86671 — Cross-session messages displayed but never enqueued; model never sees them](https://github.com/anthropics/claude-code/issues/86671)  
  A fresh Windows/desktop regression: messages sent cross-session appear in the UI but are never delivered to the model, causing silent conversation breaks.

- [#58614 — Path-pattern scanner false-positives on Windows 8.3 short names](https://github.com/anthropics/claude-code/issues/58614)  
  The security scanner flags paths like `ALICEM~1` and forces approval even when an allow-rule matches. This disproportionately affects users with non-ASCII usernames on Windows.

## Key PR Progress

Only 3 PRs were updated in the last 24 hours, so the complete list is below.

- [#84600 — Enable frontend-design plugin at project scope](https://github.com/anthropics/claude-code/pull/84600)  
  Registers the official Anthropic marketplace and enables the `frontend-design` skill via `.claude/settings.json`, making it load automatically for the repo. Closed, status unclear.

- [#82981 — Claude/automatizar inventario insumos w4n98s](https://github.com/anthropics/claude-code/pull/82981)  
  Open PR with no description provided. Likely not a core-engine change.

- [#86870 — fix: prevent false-positive CVP status changes during authorized security research](https://github.com/anthropics/claude-code/pull/86870)  
  Adds context checking before security triggers in `security-guidance/hooks/review_api.py`, including an `is_authorized_lab()` helper to reduce false positives during legitimate security work. Directly addresses the wave of safety-filter false-positive reports.

## Feature Request Trends

- **Session continuity**: The most requested direction is continuing work after session limits, ideally with automatic checkpoint/resume behavior ([#13354](https://github.com/anthropics/claude-code/issues/13354)).
- **RTL / accessibility support**: Users continue to request right-to-left language support in the TUI ([#69992](https://github.com/anthropics/claude-code/issues/69992)).
- **Better headless-agent permissions**: `dontAsk` mode needs reliable scoped-write support so automation can use `--allowedTools` effectively ([#74567](https://github.com/anthropics/claude-code/issues/74567)).
- **Plugin/skill enablement at project scope**: A PR enabling the frontend-design skill via project settings suggests growing demand for declarative plugin setup ([#84600](https://github.com/anthropics/claude-code/pull/84600)).

## Developer Pain Points

- **OAuth and login loops**: Multiple issues describe early 401s, failed refreshes, and corrupted credential state, forcing repeated `/login` ([#54443](https://github.com/anthropics/claude-code/issues/54443), [#61912](https://github.com/anthropics/claude-code/issues/61912)).
- **Safety-filter false positives**: A cluster of reports from security researchers shows legitimate firmware analysis, drone SDK work, and defensive hardening being blocked by AUP/cyber filters ([#72102](https://github.com/anthropics/claude-code/issues/72102), [#72103](https://github.com/anthropics/claude-code/issues/72103)).
- **IDE focus and input bugs**: VS Code users repeatedly hit focus-stealing dialogs, flickering inputs, and scroll constraints ([#45374](https://github.com/anthropics/claude-code/issues/45374), [#71809](https://github.com/anthropics/claude-code/issues/71809), [#57691](https://github.com/anthropics/claude-code/issues/57691)).
- **Desktop stability and MCP breakage**: Windows crashes and macOS filesystem MCP failures are undermining confidence in the desktop package ([#85199](https://github.com/anthropics/claude-code/issues/85199), [#80094](https://github.com/anthropics/claude-code/issues/80094)).
- **Permission and path-scanner false positives**: Users report allow-rules being bypassed by scanner edge cases, especially on Windows short paths and in `dontAsk` mode ([#58614](https://github.com/anthropics/claude-code/issues/58614), [#74567](https://github.com/anthropics/claude-code/issues/74567)).

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — August 16, 2026

## Today's Highlights

The repo published `rust-v0.148.0-alpha.19`, though no detailed changelog was attached. The issue tracker remains dominated by Windows desktop performance regressions: new reports describe idle Electron main-process busy loops, system-wide mouse stutter, and unbounded Crashpad dump growth. PR activity is focusing on operational hardening, with storage diagnostics for `codex doctor`, log-only policy telemetry, gRPC health endpoints, and session/TUI lifecycle fixes.

## Releases

- **[rust-v0.148.0-alpha.19](https://github.com/openai/codex/releases/tag/rust-v0.148.0-alpha.19)** — 0.148.0-alpha.19 release. No additional changelog details were included in the data.

## Hot Issues

1. **[#20214 — Codex App frequently freezes/stutters on Windows 11 Pro despite sufficient system resources](https://github.com/openai/codex/issues/20214)** — The most active issue this cycle with 104 comments and 85 👍. Users report UI freezes even with 32 GB RAM and a Ryzen 5, making it the clearest signal of a persistent Windows desktop performance bug.

2. **[#38546 — ChatGPT/Codex desktop app causes system-wide mouse stutter when running without elevation](https://github.com/openai/codex/issues/38546)** — 25 comments. A new report on build 26.810.41047 links the app to severe cursor stutter only when not elevated, suggesting an interaction between process priority and input handling.

3. **[#38750 — System-wide stutter while Codex is idle; fully exiting app immediately restores OS responsiveness](https://github.com/openai/codex/issues/38750)** — Build 26.810.50856 still shows idle stutter. Users confirm that fully quitting the app restores OS responsiveness, which makes the bug both disruptive and easy to reproduce.

4. **[#38716 — Electron main process busy loop causes system-wide mouse stutter](https://github.com/openai/codex/issues/38716)** — Build 26.810.6296.0 introduced an apparent main-process busy loop. The issue has 7 comments and 3 👍 and tracks closely with other Windows regressions from the 26.810 line.

5. **[#25921 — Codex Desktop continuously generates Crashpad pending dumps, growing without any limit](https://github.com/openai/codex/issues/25921)** — 17 comments, 8 👍. Users measure +5 GB/day in `~/Library/Application Support/com.openai.codex/web/Crashpad/pending`, creating severe macOS disk pressure.

6. **[#38547 — Codex Windows 26.810.4967 idle main-process CPU busy loop in Chrome plugin app-server hashing](https://github.com/openai/codex/issues/38547)** — 16 comments. Although closed, this regression identifies the Chrome plugin app-server hashing path as a source of idle CPU consumption on Windows.

7. **[#35746 — Paginated history drops valid flattened rollout records and reuses ordinals](https://github.com/openai/codex/issues/35746)** — 13 comments. A correctness issue in CLI paginated rollout decoding that can lose session history; source paths were still unfixed through `rust-v0.146.0-alpha.14`.

8. **[#31433 — Codex leaves valid rollout files unindexed in state DB and lacks supported reindex repair](https://github.com/openai/codex/issues/31433)** — 12 comments. On Windows, valid rollout files disappear from the state DB with no supported repair path, making session history unreliable.

9. **[#35470 — Codex copied the image file 150,000 times, consuming 400 GiB of disk space](https://github.com/openai/codex/issues/35470)** — Extreme disk-consumption bug in CLI 0.145.0 with `gpt-5.6-sol` on Windows Terminal. Highlights a runaway file-copy loop inside tool execution.

10. **[#30779 — Subagent fork sessions persist large JSONL histories indefinitely, causing severe ~/.codex disk bloat](https://github.com/openai/codex/issues/30779)** — Subagent sessions are never compacted, leading to unbounded `~/.codex` growth in long-running agent workloads.

## Key PR Progress

1. **[#38806 — Add a health endpoint to the code-mode gRPC listener](https://github.com/openai/codex/pull/38806)** — Adds `GET /healthz` over HTTP/1.1 and HTTP/2 while keeping gRPC methods HTTP/2-only.

2. **[#38795 — Add storage diagnostics to `codex doctor`](https://github.com/openai/codex/pull/38795)** — Reports free space for `CODEX_HOME` and the active worktree, warns below 5 GiB, fails below 1 GiB, and checks Dev Drive trust on Windows.

3. **[#38800 — Route executor policy audits through log-only telemetry](https://github.com/openai/codex/pull/38800)** — Moves forwarded network policy decisions to `codex_otel.log_only`, keeping audit telemetry out of the persistent state log.

4. **[#38788 — Show resume and fork status during TUI startup](https://github.com/openai/codex/pull/38788)** — Displays dimmed `Resuming session…` / `Forking session…` status in the TUI so users know a session action is being resolved.

5. **[#38785 — Keep active-turn model settings stable across updates](https://github.com/openai/codex/pull/38785)** — Captures model configuration for the active turn so thread-setting changes do not alter sampling mid-turn.

6. **[#38774 — Use paginated history for persistent exec threads](https://github.com/openai/codex/pull/38774)** — Enables paginated history for `codex exec` persistent threads, with a legacy fallback when the thread store lacks pagination support.

7. **[#38743 — Scope TUI app directory state to the active context](https://github.com/openai/codex/pull/38743)** — Prevents stale app data from crossing account, workspace, or thread boundaries by invalidating caches and dismissing the app picker on context change.

8. **[#38705 — Add MCP tool handler support to the hooks engine](https://github.com/openai/codex/pull/38705)** — Discovers `mcp_tool` hook handlers, invokes configured MCP servers/tools, expands nested placeholders, and preserves JSON types.

9. **[#38703 — Refresh hook runtimes after plugin changes](https://github.com/openai/codex/pull/38703)** — Rebuilds hook runtimes and refreshes MCP caches when effective plugins change or marketplace upgrades install new content.

10. **[#38701 — Route permission requests through shared Guardian approvals](https://github.com/openai/codex/pull/38701)** — Converts `request_permissions` calls into Guardian permission requests via the common approval path, preserving turn cancellation during automatic reviews.

## Feature Request Trends

- **Richer rate-limit/usage visibility**: Multiple enhancements ask for `resetsAt`, credit balance, plan type, and a full `/status` command via CLI statusline tokens or SDKs. See [#24080](https://github.com/openai/codex/issues/24080), [#15281](https://github.com/openai/codex/issues/15281), [#19555](https://github.com/openai/codex/issues/19555), and [#20310](https://github.com/openai/codex/issues/20310).

- **Workspace/project-scoped chat organization**: Users want Codex chats scoped to VS Code projects/workspaces instead of a global Recent Tasks list ([#3550](https://github.com/openai/codex/issues/3550)).

- **Session storage control and repair**: Several issues request bounds, cleanup, or reindexing tools for rollout/JSONL session storage ([#34337](https://github.com/openai/codex/issues/34337), [#30779](https://github.com/openai/codex/issues/30779), [#31433](https://github.com/openai/codex/issues/31433)).

- **Model/cache cost controls**: Native Bedrock Codex users want explicit GPT-5.6 Sol prompt-cache controls to avoid high cache-write spend ([#37674](https://github.com/openai/codex/issues/37674)).

## Developer Pain Points

- **Windows desktop performance regressions dominate**: Idle CPU busy loops, system-wide mouse/input stutter, elevated DWM GPU usage, and Crashpad dump growth make the desktop app disruptive on Windows and macOS ([#20214](https://github.com/openai/codex/issues/20214), [#38546](https://github.com/openai/codex/issues/38546), [#38716](https://github.com/openai/codex/issues/38716), [#38750](https://github.com/openai/codex/issues/38750), [#25921](https://github.com/openai/codex/issues/25921), [#13749](https://github.com/openai/codex/issues/13749)).

- **Runaway session/disk usage**: Rollout stores, subagent JSONL histories, and file-copy loops can silently consume hundreds of GiB or even TiB-scale disk space ([#34337](https://github.com/openai/codex/issues/34337), [#35470](https://github.com/openai/codex/issues/35470), [#30779](https://github.com/openai/codex/issues/30779)).

- **Session-history integrity issues**: Paginated history dropping records and unindexed rollout files erode trust in CLI/Desktop session continuity ([#35746](https://github.com/openai/codex/issues/35746), [#31433](https://github.com/openai/codex/issues/31433)).

- **Process/resource leak patterns**: MCP suites accumulate per session, Windows process trees miss grandchildren, and macOS `SkyComputerUseService` respawns until OOM crashes ([#34614](https://github.com/openai/codex/issues/34614), [#38769](https://github.com/openai/codex/issues/38769), [#38771](https://github.com/openai/codex/issues/38771)).

- **Insufficient usage/rate-limit feedback**: Developers cannot see remaining credits or reset times in the TUI or SDKs, forcing manual account checks ([#24080](https://github.com/openai/codex/issues/24080), [#15281](https://github.com/openai/codex/issues/15281), [#19555](https://github.com/openai/codex/issues/19555)).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-08-16

## 1. Today's Highlights

The v0.56.0 nightly release shipped a test-infrastructure fix for the a2a-server package, while a fresh batch of SSR Agent PRs from joneba-google targets two long-standing reliability bugs: subagent recovery misreporting `MAX_TURNS` exhaustion as goal success (#22323) and indefinite TUI hangs on bare Linux terminals (#21477). On the security front, a critical SSRF fix for `web-fetch` (CVSS 8.6) and a Node 22 sandbox base-image upgrade are in review, alongside a continued expansion of the behavioral eval suite from ved015.

## 2. Releases

**v0.56.0-nightly.20260815.g2a87e7be1** — Includes the SSR Agent fix migrating `process.env` mutations to `vi.stubEnv()` in a2a-server tests (PR [#28811](https://github.com/google-gemini/gemini-cli/pull/28811)), enforcing Vitest isolation guidelines. No user-facing feature changes in this nightly.

## 3. Hot Issues

1. **[#22323 — Subagent recovery after MAX_TURNS reported as GOAL success](https://github.com/google-gemini/gemini-cli/issues/22323)** (p1, 12 comments, 2 👍) — `codebase_investigator` reports `status: "success"` / `Termination Reason: "GOAL"` even when it hit its turn limit before doing any analysis. A silent correctness failure that erodes trust in agent output; fix PR [#28815](https://github.com/google-gemini/gemini-cli/pull/28815) is already in flight.

2. **[#21409 — Generalist agent hangs](https://github.com/google-gemini/gemini-cli/issues/21409)** (p1, 8 comments, 8 👍) — The highest-reacted issue this cycle. Simple changes like folder creation hang for up to an hour when the model defers to the generalist agent; disabling sub-agent delegation is the only workaround.

3. **[#25166 — Shell command stuck in "Waiting input" after completion](https://github.com/google-gemini/gemini-cli/issues/25166)** (p1, 4 comments, 3 👍) — The TUI displays trivial commands as active and awaiting input after they've already finished, making sessions appear hung and wasting user attention.

4. **[#21968 — Gemini doesn't use skills and sub-agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)** (p2, 6 comments) — Anecdotal but widely felt: custom "gradle"/"git" skills are ignored unless explicitly instructed, despite highly relevant task context. Community is asking for stronger proactive skill selection.

5. **[#26522 — Auto Memory retries low-signal sessions indefinitely](https://github.com/google-gemini/gemini-cli/issues/26522)** (p2, 5 comments) — Sessions the extraction agent deliberately skips are never marked processed, so they keep getting re-surfaced — a loop that wastes model calls and clutters memory.

6. **[#26525 — Add deterministic redaction and reduce Auto Memory logging](https://github.com/google-gemini/gemini-cli/issues/26525)** (p2, 4 comments) — Privacy concern: transcript content reaches model context before any redaction prompt runs, and skill contents can leak into logs. Security-sensitive users will want this prioritized.

7. **[#24246 — 400 error with >128 tools](https://github.com/google-gemini/gemini-cli/issues/24246)** (p2, 3 comments) — Enabling a large tool surface causes hard API failures instead of graceful scoping. Grows more urgent as MCP toolkits proliferate.

8. **[#22093 — Subagents running without permission since v0.33.0](https://github.com/google-gemini/gemini-cli/issues/22093)** (p2, 3 comments) — Regression where subagents (e.g., generalist) activate despite agents being disabled in all configurations. A permission-boundary issue that needs prompt triage.

9. **[#20079 — Symlinked agent files not recognized](https://github.com/google-gemini/gemini-cli/issues/20079)** (p2, 4 comments) — `~/.gemini/agents/filename.md` symlinks are silently ignored, breaking dotfile-managed agent setups.

10. **[#21983 — Browser subagent fails on Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)** (p1, 4 comments, 1 👍) — Browser automation terminates immediately under Wayland display servers, blocking Linux users relying on persistent browser sessions.

## 4. Key PR Progress

1. **[#28815 — Preserve original termination reason during subagent recovery](https://github.com/google-gemini/gemini-cli/pull/28815)** (p1, size/m) — The fix for #22323: when a subagent calls `complete_task` during its final grace turn after hitting `MAX_TURNS`/`TIMEOUT`, the original termination reason is now retained instead of being rewritten as success.

2. **[#28812 — Prevent indefinite TUI hang with execution timeouts](https://github.com/google-gemini/gemini-cli/pull/28812)** (p1, size/s) — Addresses #21477 by bounding `getProcessInfo()`'s `execAsync` calls to `ps`, so bare Linux terminals no longer hang at "Initializing...".

3. **[#28828 — Warn when a preview model is silently substituted](https://github.com/google-gemini/gemini-cli/pull/28828)** (p1, size/m) — Fixes #28825: requesting `gemini-3.1-pro-preview` without entitlement silently rewrites the model to `auto-gemini-2.5` with zero indication. This PR adds an explicit warning. (Related closed PR [#28608](https://github.com/google-gemini/gemini-cli/pull/28608) attempted a stable-model fallback but was superseded.)

4. **[#28725 — Prevent SSRF via DNS resolution bypass in web-fetch](https://github.com/google-gemini/gemini-cli/pull/28725)** (p2, size/m) — Critical security fix for #28555 (CVSS 8.6): blocks custom domains resolving to private/loopback IPs (e.g., `169.254.169.254`), closing the DNS-rebinding hole in the `web-fetch` tool.

5. **[#28726 — Upgrade sandbox Dockerfile to node:22-slim](https://github.com/google-gemini/gemini-cli/pull/28726)** (p1, size/s) — Migrates the sandbox and caretaker CloudRun Dockerfiles off Node 20 (EOL, unpatched CVEs) to Node 22 for continued security coverage.

6. **[#28827 — Avoid false authentication errors for 401 substrings](https://github.com/google-gemini/gemini-cli/pull/28827)** (p2, size/s) — Fixes #28203 by requiring HTTP/status context before treating "401" as an auth failure, eliminating false positives from ports, exit codes, and log lines.

7. **[#28679 — Improve Vertex AI 401 error message with standard API keys](https://github.com/google-gemini/gemini-cli/pull/28679)** (p2, size/s) — Detects the "Gemini API key + vertex-ai auth type" mismatch early and emits actionable GCP credential guidance instead of a generic request failure.

8. **[#28813 — Add composite flag to packages/cli tsconfig](https://github.com/google-gemini/gemini-cli/pull/28813)** (p1, size/xs) — Fixes #21911; unblocks root builds/typechecks where `evals/tsconfig.json` references `../packages/cli` without project-composite support.

9. **[#28814 — Fix TypeScript strict-null errors in integration tests](https://github.com/google-gemini/gemini-cli/pull/28814)** (p2, size/s) — Fixes #21919; resolves strict-null property/union failures in hooks-system integration test builds.

10. **[#28823 — Eval tracker relationships & error recovery](https://github.com/google-gemini/gemini-cli/pull/28823)** (size/xl) — Adds behavioral evals for task-graph dependencies, visualization, file-not-found re-search recovery, and shell failure diagnosis/retry. Companion PRs [#28822](https://github.com/google-gemini/gemini-cli/pull/28822) (todos/complete_task/tracker evals) and [#28824](https://github.com/google-gemini/gemini-cli/pull/28824) (multi-tool chains, large-file context safety, security-boundary enforcement) show a coordinated evals-driven quality push.

## 5. Feature Request Trends

- **AST-aware codebase tooling** ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)) — Precise method-bound reads, AST-guided search, and codebase mapping to cut token noise and misaligned read turns.
- **Expanded behavioral eval infrastructure** ([#24353](https://github.com/google-gemini/gemini-cli/issues/24353)) — The 76-test eval suite should grow into systematic component-level evals across all 6 supported Gemini models.
- **Subagent observability** ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598), [#21763](https://github.com/google-gemini/gemini-cli/issues/21763)) — Subagent trajectories should be visible via `/chat share` and included in `/bug` reports for proper debugging and evaluation.
- **Agent self-awareness & safety guardrails** ([#21432](https://github.com/google-gemini/gemini-cli/issues/21432), [#22672](https://github.com/google-gemini/gemini-cli/issues/22672)) — Accurate knowledge of CLI flags/hotkeys, plus discouraging destructive commands (`git reset`, `--force`) when safer alternatives exist.
- **Zero-dependency OS sandboxing** ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)) — Let Gemini 3's native bash proficiency run freely (grep/awk/sed chains) via sandboxing plus post-execution intent routing.
- **Auto Memory hardening** ([#26516](https://github.com/google-gemini/gemini-cli/issues/26516), [#26522](https://github.com/google-gemini/gemini-cli/issues/26522), [#26523](https://github.com/google-gemini/gemini-cli/issues/26523), [#26525](https://github.com/google-gemini/gemini-cli/issues/26525)) — Deterministic redaction before context ingestion, low-signal session handling, and quarantining invalid inbox patches.
- **Browser agent resilience & configurability** ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232), [#22267](https://github.com/google-gemini/gemini-cli/issues/22267)) — Automatic session takeover/lock recovery and honor `settings.json` overrides like `maxTurns`.

## 6. Developer Pain Points

- **Hang-prone agent execution** — The dominant frustration this cycle: generalist agent hangs ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), TUI stuck at "Initializing..." ([#21477](https://github.com/google-gemini/gemini-cli/issues/21477)), completed shell commands stuck in "Waiting input" ([#25166](https://github.com/google-gemini/gemini-cli/issues/25166)), and deadlock on interactive prompts when scaffolding vite apps ([#22465](https://github.com/google-gemini/gemini-cli/issues/22465)).
- **Silent behavior changes** — Failures reported as success ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)) and preview models quietly swapped for stable aliases ([#28825](https://github.com/google-gemini/gemini-cli/issues/28825)) undermine confidence in agent status reporting.
- **Permission/config regressions** — Subagents executing despite being disabled ([#22093](https://github.com/google-gemini/gemini-cli/issues/22093)) and browser_agent ignoring `settings.json` overrides ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)) indicate configurability isn't reliably enforced at runtime.
- **Workspace hygiene** — The model scatters temp edit scripts across directories when shell execution is restricted ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)), and symlinked agent definitions are silently ignored ([#20079](https://github.com/google-gemini/gemini-cli/issues/20079)), complicating dotfile-managed setups.
- **Scaling ceilings** — Hard 400 errors beyond ~128 tools ([#24246](https://github.com/google-gemini/gemini-cli/issues/24246)) with no smart tool-scoping strategy as MCP ecosystems grow.
- **Terminal UX polish** — Resize flicker ([#21924](https://github.com/google-gemini/gemini-cli/issues/21924)) and screen corruption after exiting external editors in terminalBuffer mode ([#24935](https://github.com/google-gemini/gemini-cli/issues/24935)) remain open quality gaps.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-08-16

## Today’s Highlights

Copilot CLI shipped **v1.0.81-0** with model configuration updates, but the last 24 hours of issue activity are dominated by regressions: Atlassian MCP OAuth failures continue into 1.0.80, NixOS users still can’t use the Bash tool since 1.0.49, and Windows autopilot hit a host-RAM OOM crash. Only two PRs were active, both focused on making invalid-label automation safer around fork PRs and `pull_request_target`.

## Releases

- **v1.0.81-0** — [Release](https://github.com/github/copilot-cli/releases/tag/v1.0.81-0)  
  **Improved:** Update model configurations.

## Hot Issues

1. **Bash tool breaks on NixOS with version >= 1.0.49** — [#3392](https://github.com/github/copilot-cli/issues/3392)  
   Open · 4 comments · 9 👍  
   Any command fails with `Failed to start bash process`. This has been open for months and is still affecting NixOS users.

2. **Atlassian MCP OAuth fails on 1.0.79** — [#4480](https://github.com/github/copilot-cli/issues/4480)  
   Closed · 4 comments · 6 👍  
   RFC 8414 issuer mismatch during OAuth discovery. A notable regression from earlier versions, still attracting user attention.

3. **Atlassian MCP OAuth broken again on 1.0.80** — [#4490](https://github.com/github/copilot-cli/issues/4490)  
   Open · 0 comments  
   Same `Incompatible authorization server` failure reported against 1.0.80, with 1.0.78 working. Suggests the OAuth fix is not stable.

4. **MCP registry policy fetch returns 403 for Actions `GITHUB_TOKEN`** — [#4346](https://github.com/github/copilot-cli/issues/4346)  
   Closed · 2 comments · 3 👍  
   Blocks all non-default MCP servers in CI even with the documented PAT-less Actions setup, hurting automated agent workflows.

5. **MCP initialize handshake has fixed 60s budget and no retry** — [#4421](https://github.com/github/copilot-cli/issues/4421)  
   Open · 1 comment  
   npx-launched stdio servers fail roughly 29% of sessions and are never respawned. A major reliability concern for MCP-heavy users.

6. **`/spawn` can inject context into an unrelated running session** — [#4491](https://github.com/github/copilot-cli/issues/4491)  
   Open · 1 comment  
   The prompt template contradicts its singular-spawn contract and lacks an approval gate for cross-session writes. Potentially destructive for parallel sessions.

7. **`/restart` fails in sessions created with `-w`** — [#4493](https://github.com/github/copilot-cli/issues/4493)  
   Open · 0 comments  
   Restarting a worktree session causes an option conflict between the worktree flag and existing session ID, leaving the session unrecoverable.

8. **Newly enabled model remains unavailable until local cache is cleared** — [#4494](https://github.com/github/copilot-cli/issues/4494)  
   Open · 0 comments  
   The local model catalog does not refresh after enabling a model in GitHub settings, affecting both CLI and VS Code.

9. **Windows OOM: “Committing semi space failed” in autopilot** — [#4499](https://github.com/github/copilot-cli/issues/4499)  
   Open · 0 comments  
   V8 heap was only ~0.6/4.3 GB at crash time, indicating a host-RAM commit failure rather than a JS heap limit. A serious long-running session crash.

10. **BYOK autopilot nudge re-serializes transcript items, breaking prompt caching** — [#4500](https://github.com/github/copilot-cli/issues/4500)  
    Open · 0 comments  
    The completion-nudge turn rebuilds previous items instead of resending them byte-for-byte, breaking prompt caching and increasing cost/latency for BYOK users.

## Key PR Progress

Only 2 PRs were updated in the last 24 hours; both are covered below.

- **Handle fork PR associations in invalid-label writer** — [#4497](https://github.com/github/copilot-cli/pull/4497)  
  Open  
  Updates the invalid-label writer to handle fork PR workflow runs where GitHub does not populate the PR association. It searches trusted workflow-run metadata and requires exactly one open PR, preserving safety.

- **Migrate pull request automation away from `pull_request_target`** — [#4449](https://github.com/github/copilot-cli/pull/4449)  
  Closed  
  Replaces `pull_request_target`-based automation with safer token scoping: issue-scoped write tokens for closing invalid issues, a no-permission `pull_request` signal for mergeable PRs, and privileged operations kept separate.

## Feature Request Trends

- **MCP reliability and authentication**  
  Users want configurable MCP initialization timeouts/retries, correct OAuth issuer discovery, and PAT-less CI registry access.  
  [#4421](https://github.com/github/copilot-cli/issues/4421) · [#4480](https://github.com/github/copilot-cli/issues/4480) · [#4490](https://github.com/github/copilot-cli/issues/4490) · [#4346](https://github.com/github/copilot-cli/issues/4346)

- **Model and context configurability**  
  Requests include exposing `contextTier` through ACP, supporting GPT-5.6 `reasoning.mode`, refreshing the model catalog without manual cache resets, and honoring subagent model overrides.  
  [#4275](https://github.com/github/copilot-cli/issues/4275) · [#4495](https://github.com/github/copilot-cli/issues/4495) · [#4494](https://github.com/github/copilot-cli/issues/4494) · [#3565](https://github.com/github/copilot-cli/issues/3565)

- **Session lifecycle management**  
  Users want the ability to un-archive sessions marked Done, `/restart` support for worktree sessions, and safety guardrails around `/spawn`.  
  [#4502](https://github.com/github/copilot-cli/issues/4502) · [#4493](https://github.com/github/copilot-cli/issues/4493) · [#4491](https://github.com/github/copilot-cli/issues/4491)

- **Observability improvements**  
  Support for standard OTLP protobuf export via `OTEL_EXPORTER_OTLP_PROTOCOL`.  
  [#2934](https://github.com/github/copilot-cli/issues/2934)

- **Skill/agent behavior controls**  
  `disable-model-invocation: true` should make a skill manual-only, not completely unreachable from the CLI.  
  [#4438](https://github.com/github/copilot-cli/issues/4438)

## Developer Pain Points

- **Repeated MCP OAuth regressions**  
  Atlassian MCP OAuth failed in 1.0.79 and again in 1.0.80, despite 1.0.78 working.  
  [#4480](https://github.com/github/copilot-cli/issues/4480) · [#4490](https://github.com/github/copilot-cli/issues/4490)

- **CI friction with MCP and installation**  
  `GITHUB_TOKEN` gets 403s from the MCP registry policy, and Codespaces still ships Copilot CLI 1.0.3 with `copilot update` requiring `sudo`.  
  [#4346](https://github.com/github/copilot-cli/issues/4346) · [#4501](https://github.com/github/copilot-cli/issues/4501)

- **Platform-specific breakage**  
  NixOS users cannot run Bash commands since 1.0.49, and Windows autopilot can OOM from host RAM commit failures rather than V8 heap limits.  
  [#3392](https://github.com/github/copilot-cli/issues/3392) · [#4499](https://github.com/github/copilot-cli/issues/4499)

- **Stale local state and cache issues**  
  Newly enabled models stay unavailable until local Copilot state is manually cleared, and BYOK prompt caching breaks on autopilot nudge turns.  
  [#4494](https://github.com/github/copilot-cli/issues/4494) · [#4500](https://github.com/github/copilot-cli/issues/4500)

- **Session safety and recovery gaps**  
  `/spawn` can silently write into unrelated sessions without approval, while `/restart` is broken for worktree sessions.  
  [#4491](https://github.com/github/copilot-cli/issues/4491) · [#4493](https://github.com/github/copilot-cli/issues/4493)

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-08-16

## Today's Highlights

No new releases landed in the last 24 hours, but the issue tracker shows two clear pressure points: the long-running demand for a persistent memory layer (#1283, #1478) and newer concerns about subscription quota/allowance behavior (#2604, #2603). On the PR side, a correctness fix for `StrReplaceFile` replacement counting is open and worth reviewing.

## Releases

No releases were published in the last 24 hours.

## Hot Issues

Only 5 issues were updated in the last 24 hours. All are listed below.

- [**#1283 — Feature Request: Memory System - Persistent context across sessions**](https://github.com/MoonshotAI/kimi-cli/issues/1283)  
  Long-standing enhancement request for a comprehensive memory system: automatic AI-managed notes plus manual user-defined instructions. With 40 comments, this is the most actively discussed item in the current data. It reflects sustained community demand for persistence across sessions and project context.

- [**#1478 — Can the memory layer be optimized? Nothing memory-related in docs?**](https://github.com/MoonshotAI/kimi-cli/issues/1478)  
  A complementary request in Chinese asking for memory-layer optimization and better documentation. The author notes only `agent.md` exists and cites another tool’s memory file structure as a reference. This reinforces the pain of using Kimi Code CLI on large projects without durable context.

- [**#2604 — Effective weekly allowance appears reduced ~3–5× without announcement**](https://github.com/MoonshotAI/kimi-cli/issues/2604)  
  A recent, data-backed report from a Vivace-tier member using client-side instrumentation. The user recorded raw token volume and observed a large drop in effective allowance, asking whether terms changed or a metering regression was introduced. Only 2 comments so far, but this could be a high-impact transparency/policy issue.

- [**#2603 — Quota-aware compaction: trigger on token budget, not only model max context**](https://github.com/MoonshotAI/kimi-cli/issues/2603)  
  Suggests context compaction should be driven by subscription token budgets as well as the model’s context window. With K3’s 1M-token window, compaction effectively never fires in real sessions, letting agentic workloads consume quota unnecessarily. Fresh issue with no comments yet, but directly relevant to subscription users.

- [**#1155 — openai_legacy provider drops reasoning content, causing APIEmptyResponseError**](https://github.com/MoonshotAI/kimi-cli/issues/1155)  
  Closed issue about the `openai_legacy` provider failing to pass `reasoning_key`, causing all reasoning/thinking content to be dropped when talking to OpenAI-compatible servers like sglang or vllm. Important for users running local/third-party reasoning models. No comments recorded; closure may indicate a fix landed, but no linked PR is present in the data.

## Key PR Progress

Only 2 PRs were updated in the last 24 hours. Both are listed below.

- [**#2524 — fix(tools): count StrReplaceFile replacements against the running content**](https://github.com/MoonshotAI/kimi-cli/pull/2524)  
  Fixes replacement counting in `StrReplaceFile` when edits are chained. Previously the count was computed against the original file content, so edits whose `old` string was produced by an earlier edit were miscounted. A meaningful correctness fix for multi-step file edits; open and pending review.

- [**#2506 — fix(kosong): raise a clear error on circular $ref in deref_json_schema**](https://github.com/MoonshotAI/kimi-cli/pull/2506)  
  Small self-contained fix that makes `deref_json_schema` fail clearly on circular local `$ref` definitions instead of recursing indefinitely. Marked closed; useful for users working with complex JSON Schemas.

## Feature Request Trends

The most requested feature directions across the current issue set are:

- **Persistent Memory System**: Automatic and manual memory for project patterns, user preferences, and cross-session context. (#1283, #1478)
- **Quota/Token-Aware Context Management**: Compaction and context handling should respect subscription token budgets, not just model context limits. (#2603)
- **Metering Transparency**: Users want clear communication or instrumentation around allowance changes. (#2604)
- **Better Provider Compatibility**: Proper handling of reasoning/thinking content from OpenAI-compatible endpoints. (#1155)

## Developer Pain Points

- **Large projects are painful without a memory layer**: Users report no memory docs beyond `agent.md` and significant context loss across sessions. (#1478)
- **Quota usage is unclear and possibly regressed**: One user measured a 3–5× reduction in effective weekly allowance with no announcement. (#2604)
- **Agentic workloads burn quota because compaction never triggers**: With a 1M-token context window, compaction is effectively useless as a quota-saving mechanism. (#2603)
- **External model-server integration remains fragile**: Dropped reasoning content can cause `APIEmptyResponseError` on OpenAI-compatible providers. (#1155)

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-08-16

## Today's Highlights
No new release was published in the last 24 hours. Community discussion centered on billing/subscription sync problems for OpenCode Go, repeated `grok-4.5` failures on hosted endpoints, and a strong batch of stability PRs around V2 eventing, memory leaks, and headless CLI behavior. The most-supported open feature request remains automating Plan Mode → Build Mode switching.

## Releases
No new versions were published in the last 24 hours.

## Hot Issues
- [#37790 OpenCode Go subscription paid successfully but workspace shows "Insufficient balance"](https://github.com/anomalyco/opencode/issues/37790) — 14 comments. A Stripe payment succeeds but the workspace does not reflect the subscription, blocking users from OpenCode Go. Top comment count this cycle signals a frustrating billing-sync bug.
- [#24879 [FEATURE] Go Pro tier ($20) and Share modifier with first-month discounts](https://github.com/anomalyco/opencode/issues/24879) — 11 comments, 11 👍. Users want a predictable paid tier instead of hitting the Go monthly cap and being pushed to pay-as-you-go Zen. Strong interest in shared quota and subscription flexibility.
- [#42143 Why does Opencode require me to subscribe when your official website states it's 100% free?](https://github.com/anomalyco/opencode/issues/42143) — 10 comments. Ongoing confusion between the open-source client and hosted OpenCode Go/Zen services; likely needs clearer product/pricing communication.
- [#7801 [FEATURE] Plan Mode + Question tool can auto switch to Build mode](https://github.com/anomalyco/opencode/issues/7801) — 10 comments, 31 👍. The most-liked open feature request. Users want the agent to automatically transition from Plan to Build when a question tool requires action.
- [#40206 grok-4.5 on opencode go not working since 2 Aug](https://github.com/anomalyco/opencode/issues/40206) — 9 comments. Reports of persistent HTTP 500 errors when calling `grok-4.5` via the OpenCode Go Chat Completions API. Related reports continue in #40886 and #42802.
- [#35649 Links wrapped across lines not clickable in Kitty terminal](https://github.com/anomalyco/opencode/issues/35649) — 5 comments, 2 👍. Terminal UX issue where long URLs wrapped by Kitty lose clickability, likely OSC 8 hyperlink handling.
- [#35295 tui.json "mouse": false causes mouse wheel to fall back to arrow keys, triggering prompt-history navigation](https://github.com/anomalyco/opencode/issues/35295) — 4 comments. Unexpected TUI input behavior when mouse support is disabled; wheel events navigate history instead of scrolling the message viewport.
- [#37671 [2.0] v2 cli: headless commands load OpenTUI and leak native temp files](https://github.com/anomalyco/opencode/issues/37671) — 4 comments, 2 👍. Headless V2 commands such as `--version`, `--help`, and `api` load the OpenTUI native library and leave a 13.1 MiB `libopentui.so` per process.
- [#42739 [Bug] Unhandled crash in `Provider.list` when Cloudflare environment variables exist without `CLOUDFLARE_API_TOKEN`](https://github.com/anomalyco/opencode/issues/42739) — 4 comments. TUI fails to start if partial Cloudflare env configuration is present; a low-effort but high-impact provider-detection crash.
- [#32911 Deepseek API burning too many tokens](https://github.com/anomalyco/opencode/issues/32911) — 3 comments, 1 👍. Users report overbilling via the Deepseek API in newer 1.17.x versions, with community reproduction linked from Reddit.

## Key PR Progress
- [#42831 feat(core): add Docker blueprint workspaces](https://github.com/anomalyco/opencode/pull/42831) — Adds immutable Docker blueprint snapshots for isolated workspaces, SDK Next forking support, and idle container cleanup.
- [#42829 feat(core): add Incus workspace forks](https://github.com/anomalyco/opencode/pull/42829) — Incus-backed container/VM workspace provider with snapshot forking and isolated subagent workspaces.
- [#42830 feat(plugin): select event subscriptions](https://github.com/anomalyco/opencode/pull/42830) — Allows plugins to subscribe to specific event types instead of the wildcard form, with manifest-resolved public event routing.
- [#42826 fix(core): batch streamed session deltas](https://github.com/anomalyco/opencode/pull/42826) — Reduces event spam by batching provider text, reasoning, and tool-input fragments instead of publishing every fragment as a separate public event.
- [#42825 fix(app): release virtualized timeline elements](https://github.com/anomalyco/opencode/pull/42825) — Fixes renderer memory growth by releasing disconnected timeline row elements from TanStack Virtual; one long session retained ~37,500 detached DOM nodes.
- [#42820 fix(app): use tree directory picker everywhere](https://github.com/anomalyco/opencode/pull/42820) — Replaces the legacy flat directory picker with the tree picker across all project-add flows.
- [#37172 fix(tui): sync model favorites](https://github.com/anomalyco/opencode/pull/37172) — Stores model favorites in the managed CLI config and reconciles them across concurrent TUI instances, fixing #37053.
- [#37156 fix(server): SSE event loss under bwrap PID namespace](https://github.com/anomalyco/opencode/pull/37156) — Fixes `opencode serve` SSE streams stalling after the first chunk inside bwrap `--unshare-pid` sandboxes.
- [#37110 fix(opencode): stop repeated empty tool loops](https://github.com/anomalyco/opencode/pull/37110) — Prevents infinite discovery-tool loops after three consecutive empty/no-match outcomes, closing #31942.
- [#37058 fix(xai): cross-process single-flight for OAuth refresh](https://github.com/anomalyco/opencode/pull/37058) — Fixes concurrent OpenCode processes invalidating each other's xAI refresh tokens by serializing OAuth refresh, closing #37059.

## Feature Request Trends
- **Paid plan flexibility**: Users want predictable Go Pro tiers, shared quota modifiers, and first-month discounts to avoid surprise pay-as-you-go fallback (#24879).
- **Smarter mode automation**: Strong demand for automatic Plan Mode → Build Mode switching when the Question tool needs to act (#7801).
- **Workspace/project handling**: Requests to remember moved project paths and improve directory browsing in the web UI (#34737, #42784).
- **Model/provider reliability**: Community pressure around cutting-edge model support, particularly `grok-4.5` on hosted OpenCode endpoints, and model capability accuracy (#40206, #40642).
- **Terminal/TUI ergonomics**: Better link handling, mouse-wheel behavior, and terminal compatibility remain recurring themes (#35649, #35295).

## Developer Pain Points
- **Billing/subscription sync failures**: Paid users blocked by "Insufficient balance" or confused by "100% free" messaging (#37790, #42143).
- **Hosted model endpoint instability**: `grok-4.5` returning HTTP 500/503 on OpenCode Go/Zen while other models work is a repeated complaint (#40206, #40886, #42802).
- **TUI input and link-handling regressions**: Disabling mouse changes wheel behavior; wrapped URLs are not clickable in terminals like Kitty (#35295, #35649).
- **Token overbilling**: Deepseek API token usage appears inflated in newer versions, directly costing users (#32911).
- **Resource leaks in V2**: Headless commands leak native temp libraries, and the renderer retains thousands of detached DOM nodes in long sessions (#37671, #42825).
- **Provider edge-case crashes**: Partial Cloudflare environment configuration can crash the TUI, and Poe provider tools regressed in 1.18.18 (#42739, #42818).

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-08-16

## Today's Highlights
A stabilization-heavy day for Pi: the team landed critical compaction fixes (safe turn boundaries, trailing-assistant-message crash, billable-only token accounting) plus provider corrections for DeepSeek V4 Flash and a default xAI switch to Grok 4.6 via the Responses API. No releases shipped, but 16 PRs and 39 touched issues indicate active hardening. The most community-supported open issue remains auto-compaction never triggering until provider overflow (#6879, 17👍).

## Releases
None in the last 24 hours.

## Hot Issues

1. **#6879 — Auto-compaction never triggers until provider overflow** (OPEN, 21 comments, 17👍) — [link](https://github.com/earendil-works/pi/issues/6879). A 2-hour agentic turn on gpt-5.6-sol pushed context past 100% to 373k tokens; compaction only fired after the API rejected the request. Community wants a check after every agent step, not just at thresholds.
2. **#6187 — Pi login hangs in WSL after GitHub Copilot device authorization** (CLOSED, 27 comments) — [link](https://github.com/earendil-works/pi/issues/6187). Device registers as authorized, but the WSL client never detects completion. High comment count signals WSL + Copilot is a widely used setup.
3. **#8105 — Codex materializes optional tool parameters on gpt-5.6-sol** (CLOSED, 4 comments) — [link](https://github.com/earendil-works/pi/issues/8105). `strict: null` serialization turns optional parameters into required ones, breaking callers that omit properties.
4. **#7855 — "Response was truncated before completion" errors** (CLOSED, 5 comments) — [link](https://github.com/earendil-works/pi/issues/7855). Random truncation with any OpenAI-compatible API (reproduced on local VLLM); the only workaround is manually prompting the model to continue.
5. **#7787 — Bash PI_* guideline triggers unnecessary permission prompts** (OPEN, 3 comments) — [link](https://github.com/earendil-works/pi/issues/7787). The "Inspect PI_* environment variables" guideline makes models run `env` for unrelated tasks, producing spurious permission prompts.
6. **#8028 — TUI fullRender crashes with RangeError past V8 string limit** (OPEN, 2 comments) — [link](https://github.com/earendil-works/pi/issues/8028). A video-production agent reading many images crashes the terminal renderer with "Invalid string length".
7. **#8170 — Windows bash tool can kill its own host via `taskkill /F /IM node.exe`** (CLOSED, 2 comments) — [link](https://github.com/earendil-works/pi/issues/8170). A model-generated command executed without confirmation took down the pi-web host and its Next.js process.
8. **#8168 — Compaction + session restore corrupts tool-result role → 422** (CLOSED, 1 comment) — [link](https://github.com/earendil-works/pi/issues/8168). After auto-compaction during a tool-heavy turn, the next request fails with `ChatMessageRole.TOOL` validation errors.
9. **#8175 — Compaction failures not exposed to extension handlers** (CLOSED, 1 comment) — [link](https://github.com/earendil-works/pi/issues/8175). Extensions routing compaction through `session_before_compact` receive silence on failure; the internal `compaction_end` event never reaches them.
10. **#8157 — Migrate grok-mermaid → lovely-mermaid** (OPEN, 2 comments) — [link](https://github.com/earendil-works/pi/issues/8157). The original was a 1:1 port of grok's renderer with inherited corner cases; lovely-mermaid offers significantly better parsers.

## Key PR Progress

1. **#8153 — fix: compact at safe turn boundaries** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8153). Adds a run-scoped boundary-compaction API, rebuilds live context in-run, preserves the native recent tail, and bounds overflow recovery.
2. **#8164 — fix(agent-session): never continue from trailing assistant message** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8164). Fixes a post-compaction crash where `continue()` was called from a trailing assistant message; retry is now limited to mid-flight errors.
3. **#8165 — fix(coding-agent): tokens.total = billable only** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8165). Excludes cacheRead/cacheWrite (billed at 1/120th input rate) from compaction budgets and status stats; cache is reported separately.
4. **#8124 — feat(ai): route xAI models through Responses, default to Grok 4.6** (OPEN) — [link](https://github.com/earendil-works/pi/pull/8124). Switches xAI from completions to the Responses API, sends a user agent, and bumps the default model from Grok 4.5 to Grok 4.6.
5. **#8148 — fix(coding-agent): scope bash PI_* guideline to session questions** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8148). Fixes #7787 by narrowing the environment-inspection guideline so models stop running `env` on unrelated tasks.
6. **#8149 — fix(ai): omit invalid OpenAI session header** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8149). Drops the `session_id` HTTP header that HTTP/1 proxies reject (`http1.unexpected_underscore`), which surfaced as an Envoy 400 with zero upstream duration.
7. **#8181 — fix(ai): expose low thinking level for DeepSeek V4 Flash on opencode/opencode-go** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8181). Applies `DEEPSEEK_V4_FLASH_THINKING_LEVEL_MAP` to opencode providers, which previously fell back to `low: null`.
8. **#8146 — fix(ai): cap Baseten DeepSeek V4 Flash output at 384k tokens** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8146). models.dev reports a 1M output limit, but Baseten serves 384k max; requests beyond that fail.
9. **#8158 — feat(coding-agent): upgrade Mermaid terminal rendering** (OPEN) — [link](https://github.com/earendil-works/pi/pull/8158). Implements the grok-mermaid → lovely-mermaid migration, closing #8157 and #7832.
10. **#8151 — fix(extensions): contain widget render failures and tear down ctx-owned widgets on invalidation** (CLOSED) — [link](https://github.com/earendil-works/pi/pull/8151). Prevents stale third-party widget closures from surviving `/reload` and crashing the runner.

Also notable: **#8076** (DRAFT dev branch with new harness, OPEN), **#8155** (TUI cursor-blink reset fix, OPEN), **#8172** (example tool-result pruner + spill extension, CLOSED), **#8174** (neutral wording for repeated length stops, CLOSED), and **#7381** (consistent model refresh state, CLOSED).

## Feature Request Trends

- **Thinking-block display control**: fixed-height scrollable blocks, auto-collapse on completion, and eliminating blank spacer lines when blocks are hidden dominate TUI requests (#8171, #8154).
- **Compaction transparency & control**: extensions want visibility into compaction failures (#8175), while users seek tool-result pruning/spilling strategies (#8173) and safer turn-boundary compaction (#8153).
- **Expanded extension event surface**: cancellable `model_select_before` hooks (#8169), events around blocking UI dialogs (#7147), and `ExtensionCommandContext` support for shortcuts (#8180).
- **Provider & model polish**: new built-in providers (LLMTR, #8178), per-model thinking-level fixes (#8182), and migration to a better Mermaid renderer (#8157).
- **Session/resume robustness**: exclusive cross-process writers for resumed JSONL sessions (#8177) and optional file restore in `/tree` navigation (#8152).

## Developer Pain Points

- **Context overflow/compaction reliability** is the top friction point: compaction not triggering (#6879), crashes after trailing assistant messages (#8164), corrupted tool roles after restore (#8168), and misleading "overflow recovery failed" messages (#8176).
- **TUI rendering issues** recur: V8 string-limit crashes (#8028), aggressive cursor flicker while streaming (#8003), and blank lines from hidden thinking blocks (#8154).
- **Model/tool serialization quirks**: optional parameters becoming required on gpt-5.6-sol (#8105) and incorrect output-limit metadata (#8146) break real workflows.
- **Platform safety gaps**: WSL login hangs (#6187) and Windows `taskkill` being able to kill the Pi host itself (#8170) erode confidence in the bash tool.
- **Unexpected model behavior from session environment**: the PI_* guideline causes spurious permission prompts (#7787), wasting user time and tokens.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-08-16

In the last 24 hours, 40 issues and 50 pull requests were active across QwenLM/qwen-code, with the `/review` pipeline dominating both bug reports and fixes. A new nightly release landed with an autofix footprint gate, while multiple SWE-bench Verified and Terminal-Bench 2.0 smoke runs confirmed the release-event benchmark chain is green.

## Releases

**v0.21.11-nightly.20260815.c396fe3d12**  
https://github.com/QwenLM/qwen-code/releases/tag/v0.21.11-nightly.20260815.c396fe3d12

- `feat(autofix)`: deny-by-default footprint gate and positional window censuses — [#9156](https://github.com/QwenLM/qwen-code/pull/9156)
- `fix(web-shell)`: DSW EAS SWE + Terminal-Bench 2.0 smoke runs all published successfully, with Terminal-Bench only dispatched after SWE-bench publication succeeds.

No stable release today; nightly channel remains the active delivery path.

## Hot Issues

1. [#7427](https://github.com/QwenLM/qwen-code/issues/7427) — **web-shell artifact panel spams “Load artifacts failed: Failed to fetch” on automatic refresh**  
   Open, P2 bug, welcome-pr. The artifact list refreshes automatically and repeatedly shows failure toasts. 5 comments; users are looking for silent retry or suppression of expected network failures.

2. [#9250](https://github.com/QwenLM/qwen-code/issues/9250) — **qwen serve host writer hard-codes new-file mode 0600**  
   Open, P3 enhancement, daemon. `write_file`/`edit`/`notebook_edit` ignore umask and provide no configuration. 4 comments; requesters want configurable permission behavior.

3. [#9089](https://github.com/QwenLM/qwen-code/issues/9089) — **autofix: PAT-bearing jobs share a host with untrusted branch code**  
   Open, P1 security, CI/CD. Runner-level isolation is required because autofix PAT steps cannot be fully hardened inside a GitHub Actions step. 4 comments; considered a security blocker.

4. [#9230](https://github.com/QwenLM/qwen-code/issues/9230) — **Follow-up suggestion side query defeats server-side prefix caching**  
   Open, P2 performance. Main session prompt-cache reuse drops to ~0% because follow-up queries reschedule by LRU and re-prefill context. 3 comments; users want `enableCacheSharing` on by default or smarter scheduling.

5. [#9219](https://github.com/QwenLM/qwen-code/issues/9219) — **/review presubmit overlap matching is exact-line only**  
   Open, P2 bug. Multi-line ranges and semantic duplicates bypass overlap detection. 4 comments; misses real duplicate findings from prior review rounds.

6. [#9205](https://github.com/QwenLM/qwen-code/issues/9205) — **Concurrent same-PR reviews race on fixed worktree path**  
   Open, P2 bug. A review worktree at `.qwen/tmp/review-pr-<n>` was deleted five minutes after creation by another session. 3 comments; causes mid-run destruction of review state.

7. [#9209](https://github.com/QwenLM/qwen-code/issues/9209) — **/review: last-gate schema friction**  
   Open, P2, needs-triage. Findings, state fields, locations[] shape, and long-line anchors fail the final validation gates after hours of analysis. 3 comments; forces manual rework at the finish line.

8. [#9198](https://github.com/QwenLM/qwen-code/issues/9198) — **qwen runs into OOM after long session**  
   Open, P2 performance. OOM on a 1 TB memory server after a week-long session; terminal/tmux output then degrades. 3 comments; very frustrated user report with unique terminal corruption.

9. [#9011](https://github.com/QwenLM/qwen-code/issues/9011) — **ask_user_question silently returns “User declined to answer”**  
   Open, P2 bug, welcome-pr. The question is never shown to the user, and no cancel reason is preserved. 3 comments; hurts interactive agent workflows.

10. [#5966](https://github.com/QwenLM/qwen-code/issues/5966) — **0.19.3 UI Chinese IME completely invalid**  
    Open, P2 bug, welcome-pr. Chinese input intermittently stops working with no error and no clear repro. 4 comments; long-standing UI blocker for CJK users.

## Key PR Progress

1. [#9220](https://github.com/QwenLM/qwen-code/pull/9220) — **fix(ci): self-heal failed checkouts on reused review runners**  
   A failed base-branch checkout now repairs the runner workspace instead of killing the job.

2. [#9222](https://github.com/QwenLM/qwen-code/pull/9222) — **fix(review): normalize last-gate inputs and anchor mid-line fragments**  
   Directly addresses #9209 by accepting the shapes the review pipeline itself produces and supporting mid-line location anchors.

3. [#9212](https://github.com/QwenLM/qwen-code/pull/9212) — **fix(review): exempt carried-id re-posts from presubmit overlap drop**  
   Makes the overlap gate id-aware, preserving ledger re-posts of previously confirmed findings.

4. [#9211](https://github.com/QwenLM/qwen-code/pull/9211) — **fix(review): lock the PR review worktree lease against concurrent sessions**  
   The worktree lease now acts as a lock, preventing destructive races and mid-run deletion.

5. [#9191](https://github.com/QwenLM/qwen-code/pull/9191) — **feat(review): transfer per-file content verdicts across rebases**  
   Preserves prior review results across force-push/rebase by storing per-file content pairs instead of a single commit anchor.

6. [#9189](https://github.com/QwenLM/qwen-code/pull/9189) — **feat(autofix): defer verified out-of-footprint findings to a follow-up queue**  
   Adds a fourth “Defer to follow-up” disposition so verified findings outside the PR’s scope are not lost.

7. [#9167](https://github.com/QwenLM/qwen-code/pull/9167) — **feat(dingtalk): support outbound file delivery**  
   DingTalk channel can now validate and upload local files through the media API and send native file messages.

8. [#9122](https://github.com/QwenLM/qwen-code/pull/9122) — **feat(web-shell): improve sidebar session management**  
   Hover preview, folder previews, overflow-aware titles, and clearer running-session indicators.

9. [#9228](https://github.com/QwenLM/qwen-code/pull/9228) — **fix(ci): narrow serve-ab’s self-hosted wipe to the A/B checkout dirs**  
   Fixes a destructive cleanup step that previously deleted the entire shared workspace including the root `.git` history.

10. [#9007](https://github.com/QwenLM/qwen-code/pull/9007) — **fix(serve): bound ACP HTTP pre-attach buffers by bytes**  
   Caps memory usage for ACP pre-attach data, preventing oversized JSON buffers from exhausting the server.

## Feature Request Trends

- **`/review` as a reliable automation primitive**  
  Recurring requests for typed state instead of free text, topology-aware chunk fan-out, id-aware overlap handling, and persistent verdicts across rebase/force-push.

- **Web Shell as a fuller IDE surface**  
  Users want better session management, Git diff sources and branch switching, export via existing transcript UI, and canonical Goal controls.

- **Server/daemon configurability**  
  Requests for file-mode/umask control, buffer limits, cache-sharing toggles, and session rotation options.

- **Channel integration expansion**  
  DingTalk file delivery, audio-bridge transcription, and bounded session lifetimes are active feature directions.

- **CI/autofix hardening**  
  Self-healing checkouts, narrow workspace wipes, runner isolation for PAT-bearing jobs, and deduplication of review-triggering events.

## Developer Pain Points

- **Long `/review` runs still fail at the last gate** — after hours of analysis, schema mismatches and exact-line-only overlap checks force manual rework.
- **Concurrent review sessions race on shared filesystem state** — fixed worktree paths, un-locked leases, and mutation probes in a shared worktree cause lost or corrupted evidence.
- **CI on reused self-hosted runners is fragile** — failed checkouts are terminal, stale workspaces get wiped wholesale, and duplicate `review_requested` events trigger self-cancelling run bursts.
- **Web Shell reliability issues persist** — artifact fetch error spam, broken Chinese IME input, lost manual session names after `/clear`, and clipped dialogs in short terminals.
- **Performance and memory tuning is under-covered** — long-running sessions can OOM even on huge servers, and prefix-cache reuse is defeated by side-query scheduling.
- **Configuration gaps in `qwen serve`** — hard-coded file modes and non-configurable daemon behavior make it hard to integrate into stricter environments.

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI / CodeWhale Community Digest — 2026-08-16

## 1. Today's Highlights

The project is in an active v0.9.8 stabilization window: a series of CI and regression fixes landed to unblock `main`, including cancel-in-progress handling, provider-count assertion updates, macOS credential-test fixes, and terminal-width restoration. Feature work also advanced with prefab third-party provider templates (#5406), a pricing fallback for unverifiable live costs (#5402), and configurable long-context tool-result budgets (#5405). No release was published in the last 24 hours.

## 2. Releases

No new releases in the last 24 hours.

## 3. Hot Issues

- [#4949 — Discussion: The Chinese Translation of "Constitution"](https://github.com/Hmbown/CodeWhale/issues/4949)  
  Closed after 17 comments; the community settled on 宪章 instead of 宪法. PR #5397 now applies that decision to the website.

- [#5316 — EPIC-005: CodeWhale TUI Crate Decomposition (Umbrella)](https://github.com/Hmbown/CodeWhale/issues/5316)  
  Main tracking issue for a large architectural refactor into crates. 7 comments; it coordinates sub-EPICs, features, and PRs for the decomposition effort.

- [#5374 — [bug] The writing its weird (the agent)](https://github.com/Hmbown/CodeWhale/issues/5374)  
  macOS users reported garbled/unreadable agent output. Root cause traced to SSE UTF-8 split across HTTP/2 DATA frames; PR #5404 is the proposed fix.

- [#5350 — [enhancement] Simplify third-party model config with pre-built templates](https://github.com/Hmbown/CodeWhale/issues/5350)  
  Bilingual request for prefab provider templates, embedded docs, and a “test connection” button. Open with 3 comments; PR #5406 implements it.

- [#5367 — [enhancement] Configurable model-visible read/tool-result size limits](https://github.com/Hmbown/CodeWhale/issues/5367)  
  Self-hosted long-context models like DeepSeek V4 hit conservative per-result ceilings, causing extra reads. PR #5405 addresses it.

- [#5322 — [bug] Regression: output area doesn't fill wide terminals](https://github.com/Hmbown/CodeWhale/issues/5322)  
  Closed; v0.9 capped transcript width, but v0.8.65 filled it. Fixed by #5400 restoring full-terminal-width behavior.

- [#5060 — [workflow-runtime] Search re-hardcodes 16-worker ceiling](https://github.com/Hmbown/CodeWhale/issues/5060)  
  Experimental workflow search ignores the Fleet concurrency seam and hardcodes 16 workers; operators also need the resolved limit in run receipts.

- [#5241 — Pricing endpoint returns 503 - all sessions show unverified_live_pricing](https://github.com/Hmbown/CodeWhale/issues/5241)  
  Cost display broke after upgrading to 0.9.3; every session was unpriced. PR #5402 now provides an honest fallback instead of staying in `unverified_live_pricing`.

- [#5392 — agy_credentials tests fail on every macOS run](https://github.com/Hmbown/CodeWhale/issues/5392)  
  `open_secure_regular_file` rejects symlinks at any path component, and macOS temp dirs live under `/var`, a symlink. Fixed by #5396.

- [#5403 — main is red on both platforms across all four completed runs](https://github.com/Hmbown/CodeWhale/issues/5403)  
  After #5395 stopped CI runs from cancelling each other, real failures surfaced: `plugin_e2e_acceptance` on macOS and NSIS provisioning on Windows.

## 4. Key PR Progress

- [#5407 — v0.9.8: finish the assigned cut](https://github.com/Hmbown/CodeWhale/pull/5407)  
  Lands the v0.9.8 finalization work onto `main`, including session-shell geometry. Open.

- [#5406 — feat(tui): prefab provider templates and test-connection](https://github.com/Hmbown/CodeWhale/pull/5406)  
  Implements #5350; adds built-in templates for OpenCode Zen, OpenCode Go, Agnes, and SenseNova so users only need an API key. Open.

- [#5402 — fix(tui): restore session cost when live pricing is unverifiable](https://github.com/Hmbown/CodeWhale/pull/5402)  
  Fixes #5241; session costs no longer stuck at `unverified_live_pricing` when the live pricing endpoint returns 503. Open.

- [#5405 — feat(tui): configurable model-visible read/tool-result budgets](https://github.com/Hmbown/CodeWhale/pull/5405)  
  Implements #5367; makes per-result read/tool budgets configurable for self-hosted long-context models. Open.

- [#5404 — fix(client): fail closed on SSE UTF-8 split across HTTP/2 DATA](https://github.com/Hmbown/CodeWhale/pull/5404)  
  Fixes #5374; prevents garbled streamed text on macOS by handling split multi-byte UTF-8 sequences without lossy fallback. Open.

- [#5401 — fix: CodeQL Highs and prepare GHSA-8hp3 / GHSA-3mgh](https://github.com/Hmbown/CodeWhale/pull/5401)  
  Security-only PR: fixes CodeQL #107 and #88–#106, and prepares two GHSA advisories without tagging a release. Open.

- [#5399 — fix(tui): v0.9.8 stabilization](https://github.com/Hmbown/CodeWhale/pull/5399)  
  Reconstructs v0.9.8 Rust stabilization: turn-owned default subagents, compaction quality, and Blue Stage web fixes. Closed.

- [#5400 — fix(tui): fill transcript to full terminal width](https://github.com/Hmbown/CodeWhale/pull/5400)  
  Closes #5322; restores v0.8.65 behavior so wide terminals and tmux panes no longer waste columns. Closed.

- [#5395 — fix(ci): stop cancel-in-progress from killing concurrent main pushes](https://github.com/Hmbown/CodeWhale/pull/5395)  
  Fixes the CI concurrency-group bug that cancelled in-flight `main` runs and hid failures. Closed.

- [#5394 — fix: unred v0.9.8 provider-count assertions and google ModelRegistry drift](https://github.com/Hmbown/CodeWhale/pull/5394)  
  Re-pins provider-count assertions to the v0.9.8 registry and fixes the related ModelRegistry drift. Closed.

## 5. Feature Request Trends

- **Simplified third-party provider onboarding**: Users want prefab templates, embedded docs, and connection testing for OpenCode Zen, OpenCode Go, Agnes, and SenseNova (#5350).
- **Long-context / self-hosted model tuning**: Community needs configurable read and tool-result budget limits, plus visibility into concurrency ceilings (#5367, #5060).
- **Sandbox flexibility**: More configurable bwrap roots for development toolchains such as Zig (#5410).
- **i18n consolidation**: The Chinese terminology debate was settled, and the web i18n dictionary spine is being completed to remove locale-specific branches (#4949, #5337).
- **Cost and operations transparency**: Users want pricing fallbacks when live verification fails, and operators want resolved limits in run receipts (#5241, #5060).

## 6. Developer Pain Points

- **Red CI across platforms**: macOS symlink handling and PTY keep-alive hangs, plus Windows NSIS provisioning, repeatedly break `main` (#5392, #5403, #5395).
- **Upgrade regressions**: v0.9 introduced terminal-width and pricing-display regressions compared to v0.8.x, eroding trust in upgrades (#5322, #5241).
- **Hardcoded limits hurt self-hosted users**: Fixed worker ceilings and small default result budgets cause extra reads or blocked sandbox operations (#5060, #5367, #5410).
- **Third-party configuration friction**: Manually entering base URLs, model names, and key env vars, then seeing `not checked` or `cache failed`, remains a common onboarding annoyance (#5350).
- **Slow i18n decisions**: Weeks-long discussions over one Chinese term show how much coordination localization changes require across TUI and web (#4949, #5397).

</details>

<details>
<summary><strong>Grok Build</strong> — <a href="https://github.com/xai-org/grok-build">xai-org/grok-build</a></summary>

No activity in the last 24 hours.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*