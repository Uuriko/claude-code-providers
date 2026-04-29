# NVIDIA NIM (containers locales con endpoint Anthropic-compat NATIVO)

NVIDIA Inference Microservices (NIM) son containers que sirven modelos LLM con OpenAI **y** Anthropic-compat nativos — no requieren proxy traductor.

- **Base URL**: `http://localhost:8000` (puerto por defecto del NIM)
- **Auth**: NIM no requiere key; setea `ANTHROPIC_AUTH_TOKEN=dummy` igualmente porque Claude Code lo exige.
- **Docs**: <https://docs.nvidia.com/nim/large-language-models/latest/ai-assistant-integrations/claude-code.html>

## Pre-requisitos

- GPU NVIDIA (H100/H200/B200/A100 enterprise; algunos NIMs corren en RTX 4090/5090 también).
- Docker + NVIDIA Container Toolkit.
- NGC API key: <https://ngc.nvidia.com/setup/api-key>

## Setup

```bash
# Login a NGC registry:
echo $NGC_API_KEY | docker login nvcr.io --username '$oauthtoken' --password-stdin

# Lanzar un NIM (ejemplo: Llama 3.3 70B Instruct):
docker run -it --rm --gpus all \
  --shm-size=16GB \
  -p 8000:8000 \
  -e NGC_API_KEY \
  -v ~/.cache/nim:/opt/nim/.cache \
  nvcr.io/nim/meta/llama-3.3-70b-instruct:latest

# En otra terminal:
cd nvidia-nim/
cp .claude/settings.local.json.example .claude/settings.local.json
# Edita ANTHROPIC_MODEL en .claude/settings.json para que coincida con el ID del NIM
claude
```

## Modelos relevantes para coding

NVIDIA mantiene NIMs para:
- `meta/llama-3.3-70b-instruct`
- `meta/llama-3.1-405b-instruct`
- `mistralai/mixtral-8x22b-instruct-v0.1`
- `nvidia/nemotron-4-340b-instruct`

Lista completa: <https://build.nvidia.com/models>

## DGX Spark

El DGX Spark (GB10 Grace Blackwell, ARM64, 128GB unified) **sí soporta NIMs ARM** publicados por NVIDIA. Verifica que el NIM tag específico tenga arquitectura `linux/arm64`:
```bash
docker manifest inspect nvcr.io/nim/meta/llama-3.3-70b-instruct:latest | grep architecture
```

Si solo hay `amd64`, usa Ollama + LiteLLM (carpeta `../ollama-local/`) en su lugar.

## Pros / contras

- ✅ Sin proxy traductor — endpoint Anthropic-compat nativo.
- ✅ Optimizaciones NVIDIA (TensorRT-LLM) → throughput y latencia mejor que Ollama puro.
- ❌ Requiere GPU NVIDIA enterprise para los modelos grandes.
- ❌ Imágenes pueden ser pesadas (~50-200GB la primera vez).
