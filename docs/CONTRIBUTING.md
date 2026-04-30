# Guía de Contribución

¡Gracias por tu interés en contribuir a Reporte-MAV! 

Este documento proporciona pautas y procedimientos para contribuir al proyecto.

## Código de Conducta

- Sé respetuoso y profesional
- Asume buenas intenciones
- Enfócate en lo que es mejor para la comunidad

## Cómo Contribuir

### 1. Reportar Bugs

Si encuentras un bug, por favor:

1. **Verifica que no esté reportado** buscando en issues existentes
2. **Describe el bug** con:
   - Título claro y descriptivo
   - Descripción detallada
   - Pasos para reproducir
   - Comportamiento esperado vs actual
   - Capturas de pantalla si aplica
   - Información del navegador/SO

Ejemplo:
```
Título: Tabla de empresas no se actualiza al cambiar filtro de tramo

Descripción:
Al cambiar el filtro de tramo en la tabla "Tasas y Montos por Empresa",
la tabla no se actualiza aunque los datos deberían cambiar.

Pasos:
1. Ir a pestaña Histórico
2. Hacer clic en botón "Todos" en Tramo
3. Hacer clic en "31-60 días"
4. Observar que tabla sigue mostrando los mismos datos

Navegador: Chrome 125
SO: Windows 11
```

### 2. Sugerir Mejoras

Para sugerir una mejora:

1. Usa un título descriptivo: "Mejorar: [descripción]"
2. Proporciona descripción detallada de la mejora
3. Explica por qué sería útil
4. Lista ejemplos/mockups si es visual

Ejemplo:
```
Mejorar: Agregar modo oscuro

Descripción:
Implementar tema oscuro para reducir fatiga visual durante análisis extendido.

Beneficios:
- Reduce fatiga ocular
- Mejor usabilidad en ambiente oscuro
- Tendencia moderna en apps financieras
```

### 3. Pull Requests

#### Antes de empezar
1. Fork el repositorio
2. Crea rama: `git checkout -b feature/tu-feature` o `bugfix/tu-bug`
3. Instala dependencias si necesario: `npm install`

#### Durante el desarrollo
1. Escribe código limpio y bien comentado
2. Mantén commits atómicos y descriptivos
3. Prueba en navegadores modernos (Chrome, Firefox, Safari)
4. Verifica que no romps funcionalidad existente

#### Antes de hacer PR
1. Pull últimos cambios de main: `git pull origin main`
2. Rebase si necesario: `git rebase main`
3. Asegúrate que los cambios funcionan localmente

#### Estructura del PR
```markdown
## Descripción
Breve descripción de qué cambia y por qué.

## Tipo de cambio
- [ ] Bug fix
- [ ] Nueva funcionalidad
- [ ] Mejora de rendimiento
- [ ] Cambio de estilo/UI

## Testing
Describe cómo probaste los cambios:
- Browser(s) probado(s)
- Pasos para reproducir/verificar

## Checklist
- [ ] Mi código sigue el estilo del proyecto
- [ ] He probado en al menos 2 navegadores
- [ ] He actualizado documentación
- [ ] No he agregado dependencias innecesarias
```

## Guías de Estilo

### JavaScript
```javascript
// ✅ DO
function fetchData() {
  // Comentarios claros
  const results = data.filter(item => item.active);
  return results;
}

// ❌ DON'T
function fd() {
  var r=data.filter(i=>i.a);return r; // Sin espacios, nombres cortos
}

// Variables descriptivas
const tramoColors = { '0-30': '#E32D91' };  // ✅ 
const tc = { '0-30': '#E32D91' };           // ❌

// Usa const por defecto
const x = 5;   // ✅
let x = 5;     // ❌ solo si necesitas reasignar
```

### CSS
```css
/* ✅ DO */
.card-header {
  display: flex;
  align-items: center;
  gap: 8px; /* spacing */
}

/* ❌ DON'T */
.card-header { display: flex; align-items: center; gap: 8px; }

/* Usa variables CSS */
color: var(--pri);      /* ✅ */
color: #1A49C8;         /* ❌ duplica definición */

/* Mobile-first */
@media (min-width: 1200px) {
  /* cambios para desktop */
}
```

### HTML
```html
<!-- ✅ DO -->
<div class="card">
  <h2 class="card-title">Título</h2>
  <p class="card-description">Descripción</p>
</div>

<!-- ❌ DON'T -->
<div class="card">
  <h2>Título</h2>
  <p>Descripción</p>
</div>
```

## Áreas de Contribución Prioritarias

1. **Responsive Design**: Mejorar layout mobile
2. **Accesibilidad**: ARIA labels, keyboard navigation
3. **Documentación**: Guías de uso, ejemplos
4. **Performance**: Optimizar rendering de charts
5. **Testing**: Agregar suite de tests
6. **Internacionalización**: Soporte múltiples idiomas

## Proceso de Revisión

1. Revisor lee y comenta el código
2. Se sugieren cambios si necesario
3. Autor actualiza PR
4. Al aprobar, se mergea a main

## Convenciones de Commit

```
<tipo>(<alcance>): <descripción corta>

<descripción detallada si es necesario>

<referencias a issues>
```

Tipos válidos: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`

Ejemplos:
```
feat(bubble-chart): Agregar selector de empresas múltiple

fix(filters): Corregir bug en filtro de segmento

docs(readme): Actualizar instrucciones de instalación

style(css): Ajustar spacing en cards
```

## Configuración Local

### Prerequisites
- Git
- Navegador web moderno
- (Opcional) Python 3 o Node.js

### Setup
```bash
# 1. Fork y clone
git clone https://github.com/TU_USUARIO/reporte-mav.git
cd reporte-mav

# 2. Crea rama
git checkout -b feature/tu-feature

# 3. Inicia servidor local
python -m http.server 8000
# o
npx http-server

# 4. Abre http://localhost:8000
```

## Preguntas?

- Lee [README.md](../README.md) para info general
- Ve [ARCHITECTURE.md](./ARCHITECTURE.md) para entender el código
- Abre una issue con etiqueta `question`
- Contacta a: consultatio@example.com

---

¡Agradecemos tu contribución! 🙏
