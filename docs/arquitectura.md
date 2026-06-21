# Arquitectura — Asistente Visual LV

## Resumen

Aplicación de una sola página HTML (`asistente-caballeros-v7.html`), sin backend, sin build step. Todo el estado vive en memoria del navegador (`localStorage` / `sessionStorage` para persistencia ligera).

## Guardrails (v7.2)

A partir de v7.2 el asistente aplica reglas de comportamiento que se inyectan en cada request al modelo (`GUARDRAIL_APPEND`) y se complementan con detección cliente del scope de la pregunta (`assessQuestionScope`). Categorías:

- **Saludos** (`hola`, `buenos días`, etc.) → respuesta breve amable en 1-2 oraciones invitando a preguntar sobre VM Caballeros Liverpool.
- **Fuera de tema** (clima, tareas, otras tiendas, preguntas no-VM) → redirección corta sin formato de 6 etapas ni razonamiento visible.
- **Otra tienda + tema VM** → aclara que solo conoce manuales Liverpool y responde la regla Liverpool si está en el contexto.
- **Pregunta sobre el creador** (`assessQuestionScope` + `isCreatorQuestion`) → única situación donde se permite mencionar a Gerardo Barrera.

Además, `sanitizeFinalAnswer` elimina automáticamente menciones al creador y prefijos internos (`[PENSAMIENTO INTERNO]`, `ETAPA N`, etc.) del bloque `[RESPUESTA FINAL]` cuando la pregunta no lo justifica.

## Pipeline de razonamiento (6 etapas)

Cada consulta del usuario pasa por un pipeline estructurado dentro del prompt enviado al modelo de IA:

1. **INTENT** — Clasifica qué tipo de pregunta es (regla de display, vocabulario, excepción, etc.)
2. **EXPANSIÓN SEMÁNTICA** — Expande la consulta a términos y sinónimos del vocabulario interno de Liverpool (vía `SYNONYMS` y `expandKeywords`)
3. **RETRIEVAL** — Recupera los fragmentos relevantes de los manuales consolidados (`getManualContext`, `getPdfContext`, `buildContext`)
4. **RAZONAMIENTO** — El modelo razona internamente sobre las reglas encontradas (bloque `[PENSAMIENTO INTERNO]`)
5. **CERTEZA** — El modelo califica su nivel de confianza antes de responder (Mandatory / Recommendation / Exception)
6. **RESPUESTA** — Se entrega la respuesta final, separada del razonamiento interno vía `parseAIResponse`

### Excepciones al formato de 6 etapas

El pipeline completo se aplica **únicamente** a preguntas válidas de VM Caballeros Liverpool. En los siguientes casos el formato se omite (`scope.clearlyOff` o `scope.isGreeting`) para evitar fricción:

- Saludos — respuesta breve sin etapas ni `[PENSAMIENTO INTERNO]`.
- Fuera de tema — redirección corta, máximo 2 oraciones, sin bullets.
- Mención del creador — respuesta corta, sin estructura de etapas.

En estos casos el bloque de razonamiento también se oculta en el render (`renderAssistantMessage`) para mantener la UI coherente.

## Proveedores de IA soportados

| Proveedor | Modelos | Endpoint |
|---|---|---|
| GitHub Models | `gpt-4o-mini`, `gpt-4o` | `models.inference.ai.azure.com` |
| OpenAI | `gpt-4o-mini`, `gpt-4o`, `gpt-4-turbo` | `api.openai.com` |
| Google Gemini | `gemini-3.5-flash`, `gemini-2.5-flash`, `gemini-2.5-flash-lite`, `gemini-3-flash-preview`, `gemini-3.1-flash-lite` | `generativelanguage.googleapis.com` |

Las API keys se guardan en `sessionStorage` (por proveedor), nunca en `localStorage`, para minimizar exposición.

## Gestión de contexto y memoria

- `buildContext(query)` combina contexto de manuales, PDFs cargados por el usuario y contexto extra configurable, respetando presupuestos de tokens por sección (`MANUAL_BUDGET`, `PDF_BUDGET`, `EXTRA_BUDGET`).

### Scoring RAG (v7.2)

`scoreText(query, keywords)` pondera los chunks de contexto con bonificaciones adicionales para señales de alta autoridad:

- **`[MANDATORY]`** en el chunk → `+4` al score. Prioriza reglas absolutas sobre recomendaciones o contexto complementario.
- **"conflicto documentado"** en el chunk → `+5` al score. Asegura que cuando hay inconsistencia entre manuales (ej. entallado 4 vs 5 vs 6 piezas), la respuesta cede a los chunks que la citan explícitamente en vez de a aquellos que omiten el conflicto.

Estas señales se suman al conteo base de keywords y al bonus de presencia en los primeros 300 caracteres (`first300.includes(kw) ? +3`).

Sinónimos relevantes para entallado: `tallas ordenadas`, `size run`, `secuencia tallas`, `piezas`, `pieza`, `cuantas piezas`.

- `memory` (`fails` / `successes`) permite que el asistente "aprenda" de interacciones pasadas marcadas por el usuario con 👍/👎.
- El historial de chat (`history`) se gestiona con cuidado especial en caso de interrupción (`AbortError`): solo se conserva si la respuesta parcial superó 50 caracteres, para evitar contaminar el contexto de turnos futuros.

## Seguridad

- Todo el HTML se sanitiza vía DOMPurify (`PURIFY_CFG`, `safeMarkdown`) antes de inyectarse al DOM.

### Versionado de prompt y storage

A partir de v7.2, `localStorage` incluye la clave `lv_prompt_version`. En cada `loadSaved()` se compara contra la versión esperada (`'7.2'`):

- Si **no coincide** → se fuerza la recarga del system prompt desde el textarea y se actualiza `lv_system` + `lv_prompt_version`. Garantiza que usuarios con prompts antiguos en `localStorage` migren automáticamente al nuevo prompt con guardrails.
- Si **coincide** → se respeta el valor guardado en `lv_system`.

Esto permite iterar el prompt en futuras versiones sin pedirle al usuario que borre su storage manualmente.

- Sin backend propio: cero superficie de ataque server-side. Las únicas llamadas salientes son directamente a las APIs oficiales de los proveedores de IA, desde el navegador del usuario, con su propia key.
