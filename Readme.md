# OPS CENTER — Gestor de Despliegues Clipp

> Documentación técnica y de proyecto convertida desde el archivo HTML original para uso en GitHub.

Documentación integral del sistema construido: arquitectura, módulos, críticas técnicas, deuda acumulada y roadmap de mejoras propuesto.

- **Versión Actual** v5.6 · 08 Abr 2026
- **Estado** En producción (GitHub Pages)
- **Backend** Supabase (PostgreSQL)
- **URL** `vicentequezada-clipp.github.io/ops-center/`
- **Tamaño** ~318 KB · ~5,000 líneas (HTML + JS + CSS monolítico)
- **Funciones JS** 213 funciones

---

# Resumen Ejecutivo

Qué se construyó, por qué existe y qué valor aporta al equipo operativo de Clipp.

| Métrica | Valor |
|---|---|
| Módulos implementados | 13 |
| Clientes cargados | 59+ activos |
| Vistas del sistema | 13 |
| Archivo HTML monolítico | 1 |
| Versión estable actual | v5.6 |

## ¿Qué es OPS Center?

OPS Center es una herramienta de gestión operativa interna construida como una **Single Page Application (SPA) en un único archivo HTML** autocontenido. Fue diseñada para que el equipo de Clipp pueda hacer seguimiento completo del ciclo de vida de sus clientes: desde el primer contacto hasta la operación continua, el cobro mensual y el análisis de capacidad del equipo.

El sistema reemplaza hojas de cálculo Excel dispersas por una interfaz centralizada accesible vía GitHub Pages, con sincronización en tiempo real a través de Supabase.

## Flujo Central del Sistema

```
📋 Prospecto → 🚀 En Despliegue → ✅ Operativo → 💰 Cobranza Mensual
                                  ↕
                          ⚠️ Deshabilitado → ❌ Perdido
```

---

# Changelog de Versiones

## v5.6 · 08 Abr 2026
- **Fix crítico:** `id="view-catalogos"` aparecía como texto visible en el Dashboard por falta de `<div` al inicio del elemento. Todos los 13 views verificados con su `<div` correcto.
- **`renderAfterSave()`:** Nueva función central que re-renderiza automáticamente la vista activa después de cualquier guardado, sin importar desde qué pestaña se dispara. Conectada a: `uD`, `saveCl`, `saveNewDep`, `saveContrato`, `saveCobro`, y todos los saves de catálogos.

## v5.5 · 08 Abr 2026
- **Manual de Uso** completamente reescrito: tabla de referencia datos→reportes, sección de cálculos explicados (KPIs, Capacidad, Cobro, Complejidad, Timeline, Riesgo), 10 tarjetas de módulos actualizadas incluyendo WhatsApp, Bot WA y Backup.
- **Botones (i) con explicaciones** en 7 módulos: Dashboard, Línea de Tiempo, Contratos, Cobranzas, Capacidad Operativa, Estrategia de Despliegue, Usuarios y Roles. Cada botón muestra la fórmula de cálculo y guía de uso.
- **`toggleInfo(id, event)`** reescrita: diccionario `INFO{}` con contenido rico por cada ID, posicionamiento dinámico del popover cerca del botón.

## v5.4 · 07 Abr 2026
- **Filtros por fecha en Dashboard:** todos los KPIs, gráficas y barras respetan el rango seleccionado (antes eran acumulados independientemente del filtro).
- **Filtros rápidos:** botones Mes actual, Mes pasado, Trim. actual, Año actual, Año pasado, Todo. El botón activo se resalta en azul. Al entrar al Dashboard se activa "Mes actual" por defecto.
- **`dashQF(preset)`:** función que calcula el rango de fechas según el preset y actualiza los inputs `#sf` y `#st`.
- **Módulos filtrados por catálogo:** `renderCls` y `depModulesRead` ahora filtran `c.mo[]` contra `cats.md` del usuario. Módulos obsoletos (ej: "App Conductor") no aparecen si no están en el catálogo activo.
- Versión actualizada en sidebar, Manual y TITLES map.

