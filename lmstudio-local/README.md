# LM Studio local (con proxy LiteLLM)

LM Studio es una alternativa GUI a Ollama: descarga GGUFs con un click, arranca un servidor HTTP OpenAI-compat (puerto 1234 por defecto), y sigue el mismo patrón que Ollama.

```
Claude Code  →  LiteLLM (:4000, Anthropic API)  →  LM Studio (:1234, OpenAI API)  →  GPU
```

- **Base URL** (Claude Code): `http://localhost:4000`
- **Auth**: dummy
- **Modelo alias**: `lmstudio-coder` (configurado en LiteLLM)

## Setup

```bash
# 1. Abre LM Studio → My Models → descarga un GGUF (ej. Qwen2.5-Coder-32B)
# 2. Tab "Server" → "Start Server" → confirma que escucha en http://localhost:1234

# 3. En la misma máquina, arranca LiteLLM:
pip install 'litellm[proxy]'
cat > litellm-config.yaml <<'YAML'
model_list:
  - model_name: lmstudio-coder
    litellm_params:
      model: openai/qwen2.5-coder-32b-instruct
      api_base: http://localhost:1234/v1
      api_key: lm-studio  # LM Studio acepta cualquier valor
YAML
litellm --config litellm-config.yaml --port 4000 &

# 4. Claude Code:
cd lmstudio-local/
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

> El `model_name` que pones en `litellm-config.yaml` y el `model:` apuntando al modelo cargado en LM Studio son cosas distintas: el primero es el alias que ve Claude Code (debe coincidir con `ANTHROPIC_MODEL` en `settings.json`), el segundo es el ID del modelo dentro de LM Studio (lo ves en la pestaña Server).

## Modelos GGUF recomendados

- `Qwen2.5-Coder-32B-Instruct-GGUF` (Q4_K_M, ~20GB)
- `Llama-3.3-70B-Instruct-GGUF` (Q4_K_M, ~40GB)
- `DeepSeek-Coder-V2-Lite-Instruct-GGUF`
- `Devstral-7B-GGUF`

## DGX Spark / Apple Silicon

LM Studio aprovecha bien Metal en Mac (Apple M2/M3/M4) y CUDA en GPUs NVIDIA. En el DGX Spark (ARM64) verifica que la build de LM Studio que descargas es ARM Linux.

## Cuándo elegir LM Studio vs Ollama

- LM Studio: GUI cómoda, mejor para experimentar/cambiar modelos, búsqueda integrada en HuggingFace.
- Ollama: CLI/headless, mejor para servidores remotos (DGX Spark sin pantalla), API más estable, modelos pre-empaquetados.

Ver [`../_docs/local-setup.md`](../_docs/local-setup.md) para más detalles y comparativas.
