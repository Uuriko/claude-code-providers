# Z.ai · BigModel API pay-as-you-go

Mismo endpoint Anthropic-compat que `zai-coding/` pero **sin Coding Plan**: facturación por tokens contra créditos en tu cuenta Z.ai/BigModel.

- **Base URL**: `https://api.z.ai/api/anthropic`
- **API key**: `sk-...` desde <https://z.ai/manage-apikey/apikey-list>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`

## Modelos

Mismos que el Coding Plan: `glm-4.7`, `glm-4.5-air`, `glm-5-turbo`, `glm-5.1`. La cuota la define tu balance prepago en BigModel.

## Cuándo usar

- Desarrollo casual o pruebas de un solo día
- Para producción intensa, usa `zai-coding/` (3× más barato).

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```
