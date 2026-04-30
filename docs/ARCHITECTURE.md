# Arquitectura del Proyecto

## Visión General

Reporte-MAV es un dashboard web moderno que visualiza datos financieros de operaciones de Cheques y Pagarés en el Mercado Argentino de Valores. La arquitectura sigue un patrón **MVC simplificado** con separación clara de concerns.

## Estructura de Capas

```
┌─────────────────────────────────────┐
│   Presentación (HTML + CSS)         │
│   - index.html (estructura)         │
│   - assets/styles.css (estilos)    │
└────────────┬────────────────────────┘
             │
┌────────────┴────────────────────────┐
│   Aplicación (JavaScript)           │
│   - src/app.js (lógica)            │
│   - Estado global                   │
│   - Event handlers                  │
└────────────┬────────────────────────┘
             │
┌────────────┴────────────────────────┐
│   Datos (Google Sheets CSV)         │
│   - Histórico (GID 220459711)      │
│   - Tiempo Real (GID 405041137)    │
└─────────────────────────────────────┘
```

## Componentes Principales

### 1. Presentación (HTML)

**`index.html`**
- Estructura semántica con dos secciones principales
- Pestaña "Histórico": Análisis de período
- Pestaña "Tiempo Real": Datos del día actual
- Referencias a estilos y scripts externos

### 2. Estilos (CSS)

**`assets/styles.css`**
- Variables CSS para tema consistente
- Componentes reutilizables (cards, buttons, tables)
- Media queries para responsividad básica
- Animaciones (spinner, pulse)
- Grid layout con breakpoints

### 3. Aplicación (JavaScript)

**`src/app.js`**

#### Estado Global
```javascript
// Histórico
let rawData = [];              // Datos crudos del CSV
let filteredData = [];         // Datos filtrados actuales
const F = {...};              // Filtros: monedas, segmento, tipo, fechas
let charts = {};              // Instancias de Chart.js

// Tiempo Real
let rtRawData = [];            // Datos en tiempo real
let rtFilteredData = [];       // Datos RT filtrados
const RT_F = {...};            // Filtros de tiempo real
let rtCharts = {};             // Instancias Chart.js de RT
```

#### Flujo de Datos

```
CSV (Google Sheets)
    ↓ (fetch)
parseCSV()
    ↓
rawData
    ↓ (filtered())
filteredData
    ↓ (applyFilters)
    ├─→ updKPIs()          → KPI values
    ├─→ updTramo()         → Bar chart
    ├─→ updTemporal()      → Line chart
    ├─→ updMontos()        → Column chart
    ├─→ updTable()         → Table rows
    ├─→ updPies()          → Pie charts
    ├─→ updTasaTramo()     → Line chart
    ├─→ updTreemap()       → Treemap
    └─→ updInstrPyme()     → Summary table
```

#### Módulos Funcionales

1. **Carga de datos** (`loadAndRender()`)
   - Fetch desde Google Sheets
   - Parseo CSV
   - Inicialización de filtros
   - Trigger de renderizado

2. **Gestión de filtros** (`applyFilters()`)
   - Filtrado de datos según criterios
   - Actualización de vista
   - Highlight de filtros activos

3. **Visualizaciones**
   - Charts: `updTramo()`, `updTemporal()`, `updMontos()`, `updTasaTramo()`
   - Tables: `updTable()`, `updInstrPyme()`
   - Treemap: `updTreemap()`
   - Pies: `updPies()`

4. **Tiempo Real**
   - Auto-refresh cada 5 minutos
   - Tab switching
   - Filtros independientes

5. **Bubble Chart**
   - Selector multi-empresa
   - Filtros por categoría/segmento
   - Color mapping dinámico

#### Paletas de Colores

```javascript
// Definidas en app.js
const TRAMO_COLORS = { '0-30': '#E32D91', '31-60': '#1A49C8', ... }
const CAT_COLORS = { 'SGR': '#E32D91', 'PyME': '#1A49C8', ... }
const MON_PALETTE = { '$': '#E32D91', 'DOL': '#1A49C8', ... }
const SEG_PALETTE = ['#E32D91', '#1A49C8', '#7B1FAE', ...]
```

