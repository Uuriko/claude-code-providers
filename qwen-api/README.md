# Qwen · DashScope API pay-as-you-go

DashScope estándar (NO Coding Plan), pago por tokens contra créditos prepago.

- **Base URL**: `https://dashscope-intl.aliyuncs.com/api/v2/apps/claude-code-proxy` (internacional). Para China: `https://dashscope.aliyuncs.com/api/v2/apps/claude-code-proxy`. Verifica la URL exacta en docs porque DashScope publica varios "apps" para Anthropic-compat según versión.
- **API key**: `sk-...` desde Model Studio console
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://www.alibabacloud.com/help/en/model-studio/anthropic-api-messages>

## Modelos

`qwen3-coder-plus`, `qwen3-coder-flash`, `qwen3-max`, `qwen3-coder-next`.

## Cuándo usar

- Para uso casual y/o cuando no quieres comprometerte con un Coding Plan mensual.
- Para producción intensiva, evalúa `qwen-coding/` (mejor relación tokens/USD).

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

> **TODO**: confirmar `BASE_URL` exacta porque Alibaba publica múltiples paths para Anthropic-compat (`/apps/anthropic`, `/apps/claude-code-proxy`). Si esta no funciona, prueba `https://dashscope-intl.aliyuncs.com/api/v2/apps/anthropic` o consulta los docs.
