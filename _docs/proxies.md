# Proxies traductores (OpenAI ↔ Anthropic)

Algunos proveedores no exponen un endpoint Anthropic-compat nativo. Para usarlos con Claude Code se intercala un proxy local que traduce el formato. Aquí están los más usados.

## LiteLLM (Python, recomendado)

[BerriAI/litellm](https://github.com/BerriAI/litellm) — proxy multi-proveedor, soporta 100+ backends. Es la opción más completa y mantenida.

```bash
pip install 'litellm[proxy]'
# o, si prefieres uv:
uv tool install 'litellm[proxy]'
```

Config básica (`litellm-config.yaml`):

```yaml
model_list:
  - model_name: ollama-coder
    litellm_params:
      model: ollama_chat/qwen2.5-coder:32b
      api_base: http://localhost:11434

  - model_name: lmstudio-coder
    litellm_params:
      model: openai/local-model
      api_base: http://localhost:1234/v1
      api_key: lm-studio

  - model_name: copilot-claude
    litellm_params:
      model: github_copilot/claude-3-5-sonnet
      # Auth via OAuth GitHub Copilot (ver más abajo)
```

Arrancar:

```bash
litellm --config litellm-config.yaml --port 4000
```

Claude Code apunta a `http://localhost:4000` (con `ANTHROPIC_AUTH_TOKEN=dummy`). El nombre del modelo que pones en `ANTHROPIC_MODEL` debe coincidir con `model_name`.

LiteLLM **no es Anthropic-nativo por sí mismo**: traduce el formato cuando recibe peticiones en `/v1/messages` y las re-envía como `/v1/chat/completions` al backend OpenAI-compat.

## claude-code-router (Node, más liviano)

[musistudio/claude-code-router](https://github.com/musistudio/claude-code-router) — proxy especializado en Claude Code, config JSON, levanta el wrapper `ccr code` que arranca el proxy + Claude Code juntos.

```bash
npm install -g @musistudio/claude-code-router
```

Config (`~/.claude-code-router/config.json`):

```json
{
  "Providers": [
    {
      "name": "ollama",
      "api_base_url": "http://localhost:11434/v1/chat/completions",
      "models": ["qwen2.5-coder:32b"]
    }
  ],
  "Router": {
    "default": "ollama,qwen2.5-coder:32b",
    "background": "ollama,qwen2.5-coder:32b"
  }
}
```

Uso: `ccr code` (en lugar de `claude`).

Trade-off: más simple que LiteLLM pero menos flexible (no soporta auth complejo, batching, ni todos los providers).

## copilot-api (GitHub Copilot Pro como backend)

[ericc-ch/copilot-api](https://github.com/ericc-ch/copilot-api) — convierte tu suscripción **GitHub Copilot Pro ($20/mes)** en un endpoint Anthropic-compat local.

```bash
npx copilot-api@latest auth        # primera vez, OAuth con GitHub
npx copilot-api@latest start --port 4141 --claude-code
```

Claude Code config:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:4141",
    "ANTHROPIC_AUTH_TOKEN": "dummy",
    "ANTHROPIC_MODEL": "claude-sonnet-4.6"
  }
}
```

Modelos disponibles: los que tu plan Copilot incluya (Claude Sonnet/Opus, GPT-5, Gemini, etc.). Útil si ya pagas Copilot Pro.

> ⚠️ **Términos de uso**: GitHub permite Copilot vía herramientas oficiales. Usar copilot-api fuera del IDE puede violar TOS — revisa antes de uso intensivo. Funciona, pero úsalo bajo tu propio riesgo.

## anthropic-proxy / y-cli

Otros wrappers ligeros existentes — útiles si solo quieres exponer un único modelo OpenAI-compat como Anthropic-compat sin la complejidad de LiteLLM:

- [maxnowack/anthropic-proxy](https://github.com/maxnowack/anthropic-proxy)
- [mattlqx/claude-code-ollama-proxy](https://github.com/mattlqx/claude-code-ollama-proxy)

## ¿Cuál elegir?

| Caso | Recomendación |
|---|---|
| Múltiples backends, uso intensivo | LiteLLM |
| Solo Ollama o LM Studio, simple | claude-code-router |
| Tienes GitHub Copilot Pro | copilot-api |
| Necesitas observabilidad/logging | LiteLLM (con `success_callback`) |
| Solo experimentar, una sola vez | anthropic-proxy / y-cli |
