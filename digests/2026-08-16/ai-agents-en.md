# OpenClaw Ecosystem Digest 2026-08-16

> Issues: 500 | PRs: 500 | Projects covered: 12 | Generated: 2026-08-15 23:14 UTC

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

# OpenClaw Project Digest — 2026-08-16

## 1. Today's Overview

OpenClaw is in a high-velocity maintenance cycle: 500 issues and 500 PRs were updated in the last 24 hours, with 20 issues closed and 52 PRs merged/closed. One new beta release, v2026.8.1-beta.2, shipped with a security hardening feature (secret egress host binding) plus GPT-5.6 Ultra runtime switching. Activity clusters around three fronts: memory-core reliability (index rebuilds, embedding deadlines, unbounded SQLite growth), agent/gateway stability (compaction anchoring, context assembly, tool-call arg parsing), and channel-specific delivery bugs (Telegram, Feishu, Slack, Discord, Weixin, MS Teams). A P0 billing-cooldown bug remains open alongside a dense P1 backlog, but numerous fix PRs are in flight — a healthy sign overall, though the volume suggests the project is straining under issue load.

## 2. Releases

**v2026.8.1-beta.2** (published 2026-08-16)

- **Secret egress host binding:** each shared-store secret is now bound to exact HTTPS destination hosts across CLI, Gateway RPC, and Control UI; unbound sentinel substitution fails closed before plaintext egress (thanks @shakkernerd). Operators should review this carefully — it changes failure semantics for any secret referenced without an explicit host binding.
- **GPT-5.6 Ultra and runtime switching:** support added (release notes truncated in source data).

No explicit breaking changes or migration notes were published beyond the secret-binding behavior change. This is a beta release; production adoption should be gated accordingly.

## 3. Project Progress

52 PRs were merged/closed in the window. Notable completed and advancing work:

**Merged/closed:**
- **PR #116489** — `feat(security): require acknowledgement for install policy warnings`: external `security.installPolicy` can now return `warn`, requiring an authorized operator to review suspicious plugin/skill installs before continuing. [PR](https://github.com/openclaw/openclaw/pull/116489)
- **PR #120900** — `feat(ui): review install policy warnings`: authenticated admins can review and deliberately continue install-policy warnings from the Control UI. [PR](https://github.com/openclaw/openclaw/pull/120900)
- **Issue #113181 (closed)** — Cron `delivery.mode="none"` + isolated agent silent no-op resolved. [Issue](https://github.com/openclaw/openclaw/issues/113181)

**High-impact fix PRs still open but actively iterating:**
- **PR #124293** — Windows cron jobs never run because the durable fence cannot read a process identity (closes #124125). Critical for Windows operators. [PR](https://github.com/openclaw/openclaw/pull/124293)
- **PR #124138** — Recover double-wrapped `tool_call`/`tool_describe` selectors (P1). [PR](https://github.com/openclaw/openclaw/pull/124138)
- **PR #124267** — Anchor compaction pressure to provider usage instead of transcript heuristics. [PR](https://github.com/openclaw/openclaw/pull/124267)
- **PR #124303** — Track context window provenance across sessions/Codex. [PR](https://github.com/openclaw/openclaw/pull/124303)
- **PR #120248** — Amazon Bedrock: avoid O(n²) tool-call arg re-parsing on large streamed payloads (related #53408). [PR](https://github.com/openclaw/openclaw/pull/120248)
- **PR #117339** — Reject non-binary generated video downloads across Google, xAI, MiniMax, Together, fal, Vydra, OpenRouter, Comfy (related #117227). [PR](https://github.com/openclaw/openclaw/pull/117339)
- **PR #120589** — Backfill tool args when provider skips `input_json_delta` (fixes #120306). [PR](https://github.com/openclaw/openclaw/pull/120589)
- **PR #124214** — Feishu: release per-chat queue lane at turn adoption (closes #54409). [PR](https://github.com/openclaw/openclaw/pull/124214)

## 4. Community Hot Topics

**Most-commented issues (20 comments each):**
- **Issue #121953 [P1]** — Cron agent turns stall on DeepSeek (`deepseek-v4-flash`) because the `[cron:<jobId> <name>]` user-message prefix is served from a lower-priority API edge. Signals cross-provider fragility of prompt prefixes. [Issue](https://github.com/openclaw/openclaw/issues/121953)
- **Issue #91009 [P1]** — Codex PreToolUse native hook relay spawns CPU-bound `openclaw-hooks` processes (~100%+ CPU each) and stalls gateway RPC. Open since June; carries `clawsweeper-recovery-stuck`. [Issue](https://github.com/openclaw/openclaw/issues/91009)

**High-signal threads (13 comments):**
- **Issue #51429 [P2]** — A hardcoded `/Users/wangtao` workspace path was merged and published; users are frustrated ("看起来有人把工作路径hardcode进代码里而且居然被合并发布了"). A QA/release-pipeline signal. [Issue](https://github.com/openclaw/openclaw/issues/51429)
- **Issue #69208 [P1, maintainer]** — Umbrella tracking duplicate transcript/replay/context-assembly bugs across MSTeams, webchat, Telegram, followup queue, and delivery-mirror paths. [Issue](https://github.com/openclaw/openclaw/issues/69208)
- **Issue #38327 [P1]** — "Cannot convert undefined or null to object" regression in 2026.3.2 with google-vertex/gemini-3.1-pro-preview (3 👍). [Issue](https://github.com/openclaw/openclaw/issues/38327)
- **Issue #79902 [P3]** — Companion-friendly SQLite transcript/session seams on the database-first runtime (2 👍). [Issue](https://github.com/openclaw/openclaw/issues/79902)

**Priority outlier:**
- **Issue #70903 [P0]** — Persistent file-based provider cooldown extends `disabledUntil` on repeated failures and survives gateway restarts; users remain blocked for hours after billing recovery. [Issue](https://github.com/openclaw/openclaw/issues/70903)

**Underlying needs:** reliable cross-provider behavior (prompt prefixes, fallback chains, OAuth workspace selection), transparent state migrations, and operational visibility (production stability labels, per-model usage logging).

## 5. Bugs & Stability

**P0:**
- **#70903** — Provider cooldown persists across restarts after 402 billing errors; blocks users even after topping up. No fix PR visible. [Issue](https://github.com/openclaw/openclaw/issues/70903)

**P1 cluster (message loss, crash-loop, regressions):**
- **#91009** — Codex hook relay CPU burn + gateway RPC stall (`clawsweeper-recovery-stuck`). [Issue](https://github.com/openclaw/openclaw/issues/91009)
- **#121953** — DeepSeek cron stall from prefix deprioritization. [Issue](https://github.com/openclaw/openclaw/issues/121953)
- **#38327** — Vertex/Gemini "Cannot convert undefined or null to object" (regression, 3 👍). [Issue](https://github.com/openclaw/openclaw/issues/38327)
- **#41744** — Feishu: read-image tool result loses media before final outbound payload. [Issue](https://github.com/openclaw/openclaw/issues/41744)
- **#119087** — Gateway cold start regressed ~2.5× from 2026.7.1-beta.1 to 2026.7.2-beta.7 on 1-vCPU. [Issue](https://github.com/openclaw/openclaw/issues/119087)
- **#94939** — 6.x migration leaves channel conversation-store SQLite at 0 bytes; orphans references, breaks MS Teams proactive sends. [Issue](https://github.com/openclaw/openclaw/issues/94939)
- **#83337** — Plugin/core version drift after upgrade silently disables channels. [Issue](https://github.com/openclaw/openclaw/issues/83337)
- **#78493** — `sudo openclaw update` creates mixed root/user ownership; doctor overwrites config after EACCES. [Issue](https://github.com/openclaw/openclaw/issues/78493)
- **#92633** — `memory_search corpus="all"` times out while individual corpora succeed. [Issue](https://github.com/openclaw/openclaw/issues/92633)
- **#43374** — All LLM API calls time out simultaneously under 4-agent concurrency despite APIs reachable via curl. [Issue](https://github.com/openclaw/openclaw/issues/43374)
- **#103231** — `claude-cli` backend `ownsNativeCompaction` assumption is false; sessions grow past 200% context, recovery paths fail silently (2 👍). [Issue](https://github.com/openclaw/openclaw/issues/103231)
- **#86214** — Codex app-server client closes mid-turn during image/tool requests with large `logs_2.sqlite`. [Issue](https://github.com/openclaw/openclaw/issues/86214)
- **#123073** — dev-channel update fails: `EUNSUPPORTEDPROTOCOL` on `workspace:*` (updater uses npm, repo requires pnpm). [Issue](https://github.com/openclaw/openclaw/issues/123073)
- **#123799** — Production deployment on 2026.5.12 needs safe upgrade/backport guidance for Codex compact 404. [Issue](https://github.com/openclaw/openclaw/issues/123799)
- **#56653** — Slack `reaction_added`/`reaction_removed` never delivered via Socket Mode (multi-account). [Issue](https://github.com/openclaw/openclaw/issues/56653)
- **#55694** — Agent falls into infinite tool-call retry loop, spamming duplicate messages (Feishu). [Issue](https://github.com/openclaw/openclaw/issues/55694)
- **#79293** — openclaw-weixin proactive sends report success but user sees "请稍后再试"/missing chunks. [Issue](https://github.com/openclaw/openclaw/issues/79293)
- **#118793** — Claude CLI "session limit" error dies with `surface_error` instead of triggering fallback chain. [Issue](https://github.com/openclaw/openclaw/issues/118793)
- **#84662** — Codex app-server stores per-turn OpenClaw runtime context in native history, causing runaway `response.create` input growth. [Issue](https://github.com/openclaw/openclaw/issues/84662)
- **#56693** — Codex OAuth can bind to a deactivated ChatGPT workspace when accounts have multiple workspaces. [Issue](https://github.com/openclaw/openclaw/issues/56693)

**Notable fix PRs in flight:** #124293 (Windows cron), #124267 (compaction pressure), #124303 (context provenance), #120248 (Bedrock), #117339 (video validation), #124138 (tool selectors), #120056 (cron NO_REPLY misclassification), #124214 (Feishu queue lane).

## 6. Feature Requests & Roadmap Signals

**Active, well-specified requests:**
- **#10687 [P2]** — Fully dynamic model discovery for OpenRouter and beyond; current static catalog is a known architectural limitation (3 👍). [Issue](https://github.com/openclaw/openclaw/issues/10687)
- **#79902 [P3]** — SQLite transcript/session seams for companion consumers (2 👍). [Issue](https://github.com/openclaw/openclaw/issues/79902)
- **#13219 [P2]** — Per-model usage logging for cost tracking and model-mix optimization (linked PR open). [Issue](https://github.com/openclaw/openclaw/issues/13219)
- **#6599 [P3]** — `/models test-fallback` command to verify fallback chains without waiting for real failures. [Issue](https://github.com/openclaw/openclaw/issues/6599)
- **#6625 [P2]** — Graceful sub-agent timeout with pre-timeout warning to save progress. [Issue](https://github.com/openclaw/openclaw/issues/6625)
- **#45758 [P3]** — YAML as alternative config format (2 👍). [Issue](https://github.com/openclaw/openclaw/issues/45758)
- **#45771 [P3]** — Built-in pace-aware rate limiting for autonomous agent loops (2 👍). [Issue](https://github.com/openclaw/openclaw/issues/45771)
- **#39343 [P2]** — Image batching / media group buffering at gateway layer for LINE/Telegram albums. [Issue](https://github.com/openclaw/openclaw/issues/39343)
- **#63930 [P2]** — Anthropic advisor tool (beta server-side tool) support. [Issue](https://github.com/openclaw/openclaw/issues/63930)
- **#66252 [P3]** — Per-agent TTS/STT configuration overrides for multi-language setups. [Issue](https://github.com/openclaw/openclaw/issues/66252)
- **#73537 [P2]** — Production-readiness stability labels on releases (2 👍). [Issue](https://github.com/openclaw/openclaw/issues/73537)

