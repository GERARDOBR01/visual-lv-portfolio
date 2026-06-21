# Asistente Visual IA — Caballeros Mercadep
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![No Build Step](https://img.shields.io/badge/no%20build%20step-yes-brightgreen)](https://github.com/GERARDOBR01/visual-lv-portfolio)
[![Language: JavaScript | HTML](https://img.shields.io/badge/language-JavaScript%20%7C%20HTML-blue)](https://github.com/GERARDOBR01/visual-lv-portfolio)
[![Live Demo](https://img.shields.io/badge/demo-live-brightgreen)](https://gerardobr01.github.io/visual-lv-portfolio/)
[![Single File](https://img.shields.io/badge/architecture-single--file-blue)]()
[![AI Providers](https://img.shields.io/badge/AI%20providers-3-orange)]()
[![Language](https://img.shields.io/badge/language-Spanish-yellow)]()
*AI-powered visual merchandising assistant for Mercadep department stores (Caballeros section)*

## Demo
<!-- ![Demo](docs/demo.gif) -->

> *Real query from a Mercadep retail floor → 6-stage reasoning → calibrated response.*

## El problema

Visual merchandisers consult 15+ retail manuals for display rules. Queries like *"¿cuántas piezas van en el entallado?"* previously required 10–15 minutes of manual search.

**This tool answers in seconds, citing the exact rule and its certainty level.**

## Qué hace

- RAG sobre 15 manuales consolidados de Mercadep Caballeros
- Pipeline de 6 etapas: INTENT → EXPANSIÓN SEMÁNTICA → RETRIEVAL → RAZONAMIENTO → CERTEZA → RESPUESTA
- 3 proveedores de IA: GitHub Models (gratis), OpenAI, Google Gemini
- Streaming de respuestas · Memoria de aprendizaje · Export a Markdown
- **100% frontend, zero backend, zero install — open the HTML and go**

## Cómo correrlo

1. [Demo live →](https://gerardobr01.github.io/visual-lv-portfolio/)
2. O descarga `asistente-caballeros-v7.html` y ábrelo en Chrome
3. Obtén una API key gratuita de GitHub Models (ver [docs/como-obtener-api-key.md](docs/como-obtener-api-key.md))
4. Configúrala en la pestaña ⚙️ Config y empieza a consultar

## Arquitectura

Ver [docs/arquitectura.md](docs/arquitectura.md) para el pipeline completo de 6 etapas y las decisiones técnicas de diseño.

## Autor

**GerardoBr_** — Asesor Visual, Mercadep
Proyecto independiente desarrollado fuera de horario laboral.

*Este proyecto es un proyecto independiente de portafolio, sin afiliación corporativa alguna.*
