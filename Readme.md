# OPS CENTER — Gestor de Despliegues Clipp

> Documentación técnica y de proyecto convertida desde el archivo HTML original para uso en GitHub.

Documentación integral del sistema construido: arquitectura, módulos, críticas técnicas, deuda acumulada y roadmap de mejoras propuesto.

- **Versión Actual** 1.0 — Monolítico HTML

- **Estado** En producción (GitHub Pages)

- **Backend** Supabase (PostgreSQL)

- **Iteraciones** 60+ en sesión de desarrollo

- **Líneas de código** ~5,000+ (HTML+JS+CSS)

# Resumen Ejecutivo

Qué se construyó, por qué existe y qué valor aporta al equipo operativo de Clipp.

12

Módulos implementados

73

Clientes cargados

60+

Iteraciones de desarrollo

1

Archivo HTML monolítico

## ¿Qué es OPS Center?

OPS Center es una herramienta de gestión operativa interna construida como una **Single Page Application (SPA) en un único archivo HTML** autocontenido. Fue diseñada para que el equipo de Clipp pueda hacer seguimiento completo del ciclo de vida de sus clientes: desde el primer contacto (prospecto) hasta la operación continua, el cobro mensual y el análisis de capacidad del equipo.

  

El sistema reemplaza hojas de cálculo Excel dispersas (DESPLIEGUES MARZO 2026, SEMÁFORO DE CLIENTES) por una interfaz centralizada accesible en línea vía GitHub Pages, con sincronización en tiempo real a través de Supabase.

## Flujo Central del Sistema

Un cliente transita por los siguientes estados, y el sistema acompaña cada transición:

📋 Prospecto
 → 
🚀 En Despliegue
 → 
✅ Operativo
 → 
💰 Cobranza Mensual

✅ Operativo
 → 
⚠️ Deshabilitado
 → 
❌ Perdido

## Módulos Principales

### 📊 Dashboard Ejecutivo

KPIs en tiempo real, gráficos por estado, país, ciudad, modalidad. Filtros por fecha y launcher.

### 🚀 Seguimiento de Despliegues

Seguimiento operativo completo: estado, fechas, módulos, fases, complejidad, horas, notas, adjuntos.

### 👥 Gestión de Clientes

CRM ligero con contacto, aplicativo, tipo, módulos, complejidad y fecha de confirmación.

### 📅 Línea de Tiempo

Reporte histórico y proyección mensual de confirmados, despliegues y operativos con gráfico SVG.

### ⚡ Capacidad Operativa

Cálculo de carga del equipo por operador, horas individuales por cliente y peso de complejidad.

### 📋 Contratos

Contratos por componente mixto: setup, mínimo, por conductor/vehículo, bot WA, fijo. Descuentos por mes.

### 💰 Cobranzas

Registro mensual de cobros, estados de pago, desglose por componente, edición por cobro individual.

### 🎯 Estrategia de Despliegue

Clasificación por complejidad, checklists por módulo, fases de implementación conectadas al despliegue.

### 🗂️ Catálogos

CRUD de módulos, aplicativos, tipos, modalidades, fases, complejidad, riesgos, tiempos y tipos de cobro.

## Tecnologías Utilizadas

### Frontend

HTML5
CSS3 Variables
Vanilla JavaScript ES6+
SVG Charts
localStorage

### Backend / Infraestructura

Supabase (PostgreSQL)
Supabase Auth
Supabase Realtime
GitHub Pages

# Módulos del Sistema

Descripción funcional detallada de cada módulo, sus entradas, salidas y dependencias.

## Mapa de Dependencias entre Módulos

| Módulo | Lee de | Escribe en | Afecta a |
| --- | --- | --- | --- |
| ** Clientes** | Catálogos (módulos, aplicativos) | BC[ ] — array de clientes | Despliegues, Cobranzas, Contratos, Dashboard, Capacidad |
| ** Despliegues** | Clientes, Catálogos | dep[ ] — array de despliegues | Dashboard, Línea de Tiempo, Capacidad, Estrategia |
| ** Catálogos** | — | cats{ } — objeto catálogos | Todos los módulos |
| ** Contratos** | Clientes, Catálogos (tipos de cobro) | contratos[ ] | Cobranzas, Dashboard |
| ** Cobranzas** | Contratos, Clientes Operativos | cobros[ ] | Dashboard, Línea de Tiempo |
| ** Capacidad** | Despliegues, Clientes Operativos, Catálogos (complejidad) | Config operativa | Estrategia de Despliegue |
| ** Línea de Tiempo** | Clientes (fconf), Despliegues (fechas) | Solo lectura | Reportes ejecutivos |
| ** Estrategia** | Despliegues, Catálogos (fases, tiempos) | dep[].fase | Despliegues |

## Catálogos (Fuente de Verdad de Configuración)

