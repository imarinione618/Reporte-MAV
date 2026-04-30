# Dashboard MAV – Cheques y Pagarés

Dashboard interactivo para análisis de datos de Cheques y Pagarés del Mercado Argentino de Valores (MAV).

## Descripción

Este dashboard proporciona visualizaciones en tiempo real y análisis históricos de operaciones de cheques y pagarés, incluyendo:

- **Histórico**: Análisis de período completo con filtros por moneda, segmento, tipo de instrumento y fechas
- **Tiempo Real**: Datos actualizados del día actual con evolución intraday
- **Gráficos**: Tasas ponderadas, evolución temporal, volúmenes por empresa, treemap de categorías
- **Tablas**: Detalle de empresas por categoría y tramo
- **Bubble Chart**: Curva de tasas por plazo con opciones de filtrado

## Características

✨ **Visualizaciones avanzadas**
- Gráficos interactivos con Chart.js
- Treemap dinámico con D3.js
- Bubble chart para análisis multidimensional

🎯 **Filtros inteligentes**
- Moneda (multi-select)
- Segmento (garantizado/no garantizado)
- Tipo de instrumento
- Rango de fechas
- Categoría y tramo (por empresa)

📊 **KPIs destacados**
- Tasa ponderada
- Actividad (operaciones/días hábiles)
- Montos totales
- Última operación en tiempo real

## Estructura del Proyecto

```
reporte-mav/
├── index.html              # HTML principal
├── src/
│   └── app.js             # Lógica de aplicación
├── assets/
│   └── styles.css         # Estilos
├── docs/                  # Documentación
├── package.json           # Metadatos del proyecto
├── .gitignore             # Git ignore
└── README.md              # Este archivo
```

## Requisitos

- Navegador web moderno (Chrome, Firefox, Safari, Edge)
- Conexión a Internet (para CDN de librerías)
- Acceso a Google Sheets (para datos)

## Instalación y Uso

### Desarrollo local

```bash
# Con Python 3
python -m http.server 8000

# O con Node.js/npm
npx http-server -p 8000 -c-1
```

Luego abrir `http://localhost:8000` en el navegador.

### En producción

1. Deployment en GitHub Pages, Vercel, o cualquier servidor web estático
2. Asegurar que el servidor sirva archivos con CORS habilitado
3. Los datos se obtienen automáticamente de Google Sheets

## Dependencias

### CDN (Cliente)
- **Chart.js 4.4.0**: Gráficos
- **D3.js 7.8.5**: Visualizaciones avanzadas (treemap)
- **chartjs-adapter-date-fns**: Adapter para fechas en Chart.js
- **Google Fonts**: DM Mono (tipografía)

### Development
- **Node.js**: Para herramientas de desarrollo (opcional)
- **http-server**: Servidor local (opcional)

## Fuente de Datos

Los datos provienen de un Google Sheet publicado en formato CSV:
- **Sheet ID**: 11r7oJ9mm4-IUHszK6LkTAFZOzFUe0RZBEWgP7ZzwvvE
- **Hojas de datos**:
  - GID 220459711: Datos históricos
  - GID 405041137: Datos en tiempo real

### Estructura de Datos

Cada registro contiene:
- FEC.SUB: Fecha de suscripción
- T.MIN: Tiempo (minuto)
- SEGMENTO: Garantizado / No Garantizado
- TIPO INSTRUMENTO: Cheque / Pagaré / FCE
- TASA C: Tasa de compra (%)
- MONEDA: Moneda ($ / DOL / U$D / etc)
- MONTO: Monto de la operación
- NOMBRE RESPONSABLE: Empresa
- CATEGORIA: Categoría de riesgo (SGR / PyME / Banco / etc)
- TRAMO: Rango de plazo (0-30 / 31-60 / etc)

## Estilo y Paleta de Colores

### Colores principales (one618)
- **Primario**: #1A49C8 (azul)
- **Secundario**: #E32D91 (rosa)
- **Complementario**: #7B1FAE (violeta)
- **Neutro**: #B2B2B2 (gris)

### Paletas por dimensión
- **Tramos**: Colores específicos por rango de plazo
- **Categorías**: Colores asignados a cada tipo de empresa
- **Monedas**: Colores por tipo de moneda

## Performance

- Carga lazy de datos (bajo demanda)
- Charts destruidos y recreados al filtrar
- Debouncing en búsquedas
- Actualización automática cada 5 minutos en tiempo real

## Navegación

### Pestaña "Histórico"
1. Seleccionar filtros
2. Ver KPIs y evolución temporal
3. Explorar empresas por categoría/tramo
4. Visualizar distribuciones con gráficos circulares

### Pestaña "Tiempo Real"
1. Datos del día actual se cargan automáticamente
2. Gráficos intraday (por hora)
3. Tabla de operaciones con búsqueda
4. Bubble chart con filtros avanzados

## Desarrollo

### Agregar nuevas funcionalidades

1. **Nuevos gráficos**: Agregar funciones `updXXX()` en `src/app.js`
2. **Nuevos filtros**: Modificar funciones de filtrado en estado `F` y `RT_F`
3. **Nuevos estilos**: Actualizar `assets/styles.css` (variables CSS en `:root`)

### Debugging

- Console browser: F12 para ver errores y logs
- Estado global disponible en console: `rawData`, `filteredData`, `charts`, etc.
- Activar modo debug descomentar logs en app.js

## Mejoras futuras

- [ ] Exportación de datos (CSV/Excel)
- [ ] Alertas por umbrales
- [ ] Comparación período a período
- [ ] Análisis de tendencias
- [ ] Dark mode
- [ ] Responsivo mobile optimizado
- [ ] PWA para instalación

## Contacto y Soporte

Consultatio Asset Management FCI

## Licencia

MIT - Ver LICENSE para más detalles.

---

**Última actualización**: Abril 2026
**Versión**: 1.0.0
