# OpenClaw Ecosystem Digest 2026-08-14

> Issues: 500 | PRs: 500 | Projects covered: 12 | Generated: 2026-08-13 23:34 UTC

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

# OpenClaw Project Digest — 2026-08-14

## 1. Today's Overview

OpenClaw remains highly active: **500 issues and 500 PRs were updated in the last 24h**, with **160 issues closed** and **84 PRs merged/closed**. The busiest conversations continue to cluster around silent reply loss, subagent completion delivery, and multi-agent orchestration reliability. No new release was published in this window, so all visible changes are landing on `main`/dev rather than a stable channel. The project shows strong contributor throughput, but also persistent reliability debt in delivery/session-state paths and a large backlog of issues awaiting maintainer review or product decisions.

## 2. Releases

**No new releases in the last 24h.** No changelog, breaking-change, or migration notes are available for this digest.

## 3. Project Progress

- **84 PRs were merged/closed** in the last 24h. Among the top updated PRs, two are already closed:
  - [PR #123274 – fix(ui): dropped attachments that fail to read vanish with no visible outcome](https://github.com/openclaw/openclaw/pull/123274)
  - [PR #123290 – fix(gateway): transcript commits leave sessions.list cache serving stale previews](https://github.com/openclaw/openclaw/pull/123290)

- **Notable in-flight feature/fix PRs:**
  - [PR #123368 – fix(cron): prevent cold turns from blocking the gateway](https://github.com/openclaw/openclaw/pull/123368) — related to [issue #120834](https://github.com/openclaw/openclaw/issues/120834)
  - [PR #123366 – feat(google): add Gemini 3.7 Flash with LOW thinking floor](https://github.com/openclaw/openclaw/pull/123366)
  - [PR #123348 – feat(docker): weekly refresh of published moving image tags](https://github.com/openclaw/openclaw/pull/123348)
  - [PR #123105 – refactor(plugins): replace node-llama-cpp with managed llama-server](https://github.com/openclaw/openclaw/pull/123105)
  - [PR #123328 – fix(ci): prevent type-aware lint timeouts on constrained runners](https://github.com/openclaw/openclaw/pull/123328)
  - [PR #112375 – feat(cron): shell precheck gate to skip LLM when no work](https://github.com/openclaw/openclaw/pull/112375)
  - [PR #96969 – fix(runtime): drop intermediate monologue text blocks from visible delivery](https://github.com/openclaw/openclaw/pull/96969) — targets the same class of problem as [issue #25592](https://github.com/openclaw/openclaw/issues/25592)

- **Notable closed issues** (updated in the last 24h) show the maintainers did close out some long-running bug reports:
  - [Issue #42273 – backup create stalls on large installations (closed already-fixed)](https://github.com/openclaw/openclaw/issues/42273)
  - [Issue #44431 – Browser tool: 7 improvements from real-world automation field test (closed)](https://github.com/openclaw/openclaw/issues/44431)
  - [Issue #85714 – Agent's final agent_message stranded when LLM forgets delivery tool (closed)](https://github.com/openclaw/openclaw/issues/85714)
  - [Issue #91456 – Telegram DM lane can remain guarded after send timeout (closed)](https://github.com/openclaw/openclaw/issues/91456)
  - [Issue #105342 – All exec command outputs rendered as images instead of text (closed)](https://github.com/openclaw/openclaw/issues/105342)
  - [Issue #121605 – Model fallback reply produced but never delivered to channel (closed)](https://github.com/openclaw/openclaw/issues/121605)

## 4. Community Hot Topics

The most active issues by comment count show a clear user focus on **silent failures, delivery guarantees, and memory/session-state correctness**.

- [Issue #121058 – Silent reply failures still recurring after #116277 closed — no queued reply payload](https://github.com/openclaw/openclaw/issues/121058) — **92 comments**. A monitoring cron still logs new occurrences after the prior fix was closed. This is the single most active issue and indicates a deeply frustrating recurring production bug.
- [Issue #7707 – Feature Request: Memory Trust Tagging by Source](https://github.com/openclaw/openclaw/issues/7707) — **48 comments**. Users want memory entries tagged by trust level to prevent memory-poisoning attacks from untrusted web/scraped/third-party content.
- [Issue #25592 – Text between tool calls leaks to messaging channels](https://github.com/openclaw/openclaw/issues/25592) — **48 comments**, P1. Internal narration/processing text is being delivered to Slack, iMessage, etc. as visible messages.
- [Issue #44925 – Subagent completion silently lost — no retry, no notification, no auto-restart on timeout](https://github.com/openclaw/openclaw/issues/44925) — **27 comments**, P1, 2 👍.
- [Issue #121953 – Cron agent turns stall on DeepSeek because of the `[cron:<jobId> <name>]` prefix](https://github.com/openclaw/openclaw/issues/121953) — **16 comments**, P1.
- [Issue #43367 – Multi-agent orchestration is unstable: concurrent config overwrites, session-lock failures, detached child work](https://github.com/openclaw/openclaw/issues/43367) — **13 comments**, P1, 1 👍.
- [Issue #91363 – Isolated cron consistently fails with "LLM request failed" on model-call-started phase](https://github.com/openclaw/openclaw/issues/91363) — **10 comments**, 6 👍, P1.

Underlying need: users are running OpenClaw as an always-on autonomous gateway, heavily using cron, subagents, and multiple channels. The top complaints are not about missing features but about **lost output, undelivered messages, and unreliable multi-agent state** — these directly affect trust in the system.

## 5. Bugs & Stability

The highest-severity open bugs updated in the last 24h are dominated by **message loss, subagent completion loss, and session-lane stalls**.

### Critical / P1 delivery and session-state bugs

- [Issue #121058 – Silent reply failures still recurring](https://github.com/openclaw/openclaw/issues/121058) — No queued reply payload; monitoring cron still logs new occurrences. No fix PR identified.
- [Issue #25592 – Text between tool calls leaks to messaging channels](https://github.com/openclaw/openclaw/issues/25592) — P1; [PR #96969](https://github.com/openclaw/openclaw/pull/96969) is an in-flight fix.
- [Issue #44925 – Subagent completion silently lost](https://github.com/openclaw/openclaw/issues/44925) — P1; no retry, no notification, no auto-restart.
- [Issue #67777 – Subagent completion delivery lost on direct-announce timeout, drain, or orphan prune](https://github.com/openclaw/openclaw/issues/67777) — P1.
- [Issue #92433 – Subagent completion silently dropped when announce steers into a requester run](https://github.com/openclaw/openclaw/issues/92433) — P1.
- [Issue #97983 – iOS/WebChat messages append but do not trigger/deliver assistant replies](https://github.com/openclaw/openclaw/issues/97983) — P1.
- [Issue #91363 – Isolated cron consistently fails with "LLM request failed"](https://github.com/openclaw/openclaw/issues/91363) — P1, 6 👍.
- [Issue #43374 – All LLM API calls time out simultaneously despite APIs being reachable](https://github.com/openclaw/openclaw/issues/43374) — P1, multi-agent concurrency issue.
- [Issue #89278 – Codex OAuth refresh succeeds but cron/heartbeat fail with 10s auth refresh timeout](https://github.com/openclaw/openclaw/issues/89278) — P1 regression.
- [Issue #111498 – Main agent blocked by persistent workspace-state migration after Anthropic auth recovery](https://github.com/openclaw/openclaw/issues/111498) — P1 regression.
- [Issue #115421 – Schema downgrade recovery must not quarantine/wipe state DB; cron jobs lost](https://github.com/openclaw/openclaw/issues/115421) — P1, data loss.
- [Issue #123073 – dev-channel update fails: EUNSUPPORTEDPROTOCOL on `workspace:*`](https://github.com/openclaw/openclaw/issues/123073) — P1, blocks dev-channel users.

### Other notable stability issues

- [Issue #97616 – OpenClaw leaks unreaped hook/tool child processes, causing zombie accumulation](https://github.com/openclaw/openclaw/issues/97616) — P1.
- [Issue #43747 – Memory management is in chaos](https://github.com/openclaw/openclaw/issues/43747) — P2 regression, 11 comments.
- [Issue #114612 – SQLite unbounded growth: memory index/embedding cache tables have no retention policy](https://github.com/openclaw/openclaw/issues/114612) — P2, will fill disk over time.
- [Issue #44502 – Discord routing / mention-gating issue](https://github.com/openclaw/openclaw/issues/44502) — P2 regression.
- [Issue #107814 – gpt-5.3-codex-spark emits empty arguments for required tool calls](https://github.com/openclaw/openclaw/issues/107814) — P2, blocks tool execution.

No new release means these fixes have not yet reached stable-channel users.

## 6. Feature Requests & Roadmap Signals

The most prominent user-requested features in this update point to **security, memory governance, and configurability**.

- [Issue #7707 – Memory Trust Tagging by Source](https://github.com/openclaw/openclaw/issues/7707) — P2, long-open, needs security/product review. Likely candidate for a future security hardening release.
- [Issue #16555 – Add TTL/Expiry for Delivery Queue Messages](https://github.com/openclaw/openclaw/issues/16555) — P1, directly addresses stale/orphaned delivery queue entries.
- [Issue #114612 – Retention policy for memory_index_chunks / memory_embedding_cache](https://github.com/openclaw/openclaw/issues/114612) — likely to be prioritized to prevent disk exhaustion.
- [Issue #45758 – Support YAML as config file format](https://github.com/openclaw/openclaw/issues/45758) — P3, community interest.
- [Issue #9016 – Expose OpenRouter usage cost to agent runtime](https://github.com/openclaw/openclaw/issues/9016) — P2, useful for cost-aware agents.
- [Issue #45508 – Self-hosted STT/TTS provider support in webchat](https://github.com/openclaw/openclaw/issues/45508) — P2, privacy/self-hosting use case.
- [Issue #45771 – Built-in pace-aware rate limiting for autonomous agents](https://github.com/openclaw/openclaw/issues/45771) — P2, relevant to subagent/cron users.
- [Issue #45501 – `session.resetPrompt` — configurable session startup message](https://github.com/openclaw/openclaw/issues/45501) — P2, small but clearly useful.
- [Issue #42276 – Reasoning stream](https://github.com/openclaw/openclaw/issues/42276) — P3, UX enhancement.

Additional roadmap signals from PRs: [PR #123105](https://github.com/openclaw/openclaw/pull/123105) moves local inference off `node-llama-cpp` to a managed `llama-server`, and [PR #123366](https://github.com/openclaw/openclaw/pull/123366) adds Gemini 3.7 Flash support. Both suggest ongoing investment in local/alternative model support.

## 7. User Feedback Summary

Real user pain points expressed in the last 24h:

- **Silent failures are the biggest source of frustration.** Issue #121058 has 92 comments and explicitly notes that a monitoring cron is still catching failures after the previous fix was closed. Users feel they cannot trust the system to reliably deliver replies.
- **Subagent orchestration is a major weak point.** Multiple P1 issues (#44925, #67777, #92433, #47975) describe completed subagent work being silently lost or leaving the main session unresponsive.
- **Channel routing leaks and stale state are common.** Users report internal text leaking to Slack/iMessage (#25592), Telegram DMs polluting the main session (#41165), Discord routing regressions (#44502), and session-lane starvation (#54488).
- **Auth/provider regressions are recurring.** Codex OAuth timeouts (#89278), GPT-5.3-codex-spark empty tool arguments (#107814), and LiteLLM cache retention issues (#37966) show that provider edge cases are still a source of instability.
- **On the positive side**, users are submitting detailed reproductions and field evidence, and maintainers are closing some old bugs as already-fixed (#42273, #44431, #85714, #91456, #105342, #121605). The project is responsive, but the recurrence of issues like #121058 suggests the delivery subsystem needs deeper architectural fixes, not just point patches.

## 8. Backlog Watch

Several high-importance issues have been open for months and are tagged with `clawsweeper:needs-maintainer-review` or `clawsweeper:needs-product-decision`, indicating they are waiting on maintainers.

- [Issue #7707 – Memory Trust Tagging by Source](https://github.com/openclaw/openclaw/issues/7707) — open since **2026-02-03**, 48 comments, needs maintainer/product/security review.
- [Issue #25592 – Text between tool calls leaks to messaging channels](https://github.com/openclaw/openclaw/issues/25592) — open since **2026-02-24**, P1, 48 comments; [PR #96969](https://github.com/openclaw/openclaw/pull/96969) is linked but still needs review.
- [Issue #40611 – Heartbeat drift fix causes aggressive retry that blocks Telegram](https://github.com/openclaw/openclaw/issues/40611) — open since **2026-03-09**, P1.
- [Issue #41165 – Telegram DMs can still land in `agent:main:main`](https://github.com/openclaw/openclaw/issues/41165) — open since **2026-03-09**, P1.
- [Issue #43367 – Multi-agent orchestration is unstable](https://github.com/openclaw/openclaw/issues/43367) — open since **2026-03-11**, P1.
- [Issue #44925 – Subagent completion silently lost](https://github.com/openclaw/openclaw/issues/44925) — open since **2026-03-13**, P1, 27 comments.
- [Issue #47975 – Subagent sessions persist after completion, main session unresponsive](https://github.com/openclaw/openclaw/issues/47975) — open since **2026-03-16**, P1.
- [Issue #67777 – Subagent completion delivery can be lost on timeout/drain/orphan prune](https://github.com/openclaw/openclaw/issues/67777) — open since **2026-04-16**, P1.
- [Issue #72015 – active-memory blocks replies; QMD boot initialization can overload multi-agent gateways](https://github.com/openclaw/openclaw/issues/72015) — open since **2026-04-26**, P1.
- [Issue #91363 – Isolated cron consistently fails with "LLM request failed"](https://github.com/openclaw/openclaw/issues/91363) — open since **2026-06-08**, P1, 6 👍.
- [Issue #97983 – iOS/WebChat messages append but do not trigger assistant replies](https://github.com/openclaw/openclaw/issues/97983) — open since **2026-06-30**, P1, maturity:stable.

**PRs also needing maintainer attention/proof:**
- [PR #82540 – fix(wechat): preserve existing accounts across hot reload](https://github.com/openclaw/openclaw/pull/82540) — open since May, P1, needs proof.
- [PR #82023 – feat(telegram): bind spawned subagents to forum topics](https://github.com/openclaw/openclaw/pull/82023) — open since May, P2, needs proof.
- [PR #96113 – feat(sessions): add read-only diagnose command](https://github.com/openclaw/openclaw/pull/96113) — open since June, P1, needs proof.

Overall, the project is shipping steadily, but the backlog of delivery-reliability and subagent-lifecycle bugs should be treated as the top health risk for OpenClaw.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant / Agent Open-Source Ecosystem
**Date:** 2026-08-14 | **Data:** 24-hour community digest summaries for 12 projects

---

## 1. Ecosystem Overview

The personal AI assistant ecosystem is shifting from feature velocity to trust engineering. Across 12 projects tracked, the most-commented issues are no longer missing features but **lost output, undelivered messages, corrupted session state, and unreliable memory** — the fundamentals of an always-on autonomous agent. Activity is highly uneven: OpenClaw processes ~10× the daily issue/PR volume of any peer, while two projects (NullClaw, ZeptoClaw) show zero activity. Four stable releases landed in one window (Hermes v0.20.1, IronClaw v1.2.0, NanoClaw v2.2.0, CoPaw v2.1.0), signaling maturation and downstream-consumer consolidation. Security hardening is accelerating in response to real incidents: weak pairing-code entropy, credential-chain bypasses, unauthenticated plugin APIs, and OAuth token leak risks.

## 2. Activity Comparison

| Project | Issues updated (closed) | PRs updated (merged/closed) | Release this window | Health score* |
|---|---|---|---|---|
| **OpenClaw** | 500 (160) | 500 (84) | None | 7/10 — Extreme throughput, but recurring delivery-reliability debt (#121058, 92 comments) |
| **CoPaw (QwenPaw)** | 42 (17) | 50 (19) | **v2.1.0** + beta.5 | 8/10 — Fast post-release iteration; task-stall and AV-conflict reports |
| **ZeroClaw** | 50 (13) | 50 (7) | None | 8/10 — Security-hardening phase; RFC decision queue is the bottleneck |
| **IronClaw** | 50 (18) | 50 (26) | **v1.2.0** | 9/10 — Healthiest mix: stable release + active "Reborn" roadmap |
| **Hermes Agent** | 50 (5) | 50 (8) | **v0.20.1** | 7/10 — 656-PR consolidation release; two P1 TUI bugs unfixed 13+ days |
| **NanoBot** | 13 (1) | 31 (9) | None | 8/10 — Best responsiveness: same-day fix PRs for filed bugs |
| **NanoClaw** | 2 (1) | 19 (13) | **v2.2.0** (breaking) | 8/10 — Supply-chain security focus; high merge ratio |
| **LobsterAI** | 1 (0) | 11 (6) | None | 6/10 — UI consolidation; test-coverage PRs stale since March |
| **PicoClaw** | 3 (0) | 9 (3 stale) | None | 6/10 — Dependency hygiene only; no feature merges |
| **Moltis** | 1 (0) | 4 (0) | None | 5/10 — No merges; large connector PR awaiting review |
| **NullClaw** | — | — | — | N/A — No activity |
| **ZeptoClaw** | — | — | — | N/A — No activity |

*Health score is a qualitative composite of throughput, closure rate, responsiveness to bugs, and unresolved-severity burden, derived from the digest data.

## 3. OpenClaw's Position

**Advantages.** OpenClaw is the ecosystem's reference implementation. Its 500-issue/500-PR daily throughput, 160 issues closed, and 84 PRs merged in 24 hours are ~10× any peer's volume. It defines the category's core model: an always-on, multi-channel autonomous gateway (Slack, Telegram, iMessage, Discord, WhatsApp) with cron-driven autonomy and subagent orchestration. Maintainer responsiveness is high — six long-running bugs were closed as already-fixed this window.

**Technical approach differences.** OpenClaw is a monolithic Node/TypeScript gateway optimized for channel breadth and continuous autonomy. Current moves — replacing node-llama-cpp with managed llama-server (PR #123105) and adding Gemini 3.7 Flash (PR #123366) — show investment in flexible local/alternative model backends. Peers are taking structurally different routes: IronClaw reframes the agent as a Rust "kernel" with pluggable agent loops (ACP); Hermes is Python-first with TUI/Desktop surfaces and webhook/CI automation; CoPaw is a consumer desktop OS-Shell experience aimed at the Chinese market.

**Community size comparison.** Unmatched. OpenClaw's top issue alone (#121058, 92 comments) exceeds the total daily issue traffic of NanoBot, PicoClaw, LobsterAI, and Moltis combined. However, scale has a cost: a large backlog awaits maintainer review, and the #1 community complaint — silent reply loss — is recurring despite a prior fix. This is the ecosystem's most visible trust gap, and several peers are explicitly positioning on reliability.

## 4. Shared Technical Focus Areas

| Focus area | Projects | Specific needs |
|---|---|---|
| **Delivery guarantees / no silent failure** | OpenClaw (#121058, #44925), Hermes (#62142), CoPaw (#6921), ZeroClaw (#9002) | Queued reply payloads, retry+notification on subagent completion, agents that execute announced plans without manual "continue" prompts |
| **Session-state persistence & isolation** | OpenClaw (#43367), NanoBot (#5378), Hermes (#84876), CoPaw (#6966), ZeroClaw (#9487) | Atomic session saves, per-run session keys, turn serialization, no cross-session working-directory leaks |
| **Memory governance & retention** | OpenClaw (#7707, #114612), NanoBot (#5372), IronClaw (#7185), CoPaw (#6951), ZeroClaw (#6850, #6998) | TTL/expiry, trust tagging by source, retention policies for embedding caches, compaction transcript transparency |
| **Cron / scheduled-task reliability** | OpenClaw (#91363), NanoBot (#5373), Hermes (#85215), LobsterAI (#1232) | Scheduler survival on persistence failure, model-repin paths, first-run UI notifications |
| **MCP / tool-schema management** | NanoBot (#5298), ZeroClaw (#9945), IronClaw (#7581), NanoClaw (#2624) | Schema budgeting for large tool sets, full browser-tool command coverage, per-server disabledTools, auth-state refresh |
| **Provider diversity & failover** | OpenClaw (#89278), Hermes (#85631), CoPaw (#6973), ZeroClaw (#9631) | No-auth fallback pools, token-plan billing, stable `session_id` for prompt caching, fail-closed credential handling |
| **Security hardening** | NanoClaw (#3229), ZeroClaw (#9389, #9969), Hermes (#63826), CoPaw (#6992) | Crypto-grade pairing codes, asset containment, OAuth token leak prevention, credential-chain verification |
| **Multi-agent / subagent orchestration** | OpenClaw (#43367), CoPaw (#6652), Hermes (#85646–48), IronClaw (#7482) | Bounded sub-agent iterations, persisted child work, exactly-once delegation, detached child recovery |

## 5. Differentiation Analysis

| Project | Core identity | Target users | Technical architecture |
|---|---|---|---|
| **OpenClaw** | Always-on autonomous multi-channel gateway | Power users running 24/7 personal agents | Monolithic Node/TS gateway; cron + subagents + channel adapters |
| **Hermes Agent** | Python agent with TUI/Desktop + webhook automation | Developers, CI/enterprise automation | Python; TUI + Desktop + gateway adapters; webhook-first |
| **IronClaw** | Agent kernel with pluggable loops (ACP) | Cloud/NEAR AI deployments, multi-agent orchestration | Rust kernel; pluggable agent harnesses; Postgres-backed |
| **CoPaw (QwenPaw)** | Consumer desktop OS-Shell agent | Chinese-market desktop users | Desktop app with windowed launcher; Qwen/OpenAI models |
| **NanoBot** | Lightweight WebUI + MCP-centric assistant | Developers wanting self-hosted chat + MCP tools | TypeScript; WebUI; Docker; MCP Apps integration |
| **ZeroClaw** | Security-hardened, RFC-driven multi-channel agent | Privacy-conscious operators; WeChat users | Governance-heavy; verifiable-intent; goal mode |
| **NanoClaw** | Ops/CLI-first agent deployment tool | Teams running agents at scale | CLI + agent images; Agent Plugins 1.0; Sigstore-verified |
| **LobsterAI** | OpenClaw-adjacent desktop management | OpenClaw users wanting GUI/management | Electron desktop; unified skills/MCP console |
| **Moltis** | Sandbox/gateway message infrastructure | Developers building agent backends | Gateway push/fanout; sandbox builds; durable connectors |
| **PicoClaw** | Minimal Go agent | Lightweight/embedded deployments | Go; minimal surface; Dependabot-maintained |

## 6. Community Momentum & Maturity

**Tier 1 — High velocity / rapid iteration:**
- **CoPaw** — post-2.1.0 release energy, 42 issues + 50 PRs/day, influx of first-time contributors (4 of today's top PRs).
- **ZeroClaw** — 50/50 daily activity, security PRs merging steadily; v0.9.0 RFCs (goal mode, shell policy, runtime sessions) are the next architecture wave.
- **IronClaw** — stable release plus the "Reborn" pluggable-agent-loop epic; best velocity-to-quality ratio in the ecosystem.
- **OpenClaw** — extreme volume, but delivery-reliability debt and maintainer backlog create churn.

**Tier 2 — Steady / stabilizing:**
- **Hermes** — consolidating via v0.20.1 (656-PR rollup); TUI P1s temper the picture.
- **NanoBot** — same-day fix cadence, feature PRs advancing (MCP schema budgeting, Matrix SAS, Telegram stickers).
- **NanoClaw** — high merge ratio, focused supply-chain security work; small but effective community.

**Tier 3 — Maintenance / low activity:**
- **PicoClaw** (dependency bumps only), **LobsterAI** (UI consolidation, stale PRs from March), **Moltis** (no merges; review needed).

**Inactive:** NullClaw, ZeptoClaw.

**Key maturity signal:** Four stable releases in one 24-hour window confirm the ecosystem is moving from pre-1.0 churn to tagged stability for downstream consumers. But the recurrence of silent-failure bugs in OpenClaw and Hermes' 13-day unfixed P1 show reliability engineering still lags feature velocity across the board.

## 7. Trend Signals

1. **Delivery guarantees are the new differentiation axis.** The most-commented issues in OpenClaw (92 comments), CoPaw, Hermes, and ZeroClaw all describe work that was completed but never delivered — replies, subagent results, cron reports. *For developers:* treat exactly-once delivery semantics, retry queues, and delivery receipts as table stakes, not post-launch fixes.

2. **MCP is consolidating, not expanding.** After the tool-proliferation wave, projects are now building schema budgets (NanoBot #5298), per-server disabledTools (NanoClaw #2624), and full command surfaces (ZeroClaw #9945). *For developers:* plan for "MCP governance" — context-cost control, tool visibility, and auth lifecycle — as a product category.

3. **Pluggable agent loops / ACP will reshape the stack.** IronClaw's Reborn epic (#7482) is the most explicit signal: decouple the agent kernel from the agent loop so external harnesses (Claude Code, codex, pi) can be swapped. OpenClaw's llama-server migration and Hermes' no-auth provider pools point the same direction. *For developers:* design agent cores as harness-agnostic kernels with well-defined capability/credential boundaries.

4. **Memory is becoming a governed subsystem.** Users want trust tagging (OpenClaw #7707), TTL/retention policies (#114612), compaction transparency (CoPaw #6951), and external memory backends (ViBo proposals in NanoBot and CoPaw). The "infinite context" promise is dead; auditable, bounded, source-tagged memory is the ask. *For developers:* expose memory operations as inspectable, policy-controlled APIs.

5. **Security is a competitive differentiator.** Real incidents this window — weak Telegram pairing codes (NanoClaw), unauthenticated plugin install APIs (CoPaw), OAuth token leakage on redirect (Hermes), credential-chain verification bypass (ZeroClaw) — are driving hardening PRs as first-class work. Antivirus false positives (CoPaw #6847) reveal a need for code-signing and allowlist guidance. *For developers:* security review must be part of the merge pipeline, not an afterthought.

6. **Provider economics are now an agent-runtime feature.** Stable `session_id` for prompt caching (ZeroClaw #9631), token-plan billing for the Chinese market (CoPaw #6973), no-auth failover pools (Hermes #85631), and OpenRouter cost recording (Hermes #85690) all treat cost control as runtime logic. *For developers:* build cost observability and provider fallback into the agent loop itself.

7. **The China-market segment is distinct and growing.** CoPaw's ~60% Chinese-language feedback, ZeroClaw's WeChat channel, and Bailian token-plan requests show local-model availability, WeChat/Telegram integration, and AV-software coexistence are first-class requirements. *For developers:* don't treat i18n as a UI problem — it includes provider billing, channel adapters, and security-tooling compatibility.

8. **Cross-platform desktop friction is the adoption bottleneck.** Windows installer loops (Hermes #82168), path-separator mangling (Hermes #85406), macOS bash 3.2 incompatibilities (Moltis #1194), and antivirus kills (CoPaw #6847) show desktop-delivered agents require serious platform-specific packaging investment. *For developers:* budget for code-signing, notarization, and per-OS test matrices if desktop is a target surface.

---

**Bottom line for decision-makers:** The agent ecosystem is converging on a common reliability core — guaranteed delivery, isolated sessions, governed memory, and secure supply chains — while differentiating on architecture (monolithic gateway vs. pluggable kernel), geography (Western vs. China-market channels), and surface (headless gateway vs. desktop OS shell). OpenClaw remains the scale leader and reference implementation, but its well-documented delivery-reliability debt is the opening that IronClaw, ZeroClaw, and CoPaw are actively exploiting.

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

## 1. Today's Overview

On 2026-08-14, NanoBot showed strong, broad activity: 13 issues were updated (12 open, 1 closed) and 31 PRs were updated (22 open, 9 merged/closed), with no new releases. The day’s focus was split between reliability fixes — session store persistence, cron scheduler survival, memory consolidation integrity — and a wave of feature PRs for Telegram stickers, MCP schema budgeting, MCP Apps metadata, and Matrix SAS verification. WebUI fixes also advanced quickly, including a native folder picker and transcript-only history restoration. Overall project health appears positive: bug reports are being answered with same-day fix PRs, and feature requests are frequently paired with implementation PRs.

## 2. Releases

No new releases were published in this 24-hour window.

## 3. Project Progress

Nine PRs were merged/closed in the last 24 hours. The visible closed/merged PRs are:

- [PR #5381 — feat(webui): add native workspace folder picker](https://github.com/HKUDS/nanobot/pull/5381) — added native macOS/Windows/Linux folder selection for locally hosted WebUI sessions.
- [PR #5384 — fix(webui): restore transcript-only session history](https://github.com/HKUDS/nanobot/pull/5384) — fixed sidebar discovery and deletion of transcript-only sessions without reconstructed model context.
- [PR #5374 — fix(cron): keep scheduler alive when job-store persistence fails](https://github.com/HKUDS/nanobot/pull/5374) — closed; addressed silent cron scheduler death after persistence errors.
- [PR #5375 — fix(cron): keep scheduler alive when job-store persistence fails](https://github.com/HKUDS/nanobot/pull/5375) — closed; same reliability fix, superseded by open PR #5376.
- [PR #4556 — feat(dream): wire up model_override for Dream consolidation](https://github.com/HKUDS/nanobot/pull/4556) — closed; applied `DreamConfig.model_override` during periodic memory consolidation.
- [PR #4550 — fix(cron): use per-run session key to prevent context sharing across cron runs](https://github.com/HKUDS/nanobot/pull/4550) — closed; isolated cron session contexts per run.

Feature work advanced via open PRs as well, including [PR #5388 (MCP schema budgeting)](https://github.com/HKUDS/nanobot/pull/5388), [PR #5387 (Telegram sticker replies)](https://github.com/HKUDS/nanobot/pull/5387), [PR #5386 (MCP Apps metadata)](https://github.com/HKUDS/nanobot/pull/5386), and [PR #5385 (Matrix SAS verification flow)](https://github.com/HKUDS/nanobot/pull/5385).

## 4. Community Hot Topics

The most active issue by comments and reactions was:

- [Issue #4010 — Feature proposal: text-to-speech / voice output support](https://github.com/HKUDS/nanobot/issues/4010) — **3 comments, 3 👍**. The underlying need is clear: NanoBot can accept voice input but cannot reply with voice, so text-only replies break the conversational loop on voice-native channels.

Other issues with active attention:

- [Issue #5298 — Budget model-visible MCP schemas for large tool sets](https://github.com/HKUDS/nanobot/issues/5298) — developers are concerned about context cost when hundreds of MCP tools are exposed to the model. Open PR [#5388](https://github.com/HKUDS/nanobot/pull/5388) directly targets this.
- [Issue #5289 — Telegram sticker support and agent-initiated reactions](https://github.com/HKUDS/nanobot/issues/5289) — users want richer Telegram interactions; current inbound stickers appear empty. PR [#5387](https://github.com/HKUDS/nanobot/pull/5387) implements reusable sticker replies.
- [Issue #4841 — Matrix bot device shows as untrusted in Element](https://github.com/HKUDS/nanobot/issues/4841) — a usability/trust blocker for E2EE Matrix users. PR [#5385](https://github.com/HKUDS/nanobot/pull/5385) addresses the SAS request flow.
- [Issue #5251 — Add MCP Apps host support to WebUI](https://github.com/HKUDS/nanobot/issues/5251) — users want MCP Apps interactive UI results preserved and rendered in the WebUI. PR [#5386](https://github.com/HKUDS/nanobot/pull/5386) preserves MCP Apps result metadata separately from model context.

## 5. Bugs & Stability

Ranked by severity:

1. **Security — `exec.allowPatterns` shell-chain bypass**  
   [Issue #5306](https://github.com/HKUDS/nanobot/issues/5306) — closed in this window, but security-relevant. Linux/macOS shell chaining can bypass intended command allow-patterns. Operators should verify their deployment includes the fix associated with this advisory.

2. **High — Cron scheduler dies permanently after a job-store persistence failure**  
   [Issue #5373](https://github.com/HKUDS/nanobot/issues/5373) — a single disk-full/permission/locked-file error inside `CronService` kills the scheduler silently. Fixes exist: [PR #5374](https://github.com/HKUDS/nanobot/pull/5374), [PR #5375](https://github.com/HKUDS/nanobot/pull/5375), and the current open fix [PR #5376](https://github.com/HKUDS/nanobot/pull/5376).

3. **High — File-cap archive failure mutates the live session before persistence**  
   [Issue #5378](https://github.com/HKUDS/nanobot/issues/5378) — if archival fails, the in-memory session has already discarded overflow messages, leading to data loss. Fix PR: [PR #5380](https://github.com/HKUDS/nanobot/pull/5380).

4. **High — Consolidation truncates archive input but advances past the full message batch**  
   [Issue #5377](https://github.com/HKUDS/nanobot/issues/5377) — messages removed by token-budget truncation are skipped by the consolidation cursor. Fix PR: [PR #5379](https://github.com/HKUDS/nanobot/pull/5379).

5. **Medium — Windows `os.replace()` transient `PermissionError` can crash the gateway**  
   Addressed by [PR #5382](https://github.com/HKUDS/nanobot/pull/5382), which retries `os.replace()` during session saves. Observed during heartbeat cron saves.

6. **Low / UX — WebUI copy and fork actions appear while an Agent turn is still running**  
   [Issue #5368](https://github.com/HKUDS/nanobot/issues/5368) — conflicting completion signals for users; no dedicated fix PR is visible yet.

Also fixed in this window: [PR #5349](https://github.com/HKUDS/nanobot/pull/5349) corrects a deterministic test failure in `tests/webui/test_settings_api.py` caused by missing `timezone_name`.

## 6. Feature Requests & Roadmap Signals

Strong roadmap signals with active implementation PRs:

- **MCP schema budgeting** — [Issue #5298](https://github.com/HKUDS/nanobot/issues/5298) / [PR #5388](https://github.com/HKUDS/nanobot/pull/5388): opt-in byte budget for model-visible MCP tool schemas, designed to reduce context cost without removing executable tools.
- **Telegram stickers and reactions** — [Issue #5289](https://github.com/HKUDS/nanobot/issues/5289) / [PR #5387](https://github.com/HKUDS/nanobot/pull/5387): reusable sticker replies plus sticker metadata extraction.
- **MCP Apps support in WebUI** — [Issue #5251](https://github.com/HKUDS/nanobot/issues/5251) / [PR #5386](https://github.com/HKUDS/nanobot/pull/5386): preserve MCP Apps UI metadata separately from model-facing text.
- **Matrix cross-signing/SAS verification** — [Issue #4841](https://github.com/HKUDS/nanobot/issues/4841) / [PR #5385](https://github.com/HKUDS/nanobot/pull/5385): complete Element SAS request flow.

Requests without visible implementation yet:

- **Text-to-speech / voice output** — [Issue #4010](https://github.com/HKUDS/nanobot/issues/4010): likely roadmap candidate but no PR open.
- **WebUI Agent activity localization** — [Issue #5366](https://github.com/HKUDS/nanobot/issues/5366).
- **QwenCloud provider path** — [Issue #5350](https://github.com/HKUDS/nanobot/issues/5350): backward-compatible QwenCloud support alongside DashScope.
- **External persistent memory proposal (ViBo)** — [Issue #5372](https://github.com/HKUDS/nanobot/issues/5372): user-submitted integration proposal for cross-session memory.

The features most likely to appear in the next NanoBot release are those with merged or actively reviewed PRs: MCP schema budgeting, Telegram sticker replies, MCP Apps metadata preservation, Matrix SAS verification, WebUI folder picking, and session reliability fixes.

## 7. User Feedback Summary

- **Voice is a recurring gap**: users with voice-native channels want the agent to reply in voice, not just text ([#4010](https://github.com/HKUDS/nanobot/issues/4010)).
- **Context cost is a real pain point**: large MCP tool sets make model-visible schemas expensive; users want deterministic, stable subsets ([#5298](https://github.com/HKUDS/nanobot/issues/5298)).
- **Reliability concerns around sessions and cron**: silent cron scheduler death ([#5373](https://github.com/HKUDS/nanobot/issues/5373)), Windows permission crashes during session saves ([#5382](https://github.com/HKUDS/nanobot/pull/5382)), and session state mutation before failed archival ([#5378](https://github.com/HKUDS/nanobot/issues/5378)) all point to persistence robustness as a top user pain area.
- **Matrix trust UX is causing dissatisfaction**: the bot device showing as untrusted in Element is confusing and has no clear verification path ([#4841](https://github.com/HKUDS/nanobot/issues/4841)).
- **WebUI state signaling is inconsistent**: copy/fork buttons appearing while a turn is still running creates conflicting completion signals ([#5368](https://github.com/HKUDS/nanobot/issues/5368)).
- **Memory persistence is desired**: the ViBo proposal ([#5372](https://github.com/HKUDS/nanobot/issues/5372)) highlights user demand for cross-session memory and reduced token costs from re-sending context.

Overall, users appear satisfied with the maintainers’ responsiveness — many bug reports received same-day fix PRs — but session/cron reliability and richer channel integrations remain the most vocal concerns.

## 8. Backlog Watch

- [Issue #4010 — Text-to-speech / voice output support](https://github.com/HKUDS/nanobot/issues/4010) — open since 2026-05-26, 3 comments, 3 👍, still no implementation PR. This is the oldest and most-supported open feature request and needs a maintainer roadmap decision.
- [Issue #4841 — Matrix device untrusted / SAS verification](https://github.com/HKUDS/nanobot/issues/4841) — open since 2026-07-07; now has PR [#5385](https://github.com/HKUDS/nanobot/pull/5385) under review.
- [PR #4549 — feat(heartbeat): add model_override config for cheaper heartbeat model](https://github.com/HKUDS/nanobot/pull/4549) — open since 2026-06-26 and still awaiting merge.
- [PR #4551 — feat(heartbeat): add isolated_session config to allow shared session](https://github.com/HKUDS/nanobot/pull/4551) — open since 2026-06-26; needs maintainer attention.
- [PR #5358 — feat(webui): add session collaboration via mentions](https://github.com/HKUDS/nanobot/pull/5358) — open since 2026-08-12 and labeled with `conflict`; needs rebase/review.
- [PR #5357 — fix(webui): cancel active turn before deleting sessions](https://github.com/HKUDS/nanobot/pull/5357) — open since 2026-08-12 and labeled with `conflict`; needs rebase/review.

The main backlog risk is a cluster of older heartbeat and cron PRs remaining open for weeks, plus two WebUI PRs marked as conflicting. Resolving those would reduce merge debt and help keep the project moving cleanly.

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-08-14

## 1. Today's Overview

Hermes Agent is in a high-activity stabilization-and-expansion phase: ~50 issues and ~50 PRs were updated in the last 24 hours, with the maintainer pipeline closing or merging 8 PRs and 5 issues. The project cut a new patch release, **v0.20.1 (v2026.8.13)**, a stable rollup of ~656 PRs merged since v0.20.0 — a signal that the project is consolidating for downstream consumers (Docker images, hosted deployments, tagged installs). Active work clusters around three themes: session-state reliability (TUI overlays, turn serialization, working-directory isolation), gateway/message-delivery correctness (Slack bot auth, Telegram attribution, Discord typing races), and scriptability/automation (non-interactive MCP config, conversational Kanban intake, no-auth provider pools). Two P1 session-state bugs remain open, suggesting the TUI/desktop surface is the current stability hotspot.

## 2. Releases

**v0.20.1 (v2026.8.13)** — *([release](https://github.com/NousResearch/hermes-agent/releases))*

- **Type:** Patch release / stable tag rollup.
- **Scope:** Rolls up the ~656 PRs merged since v0.20.0 into a fixed tagged release for downstream consumers (Docker images, hosted deployments, installs from the latest tag).
- **Breaking changes / migration notes:** None documented in the release notes; this is advertised as a stabilization tag, not a feature release.
- **Implication:** Users on rolling `latest` tags should see a materially hardened build vs. v0.20.0, including the accumulated bug fixes, security hardening, and platform fixes merged over the intervening period.

## 3. Project Progress

**Merged/closed PRs today (8 total):**

- **[#85698](https://github.com/NousResearch/hermes-agent/pull/85698)** — *Fix suggestion pills coming back dead after one click*: resolves three independent defects in the MCP connect-pill UI (stale "Added GitHub" check state, ignored clicks, vanishing on draft change).
- **[#85591](https://github.com/NousResearch/hermes-agent/pull/85591)** — *Isolate agent browser research from visible Chrome*: browser automation now defaults to isolated/headless policy across Browser Use, built-in browser, Kanban workers, computer-use controls, and terminal GUI-launch paths — a meaningful privacy/safety improvement.
- **[#85690](https://github.com/NousResearch/hermes-agent/pull/85690)** — *Record OpenRouter reported costs*: persists provider-reported `usage.cost` as actual spend while keeping the models-catalog estimate independently for fallback/comparison; exposed in one-shot usage reports.
- **[#85343](https://github.com/NousResearch/hermes-agent/pull/85343)** — *Restore Mistral Voxtral STT in tools_config catalog*: re-enables the mistral provider after the PyPI quarantine was lifted (clean 2.4.7+ releases).

**Notable open PRs advancing features (not yet merged):**

- **[#85688](https://github.com/NousResearch/hermes-agent/pull/85688) / [#85686](https://github.com/NousResearch/hermes-agent/pull/85686)** — Non-interactive MCP tool selection (`hermes mcp configure --tools` / `--all`), targeting CI/provisioning-script use cases.
- **[#85699](https://github.com/NousResearch/hermes-agent/pull/85699)** — Conversational Kanban intake for Slack (opt-in private DM thread, restart-safe stage claims, explicit promotion to Triage cards).
- **[#85694](https://github.com/NousResearch/hermes-agent/pull/85694)** — Venice.ai model-provider plugin with live model catalog in CLI/TUI/dashboard/WebUI pickers.
- **[#84965](https://github.com/NousResearch/hermes-agent/pull/84965)** — Surface memory pressure and suspected-OOM restarts in `hermes status` (heartbeat mem samples, unclean-exit verdicts, cache-pressure evictions) — addresses silent hourly OOM kills in hosted agents.
- **[#84876](https://github.com/NousResearch/hermes-agent/pull/84876)** — Serialize concurrent agent turns per `session_id` in `APIServerAdapter`, preventing overlapping conversation loops against one SessionDB transcript.
- **[#85697](https://github.com/NousResearch/hermes-agent/pull/85697)** — New Desktop chats start in the active workspace instead of the launch directory.
- **[#85631](https://github.com/NousResearch/hermes-agent/pull/85631)** — "Freemaxxing": optional no-auth loopback providers for multi-provider failover pools.

## 4. Community Hot Topics

- **[#84834 — Webhook Revolution meta-issue](https://github.com/NousResearch/hermes-agent/issues/84834)** (16 comments) — Graph-gated 5×2×3 repair campaign covering the entire webhook surface (ingress, execution, delivery, config, management UI, deployment, docs). The high engagement reflects broad developer demand for a systematic fix of webhook reliability rather than piecemeal patches.
- **[#69592 — `/sessions` and `/models` overlays invisible with ambient widget dock](https://github.com/NousResearch/hermes-agent/issues/69592)** (12 comments, P1) — Core TUI workflows (session resume, model switching, `/reload`) broken for "Day 13+" when using the documented dock pattern. This is the single loudest user-facing stability complaint, with strong "this must be fixed" sentiment.
- **[#39043 — Signal adapter native quote/reply/edit/remote-delete/read-receipt support](https://github.com/NousResearch/hermes-agent/issues/39043)** (7 comments, 👍3) — A long-standing feature request (open since June 4) for parity with signal-cli capabilities. The 👍 count indicates real demand for richer Signal integration.
- **[#75791 — Windows 11 25H2 `hermes dashboard --status` false negative](https://github.com/NousResearch/hermes-agent/issues/75791)** (6 comments) — Misleading status reporting on Windows; dashboard is actually running and serving on `127.0.0.1:9119`.
- **[#70131 — Emoji sign-off fix misses Dingbats (✨ U+2728, ✅ U+2705)](https://github.com/NousResearch/hermes-agent/issues/70131)** (6 comments, 👍1) — The `_has_natural_response_ending()` emoji gate has a wrong lower bound (`0x1F300`), so common Dingbat emoji still trigger the truncation loop — a demonstration of the long-tail risk in heuristic fixes.

## 5. Bugs & Stability

**P1 (critical):**

- **[#69592](https://github.com/NousResearch/hermes-agent/issues/69592)** — TUI `/sessions` and `/models` overlays invisible with ambient widget dock; `/reload` silent. Unmitigated for 13+ days; blocks core workflows for users following documented TUI patterns.
- **[#62142](https://github.com/NousResearch/hermes-agent/issues/62142)** — Verification-stop can discard streamed final answers and cron reports from the durable transcript; users see shorter follow-ups instead of the substantive answer. Also affects cron delivery integrity.
- **[#82168](https://github.com/NousResearch/hermes-agent/issues/82168)** — Desktop installer enters a loop of "both updating and reinstalling" on Windows; setup never completes cleanly.

**P2 (high):**

- **[#85215](https://github.com/NousResearch/hermes-agent/issues/85215)** — Cron jobs pin to a dead model snapshot and ignore `fallback_providers`, failing with HTTP 402 for days. Related to the broader cron-drift cluster ([#70050](https://github.com/NousResearch/hermes-agent/issues/70050)) where users have no supported path to repin a model.
- **[#83427](https://github.com/NousResearch/hermes-agent/issues/83427)** — `browser_exec` crashes with `pydantic_core ModuleNotFoundError` when `PYTHONPATH` points at the Hermes venv (desktop app).
- **[#85614](https://github.com/NousResearch/hermes-agent/issues/85614)** — Slack peer bot IDs pass early delivery checks but are ignored by final bot authorization — a bot-to-bot authorization inconsistency.
- **[#85406](https://github.com/NousResearch/hermes-agent/issues/85406)** — `vision_analyze` fails on Windows host + Docker terminal because host-side `Path()` mangles POSIX separators into backslashes before the container exec-read.
- **[#85658](https://github.com/NousResearch/hermes-agent/issues/85658)** — Interrupted command in one chat adopts another session's working directory; subsequent commands run in the wrong directory.
- **[#65085](https://github.com/NousResearch/hermes-agent/issues/65085)** — Telegram `observe_unmentioned_group_messages` breaks slash-command admin gating by anonymizing `event.source`.

**P3 / regressions:**

- **[#85331](https://github.com/NousResearch/hermes-agent/issues/85331)** — Desktop sidebar renders ghost title-less rows after compression-chain reorganization (backend correct; frontend rendering issue).
- **[#85104](https://github.com/NousResearch/hermes-agent/issues/85104)** — Duplicate rendering of the same assistant message in chat view; DB stores a single record, pointing to a frontend rendering-layer bug.
- **[#85659](https://github.com/NousResearch/hermes-agent/issues/85659)** — Locale-handling bug in the Desktop update PowerShell script (French Windows).
- **[#85672](https://github.com/NousResearch/hermes-agent/issues/85672)** — Kanban attachment downloads resolve to wrong path on macOS Desktop over SSH.

**Fix PRs available:** [#84876](https://github.com/NousResearch/hermes-agent/pull/84876) (turn serialization), [#85697](https://github.com/NousResearch/hermes-agent/pull/85697) (workspace cwd), [#85689](https://github.com/NousResearch/hermes-agent/pull/85689) (Kanban respawn guard, fixes #85663), [#85692](https://github.com/NousResearch/hermes-agent/pull/85692) (desktop image.generate timeout), [#61375](https://github.com/NousResearch/hermes-agent/pull/61375) (turn-dead marker on retry exhaustion). No fix PR yet for the top P1s (#69592, #62142, #82168).

## 6. Feature Requests & Roadmap Signals

**Strongest roadmap signals:**

- **Delegation durability & routing ([#85646](https://github.com/NousResearch/hermes-agent/issues/85646), [#85647](https://github.com/NousResearch/hermes-agent/issues/85647), [#85648](https://github.com/NousResearch/hermes-agent/issues/85648))** — Three linked proposals to persist completed batch children independently, deliver ready results without waiting for siblings, and let ready dependencies influence unfinished parent work. These point to a deliberate move toward more robust, parallel, exactly-once delegation semantics — likely candidates for the next minor release.
- **Webhook Revolution ([#84834](https://github.com/NousResearch/hermes-agent/issues/84834))** — The 5×2×3 graph-gated repair campaign is the umbrella for a large slate of webhook fixes; expect incremental merges against this epic.
- **Scriptability / automation:** Non-interactive MCP configuration ([#85688](https://github.com/NousResearch/hermes-agent/pull/85688), [#85686](https://github.com/NousResearch/hermes-agent/pull/85686)) and conversational Kanban intake ([#85699](https://github.com/NousResearch/hermes-agent/pull/85699)) indicate growing enterprise/CI adoption.
- **Provider ecosystem expansion:** Venice.ai plugin ([#85694](https://github.com/NousResearch/hermes-agent/pull/85694)) and no-auth failover pools ([#85631](https://github.com/NousResearch/hermes-agent/pull/85631)) suggest momentum toward flexible, multi-provider deployments with cost control.

**User-requested features with pending decisions:**

- **[#39043](https://github.com/NousResearch/hermes-agent/issues/39043)** — Full Signal quote/reply/edit/delete/read-receipt support (👍3, open since June).
- **[#85418](https://github.com/NousResearch/hermes-agent/issues/85418)** — Local-first, zero-dependency agent memory provider benchmarked against Honcho; a strong community-built proposal.
- **[#84317](https://github.com/NousResearch/hermes-agent/issues/84317)** — Opt-out for `drop_pending_updates` on Telegram cold boot.
- **[#78343](https://github.com/NousResearch/hermes-agent/pull/78343)** — Close-to-tray option with tray icon for Desktop.
- **[#33049](https://github.com/NousResearch/hermes-agent/issues/33049)** — Make credential-pool exhaustion TTLs configurable.

**Prediction:** The delegation durability/routing trio, MCP non-interactive configuration, and the Venice.ai provider are the most likely near-term merges; the Webhook Revolution will progressively land as sub-PRs tied to the epic.

## 7. User Feedback Summary

- **Session-state pain is the dominant theme.** The TUI overlay bug ([#69592](https://github.com/NousResearch/hermes-agent/issues/69592)) is explicitly reported as having gone unfixed for 13+ days, with users noting that core workflows (resume, model switch) are dead for anyone using the documented ambient-widget dock pattern. Frustration is palpable in the thread.
- **Cron model pinning is trapping users.** [#85215](https://github.com/NousResearch/hermes-agent/issues/85215) and the older cluster [#70050](https://github.com/NousResearch/hermes-agent/issues/70050) describe jobs failing for days with HTTP 402 because a pinned model snapshot is dead and no supported repin path exists — a reliability concern for automation-dependent users.
- **Windows remains the most friction-prone platform:** false-negative dashboard status ([#75791](https://github.com/NousResearch/hermes-agent/issues/75791)), path-separator mangling in Docker-terminal vision analysis ([#85406](https://github.com/NousResearch/hermes-agent/issues/85406)), installer update/reinstall loops ([#82168](https://github.com/NousResearch/hermes-agent/issues/82168)), and a locale bug in the updater ([#85659](https://github.com/NousResearch/hermes-agent/issues/85659)).
- **Positive signals:** Users are building on Hermes (the memory-provider proposal [#85418](https://github.com/NousResearch/hermes-agent/issues/85418) cites a fixed issue and expresses gratitude), and feature requests trend toward power-user automation rather than basic functionality — healthy adoption indicators.
- **UX feedback:** Suggestion-pill dead states ([#85698](https://github.com/NousResearch/hermes-agent/pull/85698)), lost composer caret on tool-call start ([#84058](https://github.com/NousResearch/hermes-agent/issues/84058)), and duplicate message rendering ([#85104](https://github.com/NousResearch/hermes-agent/issues/85104)) show users are exercising the desktop app intensely and noticing polish gaps.

## 8. Backlog Watch

- **[#39043 — Signal adapter parity](https://github.com/NousResearch/hermes-agent/issues/39043)** — Open since June 4 with 3 👍 and a `needs-decision` label; no linked PR. Longest-running popular feature request in the current batch; needs a maintainer decision or assignment.
- **[#6722 — Hardcode OSV endpoint](https://github.com/NousResearch/hermes-agent/pull/6722)** — Security PR open since April 9 (3+ months) addressing a malware-scan bypass via env var. Security fixes should not sit this long; warrants maintainer attention.
- **[#63826 — Strip Bearer credential on cross-host redirects](https://github.com/NousResearch/hermes-agent/pull/63826)** — Security fix open since July 13; stdlib redirect handler can leak the gateway's Bearer token to operator-configured connector URLs. High-value, needs review/merge.
- **[#61375 — Turn-dead marker on retry exhaustion](https://github.com/NousResearch/hermes-agent/pull/61375)** — Open since July 9; addresses machine-readable failure signaling (refs #61325). Low-complexity, useful for automation; appears stalled.
- **[#70050 — Cron drift protection / no repin path](https://github.com/NousResearch/hermes-agent/issues/70050)** — Tracks multiple sub-issues (#68380, #24258, #27530, #19615) with no supported model-repin path; a growing reliability debt in cron automation.
- **[#26854 / #26728 — Discord typing indicator races](https://github.com/NousResearch/hermes-agent/issues/26854)** — Both closed as `implemented-on-main`, a positive sign that long-lived gateway bugs are being cleared.

---

*Digest compiled from GitHub activity on 2026-08-13/14. Data: 50 issues updated (45 open / 5 closed), 50 PRs updated (42 open / 8 merged-closed), 1 release.*

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw Project Digest — 2026-08-14

## 1. Today's Overview

PicoClaw is in a steady maintenance-and-dependency-refresh phase: no new releases were published in the last 24 hours, and activity was dominated by Dependabot PRs and open feature requests. Three issues were updated, including one existing Web UI performance bug and two newly filed feature requests. Nine PRs were touched: six remain open, mostly dependency bumps, and three stale Dependabot PRs were closed as superseded. Overall project health looks stable, with maintainer attention currently focused on dependency hygiene rather than new feature delivery.

## 2. Releases

No new releases were published in the last 24 hours.

## 3. Project Progress

No feature or bug-fix merges occurred today. The three merged/closed PRs in the update window were Dependabot PRs closed as stale:

- [#3305 [CLOSED] build(deps): bump aws-sdk-go-v2/service/bedrockruntime](https://github.com/sipeed/picoclaw/pull/3305) — closed as stale, likely superseded by [#3336](https://github.com/sipeed/picoclaw/pull/3336)
- [#3306 [CLOSED] build(deps): bump aws-sdk-go-v2/config](https://github.com/sipeed/picoclaw/pull/3306) — closed as stale, likely superseded by [#3335](https://github.com/sipeed/picoclaw/pull/3335)
- [#3304 [CLOSED] build(deps): bump anthropic-sdk-go](https://github.com/sipeed/picoclaw/pull/3304) — closed as stale, likely superseded by [#3334](https://github.com/sipeed/picoclaw/pull/3334)

A notable non-merged fix is still open: [#3318 fix(web): repair unparseable pnpm-lock.yaml](https://github.com/sipeed/picoclaw/pull/3318), which addresses a broken lockfile in the web frontend.

## 4. Community Hot Topics

The most active discussion is the long-running Web UI performance bug:

- [#3281 [BUG] Web UI chat input is very laggy when history has a little bit long](https://github.com/sipeed/picoclaw/issues/3281) — 5 comments, 1 👍, open for over three weeks. Users are reporting noticeable input lag as chat history grows, indicating a need for UI virtualization, message pagination, or input-box decoupling from history rendering.

The two newly filed issues have no comments yet, but represent clear community interest:

- [#3330 [Feature] Support dynamic model override in delegate/spawn/subagent tools](https://github.com/sipeed/picoclaw/issues/3330)
- [#3331 [Feature] Allow any model with `/audio/transcriptions` endpoint, not only `*-whisper-*`](https://github.com/sipeed/picoclaw/issues/3331)

## 5. Bugs & Stability

Ranked by user impact:

1. **Web UI input lag with long chat history** — [#3281](https://github.com/sipeed/picoclaw/issues/3281)  
   Open, confirmed by users, no fix PR yet. Severity: medium-to-high, as it directly affects daily chat usability.

2. **Broken `web/frontend/pnpm-lock.yaml`** — addressed in PR [#3318](https://github.com/sipeed/picoclaw/pull/3318)  
   The lockfile contains a duplicate mapping key, causing `pnpm` to reject it. A fix PR exists but has not yet received review/merge. Severity: medium for contributors/self-hosting users using the web frontend.

No new regression reports or crashes were filed in the last 24 hours.

## 6. Feature Requests & Roadmap Signals

Two feature requests were filed on 2026-08-13:

- [#3331 Flexible audio transcription endpoint support](https://github.com/sipeed/picoclaw/issues/3331)  
  Users want to configure any model exposing an `/audio/transcriptions` endpoint, not just the older/slower `*-whisper-*` family. Proposed solution: a config flag such as `whisper-transcription: true` to force the Whisper path in `asr.go`.

- [#3330 Dynamic model override for delegate/spawn/subagent tools](https://github.com/sipeed/picoclaw/issues/3330)  
  Currently `delegate`, `spawn`, and `subagent` tools statically use configured models. Users want to specify a model at call time for more flexible agent composition.

Both are small, well-scoped quality-of-life features that could plausibly land in the next minor release, especially if maintainers are already touching agent configuration and ASR code paths.

## 7. User Feedback Summary

User feedback in the last 24 hours centers on flexibility and performance:

- Web UI performance degrades with long histories — users expect smooth input even with large sessions.
- Voice transcription support feels outdated — users want newer or faster transcription models.
- Subagent/tool model selection is too rigid — users want per-call model control for delegation.

There were no expressions of satisfaction or positive feedback in the sampled data; the overall tone is practical and feature-focused.

## 8. Backlog Watch

Items that may need maintainer attention:

- [#3281 Web UI lag bug](https://github.com/sipeed/picoclaw/issues/3281) — Open since 2026-07-21, 5 comments, still no fix linked. This is the clearest unresolved user-facing issue.
- [#3318 pnpm-lock.yaml fix PR](https://github.com/sipeed/picoclaw/pull/3318) — Open since 2026-08-05 with a concrete fix, but no visible review activity. Likely blocks web frontend installs until merged.
- Dependabot PRs from 2026-08-13 ([#3332](https://github.com/sipeed/picoclaw/pull/3332), [#3333](https://github.com/sipeed/picoclaw/pull/3333), [#3334](https://github.com/sipeed/picoclaw/pull/3334), [#3335](https://github.com/sipeed/picoclaw/pull/3335), [#3336](https://github.com/sipeed/picoclaw/pull/3336)) are awaiting review; they touch core Go dependencies including AWS SDK, Anthropic SDK, and Matrix client libraries.

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Project Digest — 2026-08-14

## 1. Today's Overview

NanoClaw had a high-activity 24 hours: 2 issues were updated (1 open, 1 closed), 19 PRs were updated (6 open, 13 closed/merged), and **v2.2.0** was released. The work is concentrated on supply-chain hardening for the agent image, CI gating improvements, and the ongoing migration of agent templates into the new “Agent Plugins 1.0.0” model. A Telegram pairing-code security fix and a database destination backfill also landed. Overall project health looks strong, with most activity coming from core-team maintainers; several older community PRs still need attention.

## 2. Releases

### v2.2.0 — [release notes](https://github.com/nanocoai/nanoclaw/releases)

The release highlights an important behavioral change:

- Template-stamped plugins now update **in place** through `ncl groups create --template <ref>`.
- If a group already carries the template's plugin, the command no longer mints a duplicate agent.
- A dry run prints a plan of every plugin-owned surface, including plugin files, skills, and MCP server configs.

The accompanying migration PR, [#3220](https://github.com/nanocoai/nanoclaw/pull/3220), is marked `feat!`, indicating a breaking format change: agent templates become **Agent Plugins 1.0.0 directories**. Existing template-stamped groups should use the new `--template` command with the dry-run flag to review needed changes before applying them.

## 3. Project Progress

The 13 closed/merged PRs in this window fall into a few main themes:

### Supply-chain / agent-image CI
- [#3236](https://github.com/nanocoai/nanoclaw/pull/3236) — repinned the agent image to `hardened-2026-08-13`; this bump carries NanoClaw’s own content, not just a base refresh.
- [#3238](https://github.com/nanocoai/nanoclaw/pull/3238) — makes `verify-agent-image` run on every PR so it can become a required status check.
- [#3240](https://github.com/nanocoai/nanoclaw/pull/3240) — opens the agent-image bump PR from a `repository_dispatch`; improves credential separation.
- [#3241](https://github.com/nanocoai/nanoclaw/pull/3241) — lets a verified Sigstore signature act as the approving review on pin bumps; off by default.
- [#3158](https://github.com/nanocoai/nanoclaw/pull/3158) — fixes missing signer identity/issuer variables that were silently skipping signature verification.
- [#3237](https://github.com/nanocoai/nanoclaw/pull/3237) — release chore for v2.2.0.

### Templates / Agent Plugins
- [#3220](https://github.com/nanocoai/nanoclaw/pull/3220) — agent templates become Agent Plugins 1.0.0 directories, including stamp-time symlink/caps/secret hardening.
- [#2909](https://github.com/nanocoai/nanoclaw/pull/2909) — adds the setup wizard’s template flow and first-agent stamping.
- [#3231](https://github.com/nanocoai/nanoclaw/pull/3231) — honors plugin MCP `cwd` in both Codex and OpenCode provider config writers.

### Security & correctness fixes
- [#3229](https://github.com/nanocoai/nanoclaw/pull/3229) — replaces `Math.random()` with `crypto.randomInt` for Telegram pairing codes and widens the code space.
- [#3145](https://github.com/nanocoai/nanoclaw/pull/3145) — migration 021 backfills missing channel destinations for existing messaging-group wirings.
- [#2624](https://github.com/nanocoai/nanoclaw/pull/2624) — adds per-server `disabledTools` support in `McpServerConfig`.

Also closed as a smoke test: [#3239](https://github.com/nanocoai/nanoclaw/pull/3239), a “DO NOT MERGE” run of the new verification gate.

## 4. Community Hot Topics

The snapshot does not include PR comment/reaction counts, and issue reactions are zero; the only measured comment is 1 on [#3234](https://github.com/nanocoai/nanoclaw/issues/3234). So “hot” here is inferred from update concentration and author clusters.

- **Agent-image verification is the clear current focus.** Eight PRs, mostly by `gavrielc`, touch the supply-chain pipeline: [#3158](https://github.com/nanocoai/nanoclaw/pull/3158), [#3236](https://github.com/nanocoai/nanoclaw/pull/3236), [#3238](https://github.com/nanocoai/nanoclaw/pull/3238), [#3239](https://github.com/nanocoai/nanoclaw/pull/3239), [#3240](https://github.com/nanocoai/nanoclaw/pull/3240), [#3241](https://github.com/nanocoai/nanoclaw/pull/3241), [#3242](https://github.com/nanocoai/nanoclaw/pull/3242), and [#3243](https://github.com/nanocoai/nanoclaw/pull/3243). The underlying need is to make image verification a real, non-forgeable CI gate rather than a human click.

- **Template/Agent Plugins migration is another hotspot.** PRs [#3220](https://github.com/nanocoai/nanoclaw/pull/3220) and [#2909](https://github.com/nanocoai/nanoclaw/pull/2909) are large, stacked changes, and issue [#3234](https://github.com/nanocoai/nanoclaw/issues/3234) reports a real user-facing ID bug in template-stamped groups.

- **Long-running community PRs remain visible**: [#2420](https://github.com/nanocoai/nanoclaw/pull/2420) (`/add-hindsight` memory skill) and [#2346](https://github.com/nanocoai/nanoclaw/pull/2346) (unknown slash commands should be treated as normal chat) are still open and updated this cycle.

## 5. Bugs & Stability

Ranked by severity:

1. **High — Telegram pairing codes use `Math.random()`**  
   [#3229](https://github.com/nanocoai/nanoclaw/pull/3229) was a security bug in `src/channels/telegram-pairing.ts`. It has been fixed with `crypto.randomInt` and a wider code space.

2. **High — Unknown webhook/bot senders produce unbounded approval cards**  
   [#3235](https://github.com/nanocoai/nanoclaw/issues/3235) is open. When `unknown_sender_policy = 'request_approval'`, automated senders keep generating approval cards, denials don’t persist, and the cards can’t be sensibly approved. No fix PR is visible yet.

3. **Medium — Template-stamped groups get a bare UUID instead of `ag-` prefix**  
   [#3234](https://github.com/nanocoai/nanoclaw/issues/3234) is closed, with 1 comment. This caused OneCLI `ensureAgent` rejection; the fix is likely tied to the template/Agent Plugins work in [#3220](https://github.com/nanocoai/nanoclaw/pull/3220)/[#2909](https://github.com/nanocoai/nanoclaw/pull/2909).

4. **Medium — `verify-agent-image` was skippable / skipped**  
   [#3238](https://github.com/nanocoai/nanoclaw/pull/3238) fixed path-filtered workflow behavior so it runs on every PR.  
   [#3158](https://github.com/nanocoai/nanoclaw/pull/3158) fixed missing signer identity variables that caused verification to be skipped.

5. **Medium — “Enable auto-merge” failure can incorrectly fail image verification**  
   [#3243](https://github.com/nanocoai/nanoclaw/pull/3243) is an open core-team PR to stop treating the auto-merge step as a verdict.

6. **Low — Docs point at a retired data/env mirror**  
   [#3230](https://github.com/nanocoai/nanoclaw/pull/3230) is an open docs fix.

## 6. Feature Requests & Roadmap Signals

- [#3218](https://github.com/nanocoai/nanoclaw/pull/3218) — `feat(cli): accept bounded JSON from stdin`; adds a generic `--stdin-json` mode to host and container `ncl` clients.
- [#2346](https://github.com/nanocoai/nanoclaw/pull/2346) — unknown slash commands should fall through to normal chat instead of being silently dropped.
- [#2420](https://github.com/nanocoai/nanoclaw/pull/2420) — opt-in `/add-hindsight` skill bundling an MCP wrapper for Hindsight memory.
- [#2624](https://github.com/nanocoai/nanoclaw/pull/2624) — per-server `disabledTools` in `McpServerConfig`; this feature request appears to have landed.
- [#3235](https://github.com/nanocoai/nanoclaw/issues/3235) — the unknown-sender approval issue signals a need for a bot/webhook-specific policy, separate from human unknown senders.

Likely next-version candidates: a follow-up fix for unknown-sender/bot approval handling, plus merge of [#3218](https://github.com/nanocoai/nanoclaw/pull/3218) or [#2346](https://github.com/nanocoai/nanoclaw/pull/2346). The `/add-hindsight` skill may need rebasing against the new Agent Plugins model.

## 7. User Feedback Summary

- Template users hit a concrete bug: template-stamped groups were created without the `ag-` prefix and rejected by OneCLI — [#3234](https://github.com/nanocoai/nanoclaw/issues/3234). The issue is closed, which suggests a fix or workaround is in place.
- Operators using webhooks or bots in messaging groups are experiencing real friction: approval cards pile up and denials don’t stick — [#3235](https://github.com/nanocoai/nanoclaw/issues/3235).
- Telegram users benefit from the pairing-code entropy fix — [#3229](https://github.com/nanocoai/nanoclaw/pull/3229).
- Contributors are pushing for better template/plugin DX: in-place plugin updates, setup-wizard stamping, and plugin MCP `cwd` support.
- Core-team feedback indicates frustration with CI ceremony that doesn’t actually verify anything: the signature-approval and auto-merge changes directly address that.

## 8. Backlog Watch

These need maintainer attention:

- [#2420](https://github.com/nanocoai/nanoclaw/pull/2420) — `feat(skills): /add-hindsight`  
  Open since **2026-05-11**. A substantial opt-in skill with a bundled MCP wrapper; needs a decision or rebase against the v2.2.0 plugin format.

- [#2346](https://github.com/nanocoai/nanoclaw/pull/2346) — `fix(formatter): treat unknown slash commands as normal chat`  
  Open since **2026-05-08**. This is a user-facing behavior fix that could affect chat handling and Agent SDK compatibility.

- [#3218](https://github.com/nanocoai/nanoclaw/pull/3218) — `feat(cli): accept bounded JSON from stdin`  
  Open since **2026-08-09**, still updated on 2026-08-13. No visible comments in the snapshot.

- [#3235](https://github.com/nanocoai/nanoclaw/issues/3235) — unknown-sender approval unbounded cards  
  Newly opened, but it describes an unresolved bug with no fix PR yet; needs triage.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-08-14

## 1. Today's Overview

IronClaw is in a high-activity period: 50 issues and 50 PRs were updated in the last 24 hours, with a healthy mix of open work (32 open/active issues, 24 open PRs) and completed work (18 closed issues, 26 merged/closed PRs). The project shipped **ironclaw-v1.2.0** as a stable promotion of the long-running `1.2.0-rc.3` candidate, closing out a release cycle that included RC1 feature work and RC2/RC3 fixes. In parallel, the "Reborn" pluggable-agent-loop epic (#7482) received a large batch of consolidated implementation and tracking issues, signaling a major architecture push. Performance work is also prominent, with multiple new PRs targeting Postgres write amplification, heartbeat journal churn, and event coalescing. Overall, the project is healthy: stable release, active roadmap execution, and a steady stream of user-reported bugs being triaged.

---

## 2. Releases

### ironclaw-v1.2.0 — 2026-08-13

- **Type:** Stable release
- **Source:** [PR #7625 — chore(release): promote 1.2.0-rc.3 to 1.2.0](https://github.com/nearai/ironclaw/pull/7625)
- **Summary:** Stable promotion of `1.2.0-rc.3`, including the complete RC1 feature set and fixes validated in RC2/RC3.
- **Notable fix documented in release notes:** The runtime container image now installs `curl`, enabling in-container HTTP healthchecks to execute; orchestrators probe the worker with this tooling.
- **Breaking changes / migration notes:** No explicit breaking changes or migration notes were included in the available release-note excerpt.
- The release PR consolidated RC1–RC3 changelog entries under the stable heading, so the full changelog should be treated as the cumulative 1.2.0 record.

---

## 3. Project Progress

The last 24 hours saw 26 merged/closed PRs. Notable advances:

- **[#7625 — Release 1.2.0](https://github.com/nearai/ironclaw/pull/7625):** Promoted `1.2.0-rc.3` to stable `1.2.0`, with package manifest and lockfile updated together.
- **[#7163 — Structural docx/xlsx/pptx editing + PDF from HTML](https://github.com/nearai/ironclaw/pull/7163):** Closed the deferred "real document round-trip capability" item from #6898, and fixed the #7109 text-log regression. Users can now edit Office documents structurally and render PDFs from HTML.
- **[#7581 — Refresh bundled MCP state after auth](https://github.com/nearai/ironclaw/pull/7581):** Fixed a bug where active tools showed as `setup_needed` in Extensions after OAuth discovery; also preserves newer bundled endpoint/auth/effect policy across upgrades.
- **[#7531 — Repeated-call detection made advisory-only](https://github.com/nearai/ironclaw/pull/7531):** Replaced the sliding-window frequency heuristic with a simpler three-consecutive-identical-calls check, and removed the risk of false-positive "repeated call" warnings affecting the loop.
- **[#7376 — Doc-truth CI gate extended to docs/](https://github.com/nearai/ironclaw/pull/7376):** The guidance path-reference gate now scans the public Mintlify docs, the Chinese locale mirror, and the internal contract corpus. This is part 2/5 of the doc-truth work.
- **[#7590 / #7579 — Live-canary fixes](https://github.com/nearai/ironclaw/pull/7590):** Aligned the bundled-skill marker owner with the runtime mint and widened seeded Slack grants; both changes were driven by first-run failures of the newly scheduled live canary.
- **[#7576 — Kernel admission contract tests](https://github.com/nearai/ironclaw/pull/7576):** Tests-only PR A of the AgentExecution seam train, pinning today's admission behavior before Phase 2 moves it behind the new port.
- **[#7506 — Dependency bump batch](https://github.com/nearai/ironclaw/pull/7506):** 17 dependency updates in the "everything-else" group.

Also opened in the last 24 hours — a new performance train targeting write amplification:

- [#7631 — Coalesce runtime milestone writes](https://github.com/nearai/ironclaw/pull/7631)
- [#7629 — Reduce trigger and outbound state writes](https://github.com/nearai/ironclaw/pull/7629)
- [#7630 — Stress preset to measure per-turn Postgres writes](https://github.com/nearai/ironclaw/pull/7630)
- [#7628 — Remove heartbeat journal churn](https://github.com/nearai/ironclaw/pull/7628)

---

## 4. Community Hot Topics

The most-discussed issues by comment count:

- **[#7482 — Epic: Pluggable agent loops — ACP executor, edge credential injection, kernel architecture](https://github.com/nearai/ironclaw/issues/7482)** — 6 comments. This is the central "IronClaw Reborn" architecture epic. It reframes IronClaw as a kernel that schedules agents and mediates capabilities, while agent loops and integrations become pluggable harnesses. Underlying need: the project wants to support external agent harnesses (Claude Code, pi, codex) without owning the loop itself, while preserving security, audit, and credential isolation.
- **[#6257 — "Invalid value (attachments.mime_type)" error when sending/generating PDF files](https://github.com/nearai/ironclaw/issues/6257)** — 4 comments. Closed bug; likely fixed in the document-handling work. The underlying need was reliable PDF generation/sending through the assistant.
- **[#2117 — ironclaw-bridge: local file/MCP bridge daemon for cloud-hosted deployments](https://github.com/nearai/ironclaw/issues/2117)** — 2 comments. Open since April. Users want cloud-hosted IronClaw to access local laptop resources (Obsidian vaults, project directories). This remains a key gap for hybrid local/cloud workflows.
- **[#7185 — Memory not reliably recalled across conversations](https://github.com/nearai/ironclaw/issues/7185)** — 2 comments. Multiple testers independently observed that context from one conversation is missing in later conversations. This is a high-impact trust issue for long-term memory features.

The large number of newly filed "Reborn" issues (#7606–#7624) also shows strong internal momentum around the pluggable loops epic, with the community/repo converging on a clear delivery ladder: M0 spike → egress edge → foreign-harness execution → capability socket/rollout.

---

## 5. Bugs & Stability

Ranked by severity:

1. **Memory not reliably recalled across conversations** — [#7185](https://github.com/nearai/ironclaw/issues/7185) (open, 2 comments). Multiple IronClaw Champions testers reported that context established in one conversation is missing in later conversations. This is a core product trust issue. No fix PR is linked yet.
2. **Custom MCP with browser/email auth gets stuck during connection** — [#7626](https://github.com/nearai/ironclaw/issues/7626) (open, new). IronClaw's custom-MCP harness hangs when the provider requires browser/email verification (e.g., MKT1 paid access). This blocks paid/protected MCP integrations.
3. **GitHub extension shows as connected after invalid credentials** — [#7627](https://github.com/nearai/ironclaw/issues/7627) (open, new). Entering arbitrary credentials (e.g., "1") makes the extension appear connected, then it fails on actual auth. Misleading connected-state UX and a potential auth-flow bug.
4. **NEAR AI Cloud Sonnet-5 returns 500 errors** — [#7589](https://github.com/nearai/ironclaw/issues/7589) (closed). Three-day 500-error streak reported; related to Anthropic issues in nearai/cloud-api#920. Presumably resolved or escalated externally.
5. **PDF MIME type error** — [#6257](https://github.com/nearai/ironclaw/issues/6257) (closed). Fixed; likely addressed by the document round-trip work in [#7163](https://github.com/nearai/ironclaw/pull/7163).

Fix-related PRs in this window:

- [#7581](https://github.com/nearai/ironclaw/pull/7581) fixes MCP state refresh after auth.
- [#7531](https://github.com/nearai/ironclaw/pull/7531) reduces false-positive loop warnings.
- [#7163](https://github.com/nearai/ironclaw/pull/7163) fixes a text-log regression from #7109.

---

## 6. Feature Requests & Roadmap Signals

Strong roadmap signals:

- **Pluggable agent loops (Reborn epic #7482):** The epic generated 15+ child issues this week. The immediate "build now" item is **[#7624 — v0: ACP harness executor — claude-code as the loop, dev-only yolo](https://github.com/nearai/ironclaw/issues/7624)**. Deferred consolidation items are #7621 (egress edge), #7622 (foreign-harness execution), and #7623 (capability access/rollout).
- **ACP support:** [#7513 — `ic` CLI ACP serve command with streaming + cancel](https://github.com/nearai/ironclaw/pull/7513) is open and would let external tools (Copilot CLI, VS Code) connect to IronClaw agents. This aligns directly with the pluggable loops direction and could land in a near-term release if reviewed and merged.
- **Local file/MCP bridge:** [#2117](https://github.com/nearai/ironclaw/issues/2117) continues to represent real user demand for cloud-hosted IronClaw to access local files.
- **Web UI version display:** [#7580 — Expose IronClaw Reborn version in the web UI](https://github.com/nearai/ironclaw/issues/7580) is a small UX request likely to ship in the next patch/minor.
- **Structured automation contracts:** [#7548 — feat(automations): add structured execution contracts](https://github.com/nearai/ironclaw/pull/7548) is open; if merged, it will require execution contracts for new scheduled automations.

Prediction for next release: a 1.2.x patch including the Web UI version banner (#7580) and possibly the MCP auth-flow fix (#7626); a 1.3.0 or 2.0-preview line depending on whether ACP serve (#7513) and the first foreign-harness executor (#7624) land.

---

## 7. User Feedback Summary

Real user pain points surfaced in the last 24 hours:

- **Memory reliability is the biggest trust gap.** Multiple Champions testers reported that the agent doesn't retain context across conversations — e.g., a legal user's information did not carry over ([#7185](https://github.com/nearai/ironclaw/issues/7185)).
- **Protected MCP providers are hard to use.** A user trying to connect a custom MCP requiring browser/email verification found IronClaw stuck during auth ([#7626](https://github.com/nearai/ironclaw/issues/7626)).
- **Auth state can lie.** The GitHub extension showing "connected" after invalid credentials confuses users and undermines confidence ([#7627](https://github.com/nearai/ironclaw/issues/7627)).
- **Version discoverability is poor.** A user in `#x-ai-product-feedback` could not find the running IronClaw Reborn version in the Web UI ([#7580](https://github.com/nearai/ironclaw/issues/7580)).
- **Cloud model availability affects trust.** Sonnet-5 on NEAR AI Cloud returned 500s for three days ([#7589](https://github.com/nearai/ironclaw/issues/7589)).
- **PDF handling was a positive fix.** The MIME-type error for sending/generating PDFs has been closed ([#6257](https://github.com/nearai/ironclaw/issues/6257)), and structural Office document editing is now supported via [#7163](https://github.com/nearai/ironclaw/pull/7163).

Overall, users are actively testing deep workflows (memory, MCP auth, document round-trips, local-file access) and surfacing real-world friction — a sign of growing adoption beyond basic chat.

---

## 8. Backlog Watch

Issues/PRs that appear to need maintainer attention:

- **[#2117 — ironclaw-bridge (local file/MCP bridge daemon)](https://github.com/nearai/ironclaw/issues/2117)** — Open since 2026-04-07, only 2 comments. This is a long-standing feature request with clear demand (Obsidian vaults, local project directories) but no visible progress beyond the existing tunnel system.
- **[#7185 — Memory not reliably recalled across conversations](https://github.com/nearai/ironclaw/issues/7185)** — Open since 2026-08-04 with no linked fix. Given the severity of the user reports, this deserves prioritization.
- **[#7513 — ACP serve command PR by new contributor](https://github.com/nearai/ironclaw/pull/7513)** — Open since 2026-08-11, no comments recorded, contributor labeled "new." This is a substantial feature PR and may need maintainer review/guidance.
- **[#7020 — tokio-tungstenite dependency bump](https://github.com/nearai/ironclaw/pull/7020)** — Open since 2026-08-02; routine dependency update that has not yet been merged.
- **[#7378 — Doc-fact contract tests for CLI/manifest/Responses claims](https://github.com/nearai/ironclaw/pull/7378)** — Open since 2026-08-07; part 3/5 of the doc-truth series. It is a deterministic CI gate and presumably waiting on review alongside #7376.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

## 1. Today's Overview

As of 2026-08-14, LobsterAI shows moderate-to-high activity: 11 PRs were updated in the last 24 hours — 6 closed and 5 still open — while only 1 issue was updated and remains open. No new releases were published. The project is currently in a UI/UX consolidation and reliability-hardening phase, with notable PRs covering cowork management UI, unified skills/MCP views, evergreen daily check-in, enterprise edition work, and scheduled task fixes. A number of stale-labelled PRs from late March were also touched, suggesting ongoing maintainer/contributor triage of a pending backlog.

## 2. Releases

None. No new releases or tags were reported in the last 24 hours, so no release notes, breaking changes, or migration guidance are available.

## 3. Project Progress

Six PRs were closed in the last 24 hours. Based on their titles/descriptions, the main areas of progress were UI/UX consolidation, feature work, and a scheduled-task reliability fix.

- **UI/UX refactor: cowork management** — [PR #2488](https://github.com/netease-youdao/LobsterAI/pull/2488) "Refactor/cowork btw and management UI" was closed.
- **Unified skills + MCP management view** — [PR #2487](https://github.com/netease-youdao/LobsterAI/pull/2487) "refactor(skills): merge skills and mcp views into unified skills-and-connectors view" was closed.
- **MCP UI unification** — [PR #2486](https://github.com/netease-youdao/LobsterAI/pull/2486) "refactor(mcp): unify MCP card/detail UI with kits and skills styling" was closed. It introduced shared `McpCard`/`McpDetailModal` components and standardized card/detail typography.
- **Evergreen daily check-in** — [PR #2485](https://github.com/netease-youdao/LobsterAI/pull/2485) "feat(activity): support evergreen daily check-in" was closed. It converts the old check-in activity into a permanent evergreen form and adds automatic status refresh.
- **Enterprise edition work** — [PR #2484](https://github.com/netease-youdao/LobsterAI/pull/2484) "Feat/enterprise edition" was closed, though the provided summary is sparse.
- **Scheduled task first-run fix** — [PR #1232](https://github.com/netease-youdao/LobsterAI/pull/1232) "fix(scheduledTask): 修复定时任务首次执行结果不推送到 UI 的问题" was closed. It fixes a bug where the first-ever scheduled task execution did not send `runUpdate` notifications to the UI due to a `previousRunAtMs > 0` condition.

## 4. Community Hot Topics

Community discussion was minimal in this window. The only item with an explicit comment count is:

- [Issue #1162](https://github.com/netease-youdao/LobsterAI/issues/1162) — **"为 openclawMemoryFile 和 openclawLocalTimeContextPrompt 补充 Vitest 单元测试"** (1 comment, 0 reactions). It requests Vitest coverage for OpenClaw's core memory file manager and local time-context prompt generator. Both modules are said to have zero existing test coverage despite being heavily used in memory synchronization and prompt generation.

The underlying need here is **maintainability/risk reduction**: `openclawMemoryFile.ts` is a critical memory-management module (MEMORY.md read/write, SQLite migration, workspace switching), and the lack of tests increases the risk of silent regressions. This is reinforced by related open PRs [#1156](https://github.com/netease-youdao/LobsterAI/pull/1156) and [#1165](https://github.com/netease-youdao/LobsterAI/pull/1165), which add tests for safety/quality modules.

## 5. Bugs & Stability

No regressions or crashes were explicitly reported in the data. Several bug-fix PRs are in various states.

**High severity:**

- **OpenClaw skill enable/disable silently ineffective** — [PR #2483](https://github.com/netease-youdao/LobsterAI/pull/2483) (open) fixes a dangerous/config-correctness bug: OpenClaw skill `entries` are keyed by directory name instead of the skill's frontmatter `name`. When directory and frontmatter names differ, UI toggles have no effect. The PR directly addresses the skill-key mismatch reported in [issue #244](https://github.com/netease-youdao/LobsterAI/issues/244) (truncated in source, but referenced).

**Medium severity:**

- **No feedback when manually running scheduled tasks** — [PR #1163](https://github.com/netease-youdao/LobsterAI/pull/1163) (open, stale-labelled). Users cannot tell if "Run manually" was triggered; the IPC handler blocks until execution finishes, and task state relies on up to 15s polling. The PR adds optimistic updates, loading state, and Gateway status sync.
- **First scheduled task result not pushed to UI** — [PR #1232](https://github.com/netease-youdao/LobsterAI/pull/1232) (closed). Users had to wait until the *second* execution to see results in the UI. Fix is closed and likely available.

**Lower severity / usability:**

- **Duplicate custom agent names allowed** — [PR #1166](https://github.com/netease-youdao/LobsterAI/pull/1166) (open, stale-labelled). The create-agent modal does not validate against existing agent names, leading to ambiguous lists. The proposed fix checks duplicates in renderer state before submission.

## 6. Feature Requests & Roadmap Signals

Recent closed PRs point to the following roadmap directions:

- **Unified skills/MCP management console** — [#2487](https://github.com/netease-youdao/LobsterAI/pull/2487) and [#2486](https://github.com/netease-youdao/LobsterAI/pull/2486) suggest LobsterAI is consolidating separate skills, MCP, and "kits" UI into a single shared management surface.
- **Evergreen engagement/gamification** — [#2485](https://github.com/netease-youdao/LobsterAI/pull/2485) makes daily check-in a permanent feature rather than a temporary campaign.
- **Enterprise edition** — [#2484](https://github.com/netease-youdao/LobsterAI/pull/2484) signals enterprise-oriented packaging or capabilities are actively being developed.
- **Scheduled task UX reliability** — [#1163](https://github.com/netease-youdao/LobsterAI/pull/1163) and [#1232](https://github.com/netease-youdao/LobsterAI/pull/1232) indicate a push to make scheduled-task behavior more responsive and observable.
- **Test coverage for safety-critical modules** — [#1156](https://github.com/netease-youdao/LobsterAI/pull/1156) and [#1165](https://github.com/netease-youdao/LobsterAI/pull/1165) are strong signals that maintainers/contributors are prioritizing memory, command-safety, and prompt-context correctness.

Likely next-version themes: unified skills/MCP/connectors UI, evergreen daily check-in, enterprise features, and improved task-feedback/status handling.

## 7. User Feedback Summary

No direct user satisfaction surveys were available, but several concrete pain points are visible from issue/PR descriptions:

- **Scheduled tasks are hard to trust**: users cannot tell whether "Run manually" succeeded ([#1163](https://github.com/netease-youdao/LobsterAI/pull/1163)), and the first-ever execution can silently miss UI updates ([#1232](https://github.com/netease-youdao/LobsterAI/pull/1232)).
- **Agent naming ambiguity**: duplicate custom agent names create confusion and force manual cleanup ([#1166](https://github.com/netease-youdao/LobsterAI/pull/1166)).
- **OpenClaw skill toggles can silently fail**: users may enable/disable skills from the UI but the override has no actual effect when directory/frontmatter names differ ([#2483](https://github.com/netease-youdao/LobsterAI/pull/2483)).
- **Maintainer concern: missing tests in critical modules**: core memory management and command safety modules have zero test coverage, which raises risk for future changes ([#1162](https://github.com/netease-youdao/LobsterAI/issues/1162), [#1156](https://github.com/netease-youdao/LobsterAI/pull/1156), [#1165](https://github.com/netease-youdao/LobsterAI/pull/1165)).

## 8. Backlog Watch

Several items have been open for roughly 4.5 months (created 2026-03-31, updated 2026-08-13) and are marked stale. They appear to need maintainer attention or triage:

- **Issue [#1162](https://github.com/netease-youdao/LobsterAI/issues/1162)** *(open, stale)* — Request to add Vitest tests for `openclawMemoryFile` and `openclawLocalTimeContextPrompt`.
- **PR [#1156](https://github.com/netease-youdao/LobsterAI/pull/1156)** *(open, stale)* — Adds Vitest tests for `commandSafety.ts` and `coworkMemoryJudge.ts`, two safety/quality gate modules with zero coverage.
- **PR [#1165](https://github.com/netease-youdao/LobsterAI/pull/1165)** *(open, stale)* — Adds 75 Vitest tests for `openclawMemoryFile.ts` and `openclawLocalTimeContextPrompt.ts`.
- **PR [#1163](https://github.com/netease-youdao/LobsterAI/pull/1163)** *(open, stale)* — Scheduled-task "immediate run" feedback fix.
- **PR [#1166](https://github.com/netease-youdao/LobsterAI/pull/1166)** *(open, stale)* — Prevents duplicate custom agent names.

Among these, the test-coverage PRs ([#1156](https://github.com/netease-youdao/LobsterAI/pull/1156), [#1165](https://github.com/netease-youdao/LobsterAI/pull/1165)) are especially important because they target safety-critical and memory-critical modules. Their stale status suggests they have not been reviewed or merged for a long time, and they could become merge conflicts if not addressed soon.

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis Project Digest — 2026-08-14

## 1. Today’s Overview
Moltis saw modest activity over the last 24 hours: 1 open issue was updated and 4 pull requests remain open, with no merges, closures, or new releases. The main theme is reliability tooling: three PRs from Lstarsky0 address broken install paths and macOS script compatibility, while one feature PR from penso adds durable CalDAV and channel history connectors. A flaky full-suite test was also reported and is being tracked as issue #1193. Project health appears stable, but no work has landed to `main` today. Maintainer review is needed on the open PRs to convert churn into merged progress.

## 2. Releases
No new releases were published for Moltis in this period.

## 3. Project Progress
No pull requests were merged or closed today. Open PRs that advanced substantive work:

- **#1190 — Add durable CalDAV and channel history connectors**  
  Author: penso | Updated: 2026-08-13  
  Adds provider-neutral connector persistence, atomic snapshots, scheduling, projections, bounded local full-text search, read-only CalDAV datasets, and reusable Slack/Discord/Matrix/Teams message-history datasets. This is the main feature candidate waiting for review.  
  https://github.com/moltis-org/moltis/pull/1190

- **#1191 — fix(sandbox): point gogcli module path at the openclaw org**  
  Author: Lstarsky0 | Updated: 2026-08-13  
  Fixes broken `moltis sandbox build` caused by `github.com/steipete/gogcli` redirecting to `github.com/openclaw/gogcli`.  
  https://github.com/moltis-org/moltis/pull/1191

- **#1192 — fix(skills): point wacrawl install metadata at the openclaw org**  
  Author: Lstarsky0 | Updated: 2026-08-13  
  Fixes the `wacrawl` skill’s Go install fallback after the project moved to the `openclaw` organization.  
  https://github.com/moltis-org/moltis/pull/1192

- **#1194 — fix(scripts): guard empty bash array expansions for macOS bash 3.2**  
  Author: Lstarsky0 | Updated: 2026-08-13  
  Fixes `just local-validate-full` failing under `set -euo pipefail` on macOS when the `args[@]` array is empty.  
  https://github.com/moltis-org/moltis/pull/1194

## 4. Community Hot Topics
The most active item is the only newly surfaced issue, though it currently has 0 comments and 0 reactions:

- **#1193 [OPEN] Flaky test: push fanout timeout assertion races under full-suite load**  
  Author: Lstarsky0 | Created: 2026-08-13 | Updated: 2026-08-13 | Comments: 0 | 👍: 0  
  Test `moltis-gateway push::tests::fanout_is_bounded_and_times_out_a_hung_endpoint` intermittently fails, but only when the full workspace suite runs. It failed 2 of 3 full-suite runs on an otherwise idle 10-core macOS machine.  
  https://github.com/moltis-org/moltis/issues/1193

The open PRs also have no recorded comments/reactions, suggesting discussion is happening outside GitHub or simply hasn’t started yet. Underlying need: the community is hitting upstream module renames (`steipete` → `openclaw`) and macOS packaging quirks, plus test infrastructure flakiness that reduces confidence in full-suite validation.

## 5. Bugs & Stability
Ranked by severity:

1. **High — `moltis sandbox build` fails on every pre-built image**  
   #1191: The generated Dockerfile references `github.com/steipete/gogcli/cmd/gog@latest`, but gogcli moved to the `openclaw` org, so GitHub redirects break the install. Fix PR exists: #1191.  
   https://github.com/moltis-org/moltis/pull/1191

2. **Medium — `wacrawl` skill install fallback broken**  
   #1192: The `requires.install` entry points to `github.com/steipete/wacrawl/cmd/wacrawl@latest`, but the module now declares `github.com/openclaw/wacrawl`. Fix PR exists: #1192.  
   https://github.com/moltis-org/moltis/pull/1192

3. **Medium/Low — macOS local validation script fails with empty arrays**  
   #1194: `just local-validate-full` without a PR number dies on macOS bash 3.2 with `args[@]: unbound variable`. Fix PR exists: #1194.  
   https://github.com/moltis-org/moltis/pull/1194

4. **Low but reliability-relevant — flaky fanout timeout test under full suite**  
   #1193: The test only fails when the whole workspace suite runs, indicating a possible timeout/race or resource contention issue rather than a deterministic logic bug. No fix PR yet.  
   https://github.com/moltis-org/moltis/issues/1193

## 6. Feature Requests & Roadmap Signals
- **PR #1190 is the clearest roadmap signal** for the next version: durable connector persistence, CalDAV read-only datasets, and message-history connectors for Slack, Discord, Matrix, and Microsoft Teams. This would expand Moltis beyond push/fanout into long-term channel history and scheduling.
- The rename fixes (#1191, #1192) are required maintenance and will likely be merged before or alongside #1190.
- No explicit user-submitted feature requests appeared in the last 24 hours, but the proactive connector work suggests the maintainers are prioritizing external data-source integrations.

Prediction: the next Moltis release will include the openclaw module-path fixes and possibly the connector persistence work if #1190 gets reviewed and merged soon.

## 7. User Feedback Summary
User pain points visible in today’s data:

- **Upstream dependency moves cause immediate breakage** – two separate PRs (#1191, #1192) were needed because `gogcli` and `wacrawl` moved from the `steipete` org to `openclaw`. This indicates a fragile dependency-on-fork pattern that users feel directly when building sandboxes or installing skills.
- **macOS is a first-class environment but not fully covered** – the bash 3.2 array issue (#1194) shows scripts are tested on Linux-style `bash` but fail on macOS's older default shell.
- **Full-suite reliability is a concern** – issue #1193 reports intermittent failures only under full-suite load, which can erode developer trust in CI and slow down contributions.

No positive/negative user satisfaction comments were captured in this window, so sentiment is inferred from the issues and PRs themselves.

## 8. Backlog Watch
- **#1190 — Add durable CalDAV and channel history connectors**  
  Open since 2026-08-11, updated 2026-08-13, with no recorded comments. This is a large feature PR that needs maintainer review and potentially design discussion. If left unreviewed, it may become stale due to its size.  
  https://github.com/moltis-org/moltis/pull/1190

- **#1193 — Flaky test report**  
  Fresh, but likely to need maintainer input on whether to quarantine/skip the test under full-suite load or adjust timeout handling. No assignee or fix PR yet.  
  https://github.com/moltis-org/moltis/issues/1193

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw (QwenPaw) Project Digest — 2026-08-14

**Data source:** [github.com/agentscope-ai/QwenPaw](https://github.com/agentscope-ai/QwenPaw) · Activity window: 2026-08-13 → 2026-08-14

---

## 1. Today's Overview

CoPaw shows very high release-day activity: 42 issues and 50 PRs were touched in the last 24 hours, with a healthy closure rate (17 issues and 19 PRs closed/merged vs. 25 open issues and 31 open PRs). The headline event is the **v2.1.0 release**, which introduces the QwenPaw OS Shell — a windowed app-launcher experience with taskbar, notifications, and saved layouts. Behind the release, the community is reporting friction concentrated in three areas: **agent task-continuation reliability**, **context compaction transparency**, and **Windows/security-tooling conflicts**. Contributor diversity is strong — four of today's most active PRs are from first-time contributors, and the maintainer team is simultaneously landing memory, provider, and channel fixes. Overall, the project is in a rapid, healthy iteration phase immediately after a major feature release.

---

## 2. Releases

### [v2.1.0](https://github.com/agentscope-ai/QwenPaw/releases) — Stable (headline)
Based on the visible changelog:
- **✨ QwenPaw OS Shell**: open apps in movable, resizable windows with a launcher, taskbar, notifications, and saved layouts ([#6645](https://github.com/agentscope-ai/QwenPaw/pull/6645)).
- Installed and marketplace apps now share **one unified catalog** across the App Center (changelog truncated in source data).

> ⚠️ **Migration note:** The truncated changelog does not list breaking changes. Given the shell is a new UI layer, users on 2.1.0-beta.x should verify saved layouts, window state, and app-catalog entries after upgrade. No data migration warnings were visible in the release notes.

### [v2.1.0-beta.5](https://github.com/agentscope-ai/QwenPaw/releases) — Pre-release
- `fix(chats): handle dict-like model responses` ([#6813](https://github.com/agentscope-ai/QwenPaw/issues/6813) / [PR #6816](https://github.com/agentscope-ai/QwenPaw/pull/6816)) — makes chat rendering resilient to providers returning dict-shaped message payloads.
- `fix(memory): simplify long-term memory guidance` ([#6942](https://github.com/agentscope-ai/QwenPaw/pull/6942)) — reduces prompt complexity around LTM behavior.
- `docs(website): make the Files workspace` — documentation refresh for the Files workspace (truncated).

---

## 3. Project Progress

**Merged/closed PRs today** (from the top-20 list; all URLs are QwenPaw pull requests):

- **[#6884](https://github.com/agentscope-ai/QwenPaw/pull/6884) — `fix: make Auto-Dream integration resilient`** *(closed, first-time contributor)* — A single malformed/empty structured output from the LLM previously failed the entire Auto-Dream memory task; now failed units are isolated from successful ones.
- **[#6387](https://github.com/agentscope-ai/QwenPaw/pull/6387) — `feat(channels): install optional dependencies on demand`** *(closed)* — Channel-specific SDKs move out of the default dependency set; the Console keeps unavailable channels visible but grayed-out with install prompts.
- **[#6652](https://github.com/agentscope-ai/QwenPaw/pull/6652) — `fix(mission): enforce max_iterations server-side in MissionGate`** *(closed, fixes [#6505](https://github.com/agentscope-ai/QwenPaw/issues/6505))* — Mission Mode could previously dispatch unbounded sub-agents (observed 54+ sub-sessions instead of the configured 20); now enforced server-side.
- **[#6636](https://github.com/agentscope-ai/QwenPaw/pull/6636) — `fix(chats): add pagination to chat history and enable GZip compression`** *(closed, fixes [#6635](https://github.com/agentscope-ai/QwenPaw/issues/6635))* — Chat history endpoint no longer returns 1MB+ transcripts in one response, eliminating 30s slow-network timeouts.
- **[#6989](https://github.com/agentscope-ai/QwenPaw/pull/6989) — `chore: update release notes for v2.1.0`** *(closed)* — Release-note preparation for the v2.1.0 ship.

**Closed issues signaling landed fixes/decisions:** [#6811](https://github.com/agentscope-ai/QwenPaw/issues/6811) (OpenAI Responses continuation-summary `disable_thinking` bug), [#6853](https://github.com/agentscope-ai/QwenPaw/issues/6853) (Dream→MEMORY.md prompt falsehood), [#6047](https://github.com/agentscope-ai/QwenPaw/issues/6047) (stale session ordering after upgrade), [#6100](https://github.com/agentscope-ai/QwenPaw/issues/6100) (workspace/`agent.json` loss on upgrade), [#6916](https://github.com/agentscope-ai/QwenPaw/issues/6916) (plugin cron/message-injection permission gap), [#6768](https://github.com/agentscope-ai/QwenPaw/issues/6768) (infinite loop after multi-step tasks).

---

## 4. Community Hot Topics

The most-discussed items in the last 24h (by comment count):

- **[#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921) — Agent stops right after saying "Now 2.1, 3.1, 3.2. Let me do all three."** *(open, 6 comments, 0 👍)* — On Windows 11 / v2.1beta2, the model plans the next step, outputs a "Let me do all three" style message, then halts with **no visible prompt** until the user types "继续" (continue). This is the single most-commented issue today and signals an execution-loop reliability problem users experience as "the agent gives up after planning."
- **[#6973](https://github.com/agentscope-ai/QwenPaw/issues/6973) — Support Alibaba Cloud Bailian token plans in qwenpaw creator** *(open, 5 comments)* — Chinese users want to use prepaid/packaged Bailian token plans instead of pay-as-you-go API billing.
- **[#6811](https://github.com/agentscope-ai/QwenPaw/issues/6811) — OpenAI Responses continuation summary ignores `disable_thinking` and misreports 60s cancellation** *(closed, 5 comments)* — Eviction-triggered continuation summaries block the main conversation for up to a minute and surface as "malformed output."
- **[#6853](https://github.com/agentscope-ai/QwenPaw/issues/6853) — `prompts.py` lies to agents: Dream writes to `digest/`, not `MEMORY.md`** *(closed, 5 comments)* — Docs/prompts claim dream auto-syncs digests into `MEMORY.md`, but the pipeline never implemented that. Closed today, indicating a prompt/doc fix was accepted.
- **[#6847](https://github.com/agentscope-ai/QwenPaw/issues/6847) — Same task/model: QwenPaw gets killed by antivirus, WorkBuddy doesn't** *(open, 4 comments)* — AV vendors are flagging QwenPaw's execution patterns (likely sandbox/shell behavior).
- **[#6780](https://github.com/agentscope-ai/QwenPaw/issues/6780) — v2.0.1 freezes after being idle for tens of minutes** *(open, 4 comments)* — Requires process kill/restart.
- **[#6882](https://github.com/agentscope-ai/QwenPaw/issues/6882) — How to integrate CopilotKit?** *(closed, 4 comments)* — Ecosystem/embedding demand unanswered by docs at the time; now closed, presumably answered.

**Underlying needs:** (a) users need agents to *execute* announced plans without manual "continue" nudges; (b) Chinese-market users want cost-efficient provider plans (Bailian); (c) users expect prompts/docs to reflect actual memory behavior; (d) embeddable/ecosystem integration (CopilotKit) is a recurring request.

---

## 5. Bugs & Stability

Ranked by severity:

| # | Issue | Severity | Status | Notes / Fix PR |
|---|-------|----------|--------|----------------|
| 1 | **[#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921)** — Mid-task stall after planning ("Let me do all three." → silence) | **High** | Open | Top community pain; no fix PR visible yet. |
| 2 | **[#6951](https://github.com/agentscope-ai/QwenPaw/issues/6951)** — After Scroll compaction `/compact`, pre-compaction transcript invisible; UI shows only internal eviction index | **High** | Open | User-visible transcript loss; compaction should only affect model input, not UI history. |
| 3 | **[#6992](https://github.com/agentscope-ai/QwenPaw/issues/6992) / [#6993](https://github.com/agentscope-ai/QwenPaw/issues/6993)** — Security report: 0.0.0.0:8088 exposure, unauthenticated plugin install API, arbitrary command execution, persistence claims | **High (security)** | Closed (one as `invalid`, one as `bug`, both same reporter/PDF) | Duplicate reports; closed. Recommend a maintainer public statement if the report is a false positive, given incident-report severity. |
| 4 | **[#7008](https://github.com/agentscope-ai/QwenPaw/issues/7008)** — Anthropic false-positive "input sensitive image" (1026) kills long sessions with ordinary historical images | **Medium-High** | Open | Model-side moderation; needs client-side image sanitization or retry/fallback strategy. |
| 5 | **[#6780](https://github.com/agentscope-ai/QwenPaw/issues/6780)** — Idle freeze after tens of minutes (v2.0.1) | **Medium-High** | Open | No maintainer response visible yet. |
| 6 | **[#7007](https://github.com/agentscope-ai/QwenPaw/issues/7007)** — Windows Desktop TUI fails: `transport: Connection closed` because packaged `qwenpaw.exe` rejects `-m qwenpaw acp` | **Medium** | Open | New regression in v2.1.0 packaging. |
| 7 | **[#6966](https://github.com/agentscope-ai/QwenPaw/issues/6966)** — Telegram `/new` doesn't rotate session ID → context grows indefinitely via `scroll` history.db | **Medium** | Open | Channel session-identity bug. |
| 8 | **[#6955](https://github.com/agentscope-ai/QwenPaw/issues/6955)** — Intermittent startup crash on Windows, pip-installed v2.0.1 | **Medium** | Open | Windows event-loop traceback (`finish_socket_func`). |
| 9 | **[#7005](https://github.com/agentscope-ai/QwenPaw/issues/7005)** — Enabling Shabox breaks `uv run` (cannot write `~/.cache/uv`) | **Medium** | Open | Workaround exists via policy.yaml `Write(~/.cache/uv/**)`. |
| 10 | **[#6768](https://github.com/agentscope-ai/QwenPaw/issues/6768)** — Infinite loop after completing multi-step task; session blocked for hours | **Medium** | Closed (`need-info`) | Root-caused to executor loop; closed with diagnostics quality flag. |
| 11 | **[#7006](https://github.com/agentscope-ai/QwenPaw/issues/7006)** — Language-options list inconsistent between dropdown and settings gear | **Low** | Open | Agent-authored report; minor UI consistency. |

**Fix PRs in flight today:** [#6998](https://github.com/agentscope-ai/QwenPaw/pull/6998) (semaphore leak from unconsumed LLM streams — fixes [#5411](https://github.com/agentscope-ai/QwenPaw/issues/5411)), [#6975](https://github.com/agentscope-ai/QwenPaw/pull/6975) (context-usage ring not reset after `/compact`), [#6996](https://github.com/agentscope-ai/QwenPaw/pull/6996) (plugin workspace state not restored before reload swap), [#6884](https://github.com/agentscope-ai/QwenPaw/pull/6884) (Auto-Dream malformed-output isolation, already closed).

---

## 6. Feature Requests & Roadmap Signals

**User-requested features today:**

- **[#7002](https://github.com/agentscope-ai/QwenPaw/issues/7002)** — Thin **server-deployed proxy client**: install a lightweight agent on a personal PC that connects to a server-side QwenPaw, retaining desktop-control abilities without the heavy desktop client. Strong architectural signal for headless/remote agent use.
- **[#6970](https://github.com/agentscope-ai/QwenPaw/issues/6970)** — Embeddable chat sub-page (no sidebar/header), URL `apikey` auth for sessions, and session-list filtering by date/sessionId. Points to B2B embedding demand.
- **[#6995](https://github.com/agentscope-ai/QwenPaw/issues/6995)** — Inject `QWENPAW_CHANNEL` env var into shell subprocesses so external tools know the originating channel. Small, high-value for multi-channel users.
- **[#6973](https://github.com/agentscope-ai/QwenPaw/issues/6973)** — Alibaba Cloud Bailian token-plan support in the creator/console.
- **[#7003](https://github.com/agentscope-ai/QwenPaw/issues/7003)** — "ViBo" memory proposal claiming 97.5% fewer tokens (encrypted memory, external). Third-party promotional proposal; should be triaged for genuine memory-economics interest.
- **[#6945](https://github.com/agentscope-ai/QwenPaw/issues/6945)** — Clarification/feature: smart-mode writes outside the sandbox fail; users expect smart mode to execute, not just approve.

**PRs pointing at the near-term roadmap (all open today):**
- [#6960](https://github.com/agentscope-ai/QwenPaw/pull/6960) — **PawPort**: import flow from other agents (Codex, Qoder) for instructions, skills, plugins, projects, recent work.
- [#6976](https://github.com/agentscope-ai/QwenPaw/pull/6976) — **Session-scoped multi-project directories** (ordered list, primary cwd).
- [#6984](https://github.com/agentscope-ai/QwenPaw/pull/6984) — **ReMe memory runtime dashboard** (health, background capture, resource diagnostics, maintenance actions).
- [#7001](https://github.com/agentscope-ai/QwenPaw/pull/7001) — **Matrix per-sender session/memory isolation** in group rooms.
- [#7004](https://github.com/agentscope-ai/QwenPaw/pull/7004) — Persist spawn parent-child linkage in chat meta.
- [#6302](https://github.com/agentscope-ai/QwenPaw/pull/6302) — Large unification of **provider discovery, model metadata, routing, and agent controls** (open since 07-21; likely a v2.2 centerpiece).

**Prediction:** v2.1.x patch releases will absorb console/memory fixes (context-ring, ReMe docs); PawPort ([#6960](https://github.com/agentscope-ai/QwenPaw/pull/6960)) and multi-project directories ([#6976](https://github.com/agentscope-ai/QwenPaw/pull/6976)) are the most likely new features to land next, while provider unification ([#6302](https://github.com/agentscope-ai/QwenPaw/pull/6302)) anchors the following minor version.

---

## 7. User Feedback Summary

Synthesized from today's 42 updated issues:

- **Chinese-speaking users are the dominant feedback group** (~60% of top issues are written in Chinese). Core complaints are about *behavioral reliability*, not feature completeness: agents stalling after announcing plans ([#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921)), freezing when idle ([#6780](https://github.com/agentscope-ai/QwenPaw/issues/6780)), crashing at startup ([#6955](https://github.com/agentscope-ai/QwenPaw/issues/6955)), and being killed by antivirus software ([#6847](https://github.com/agentscope-ai/QwenPaw/issues/6847)) — the last one implying real-world desktop-automation use is being blocked by security tooling.
- **Upgrade/data-safety anxiety persists**: two closed issues today recount post-upgrade data loss ([#6100](https://github.com/agentscope-ai/QwenPaw/issues/6100) workspace loss, [#6047](https://github.com/agentscope-ai/QwenPaw/issues/6047) session mix-up). Their closure is good news, but users are clearly watching for regressions across upgrades.
- **Memory transparency matters**: users are unhappy when compaction hides their own transcript ([#6951](https://github.com/agentscope-ai/QwenPaw/issues/6951)) or when prompts claim behavior that doesn't exist ([#6853](https://github.com/agentscope-ai/QwenPaw/issues/6853)).
- **Ecosystem/cost pressure**: Bailian token plans ([#6973](https://github.com/agentscope-ai/QwenPaw/issues/6973)), CopilotKit integration ([#6882](https://github.com/agentscope-ai/QwenPaw/issues/6882)), and embedding/API capabilities ([#6970](https://github.com/agentscope-ai/QwenPaw/issues/6970)) reflect real deployment needs.
- **Positive signals:** the project is referenced as "impressive — 33,748 stars" ([#7003](https://github.com/agentscope-ai/QwenPaw/issues/7003)) and one user opens with "非常不错的项目" ("a really great project") ([#6585](https://github.com/agentscope-ai/QwenPaw/issues/6585)). A high fraction of today's closed items indicates the team is responding quickly.

---

## 8. Backlog Watch

Items needing maintainer attention (open, unanswered or long-dormant):

- **[#6715](https://github.com/agentscope-ai/QwenPaw/pull/6715)** — `feat(onebot): localize inbound media before agent processing` · open since **08-05**, "Under Review", **zero comments**. OneBot channel inbound media security/size-limit work appears stuck in review.
- **[#6302](https://github.com/agentscope-ai/QwenPaw/pull/6302)** — `feat: unify provider discovery, model metadata, routing, and agent controls` · open since **07-21**, zero comments. Large architectural PR; likely needs a maintainer decision (merge, split, or close) to avoid bit-rotting.
- **[#6823](https://github.com/agentscope-ai/QwenPaw/pull/6823)** — `feat(providers): apply documented capability templates to custom providers` · open since **08-08**, first-time contributor, zero comments. Simple, useful change; risks fading.
- **[#6780](https://github.com/agentscope-ai/QwenPaw/issues/6780)** — v2.0.1 idle freeze · open since **08-07**, 4 comments, no visible maintainer reply.
- **[#6847](https://github.com/agentscope-ai/QwenPaw/issues/6847)** — Antivirus kills QwenPaw · open since **08-09**, 4 comments, no maintainer reply. AV false-positive handling needs official guidance (code-signing, allowlist docs).
- **[#6966](https://github.com/agentscope-ai/QwenPaw/issues/6966)** — Telegram `/new` session rotation bug · open since **08-13**, 1 comment; directly affects channel memory growth.
- **[#7003](https://github.com/agentscope-ai/QwenPaw/issues/7003)** — ViBo memory proposal · open, 2 comments; should get a triage response (adopt / decline / "point us to a discussion").
- **[#6992](https://github.com/agentscope-ai/QwenPaw/issues/6992)** — Security incident report closed as `invalid` with no visible public technical rebuttal; given the PDF's claims (exposed 0.0.0.0:8088, unauthenticated plugin API), a brief maintainer note confirming the assessment would prevent confusion and duplicate reports.

---

*Digest generated 2026-08-14 · All metrics derived from public issue/PR activity in the preceding 24h. PR comment counts were not exposed in the source snapshot.*

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-08-14

## 1. Today's Overview

ZeroClaw logged a very high-activity window: 50 issues and 50 PRs were updated in the last 24 hours, with 13 issues closed and 7 PRs merged/closed. No releases shipped. The dominant themes are security hardening (gateway asset containment, provider credential integrity, pairing lockout) and large architectural RFCs targeting v0.9.0 (goal mode, unified shell permission policy, runtime-owned sessions). Maintainer response on p1 security items is strong, but a visible queue of RFCs is awaiting maintainer decision, formally tracked in [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692). Overall project health looks solid — this is a deliberate design-and-hardening phase rather than a feature-shipping phase.

## 2. Releases

No new releases in the last 24 hours.

## 3. Project Progress

Merged/closed PRs today (7):

- **Security — gateway dashboard asset containment** [#9969](https://github.com/zeroclaw-labs/zeroclaw/pull/9969): canonicalizes filesystem-backed dashboard asset paths and rejects symlink escapes outside the configured distribution root.
- **Infra — session queue eviction race** [#9674](https://github.com/zeroclaw-labs/zeroclaw/pull/9674): registers session requests while the session-slot map is still locked so idle eviction cannot remove a selected slot before its pending count is visible.
- **Docs — provider routing lifecycle** [#9639](https://github.com/zeroclaw-labs/zeroclaw/pull/9639): documents profile construction, hint routing, retry/fallback order, cooldowns, streaming recovery, and no-replay boundaries.
- **CLI — localized status output** [#8546](https://github.com/zeroclaw-labs/zeroclaw/pull/8546): routes remaining `zeroclaw status` risk-profile summary fragments through Fluent i18n keys, including Web UI availability text.
- **CI — CodeQL false positives** [#9932](https://github.com/zeroclaw-labs/zeroclaw/pull/9932): drops the `rust/hard-coded-cryptographic-value` query (27 "critical" alerts, all false positives in `cfg(test)`).
- **CI — Blacksmith Docker layer cache** [#9980](https://github.com/zeroclaw-labs/zeroclaw/pull/9980): sticky-disk layer cache for PR image builds on Blacksmith runners.
- **CI — validation only** [#9984](https://github.com/zeroclaw-labs/zeroclaw/pull/9984): temporary same-repo PR exercising the Blacksmith rust-cache path; closed after validation.

Notable closed issues today: security bug [#9389](https://github.com/zeroclaw-labs/zeroclaw/issues/9389) (pairing lockout bypass), feature [#9712](https://github.com/zeroclaw-labs/zeroclaw/issues/9712) (weekly lettered release cuts), CI bug [#9951](https://github.com/zeroclaw-labs/zeroclaw/issues/9951) (WeChat channel never compiled/executed in CI), and cleanup bugs [#9710](https://github.com/zeroclaw-labs/zeroclaw/issues/9710), [#9706](https://github.com/zeroclaw-labs/zeroclaw/issues/9706), [#9366](https://github.com/zeroclaw-labs/zeroclaw/issues/9366), and [#9643](https://github.com/zeroclaw-labs/zeroclaw/issues/9643).

## 4. Community Hot Topics

Most-commented issues:

- [#8303 RFC: Goal mode v1 — bounded foreground Matrix work](https://github.com/zeroclaw-labs/zeroclaw/issues/8303) — 20 comments, 1 👍. Wants durable bounded user objectives spanning multiple turns; the discussion is pushing to decouple restart handoff, broad channel admission, and async child work from the first delivery.
- [#7155 RFC: Per-execution confirmation tier for high-risk shell commands](https://github.com/zeroclaw-labs/zeroclaw/issues/7155) — 18 comments. Claude Code-style allow/ask/deny policy; Revision 3 narrowed scope to a reconciled shell-policy contract per maintainer direction.
- [#8692 Tracker: Maintainer decision queue for RFCs](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) — 13 comments. Coordination surface for RFCs, design issues, and release-policy items awaiting maintainer action.
- [#6850 RFC: Decouple memory lifecycle policy from storage backends](https://github.com/zeroclaw-labs/zeroclaw/issues/6850) — 12 comments.
- [#9328 Bug: verifiable-intent evaluates constraints without verifying the credential chain](https://github.com/zeroclaw-labs/zeroclaw/issues/9328) — 12 comments.
- [#9487 RFC: Runtime-owned conversation sessions and transport surface adapters](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) — 11 comments.

Underlying signal: the community is heavily engaged in shaping v0.9.0's architecture — session ownership, tool permissioning, and security boundaries are the three contested areas. The existence of a maintainer decision queue (#8692) indicates the bottleneck is decision throughput, not issue discovery.

## 5. Bugs & Stability

**High severity:**

- **verifiable-intent skips credential-chain verification** [#9328](https://github.com/zeroclaw-labs/zeroclaw/issues/9328) (p2, risk:high, accepted, in-progress): `evaluate_constraints` runs on caller-supplied fulfillment without a chain verifier — a validation bypass in the VI reference implementation.
- **Unauthenticated POST /api/pair lockout bypass** [#9389](https://github.com/zeroclaw-labs/zeroclaw/issues/9389) (p1, risk:high, closed): lockout keyed on an attacker-supplied header; closed today.
- **Headless SOP step turns never persisted** [#9929](https://github.com/zeroclaw-labs/zeroclaw/issues/9929) (p1, risk:high, accepted, blocked): headless SOP steps get a session path but are never written to the session store.
- **Zhipu compatible-provider integrity** [PR #9968](https://github.com/zeroclaw-labs/zeroclaw/pull/9968) (p1, risk:high, open): fail-closed fix so a Zhipu credential that cannot produce a valid JWT is rejected instead of forwarded as a raw bearer token; structured JSON serialization prevents credential text from breaking the payload.

**Medium severity:**

- **WeChat channel never compiles in CI** [#9951](https://github.com/zeroclaw-labs/zeroclaw/issues/9951) (p2, closed): 51 lib unit tests silently never ran because no CI feature set enables `channel-wechat`.
- **browser tool exposes only 16 of 100+ commands** [#9945](https://github.com/zeroclaw-labs/zeroclaw/issues/9945) (p2, accepted, blocked): iframes, JS dialogs, tabs, and form controls are unreachable.
- **WhatsApp Web ignores `approval_timeout_secs`** [#9366](https://github.com/zeroclaw-labs/zeroclaw/issues/9366) (p2, closed).
- **OpenRouter prompt-cache blocked** [#9631](https://github.com/zeroclaw-labs/zeroclaw/issues/9631) (p2, blocked, needs-author-action): stable `session_id` not sent, so caching savings are unattainable.

**Low severity:**

- Desktop screenshot temp-file cleanup on early returns [#9710](https://github.com/zeroclaw-labs/zeroclaw/issues/9710) (p3, closed).
- Edge TTS temp-output cleanup on error paths [#9706](https://github.com/zeroclaw-labs/zeroclaw/issues/9706) (p3, closed).

## 6. Feature Requests & Roadmap Signals

Items with status:accepted or strong momentum, likely candidates for the next release:

- **Provider-grouped, paginated Telegram /model picker** [#9895](https://github.com/zeroclaw-labs/zeroclaw/issues/9895) (accepted): mobile UX fix for deployments with many routes.
- **Downscale oversized images instead of dropping them** [#9887](https://github.com/zeroclaw-labs/zeroclaw/issues/9887) (accepted, blocked): also allows `0` to disable multimodal limits.
- **Full browser tool command surface** [#9945](https://github.com/zeroclaw-labs/zeroclaw/issues/9945) (accepted, blocked): unblocks iframes, dialogs, tabs, and form controls.
- **Weekly lettered release cuts** [#9712](https://github.com/zeroclaw-labs/zeroclaw/issues/9712) (closed): SemVer-compatible `v0.8.5-a/b` cuts within a numbered release line; outcome not visible in digest data but closed.

RFCs likely to shape v0.9.0 architecture:

- Goal mode v1 ([#8303](https://github.com/zeroclaw-labs/zeroclaw/issues/8303))
- Unified shell permission policy ([#7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155))
- Runtime-owned conversation sessions ([#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487))
- SOP capability permission contract ([#9598](https://github.com/zeroclaw-labs/zeroclaw/issues/9598))
- Agent Plugins 1.0 skill/MCP loading ([#9810](https://github.com/zeroclaw-labs/zeroclaw/issues/9810))
- Publish-safe exceptions for public blockchain identifiers ([#9825](https://github.com/zeroclaw-labs/zeroclaw/issues/9825))

## 7. User Feedback Summary

- **Cost pressure**: OpenRouter prompt-cache savings via stable `session_id` is a repeated ask ([#9631](https://github.com/zeroclaw-labs/zeroclaw/issues/9631)) — "a single conversation spawns dozens of LLM requests; the system prompt and tool schemas are replayed every turn."
- **Safety vs. usability**: The shell permission discussion ([#7155](https://github.com/zeroclaw-labs/zeroclaw/issues/7155)) shows users want Claude Code-grade guardrails without a second grant system; a new design-ideas issue ([#9978](https://github.com/zeroclaw-labs/zeroclaw/issues/9978)) compares DeepSeek Harness for the permission/sandbox roadmap.
- **Agentic tooling gaps**: browser tool coverage ([#9945](https://github.com/zeroclaw-labs/zeroclaw/issues/9945)) and LSP support for ZeroCode ([#5907](https://github.com/zeroclaw-labs/zeroclaw/issues/5907)) frustrate coding and automation workflows — the LSP request explicitly targets reducing hallucination for local models.
- **Mobile/channel UX**: Telegram /model picker ([#9895](https://github.com/zeroclaw-labs/zeroclaw/issues/9895)) and the WhatsApp `approval_timeout_secs` bug ([#9366](https://github.com/zeroclaw-labs/zeroclaw/issues/9366)) indicate channel UX remains a recurring pain point.
- **False-positive leak detection**: [#9825](https://github.com/zeroclaw-labs/zeroclaw/issues/9825) reports public blockchain addresses being redacted, breaking payment-request URLs — users are asking for publish-safe exceptions rather than looser heuristics.
- Positive contributor signal: many PRs carry "distinguished contributor" / "principal contributor" labels, indicating a recognized, sustained contributor community maintaining review velocity.

## 8. Backlog Watch

Items needing author or maintainer attention, roughly by age:

- [#5907 Opt-in LSP support for ZeroCode](https://github.com/zeroclaw-labs/zeroclaw/issues/5907) — opened 2026-04-19, needs-author-action, 6 comments. Oldest significant feature request; no visible movement in ~4 months.
- [#6850 RFC: Decouple memory lifecycle policy from storage backends](https://github.com/zeroclaw-labs/zeroclaw/issues/6850) — opened 2026-05-22, needs-author-action, 12 comments, risk:high.
- [#6998 Schema-validated memory consolidation with bounded fallback](https://github.com/zeroclaw-labs/zeroclaw/issues/6998) — opened 2026-05-29, needs-author-action, risk:high.
- [#7155 Shell command confirmation tier RFC](https://github.com/zeroclaw-labs/zeroclaw/issues/7155) — opened 2026-06-03, p1, needs-maintainer-review, 18 comments. Highest-profile item waiting on a maintainer decision.
- [#7929 Unify slash-command registries across web UI, ZeroCode TUI, and channel runtime](https://github.com/zeroclaw-labs/zeroclaw/issues/7929) — opened 2026-06-18, needs-author-action, 7 comments.

Open PRs awaiting maintainer review:

- [#9002 fix(gateway): keep agent turns alive after viewer disconnect](https://github.com/zeroclaw-labs/zeroclaw/pull/9002) — p1, risk:high, open since 2026-07-11.
- [#8955 fix(telegram): batch media group attachments](https://github.com/zeroclaw-labs/zeroclaw/pull/8955) — size:XL, open since 2026-07-10.
- [#9424 fix(runtime): reject semantic-empty terminal completions](https://github.com/zeroclaw-labs/zeroclaw/pull/9424) — p1, size:XL, status:in-progress, open since 2026-07-27.

The maintainer decision queue tracker [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) is the canonical place to watch for resolution of the RFC-level items.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/Neare-Design/agents-radar).*