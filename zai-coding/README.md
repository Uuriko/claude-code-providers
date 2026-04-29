# Z.ai · GLM Coding Plan (suscripción)

Z.ai (Zhipu/BigModel) ofrece el **GLM Coding Plan** específicamente para herramientas tipo Claude Code, con 3× el uso al precio del API estándar.

- **Base URL**: `https://api.z.ai/api/anthropic`
- **API key**: empieza con `sk-` — créala en <https://z.ai/manage-apikey/apikey-list>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Suscripción**: <https://z.ai/subscribe>
- **Docs oficial**: <https://docs.z.ai/scenario-example/develop-tools/claude>

## Planes

| Plan | USD/mes (aprox) | Notas |
|---|---|---|
| Lite | ~$10 | uso básico, ideal probar |
| Pro | ~$30 | uso intensivo |
| Max | ~$80 | proyectos grandes |

## Modelos

`glm-4.7` (default actual, upgraded desde `glm-4.5` el 2025-09-30), `glm-4.5-air` (rápido), `glm-5-turbo`, `glm-5.1`.

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
# Pega tu key sk-...
claude
```

## Nota

`API_TIMEOUT_MS=3000000` se incluye porque GLM puede tardar en respuestas largas; ese valor lo recomienda la documentación oficial.
