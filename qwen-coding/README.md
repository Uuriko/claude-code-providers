# Qwen · Coding Plan (Alibaba DashScope)

Plan de suscripción específico para coding con modelos Qwen3. Endpoint Anthropic-compat dedicado distinto al DashScope general.

- **Base URL**: `https://coding-intl.dashscope.aliyuncs.com/apps/anthropic` (internacional)
- **API key**: empieza con `sk-sp-` (Service Plan key) — desde el Coding Plan dashboard
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://www.alibabacloud.com/help/en/model-studio/coding-plan>

## Modelos

- `qwen3-coder-plus` — flagship coding, default
- `qwen3-coder-next` — variante más rápida
- `qwen3-max-2026-01-23` — máxima capacidad para razonamiento
- `qwen3-coder-flash` — más económico

## Plan

Suscripción Alibaba Coding Plan (precio variable según región y nivel). Las cuentas China e Internacional son separadas — verifica desde dónde te suscribes.

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```