**Prediction for next releases:** dynamic model discovery (#10687) and per-model usage logging (#13219) align strongly with the GPT-5.6/runtime-switching theme of v2026.8.1 and are plausible targets for 2026.9.x. YAML config (#45758) and graceful sub-agent timeout (#6625) are older, clearly specified asks that could land as community PRs. Security-install-policy work already merged suggests the v2026.8.x line is emphasizing supply-chain hardening.

## 7. User Feedback Summary

- **Upgrade/migration pain is the dominant theme:** silent cron-store JSON→SQLite migration (#90378), empty conversation-store SQLite after 6.x upgrade breaking MS Teams (#94939), plugin/core version drift (#83337), and production users explicitly requesting backport/upgrade guidance for a known regression (#123799).
- **QA frustration:** the hardcoded `/Users/wangtao` path incident (#51429) drew sharp criticism and undermines trust in the release pipeline; it was reported as "someone merged and published" without review catching it.
- **Positive production sentiment:** #73537 explicitly thanks maintainers — "we've been running it as a family and business assistant … it has genuinely become part of our daily workflow" — while requesting enterprise-readiness labels. Users clearly want to depend on OpenClaw operationally.
- **Chinese-community channel pain:** Feishu media loss (#41744), Weixin false-success sends (#79293), agent retry spam (#55694), and the hardcoded-path bug (#51429) form a cluster of channel-specific reliability complaints from Chinese users.
- **Performance concerns:** gateway cold start 2.5× regression (#119087) and a 10–15s synchronous embedded-run auth stage (#75782) indicate operational overhead problems in constrained (1-vCPU) environments.

## 8. Backlog Watch

**Long-running issues awaiting maintainer/product decisions (open since Feb–Mar):**
- **#6599 [P3]** — `/models test-fallback` command (opened 2026-02-01, needs product decision + maintainer review). [Issue](https://github.com/openclaw/openclaw/issues/6599)
- **#6625 [P2]** — Graceful sub-agent timeout (opened 2026-02-01, needs maintainer review/product decision). [Issue](https://github.com/openclaw/openclaw/issues/6625)
- **#10687 [P2]** — Dynamic model discovery (opened 2026-02-06, 3 👍, needs product decision). [Issue](https://github.com/openclaw/openclaw/issues/10687)
- **#13219 [P2]** — Per-model usage logging (opened 2026-02-10, linked PR open). [Issue](https://github.com/openclaw/openclaw/issues/13219)
- **#38327 [P1]** — Vertex/Gemini regression (opened 2026-03-06, 3 👍, needs live repro). [Issue](https://github.com/openclaw/openclaw/issues/38327)

**PRs waiting on author (stall risk):**
- **#112811** — MSTeams multiple bot accounts (XL, open since 2026-07-23; compatibility/auth-provider/session-state merge risks). [PR](https://github.com/openclaw/openclaw/pull/112811)
- **#120589** — Tool-arg backfill for skipped `input_json_delta` (P1). [PR](https://github.com/openclaw/openclaw/pull/120589)
- **#121760** — Codex: prevent catalog continuation when supervision disabled. [PR](https://github.com/openclaw/openclaw/pull/121760)
- **#124214** — Feishu queue-lane release at turn adoption (closes #54409). [PR](https://github.com/openclaw/openclaw/pull/124214)
- **#120248** — Bedrock O(n²) parsing fix (P1). [PR](https://github.com/openclaw/openclaw/pull/120248)

**Recovery-stuck issues:** #91009, #103231, #70903, #92633, #84662, #86214, #114612, #78493, #77298, #69572 all carry `clawsweeper-recovery-stuck`, indicating the automated triage/recovery pipeline itself is struggling with a subset of hard-to-reproduce, high-severity cases — worth a maintainer deep-dive.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant / Agent Open-Source Ecosystem
**Data window:** 2026-08-16 (24h community digest)

---

## 1. Ecosystem Overview

As of 2026-08-16, the personal AI assistant open-source landscape is dominated by gateway-centric architectures: a durable memory core, a provider-routing layer, channel adapters, and increasingly rich WebUI/desktop shells. OpenClaw remains the reference implementation and the largest community by roughly an order of magnitude, with Hermes Agent and ZeroClaw forming a second tier at comparable issue/PR volume. The broader "Claw" family (NanoClaw, PicoClaw, NullClaw, IronClaw, CoPaw/QwenPaw) and adjacent projects (NanoBot, LobsterAI, Moltis) are concentrating on reliability concerns — memory durability, cron correctness, provider fallbacks, and security hardening — rather than new agent capabilities. No releases shipped this window outside OpenClaw's beta, signaling an ecosystem-wide stabilization phase.

---

## 2. Activity Comparison

*Health score (1–10) = composite of issue/PR velocity, merge/closure ratio, release cadence, and unresolved-severity burden.*

| Project | Issues updated (closed) | PRs updated (merged/closed) | Release in window | Health |
|---|---|---|---|---|
| **OpenClaw** | 500 (20) | 500 (52) | v2026.8.1-beta.2 | 7/10 |
| **Hermes Agent** | 50 (8) | 50 (3) | None | 7/10 |
| **ZeroClaw** | 50 (4) | 50 (6) | None | 6/10 |
| **NanoClaw** | 0 (0) | 22 (3) | None | 6/10 |
| **IronClaw** | 27 (21) | 13 (6) | None | 8/10 |
| **LobsterAI** | 18 (16) | 6 (2 non-Dependabot) | None | 5/10 |
| **CoPaw / QwenPaw** | 10 (1) | 11 (0) | None | 5/10 |
| **NanoBot** | 2 (1) | 16 (7) | None | 8/10 |
| **Moltis** | 0 (0) | 6 (3) | None | 7/10 |
| **NullClaw** | 1 (0) | 1 (0) | None | 5/10 |
| **PicoClaw** | 0 (0) | 0 (0) | None | 4/10 |
| **ZeptoClaw** | 0 (0) | 0 (0) | None | 2/10 |

**Key takeaways:** NanoBot and IronClaw lead on responsiveness-to-throughput balance. OpenClaw and Hermes lose points on open P0/P1 density. PicoClaw and ZeptoClaw score lowest due to stalled or absent contribution flow.

---

## 3. OpenClaw's Position

**Advantages vs. peers:**

- **Only project shipping releases in this window.** v2026.8.1-beta.2 adds secret egress host binding (fail-closed security) and GPT-5.6 Ultra runtime switching.
- **~10× community throughput of the next tier** (500 issues/PRs updated vs. 50 for Hermes/ZeroClaw) — the largest contributor base and bug-discovery surface in the ecosystem.
- **Broadest channel matrix:** Telegram, Feishu, Slack, Discord, Weixin, MS Teams, plus Codex/Claude CLI and Bedrock backends.
- **Security-hardening momentum no peer has matched:** install-policy warnings requiring operator acknowledgment, Control UI review flows, and host-bound secrets.

**Technical approach differences:**

- Deep investment in memory-core reliability (SQLite index rebuilds, compaction anchoring, context provenance tracking) rather than pure chat orchestration.
- Explicit migration/upgrade surface (JSON→SQLite cron stores, 6.x conversation-store migration) — transparent, but a recurring source of user pain and a QA signal (#51429 hardcoded-path incident).

**Risks:** A P0 provider-cooldown bug (#70903) persists with no visible fix; 10 issues carry the `clawsweeper-recovery-stuck` label; Windows cron remains broken (#124293 open). OpenClaw's constraint is triage and migration stability — not feature velocity.

---

## 4. Shared Technical Focus Areas

**Memory & context durability** — the most cross-cutting theme:
- **NanoBot:** consolidation truncation can permanently lose history (#5377).
- **Hermes Agent:** compaction dead-loop on `codex_responses` sessions (#84371).
- **OpenClaw:** compaction anchoring, unbounded SQLite growth, context provenance.
- **IronClaw:** write-amplification reduction across threads/triggers/heartbeats.
- **NullClaw:** compressed history injection + prompt-cache-friendly prefixes (PR #987).
- **ZeroClaw:** RFC to separate authoritative memory storage from enrichment connectors (#9103).

**Cron & scheduling reliability:**
- **OpenClaw:** Windows cron never runs (#124125), DeepSeek cron stall (#121953), NO_REPLY misclassification.
- **NanoBot:** single persistence failure kills the whole scheduler (#5376).
- **Hermes:** lifecycle-guard false positives on `$(( x / y ))`.
- **ZeroClaw:** wall-clock timeout for stuck cron locks; per-job model pinning (#7762).
- **CoPaw:** cron update returns success without applying (#7048).

**Provider routing & resilience:**
- **OpenClaw:** P0 cooldown persistence (#70903), fallback chains, dynamic model discovery (#10687).
- **ZeroClaw:** completed Anthropic refusal/fallback stack (5 issues closed).
- **Hermes:** slow-local-model timeouts, NVIDIA curated-catalog fallback.
- **CoPaw:** OAuth2 refresh-token rotation degrades remote MCP servers (#7053).
- **Moltis:** OpenAI reasoning tool-call routing through Responses API.
- **NanoBot:** OrcaRouter named gateway provider.

**Channel delivery fidelity:**
- **OpenClaw:** Feishu media loss, Weixin false-success, Slack Socket Mode gaps, MSTeams proactive sends.
- **NanoClaw:** Telegram bold rendered as italic; Discord attachments unreadable by agents.
- **Hermes:** Telegram topic routing after gateway restart.
- **CoPaw:** Matrix E2EE missing; video tool-result blocks silently dropped.
- **ZeroClaw:** Discord mention-triggered threads; WeCom proactive messaging.

**Security hardening:**
- **OpenClaw:** secret egress host binding; install-policy warnings + UI review.
- **Hermes:** dangerous-command wrapper bypass (#84551); child-process credential-inheritance epic.
- **ZeroClaw:** webhook audit credential scrubbing; per-agent knowledge/session scoping.
- **LobsterAI:** path traversal in mail skill `downloadAttachments`.

**WebUI / session collaboration:**
- **NanoBot:** drag-and-drop session organization, side conversations, mention-based collaboration.
- **ZeroClaw:** multi-session panes with agent sidebar.
- **Hermes:** desktop state-consistency bugs, Topic-prefixed session sidebar.
- **CoPaw:** virtual scrolling for long conversations; context-strategy selector restoration.

---

## 5. Differentiation Analysis

| Project | Focus | Target users | Architecture signals |
|---|---|---|---|
| **OpenClaw** | General-purpose agent gateway; broadest provider/channel surface; supply-chain security | Self-hosters, teams needing many integrations | Durable memory core, gateway RPC, channel adapters |
| **Hermes Agent** | Desktop + local-model centric; cross-platform (Windows/macOS/Linux/Termux); Discord API v10 parity; strict maintainability discipline | Power users, researchers | Electron desktop, TUI/REST clients, god-file sharding policy |
| **ZeroClaw** | Architecture/RFC-driven platform; OpenAI Chat Completions compatibility; SOP permission contracts | Platform builders, Open WebUI/LobeChat ecosystem | Design-heavy queue, staged telemetry RFC |
| **NanoBot** | Lightweight WebUI-first personal assistant; multi-session UX | Individual productivity users | WebUI collaboration features, session consolidation model |
| **IronClaw** | Performance & technical-debt paydown | Throughput-sensitive deployments | Rust/Wasm runtime, write-amplification reductions, benchmark culture |
| **NanoClaw** | Embeddable agent SDK; container lifecycle; channel adapter seams | Developers embedding agents in products | TypeScript-style SDK, poll-loop management, core-team PR wave |
| **CoPaw / QwenPaw** | Qwen/NetEase ecosystem; desktop analysis runtime | Chinese-market / enterprise users | DataPaw desktop runtime, Matrix E2EE, Chrome remote bridge |
| **LobsterAI** | Desktop console/IDE front-end on OpenClaw gateway | NetEase model subscribers, GUI-first users | Deep OpenClaw integration, member-login gating |
| **Moltis** | Small, maintainer-driven; Slack-native collaboration | Slack-centric teams | Slack live task cards, Coder sandboxes, durable connectors |
| **NullClaw** | Minimal footprint; constrained-network operations | Enterprise/network-restricted users | Zig components, proxy-support request, long-run loop hygiene |
| **PicoClaw** | Lightweight edge-oriented assistant | Low-footprint / device deployments | Go/whatsmeow WhatsApp channel, prefix-cache cost optimization |
| **ZeptoClaw** | — | — | Dormant |

---

## 6. Community Momentum & Maturity

**Tier 1 — Rapid iteration, high volume:**
- **OpenClaw** — only project shipping releases; straining under issue load.
- **Hermes Agent** — high triage/fix velocity; large refactor epic (20/20 god-file sharding) completed.
- **ZeroClaw** — high design throughput; gated by maintainer review queue.

**Tier 2 — Responsive / stabilizing:**
- **IronClaw** — best closure ratio in the window (21/27 issues); aggressively paying down correctness debt.
- **NanoBot** — fastest fix turnaround relative to surface; all seven PRs merged quickly.
- **Moltis** — steady merges; bus-factor risk (single author).
- **NanoClaw** — dense core-team PR wave; no release yet, so fixes not user-visible.

**Tier 3 — Quiet / at risk:**
- **LobsterAI** — stale-bot-dominated; paid-user login blocker (#1903) still open.
- **CoPaw** — incoming bug wave with zero merges; review backlog growing; many first-time-contributor PRs awaiting signal.
- **PicoClaw** — two stale PRs leave WhatsApp channel broken.
- **NullClaw** — new contributions awaiting maintainer acknowledgment >24h.

**Dormant:** ZeptoClaw.

---

## 7. Trend Signals

1. **Memory durability is the ecosystem's largest reliability battleground.** Consolidation truncation (NanoBot), compaction dead-loops (Hermes), migration corruption (OpenClaw), and write amplification (IronClaw) all target the same root cause: context is still treated as an append-only buffer. **Value:** build context management as a transactional subsystem with lossless compression and bounded lifecycle.

2. **Provider resilience is now a core feature, not a nice-to-have.** Refusal routing, fallback chains, cooldown persistence, OAuth rotation, and dynamic model discovery appear across at least six projects. **Value:** multi-provider degradation paths must be tested with tools like `/models test-fallback` (OpenClaw #6599) — otherwise billing/API failures become user-blocking incidents.

3. **Security is moving up the stack.** Egress binding for secrets, install-policy gates, audit-log credential scrubbing, approval-gate bypasses, and credential inheritance closure indicate least-privilege defaults are becoming table stakes. Expect supply-chain signing and capability-scoped permissions (ZeroClaw SOP contract) to follow.

4. **Channel-specific correctness is a moat — and Chinese platforms are a distinct front.** Feishu, Weixin, and WeCom reliability issues cluster separately from Telegram/Slack/Discord gaps. Matrix E2EE demand is emerging as a privacy-driven requirement.

5. **Desktop/packaging reliability gates adoption.** Windows updater self-locks, Linux chrome-sandbox setuid, macOS blank-window/self-update bugs recur across Hermes, OpenClaw, and ZeroClaw. Cross-platform packaging maturity correlates directly with user trust.

6. **The next UX frontier is multi-session collaboration.** Side conversations, drag-and-drop organization, cross-session memory, mention-based agent collaboration, and per-agent scoping are being built in parallel by NanoBot, NanoClaw, ZeroClaw, and Hermes.

7. **Evaluation infrastructure is the biggest open gap.** IronClaw's trajectory benchmark request (#467), OpenClaw's per-model usage logging (#13219), Hermes request/response dumps, and ZeroClaw's telemetry RFC all point to the same conclusion: without trace-level observability, agent quality claims are unverifiable. Expect dedicated investments in benchmarks, traces, and usage analytics in upcoming releases.

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

## 1. Today's Overview

NanoBot saw active development on 2026-08-16 with no new releases published; the latest activity was concentrated in PRs and issue triage. A total of 16 PRs were updated in the last 24 hours, of which 7 are marked closed/merged, and 2 issues were touched (1 open, 1 closed). The busiest themes were WebUI interaction fixes, session-state safety, consolidation/data-loss prevention, and new provider/model-management features. One potentially data-affecting bug remains open — memory consolidation (#5377) — with a fix PR already submitted for review. Overall project health looks strong: maintainers are closing fixes quickly and contributors are actively addressing regressions, security concerns, and UX requests.

## 3. Project Progress

Seven PRs reached closed/merged state in the last 24 hours:

- **[#5371](https://github.com/HKUDS/nanobot/pull/5371) — fix(webui): hide assistant actions until turn end**  
  Hides copy/fork actions while an agent turn is still running, fixing the conflicting-completion-signal bug from [#5368](https://github.com/HKUDS/nanobot/issues/5368).

- **[#5369](https://github.com/HKUDS/nanobot/pull/5369) — fix(plugins): revalidate cached skill roots after package changes**  
  Closes a security/regression gap where in-place plugin package changes could leave cached skill directories readable from a restricted project.

- **[#5370](https://github.com/HKUDS/nanobot/pull/5370) — fix(agent): bound per-session file state lifecycle**  
  Fixes unbounded `FileStates` retention in `FileStateStore`, preventing memory growth for high-cardinality or temporary sessions.

- **[#5376](https://github.com/HKUDS/nanobot/pull/5376) — fix(cron): keep scheduler alive when job-store persistence fails**  
  Prevents a single persistence failure from silently killing the entire cron scheduler.

- **[#5328](https://github.com/HKUDS/nanobot/pull/5328) — feat(providers): add OrcaRouter as a named gateway provider**  
  Adds OrcaRouter, an OpenAI-compatible routing gateway, as a first-class provider.

- **[#5399](https://github.com/HKUDS/nanobot/pull/5399) — fix(webui): clarify model preset display names**  
  Distinguishes a preset’s display label from its stable `/model` command name and localizes the clarification.

- **[#5397](https://github.com/HKUDS/nanobot/pull/5397) — fix(webui): preserve range selection and turn timing**  
  Adds macOS-style Shift range selection in bulk-delete mode and keeps turn timing stable across guidance and projected reasoning runs.

## 4. Community Hot Topics

The only issue with recorded discussion in the last 24 hours is:

- **[#5377](https://github.com/HKUDS/nanobot/issues/5377) — Bug: consolidation truncates archive input but advances past the full message batch**  
  2 comments; no reactions yet. The issue describes a potential data-loss scenario during memory consolidation: the input is truncated for the model, but the session’s consolidation cursor advances past the full original batch. This topic is likely drawing attention because it affects conversation history integrity. A fix PR ([#5379](https://github.com/HKUDS/nanobot/pull/5379)) is already open.

No other issue or PR in this window recorded comments or 👍 reactions, indicating that this update cycle was driven more by contributor code activity than by long discussion threads.

## 5. Bugs & Stability

Ranked by estimated severity:

1. **High — Memory consolidation data-loss risk**  
   [#5377](https://github.com/HKUDS/nanobot/issues/5377) (open): `Consolidator.archive()` truncates the formatted conversation to the model’s input budget, but callers still advance `Session.last_consolidated` over the entire original batch. This can leave messages or suffixes permanently unreachable after consolidation.  
   **Fix PR:** [#5379](https://github.com/HKUDS/nanobot/pull/5379) proposes lossless bounded chunks instead of truncation.

2. **Medium-high — Plugin cache security/regression**  
   [#5369](https://github.com/HKUDS/nanobot/pull/5369) (closed): Cached plugin skill directories were reused without confirming whether the underlying package had changed, meaning an in-place replacement could remain readable from a restricted project. Closed by the same PR.

3. **Medium — Unbounded file-state memory growth**  
   [#5370](https://github.com/HKUDS/nanobot/pull/5370) (closed): `FileStateStore` retained one entry per session for the lifetime of the agent loop, and state could survive lifecycle boundaries like `/new`. Fixed by bounding state lifecycle.

4. **Medium — Cron scheduler can die silently**  
   [#5376](https://github.com/HKUDS/nanobot/pull/5376) (closed): A single persistence failure inside `CronService._on_timer` could permanently kill the scheduler. Fixed by ensuring `_arm_timer()` still runs after save errors.

5. **Low-medium — WebUI completion-signal confusion**  
   [#5368](https://github.com/HKUDS/nanobot/issues/5368) (closed): Copy/fork actions appeared while an agent turn was still running, conflicting with the “Working…” indicator. Fixed by [#5371](https://github.com/HKUDS/nanobot/pull/5371).

## 6. Feature Requests & Roadmap Signals

Open feature areas visible in the current PR queue:

- **Session collaboration**  
  [#5358](https://github.com/HKUDS/nanobot/pull/5358) gives persisted WebUI sessions stable, server-owned `@name` identities and extends the composer mention picker to select peer sessions.

- **Temporary side conversations**  
  [#5364](https://github.com/HKUDS/nanobot/pull/5364) adds `/side` to open transient, parallel side conversations alongside the main WebUI topic.

- **Drag-and-drop session organization**  
  [#5389](https://github.com/HKUDS/nanobot/pull/5389) adds drag-and-drop reordering for standalone sessions and groups, plus session-group creation by dragging one session onto another.

- **Model preset naming unification**  
  [#5400](https://github.com/HKUDS/nanobot/pull/5400) makes preset keys the canonical names across config, WebUI, commands, sessions, fallbacks, and runtime snapshots, with inline rename feedback.

- **Reconnect-safe WebUI mutations**  
  [#5401](https://github.com/HKUDS/nanobot/pull/5401) retries pending mutations after reconnect without double-executing actions, using a bounded replay cache.

- **DashScope native provider support**  
  [#5398](https://github.com/HKUDS/nanobot/pull/5398) adds a `dashscope_native` provider alongside the existing OpenAI-compatible `dashscope` provider, unlocking native thinking-related parameters.

**Likely next-version candidates:** The cluster of WebUI session-management features — side conversations, drag-and-drop organization, and canonical preset naming — looks close to release if remaining conflict labels are resolved. Reconnect-safe mutations are also likely to ship soon, as they directly improve reliability.

## 7. User Feedback Summary

Real pain points expressed through issues and PR descriptions include:

- **Conversation durability:** Users are concerned about losing context through consolidation truncation ([#5377](https://github.com/HKUDS/nanobot/issues/5377)) and about subagent transcripts disappearing entirely ([#5291](https://github.com/HKUDS/nanobot/pull/5291)).
- **WebUI clarity:** The appearance of copy/fork actions while a turn is still running confused users ([#5368](https://github.com/HKUDS/nanobot/issues/5368)).
- **Model preset naming confusion:** Editing a preset named `openai` to display as `minimax` looked like a rename, not a display-label change ([#5399](https://github.com/HKUDS/nanobot/pull/5399), [#5400](https://github.com/HKUDS/nanobot/pull/5400)).
- **Session organization and collaboration:** Requests for side conversations, drag-and-drop grouping, and mention-based session collaboration suggest users want richer, multi-session WebUI workflows.

No explicit satisfaction metrics are available from the GitHub data, but the fast closure of WebUI and stability bugs indicates a responsive maintenance process.

## 8. Backlog Watch

Open PRs needing the most attention based on age and conflict flags:

- **[#5271](https://github.com/HKUDS/nanobot/pull/5271) — fix(session): prevent stale background task saves from overwriting session data**  
  Open since 2026-08-06, labeled `priority: p0` and `conflict`. This is the oldest high-priority open PR and likely needs conflict resolution and maintainer review.

- **[#5291](https://github.com/HKUDS/nanobot/pull/5291) — fix(agent): persist subagent conversation transcripts**  
  Open since 2026-08-07. Addresses a significant usability gap: subagent tool calls and reasoning steps are currently lost after background execution.

- **[#5358](https://github.com/HKUDS/nanobot/pull/5358) — feat(webui): add session collaboration via mentions**  
  Open since 2026-08-12 with no recorded comments, so it may need a maintainer review or design feedback.

- **[#5364](https://github.com/HKUDS/nanobot/pull/5364) — feat(webui): add temporary side conversations**  
  Labeled `conflict`; likely needs rebase before it can be considered for merge.

- **[#5389](https://github.com/HKUDS/nanobot/pull/5389) — feat(webui): add drag-and-drop session organization**  
  Also labeled `conflict`; needs rebase and review.

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-08-16

## 1. Today's Overview

Hermes Agent is in a **high-activity development and triage cycle**: 50 issues and 50 PRs were updated in the last 24 hours, with 42 issues and 47 PRs still open. Two P1 Windows/Linux packaging bugs were prominent this window; one was closed, while a new P2 desktop process-isolation bug already has an open fix PR. No new releases were published. The project continues to show strong momentum in refactoring, security hardening, and platform-support work, though the volume of open P2 stability issues suggests testing around installer/updater flows and desktop state management still needs attention.

## 2. Releases

**No new releases were published in this window.** There are consequently no changelog entries, breaking-change notes, or migration instructions to report.

## 3. Project Progress

Three PRs were closed/merged in this window:

- [PR #87139 — fix(cron): respect explicit Slack channel targets](https://github.com/NousResearch/hermes-agent/pull/87139)  
  Stops explicit `slack:CHANNEL_ID` cron destinations from inheriting the creating thread; thread delivery is only preserved when the target explicitly includes `:THREAD_ID` or uses `origin`. Includes a regression test.

- [PR #66512 — feat: capture model responses next to request dumps (HERMES_DUMP_REQUESTS)](https://github.com/NousResearch/hermes-agent/pull/66512)  
  Adds response capture alongside request dumps, improving model I/O observability. Closes #66530.

- [PR #13746 — fix: stabilize local Hermes UX and provider selection](https://github.com/NousResearch/hermes-agent/pull/13746)  
  Trims Telegram DM session prompt overhead, stabilizes NVIDIA curated catalog selection/fallback, and prevents TUI status-bar wrap ghosting.

Also notable on the issue side:

- [Issue #78647 — Large-file decomposition: 20/20 done](https://github.com/NousResearch/hermes-agent/issues/78647)  
  The repo-wide god-file sharding epic is **closed as complete**. This is a major maintainability milestone aligned with the project’s standing policy that god files are sharded and never reverted.

Several other bug issues were closed this window, including the Windows `cryptography._rust.pyd` updater self-lock (#83569), stale prompt-ordinal state (#69107), desktop `/api/ws` auth boot loop (#85496), repeated TUI/CLI status lines (#70031), and desktop elapsed-time reset (#62158).

## 4. Community Hot Topics

- [Issue #78647 — Large-file decomposition epic (78 comments, closed)](https://github.com/NousResearch/hermes-agent/issues/78647)  
  The most-commented issue by a wide margin. Community attention is strongly focused on architectural cleanup, god-file sharding, and maintainability discipline.

- [Issue #66616 — Skills index stale/degraded watchdog (36 comments, open)](https://github.com/NousResearch/hermes-agent/issues/66616)  
  The automated freshness probe reports the Skills Hub index is 29.8h old against a 26h limit. The long comment thread suggests sustained user/contributor interest in docs infrastructure reliability.

- [Issue #4178 — python-olm build fail (11 comments, 2 👍, closed)](https://github.com/NousResearch/hermes-agent/issues/4178)  
  A build warning during `0.5.0 → 0.6.0` update. Closed, but the reactions indicate users are sensitive to update-path warnings.

- [Issue #51327 — Hermes Desktop silently fails from .desktop launcher without setuid chrome-sandbox (9 comments, open, P1)](https://github.com/NousResearch/hermes-agent/issues/51327)  
  Linux desktop packaging/launch reliability is a visible community pain point.

- [Issue #83569 — Windows updater self-lock on cryptography._rust.pyd (7 comments, closed, P1)](https://github.com/NousResearch/hermes-agent/issues/83569)  
  High engagement around Windows update reliability; closed in this window.

- [Issue #69107 — prompt.submit stale in-memory history / multi-client session conflicts (7 comments, closed, P2)](https://github.com/NousResearch/hermes-agent/issues/69107)  
  Users running both TUI and REST/web clients on the same session hit silent state desync; now closed.

## 5. Bugs & Stability

### Critical / P1

- [Issue #51327 — Hermes Desktop silently fails from .desktop launcher when Electron chrome-sandbox lacks setuid 4755](https://github.com/NousResearch/hermes-agent/issues/51327)  
  Linux desktop launch can fail with no window and no error. **No fix PR is visible in the surfaced data.** This is the highest-severity open desktop packaging bug.

- [Issue #83569 — Windows: hermes update self-locks cryptography._rust.pyd; any cryptography bump fails with OS error 5](https://github.com/NousResearch/hermes-agent/issues/83569)  
  Closed in this window. This removes a major Windows updater failure mode.

### High / P2

- [Issue #84551 — detect_dangerous_command does not unwrap timeout / bash -c wrappers, bypassing the approval gate](https://github.com/NousResearch/hermes-agent/issues/84551)  
  A security-boundary bug: wrapping a dangerous command in `timeout` or `bash -c` can avoid the approval prompt. **No fix PR is visible.**

- [Issue #87295 — Desktop: second launch silently kills the running app's backend](https://github.com/NousResearch/hermes-agent/issues/87295)  
  New P2 bug, reported 2026-08-15. [PR #87314](https://github.com/NousResearch/hermes-agent/pull/87314) is already open and claims to close it.

- [Issue #87292 — Timeouts with slow local models (>16 TPS)](https://github.com/NousResearch/hermes-agent/issues/87292)  
  Local-model users see `WinError 10053` or “Provider has been unresponsive.” [PR #87310](https://github.com/NousResearch/hermes-agent/pull/87310) is open to let slow local reasoning models finish long responses.

- [Issue #87309 — delegate_task hangs ~600s when target CLI rejects --acp](https://github.com/NousResearch/hermes-agent/issues/87309)  
  `delegate_task` against Claude Code CLI without `--acp` support hangs for the full child timeout. **No fix PR is visible.**

- [Issue #75584 — Windows update fails after interrupted install: hermes.exe missing + node_modules ENOTEMPTY](https://github.com/NousResearch/hermes-agent/issues/75584)  
  Open P2 update-path failure on Windows. **No fix PR is visible.**

- [Issue #84371 — Compaction dead-loop: preflight charges full reasoning replay but tail-budget walk excludes it](https://github.com/NousResearch/hermes-agent/issues/84371)  
  Infinite ineffective compaction on some `codex_responses` sessions. **No fix PR is visible.**

- [Issue #87051 — /loop responses delivered outside the active Telegram topic after gateway restart](https://github.com/NousResearch/hermes-agent/issues/87051)  
  Message-delivery correctness bug across gateway restarts. **No fix PR is visible.**

- [Issue #87268 — install.sh --commit with abbreviated SHA silently installs unpinned main and exits 0](https://github.com/NousResearch/hermes-agent/issues/87268)  
  Installer mis-pins on short SHAs; users can end up on unpinned `main`. **No fix PR is visible.**

- [Issue #85868 — macOS live self-update leaves stale renderer, blank reload, and stale quit guard](https://github.com/NousResearch/hermes-agent/issues/85868)  
  Desktop self-update correctness issue on macOS. **No fix PR is visible.**

- [Issue #87200 — Desktop: subagent timeout leaves “computing…” indicator stuck until restart](https://github.com/NousResearch/hermes-agent/issues/87200)  
  Reported on Windows 10 packaged v0.20.1. **No fix PR is visible.**

### Moderate / P3

- [Issue #87280 — cron lifecycle guard false-positives on bash arithmetic division `$(( x / y ))`](https://github.com/NousResearch/hermes-agent/issues/87280)  
  Blocks legitimate cron scripts. **No fix PR is visible.**

- [Issue #84350 — hermes kanban show crashes with “Cannot operate on a closed database”](https://github.com/NousResearch/hermes-agent/issues/84350)  
  Marked as duplicate, but still surfaced as an open crash.

- [Issue #73890 — Desktop right-side Artifacts and Preview leak context across Projects](https://github.com/NousResearch/hermes-agent/issues/73890)  
  Open UX/state-isolation bug in the Desktop workspace.

## 6. Feature Requests & Roadmap Signals

Several meta-issues and feature PRs signal where the project is heading:

- [Issue #79564 — Discord Feature Parity & Alignment Campaign (API v10)](https://github.com/NousResearch/hermes-agent/issues/79564)  
  A broad campaign to align Hermes’ Discord surface with official Discord API v10 / discord.py 2.7.1. This is likely to drive a wave of Discord-related PRs.

- [Issue #82591 — Kanban zero-authority workers, durable publication, safe reclaim, and large-file eradication](https://github.com/NousResearch/hermes-agent/issues/82591)  
  A complete 3-part implementation plan is posted in comments; parts 2/3 are still pending. Signals continued investment in Kanban automation and disciplined PR/landing workflows.

- [Issue #83565 — Child-process credential-inheritance closure epic](https://github.com/NousResearch/hermes-agent/issues/83565)  
  Security-hardening epic anchoring multiple related PRs/issues. Aligns with ongoing work to prevent trusted credentials leaking into untrusted/model-authored child processes.

- [Issue #40306 — Auto reasoning mode (ChatGPT-style)](https://github.com/NousResearch/hermes-agent/issues/40306)  
  User-requested AI behavior: automatically decide when to reason vs. respond directly. Could become a roadmap item if design work is accepted.

- [Issue #86986 — Termux: make native pkg install/upgrade the first-class Android path](https://github.com/NousResearch/hermes-agent/issues/86986)  
  Platform-expansion signal for Android/Termux users.

- [Issue #87267 — Add MAX messenger platform plugin (Russian messenger by VK)](https://github.com/NousResearch/hermes-agent/issues/87267)  
  New messaging-platform request alongside Telegram/WhatsApp/Slack.

Open feature PRs that are likely candidates for upcoming releases:

- [PR #87312 — Capabilities-wide profile scoping + one-click hub installs on the Skills tab](https://github.com/NousResearch/hermes-agent/pull/87312)
- [PR #87311 — Plugin orchestrator worker behavior disclosure](https://github.com/NousResearch/hermes-agent/pull/87311)
- [PR #86948 — Configurable memory provider timeouts](https://github.com/NousResearch/hermes-agent/pull/86948)
- [PR #86625 — Cluster [Topic]-prefixed sessions in Desktop sidebar](https://github.com/NousResearch/hermes-agent/pull/86625)
- [PR #76772 — TUI /widgets subcommand family](https://github.com/NousResearch/hermes-agent/pull/76772)

## 7. User Feedback Summary

- **Windows update/install pain is the loudest recurring theme.** Users report updater self-locks (#83569), interrupted-install corruption (#75584), short-SHA pin failures (#87268), and Termux/Android installation friction (#86986). The closure of #83569 is a concrete win, but #75584 and #87268 remain open.

- **Desktop users are hitting state-consistency and process-lifecycle bugs.** Stale “Thinking” indicators (#50159), stuck “computing…” after subagent timeout (#87200), stale macOS renderers after self-update (#85868), and second-launch backend killing (#87295) all reduce trust in Desktop UI status.

- **Security-conscious users are probing command-safety boundaries.** The dangerous-command wrapper bypass (#84551) and child-process credential inheritance (#83565) show real demand for stricter local security guarantees.

- **Local-model and power users want fewer arbitrary timeouts.** Slow local models are being disconnected (#87292), and delegation to external CLIs like Claude Code can hang for 600 seconds (#87309). Users clearly want configurable, patient execution paths.

- **Multi-client session workflows are real and need better sync.** TUI + REST/web client users reported stale ordinals and invisible session changes (#69107), which is now closed. This use case will likely continue generating requirements.

Overall, users are actively testing Hermes across Windows, macOS, Linux, Android/Termux, and multiple messaging platforms. The volume of bug reports is high, but the project is also closing issues and shipping fix PRs quickly in several cases.

## 8. Backlog Watch

These items look most in need of maintainer attention:

- [Issue #66616 — Skills index is stale or degraded (open since 2026-07-18, 36 comments)](https://github.com/NousResearch/hermes-agent/issues/66616)  
  A long-running automated-infrastructure failure with high engagement. No fix PR is visible in the surfaced data.

- [Issue #51327 — Linux Desktop silent failure due to chrome-sandbox permissions (open since 2026-06-23, P1)](https://github.com/NousResearch/hermes-agent/issues/51327)  
  A P1 desktop packaging bug with no visible fix PR. Easily triggered by `.desktop` launcher users.

- [Issue #50159 — Desktop stuck on stale “Thinking” state after persisted transcript (open since 2026-06-21, P2)](https://github.com/NousResearch/hermes-agent/issues/50159)  
  Long-standing desktop state bug. [PR #78058](https://github.com/NousResearch/hermes-agent/pull/78058) may be related and is still open.

- [Issue #84551 — Dangerous-command classifier bypass via timeout/bash -c wrappers (P2, security)](https://github.com/NousResearch/hermes-agent/issues/84551)  
  A security-boundary bug with no visible fix PR. This should be a priority due to the approval-gate bypass.

- [Issue #75584 — Windows update fails after interrupted install (P2, open since 2026-07-31)](https://github.com/NousResearch/hermes-agent/issues/75584)  
  Another Windows updater reliability bug without a visible fix PR.

- [PR #75154 — fix(update): preserve local-only commits instead of silently discarding them on reset](https://github.com/NousResearch/hermes-agent/pull/75154)  
  Open since 2026-07-31 and marked `needs-decision` with `sweeper:blast-moderate`. This is an important data-safety update-path fix waiting for maintainer decision.

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

## PicoClaw Project Digest — 2026-08-16

### 1. Today's Overview
PicoClaw saw minimal activity in the last 24 hours: zero Issues were updated, zero PRs were merged/closed, and no new releases were published. The only activity signal is two open PRs from contributor `grrowl`, both updated on Aug 15 and marked `stale`. This points to a quiet maintenance phase rather than active development, though the stale labels indicate pending work that needs maintainer attention. Project health appears stable, but the lack of issue/PR closure suggests review velocity is currently low.

### 2. Releases
No releases were published as of 2026-08-16. There are no changelogs, breaking changes, or migration notes to report.

### 3. Project Progress
- **Merged/closed PRs today:** 0.
- No features or fixes advanced into the codebase in the last 24 hours.
- Two open PRs were updated yesterday but remain unmerged:
  - [#3321](https://github.com/sipeed/picoclaw/pull/3321) — `fix(agent): move dynamic context after history to preserve prefix caching`
  - [#3320](https://github.com/sipeed/picoclaw/pull/3320) — `fix(deps): bump whatsmeow to unblock WhatsApp "client outdated (405)"`

### 4. Community Hot Topics
There are no Issues or PRs with significant comment/reaction activity in the last 24 hours; both open PRs have zero comments and zero 👍 reactions. However, the two updated PRs represent the only community/contributor-generated topics:

- **PR #3321 — [Prefix caching optimization](https://github.com/sipeed/picoclaw/pull/3321)**  
  Moves per-request dynamic context behind conversation history to preserve positional prefix caching. Underlying need: reduce token costs and latency for long multi-turn conversations.

- **PR #3320 — [WhatsApp channel fix](https://github.com/sipeed/picoclaw/pull/3320)**  
  Bumps `whatsmeow` to resolve WhatsApp `Client outdated (405)` disconnects. Underlying need: restore reliability for native WhatsApp users, whose channel is currently dead.

### 5. Bugs & Stability
No new bug reports or regression issues were filed today. However, PR #3320 documents an active bug:

- **WhatsApp channel unusable — `client outdated (405)`**  
  The pinned `whatsmeow` version is rejected by WhatsApp, causing socket connection drops and no reconnect.  
  **Severity:** High for WhatsApp users.  
  **Fix status:** A PR exists but is unmerged and marked stale: [#3320](https://github.com/sipeed/picoclaw/pull/3320).

No crashes, memory issues, or regressions were reported in the last 24 hours.

### 6. Feature Requests & Roadmap Signals
No new feature requests were filed. The only roadmap-adjacent signal is **PR #3321**, which proposes an internal architecture improvement for prefix caching. If merged, it could improve performance and cost-efficiency for long conversations in a future release. This is an infrastructure-level optimization rather than a user-facing feature, but it signals direction toward lower operational costs and better handling of large context windows.

### 7. User Feedback Summary
There is no directly captured user feedback in Issues/PRs from the last 24 hours. Indirect signals from the open PRs highlight two real pain points:

- **WhatsApp users** are affected by channel disconnects with no automatic recovery.
- **Heavy/long-context users** may be experiencing higher token usage and latency due to inefficient prefix cache positioning.

Overall satisfaction/dissatisfaction cannot be assessed from current data because no new comments, reactions, or issue reports were recorded.

### 8. Backlog Watch
Two open PRs by `grrowl` are in danger of stalling and need maintainer attention:

- [#3321 — `fix(agent): move dynamic context after history to preserve prefix caching`](https://github.com/sipeed/picoclaw/pull/3321)  
  Open since Aug 7, updated Aug 15, marked stale. Appears to be a focused performance fix.

- [#3320 — `fix(deps): bump whatsmeow to unblock WhatsApp "client outdated (405)"`](https://github.com/sipeed/picoclaw/pull/3320)  
  Open since Aug 7, updated Aug 15, marked stale. Directly addresses a broken WhatsApp channel.

Both PRs should be reviewed, merged, or explicitly closed soon to avoid losing contributor momentum and to restore WhatsApp functionality.

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Project Digest — 2026-08-16

## Today's Overview
High PR activity with no issue-tracker movement and no new releases. 22 PRs were updated in the last 24 hours: 19 remain open, and 3 were closed/merged. Activity is strongly concentrated on core-team infrastructure work — channel adapters, permissions, delivery, cross-session context, and container lifecycle — alongside several community-submitted fixes for heartbeat reliability, Telegram formatting, and Discord attachments. No new issues were filed, indicating maintainer focus is on landing in-flight work rather than triaging new bug reports.

## Releases
No new releases were published in the last 24 hours. Latest release data is empty, so no changelog, breaking-change, or migration notes are available.

## Project Progress
Three PRs were closed/merged in the last 24 hours:

- **[#3268](https://github.com/nanocoai/nanoclaw/pull/3268) — fix(poll-loop): stopped loops leaked their active query's follow-up poller**  
  Fixes a resource/behavioral leak where an aborted poll loop could leave a follow-up poller running from the active query.

- **[#3117](https://github.com/nanocoai/nanoclaw/pull/3117) — feat(skill): add-omarchy-statusbar**  
  Adds a community utility skill for a Waybar status indicator for NanoClaw.

- **[#37](https://github.com/nanocoai/nanoclaw/pull/37) — Rename to DotClaw and switch from WhatsApp to Telegram**  
  A large, long-lived PR proposing a project rename to DotClaw and replacement of WhatsApp with a Telegraf-based Telegram bot. It was closed/merged in this window after being open since February 2026.

## Community Hot Topics
Comment and reaction counts were not available in the data, so this section is based on PR recency, community authorship, and thematic significance.

- **[#3251](https://github.com/nanocoai/nanoclaw/pull/3251) — fix(agent-runner): prevent heartbeat stall during rate-limiting**  
  Addresses a critical reliability issue where heartbeat files stop updating during Claude API rate-limiting, causing false stale-container kills. Authored by a community contributor.

- **[#3250](https://github.com/nanocoai/nanoclaw/pull/3250) — fix(telegram): drop the legacy-Markdown sanitizer**  
  Fixes a user-visible Telegram formatting bug where `**bold**` is rendered as italic.

- **[#2752](https://github.com/nanocoai/nanoclaw/pull/2752) — fix: stage inbound attachments that expose only a url (Discord)**  
  Long-running fix to make Discord attachments actually readable by the agent instead of appearing as bare placeholders.

- **[#3253](https://github.com/nanocoai/nanoclaw/pull/3253) — fix(opencode): honor the group reasoning effort in the model config**  
  Community fix ensuring NanoClaw respects the configured reasoning effort when talking to opencode.

- **[#3260](https://github.com/nanocoai/nanoclaw/pull/3260) — permissions: `decline_notify` unknown-sender policy**  
  Adds a polite-decline mode for unknown senders, reducing admin interruption while still notifying the owner.

**Underlying needs:** stronger container liveness handling, better platform-specific message fidelity, more control over unknown-sender approvals, and smoother multi-agent/back-end integration behavior.

## Bugs & Stability
Ranked by potential severity:

| Severity | Bug | Status / Fix PR |
|---|---|---|
| **High** | Heartbeat can stall for 30+ minutes during API rate-limiting, causing false stale-container kills | Fix open — [#3251](https://github.com/nanocoai/nanoclaw/pull/3251) |
| **High** | Idle container with no heartbeat file is never subject to the absolute-ceiling kill, risking eternal idle resources | Fix open — [#3252](https://github.com/nanocoai/nanoclaw/pull/3252) |
| **Medium** | Inbound batch selection can let context rows crowd out due task rows, so work never reaches the agent | Fix open — [#3254](https://github.com/nanocoai/nanoclaw/pull/3254) |
| **Medium** | Stopped poll loops leak their active query's follow-up poller | Fix closed/merged — [#3268](https://github.com/nanocoai/nanoclaw/pull/3268) |
| **Medium** | Outbound delivery can resolve a sibling adapter-instance row instead of the sender's own channel row | Fix open — [#3255](https://github.com/nanocoai/nanoclaw/pull/3255) |
| **Low/Medium** | Telegram legacy-Markdown sanitizer downgrades bold to italic | Fix open — [#3250](https://github.com/nanocoai/nanoclaw/pull/3250) |
| **Low/Medium** | Inbound Discord attachments are not readable by the agent | Fix open — [#2752](https://github.com/nanocoai/nanoclaw/pull/2752) |

## Feature Requests & Roadmap Signals
Several open core-team PRs point to a deliberate roadmap slice focused on channel flexibility, permissions, and cross-session context:

- **[#3257](https://github.com/nanocoai/nanoclaw/pull/3257)** — Cross-session context: fan-out, DM backfill, echo pruning, and `ncl sessions history`
- **[#3266](https://github.com/nanocoai/nanoclaw/pull/3266)** — Channel registration-card interceptor seam (`registerChannelCardInterceptor`)
- **[#3265](https://github.com/nanocoai/nanoclaw/pull/3265)** — `CreateAgentOptions.suppressCreatedNotify`
- **[#3264](https://github.com/nanocoai/nanoclaw/pull/3264)** — `registerDeliveryBatchPreview` hook for undelivered batches
- **[#3263](https://github.com/nanocoai/nanoclaw/pull/3263)** — Channel registry hot-start for newly registered adapters
- **[#3262](https://github.com/nanocoai/nanoclaw/pull/3262)** — Chat SDK bridge improvements for DM-thread normalization and app-context capture
- **[#3261](https://github.com/nanocoai/nanoclaw/pull/3261)** — Optional channel adapter capabilities: status-bearing `setTyping`, `setThreadTitle`, `setSuggestedPrompts`
- **[#3260](https://github.com/nanocoai/nanoclaw/pull/3260)** — New `decline_notify` unknown-sender policy
- **[#3256](https://github.com/nanocoai/nanoclaw/pull/3256)** — `messaging_groups.detached_at` migration and delivery refusal into detached conversations

The “A1-A4 / C4” naming and `core-team` labels suggest these are part of a planned roadmap. The next release may bundle the channel/permission/delivery improvements plus the high-impact reliability fixes in `#3251`, `#3254`, and `#3255`.

## User Feedback Summary
No issue comments or direct user feedback were captured in the 24-hour window. Inferred pain points from PR descriptions include:

- False stale-container kills during Claude API rate-limiting are disruptive and create distrust in the host-sweep logic ([#3251](https://github.com/nanocoai/nanoclaw/pull/3251)).
- Telegram output formatting is visibly wrong for bold text ([#3250](https://github.com/nanocoai/nanoclaw/pull/3250)).
- Discord media/text attachments are effectively invisible to agents ([#2752](https://github.com/nanocoai/nanoclaw/pull/2752)).
- Multi-identity channel rooms can cause outbound messages to target the wrong bot instance ([#3255](https://github.com/nanocoai/nanoclaw/pull/3255)).
- Skill-apply step captions display wrong ordinals across skipped/multi-skill runs ([#3259](https://github.com/nanocoai/nanoclaw/pull/3259)).

Overall, contributors are investing in stability and platform parity, but the absence of a release means these fixes are not yet user-visible.

## Backlog Watch
No open issues are currently waiting for triage. Long-standing PRs to watch:

- **[#2752](https://github.com/nanocoai/nanoclaw/pull/2752)** — Discord inbound attachment staging fix, open since **2026-06-12**. This is a user-facing Discord gap and has been waiting for maintainer review/merge for over two months.
- **[#37](https://github.com/nanocoai/nanoclaw/pull/37)** — Rename/Telegram-switch PR created **2026-02-02** and closed/merged only this cycle. If not merged, the final disposition should be documented; if merged, it represents a major project direction change without a release note yet.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

# NullClaw Project Digest — 2026-08-16

## 1. Today's Overview

NullClaw is in a quiet, low-activity phase: only 1 open issue and 1 open PR were updated in the last 24 hours, and no releases or merges occurred. Both items were created yesterday (2026-08-15) and remain awaiting maintainer attention, with zero comments or reactions so far. This suggests a maintenance pause rather than active development, though the open PR (#987) indicates community-driven work is still being submitted. The absence of closed/merged PRs means no feature officially advanced to the main branch today. Overall project health looks stable, but the lack of maintainer response on new contributions could become a concern if it persists.

## 2. Releases

No new releases were published in the last 24 hours. This section is omitted.

## 3. Project Progress

**Merged/Closed PRs today:** None. No features or fixes were merged into the main branch.

**Open PR under review:**

- [PR #987 — feat(agent): loop hygiene for long local tool-heavy runs](https://github.com/nullclaw/nullclaw/pull/987) — Submitted by `vernonstinebaker`, this PR targets a real operational pain point: long, tool-heavy local agent runs. Key improvements proposed:
  - Splits the system prompt into a cache-friendly stable prefix + variable datetime tail (`buildStablePrefix`, `buildVariableTail`, `stablePrefixHash`), which should reduce token costs and improve prompt-cache hit rates.
  - Compresses tool outputs before history injection (`result_compress.zig`) while keeping full output visible in observer logs — a memory/context-efficiency win for long runs.
  - Adds per-turn identical-call loop detection (summary truncated in source data), likely to prevent agents from repeating the same tool call indefinitely.

This is a forward-looking PR that addresses agent stability and cost efficiency; it deserves maintainer review.

## 4. Community Hot Topics

Neither item has accrued comments or reactions yet, so there are no "hot" threads by engagement metrics. However, the two active items represent the community's current focus areas:

1. **[Issue #988 — Proxy support (enhancement)](https://github.com/nullclaw/nullclaw/issues/988)** — User `anpic` requests HTTP(s) and SOCKS(5h) proxy support for providers. Zero comments, but the underlying need is clear: users in restricted/corporate/enterprise networks cannot reach provider APIs directly.

2. **[PR #987 — Loop hygiene for long tool-heavy runs](https://github.com/nullclaw/nullclaw/pull/987)** — A substantial contribution from a community member; no review comments yet. It signals community investment in making the agent production-ready for long-running local workloads.

Underlying need analysis: both items point to a shared theme — users are running NullClaw in real-world, constrained environments (corporate proxies, long-running unattended jobs) and need the agent to be more resilient and infrastructure-aware.

## 5. Bugs & Stability

No bugs, crashes, or regressions were reported in the last 24 hours. The sole issue (#988) is an enhancement request, not a defect. There are no stability concerns to rank today. PR #987 can be seen as a preemptive stability improvement (loop prevention), but it is not a fix for a reported bug.

## 6. Feature Requests & Roadmap Signals

- **[Issue #988 — HTTP(s) and SOCKS(5h) proxy support](https://github.com/nullclaw/nullclaw/issues/988)** — Clearly requested for provider connectivity. Likely to be prioritized if NullClaw targets enterprise or network-restricted users. Proxy support is a common and high-value addition; a reasonable candidate for the next minor release (e.g., adding `proxy_url` or `proxy_env` configuration in provider settings).

- **PR #987 signals roadmap direction**: prompt-cache optimization, compressed context history, and loop protection all point toward *long-run cost and reliability* as a roadmap theme. If merged, this could enable heavier local workloads and unattended agent operation, which pairs naturally with the proxy request (both are "production-readiness" features).

Additional signals: no other feature requests were filed in the last 24h, so the maintainers' immediate input queue is small and focused.

## 7. User Feedback Summary

- **Pain point (network):** One user explicitly stated their environment requires HTTP(s)/SOCKS proxy support for providers, indicating deployment behind firewalls/corporate proxies. Without it, the tool is unusable in those settings.
- **Pain point (long runs):** The PR author's work implies frustration with context bloat, prompt-cache inefficiency, and repeated identical tool calls during long local runs.
- **Satisfaction signals:** No merged work, no release, and no reactions/comments — too little data to gauge overall satisfaction. The absence of bug reports is mildly positive but not conclusive.

## 8. Backlog Watch

No long-unanswered items exist yet — both #988 and #987 were created on 2026-08-15 (yesterday). However, they are already candidates for backlog status if maintainers do not respond in the coming days:

- **[Issue #988 (proxy support)](https://github.com/nullclaw/nullclaw/issues/988)** — Small, clear, actionable. High priority for a quick triage response or a "good first issue" label.
- **[PR #987 (loop hygiene)](https://github.com/nullclaw/nullclaw/pull/987)** — Substantial community contribution with no reviewer activity. Needs maintainer review/CI status to avoid contributor churn.

**Recommendation:** Both items should receive at least an acknowledgment or triage within 48 hours, with particular attention to PR #987, since large community PRs that go silent often discourage future contributions.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-08-16

## 1. Today's Overview

IronClaw is in a high-activity cleanup and stabilization phase. In the last 24 hours, 27 issues were updated (21 closed, 6 open) and 13 PRs were updated (6 closed/merged, 7 open), with no new releases published. The main workstreams were database write-amplification reduction (#7628, #7629, #7676), completion of the unbound-turns switchover (#7562, #7634), and follow-up architectural hardening from the #7634 review (#7671–#7674). Overall project health looks positive: most touched issues are closing, and the remaining open items are mostly technical-debt/architecture tasks rather than new user-facing regressions.

## 2. Releases

None.

---

## 3. Project Progress

Six PRs closed/merged in this window:

- [PR #7562 — feat(unbound-turns): design + phase 1 — prepared-context accept door, unbound run lane, kernel binding-ref deletion](https://github.com/nearai/ironclaw/pull/7562)  
  Base PR for the unbound-turns train; includes the design docs and phase-1 implementation.

- [PR #7634 — feat(unbound-turns): complete the switchover to prepared-context turns](https://github.com/nearai/ironclaw/pull/7634)  
  Completes the unbound-turns switchover, including the follow-ups from #7633 and a 71-clause conformance audit of the design docs.

- [PR #7676 — perf(threads): coalesce thread index touches](https://github.com/nearai/ironclaw/pull/7676)  
  Reduces bursty thread-index rewrite amplification; addresses the Tier-1 waste described in #7596.

- [PR #7629 — perf: reduce trigger and outbound state writes](https://github.com/nearai/ironclaw/pull/7629)  
  Gates trigger run-history pruning to the initial fire claim and reduces per-update DB writes; related to #7595.

- [PR #7628 — perf(processes): remove heartbeat journal churn](https://github.com/nearai/ironclaw/pull/7628)  
  Stops permanent heartbeat journal rows and widens the turn-runner heartbeat interval; implements the conservative subset of #7591 (issues #7593 and #7599).

- [PR #7670 — chore(agents): refresh codebase knowledge graph](https://github.com/nearai/ironclaw/pull/7670)  
  CI-generated refresh of the committed codebase-memory bootstrap snapshot.

---

## 4. Community Hot Topics

The most discussed items in this window were:

- [Issue #467 — Trajectory benchmark system for agent quality evaluation](https://github.com/nearai/ironclaw/issues/467) — 4 comments, open  
  Open since March 2026 and still the top-discussed issue. It calls for a benchmark that runs real user scenarios through the real agent loop and evaluates trajectories with hard assertions plus LLM-as-judge. This is a strong signal that evaluation infrastructure is a recognized roadmap gap.

- [Issue #3236 — [Reborn] Define same-thread follow-up and steering policy](https://github.com/nearai/ironclaw/issues/3236) — 3 comments, closed  
  Addresses Reborn's behavior when follow-up input arrives while a turn already owns the active-thread lock: queueing, `/btw` steering, cancellation, blocked-run behavior, and profile composability. Though closed, its discussion reflects the complexity of deterministic run-control semantics.

PR comment counts were not significant in this window; attention was concentrated on the issue side.

---

## 5. Bugs & Stability

Ranked by likely impact:

1. [Issue #7675 — E2E: qa_6c gmail-to-sheet flake cascades across the whole provider-contracts session](https://github.com/nearai/ironclaw/issues/7675)  
   **High** — The scheduled Live Canary has reportedly been red 30/30 runs due to harness defects: an intermittent resource-class capability failure in the Gmail-to-sheet live chat leg, plus a cascade that poisons the rest of the session.  
   **Fix PR exists:** [PR #7679](https://github.com/nearai/ironclaw/pull/7679) is open and targets exactly these harness bugs.

2. [Issue #7671 — Capability dispatch stack pressure: kernel sandbox path still near the test-stack edge](https://github.com/nearai/ironclaw/issues/7671)  
   **Medium-High** — The capability-port decorator chain previously overflowed the default 2 MiB test-thread stack; chain-boxing in #7634 fixed the current suite, but the kernel sandbox path remains close to the edge. Worth fixing before new decorators are added.

3. [Issue #7674 — Architecture tests: symbol-level allowlist for the openai-compat → threads edge](https://github.com/nearai/ironclaw/issues/7674)  
   **Medium** — Not a runtime bug, but a missing architectural pin. The new `ironclaw_openai_compat` → `ironclaw_threads` edge is only gated at crate level, so unintended symbol imports could creep in.

4. [Issue #7673 — BudgetLedger accounting refinements: truncated-launch reconciliation and charge durability](https://github.com/nearai/ironclaw/issues/7673)  
   **Medium-Low** — Two conservative accounting gaps: truncated launch windows can double-charge, and invocation charges are not durable until terminal edges. This errs toward earlier stops, never cap-exceed.

5. [Issue #7672 — Typed ToolChoice: retire the overloaded tool_choice string across providers](https://github.com/nearai/ironclaw/issues/7672)  
   **Low-Medium** — Provider-facing `tool_choice` is a string that overloads mode names and tool names; every encoder string-matches "auto"/"required"/etc. This is error-prone tech debt rather than an active crash.

Also closed in this window:

- [Issue #6821 — IronHub search: free-text matches read as a complete catalog listing](https://github.com/nearai/ironclaw/issues/6821)
- [Issue #6835 — MCP auth failures never raise a re-auth gate](https://github.com/nearai/ironclaw/issues/6835)
- [Issue #5239 — Scheduler treats stale terminal heartbeat as runner failure](https://github.com/nearai/ironclaw/issues/5239)
- [Issue #5237 — Reborn hosted debug logging floods Railway with compiler DEBUG output](https://github.com/nearai/ironclaw/issues/5237)

---

## 6. Feature Requests & Roadmap Signals

No new releases, but the open PRs and issues point clearly at the next feature wave:

- [PR #7651 — feat(automations): add deterministic no-result suppression](https://github.com/nearai/ironclaw/pull/7651) — open, XL  
  Adds model-derived `result_delivery` semantics so automations can suppress notifications unless a match/change/result occurs.

- [PR #7491 — feat(coding): omp core-tool contract + engines + benchmark arm](https://github.com/nearai/ironclaw/pull/7491) — open, XL  
  Consolidates the coding-tool surface into six exact bare names (`read`, `write`, `edit`, `glob`, `grep`, `bash`) and removes old spellings.

- [PR #7516 — feat(webui): operator surface for the IronHub agent link](https://github.com/nearai/ironclaw/pull/7516) — open, by new contributor  
  Adds a WebUI-accessible way to complete the IronHub agent-link flow instead of requiring CLI-only access.

- [PR #7677 — perf(threads): fold message lookup indexes into message rows](https://github.com/nearai/ironclaw/pull/7677) — open  
  Further write-amplification reduction: replaces 1–3 sibling index rows per message with indexed projections.

- [PR #7678 — perf(capabilities): persist invocation state at gate and terminal edges](https://github.com/nearai/ironclaw/pull/7678) — open  
  Keeps capability invocation state worker-local and materializes it only at terminal/blocked edges.

**Prediction for next version:** The just-closed unbound-turns switchover and write-amplification reductions will almost certainly be in the next release. For features, #7651 (automation suppression), #7491 (core-tool contract), and #7516 (WebUI IronHub operator surface) are strong candidates. The #7634 review follow-ups — especially #7672 typed ToolChoice — are likely to land as focused refactors in a subsequent cycle.

---

## 7. User Feedback Summary

The clearest user-facing pain in this window was correctness/trust of agent answers:

- [Issue #6821](https://github.com/nearai/ironclaw/issues/6821) — Asking "what can I install from IronHub" returned 3 tools when the catalog had 18, and 20 of 21 listed skills were not catalog entries. Closed, but this kind of hallucination is a serious trust issue.
- [Issue #4992](https://github.com/nearai/ironclaw/issues/4992) — Railway-hosted Reborn `local-dev` instances could create scheduled automations, but the automation runs failed before any thread was attached due to SSO access mismatch.
- [Issue #5239](https://github.com/nearai/ironclaw/issues/5239) — A stale heartbeat after a completed run was misclassified as a scheduler failure, producing false terminal-failure paths.
- [Issue #5237](https://github.com/nearai/ironclaw/issues/5237) — Setting `IRONCLAW_REBORN_LOG=debug` in production flooded Railway with Wasmtime/Cranelift compiler DEBUG output.

On the positive side, these were all closed in this window. The only newly open user-visible issue is #7675, and that is a live-QA harness flake rather than a product-behavior regression. The broader signal is that maintainers are aggressively paying down correctness debt.

---

## 8. Backlog Watch

- [Issue #467 — Trajectory benchmark system for agent quality evaluation](https://github.com/nearai/ironclaw/issues/467)  
  Open since 2026-03-02, only 4 comments, no linked PR. This is the oldest open issue in the window and a major roadmap item. It needs an owner or a roadmap decision.

- [PR #7491 — omp core-tool contract + engines + benchmark arm](https://github.com/nearai/ironclaw/pull/7491)  
  Open since 2026-08-11, XL, risk: medium. Large cross-cutting change to the coding-tool surface; needs maintainer review and benchmark validation.

- [PR #7516 — WebUI operator surface for the IronHub agent link](https://github.com/nearai/ironclaw/pull/7516)  
  Open since 2026-08-12 by a new contributor. This is a user-visible feature touching WebUI, secrets, and channels; it may need extra reviewer attention due to the new-contributor context.

- [PR #7651 — deterministic no-result suppression for automations](https://github.com/nearai/ironclaw/pull/7651)  
  Open since 2026-08-14, XL. Deliberately scoped to automation behavior; should be reviewed early to avoid conflicting with the current post-unbound-turns stabilization work.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI Project Digest — 2026-08-16

## 1. Today's Overview

In the last 24 hours, LobsterAI showed moderate maintenance activity: 18 issues and 6 PRs were updated, but no new releases were published. The majority of updated issues (16 of 18) are now closed, with only 2 issues remaining open — both carrying the `stale` label. PR activity was dominated by stale Dependabot build updates, while 2 non-Dependabot PRs were closed: one fixing plugin path persistence and one fixing cron/yield behavior. Overall, the project appears to be in a quiet stabilization phase, with no new feature shipments or releases detected.

## 2. Releases

No new releases were published in this window.

## 3. Project Progress

Two closed PRs advanced fixes in the last 24 hours:

- [PR #1879](https://github.com/netease-youdao/LobsterAI/pull/1879) — **fix: preserve manually-added plugin load paths on config sync**  
  Prevents `OpenClawConfigSync.sync()` from discarding user-manually-added `plugins.load.paths` when writing `openclaw.json`. This is important for community plugin workflows such as `memory-lancedb-pro`.

- [PR #2234](https://github.com/netease-youdao/LobsterAI/pull/2234) — **fix(openclaw): cron yield descendant finalization**  
  Fixes sub-agent completion events not driving the parent agent after `sessions_yield`, plus related issues with active requester steering and cron finalization. Includes test coverage for normal parallel, cron parallel, and cron serial sub-agent scenarios.

No other merged functionality was observed.

## 4. Community Hot Topics

The most commented issues were mostly older, now-stale-closed items. The most active were:

- [Issue #1849](https://github.com/netease-youdao/LobsterAI/issues/1849) — **Infinite NO_REPLY / truncated output** (4 comments)  
  User reports tasks being completed early while the model is still producing output, resulting in no data response. Underlying need: reliable streaming and task/output lifecycle synchronization.

- [Issue #1878](https://github.com/netease-youdao/LobsterAI/issues/1878) — **WeChat IM bot cannot input verification code** (4 comments)  
  New WeChat scan flow requires a 6-digit code to be entered in OpenClaw, but the client has no input UI. Underlying need: better IM-channel onboarding and interactive auth support.

- Several issues with 3 comments continued to signal recurring pain points:
  - [Issue #1836](https://github.com/netease-youdao/LobsterAI/issues/1836) — UI design/redesign request
  - [Issue #1903](https://github.com/netease-youdao/LobsterAI/issues/1903) — Member login failures
  - [Issue #1920](https://github.com/netease-youdao/LobsterAI/issues/1920) — Cowork blank loading state
  - [Issue #1988](https://github.com/netease-youdao/LobsterAI/issues/1988) — Forced model switching to NetEase models
  - [Issue #1993](https://github.com/netease-youdao/LobsterAI/issues/1993) — AI engine connection lost
  - [Issue #2017](https://github.com/netease-youdao/LobsterAI/issues/2017) — Local runtime not detected
  - [Issue #2036](https://github.com/netease-youdao/LobsterAI/issues/2036) — Request for OpenClaw gateway `agent:turn`/`agent:loop` events

There were no reactions on any recent items, so issue attention is driven by discussion rather than upvotes.

## 5. Bugs & Stability

The following bugs were reported/active in this window, ranked by severity:

| Severity | Issue | Description | Notes |
| --- | --- | --- | --- |
| High | [Issue #1885](https://github.com/netease-youdao/LobsterAI/issues/1885) | Security: path traversal in mail SKILL `downloadAttachments` due to unsanitized attachment names | Needs immediate security review/fix |
| High | [Issue #1903](https://github.com/netease-youdao/LobsterAI/issues/1903) | Member login repeatedly fails, blocking paid NetEase model access | Open, stale; auth reliability issue |
| High | [Issue #1988](https://github.com/netease-youdao/LobsterAI/issues/1988) | After update, `qwen3.6-plus` is forced to use NetEase built-in model; config override ineffective | Hits paid users/aliyun coding plans |
| High | [Issue #1849](https://github.com/netease-youdao/LobsterAI/issues/1849) | Infinite NO_REPLY or early termination with no UI output | Core response/streaming reliability |
| Medium | [Issue #1993](https://github.com/netease-youdao/LobsterAI/issues/1993) | AI engine connection lost on desktop app; IM bot stable | Desktop runtime stability issue |
| Medium | [Issue #2017](https://github.com/netease-youdao/LobsterAI/issues/2017) | Local run fails: “OpenClaw runtime not detected, run build script first” | Local development/onboarding blocker |
| Medium | [Issue #1878](https://github.com/netease-youdao/LobsterAI/issues/1878) | WeChat scan login cannot complete due to missing verification code input | Channel integration bug |
| Low | [Issue #1971](https://github.com/netease-youdao/LobsterAI/issues/1971) | Virtual scroll breaks with long content (e.g., Mermaid), causing infinite re-render | UI/scroll regression |
| Low | [Issue #2039](https://github.com/netease-youdao/LobsterAI/issues/2039) | `/dreaming on` state not persisted; upstream OpenClaw schema issue | Configuration persistence bug |

No fix PRs were directly attached to the above issues in this window, but [PR #1879](https://github.com/netease-youdao/LobsterAI/pull/1879) and [PR #2234](https://github.com/netease-youdao/LobsterAI/pull/2234) address nearby configuration-persistence and cron/yield correctness problems.

## 6. Feature Requests & Roadmap Signals

Several feature requests and design signals appeared in updated issues:

- [Issue #2046](https://github.com/netease-youdao/LobsterAI/issues/2046) — **Agent memory system** (open)  
  Detailed proposal for session title/metadata persistence, cross-session memory retrieval, and agent self-managed memory. This is the most substantive open product request.

- [Issue #2040](https://github.com/netease-youdao/LobsterAI/issues/2040) and [Issue #2041](https://github.com/netease-youdao/LobsterAI/issues/2041) — **OpenClaw weaknesses and memory constraints**  
  Both are analysis-style issues highlighting memory, security, token cost, and deployment friction. They reinforce memory as the top roadmap signal.

- [Issue #2036](https://github.com/netease-youdao/LobsterAI/issues/2036) — **OpenClaw gateway event broadcasting**  
  Request for `agent:turn`/`agent:loop` events to enable real-time state persistence.

- [Issue #1836](https://github.com/netease-youdao/LobsterAI/issues/1836) — **Professional UI redesign**  
  User dissatisfaction with visual polish compared to competitors.

- [Issue #1880](https://github.com/netease-youdao/LobsterAI/issues/1880) — **Hermes Agent integration**  
  Wants Hermes Agent/OpenClaw as pluggable agents, similar to Open WebUI.

- [Issue #2016](https://github.com/netease-youdao/LobsterAI/issues/2016) — **OpenHuman engine support**  
  General request to add another engine.

Prediction: The next feature release is likely to focus on **agent memory/persistence** given the repeated, detailed community feedback and existing self-evolver/memory infrastructure. UI polish and empty-state improvements ([#1920](https://github.com/netease-youdao/LobsterAI/issues/1920), [#1921](https://github.com/netease-youdao/LobsterAI/issues/1921)) are also plausible quick wins.

## 7. User Feedback Summary

Real user pain points from this batch include:

- **Login and account friction** — member login failures ([#1903](https://github.com/netease-youdao/LobsterAI/issues/1903)) and WeChat IM auth incomplete ([#1878](https://github.com/netease-youdao/LobsterAI/issues/1878)) are blocking usability.
- **Model control frustration** — users cannot override to non-NetEase models without forced fallback ([#1988](https://github.com/netease-youdao/LobsterAI/issues/1988)).
- **Reliability concerns** — AI engine disconnects and output truncation make desktop usage feel unstable ([#1993](https://github.com/netease-youdao/LobsterAI/issues/1993), [#1849](https://github.com/netease-youdao/LobsterAI/issues/1849)).
- **Local development barrier** — missing OpenClaw runtime detection blocks local testing ([#2017](https://github.com/netease-youdao/LobsterAI/issues/2017)).
- **UI dissatisfaction** — users continue to request professional redesign and better empty/loading states ([#1836](https://github.com/netease-youdao/LobsterAI/issues/1836), [#1920](https://github.com/netease-youdao/LobsterAI/issues/1920), [#1921](https://github.com/netease-youdao/LobsterAI/issues/1921)).
- **Security awareness** — at least one user is actively auditing skills for vulnerabilities ([#1885](https://github.com/netease-youdao/LobsterAI/issues/1885)).

Overall satisfaction appears mixed: users value the product’s flexibility but are frustrated by login, model routing, and stability issues.

## 8. Backlog Watch

Items that need maintainer attention:

- [Issue #1903](https://github.com/netease-youdao/LobsterAI/issues/1903) — **Member login failures** (open, stale, updated 2026-08-15)  
  This is a paid-user blocker that remains open. Needs triage and a fix or workaround.

- [Issue #2046](https://github.com/netease-youdao/LobsterAI/issues/2046) — **Agent memory system proposal** (open, stale, updated 2026-08-15)  
  A high-quality product proposal with detailed priorities. Worth converting into a roadmap item.

- [PR #2164](https://github.com/netease-youdao/LobsterAI/pull/2164), [PR #2165](https://github.com/netease-youdao/LobsterAI/pull/2165), [PR #2166](https://github.com/netease-youdao/LobsterAI/pull/2166), [PR #2167](https://github.com/netease-youdao/LobsterAI/pull/2167) — **Dependabot CI updates**  
  All four open since June 15 and now marked stale. They have no comments or reviews. Maintainers should decide whether to merge, close, or refresh these dependency bumps.

- [Issue #2036](https://github.com/netease-youdao/LobsterAI/issues/2036) — **OpenClaw gateway event proposal** (closed as stale)  
  Although closed, the underlying need for real-time event broadcasting may still be relevant to future development.

The high number of stale-closed issues suggests the project may be using automated stale bots heavily; important items like #1903 and #2046 should be manually rescued from the stale process.

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis Project Digest — 2026-08-16

## Today's Overview
Moltis shows a steady, maintainer-driven pulse: no issues were updated in the last 24 hours, while 6 pull requests were active — 3 closed/merged and 3 still open. No new releases were published. The closed PRs are focused fixes and UX improvements, while the open PRs point at broader platform capabilities: remote sandboxes, durable connectors, and Slack-native task surfaces. All PR activity is from a single author (`penso`), suggesting active development but a possible bus-factor risk. Overall, this is a healthy upstream snapshot with no incoming issue load.

## Releases
No new releases in the last 24 hours.

## Project Progress
Three PRs were closed/merged today:

- [PR #1196](https://github.com/moltis-org/moltis/pull/1196) — Fix ClawHub skill search results  
  Removes per-result ClawHub metadata requests that were causing RPC timeouts. Search metadata is now consumed directly, and owner-qualified references are carried through detail, scan, download, and install flows.

- [PR #1197](https://github.com/moltis-org/moltis/pull/1197) — Start agent chats from command palette  
  Adds “Ask agent” as the final item for non-empty command-palette queries, keeps it available while session search is pending, and creates/sends a fresh chat session with the palette query.

- [PR #1198](https://github.com/moltis-org/moltis/pull/1198) — Route OpenAI reasoning tool calls through Responses  
  Ensures OpenAI requests that combine function tools with `reasoning_effort` use the Responses API. Chat Completions behavior is preserved when tools/reasoning are absent, and Responses request construction is shared across streaming and non-streaming paths.

## Community Hot Topics
There were no issues updated and no recorded comments/reactions on PRs, so engagement cannot be ranked by community response. The active PRs still represent the most significant ongoing discussion areas:

- [PR #1199](https://github.com/moltis-org/moltis/pull/1199) — Add Coder remote workspace sandbox support  
  Ephemeral workspaces via REST API and reconnecting PTY WebSockets. This addresses the underlying need for isolated, remote execution environments.

- [PR #1195](https://github.com/moltis-org/moltis/pull/1195) — Add Slack native live task cards  
  Renders plan/task lifecycle updates directly in Slack with per-run privacy protection. Signals a need for richer collaboration surfaces outside the main chat UI.

- [PR #1190](https://github.com/moltis-org/moltis/pull/1190) — Add durable calendar, channel, and email connectors  
  Provider-neutral persistence, snapshots, and read-only CalDAV/Gmail/Himalaya integration. This is a foundational connector infrastructure effort.

## Bugs & Stability
No new issues or bug reports were filed in the last 24 hours. However, two merged PRs address user-facing bugs:

1. **ClawHub skill search timeout** — [PR #1196](https://github.com/moltis-org/moltis/pull/1196)  
   Per-result metadata requests pushed search beyond the RPC timeout. Severity: high because it directly broke skill discovery/install flows. Fix merged/closed.

2. **OpenAI reasoning tool-call routing** — [PR #1198](https://github.com/moltis-org/moltis/pull/1198)  
   Mixed `reasoning_effort` + function-tool requests likely produced incorrect or unsupported behavior. Fix merges/closed.

Both fixes landed today; no unresolved regressions were reported.

## Feature Requests & Roadmap Signals
The open PRs act as the clearest roadmap signals:

- **Coder remote workspace sandbox support** — [PR #1199](https://github.com/moltis-org/moltis/pull/1199)  
  Supports template IDs/names, presets, rich parameters, TTLs, and environment aliases. This is likely a near-term release candidate for users who want ephemeral remote workspaces.

- **Durable calendar, channel, and email connectors** — [PR #1190](https://github.com/moltis-org/moltis/pull/1190)  
  Adds provider-neutral connector persistence, atomic snapshots, scheduling, projections, and local full-text search. This is broader infrastructure and may take longer to converge.

- **Slack native live task cards** — [PR #1195](https://github.com/moltis-org/moltis/pull/1195)  
  Exposes native streaming tool lifecycle events as Slack cards, with privacy-aware opaque run IDs. A strong candidate for the next minor release.

## User Feedback Summary
Direct user feedback data is unavailable, but PR motivations reveal likely pain points:

- ClawHub skill search was too slow or unreliable, causing user-facing timeouts.
- OpenAI users combining reasoning controls with function tools needed a compatible API path.
- Users wanted a faster way to start agent chats from the command palette.
- There is continued demand for remote execution sandboxes and visible task tracking inside Slack.

No strong dissatisfaction signals are visible; the merged fixes suggest responsive maintainer support.

## Backlog Watch
No issues are sitting unanswered. Among open PRs, the main items awaiting merge/review are:

- [PR #1190](https://github.com/moltis-org/moltis/pull/1190) — Open since 2026-08-11, updated 2026-08-15. Longest-open PR; large connector infrastructure change. Needs careful review.
- [PR #1195](https://github.com/moltis-org/moltis/pull/1195) — Open since 2026-08-15; Slack card feature.
- [PR #1199](https://github.com/moltis-org/moltis/pull/1199) — Open since 2026-08-15; Coder sandbox support.

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw / QwenPaw Project Digest — 2026-08-16

## 1. Today's Overview
As of 2026-08-16, CoPaw / QwenPaw shows **moderate-high activity**: 10 issues were updated (9 open, 1 closed), 11 PRs were updated (all still open), and there were **no releases**. The last 24 hours are heavily weighted toward bug reports against v2.1.0 and a wave of first-time-contributor fixes still awaiting review. No PRs were merged or closed, so the project currently has a growing review backlog rather than a clear merge cadence.

## 2. Releases
**No new releases in the last 24 hours.**  
There is no changelog, no breaking-change notice, and no migration note to report.

## 3. Project Progress
No PRs were merged or closed in the last 24 hours. However, several notable open PRs advanced:

- [#7061 fix(video): deliver tool-result videos on OpenAI Responses API](https://github.com/agentscope-ai/QwenPaw/pull/7061) — targets #7059 and the video-promotion defects for OpenAI-compatible providers.
- [#7055 fix(cli): sync top-level text on agent cron --text update](https://github.com/agentscope-ai/QwenPaw/pull/7055) — fixes #7048.
- [#7049 feat(chats): add limit/before pagination to GET /chats/{chat_id}](https://github.com/agentscope-ai/QwenPaw/pull/7049) — enables future lazy-loading of chat history.
- [#7050 feat(console): add per-cron-job model override picker](https://github.com/agentscope-ai/QwenPaw/pull/7050)
- [#7054 feat(chrome): support remote bridge endpoint for LAN/network browsers](https://github.com/agentscope-ai/QwenPaw/pull/7054)
- [#7033 feat(skill-system): dynamic skill loading + auto-unload + frontmatter fix](https://github.com/agentscope-ai/QwenPaw/pull/7033)
- [#6940 feat(pawapp): add native DataPaw app runtime and durable analysis workspace](https://github.com/agentscope-ai/QwenPaw/pull/6940)
- [#6302 feat: unify provider discovery, model metadata, routing, and agent controls](https://github.com/agentscope-ai/QwenPaw/pull/6302)
- [#6623 fix(acp): prevent final text loss when notifications race the prompt response](https://github.com/agentscope-ai/QwenPaw/pull/6623)
- [#7001 feat(matrix): isolate session and memory per sender in group rooms](https://github.com/agentscope-ai/QwenPaw/pull/7001)
- [#7057 fix(shell): add user-local bin dirs to subprocess PATH](https://github.com/agentscope-ai/QwenPaw/pull/7057)

## 4. Community Hot Topics
The most commented items in the last 24 hours were:

- [Issue #6476: Matrix end-to-end encryption unavailable](https://github.com/agentscope-ai/QwenPaw/issues/6476) — **3 comments, closed**. Users walked through manual install steps for `libolm` and `matrix-nio[e2e]`, showing demand for a more turnkey Matrix E2EE setup.
- [Issue #3915: Introduce virtual scrolling for Console WebUI](https://github.com/agentscope-ai/QwenPaw/issues/3915) — **3 comments, 1 👍**. A long-standing performance complaint about full DOM rendering of long conversations.

The remaining updated issues each have 1 comment, but the underlying themes are clear: users are hitting **silent failures**, **missing UI options**, and **remote/enterprise integration pain** around OAuth2, video context, background tasks, and plugin permissions.

## 5. Bugs & Stability
Bugs reported today, ranked by severity:

- **High — [Issue #7059: `view_video` tool-result video blocks are silently dropped](https://github.com/agentscope-ai/QwenPaw/issues/7059)**  
  The model never receives video frames via OpenAI Responses API / Volcengine Ark. No error or warning is shown.  
  *Fix PR exists: [#7061](https://github.com/agentscope-ai/QwenPaw/pull/7061).*

- **High — [Issue #7053: OAuth2 refresh never renews refresh_token, no proactive renewal](https://github.com/agentscope-ai/QwenPaw/issues/7053)**  
  Remote MCP servers using rotating refresh tokens permanently degrade to manual re-auth after the first refresh.  
  *No fix PR yet.*

- **Medium — [Issue #7060: `view_video` inline cap hardcoded to 2 MB](https://github.com/agentscope-ai/QwenPaw/issues/7060)**  
  The configured provider `max_inline_media_bytes` is ignored on the video path; larger videos are replaced with a placeholder.  
  *Related fix likely covered by #7061, but no separate PR.*

- **Medium — [Issue #7051: Image attachments lost on Console session reload](https://github.com/agentscope-ai/QwenPaw/issues/7051)**  
  Images render correctly when first sent, but after reopening a chat the history shows broken thumbnails.  
  *No fix PR yet.*

- **Medium — [Issue #7048: `qwenpaw cron update --text` returns success but prompt never updates](https://github.com/agentscope-ai/QwenPaw/issues/7048)**  
  Silent CLI failure on agent-type cron jobs.  
  *Fix PR exists: [#7055](https://github.com/agentscope-ai/QwenPaw/pull/7055).*

- **Closed — [Issue #6476: Matrix E2EE unavailable](https://github.com/agentscope-ai/QwenPaw/issues/6476)**  
  Closed after user-driven workaround attempts; still signals missing out-of-box Matrix crypto support.

## 6. Feature Requests & Roadmap Signals
New or recurring feature signals from the last 24 hours:

- [Issue #7058: Restore native context-strategy option in the web UI](https://github.com/agentscope-ai/QwenPaw/issues/7058) — backend supports `native` and `scroll`, but the v2.1.0 UI removed the selector.
- [Issue #7056: Background task callback / notification mechanism](https://github.com/agentscope-ai/QwenPaw/issues/7056) — users want event-driven completion instead of polling.
- [Issue #7052: Plugin API `system_prompt` permission](https://github.com/agentscope-ai/QwenPaw/issues/7052) — enterprise/corporate plugin authors want to hide internal prompts from end users.
- [Issue #3915: Virtual scrolling for Console WebUI](https://github.com/agentscope-ai/QwenPaw/issues/3915) — recurring performance request for long conversations.

Open PRs also point to roadmap direction:

- Native DataPaw desktop/analysis workspace ([#6940](https://github.com/agentscope-ai/QwenPaw/pull/6940))
- Unified provider discovery, routing, and model metadata ([#6302](https://github.com/agentscope-ai/QwenPaw/pull/6302))
- Dynamic skill lifecycle ([#7033](https://github.com/agentscope-ai/QwenPaw/pull/7033))
- Per-cron model overrides ([#7050](https://github.com/agentscope-ai/QwenPaw/pull/7050))

Prediction: the next minor release is likely to include the video-delivery fix ([#7061](https://github.com/agentscope-ai/QwenPaw/pull/7061)), the cron update fix ([#7055](https://github.com/agentscope-ai/QwenPaw/pull/7055)), and possibly chat-history pagination ([#7049](https://github.com/agentscope-ai/QwenPaw/pull/7049)). Larger items like provider unification and the DataPaw runtime are probably further out.

## 7. User Feedback Summary
User sentiment is a mix of engagement and frustration:

- **Silent failures are a major irritation**: issues #7059 and #7048 both describe commands/tools that return success while doing nothing useful.
- **v2.1.0 regression concerns**: multiple reports point to missing UI options (#7058), lost image attachments (#7051), and broken video context (#7059, #7060).
- **Enterprise/remote workflows are underserved**: OAuth2 rotation (#7053), background task callbacks (#7056), and plugin `system_prompt` privacy (#7052) all involve production/team use cases.
- **Performance remains a recurring complaint**: long Console conversations still badly lag (#3915).

On the positive side, the high number of **first-time-contributor PRs** suggests growing community interest and willingness to contribute fixes.

## 8. Backlog Watch
Items that likely need maintainer attention:

- [Issue #3915: Virtual scrolling for Console WebUI](https://github.com/agentscope-ai/QwenPaw/issues/3915) — open since **April 28**, 110+ days old, 3 comments, 1 👍, no linked PR.
- [PR #6302: Unify provider discovery, model metadata, routing, and agent controls](https://github.com/agentscope-ai/QwenPaw/pull/6302) — large foundational PR open since **July 21**; no visible review activity in the data.
- [PR #6623: fix(acp) final text loss race](https://github.com/agentscope-ai/QwenPaw/pull/6623) — marked **Under Review** since **August 1**; still not merged.
- [Issue #7053: OAuth2 refresh-token rotation bug](https://github.com/agentscope-ai/QwenPaw/issues/7053) — new but high-impact for remote MCP; no fix PR or maintainer response yet.
- [PR #7001: Matrix group-room session/memory isolation](https://github.com/agentscope-ai/QwenPaw/pull/7001) — first-time contributor, no visible comments; needs maintainer signal.

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

## 1. Today's Overview

ZeroClaw is in a high-volume design and hardening phase as of 2026-08-16: 50 issues and 50 PRs were updated in the last 24 hours, with 46 issues and 44 PRs still open. Four issues were closed and six PRs moved to merged/closed status, but no new release was published. The most visible activity is architectural RFC work around chat compatibility, session ownership, attachments, and security posture, while a large batch of Anthropic refusal/fallback PRs appears to have closed. A notable portion of the queue is blocked on either `needs-maintainer-review` or `needs-author-action`, suggesting maintainer bandwidth and author follow-up are the main throughput constraints.

## 2. Releases

No new releases were published in the observed window, so there are no changelog entries, breaking changes, or migration notes to report.

## 3. Project Progress

Six PRs were closed/merged in the last 24 hours. The clearest completed line of work is the **Anthropic refusal and fallback stack**:

- [zeroclaw-labs/zeroclaw#9262](https://github.com/zeroclaw-labs/zeroclaw/issues/9262) — Surface native Anthropic refusals as typed errors
- [zeroclaw-labs/zeroclaw#9263](https://github.com/zeroclaw-labs/zeroclaw/issues/9263) — Route refusals through client-side fallback entries
- [zeroclaw-labs/zeroclaw#9265](https://github.com/zeroclaw-labs/zeroclaw/issues/9265) — Opt-in Anthropic server-side fallback requests
- [zeroclaw-labs/zeroclaw#9266](https://github.com/zeroclaw-labs/zeroclaw/issues/9266) — Detect Anthropic server-side fallback responses
- [zeroclaw-labs/zeroclaw#9268](https://github.com/zeroclaw-labs/zeroclaw/issues/9268) — Surface safeguard fallback notices in channels

This stack closes the gap where Anthropic `stop_reason: "refusal"` responses were treated as empty successes instead of routed through fallback handling.

Other significant PRs updated in the window, still open, include:

- [zeroclaw-labs/zeroclaw#9320](https://github.com/zeroclaw-labs/zeroclaw/issues/9320) — Wall-clock timeout for cron agent jobs to prevent stuck SQLite locks
- [zeroclaw-labs/zeroclaw#9995](https://github.com/zeroclaw-labs/zeroclaw/issues/9995) — Hardening webhook audit exports against credential leaks
- [zeroclaw-labs/zeroclaw#9739](https://github.com/zeroclaw-labs/zeroclaw/issues/9739) — Zerocode multi-session panes with agent sidebar
- [zeroclaw-labs/zeroclaw#9745](https://github.com/zeroclaw-labs/zeroclaw/issues/9745) — Per-agent attribution/scoping for the knowledge graph
- [zeroclaw-labs/zeroclaw#9746](https://github.com/zeroclaw-labs/zeroclaw/issues/9746) — Per-agent ownership scoping for session tools and `discord_search`
- [zeroclaw-labs/zeroclaw#9954](https://github.com/zeroclaw-labs/zeroclaw/issues/9954) — Fix double-encoded SOP step output before schema validation
- [zeroclaw-labs/zeroclaw#9957](https://github.com/zeroclaw-labs/zeroclaw/issues/9957) — Record why a failed SOP run failed

## 4. Community Hot Topics

The dataset does not include meaningful reaction counts, so comment volume is the primary signal. The most active items are concentrated around architecture, security, and protocol compatibility.

- [zeroclaw-labs/zeroclaw#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) — RFC: ZeroClaw Chat Completions profile, 20 comments. Underlying need: allow OpenAI-protocol clients like Open WebUI, LobeChat, and LangChain to drive ZeroClaw agents.
- [zeroclaw-labs/zeroclaw#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) — RFC: Runtime-owned conversation sessions and transport surface adapters, 17 comments. Underlying need: session lifecycle should not be tied to temporary channel/WebSocket connections.
- [zeroclaw-labs/zeroclaw#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488) — RFC: Unified attachment architecture, 15 comments. Underlying need: consistent file/attachment handling across web chat and channel adapters.
- [zeroclaw-labs/zeroclaw#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) — Maintainer decision queue tracker, 13 comments. Underlying need: RFCs and design issues need a clearer decision pipeline to avoid stalled architecture work.
- [zeroclaw-labs/zeroclaw#6954](https://github.com/zeroclaw-labs/zeroclaw/issues/6954) — RFC: Provenance, conversation binding, and reply contract for internal agent turns, 12 comments. Underlying need: cron/internal-triggered turns need identity and reply semantics.
- [zeroclaw-labs/zeroclaw#6971](https://github.com/zeroclaw-labs/zeroclaw/issues/6971) — RFC: Security posture, credential boundaries, and universal ingress policy, 12 comments. Underlying need: operators need a unified security inspection model.
- [zeroclaw-labs/zeroclaw#9103](https://github.com/zeroclaw-labs/zeroclaw/issues/9103) — RFC: Separate authoritative memory storage from enrichment connectors, 12 comments. Underlying need: `memory.backend` conflates durable storage with optional vector connectors.
- [zeroclaw-labs/zeroclaw#8780](https://github.com/zeroclaw-labs/zeroclaw/issues/8780) — RFC: Realtime speech-to-speech channel for Gemini Live, 11 comments. Underlying need: voice-first realtime interaction through a broker-style channel.

## 5. Bugs & Stability

Ranked by severity and impact:

- **High — Cron test race causing unrelated PR failures:** [zeroclaw-labs/zeroclaw#9965](https://github.com/zeroclaw-labs/zeroclaw/issues/9965) — `cron custom-shell test hits ETXTBSY under the parallel runtime gate` and fails unrelated PRs. Status is `p1` and accepted; no dedicated fix PR is visible in the top PR list.
- **High — macOS desktop app can reopen blank or without a window:** [zeroclaw-labs/zeroclaw#7527](https://github.com/zeroclaw-labs/zeroclaw/issues/7527) — severity S1, workflow blocked. The issue is closed, but the dataset does not show resolution details.
- **High — Cron agent jobs can hang forever and hold locks:** [zeroclaw-labs/zeroclaw#9320](https://github.com/zeroclaw-labs/zeroclaw/issues/9320) — fix PR adds a wall-clock timeout that releases the job lock.
- **High — Webhook audit exports may leak credentials:** [zeroclaw-labs/zeroclaw#9995](https://github.com/zeroclaw-labs/zeroclaw/issues/9995) — fix PR scrubs credentials, provider-token patterns, and inline image markers from audit arguments.
- **High — Config set failures leave auto-created aliases behind:** [zeroclaw-labs/zeroclaw#9281](https://github.com/zeroclaw-labs/zeroclaw/issues/9281) — fix PR makes `config/set` transactional.
- **High — Viewer disconnect cancels active agent turns:** [zeroclaw-labs/zeroclaw#9002](https://github.com/zeroclaw-labs/zeroclaw/issues/9002) — fix PR treats dashboard WebSocket as viewer/controller rather than turn owner.
- **High — Knowledge graph and session tools share data across agents:** [zeroclaw-labs/zeroclaw#9745](https://github.com/zeroclaw-labs/zeroclaw/issues/9745) and [zeroclaw-labs/zeroclaw#9746](https://github.com/zeroclaw-labs/zeroclaw/issues/9746) — security hardening PRs add per-agent ownership and scoping.
- **Medium/High — Interactive Ctrl+C handling is not state-aware:** [zeroclaw-labs/zeroclaw#9229](https://github.com/zeroclaw-labs/zeroclaw/issues/9229) — fix PR gives the REPL a single `Idle/Active/Stopping` lifecycle.
- **Medium — SOP output validation and failure diagnostics:** [zeroclaw-labs/zeroclaw#9954](https://github.com/zeroclaw-labs/zeroclaw/issues/9954) and [zeroclaw-labs/zeroclaw#9957](https://github.com/zeroclaw-labs/zeroclaw/issues/9957) — fixes for double-encoded step output and missing failure reasons.

## 6. Feature Requests & Roadmap Signals

Several accepted or actively discussed features are likely candidates for the next release:

- [zeroclaw-labs/zeroclaw#9598](https://github.com/zeroclaw-labs/zeroclaw/issues/9598) — RFC: Define the SOP capability permission contract. Explicitly targets v0.9.0.
- [zeroclaw-labs/zeroclaw#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) — OpenAI Chat Completions compatibility would unlock many third-party clients.
- [zeroclaw-labs/zeroclaw#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) and [zeroclaw-labs/zeroclaw#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488) — Runtime-owned sessions and unified attachments would be foundational architecture changes.
- [zeroclaw-labs/zeroclaw#8780](https://github.com/zeroclaw-labs/zeroclaw/issues/8780) — Gemini Live realtime speech-to-speech channel.
- [zeroclaw-labs/zeroclaw#9810](https://github.com/zeroclaw-labs/zeroclaw/issues/9810) — Loading Agent Plugins 1.0 skill and MCP packages.
- [zeroclaw-labs/zeroclaw#6909](https://github.com/zeroclaw-labs/zeroclaw/issues/6909) — Computer-use support for desktop screen interaction and input control.

Accepted lower-risk feature/process work that could land soon includes:

- [zeroclaw-labs/zeroclaw#7108](https://github.com/zeroclaw-labs/zeroclaw/issues/7108) — Improve cached Rust builds and CI critical path
- [zeroclaw-labs/zeroclaw#7130](https://github.com/zeroclaw-labs/zeroclaw/issues/7130) — Workspace-wide `forbid(unsafe_code)` with a narrow reviewed boundary
- [zeroclaw-labs/zeroclaw#9512](https://github.com/zeroclaw-labs/zeroclaw/issues/9512) — Annotate bespoke CI gates with motivating incidents
- [zeroclaw-labs/zeroclaw#9345](https://github.com/zeroclaw-labs/zeroclaw/issues/9345) — Recalculate PR risk/size labels on every update
- [zeroclaw-labs/zeroclaw#7849](https://github.com/zeroclaw-labs/zeroclaw/issues/7849) — Discord mention-triggered thread mode
- [zeroclaw-labs/zeroclaw#7824](https://github.com/zeroclaw-labs/zeroclaw/issues/7824) — WeCom proactive messaging and media file sending
- [zeroclaw-labs/zeroclaw#7410](https://github.com/zeroclaw-labs/zeroclaw/issues/7410) — Read gateway webhook signing secrets at handler time instead of startup caching

## 7. User Feedback Summary

Real user pain points visible in the issue queue:

- **OpenAI-compatible clients cannot use ZeroClaw.** Users want Open WebUI, LobeChat, Aider, and LangChain to work directly against ZeroClaw agents ([#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)).
- **Memory backend configuration is confusing.** One setting is expected to serve as both authoritative storage and connector plug-in point ([#9103](https://github.com/zeroclaw-labs/zeroclaw/issues/9103)).
- **Cron documentation is missing, and cron jobs cannot be pinned to a specific model/agent.** Users want cheap models for low-priority scheduled tasks ([#7762](https://github.com/zeroclaw-labs/zeroclaw/issues/7762)).
- **Windows shell host is not configurable.** Users want PowerShell/Git Bash evaluation and selection instead of being stuck with `cmd.exe` ([#7089](https://github.com/zeroclaw-labs/zeroclaw/issues/7089)).
- **Discord agents take over shared channels.** Mention-triggered threads are requested to keep conversations contained ([#7849](https://github.com/zeroclaw-labs/zeroclaw/issues/7849)).
- **WeCom users cannot send proactive messages or media.** Current support is limited to passive WebSocket responses ([#7824](https://github.com/zeroclaw-labs/zeroclaw/issues/7824)).
- **Public blockchain addresses are being redacted by the leak detector.** Payment-request URLs become undeliverable due to false positives ([#9825](https://github.com/zeroclaw-labs/zeroclaw/issues/9825)).
- **Zerocode does not yet match web dashboard operator surfaces.** Power users want TUI parity for headless/enclave setups ([#7790](https://github.com/zeroclaw-labs/zeroclaw/issues/7790)).
- **Maintainers admit they lack product-usage visibility.** RFC #9621 argues for staged opt-in telemetry to make support and removal decisions.

## 8. Backlog Watch

The following items are high-risk, high-comment, or long-lived and still need maintainer decision/review:

- [zeroclaw-labs/zeroclaw#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) — The maintainer decision queue tracker itself; it coordinates many other RFCs.
- [zeroclaw-labs/zeroclaw#6954](https://github.com/zeroclaw-labs/zeroclaw/issues/6954) — Created 2026-05-26, `needs-maintainer-review`, high risk. Internal agent turn provenance and reply contract.
- [zeroclaw-labs/zeroclaw#6971](https://github.com/zeroclaw-labs/zeroclaw/issues/6971) — Created 2026-05-27, `needs-maintainer-review`, high risk. Security posture and universal ingress policy.
- [zeroclaw-labs/zeroclaw#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) — 20 comments, `needs-maintainer-review`, high risk. Chat Completions profile decision.
- [zeroclaw-labs/zeroclaw#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) — 17 comments, `needs-maintainer-review`, high risk. Runtime-owned conversation sessions.
- [zeroclaw-labs/zeroclaw#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488) — 15 comments, `needs-maintainer-review`, high risk. Unified attachment architecture.
- [zeroclaw-labs/zeroclaw#9621](https://github.com/zeroclaw-labs/zeroclaw/issues/9621) — RFC for staged opt-in product telemetry, `needs-maintainer-review`, high risk.
- [zeroclaw-labs/zeroclaw#9598](https://github.com/zeroclaw-labs/zeroclaw/issues/9598) — SOP permission contract, `needs-maintainer-review`, high risk, explicitly targeting v0.9.0.
- [zeroclaw-labs/zeroclaw#9954](https://github.com/zeroclaw-labs/zeroclaw/issues/9954) — Open PR fixing SOP double-encoded output, currently `needs-maintainer-review`.

The number of high-risk RFCs waiting on maintainer action is the clearest signal that ZeroClaw's design queue is the current bottleneck.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*