## v5.3 · 07 Abr 2026
- **Fix Supabase Bug #1 (crítico):** `cls` ya no se sobreescribe con los 80 clientes hardcoded de `BC`. La condición era `if(!cls.length || cls.length !== BC.length)` — si el usuario tenía cualquier número ≠ 80, reseteaba. Cambiado a `if(!cls.length)`.
- **Fix Supabase Bug #2 (crítico):** `DATA_VERSION` ya no borra despliegues del usuario. `_needDepsClear` forzado a `false` permanentemente.
- **Fix Supabase Bug #3:** Eliminado `sv()` automático al cargar. El `.then` de `loadFromSupabase` ahora solo actualiza localStorage, no escribe en Supabase con datos potencialmente vacíos.
- **Fix `applyColWidths`:** movida a `setTimeout(50ms)` después del primer render para que las tablas ya existan en el DOM.
- **Funciones de resize agregadas:** `addColResizers(tblEl)`, `applyColWidths(tblEl)` (alias de `applyCW`), `initColResize` con persistencia en `localStorage._colW`.

## v5.2 (sesión anterior)
- **Bot WA en clientes:** campo `cm-bot` en modal (Sin Bot / 🤖 Normal / 🔄 Coexistencia), columna Bot WA en tabla de clientes con badge de color.
- **WhatsApp en clientes:** campo `cm-wh` con múltiples números separados por coma, función `waLinks(wh)` genera links `📱 wa.me/número` en la tabla.
- **Tabla Clientes:** 16 columnas (TH=TD), incluyendo Bot WA y WhatsApp. `saveCl` guarda `wh:[]` y `bot:''`. `editCl` los carga correctamente.
- **`saveContrato` mejorado:** `try/catch` visible, `caEfectivo = max(conductoresIngresados, mínimoDelComponente)`.
- **`calcFactura` reescrita:** maneja correctamente todos los tipos: `conductor`, `vehiculo`, `bot_wa`, `fijo`, `minimo`, `setup`, `deposito`, `modulo`, tipos custom del catálogo.
- **`renderContComps`:** tabla con columna "Fórmula" visible por tipo, `updateCompFields` muestra/oculta campos Mínimo y Cantidad según el tipo.
- **`renderContratos`:** usa mínimos como base del cálculo cuando `conductoresActivos=0` para evitar mostrar `$0/mes`.
- **CSS tablas:** `table-layout:fixed` en todas las tablas (`tbl-d`, `tbl-o`, `tbl-i`, `tbl-c`, `cont-table`). `td` con `overflow:hidden` y `text-overflow:ellipsis`. `.tbl-wrap` con `overflow-x:auto; overflow-y:auto; max-height:75vh` (sin el `;` extra que invalidaba el CSS). Anchos explícitos en thead de Contratos y Cobranzas.
- **Auditoría de catálogos:** todos los 11 catálogos tienen su `save*()` → guarda en `cats.*` → llama `sv()` → re-renderiza su sección.
- **`openAddDep`:** fallback `BASE_MODAL` cuando `cats.modal` está vacío.

---

# Módulos del Sistema

## 13 Vistas Implementadas

| ID | Módulo | Descripción |
|---|---|---|
| `view-dashboard` | 📊 Dashboard Ejecutivo | KPIs filtrados por fecha, gráficas por estado/launcher/país/ciudad/tipo |
| `view-despliegues` | 🚀 Despliegues | Seguimiento operativo, edición inline, fases, horas, notas |
| `view-operativos` | ✅ Clientes Operativos | Implementaciones finalizadas, edición inline |
| `view-inactivos` | ⛔ Clientes Inactivos | Deshabilitados y perdidos |
| `view-clientes` | 👥 Clientes (CRM) | 16 columnas: nombre, país, ciudad, aplicativo, tipo, Bot WA, estado, módulos, WhatsApp, notas |
| `view-timeline` | 📅 Línea de Tiempo | Reporte histórico + proyección SVG, 4 series |
| `view-capacidad` | ⚡ Capacidad Operativa | Carga del equipo, fórmula por complejidad |
| `view-contratos` | 📋 Contratos | Componentes mixtos de cobro, estimado en tiempo real |
| `view-cobranzas` | 💰 Cobranzas | Facturación mensual, estados de pago |
| `view-estrategia` | 🎯 Estrategia Despliegue | Clasificación por complejidad, fases, checklists |
| `view-catalogos` | 🗂 Catálogos | 11 secciones accordion configurables |
| `view-manual` | 📖 Manual de Uso | Guía de referencia completa con cálculos explicados |
| `view-usuarios` | 👤 Usuarios y Roles | RBAC por módulo: Sin acceso / Solo lectura / Editar / Control total |

