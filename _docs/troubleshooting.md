# Troubleshooting

## "Model not found" o "It may not exist or you may not have access to it"

Causa más común: nombre del modelo no coincide con el catálogo del proveedor. Verifica:

1. El valor exacto de `ANTHROPIC_MODEL` en tu `.claude/settings.json`.
2. Que el proveedor todavía expone ese modelo (algunos se renombran o se descontinúan rápido — Z.ai migró `glm-4.5` → `glm-4.7` el 2025-09-30, MiMo retiró `mimo-v2-flash`, etc.).
3. Que el endpoint `BASE_URL` sea el correcto (Anthropic-compat, no OpenAI-compat).

Test rápido con `curl`:
```bash
curl "$ANTHROPIC_BASE_URL/v1/messages" \
  -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model": "<el-modelo>", "max_tokens": 50, "messages": [{"role": "user", "content": "ping"}]}'
```

Si `404` → `BASE_URL` mal. Si `400 model not found` → nombre del modelo mal. Si `401` → key mal o variable equivocada (`ANTHROPIC_API_KEY` vs `ANTHROPIC_AUTH_TOKEN`).

## Variables del shell sobreescriben `settings.json`

`ANTHROPIC_*` exportadas en `~/.zshrc`, `~/.bashrc`, `~/.envrc` (direnv) o lanzadas al iniciar la terminal **toman prioridad** sobre `settings.json`. La docs de MiniMax y Xiaomi avisan de esto explícitamente.

Verifica:
```bash
env | grep ANTHROPIC
```

Si ves variables que no quieres, antes de lanzar Claude Code:
```bash
unset ANTHROPIC_AUTH_TOKEN ANTHROPIC_BASE_URL ANTHROPIC_API_KEY ANTHROPIC_MODEL
# o quítalas permanentemente de ~/.zshrc
```

## `ANTHROPIC_API_KEY` vs `ANTHROPIC_AUTH_TOKEN`

- **`ANTHROPIC_API_KEY`** → para keys reales de Anthropic (`sk-ant-api03-...`) cuando facturas pay-as-you-go directo a Anthropic.
- **`ANTHROPIC_AUTH_TOKEN`** → para todos los proveedores third-party (Xiaomi, Z.ai, Moonshot, MiniMax, etc.) cuando usan endpoint Anthropic-compat con su propia key.

Usar la equivocada da `401 Unauthorized` aunque la key esté bien.

## Onboarding repetido al abrir Claude Code en otra carpeta

Claude Code guarda el flag `hasCompletedOnboarding` en `~/.claude.json`. Si entras a una nueva carpeta y te pide hacer el onboarding de nuevo, edita ese archivo:

```json
{
  "hasCompletedOnboarding": true
}
```

## OAuth de Pro/Max no funciona en proveedores third-party

Desde abril 2026, los tokens OAuth `sk-ant-oat01-...` que se generan vía `claude /login` (Pro/Max) están **bloqueados** para uso fuera de:
- Claude Code CLI oficial
- claude.ai
- Claude Desktop

Si necesitas Anthropic en otro IDE/herramienta, paga API key separada (pay-as-you-go) en `console.anthropic.com`.

## settings.local.json en git

Verifica que `.gitignore` de tu provider folder contiene:
```
.claude/settings.local.json
```

Si por error commiteaste una key real, **rota inmediatamente** la key en el dashboard del proveedor. Las keys filtradas a GitHub son scrappeadas en minutos.

## /status muestra "Disconnected"

- Network: `curl -I $ANTHROPIC_BASE_URL` debe devolver algo (200/404 está OK; "Could not resolve host" no).
- Firewall corporativo / VPN: algunos proveedores chinos están bloqueados desde redes corporativas. Prueba desde otra red.
- Reloj del sistema: TLS falla con clock drift > 5 min. Verifica con `date`.

## Debug detallado

```bash
claude --debug 2>&1 | tee /tmp/claude-debug.log
```

Te muestra requests/responses HTTP. Útil para ver exactamente qué endpoint y modelo se están llamando.

## /status reporta el modelo equivocado

Claude Code re-mapea internamente:
- "Sonnet" → `ANTHROPIC_DEFAULT_SONNET_MODEL` (o `ANTHROPIC_MODEL` si no está)
- "Opus" → `ANTHROPIC_DEFAULT_OPUS_MODEL`
- "Haiku" → `ANTHROPIC_DEFAULT_HAIKU_MODEL` (también acepta el nombre legacy `ANTHROPIC_SMALL_FAST_MODEL`)

Si tu proveedor solo tiene un modelo (típico en Coding Plans), apunta los tres `ANTHROPIC_DEFAULT_*` al mismo. Para tener un "small/fast" distinto (ahorra créditos), usa un modelo menor en `_HAIKU_`.

## El modelo responde pero `tool use` falla

Modelos open-source o de proveedores con tool-use limitado pueden:
- Ignorar las tool definitions
- Devolver "tool_use" malformado
- Llamarse en loop infinito

Prueba: con un modelo flagship del proveedor (en vez del fast/cheap). Si sigue fallando, ese proveedor/modelo no soporta bien Claude Code → cambia.

## Latencias altas / timeouts

```json
{
  "env": {
    "API_TIMEOUT_MS": "3000000"
  }
}
```

Algunas docs (Z.ai) lo recomiendan explícitamente. 3000000 ms = 50 min. Útil para reasoning largo (DeepSeek R1, qwen3-max) o uso de tools complejas.
