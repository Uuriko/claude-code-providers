# Xiaomi MiMo · API pay-as-you-go

Mismo proveedor que `xiaomi/`, pero con **facturación por tokens** en vez de suscripción mensual. Útil para uso ligero o pruebas.

- **Base URL**: `https://api.xiaomimimo.com/anthropic`
- **API key**: empieza con `sk-` — se obtiene en [API Keys console](https://platform.xiaomimimo.com/#/console/api-keys)
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs oficial**: <https://platform.xiaomimimo.com/docs/en-US/integration/claudecode>

## Modelos

Comparte catálogo con el Token Plan: `mimo-v2.5-pro`, `mimo-v2.5`, `mimo-v2-pro`, `mimo-v2-omni`, TTS.

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
# Edita y pega tu key sk-...
claude
```
