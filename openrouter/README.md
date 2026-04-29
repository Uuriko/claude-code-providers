# OpenRouter

OpenRouter agrega 300+ modelos (Anthropic, xAI, OpenAI, Google, modelos abiertos) bajo una sola key. Funciona con Claude Code mediante un endpoint que respeta el formato Anthropic.

- **Base URL**: `https://openrouter.ai/api/v1`
- **API key**: `sk-or-v1-...` desde <https://openrouter.ai/keys>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs Claude Code**: <https://openrouter.ai/docs/community/claude-code>

## Modelos (ejemplos)

Los modelos se referencian con prefijo de proveedor:
- `anthropic/claude-sonnet-4.6`, `anthropic/claude-opus-4.7`, `anthropic/claude-haiku-4.5`
- `x-ai/grok-4-fast`, `google/gemini-2.5-pro`
- `qwen/qwen3-coder-480b-a35b`, `deepseek/deepseek-chat`

Lista completa: <https://openrouter.ai/models>

## Costo

OpenRouter cobra solo el precio del modelo subyacente + ~5% margen, contra créditos prepago. **No hay plan de suscripción mensual fijo** — paga por uso real.

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
# Pega tu key sk-or-v1-...
claude
```

## Notas

- Si quieres que Claude Code use un modelo específico (ej. Grok), edita `ANTHROPIC_MODEL` en `.claude/settings.json` con el ID exacto de OpenRouter.
- Algunos modelos requieren "BYOK" (bring your own key). Para evitarlo, filtra por modelos sin prefijo "BYOK" en la página de OpenRouter.
- Cuando llamas modelos no-Anthropic vía endpoint Anthropic, OpenRouter traduce el formato. La compatibilidad con tools/streaming es buena pero no perfecta — si encuentras bugs, prueba el modelo nativo de Anthropic primero.