## Flujo de Usuario

### Histórico
```
Usuario accede → HTML carga → script inicia → loadAndRender()
                                    ↓
                            Fetch Google Sheets
                                    ↓
                            parseCSV() → rawData
                                    ↓
                            initFilters() [UI]
                                    ↓
                            applyFilters()
                                    ↓
                            Renderizar charts + tablas
                                    ↓
                            Usuario interactúa (filtros/búsqueda)
                                    ↓
                            applyFilters()
                                    ↓
                            Actualizar vista
```

### Tiempo Real
```
Usuario selecciona pestaña "Tiempo Real"
        ↓
switchTab('rt') activa
        ↓
Verifica si rtRawData está vacío
        ↓
Si vacío: loadRT() → fetch → parseCSV
        ↓
rtInitFilters() + rtApplyFilters()
        ↓
Renderizar charts + tablas RT
        ↓
Inicia setInterval para auto-refresh (5 min)
```

## Gestión de Estado

### Estado Local (por vista)
```javascript
// Histórico
F = {
  monedas: [],      // Array de monedas seleccionadas
  segmento: 'ALL',  // Valor del select
  tipo: 'ALL',
  desde: '',        // yyyy-mm-dd
  hasta: ''
}
tblCat = 'ALL';     // Filtro de tabla
tblTramo = 'ALL';   // Filtro por tramo
tblSearch = '';     // Búsqueda por nombre

// Tiempo Real (similar)
RT_F = { ... }
rtTblCat = 'ALL';
```

### Derivados (Computed)
```javascript
filteredData = filtered();  // Resultado de aplicar F
rtFilteredData = rtFiltered();
```

## Librerías Externas

| Librería | Versión | Uso |
|----------|---------|-----|
| Chart.js | 4.4.0 | Gráficos (bar, line, pie, doughnut) |
| D3.js | 7.8.5 | Treemap (volumenes por empresa) |
| chartjs-adapter-date-fns | 3.0.0 | Escalas de tiempo en charts |
| Google Fonts (DM Mono) | Latest | Tipografía monoespaciada |

## Rendimiento

### Optimizaciones
- Charts destruidos antes de recrearlos (evita memory leak)
- Treemap solo se recalcula al resize
- CSV parseado una sola vez al cargar
- Auto-refresh en RT se detiene al cambiar de pestaña

### Límites
- Máximo 60 empresas en treemap
- Máximo 20 empresas en tabla de detalle
- Máximo 18 empresas en bubble chart

## Extensibilidad

### Para agregar un nuevo gráfico
1. Crear función `updNewChart(data)` en `app.js`
2. Agregar canvas en HTML con ID único
3. Llamar función desde `applyFilters()`
4. Destruir chart previo si existe

### Para agregar filtro
1. Agregar propiedad a objeto `F` o `RT_F`
2. Actualizar `initFilters()` o `rtInitFilters()`
3. Modificar función `filtered()` o `rtFiltered()`
4. Llamar `applyFilters()` en evento de cambio

## Testing

No hay suite de tests configurada actualmente. Para testing manual:
1. Abrir DevTools (F12)
2. Inspeccionar estado: `rawData`, `filteredData`, `charts`
3. Verificar requests en tab Network
4. Buscar errores en Console

## Despliegue

### Desarrollo
```bash
python -m http.server 8000
# o
npx http-server
```

### Producción
- Copiar archivos a servidor web estático
- Asegurar CORS habilitado
- CDN seguirá funcionando desde HTML

## Consideraciones de Seguridad

⚠️ **Nota importante**: Este dashboard confía en Google Sheets como fuente de datos. Cualquier persona con el Sheet ID público puede ver los datos.

- No almacena credenciales
- No envía datos al servidor (solo lectura de Sheets)
- Datos públicos en Google (no información sensible)

## Limitaciones Conocidas

- No funciona offline (requiere CDN)
- Datos limitados a lo que existe en Sheets
- Treemap puede ser lento con > 100 empresas
- Mobile: layout no completamente optimizado
