# Claude Code Providers · Catálogo de configuraciones Anthropic-compatibles

> Workspaces listos para usar con [Claude Code](https://docs.claude.com/en/docs/claude-code) en 20+ proveedores LLM, con todas las modalidades: **suscripciones mensuales** (Anthropic Pro/Max, GLM Coding Plan, Xiaomi MiMo Token Plan, MiniMax, Ollama Cloud), **APIs pay-as-you-go** (DeepSeek, Kimi, Qwen, OpenRouter), **cloud enterprise** (AWS Bedrock, Google Vertex, Azure Foundry) y **self-hosted local** (Ollama, LM Studio, NVIDIA NIM, DGX Spark).

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-7c3aed)](https://docs.claude.com/en/docs/claude-code)
[![Providers](https://img.shields.io/badge/providers-20+-success)](#tabla-comparativa)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](CONTRIBUTING.md)

Cada subcarpeta de este repositorio es un **workspace Claude Code listo**: clonas, entras a la carpeta del proveedor, copias la plantilla con tu API key, y `claude` arranca apuntando al endpoint correcto. Sin proxies en cadena, sin variables de entorno globales, sin sobrescribir tu configuración.

---

## ¿Para quién es este repo?

- **Devs que usan Claude Code y quieren bajar el coste** desde $200/mes (Anthropic Max) a $6-30/mes con planes de proveedores chinos como Xiaomi MiMo, Z.ai GLM, MiniMax M2.7 o Qwen Coding Plan.
- **Equipos que ya pagan Anthropic Pro/Max** pero necesitan un fallback cuando se agota la cuota — pagan pay-as-you-go en DeepSeek, Moonshot Kimi, OpenRouter sin cambiar de herramienta.
- **Empresas con cuentas AWS / GCP / Azure** que quieren consumir Claude (Bedrock, Vertex) o modelos third-party (Foundry) bajo su IAM y facturación corporativa.
- **Owners de hardware potente** (DGX Spark, GPUs 24GB+, Apple Silicon) que quieren correr Qwen3-Coder, DeepSeek-V3, Llama 3.3 o GLM-4.5-Air **localmente** y conectarlos a Claude Code vía LiteLLM o NVIDIA NIM.
- **Hispanohablantes** que prefieren documentación técnica en español (los READMEs específicos también incluyen referencias a docs oficiales en inglés).

Si solo te interesa el ranking de modelos por benchmark (no la configuración del proveedor), ver el repo hermano: [`ctala/ai-benchmarks-alternativos`](https://github.com/ctala/ai-benchmarks-alternativos).

---

## ¿Cuál proveedor usar? (árbol de decisión)

```text
¿Tienes hardware potente (DGX Spark / GPU 24GB+)?
├── Sí + quieres privacidad / coste fijo  →  ollama-local/  ó  lmstudio-local/  ó  nvidia-nim/
└── No
    ├── ¿Quieres usar Claude oficialmente?       →  anthropic-pro-max/  ($20-200/mes, OAuth)
    ├── ¿Presupuesto bajo y plan mensual?         →  xiaomi/ ($6+) ó zai-coding/ ($10+)
    ├── ¿Solo experimentar / pay-as-you-go?       →  deepseek/  ó  openrouter/  ó  moonshot/
    ├── ¿Eres usuario AWS/GCP/Azure enterprise?   →  aws-bedrock/  ó  google-vertex/  ó  azure-foundry/
    └── ¿Suscripción + agentes coding nativos?    →  minimax/ ($19+) ó qwen-coding/ ó stepfun/
```

---

## Tabla comparativa

### Suscripción mensual fija (endpoint Anthropic-compat nativo)

| Carpeta | Proveedor | Modelos clave | USD/mes | Auth var |
|---|---|---|---|---|
| [`anthropic-pro-max/`](anthropic-pro-max/) | Anthropic Pro / Max | Opus 4.7, Sonnet 4.6, Haiku 4.5 | $20 / $100 / $200 | OAuth |
| [`xiaomi/`](xiaomi/) | Xiaomi MiMo Token Plan | mimo-v2.5-pro, v2.5, v2-pro | $6 / $16 / $50 / $88 | `tp-...` |
| [`zai-coding/`](zai-coding/) | Z.ai GLM Coding Plan | glm-4.7, glm-5.1, glm-4.5-air | ~$10 / $30 / $80 | `sk-...` |
| [`minimax/`](minimax/) | MiniMax Coding Plan | MiniMax-M2.7, M2.7-highspeed | $19 / $20 / $50 | propio |
| [`qwen-coding/`](qwen-coding/) | Alibaba Qwen Coding Plan | qwen3-coder-plus, qwen3-max | variable | `sk-sp-...` |
| [`stepfun/`](stepfun/) | StepFun Step Plan | step-3-fast, step-3 | suscripción | propio |
| [`vercel-gateway/`](vercel-gateway/) | Vercel AI Gateway | passthrough Anthropic + 100+ | $5 trial / $20 Pro | `vck_...` |
| [`ollama-cloud/`](ollama-cloud/) | Ollama Cloud | kimi-k2.5:cloud, glm-5:cloud, minimax-m2.7:cloud | desde ~$20 | `ollama-...` |

### Pay-as-you-go (endpoint Anthropic-compat nativo)

| Carpeta | Proveedor | Modelos clave | Coste | Auth var |
|---|---|---|---|---|
| [`xiaomi-api/`](xiaomi-api/) | Xiaomi MiMo API | igual que `xiaomi/` | per-token | `sk-...` |
| [`zai-api/`](zai-api/) | Z.ai BigModel API | igual que `zai-coding/` | per-token | `sk-...` |
| [`moonshot/`](moonshot/) | Moonshot Kimi K2 | kimi-k2-turbo-preview, k2-0711 | per-token | `sk-...` |
| [`qwen-api/`](qwen-api/) | DashScope API | qwen3-coder-plus, qwen3-max | per-token | `sk-...` |
| [`deepseek/`](deepseek/) | DeepSeek | deepseek-chat, reasoner, coder | per-token (barato) | `sk-...` |
| [`openrouter/`](openrouter/) | OpenRouter (300+ modelos) | anthropic/*, x-ai/*, etc. | per-token (créditos) | `sk-or-...` |

### Cloud enterprise (auth con IAM/SSO, sin `ANTHROPIC_BASE_URL` custom)

| Carpeta | Cloud | Activación | Modelos |
|---|---|---|---|
| [`aws-bedrock/`](aws-bedrock/) | AWS | `CLAUDE_CODE_USE_BEDROCK=1` | `anthropic.claude-*` |
| [`google-vertex/`](google-vertex/) | GCP | `CLAUDE_CODE_USE_VERTEX=1` | `claude-*@<fecha>` |
| [`azure-foundry/`](azure-foundry/) | Azure | endpoint Anthropic-compat | `claude-*` |

### Local self-hosted (DGX Spark, GPU propia, Apple Silicon)

| Carpeta | Stack | Anthropic-compat | Modelos típicos |
|---|---|---|---|
| [`ollama-local/`](ollama-local/) | Ollama (server local) | **NATIVO** (Ollama 0.11+) | qwen3-coder, llama3.3, deepseek-v3, glm-4.5-air |
| [`lmstudio-local/`](lmstudio-local/) | LM Studio + LiteLLM | vía proxy | cualquier GGUF |
| [`nvidia-nim/`](nvidia-nim/) | Docker NIM container | **NATIVO** | NIMs publicados por NVIDIA |

---

## Convenciones del repo

Cada carpeta de proveedor sigue este patrón:

```text
proveedor/
├── .claude/
│   ├── settings.json                  # config compartible (BASE_URL, modelos)
│   ├── settings.local.json.example    # plantilla con placeholder
│   └── settings.local.json            # TU key real (gitignored)
├── .gitignore                         # incluye settings.local.json
└── README.md                          # instrucciones específicas
```

## Cómo usar (general)

```bash
# 1. Antes de empezar: verifica que NADA en tu shell sobreescribe el setup
env | grep ANTHROPIC
# Si ves variables, considera unset o quitarlas de ~/.zshrc

# 2. Entra a la carpeta del proveedor que quieres probar:
cd <proveedor>/

# 3. Copia la plantilla y pega tu API key:
cp .claude/settings.local.json.example .claude/settings.local.json
$EDITOR .claude/settings.local.json

# 4. Lanza Claude Code:
claude

# Dentro de Claude:
/status     # confirma BASE_URL + modelo activo
/model      # cambiar de modelo si el proveedor tiene varios
```

---

## Notas críticas

1. **Variables del shell tienen prioridad sobre `settings.json`** — si tienes `ANTHROPIC_*` exportadas en tu `~/.zshrc`, sobrescribirán la configuración por carpeta. Antes de probar:
   ```bash
   unset ANTHROPIC_AUTH_TOKEN ANTHROPIC_BASE_URL ANTHROPIC_API_KEY
   ```

2. **`ANTHROPIC_AUTH_TOKEN` ≠ `ANTHROPIC_API_KEY`** — proveedores Anthropic-compat third-party usan `_AUTH_TOKEN`. Anthropic directo (pay-as-you-go) usa `_API_KEY`. Confundirlas da `401`. Detalle completo en [`_docs/troubleshooting.md`](_docs/troubleshooting.md).

3. **Onboarding** — la primera vez que Claude Code se abre en una carpeta nueva, pide aceptar "Trust this folder". Para saltar el onboarding inicial general, crea `~/.claude.json` con `{"hasCompletedOnboarding": true}`.

4. **Las URLs y nombres de modelos cambian** — los proveedores chinos en particular renombran modelos rápido (Z.ai migró `glm-4.5` → `glm-4.7`, MiMo retiró `mimo-v2-flash`). Si ves `model not found`, revisa el README del proveedor y los docs oficiales.

5. **Carpetas marcadas con TODO** — `qwen-api/` y `stepfun/` tienen URLs que requieren verificación adicional con docs oficiales antes de uso intensivo.

---

## FAQ

### ¿Puedo usar Claude Code sin pagar la suscripción de Anthropic?

Sí. Claude Code es la CLI; el "modelo" detrás es configurable. Setea `ANTHROPIC_BASE_URL` apuntando a un proveedor third-party con endpoint Anthropic-compat (Z.ai, Xiaomi, Moonshot, DeepSeek, etc.) y Claude Code envía las peticiones ahí en lugar de a `api.anthropic.com`. Ver cualquier carpeta de este repo para un setup concreto.

### ¿Cómo uso Claude Code con Kimi K2?

Carpeta [`moonshot/`](moonshot/). Moonshot expone un endpoint Anthropic-compat oficial en `https://api.moonshot.ai/anthropic` (o `https://api.moonshot.cn/anthropic` para China). Pega tu key `sk-...` en `.claude/settings.local.json` y `claude` arranca usando `kimi-k2-turbo-preview`.

### ¿Cómo uso Claude Code con DeepSeek?

Carpeta [`deepseek/`](deepseek/). DeepSeek expone Anthropic-compat en `https://api.deepseek.com/anthropic`. Es de los más baratos del catálogo (per-token).

### ¿Cómo uso Claude Code con GLM-5.1 / Z.ai?

Dos opciones:
- [`zai-coding/`](zai-coding/) si quieres el **GLM Coding Plan** (suscripción mensual desde ~$10).
- [`zai-api/`](zai-api/) si prefieres pay-as-you-go vía BigModel API.

Ambas usan `https://api.z.ai/api/anthropic` y key `sk-...`.

### ¿Es OpenRouter Anthropic-compatible?

Sí. OpenRouter expone `/v1/messages` nativo en `https://openrouter.ai/api`. Carpeta [`openrouter/`](openrouter/). Te da acceso a 300+ modelos (Anthropic, x-ai, Google, Meta, Qwen, DeepSeek) bajo una sola key con créditos prepagados.

### ¿Funciona Claude Code con modelos locales (Ollama / LM Studio)?

Sí. **Ollama 0.11+ tiene Anthropic-compat NATIVO** (`/v1/messages` directo en `:11434`) — sin proxy. Misma simplicidad que NVIDIA NIM. Carpeta [`ollama-local/`](ollama-local/). LM Studio sigue exponiendo solo OpenAI-compat, así que [`lmstudio-local/`](lmstudio-local/) requiere LiteLLM como traductor. Setup completo en [`_docs/local-setup.md`](_docs/local-setup.md).

¿Y si quieres modelos hosted sin GPU local? [`ollama-cloud/`](ollama-cloud/) corre `kimi-k2.5:cloud`, `glm-5:cloud`, `minimax-m2.7:cloud` en infraestructura Ollama bajo suscripción mensual.

### ¿Puedo correr Claude Code en una NVIDIA DGX Spark?

Sí, y desde Ollama 0.11+ es trivial: Ollama corre nativo en ARM64 y expone Anthropic-compat directo. La carpeta [`ollama-local/`](ollama-local/) y la guía [`_docs/local-setup.md`](_docs/local-setup.md) cubren el setup específico para Spark (GB10 Grace Blackwell ARM64, 128GB unified). Modelos recomendados: `qwen3-coder:480b`, `qwen2.5-coder:32b`, `llama3.3:70b`, `deepseek-coder-v2:236b`.

### ¿Cuál es la diferencia entre `xiaomi/` y `xiaomi-api/`?

[`xiaomi/`](xiaomi/) es el **Token Plan**: suscripción mensual con créditos prepagados (desde $6/mes). [`xiaomi-api/`](xiaomi-api/) es la **API pay-as-you-go**: pagas por token consumido. Ambas usan los mismos modelos `mimo-*`. Lo mismo aplica a `zai-coding/` vs `zai-api/`.

### ¿Por qué OAuth de Pro/Max no funciona en otros IDEs?

Desde abril 2026, Anthropic bloquea los tokens OAuth (`sk-ant-oat01-...`) de Pro/Max para uso fuera de Claude Code CLI oficial, claude.ai y Claude Desktop. Si necesitas Claude en Cline/Aider/etc., paga API key separada (`sk-ant-api03-...`) en `console.anthropic.com`. Detalle en [`anthropic-pro-max/`](anthropic-pro-max/).

### ¿Qué proveedor es el más barato?

- **Suscripción**: Xiaomi MiMo Lite a ~$6/mes (60M créditos) o el plan free de Vercel Gateway ($5 trial 30 días).
- **Pay-as-you-go**: DeepSeek tiene de los precios más bajos por token del mercado.
- **Local**: si ya tienes la GPU, solo pagas electricidad.

### ¿Puedo combinar varios proveedores?

Sí. Cada carpeta es independiente. Tener `xiaomi/` + `anthropic-pro-max/` + `deepseek/` clonadas no entra en conflicto: el `settings.json` es por carpeta. Cambias de proveedor con `cd`. Para fallback automático entre varios, usa [OpenRouter](openrouter/) o un router como [`musistudio/claude-code-router`](https://github.com/musistudio/claude-code-router).

---

## Documentación adicional

- [`_docs/proxies.md`](_docs/proxies.md) — comparativa de proxies traductores (LiteLLM, claude-code-router, copilot-api, y-router).
- [`_docs/local-setup.md`](_docs/local-setup.md) — Ollama / LM Studio / vLLM / NIM en DGX Spark u otras GPUs.
- [`_docs/troubleshooting.md`](_docs/troubleshooting.md) — errores comunes (`model not found`, `401`, OAuth bloqueado, latencias).

---

## Repos relacionados

- [`ctala/ai-benchmarks-alternativos`](https://github.com/ctala/ai-benchmarks-alternativos) — benchmarks comparativos de los modelos cubiertos aquí (mismo autor).
- [`musistudio/claude-code-router`](https://github.com/musistudio/claude-code-router) — proxy ligero en Node, alternativa a LiteLLM.
- [`BerriAI/litellm`](https://github.com/BerriAI/litellm) — proxy multi-backend Python (recomendado para setups con múltiples proveedores).
- [`Alorse/cc-compatible-models`](https://github.com/Alorse/cc-compatible-models) — guía estilo cheatsheet de modelos compatibles, complementaria.
- [`hesreallyhim/awesome-claude-code`](https://github.com/hesreallyhim/awesome-claude-code) — lista awesome de skills/hooks/plugins (no proveedores).

---

## Contribuir

¿Falta un proveedor? ¿Cambió un precio o un nombre de modelo? Abre un PR. Ver [CONTRIBUTING.md](CONTRIBUTING.md) para el formato de carpeta y la checklist de verificación.

## Licencia

[MIT](LICENSE) — usa, modifica, redistribuye. Atribución apreciada pero no requerida. La elección de MIT (vs. Apache-2.0) prioriza adopción y compatibilidad: cualquier dev puede copiar un `settings.json` a su repo sin fricción legal.

---

<sub>Mantenido por [@ctala](https://github.com/ctala). Si este repo te ahorra una tarde de debugging de `ANTHROPIC_BASE_URL`, considera darle una estrella — ayuda a que otros devs lo encuentren.</sub>
