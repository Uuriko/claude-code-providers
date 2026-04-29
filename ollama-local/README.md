# Ollama local — Anthropic-compat NATIVO

Ollama desde versiones recientes expone `/v1/messages` Anthropic-compatible **directamente**: no se necesita LiteLLM ni ningún proxy.

```
Claude Code  →  Ollama (:11434, /v1/messages NATIVO)  →  GPU
```

- **Base URL**: `http://localhost:11434` (sin `/v1`, Ollama lo maneja internamente)
- **Auth**: cualquier valor — Ollama lo ignora. Por convención: `ollama` o `dummy`.
- **Docs oficial**: <https://docs.ollama.com/api/anthropic-compatibility>
- **Guía Claude Code de Ollama**: <https://docs.ollama.com/integrations/claude-code>

## Setup

```bash
# 1. Instala Ollama (si no lo tienes):
curl -fsSL https://ollama.com/install.sh | sh

# 2. Lánzalo:
ollama serve &

# 3. Descarga un modelo de coding:
ollama pull qwen3-coder              # default en este settings.json
# alternativas:
ollama pull qwen2.5-coder:32b        # ~20 GB, GPU 24GB+
ollama pull llama3.3:70b             # ~40 GB
ollama pull deepseek-coder-v2:236b   # ~150 GB, requiere DGX Spark
ollama pull glm-4.5-air              # ~20 GB

# 4. Claude Code:
cd ollama-local/
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

Si tu modelo Ollama tiene otro nombre, edita `ANTHROPIC_MODEL` en `.claude/settings.json`.

## Atajo: `ollama launch claude`

Ollama también incluye un launcher que arranca Claude Code con la config correcta automáticamente:

```bash
ollama launch claude --model qwen3-coder
# o, sin pre-pull:
ollama launch claude --model kimi-k2.5:cloud --yes
```

Útil si solo quieres probar rápido sin tocar `settings.json`. Pero para tener un workspace persistente con tu key, esta carpeta es lo que necesitas.

## Ollama remoto (DGX Spark)

Si Ollama corre en otra máquina (ej. tu DGX Spark en LAN):

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://<spark-ip>:11434",
    "ANTHROPIC_MODEL": "qwen3-coder:480b"
  }
}
```

Asegúrate de que Ollama escuche en todas las IPs:
```bash
OLLAMA_HOST=0.0.0.0:11434 ollama serve
```

## Modelos top para coding (Aider Polyglot)

| Modelo | Tamaño Q4 | Score | Comando |
|---|---|---|---|
| `qwen3-coder:480b` | ~320 GB | 61.8% | `ollama pull qwen3-coder:480b` |
| `qwen2.5-coder:32b` | ~20 GB | 57.2% | `ollama pull qwen2.5-coder:32b` |
| `deepseek-v3` | ~400 GB | 58% | `ollama pull deepseek-v3` |
| `llama3.3:70b` | ~40 GB | 54.1% | `ollama pull llama3.3:70b` |
| `glm-4.5-air` | ~20 GB | 52% | `ollama pull glm-4.5-air` |

## Pros / contras

- ✅ Anthropic-compat nativo, sin proxy adicional.
- ✅ Coste fijo (electricidad), privacidad total.
- ✅ Modelos open-source de última generación (Qwen3-Coder 480B alcanza 61.8% Aider Polyglot).
- ❌ Hardware: necesitas GPU 24GB+ (modelos pequeños) o DGX Spark (modelos 70B+).
- ❌ Tool-use varía por modelo. Si un modelo open-source tiene problemas con tools de Claude Code, prueba `qwen3-coder` o `qwen2.5-coder` (los mejores en tool calling actualmente).

## Alternativa: Ollama Cloud

Si no tienes hardware potente pero te gusta el setup de Ollama, mira [`../ollama-cloud/`](../ollama-cloud/) — modelos `:cloud` corren en la infraestructura de Ollama bajo suscripción mensual, sin GPU local.

Más detalles de hardware/modelos: [`../_docs/local-setup.md`](../_docs/local-setup.md).