Los catálogos son el corazón del sistema. Cualquier cambio aquí impacta en toda la aplicación. Son 9 sub-catálogos:

CONFIGURACIÓN OPERATIVA

Módulos

Categorías de Módulos

Aplicativos

Tipos de Cliente

Modalidades de Despliegue

IMPLEMENTACIÓN

Fases de Implementación

T. de Implementación

Niveles de Complejidad

COBROS & RIESGO

Tipos de Cobro

Estados de Riesgo

## Módulo: Cobranzas — Lógica de Facturación

El modelo de cobro es ** por componentes mixtos**. Un contrato puede combinar:

| Tipo de Cobro | Lógica | Periodicidad |
| --- | --- | --- |
| `setup` | Pago único al inicio del contrato | Una vez |
| `deposito` | Depósito reembolsable | Una vez |
| `fijo` | Monto fijo sin importar uso | Mensual |
| `minimo` | Garantía mínima (cobra diferencia si activos < mínimo) | Mensual |
| `conductor` | Precio × N° conductores activos ese mes | Mensual |
| `vehiculo` | Precio × N° vehículos activos ese mes | Mensual |
| `bot_wa` | Precio × N° bots WhatsApp en coexistencia | Mensual |
| `modulo` | Cobro por módulo adicional activo | Mensual |

** Regla clave:** Al generar cobros de un mes, el operador ingresa cuántos conductores, vehículos y bots tuvo cada cliente ese mes. El sistema calcula el total en tiempo real antes de confirmar.

## Módulo: Capacidad Operativa — Fórmula

```
Hrs Disponibles = Operadores × Hrs/día × Días/mes

Hrs Consumidas = Σ(cliente_operativo × hrsOp_individual)
               + Σ(despliegue_activo × hrsNew_individual)

Si no tiene horas individuales:
  hrsOp = hrs_global × peso_complejidad
  hrsNew = hrs_despliegue_global × peso_complejidad

Peso Complejidad (configurable en Catálogos):
  Básico   = 1.0×
  Medio    = 1.5×
  Avanzado = 2.5×

Saturación = (Hrs Consumidas / Hrs Disponibles) × 100%
Semáforo: Verde < 70% | Ámbar 70-90% | Rojo > 90%
```

# Arquitectura Técnica Actual

Descripción de cómo está construido el sistema hoy — sin filtros.

## Estructura del Sistema

```
gestor_despliegues.html  (~5,000+ líneas)
├── <head>
│   ├── CDN: Supabase JS (cargado condicionalmente)
│   └── <style> — CSS completo embebido (~600 líneas)
├── <body>
│   ├── Login overlay
│   ├── Sidebar nav
│   ├── 12 vistas (.view panels)
│   │   ├── #v-dash (Dashboard)
│   │   ├── #v-timeline (Línea de Tiempo)
│   │   ├── #v-deps (Despliegues)
│   │   ├── #v-cls (Clientes)
│   │   ├── #v-ops (Clientes Operativos)
│   │   ├── #v-inactivos (Clientes Inactivos)
│   │   ├── #v-capac (Capacidad Operativa)
│   │   ├── #v-contratos (Contratos)
│   │   ├── #v-cobros (Cobranzas)
│   │   ├── #v-estrategia (Estrategia)
│   │   ├── #v-cats (Catálogos)
│   │   └── #v-manual (Manual)
│   ├── 8+ modales superpuestos
│   └── <script> — JS completo embebido (~4,000+ líneas)
│       ├── Constantes y estado global
│       ├── Supabase integration
│       ├── sv() / ld() — save/load localStorage+Supabase
│       ├── Funciones de render por vista (12+)
│       ├── Funciones de CRUD
│       ├── Helpers (complejidad, riesgo, badges)
│       └── INIT — inicialización y migración de datos
```

## Persistencia de Datos

### 📦 Estado en Memoria (JS Global)

```
let dep = []       // Despliegues
let BC  = []       // Clientes (Base Clientes)
let contratos = [] // Contratos
let cobros = []    // Registros de cobro
let cats = {       // Catálogos
  mds: [],         // Módulos
  apts: [],        // Aplicativos
  tipos: [],       // Tipos de cliente
  modDeps: [],     // Modalidades
  phases: [],      // Fases
  implTimes: [],   // T. Implementación
  complexity: [],  // Niveles complejidad
  risk: [],        // Estados de riesgo
  billingTypes: [], // Tipos de cobro
  mcat: []         // Categorías de módulos
}
```

### 💾 Estrategia de Persistencia

Doble capa de guardado:

#### localStorage (primera capa)

Guarda inmediatamente. Funciona offline. Límite 5-10MB.

#### Supabase (segunda capa)

Serializa todo el estado en una tabla `ops_data` con key-value JSONB. Sincronización vía WebSocket + polling 10s.

#### Modelo de datos plano

