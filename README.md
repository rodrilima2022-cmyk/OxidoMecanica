
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gestión de Taller Mecánico</title>
  <style>
    :root {
      --primary: #1e293b;
      --accent: #d97706;
      --bg: #f8fafc;
      --surface: #ffffff;
      --text: #0f172a;
      --border: #e2e8f0;
      --success: #16a34a;
      --danger: #dc2626;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, -apple-system, sans-serif; }
    body { background-color: var(--bg); color: var(--text); padding: 20px; }
    .container { max-width: 1200px; margin: 0 auto; }

    /* Header & Logo */
    header { display: flex; justify-content: space-between; align-items: center; background: var(--primary); color: white; padding: 20px; border-radius: 8px; margin-bottom: 20px; }
    .brand { display: flex; align-items: center; gap: 15px; }
    .brand img { max-height: 50px; border-radius: 4px; background: white; padding: 2px; }
    .brand h1 { font-size: 1.5rem; }

    /* Key Metrics Dashboard */
    .metrics-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 20px; }
    .metric-card { background: var(--surface); padding: 15px; border-radius: 8px; border: 1px solid var(--border); }
    .metric-card h3 { font-size: 0.85rem; color: #64748b; text-transform: uppercase; }
    .metric-card .value { font-size: 1.5rem; font-weight: bold; margin-top: 5px; }
    .metric-card .value.positive { color: var(--success); }
    .metric-card .value.negative { color: var(--danger); }

    /* Forms and Inputs */
    .panel { background: var(--surface); padding: 20px; border-radius: 8px; border: 1px solid var(--border); margin-bottom: 20px; }
    .panel h2 { font-size: 1.1rem; margin-bottom: 15px; color: var(--primary); }
    .form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 12px; }
    .form-group { display: flex; flex-direction: column; gap: 5px; }
    .form-group label { font-size: 0.8rem; font-weight: 600; }
    input, select, textarea { padding: 8px 12px; border: 1px solid var(--border); border-radius: 4px; font-size: 0.9rem; }
    button { background: var(--accent); color: white; border: none; padding: 10px 15px; border-radius: 4px; cursor: pointer; font-weight: bold; }
    button:hover { opacity: 0.9; }

    /* Search & Table */
    .search-bar { width: 100%; padding: 10px; margin-bottom: 15px; font-size: 1rem; }
    .table-container { overflow-x: auto; }
    table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
    th, td { text-align: left; padding: 12px; border-bottom: 1px solid var(--border); }
    th { background: #f1f5f9; color: var(--primary); }
    .btn-danger { background: var(--danger); padding: 4px 8px; font-size: 0.8rem; }
  </style>
</head>
<body>

<div class="container">
  <!-- HEADER CON LOGO -->
  <header>
    <div class="brand">
      <img id="logo-preview" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='40' height='40' viewBox='0 0 24 24' fill='none' stroke='%231e293b' stroke-width='2'%3E%3Cpath d='M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z'/%3E%3C/svg%3E" alt="Logo">
      <h1 id="shop-title">Oxido Mecanica - Control de Gestión</h1>
    </div>
    <div>
      <input type="file" id="logo-input" accept="image/*" style="display:none;" onchange="uploadLogo(event)">
      <button onclick="document.getElementById('logo-input').click()" style="background:#475569; font-size:0.8rem;">Cambiar Logo</button>
    </div>
  </header>

  <!-- METRICAS Y ESTADISTICAS -->
  <div class="metrics-grid">
    <div class="metric-card">
      <h3>Autos Atendidos (Total)</h3>
      <div class="value" id="st-total-autos">0</div>
    </div>
    <div class="metric-card">
      <h3>Autos (Últimos 7 días)</h3>
      <div class="value" id="st-weekly-autos">0</div>
    </div>
    <div class="metric-card">
      <h3>Ingresos Totales</h3>
      <div class="value positive" id="st-ingresos">$0</div>
    </div>
    <div class="metric-card">
      <h3>Costos Totales</h3>
      <div class="value negative" id="st-costos">$0</div>
    </div>
    <div class="metric-card">
      <h3>Ganancia Neta</h3>
      <div class="value" id="st-ganancia">$0</div>
    </div>
  </div>

  <!-- FORMULARIO DE INGRESO -->
  <div class="panel">
    <h2>Registrar Nuevo Service / Trabajo</h2>
    <form id="service-form" onsubmit="addRecord(event)">
      <div class="form-grid">
        <div class="form-group">
          <label>Cliente</label>
          <input type="text" id="cliente" required placeholder="Nombre completo">
        </div>
        <div class="form-group">
          <label>Teléfono</label>
          <input type="text" id="telefono" placeholder="Ej: 11 1234-5678">
        </div>
        <div class="form-group">
          <label>Vehículo / Modelo</label>
          <input type="text" id="vehiculo" required placeholder="Ej: VW Gol 1.6">
        </div>
        <div class="form-group">
          <label>Patente</label>
          <input type="text" id="patente" required placeholder="AA123CD">
        </div>
        <div class="form-group">
          <label>Kilometraje</label>
          <input type="number" id="km" required placeholder="120000">
        </div>
        <div class="form-group">
          <label>Fecha del Service</label>
          <input type="date" id="fecha" required>
        </div>
        <div class="form-group">
          <label>Costo del Taller ($)</label>
          <input type="number" id="costo" step="0.01" required placeholder="Repuestos, insumos">
        </div>
        <div class="form-group">
          <label>Cobrado al Cliente ($)</label>
          <input type="number" id="cobrado" step="0.01" required placeholder="Precio final">
        </div>
      </div>
      <div class="form-group" style="margin-top: 10px;">
        <label>Detalle del Trabajo / Próxima Revisión</label>
        <input type="text" id="detalle" placeholder="Ej: Cambio de aceite y filtro. Próximo service a los 130.000 km">
      </div>
      <button type="submit" style="margin-top: 15px; width: 100%;">Guardar Registro</button>
    </form>
  </div>

  <!-- GRILLA Y BUSCADOR -->
  <div class="panel">
    <h2>Historial de Clientes y Revisione</h2>
    <input type="text" id="search" class="search-bar" placeholder="🔍 Buscar por cliente, patente, modelo..." oninput="renderTable()">
    
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th>Fecha</th>
            <th>Cliente / Tel</th>
            <th>Vehículo</th>
            <th>Patente</th>
            <th>Km</th>
            <th>Detalle / Pendientes</th>
            <th>Costo</th>
            <th>Cobrado</th>
            <th>Acción</th>
          </tr>
        </thead>
        <tbody id="table-body">
          <!-- Filas dinámicas -->
        </tbody>
      </table>
    </div>
  </div>
</div>

<script>
  let db = JSON.parse(localStorage.getItem('taller_db')) || [];

  document.getElementById('fecha').valueAsDate = new Date();

  function saveStorage() {
    localStorage.setItem('taller_db', JSON.stringify(db));
  }

  function addRecord(e) {
    e.preventDefault();
    const record = {
      id: Date.now(),
      cliente: document.getElementById('cliente').value,
      telefono: document.getElementById('telefono').value,
      vehiculo: document.getElementById('vehiculo').value,
      patente: document.getElementById('patente').value.toUpperCase(),
      km: Number(document.getElementById('km').value),
      fecha: document.getElementById('fecha').value,
      costo: Number(document.getElementById('costo').value),
      cobrado: Number(document.getElementById('cobrado').value),
      detalle: document.getElementById('detalle').value
    };

    db.unshift(record);
    saveStorage();
    document.getElementById('service-form').reset();
    document.getElementById('fecha').valueAsDate = new Date();
    updateAll();
  }

  function deleteRecord(id) {
    if(confirm('¿Seguro que querés borrar este registro?')) {
      db = db.filter(item => item.id !== id);
      saveStorage();
      updateAll();
    }
  }

  function renderMetrics() {
    const totalAutos = db.length;
    
    // Filtro semanal (últimos 7 días)
    const now = new Date();
    const sevenDaysAgo = new Date(now.setDate(now.getDate() - 7));
    const weeklyAutos = db.filter(item => new Date(item.fecha) >= sevenDaysAgo).length;

    const totalIngresos = db.reduce((acc, item) => acc + item.cobrado, 0);
    const totalCostos = db.reduce((acc, item) => acc + item.costo, 0);
    const ganancia = totalIngresos - totalCostos;

    document.getElementById('st-total-autos').innerText = totalAutos;
    document.getElementById('st-weekly-autos').innerText = weeklyAutos;
    document.getElementById('st-ingresos').innerText = `$${totalIngresos.toLocaleString()}`;
    document.getElementById('st-costos').innerText = `$${totalCostos.toLocaleString()}`;
    
    const gananciaEl = document.getElementById('st-ganancia');
    gananciaEl.innerText = `$${ganancia.toLocaleString()}`;
    gananciaEl.className = `value ${ganancia >= 0 ? 'positive' : 'negative'}`;
  }

  function renderTable() {
    const query = document.getElementById('search').value.toLowerCase();
    const tbody = document.getElementById('table-body');
    tbody.innerHTML = '';

    const filtered = db.filter(item => 
      item.cliente.toLowerCase().includes(query) ||
      item.patente.toLowerCase().includes(query) ||
      item.vehiculo.toLowerCase().includes(query)
    );

    filtered.forEach(item => {
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td>${item.fecha}</td>
        <td><strong>${item.cliente}</strong><br><small>${item.telefono}</small></td>
        <td>${item.vehiculo}</td>
        <td><strong>${item.patente}</strong></td>
        <td>${item.km.toLocaleString()} km</td>
        <td>${item.detalle}</td>
        <td style="color:var(--danger)">$${item.costo.toLocaleString()}</td>
        <td style="color:var(--success)">$${item.cobrado.toLocaleString()}</td>
        <td><button class="btn-danger" onclick="deleteRecord(${item.id})">X</button></td>
      `;
      tbody.appendChild(tr);
    });
  }

  function uploadLogo(e) {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = function(evt) {
        document.getElementById('logo-preview').src = evt.target.result;
        localStorage.setItem('taller_logo', evt.target.result);
      };
      reader.readAsDataURL(file);
    }
  }

  function loadLogo() {
    const savedLogo = localStorage.getItem('taller_logo');
    if (savedLogo) {
      document.getElementById('logo-preview').src = savedLogo;
    }
  }

  function updateAll() {
    renderMetrics();
    renderTable();
  }

  // Carga inicial
  loadLogo();
  updateAll();
</script>
</body>
</html>
