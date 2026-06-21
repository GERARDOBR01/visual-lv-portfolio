# Changelog — Asistente Visual AV

## v7.2 — Junio 2026
- Guardrails: scope detection para saludos, fuera de tema, otra tienda y mención del creador
- Saludos (hola, buenos días) ya no reciben redirección de fuera de tema
- Preguntas VM válidas reciben nota para usar formato de 6 etapas y revisar contexto
- Regla explícita: conflictos documentados se citan, no se responde "no especifica"
- Entallado: sección dedicada en biblia + sinónimos pieza/piezas + boost RAG (MUST/CONFLICT)
- Parser más robusto: recupera respuesta aunque el modelo omita marcadores exactos
- Streaming: solo se muestra "Consultando manuales…" hasta la respuesta final
- Sanitizado: no filtra menciones al creador ni ETAPAS en la respuesta visible
- Auto-migración de prompt vía `av_prompt_version`

## v7.0 — Junio 2026
- API Experta: selector de modelo por rol (Normativo / Estratega / Rápido) con validación de API key por proveedor (`hasKey`)
- Fix crítico: `renderQuickBtns()` implementado — accesos rápidos funcionan correctamente
- Fix crítico: `maxTokens` aumentado de 900 a 2500 para evitar respuestas truncadas en el pipeline de 6 etapas
- `saveConfig()` refactorizado para recibir el evento explícitamente (sin depender de `event` global)
- Sistema de toasts no bloqueante reemplaza alerts nativos
- Manejo robusto de `AbortError`: el historial solo se actualiza si la respuesta parcial tiene contenido útil

## v6 y anteriores
- Versiones de desarrollo interno (paso 1 a paso 4), no publicadas