Todo en una sola fila Supabase por key. No normalizado. Ver sección de riesgos.

## Patrón de Render

Cada módulo sigue el mismo patrón de renderizado imperativo:

```
// Patrón usado en los 12 módulos
function renderDeps() {
  const filtered = dep.filter(/* filtros activos */);
  document.querySelector('#deps-tbody').innerHTML = 
    filtered.map(d => depRow(d)).join('');
  updBadges(); // actualiza contadores en sidebar
}

// Cada cambio (inline edit, dropdown change) llama:
function saveVal(id, field, val) {
  const d = dep.find(x => x.id === id);
  d[field] = val;
  sv();           // guarda en localStorage + Supabase
  renderDeps();   // re-renderiza toda la tabla
}
```

**Problema:** Cada cambio re-renderiza TODA la tabla (no solo el elemento modificado). Con 73+ clientes y tablas complejas, esto genera parpadeos y puede ser lento en dispositivos lentos.

# 🔬 Crítica Arquitectónica

Análisis como arquitecto de software senior. Sin suavizar — esto es lo que necesitas saber para escalar el sistema.

🔴

#### CRÍTICO: Archivo monolítico de 5,000+ líneas

Todo el sistema — HTML, CSS y JavaScript — está en un solo archivo. Esto hace imposible el trabajo paralelo en equipo, las pruebas unitarias, el control de versiones significativo (un solo commit cambia todo), y el debugging preciso. Un error de sintaxis en cualquier línea puede romper silenciosamente todo el sistema, como ocurrió múltiples veces durante el desarrollo (template literals rotos, `let cobCharts` duplicado, `linReg` declarada dos veces).

🔴

#### CRÍTICO: Modelo de datos no normalizado en Supabase

Toda la data se guarda como un blob JSONB en una tabla `ops_data` con key-value. Esto significa: sin joins, sin queries eficientes, sin indexación, sin integridad referencial. Si tienes 500 clientes y 3,000 cobros, deserializar todo en cada sync es ineficiente. Además, Supabase Realtime notifica el cambio del registro completo, no solo el campo modificado — transmitiendo megabytes innecesariamente.

🔴

#### CRÍTICO: Sin control de concurrencia

Si dos usuarios editan simultáneamente, el último en guardar gana (last-write-wins total). No hay bloqueo optimista, no hay merge de cambios, no hay notificación de conflicto. Usuario A edita cliente X → guarda. Usuario B también editó cliente X → guarda → sobrescribe los cambios de A sin aviso. En un equipo de 5+ personas esto causará pérdida de datos.

🟡

#### ALTO: Re-render total en cada cambio

La función `saveVal()` llama a `sv()` (guarda todo) y luego `renderDeps()` (re-renderiza toda la tabla). Cambiar una celda = reconstruir 73+ filas DOM. En tablas grandes con columnas editables, esto produce parpadeos y puede perder el foco del input activo.

🟡

#### ALTO: Variables globales como estado compartido

Todo el estado vive en variables globales JS (`dep`, `BC`, `contratos`, `cats`). No hay patrón de estado (Redux, Zustand, Context). Cualquier función puede mutar cualquier dato sin que el sistema lo sepa. Esto causó múltiples bugs de sincronización durante el desarrollo (módulos que no se actualizaban, datos que se "perdían" al cambiar de vista).

🟡

#### ALTO: Sin manejo de errores estructurado

No hay un sistema global de manejo de errores. Los errores de Supabase se capturan localmente en algunos casos, pero no hay logging, no hay UI de error consistente, y no hay fallback inteligente. Un fallo de red durante un save puede corromper el estado local sin que el usuario lo sepa.

🟢

#### MEDIO: Autenticación básica

El sistema usa Supabase Auth correctamente para login, pero no tiene roles (admin vs. operador vs. solo lectura). Cualquier usuario autenticado puede editar cualquier dato, incluyendo catálogos críticos o eliminar clientes. Para un equipo de trabajo real, necesitas RBAC (Role-Based Access Control).

🟢

#### MEDIO: Falta de validación de datos

No hay validación de formularios consistente. Se puede guardar un contrato sin cliente, un cobro sin monto, o una fecha de entrega anterior a la de inicio. La integridad de los datos depende completamente del operador.

## Lo que SÍ está bien hecho

#### Arquitectura de catálogos configurable

Separar los datos de configuración (módulos, fases, complejidad) del código es una decisión correcta. Permite cambios de reglas sin tocar código.

#### Doble capa de persistencia (localStorage + Supabase)

Funciona offline y sincroniza cuando hay red. El patrón es válido para aplicaciones con equipos pequeños.

#### Migración automática de datos

El bloque INIT que migra estructuras antiguas al nuevo formato es una práctica correcta para mantener retrocompatibilidad.

#### SVG charts como alternativa a Chart.js

