# Changelog

Todos los cambios notables a este proyecto serán documentados en este archivo.

El formato se basa en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/) y este proyecto adhiere a [Semantic Versioning](https://semver.org/es/).

## [1.0.0] - 2026-04-30

### ✨ Added

#### Estructura de Proyecto
- Separación de HTML, CSS y JavaScript en archivos independientes
- Carpeta `src/` para lógica de aplicación
- Carpeta `assets/` para estilos
- Carpeta `docs/` para documentación
- Archivos de configuración: `package.json`, `.gitignore`

#### Visualizaciones
- **Histórico Tab**
  - KPIs: Tasa ponderada, actividad del período
  - Gráfico de barras: Tasa ponderada por tramo
  - Gráfico de líneas: Evolución temporal de tasas
  - Gráfico de montos: Negociados por día
  - Tabla: Tasas y montos por empresa (top 20)
  - Tabla: Resumen por instrumento (CPD/PAG/FCE)
  - Gráficos circulares: Por instrumento, moneda, segmento
  - Treemap: Volúmenes por empresa

- **Tiempo Real Tab**
  - KPIs: Tasa ponderada, actividad del día, última operación
  - Gráficos intraday: Por hora
  - Tabla: Operaciones del día con búsqueda
  - Bubble chart: Curva de tasas por plazo

#### Filtros Inteligentes
- Moneda: Multi-select (todas/individual)
- Segmento: Garantizado / No Garantizado / Todos
- Tipo de instrumento: Todos / CPD / PAG / FCE
- Rango de fechas: Desde/Hasta
- Categoría de empresa: SGR / PyME / Banco / Gran empresa
- Tramo de plazo: 0-30 / 31-60 / 61-90 / etc.
- Búsqueda por nombre de empresa

#### Funcionalidades
- Auto-refresh de datos en tiempo real (cada 5 minutos)
- Tab navigation entre Histórico y Tiempo Real
- Highlight de filtros activos
- Validación de datos y manejo de errores
- Loading spinner con mensajes descriptivos
- Error boxes con opción de reintentar

#### Documentación
- README.md con descripción general
- ARCHITECTURE.md con detalles técnicos
- CONTRIBUTING.md con guía de contribución
- Este CHANGELOG.md

#### Librerías y Dependencias
- Chart.js 4.4.0 para gráficos interactivos
- D3.js 7.8.5 para treemap
- Google Fonts (DM Mono) para tipografía
- Integración con Google Sheets como fuente de datos

### 🎨 Design
- Paleta de colores one618 (azul, rosa, violeta, gris)
- Responsive layout con CSS Grid y Flexbox
- Dark-friendly UI con colores accesibles
- Animaciones suaves (spinner, pulse badge)
- Variables CSS para consistencia de temas

### ⚙️ Infrastructure
- Git repository initialized
- .gitignore configurado
- package.json para metadatos del proyecto
- Setup para desarrollo local

### 🔧 Technical Details
- CSV parser custom para datos de Google Sheets
- State management pattern para datos filtrados
- Chart lifecycle management (destroy/recreate)
- Multi-select pattern para monedas
- Virtual filter options (Garantizado Total, No Garantizado Total)

---

## Futuro

### Planeado para v1.1
- [ ] Exportación a Excel/CSV
- [ ] Comparación período a período
- [ ] Análisis de tendencias
- [ ] PWA (Progressive Web App)

### Planeado para v1.2
- [ ] Dark mode
- [ ] Responsivo mobile optimizado
- [ ] Alertas por umbrales
- [ ] Análisis de volatilidad

### Planeado para v2.0
- [ ] Backend REST API
- [ ] Autenticación de usuarios
- [ ] Almacenamiento de preferencias
- [ ] Reportes programados por email
- [ ] Suite de tests automatizados

---

## Formato de Cambios

Cada sección utiliza los siguientes keywords:
- **Added**: Nuevas funcionalidades
- **Changed**: Cambios en funcionalidad existente
- **Deprecated**: Funcionalidad que será removida pronto
- **Removed**: Funcionalidad removida
- **Fixed**: Bug fixes
- **Security**: Vulnerabilidades

---

_Actualizado: 30 de Abril de 2026_