## Catálogos — 11 Sub-catálogos

| Catálogo (`cats.X`) | Usado en | Save function |
|---|---|---|
| `cats.md` | Módulos → picker cliente/dep, filtro tabla | `saveMd()` |
| `cats.ap` | Aplicativos → modal cliente (`cm-ap`) | `saveAp()` |
| `cats.tp` | Tipos cliente → modal cliente (`cm-tp`) | `saveTp()` |
| `cats.modal` | Modalidades → modal despliegue (`dm-tp`) | `saveModDep()` |
| `cats.launchers` | Launchers → deps, filtros, timeline | `saveLauncher()` |
| `cats.phases` | Fases → badge en filas dep | `savePhase()` |
| `cats.complexity` | Complejidad → badge automático | `saveCx()` |
| `cats.billingTypes` | Tipos de cobro → modal contrato (`comp-tipo`) | `saveBillingType()` |
| `cats.implTimes` | T. Implementación → Capacidad Operativa | `saveImplTime()` |
| `cats.risk` | Estados riesgo → semáforo cobranzas | `saveRisk()` |
| `cats.catmod` | Categorías módulos → filtro en catálogos | `saveCatmod()` |

**Regla:** Cada `save*()` → actualiza `cats.*` → llama `sv()` (Supabase + localStorage) → re-renderiza su tabla + llama `renderAfterSave()` para actualizar la vista activa.

---

# Arquitectura Técnica Actual

## Estructura del Archivo

```
index.html (~318 KB · ~5,000 líneas)
├── <head>
│   ├── Google Fonts (DM Sans, DM Mono)
│   └── <style> bloque 1: variables CSS, sidebar, layout (~6 KB)
│   └── <style> bloque 2: componentes, tablas, modales, badges (~23 KB)
├── <body>
│   ├── Login overlay (#login-overlay)
│   ├── info-popover (#info-pop) — popover de botones (i)
│   ├── mod-popup — picker de módulos
│   ├── Sidebar nav (.sidebar) — 13 items + footer versión
│   ├── Topbar (.topbar) — título + subtítulo + acciones
│   ├── 13 vistas (.view) — ver tabla arriba
│   ├── 10+ modales superpuestos (.modal-ov)
│   └── <script> — ~4,500 líneas JS
│       ├── Constantes BASE_* y datos hardcoded de fallback
│       ├── Estado global: deps[], cls[], cats{}, contratos[], cobros[]
│       ├── Supabase: loadFromSupabase(), subscribeRealtime(), sv()
│       ├── 213 funciones de render, CRUD y helpers
│       └── bootApp() + auto-login
```

## Persistencia de Datos

### Estado global en memoria

```javascript
let deps       = []   // Despliegues
let cls        = []   // Clientes (antes BC[])
let cats       = {    // Catálogos configurables
  md:           [],   // Módulos
  ap:           [],   // Aplicativos
  tp:           [],   // Tipos de cliente
  modal:        [],   // Modalidades de despliegue
  launchers:    [],   // Launchers
  phases:       [],   // Fases
  complexity:   [],   // Niveles de complejidad
  billingTypes: [],   // Tipos de cobro
  implTimes:    [],   // Tiempos de implementación
  risk:         [],   // Estados de riesgo
  catmod:       [],   // Categorías de módulos
}
let contratos  = []
let cobros     = []
let checklists = {}
let trash      = []
let roles      = {}
let usuarios   = []
```

### Claves localStorage

