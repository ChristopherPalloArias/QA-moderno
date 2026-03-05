# QA Moderno — Budget Management App

> **Proyecto:** Budget Management App
> **Propósito:** Repositorio de artefactos de QA generados y validados con asistencia de IA (Gemas de Gemini), cubriendo desde el refinamiento de historias de usuario hasta la generación de casos de prueba y la ejecución automatizada de tests.

---

## Estructura del Repositorio

```
├── BUSINESS_CONTEXT_GEMA.md
├── BUSINESS_CONTEXT_GEMB.md
├── BUSINESS_CONTEXT_general.md
├── USER_STORIES_REFINEMENT.md
├── TEST_CASES_AI.md
└── README.md
```

---

## Contextos de Negocio

Cada archivo de contexto de negocio fue diseñado con un propósito específico para alimentar a las Gemas de Gemini y obtener resultados más precisos y relevantes.

### `BUSINESS_CONTEXT_GEMA.md` — Contexto para la Gema A (Diagnóstico INVEST)

| Detalle | Descripción |
|---|---|
| **Gema** | [Gema A — Diagnóstico INVEST de Historias de Usuario](https://gemini.google.com/gem/1pSq07TA92ExOnsbQlBTzfdwrcuEUQ02c?usp=sharing) |
| **Propósito del contexto** | Proveer a la Gema A la información de negocio necesaria para realizar un diagnóstico INVEST completo sobre las historias de usuario. |
| **Contenido clave** | Descripción del proyecto, flujos críticos del negocio, roles de usuario, stack tecnológico y reglas de negocio relevantes para evaluar la calidad de las HU. |

### `BUSINESS_CONTEXT_GEMB.md` — Contexto para la Gema B (Generación de Casos de Prueba)

| Detalle | Descripción |
|---|---|
| **Gema** | [Gema B — Generación de Casos de Prueba en Gherkin](https://gemini.google.com/gem/1ubyJYIpdYv5FZhAAefV_G4gcJHS-4_as?usp=sharing) |
| **Propósito del contexto** | Proveer a la Gema B la información de negocio y especificaciones técnicas (tipos de datos, límites de campos, endpoints) para generar casos de prueba detallados en formato Gherkin. |
| **Contenido clave** | Toda la información del contexto de la Gema A más datos técnicos orientados a testing: tipos de datos esperados, límites de campos y endpoints relevantes del API. |

### `BUSINESS_CONTEXT_general.md` — Contexto General (Gema A + Gema B)

| Detalle | Descripción |
|---|---|
| **Gemas** | Ambas — [Gema A](https://gemini.google.com/gem/1pSq07TA92ExOnsbQlBTzfdwrcuEUQ02c?usp=sharing) y [Gema B](https://gemini.google.com/gem/1ubyJYIpdYv5FZhAAefV_G4gcJHS-4_as?usp=sharing) |
| **Propósito del contexto** | Plantilla unificada y completa que sirve para ambas Gemas. Contiene tanto la información de negocio como las especificaciones técnicas, personalizada para maximizar el rendimiento de cada Gema al tener toda la información consolidada en un solo documento. |
| **Contenido clave** | Información completa del proyecto, flujos de negocio, roles, stack tecnológico, reglas de negocio y especificaciones técnicas de testing en un solo lugar. |

> **¿Por qué un contexto general?** Aunque existen contextos individuales para cada Gema, el contexto general consolida toda la información en un único archivo más personalizado. Esto permite que ambas Gemas tengan una visión completa del sistema, lo que mejora la coherencia y calidad de los resultados generados.

---

## Artefactos de Análisis

### `USER_STORIES_REFINEMENT.md` — Refinamiento de Historias de Usuario

Documento donde se realizó el análisis comparativo de las historias de usuario utilizando ambas Gemas. Contiene:

- **Historias de usuario originales** tal como fueron redactadas inicialmente.
- **Versiones refinadas** generadas tras pasar por el diagnóstico INVEST de la Gema A.
- **Comparativa detallada** identificando cambios en alcance, criterios de aceptación y reglas de negocio.
- Se analizaron las HU: **US-017**, **US-019** y **US-021**.

### `TEST_CASES_AI.md` — Matriz de Casos de Prueba Generados por IA

Matriz completa de casos de prueba generados por la Gema B en formato Gherkin, organizada por historia de usuario. Incluye:

- **Casos de prueba generados por la Gema** con técnicas como Caso de Uso, Transición de Estados, Tabla de Decisión, Análisis de Valores Límite y Adivinación de Errores.
- **Ajustes realizados por el probador** donde se identificaron gaps en los casos generados.
- **Justificación de cada ajuste** explicando por qué fue necesaria la intervención humana.
- Total de **21 casos de prueba** (CP-001 a CP-021) cubriendo las tres historias de usuario.

---

## Pruebas Automatizadas — Repositorio Externo

Las pruebas automatizadas de **UI**, **API** y **Unit Test** se encuentran en un repositorio externo independiente. El objetivo es demostrar y comparar los tiempos de ejecución de cada tipo de prueba.

| Detalle | Información |
|---|---|
| **Repositorio** | [Budget_Management_App](https://github.com/ChristopherPalloArias/Budget_Management_App.git) |
| **Rama** | `📌 _pendiente de definir_` |

### Tipos de Prueba

| Tipo de Prueba | Herramientas / Framework | Descripción |
|---|---|---|
| **UI Tests** | Gradle + Serenity BDD | Pruebas de interfaz de usuario end-to-end que validan flujos completos desde la perspectiva del usuario. Serenity genera reportes detallados con capturas de pantalla paso a paso. |
| **API Tests** | _Por definir_ | Pruebas que validan los endpoints del backend, verificando respuestas HTTP, códigos de estado, estructura de payloads y reglas de negocio a nivel de servicio. |
| **Unit Tests** | _Por definir_ | Pruebas unitarias que validan la lógica de negocio de componentes individuales de forma aislada. |

### Métricas de Tiempo de Ejecución

> ⏱️ _Sección pendiente de completar tras la ejecución de las pruebas._

| Tipo de Prueba | Cantidad de Tests | Tiempo de Ejecución | Observaciones |
|---|---|---|---|
| UI Tests | — | — | — |
| API Tests | — | — | — |
| Unit Tests | — | — | — |

---

## Gemas Utilizadas

| Gema | Propósito | Enlace |
|---|---|---|
| **Gema A** | Diagnóstico INVEST de Historias de Usuario — Evalúa la calidad de las HU según los criterios INVEST y sugiere refinamientos. | [Abrir Gema A](https://gemini.google.com/gem/1pSq07TA92ExOnsbQlBTzfdwrcuEUQ02c?usp=sharing) |
| **Gema B** | Generación de Casos de Prueba en Gherkin — Genera casos de prueba detallados a partir de HU refinadas, usando múltiples técnicas de diseño de pruebas. | [Abrir Gema B](https://gemini.google.com/gem/1ubyJYIpdYv5FZhAAefV_G4gcJHS-4_as?usp=sharing) |
