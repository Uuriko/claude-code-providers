# Internal SEO notes — keywords & positioning

> Working document for the repo owner. Not meant for end users. Update as algorithms and provider names shift.

## Primary keywords (head terms, English)

These are the queries we want to rank for. Confirmed via WebSearch April 2026.

| Keyword | Intent | Notes |
|---|---|---|
| claude code alternatives | Informational + Commercial | High volume, very competitive (blogs, Cursor/Cline content). Our angle: **provider catalog, not tool comparison**. |
| claude code anthropic compatible | Transactional | Lower volume, very high intent. Devs already know they want `ANTHROPIC_BASE_URL`. |
| claude code providers | Navigational | Direct match for our repo name. |
| claude code with deepseek / kimi / glm / qwen | Transactional | Long-tail, one per provider. |
| anthropic compatible api | Informational | Devs evaluating proxies. |
| claude code subscription | Commercial | Compares Pro/Max with $6-30/mo Chinese plans. |

## Long-tail (lower competition, real traffic)

- "how to use claude code with kimi k2"
- "how to use claude code with glm 5.1"
- "claude code deepseek setup"
- "claude code openrouter base url"
- "claude code ollama local model"
- "claude code dgx spark"
- "minimax m2 claude code"
- "xiaomi mimo claude code"
- "claude code china model api"
- "claude code without anthropic subscription"
- "claude code cheaper alternative"
- "ANTHROPIC_AUTH_TOKEN vs ANTHROPIC_API_KEY"

## Spanish long-tail (owner is hispanohablante, room for low-competition wins)

- "claude code alternativas"
- "claude code modelos chinos"
- "claude code sin suscripción anthropic"
- "claude code con kimi"
- "claude code con deepseek"
- "claude code barato"

## Differentiation vs. similar repos

| Repo | Their angle | Our angle |
|---|---|---|
| `Alorse/cc-compatible-models` (28★) | Single README guide, pricing focus | **Per-provider folder = ready-to-use workspace**. `cd xiaomi/ && claude` and you're in. |
| `musistudio/claude-code-router` | Proxy software | We catalog providers, we don't ship code. We *recommend* their router. |
| `hesreallyhim/awesome-claude-code` | Skills / hooks / commands | We're focused on **model backends**, not Claude Code extensions. |
| `ctala/ai-benchmarks-alternativos` (same owner) | Benchmarks of LLM models | Cross-link: this repo says "which provider", that repo says "which model is good". |
| `nielspeter/claude-code-proxy` etc. | Proxy implementations | We're the directory; they're tools. We link to them. |

## Search intent distribution

Roughly:
- 50% transactional ("setup X with claude code")
- 30% commercial investigation ("X vs Y", "is it worth")
- 15% informational ("what is anthropic compatible")
- 5% navigational ("github claude code providers")

Repo README should serve all four with: hero pitch (informational), comparison table (commercial), per-folder quick start (transactional), repo title/topics (navigational).

## E-E-A-T signals to maintain

- Author bio / GitHub profile linked.
- Cite **official provider docs** in every README (already doing this).
- Mark unverified info with `TODO`.
- Date-stamp model lists for Chinese providers (they rename fast — Z.ai migrated `glm-4.5` → `glm-4.7`, etc.).
- Link to working `curl` examples in `_docs/troubleshooting.md`.

## Repo name candidates (for slug optimization)

| Slug | Pros | Cons |
|---|---|---|
| `claude-code-providers` | Exact-match keyword, descriptive, no awesome-list expectations | A little dry |
| `claude-code-models` | Shorter, slightly broader | "Models" is ambiguous (could be benchmarks) |
| `awesome-claude-code-providers` | Plugs into `awesome-*` ecosystem, gets aggregated by awesome-bot indexers | Implies curated list of links, not workspace folders |
| `claude-code-anthropic-compat` | Most precise technically | Niche, less searchable |

**Recommendation**: `claude-code-providers` as primary (description: "Ready-to-use Claude Code workspaces for 19+ LLM providers — Anthropic, GLM, Kimi, DeepSeek, Qwen, Ollama, AWS Bedrock and more.").

## GitHub repo settings checklist

- [ ] Description (250 char limit) — first ~80 chars show in previews. Open with the keyword.
- [ ] Website URL → link to docs.claude.com or your blog post about the repo.
- [ ] Topics — 15-20 from `.github/topics.txt`.
- [ ] Social preview image (1280×640, optional) — table screenshot works well.
- [ ] License: MIT (already added).
- [ ] Pin in your profile.
- [ ] Add to `awesome-claude-code` lists via PR (jqueryscript, hesreallyhim, ccplugins).

## Suggested GitHub description (250 char hard limit)

> Ready-to-use Claude Code workspaces for 19+ LLM providers: Anthropic Pro/Max, GLM Coding Plan, Kimi K2, DeepSeek, Qwen, MiniMax M2, Xiaomi MiMo, OpenRouter, AWS Bedrock, Google Vertex, Ollama and DGX Spark local setups.

(247 chars.)

## Anti-patterns to avoid

- Don't keyword-stuff the README.
- Don't auto-translate every README to 6 languages — Google flags low-quality multilingual content.
- Don't add badges that don't actually validate (fake "build passing" badges hurt trust).
- Don't re-list the same comparison table 3 times in different sections (cannibalization).
- Don't add a "tip jar" or affiliate link — kills the neutrality positioning.

## Backlink ideas (legitimate, value-driven)

- Submit to `awesome-claude-code` lists (PR, not spam).
- Hacker News post: "Show HN: 19 ready-to-use Claude Code workspaces for non-Anthropic models". Best window: Tue-Thu 8-10 AM PT.
- Reddit r/LocalLLaMA: focus on the DGX Spark / Ollama angle.
- Reddit r/ClaudeAI: focus on the cost-saving angle.
- Dev.to article: "How I run Claude Code with 19 different LLM providers" — link back.
- Cross-link from `ctala/ai-benchmarks-alternativos`.
- HN: do NOT title with "I made". Lead with the use case.
