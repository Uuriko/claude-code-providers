# Contributing

Thanks for considering a contribution. This repo is a community catalog of LLM providers compatible with [Claude Code](https://docs.claude.com/en/docs/claude-code). The goal is **accurate, copy-pasteable configurations** — not marketing.

## What we accept

- **New providers** that expose an Anthropic-compatible endpoint (`/v1/messages`) or that can be reached via a documented proxy (LiteLLM, claude-code-router, copilot-api, y-router, etc.).
- **Subscription plans** (token plans, coding plans) and pay-as-you-go APIs.
- **Cloud routes** (AWS Bedrock, Google Vertex, Azure Foundry) and local self-hosted setups (Ollama, LM Studio, vLLM, NVIDIA NIM, DGX Spark workflows).
- **Fixes** to model names, prices, base URLs, and broken links — providers rename and reprice constantly.
- **Translations** of provider READMEs (the catalog is currently in Spanish; English READMEs per provider are welcome as `README.en.md`).

## What we do NOT accept

- Affiliate / referral links or sponsored placements.
- Providers that require credit-card-only signup with no documented Anthropic-compat endpoint.
- Hand-wavy "this should work" entries — we want tested setups with a working `curl` example.
- Proxies or wrappers that violate a provider's Terms of Service without a clear warning to the user.

## Folder layout

Every provider lives in its own top-level folder:

```
<provider-slug>/
├── .claude/
│   ├── settings.json                  # shareable: BASE_URL, model defaults, env
│   ├── settings.local.json.example    # placeholder for the API key
│   └── settings.local.json            # YOUR key — gitignored
├── .gitignore                         # must include settings.local.json
└── README.md                          # specific instructions
```

The slug should be lowercase-kebab-case and match the provider's common name (`moonshot/`, `zai-coding/`, `aws-bedrock/`).

## Minimum README for a new provider

```markdown
# <Provider> · <Plan or API mode>

One-line description.

- **Base URL**: `https://...`
- **API key**: format `<prefix>-...` — get from <link>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN` or `ANTHROPIC_API_KEY` (be explicit)
- **Docs**: <official link>

## Plans / pricing
| Plan | USD/month or per-token | Quota | Notes |
|------|------------------------|-------|-------|

## Models
List of model IDs with brief notes (flagship, fast, deprecated).

## Usage
```bash
cp .claude/settings.local.json.example .claude/settings.local.json
$EDITOR .claude/settings.local.json
claude
```

## Notes / gotchas
Anything specific (regional endpoints, off-peak pricing, OAuth quirks, tool-use limitations).
```

## Verification checklist before opening a PR

- [ ] `curl` against the Anthropic-compat endpoint works (see [`_docs/troubleshooting.md`](_docs/troubleshooting.md) for the exact command).
- [ ] `.gitignore` excludes `.claude/settings.local.json`.
- [ ] No real API keys are committed.
- [ ] Prices, URLs, and model IDs are linked to **official documentation**, not blog posts.
- [ ] The provider entry is added to the comparison tables in the root `README.md`.
- [ ] If the model list is volatile (Chinese providers tend to rename quickly), add a "last verified" date in the README.

## Verifying a config quickly

```bash
curl "$ANTHROPIC_BASE_URL/v1/messages" \
  -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model": "<model-id>", "max_tokens": 50, "messages": [{"role": "user", "content": "ping"}]}'
```

A 200 with a valid `content` block means the provider is reachable. Anything else, see [`_docs/troubleshooting.md`](_docs/troubleshooting.md).

## Style

- Be **concise and technical**. Devs who land here want to copy-paste, not read marketing.
- Use **fenced code blocks** with language hints (`bash`, `json`, `yaml`).
- Prefer **tables** over prose for plan/pricing comparisons.
- Link to the **official docs** of the provider — never to scraped mirrors or third-party tutorials as primary sources.
- Don't invent numbers. If you can't verify a price or model ID, mark it `TODO: verify`.

## Reporting outdated info

Open an issue with:
- Provider folder
- What's outdated (URL, model name, price)
- Link to the official source with the new value

Or open a PR directly with the fix.

## License

By contributing you agree your contribution is licensed under the [MIT License](LICENSE).
