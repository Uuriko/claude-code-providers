# Google Vertex AI · Claude vía gcloud

Modelos Anthropic alojados en Vertex AI (Google Cloud). Activación con `CLAUDE_CODE_USE_VERTEX=1`.

- **Activación**: `CLAUDE_CODE_USE_VERTEX=1`
- **Auth**: `gcloud auth application-default login` una vez (Application Default Credentials)
- **Variables**: `CLOUD_ML_REGION` (ej. `us-east5`), `ANTHROPIC_VERTEX_PROJECT_ID` (tu GCP project ID)
- **Docs**: <https://docs.claude.com/en/docs/claude-code/google-vertex-ai>

## Modelos

Los IDs en Vertex usan `@<fecha>` como versión:
- `claude-opus-4-7@20251128`
- `claude-sonnet-4-6@20251022`
- `claude-haiku-4-5@20251001`

## Costo

Pay-per-token, sin compromiso mensual. Mismo precio que Anthropic directo. Provisioned throughput disponible para uso muy alto.

## Uso

```bash
gcloud auth application-default login
gcloud config set project <tu-project-id>

cp .claude/settings.local.json.example .claude/settings.local.json
# Edita ANTHROPIC_VERTEX_PROJECT_ID en .claude/settings.json
claude
```

## Pre-requisitos

- API "Vertex AI" habilitada en tu GCP project.
- Modelos Anthropic activados (en GCP console → Vertex AI → Model Garden → Anthropic models → Enable).
- Cuotas suficientes en la región elegida (algunas regiones tienen modelos limitados).