| Clave | Contenido |
|---|---|
| `od27` | `deps[]` — despliegues |
| `oc27` | `cls[]` — clientes |
| `ct27` | `cats{}` — catálogos |
| `ct_v1` | `contratos[]` |
| `cobros_v1` | `cobros[]` |
| `chk_v1` | `checklists{}` |
| `trash_v1` | `trash[]` |
| `roles_v1` | `roles{}` |
| `usuarios_v1` | `usuarios[]` |
| `_colW` | Anchos de columnas por tabla (resize persistente) |
| `data_version` | Versión de esquema (actualmente `v5.2_stable`) |

### Supabase — tabla `ops_data`

```sql
CREATE TABLE ops_data (
  key        TEXT PRIMARY KEY,  -- 'deps', 'cls', 'cats', 'contratos', etc.
  value      JSONB,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

**Regla crítica (v5.3+):** `DATA_VERSION` ya **no borra datos del usuario** al actualizar el archivo. `_needDepsClear` es siempre `false`. El borrado solo ocurre manualmente.

---

# Funciones Clave del Sistema

## Flujo de Guardado

```
Cualquier cambio del usuario
    ↓
save*() / uD() / depSaveEdit()
    ↓
sv() — guarda en localStorage + upsert Supabase async
    ↓
renderAfterSave() — detecta curV y renderiza solo la vista activa
    ↓
updBadges() — actualiza contadores del sidebar
```

## Funciones Críticas

| Función | Rol |
|---|---|
| `bootApp()` | Inicialización: carga localStorage → migra datos → renderiza → sync Supabase |
| `loadFromSupabase()` | Lee `ops_data` de Supabase y actualiza arrays en memoria |
| `subscribeRealtime()` | WebSocket Supabase — actualiza datos y re-renderiza cuando otro usuario guarda |
| `renderAfterSave()` | Re-renderiza la vista activa (`curV`) después de cualquier guardado |
| `sv()` | Guarda todo en localStorage y hace upsert en Supabase |
| `setV(v)` | Cambia de vista: actualiza `curV`, activa la vista, llama el render correspondiente |
| `dashQF(preset)` | Aplica filtros rápidos de fecha en Dashboard (mes actual, año, etc.) |
| `calcFactura(contrato, mes, activos)` | Calcula el total mensual según los componentes del contrato |
| `waLinks(wh)` | Genera links `📱 wa.me/número` para cada número de WhatsApp |
| `toggleInfo(id, event)` | Muestra popover con explicación del módulo, posicionado cerca del botón |
| `initColResize(e)` | Maneja el drag de columnas con persistencia en `localStorage._colW` |
| `renderCls()` | Renderiza tabla de clientes filtrando módulos obsoletos contra `cats.md` |
| `depModulesRead(mods)` | Renderiza módulos de despliegues filtrando contra `cats.md` |

## Modelo de Cobro — `calcFactura()`

```javascript
// Para cada componente del contrato:
if tipo === 'conductor' → max(activos, mínimo) × precio
if tipo === 'vehiculo'  → max(activos, mínimo) × precio
if tipo === 'bot_wa'    → max(bots, mínimo, cantidad) × precio
if tipo === 'fijo'      → precio fijo mensual
if tipo === 'minimo'    → precio fijo (garantía)
if tipo === 'setup'     → cantidad × precio (cobro único)
if tipo === 'deposito'  → cantidad × precio (reembolsable)
if tipo === 'modulo'    → cantidad × precio
if tipo custom          → max(cantidad, mínimo) × precio
```

## Fórmula de Capacidad Operativa

```
Hrs disponibles = Operadores × Hrs/día × Días/mes

Hrs consumidas = Σ (Hrs cliente × Peso complejidad)

Pesos (configurables en Catálogos → Complejidad):
  Básico   = 1.0×
  Medio    = 1.5×
  Avanzado = 2.5×