Reemplazar una librería con problemas de timing por SVG puro fue la decisión correcta para el contexto de un SPA de un solo archivo.

#### Modelo de cobro flexible por componentes

El sistema de tipos de cobro mezclables (setup + mínimo + por conductor + bot WA) es elegante y refleja bien la realidad del negocio.

# 📋 Crítica de Gestión de Proyecto

Análisis como PMO / Project Manager senior. El foco está en proceso, scope, deuda acumulada y eficiencia del equipo.

## Diagnóstico del Proceso

🔴

#### Desarrollo iterativo sin backlog formal

El proyecto se desarrolló mediante conversación directa sin documentación previa de requerimientos, sin casos de uso, sin criterios de aceptación. Cada mensaje fue un nuevo requerimiento o un reporte de bug. Esto generó: scope creep constante, retrabajo, features incompletas que se dejaron a medias, y bugs introducidos al agregar nuevas funcionalidades sin regresión.

🔴

#### Múltiples "continuar" — síntoma de falta de diseño previo

El chat registra al menos 12 mensajes de "Continuar" indicando que las sesiones quedaron incompletas. Cada "continuar" introdujo riesgo de romper lo ya construido. El desarrollo se interrumpió en estados intermedios varias veces, requiriendo debugging adicional al retomar. ** En un proyecto real esto equivale a deployments a producción con código a medio terminar.**

🟡

#### Scope creep no controlado

El proyecto pasó de "organizar despliegues del Excel" a un sistema con 12 módulos, CRM, billing, análisis de capacidad, estrategia de despliegue, y sincronización en tiempo real. Sin priorización, se perdió tiempo en features complejas (cobros por componente, línea de tiempo con proyección) mientras funcionalidades básicas seguían con bugs (botón Nuevo Despliegue, gráficos que no renderizaban).

🟡

#### Sin ambiente de testing / staging

Todos los cambios se aplicaron directamente sobre el archivo de producción. No hay un ambiente de staging, no hay tests, no hay validación antes de publicar en GitHub Pages. El equipo ha estado usando el sistema mientras se modificaba activamente, con riesgo de pérdida de datos.

🟡

#### Features incompletas en producción

Varios módulos quedaron parcialmente implementados: la lógica de fases de estrategia no avanzaba correctamente, el botón "Nuevo Contrato" abría la vista de despliegue, la línea de tiempo no generaba gráficas por múltiples sesiones. Estas regresiones, en un software de uso del equipo, erosionan la confianza en la herramienta.

## Estimación de Deuda Técnica Acumulada

| Área | Deuda | Esfuerzo para resolver | Impacto si no se resuelve |
| --- | --- | --- | --- |
| ** Modelo de datos** | Sin normalización en Supabase | 🔴 4–6 días | Escalabilidad bloqueada |
| ** Arquitectura** | Monolítico, sin separación | 🔴 2–3 semanas | Imposible mantener en equipo |
| ** Concurrencia** | Last-write-wins total | 🟡 2–3 días | Pérdida de datos en uso simultáneo |
| ** Validación** | Sin validación de formularios | 🟢 1–2 días | Datos corruptos en BD |
| ** Roles** | Sin control de acceso por rol | 🟡 1–2 días | Cualquiera puede borrar catálogos |
| ** Testing** | Cero cobertura | 🔴 Proceso continuo | Regresiones constantes al iterar |

## Lo que se hizo bien desde PM

### ✅ Claridad de visión del negocio

El usuario siempre tuvo claro qué necesitaba operativamente. Los requerimientos de negocio fueron precisos aunque no estructurados formalmente.

### ✅ Datos reales cargados

Se trabajó con datos reales desde el inicio (73 clientes del Excel), lo que permite validar el sistema con casos de uso reales.

### ✅ Decisión de Supabase correcta

Elegir Supabase para multiusuario en tiempo real fue la decisión correcta para el presupuesto y la escala del proyecto.

### ✅ Manual de uso incluido

Agregar un módulo de manual dentro de la propia herramienta facilita el onboarding del equipo sin documentación externa.

# 🐛 Bugs Identificados & Deuda Técnica

Registro de bugs documentados en el chat y problemas técnicos conocidos.

## Bugs Críticos Resueltos (historial)

