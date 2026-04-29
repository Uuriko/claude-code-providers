# Moonshot · Kimi K2 (pay-as-you-go)

- **Base URL**: `https://api.moonshot.ai/anthropic` (internacional) — alternativa CN: `https://api.moonshot.cn/anthropic`
- **API key**: `sk-...` desde <https://platform.moonshot.ai/console/api-keys>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://platform.moonshot.ai/docs/guide/agent-support>

## Modelos

- `kimi-k2-turbo-preview` — flagship Kimi K2 con tool use, 256K contexto
- `kimi-k2-0711-preview` — versión más estable
- `kimi-k2.5-preview` — siguiente generación (verificar disponibilidad)

> Si tu cuenta está en `moonshot.cn` (China), cambia `ANTHROPIC_BASE_URL` a `https://api.moonshot.cn/anthropic`. Las cuentas son separadas (no se puede usar key CN contra endpoint .ai ni viceversa).

## Plan

Moonshot ofrece pay-as-you-go con créditos prepago. **No** tienen un "Coding Plan" separado tipo Z.ai/Xiaomi en este momento (verificar antes de pagar — el catálogo evoluciona).

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```