Saturación = (Hrs consumidas / Hrs disponibles) × 100%
Semáforo: Verde <70% | Ámbar 70–90% | Rojo >90%
```

## Series de la Línea de Tiempo

| Serie | Fuente del dato |
|---|---|
| 🔵 Confirmados | `c.fconf` — F. Confirmación del cliente en ese mes |
| 🟢 Finalizados | `d.fe` — F. Entrega del despliegue en ese mes (estado ✅) |
| 🟡 En Proceso | `d.fi ≤ mes` y sin `d.fe` o `d.fe ≥ mes` |
| 🟣 Operativos | Acumulado de despliegues con `d.fe ≤ mes` |

Las líneas punteadas son proyección por regresión lineal sobre datos reales.

---

# Estructura de Datos

## Cliente (`cls[]`)

```javascript
{
  id:    1,                          // Número entero autoincremental
  nm:    "UNITAXI",                  // Nombre
  pa:    "Ecuador",                  // País
  ci:    "Quito",                    // Ciudad
  ap:    "Clipp",                    // Aplicativo (de cats.ap)
  tp:    "Taxi",                     // Tipo (de cats.tp)
  es:    "activo",                   // Estado: activo | deshabilitado | perdido
  mo:    ["Bot WA Normal", "Counter"], // Módulos activos (de cats.md)
  co:    "Juan Pérez",               // Contacto
  ph:    "+593998651939",            // Tel / Email
  wh:    ["+593987654321"],          // WhatsApp (array, genera links wa.me)
  bot:   "normal",                   // Bot WA: "" | "normal" | "coexistencia"
  no:    "Observaciones...",         // Notas
  fconf: "2025-01-15",              // Fecha confirmación → Línea de Tiempo
}
```

## Despliegue (`deps[]`)

```javascript
{
  id:     1,
  cid:    5,                         // ID del cliente vinculado
  c:      "UNITAXI",                 // Nombre cliente
  pa:     "Ecuador",
  ci:     "Quito",
  tp:     "Cliente Nuevo",           // Modalidad (de cats.modal)
  es:     "✅ Listo",                // Estado
  fase:   3,                         // Índice de fase (0-based, de cats.phases)
  ln:     "Pablo Malla",             // Launcher (de cats.launchers)
  mo:     ["Bot WA Normal"],         // Módulos
  fi:     "2025-01-10",             // F. Inicio
  fd:     "2025-01-25",             // F. Ideal
  fe:     "2025-02-01",             // F. Entrega real
  pe:     "Pendientes...",           // Notas de pendientes
  no:     "Notas...",
  lk:     "https://...",             // Link adjunto
  hrsOp:  20,                        // Hrs/mes operativo (para Capacidad)
  hrsNew: 30,                        // Hrs/mes en despliegue
}
```

## Contrato (`contratos[]`)

```javascript
{
  id:                  1,
  clienteId:           5,
  cliente:             "UNITAXI",
  modalidad:           "componentes",        // o "conductor" (legacy)
  componentes: [
    { tipo: "conductor", nombre: "Por Conductor", precio: 15, minimo: 12, cantidad: 1 },
    { tipo: "bot_wa",    nombre: "Bot WA",        precio: 30, minimo: 1,  cantidad: 1 },
    { tipo: "setup",     nombre: "Setup",         precio: 500, minimo: 0, cantidad: 1 },
  ],
  conductoresActivos:  15,
  vehiculosActivos:    0,
  botsActivos:         1,
  es:                  "activo",
  nota:                "Condiciones especiales...",
  descuentos:          [],
}
```

---

# CSS — Reglas Clave de Tablas

```css
/* Todas las tablas principales usan layout fixed para respetar anchos */
#tbl-d, #tbl-o, #tbl-i, #tbl-c { table-layout: fixed; }
.cont-table                      { table-layout: fixed; }

