# Models and harness (Q15): options

*Discussion draft by Claude, 2026-10-07. Nothing here is decided (D17). Facts were checked on 2026-10-07 against the sources linked; vendor plans and limits change often, so re-check before any run.*

## 1. What the evaluation design needs from the harness

From `benchmark_design.md` (D23):

1. **Multi-hour autonomous episodes** in a sandbox with open-source engineering tools installed, and no internet access beyond the model API.
2. **Fresh instances:** no memory, project instructions or history from development sessions.
3. **Resource accounting:** inference tokens or cost, solver calls, investigation spend, wall-clock time, and checkpoint submissions with timestamps, so the completion-vs-budget surface can be computed.
4. **Repeats and best-of-k** (parallelism).
5. **Reproducibility:** pinned agent versions, recorded model identifiers and settings, full transcripts.
6. **Fair comparison** across vendors, or at least a clearly stated basis for it.
7. **Minimal cost** (D15): preferably existing Claude and ChatGPT subscriptions, frontier models only.

## 2. Facts checked (2026-10-07)

- **Claude Code with a Pro or Max plan.**
  - Interactive and headless (`claude -p`) use both draw on the subscription's usage limits: a five-hour session limit plus a weekly limit, with exact token numbers not published.
  - Anthropic announced in May 2026 that headless and Agent SDK use would move to a separate monthly credit from 15 June 2026. It then **paused that change on 15 June**, saying it will give notice before any revised plan takes effect ([Anthropic help centre](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)).
  - **This is a policy risk for a subscription-based pilot.**
- **Claude Code headless features** ([docs](https://code.claude.com/docs/en/headless)):
  - `--output-format json` or `stream-json` reports usage and an estimated `total_cost_usd`;
  - permission modes and allowed/denied tool lists;
  - SIGINT or SIGTERM handling for time caps.
  - The recommended reproducible mode, `--bare`, skips CLAUDE.md, memory, hooks and plugins, but **does not use a subscription login** (it needs an API key). With a subscription, isolation must instead come from a clean container with a fresh configuration directory.
- **Codex CLI with a ChatGPT plan** ([docs](https://learn.chatgpt.com/docs/non-interactive-mode)):
  - `codex exec --json` streams JSONL events, including per-turn token usage (input, cached, output, reasoning).
  - In containers, ChatGPT-account authentication works by seeding `~/.codex/auth.json`. It is documented as an advanced option for trusted runners, and the file must be treated like a password.
  - `--ignore-user-config` and `--sandbox` modes help isolation.
  - Usage limits run on rolling five-hour windows; since April 2026, Codex credits are token-based and aligned with API prices ([summary](https://www.developersdigest.tech/blog/codex-usage-limits-pricing-2026)).
- **Inspect + inspect_swe** ([docs](https://meridianlabs-ai.github.io/inspect_swe/)): runs Claude Code, Codex CLI and other agents inside Inspect sandboxes with full logging and token and time limits. Model calls are proxied through Inspect's model providers, which means **API keys (pay per token), not subscriptions**.
- **Models available now:**
  - Anthropic: Claude Fable 5.1 and Claude Opus 5.5.
  - OpenAI: GPT-5.6 Sol (July 2026), and GPT-6 Astra (September 2026), available in Codex for paid plans according to press reports ([e.g.](https://www.notebookcheck.net/GPT-6-Astra-is-on-ChatGPT-Plus-but-only-in-Work-and-Codex.1391574.0.html)).
  - Which models each CLI offers on Guglielmo's plans must be checked in the tools themselves.
- **Terms and data.**
  - Claude Code is an intended channel for programmatic use under Anthropic's consumer terms, and Codex supports ChatGPT sign-in for automation. Guglielmo should still read both vendors' current terms before running a published benchmark on consumer plans (Claude is not a lawyer).
  - Consumer accounts may use sessions for model training if opted in. **Turn training off in both accounts**, because private task content will pass through them.

## 3. Options

| | **H1 — Each vendor's own agent, on subscriptions** | **H2 — Common harness via APIs** | **H3 — Hybrid** |
|---|---|---|---|
| How | Claude Code (Claude models) and Codex CLI (OpenAI models) run headless inside our containers, signed in with Guglielmo's plans | Inspect with one generic agent loop, or with inspect_swe running the vendor agents, using pay-per-token API keys | H1 for the pilot; a small H2 subset later to measure the harness effect |
| Cost | Flat (existing subscriptions) | API cost: multi-hour frontier episodes could cost tens of US$ each; ~140 episodes could reach the low thousands | Flat now; small API budget later |
| Elicitation | Strong: each model in the harness its vendor tuned it for | A generic loop may under-elicit; inspect_swe keeps vendor agents | — |
| Comparability | Results compare *systems* (model plus native agent), not bare models | Same harness for all models | Both, eventually |
| Control and logging | Good through JSON logs plus our environment's own logs; token budgets **measured, not enforced** | Full; token budgets enforceable | — |
| Throughput | Bounded by subscription rate limits (five-hour and weekly windows) | Bounded by API rate limits and budget | — |
| Policy risk | Anthropic's paused change could return; plans and limits change often | Low | Fallback built in |

## 4. Recommendation [proposal]

**H1 for the pilot, with all measurement that matters done inside our environment, keeping H2 as the fallback.**

- **Put measurement in the environment, not the harness.** Our tool wrappers log and enforce:
  - solver calls;
  - investigation spend;
  - checkpoint submissions (timestamped);
  - the wall-clock cap.

  Only token usage comes from the vendor logs (Claude's `stream-json`, Codex's per-turn JSONL). Aligning cumulative tokens with checkpoint timestamps gives completion-versus-tokens curves after the fact. Switching harness later then changes only the token source.
- **Report results as model-plus-harness systems.** Disclose CLI versions, model identifiers, reasoning-effort settings and limits. This is honest, and it is how AstroAgentBench reported its results.
- **Convert token usage to API list-price equivalents** for the budget axis, so results stay meaningful to API users even though we pay flat subscriptions.

## 5. Run protocol sketch (for implementation, once agreed)

1. One fresh container per episode, from a pinned image (tools, Python, agent CLI at a pinned version).
2. A fresh agent configuration directory containing only credentials. No CLAUDE.md, memory, skills or plugins from development; task instructions arrive in the prompt and the work-package README.
3. Network egress allowed only to the vendor's model API. Web search and fetch tools disabled.
4. Identical task prompt and work package for every system; the highest available reasoning effort, recorded.
5. Wall-clock cap enforced by the episode runner (graceful SIGINT, then SIGTERM); checkpoints and logs collected from the container.
6. Hidden material (truth models, verifiers, withheld conditions) never mounted into the container; the verifier runs afterwards in a separate container.

**Throughput on the current Mac** (M2 Pro, 10 cores, 16 GB RAM): realistically 1–2 concurrent episodes, which also respects subscription rate limits. A pilot of about 140 episodes of up to a few hours each would take several weeks of background running. The pathfinder's trials will calibrate this.

## 6. Questions for Guglielmo

1. Which plans do you have (Claude Pro, Max 5x or Max 20x; ChatGPT Plus or Pro)?
2. Which two systems do you want in the pilot? Proposal: Claude Code with the strongest Claude model your plan allows (Fable 5.1 or Opus 5.5), and Codex CLI with GPT-6 Astra (or GPT-5.6 Sol).
3. Do you accept the "model-plus-native-agent system" framing, with token budgets measured rather than enforced?
4. Are you OK running episodes on this Mac with Docker? A cheap cloud VM could be considered later if throughput binds.
5. Should a small API budget be kept in reserve as a fallback, in case subscription policies change mid-pilot?
