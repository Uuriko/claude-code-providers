# StepFun · Step Plan (suscripción)

StepFun (阶跃星辰) ofrece sus modelos Step-3 con endpoint Anthropic-compat dentro de su Step Plan.

- **Base URL**: `https://api.stepfun.com/step_plan/anthropic` (verificar)
- **API key**: desde <https://platform.stepfun.com/interface-key>
- **Auth var**: `ANTHROPIC_AUTH_TOKEN`
- **Docs**: <https://platform.stepfun.com/docs/en/step-plan/integrations/claude-code>

## Modelos

- `step-3-fast` — modelo rápido del Step Plan
- `step-3` — modelo estándar
- (Verificar el catálogo más reciente; StepFun renombra modelos con relativa frecuencia)

## Plan

Suscripción + consumo. Detalles de pricing en <https://platform.stepfun.com>.

## Uso

```bash
cp .claude/settings.local.json.example .claude/settings.local.json
claude
```

> **TODO**: este `BASE_URL` y los nombres de modelos requieren verificación en la docs oficial de StepFun en español/inglés. La página estaba devolviendo 404 al snapshot de esta config; confirma la URL correcta antes de pagar la suscripción.
