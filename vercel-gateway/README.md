# Vercel AI Gateway

Vercel ofrece un gateway unificado que expone Anthropic, OpenAI, Google, Bedrock, etc. con una sola key. Tiene endpoint Anthropic-compat nativo.

- **Base URL**: `https://ai-gateway.vercel.sh/v1/anthropic`
- **API key**: empieza con `vck_...` — desde Vercel Dashboard → AI → Gateway
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://vercel.com/docs/ai-gateway/anthropic-api-compatibility>

## Costo

- Sin markup adicional sobre el precio de Anthropic.
- Hobby tier: $5 en créditos cada 30 días gratis.
- Vercel Pro ($20/mes) incluye más créditos y SLA.

## Modelos

Cuando usas el endpoint `/v1/anthropic`, los modelos se nombran con prefijo `anthropic/`:
- `anthropic/claude-opus-4-7`
- `anthropic/claude-sonnet-4-6`
- `anthropic/claude-haiku-4-5`

Otros providers (xAI, Google, OpenAI) están disponibles vía endpoints distintos del gateway.

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

## Cuándo usar

- Necesitas observabilidad/auditoría centralizada de tus llamadas a varios LLMs.
- Quieres un fallback automático (gateway puede rutear a otro modelo si uno falla).
- Eres usuario Vercel y quieres consolidar billing.
