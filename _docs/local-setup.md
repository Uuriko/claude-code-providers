# Setup local: Ollama / LM Studio / vLLM en DGX Spark u otra GPU

Esta guía documenta cómo correr modelos open-source localmente y conectar Claude Code via proxy. Aplica a:

- NVIDIA DGX Spark (GB10 Grace Blackwell, 128GB unified, ARM64)
- GPUs NVIDIA estándar (RTX 4090, 5090, A100, H100, etc.)
- Apple Silicon (M2/M3/M4 con Metal — solo Ollama y LM Studio)

## Comparativa de runtimes

| Runtime | Anthropic-compat | Pros | Contras | Mejor para |
|---|---|---|---|---|
| **Ollama** (0.11+) | **NATIVO** | CLI simple, modelos pre-empaquetados, headless, sin proxy | Sin GUI | Servidores remotos, DGX Spark, default recomendado |
| **LM Studio** | vía proxy (LiteLLM) | GUI, búsqueda integrada de GGUFs, fácil | Más pesado, requiere LiteLLM | Workstation local, experimentación |
| **vLLM** | vía proxy (LiteLLM) | Alto throughput, paged-attention | Más complejo, requiere Python | Producción interna, multi-tenant |
| **NVIDIA NIM** | **NATIVO** | Optimizado TensorRT-LLM, sin proxy | Requiere GPU NVIDIA enterprise, NGC key | Hardware NVIDIA pro |

**Recomendación**: si solo quieres correr modelos locales con Claude Code, usa **Ollama** — desde la versión 0.11 expone `/v1/messages` Anthropic-compat directo en `:11434`, sin proxy.

## Ollama — quick start

```bash
# Instalación
curl -fsSL https://ollama.com/install.sh | sh

# Arrancar (queda como servicio):
ollama serve &

# Descargar modelos para coding (elige según VRAM):
ollama pull qwen2.5-coder:32b              # ~20 GB Q4 — entra en cualquier GPU 24GB+
ollama pull qwen3-coder:480b               # ~320 GB Q4 — solo DGX Spark / multi-GPU
ollama pull deepseek-coder-v2:236b         # ~150 GB Q4 — DGX Spark
ollama pull llama3.3:70b-instruct          # ~40 GB Q4 — GPU 48GB+ o DGX Spark
ollama pull glm-4.5-air                    # ~20 GB
ollama pull devstral:7b                    # ~5 GB — laptops

# Verifica:
curl http://localhost:11434/v1/models
```

## LM Studio — quick start

1. Descarga e instala desde <https://lmstudio.ai>.
2. **My Models** → busca "Qwen2.5-Coder" → descarga la variante GGUF Q4_K_M.
3. **Server** tab → toggle **Start Server** → confirma `http://localhost:1234`.
4. Carga el modelo desde el dropdown.

LM Studio expone OpenAI-compat. Cualquier client OpenAI puede llamarlo apuntando a `http://localhost:1234/v1`. Para Claude Code se necesita LiteLLM (ver `proxies.md`).

## vLLM — para producción

```bash
pip install vllm
python -m vllm.entrypoints.openai.api_server \
  --model Qwen/Qwen2.5-Coder-32B-Instruct \
  --dtype float16 \
  --gpu-memory-utilization 0.9 \
  --port 8001
```

Como Ollama y LM Studio, expone OpenAI-compat → necesita LiteLLM en frente para Claude Code.

## NVIDIA NIM — sin proxy

Ver carpeta `../nvidia-nim/` con instrucciones específicas. NIM es la única opción que **no requiere proxy** porque expone Anthropic-compat nativo.

## DGX Spark: notas específicas

**Hardware**: GB10 Grace Blackwell, 20 ARM CPU cores, 128 GB LPDDR5X unified, 4 TB SSD, ~1 PFLOP FP4 sparse, NVLink C2C entre CPU y GPU.

**Memoria unificada**: a diferencia de las GPUs discretas, CPU y GPU comparten RAM. Esto permite cargar modelos grandes (Qwen3-Coder 480B Q4 ~320GB **no cabe** sin offload; pero modelos ~70-128B caben holgados).

**ARM64**: verifica que las imágenes Docker / runtimes que descargas son `linux/arm64`:
```bash
docker manifest inspect <imagen> | grep architecture
file $(which ollama)   # debería decir aarch64
```

**Setup recomendado para Spark**:
1. Ollama (nativo ARM, ya soportado).
2. Tira `ollama pull` con modelos que entren en 128GB (recomendados: `qwen2.5-coder:32b`, `llama3.3:70b`, `deepseek-coder-v2:236b` con offload limitado).
3. LiteLLM en la misma máquina (puerto 4000).
4. Desde tu laptop u otra máquina cliente: apunta `ANTHROPIC_BASE_URL=http://<spark-ip>:4000` en el `settings.json` de la carpeta `ollama-local/`.

## Modelos top para coding (rankeo por capacidad)

| Modelo | Aider Polyglot | Tamaño Q4 | Comando |
|---|---|---|---|
| Qwen3-Coder-480B-A35B | 61.8% | ~320 GB | `ollama pull qwen3-coder:480b` |
| Qwen2.5-Coder-32B | 57.2% | ~20 GB | `ollama pull qwen2.5-coder:32b` |
| DeepSeek-V2-236B | 55.8% | ~150 GB | `ollama pull deepseek-v2` |
| Llama-3.3-70B | 54.1% | ~40 GB | `ollama pull llama3.3:70b` |
| GLM-4.5-Air | ~52% | ~20 GB | `ollama pull glm-4.5-air` |
| Devstral-7B | 45.2% | ~5 GB | `ollama pull devstral:7b` |

## Conectar Claude Code

### Ollama (nativo, sin proxy)

```bash
# 1. Asegúrate de tener Ollama 0.11+
ollama --version

# 2. Lanza:
ollama serve &
ollama pull qwen3-coder

# 3. Verifica el endpoint Anthropic-compat:
curl http://localhost:11434/v1/messages \
  -H "x-api-key: ollama" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model": "qwen3-coder", "max_tokens": 100, "messages": [{"role": "user", "content": "ping"}]}'

# 4. Claude Code:
cd ../ollama-local/
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

### LM Studio / vLLM (vía LiteLLM)

```bash
pip install 'litellm[proxy]'

# Ejemplo LM Studio (server arrancado en :1234):
litellm --model openai/qwen2.5-coder-32b \
        --api_base http://localhost:1234/v1 \
        --port 4000 &

# Claude Code apunta a :4000 (ver lmstudio-local/.claude/settings.json)
cd ../lmstudio-local/
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

## Troubleshooting

- **"Connection refused"**: ¿está corriendo Ollama? `curl http://localhost:11434/api/tags`.
- **Tools no funcionan**: muchos modelos open-source tienen tool-use limitado. Prueba con `qwen2.5-coder` o `llama3.3` que tienen mejor cobertura.
- **DGX Spark muy lento al primer prompt**: el modelo se carga en VRAM al primer call; los siguientes son rápidos. Considera precargar con `ollama run <modelo> ""`.
