# Dasha Compute · red de Macs Apple-silicon (alpha)

[Dasha Compute](https://www.getdasha.com/compute) es una red de Macs reales que sirven modelos locales detrás de una API OpenAI-compatible. No expone endpoint Anthropic nativo: se intercala LiteLLM como proxy traductor (ver `_docs/proxies.md`).

- **Base URL (OpenAI-compat)**: `https://www.getdasha.com/compute/api/v1`
- **Guest key (24h, sin registro)**: `curl -X POST https://www.getdasha.com/compute/api/guest-keys` → el campo `api_key` de la respuesta (`dgk_...`)
- **Modelos**: la red sirve un conjunto pequeño y cambiante (al escribir esto: `qwen3-4b`, `qwen3-8b`, `gemma3-12b`, `gemma3-27b`); la lista viva está en `GET /compute/api/v1/models`
- **Estado**: alpha. La disponibilidad varía con los Macs conectados; no hay SLA.
- **Docs**: <https://www.getdasha.com/compute/llms.txt>

## Uso

```bash
pip install 'litellm[proxy]'
export DASHA_GUEST_KEY=dgk_...   # la key que devuelve /compute/api/guest-keys
litellm --config litellm-config.yaml --port 4000
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

El `model_name` de cada entrada del config de LiteLLM coincide con los valores `ANTHROPIC_MODEL` / `ANTHROPIC_DEFAULT_*` de `.claude/settings.json` (mapeo por tamaño: haiku → qwen3-4b, sonnet → qwen3-8b, opus → gemma3-27b). Cambia los nombres si la lista viva de modelos cambia.

## Costo

La guest key es gratuita (24h, chat + lista de modelos). Es una red comunitaria en alpha: sirve para probar el flujo, no para producción.