| Bug | Causa Raíz | Solución Aplicada |
| --- | --- | --- |
| Ningún módulo cargaba | Template literal roto en `TIPOS_DEP().map()` — la comilla cerraba antes | Corrección del template literal. Se implementó migración automática en INIT. |
| Gráfico de línea de tiempo vacío | `linReg()` declarada dos veces (dentro y fuera de `renderTimeline`). En JS esto causa SyntaxError silencioso. | Eliminada la declaración duplicada. Función movida fuera del scope. |
| Chart.js no renderizaba | Canvas tenía tamaño 0×0 al estar en contenedor `display:none`. Chart.js no puede medir el canvas. | Reemplazo de Chart.js por SVG puro generado con `innerHTML`. |
| Todo el script se rompía con `let cobCharts` | Variable declarada con `let` dos veces en el mismo scope | Eliminada la declaración duplicada. |
| Botón "Nuevo Contrato" abría vista de Despliegue | Conflicto de IDs entre el modal de contrato y el modal de despliegue | Separación de modales y corrección de `openAddContrato()`. |
| Login no funcionaba en modo online | Librería Supabase CDN con formato `sb_publishable_...` incompatible con versión JS importada | Cambio al anon key legacy (`eyJ...`) y actualización del CDN. |
| Columnas Pendientes y Hrs/mes invertidas | El orden en `depTH` y `depRow` no coincidía | Swap de celdas en `depRow`. |

## Problemas Conocidos Actuales (pendientes)

🔴

#### Concurrencia sin manejo de conflictos

Dos usuarios editando al mismo tiempo: last-write-wins. No hay merge, no hay notificación de conflicto.

🟡

#### Fases de estrategia no avanzan correctamente en todos los casos

Los botones ▶/◀ en despliegues y en estrategia no están 100% sincronizados bidireccionalemnte en todos los paths de código.

🟡

#### Pérdida de foco en inputs inline al guardar

Al editar una celda inline y guardar, el re-render total de la tabla quita el foco del input. En tablas largas el usuario pierde contexto visual.

🟢

#### Sin paginación en tablas

Con 73+ clientes y futuras adiciones, las tablas sin paginación serán lentas. No hay limit/offset.

# ⚠️ Matriz de Riesgos

Riesgos actuales del sistema ordenados por impacto y probabilidad.

#### 🔴 CRÍTICO — Pérdida de datos por concurrencia

** Prob:** Alta (uso diario multi-usuario)  
** Impacto:** Datos de clientes y cobros sobrescritos sin aviso  
** Mitigación:** Implementar versionado optimista o bloqueo por registro en Supabase

#### 🔴 CRÍTICO — Corrupción por límite localStorage

** Prob:** Media (crecimiento de datos)  
** Impacto:** La app falla silenciosamente cuando localStorage llega a ~5MB  
** Mitigación:** Eliminar localStorage como persistencia primaria; Supabase como única fuente de verdad

#### 🟡 ALTO — Un bug de JS rompe toda la app

** Prob:** Alta (al agregar features)  
** Impacto:** La app queda completamente inutilizable  
** Mitigación:** Separar en módulos, agregar error boundaries, testing básico

#### 🟡 ALTO — Sin backup ni versionado de datos

** Prob:** Media  
** Impacto:** Un delete masivo accidental es irrecuperable  
** Mitigación:** Soft deletes, backup diario de Supabase, botón "Exportar JSON" completo

#### 🟢 MEDIO — Supabase free tier limitaciones

** Prob:** Alta (crecimiento)  
** Impacto:** Supabase free pausa proyectos inactivos 7 días, límite 500MB DB  
** Mitigación:** Upgrade a plan Pro ($25/mes) cuando el equipo lo use diariamente

#### 🔵 BAJO — Dependencia de GitHub Pages

** Prob:** Baja (servicio estable)  
** Impacto:** App no disponible si GitHub tiene outage  
** Mitigación:** Netlify como alternativa, el HTML funciona también localmente

## Plan de Contingencia Inmediato

#### Esta semana: Exportar respaldo manual

Desde Supabase → Table Editor → exportar como CSV. Hacerlo semanalmente hasta tener backup automático.

#### Esta semana: Agregar botón "Exportar JSON completo"

Un botón que descargue todo el estado del sistema como JSON. Esto permite restauración manual en caso de pérdida.

#### Próximas 2 semanas: Soft deletes

En lugar de borrar registros, marcarlos como `deleted: true`. Permite recuperación ante errores.

# 🗺️ Roadmap Propuesto

Plan de evolución en 3 fases, priorizando estabilidad antes de nuevas features.

** Principio guía:** Estabilizar antes de crecer. La deuda técnica actual es alta. Agregar features sobre una base inestable aumentará el costo exponencialmente. La Fase 1 es no negociable.

### FASE 1 — Estabilizar (4 semanas)

** P1 — Semana 1-2**Modelo de datos normalizado en Supabase (tablas reales, no key-value JSONB)

** P1 — Semana 1-2**Eliminar localStorage como persistencia primaria. Solo Supabase.

** P1 — Semana 1**Soft deletes en todas las entidades (deleted\_at timestamp)

** P1 — Semana 1**Exportar/Importar JSON completo del sistema

** P2 — Semana 2-3**Validación de formularios (campos requeridos, fechas consistentes)

** P2 — Semana 3-4**Roles básicos: Admin / Operador / Solo lectura

