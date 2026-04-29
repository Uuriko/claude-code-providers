# Ollama Cloud — suscripción mensual + Anthropic-compat NATIVO

Ollama Cloud es la oferta hosted de Ollama: corre modelos grandes (Kimi K2.5, GLM-5, MiniMax M2.7, Qwen3.5) en la infraestructura de Ollama bajo suscripción mensual fija. Como Ollama local, expone `/v1/messages` Anthropic-compat **directo, sin proxy**.

- **Base URL**: `https://ollama.com`
- **API key**: desde tu cuenta en <https://ollama.com>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://docs.ollama.com/cloud>

## Planes

Ollama Cloud ofrece tiers de suscripción (verifica precios actuales en <https://ollama.com/pricing> — han variado recientemente):

| Plan típico | USD/mes | Notas |
|---|---|---|
| Free / Hobby | $0 | Cuota muy limitada |
| Pro | ~$20 | Para uso personal moderado |
| Max | ~$100 | Heavy use, multiple models |

Suscripción equivalente conceptual a Anthropic Pro/Max pero con catálogo open-source.

## Modelos `:cloud` recomendados

- `kimi-k2.5:cloud` — Moonshot Kimi K2.5 (default, excelente para coding)
- `glm-5:cloud` — Zhipu GLM-5
- `minimax-m2.7:cloud` — MiniMax M2.7
- `qwen3.5:cloud` — Qwen 3.5
- `glm-4.7-flash` — versión rápida de GLM (haiku-style)

Lista completa: <https://ollama.com/search?c=cloud>

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
# Pega tu Ollama Cloud API key
claude
```

O usa el launcher de Ollama:

```bash
ollama launch claude --model kimi-k2.5:cloud
```

## Cuándo elegir Ollama Cloud vs alternativas

- **vs `ollama-local/`**: si no tienes hardware potente (DGX Spark / GPU 24GB+), Cloud te da los modelos grandes sin invertir.
- **vs `xiaomi/`/`zai-coding/`**: Cloud abstrae varios proveedores chinos bajo una sola key Ollama. Cómodo, pero suele ser más caro que ir directo al proveedor.
- **vs `anthropic-pro-max/`**: Cloud te da Kimi/GLM/MiniMax (modelos open-source), no los Claude originales.
