# MiniMax · M2.7 (Token Plan / API)

MiniMax ofrece su flagship M2.7 con endpoint Anthropic-compat nativo.

- **Base URL**: `https://api.minimax.io/anthropic` (internacional) — para China: `https://api.minimaxi.com/anthropic`
- **API key**: desde [MiniMax Developer Platform](https://platform.minimax.io/user-center/payment/token-plan)
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://platform.minimax.io/docs/guides/text-ai-coding-tools>

## Planes

| Plan | USD/mes |
|---|---|
| Agent Pro | $19 |
| Coding Plus | $20 |
| Coding Max | $50 |
| Pay-as-you-go | $0 mínimo |

Mismo endpoint funciona para suscripción y pay-as-you-go — la diferencia es la cuenta y la key.

## Modelos

- `MiniMax-M2.7` — flagship reasoning + coding
- `MiniMax-M2.7-highspeed` — variante rápida
- (También M2.5, M2.1 disponibles para compatibilidad)

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
# /status debe mostrar BASE_URL=api.minimax.io/anthropic y modelo MiniMax-M2.7
```

## Aviso (de docs oficial)

Antes de iniciar, ejecuta:
```bash
unset ANTHROPIC_AUTH_TOKEN
unset ANTHROPIC_BASE_URL
```
Si exportas estas vars en `~/.zshrc`/`~/.bashrc`, MiniMax avisa explícitamente que las env vars del shell tienen prioridad sobre `settings.json`.