** P2 — Semana 3-4**Fix de concurrencia: versión de timestamp por registro

### FASE 2 — Refactorizar (6-8 semanas)

** Arquitectura**Separar en archivos: app.js, modules/deps.js, modules/clients.js, etc.

** Estado**Implementar patrón Store centralizado (simple event-driven)

** Render**Actualización granular (solo el elemento modificado, no toda la tabla)

** Testing**Tests unitarios para lógica de negocio (calcFactura, complejidad, capacidad)

** Paginación**Virtualización de tablas largas (50 filas por página)

** Logging**Audit trail: quién cambió qué y cuándo

** Notificaciones**Alertas por clientes en riesgo, cobros vencidos, despliegues sin avance

### FASE 3 — Crecer (ongoing)

** Reportes PDF**Exportar estados de cuenta por cliente, reporte ejecutivo mensual

** Integraciones**WhatsApp API para notificaciones de cobro, email automático de estado

** App móvil**PWA para uso desde celular (especialmente útil para el equipo en campo)

** Analytics avanzado**LTV por cliente, churn rate, NPS, predicción de renovaciones

** Facturación**Integración con sistema de facturación (Facturae, SRI Ecuador)

** Multi-empresa**Soporte para múltiples instancias de Clipp / Azutaxo

# ⚡ Arquitectura Ideal

Cómo debería verse el sistema a largo plazo para ser mantenible, escalable y seguro.

## Modelo de Datos Normalizado (Supabase)

```
-- ENTIDADES PRINCIPALES
clientes          (id, nombre, ciudad, pais, aplicativo_id, tipo_id, estado, fconf, ...)
despliegues       (id, cliente_id, launcher_id, estado, fase_id, modalidad_id, f_inicio, ...)
contratos         (id, cliente_id, estado, vigente_desde, ...)
contrato_items    (id, contrato_id, tipo_cobro_id, precio, minimo, ...)
cobros_mes        (id, contrato_id, mes, estado, total_calculado, total_pagado, ...)
cobro_items       (id, cobro_mes_id, tipo_cobro_id, cantidad, precio_unitario, subtotal)
cobro_conductores (id, cobro_mes_id, conductores, vehiculos, bots)

-- CATÁLOGOS (tablas lookup)
modulos           (id, nombre, categoria_id, descripcion)
categorias_mod    (id, nombre, icono, color)
aplicativos       (id, nombre, descripcion, color)
tipos_cliente     (id, nombre, descripcion, aplicativo_id)
modalidades_dep   (id, nombre, icono, descripcion)
fases             (id, nombre, orden, descripcion)
complejidad       (id, nombre, icono, color, peso, modulos_trigger[])
tipos_cobro       (id, nombre, tipo_base, descripcion)
tiempos_impl      (id, nombre, semanas, color)
estados_riesgo    (id, nombre, icono, color, umbral_desde)

-- SEGURIDAD
users             (id, email, rol: 'admin'|'operador'|'viewer')
audit_log         (id, user_id, tabla, accion, before_json, after_json, timestamp)
```

## Estructura de Archivos Propuesta

```
ops-center/
├── index.html              (shell mínimo, ~50 líneas)
├── src/
│   ├── app.js              (bootstrap, router, store init)
│   ├── store.js            (estado global con eventos)
│   ├── supabase.js         (cliente y sync)
│   ├── auth.js             (login, logout, roles)
│   ├── modules/
│   │   ├── dashboard.js
│   │   ├── clients.js
│   │   ├── deployments.js
│   │   ├── contracts.js
│   │   ├── billing.js
│   │   ├── capacity.js
│   │   ├── timeline.js
│   │   ├── strategy.js
│   │   └── catalogs.js
│   ├── components/
│   │   ├── table.js        (tabla genérica reutilizable)
│   │   ├── modal.js        (sistema de modales)
│   │   ├── chart-svg.js    (gráficos SVG reutilizables)
│   │   └── badges.js       (badges de estado)
│   └── utils/
│       ├── dates.js
│       ├── calculations.js (calcFactura, complejidad, capacidad)
│       └── validators.js
├── styles/
│   ├── main.css
│   └── components.css
└── tests/
    ├── calculations.test.js
    └── validators.test.js
```

## Patrón de Estado Propuesto

```
// store.js — Event-driven simple state
const store = {
  state: { clients: [], deployments: [], contracts: [], billing: [], cats: {} },
  listeners: {},

  on(event, fn) { (this.listeners[event] ||= []).push(fn); },

  dispatch(event, payload) {
    this.state = reducer(this.state, event, payload);
    (this.listeners[event] || []).forEach(fn => fn(this.state));
    (this.listeners['*'] || []).forEach(fn => fn(this.state));
    syncToSupabase(event, payload); // async, no bloquea render
  }
};

// Cada módulo solo escucha lo que necesita:
store.on('client:updated', (state) => renderClientRow(state.clients));
store.on('deployment:phase_changed', (state) => renderStrategyView(state));
```

