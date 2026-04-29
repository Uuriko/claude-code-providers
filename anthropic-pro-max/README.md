# Anthropic · Pro / Max (OAuth)

La forma "oficial" de usar Claude Code con suscripción mensual fija. **No hay BASE_URL custom**: usa los endpoints estándar de Anthropic, autenticados vía OAuth desde tu cuenta claude.ai.

- **BASE_URL**: por defecto `https://api.anthropic.com` (no se setea)
- **Auth**: OAuth — se ejecuta `claude /login` la primera vez, abre el navegador, y guarda el token (`sk-ant-oat01-...`) cifrado en el keychain del SO.
- **Docs**: <https://docs.claude.com/en/docs/claude-code/setup>

## Planes

| Plan | USD/mes | Cuota relativa | Notas |
|---|---|---|---|
| Pro | $20 | 1× | Uso personal moderado |
| Max 5x | $100 | 5× cuota Pro | Intensivo |
| Max 20x | $200 | 20× cuota Pro | Heavy users / equipos pequeños |

Las cuotas se miden en mensajes/sesión 5h, no en tokens absolutos. Anthropic re-balancea periódicamente.

## Modelos disponibles

Todos los modelos Claude actuales:
- `claude-opus-4-7` (Opus 4.7)
- `claude-sonnet-4-6` (Sonnet 4.6)
- `claude-haiku-4-5` (Haiku 4.5)

Claude Code los selecciona automáticamente según el contexto y `/model`.

## Uso

```bash
cd anthropic-pro-max/
claude /login    # abre navegador, autentica
claude           # ya estás dentro
/status          # confirma "Connected" + modelo activo
```

## Notas importantes

- **OAuth de Pro/Max NO funciona con APIs de terceros** desde abril 2026: solo Claude Code CLI oficial, claude.ai y Claude Desktop pueden usarlo. Otros wrappers (Cline, Aider) requieren una API key `sk-ant-api03-...` separada (pay-as-you-go).
- Si tienes ambos plan + API key, Claude Code prioriza el OAuth a menos que setees `ANTHROPIC_API_KEY` explícito en `settings.json`.
- Para usar API key directa (pay-as-you-go) en vez de Pro/Max, no uses esta carpeta — crea otra con `ANTHROPIC_API_KEY`.
