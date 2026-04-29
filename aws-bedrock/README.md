# AWS Bedrock · Claude vía IAM

Acceso enterprise a los modelos Anthropic vía AWS Bedrock. No usa `ANTHROPIC_BASE_URL`; Claude Code se conecta vía SDK de AWS cuando ve `CLAUDE_CODE_USE_BEDROCK=1`.

- **Activación**: env var `CLAUDE_CODE_USE_BEDROCK=1`
- **Auth**: IAM (perfil AWS). Recomendado: `aws sso login` o `aws configure`.
- **Región**: define `AWS_REGION` (ej. `us-west-2`, `us-east-5`).
- **Docs**: <https://docs.claude.com/en/docs/claude-code/amazon-bedrock>

## Modelos

Los IDs de Bedrock incluyen versión y sufijo `:0`:
- `anthropic.claude-opus-4-7-20251128-v1:0`
- `anthropic.claude-sonnet-4-6-20251022-v1:0`
- `anthropic.claude-haiku-4-5-20251001-v1:0`

> Verifica IDs exactos con `aws bedrock list-foundation-models --region us-west-2 | grep claude` — Anthropic ajusta el sufijo de fecha periódicamente.

## Costo

- **On-demand**: pay-per-token, mismo precio que Anthropic directo.
- **Provisioned throughput**: compromiso 1m / 6m con descuento. Útil para uso muy alto, pero requiere ~$500-2000/mes mínimo según modelo.

## Uso

```bash
# Una vez, fuera de Claude Code:
aws sso login --profile mi-perfil
# o
aws configure

# Luego:
cp .claude/settings.local.json.example .claude/settings.local.json
# Edita AWS_PROFILE si usas algo distinto a "default"
claude
```

## Pre-requisitos

- Modelos Anthropic deben estar **activados** en tu cuenta Bedrock (ir a console → Bedrock → Model access → Request access).
- Permisos IAM mínimos: `bedrock:InvokeModel`, `bedrock:InvokeModelWithResponseStream` para los model IDs específicos.