# 🔄 Flujos de Estado Propuestos

Estados correctos, transiciones válidas y dependencias entre módulos.

## Estados de Cliente

📋 prospecto
 → 
🚀 en\_despliegue
 → 
✅ operativo

↔

⚠️ pausado

❌ perdido

←

⚠️ pausado

| Transición | Trigger | Efecto en otros módulos |
| --- | --- | --- |
| prospecto → en\_despliegue | Crear nuevo despliegue para el cliente | Aparece en tabla Despliegues. Suma a Capacidad como "cliente nuevo". |
| en\_despliegue → operativo | Cambiar estado del despliegue a "Listo" | Sale de Despliegues. Aparece en Clientes Operativos. Disponible en Cobranzas. |
| operativo → pausado | Cambiar estado manualmente | Sale de Cobranzas activa. Pasa a Clientes Inactivos. No se generan cobros. |
| pausado → perdido | Cambiar estado manualmente | Se marca en rojo. Requiere nota de motivo (recomendado). |

## Estados de Despliegue

por\_iniciar
 → 
en\_proceso
 → 
capacitacion
 → 
listo

**Regla clave:** Solo cuando el despliegue pasa a "listo", el cliente migra automáticamente a Clientes Operativos. Los demás estados no deben disparar esta migración.

## Estados de Cobro Mensual

generado
 → 
pendiente
 → 
pagado

pendiente
 → 
atrasado
 → 
parcial
 → 
pagado

** Regla de riesgo:** Si un cobro pasa a "atrasado" el sistema debe marcar automáticamente el cliente con el badge de riesgo correspondiente del catálogo.

## Transiciones entre Módulos — Diagrama de Dependencia

```
CATÁLOGOS ──────────────────────────────────────────────────────┐
    │                                                           │
    ↓ (módulos, complejidad, fases, tipos)                      │
CLIENTES ──────────────────────────────────────────────────────►│
    │                                                           │
    ↓ (cliente seleccionado → hereda módulos)                   │
DESPLIEGUES ────────────────────────────────────────────────────►│
    │                         │                                 │
    ↓ (estado=listo)          ↓ (fase, horas)                  │
CLIENTES OPERATIVOS    ESTRATEGIA DE DESPLIEGUE                │
    │                         │                                 │
    ↓                         ↓                                 │
CONTRATOS ──────────────────────────────────────────────────────►│
    │                                                           │
    ↓ (componentes de cobro)                                    │
COBRANZAS                                                       │
    │                                                           │
    ↓ (todos los datos anteriores)                              │
DASHBOARD + LÍNEA DE TIEMPO + CAPACIDAD OPERATIVA ◄─────────────┘
```

# 💾 Modelo de Datos Actual

Estructura de datos JavaScript usada actualmente en el sistema.

## Estructura de un Cliente (BC[])

```
{
  id: "cl_1234567890",       // ID único timestamp
  n:  "Cabify Ecuador",       // Nombre
  ci: "Quito",                // Ciudad
  pa: "Ecuador",              // País
  ap: "Clipp",                // Aplicativo (del catálogo)
  dt: "Clipp",                // Tipo de cliente (del catálogo)
  ms: ["Bot WA Normal", "App Conductor Clipp"],  // Módulos activos
  co: "Juan Pérez",           // Contacto nombre
  ph: "593998651939",         // Teléfono
  no: "Cliente desde 2024",   // Notas
  fconf: "2024-01-15",        // Fecha confirmación del cliente
  st: "activo",               // Estado: activo | deshabilitado | perdido
}
```

## Estructura de un Despliegue (dep[])

```
{
  id: "dep_1234567890",
  cl: "Cabify Ecuador",       // Nombre del cliente (sincronizado)
  ci: "Quito",                // Ciudad
  pa: "Ecuador",              // País
  tp: "Cliente Nuevo",        // Modalidad de despliegue (catálogo)
  st: "en_proceso",           // Estado: por_iniciar | en_proceso | capacitacion | listo
  ln: "Vicente Quezada",      // Launcher (nombre completo)
  fi: "2026-01-10",           // Fecha inicio
  fd: "2026-01-25",           // Fecha ideal de entrega
  fe: "2026-02-01",           // Fecha entrega real
  ms: ["Bot WA Normal"],      // Módulos
  pe: "Falta configurar bot", // Pendientes
  no: "Cliente difícil",      // Notas
  lk: "https://drive.google.com/...", // Link adjunto
  fase: 2,                    // Índice de fase actual (0-based)
  hrsOp: 20,                  // Horas/mes cuando es operativo (null = global)
  hrsNew: 30,                 // Horas/mes en despliegue (null = global)
  clientId: "cl_1234567890",  // Referencia al cliente
}
```

