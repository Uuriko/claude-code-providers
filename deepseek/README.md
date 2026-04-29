# DeepSeek · API pay-as-you-go

DeepSeek expone endpoint Anthropic-compat nativo, pero **solo pay-as-you-go**: no tiene plan de suscripción mensual fijo equivalente a Xiaomi/Z.ai/Qwen.

- **Base URL**: `https://api.deepseek.com/anthropic`
- **API key**: `sk-...` desde <https://platform.deepseek.com/api_keys>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://api-docs.deepseek.com/guides/anthropic_api>

## Modelos

- `deepseek-chat` — modelo conversacional general (V3.2)
- `deepseek-reasoner` — modelo con reasoning explícito (R1-style)
- `deepseek-coder` — versión específica para código (verificar disponibilidad actual)

## Costo

DeepSeek tiene precios muy competitivos vs Anthropic (~10× más barato en input/output), pero al no haber plan mensual no hay descuento por volumen comprometido.

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```
