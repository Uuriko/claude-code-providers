# Xiaomi MiMo · Token Plan (suscripción)

Endpoint Anthropic-compat con suscripción mensual fija (créditos prepagados al mes).

- **Base URL**: `https://token-plan-sgp.xiaomimimo.com/anthropic` (Singapur) o `https://token-plan-cn.xiaomimimo.com/anthropic` (China)
- **API key**: empieza con `tp-` — se obtiene en [Subscription Console](https://platform.xiaomimimo.com/#/console/plan-manage)
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs oficial**: <https://platform.xiaomimimo.com/docs/en-US/integration/claudecode>

## Planes

| Plan | USD/mes | Créditos/mes |
|---|---|---|
| Lite | ~$6 | 60M |
| Standard | ~$16 | 200M |
| Pro | ~$50 | 700M |
| Max | ~$88 | unlimited tier |

Off-peak 16:00–24:00 UTC = consumo 0.8x.

## Modelos

`mimo-v2.5-pro` (flagship), `mimo-v2.5`, `mimo-v2-pro`, `mimo-v2-omni`, `mimo-v2.5-tts*` (TTS gratis tiempo limitado).

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
# Edita y pega tu key tp-...
env | grep ANTHROPIC   # verifica que NADA del shell colisione
claude
# Dentro de Claude: /status
```

Si quieres usar el endpoint China en vez de SGP, edita `ANTHROPIC_BASE_URL` en `.claude/settings.json`.