## Estructura de un Contrato (contratos[])

```
{
  id: "cont_1234567890",
  cliente: "Cabify Ecuador",
  clientId: "cl_1234567890",
  items: [
    { tipo: "setup",     label: "Setup inicial",    precio: 500, minimo: 0 },
    { tipo: "conductor", label: "Por conductor",    precio: 15,  minimo: 20 },
    { tipo: "bot_wa",    label: "Bot WA adicional", precio: 30,  minimo: 0 },
    { tipo: "minimo",    label: "Mínimo mensual",   precio: 300, minimo: 0 },
  ],
  modelo: "estandar",         // Modelo de activo: conservador | estandar | agresivo
  estado: "activo",
  descuentos: [               // Descuentos variables por mes
    { mes: "2026-01", pct: 50, nota: "Descuento lanzamiento" },
    { mes: "2026-02", pct: 25, nota: "Descuento mes 2" },
  ],
}
```

## Tabla Supabase (ops\_data) — Esquema actual

```
-- Tabla única para toda la data del sistema
CREATE TABLE ops_data (
  key        TEXT PRIMARY KEY,    -- ej: 'deps', 'clients', 'cats', 'contratos'
  value      JSONB,               -- Array o objeto completo serializado
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- RLS: Solo usuarios autenticados
ALTER TABLE ops_data ENABLE ROW LEVEL SECURITY;
CREATE POLICY "auth_users_all" ON ops_data
  FOR ALL USING (auth.role() = 'authenticated');

-- Realtime habilitado para sync
ALTER PUBLICATION supabase_realtime ADD TABLE ops_data;
```

** ⚠️ Problema:** Este esquema serializa arrays completos en un solo campo JSONB. No permite queries SQL eficientes ni integridad referencial. Funcional para el equipo actual, pero debe migrarse en Fase 2.

# 🌐 Despliegue & Operaciones

Infraestructura actual, acceso, y guía de actualización.

## Stack de Infraestructura

### Frontend — GitHub Pages

URL: `vicentequezada-clipp.github.io/ops-center/`

#### Hosting gratuito

GitHub Pages es gratis para repos públicos

#### CDN global

Latencia baja desde cualquier ubicación

#### Solo archivos estáticos

No puede ejecutar código servidor. Toda la lógica es client-side.

### Backend — Supabase

Proyecto: `owfetilyxgvkrtdopskz.supabase.co`

#### PostgreSQL gestionado

Base de datos en São Paulo (baja latencia para Ecuador)

#### Auth + Realtime

Usuarios autenticados, sync en tiempo real via WebSocket

#### Free tier

Pausa proyectos inactivos por 7 días. Límite 500MB. Upgradar cuando el uso sea diario.

## Guía de Actualización del Sistema

#### Paso 1: Descargar el archivo actual

En GitHub: `github.com/vicentequezada-clipp/ops-center` → click en `index.html` → botón de descarga

#### Paso 2: Editar localmente

Abrir con VS Code o editor de texto. Hacer los cambios necesarios. Probar abriendo el archivo en Chrome (modo demo funciona localmente).

#### Paso 3: Subir a GitHub

En GitHub → click en `index.html` → ícono de lápiz ✏️ → borrar todo → pegar nuevo código → ** Commit changes**

#### Paso 4: Esperar deploy automático

GitHub Pages se actualiza en ~1-2 minutos. Recargar la URL con Ctrl+Shift+R para evitar caché.

## Gestión de Usuarios en Supabase

| Acción | Ruta en Supabase | Detalle |
| --- | --- | --- |
| Crear usuario | Authentication → Users → Add user → Create new user | Marcar "Auto Confirm User". Compartir credenciales por canal seguro. |
| Resetear contraseña | Authentication → Users → click usuario → Send recovery email | O "Update password" directamente. |
| Eliminar usuario | Authentication → Users → click usuario → Delete | Los datos del sistema NO se eliminan — solo el acceso. |
| Ver datos BD | Table Editor → ops\_data | Ver y exportar manualmente como CSV. |
| SQL directo | SQL Editor | Para queries de emergencia o migración. |

## Recomendaciones Inmediatas

### 🔒 Cambiar contraseñas iniciales

Si todos los usuarios tienen la misma contraseña inicial (ej: Clipp2026!), solicitar cambio inmediato desde Authentication → Send recovery email.

### 💾 Backup semanal manual

Cada semana: Table Editor → ops\_data → exportar CSV. Guardar en Google Drive. Hasta tener backup automático.

### ⬆️ Upgrade Supabase Pro

Cuando el equipo use la app diariamente, upgradar a Pro ($25/mes) para evitar pausas por inactividad y tener backups automáticos.

### 📋 Documento de requerimientos

Antes de la siguiente ronda de desarrollo, documentar requerimientos formalmente con criterios de aceptación para evitar retrabajo.