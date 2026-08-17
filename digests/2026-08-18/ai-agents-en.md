# OpenClaw Ecosystem Digest 2026-08-18

> Issues: 500 | PRs: 500 | Projects covered: 12 | Generated: 2026-08-17 23:16 UTC

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

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-08-18

## 1. Today's Overview

OpenClaw remains very active: 500 issues and 500 PRs were updated in the last 24 hours, with 488 issues and 402 PRs still open, while 12 issues and 98 PRs were closed/merged. No new release was published in this window, so the project appears to be in an inter-release development/stabilization phase. The most concentrated problem areas are session/message reliability, provider/auth resilience, memory/context bloat, and Control UI/UX polish. PR throughput is healthy, especially around durable progress-card UI work and channel fixes, but many high-severity issues remain tagged `clawsweeper:needs-maintainer-review` or `needs-product-decision`, suggesting a growing maintainer-review backlog.

## 2. Releases

None in the last 24 hours (`new releases: 0`). No release notes, breaking changes, or migration notes are available to summarize.

## 3. Project Progress

In the full dataset, **98 PRs** were merged/closed in the last 24 hours. Within the top-30 PR slice, two closed PRs stand out:

- [PR #125440](https://github.com/openclaw/openclaw/pull/125440) — `test(browser): use per-run temp dirs for shared /tmp profile fixtures`  
  Closes out a test-infrastructure flake by removing shared fixed `/tmp` browser profile paths.

- [PR #125117](https://github.com/openclaw/openclaw/pull/125117) — `fix(plugins): report malformed manifests during metadata discovery`  
  Improves plugin metadata discovery so unreadable/malformed manifests are surfaced instead of silently dropped.

Active PRs show several feature areas moving toward merge:

- **Durable progress-card migration:** [PR #125444](https://github.com/openclaw/openclaw/pull/125444) (Android), [PR #125442](https://github.com/openclaw/openclaw/pull/125442) (iOS/macOS), and [PR #125438](https://github.com/openclaw/openclaw/pull/125438) (dashboard tile) all extend the new progress-card surface.
- **Multi-bot Microsoft Teams support:** [PR #112811](https://github.com/openclaw/openclaw/pull/112811) adds support for multiple Teams bot accounts on one gateway.
- **Control UI and onboarding:** [PR #124687](https://github.com/openclaw/openclaw/pull/124687) stops onboarding from printing reusable gateway token URLs; [PR #123535](https://github.com/openclaw/openclaw/pull/123535) reduces session-catalog refresh storms; [PR #120319](https://github.com/openclaw/openclaw/pull/120319) lets operators forget stale stored browser credentials.
- **Provider/channel correctness:** [PR #125443](https://github.com/openclaw/openclaw/pull/125443) fixes Discord realtime voice API key references; [PR #125424](https://github.com/openclaw/openclaw/pull/125424) hides OpenClaw-created sessions from the native Codex list; [PR #125383](https://github.com/openclaw/openclaw/pull/125383) adds llama.cpp embedding-only managed-server support.
- **Reliability/process fixes:** [PR #125429](https://github.com/openclaw/openclaw/pull/125429) fixes Workboard lifecycle sync errors with multiple agents; [PR #75299](https://github.com/openclaw/openclaw/pull/75299) adds a starvation guard to the priority command queue; [PR #80396](https://github.com/openclaw/openclaw/pull/80396) warns when `MEDIA:` tokens are skipped inside fenced code blocks.

## 4. Community Hot Topics

The most active issues by comment count in the last 24 hours:

- [Issue #77598](https://github.com/openclaw/openclaw/issues/77598) — *Track live dev agent behavior and trajectory* (23 comments)  
  Maintainer-facing meta-issue for a 24-hour observational watch of a dev agent. Signals strong maintainer interest in agent-behavior telemetry.

- [Issue #91009](https://github.com/openclaw/openclaw/issues/91009) — *Codex PreToolUse native hook relay spawns CPU-bound `openclaw-hooks` processes and stalls gateway RPC* (20 comments, 2 👍)  
  High-impact reliability issue affecting Codex integrations, with CPU saturation and session/message-loss risk.

- [Issue #68596](https://github.com/openclaw/openclaw/issues/68596) — *Feature Request: Configurable streaming watchdog timeout threshold* (15 comments, 8 👍)  
  Users with long-reasoning models (DeepSeek-R1, Kimi K2.5) are hitting false streaming-watchdog warnings.

- [Issue #62505](https://github.com/openclaw/openclaw/issues/62505) — *[Bug]: Coding Agent never completes anything (worked in 2026.4.2 and earlier)* (15 comments)  
  One of the more serious regressions reported, with a clear “worked before, now fails” signal.

- [Issue #96834](https://github.com/openclaw/openclaw/issues/96834) — *WhatsApp 1:1: inbound image wedges main lane ~3min before processing* (15 comments)  
  Multimodal WhatsApp image handling is stranding active runs and blocking the message lane.

- [Issue #42840](https://github.com/openclaw/openclaw/issues/42840) — *Feature Request: Add MathJax/LaTeX Support to Control UI* (8 comments, 10 👍)  
  The highest-reacted issue in the top slice; scientific/mathematical users want formula rendering in the UI.

Underlying community demand is consistent: users need fewer silent failures, more explicit timeouts/configurability, better channel-specific message delivery, and a more polished, human-readable Control UI.

## 5. Bugs & Stability

Ranked by severity label. “No fix PR indicated” means the issue still carries `clawsweeper:no-new-fix-pr` or no linked PR was visible in the supplied data.

### P0 / Release-blocking risk

- [Issue #70903](https://github.com/openclaw/openclaw/issues/70903) — **Persistent file-based provider cooldown blocks user for hours after billing recovery**  
  A 402/billing cooldown persists across restarts, so users remain blocked even after topping up. No fix PR indicated.

### P1 / High severity

- [Issue #91009](https://github.com/openclaw/openclaw/issues/91009) — Codex PreToolUse hook relay spawns CPU-bound processes and stalls gateway RPC. No fix PR indicated.
- [Issue #62505](https://github.com/openclaw/openclaw/issues/62505) — Coding agent stops completing anything; regression from 2026.4.2. No fix PR indicated.
- [Issue #96834](https://github.com/openclaw/openclaw/issues/96834) — WhatsApp inbound images wedge the main lane for ~3 minutes and strand runs. No fix PR indicated.
- [Issue #38327](https://github.com/openclaw/openclaw/issues/38327) — `Cannot convert undefined or null to object` with google-vertex/gemini-3.1-pro-preview. No fix PR indicated.
- [Issue #74586](https://github.com/openclaw/openclaw/issues/74586) — `active-memory` embedded runs abort `memory_search` calls and misclassify timeouts. No fix PR indicated.
- [Issue #67777](https://github.com/openclaw/openclaw/issues/67777) — Subagent completion delivery can be lost on timeout, drain, or orphan prune. No fix PR indicated.
- [Issue #53408](https://github.com/openclaw/openclaw/issues/53408) — `write`/`exec` tool parameters silently dropped after long conversations. No fix PR indicated.
- [Issue #53540](https://github.com/openclaw/openclaw/issues/53540) — “Network connection lost” when large tool-call parameters cause generation latency. No fix PR indicated.
- [Issue #86215](https://github.com/openclaw/openclaw/issues/86215) — Codex OAuth refresh failures can wedge an agent for hours. No fix PR indicated.
- [Issue #45224](https://github.com/openclaw/openclaw/issues/45224) — Unhandled Playwright CDP assertion can crash the whole Gateway. Not reproducible on `main`.
- [Issue #71689](https://github.com/openclaw/openclaw/issues/71689) — Task registry restore fails on malformed SQLite image. No fix PR indicated.
- [Issue #78493](https://github.com/openclaw/openclaw/issues/78493) — `sudo openclaw update` creates mixed ownership; later `doctor` can overwrite config after EACCES. No fix PR indicated.
- [Issue #97616](https://github.com/openclaw/openclaw/issues/97616) — Unreaped hook/tool child processes accumulate as zombies and degrade runtime. No fix PR indicated.

### P1/P2 regressions with possible fixes in flight

- [Issue #77930](https://github.com/openclaw/openclaw/issues/77930) — Discord channel not loaded in 2026.5.4; regression matrix supplied. Linked PR is open.
- [Issue #62328](https://github.com/openclaw/openclaw/issues/62328) — `node:sqlite` missing FTS5 breaks memory keyword search fallback. Linked PR is open.
- [Issue #112196](https://github.com/openclaw/openclaw/issues/112196) — `memory_search` transient sync timeout is masked as a persistent provider failure. Linked PR is open.
- [Issue #51429](https://github.com/openclaw/openclaw/issues/51429) — Hardcoded workspace path `/Users/wangtao` released into the wild. No fix PR indicated.

## 6. Feature Requests & Roadmap Signals

The most notable user-requested features in the current pipeline:

- [Issue #68596](https://github.com/openclaw/openclaw/issues/68596) — Configurable streaming watchdog timeout threshold (8 👍)
- [Issue #42840](https://github.com/openclaw/openclaw/issues/42840) — MathJax/LaTeX support in Control UI (10 👍)
- [Issue #67413](https://github.com/openclaw/openclaw/issues/67413) — Per-agent dreaming configuration to avoid OOM spikes
- [Issue #60572](https://github.com/openclaw/openclaw/issues/60572) — Multi-slot memory architecture (linked PR open)
- [Issue #56781](https://github.com/openclaw/openclaw/issues/56781) — Fallback model chain for compaction and LCM `summaryModel`
- [Issue #50093](https://github.com/openclaw/openclaw/issues/50093) — WhatsApp backfill of missed messages after reconnection
- [Issue #71058](https://github.com/openclaw/openclaw/issues/71058) — Multiple Azure/Teams bots per gateway (PR #112811 is actively open)
- [Issue #67419](https://github.com/openclaw/openclaw/issues/67419) — Stop re-injecting bootstrap files every turn, wasting 20–30% of tokens
- [Issue #52640](https://github.com/openclaw/openclaw/issues/52640) — Persistent task-status surface for long-running channel turns
- [Issue #74704](https://github.com/openclaw/openclaw/issues/74704) — Stabilize the SDK happy path for external app clients

**Likely next-version signals:** The durable progress-card work is already in active PRs across Android, iOS/macOS, and the dashboard, so that may ship soon. Multi-bot Teams support also looks close via [PR #112811](https://github.com/openclaw/openclaw/pull/112811). Given repeated timeouts and provider-failure complaints, configurable watchdog thresholds and fallback model chains are strong candidates for the next release round.

## 7. User Feedback Summary

Users are clearly relying on OpenClaw for serious day-to-day work: coding agents, multi-agent gateways, WhatsApp/Telegram/Discord/MSTeams automation, and even family/business assistant use. [Issue #73537](https://github.com/openclaw/openclaw/issues/73537) includes direct positive feedback: *“It has genuinely become part of our daily workflow.”*

The main dissatisfaction clusters are:

- **Regressions erode trust:** “Coding Agent never completes anything” (#62505), Discord channel regression (#77930), and provider regressions like Gemini/Vertex (#38327) are painful because they break previously working setups.
- **Silent failures frustrate users:** memory_search timeouts masked as provider failure (#112196), silently dropped tool parameters (#53408), and silent model-switch failures (#58957) are especially damaging because users cannot tell what went wrong.
- **Performance/overhead concerns:** CPU-bound hook processes (#91009), zombie process accumulation (#97616), and session context bloat (#67419) are causing long-running gateways to degrade.
- **Trust/QA concerns:** The hardcoded `/Users/wangtao` path (#51429) produced strongly negative reactions and highlights release-process risk.
- **UI/UX and i18n needs:** Dense/unreadable UI (#75947), missing LaTeX rendering (#42840), English-only slash-command descriptions (#79458), and RTL bidi issues (#68105) show a broad desire for more polished and internationalized surfaces.

Overall satisfaction appears tied to stability: users praise the product when it “just works,” but regressions and silent message/state loss generate the strongest negative feedback.

## 8. Backlog Watch

Several important issues have been open for months, still carry `no-new-fix-pr`, and need maintainer/product decisions:

- [Issue #38327](https://github.com/openclaw/openclaw/issues/38327) — P1 Gemini/Vertex regression, open since March, no fix PR indicated.
- [Issue #50093](https://github.com/openclaw/openclaw/issues/50093) — P1 WhatsApp backfill after reconnection, open since March, no fix PR indicated.
- [Issue #53408](https://github.com/openclaw/openclaw/issues/53408) — P1 `write`/`exec` parameters silently dropped, open since March, no fix PR indicated.
- [Issue #62505](https://github.com/openclaw/openclaw/issues/62505) — P1 Coding Agent never completes, open since April, no fix PR indicated.
- [Issue #67777](https://github.com/openclaw/openclaw/issues/67777) — P1 Subagent completion delivery can be lost, open since April, no fix PR indicated.
- [Issue #69208](https://github.com/openclaw/openclaw/issues/69208) — P1 Umbrella issue for duplicate transcript/replay/context bugs, open since April, no fix PR indicated.
- [Issue #70903](https://github.com/openclaw/openclaw/issues/70903) — P0 persistent provider cooldown after billing recovery, open since April, no fix PR indicated.

PRs that appear to need maintainer attention or updated proof:

- [PR #111313](https://github.com/openclaw/openclaw/pull/111313) — P1 MCP standalone apps hang on stalled gateway requests; open since July, still `needs proof`.
- [PR #112811](https://github.com/openclaw/openclaw/pull/112811) — Multi-Teams-bot support; open since July, still `needs proof`.
- [PR #123535](https://github.com/openclaw/openclaw/pull/123535) — Control UI session catalog refresh storms; `ready for maintainer look`.
- [PR #123975](https://github.com/openclaw/openclaw/pull/123975) — Typecheck hangs forever when tsgo wedges; `ready for maintainer look`.
- [PR #124687](https://github.com/openclaw/openclaw/pull/124687) — Stop printing gateway token URLs during onboarding; P1, `ready for maintainer look`.

The project is shipping meaningful improvements, but the volume of P1/P0 issues with no fix PR and the maintainer-review bottleneck are the main health risks to watch next.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report: AI Agent & Personal Assistant Open-Source Ecosystem
**Date:** 2026-08-18 · **Basis:** 24-hour community digest data across 12 projects

---

## 1. Ecosystem Overview

The personal AI assistant / agent open-source landscape is in a **post-hype reliability phase**: feature velocity remains high (12 projects moved ~230 PRs to merged/closed in 24 hours), but the dominant user complaints have shifted from "what can agents do" to **"why do agents silently fail, lose state, and waste tokens."** Projects are converging on a shared infrastructure stack — multi-channel gateways (WhatsApp, Telegram, Discord, Slack, Teams, WeChat/Feishu/DingTalk), provider abstraction with fallback, persistent memory, and WebUI/desktop consoles — while differentiating on portability (cloud vs. local), security posture, and target-user sophistication. Two distinct ecosystems are visible: the global/English-language cluster (OpenClaw, NanoBot, Hermes, ZeroClaw, IronClaw) prioritizing interop and security, and the China-focused cluster (CoPaw, LobsterAI) prioritizing enterprise IM platform integration and multi-agent collaboration. The clearest industry-wide signal is that **OpenAI-protocol compatibility and Codex/A CP integrations are becoming table stakes**, not differentiators.

---

## 2. Activity Comparison

| Project | Issues Updated (24h) | PRs Updated (24h) | PRs Merged/Closed | Releases | Health Score* |
|---|---|---|---|---|---|
| **OpenClaw** | 500 | 500 | 98 | None | **B+** — Massive throughput, but P0/P1 backlog (13 high-severity no-fix items) and maintainer review bottleneck |
| **ZeroClaw** | 50 | 50 | 16 | None | **A-** — RFC-governed, security-focused, fast merges; Windows support remains P1 gap |
| **Hermes Agent** | 50 | 50 | 12 | **v0.20.3** (patch) | **B+** — Steady shipping, but silent-failure P1s (cron alerting, clarify rendering) open for weeks |
| **IronClaw** | 28 | 44 | 16 | **v1.3.0-rc.1** | **A-** — Closes performance epics same-week; libSQL starvation fix in flight |
| **CoPaw (QwenPaw)** | 14 | 35 | 22 | None | **B+** — High merge velocity, active first-time contributors; session-identity collision bug open |
| **NanoClaw** | 4 | 39 | 23 | None | **A-** — Core-team PRs landing fast; two regressions have open fix PRs |
| **LobsterAI** | 7 | 21 | 17 | None | **B** — Strong PR momentum, but 4 stale April bugs (Ollama, MCP) untouched |
| **NanoBot** | 3 | 15 | 5 | None | **A-** — Small, responsive, fixes merged within days; one gateway serialization bug drags |
| **Moltis** | 2 | 9 | 6 | None | **B+** — Healthy; long review cycles (~2 months) are the main friction |
| **PicoClaw** | 4 | 4 | 3 | None | **B** — Small but responsive; Slack fix awaiting review |
| **NullClaw** | 0 | 1 | 0 | None | **C** — Effectively idle; 2-month-old Dependabot PR unmerged |
| **ZeptoClaw** | 0 | 0 | 0 | None | **C** — No activity |

*\*Qualitative composite of throughput, bug-response latency, and backlog risk.*

---

## 3. OpenClaw's Position

OpenClaw remains the **reference implementation and community center of gravity** of the ecosystem — its 24-hour activity (500 issues + 500 PRs updated) is **10× the next busiest project** and its open backlog (488 issues, 402 PRs) exceeds the *total* issue+PR footprint of most peers. Its advantages:

- **Channel breadth:** No other project matches its coverage — WhatsApp, Telegram, Discord, Microsoft Teams (multi-bot), browser automation, Codex, and an active Control UI. Competitors typically own 2–4 channels.
- **Ecosystem gravity:** PRs like the durable progress-card migration (Android/iOS/macOS/dashboard) and plugin manifest discovery show platform-level investment, not point fixes.
- **Technical approach:** Gateway-centric with a pluggable provider layer, managed embedding servers (llama.cpp), and a starved-queue guard. This is the most "distributed-systems" architecture in the cohort.

The weaknesses are equally visible: **maintainer review is the bottleneck** (many `clawsweeper:needs-maintainer-review` tags), a P0 billing-cooldown bug persists, and high-severity regressions (coding-agent completion, WhatsApp image wedge, Codex hook CPU saturation) have no linked fixes. Smaller projects (NanoBot, PicoClaw) are demonstrably faster at shipping fixes for similar bug classes — a responsiveness gap OpenClaw's scale has created.

---

## 4. Shared Technical Focus Areas

These requirements emerged independently across multiple projects in the same 24-hour window:

| Focus Area | Projects | Specific Need |
|---|---|---|
| **Anti-silent-failure mechanics** | OpenClaw, NanoBot, Hermes, PicoClaw, NanoClaw | Early termination on repeated tool failure (PicoClaw #3311), visible cron/alert failures (Hermes #88655), no dropped tool params (OpenClaw #53408), no silent tool-call loops (NanoBot #4864) |
| **Provider/auth resilience** | OpenClaw, NanoBot, ZeroClaw, IronClaw, CoPaw | Persistent cooldowns cleared on billing recovery (OpenClaw P0), provider fallback applied to all exceptions (NanoBot #5413), keys out of URLs (ZeroClaw #9973), normalized failure diagnostics (IronClaw #7692) |
| **Token/cost governance** | OpenClaw, NanoBot, Hermes, ZeroClaw | Configurable streaming watchdog timeouts (OpenClaw #68596), spend firewalls (NanoBot #5409), stop bootstrap re-injection wasting 20–30% tokens (OpenClaw #67419), disabled cron jobs must not fire (NanoBot #5407) |
| **Channel/media correctness** | OpenClaw, PicoClaw, CoPaw, ZeroClaw, NanoClaw | WhatsApp inbound image wedging (OpenClaw #96834), Slack `FileSize=0` upload failures (PicoClaw #3340), short-lived QQ URL expiry poisoning history (CoPaw #7088), bounded QQ/Mattermost downloads (ZeroClaw #10000) |
| **Memory/context integrity** | OpenClaw, Hermes, IronClaw, Moltis | Multi-slot memory (OpenClaw #60572), FTS loss on interrupted optimize (Hermes #72716), persistent memory recall verification (IronClaw #7275), heartbeat config not clobbered (Moltis #1209) |
| **Config persistence / no silent overwrites** | LobsterAI, Moltis, OpenClaw | `groupPolicy` silently reverted to allowlist (LobsterAI #1653), `heartbeat.update` treating partial input as full config (Moltis #1209), persistence across restarts |
| **Multi-agent orchestration** | CoPaw, LobsterAI, Hermes, OpenClaw | Single-window agent collaboration (CoPaw #6925), main-agent orchestration of existing agents (LobsterAI #1644), bot-to-bot DMs (Hermes `hermes peer`), multi-bot Teams gateway (OpenClaw #112811) |
| **WebUI/desktop UX polish** | OpenClaw, NanoBot, CoPaw, IronClaw | LaTeX/MathJax rendering (OpenClaw #42840), side conversations + follow-up suggestions (NanoBot #5364/#5408), per-channel model config (CoPaw #7085), durable notification inbox (IronClaw #7687–7691) |

---

## 5. Differentiation Analysis

| Project | Target User | Feature Center | Architectural Signature |
|---|---|---|---|
| **OpenClaw** | Power users, devs, multi-channel automation operators | Breadth: every channel, every provider, Codex/browser | Gateway core + Control UI; plugin/PawApp-like extension; heavyweight but comprehensive |
| **Hermes Agent** | Desktop-first users, bot-mode social operators | Desktop profile lifecycle, bot chat UX, skills, webhooks | Desktop client + gateway split; aggressive security hardening of child processes |
| **ZeroClaw** | Security-conscious operators, OpenAI-ecosystem tooling | Chat Completions compatibility, shell-command policy, auth pipelines | RFC-governed, zero-trust defaults, security-first merge discipline |
| **IronClaw** | Automation/ops-heavy teams | Durable DB write reduction, resource governor, notification inbox, WASM tools | Performance/systems-engineering focus; libSQL, lease fencing, capability normalization |
| **CoPaw / LobsterAI** | Chinese enterprise users (Feishu, DingTalk, WeChat/QQ, Weixin) | Enterprise IM parity, PawApp plugins, multi-agent workflows | Very high merge velocity; largest gap is stale cross-platform/local-model bugs |
| **NanoBot** | Lightweight tinkerers, WebUI-first | Side conversations, follow-up suggestions, native TS terminal UI | Fast, small, Telegram-first; excellent fix latency |
| **NanoClaw** | Channel/container extension developers | Channel-layer seams, session runtime driver, local web chat | Infrastructure abstraction ("one-door" delivery, Docker runtime seam) |
| **Moltis** | Rust ecosystem developers | Browser tooling (shadow DOM), external ACP agents, managed Files library | Rust-native, long review cycles, dependency-clean |
| **PicoClaw** | Lightweight multi-channel users | IRC, Weixin, Slack, env-only deployments | Small footprint, community-patched; effectively 1–2 maintainers |
| **NullClaw / ZeptoClaw** | — | — | Inactive / maintenance. Watch only |

---

## 6. Community Momentum & Maturity

**Tier 1 — Rapid iteration, high throughput (production-scale):**
OpenClaw (98 merges/24h), NanoClaw (23), CoPaw (22), LobsterAI (17), ZeroClaw (16), IronClaw (16). These projects are shipping daily but show **scale-related strain**: OpenClaw's maintainer review backlog, ZeroClaw's Windows P1, CoPaw's critical session-identity bug. IronClaw and ZeroClaw are the most *disciplined* — epics closed same-week, security fixes merged ahead of features.

**Tier 2 — Steady, responsive (solid mid-size):**
Hermes (patch release, 12 merges), NanoBot (5 merges, near-zero backlog), Moltis (6 merges), PicoClaw (3 merges). NanoBot is the model of **proportional responsiveness**: small surface area, most issues closed within days. Hermes has broader ambitions than its review capacity currently sustains.

**Tier 3 — Stabilizing / idle:**
NullClaw and ZeptoClaw show no development velocity. For ecosystem watchers, their main value is Docker distribution and niche channels (ZeptoClaw), not roadmap signal.

**Maturity pattern:** The ecosystem is bifurcating into **"platform" projects** (OpenClaw, ZeroClaw, Hermes — broad, integrated, slower to fix) and **"utility" projects** (NanoBot, PicoClaw, Moltis — narrow, fast, reliable). Platform projects win on capability; utility projects win on trust.

---

## 7. Trend Signals

For AI agent developers, the strongest cross-project signals from this 24-hour window:

1. **Silent failure is the #1 trust killer.** Every project with user feedback reports the same pattern: users tolerate bugs they can see, but abandon agents that lose messages, drop tool parameters, or swallow errors. Expect **observability and explicit failure surfacing** to become a core feature category (diagnostics envelopes, stall watchdogs, failure nudges).

2. **Token economics are now a user-facing feature.** Requests for spend firewalls (NanoBot), configurable timeouts (OpenClaw), and complaints about bootstrap token waste (OpenClaw), disabled-cron token burn (NanoBot), and repeated clarification loops (NanoBot) indicate users are actively measuring per-turn cost. **Cost governance will differentiate agents** aimed at heavy/production use.

3. **OpenAI-protocol / Codex compatibility is the new interoperability floor.** ZeroClaw's Chat Completions RFC, OpenClaw's Codex integration (and its failures), NanoClaw's codex provider, and Moltis's ACP agents all point to a single conclusion: agents must speak the OpenAI protocol dialects to plug into the existing tool ecosystem (Open WebUI, LobeChat, Continue.dev, Aider, LangChain).

4. **Multi-agent orchestration is moving from prototype to product.** Bot-to-bot DMs (Hermes), single-window collaboration (CoPaw), workflow orchestration of existing agents (LobsterAI), and multi-bot gateways (OpenClaw Teams) are all active. The open question is **session identity** — every project touching multi-agent or multi-session workflows has an identity-isolation bug (OpenClaw, CoPaw, Hermes, Moltis).

5. **Security hardening is now a permanent workstream, not an incident response.** Credential scrubbing (Hermes), API keys out of URLs (ZeroClaw), redirect-chain validation (NanoBot), bounded downloads (ZeroClaw), and implicit-file-read elimination (ZeroClaw) are being merged proactively. Zero-trust defaults (empty allowlist = deny all) are the emerging standard.

6. **Local-first and cross-platform are converging.** Ollama support (LobsterAI, OpenClaw llama.cpp), local web chat (NanoClaw — two competing PRs in one day), and Windows/macOS CI investments (ZeroClaw, NanoBot) show that **the next user cohort is on Windows and wants private, non-external runtimes.**

7. **Memory is still unsolved but actively invested:** persistent recall verification (IronClaw), multi-slot architectures (OpenClaw), FTS failure modes (Hermes), and memory-backend plugins (CoPaw) — expect memory architecture to be a headline feature battleground in 2026 H2.

---

**Bottom line for decision-makers:** Choose OpenClaw for breadth and ecosystem leverage (budget for its instability and review lag); choose NanoBot/PicoClaw-class projects when fix-latency and reliability matter more than surface area; watch ZeroClaw and IronClaw as the security/performance reference points; and treat multi-agent orchestration, token governance, and OpenAI-protocol compatibility as the three features that will shape the next 6 months of the ecosystem.

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot Project Digest — 2026-08-18

## 1. Today's Overview
NanoBot saw a high-activity period: **3 issues** and **15 PRs** were updated in the last 24 hours, with **5 PRs closed** and **10 still open**. The dominant themes were reliability hardening — Telegram polling recovery, gateway process identity, provider fallback behavior — and WebUI/CLI feature momentum around side conversations, follow-up suggestions, and a native TypeScript terminal UI. No new release was published. Overall project health looks strong, though one older gateway serialization bug ([#4864](https://github.com/HKUDS/nanobot/issues/4864)) remains open and is drawing the most community discussion.

## 2. Releases
No new releases were published in this window.

## 3. Project Progress
Five PRs were closed/merged in the last 24 hours:

- **[#5406 — feat(cli): add native TypeScript terminal UI](https://github.com/HKUDS/nanobot/pull/5406)**  
  Adds a native TypeScript terminal UI for `nanobot agent`. The PR also recovers the accidentally reverted history of [#4329](https://github.com/HKUDS/nanobot/pull/4329) and includes cross-terminal fixes.

- **[#5416 — fix(gateway): stabilize process identities](https://github.com/HKUDS/nanobot/pull/5416)**  
  Replaces locale-dependent macOS `ps lstart` identities with native `proc_pidinfo` birth timestamps and moves gateway client leases to a shared process-identity contract. This reduces cross-platform gateway identification bugs.

- **[#5156 — fix(telegram): recover from silently stalled polling](https://github.com/HKUDS/nanobot/pull/5156)**  
  Fixes [#5171](https://github.com/HKUDS/nanobot/issues/5171). Adds a watchdog to rebuild stalled Telegram polling connection pools after transient network failures.

- **[#5301 — fix(telegram): bridge stdlib logging and detect stalled polling](https://github.com/HKUDS/nanobot/pull/5301)**  
  Splits out the low-risk observability portion of the Telegram fix: stdlib logging is bridged into loguru, and a lightweight liveness check logs stalled polling without tearing down connections.

- **[#5410 — fix(goal): stop repeating clarification replies](https://github.com/HKUDS/nanobot/pull/5410)**  
  Fixes a sustained-goal bug where `AgentRunner` re-injected goal continuation after normal plain-text responses, causing repeated clarification replies. Continuation is now preserved only at the actual tool-call budget boundary.

## 4. Community Hot Topics
The clearest community hotspot is:

- **[#4864 — [bug] Endless loop for `<tool_call> <function=complete_goal>`](https://github.com/HKUDS/nanobot/issues/4864)**  
  **7 comments, 1 reaction** · Open since 2026-07-09.  
  Users report that `complete_goal` repeatedly errors because the gateway parses the `recap` parameter as a bare string instead of a JSON object. The issue points to a probable gateway bug introduced when tool parameter serialization changed. The underlying need is **reliable tool-call serialization** and **protection against runaway agent loops**.

Other attention-worthy PRs are the WebUI feature set:

- [#5408 — feat(webui): add follow-up suggestions](https://github.com/HKUDS/nanobot/pull/5408)
- [#5364 — feat(webui): add temporary side conversations](https://github.com/HKUDS/nanobot/pull/5364)
- [#5358 — feat(webui): add session messaging via mentions](https://github.com/HKUDS/nanobot/pull/5358)

These signal strong community demand for richer, multi-session WebUI interaction.

## 5. Bugs & Stability
Ranked by severity:

1. **Gateway tool-parameter serialization / `complete_goal` endless loop** — [#4864](https://github.com/HKUDS/nanobot/issues/4864)  
   Severe: prevents goal completion and can burn LLM tokens indefinitely. No dedicated fix PR has been linked yet.

2. **Disabled cron jobs still firing** — [#5407](https://github.com/HKUDS/nanobot/pull/5407)  
   Persisted heartbeat/dream system jobs in `<workspace>/cron/jobs.json` keep running after being disabled by config. This is a token-wasting regression. A fix PR exists.

3. **Provider fallback policy not applied to raised exceptions** — [#5413](https://github.com/HKUDS/nanobot/pull/5413)  
   Provider exceptions can escape the fallback loop entirely, breaking failover reliability. A fix PR exists.

4. **Windows gateway venv child process adoption** — [#5415](https://github.com/HKUDS/nanobot/pull/5415)  
   On Windows, the gateway may not correctly adopt the recorded PID of a venv launcher, disrupting lifecycle management. A fix PR with regression coverage exists.

5. **Slack file download redirect validation** — [#5414](https://github.com/HKUDS/nanobot/pull/5414)  
   Slack private download URLs may redirect across untrusted targets; validation does not cover the full redirect chain. This has security implications. A fix PR exists.

6. **Weather workflow Windows `curl` alias issue** — [#5341](https://github.com/HKUDS/nanobot/pull/5341)  
   On PowerShell, bare `curl` can resolve to `Invoke-WebRequest`, causing the first weather command to fail. A fix PR exists but carries a `conflict` label.

7. **Background gateway output not flushed to logs** — [#5412](https://github.com/HKUDS/nanobot/pull/5412)  
   Python block-buffering can delay startup output in background process logs. A fix PR exists.

Previously reported **Telegram polling stall** ([#5171](https://github.com/HKUDS/nanobot/issues/5171)) is now **closed**, with fixes merged via [#5156](https://github.com/HKUDS/nanobot/pull/5156) and [#5301](https://github.com/HKUDS/nanobot/pull/5301).

## 6. Feature Requests & Roadmap Signals
- **[#5409 — Add a Hybrid Spend Firewall](https://github.com/HKUDS/nanobot/issues/5409)**  
  Requests spend limits and anti-infinite-loop safeguards to prevent power users from exhausting LLM budgets. This is likely part of the commercialization discussion and could become a future enterprise/admin feature.

- **[#5408 — WebUI follow-up suggestions](https://github.com/HKUDS/nanobot/pull/5408)**  
  Ephemeral, chat-scoped follow-up suggestions after successful WebUI turns.

- **[#5364 — WebUI temporary side conversations](https://github.com/HKUDS/nanobot/pull/5364)**  
  Adds `/side` for transient parallel conversations with tab switching and independent drafts/streaming state.

- **[#5358 — WebUI session messaging via mentions](https://github.com/HKUDS/nanobot/pull/5358)**  
  Gives persisted sessions stable `@name` handles and exposes `list_sessions` / `send_session_message`.

- **[#5406 — Native TypeScript terminal UI](https://github.com/HKUDS/nanobot/pull/5406)**  
  Already closed, so this is likely landed or very close to landing.

Prediction: the next version will likely continue **WebUI/chat UX polish** — side conversations and follow-up suggestions are both moderate, provider-neutral additions with tests. Session mentions and the spend firewall are larger surface-area changes and may take longer.

## 7. User Feedback Summary
Real user pain points in this window:

- **Silent failures** are the most frustrating: Telegram polling can stop permanently with no logs ([#5171](https://github.com/HKUDS/nanobot/issues/5171)), and gateway serialization errors can cause endless tool-call loops ([#4864](https://github.com/HKUDS/nanobot/issues/4864)).
- **Token waste** is a recurring concern: disabled cron jobs still fire ([#5407](https://github.com/HKUDS/nanobot/pull/5407)), sustained goals repeat clarification replies ([#5410](https://github.com/HKUDS/nanobot/pull/5410)), and users want a spend firewall ([#5409](https://github.com/HKUDS/nanobot/issues/5409)).
- **Windows compatibility** still needs attention: PowerShell `curl` alias issues ([#5341](https://github.com/HKUDS/nanobot/pull/5341)) and venv child-process lifecycle ([#5415](https://github.com/HKUDS/nanobot/pull/5415)).
- **Satisfaction signals**: maintainers are shipping fixes quickly, and community contributors are submitting substantial WebUI and CLI improvements.

## 8. Backlog Watch
Items needing maintainer attention:

- **[#4864 — Endless loop for `complete_goal`](https://github.com/HKUDS/nanobot/issues/4864)**  
  Open since 2026-07-09, most commented issue in the dataset, no linked fix PR. High priority.

- **[#5341 — Windows-safe weather workflow](https://github.com/HKUDS/nanobot/pull/5341)**  
  Open since 2026-08-11, labelled `conflict`; needs conflict resolution and review.

- **[#5358 — WebUI session messaging via mentions](https://github.com/HKUDS/nanobot/pull/5358)**  
  Open since 2026-08-12; a meaningful API/UX addition still waiting on review.

- **[#5364 — WebUI temporary side conversations](https://github.com/HKUDS/nanobot/pull/5364)**  
  Open since 2026-08-13, labelled `conflict`; feature is ready for maintainer attention.

- **[#5409 — Hybrid Spend Firewall](https://github.com/HKUDS/nanobot/issues/5409)**  
  Newly opened, no responses yet; worth acknowledging to keep community trust.

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-08-18

## 1. Today’s Overview
Hermes Agent remains highly active, with 50 issues and 50 PRs updated in the last 24 hours. The repo shows a healthy balance of bug fixing, security hardening, and feature development: 12 issues closed, 12 PRs closed/merged, and one patch release (`v0.20.3`) rolled out. Desktop/profile lifecycle fixes dominate the recent merge queue, while open security and session-data-integrity issues indicate focused maintainer attention. Community engagement is steady, with the top issue accumulating 76 comments and several multi-comment feature-tracker issues still open.

---

## 2. Releases
### v2026.8.16.2 — Hermes Agent v0.20.3 (patch release)
- **Release date:** August 16, 2026
- **Tag:** `v2026.8.16.2`
- **Summary:** Patch release that rolls up **~125 PRs** merged since `v0.20.2` into a stable tagged release for downstream consumers (Docker images, hosted deployments, fresh installs).
- **Breaking changes / migration notes:** None indicated in the available release metadata. Consumers on `v0.20.2` should treat this as a backward-compatible patch-level update.

🔗 [Release v2026.8.16.2](https://github.com/NousResearch/hermes-agent/releases/tag/v2026.8.16.2)

---

## 3. Project Progress
In the last 24 hours, 12 PRs were closed/merged. The visible highlights are strongly focused on **desktop profile lifecycle, bot-mode UX, and cross-gateway messaging**:

- **fix(desktop): re-home backend and retire sockets on profile rename** — Fixes the stale-backend/ghost-profile problem on rename.  
  🔗 [#88732](https://github.com/NousResearch/hermes-agent/pull/88732)

- **fix(desktop): make Bot Chat visibility toggle effective** — Corrects the visibility boundary for canonical Bot Chat sessions.  
  🔗 [#88687](https://github.com/NousResearch/hermes-agent/pull/88687)

- **fix(hermes-bots): group chats render Hermes, not @default** — Bot Mode group chat display now shows the primary agent as “Hermes” with full speaker disambiguation.  
  🔗 [#88721](https://github.com/NousResearch/hermes-agent/pull/88721)

- **feat(cli): hermes peer — bot-to-bot DMs across machines and gateways** — Adds direct bot-to-bot messaging without a desktop client.  
  🔗 [#88725](https://github.com/NousResearch/hermes-agent/pull/88725)

- **fix desktop profile rename backend teardown** — Salvaged edge-case fix for profile rename leaving the old backend running.  
  🔗 [#51852](https://github.com/NousResearch/hermes-agent/pull/51852)

Other notable open PRs advancing features and fixes:
- `feat(desktop): Discord-style group-chat rows in Bot Mode` — [#88738](https://github.com/NousResearch/hermes-agent/pull/88738)
- `feat(desktop): unify Gateway + Connections into one Gateways settings page` — [#88735](https://github.com/NousResearch/hermes-agent/pull/88735)
- `fix(gateway): resolve the session DB inside the active profile scope` — [#88734](https://github.com/NousResearch/hermes-agent/pull/88734)
- `feat(skills): add skills.external_repo for git-backed shared skills` — [#88719](https://github.com/NousResearch/hermes-agent/pull/88719)
- `feat(providers): add Inworld model provider` — [#85774](https://github.com/NousResearch/hermes-agent/pull/85774)

---

## 4. Community Hot Topics
The most-discussed issues reveal recurring themes: architecture debt, webhook reliability, desktop performance, and tool-discovery correctness.

- **#78647 — [CLOSED] Large-file decomposition: 20/20 done (76 comments)**  
  A massive refactoring epic for sharding god-files across the repo. Closed after completing all 20 tracked items.  
  🔗 [Issue #78647](https://github.com/NousResearch/hermes-agent/issues/78647)

- **#84834 — Webhook Feature Package — graph-gated repair meta-issue (17 comments)**  
  A broad, structured repair package for the entire Hermes webhook surface. Still open and acting as a canonical tracker.  
  🔗 [Issue #84834](https://github.com/NousResearch/hermes-agent/issues/84834)

- **#53902 — Renderer stuck in fontations+temporal_rs loop; GPU 98%, 13W sustained (7 comments)**  
  Desktop performance/power regression since `v0.17.0`. Remains open with no visible fix PR.  
  🔗 [Issue #53902](https://github.com/NousResearch/hermes-agent/issues/53902)

- **#87654 — Vision tools disappear after first availability probe (5 comments)**  
  Silent tool-disappearance bug in `vision_analyze`/`browser_vision` due to cached `_AuxProbeClientStub`. High user impact, still open.  
  🔗 [Issue #87654](https://github.com/NousResearch/hermes-agent/issues/87654)

- **#88706 — [Security]: Close use-time, provenance, and authority gaps behind #88232 / #88435 (4 comments)**  
  A security follow-up tracker, proposing broader hardening beyond an already-fixed SSH credential-exposure path.  
  🔗 [Issue #88706](https://github.com/NousResearch/hermes-agent/issues/88706)

**Underlying needs:** Users are pushing for deterministic tool visibility, lower desktop resource usage, and stronger security boundaries. The webhook and god-file-sharding trackers show a desire for systematic, graph-planned repair rather than one-off patches.

---

## 5. Bugs & Stability
Ranked by severity from the active issue set:

### Critical / P1
- **hermes_state: SessionDB leaks per-thread WAL read connections → fd exhaustion / EMFILE**  
  Long-lived processes can slowly exhaust file descriptors after reader threads die. No fix PR observed in the top-20 list.  
  🔗 [#79742](https://github.com/NousResearch/hermes-agent/issues/79742)

- **Scheduler-level cron processing errors bypass failure_nudge alerting — jobs can die silently for hours**  
  Cron jobs can fail at the scheduler layer without alerting. Still open.  
  🔗 [#88655](https://github.com/NousResearch/hermes-agent/issues/88655)

- **`clarify` tool prompts don't render in chat UI — user sees no question, replies empty**  
  Open since June 27; no fix PR visible.  
  🔗 [#53666](https://github.com/NousResearch/hermes-agent/issues/53666)

### High / P2
- **Vision tools (`vision_analyze`/`browser_vision`) disappear after first availability probe**  
  Silent regression caused by cached `_AuxProbeClientStub`.  
  🔗 [#87654](https://github.com/NousResearch/hermes-agent/issues/87654)

- **`/save` session export crashes with `AttributeError: 'GatewayRunner' object has no attribute 'get_adapter'`**  
  Marked duplicate, but still reproduces.  
  🔗 [#88713](https://github.com/NousResearch/hermes-agent/issues/88713)

- **Desktop “Read Aloud Replies” triggers TTS synthesis and playback twice per message**  
  Duplicate of an earlier auto-TTS regression.  
  🔗 [#87823](https://github.com/NousResearch/hermes-agent/issues/87823)

- **`optimize-storage` can stamp empty FTS after interrupted demote — permanent search loss**  
  High-risk session-state bug, still open.  
  🔗 [#72716](https://github.com/NousResearch/hermes-agent/issues/72716)

- **install.sh `--stage` protocol masks stage failures; reports success after uv venv hard-fails**  
  Silent bootstrap failure risk. No fix PR observed.  
  🔗 [#61828](https://github.com/NousResearch/hermes-agent/issues/61828)

### Security-related fixes in flight
- **scrub credentials from embedded terminal PTY env** — [#70370](https://github.com/NousResearch/hermes-agent/pull/70370)
- **scrub credentials from `hermes serve` spawn env** — [#70372](https://github.com/NousResearch/hermes-agent/pull/70372)
- **release memory-store handles before rmtree on profile delete (Windows WinError 32)** — [#88727](https://github.com/NousResearch/hermes-agent/pull/88727)

---

## 6. Feature Requests & Roadmap Signals
Several visible feature threads are likely to shape upcoming releases:

- **Webhook Feature Package repair (meta-issue)** — Graph-gated repair across ingress, execution, delivery, config, UI, deployment, docs.  
  🔗 [#84834](https://github.com/NousResearch/hermes-agent/issues/84834)

- **Project-local `.hermes/` — per-project skills & MCP, gated by consent** — Strong platform-direction signal.  
  🔗 [#48970](https://github.com/NousResearch/hermes-agent/issues/48970)

- **Child-process credential-inheritance closure (tracker)** — Security-driven campaign epic.  
  🔗 [#83565](https://github.com/NousResearch/hermes-agent/issues/83565)

- **ByteDance (TikTok Business + Douyin) Plugin Integration** — New platform adapter demand.  
  🔗 [#86950](https://github.com/NousResearch/hermes-agent/issues/86950)

- **Transactional deployment plan for install/update/bootstrap** — Architecture proposal to unify deployment paths.  
  🔗 [#88683](https://github.com/NousResearch/hermes-agent/issues/88683)

- **New model providers:** Inworld (`#85774`) and native Cursor provider (`#88212`) are already in open PRs.
- **Bot-to-bot DMs via `hermes peer`** — just landed as a closed PR (`#88725`).
- **Git-backed shared skills** — open PR `#88719`.

**Next-version prediction:** The next minor release will likely include the desktop settings consolidation (`#88733`, `#88735`), profile-scoped session storage (`#88734`), bot-mode group-chat improvements (`#88738`, `#88721`), and possibly the new providers if review completes quickly.

---

## 7. User Feedback Summary
Real user pain points visible in the data:

- **Silent failures are the biggest trust issue.** Users repeatedly report problems that happen without obvious errors:
  - Cron jobs dying silently for hours — [#88655](https://github.com/NousResearch/hermes-agent/issues/88655)
  - Vision tools disappearing while Dashboard shows them enabled — [#87654](https://github.com/NousResearch/hermes-agent/issues/87654)
  - `install.sh` claiming success after a hard failure — [#61828](https://github.com/NousResearch/hermes-agent/issues/61828)

- **Desktop performance matters.** The renderer power draw issue (`#53902`) shows users care about background resource consumption, especially on laptops.

- **Multi-profile behavior remains confusing.** Users report stale profiles, duplicate backends, wrong session previews, and config double-write conflicts:
  - [#45474](https://github.com/NousResearch/hermes-agent/issues/45474), [#88200](https://github.com/NousResearch/hermes-agent/issues/88200), [#61023](https://github.com/NousResearch/hermes-agent/issues/61023), [#37751](https://github.com/NousResearch/hermes-agent/issues/37751)

- **Security consciousness is high.** Several issues and PRs focus on preventing credential leakage into child processes, PTYs, and spawned backends — e.g., `#88706`, `#83565`, `#70370`, `#70372`.

- **Feature demand is broad and platform-oriented:** webhooks, Telegram menu APIs, WhatsApp stickers, TikTok/Douyin, project-local skills, and new model providers.

Overall tone: users appreciate the fast iteration and breadth of integrations, but are frustrated by silent state corruption, session/profile edge cases, and desktop reliability gaps.

---

## 8. Backlog Watch
These items appear important but have been waiting for maintainer action:

- **#53902 — Renderer stuck in fontations+temporal_rs loop (GPU 98%, 13W)**  
  Opened June 28, P3, 7 comments, no fix PR. High user-visible impact on desktop power draw.  
  🔗 [Issue #53902](https://github.com/NousResearch/hermes-agent/issues/53902)

- **#53666 — `clarify` tool prompts don't render in chat UI**  
  Opened June 27, P1, 3 comments, no fix PR. Blocks interactive tool workflows.  
  🔗 [Issue #53666](https://github.com/NousResearch/hermes-agent/issues/53666)

- **#72716 — `optimize-storage` can stamp empty FTS after interrupted demote**  
  Opened July 27, P2, 3 comments, no fix PR. Permanent search loss risk.  
  🔗 [Issue #72716](https://github.com/NousResearch/hermes-agent/issues/72716)

- **#61828 — install.sh `--stage` masks stage failures**  
  Opened July 10, P2, 2 comments, no fix PR. Silent bootstrap failure.  
  🔗 [Issue #61828](https://github.com/NousResearch/hermes-agent/issues/61828)

- **#37751 — Desktop/Gateway config double-write conflict**  
  Opened June 3, P2, 2 comments, no visible fix PR. Still causes model-switch regression for Windows users.  
  🔗 [Issue #37751](https://github.com/NousResearch/hermes-agent/issues/37751)

- **#70370 / #70372 — Desktop credential-scrub security PRs**  
  Both opened July 23, still open, with security-related fixes. These have been waiting nearly a month and should be prioritized for merge.  
  🔗 [#70370](https://github.com/NousResearch/hermes-agent/pull/70370) · [#70372](https://github.com/NousResearch/hermes-agent/pull/70372)

---

**Overall project health:** Hermes Agent is shipping steadily and addressing a complex surface area — desktop, gateway, plugins, providers, and security. The biggest risks are silent failure modes and a growing set of P1/P2 issues that have been open for weeks without observable fix PRs.

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw Project Digest — 2026-08-18

## 1. Today's Overview
As of 2026-08-18, PicoClaw saw moderate activity: 4 issues and 4 PRs were updated in the last 24 hours. One issue was closed and three remain open; three PRs were closed/merged and one is open. No new releases were published. Project health looks positive: a long-standing silent tool-loop bug (#3311) was fixed by #3312, and a Slack media upload bug (#3338) already has an open community fix (#3340). Several stale-labeled items were also closed, suggesting maintainer backlog cleanup is happening.

## 2. Releases
No new releases were published in the last 24 hours.

## 3. Project Progress
- **#3312 — fix(agent): stop turn early on repeated identical tool failure**  
  Closed/merged. Fixes the silent agent loop that could run until `max_tool_iterations` when a tool fails with the same error every time. Resolves issue #3311.  
  [sipeed/picoclaw PR #3312](https://github.com/sipeed/picoclaw/pull/3312)

- **#271 — fix: env overrides when config.json is missing and add regression test**  
  Closed/merged. Ensures environment variables are applied even when `config.json` is absent, which is important for Fly.io-style env-only deployments.  
  [sipeed/picoclaw PR #271](https://github.com/sipeed/picoclaw/pull/271)

- **#2606 — feat: enhance Weixin channel support and configuration**  
  Closed/merged. Adds multi-instance channel directory handling, dynamic instance support, and better validation/error handling for illegal channel names.  
  [sipeed/picoclaw PR #2606](https://github.com/sipeed/picoclaw/pull/2606)

- **#3340 — fix(slack): set FileSize on media upload params**  
  Open. Community fix for Slack media uploads failing with `file size cannot be 0`; needs review and merge.  
  [sipeed/picoclaw PR #3340](https://github.com/sipeed/picoclaw/pull/3340)

## 4. Community Hot Topics
- **#3287 — [Feature] Better support long messages in IRC**  
  Most active item with 6 comments. Users want PicoClaw to treat long IRCv3 messages as a single cohesive message, despite IRC's 512-byte limit and client-side splitting. Underlying need: proper IRC protocol handling for modern long-form content.  
  [sipeed/picoclaw Issue #3287](https://github.com/sipeed/picoclaw/issues/3287)

- **#3311 — [BUG] Repeated identical tool failure loops silently to max_tool_iterations**  
  2 comments. Reported from production Telegram usage: a user asked the agent to run a `git` command and never received a reply. This highlights the need for early termination and visible failure reporting in agent loops.  
  [sipeed/picoclaw Issue #3311](https://github.com/sipeed/picoclaw/issues/3311)

## 5. Bugs & Stability
Ranked by severity:

1. **#3311 — Silent tool-failure loop (Resolved)**  
   High severity: agent could run for minutes and never answer the user. Fixed by PR #3312.  
   [Issue #3311](https://github.com/sipeed/picoclaw/issues/3311) · [PR #3312](https://github.com/sipeed/picoclaw/pull/3312)

2. **#3338 — Slack media uploads always fail (Open)**  
   High severity for Slack users: uploads fail before any network call because `FileSize` is not set. Open fix exists in #3340.  
   [Issue #3338](https://github.com/sipeed/picoclaw/issues/3338) · [PR #3340](https://github.com/sipeed/picoclaw/pull/3340)

3. **#3339 — Antigravity generation returns generic 429 despite valid auth (Open)**  
   Medium/high severity for Antigravity users: model discovery works, but every generation request fails with `RESOURCE_EXHAUSTED`. No fix PR yet.  
   [Issue #3339](https://github.com/sipeed/picoclaw/issues/3339)

4. **#271 — Env overrides ignored when config.json is missing (Resolved)**  
   Medium severity for env-based deployments; fixed by PR #271.  
   [Issue context in PR #271](https://github.com/sipeed/picoclaw/pull/271)

## 6. Feature Requests & Roadmap Signals
- **#3287 — Long IRC message support**  
  This is the clearest active feature request. It will likely require protocol-aware message splitting/coalescing for IRCv3. Could land in a future IRC channel improvement release.  
  [Issue #3287](https://github.com/sipeed/picoclaw/issues/3287)

- **#2606 — Weixin channel multi-instance enhancement**  
  Now closed/merged, signaling continued investment in multi-platform channel support and configuration management.  
  [PR #2606](https://github.com/sipeed/picoclaw/pull/2606)

- **#3339 — Antigravity provider issues**  
  Suggests Google Antigravity is a supported or emerging provider, but rate-limit/quota error handling needs improvement.  
  [Issue #3339](https://github.com/sipeed/picoclaw/issues/3339)

## 7. User Feedback Summary
- Users are running PicoClaw in production via Telegram and Slack, and reliability issues like silent agent loops are considered critical.
- Slack media upload failure is a concrete blocker for Slack users; the community quickly proposed a fix.
- IRC users need better handling of long messages constrained by protocol limits.
- Env-only deployments, especially on Fly, surfaced a real configuration loading bug that has now been fixed.
- Overall sentiment is engaged: users are reporting detailed bugs and contributing fixes, but maintainer attention is still needed on the open Slack fix and new Antigravity issue.

## 8. Backlog Watch
- **#3287 — Long IRC message support**  
  Open since 2026-07-22, marked stale, with 6 comments and no linked PR. Needs maintainer decision or implementation.  
  [Issue #3287](https://github.com/sipeed/picoclaw/issues/3287)

- **#3340 — Slack FileSize fix PR**  
  Open and currently the direct fix for #3338. Needs review/merge to unblock Slack media uploads.  
  [PR #3340](https://github.com/sipeed/picoclaw/pull/3340)

- **#3339 — Antigravity 429 errors**  
  New issue with no comments or fix yet; needs triage to determine whether this is a provider-side quota problem or a PicoClaw integration bug.  
  [Issue #3339](https://github.com/sipeed/picoclaw/issues/3339)

- **#3338 — Slack media upload bug**  
  Open and unresolved until #3340 is merged; needs maintainer acknowledgement.  
  [Issue #3338](https://github.com/sipeed/picoclaw/issues/3338)

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Project Digest — 2026-08-18

## 1. Today's Overview

NanoClaw saw a high-activity 24-hour window: **4 issues updated** (3 open, 1 closed) and **39 PRs updated** (16 open, 23 merged/closed), with no new releases. Activity was concentrated in channel-layer infrastructure, container extension seams, and several open regression fixes. The project looks healthy and responsive, with core-team PRs landing around Slack/canvas channel support, inbound-policy hooks, and MCP tool extensibility. Two notable regressions involving task logs and pending-message polling both have corresponding open fix PRs.

## 2. Releases

No new releases were published in the last 24 hours.

## 3. Project Progress

The 23 closed/merged PRs include a substantial amount of channel and container infrastructure work. Notable items:

- [PR #3305](https://github.com/nanocoai/nanoclaw/pull/3305): Shared Slack channel-layer library + canvas cluster, including a main sync merge.
- [PR #3304](https://github.com/nanocoai/nanoclaw/pull/3304): Adapter-declared session-mode context defaults, with thread-derived stamping.
- [PR #3292](https://github.com/nanocoai/nanoclaw/pull/3292): Inbound-policy registration seam on the Chat SDK bridge.
- [PR #3297](https://github.com/nanocoai/nanoclaw/pull/3297): Setup wizard per-channel pre-steps and companion-skill declarations.
- [PR #3293](https://github.com/nanocoai/nanoclaw/pull/3293): Router session-created hook for new engaged sessions.
- [PR #3294](https://github.com/nanocoai/nanoclaw/pull/3294): Post-delivery hook with first-delivery context.
- [PR #3296](https://github.com/nanocoai/nanoclaw/pull/3296): `extendTool` additive MCP tool schema/description extension.
- [PR #3295](https://github.com/nanocoai/nanoclaw/pull/3295): Generic membership-event hook on the Chat SDK bridge.

Also closed: [Issue #1143](https://github.com/nanocoai/nanoclaw/issues/1143), a documentation bug about the removed `/data/env` path.

A clear next wave is visible in the open stacked PRs [ #3306](https://github.com/nanocoai/nanoclaw/pull/3306) and [ #3307](https://github.com/nanocoai/nanoclaw/pull/3307), which introduce a session-runtime driver seam with Docker as the built-in realization, plus [ #3308](https://github.com/nanocoai/nanoclaw/pull/3308) to prevent group creation over existing undisposed folders.

## 4. Community Hot Topics

By comment count, issue activity was modest:

- [Issue #1143](https://github.com/nanocoai/nanoclaw/issues/1143) — 2 comments — closed documentation bug about stale `/data/env` references in skill docs.
- [Issue #3203](https://github.com/nanocoai/nanoclaw/issues/3203) — 1 comment — open issue about the codex provider emitting an undeclared `file` ProviderEvent, causing typecheck failures and dropped generated images.

PR comment counts were not provided, but the concentrated core-team activity around [PR #3305](https://github.com/nanocoai/nanoclaw/pull/3305), [PR #3306](https://github.com/nanocoai/nanoclaw/pull/3306), [PR #3307](https://github.com/nanocoai/nanoclaw/pull/3307), and [PR #3308](https://github.com/nanocoai/nanoclaw/pull/3308) signals strong momentum around channel abstractions and session runtime extensibility.

Underlying needs visible in these threads:

- Skills documentation must stay accurate as the container layout changes.
- Provider event contracts need to be type-safe and must not lose artifacts such as codex-generated images.
- The community/contributor base is pushing for reusable channel seams rather than per-bridge source edits.

## 5. Bugs & Stability

Ranked roughly by severity:

1. **High — codex provider event contract breaks typecheck and drops images**  
   [Issue #3203](https://github.com/nanocoai/nanoclaw/issues/3203) — The codex provider emits an undeclared `file` ProviderEvent; `/add-codex` fails typecheck on `main`, and nothing consumes the event, so generated images are silently dropped. No direct fix PR is visible yet, though [PR #3299](https://github.com/nanocoai/nanoclaw/pull/3299) addresses a related codex pin issue.

2. **High — task rows firing in chat sessions lose logs and replies**  
   [Issue #3301](https://github.com/nanocoai/nanoclaw/issues/3301) — Since the one-door task delivery change, task runs in chat sessions lose run logs, replies are eaten, and series are unlisted.  
   Fix PR: [PR #3303](https://github.com/nanocoai/nanoclaw/pull/3303).

3. **Medium — pending-message polling is unbounded**  
   [Issue #3289](https://github.com/nanocoai/nanoclaw/issues/3289) — `getPendingMessages()` loads all due pending rows into JavaScript before filtering, which can accumulate badly under backlog.  
   Fix PR: [PR #3291](https://github.com/nanocoai/nanoclaw/pull/3291).

4. **Medium — OneCLI gateway bind address is wrong**  
   [PR #3302](https://github.com/nanocoai/nanoclaw/pull/3302) fixes [Issue #2903](https://github.com/nanocoai/nanoclaw/issues/2903) by correcting the default OneCLI gateway bind address.

5. **Low/Medium — attachment type not escaped in agent-facing XML**  
   [PR #3300](https://github.com/nanocoai/nanoclaw/pull/3300) fixes `formatAttachments` escaping every rendered field except `type`.

6. **Closed — stale docs reference removed `/data/env` path**  
   [Issue #1143](https://github.com/nanocoai/nanoclaw/issues/1143) was closed, removing a documentation risk.

Also notable as a data-loss prevention: [PR #3308](https://github.com/nanocoai/nanoclaw/pull/3308) refuses to create a new agent group over an already-existing folder.

## 6. Feature Requests & Roadmap Signals

Several feature-forward PRs are open and likely candidates for the next version:

- **Local web chat** — Two independent proposals are now open: [PR #3298](https://github.com/nanocoai/nanoclaw/pull/3298) and [PR #3290](https://github.com/nanocoai/nanoclaw/pull/3290). Both aim to add a browser-accessible local chat channel, suggesting real community demand.
- **Observability dashboard** — [PR #3288](https://github.com/nanocoai/nanoclaw/pull/3288) adds an `/add-clawmetry` skill for a read-only local dashboard with a NanoClaw session adapter.
- **Session runtime driver seam** — [PR #3306](https://github.com/nanocoai/nanoclaw/pull/3306) and [PR #3307](https://github.com/nanocoai/nanoclaw/pull/3307) lay groundwork for pluggable session runtimes beyond Docker.
- **Bounded structured CLI input** — [PR #3218](https://github.com/nanocoai/nanoclaw/pull/3218) proposes `--stdin-json` for host and container `ncl` clients.
- **Codex model retirement handling** — [PR #3299](https://github.com/nanocoai/nanoclaw/pull/3299) bumps `@openai/codex` before GPT-5.4 retires from Codex on 2026-08-31.

Prediction: the next NanoClaw release is likely to include channel-layer extensions, the local web chat work, and fixes for task-run logging and bounded polling.

## 7. User Feedback Summary

Real user pain points visible in today's data:

- **Documentation drift**: Users following skill instructions can hit removed paths like `/data/env`.  
  [Issue #1143](https://github.com/nanocoai/nanoclaw/issues/1143)
- **Data loss in chat-session task runs**: Run logs and replies disappear for task rows firing in chat sessions.  
  [Issue #3301](https://github.com/nanocoai/nanoclaw/issues/3301)
- **Codex artifacts silently dropped**: Generated images are not wired into any consumer even after typecheck is fixed.  
  [Issue #3203](https://github.com/nanocoai/nanoclaw/issues/3203)
- **Backlogged pending messages can cause runaway polling**.  
  [Issue #3289](https://github.com/nanocoai/nanoclaw/issues/3289)
- **Users want local, non-external chat surfaces**: Two separate webchat PRs were opened on the same day.  
  [PR #3298](https://github.com/nanocoai/nanoclaw/pull/3298), [PR #3290](https://github.com/nanocoai/nanoclaw/pull/3290)
- **Sessions/observability**: Contributors want an overnight-scanning dashboard rather than relying only on external AI tools.  
  [PR #3288](https://github.com/nanocoai/nanoclaw/pull/3288)

Overall sentiment seems cooperative and feature-hungry, with frustration mainly around regressions and silent data loss.

## 8. Backlog Watch

- [Issue #3203](https://github.com/nanocoai/nanoclaw/issues/3203) — Open since 2026-08-08, still unresolved, and critical: typecheck failure plus dropped generated images. Needs a direct provider event fix or explicit triage.
- [PR #3218](https://github.com/nanocoai/nanoclaw/pull/3218) — Open since 2026-08-09 with no visible merge/close; a generic `--stdin-json` feature that could still be relevant for the next release.
- [PR #3298](https://github.com/nanocoai/nanoclaw/pull/3298) and [PR #3290](https://github.com/nanocoai/nanoclaw/pull/3290) — Two competing webchat implementations need maintainer direction to avoid divergence.
- [PR #3291](https://github.com/nanocoai/nanoclaw/pull/3291) and [PR #3303](https://github.com/nanocoai/nanoclaw/pull/3303) — Fix PRs for two open regressions should be prioritized for review/merge.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

# NullClaw Project Digest — 2026-08-18

## 1. Today's Overview
NullClaw is in a quiet maintenance phase: zero issues were created or updated in the last 24 hours, no PRs were merged or closed, and no releases were published. The only activity is a single open Dependabot pull request (#956) that updates the Alpine base image from 3.23 to 3.24 and was last touched on August 17. With no new bug reports, regressions, or feature requests, project health appears stable but development velocity is effectively nil for this window. The primary actionable item for maintainers is clearing the two-month-old dependency PR sitting in the backlog.

## 2. Releases
No new releases were published in this reporting window.

## 3. Project Progress
No PRs were merged or closed during the last 24 hours, so no features advanced or fixes landed. The only PR-level activity is Dependabot's open [PR #956](https://github.com/nullclaw/nullclaw/pull/956), which remains unintegrated. This is expected for a low-activity day rather than a sign of stalled development.

## 4. Community Hot Topics
There is no meaningful community discussion in this window. The sole active PR, [#956](https://github.com/nullclaw/nullclaw/pull/956) — an automated Alpine 3.23 → 3.24 Docker dependency bump — has zero comments and zero reactions. Since it is bot-generated housekeeping, it reflects routine infrastructure maintenance rather than any user-driven conversation.

## 5. Bugs & Stability
No bugs, crashes, or regressions were reported in the last 24 hours, and no stability-related issues are currently open. There are no known acute problems in the project, and no fix PRs are outstanding beyond the routine dependency bump.

## 6. Feature Requests & Roadmap Signals
No feature requests were filed or updated in this window, and no roadmap signals can be inferred from the available data. The only open PR is a dependency update, which carries no forward-looking feature implications. Any product direction signals will require tracking future issue activity.

## 7. User Feedback Summary
No user feedback was recorded in the past 24 hours — no comments, issue reports, or reactions on the open PR. Without new qualitative data, there is no measurable satisfaction or dissatisfaction signal for this period.

## 8. Backlog Watch
The item most needing maintainer attention is [PR #956](https://github.com/nullclaw/nullclaw/pull/956): a Dependabot-bumped Alpine Docker image (3.23 → 3.24) opened on **June 15, 2026** and still open after more than two months (last updated August 17). Despite the update, it has received no comments or reviews, indicating it has been silently waiting. Maintainers should review and either merge or close it; an open dependency PR of this age also carries minor supply-chain hygiene implications if left indefinitely pending.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-08-18

**Data source:** GitHub issues, PRs, and releases for [nearai/ironclaw](https://github.com/nearai/ironclaw)

---

## 1. Today's Overview

IronClaw activity is very high: 28 issues were updated in the last 24 hours (22 open, 6 closed), and 44 PRs were updated (28 open, 16 merged/closed). Project work is concentrated on reducing durable DB write pressure, hardening the resource governor under libSQL load, building a durable notification inbox, and normalizing WASM/provider capability responses. A new release candidate, `ironclaw-v1.3.0-rc.1`, was published on 2026-08-17, although its release notes are empty. Overall project health looks strong: core maintainers are actively closing performance epics and responding to QA/bug-bash findings with targeted fix PRs.

---

## 2. Releases

### [ironclaw-v1.3.0-rc.1](https://github.com/nearai/ironclaw/releases/tag/ironclaw-v1.3.0-rc.1)
- **Version:** 1.3.0-rc.1
- **Published:** 2026-08-17
- **Release notes:** None provided.
- **Install:** Prebuilt installers are available via `curl` shell script and PowerShell script.

No explicit breaking changes or migration notes were included in the release artifact. Given the active PR pipeline, this RC likely captures the current `main` state including performance, automation, and WebUI/notification work, but verification from release notes is not possible yet.

---

## 3. Project Progress

### Closed issues in the last 24 hours
- [Issue #7275](https://github.com/nearai/ironclaw/issues/7275) — Closed: production verification of explicit persistent memory recall across conversations.
- [Issue #7594](https://github.com/nearai/ironclaw/issues/7594) — Closed: Tier 1 write-pressure reduction by routing loop milestone sink through `CoalescingEventSink`.
- [Issue #7598](https://github.com/nearai/ironclaw/issues/7598) — Closed: Tier 2 collapse capability invocation-state writes to gate/terminal edges.
- [Issue #7605](https://github.com/nearai/ironclaw/issues/7605) — Closed: Tier 3 fold message lookup-index sibling rows into the message row.
- [Issue #7637](https://github.com/nearai/ironclaw/issues/7637) — Closed: typed design-system component boundary.
- [Issue #7647](https://github.com/nearai/ironclaw/issues/7647) — Closed: deterministic no-delivery outcome for scheduled automations.

### Closed/merged PRs visible in the sample
- [PR #7663](https://github.com/nearai/ironclaw/pull/7663) — Closed/merged: forward-port of 1.2 release fixes and thread-index repair.
- [PR #7703](https://github.com/nearai/ironclaw/pull/7703) — Closed: typed WASM WIT tool response PR, superseded and folded into [PR #7711](https://github.com/nearai/ironclaw/pull/7711).
- [PR #7710](https://github.com/nearai/ironclaw/pull/7710) — Closed: addressed multi-agent review findings on [PR #7682](https://github.com/nearai/ironclaw/pull/7682), targeting the Slack connect-nudge branch.

### Notable open PRs advanced
- [PR #7717](https://github.com/nearai/ironclaw/pull/7717) — Fix for libSQL write-lane starvation cascading through the resource governor.
- [PR #7712](https://github.com/nearai/ironclaw/pull/7712) — Makes BeforeModel checkpoint batching opt-in and side-effect-safe.
- [PR #7709](https://github.com/nearai/ironclaw/pull/7709) — Bounds the lease fence read by the observed lease to reduce redundant reads.
- [PR #7708](https://github.com/nearai/ironclaw/pull/7708) — Adds run-now for automations across trigger domain and WebUI.
- [PR #7718](https://github.com/nearai/ironclaw/pull/7718) — Adds semantic Google Docs editing tools.
- [PR #7711](https://github.com/nearai/ironclaw/pull/7711) — Final PR of the capability-response-normalization stack: typed tool response, guest migration, dispatch-error cleanup.
- [PR #7694](https://github.com/nearai/ironclaw/pull/7694) — Adds durable backend suggestions.
- [PR #7693](https://github.com/nearai/ironclaw/pull/7693) — Adds native structured output finalization.
- [PR #7692](https://github.com/nearai/ironclaw/pull/7692) — Normalizes provider failures and auth diagnostics for model context.
- [PR #7682](https://github.com/nearai/ironclaw/pull/7682) — Delivers Slack unlinked-user connect nudge privately with one-click flow.

---

## 4. Community Hot Topics

Most active issues by comment count:

- [Issue #7275](https://github.com/nearai/ironclaw/issues/7275) — 4 comments. Closed verification of persistent memory recall; stems from user feedback in #7185. Underlying need: reliable long-term memory across conversations.
- [Issue #7591](https://github.com/nearai/ironclaw/issues/7591) — 3 comments. Epic to reduce durable DB write pressure ~60% while preserving multi-worker safety. Underlying need: performance and cost stabilization on write-heavy paths.
- [Issue #3762](https://github.com/nearai/ironclaw/issues/3762) — 2 comments. Editing `AGENTS.md` in the WebUI does not update the system prompt. Underlying need: live identity/prompt updates for current and future conversations.
- [Issue #7701](https://github.com/nearai/ironclaw/issues/7701) — 2 comments. Collapse resource-governor reserve+reconcile into one post-call spend write.
- [Issue #7603](https://github.com/nearai/ironclaw/issues/7603) — 2 comments. Batch BeforeModel checkpoints per-N iterations.
- [Issue #7604](https://github.com/nearai/ironclaw/issues/7604) — 2 comments. Collapse paired row writes across events, audit spans, trigger fires, and auth flows.

No reaction data was captured in this dataset. The comment activity centers on durability, write amplification, persistent memory, and identity-file editing — indicating the community/maintainer focus is shifting from raw feature work toward operational reliability and user-visible consistency.

---

## 5. Bugs & Stability

Ranked by estimated severity:

1. **High — libSQL write-lane starvation cascades through resource governor**  
   [Issue #7714](https://github.com/nearai/ironclaw/issues/7714)  
   During PinchBench, the shared libSQL write connection starves the resource-governor journal for ~40s, causing authority invalidation, permanent reservation leaks, and capability-call failures.  
   **Fix PR:** [PR #7717](https://github.com/nearai/ironclaw/pull/7717) is open.

2. **High correctness gap — obligation audit records never attached in production**  
   [Issue #7702](https://github.com/nearai/ironclaw/issues/7702)  
   `AuditBefore`/`AuditAfter` records required by the documented host-api contract are not being written at all. This is a silent contract violation rather than a user-facing crash, but it matters for auditability and correctness.

3. **Medium — storage write-lane concurrency defect in clawbench failures**  
   [Issue #7704](https://github.com/nearai/ironclaw/issues/7704)  
   Daily failure taxonomy identifies the largest fixable IronClaw defect as a storage write-lane concurrency issue, likely related to the broader write-pressure work.

4. **Medium — AGENTS.md edits do not update the system prompt**  
   [Issue #3762](https://github.com/nearai/ironclaw/issues/3762)  
   Customer-facing P1 for v1.4.0; affects ongoing and future conversations after WebUI saves.

5. **Medium — Slack unlinked-user connect message is public and manual**  
   [Issue #7681](https://github.com/nearai/ironclaw/issues/7681)  
   Privacy/UX bug: reply is visible to a shared channel and requires manual round-trip.  
   **Fix PR:** [PR #7682](https://github.com/nearai/ironclaw/pull/7682), with review fixes in [PR #7710](https://github.com/nearai/ironclaw/pull/7710).

6. **Medium QA bug — MCP server flow missing bearer key auth and transport options**  
   [Issue #7716](https://github.com/nearai/ironclaw/issues/7716)  
   Add MCP server flow lacks bearer/token auth and STDIO/HTTP transport choices.

7. **Medium QA bug — Telegram connection flow lacks bot vs. personal account consent**  
   [Issue #7715](https://github.com/nearai/ironclaw/issues/7715)  
   Users cannot choose or confirm whether they are connecting a bot or a personal account.

8. **Low/Medium — unbounded shutdown flush and latching `pending_flush_error`**  
   [Issue #7705](https://github.com/nearai/ironclaw/issues/7705)  
   Two non-blocking findings from review of #7631: shutdown can hang on a wedged event backend, and `CoalescingEventSink` latches errors.

9. **Low — BeforeModel checkpoint batching approach was unsafe as originally specified**  
   [Issue #7707](https://github.com/nearai/ironclaw/issues/7707)  
   Split out of #7603 after integration tests; [PR #7712](https://github.com/nearai/ironclaw/pull/7712) makes batching opt-in and side-effect-safe.

10. **Resolved — persistent memory recall verification**  
    [Issue #7275](https://github.com/nearai/ironclaw/issues/7275) was closed after production verification.

---

## 6. Feature Requests & Roadmap Signals

- **GitHub Projects v2 field manipulation**  
  [Issue #7719](https://github.com/nearai/ironclaw/issues/7719) — Request to let IronClaw update Projects v2 fields such as backlog priority. Blocked an internal priority update, so this is a concrete user need.

- **MCP server auth and transport options**  
  [Issue #7716](https://github.com/nearai/ironclaw/issues/7716) — Bearer key/token auth plus STDIO/HTTP transport selection for MCP servers.

- **Telegram connection consent/selection**  
  [Issue #7715](https://github.com/nearai/ironclaw/issues/7715) — Let users choose between bot and personal account during connection.

- **Durable notification inbox**  
  [Issue #7687](https://github.com/nearai/ironclaw/issues/7687), [Issue #7688](https://github.com/nearai/ironclaw/issues/7688), [Issue #7689](https://github.com/nearai/ironclaw/issues/7689), [Issue #7690](https://github.com/nearai/ironclaw/issues/7690), [Issue #7691](https://github.com/nearai/ironclaw/issues/7691) — A clear roadmap push toward a server-backed, durable user inbox for approvals, auth requirements, blocked runs, failures, and completions.

- **Automation run-now**  
  [PR #7708](https://github.com/nearai/ironclaw/pull/7708) — Atomic manual-fire path preserving schedule while creating a separated fire identity.

- **Native structured output finalization**  
  [PR #7693](https://github.com/nearai/ironclaw/pull/7693) — Provider-neutral immutable output contract for turn/run context.

- **Durable backend suggestions**  
  [PR #7694](https://github.com/nearai/ironclaw/pull/7694) — Product-surface-neutral suggestion operations.

- **AGENTS.md system prompt live updates**  
  [Issue #3762](https://github.com/nearai/ironclaw/issues/3762) — Explicitly labeled for v1.4.0, so it is a strong next-version candidate.

**Prediction:** With `v1.3.0-rc.1` already cut, the next minor release is likely to include the durable inbox/notification work, Slack connect-flow privacy fixes, MCP auth/transport improvements, and possibly the live `AGENTS.md` system-prompt update labeled for v1.4.0.

---

## 7. User Feedback Summary

Real user pain points visible in this dataset:

- **Persistent memory reliability** — Users reported that information established in one conversation is not reliably recalled later; this drove the closed verification issue [Issue #7275](https://github.com/nearai/ironclaw/issues/7275).
- **Identity-file edits not honored** — [Issue #3762](https://github.com/nearai/ironclaw/issues/3762) shows user confusion when `AGENTS.md` edits appear to succeed but never affect the system prompt.
- **Slack onboarding friction** — [Issue #7681](https://github.com/nearai/ironclaw/issues/7681) shows dissatisfaction with a public, multi-step connect message.
- **QA/bug-bash friction on integrations** — [Issue #7715](https://github.com/nearai/ironclaw/issues/7715) and [Issue #7716](https://github.com/nearai/ironclaw/issues/7716) report missing consent and auth options in Telegram/MCP setup flows.
- **Performance pain under load** — [Issue #7704](https://github.com/nearai/ironclaw/issues/7704) and [Issue #7714](https://github.com/nearai/ironclaw/issues/7714) document real benchmark/load failures caused by write-lane contention.

Satisfaction signals are indirect but positive: the maintainers are closing high-severity write-pressure issues in the same week they are reported, and multiple fix PRs are already open or merged.

---

## 8. Backlog Watch

Items that may need maintainer attention:

- [Issue #3762](https://github.com/nearai/ironclaw/issues/3762) — Open since 2026-05-18; customer-facing, labeled suggested_P1 and v1.4.0. Longest-standing user-facing issue in the dataset.
- [PR #6994](https://github.com/nearai/ironclaw/pull/6994) — Open since 2026-08-01; OOBE automation-tasks prototype, large XL WebUI/design PR that has not been merged.
- [PR #7184](https://github.com/nearai/ironclaw/pull/7184) — Open since 2026-08-04; Nostr host functions for WASM tools from a new contributor.
- [PR #7406](https://github.com/nearai/ironclaw/pull/7406) — Open since 2026-08-09; dependency bump for the actions group.
- [PR #7513](https://github.com/nearai/ironclaw/pull/7513) — Open since 2026-08-11; ACP serve command with streaming and cancel support from a new contributor.
- [PR #7491](https://github.com/nearai/ironclaw/pull/7491) — Open since 2026-08-11; large coding-tool contract + engines + benchmark arm.

These are not necessarily ignored, but they are the oldest open PRs in the current dataset and would benefit from review/merge decisions to avoid a growing queue.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI Project Digest — 2026-08-18

## 1. Today's Overview

LobsterAI is in a high-velocity PR phase: 21 PRs were updated in the last 24 hours, with 17 closed/merged and 4 still open. No new releases were published, and all 7 touched issues remain open/active, meaning no issue was resolved in this window. The high PR closure count includes both new DeepSeek Harness/runtime work and a backlog of older Cowork UX PRs from April being cleaned up. Project health is generally positive on the contribution side, but the issue tracker is still dominated by long-standing, stale bug reports around local models, MCP, and policy persistence.

## 2. Releases

None.  
There are no new versions to report, so no breaking changes or migration notes are available for this digest.

## 3. Project Progress

Aggregate data says 17 PRs were closed/merged in the last 24 hours, with 4 open. Notable movements:

### New runtime/integration work
- **DeepSeek Harness engine integration** — [PR #2502](https://github.com/netease-youdao/LobsterAI/pull/2502) closed/merged; adds dsh engine integration.
- **dsh process launcher** — [PR #2505](https://github.com/netease-youdao/LobsterAI/pull/2505) closed/merged.
- **dsh documentation** — [PR #2506](https://github.com/netease-youdao/LobsterAI/pull/2506) open, adding DeepSeek Harness runtime setup instructions.
- **OrcaRouter provider integration** — [PR #2504](https://github.com/netease-youdao/LobsterAI/pull/2504) open, adding OrcaRouter as a first-class provider.

### Desktop / Electron fixes
- **Edit context menu for text inputs** — [PR #2503](https://github.com/netease-youdao/LobsterAI/pull/2503) closed/merged, adds Cut/Copy/Paste/Select All to native text inputs.
- **Skill upgrade progress overlay fix** — [PR #2501](https://github.com/netease-youdao/LobsterAI/pull/2501) closed/merged, renders overlay through `document.body` and adds better logging.
- **Windows right-click menu registration** — [PR #1642](https://github.com/netease-youdao/LobsterAI/pull/1642) closed/merged, adds optional `LobsterAI` directory context menu.

### Cowork / chat UX
- **Scroll-to-bottom button** — [PR #1636](https://github.com/netease-youdao/LobsterAI/pull/1636) closed/merged.
- **AI message “regenerate” button** — [PR #1637](https://github.com/netease-youdao/LobsterAI/pull/1637) closed/merged.
- **i18n fix for hardcoded tooltips** — [PR #1639](https://github.com/netease-youdao/LobsterAI/pull/1639) closed/merged.
- **Copy button for tool results** — [PR #1640](https://github.com/netease-youdao/LobsterAI/pull/1640) closed/merged.
- **Esc-to-close for all modals** — [PR #1641](https://github.com/netease-youdao/LobsterAI/pull/1641) closed/merged.
- **Session list grouping by time period** — [PR #1675](https://github.com/netease-youdao/LobsterAI/pull/1675) closed/merged.

### Agent / settings / infrastructure
- **OpenClaw upgrade to v2026.4.12** — [PR #1663](https://github.com/netease-youdao/LobsterAI/pull/1663) closed/merged; also fixes `openclaw-weixin` plugin compatibility.
- **Per-agent working directories** — [PR #1668](https://github.com/netease-youdao/LobsterAI/pull/1668) closed/merged.
- **Log desensitization** — [PR #1661](https://github.com/netease-youdao/LobsterAI/pull/1661) closed/merged; prevents API keys/tokens from leaking into exported logs.
- **Qwen console link migration from Lingji to Bailian** — [PR #1667](https://github.com/netease-youdao/LobsterAI/pull/1667) closed/merged.
- **Settings provider UX improvements** — [PR #1669](https://github.com/netease-youdao/LobsterAI/pull/1669) closed/merged; fixes disabled state for “Test Connection” and custom provider display name.

## 4. Community Hot Topics

The most-discussed item in the issue tracker is:

- **[Issue #1653 — `groupPolicy` is overwritten to `allowlist` after a while](https://github.com/netease-youdao/LobsterAI/issues/1653)**  
  2 comments, 0 reactions. Created in April and still open. The user reports that their policy configuration is silently reverted over time, which points to a config persistence/reconciliation bug.

- **[Issue #2500 — VOKO: cross-platform instant messaging and group collaboration for AI agents](https://github.com/netease-youdao/LobsterAI/issues/2500)**  
  1 comment, new on 2026-08-17. This is a project pitch from the VOKO author proposing A2A standardization and cross-agent communication support.

No reactions were recorded on any issue. Since PR comment counts are not shown in the provided data, issue comments are the main signal; overall discussion volume is low, but the active items are meaningful.

## 5. Bugs & Stability

All 7 touched issues are still open. Ranked by severity:

### High severity
- **[Issue #1635 — Ollama local models cannot be used at all](https://github.com/netease-youdao/LobsterAI/issues/1635)**  
  Users report failures across `qwen3` and `gemma4`, while the same models work in other clients like CherryStudio. This blocks an important local-model use case. No linked fix PR in this batch.

- **[Issue #1662 — Non-SSE MCP engines cannot be used](https://github.com/netease-youdao/LobsterAI/issues/1662)**  
  Only SSE-based MCP works; other MCP transport types cannot be found/used. This is likely a core MCP integration gap. No linked fix PR in this batch.

- **[Issue #1653 — `groupPolicy` silently overwritten to `allowlist`](https://github.com/netease-youdao/LobsterAI/issues/1653)**  
  Repeated policy overwrites can break user-defined security/access rules. No linked fix PR in this batch.

### Medium severity
- **[Issue #1671 — MD-to-Word conversion stops with `sse response finish reason: full`](https://github.com/netease-youdao/LobsterAI/issues/1671)**  
  Long document conversion is interrupted mid-task, likely due to response length limits. No linked fix PR in this batch.

- **[Issue #1643 — Manual scheduled task save shows “内容未保存” even though saving succeeds](https://github.com/netease-youdao/LobsterAI/issues/1643)**  
  UX bug causing confusing false errors. No linked fix PR in this batch.

Some closed PRs address adjacent stability/security areas: [PR #2503](https://github.com/netease-youdao/LobsterAI/pull/2503) improves text-input editing, [PR #2501](https://github.com/netease-youdao/LobsterAI/pull/2501) fixes skill upgrade overlay rendering, [PR #1661](https://github.com/netease-youdao/LobsterAI/pull/1661) fixes log leakage, and [PR #1663](https://github.com/netease-youdao/LobsterAI/pull/1663) upgrades OpenClaw to fix runtime compatibility.

## 6. Feature Requests & Roadmap Signals

Several items point toward multi-agent orchestration and cross-platform interoperability as the next major product direction:

- **[Issue #1644 — Feature request: markdown-based workflow orchestration](https://github.com/netease-youdao/LobsterAI/issues/1644)**  
  Users want the main agent to organize other agent instances to complete complex tasks. The user explicitly notes that agents are currently unaware of each other unless spawned as OpenClaw subagents.

- **[Issue #2500 — VOKO cross-platform A2A communication](https://github.com/netease-youdao/LobsterAI/issues/2500)**  
  Suggests adding a “cross-platform communication layer” for AI agents across different frameworks and IM channels. This aligns with growing multi-agent interoperability demand.

- **[PR #2504 — OrcaRouter provider integration](https://github.com/netease-youdao/LobsterAI/pull/2504)**  
  Open PR; would add OrcaRouter as a first-class provider alongside OpenRouter. Likely to land in an upcoming release if accepted.

- **[PR #1668 — Per-agent working directory](https://github.com/netease-youdao/LobsterAI/pull/1668)**  
  Already closed/merged; likely to appear in the next release and gives each agent isolated workspace behavior.

- **[PR #2502 / #2505 — DeepSeek Harness engine integration](https://github.com/netease-youdao/LobsterAI/pull/2502)**  
  Closed/merged, with docs still open in [PR #2506](https://github.com/netease-youdao/LobsterAI/pull/2506). This strongly suggests DeepSeek Harness will become a supported runtime in the next version.

## 7. User Feedback Summary

User feedback is primarily in Chinese and focuses on real workflow blockers:

- **Local model support is a top pain point** — [Issue #1635](https://github.com/netease-youdao/LobsterAI/issues/1635) shows users expect parity with other LLM clients for Ollama models.
- **MCP compatibility is incomplete** — [Issue #1662](https://github.com/netease-youdao/LobsterAI/issues/1662) indicates non-SSE MCP engines are missing, limiting tool/plugin usability.
- **Configuration reliability matters** — [Issue #1653](https://github.com/netease-youdao/LobsterAI/issues/1653) and [Issue #1643](https://github.com/netease-youdao/LobsterAI/issues/1643) show frustration with settings that are overwritten or unclear save states.
- **Document conversion reliability is still weak** — [Issue #1671](https://github.com/netease-youdao/LobsterAI/issues/1671) shows real user workflows being cut off in long tasks.
- **Multi-agent visibility is expected** — [Issue #1644](https://github.com/netease-youdao/LobsterAI/issues/1644) indicates users want the main agent to discover and orchestrate existing agents, not only spawn new subagents.

On the positive side, the community is actively contributing UX improvements: 0xFLX contributed multiple Cowork polish PRs, and other contributors fixed context menus, log security, and settings usability. This suggests an engaged user base that is willing to help improve the product.

## 8. Backlog Watch

These items are long-open and still need maintainer attention:

- **[Issue #1635 — Ollama models unusable](https://github.com/netease-youdao/LobsterAI/issues/1635)**  
  Open since 2026-04-12, marked stale. Core local-model functionality is affected.

- **[Issue #1662 — Non-SSE MCP unavailable](https://github.com/netease-youdao/LobsterAI/issues/1662)**  
  Open since 2026-04-14, marked stale. MCP compatibility is a major ecosystem feature.

- **[Issue #1653 — `groupPolicy` overwritten](https://github.com/netease-youdao/LobsterAI/issues/1653)**  
  Open since 2026-04-13, marked stale. Policy persistence is a reliability issue.

- **[Issue #1644 — Markdown workflow orchestration request](https://github.com/netease-youdao/LobsterAI/issues/1644)**  
  Open since 2026-04-12, marked stale. This is a roadmap-significant feature request that deserves formal triage.

- **[Issue #1671 — MD-to-Word conversion interrupted](https://github.com/netease-youdao/LobsterAI/issues/1671)**  
  Open since 2026-04-14, marked stale. Long-running task reliability is important for real-world usage.

- **[PR #1277 — Dependabot: Electron group update](https://github.com/netease-youdao/LobsterAI/pull/1277)**  
  Open since 2026-04-02. It proposes upgrading `electron` from 40.2.1 to 43.4.0 and electron-builder. This is a major dependency update that should be reviewed to avoid accumulating version drift.

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis Project Digest — 2026-08-18

## 1. Today's Overview
Moltis saw steady, maintenance-focused activity over the last 24 hours: 2 issues were updated and both closed, while 9 pull requests were touched — 3 remain open and 6 were merged or closed. No new releases were published. Several long-running feature PRs finally landed, including external-agent model/effort selection, configurable WebUI RPC timeouts, and shadow DOM piercing in browser tooling. Two open heartbeat-related bug fixes and a large new Files library PR suggest active feature and stability work in progress. Overall, the project appears healthy, with CI issues being addressed promptly and a mix of community and dependency-driven contributions.

## 2. Releases
No new releases were recorded for Moltis on 2026-08-18. No changelog, breaking-change, or migration notes are available for this period.

## 3. Project Progress
The following PRs were merged or closed, representing concrete project advancement:

- **PR #1125** — [Support model and effort selection for external agents](https://github.com/moltis-org/moltis/pull/1125): Adds first-class `models` and `efforts` configuration for external-agent providers, plus `/model` UI/registry integration.
- **PR #1103** — [fix(browser): pierce shadow DOM lookups efficiently](https://github.com/moltis-org/moltis/pull/1103): Improves browser snapshot and ref-based lookup paths to handle shadow DOM more efficiently.
- **PR #1130** — [feat: make webui rpc timeout configurable](https://github.com/moltis-org/moltis/pull/1130): Delivers the requested WebUI RPC timeout configuration.
- **PR #1204** — [feat: add MiniMax Code ACP agent](https://github.com/moltis-org/moltis/pull/1204): Adds a new `acp-minimax-code` external-agent kind and default executable detection.
- **PR #1207** / **PR #1087** — Dependency bumps for the cargo group: [wasmtime-wasi/cmov/quinn-proto/serde_with](https://github.com/moltis-org/moltis/pull/1207) and [tar](https://github.com/moltis-org/moltis/pull/1087).

An open PR worth noting for forward progress is **PR #1206** — [Add managed Files library and Settings browser](https://github.com/moltis-org/moltis/pull/1206), which introduces authenticated file APIs, a Finder-style browser, and container mount defaults.

## 4. Community Hot Topics
No comment or reaction counts were provided for current issues/PRs, so activity is inferred from recency, scope, and open state.

The most conversation-worthy topics appear to be:

- **PR #1209** — [fix(gateway): treat heartbeat.update params as a patch, not a whole config](https://github.com/moltis-org/moltis/pull/1209): Addresses a subtle but important config-update bug where missing fields silently reset to defaults.
- **PR #1208** — [fix(cron): honor heartbeat active hours when the scheduler fires](https://github.com/moltis-org/moltis/pull/1208): Fixes `heartbeat.active_hours` being documented but never actually used by the scheduler.
- **PR #1206** — [Add managed Files library and Settings browser](https://github.com/moltis-org/moltis/pull/1206): A broad new feature touching file storage, UI, and container mount behavior.

These PRs reveal underlying user needs around predictable configuration semantics, reliable scheduled operation, and first-class file management inside Moltis.

## 5. Bugs & Stability
Ranked by severity:

1. **High — `heartbeat.update` config overwrite**  
   [PR #1209](https://github.com/moltis-org/moltis/pull/1209) fixes a bug where partial `heartbeat.update` params deserialize into a full `HeartbeatConfig` with defaults, clobbering existing config in both state and `moltis.toml`. This is a data-corruption-adjacent bug for user config, and a fix is open.

2. **Medium — `heartbeat.active_hours` ignored**  
   [PR #1208](https://github.com/moltis-org/moltis/pull/1208) fixes the scheduler running heartbeats outside configured active hours. The helper existed but was never called by the cron pipeline.

3. **Low — CI format gate failure**  
   [Issue #1202](https://github.com/moltis-org/moltis/issues/1202) reported two files exceeding the 1500-line limit, breaking the `Format` job on `main`. The issue is now closed, indicating the formatting problem was resolved.

## 6. Feature Requests & Roadmap Signals
- **Configurable WebUI RPC timeout** — Requested in [Issue #1127](https://github.com/moltis-org/moltis/issues/1127) and implemented by [PR #1130](https://github.com/moltis-org/moltis/pull/1130). This is a strong candidate for the next release.
- **External-agent model and effort selection** — [PR #1125](https://github.com/moltis-org/moltis/pull/1125) adds configuration and UI support; likely to ship soon.
- **MiniMax Code ACP agent support** — [PR #1204](https://github.com/moltis-org/moltis/pull/1204) expands the external-agent registry, indicating continued investment in third-party agent integrations.
- **Managed Files library and Settings browser** — [PR #1206](https://github.com/moltis-org/moltis/pull/1206) is open and suggests a major upcoming feature around persistent file storage, authenticated APIs, and container-friendly defaults.

## 7. User Feedback Summary
Contributors and users are expressing practical, operational concerns:

- **Heartbeat config surprises**: Users of `heartbeat.update` are likely hitting unintended resets of unset fields to defaults; the fix in #1209 directly targets this pain point.
- **Scheduled heartbeat behavior**: `heartbeat.active_hours` being ignored undermines user expectations around scheduled activity windows; #1208 addresses that mismatch.
- **Agent configuration flexibility**: The effort around #1125 shows demand for controlling model and effort levels per external agent, not just built-in providers.
- **Infrastructure friction**: Large generated files breaking CI (#1202) is a minor annoyance but highlights repository maintainability concerns.
- **Positive signals**: Multiple contributors are actively submitting fixes and features, and long-running PRs like #1125 and #1103 were ultimately closed/merged, suggesting maintainers are clearing backlog.

## 8. Backlog Watch
No open issues older than 24h appear to be waiting for maintainer attention. The oldest recently updated PRs were eventually closed/merged:

- **PR #1103** (created 2026-06-04) and **PR #1125** (created 2026-06-15) both took roughly two months to reach closure. This suggests review cycles can be long, though they do complete.

Maintainers should keep an eye on the currently open PRs to avoid a similar backlog buildup:

- [PR #1206 — Managed Files library and Settings browser](https://github.com/moltis-org/moltis/pull/1206)
- [PR #1208 — Honor heartbeat active hours](https://github.com/moltis-org/moltis/pull/1208)
- [PR #1209 — Treat heartbeat.update as a patch](https://github.com/moltis-org/moltis/pull/1209)

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

## 1. Today's Overview

CoPaw/QwenPaw shows a high-activity day: **14 issues were updated in the last 24h** (8 open/active, 6 closed), and **35 PRs were updated** (13 open, 22 merged/closed). No new release was cut, meaning the recent batch of fixes and features is still accumulating on the main branch.

The PR activity is concentrated in Console UX fixes, token/media accounting, PawApp/plugin lifecycle, and provider integration. Several first-time-contributor PRs are present, indicating a healthy external contributor pipeline. Meanwhile, open bugs around multi-session identity isolation, media URL handling, and plugin hot-reload remain the main stability watch items.

---

## 2. Releases

**No new releases in the last 24h.**

Current user-visible versions referenced in recent issues: `v2.1.0` and Docker `v2.0.0post3`. No changelog, breaking-change, or migration notes are available for this digest period.

---

## 3. Project Progress

22 PRs moved to **merged/closed** in the last 24h. Highlights from the top-updated set:

- **#6940** — `feat(pawapp): add native DataPaw app runtime and durable analysis workspace`  
  A major plugin/PawApp feature: native DataPaw runtime plus persistent analysis workspace.  
  https://github.com/agentscope-ai/QwenPaw/pull/6940

- **#6817** — `feat: integrate AnySearch web search (SearchProvider + MCP)`  
  Closed/merged external integration; a revised version is also open as #7081.  
  https://github.com/agentscope-ai/QwenPaw/pull/6817

- **#7036** — `feat(console): add media download controls`  
  Unified download behavior for audio/media attachments.  
  https://github.com/agentscope-ai/QwenPaw/pull/7036

- **#7017** — `fix(console): open newly installed PawApps without reload`  
  Removes the manual-page-refresh requirement after installing a PawApp.  
  https://github.com/agentscope-ai/QwenPaw/pull/7017

- **#6975** — `fix(console): update context-usage ring after compact`  
  Fixes stale context-window display after `/compact`.  
  https://github.com/agentscope-ai/QwenPaw/pull/6975

- **#6968** — `fix(token-usage): stop counting image base64 as text tokens`  
  Fixes inflated token usage caused by base64 image data.  
  https://github.com/agentscope-ai/QwenPaw/pull/6968

- **#5151** — `fix(GitPanel): fix tabs styles not applied due to incorrect class prefix`  
  Long-lived fix for GitPanel styling with the custom `qwenpaw` prefix.  
  https://github.com/agentscope-ai/QwenPaw/pull/5151

- **#6981** — `feat(console): remove approval hints from i18n placeholders`  
  Cleans up chat-input placeholders across all 7 locale files.  
  https://github.com/agentscope-ai/QwenPaw/pull/6981

- **#7083** — `feat(console): compact background task list and add scroll hint`  
  Console background-task panel no longer pushes chat input down.  
  https://github.com/agentscope-ai/QwenPaw/pull/7083

Notable **open** PRs advancing features:

- **#7087** — `fix(agents): localize remote media URLs client-side before model requests`  
  Likely fixes issues where model backends cannot fetch hotlink-protected remote images.  
  https://github.com/agentscope-ai/QwenPaw/pull/7087

- **#7089** — `ci(datapaw): add a standalone version-driven release pipeline`  
  Gives DataPaw its own plugin-CDN release cadence.  
  https://github.com/agentscope-ai/QwenPaw/pull/7089

- **#7086** — `fix(console): unify language options between settings gear and dropdown`  
  First-time contributor fix for inconsistent language options.  
  https://github.com/agentscope-ai/QwenPaw/pull/7086

- **#6302** — `feat: unify provider discovery, model metadata, routing, and agent controls`  
  Large architecture PR, still open.  
  https://github.com/agentscope-ai/QwenPaw/pull/6302

---

## 4. Community Hot Topics

- **#6405** — 7 comments, closed question  
  **MCP tool “Tool not found” after upgrading to 2.0**  
  Users report that `[mcp-key]__[tool_name]` naming is present but tools still fail to resolve.  
  Underlying need: clearer MCP tool registration/error diagnostics after upgrades.  
  https://github.com/agentscope-ai/QwenPaw/issues/6405

- **#7011** — 6 comments, open bug  
  **Console stop request can cancel an active Feishu session under multiple UI sessions**  
  Session identity values crossed between two UI sessions, causing an active Feishu conversation to be cancelled.  
  https://github.com/agentscope-ai/QwenPaw/issues/7011

- **#7085** — 3 comments, open enhancement  
  **按频道独立配置模型 / Per-channel model configuration**  
  Users want different models per channel (e.g., DingTalk → `gpt-4o`, WeChat → `qwen-max`, Console → local `llama.cpp`).  
  https://github.com/agentscope-ai/QwenPaw/issues/7085

- **#7063** — 3 comments, closed invalid  
  **Agent crashes when executing a tool call**  
  Reported crash around `_execute_tool_call`; closed as invalid, but reveals confusion around async generator vs. coroutine tool APIs.  
  https://github.com/agentscope-ai/QwenPaw/issues/7063

- **#7088** — 2 comments, closed bug  
  **OneBot channel passes short-lived QQ image URLs to the model**  
  Expired signed `rkey` causes HTTP 400 and poisons session history.  
  https://github.com/agentscope-ai/QwenPaw/issues/7088

- **#6925** — 2 comments, open enhancement  
  **智能体协作希望在一个会话窗口里 / Agent collaboration in one session window**  
  Collaboration currently creates a new session each time and forces manual agent switching.  
  https://github.com/agentscope-ai/QwenPaw/issues/6925

---

## 5. Bugs & Stability

Ranked by severity / user impact:

1. **#7011 — Multi-UI session identity collision cancels active Feishu session**  
   High severity, open, no linked fix PR yet.  
   https://github.com/agentscope-ai/QwenPaw/issues/7011

2. **#7082 — `_StructuredOutputDynamicClass is not fully defined` during agent/toolkit initialization**  
   Blocks Console channel initialization with Pydantic model errors. Open.  
   https://github.com/agentscope-ai/QwenPaw/issues/7082

3. **#7088 — Expired signed QQ image URLs cause 400 and poisoned session history**  
   Closed; open PR **#7087** appears to be the intended fix by localizing remote media client-side.  
   Issue: https://github.com/agentscope-ai/QwenPaw/issues/7088  
   Fix PR: https://github.com/agentscope-ai/QwenPaw/pull/7087

4. **#7077 — Plugin runtime hooks silently lost after workspace reload / hot-install**  
   Closed, but no obvious fix PR surfaced. Important for plugin authors.  
   https://github.com/agentscope-ai/QwenPaw/issues/7077

5. **#7051 — Console image attachments lost/blank after session reload**  
   Closed bug; backend serves data URLs but frontend shows broken thumbnails.  
   https://github.com/agentscope-ai/QwenPaw/issues/7051

6. **#7084 — With only one historical conversation, opening it from a new chat does nothing**  
   Open UI regression in conversation-history navigation.  
   https://github.com/agentscope-ai/QwenPaw/issues/7084

7. **#7048 — `qwenpaw cron update --text` returns success but prompt is not updated**  
   Closed as invalid, but still a confusing CLI behavior for agent-type cron jobs.  
   https://github.com/agentscope-ai/QwenPaw/issues/7048

8. **#7076 — `qwenpaw-creator` LLM model configuration returns 404 on v2.1.0**  
   Open, configuration/setup blocker for creator users.  
   https://github.com/agentscope-ai/QwenPaw/issues/7076

Also noted: **#7063** was closed invalid, but the “agent crashes on tool call” framing may indicate a documentation/API-expectation gap.

---

## 6. Feature Requests & Roadmap Signals

Strong roadmap signals from the last 24h:

- **#7085 — Per-channel model configuration**  
  Currently models are global or agent-level. Channel-scoped model selection is a recurring request and likely a near-term roadmap candidate.  
  https://github.com/agentscope-ai/QwenPaw/issues/7085

- **#7075 — Detailed scheduled-task run information**  
  Users want start time, duration, end time, and result status for cron/scheduled tasks.  
  https://github.com/agentscope-ai/QwenPaw/issues/7075

- **#6925 — Agent collaboration in a single session window**  
  Collaboration UX is fragmented by per-session creation and agent switching.  
  https://github.com/agentscope-ai/QwenPaw/issues/6925

- **#7079 / #7080 — Pluggable PowerContext long-term memory backend**  
  Feature request plus first-time-contributor implementation using `BaseMemoryManager`.  
  Issue: https://github.com/agentscope-ai/QwenPaw/issues/7079  
  PR: https://github.com/agentscope-ai/QwenPaw/pull/7080

- **#7081 — AnySearch web search integration (SearchProvider + MCP)**  
  External contributor integrating AnySearch as a first-class web-search backend.  
  https://github.com/agentscope-ai/QwenPaw/pull/7081

- **#6940 / #7089 — DataPaw native app runtime and standalone release pipeline**  
  Signals continued investment in plugin/PawApp distribution independent from the core release cycle.  
  https://github.com/agentscope-ai/QwenPaw/pull/6940  
  https://github.com/agentscope-ai/QwenPaw/pull/7089

- **#6302 / #6976 / #6719 — Larger open feature PRs**  
  Provider/model unification, session-scoped multi project directories, and persistent workspace artifact cards are likely candidates for the next minor release if they continue to receive review.  
  https://github.com/agentscope-ai/QwenPaw/pull/6302  
  https://github.com/agentscope-ai/QwenPaw/pull/6976  
  https://github.com/agentscope-ai/QwenPaw/pull/6719

---

## 7. User Feedback Summary

Real pain points and use cases from the last 24h:

- **Upgrade friction:** Users upgrading to 2.x report MCP tools suddenly not resolving, especially in Docker deployments (#6405).
- **Multi-platform deployment needs:** Chinese enterprise chat platforms (Feishu, DingTalk, WeChat/QQ) are heavily used. Users want per-channel model routing and reliable media handling for short-lived URLs (#7085, #7088, #7011).
- **Session/history reliability concerns:** Image attachments disappear after reload, and single-history-session navigation breaks in Console (#7051, #7084).
- **Agent collaboration UX:** Multi-agent cooperation is powerful but difficult to follow because each collaboration creates a new session and requires manual agent switching (#6925).
- **Cron observability:** Scheduled tasks are a black box until they fail; users want detailed execution records (#7075).
- **Positive signals:** Several first-time-contributor PRs are being opened and closed, and maintainers are actively triaging invalid or duplicate issues. The extension ecosystem — plugin hooks, memory backends, MCP providers — is attracting real community experimentation.

---

## 8. Backlog Watch

Items needing maintainer attention:

- **#6302 — Open PR: unify provider discovery, model metadata, routing, and agent controls**  
  Open since **2026-07-21**, large architectural change. This could unblock several model-related feature requests, including #7085.  
  https://github.com/agentscope-ai/QwenPaw/pull/6302

- **#6515 — Open PR: add Volcengine Agent Plan and Xiaomi MiMo V2.5 API as built-in providers**  
  Open since **2026-07-28**, no visible comments in the digest window.  
  https://github.com/agentscope-ai/QwenPaw/pull/6515

- **#6719 — Open PR: persistent workspace artifact cards**  
  Open since **2026-08-05**, feature-complete-looking but waiting on review.  
  https://github.com/agentscope-ai/QwenPaw/pull/6719

- **#6976 — Open PR: session-scoped multi project directories**  
  Open since **2026-08-13**, significant workflow enhancement.  
  https://github.com/agentscope-ai/QwenPaw/pull/6976

- **#6986 — Open PR: fix antivirus software blocking issues**  
  Open since **2026-08-13**, relevant for Windows/sandbox users.  
  https://github.com/agentscope-ai/QwenPaw/pull/6986

- **#7011 — Open bug: Console stop request cancels active Feishu session**  
  Active, 6 comments, no linked fix yet. This is the most important open stability issue in the current digest.  
  https://github.com/agentscope-ai/QwenPaw/issues/7011

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-08-18

## 1. Today's Overview

ZeroClaw is in a period of intense, RFC-driven architecture and security hardening, with 50 issues and 50 PRs updated in the last 24 hours. Maintainer velocity is high: 16 PRs merged/closed versus 34 still open, headlined by five security fixes and a batch of cross-platform CI improvements. No release was published; the project sits between 0.8.4 and the v0.9.0 auth/security/gateway milestone coordinated in [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432). Community discussion remains concentrated on accepted RFCs — Chat Completions compatibility, goal-mode execution, shell-command policy, and runtime-owned sessions — indicating a broad but deliberately governed roadmap. The most prominent stability gap is Windows support, which is now being addressed by newly merged scheduled macOS/Windows test workflows.

## 2. Releases

No releases published in this window.

## 3. Project Progress

Sixteen PRs were merged or closed in this window. Security fixes dominate:

- **[#9973](https://github.com/zeroclaw-labs/zeroclaw/pull/9973)** — `fix(providers): keep Gemini API keys out of URLs` (P1). API keys moved from request URLs to the documented `x-goog-api-key` header; closes a credential-exposure vector.
- **[#10000](https://github.com/zeroclaw-labs/zeroclaw/pull/10000)** — `fix(channels): bound QQ and Mattermost downloads` (P1). Shared bounded HTTP reader enforces QQ's 10 MiB and Mattermost's 25 MiB limits; closes #9998-class unbounded-download risk.
- **[#9996](https://github.com/zeroclaw-labs/zeroclaw/pull/9996)** — `fix(security): make action budget accounting atomic`. Reserves sender-scoped capacity before the side-effect boundary; fixes both [#9849](https://github.com/zeroclaw-labs/zeroclaw/issues/9849) (non-atomic `RateLimitedTool` check) and [#9594](https://github.com/zeroclaw-labs/zeroclaw/issues/9594) (double action-budget charge).
- **[#9993](https://github.com/zeroclaw-labs/zeroclaw/pull/9993)** — `fix(email): stop implicit attachment file reads`. Outbound MIME attachments now only use in-memory `MediaAttachment.data`.
- **[#9612](https://github.com/zeroclaw-labs/zeroclaw/pull/9612)** — `fix(channels): tie WhatsApp Cloud approval token to a guard`, preventing orphaned bearer credentials on early exit.

Feature/correctness advances:

- **[#9765](https://github.com/zeroclaw-labs/zeroclaw/pull/9765)** — SOP definitions now load from the shared workspace instead of `data_dir`.
- **[#9544](https://github.com/zeroclaw-labs/zeroclaw/pull/9544)** — Delegated targets now honor the canonical session provider builder, so configured aliases, routes, retries, and fallbacks are respected.
- **[#9547](https://github.com/zeroclaw-labs/zeroclaw/pull/9547)** — CPAL upgraded 0.15.3 → 0.18.1 with Voice Wake migrated to unified APIs.

CI/test infrastructure:

- **[#9398](https://github.com/zeroclaw-labs/zeroclaw/pull/9398)** — Adds scheduled macOS and Windows test workflows (nightly + manual).
- **[#10039](https://github.com/zeroclaw-labs/zeroclaw/pull/10039)** — Shared Clippy command runner (`scripts/ci/run_clippy.sh`) extracted; closes [#7884](https://github.com/zeroclaw-labs/zeroclaw/issues/7884).
- **[#10043](https://github.com/zeroclaw-labs/zeroclaw/pull/10043)** — Removes duplicate architecture-test invocations from Lint.
- **[#10010](https://github.com/zeroclaw-labs/zeroclaw/pull/10010)** — Cron custom-shell test avoids the ETXTBSY race via a per-test symlink.

## 4. Community Hot Topics

The most active issues (by comment count, all updated in the last 24h) are accepted or in-progress RFCs rather than contentious disputes:

- **[#6808](https://github.com/zeroclaw-labs/zeroclaw/issues/6808)** — RFC: Work Lanes, Board Automation, and Label Cleanup (23 comments, Rev 26). Governance tracker reflecting maintainer process overload and the desire to automate routing.
- **[#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)** — RFC: ZeroClaw Chat Completions profile (23 comments, risk:high). Strong external-ecosystem demand: Open WebUI, LobeChat, Continue.dev, Aider, LangChain, OpenAI SDK.
- **[#8303](https://github.com/zeroclaw-labs/zeroclaw/issues/8303)** — RFC: Goal mode v1 — bounded foreground Matrix work (22 comments, 1 👍). Durable multi-turn user objectives.
- **[#7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)** — RFC: per-execution confirmation tier for high-risk shell commands (20 comments). Claude Code-style allow/ask/deny policy.
- **[#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)** — RFC: Runtime-owned conversation sessions and transport adapters (19 comments).
- **[#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488)** — RFC: Unified attachment architecture (18 comments).
- **[#7141](https://github.com/zeroclaw-labs/zeroclaw/issues/7141)** — RFC: Pluggable inbound authentication and canonical principals (16 comments).
- **[#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462)** — Bug: 74 test failures on Windows (16 comments; the most-commented bug).

Underlying needs: (a) interoperability so existing LLM clients can drive ZeroClaw agents; (b) security hardening — auth, shell policy, credential boundaries, bounded downloads; (c) sustainable governance — RFC process reform ([#9496](https://github.com/zeroclaw-labs/zeroclaw/issues/9496)) and a maintainer decision queue ([#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)); (d) cross-platform maturity (Windows/macOS CI).

## 5. Bugs & Stability

Ranked by severity (all items updated in this window):

- **[#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) [P1, open]** — 74 test failures on Windows 11 (Chinese console, code page 936): Unix-only commands, path semantics, console encoding. Mitigation merged today ([#9398](https://github.com/zeroclaw-labs/zeroclaw/pull/9398) adds scheduled Windows/macOS runs); the underlying fixes remain in progress.
- **[#9314](https://github.com/zeroclaw-labs/zeroclaw/pull/9314) [P1, open PR]** — Telegram advances long-poll offset before download/transcription/delivery, causing permanent update loss on transient failure. Fix pending review.
- **[#10023](https://github.com/zeroclaw-labs/zeroclaw/issues/10023) [P2, open]** — Reliable-provider failure logs name the requested model instead of the pinned fallback that actually served; misleading diagnostics.
- **[#9056](https://github.com/zeroclaw-labs/zeroclaw/pull/9056) [P2, open, stale]** — Provider failures surface as a generic `All model_providers/models failed` envelope; PR needs author action after being marked stale.
- **[#10038](https://github.com/zeroclaw-labs/zeroclaw/pull/10038) [P2, open PR]** — `POST /api/cron` accepts invalid `session_target` values with HTTP 200 and persists them.
- **[#10021](https://github.com/zeroclaw-labs/zeroclaw/pull/10021) [P2, open PR]** — Target thinking policy not applied to independent delegates.
- **[#10003](https://github.com/zeroclaw-labs/zeroclaw/pull/10003) [P2, open PR]** — Reliable provider rejected-attempt accounting is not exact across retries/failover.
- **[#10011](https://github.com/zeroclaw-labs/zeroclaw/issues/10011) [P2, open task]** — Daemon heartbeat test writes and executes a runtime file; needs restructuring to avoid post-start writes.

Fixed in this window: unbounded QQ/Mattermost downloads ([#10000](https://github.com/zeroclaw-labs/zeroclaw/pull/10000)), Gemini key URL exposure ([#9973](https://github.com/zeroclaw-labs/zeroclaw/pull/9973)), non-atomic budget check + double charge ([#9996](https://github.com/zeroclaw-labs/zeroclaw/pull/9996), closing [#9849](https://github.com/zeroclaw-labs/zeroclaw/issues/9849) and [#9594](https://github.com/zeroclaw-labs/zeroclaw/issues/9594)), implicit email file reads ([#9993](https://github.com/zeroclaw-labs/zeroclaw/pull/9993)), and orphaned WhatsApp approval tokens ([#9612](https://github.com/zeroclaw-labs/zeroclaw/pull/9612)). No crashes or regressions were newly reported.

## 6. Feature Requests & Roadmap Signals

Accepted RFCs in rollout are the clearest v0.9.0 signals:

- **[#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)** — Chat Completions profile: likely the next major user-facing feature, opening ZeroClaw to the OpenAI-protocol tool ecosystem.
- **[#7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)** — Shell command allow/ask/deny confirmation policy (Claude Code-style).
- **[#7141](https://github.com/zeroclaw-labs/zeroclaw/issues/7141)** + **[#7142](https://github.com/zeroclaw-labs/zeroclaw/issues/7142)** — Pluggable inbound auth and a runtime-owned security decision pipeline; core of the v0.9.0 auth milestone.
- **[#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)** / **[#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488)** — Runtime-owned sessions and unified attachment architecture; prerequisites for channel parity and web chat parity.
- **[#8303](https://github.com/zeroclaw-labs/zeroclaw/issues/8303)** — Goal mode v1 for bounded, durable multi-turn objectives.
- **[#7100](https://github.com/zeroclaw-labs/zeroclaw/issues/7100)** — Per-model capability/context-window config, fixing vision and context-budget misreporting.
- **[#6971](https://github.com/zeroclaw-labs/zeroclaw/issues/6971)** — Security posture, credential boundaries, and universal ingress policy.
- **[#9621](https://github.com/zeroclaw-labs/zeroclaw/issues/9621)** — Staged opt-in product telemetry so maintainers can measure real feature usage.
- **[#6165](https://github.com/zeroclaw-labs/zeroclaw/issues/6165)** — Lighter core by moving long-tail integrations out of the default build.

New provider capability is also in flight: native Hailo-Ollama support via [#9109](https://github.com/zeroclaw-labs/zeroclaw/pull/9109) (open PR).

Prediction: v0.9.0 will center on auth/security (pluggable auth, security pipeline, shell policy), an OpenAI-compatible ingress surface, and the session/attachment architecture that underpins multi-channel consistency.

## 7. User Feedback Summary

- **Windows users**: explicit pain — 74 failing tests, Unix-only commands, path semantics, and console encoding issues; CI previously only ran on Linux. Mitigation merged today (scheduled macOS/Windows tests), though the actual Windows fixes remain open.
- **Operator debuggability**: complaints that failure logs name the wrong model ([#10023](https://github.com/zeroclaw-labs/zeroclaw/issues/10023)) and that provider errors are generic envelopes ([#9056](https://github.com/zeroclaw-labs/zeroclaw/pull/9056)) make production troubleshooting harder.
- **Security expectations**: the community (and maintainers) are pushing zero-trust defaults — empty WhatsApp `allowed_groups` must deny all ([#9397](https://github.com/zeroclaw-labs/zeroclaw/issues/9397)), shell commands need per-execution confirmation ([#7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)), credentials must be kept out of URLs and logs ([#9973](https://github.com/zeroclaw-labs/zeroclaw/pull/9973), [#6971](https://github.com/zeroclaw-labs/zeroclaw/issues/6971)).
- **Ecosystem demand**: requesters want to use familiar OpenAI-compatible clients (Open WebUI, LobeChat, Continue.dev, Aider, LangChain) against ZeroClaw agents ([#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)).
- **Process fatigue**: maintainer-authored RFCs call out the RFC process as slow and cumbersome ([#9496](https://github.com/zeroclaw-labs/zeroclaw/issues/9496)) and request board automation ([#6808](https://github.com/zeroclaw-labs/zeroclaw/issues/6808)) — an internal, but real, satisfaction signal.

## 8. Backlog Watch

- **[#9056](https://github.com/zeroclaw-labs/zeroclaw/pull/9056) [P2, stale]** — Provider failure diagnostics PR open since 2026-07-14, marked `needs-author-action` and `stale-candidate`; needs revival or closure.
- **[#9109](https://github.com/zeroclaw-labs/zeroclaw/pull/9109) [P2, XL]** — Hailo-Ollama native provider, open since 2026-07-17, `needs-author-action`; large, important feature stalled.
- **[#6165](https://github.com/zeroclaw-labs/zeroclaw/issues/6165) [RFC, needs-maintainer-review]** — "Lighter ZeroClaw core" open since 2026-04-27; oldest high-traffic RFC still awaiting a maintainer decision.
- **[#6653](https://github.com/zeroclaw-labs/zeroclaw/issues/6653) [P3]** — Host-architecture policy for emulated installs, open since 2026-05-14 with no maintainer response.
- **[#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)** / **[#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488)** — Session and attachment RFCs, both flagged `needs-author-action` + `needs-maintainer-review`; the coupled pair is gating channel/architecture work.
- **[#9808](https://github.com/zeroclaw-labs/zeroclaw/pull/9808) [P3, large, risk:high]** — Dependabot rust-all bump with 46 updates; needs review to avoid dependency drift.
- **[#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)** — The maintainer decision queue tracker itself is the best barometer: a visible backlog of RFCs and design issues awaiting maintainer attention. Monitor this issue as a project-health proxy.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*