# HANDOFF — Reporte MAV

> Documento de contexto para retomar el proyecto sin explicar todo de nuevo.
> Última actualización: **2026-07-03**.
> Si trabajás con IA/asistente: leé este archivo primero, y `docs/ARCHITECTURE.md` para el detalle técnico.

---

## 1. Qué es

Dashboard web estático que visualiza operaciones de **cheques y pagarés del Mercado Argentino de Valores (MAV)**. Lee datos desde un Google Sheet publicado como CSV (sin autenticación). Desarrollado para **one618**.

## 2. Dónde está / cómo se trabaja

- **Repo (working folder):** `G:\Tecnico\Mesa\Estrategia\Isaias\GitHub\Reporte-MAV`
  - Es una **unidad de red** → un asistente solo la accede vía file tools (Read/Write/Edit/Grep/Glob), **no** vía bash/shell.
- **Remote:** https://github.com/imarinione618/Reporte-MAV.git — branch `main`.
- **El usuario commitea y pushea desde VS Code.** El asistente edita archivos; NO corre git.
- Carpeta vieja abandonada: `...\Documentos\Claude\Projects\Reporte MAV\dashboard_mav.html` → **no tocar**.

## 3. Estructura de archivos

| Archivo | Contenido |
|---|---|
| `index.html` | Shell HTML, carga CSS y JS al final del body |
| `assets/styles.css` | Todo el CSS |
| `src/app.js` | Todo el JS: parseo, filtros, gráficos, tabs (archivo grande, ~2000+ líneas) |
| `docs/ARCHITECTURE.md` | Detalle técnico de la arquitectura |
| `docs/CHANGELOG.md`, `docs/CONTRIBUTING.md` | Historial y guía de contribución |

## 4. Fuente de datos

```js
const SHEET_ID = '11r7oJ9mm4-IUHszK6LkTAFZOzFUe0RZBEWgP7ZzwvvE';
const PUB_ID   = '2PACX-1vTwkGoYISKklkkzXek9Rr_buY6B-85SWYee03zbLdAIRjwONAFtNOm1NW24ixVU-l0AJL1A3UXyMWvk';
const GID      = '220459711';   // Histórico (subastas_historico)
const RT_GID   = '405041137';   // Tiempo Real (día actual)
```

El CSV se parsea en `parseCSV()`. Mapeo de columnas relevantes:

| Campo | Header CSV | Fallback col |
|---|---|---|
| fecha | FEC.SUB. | 1 |
| segmento | SEGMENTO | 2 |
| tipo | TIPO INSTRUMENTO | 6 |
| tasa | TASA C. | 14 |
| moneda | MONEDA | 17 |
| monto | MONTO | 18 |
| empresa | NOMBRE RESPONSABLE | 24 |
| categoria | CATEGORIA | 26 |
| tramo | TRAMO | 27 |
| ppv | PPV | — (NaN si no existe) |

- **PPV** = días de plazo de cada operación. Vencimiento aproximado = `fecha_emisión + PPV días`.

## 5. Las 3 pestañas

1. **Histórico** (`switchTab('hist')`) — filtros por moneda/segmento/tipo/fechas; KPIs; gráficos de tasa por tramo, evolución temporal, pies, montos por día; tabla top empresas; treemap por empresa.
2. **Tiempo Real** (`switchTab('rt')`) — datos del día (RT_GID); KPIs, gráficos intraday, bubble chart (plazo vs tasa vs volumen).
3. **Empresas** (`switchTab('emp')`) — selector "Empresa principal" (single) + "Comparar con…" (multi). KPIs, pies por moneda/instrumento, resumen por instrumento, gráfico de vencimientos por mes, distribución por tramo, y tabla **Detalle de Operaciones**.

## 6. Cosas importantes a saber (gotchas)

- Las columnas de tramo en la tabla comparativa de Empresas (`0-30 d`, `31-60 d`, …) son la **tasa TNA ponderada por monto dentro de cada tramo** (`tm[t] = Σ(tasa·monto)/Σmonto`), NO la distribución de montos.
  - Por eso puede pasar que una empresa tenga tasa promedio total más alta pero menor en cada tramo: es un **efecto de composición (paradoja de Simpson)**, según cómo reparte el monto entre plazos. No es un bug.
- El footer "Prom. pond. comparación" pondera **solo** las empresas de comparación (`stats.slice(1)`), no la principal.
- Montos en gráficos de "Montos Negociados por Día" están en **miles de millones (MM)** = `v/1e9`.
- Colores: `EMP_MAIN_COLOR = '#1A49C8'` (principal siempre azul); comparación usa `EMP_PALETTE`.

## 7. Google Apps Scripts (viven en el Sheet, no en el repo)

- `consolidar_diario.gs` — copia `subastas_modelo` → `subastas_historico` 1x/día (~17:00).
- `fetch_subastas_listado_fixed.gs` — fetch a la API MAV cada 5 min, actualiza `subastas_listado`.

## 8. Cambios recientes

- **2026-07-03** — Tabla "Detalle de Operaciones" (pestaña Empresas): ordenamiento cambiado de `monto desc` a **fecha de emisión desc** (más reciente primero, monto como desempate). `renderEmpOpTable()` en `src/app.js`.
- **2026-05-29** — Pestaña Empresas v2 (dos selectores, pies, resumen por instrumento, vencimientos, distribución por tramo); montos por día pasados a MM.

## 9. Pendientes / ideas abiertas

- (Opcional) Agregar vista de **distribución de monto por tramo** en la tabla comparativa de Empresas, para hacer visible el efecto de composición descrito en §6.
- Testear pestaña Empresas con más casos reales y ajustar visualizaciones si hace falta.

## 10. Preferencias de trabajo del usuario

- Respuestas **concisas y directas**, en español.
- **Nunca inventar datos ni asumir sin explicitarlo.** Ante la duda, preguntar.
