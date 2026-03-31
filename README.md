<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>OPS CENTER — Documentación del Proyecto</title>
<style>
  :root {
    --navy: #0f1f3d;
    --blue: #1a3a6b;
    --accent: #2563eb;
    --accent2: #10b981;
    --warn: #f59e0b;
    --danger: #ef4444;
    --light: #f8fafc;
    --border: #e2e8f0;
    --text: #1e293b;
    --muted: #64748b;
    --card: #ffffff;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Segoe UI', system-ui, sans-serif; background: var(--light); color: var(--text); line-height: 1.6; }

  /* SIDEBAR */
  .sidebar {
    position: fixed; left: 0; top: 0; bottom: 0; width: 260px;
    background: var(--navy); color: #e2e8f0; overflow-y: auto; z-index: 100;
    padding: 0 0 40px;
  }
  .sidebar-logo {
    padding: 24px 20px 20px;
    border-bottom: 1px solid rgba(255,255,255,0.08);
    margin-bottom: 8px;
  }
  .sidebar-logo h2 { font-size: 15px; font-weight: 700; color: #fff; letter-spacing: 1px; }
  .sidebar-logo p { font-size: 11px; color: #94a3b8; margin-top: 2px; }
  .sidebar-section { padding: 8px 16px 4px; font-size: 10px; font-weight: 700; color: #475569; letter-spacing: 1.5px; text-transform: uppercase; margin-top: 12px; }
  .sidebar a {
    display: flex; align-items: center; gap: 10px; padding: 9px 20px;
    color: #94a3b8; text-decoration: none; font-size: 13px; border-radius: 0;
    transition: all .15s; cursor: pointer; border-left: 3px solid transparent;
  }
  .sidebar a:hover, .sidebar a.active {
    background: rgba(255,255,255,0.07); color: #fff;
    border-left-color: var(--accent);
  }
  .sidebar a .icon { width: 18px; text-align: center; font-size: 14px; }
  .badge-nav {
    margin-left: auto; background: var(--accent); color: #fff;
    font-size: 10px; font-weight: 700; padding: 2px 6px; border-radius: 10px;
  }
  .badge-warn { background: var(--warn); color: #000; }
  .badge-ok { background: var(--accent2); }

  /* MAIN */
  .main { margin-left: 260px; min-height: 100vh; }
  .view { display: none; padding: 40px 48px; max-width: 1100px; }
  .view.active { display: block; }

  /* HEADER */
  .doc-header {
    background: linear-gradient(135deg, var(--navy) 0%, #1e3a5f 100%);
    color: white; padding: 60px 48px 48px; margin-left: 260px;
  }
  .doc-header .tag { background: var(--accent); color: white; font-size: 11px; font-weight: 700; padding: 4px 12px; border-radius: 20px; letter-spacing: 1px; display: inline-block; margin-bottom: 16px; }
  .doc-header h1 { font-size: 36px; font-weight: 800; margin-bottom: 8px; }
  .doc-header p { color: #94a3b8; font-size: 15px; max-width: 600px; }
  .doc-header .meta { display: flex; gap: 32px; margin-top: 24px; }
  .doc-header .meta-item { font-size: 12px; color: #64748b; }
  .doc-header .meta-item strong { display: block; font-size: 14px; color: #cbd5e1; }

  /* CARDS */
  h1.section-title { font-size: 26px; font-weight: 800; margin-bottom: 6px; color: var(--navy); }
  h2.sub-title { font-size: 18px; font-weight: 700; margin: 28px 0 12px; color: var(--blue); padding-bottom: 6px; border-bottom: 2px solid var(--border); }
  h3.card-title { font-size: 14px; font-weight: 700; margin-bottom: 8px; color: var(--text); }
  .section-desc { color: var(--muted); font-size: 14px; margin-bottom: 28px; }

  .card { background: var(--card); border: 1px solid var(--border); border-radius: 12px; padding: 24px; margin-bottom: 16px; }
  .card-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 16px; margin-bottom: 24px; }
  .card-sm { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 18px; }

  .kpi-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin-bottom: 28px; }
  .kpi { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 20px; text-align: center; }
  .kpi .num { font-size: 32px; font-weight: 800; color: var(--accent); }
  .kpi .label { font-size: 12px; color: var(--muted); margin-top: 4px; }

  /* TAGS & BADGES */
  .tag { display: inline-block; font-size: 11px; font-weight: 600; padding: 3px 10px; border-radius: 20px; margin: 2px; }
  .tag-blue { background: #dbeafe; color: #1d4ed8; }
  .tag-green { background: #d1fae5; color: #065f46; }
  .tag-amber { background: #fef3c7; color: #92400e; }
  .tag-red { background: #fee2e2; color: #991b1b; }
  .tag-purple { background: #ede9fe; color: #5b21b6; }
  .tag-gray { background: #f1f5f9; color: #475569; }

  /* STATUS PILLS */
  .status { display: inline-flex; align-items: center; gap: 6px; font-size: 12px; font-weight: 600; padding: 4px 12px; border-radius: 20px; }
  .s-ok { background: #d1fae5; color: #065f46; }
  .s-warn { background: #fef3c7; color: #92400e; }
  .s-error { background: #fee2e2; color: #991b1b; }
  .s-info { background: #dbeafe; color: #1d4ed8; }
  .s-gray { background: #f1f5f9; color: #475569; }

  /* SEVERITY ROWS */
  .sev-row { display: flex; align-items: flex-start; gap: 16px; padding: 16px; border-radius: 8px; margin-bottom: 12px; }
  .sev-high { background: #fff5f5; border-left: 4px solid var(--danger); }
  .sev-med { background: #fffbeb; border-left: 4px solid var(--warn); }
  .sev-low { background: #f0fdf4; border-left: 4px solid var(--accent2); }
  .sev-icon { font-size: 20px; flex-shrink: 0; }
  .sev-content h4 { font-size: 14px; font-weight: 700; margin-bottom: 4px; }
  .sev-content p { font-size: 13px; color: var(--muted); }
  .sev-content code { background: #f1f5f9; padding: 2px 6px; border-radius: 4px; font-size: 12px; font-family: monospace; }

  /* MODULES TABLE */
  table { width: 100%; border-collapse: collapse; font-size: 13px; margin-bottom: 20px; }
  thead th { background: var(--navy); color: white; padding: 10px 14px; text-align: left; font-size: 12px; font-weight: 600; letter-spacing: 0.5px; }
  tbody td { padding: 10px 14px; border-bottom: 1px solid var(--border); vertical-align: top; }
  tbody tr:hover { background: #f8fafc; }
  tbody tr:last-child td { border-bottom: none; }

  /* FLOW DIAGRAM */
  .flow { display: flex; align-items: center; flex-wrap: wrap; gap: 6px; margin: 16px 0; }
  .flow-box { background: #dbeafe; border: 1px solid #93c5fd; color: #1d4ed8; padding: 8px 16px; border-radius: 8px; font-size: 12px; font-weight: 600; }
  .flow-arrow { color: #94a3b8; font-size: 18px; }
  .flow-box.green { background: #d1fae5; border-color: #6ee7b7; color: #065f46; }
  .flow-box.amber { background: #fef3c7; border-color: #fcd34d; color: #92400e; }
  .flow-box.red { background: #fee2e2; border-color: #fca5a5; color: #991b1b; }
  .flow-box.gray { background: #f1f5f9; border-color: #cbd5e1; color: #475569; }

  /* RECOMMENDATION */
  .rec { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 20px; margin-bottom: 12px; display: flex; gap: 16px; }
  .rec-num { width: 36px; height: 36px; border-radius: 50%; background: var(--accent); color: white; font-weight: 700; font-size: 14px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
  .rec-body h4 { font-size: 14px; font-weight: 700; margin-bottom: 4px; }
  .rec-body p { font-size: 13px; color: var(--muted); }
  .rec-body .imp { font-size: 11px; font-weight: 700; color: var(--danger); margin-top: 6px; }

  /* ROADMAP */
  .roadmap { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
  .rm-col h3 { font-size: 13px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 12px; padding: 8px 12px; border-radius: 8px; text-align: center; }
  .rm-col.v1 h3 { background: #fee2e2; color: #991b1b; }
  .rm-col.v2 h3 { background: #fef3c7; color: #92400e; }
  .rm-col.v3 h3 { background: #d1fae5; color: #065f46; }
  .rm-item { background: var(--card); border: 1px solid var(--border); border-radius: 8px; padding: 12px 14px; margin-bottom: 8px; font-size: 13px; }
  .rm-item strong { display: block; font-size: 12px; color: var(--muted); margin-bottom: 2px; }

  /* CODE */
  pre { background: #0f172a; color: #e2e8f0; padding: 20px; border-radius: 10px; font-family: 'Consolas', monospace; font-size: 12px; overflow-x: auto; margin: 12px 0; line-height: 1.5; }
  code { background: #f1f5f9; color: #be185d; padding: 2px 6px; border-radius: 4px; font-size: 12px; font-family: monospace; }

  /* RISK MATRIX */
  .risk-matrix { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin: 16px 0; }
  .risk-cell { padding: 16px; border-radius: 8px; }
  .r-critical { background: #fff5f5; border: 1px solid #fca5a5; }
  .r-high { background: #fffbeb; border: 1px solid #fcd34d; }
  .r-med { background: #ecfdf5; border: 1px solid #6ee7b7; }
  .r-low { background: #eff6ff; border: 1px solid #93c5fd; }
  .risk-cell h4 { font-size: 13px; font-weight: 700; margin-bottom: 6px; }
  .risk-cell p { font-size: 12px; color: var(--muted); }

  /* TIMELINE TRACK */
  .tl { position: relative; padding-left: 28px; margin: 16px 0; }
  .tl::before { content: ''; position: absolute; left: 7px; top: 4px; bottom: 4px; width: 2px; background: var(--border); }
  .tl-item { position: relative; margin-bottom: 20px; }
  .tl-dot { position: absolute; left: -28px; top: 4px; width: 14px; height: 14px; border-radius: 50%; background: var(--accent); border: 2px solid white; box-shadow: 0 0 0 2px var(--accent); }
  .tl-dot.done { background: var(--accent2); box-shadow: 0 0 0 2px var(--accent2); }
  .tl-dot.warn { background: var(--warn); box-shadow: 0 0 0 2px var(--warn); }
  .tl-item h4 { font-size: 13px; font-weight: 700; margin-bottom: 2px; }
  .tl-item p { font-size: 12px; color: var(--muted); }

  /* PRINT */
  @media print {
    .sidebar { display: none; }
    .main { margin-left: 0; }
    .doc-header { margin-left: 0; }
    .view { display: block !important; page-break-before: always; }
    .view:first-child { page-break-before: avoid; }
  }

  /* SCROLLBAR */
  .sidebar::-webkit-scrollbar { width: 4px; }
  .sidebar::-webkit-scrollbar-track { background: transparent; }
  .sidebar::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.15); border-radius: 4px; }

  .divider { height: 1px; background: var(--border); margin: 24px 0; }
  .note { background: #eff6ff; border: 1px solid #bfdbfe; border-radius: 8px; padding: 14px 16px; font-size: 13px; color: #1e40af; margin: 12px 0; }
  .note strong { font-weight: 700; }

  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
  .three-col { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; }
</style>
</head>
<body>

<!-- SIDEBAR -->
<nav class="sidebar">
  <div class="sidebar-logo">
    <h2>🚀 OPS CENTER</h2>
    <p>Documentación Técnica v1.0</p>
  </div>
  <div class="sidebar-section">Visión General</div>
  <a href="#" class="active" onclick="show('overview')"><span class="icon">🏠</span> Resumen Ejecutivo</a>
  <a href="#" onclick="show('modules')"><span class="icon">🧩</span> Módulos del Sistema</a>
  <a href="#" onclick="show('arch')"><span class="icon">🏗️</span> Arquitectura Técnica</a>

  <div class="sidebar-section">Análisis</div>
  <a href="#" onclick="show('arq-critique')"><span class="icon">🔬</span> Crítica Arquitectónica <span class="badge-nav badge-warn">!</span></a>
  <a href="#" onclick="show('pm-critique')"><span class="icon">📋</span> Crítica de PM <span class="badge-nav badge-warn">!</span></a>
  <a href="#" onclick="show('bugs')"><span class="icon">🐛</span> Bugs & Deuda Técnica</a>
  <a href="#" onclick="show('risks')"><span class="icon">⚠️</span> Riesgos del Proyecto</a>

  <div class="sidebar-section">Mejoras</div>
  <a href="#" onclick="show('roadmap')"><span class="icon">🗺️</span> Roadmap Propuesto</a>
  <a href="#" onclick="show('arch2')"><span class="icon">⚡</span> Arquitectura Ideal</a>
  <a href="#" onclick="show('flows')"><span class="icon">🔄</span> Flujos de Estado</a>

  <div class="sidebar-section">Referencia</div>
  <a href="#" onclick="show('data')"><span class="icon">💾</span> Modelo de Datos</a>
  <a href="#" onclick="show('deploy')"><span class="icon">🌐</span> Despliegue & Ops</a>
</nav>

<!-- HEADER -->
<header class="doc-header" id="main-header">
  <div class="tag">DOCUMENTACIÓN TÉCNICA Y DE PROYECTO</div>
  <h1>OPS CENTER — Gestor de Despliegues Clipp</h1>
  <p>Documentación integral del sistema construido: arquitectura, módulos, críticas técnicas, deuda acumulada y roadmap de mejoras propuesto.</p>
  <div class="meta">
    <div class="meta-item"><strong>Versión Actual</strong>1.0 — Monolítico HTML</div>
    <div class="meta-item"><strong>Estado</strong>En producción (GitHub Pages)</div>
    <div class="meta-item"><strong>Backend</strong>Supabase (PostgreSQL)</div>
    <div class="meta-item"><strong>Iteraciones</strong>60+ en sesión de desarrollo</div>
    <div class="meta-item"><strong>Líneas de código</strong>~5,000+ (HTML+JS+CSS)</div>
  </div>
</header>

<!-- MAIN CONTENT -->
<div class="main">

<!-- ===== OVERVIEW ===== -->
<div id="v-overview" class="view active" style="padding-top:40px;">
  <h1 class="section-title">Resumen Ejecutivo</h1>
  <p class="section-desc">Qué se construyó, por qué existe y qué valor aporta al equipo operativo de Clipp.</p>

  <div class="kpi-row">
    <div class="kpi"><div class="num">12</div><div class="label">Módulos implementados</div></div>
    <div class="kpi"><div class="num" style="color:var(--accent2)">73</div><div class="label">Clientes cargados</div></div>
    <div class="kpi"><div class="num" style="color:var(--warn)">60+</div><div class="label">Iteraciones de desarrollo</div></div>
    <div class="kpi"><div class="num" style="color:var(--danger)">1</div><div class="label">Archivo HTML monolítico</div></div>
  </div>

  <h2 class="sub-title">¿Qué es OPS Center?</h2>
  <div class="card">
    <p>OPS Center es una herramienta de gestión operativa interna construida como una <strong>Single Page Application (SPA) en un único archivo HTML</strong> autocontenido. Fue diseñada para que el equipo de Clipp pueda hacer seguimiento completo del ciclo de vida de sus clientes: desde el primer contacto (prospecto) hasta la operación continua, el cobro mensual y el análisis de capacidad del equipo.</p>
    <br>
    <p>El sistema reemplaza hojas de cálculo Excel dispersas (DESPLIEGUES MARZO 2026, SEMÁFORO DE CLIENTES) por una interfaz centralizada accesible en línea vía GitHub Pages, con sincronización en tiempo real a través de Supabase.</p>
  </div>

  <h2 class="sub-title">Flujo Central del Sistema</h2>
  <div class="card">
    <p style="font-size:13px;color:var(--muted);margin-bottom:16px;">Un cliente transita por los siguientes estados, y el sistema acompaña cada transición:</p>
    <div class="flow">
      <div class="flow-box gray">📋 Prospecto</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box">🚀 En Despliegue</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box green">✅ Operativo</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box amber">💰 Cobranza Mensual</div>
    </div>
    <div class="flow" style="margin-top:8px;">
      <div class="flow-box green">✅ Operativo</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box amber">⚠️ Deshabilitado</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box red">❌ Perdido</div>
    </div>
  </div>

  <h2 class="sub-title">Módulos Principales</h2>
  <div class="card-grid">
    <div class="card-sm">
      <h3 class="card-title">📊 Dashboard Ejecutivo</h3>
      <p style="font-size:13px;color:var(--muted)">KPIs en tiempo real, gráficos por estado, país, ciudad, modalidad. Filtros por fecha y launcher.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">🚀 Seguimiento de Despliegues</h3>
      <p style="font-size:13px;color:var(--muted)">Seguimiento operativo completo: estado, fechas, módulos, fases, complejidad, horas, notas, adjuntos.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">👥 Gestión de Clientes</h3>
      <p style="font-size:13px;color:var(--muted)">CRM ligero con contacto, aplicativo, tipo, módulos, complejidad y fecha de confirmación.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">📅 Línea de Tiempo</h3>
      <p style="font-size:13px;color:var(--muted)">Reporte histórico y proyección mensual de confirmados, despliegues y operativos con gráfico SVG.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">⚡ Capacidad Operativa</h3>
      <p style="font-size:13px;color:var(--muted)">Cálculo de carga del equipo por operador, horas individuales por cliente y peso de complejidad.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">📋 Contratos</h3>
      <p style="font-size:13px;color:var(--muted)">Contratos por componente mixto: setup, mínimo, por conductor/vehículo, bot WA, fijo. Descuentos por mes.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">💰 Cobranzas</h3>
      <p style="font-size:13px;color:var(--muted)">Registro mensual de cobros, estados de pago, desglose por componente, edición por cobro individual.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">🎯 Estrategia de Despliegue</h3>
      <p style="font-size:13px;color:var(--muted)">Clasificación por complejidad, checklists por módulo, fases de implementación conectadas al despliegue.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">🗂️ Catálogos</h3>
      <p style="font-size:13px;color:var(--muted)">CRUD de módulos, aplicativos, tipos, modalidades, fases, complejidad, riesgos, tiempos y tipos de cobro.</p>
    </div>
  </div>

  <h2 class="sub-title">Tecnologías Utilizadas</h2>
  <div class="two-col">
    <div class="card">
      <h3 class="card-title">Frontend</h3>
      <div style="display:flex;flex-wrap:wrap;gap:6px;margin-top:8px;">
        <span class="tag tag-blue">HTML5</span>
        <span class="tag tag-blue">CSS3 Variables</span>
        <span class="tag tag-blue">Vanilla JavaScript ES6+</span>
        <span class="tag tag-gray">SVG Charts</span>
        <span class="tag tag-gray">localStorage</span>
      </div>
    </div>
    <div class="card">
      <h3 class="card-title">Backend / Infraestructura</h3>
      <div style="display:flex;flex-wrap:gap:6px;margin-top:8px;">
        <span class="tag tag-green">Supabase (PostgreSQL)</span>
        <span class="tag tag-green">Supabase Auth</span>
        <span class="tag tag-green">Supabase Realtime</span>
        <span class="tag tag-purple">GitHub Pages</span>
      </div>
    </div>
  </div>
</div>

<!-- ===== MODULES ===== -->
<div id="v-modules" class="view" style="padding-top:40px;">
  <h1 class="section-title">Módulos del Sistema</h1>
  <p class="section-desc">Descripción funcional detallada de cada módulo, sus entradas, salidas y dependencias.</p>

  <h2 class="sub-title">Mapa de Dependencias entre Módulos</h2>
  <div class="card">
    <table>
      <thead><tr><th>Módulo</th><th>Lee de</th><th>Escribe en</th><th>Afecta a</th></tr></thead>
      <tbody>
        <tr><td><strong>Clientes</strong></td><td>Catálogos (módulos, aplicativos)</td><td>BC[ ] — array de clientes</td><td>Despliegues, Cobranzas, Contratos, Dashboard, Capacidad</td></tr>
        <tr><td><strong>Despliegues</strong></td><td>Clientes, Catálogos</td><td>dep[ ] — array de despliegues</td><td>Dashboard, Línea de Tiempo, Capacidad, Estrategia</td></tr>
        <tr><td><strong>Catálogos</strong></td><td>—</td><td>cats{ } — objeto catálogos</td><td>Todos los módulos</td></tr>
        <tr><td><strong>Contratos</strong></td><td>Clientes, Catálogos (tipos de cobro)</td><td>contratos[ ]</td><td>Cobranzas, Dashboard</td></tr>
        <tr><td><strong>Cobranzas</strong></td><td>Contratos, Clientes Operativos</td><td>cobros[ ]</td><td>Dashboard, Línea de Tiempo</td></tr>
        <tr><td><strong>Capacidad</strong></td><td>Despliegues, Clientes Operativos, Catálogos (complejidad)</td><td>Config operativa</td><td>Estrategia de Despliegue</td></tr>
        <tr><td><strong>Línea de Tiempo</strong></td><td>Clientes (fconf), Despliegues (fechas)</td><td>Solo lectura</td><td>Reportes ejecutivos</td></tr>
        <tr><td><strong>Estrategia</strong></td><td>Despliegues, Catálogos (fases, tiempos)</td><td>dep[].fase</td><td>Despliegues</td></tr>
      </tbody>
    </table>
  </div>

  <h2 class="sub-title">Catálogos (Fuente de Verdad de Configuración)</h2>
  <div class="card">
    <p style="font-size:13px;color:var(--muted);margin-bottom:14px;">Los catálogos son el corazón del sistema. Cualquier cambio aquí impacta en toda la aplicación. Son 9 sub-catálogos:</p>
    <div class="three-col">
      <div>
        <div style="font-size:12px;font-weight:700;color:var(--blue);margin-bottom:8px;">CONFIGURACIÓN OPERATIVA</div>
        <div class="tag tag-blue">Módulos</div>
        <div class="tag tag-blue">Categorías de Módulos</div>
        <div class="tag tag-blue">Aplicativos</div>
        <div class="tag tag-blue">Tipos de Cliente</div>
        <div class="tag tag-blue">Modalidades de Despliegue</div>
      </div>
      <div>
        <div style="font-size:12px;font-weight:700;color:var(--blue);margin-bottom:8px;">IMPLEMENTACIÓN</div>
        <div class="tag tag-green">Fases de Implementación</div>
        <div class="tag tag-green">T. de Implementación</div>
        <div class="tag tag-green">Niveles de Complejidad</div>
      </div>
      <div>
        <div style="font-size:12px;font-weight:700;color:var(--blue);margin-bottom:8px;">COBROS & RIESGO</div>
        <div class="tag tag-amber">Tipos de Cobro</div>
        <div class="tag tag-amber">Estados de Riesgo</div>
      </div>
    </div>
  </div>

  <h2 class="sub-title">Módulo: Cobranzas — Lógica de Facturación</h2>
  <div class="card">
    <p style="font-size:13px;color:var(--muted);margin-bottom:16px;">El modelo de cobro es <strong>por componentes mixtos</strong>. Un contrato puede combinar:</p>
    <table>
      <thead><tr><th>Tipo de Cobro</th><th>Lógica</th><th>Periodicidad</th></tr></thead>
      <tbody>
        <tr><td><code>setup</code></td><td>Pago único al inicio del contrato</td><td>Una vez</td></tr>
        <tr><td><code>deposito</code></td><td>Depósito reembolsable</td><td>Una vez</td></tr>
        <tr><td><code>fijo</code></td><td>Monto fijo sin importar uso</td><td>Mensual</td></tr>
        <tr><td><code>minimo</code></td><td>Garantía mínima (cobra diferencia si activos < mínimo)</td><td>Mensual</td></tr>
        <tr><td><code>conductor</code></td><td>Precio × N° conductores activos ese mes</td><td>Mensual</td></tr>
        <tr><td><code>vehiculo</code></td><td>Precio × N° vehículos activos ese mes</td><td>Mensual</td></tr>
        <tr><td><code>bot_wa</code></td><td>Precio × N° bots WhatsApp en coexistencia</td><td>Mensual</td></tr>
        <tr><td><code>modulo</code></td><td>Cobro por módulo adicional activo</td><td>Mensual</td></tr>
      </tbody>
    </table>
    <div class="note"><strong>Regla clave:</strong> Al generar cobros de un mes, el operador ingresa cuántos conductores, vehículos y bots tuvo cada cliente ese mes. El sistema calcula el total en tiempo real antes de confirmar.</div>
  </div>

  <h2 class="sub-title">Módulo: Capacidad Operativa — Fórmula</h2>
  <div class="card">
    <pre>Hrs Disponibles = Operadores × Hrs/día × Días/mes

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
Semáforo: Verde < 70% | Ámbar 70-90% | Rojo > 90%</pre>
  </div>
</div>

<!-- ===== ARCHITECTURE ===== -->
<div id="v-arch" class="view" style="padding-top:40px;">
  <h1 class="section-title">Arquitectura Técnica Actual</h1>
  <p class="section-desc">Descripción de cómo está construido el sistema hoy — sin filtros.</p>

  <h2 class="sub-title">Estructura del Sistema</h2>
  <div class="card">
    <pre>gestor_despliegues.html  (~5,000+ líneas)
├── &lt;head&gt;
│   ├── CDN: Supabase JS (cargado condicionalmente)
│   └── &lt;style&gt; — CSS completo embebido (~600 líneas)
├── &lt;body&gt;
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
│   └── &lt;script&gt; — JS completo embebido (~4,000+ líneas)
│       ├── Constantes y estado global
│       ├── Supabase integration
│       ├── sv() / ld() — save/load localStorage+Supabase
│       ├── Funciones de render por vista (12+)
│       ├── Funciones de CRUD
│       ├── Helpers (complejidad, riesgo, badges)
│       └── INIT — inicialización y migración de datos</pre>
  </div>

  <h2 class="sub-title">Persistencia de Datos</h2>
  <div class="two-col">
    <div class="card">
      <h3 class="card-title">📦 Estado en Memoria (JS Global)</h3>
      <pre style="font-size:11px;">let dep = []       // Despliegues
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
}</pre>
    </div>
    <div class="card">
      <h3 class="card-title">💾 Estrategia de Persistencia</h3>
      <p style="font-size:13px;color:var(--muted);margin-bottom:12px;">Doble capa de guardado:</p>
      <div class="tl">
        <div class="tl-item">
          <div class="tl-dot done"></div>
          <h4>localStorage (primera capa)</h4>
          <p>Guarda inmediatamente. Funciona offline. Límite 5-10MB.</p>
        </div>
        <div class="tl-item">
          <div class="tl-dot done"></div>
          <h4>Supabase (segunda capa)</h4>
          <p>Serializa todo el estado en una tabla <code>ops_data</code> con key-value JSONB. Sincronización vía WebSocket + polling 10s.</p>
        </div>
        <div class="tl-item">
          <div class="tl-dot warn"></div>
          <h4>Modelo de datos plano</h4>
          <p>Todo en una sola fila Supabase por key. No normalizado. Ver sección de riesgos.</p>
        </div>
      </div>
    </div>
  </div>

  <h2 class="sub-title">Patrón de Render</h2>
  <div class="card">
    <p style="font-size:13px;color:var(--muted);margin-bottom:12px;">Cada módulo sigue el mismo patrón de renderizado imperativo:</p>
    <pre>// Patrón usado en los 12 módulos
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
}</pre>
    <div class="note" style="margin-top:12px;"><strong>Problema:</strong> Cada cambio re-renderiza TODA la tabla (no solo el elemento modificado). Con 73+ clientes y tablas complejas, esto genera parpadeos y puede ser lento en dispositivos lentos.</div>
  </div>
</div>

<!-- ===== ARQ CRITIQUE ===== -->
<div id="v-arq-critique" class="view" style="padding-top:40px;">
  <h1 class="section-title">🔬 Crítica Arquitectónica</h1>
  <p class="section-desc">Análisis como arquitecto de software senior. Sin suavizar — esto es lo que necesitas saber para escalar el sistema.</p>

  <div class="sev-row sev-high">
    <div class="sev-icon">🔴</div>
    <div class="sev-content">
      <h4>CRÍTICO: Archivo monolítico de 5,000+ líneas</h4>
      <p>Todo el sistema — HTML, CSS y JavaScript — está en un solo archivo. Esto hace imposible el trabajo paralelo en equipo, las pruebas unitarias, el control de versiones significativo (un solo commit cambia todo), y el debugging preciso. Un error de sintaxis en cualquier línea puede romper silenciosamente todo el sistema, como ocurrió múltiples veces durante el desarrollo (template literals rotos, <code>let cobCharts</code> duplicado, <code>linReg</code> declarada dos veces).</p>
    </div>
  </div>

  <div class="sev-row sev-high">
    <div class="sev-icon">🔴</div>
    <div class="sev-content">
      <h4>CRÍTICO: Modelo de datos no normalizado en Supabase</h4>
      <p>Toda la data se guarda como un blob JSONB en una tabla <code>ops_data</code> con key-value. Esto significa: sin joins, sin queries eficientes, sin indexación, sin integridad referencial. Si tienes 500 clientes y 3,000 cobros, deserializar todo en cada sync es ineficiente. Además, Supabase Realtime notifica el cambio del registro completo, no solo el campo modificado — transmitiendo megabytes innecesariamente.</p>
    </div>
  </div>

  <div class="sev-row sev-high">
    <div class="sev-icon">🔴</div>
    <div class="sev-content">
      <h4>CRÍTICO: Sin control de concurrencia</h4>
      <p>Si dos usuarios editan simultáneamente, el último en guardar gana (last-write-wins total). No hay bloqueo optimista, no hay merge de cambios, no hay notificación de conflicto. Usuario A edita cliente X → guarda. Usuario B también editó cliente X → guarda → sobrescribe los cambios de A sin aviso. En un equipo de 5+ personas esto causará pérdida de datos.</p>
    </div>
  </div>

  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>ALTO: Re-render total en cada cambio</h4>
      <p>La función <code>saveVal()</code> llama a <code>sv()</code> (guarda todo) y luego <code>renderDeps()</code> (re-renderiza toda la tabla). Cambiar una celda = reconstruir 73+ filas DOM. En tablas grandes con columnas editables, esto produce parpadeos y puede perder el foco del input activo.</p>
    </div>
  </div>

  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>ALTO: Variables globales como estado compartido</h4>
      <p>Todo el estado vive en variables globales JS (<code>dep</code>, <code>BC</code>, <code>contratos</code>, <code>cats</code>). No hay patrón de estado (Redux, Zustand, Context). Cualquier función puede mutar cualquier dato sin que el sistema lo sepa. Esto causó múltiples bugs de sincronización durante el desarrollo (módulos que no se actualizaban, datos que se "perdían" al cambiar de vista).</p>
    </div>
  </div>

  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>ALTO: Sin manejo de errores estructurado</h4>
      <p>No hay un sistema global de manejo de errores. Los errores de Supabase se capturan localmente en algunos casos, pero no hay logging, no hay UI de error consistente, y no hay fallback inteligente. Un fallo de red durante un save puede corromper el estado local sin que el usuario lo sepa.</p>
    </div>
  </div>

  <div class="sev-row sev-low">
    <div class="sev-icon">🟢</div>
    <div class="sev-content">
      <h4>MEDIO: Autenticación básica</h4>
      <p>El sistema usa Supabase Auth correctamente para login, pero no tiene roles (admin vs. operador vs. solo lectura). Cualquier usuario autenticado puede editar cualquier dato, incluyendo catálogos críticos o eliminar clientes. Para un equipo de trabajo real, necesitas RBAC (Role-Based Access Control).</p>
    </div>
  </div>

  <div class="sev-row sev-low">
    <div class="sev-icon">🟢</div>
    <div class="sev-content">
      <h4>MEDIO: Falta de validación de datos</h4>
      <p>No hay validación de formularios consistente. Se puede guardar un contrato sin cliente, un cobro sin monto, o una fecha de entrega anterior a la de inicio. La integridad de los datos depende completamente del operador.</p>
    </div>
  </div>

  <h2 class="sub-title">Lo que SÍ está bien hecho</h2>
  <div class="card">
    <div class="tl">
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Arquitectura de catálogos configurable</h4>
        <p>Separar los datos de configuración (módulos, fases, complejidad) del código es una decisión correcta. Permite cambios de reglas sin tocar código.</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Doble capa de persistencia (localStorage + Supabase)</h4>
        <p>Funciona offline y sincroniza cuando hay red. El patrón es válido para aplicaciones con equipos pequeños.</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Migración automática de datos</h4>
        <p>El bloque INIT que migra estructuras antiguas al nuevo formato es una práctica correcta para mantener retrocompatibilidad.</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>SVG charts como alternativa a Chart.js</h4>
        <p>Reemplazar una librería con problemas de timing por SVG puro fue la decisión correcta para el contexto de un SPA de un solo archivo.</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Modelo de cobro flexible por componentes</h4>
        <p>El sistema de tipos de cobro mezclables (setup + mínimo + por conductor + bot WA) es elegante y refleja bien la realidad del negocio.</p>
      </div>
    </div>
  </div>
</div>

<!-- ===== PM CRITIQUE ===== -->
<div id="v-pm-critique" class="view" style="padding-top:40px;">
  <h1 class="section-title">📋 Crítica de Gestión de Proyecto</h1>
  <p class="section-desc">Análisis como PMO / Project Manager senior. El foco está en proceso, scope, deuda acumulada y eficiencia del equipo.</p>

  <h2 class="sub-title">Diagnóstico del Proceso</h2>

  <div class="sev-row sev-high">
    <div class="sev-icon">🔴</div>
    <div class="sev-content">
      <h4>Desarrollo iterativo sin backlog formal</h4>
      <p>El proyecto se desarrolló mediante conversación directa sin documentación previa de requerimientos, sin casos de uso, sin criterios de aceptación. Cada mensaje fue un nuevo requerimiento o un reporte de bug. Esto generó: scope creep constante, retrabajo, features incompletas que se dejaron a medias, y bugs introducidos al agregar nuevas funcionalidades sin regresión.</p>
    </div>
  </div>

  <div class="sev-row sev-high">
    <div class="sev-icon">🔴</div>
    <div class="sev-content">
      <h4>Múltiples "continuar" — síntoma de falta de diseño previo</h4>
      <p>El chat registra al menos 12 mensajes de "Continuar" indicando que las sesiones quedaron incompletas. Cada "continuar" introdujo riesgo de romper lo ya construido. El desarrollo se interrumpió en estados intermedios varias veces, requiriendo debugging adicional al retomar. <strong>En un proyecto real esto equivale a deployments a producción con código a medio terminar.</strong></p>
    </div>
  </div>

  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>Scope creep no controlado</h4>
      <p>El proyecto pasó de "organizar despliegues del Excel" a un sistema con 12 módulos, CRM, billing, análisis de capacidad, estrategia de despliegue, y sincronización en tiempo real. Sin priorización, se perdió tiempo en features complejas (cobros por componente, línea de tiempo con proyección) mientras funcionalidades básicas seguían con bugs (botón Nuevo Despliegue, gráficos que no renderizaban).</p>
    </div>
  </div>

  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>Sin ambiente de testing / staging</h4>
      <p>Todos los cambios se aplicaron directamente sobre el archivo de producción. No hay un ambiente de staging, no hay tests, no hay validación antes de publicar en GitHub Pages. El equipo ha estado usando el sistema mientras se modificaba activamente, con riesgo de pérdida de datos.</p>
    </div>
  </div>

  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>Features incompletas en producción</h4>
      <p>Varios módulos quedaron parcialmente implementados: la lógica de fases de estrategia no avanzaba correctamente, el botón "Nuevo Contrato" abría la vista de despliegue, la línea de tiempo no generaba gráficas por múltiples sesiones. Estas regresiones, en un software de uso del equipo, erosionan la confianza en la herramienta.</p>
    </div>
  </div>

  <h2 class="sub-title">Estimación de Deuda Técnica Acumulada</h2>
  <div class="card">
    <table>
      <thead><tr><th>Área</th><th>Deuda</th><th>Esfuerzo para resolver</th><th>Impacto si no se resuelve</th></tr></thead>
      <tbody>
        <tr>
          <td><strong>Modelo de datos</strong></td>
          <td>Sin normalización en Supabase</td>
          <td>🔴 4–6 días</td>
          <td>Escalabilidad bloqueada</td>
        </tr>
        <tr>
          <td><strong>Arquitectura</strong></td>
          <td>Monolítico, sin separación</td>
          <td>🔴 2–3 semanas</td>
          <td>Imposible mantener en equipo</td>
        </tr>
        <tr>
          <td><strong>Concurrencia</strong></td>
          <td>Last-write-wins total</td>
          <td>🟡 2–3 días</td>
          <td>Pérdida de datos en uso simultáneo</td>
        </tr>
        <tr>
          <td><strong>Validación</strong></td>
          <td>Sin validación de formularios</td>
          <td>🟢 1–2 días</td>
          <td>Datos corruptos en BD</td>
        </tr>
        <tr>
          <td><strong>Roles</strong></td>
          <td>Sin control de acceso por rol</td>
          <td>🟡 1–2 días</td>
          <td>Cualquiera puede borrar catálogos</td>
        </tr>
        <tr>
          <td><strong>Testing</strong></td>
          <td>Cero cobertura</td>
          <td>🔴 Proceso continuo</td>
          <td>Regresiones constantes al iterar</td>
        </tr>
      </tbody>
    </table>
  </div>

  <h2 class="sub-title">Lo que se hizo bien desde PM</h2>
  <div class="card-grid">
    <div class="card-sm">
      <h3 class="card-title">✅ Claridad de visión del negocio</h3>
      <p style="font-size:13px;color:var(--muted)">El usuario siempre tuvo claro qué necesitaba operativamente. Los requerimientos de negocio fueron precisos aunque no estructurados formalmente.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">✅ Datos reales cargados</h3>
      <p style="font-size:13px;color:var(--muted)">Se trabajó con datos reales desde el inicio (73 clientes del Excel), lo que permite validar el sistema con casos de uso reales.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">✅ Decisión de Supabase correcta</h3>
      <p style="font-size:13px;color:var(--muted)">Elegir Supabase para multiusuario en tiempo real fue la decisión correcta para el presupuesto y la escala del proyecto.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">✅ Manual de uso incluido</h3>
      <p style="font-size:13px;color:var(--muted)">Agregar un módulo de manual dentro de la propia herramienta facilita el onboarding del equipo sin documentación externa.</p>
    </div>
  </div>
</div>

<!-- ===== BUGS ===== -->
<div id="v-bugs" class="view" style="padding-top:40px;">
  <h1 class="section-title">🐛 Bugs Identificados & Deuda Técnica</h1>
  <p class="section-desc">Registro de bugs documentados en el chat y problemas técnicos conocidos.</p>

  <h2 class="sub-title">Bugs Críticos Resueltos (historial)</h2>
  <div class="card">
    <table>
      <thead><tr><th>Bug</th><th>Causa Raíz</th><th>Solución Aplicada</th></tr></thead>
      <tbody>
        <tr>
          <td>Ningún módulo cargaba</td>
          <td>Template literal roto en <code>TIPOS_DEP().map()</code> — la comilla cerraba antes</td>
          <td>Corrección del template literal. Se implementó migración automática en INIT.</td>
        </tr>
        <tr>
          <td>Gráfico de línea de tiempo vacío</td>
          <td><code>linReg()</code> declarada dos veces (dentro y fuera de <code>renderTimeline</code>). En JS esto causa SyntaxError silencioso.</td>
          <td>Eliminada la declaración duplicada. Función movida fuera del scope.</td>
        </tr>
        <tr>
          <td>Chart.js no renderizaba</td>
          <td>Canvas tenía tamaño 0×0 al estar en contenedor <code>display:none</code>. Chart.js no puede medir el canvas.</td>
          <td>Reemplazo de Chart.js por SVG puro generado con <code>innerHTML</code>.</td>
        </tr>
        <tr>
          <td>Todo el script se rompía con <code>let cobCharts</code></td>
          <td>Variable declarada con <code>let</code> dos veces en el mismo scope</td>
          <td>Eliminada la declaración duplicada.</td>
        </tr>
        <tr>
          <td>Botón "Nuevo Contrato" abría vista de Despliegue</td>
          <td>Conflicto de IDs entre el modal de contrato y el modal de despliegue</td>
          <td>Separación de modales y corrección de <code>openAddContrato()</code>.</td>
        </tr>
        <tr>
          <td>Login no funcionaba en modo online</td>
          <td>Librería Supabase CDN con formato <code>sb_publishable_...</code> incompatible con versión JS importada</td>
          <td>Cambio al anon key legacy (<code>eyJ...</code>) y actualización del CDN.</td>
        </tr>
        <tr>
          <td>Columnas Pendientes y Hrs/mes invertidas</td>
          <td>El orden en <code>depTH</code> y <code>depRow</code> no coincidía</td>
          <td>Swap de celdas en <code>depRow</code>.</td>
        </tr>
      </tbody>
    </table>
  </div>

  <h2 class="sub-title">Problemas Conocidos Actuales (pendientes)</h2>
  <div class="sev-row sev-high">
    <div class="sev-icon">🔴</div>
    <div class="sev-content">
      <h4>Concurrencia sin manejo de conflictos</h4>
      <p>Dos usuarios editando al mismo tiempo: last-write-wins. No hay merge, no hay notificación de conflicto.</p>
    </div>
  </div>
  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>Fases de estrategia no avanzan correctamente en todos los casos</h4>
      <p>Los botones ▶/◀ en despliegues y en estrategia no están 100% sincronizados bidireccionalemnte en todos los paths de código.</p>
    </div>
  </div>
  <div class="sev-row sev-med">
    <div class="sev-icon">🟡</div>
    <div class="sev-content">
      <h4>Pérdida de foco en inputs inline al guardar</h4>
      <p>Al editar una celda inline y guardar, el re-render total de la tabla quita el foco del input. En tablas largas el usuario pierde contexto visual.</p>
    </div>
  </div>
  <div class="sev-row sev-low">
    <div class="sev-icon">🟢</div>
    <div class="sev-content">
      <h4>Sin paginación en tablas</h4>
      <p>Con 73+ clientes y futuras adiciones, las tablas sin paginación serán lentas. No hay limit/offset.</p>
    </div>
  </div>
</div>

<!-- ===== RISKS ===== -->
<div id="v-risks" class="view" style="padding-top:40px;">
  <h1 class="section-title">⚠️ Matriz de Riesgos</h1>
  <p class="section-desc">Riesgos actuales del sistema ordenados por impacto y probabilidad.</p>

  <div class="risk-matrix">
    <div class="risk-cell r-critical">
      <h4>🔴 CRÍTICO — Pérdida de datos por concurrencia</h4>
      <p><strong>Prob:</strong> Alta (uso diario multi-usuario)<br><strong>Impacto:</strong> Datos de clientes y cobros sobrescritos sin aviso<br><strong>Mitigación:</strong> Implementar versionado optimista o bloqueo por registro en Supabase</p>
    </div>
    <div class="risk-cell r-critical">
      <h4>🔴 CRÍTICO — Corrupción por límite localStorage</h4>
      <p><strong>Prob:</strong> Media (crecimiento de datos)<br><strong>Impacto:</strong> La app falla silenciosamente cuando localStorage llega a ~5MB<br><strong>Mitigación:</strong> Eliminar localStorage como persistencia primaria; Supabase como única fuente de verdad</p>
    </div>
    <div class="risk-cell r-high">
      <h4>🟡 ALTO — Un bug de JS rompe toda la app</h4>
      <p><strong>Prob:</strong> Alta (al agregar features)<br><strong>Impacto:</strong> La app queda completamente inutilizable<br><strong>Mitigación:</strong> Separar en módulos, agregar error boundaries, testing básico</p>
    </div>
    <div class="risk-cell r-high">
      <h4>🟡 ALTO — Sin backup ni versionado de datos</h4>
      <p><strong>Prob:</strong> Media<br><strong>Impacto:</strong> Un delete masivo accidental es irrecuperable<br><strong>Mitigación:</strong> Soft deletes, backup diario de Supabase, botón "Exportar JSON" completo</p>
    </div>
    <div class="risk-cell r-med">
      <h4>🟢 MEDIO — Supabase free tier limitaciones</h4>
      <p><strong>Prob:</strong> Alta (crecimiento)<br><strong>Impacto:</strong> Supabase free pausa proyectos inactivos 7 días, límite 500MB DB<br><strong>Mitigación:</strong> Upgrade a plan Pro ($25/mes) cuando el equipo lo use diariamente</p>
    </div>
    <div class="risk-cell r-low">
      <h4>🔵 BAJO — Dependencia de GitHub Pages</h4>
      <p><strong>Prob:</strong> Baja (servicio estable)<br><strong>Impacto:</strong> App no disponible si GitHub tiene outage<br><strong>Mitigación:</strong> Netlify como alternativa, el HTML funciona también localmente</p>
    </div>
  </div>

  <h2 class="sub-title">Plan de Contingencia Inmediato</h2>
  <div class="card">
    <div class="tl">
      <div class="tl-item">
        <div class="tl-dot warn"></div>
        <h4>Esta semana: Exportar respaldo manual</h4>
        <p>Desde Supabase → Table Editor → exportar como CSV. Hacerlo semanalmente hasta tener backup automático.</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot warn"></div>
        <h4>Esta semana: Agregar botón "Exportar JSON completo"</h4>
        <p>Un botón que descargue todo el estado del sistema como JSON. Esto permite restauración manual en caso de pérdida.</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot"></div>
        <h4>Próximas 2 semanas: Soft deletes</h4>
        <p>En lugar de borrar registros, marcarlos como <code>deleted: true</code>. Permite recuperación ante errores.</p>
      </div>
    </div>
  </div>
</div>

<!-- ===== ROADMAP ===== -->
<div id="v-roadmap" class="view" style="padding-top:40px;">
  <h1 class="section-title">🗺️ Roadmap Propuesto</h1>
  <p class="section-desc">Plan de evolución en 3 fases, priorizando estabilidad antes de nuevas features.</p>

  <div class="note">
    <strong>Principio guía:</strong> Estabilizar antes de crecer. La deuda técnica actual es alta. Agregar features sobre una base inestable aumentará el costo exponencialmente. La Fase 1 es no negociable.
  </div>

  <div class="roadmap" style="margin-top:24px;">
    <div class="rm-col v1">
      <h3>FASE 1 — Estabilizar (4 semanas)</h3>
      <div class="rm-item"><strong>P1 — Semana 1-2</strong>Modelo de datos normalizado en Supabase (tablas reales, no key-value JSONB)</div>
      <div class="rm-item"><strong>P1 — Semana 1-2</strong>Eliminar localStorage como persistencia primaria. Solo Supabase.</div>
      <div class="rm-item"><strong>P1 — Semana 1</strong>Soft deletes en todas las entidades (deleted_at timestamp)</div>
      <div class="rm-item"><strong>P1 — Semana 1</strong>Exportar/Importar JSON completo del sistema</div>
      <div class="rm-item"><strong>P2 — Semana 2-3</strong>Validación de formularios (campos requeridos, fechas consistentes)</div>
      <div class="rm-item"><strong>P2 — Semana 3-4</strong>Roles básicos: Admin / Operador / Solo lectura</div>
      <div class="rm-item"><strong>P2 — Semana 3-4</strong>Fix de concurrencia: versión de timestamp por registro</div>
    </div>
    <div class="rm-col v2">
      <h3>FASE 2 — Refactorizar (6-8 semanas)</h3>
      <div class="rm-item"><strong>Arquitectura</strong>Separar en archivos: app.js, modules/deps.js, modules/clients.js, etc.</div>
      <div class="rm-item"><strong>Estado</strong>Implementar patrón Store centralizado (simple event-driven)</div>
      <div class="rm-item"><strong>Render</strong>Actualización granular (solo el elemento modificado, no toda la tabla)</div>
      <div class="rm-item"><strong>Testing</strong>Tests unitarios para lógica de negocio (calcFactura, complejidad, capacidad)</div>
      <div class="rm-item"><strong>Paginación</strong>Virtualización de tablas largas (50 filas por página)</div>
      <div class="rm-item"><strong>Logging</strong>Audit trail: quién cambió qué y cuándo</div>
      <div class="rm-item"><strong>Notificaciones</strong>Alertas por clientes en riesgo, cobros vencidos, despliegues sin avance</div>
    </div>
    <div class="rm-col v3">
      <h3>FASE 3 — Crecer (ongoing)</h3>
      <div class="rm-item"><strong>Reportes PDF</strong>Exportar estados de cuenta por cliente, reporte ejecutivo mensual</div>
      <div class="rm-item"><strong>Integraciones</strong>WhatsApp API para notificaciones de cobro, email automático de estado</div>
      <div class="rm-item"><strong>App móvil</strong>PWA para uso desde celular (especialmente útil para el equipo en campo)</div>
      <div class="rm-item"><strong>Analytics avanzado</strong>LTV por cliente, churn rate, NPS, predicción de renovaciones</div>
      <div class="rm-item"><strong>Facturación</strong>Integración con sistema de facturación (Facturae, SRI Ecuador)</div>
      <div class="rm-item"><strong>Multi-empresa</strong>Soporte para múltiples instancias de Clipp / Azutaxo</div>
    </div>
  </div>
</div>

<!-- ===== IDEAL ARCH ===== -->
<div id="v-arch2" class="view" style="padding-top:40px;">
  <h1 class="section-title">⚡ Arquitectura Ideal</h1>
  <p class="section-desc">Cómo debería verse el sistema a largo plazo para ser mantenible, escalable y seguro.</p>

  <h2 class="sub-title">Modelo de Datos Normalizado (Supabase)</h2>
  <div class="card">
    <pre>-- ENTIDADES PRINCIPALES
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
audit_log         (id, user_id, tabla, accion, before_json, after_json, timestamp)</pre>
  </div>

  <h2 class="sub-title">Estructura de Archivos Propuesta</h2>
  <div class="card">
    <pre>ops-center/
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
    └── validators.test.js</pre>
  </div>

  <h2 class="sub-title">Patrón de Estado Propuesto</h2>
  <div class="card">
    <pre>// store.js — Event-driven simple state
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
store.on('deployment:phase_changed', (state) => renderStrategyView(state));</pre>
  </div>
</div>

<!-- ===== FLOWS ===== -->
<div id="v-flows" class="view" style="padding-top:40px;">
  <h1 class="section-title">🔄 Flujos de Estado Propuestos</h1>
  <p class="section-desc">Estados correctos, transiciones válidas y dependencias entre módulos.</p>

  <h2 class="sub-title">Estados de Cliente</h2>
  <div class="card">
    <div class="flow">
      <div class="flow-box gray">📋 prospecto</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box">🚀 en_despliegue</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box green">✅ operativo</div>
      <div class="flow-arrow">↔</div>
      <div class="flow-box amber">⚠️ pausado</div>
    </div>
    <div class="flow" style="margin-top:8px;">
      <div class="flow-box red">❌ perdido</div>
      <div class="flow-arrow">←</div>
      <div class="flow-box amber">⚠️ pausado</div>
    </div>
    <table style="margin-top:16px;">
      <thead><tr><th>Transición</th><th>Trigger</th><th>Efecto en otros módulos</th></tr></thead>
      <tbody>
        <tr><td>prospecto → en_despliegue</td><td>Crear nuevo despliegue para el cliente</td><td>Aparece en tabla Despliegues. Suma a Capacidad como "cliente nuevo".</td></tr>
        <tr><td>en_despliegue → operativo</td><td>Cambiar estado del despliegue a "Listo"</td><td>Sale de Despliegues. Aparece en Clientes Operativos. Disponible en Cobranzas.</td></tr>
        <tr><td>operativo → pausado</td><td>Cambiar estado manualmente</td><td>Sale de Cobranzas activa. Pasa a Clientes Inactivos. No se generan cobros.</td></tr>
        <tr><td>pausado → perdido</td><td>Cambiar estado manualmente</td><td>Se marca en rojo. Requiere nota de motivo (recomendado).</td></tr>
      </tbody>
    </table>
  </div>

  <h2 class="sub-title">Estados de Despliegue</h2>
  <div class="card">
    <div class="flow">
      <div class="flow-box gray">por_iniciar</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box">en_proceso</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box amber">capacitacion</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box green">listo</div>
    </div>
    <div class="note" style="margin-top:12px;"><strong>Regla clave:</strong> Solo cuando el despliegue pasa a "listo", el cliente migra automáticamente a Clientes Operativos. Los demás estados no deben disparar esta migración.</div>
  </div>

  <h2 class="sub-title">Estados de Cobro Mensual</h2>
  <div class="card">
    <div class="flow">
      <div class="flow-box gray">generado</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box amber">pendiente</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box green">pagado</div>
    </div>
    <div class="flow" style="margin-top:8px;">
      <div class="flow-box amber">pendiente</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box red">atrasado</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box amber">parcial</div>
      <div class="flow-arrow">→</div>
      <div class="flow-box green">pagado</div>
    </div>
    <div class="note" style="margin-top:12px;"><strong>Regla de riesgo:</strong> Si un cobro pasa a "atrasado" el sistema debe marcar automáticamente el cliente con el badge de riesgo correspondiente del catálogo.</div>
  </div>

  <h2 class="sub-title">Transiciones entre Módulos — Diagrama de Dependencia</h2>
  <div class="card">
    <pre style="font-size:12px;">
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
    </pre>
  </div>
</div>

<!-- ===== DATA MODEL ===== -->
<div id="v-data" class="view" style="padding-top:40px;">
  <h1 class="section-title">💾 Modelo de Datos Actual</h1>
  <p class="section-desc">Estructura de datos JavaScript usada actualmente en el sistema.</p>

  <h2 class="sub-title">Estructura de un Cliente (BC[])</h2>
  <div class="card">
    <pre>{
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
}</pre>
  </div>

  <h2 class="sub-title">Estructura de un Despliegue (dep[])</h2>
  <div class="card">
    <pre>{
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
}</pre>
  </div>

  <h2 class="sub-title">Estructura de un Contrato (contratos[])</h2>
  <div class="card">
    <pre>{
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
}</pre>
  </div>

  <h2 class="sub-title">Tabla Supabase (ops_data) — Esquema actual</h2>
  <div class="card">
    <pre>-- Tabla única para toda la data del sistema
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
ALTER PUBLICATION supabase_realtime ADD TABLE ops_data;</pre>
    <div class="note" style="margin-top:12px;"><strong>⚠️ Problema:</strong> Este esquema serializa arrays completos en un solo campo JSONB. No permite queries SQL eficientes ni integridad referencial. Funcional para el equipo actual, pero debe migrarse en Fase 2.</div>
  </div>
</div>

<!-- ===== DEPLOY ===== -->
<div id="v-deploy" class="view" style="padding-top:40px;">
  <h1 class="section-title">🌐 Despliegue & Operaciones</h1>
  <p class="section-desc">Infraestructura actual, acceso, y guía de actualización.</p>

  <h2 class="sub-title">Stack de Infraestructura</h2>
  <div class="two-col">
    <div class="card">
      <h3 class="card-title">Frontend — GitHub Pages</h3>
      <p style="font-size:13px;color:var(--muted);margin-bottom:12px;">URL: <code>vicentequezada-clipp.github.io/ops-center/</code></p>
      <div class="tl">
        <div class="tl-item"><div class="tl-dot done"></div><h4>Hosting gratuito</h4><p>GitHub Pages es gratis para repos públicos</p></div>
        <div class="tl-item"><div class="tl-dot done"></div><h4>CDN global</h4><p>Latencia baja desde cualquier ubicación</p></div>
        <div class="tl-item"><div class="tl-dot warn"></div><h4>Solo archivos estáticos</h4><p>No puede ejecutar código servidor. Toda la lógica es client-side.</p></div>
      </div>
    </div>
    <div class="card">
      <h3 class="card-title">Backend — Supabase</h3>
      <p style="font-size:13px;color:var(--muted);margin-bottom:12px;">Proyecto: <code>owfetilyxgvkrtdopskz.supabase.co</code></p>
      <div class="tl">
        <div class="tl-item"><div class="tl-dot done"></div><h4>PostgreSQL gestionado</h4><p>Base de datos en São Paulo (baja latencia para Ecuador)</p></div>
        <div class="tl-item"><div class="tl-dot done"></div><h4>Auth + Realtime</h4><p>Usuarios autenticados, sync en tiempo real via WebSocket</p></div>
        <div class="tl-item"><div class="tl-dot warn"></div><h4>Free tier</h4><p>Pausa proyectos inactivos por 7 días. Límite 500MB. Upgradar cuando el uso sea diario.</p></div>
      </div>
    </div>
  </div>

  <h2 class="sub-title">Guía de Actualización del Sistema</h2>
  <div class="card">
    <div class="tl">
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Paso 1: Descargar el archivo actual</h4>
        <p>En GitHub: <code>github.com/vicentequezada-clipp/ops-center</code> → click en <code>index.html</code> → botón de descarga</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Paso 2: Editar localmente</h4>
        <p>Abrir con VS Code o editor de texto. Hacer los cambios necesarios. Probar abriendo el archivo en Chrome (modo demo funciona localmente).</p>
      </div>
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Paso 3: Subir a GitHub</h4>
        <p>En GitHub → click en <code>index.html</code> → ícono de lápiz ✏️ → borrar todo → pegar nuevo código → <strong>Commit changes</strong></p>
      </div>
      <div class="tl-item">
        <div class="tl-dot done"></div>
        <h4>Paso 4: Esperar deploy automático</h4>
        <p>GitHub Pages se actualiza en ~1-2 minutos. Recargar la URL con Ctrl+Shift+R para evitar caché.</p>
      </div>
    </div>
  </div>

  <h2 class="sub-title">Gestión de Usuarios en Supabase</h2>
  <div class="card">
    <table>
      <thead><tr><th>Acción</th><th>Ruta en Supabase</th><th>Detalle</th></tr></thead>
      <tbody>
        <tr><td>Crear usuario</td><td>Authentication → Users → Add user → Create new user</td><td>Marcar "Auto Confirm User". Compartir credenciales por canal seguro.</td></tr>
        <tr><td>Resetear contraseña</td><td>Authentication → Users → click usuario → Send recovery email</td><td>O "Update password" directamente.</td></tr>
        <tr><td>Eliminar usuario</td><td>Authentication → Users → click usuario → Delete</td><td>Los datos del sistema NO se eliminan — solo el acceso.</td></tr>
        <tr><td>Ver datos BD</td><td>Table Editor → ops_data</td><td>Ver y exportar manualmente como CSV.</td></tr>
        <tr><td>SQL directo</td><td>SQL Editor</td><td>Para queries de emergencia o migración.</td></tr>
      </tbody>
    </table>
  </div>

  <h2 class="sub-title">Recomendaciones Inmediatas</h2>
  <div class="card-grid">
    <div class="card-sm">
      <h3 class="card-title">🔒 Cambiar contraseñas iniciales</h3>
      <p style="font-size:13px;color:var(--muted)">Si todos los usuarios tienen la misma contraseña inicial (ej: Clipp2026!), solicitar cambio inmediato desde Authentication → Send recovery email.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">💾 Backup semanal manual</h3>
      <p style="font-size:13px;color:var(--muted)">Cada semana: Table Editor → ops_data → exportar CSV. Guardar en Google Drive. Hasta tener backup automático.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">⬆️ Upgrade Supabase Pro</h3>
      <p style="font-size:13px;color:var(--muted)">Cuando el equipo use la app diariamente, upgradar a Pro ($25/mes) para evitar pausas por inactividad y tener backups automáticos.</p>
    </div>
    <div class="card-sm">
      <h3 class="card-title">📋 Documento de requerimientos</h3>
      <p style="font-size:13px;color:var(--muted)">Antes de la siguiente ronda de desarrollo, documentar requerimientos formalmente con criterios de aceptación para evitar retrabajo.</p>
    </div>
  </div>
</div>

</div><!-- /main -->

<script>
function show(key) {
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.querySelectorAll('.sidebar a').forEach(a => a.classList.remove('active'));
  const el = document.getElementById('v-' + key);
  if (el) el.classList.add('active');
  event.currentTarget.classList.add('active');
  window.scrollTo(0,0);
}
</script>
</body>
</html>
