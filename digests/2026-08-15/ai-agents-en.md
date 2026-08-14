# OpenClaw Ecosystem Digest 2026-08-15

> Issues: 500 | PRs: 500 | Projects covered: 12 | Generated: 2026-08-14 23:14 UTC

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

# OpenClaw Project Digest — 2026-08-15

Data window: 2026-08-14 (last 24h)

## 1. Today's Overview

OpenClaw remains very active, with 500 issues and 500 PRs updated in the last 24 hours, but the project is under sustained stability pressure. Of those, 488 issues are open/active and 12 were closed, while PRs show 403 open and 97 merged/closed. No new releases were published in this window. The highest-engagement threads are mostly long-running reliability bugs — silent reply failures, gateway memory leaks, and channel-specific regressions — rather than new feature discussion. Community contribution volume is strong, but many P0/P1 items are still waiting on maintainer review or product decisions.

## 2. Releases

No new releases were published in the last 24 hours. There are no changelog, breaking-change, or migration notes to report.

## 3. Project Progress

- Aggregate data shows **97 PRs merged/closed** and **12 issues closed** in the last 24 hours.
- The visible top-PR sample includes one closed PR: [#123878 — fix(security): skip unused default workspace in explicit fleets](https://github.com/openclaw/openclaw/pull/123878).
- Notable open PRs close to landing or ready for maintainer review:
  - [#123582 — improve(ui): unify sidebar account footer and identity menu](https://github.com/openclaw/openclaw/pull/123582)
  - [#123276 — Start new sessions with folder group defaults](https://github.com/openclaw/openclaw/pull/123276)
  - [#123743 — feat: cloud workers for the codex runtime (remote-exec placements)](https://github.com/openclaw/openclaw/pull/123743)
  - [#112811 — feat(msteams): support multiple bot accounts](https://github.com/openclaw/openclaw/pull/112811)
  - [#120900 — feat(ui): review install policy warnings](https://github.com/openclaw/openclaw/pull/120900)
  - [#121195 — fix(agents): settle yielded requester completions exactly once](https://github.com/openclaw/openclaw/pull/121195)
  - [#121204 — fix(discord): keep stale ambient backlog from starving live mentions after gateway recovery](https://github.com/openclaw/openclaw/pull/121204)

## 4. Community Hot Topics

The most active issue threads by comment count show clear user focus on delivery reliability and regressions:

- [#121058 — Silent reply failures still recurring after #116277 closed — no queued reply payload](https://github.com/openclaw/openclaw/issues/121058) — **94 comments**. This is the dominant community thread. Users report the previous fix did not fully resolve silent reply loss, and monitoring cron jobs still capture new occurrences.
- [#91588 — Critical: Gateway Memory Leak — RSS grows from 350MB to 15.5GB, OOM crashes](https://github.com/openclaw/openclaw/issues/91588) — **24 comments, 1 👍**. A P0 stability issue causing repeated crash-loop restarts.
- [#121953 — Cron agent turns stall on DeepSeek — `[cron:` prefix deprioritized](https://github.com/openclaw/openclaw/issues/121953) — **20 comments**. Provider-specific API behavior stalls cron jobs for tens of seconds to minutes.
- [#96834 — WhatsApp 1:1: inbound image wedges main lane ~3min before processing](https://github.com/openclaw/openclaw/issues/96834) — **15 comments, 1 👍**. Multimodal inbound handling blocks an active chat lane.
- [#62505 — Coding Agent never completes anything (worked in 2026.4.2 and earlier)](https://github.com/openclaw/openclaw/issues/62505) — **15 comments, 1 👍**. A long-standing regression that severely affects coding-agent users.
- [#108435 — Update to OpenClaw 2026.7.1: gateway fails to start](https://github.com/openclaw/openclaw/issues/108435) — **14 comments, 3 👍**. Release-blocking regression for self-hosted gateway users.

Other reaction-heavy threads: [#10687 — dynamic model discovery for OpenRouter](https://github.com/openclaw/openclaw/issues/10687) (**3 👍**), [#81061 — pre-routing inbound message hook](https://github.com/openclaw/openclaw/issues/81061) (**3 👍**), and [#73537 — production-readiness stability label](https://github.com/openclaw/openclaw/issues/73537) (**2 👍**).

## 5. Bugs & Stability

### Critical / P0
- [#91588 — Gateway memory leak grows RSS to 15.5GB, causing OOM crash loops](https://github.com/openclaw/openclaw/issues/91588) — open since June; no visible fix PR.
- [#119270 — File tools strip leading `@` from destination paths, silently writing/deleting the wrong file](https://github.com/openclaw/openclaw/issues/119270) — data-loss risk; no visible fix PR.
- [#108435 — Gateway fails to start after 2026.7.1 update](https://github.com/openclaw/openclaw/issues/108435) — affects systemd, Ollama, and manual launch paths.
- [#121058 — Silent reply failures still recurring](https://github.com/openclaw/openclaw/issues/121058) — critical message-delivery reliability issue with 94 comments.

### High-severity P1 regressions/stability issues
- [#62505 — Coding Agent never completes anything](https://github.com/openclaw/openclaw/issues/62505) — regression since 2026.4.2.
- [#121953 — Cron agent stalls on DeepSeek due to `[cron:` prefix](https://github.com/openclaw/openclaw/issues/121953)
- [#96834 — WhatsApp image wedges message lane](https://github.com/openclaw/openclaw/issues/96834)
- [#87109 — Gateway heap grows to 1073MB+ at idle; cron jobs fail silently](https://github.com/openclaw/openclaw/issues/87109)
- [#120563 — Conversation history not sent to custom/Ollama providers](https://github.com/openclaw/openclaw/issues/120563)
- [#94939 — 6.x state migration leaves conversation-store SQLite empty, breaks MS Teams proactive sends](https://github.com/openclaw/openclaw/issues/94939)
- [#91144 — Windows Scheduled Task gateway does not stay running](https://github.com/openclaw/openclaw/issues/91144)
- [#97616 — Hook/tool child process zombie accumulation](https://github.com/openclaw/openclaw/issues/97616)
- [#99910 — Memory dreaming pegs the gateway event loop for ~10 minutes](https://github.com/openclaw/openclaw/issues/99910)
- [#107244 — WhatsApp group messages never reach inbound handling](https://github.com/openclaw/openclaw/issues/107244)
- [#92241 — Stale module paths after update/rollback silently drop inbound messages](https://github.com/openclaw/openclaw/issues/92241)
- [#86215 — Codex OAuth refresh failures can wedge an agent for hours](https://github.com/openclaw/openclaw/issues/86215)

### Fix PRs in flight
Several stability-oriented PRs are in the open queue, including:
- [#121195 — settle yielded requester completions exactly once](https://github.com/openclaw/openclaw/pull/121195)
- [#121167 — complete only sessions confirmed inactive (Beam)](https://github.com/openclaw/openclaw/pull/121167)
- [#121186 — retry failed terminal mirror uploads (Beam)](https://github.com/openclaw/openclaw/pull/121186)
- [#121204 — Discord: stop stale backlog from starving live mentions](https://github.com/openclaw/openclaw/pull/121204)
- [#121022 — await config runtime application](https://github.com/openclaw/openclaw/pull/121022)
- [#121081 — reject stale worker bundles before tunnel start](https://github.com/openclaw/openclaw/pull/121081)
- [#121172 — keep sessions with unreadable transcripts during cleanup --fix-missing](https://github.com/openclaw/openclaw/pull/121172)
- [#120781 — stop doctor archiving live transcripts as orphans](https://github.com/openclaw/openclaw/pull/120781)

## 6. Feature Requests & Roadmap Signals

Strong user demand continues around channel reliability, context handling, and operational visibility:

- [#50093 — WhatsApp: Backfill missed messages after reconnection](https://github.com/openclaw/openclaw/issues/50093)
- [#96975 — Isolate subagent completion from parent context](https://github.com/openclaw/openclaw/issues/96975)
- [#10687 — Fully dynamic model discovery for OpenRouter](https://github.com/openclaw/openclaw/issues/10687)
- [#71142 — Configurable upload size limit for Control UI](https://github.com/openclaw/openclaw/issues/71142)
- [#75947 — UI quality update based on UX scoring](https://github.com/openclaw/openclaw/issues/75947)
- [#13219 — Per-model usage logging for cost tracking](https://github.com/openclaw/openclaw/issues/13219)
- [#81061 — Pre-routing inbound message hook for channel bridging/proxying](https://github.com/openclaw/openclaw/issues/81061)
- [#88154 — Slack Modal support for interactive workflows](https://github.com/openclaw/openclaw/issues/88154)
- [#17840 — Opt-in reaction-triggered agent turns](https://github.com/openclaw/openclaw/issues/17840)
- [#68920 — HTTP /v1/chat/completions needs lightContext/voice mode](https://github.com/openclaw/openclaw/issues/68920)
- [#44395 — Heading-aware chunking + entity extraction for memory search](https://github.com/openclaw/openclaw/issues/44395)
- [#73537 — Production-readiness stability label on releases](https://github.com/openclaw/openclaw/issues/73537)

**Likely next-version signals:** The next release will probably prioritize reliability fixes already in PR review, such as config hot-reload correctness, Discord/Beam delivery fixes, and Codex/cloud-worker enablement. Larger feature PRs — multi-account Microsoft Teams ([#112811](https://github.com/openclaw/openclaw/pull/112811)) and Twilio RCS support ([#105025](https://github.com/openclaw/openclaw/pull/105025)) — are further out due to compatibility/security review.

## 7. User Feedback Summary

- **Positive:** [#73537](https://github.com/openclaw/openclaw/issues/73537) contains a strong endorsement: the project has “genuinely become part of our daily workflow” for family/business automation, Home Assistant control, and Telegram integration. The user asks for a clearer production-readiness signal on releases.
- **Frustration with regressions:** [#62505](https://github.com/openclaw/openclaw/issues/62505) reports a coding agent that now “just doesnt do _anything_ apart from vague status updates and then apologies for the vagueness.” This reflects significant trust loss from a regression that worked in 2026.4.2.
- **Delivery reliability pain:** [#121058](https://github.com/openclaw/openclaw/issues/121058) shows users actively monitoring silent reply failures with cron jobs; the problem persists after a supposedly closing fix.
- **UX dissatisfaction:** [#75947](https://github.com/openclaw/openclaw/issues/75947) describes the UI as “hard to navigate and understand… feels dense and looks too much like AI-generated code.”
- **Data-loss concerns:** [#94939](https://github.com/openclaw/openclaw/issues/94939) reports migration leaving an empty 0-byte SQLite conversation store, breaking Teams proactive sends.
- **Provider confusion:** [#120563](https://github.com/openclaw/openclaw/issues/120563) shows custom/Ollama users receiving fixed-size context every turn, making the agent appear to have no memory.

## 8. Backlog Watch

Important issues and PRs that have been waiting for maintainer attention:

- [#91588 — P0 Gateway memory leak](https://github.com/openclaw/openclaw/issues/91588) — open since 2026-06-09, critical crash-loop impact.
- [#62505 — Coding Agent regression](https://github.com/openclaw/openclaw/issues/62505) — open since 2026-04-07, needs maintainer review and product decision.
- [#50093 — WhatsApp backfill after reconnection](https://github.com/openclaw/openclaw/issues/50093) — open since 2026-03-19.
- [#47975 — Subagent sessions persist; main session unresponsive](https://github.com/openclaw/openclaw/issues/47975) — open since 2026-03-16.
- [#86215 — Codex OAuth refresh wedge](https://github.com/openclaw/openclaw/issues/86215) — open since 2026-05-24.
- [#86214 — Codex app-server client closes mid-turn](https://github.com/openclaw/openclaw/issues/86214) — open since 2026-05-24.
- [#10687 — Dynamic model discovery](https://github.com/openclaw/openclaw/issues/10687) — open since 2026-02-06, 3 👍.
- [#13219 — Per-model usage logging](https://github.com/openclaw/openclaw/issues/13219) — open since 2026-02-10.
- [#44395 — Heading-aware memory chunking](https://github.com/openclaw/openclaw/issues/44395) — open since 2026-03-12.
- [#68920 — HTTP completions TTFB optimization](https://github.com/openclaw/openclaw/issues/68920) — open since 2026-04-19.
- PR [#105025 — Twilio RCS channel](https://github.com/openclaw/openclaw/pull/105025) — open since 2026-07-12, currently “needs proof.”
- PR [#112811 — MS Teams multiple bot accounts](https://github.com/openclaw/openclaw/pull/112811) — open since 2026-07-23, ready for maintainer look.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Open-Source Personal AI Assistants & Agents
**Date:** 2026-08-15 | **Data window:** 2026-08-14 → 2026-08-15

---

## 1. Ecosystem Overview

The open-source personal AI assistant ecosystem remains highly fragmented but converging on a shared core: gateway-style message routing, MCP-based tooling, multi-provider model support, and chat-channel parity. Activity is heavily skewed toward **reliability work** — silent message loss, session-integrity bugs, gateway memory leaks, and MCP connection failures dominate the most-commented threads across nearly every project. A second wave of investment is visible in **protocol interop** (OpenAI Responses API, Chat Completions profiles so tools like Open WebUI and Aider can connect), **Windows/desktop distribution** (auto-update, test gaps, shell handling), and **design-heavy roadmaps** around security guardrails and deterministic automations. Notably, only IronClaw and LobsterAI shipped releases in this window; the rest are pre-release or in stabilization, indicating that the ecosystem is mid-cycle: merging features and absorbing regression reports rather than cutting versions.

---

## 2. Activity Comparison

*Updated in last 24h. Health score = qualitative 1–10 based on closure rates, critical-bug load, release cadence, maintainer responsiveness, and backlog age.*

| Project | Issues (updated) | PRs (updated) | Release status | Health | Notes |
|---|---|---|---|---|---|
| **OpenClaw** | 500 (12 closed) | 500 (97 merged/closed) | None in window | **4/10** | Extreme volume, but 94-comment silent-reply thread, P0 memory leak open since June, no release signal |
| **NanoBot** | 3 | 22 (8 closed/merged) | None | **7/10** | Fast bug turnaround (same-day Anthropic fix); P0 session-overwrite PR open |
| **Hermes Agent** | 50 (4 closed) | 50 (4 merged) | None | **7/10** | Epic completed (god-file sharding); strong Discord parity push; P2 desktop backlog |
| **PicoClaw** | 3 (2 closed stale) | 9 (5 merged) | None | **6/10** | MCP hang bug has fix PR but not merged; healthy contributor flow |
| **NanoClaw** | 2 | 9 | None | **7/10** | Low issue load; setup bugs getting fix PRs same day; AVX2 SIGILL unaddressed |
| **NullClaw** | 0 | 1 (merged) | None | **6/10** | Maintenance/steady-state; single config enhancement merged |
| **IronClaw** | 25 (9 closed) | 47 (22 merged) | **v1.2.0 (Aug 13)** | **8/10** | Best closure rate in window; automation-reliability epic driving v1.3.0; healthy release-line backports |
| **LobsterAI** | 2 | 27 (22 merged) | **2026.8.14** | **8/10** | High merge velocity; large release merged; same-day revert and duplicate fixes suggest process friction |
| **Moltis** | 0 | 1 (open) | None | **5/10** | Large connector PR unreviewed; no other movement |
| **CoPaw / QwenPaw** | 50 (38 closed) | 41 (15 closed) | None | **6/10** | Active triage but 26 open PRs; cluster of MCP/plugin 2.x regressions; Windows auto-update pain |
| **ZeptoClaw** | 0 | 0 | None | **2/10** | No activity detected |
| **ZeroClaw** | 33 (3 closed) | 50 (3 merged) | None (v0.8.5 freeze) | **6/10** | High RFC engagement; Windows test gap (74 failures) open since June; many stalled PRs |

---

## 3. OpenClaw's Position

**Advantages:**
- **Unmatched community scale** — 500 issues/500 PRs touched in 24h is ~10× the nearest peer (CoPaw/ZeroClaw at ~50). This creates gravity: more contributors, faster PR throughput (97 merged/closed in a day), and a broad channel surface (WhatsApp, Discord, Teams, Telegram, Slack, MSTeams, plus codex/Beam runtimes).
- **De-facto reference implementation** — several projects (NanoClaw, PicoClaw, ZeroClaw, IronClaw) are derivatives, forks, or direct competitors to OpenClaw's architecture (gateway + channels + provider abstraction). Its naming conventions and repo structure are the ecosystem baseline.
- **Feature surface is ahead of peers** — cloud workers for codex runtime, multi-account Teams, Twilio RCS, pre-routing hooks are all in-flight; no peer matches that breadth.

**Weaknesses vs peers:**
- **Stability debt is the worst in the ecosystem.** The dominant thread (94 comments) is a **silent reply failure** that persists despite a claimed fix; a P0 gateway memory leak (350MB→15.5GB RSS, OOM crash loops) has been open since June; a release-blocking regression (#108435) prevents 2026.7.1 gateway startup for self-hosters. Meanwhile IronClaw shipped v1.2.0 with backported migration fixes this week, and NanoBot closed its P2 Anthropic timeout bug the same day it was reported.
- **No release in window** while two peers shipped, reinforcing the perception that OpenClaw is "always merging, never stabilizing."
- **Contribution-to-review bottleneck** — 488 open issues and 403 open PRs suggest maintainers cannot keep pace; several P0/P1 items await product decisions.

**Technical approach difference:** OpenClaw is a **monolithic, channel-heavy gateway** (Rust-adjacent/Node-style runtime implied by "codex runtime," "Beam," "gateway") that assumes a single install serving many surfaces. Peers are diverging: IronClaw prioritizes **deterministic automation**; ZeroClaw is **RFC-driven with security-first architecture**; CoPaw is **AgentScope/Python-ecosystem-native**; NanoBot is **WebUI-first with strict typing discipline**. OpenClaw's advantage is breadth; its risk is that peers convert its reliability failures into differentiation.

---

## 4. Shared Technical Focus Areas

| Focus area | Projects involved | Specific needs observed |
|---|---|---|
| **Message delivery reliability** | OpenClaw, NanoBot, PicoClaw, CoPaw, IronClaw | Silent reply loss (OpenClaw #121058, 94 comments); stale background saves overwriting sessions (NanoBot P0); MCP failure freezing agent loop (PicoClaw #3269); tool-call 404s during streaming (CoPaw #7016); deterministic no-delivery suppression for scheduled runs (IronClaw #7647) |
| **MCP / tool-ecosystem trust** | PicoClaw, CoPaw, IronClaw, Moltis, OpenClaw | Connection failures must not block the loop; duplicate tool results (`structuredContent` + `content`, CoPaw #6958); plugin isolation (CoPaw Creator plugin disables others); pluggable memory over MCP (IronClaw #7664); durable provider-neutral connectors (Moltis PR #1190); Go agent loop should treat MCP init errors as recoverable |
| **Model/provider flexibility & discovery** | OpenClaw, CoPaw, ZeroClaw, Hermes, NanoBot, IronClaw | Dynamic OpenRouter model discovery; auto model discovery UX confusion (CoPaw #3045); OpenAI Responses API/`/v1/chat/completions` interop (CoPaw #3002, ZeroClaw #8603); per-session model overrides (CoPaw PR #5992); MiniMax/anthropic-compatible provider checks (CoPaw #2303); Anthropic idle-timeout misapplication (NanoBot #5391); per-user model selection (IronClaw, resolved) |
| **Windows & desktop distribution** | NanoBot, CoPaw, ZeroClaw, Hermes, IronClaw | Transient `PermissionError` crashes gateway (NanoBot #5382); 74 test failures + Linux-only CI (ZeroClaw #7462); missing auto-update, icon issues (CoPaw #2846); LSP POSIX-shim WinError 193 (Hermes #86445); Windows filesystem/smoke fixes in IronClaw 1.2.0 forward-ports |
| **WebUI / session UX** | NanoBot, LobsterAI, CoPaw, OpenClaw | Drag-and-drop session organization (NanoBot #5389); sidebar polish/group refinements (NanoBot #5395, LobsterAI sidebar releases); localization across 10 locales (NanoBot #5367); misleading skill-toggle state (LobsterAI #2483/#2491); UI density complaints (OpenClaw #75947) |
| **Security / guardrails** | ZeroClaw, Hermes, OpenClaw, IronClaw | Shell-policy allow/ask/deny (ZeroClaw #7155); atomic action-budget limits (#9996); blocking destructive commands (#9839); extension state leaking between users (IronClaw #7659); configurable auth + OIDC (ZeroClaw #7141); data-loss bugs in file tools (OpenClaw #119270) |
| **Automation / unattended runs** | IronClaw, OpenClaw, ZeroClaw | Cron agent stalls on provider-specific prefixes (OpenClaw #121953); structured execution contracts, preflight grants, model pinning, semantic success evaluation (IronClaw v1.3.0 epic #6879 + #7644–7647) |
| **Session/data persistence** | OpenClaw, NanoBot, NullClaw, Hermes | SQLite migration leaving empty stores (OpenClaw #94939); configurable memory DB path (NullClaw #986); file-cap archive mutation before persistence (NanoBot #5378); transcript refresh/visibility bugs (Hermes #59591) |

---

## 5. Differentiation Analysis

| Project | Primary focus | Target users | Architectural signature |
|---|---|---|---|
| **OpenClaw** | Breadth-first personal assistant platform | General users, self-hosters, family/business automation | Monolithic gateway + many channels; largest integration surface; codex/Beam runtimes for agentic work |
| **IronClaw** | Reliable automations & enterprise-ish stability | Ops-minded users, automation-heavy workflows | Release-line discipline (1.2.0 shipped, RC validation, backports); structured execution contracts; nearai ecosystem |
| **ZeroClaw** | Security-first, RFC-driven design | Trust-conscious operators, power users | Design RFCs as gatekeepers (security pipeline, auth, Chat Completions profile); restrictive overlays; v0.8.5 stabilization + v0.9.0 architecture |
| **CoPaw / QwenPaw** | Python/AgentScope native, Chinese-speaking community | Chinese users, AgentScope ecosystem, desktop Windows | Deep integration with `agentscope`; heavy plugin/skill-system iteration; Chinese-first UI |
| **NanoBot** | WebUI polish + session integrity | UX-sensitive users, gateway operators | Python + BasedPyright strict typing; fast bug turnaround; marketplace-skill lifecycle |
| **Hermes Agent** | Channel completeness (Discord) + codebase modularity | Discord-heavy communities, desktop users | Discord API v10 omniscience modules (22–82 tests each); god-file sharding; per-profile MCP RPCs |
| **LobsterAI** | Product-driven UI/UX + Team Edition | Chinese/NetEase-Youdao ecosystem, team deployments | Sidebar/session UI innovation; account/quota flows; cowork multi-agent activity |
| **PicoClaw** | Lightweight channel integrations | Channel-focused users (DingTalk, WeChat, DeltaChat) | Go-based agent loop; modular channel adapters; small, fast-moving PRs |
| **NanoClaw** | Setup simplicity + hardened images | New users, low-power devices | Prebuilt agent images; signature-approval CI workflow; Dial (SMS/voice) channel push |
| **NullClaw / Moltis / ZeptoClaw** | Maintenance / connectors | Niche or early-stage | Minimal activity; storage configurability (NullClaw), durable CalDAV/Gmail/channel connectors (Moltis) |

---

## 6. Community Momentum & Maturity

**Tier 1 — High velocity, shipping:**
- **IronClaw** — strongest overall health: 9 issues + 22 PRs closed, released v1.2.0, and is already executing a coherent v1.3.0 plan (automation reliability, pluggable memory, unbound turns). The model to watch for execution discipline.
- **LobsterAI** — 22 PRs merged in a day, two releases in window, strong feature output; risk is process debt (same-day revert, duplicate fix PRs, March-era stale PRs) ahead of a major "v4pro" release.

**Tier 2 — High velocity, pre-release / stability-challenged:**
- **OpenClaw** — the largest community and highest merge volume, but its health metric is dragged down by unresolved P0s and a visible gap between community reporting and maintainer resolution.
- **ZeroClaw** — momentum is in design, not delivery; RFCs are accumulating comments and revisions, but merged PR count (3) is low and a Windows test gap persists. In a deliberate design phase before v0.9.0.
- **CoPaw / QwenPaw** — high issue-triaging throughput (38 closed) but 26 open PRs and a 2.x regression cluster suggest the review pipeline and regression harness are the limiting factors.
- **Hermes Agent** — healthy contributor rhythm; the Discord parity campaign is well-structured (small validated modules), but P2 desktop/platform bugs remain open for weeks.

**Tier 3 — Moderate activity:**
- **NanoBot, PicoClaw, NanoClaw** — small but responsive. NanoBot's same-day fix culture and PicoClaw/NanoClaw's quick contributor follow-ups show healthy communities; scope is the constraint, not quality.

**Tier 4 — Maintenance / dormant:**
- **NullClaw** (steady-state, low risk), **Moltis** (single large PR pending review), **ZeptoClaw** (no activity).

**Cross-cutting maturity observation:** The ecosystem is converging on **release discipline as the differentiator**. Projects with formal stabilization lines (IronClaw, ZeroClaw's v0.8.5 freeze) or rapid same-day fixes (NanoBot, IronClaw) generate more user trust than projects with raw contribution volume and unresolved P0s.

---

## 7. Trend Signals

1. **Reliability has overtaken features as the top user demand.** The most-commented issue in the entire ecosystem is OpenClaw's silent-reply failure (94 comments, users running monitoring cron jobs). IronClaw's automation-reliability epic explicitly rejects "model-dependent behavior" as unacceptable. Expect users to migrate to projects that can demonstrate deterministic message delivery and deterministic automation outcomes.

2. **MCP is the new trust bottleneck.** Connection hangs (PicoClaw), duplicate results (CoPaw), plugin isolation failures (CoPaw), and provider-neutral connector durability (Moltis, IronClaw) all point to MCP as the ecosystem's critical integration layer. AI agent developers should treat MCP connection failures as recoverable, review `content`+`structuredContent` duplication, and expose per-profile MCP lifecycle management.

3. **OpenAI protocol interop is becoming table stakes.** ZeroClaw (Chat Completions profile targeting Open WebUI, LobeChat, Continue.dev, Aider, LangChain), CoPaw (Responses API + Azure proxy gateways), and OpenClaw (HTTP `/v1/chat/completions` voice/lightContext modes) all signal the same need: agents must be usable as drop-in OpenAI-compatible endpoints to win ecosystem tooling.

4. **Windows is the underserved platform.** ZeroClaw (74 test failures, Linux-only CI), NanoBot (gateway crashing on `os.replace` PermissionError), CoPaw (no auto-update, cmd.exe flashing), and Hermes (LSP shim WinError 193) reveal systemic neglect. Projects that add Windows CI coverage and desktop auto-update will capture an under-served user base.

5. **Per-user / per-session model control is the new UX frontier.** CoPaw's per-session overrides, IronClaw's per-user model selection (closed this week after user demand), and Telegram/console model pickers (ZeroClaw #9895, CoPaw #2763) show that operators want model choice at the conversation level, not just the config-file level.

6. **Security guardrails are shifting from policy to architecture.** ZeroClaw's RFC cluster (runtime-owned security decision pipeline, shell allow/ask/deny, atomic action budgets, OIDC pluggable auth) plus IronClaw's extension-isolation bug and OpenClaw's data-loss file-tool bug indicate security is moving from prompt-level restrictions to enforceable runtime controls.

7. **Chinese-language communities are a major, distinct constituency.** CoPaw and LobsterAI both show significant Chinese user activity, Chinese-first UI requirements, and different platform priorities (Feishu, WeChat audio, OneBot). Western projects that ignore localization and Chinese channel coverage leave a large market open.

8. **For AI agent developers specifically:** the highest-leverage investments right now are (a) deterministic execution for unattended/scheduled runs with explicit success/failure semantics, (b) graceful degradation when MCP or model providers fail, (c) OpenAI-compatible endpoints for ecosystem tooling, and (d) Windows test coverage — the fastest way to differentiate in an ecosystem where every project is struggling with the same reliability, trust, and platform gaps.

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot Project Digest — 2026-08-15

## 1. Today's Overview

NanoBot is in a high-activity development and maintenance phase: 22 PRs were updated in the last 24 hours (14 open, 8 closed/merged), while issue activity was relatively low at 3 updated issues. No new release was published, so all visible progress remains pre-release. The busiest areas are WebUI polish/organization, session-save reliability, provider streaming fixes, and refactoring for stricter type checking. Bug-fix turnaround is strong — for example, the Anthropic streaming timeout issue (#5391) was reported and closed with a fix PR on the same day.

## 2. Releases

**None.** No new releases were published in this window, so no changelog, breaking-change, or migration notes are available.

## 3. Project Progress

The following PRs were closed or updated to closed status in the last 24 hours. Note that labels such as `conflict` or `invalid` indicate some closures were not merges.

- **[#5392](https://github.com/HKUDS/nanobot/pull/5392) — `fix(anthropic): treat stream idle timeout as inactivity only, not total time`**  
  Fixes #5391. Prevents `NANOBOT_STREAM_IDLE_TIMEOUT_S` from killing long but active Anthropic generations. Closed.

- **[#5395](https://github.com/HKUDS/nanobot/pull/5395) — `feat(webui): refine conversation groups and shared shapes`**  
  Improves group terminology, drag behavior, and consistent WebUI control styling. Closed.

- **[#5393](https://github.com/HKUDS/nanobot/pull/5393) — `feat(webui): polish sidebar and session transitions`**  
  UI-only follow-up split from #5358, improving sidebar hierarchy and session transition visuals. Closed.

- **[#5018](https://github.com/HKUDS/nanobot/pull/5018) — `feat(skills): support explicit context loading`**  
  Addresses the ignored `skill_names` input in `ContextBuilder`. Closed with `conflict` label.

- **[#4689](https://github.com/HKUDS/nanobot/pull/4689) — `feat(providers): surface OAuth status and expiry warnings`**  
  Closed with `invalid` and `conflict` labels, so it did not land as originally proposed.

- **[#5390](https://github.com/HKUDS/nanobot/pull/5390) — `Agent/knowledge graph` chore**  
  Closed; no substantive summary provided.

Closed issues today:
- **[#5391](https://github.com/HKUDS/nanobot/issues/5391)** — Anthropic idle timeout bug; fixed by #5392.
- **[#5378](https://github.com/HKUDS/nanobot/issues/5378)** — File-cap archive failure mutating session before persistence; closed.

## 4. Community Hot Topics

PR comment counts were not exposed in this snapshot, so activity here is inferred from priority labels, cross-linked issues, and update recency.

- **[#5391 / #5392 — Anthropic streaming timeout](https://github.com/HKUDS/nanobot/issues/5391)**  
  This was a quickly reproduced and fixed bug. Users running long Anthropic generations with the no-callback path could have their streams killed by an idle timeout that was incorrectly applied as a total timeout. The same-day fix PR indicates strong maintainer/contributor responsiveness.

- **[#5271 — P0: stale background task saves overwriting session data](https://github.com/HKUDS/nanobot/pull/5271)**  
  Open, priority `p0`. This is a session-integrity fix preventing background saves from overwriting a session after `/new` or lifecycle replacement. It is the highest-severity open PR and likely receiving close maintainer attention.

- **[#5161 / #5396 — Narrowing Pyright suppressions](https://github.com/HKUDS/nanobot/issues/5161)**  
  A refactor to reduce file-level type-check suppressions after enabling strict BasedPyright checks. PR #5396 is open and touches many tool modules. This reflects an ongoing code-quality push rather than user-facing functionality.

- **[#5309 — Marketplace skills shadowing builtins](https://github.com/HKUDS/nanobot/pull/5309)**  
  Open bug fix. Users cannot install workspace/marketplace skills that share names with bundled skills, because the marketplace marks the bundled skill as already installed. It is a concrete UX issue for skill management.

## 5. Bugs & Stability

Ranked by severity:

- **[P0] #5271 — Stale background task saves can overwrite session data](https://github.com/HKUDS/nanobot/pull/5271)**  
  A session after `/new` can be overwritten by in-flight compactor/save work. Fix PR is open and proposes serializing `/new` and rejecting stale saves. This is the most serious stability issue in the current queue.

- **[P2] #5382 — Windows `os.replace()` transient PermissionError crashes gateway](https://github.com/HKUDS/nanobot/pull/5382)**  
  `JsonlSessionStore.save()` failed with `[WinError 5] Access is denied` during heartbeat-triggered session saves; the error crashed the whole gateway. A retry fix is open.

- **[P2] #5391 — Anthropic idle timeout killed long active generations](https://github.com/HKUDS/nanobot/issues/5391)**  
  Closed by [#5392](https://github.com/HKUDS/nanobot/pull/5392). The streaming idle timeout was applied as a total timeout on the no-callback path.

- **[P2] #5378 — File-cap archive failure mutated session before persistence](https://github.com/HKUDS/nanobot/issues/5378)**  
  `Session.enforce_file_cap()` discarded overflow in memory before the archive callback ran; if archive failed, the in-memory session was already changed. Closed.

- **[P2] #5371 — Assistant actions shown before turn end](https://github.com/HKUDS/nanobot/pull/5371)**  
  Open WebUI bug fix to hide copy/fork actions until `turn_end`, preventing premature user actions on incomplete turns.

## 6. Feature Requests & Roadmap Signals

Several open PRs point to a strong WebUI experience roadmap for future releases:

- **[#5356](https://github.com/HKUDS/nanobot/pull/5356) — Improve setup flows across chat channels**  
  Reorganizes channel configuration forms into clearer sections and makes unconfigured channels actionable.

- **[#5367](https://github.com/HKUDS/nanobot/pull/5367) — Localize agent activity in the WebUI**  
  Adds localization across 10 supported locales and live language-switch updates.

- **[#5389](https://github.com/HKUDS/nanobot/pull/5389) — Drag-and-drop session organization**  
  Session reordering, grouping, and moving between groups via drag-and-drop.

- **[#5358](https://github.com/HKUDS/nanobot/pull/5358) — Session collaboration via mentions**  
  Persisted server-owned `@name` handles and composer mention picker for peer sessions.

- **[#5179](https://github.com/HKUDS/nanobot/pull/5179) — MCP SDK v2 migration**  
  Modernizes MCP integration while preserving SSRF validation and legacy SSE compatibility.

- **[#4329](https://github.com/HKUDS/nanobot/pull/4329) — Native TypeScript terminal UI**  
  Long-running enhancement to rebuild `nanobot agent` as a native TypeScript/OpenTUI client while keeping the Python gateway as the agent-loop backend.

- **[#5309](https://github.com/HKUDS/nanobot/pull/5309) — Allow marketplace skills to shadow builtins**  
  A small but important skills/UX feature expected to unblock workspace skill overrides.

These signals suggest the next release will likely emphasize WebUI usability, session organization, and provider/session reliability.

## 7. User Feedback Summary

Real user pain points visible in this snapshot:

- **Anthropic long generations being killed** despite active streaming — reported and fixed quickly, which is a positive signal for user responsiveness.
- **Windows stability**: a transient `PermissionError` during session save crashed the whole gateway; this is a serious reliability concern for Windows users.
- **Session data integrity**: users could lose session data after `/new` due to stale background saves, or after file-cap archive failures.
- **Skill management confusion**: marketplace skills could not override builtins, preventing users from installing workspace copies of same-named skills.

Satisfaction indicators are indirect but mostly positive: same-day fixes, active contributor PRs, and maintainers splitting large PRs to keep WebUI changes reviewable. No direct satisfaction metrics, ratings, or user comments beyond bug reports were available in this snapshot.

## 8. Backlog Watch

These open PRs/issues have been active for a longer period or carry unresolved complexity and may need maintainer attention:

- **[#4145](https://github.com/HKUDS/nanobot/pull/4145) — Weather Skill**  
  Open since June 1. A multi-file skill contribution that has remained unreviewed/merged for over two months.

- **[#4329](https://github.com/HKUDS/nanobot/pull/4329) — Native TypeScript terminal UI**  
  Open since June 13. Large architectural enhancement; requires significant review.

- **[#5152](https://github.com/HKUDS/nanobot/pull/5152) — Subagent partial completion results**  
  Open since July 28. It adds `subagent_remaining_count` metadata so models do not infer unfinished results. Needs review and testing.

- **[#5179](https://github.com/HKUDS/nanobot/pull/5179) — MCP SDK v2 migration**  
  Open since July 30. High-impact provider change with security-sensitive transport rewriting.

- **[#5161](https://github.com/HKUDS/nanobot/issues/5161) / [#5396](https://github.com/HKUDS/nanobot/pull/5396) — Pyright suppression narrowing**  
  Issue open since July 29; PR #5396 opened August 14. Refactor touches many core tool files and could use focused review.

The `conflict` labels on several closed/large PRs also suggest that WebUI and provider changes are accumulating overlapping scope; maintainers may need to sequence or rebase these to avoid further merge conflicts.

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-08-15

## 1. Today's Overview
High-activity day for Hermes Agent: 50 issues and 50 PRs were updated in the last 24h, with 4 issues closed and 4 PRs merged/closed (the rest remain open). No new release was published. The dominant theme is the **Discord API v10 feature-parity campaign**, which produced a wave of small, well-tested sub-issues and matching PRs across message, thread, forum, channel, role, moderation, and transport modules. Alongside that, the long-running **“All Gods Must Die” god-file sharding epic (#78647) was completed**, and multiple bug fixes advanced for TUI, Desktop, delegation, browser, voice, and LSP. Overall project health is strong, though the open bug backlog includes several P2 desktop/platform issues waiting for maintainer attention.

## 2. Releases
No new releases in the last 24 hours.

## 3. Project Progress
4 PRs were merged/closed in the last 24h, but the provided top-20 PR list is entirely open PRs, so the individual merged PRs are not enumerated in this excerpt. Four issues were closed, confirming delivered fixes:
- **#78647** – [EPIC — COMPLETE] All Gods Must Die: 20/20 killed — god-file sharding completed (https://github.com/NousResearch/hermes-agent/issues/78647)
- **#59591** – Dashboard resumed session transcript incomplete until theme change forces refresh (https://github.com/NousResearch/hermes-agent/issues/59591)
- **#66490** – TUI enables DEC 2026 synchronized output inside Zellij, causing repeated/scrolling frames (https://github.com/NousResearch/hermes-agent/issues/66490)
- **#41480** – TUI status bar flickers heavily during streaming (https://github.com/NousResearch/hermes-agent/issues/41480)

Feature work advanced through open PRs:
- **Discord omniscience modules**: M2 message edit/delete (#86449), M5 poll projection (#86451), T1 thread lifecycle (#86454), T3 forum starter/tag (#86458), A1 channel/category CRUD (#86460), A3 role CRUD + assignment (#86462), A6 scheduled events (#86466), R1 rate-limit contract (#86468).
- **Provider support**: GLM-5.3 added to the zai provider (#86433).
- **Gateway/control plane**: per-profile MCP server lifecycle RPCs (#86473).

Fix-oriented PRs proposed today:
- Browser subprocess env sanitization (#86371), idle browser session reclamation (#86472), slash_worker PATH fixes (#86374), /context routing through live session (#86434), Kanban delivery profile-safety (#86455), Windows LSP wrapper probing (#86456), delegation reliability (#68499, #77050, #83485), and xAI TTS WebSocket compatibility (#86368).

## 4. Community Hot Topics
- **#78647** — *[EPIC — COMPLETE] All Gods Must Die: 20/20 killed* — 71 comments, closed. Massive community engagement around repo-wide refactoring; reflects strong demand for maintainability and modularization. (https://github.com/NousResearch/hermes-agent/issues/78647)
- **#79564** — *Discord Feature Parity & Alignment Campaign (API v10) — meta-issue* — 4 comments. Central hub for the Discord feature wave; indicates systematic user demand for REST v10 coverage. (https://github.com/NousResearch/hermes-agent/issues/79564)
- **#8751** — *PermissionError when walking parent directories for .git root* — 3 comments, P2, open since April. Highlights filesystem-permission edge cases in the agent’s prompt builder. (https://github.com/NousResearch/hermes-agent/issues/8751)
- **#59591** — *Dashboard resumed session transcript appears incomplete until theme change* — 3 comments, closed; users were confused by stale transcript views, now fixed. (https://github.com/NousResearch/hermes-agent/issues/59591)

No PR reactions/comments data was available in the provided excerpt; issue comment counts drive the ranking.

## 5. Bugs & Stability
Bugs reported or active in the last 24h, ranked by severity:

**P2 — High impact**
- **#86411** — Explicit `terminal.cwd` re-pins the working directory mid-turn, overriding launch dir on local backend (CLI/Gateway). No fix PR yet. (https://github.com/NousResearch/hermes-agent/issues/86411)
- **#86385** — macOS Screen Recording prompt loops after update; stale TCC grant with no way to re-grant. No fix PR yet. (https://github.com/NousResearch/hermes-agent/issues/86385)
- **#73722** — Desktop boot fails fatally on transient gateway connect failures; boot path never retries. Open since 07-29, no fix PR in current list. (https://github.com/NousResearch/hermes-agent/issues/73722)
- **#8751** — `PermissionError` when walking parent directories in `agent/prompt_builder.py`; open since April, no fix PR currently. (https://github.com/NousResearch/hermes-agent/issues/8751)

**P3 — Targeted / platform-specific**
- **#86445** — Windows LSP server resolution picks POSIX shim → WinError 193. **Fix PR #86456 open.** (https://github.com/NousResearch/hermes-agent/issues/86445)
- **#86403** — Tool calling broken for Xiaomi MiMo v2.5 Pro (enabled tools not exposed). No fix PR yet. (https://github.com/NousResearch/hermes-agent/issues/86403)
- **#86452** — Recurring background MCP discovery WARNING when no MCP servers configured; duplicate. (https://github.com/NousResearch/hermes-agent/issues/86452)
- **#86393** — Kanban runtime `TERMINAL_CWD` misreported as deprecated `.env` setting. (https://github.com/NousResearch/hermes-agent/issues/86393)
- **#84274** — Desktop UI zoom resets to 100% after RDP session reconnect. (https://github.com/NousResearch/hermes-agent/issues/84274)
- **#73495** — Desktop Cloud cold start can hide agents until Portal re-login. (https://github.com/NousResearch/hermes-agent/issues/73495)

## 6. Feature Requests & Roadmap Signals
The biggest roadmap signal is the **Discord omniscience campaign** (#79564): 14 new sub-issues and 8 matching PRs landed in the last 24h, covering messages, reactions, polls, threads, forums, channels, permissions, roles, moderation, guild settings, scheduled events, rate limits, pagination, and reliability telemetry. These are small, validated modules (22–82 tests each) and are likely to be merged in the near term, making the next release a major Discord REST expansion.

Also notable:
- **Per-profile MCP server lifecycle RPCs** (#86473) — enables desktop clients to fully manage MCP servers for any profile, not just the launch profile.
- **GLM-5.3 support** (#86433) — quick route to expanding provider coverage.
- **Browser session reclamation** (#86472) and **TTS WebSocket compatibility** (#86368) — address stability/UX issues rather than new surface area.

Prediction: next release will fold in the Discord omniscience modules, GLM-5.3, MCP lifecycle RPCs, and the delegation/browser/TTS fixes currently in open PRs.

## 7. User Feedback Summary
User pain points visible in this window:
- **Permission and platform trust issues**: Windows LSP shim failure, macOS Screen Recording re-grant loop, and filesystem `PermissionError` all cause hard failures and are especially disruptive for desktop/enterprise users.
- **Desktop reliability**: cold-start agent visibility, RDP zoom resets, and fatal boot no-retry behavior on transient gateway outages are recurring complaints.
- **Discord coverage**: users are actively filing small, precise feature requests with test evidence, indicating both enthusiasm and a clear expectation of API completeness.
- **Positive signal**: the completion of the god-file epic was met with strong participation (71 comments) and signals community support for architectural cleanup.

## 8. Backlog Watch
Issues/PRs that have been open for a while and need maintainer attention:
- **#8751** (P2, open since 2026-04-13) — PermissionError walking parent directories; high relevance to agent prompt building. (https://github.com/NousResearch/hermes-agent/issues/8751)
- **#73722** (P2, open since 2026-07-29) — Desktop boot fatal on transient gateway failures; no fix PR. (https://github.com/NousResearch/hermes-agent/issues/73722)
- **#73495** (P3, open since 2026-07-28) — Desktop Cloud cold start hides agents; no fix PR. (https://github.com/NousResearch/hermes-agent/issues/73495)
- **PR #68499** (open since 2026-07-21) — fix(delegation): separate lifecycle from task outcome; broad blast radius, needs careful review. (https://github.com/NousResearch/hermes-agent/pull/68499)
- **PR #77050** (open since 2026-08-02) — fix(delegation): expose remote-readable artifact paths. (https://github.com/NousResearch/hermes-agent/pull/77050)
- **#79564** (meta-issue, open since 2026-08-05) — Discord parity campaign will keep growing; maintainers should define roadmap/priority to avoid issue sprawl. (https://github.com/NousResearch/hermes-agent/issues/79564)

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw Project Digest — 2026-08-15

## 1. Today's Overview

PicoClaw saw a moderately active 24-hour window: 3 issues were updated (1 open, 2 closed as stale) and 9 pull requests were touched (4 open, 5 closed/merged). The most important development is the new open PR #3337, which directly targets the currently open high-severity bug #3269 where an MCP server connection failure hangs the agent loop and freezes the chat interface. No new releases were published. The project remains healthy with a steady stream of community-contributed fixes, though several older PRs are still waiting for review or action from maintainers.

## 2. Releases

No new releases were published in the last 24 hours.

## 3. Project Progress

The following PRs were closed/merged or otherwise advanced:

- **#3303** — `build(deps): bump actions/stale from 10 to 11`  
  Dependency/CI update.  
  https://github.com/sipeed/picoclaw/pull/3303

- **#3283** — `fix(dingtalk): support picture/image message inbound`  
  Adds image message handling for DingTalk, with OpenAPI token caching and graceful degradation.  
  https://github.com/sipeed/picoclaw/pull/3283

- **#3279** — `fix(seahorse): prevent tool-call format leakage into LLM summaries`  
  Fixes a bug where tool-call formatting leaked into user-visible/LLM-readable summaries via `partsToReadableContent`.  
  https://github.com/sipeed/picoclaw/pull/3279

- **#3271** — `chore(providers): update default model names to 2026-07 latest`  
  Refreshes default model lists across 9 providers, including OpenAI, Anthropic, and others, verified against official docs.  
  https://github.com/sipeed/picoclaw/pull/3271

- **#3270** — `feat: add DashScope TTS provider and WeChat audio file sending`  
  Adds a new DashScope/Bailian TTS provider and WeChat audio file sending support.  
  https://github.com/sipeed/picoclaw/pull/3270

Also actively updated:

- **#3337** — `Fix/mcp failure hangs agent loop`  
  Open PR that fixes the MCP failure hang described in issue #3269.  
  https://github.com/sipeed/picoclaw/pull/3337

- **#3222** — `refactor(deltachat): cleanup implementation, documentation -200LOC`  
  Open refactor removing legacy features and cleaning DeltaChat documentation.  
  https://github.com/sipeed/picoclaw/pull/3222

## 4. Community Hot Topics

The most active discussion is around MCP connection reliability:

- **#3269** — `[BUG] If the MCP server connection fails, the agent loop will hang`  
  Open, 5 comments, 1 👍.  
  Users report that a failed MCP server connection completely freezes the PicoClaw chat interface. This is a critical reliability issue for anyone using MCP tools.  
  https://github.com/sipeed/picoclaw/issues/3269

- **#3337** — `Fix/mcp failure hangs agent loop`  
  Open PR intended to resolve #3269. It changes `AgentLoop.Run` so an `ensureMCPInitialized` error no longer exits the entire agent loop.  
  https://github.com/sipeed/picoclaw/pull/3337

Two stale-closed issues also received retain attention:

- **#3308** — Code review raising concurrency hazards, goroutine leaks, and memory/speed concerns in SeaHorse, Channel Manager, and Hooks.  
  https://github.com/sipeed/picoclaw/issues/3308

- **#3307** — Feature request for session list/switch commands in Telegram and other chat channels, since session management currently exists only in the Web UI.  
  https://github.com/sipeed/picoclaw/issues/3307

## 5. Bugs & Stability

Ranked by severity:

1. **High — MCP connection failure hangs agent loop**  
   #3269: If MCP server connection fails, the agent loop hangs and PicoClaw stops replying to users.  
   Fix PR: #3337 is open but not yet merged.  
   https://github.com/sipeed/picoclaw/issues/3269  
   https://github.com/sipeed/picoclaw/pull/3337

2. **Medium — Tool-call format leakage into LLM summaries**  
   #3279: Fixed in PR; tool-call formatting was leaking into readable content used for LLM summaries.  
   https://github.com/sipeed/picoclaw/pull/3279

3. **Medium — `exec` tool ignores per-run timeout and boolean options**  
   #3319: The `exec` tool advertises a `timeout` argument but always uses the global timeout; `background` and `pty` are declared as strings instead of booleans. Open PR exists.  
   https://github.com/sipeed/picoclaw/pull/3319

4. **Review-level concern — Concurrency hazards, goroutine leaks, memory/speed**  
   #3308: Raised in a code-review issue, but it has been closed as stale. Still worth tracking.  
   https://github.com/sipeed/picoclaw/issues/3308

## 6. Feature Requests & Roadmap Signals

- **Session management for non-Web channels** — #3307 asks for Telegram session list/switch/delete commands, matching the Web UI's existing history dropdown. This remains an unaddressed user-facing gap.  
  https://github.com/sipeed/picoclaw/issues/3307

- **Configurable default model fallback chain** — #3200 proposes a configurable default fallback chain in the web UI, persisted through the backend API. Open since July, with no comments yet.  
  https://github.com/sipeed/picoclaw/pull/3200

- **DashScope TTS + WeChat audio** — #3270 was closed/merged and adds new audio capabilities. This suggests continued investment in multi-channel and TTS functionality.

- **DeltaChat cleanup** — #3222 proposes a significant refactor of the DeltaChat implementation, including dropping legacy features and improving documentation. Still open.  
  https://github.com/sipeed/picoclaw/pull/3222

Likely next-version candidates: MCP hang fix (#3337), exec tool timeout/boolean fix (#3319), and possibly session-command support for Telegram if maintainers pick up #3307.

## 7. User Feedback Summary

Real user pain points visible in this data:

- **MCP reliability is critical** — users depend on MCP servers and are severely impacted when a connection failure freezes the entire assistant.
- **Per-invocation tool settings need to work correctly** — the `exec` tool's timeout and boolean options are silently ignored, reducing trust in tool behavior.
- **Cross-channel feature parity matters** — Telegram users do not have the session management features available in the Web UI.
- **Performance and concurrency are being watched** — community contributors are proactively reviewing core components for goroutine leaks and memory issues.

On the positive side, contributors are actively submitting fixes and new integrations (DingTalk images, DashScope TTS, WeChat audio), indicating a healthy and engaged community. The quick creation of PR #3337 in response to #3269 shows responsive maintainer/contributor collaboration.

## 8. Backlog Watch

These items may need maintainer attention:

- **#3269** — Open MCP hang bug, with an open fix PR #3337. Needs review and merge priority.  
  https://github.com/sipeed/picoclaw/issues/3269  
  https://github.com/sipeed/picoclaw/pull/3337

- **#3200** — Open PR for configurable fallback chains, created July 1, no comments, still open. May need review or explicit closure.  
  https://github.com/sipeed/picoclaw/pull/3200

- **#3222** — Open DeltaChat refactor PR, created July 3. No comments in the data, needs maintainer feedback.  
  https://github.com/sipeed/picoclaw/pull/3222

- **#3319** — Open `exec` tool fix, created August 7, not yet merged. Addresses a concrete bug in tool functionality.  
  https://github.com/sipeed/picoclaw/pull/3319

- **#3308** — Closed as stale, but raises valid concurrency/performance concerns. Maintainers may want to reopen or create follow-up issues.  
  https://github.com/sipeed/picoclaw/issues/3308

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Project Digest — 2026-08-15

## 1. Today's Overview

NanoClaw had a compact but active 24 hours: two new open issues were filed, nine pull requests were touched, and no release was published. The issue load is low, but both reports are setup-affecting bugs that are already receiving contributor attention — one already has a matching fix PR. Maintainer activity was visible around the signature-approval workflow, with three intentionally-closed "DO NOT MERGE" test PRs. Open work spans setup hardening, scheduling, Windows container-runtime cleanup, documentation, and the long-running Dial channel feature. Overall, the project looks stable: bug reports are concrete, repair PRs are materializing quickly, and no merge-blocking regressions are visible.

## 2. Releases

No new releases were published in this window, so there are no changelog entries, breaking-change notes, or migration instructions to report.

## 3. Project Progress

No confirmed user-facing merge happened in the last 24 hours. The three closed PRs were internal workflow tests, not feature deliveries:

- [#3244 — "DO NOT MERGE — live-fire the signature approver (take 2)"](https://github.com/nanocoai/nanoclaw/pull/3244) — closed unmerged; tested the verified-agent-image approval chain.
- [#3242 — "DO NOT MERGE — live-fire test of the signature approver"](https://github.com/nanocoai/nanoclaw/pull/3242) — closed unmerged; exercised verify → approve-agent-image → cosign verify → approving review.
- [#3243 — "verify-agent-image: arming auto-merge is not a verdict"](https://github.com/nanocoai/nanoclaw/pull/3243) — closed; improves the CI job so auto-merge arming failures are not mistaken for image-verification verdicts.

Forward progress is visible in open PRs: a fix for the old-Node setup bug ([#3249](https://github.com/nanocoai/nanoclaw/pull/3249)), a scheduling fix for malformed cron strings ([#3247](https://github.com/nanocoai/nanoclaw/pull/3247)), a Windows container-cleanup fix ([#3246](https://github.com/nanocoai/nanoclaw/pull/3246)), and a skills-documentation fix ([#3230](https://github.com/nanocoai/nanoclaw/pull/3230)).

## 4. Community Hot Topics

No comment or reaction counts were available for the PRs, and the two open issues both have 0 comments and 0 👍. By recency and contributor action, the active items are:

- [#3248 — setup.sh's Node "too old" branch is broken because install-node.sh short-circuits on any existing Node](https://github.com/nanocoai/nanoclaw/issues/3248) — user-side setup friction; fix PR [#3249](https://github.com/nanocoai/nanoclaw/pull/3249) is already open.
- [#3245 — Prebuilt agent image requires AVX2; SIGILL on CPUs without it](https://github.com/nanocoai/nanoclaw/issues/3245) — default installation path can crash on certain Intel Atoms and similar CPUs.
- [#3041 — Dial channel adapter (SMS + AI voice calls)](https://github.com/nanocoai/nanoclaw/pull/3041) and [#3050 — add Dial to setup picker/wizard/skills](https://github.com/nanocoai/nanoclaw/pull/3050) — long-running feature work, still open and updated in this window.

The underlying need is clear: users want setup to succeed on a wider range of environments, and contributors are pushing for new communication channels to be first-class setup options.

## 5. Bugs & Stability

Ranked by severity:

1. **High — Prebuilt agent image can crash with SIGILL on CPUs without AVX2**  
   [#3245](https://github.com/nanocoai/nanoclaw/issues/3245)  
   The default hardened agent image contains a Bun binary built for the non-baseline x64 target, so CPUs like Intel Tremont/Elkhart Lake Atoms (Celeron J6413/N5105, etc.) will fault during execution. No fix PR is open yet.

2. **Medium — setup.sh cannot repair an existing but too-old Node**  
   [#3248](https://github.com/nanocoai/nanoclaw/issues/3248)  
   The version check correctly detects Node < 20, but the helper path short-circuits because `install-node.sh` refuses to act when any Node exists. Fix PR: [#3249](https://github.com/nanocoai/nanoclaw/pull/3249).

Additional stability fixes are in review:

- [#3247](https://github.com/nanocoai/nanoclaw/pull/3247) — retire malformed cron strings instead of re-erroring on every sweep tick.
- [#3246](https://github.com/nanocoai/nanoclaw/pull/3246) — stop orphan container cleanup from silently no-oping on Windows.
- [#3230](https://github.com/nanocoai/nanoclaw/pull/3230) — stop removal docs from pointing at the retired data/env mirror.

## 6. Feature Requests & Roadmap Signals

No new feature-request issues were filed in this window. The clearest roadmap signal continues to be the Dial channel work from OmriBenShoham:

- [#3041 — Dial channel adapter (SMS + AI voice calls)](https://github.com/nanocoai/nanoclaw/pull/3041)
- [#3050 — Dial in the channel picker, wizard, and runChannelSkill model](https://github.com/nanocoai/nanoclaw/pull/3050)

Both have been open since July 14 and were updated again today; if merged, Dial would become a standard channel in the setup flow. These are the most likely feature candidates for the next release.

## 7. User Feedback Summary

The two new issues reveal real setup pain points:

- **CPU compatibility**: users on AVX2-less Intel processors cannot use the default hardened image, and the failure is a hard `SIGILL` rather than a graceful fallback.
- **Node upgrade path**: users with an existing but too-old Node cannot be repaired by setup because the install helper short-circuits on any existing Node.

No direct satisfaction/dissatisfaction comments were captured, but the fact that users are filing detailed, reproducible issues — and that one user also submitted a fix PR — suggests the community is engaged and invested in setup robustness.

## 8. Backlog Watch

No long-unanswered issues are present in the current two-item issue list; both are only about one day old.

The notable backlog items are the open Dial feature PRs:

- [#3041 — Dial channel adapter](https://github.com/nanocoai/nanoclaw/pull/3041)
- [#3050 — Dial setup picker/wizard integration](https://github.com/nanocoai/nanoclaw/pull/3050)

These have been open for roughly a month and need a maintainer review/merge decision. They were updated again on 2026-08-14, so they are not abandoned, but they are the longest-running open feature work in the current window.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

## NullClaw Project Digest — 2026-08-15

### 1. Today's Overview
NullClaw saw minimal activity over the past 24 hours: no issues were updated, no new releases were published, and one pull request was closed/merged. The only significant change was a configuration enhancement for SQLite-backed memory engines, making the memory database path configurable. While the low issue volume indicates short-term stability, overall development activity is light. The project remains in a maintenance/steady-state phase rather than a high-velocity feature push.

### 2. Releases
No new releases were published in the last 24 hours.

### 3. Project Progress
Merged/closed PRs today:
- **[#986 — GEN-548: make SQLite memory database path configurable](https://github.com/nullclaw/nullclaw/pull/986)**  
  Closed by `gently-whitesnow`. This PR adds a `memory.database_path` setting for SQLite-backed primary memory engines. It preserves the existing default location (`<workspace>/memory.db`) when the setting is empty, and resolves relative paths from the workspace while also supporting absolute paths for read-only workspace deployments. The setting is documented in the example configuration. This is a backward-compatible improvement for deployment flexibility.

### 4. Community Hot Topics
No issues or PRs with meaningful comments or reactions were updated in the last 24 hours. The sole closed PR (#986) has no recorded comments or reactions. There are no active community discussions to highlight at this time.

### 5. Bugs & Stability
No bugs, crashes, or regressions were reported in the last 24 hours. The only change (PR #986) is a configuration enhancement, not a defect fix. No stability concerns were identified.

### 6. Feature Requests & Roadmap Signals
The merged PR #986 signals a roadmap focus on deployment configurability. Allowing `memory.database_path` to be set explicitly — rather than relying on the workspace default — addresses use cases where workspaces are read-only or where memory files must be stored outside the workspace. This suggests future work may continue around storage configuration, custom memory backends, and deployment-oriented settings. No explicit user feature requests appeared in the issue tracker, but the PR itself is a likely response to real deployment pain points.

### 7. User Feedback Summary
No direct user feedback was captured through issues or PR comments in the last 24 hours. The only indirect signal is the motivation behind PR #986: users or internal stakeholders required a configurable SQLite memory path, especially for read-only workspace deployments. This indicates a need for greater control over runtime state location. There is no evidence of dissatisfaction, but also no strong positive sentiment documented in this window.

### 8. Backlog Watch
There are no long-unanswered issues or PRs currently visible. The issue tracker has zero open/active items, and the only PR was promptly closed. No maintainer attention is required for stale or unaddressed work at this time.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-08-15

## 1. Today's Overview

IronClaw is in a high-velocity cycle immediately following the `1.2.0` stable release, with 25 issues and 47 PRs updated in the last 24 hours. The project is splitting roughly evenly between stabilization/QA fixes (Slack, Telegram, extensions, release backports) and forward-looking v1.3.0 feature work focused on reliable automations, unbound-turn execution, and pluggable memory. A notable amount of activity is concentrated around the automation reliability epic (#6879), which is driving multiple structured-execution PRs and sub-issues. Closed-work volume is healthy: 9 issues and 22 PRs closed/merged in the window, indicating the team is converting review and QA cycles into merged changes. Overall project health looks strong, with release-line reconciliation and regression-harness work happening alongside new feature development.

## 2. Releases

**ironclaw-v1.2.0** — released 2026-08-13  
[Release page](https://github.com/nearai/ironclaw/releases/tag/ironclaw-v1.2.0)

Stable promotion of `1.2.0-rc.3`, including fixes validated in RC2/RC3 and the complete RC1 feature set. The visible release-notes fragment calls out:

- Runtime container now installs `curl`, so in-container HTTP healthchecks can execute.
- Orchestrators can probe the worker reliably.

Additional 1.2.0 content can be inferred from related PRs:

- [#7657: Merge the 1.2.0 release line back into main](https://github.com/nearai/ironclaw/pull/7657) — forward-port of state-preserving 1.0/1.1→1.2 startup migrations; release artifact upgrade canaries; Windows filesystem/smoke fixes.
- [#7663: Forward-port 1.2 fixes without legacy migration](https://github.com/nearai/ironclaw/pull/7663) — includes thread-index projection repair, Windows reliability fixes, clean Windows JSON output, and runtime `curl` for healthchecks.

No breaking changes are called out in the provided data. Migration notes point to state-preserving startup migrations from 1.0/1.1 to 1.2.

## 3. Project Progress

Merged/closed PRs in the last 24 hours — primary themes:

- **Extensions & auth diagnostics**  
  [#7668: Surface provider auth diagnostics](https://github.com/nearai/ironclaw/pull/7668) — preserves bounded GitHub provider error messages and stable codes instead of collapsing runtime 401s to generic re-auth context.

- **Hosted MCP OAuth**  
  [#7665: Support origin-scoped hosted MCP OAuth](https://github.com/nearai/ironclaw/pull/7665) — admits the narrowly bounded hosted-MCP OAuth shape used by MKT1 (`/mcp` endpoint + RFC 9728 resource).

- **DB write-pressure measurement**  
  [#7652: Measure production DB write workloads](https://github.com/nearai/ironclaw/pull/7652) — canonical agent turn with 10 built-in capability calls, plus measurement of idle-process journal churn. Supports the DB write-pressure epic.

- **Extensions UI truthfulness**  
  [#7666: Tell the truth on cards and install results](https://github.com/nearai/ironclaw/pull/7666) — fixes the Slack “Reconnect / Finish Setup” false state from #7660 and improves install guidance for device-link installs.

- **CI coverage ratchet**  
  [#7655: Re-pin Slack/Telegram integration coverage floors](https://github.com/nearai/ironclaw/pull/7655) — aligns coverage floors with observed CI output.

- **Telegram linked-device fixes**  
  [#7658: Recognize the 2FA gate on migrated DCs and say where login codes arrive](https://github.com/nearai/ironclaw/pull/7658).

- **Release line reconciliation**  
  [#7657: Merge the 1.2.0 release line back into main](https://github.com/nearai/ironclaw/pull/7657).

Nine issues also closed, including:

- [#7532: Structured execution specs for reliable scheduled automations](https://github.com/nearai/ironclaw/issues/7532)
- [#7592: Per-turn DB write measurement harness](https://github.com/nearai/ironclaw/issues/7592)
- [#7569: Shared SearchField](https://github.com/nearai/ironclaw/issues/7569)
- [#7565: i18n coverage across exposed WebUI routes](https://github.com/nearai/ironclaw/issues/7565)
- [#7183: Per-user LLM model selection](https://github.com/nearai/ironclaw/issues/7183)
- [#6869: Generated DOCX files unreadable by Word](https://github.com/nearai/ironclaw/issues/6869)

## 4. Community Hot Topics

The dataset shows low explicit comment/reaction volume, but the most active and structurally important thread is:

- **[#6879 — Automation runs are hit-or-miss: unattended runs execute as plain interactive chat turns](https://github.com/nearai/ironclaw/issues/6879)**  
  This is the only issue with comment activity in the window and has spawned a dedicated v1.3.0 epic cluster: #7644, #7645, #7646, #7647, plus PRs #7650 and #7651. The underlying need is clear: **scheduled/unattended runs must not degrade into interactive chat behavior**. The project is responding with structured execution contracts, deterministic no-delivery suppression, preflight grant checks, model pinning, and semantic success evaluation.

- **[#7664 — Pluggable memory over MCP: wire the provider, land Mnesis as first consumer](https://github.com/nearai/ironclaw/issues/7664)**  
  This is a significant architectural request: external memory systems should be bindable by configuration rather than compiled into a factory arm. The accompanying draft PR [#7661](https://github.com/nearai/ironclaw/pull/7661) is open and marked XL.

- **[#7656 — Slack-to-Console bridge with interactive Slack response metadata](https://github.com/nearai/ironclaw/issues/7656)**  
  Closed in this window, suggesting the enhancement was accepted or completed. It would tie Slack agent responses back to IronClaw Console threads with deep links and run metadata.

## 5. Bugs & Stability

Ranked by severity/risk:

1. **Extension state leaking between users** — [#7659](https://github.com/nearai/ironclaw/issues/7659)  
   Extensions installed by other users appear as installed on the Extensions/Registry page. This is a multi-tenant isolation concern and is tagged `bug_bash_P2`. No explicit fix PR was visible in the window.

2. **Telegram MP4 attachment failure** — [#7662](https://github.com/nearai/ironclaw/issues/7662)  
   Sending an `.mp4` fails with `invalid_value (attachments.mime_type)` even though the file is recognized as `video/mp4`. Functional bug, no fix PR identified yet.

3. **Telegram phone-mode login code hint incorrect** — [#7667](https://github.com/nearai/ironclaw/issues/7667)  
   On `PHONE_MIGRATE`, the user does not receive the code where expected. Related fix in [#7658](https://github.com/nearai/ironclaw/pull/7658) addresses 2FA on migrated DCs and login-code arrival messaging, but the issue remains open for the `sentCode.type_` raw-TL path.

4. **Slack “Reconnect” / “Finish Setup” false state** — [#7660](https://github.com/nearai/ironclaw/issues/7660)  
   UI shows setup badges despite an active, working Slack connection. Fix merged via [#7666](https://github.com/nearai/ironclaw/pull/7666).

5. **Generated DOCX unreadable by Word** — [#6869](https://github.com/nearai/ironclaw/issues/6869)  
   Closed in this window, presumably fixed. This was an externally reported user-facing corruption bug.

Perf/stability improvement: [#7628](https://github.com/nearai/ironclaw/pull/7628) removes heartbeat journal churn, reducing write pressure in long-lived process loops.

## 6. Feature Requests & Roadmap Signals

The v1.3.0 roadmap is visibly taking shape around **reliable automations**:

- [#7644 — Verify a structured automation once before arming its schedule](https://github.com/nearai/ironclaw/issues/7644)
- [#7645 — Pin an LLM model profile per structured execution contract](https://github.com/nearai/ironclaw/issues/7645)
- [#7646 — Preflight grants and acquire scoped standing approval leases](https://github.com/nearai/ironclaw/issues/7646)
- [#7647 — Deterministic no-delivery outcome for scheduled runs](https://github.com/nearai/ironclaw/issues/7647)

These are implemented/driven by open PRs:

- [#7651 — deterministic no-result suppression](https://github.com/nearai/ironclaw/pull/7651)
- [#7650 — persist semantic execution outcomes](https://github.com/nearai/ironclaw/pull/7650)

Other roadmap signals:

- **Pluggable memory over MCP** — [#7664](https://github.com/nearai/ironclaw/issues/7664), PR [#7661](https://github.com/nearai/ironclaw/pull/7661)
- **Structured Ask User cards in WebUI** — [#7653](https://github.com/nearai/ironclaw/issues/7653)
- **Unbound-turns / prepared-context execution** — [#7562](https://github.com/nearai/ironclaw/pull/7562) and stacked [#7634](https://github.com/nearai/ironclaw/pull/7634)
- **Design-system typing and shared components** — [#7637](https://github.com/nearai/ironclaw/issues/7637), [#7639](https://github.com/nearai/ironclaw/issues/7639), [#7638](https://github.com/nearai/ironclaw/issues/7638)

Prediction: next minor release v1.3.0 is likely to center on **structured, reliable scheduled automations**, with supporting infrastructure in unbound-turn execution, pluggable memory, and improved WebUI component primitives.

## 7. User Feedback Summary

- **Per-user LLM model selection** was explicitly raised in the IronClaw Champions weekly check-in by marketing user Jeremy Koch; issue [#7183](https://github.com/nearai/ironclaw/issues/7183) closed this window, indicating the request has been resolved after being admin-only.
- **DOCX generation corruption** was reported by Davin Basi as a direct comparison with ChatGPT/Claude; issue [#6869](https://github.com/nearai/ironclaw/issues/6869) is now closed, suggesting a fix landed.
- **Automation unreliability** remains a live pain point: [the epic #6879](https://github.com/nearai/ironclaw/issues/6879) references failures on small models (DeepSeek V4 Flash) where stored prompts sometimes produce nothing useful. Users need deterministic outcomes, not model-dependent behavior.
- Internal QA feedback surfaced several product-quality concerns: Telegram MP4 upload failures, extension state leaking across users, false Slack connection state, and Telegram login-code confusion. These are being actively worked.

## 8. Backlog Watch

Open PRs/issues that may need maintainer attention due to age, size, or strategic importance:

- **[#7255 — Evaluate the APDD kit + propose scoped integration](https://github.com/nearai/ironclaw/pull/7255)**  
  Open since 2026-08-05, docs-only governance proposal. Large scope, low risk, still open after 10 days.

- **[#7379 — Deploy public docs from a docs-live branch](https://github.com/nearai/ironclaw/pull/7379)**  
  Open since 2026-08-07, part of the doc-truth series. It addresses docs/release skew and is important for public site accuracy.

- **[#7378 — Doc-fact contract tests for CLI, manifest, Responses](https://github.com/nearai/ironclaw/pull/7378)**  
  Open since 2026-08-07; companion to #7379. Would prevent documentation drift.

- **[#7456 — Make durable storage profile-agnostic](https://github.com/nearai/ironclaw/pull/7456)**  
  Open since 2026-08-10, size XL, risk medium, touches sandbox/CI/docs/dependencies. Long-running architectural PR.

- **[#7562 / #7634 — Unbound-turns base PR and switchover](https://github.com/nearai/ironclaw/pull/7634)**  
  Large stacked PRs central to the prepared-context execution model. Both are open and represent a major execution-architecture shift.

- **[#6879 — Automation reliability epic](https://github.com/nearai/ironclaw/issues/6879)**  
  Open since 2026-07-29 and still the root issue for several v1.3.0 items. It is actively worked, but remains the highest-signal open issue in the project.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# 🦞 LobsterAI Project Digest — 2026-08-15

## 1. Today's Overview

LobsterAI saw a high-velocity merge day: **27 PRs updated, 22 merged/closed**, with only 5 still open. Maintainer activity was concentrated around a major release merge — `release/2026.7.30` (67 commits, 264 files, +24,736/−4,253) landed into `main` via [#2498](https://github.com/netease-youdao/LobsterAI/pull/2498), alongside a companion **2026.8.14 release** shipping new sidebar features. A batch of cowork, OpenClaw, and account/UI fixes also closed out. Issues were quiet (2 open, 0 closed), but one new issue shows growing user pressure for the next major version ("v4pro"). Overall project health is strong, though a reverted fix ([#2422](https://github.com/netease-youdao/LobsterAI/pull/2422)/[#2423](https://github.com/netease-youdao/LobsterAI/pull/2423)) and a stale-bot sweep of March-era PRs suggest some coordination and backlog debt.

## 2. Releases

**New: LobsterAI 2026.8.14**
- `feat(sidebar)`: support check-in and banner carousel ([#2411](https://github.com/netease-youdao/LobsterAI/pull/2411))
- `feat(sidebar)`: add multi-agent task activity filter ([#2418](https://github.com/netease-youdao/LobsterAI/pull/2418))
- Release notes truncated in source data; additional sidebar changes likely included.

**Merged (not yet tagged): Release 2026.7.30** via [#2498](https://github.com/netease-youdao/LobsterAI/pull/2498)
- Introduces **Team Edition account and quota flows**
- Refreshes the **Skills and Connectors experience** across the app
- ⚠️ **Notable scope**: 67 commits across 264 files. The size suggests potential breaking changes in account/quota and skill-management areas; no explicit migration notes were captured in the PR summary — worth confirming before users upgrade.

## 3. Project Progress

**Feature work merged/closed today:**
- **Team Edition release merge** — account/quota flows, Skills/Connectors refresh ([#2498](https://github.com/netease-youdao/LobsterAI/pull/2498))
- **Browser annotation preview** — numbered attachment cards in messages, opened in the artifact panel instead of the generic image modal ([#2490](https://github.com/netease-youdao/LobsterAI/pull/2490))
- **Typography** — default UI/code font-size bump with one-time migration ([#2495](https://github.com/netease-youdao/LobsterAI/pull/2495))

**Fixes merged/closed today:**
- **Cowork**: run state now stays expanded until an answer exists, avoiding misleading empty-duration lines ([#2499](https://github.com/netease-youdao/LobsterAI/pull/2499))
- **Cowork**: badge popovers kept within viewport and above later messages ([#2496](https://github.com/netease-youdao/LobsterAI/pull/2496))
- **Cowork i18n**: improved goal/steer copy ([#2497](https://github.com/netease-youdao/LobsterAI/pull/2497))
- **OpenClaw**: skill entries keyed by frontmatter name so UI toggles work reliably — fixed independently in both [#2483](https://github.com/netease-youdao/LobsterAI/pull/2483) and [#2491](https://github.com/netease-youdao/LobsterAI/pull/2491) (duplicate efforts landed same window)
- **Account**: credits icon style/color alignment ([#2494](https://github.com/netease-youdao/LobsterAI/pull/2494), [#2492](https://github.com/netease-youdao/LobsterAI/pull/2492))
- **Session UI**: export image and card toggle fixes ([#2493](https://github.com/netease-youdao/LobsterAI/pull/2493))
- Stale-bot cleanup: old April PRs (mark-unread [#1228](https://github.com/netease-youdao/LobsterAI/pull/1228), AgentCreateModal Esc/reset [#1231](https://github.com/netease-youdao/LobsterAI/pull/1231)) closed.

⚠️ **Watch item**: [#2422](https://github.com/netease-youdao/LobsterAI/pull/2422) ("fix btw tools") was merged then **reverted** by [#2423](https://github.com/netease-youdao/LobsterAI/pull/2423) the same day — indicates the original fix caused a regression and needs a revised follow-up.

## 4. Community Hot Topics

- **[#2489 — "快更新v4pro！" (Update v4pro soon!)**](https://github.com/netease-youdao/LobsterAI/issues/2489) — *1 comment*. A user explicitly demanding the next major version. Signals strong anticipation and possible dissatisfaction with release cadence. Created and updated same day.
- **[#1154 — Unit tests for commandSafety & coworkMemoryJudge**](https://github.com/netease-youdao/LobsterAI/issues/1154) — *1 comment, marked stale*. Contributor-flagged gap: two safety-critical modules (dangerous-command detection, memory-quality scoring) have zero test coverage. High-value quality request from March that is still unaddressed.
- **[#2374 — Permanent setting to hide sidebar ad banner**](https://github.com/netease-youdao/LobsterAI/pull/2374) — *open since Jul 21, addresses [#2342](https://github.com/netease-youdao/LobsterAI/issues/2342)*. Users can currently only dismiss banners temporarily; this PR adds a Settings → General toggle to permanently disable them. Long unresolved.

## 5. Bugs & Stability

Ranked by severity:

1. **High — OpenClaw skill toggles silently ineffective** 🔴 *Fixed.* Directory/frontmatter mismatch made enable/disable overrides no-ops, misleading users into thinking skills were toggled. Fixes in [#2483](https://github.com/netease-youdao/LobsterAI/pull/2483) and [#2491](https://github.com/netease-youdao/LobsterAI/pull/2491).
2. **Medium — "fix btw tools" regression** 🟠 *Reopened risk.* [#2422](https://github.com/netease-youdao/LobsterAI/pull/2422) merged then fully reverted in [#2423](https://github.com/netease-youdao/LobsterAI/pull/2423). The underlying bug is presumably still present — needs a new correct fix.
3. **Medium — Cowork turn shows misleading failure state** 🟠 *Fixed.* Turns ending mid-wait collapsed into an empty duration line that read as failure ([#2499](https://github.com/netease-youdao/LobsterAI/pull/2499)).
4. **Medium — Badge popovers overflow viewport** 🟠 *Fixed.* Popovers could clip outside the visible area or render under newer messages ([#2496](https://github.com/netease-youdao/LobsterAI/pull/2496)).
5. **Low — Google Gemini `/v1` URL concatenation bug** 🟡 *Still open.* `buildOpenAIChatCompletionsURL` drops the `/` separator, producing broken URLs like `https://generativelanguage.googleapis.comv1beta/...`. Fix PR [#1153](https://github.com/netease-youdao/LobsterAI/pull/1153) has been open since **March 31**.

## 6. Feature Requests & Roadmap Signals

- **v4 Pro release** — [#2489](https://github.com/netease-youdao/LobsterAI/issues/2489) is the loudest signal that users expect the next major version soon. Likely includes or follows the Team Edition/quota work just merged via [#2498](https://github.com/netease-youdao/LobsterAI/pull/2498).
- **Permanent ad-banner opt-out** — [#2374](https://github.com/netease-youdao/LobsterAI/pull/2374) is ready and just needs review; likely lands in the next release.
- **In-session Ctrl+F / Cmd+F search** — [#1155](https://github.com/netease-youdao/LobsterAI/pull/1155) (TreeWalker-based, precise text-node highlighting) has been open since March; an ideal candidate for a cowork UX-focused release.
- **Safety-module unit tests** — [#1154](https://github.com/netease-youdao/LobsterAI/issues/1154) is less a feature and more a quality gate; given the high stakes of `commandSafety` (false negatives → silent `rm -rf`/`git push --force`), this should precede any major release.

**Prediction**: next version will likely include the ad-banner toggle, skill-toggle fix, and the annotation/artifact improvements from today's merge batch; v4pro naming suggests a feature-branded release rather than a date-based one.

## 7. User Feedback Summary

- **Release cadence pressure**: Users are explicitly asking for "v4pro" to ship ([#2489](https://github.com/netease-youdao/LobsterAI/issues/2489)). There's pent-up demand, possibly tied to advertised Team Edition features.
- **Ad fatigue**: Sidebar banner dismissals are temporary, and users want a permanent opt-out — a recurring complaint surfaced in [#2342](https://github.com/netease-youdao/LobsterAI/issues/2342) and addressed by [#2374](https://github.com/netease-youdao/LobsterAI/pull/2374).
- **Trust issue with silent failures**: The OpenClaw skill-toggle bug ([#2483](https://github.com/netease-youdao/LobsterAI/pull/2483), [#2491](https://github.com/netease-youdao/LobsterAI/pull/2491)) eroded confidence in UI state — users thought they enabled skills when nothing changed. This was resolved, but highlights the need for better visual feedback.
- **Contributor concern on safety**: The test-coverage gap in `commandSafety`/`coworkMemoryJudge` ([#1154](https://github.com/netease-youdao/LobsterAI/issues/1154)) is framed as a safety risk, not just code quality — likely to resonate with the community.

## 8. Backlog Watch

Items needing maintainer attention (all without recent activity):

| Item | Age | Why it matters |
|---|---|---|
| [Issue #1154](https://github.com/netease-youdao/LobsterAI/issues/1154) — tests for `commandSafety`/`coworkMemoryJudge` | since Mar 31, now stale | Safety-critical; no coverage at all. High priority for release readiness |
| [PR #1153](https://github.com/netease-youdao/LobsterAI/pull/1153) — Gemini `/v1` URL fix | since Mar 31, now stale | Known bug with a ready fix, sitting for ~4.5 months |
| [PR #1155](https://github.com/netease-youdao/LobsterAI/pull/1155) — in-session Ctrl+F search | since Mar 31, now stale | Fully specced feature; would need rebase/conflict review |
| [PR #2374](https://github.com/netease-youdao/LobsterAI/pull/2374) — permanent ad-banner hide | since Jul 21 | Directly addresses user complaints; needs review/merge |
| [PR #2460](https://github.com/netease-youdao/LobsterAI/pull/2460) / [PR #2465](https://github.com/netease-youdao/LobsterAI/pull/2465) — dependabot major bumps (rimraf 6, vite 8) | since Aug 10 | Major-version dependency updates; need CI validation before auto-merge |

**Health summary**: The project is shipping at a fast clip with strong maintainer throughput, but the March–April stale backlog, a same-day revert ([#2422](https://github.com/netease-youdao/LobsterAI/pull/2422)), and duplicate fix PRs ([#2483](https://github.com/netease-youdao/LobsterAI/pull/2483) vs [#2491](https://github.com/netease-youdao/LobsterAI/pull/2491)) point to process friction worth tightening — ideally before the much-anticipated v4pro release.

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis Project Digest — 2026-08-15

## 1. Today's Overview
Moltis shows minimal public activity in the last 24 hours: no issues were updated, no releases were published, and one pull request remains open. The only movement is continued discussion/update activity around PR #1190, a substantial feature PR adding durable calendar, channel, and email connectors. Overall project health appears stable, though community engagement is low at the moment. The focus is clearly on connector infrastructure rather than bug fixes or user-facing changes today.

## 2. Releases
None.  
No new releases were published in the last 24 hours, and no recent release history is available in the provided data.

## 3. Project Progress
No PRs were merged or closed today.  
The only active PR is:

- **PR #1190 — [OPEN] Add durable calendar, channel, and email connectors**  
  Author: penso | Created: 2026-08-11 | Updated: 2026-08-14  
  URL: https://github.com/moltis-org/moltis/pull/1190

This PR adds provider-neutral connector persistence, atomic snapshots, scheduling, projections, and bounded local full-text search. It introduces read-only CalDAV, Gmail, Himalaya v2, and reusable channel-history datasets with provider-owned schemas and no copied credentials. It also adds provider-scoped trust controls. These are significant architectural additions, but they have not yet been merged.

## 4. Community Hot Topics
The only active item is **PR #1190** (no comments or reactions recorded in the provided data):  
https://github.com/moltis-org/moltis/pull/1190

Underlying needs: the PR appears to respond to a need for long-lived, local-first access to external communication channels — calendar, email, and chat history — without duplicating credentials or coupling to a specific provider. The emphasis on atomic snapshots, scheduling, and local full-text search points toward a durable offline-first data layer for personal AI assistants.

There are no other PRs or issues with meaningful comment/reaction activity to analyze.

## 5. Bugs & Stability
No bugs, crashes, or regressions were reported in the last 24 hours.  
There are no open issues and no hotfix PRs at this time.

## 6. Feature Requests & Roadmap Signals
No new feature requests were filed as issues.  
The clearest roadmap signal is **PR #1190**, which proposes:

- Durable calendar, channel, and email connectors
- Provider-neutral persistence with atomic snapshots
- Bounded local full-text search
- Provider-owned schemas and no copied credentials
- Provider-scoped trust enforcement

If merged, these features would likely appear in the next Moltis release. Given the scope and the current lack of releases, this PR may represent a major upcoming milestone rather than an incremental change.

## 7. User Feedback Summary
No user feedback was available in the last 24 hours — no issues, comments, or reactions were recorded.  
The one open PR suggests maintainers are prioritizing extensibility and provider independence, but there is no direct user satisfaction/dissatisfaction data in this window.

## 8. Backlog Watch
The only item requiring attention is **PR #1190**, which has been open since 2026-08-11 and was last updated on 2026-08-14.  
URL: https://github.com/moltis-org/moltis/pull/1190

This is a large, feature-heavy PR with no recorded comments, approvals, or CI/merge status visible in the provided data. It may need maintainer review or contributor follow-up to keep momentum. No issues are pending, so this PR is the current bottleneck for project advancement.

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw Project Digest — 2026-08-15

Source: GitHub data from `agentscope-ai/QwenPaw` (CoPaw project tracking)

---

## 1. Today's Overview

CoPaw/QwenPaw showed very high maintenance activity on 2026-08-15: **50 issues updated** (38 closed, 12 open), **41 PRs updated** (15 closed/merged, 26 open), and **no new releases**. The large number of closed issues suggests active triage and bug-fix throughput, while the 26 open PRs indicate a possible review/merge bottleneck. The majority of today’s discussion centers on **model-provider compatibility, MCP/tool reliability, Windows desktop UX, and the new skill-system lifecycle**. Several 2.0/2.1 regressions were reported, but many older issues are being closed out. Overall, the project is healthy but distribution-facing polish (auto-update, daemon mode, plugin isolation) remains a clear user pain point.

---

## 2. Releases

**No new releases in the last 24 hours.** No changelog, breaking-change, or migration notes to report.

---

## 3. Project Progress

### Closed/merged PRs in last 24h (notable)
- [#6943 – feat(channels): support interactive configurators for plugin channels](https://github.com/agentscope-ai/QwenPaw/pull/6943) — Restores plugin channel `get_configurator()` support in the interactive CLI channel flow.
- [#6715 – feat(onebot): localize inbound media before agent processing](https://github.com/agentscope-ai/QwenPaw/pull/6715) — Aligns OneBot media handling with AgentScope 2.0 local `DataBlock` pipeline.
- [#2105 – docs: add whisper installation instructions](https://github.com/agentscope-ai/QwenPaw/pull/2105) — Adds local speech-to-text setup docs (`--extras whisper`).
- [#7029 / #7030 / #7031 – skill-system and auto-title-sync iterations](https://github.com/agentscope-ai/QwenPaw/pull/7029) — Closed, likely superseded by newer open versions [#7033](https://github.com/agentscope-ai/QwenPaw/pull/7033) and [#7032](https://github.com/agentscope-ai/QwenPaw/pull/7032).

### In-flight feature/PR work
- [#6940 – native DataPaw app runtime + durable analysis workspace](https://github.com/agentscope-ai/QwenPaw/pull/6940)
- [#7033 – dynamic skill loading + auto-unload + frontmatter fix](https://github.com/agentscope-ai/QwenPaw/pull/7033)
- [#7032 – auto-memory linked chat title refresh + observability](https://github.com/agentscope-ai/QwenPaw/pull/7032)
- [#6969 – fix duplicate MCP tool result when `structuredContent` is present](https://github.com/agentscope-ai/QwenPaw/pull/6969) — fixes [#6958](https://github.com/agentscope-ai/QwenPaw/issues/6958)
- [#6302 – unify provider discovery, model metadata, routing, and agent controls](https://github.com/agentscope-ai/QwenPaw/pull/6302)
- [#5992 – add per-session model overrides](https://github.com/agentscope-ai/QwenPaw/pull/5992)
- [#7035 – organize subagent conversations into groups](https://github.com/agentscope-ai/QwenPaw/pull/7035)
- [#7037 – computer-use: observe related window surfaces](https://github.com/agentscope-ai/QwenPaw/pull/7037)
- [#7024 – fix DashScope audio formatting and fallback retry](https://github.com/agentscope-ai/QwenPaw/pull/7024)

---

## 4. Community Hot Topics

Most-discussed issues updated in the last 24h:

- [#3045 – [Bug] 自动获取模型为什么不可用 (8 comments)](https://github.com/agentscope-ai/QwenPaw/issues/3045)  
  User confusion around automatic model discovery/configuration. Suggests the model-setup UX is still not transparent enough.

- [#2418 – [Question] skills-hub management page (7 comments)](https://github.com/agentscope-ai/QwenPaw/issues/2418)  
  Users want a first-class skills marketplace / manager to discover and install mainstream skills faster.

- [#2846 – Desktop auto-update + Windows icon fix (6 comments)](https://github.com/agentscope-ai/QwenPaw/issues/2846)  
  Recurring Windows pain: reinstall required for updates; Python icon instead of CoPaw icon in taskbar.

- [#2303 – MiniMax provider `check_connection()` calls unsupported `/models` endpoint (6 comments)](https://github.com/agentscope-ai/QwenPaw/issues/2303)  
  Anthropic-compatible provider compatibility bug: connection check fails despite working chat completions.

- [#7010 – No true daemon/background mode for `qwenpaw app` (6 comments)](https://github.com/agentscope-ai/QwenPaw/issues/7010)  
  SSH/script users want a non-blocking daemon mode; command hangs when starting the app remotely.

- [#6405 – MCP tools always "Tool not found" after 2.0 upgrade (6 comments)](https://github.com/agentscope-ai/QwenPaw/issues/6405)  
  Community-wide tooling compatibility concern: MCP tool namespace changed but lookup is failing.

- [#7011 – Console stop request can cancel an active Feishu session under multiple UI sessions (5 comments)](https://github.com/agentscope-ai/QwenPaw/issues/7011)  
  Open, high-severity session-isolation bug affecting real channel workloads.

- [#3002 – CoPaw incompatible with OpenAI Responses API format (5 comments)](https://github.com/agentscope-ai/QwenPaw/issues/3002)  
  Enterprise users hitting 400 errors through Azure OpenAI proxy gateways.

- [#2763 – `/models` and `/model` switch commands (4 comments, 2 👍)](https://github.com/agentscope-ai/QwenPaw/issues/2763)  
  Users want chat-native model listing/switching without opening the backend UI.

---

## 5. Bugs & Stability

Ranked by severity:

### High
- **#7011 [OPEN] – Console stop request cancels active Feishu session (2.1.0)**  
  https://github.com/agentscope-ai/QwenPaw/issues/7011  
  Cross-session identity leakage can cause one UI session’s stop request to kill a live Feishu conversation. Needs urgent session-scoping fix.

- **#7016 [OPEN] – Tool call API returns 404 during streaming session**  
  https://github.com/agentscope-ai/QwenPaw/issues/7016  
  `/api/tool-calls/{session_id}/{tool_call_id}/offload` repeatedly returns `404 Tool call not found`, breaking interactive tool calls.

- **#7025 [OPEN] – QwenPaw Creator plugin disables all other plugins**  
  https://github.com/agentscope-ai/QwenPaw/issues/7025  
  Installing the Creator plugin appears to invalidate/exclude every other plugin. High impact on plugin ecosystem trust.

- **#6958 [OPEN] – FastMCP tool results duplicate data in tool result file**  
  https://github.com/agentscope-ai/QwenPaw/issues/6958  
  When MCP returns both `content` and `structuredContent`, truncation causes two duplicate copies in the tool result file.  
  **Fix PR exists:** [#6969](https://github.com/agentscope-ai/QwenPaw/pull/6969) — needs review/merge.

### Moderate
- **#6972 [CLOSED] – Chrome extension WebSocket drops when sending `tab.create`**  
  https://github.com/agentscope-ai/QwenPaw/issues/6972  
  Browser tool JSON-RPC handling bug; connection succeeds then breaks on command.

- **#6405 [CLOSED] – MCP tools report "Tool not found" after upgrading to 2.0**  
  https://github.com/agentscope-ai/QwenPaw/issues/6405  
  Tool name prefixing changed to `[mcp-key]__[tool_name]`, but lookup still fails.

- **#6951 [CLOSED] – Scroll compression hides pre-compression transcript**  
  https://github.com/agentscope-ai/QwenPaw/issues/6951  
  After `/compact`, only eviction index/retained tail is visible; compressed messages disappear from UI transcript.

- **#6612 [CLOSED] – QwenPaw 2.0.1 incompatible with agentscope 2.0.4.post1**  
  https://github.com/agentscope-ai/QwenPaw/issues/6612  
  Proactive/memory subsystem crashes and tool-permission deadlock caused by upstream API changes.

- **#6197 [CLOSED] – Desktop frozen binary hangs at startup when `nvidia-smi` hangs**  
  https://github.com/agentscope-ai/QwenPaw/issues/6197  
  GPU probe can block startup indefinitely on Windows.

### Low / UX
- **#4832 [CLOSED] – `cmd.exe` window flashes on shell commands (Windows)**  
  https://github.com/agentscope-ai/QwenPaw/issues/4832  
  Missing `CREATE_NO_WINDOW` flag in subprocess creation.

- **#6806 [CLOSED] – qwenpaw-creator: Windows cannot save any model config**  
  https://github.com/agentscope-ai/QwenPaw/issues/6806  
  Every save returns `Internal Server Error`; likely Windows-specific plugin path issue.

- **#2303 [CLOSED] – MiniMax provider connection check 404**  
  https://github.com/agentscope-ai/QwenPaw/issues/2303

- **#3002 [CLOSED] – OpenAI Responses API format rejection**  
  https://github.com/agentscope-ai/QwenPaw/issues/3002

> Note: “CLOSED” means the issue was updated to closed in the last 24h; it does not necessarily confirm a fix was released.

---

## 6. Feature Requests & Roadmap Signals

Strong roadmap signals based on issue/PR volume:

- **Windows desktop auto-update**  
  [#2846](https://github.com/agentscope-ai/QwenPaw/issues/2846), [#3464](https://github.com/agentscope-ai/QwenPaw/issues/3464)  
  Repeated demand. Likely a near-term desktop client improvement.

- **OpenAI Responses API compatibility**  
  [#944](https://github.com/agentscope-ai/QwenPaw/issues/944), [#2737](https://github.com/agentscope-ai/QwenPaw/issues/2737), [#3002](https://github.com/agentscope-ai/QwenPaw/issues/3002)  
  Enterprise/proxy users need support beyond `/v1/chat/completions`. PR [#6302](https://github.com/agentscope-ai/QwenPaw/pull/6302) points in this direction.

- **Per-session model overrides**  
  [#2763](https://github.com/agentscope-ai/QwenPaw/issues/2763), PR [#5992](https://github.com/agentscope-ai/QwenPaw/pull/5992)  
  Users want to switch providers/models per conversation without losing history compatibility.

- **Dynamic skill lifecycle and skills hub**  
  [#2418](https://github.com/agentscope-ai/QwenPaw/issues/2418), PRs [#7033](https://github.com/agentscope-ai/QwenPaw/pull/7033), [#7031](https://github.com/agentscope-ai/QwenPaw/pull/7031)  
  The project is actively building runtime skill loading/unloading and auto-unload hooks.

- **Computer use support**  
  [#5551](https://github.com/agentscope-ai/QwenPaw/issues/5551), PR [#7037](https://github.com/agentscope-ai/QwenPaw/pull/7037)  
  Computer-use observation is expanding to related window surfaces (menus, dialogs).

- **Local GGUF model management**  
  [#6433](https://github.com/agentscope-ai/QwenPaw/issues/6433)  
  Users want in-app download/run of GGUF models instead of managing external local model endpoints.

- **Conversation management improvements**  
  [#4001](https://github.com/agentscope-ai/QwenPaw/issues/4001) — delete single messages  
  [#4436](https://github.com/agentscope-ai/QwenPaw/issues/4436) — split conversation into new sessions  
  Both remain open and low-comment; useful backlog candidates.

---

## 7. User Feedback Summary

Real user pain points visible in the last 24h:

- **Windows update experience is the most repeated complaint**  
  Users are tired of uninstall/reinstall loops ([#2846](https://github.com/agentscope-ai/QwenPaw/issues/2846), [#3464](https://github.com/agentscope-ai/QwenPaw/issues/3464)).

- **Daemon/background mode is missing**  
  SSH/automation users cannot start QwenPaw without the command hanging ([#7010](https://github.com/agentscope-ai/QwenPaw/issues/7010)).

- **MCP/tool trust is fragile after 2.0**  
  Reports of “Tool not found”, 404 tool calls, duplicate tool results, and plugin conflicts ([#6405](https://github.com/agentscope-ai/QwenPaw/issues/6405), [#7016](https://github.com/agentscope-ai/QwenPaw/issues/7016), [#6958](https://github.com/agentscope-ai/QwenPaw/issues/6958), [#7025](https://github.com/agentscope-ai/QwenPaw/issues/7025), [#6972](https://github.com/agentscope-ai/QwenPaw/issues/6972)) indicate a need for stronger regression testing around MCP/plugin integrations.

- **Model configuration remains confusing**  
  Auto model discovery and provider-specific compatibility issues are common ([#3045](https://github.com/agentscope-ai/QwenPaw/issues/3045), [#2303](https://github.com/agentscope-ai/QwenPaw/issues/2303), [#3002](https://github.com/agentscope-ai/QwenPaw/issues/3002)).

- **Chinese-speaking users are a major part of the community**  
  Many issue reports are in Chinese; Chinese UI copy quality matters (e.g. typo report [#7040](https://github.com/agentscope-ai/QwenPaw/issues/7040)).

- **Maintainer responsiveness appears positive**  
  38 issues closed in 24h suggests active triage, but the volume of closed items without visible public fix PRs should be verified.

---

## 8. Backlog Watch

Open items needing maintainer attention:

- **#7011 [OPEN] – Cross-session cancellation bug (2.1.0)**  
  https://github.com/agentscope-ai/QwenPaw/issues/7011  
  High-severity, recently reported, no linked fix PR yet.

- **#7016 [OPEN] – Tool-call 404 during streaming**  
  https://github.com/agentscope-ai/QwenPaw/issues/7016  
  Core API reliability issue; needs root-cause and fix.

- **#7025 [OPEN] – Creator plugin breaks all other plugins**  
  https://github.com/agentscope-ai/QwenPaw/issues/7025  
  Plugin isolation issue; likely blocks Creator adoption.

- **#6958 [OPEN] – FastMCP duplicate tool result**  
  https://github.com/agentscope-ai/QwenPaw/issues/6958  
  **Fix PR #6969 is open** — should be reviewed/merged soon.

- **#4436 [OPEN] – Conversation splitting feature request**  
  https://github.com/agentscope-ai/QwenPaw/issues/4436  
  Open since May 2026 with only 2 comments; still relevant.

- **#4001 [OPEN] – Delete individual messages in chat**  
  https://github.com/agentscope-ai/QwenPaw/issues/4001  
  Open since May 2026; commonly expected chat UX.

- **PRs in review that need maintainer bandwidth**  
  - [#5992 per-session model overrides](https://github.com/agentscope-ai/QwenPaw/pull/5992) — open since July 2026, “Under Review”  
  - [#6302 provider discovery/routing unification](https://github.com/agentscope-ai/QwenPaw/pull/6302) — large architectural PR  
  - [#6908 agentscope dependency bump to 2.0.6](https://github.com/agentscope-ai/QwenPaw/pull/6908) — potentially unblocks compatibility issues  
  - [#6940 DataPaw native app runtime](https://github.com/agentscope-ai/QwenPaw/pull/6940) — first-time contributor, large surface area

---

**Overall assessment:** CoPaw/QwenPaw is in an active development and bug-fix cycle. The community is engaged, issue closure rate is high, and several substantial features are in flight. The main risks are the growing open-PR backlog and the cluster of MCP/plugin/tooling regressions reported against 2.x. The next release will likely need to address desktop auto-update, Responses API compatibility, and MCP/tool reliability to ease current user friction.

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-08-15

## 1. Today's Overview

ZeroClaw saw a very high level of activity in the last 24 hours: 33 issues touched (30 open/active, 3 closed) and 50 PRs touched (47 open, 3 merged/closed). No new releases shipped. The project remains in a design-heavy phase — the most-discussed items are RFCs covering security policy, authentication, goal/turn semantics, and OpenAI-protocol interop, while a parallel stream of bug fixes and CI-hardening PRs keeps moving. Community engagement is strong: top RFCs continue accumulating comments past the 20 mark, and several accepted design items (e.g., #8303, #7155, #7142) are being actively refined. The main health concerns are a large set of open PRs blocked on author action, a queue of `needs-maintainer-review` items, and a multi-month S2 Windows test gap that CI still does not catch.

## 2. Releases

No new releases in this window. The v0.8.5 stabilization line (#9459) remains active through August 30, 2026, with intake frozen since August 4.

## 3. Project Progress

Three PRs were merged/closed in the window; they are outside the top-20 by comment count, so they cannot be itemized from this data. The active PR set shows clear forward motion in several areas:

- **Security hardening:**
  - [#9996](https://github.com/zeroclaw-labs/zeroclaw/pull/9996) — makes action-budget accounting atomic so parallel calls cannot exceed `max_actions_per_hour`.
  - [#9839](https://github.com/zeroclaw-labs/zeroclaw/pull/9839) — blocks direct spellings of irreversible destructive commands.
  - [#9580](https://github.com/zeroclaw-labs/zeroclaw/pull/9580) — hardens built-in HTTP egress on the shared network guard.
- **Provider/correctness fixes:**
  - [#9999](https://github.com/zeroclaw-labs/zeroclaw/pull/9999) — classifies output-limited terminal responses as failures (addresses #9421).
  - [#9707](https://github.com/zeroclaw-labs/zeroclaw/pull/9707) — migrates bare `vision_model_provider` to dotted alias refs.
  - [#9420](https://github.com/zeroclaw-labs/zeroclaw/pull/9420) — adds stored OAuth profile support for Anthropic.
- **Channels & gateway:**
  - [#8443](https://github.com/zeroclaw-labs/zeroclaw/pull/8443) — Matrix single-message progress drafts.
  - [#9997](https://github.com/zeroclaw-labs/zeroclaw/pull/9997) — Telegram provider-grouped, paginated model picker.
  - [#9002](https://github.com/zeroclaw-labs/zeroclaw/pull/9002) — keeps agent turns alive after dashboard viewer disconnect.
- **CI/tooling:** [#9962](https://github.com/zeroclaw-labs/zeroclaw/pull/9962) and [#9985](https://github.com/zeroclaw-labs/zeroclaw/pull/9985) extend provider-aware CI caching and Blacksmith runners; [#9986](https://github.com/zeroclaw-labs/zeroclaw/pull/9986) adds portable agent export; [#9994](https://github.com/zeroclaw-labs/zeroclaw/pull/9994) adds a ZeroCode transcript copy menu.

Closed issues include the vendor memory pitch [#9982](https://github.com/zeroclaw-labs/zeroclaw/issues/9982) (wontfix, closed within ~2 days of creation) and the Telegram tool-call progress feature [#6663](https://github.com/zeroclaw-labs/zeroclaw/issues/6663).

## 4. Community Hot Topics

The most active discussions are all design RFCs with substantial comment counts:

1. [#8303 — RFC: Goal mode v1](https://github.com/zeroclaw-labs/zeroclaw/issues/8303) — 22 comments, 1 👍, accepted, risk:high. The live debate is about a bounded foreground work mode; the revision history explicitly rejects over-coupling into the first delivery (restart handoff, broad channel admission, Web, async child work).
2. [#7155 — RFC: high-risk shell command confirmation + allow/ask/deny policy](https://github.com/zeroclaw-labs/zeroclaw/issues/7155) — 20 comments, accepted, risk:high. Revision 3 narrowed scope to a reconciled shell-policy contract.
3. [#8603 — RFC: ZeroClaw Chat Completions profile](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) — 19 comments, needs-maintainer-review. Client demand is evident: Open WebUI, LobeChat, Continue.dev, Aider, LangChain, and the OpenAI SDK are all named as targets.
4. [#7141 — RFC: pluggable inbound authentication](https://github.com/zeroclaw-labs/zeroclaw/issues/7141) — 16 comments, in-progress/accepted. OIDC + pluggable providers, now at Rev 8.
5. [#7462 — Bug: 74 Windows test failures](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) — 15 comments, accepted, p1. Also the top *bug* discussion; CI only runs tests on Linux.
6. [#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) and [#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488) — runtime-owned conversation sessions and unified attachment architecture, 14 comments each, needs-maintainer-review.

**Underlying needs:** operators want enforceable guardrails (shell policy, security pipeline, egress controls), the ecosystem wants OpenAI-protocol compatibility, and the community wants a deterministic, bounded execution model for multi-turn goals. The RFC volume around security/identity suggests trust and auditability are the dominant product concerns right now.

## 5. Bugs & Stability

Ranked by severity:

- **S1 — workflow blocked:** [#9421](https://github.com/zeroclaw-labs/zeroclaw/issues/9421) — incomplete terminal responses can be reported as successful (p1, in-progress). Fix PR [#9999](https://github.com/zeroclaw-labs/zeroclaw/pull/9999) is open.
- **S2 — degraded behavior:** [#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) — 74 test failures on Windows (Unix-only test commands, path semantics, console encoding); CI's Linux-only Test job misses it (p1, accepted, open since June 10, no fix PR yet).
- **S2 — degraded behavior:** [#9486](https://github.com/zeroclaw-labs/zeroclaw/issues/9486) — high-entropy detector redacts Solana wallet addresses, and `high_entropy_tokens=false` does not stop it on the channel path (p2, accepted).
- **S2 — config:** [#9759](https://github.com/zeroclaw-labs/zeroclaw/issues/9759) — duplicate enabled webhook ports are not rejected by Quickstart (p2, accepted).
- **p1 — silent misconfiguration:** [#9919](https://github.com/zeroclaw-labs/zeroclaw/issues/9919) — Qdrant is silently routed through the MarkdownMemory fallback when storage config is absent (p1, accepted).
- **p1 — test flake:** [#9965](https://github.com/zeroclaw-labs/zeroclaw/issues/9965) — cron custom-shell test hits `ETXTBSY` under the parallel runtime gate, failing unrelated PRs (p1, accepted).
- **S3 — minor:** [#9983](https://github.com/zeroclaw-labs/zeroclaw/issues/9983) — fallback model without vision reports an incorrect cause of error for vision-requiring requests (reported 2026-08-13).

## 6. Feature Requests & Roadmap Signals

- **[#9895 — Telegram /model picker](https://github.com/zeroclaw-labs/zeroclaw/issues/9895)** is already implemented by PR [#9997](https://github.com/zeroclaw-labs/zeroclaw/pull/9997) — likely to land in the next cut.
- **[#9970 — Discord role-based authorization](https://github.com/zeroclaw-labs/zeroclaw/issues/9970)** — allowlist by role ID, additive with user-ID allowlists; reasonable candidate for a near-term channels release.
- **[#7065 — Agent evaluation harness](https://github.com/zeroclaw-labs/zeroclaw/issues/7065)** plus tracker [#9967](https://github.com/zeroclaw-labs/zeroclaw/issues/9967) — replay + live eval with LLM-as-judge; a roadmap-level investment signal.
- **[#9788 — Report active shell dialect in system prompt](https://github.com/zeroclaw-labs/zeroclaw/issues/9788)** — small, blocked, but useful prompt-quality improvement.
- **v0.9.0 security architecture:** [#7142](https://github.com/zeroclaw-labs/zeroclaw/issues/7142) explicitly targets v0.9.0 (runtime-owned security decision pipeline, restrictive overlays), alongside [#7141](https://github.com/zeroclaw-labs/zeroclaw/issues/7141) pluggable auth.
- **Protocol interop:** [#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) (Chat Completions profile) is the strongest external-integration signal and a likely v0.9.x feature if it clears maintainer review.
- The **v0.8.5 stabilization line** ([#9459](https://github.com/zeroclaw-labs/zeroclaw/issues/9459)) continues through August 30.

## 7. User Feedback Summary

Real user pain points are visible in concrete, sometimes vivid, reports:

- A Telegram user running a Solana MCP server cannot state wallet addresses — every address is redacted in outbound messages even with `high_entropy_tokens=false` ([#9486](https://github.com/zeroclaw-labs/zeroclaw/issues/9486)). This indicates real crypto/DeFi usage of the channel path.
- Mobile users find the text-based `/model` command cumbersome; the request for a provider-grouped, paginated picker ([#9895](https://github.com/zeroclaw-labs/zeroclaw/issues/9895)) is a UX-friction signal from a real Telegram workflow.
- Windows-based contributors are effectively locked out of test verification: 74 failures and a CI blind spot ([#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462)).
- Operator trust is affected by silent misbehavior: wrong memory backend selected ([#9919](https://github.com/zeroclaw-labs/zeroclaw/issues/9919)), misleading vision-fallback errors ([#9983](https://github.com/zeroclaw-labs/zeroclaw/issues/9983)), and false success on truncated LLM output ([#9421](https://github.com/zeroclaw-labs/zeroclaw/issues/9421)).
- The community is also self-policing: the ViBo Cloud hosted-memory pitch ([#9982](https://github.com/zeroclaw-labs/zeroclaw/issues/9982)) was rejected as wontfix within two days — a sign of focused maintainership.

## 8. Backlog Watch

Items needing maintainer attention, ranked by age and importance:

- **[#8603 — Chat Completions profile](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)** — 19 comments, `needs-maintainer-review` since early July; high external demand and a common integration ask.
- **[#6971 — Security posture & ingress policy](https://github.com/zeroclaw-labs/zeroclaw/issues/6971)** — 11 comments, `needs-maintainer-review` since May 27.
- **[#6954 — Provenance & reply contract for internal turns](https://github.com/zeroclaw-labs/zeroclaw/issues/6954)** — 11 comments, `needs-maintainer-review` since May 26; revised twice and awaiting a decision.
- **[#9621 — Staged opt-in telemetry](https://github.com/zeroclaw-labs/zeroclaw/issues/9621)** — 3 comments, `needs-maintainer-review` since August 1.
- **High-priority PRs awaiting review:** [#9002](https://github.com/zeroclaw-labs/zeroclaw/pull/9002) (gateway turn lifetime, p1) and [#9281](https://github.com/zeroclaw-labs/zeroclaw/pull/9281) (config set rollback, p1).
- **Stalled large PRs needing author action:** [#9137](https://github.com/zeroclaw-labs/zeroclaw/pull/9137), [#9126](https://github.com/zeroclaw-labs/zeroclaw/pull/9126), [#8443](https://github.com/zeroclaw-labs/zeroclaw/pull/8443), [#9420](https://github.com/zeroclaw-labs/zeroclaw/pull/9420), [#9713](https://github.com/zeroclaw-labs/zeroclaw/pull/9713), [#9707](https://github.com/zeroclaw-labs/zeroclaw/pull/9707) — several carry `needs-author-action` and are over two weeks old.
- **Longest-standing accepted bug without a fix PR:** [#7462](https://github.com/zeroclaw-labs/zeroclaw/issues/7462) (Windows test suite, p1/S2, open since June 10).

The central coordination point remains the maintainer decision queue ([#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)), which tracks RFC acceptance, rejection, deferral, and split follow-ups.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*