/* Celdas cortan texto con ellipsis */
td { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

/* Excepciones: celdas que SÍ deben wrap */
.cl-mods-ro, .dep-modules { white-space: normal; overflow: visible; }
.cont-table td, .cat-table td { white-space: normal; overflow: visible; }

/* Contenedor de tablas con scroll */
.tbl-wrap { overflow-x: auto; overflow-y: auto; max-height: 75vh; }
```

**Anchos definidos en thead** (respetados por `table-layout:fixed`):
- Clientes: 16 columnas, de 40px (#) a 220px (Módulos)
- Contratos: 8 columnas, de 44px (#) a 260px (Tipos de Cobro)
- Cobranzas: 9 columnas definidas

**Resize de columnas persistente:** el usuario arrastra el handle `.col-r` en cualquier `<th>`. El ancho se guarda en `localStorage._colW` como `{tblId_colIndex: "150px"}` y se restaura en cada render.

---

# Mapa de Dependencias entre Módulos

| Módulo | Lee de | Escribe en | Afecta a |
|---|---|---|---|
| **Clientes** | `cats.ap`, `cats.tp`, `cats.md` | `cls[]` | Despliegues, Cobranzas, Contratos, Dashboard, Capacidad |
| **Despliegues** | `cls[]`, `cats.*` | `deps[]` | Dashboard, Timeline, Capacidad, Estrategia |
| **Catálogos** | — | `cats{}` | Todos los módulos |
| **Contratos** | `cls[]`, `cats.billingTypes` | `contratos[]` | Cobranzas |
| **Cobranzas** | `contratos[]`, `cls[]` | `cobros[]` | Dashboard |
| **Capacidad** | `deps[]`, `cls[]`, `cats.complexity` | Config local | Estrategia |
| **Timeline** | `cls[].fconf`, `deps[]` (fechas) | Solo lectura | Reportes |
| **Estrategia** | `deps[]`, `cats.phases` | `dep.fase` | Despliegues |
| **Dashboard** | Todo (filtrado por fechas) | Solo lectura | — |

---

# Bugs Resueltos (historial completo)

| Versión | Bug | Causa Raíz | Solución |
|---|---|---|---|
| v5.3 | Supabase no cargaba datos de clientes | `cls.length !== BC.length` sobreescribía con hardcoded | `if(!cls.length)` como única condición |
| v5.3 | Despliegues se borraban al actualizar archivo | `DATA_VERSION` nuevo → `_needDepsClear=true` → borraba Supabase | `_needDepsClear=false` permanente |
| v5.3 | `sv()` automático sobreescribía Supabase con vacíos | `.then` llamaba `sv()` con `deps=[]` | Solo actualiza localStorage al cargar |
| v5.3 | `applyColWidths` fallaba al boot | Se llamaba antes de que las tablas existieran en el DOM | `setTimeout(50ms)` después del render |
| v5.3 | `addColResizers is not defined` | Función llamada pero no definida | Definida como función real + alias |
| v5.4 | Módulos obsoletos aparecían en tabla | `c.mo[]` no se filtraba contra `cats.md` | `_validMods = new Set(cats.md.map(m=>m.name))` |
| v5.4 | Dashboard KPIs no respetaban filtro de fechas | `allF` ignoraba `from`/`to` para los contadores | `filtered` con rango de fechas aplicado a todos los KPIs |
| v5.5 | Formato del modal de contratos | Campos Mínimo/Cantidad siempre visibles | `updateCompFields()` muestra/oculta según tipo |
| v5.5 | Contratos mostraban `$0/mes` | `conductoresActivos=0` → `0×precio=0` | `caEfectivo = max(activos, mínimoDelComponente)` |
| v5.6 | Catálogos se mezclaban con Dashboard | `<div` faltante antes de `id="view-catalogos"` | Insertado `<div` en la posición correcta |
| v5.6 | Cambios en catálogos no se reflejaban sin recargar | Cada `save*()` solo renderizaba su propia sección | `renderAfterSave()` central conectada a todos los saves |
| v5.6 | `uD()` no re-renderizaba si no estabas en la vista correcta | `if(curV==='despliegues')renderDeps()` — hardcodeado | Reemplazado por `renderAfterSave()` |

## Problemas Conocidos Pendientes

| Prioridad | Issue | Impacto |
|---|---|---|
| 🔴 CRÍTICO | Concurrencia sin control — last-write-wins | Pérdida de datos con uso simultáneo |
| 🟡 ALTO | Sin paginación en tablas | Lentitud con 100+ registros |
| 🟡 ALTO | Sin validación de formularios consistente | Datos corruptos posibles |
| 🟢 MEDIO | Fases de estrategia no 100% sincronizadas bidireccionalmente | Edge cases en botones ▶/◀ |

---

# 🔬 Crítica Arquitectónica

## Lo que funciona bien

- **Catálogos configurables:** separar configuración del código es correcto. Cualquier cambio de reglas no requiere tocar JS.
- **Doble persistencia (localStorage + Supabase):** funciona offline, sincroniza al reconectar.
- **Modelo de cobro por componentes:** flexible, refleja la realidad del negocio.
- **`renderAfterSave()` (nuevo v5.6):** resuelve el problema de vistas desactualizadas de forma eficiente sin re-renderizar todo.
- **Resize de columnas persistente:** UX mejorada significativamente.
- **Filtros de fecha en Dashboard:** todos los KPIs y gráficas respetan el período seleccionado.

## Deuda técnica activa

| Área | Deuda | Esfuerzo | Impacto si no se resuelve |
|---|---|---|---|
| **Modelo de datos** | JSONB sin normalización en Supabase | 🔴 4–6 días | Escalabilidad bloqueada |
| **Arquitectura** | Monolítico 318KB, sin separación | 🔴 2–3 semanas | Imposible mantener en equipo |
| **Concurrencia** | Last-write-wins total | 🟡 2–3 días | Pérdida de datos en uso simultáneo |
| **Validación** | Sin validación consistente de formularios | 🟢 1–2 días | Datos corruptos en BD |
| **Testing** | Cero cobertura | 🔴 Proceso continuo | Regresiones al iterar |
| **Paginación** | Sin límite de filas | 🟢 1 día | Lentitud con 200+ registros |

---

# 🗺️ Roadmap Propuesto

## FASE 1 — Estabilizar (4 semanas)

- **P1** Soft deletes en todas las entidades (`deleted_at` timestamp)
- **P1** Exportar/Importar JSON completo del sistema (ya existe botón Backup/Restaurar)
- **P1** Validación de formularios: campos requeridos, fechas consistentes
- **P2** Fix de concurrencia: versión de timestamp por registro
- **P2** Roles refinados por módulo (ya implementado básico, refinar permisos)

## FASE 2 — Refactorizar (6–8 semanas)

- Separar en archivos: `app.js`, `modules/deps.js`, `modules/clients.js`, etc.
- Implementar Store centralizado event-driven
- Actualización granular (solo elemento modificado, no toda la tabla)
- Tests unitarios para `calcFactura()`, complejidad, capacidad
- Paginación de tablas (50 filas por página)
- Audit trail: quién cambió qué y cuándo

## FASE 3 — Crecer (ongoing)

- Reportes PDF por cliente
- PWA para uso móvil
- Integración WhatsApp API para notificaciones de cobro
- Multi-empresa

---

# 🌐 Despliegue & Operaciones

## Stack

- **Frontend:** GitHub Pages — `vicentequezada-clipp.github.io/ops-center/`
- **Backend:** Supabase — `owfetilyxgvkrtdopskz.supabase.co` (São Paulo)
- **Auth:** Supabase Auth con sesión persistente (`clipp_ops_session`)

## Actualizar el sistema

1. Descargar `index.html` actual desde GitHub
2. Editar (o reemplazar con nueva versión)
3. En GitHub → `index.html` → ✏️ → pegar → **Commit changes**
4. Esperar 1–2 min → recargar con `Ctrl+Shift+R`

## Gestión de usuarios Supabase

| Acción | Ruta |
|---|---|
| Crear usuario | Authentication → Users → Add user → Create new user |
| Resetear contraseña | Authentication → Users → Send recovery email |
| Ver datos | Table Editor → ops_data |
| SQL directo | SQL Editor |

## Backup manual (recomendado semanal)

Supabase → Table Editor → `ops_data` → Export as CSV → guardar en Google Drive.

El botón **Backup** dentro de la app exporta un JSON completo del estado actual. **Restaurar** importa ese JSON. Hacerlo antes de cualquier actualización mayor.
