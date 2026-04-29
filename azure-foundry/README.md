# Azure AI Foundry · Claude vía Anthropic-compat

Microsoft Azure ofrece modelos Anthropic en AI Foundry con un endpoint Anthropic-compat nativo (a diferencia de Bedrock/Vertex que requieren flags propios).

- **Base URL**: `https://<your-resource>.services.ai.azure.com/anthropic`
- **Auth**: Azure API key o token Entra ID en `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/use-foundry-models-claude>

## Modelos

`claude-opus-4-7`, `claude-sonnet-4-6`, `claude-haiku-4-5` (los nombres exactos los muestras en tu Azure AI Foundry deployment).

## Costo

Pay-per-token contra tu suscripción Azure. Sin plan mensual fijo a nivel del modelo (la suscripción Azure es separada).

## Uso

```bash
# Edita .claude/settings.json:
#   ANTHROPIC_BASE_URL → reemplaza <your-resource> por el nombre de tu recurso de AI Foundry
cp .claude/settings.local.json.example .claude/settings.local.json
# Pega tu Azure API key como ANTHROPIC_AUTH_TOKEN
claude
```

## Pre-requisitos

- Recurso de Azure AI Foundry creado.
- Deployment de los modelos Anthropic en tu hub.
- API key del recurso (Foundry → Keys and Endpoint).
