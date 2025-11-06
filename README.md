<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gestor Académico</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #f5f7fa;
    margin: 0;
    padding: 0;
  }

  header {
    background: #2f3e46;
    color: white;
    text-align: center;
    padding: 1rem 0;
    font-size: 1.4rem;
    font-weight: bold;
    letter-spacing: 1px;
  }

  .tabs {
    display: flex;
    background: #354f52;
  }

  .tab {
    flex: 1;
    text-align: center;
    padding: 1rem;
    color: white;
    cursor: pointer;
    transition: background 0.3s;
  }

  .tab:hover, .tab.active {
    background: #52796f;
  }

  .content {
    display: none;
    padding: 1rem;
    background: white;
  }

  .content.active {
    display: block;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 1rem;
  }

  th, td {
    border: 1px solid #ccc;
    padding: 0.7rem;
    text-align: center;
  }

  th {
    background: #e9ecef;
  }

  input[type="text"], input[type="date"], input[type="number"] {
    width: 90%;
    padding: 0.4rem;
  }

  button {
    background: #52796f;
    color: white;
    border: none;
    padding: 0.6rem 1.2rem;
    border-radius: 5px;
    cursor: pointer;
    transition: 0.3s;
  }

  button:hover {
    background: #354f52;
  }

  .resultado {
    font-size: 1.2rem;
    text-align: center;
    font-weight: bold;
    padding: 1rem;
    border-radius: 8px;
    margin-top: 1rem;
  }
</style>
</head>
<body>
  <header>Gestor Académico</header>

  <div class="tabs">
    <div class="tab active" onclick="showTab('tareas')">Tareas</div>
    <div class="tab" onclick="showTab('examenes')">Exámenes</div>
  </div>

  <div id="tareas" class="content active">
    <h2>Tareas</h2>
    <table id="tablaTareas">
      <thead>
        <tr>
          <th>Módulo</th>
          <th>Fecha de Entrega</th>
          <th>Entregada</th>
          <th>Observaciones</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
    <button onclick="agregarTarea()">Añadir Tarea</button>
  </div>

  <div id="examenes" class="content">
    <h2>Exámenes</h2>
    <table id="tablaExamenes">
      <thead>
        <tr>
          <th>Fecha</th>
          <th>Módulo</th>
          <th>Tema</th>
          <th>Nota</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
    <button onclick="agregarExamen()">Añadir Examen</button>

    <div class="resultado" id="resultadoMedia">Nota media: -</div>
  </div>

<script>
  function showTab(tabId) {
    document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
    document.querySelectorAll('.content').forEach(c => c.classList.remove('active'));
    document.querySelector(`.tab[onclick="showTab('${tabId}')"]`).classList.add('active');
    document.getElementById(tabId).classList.add('active');
  }

  function agregarTarea() {
    const tabla = document.getElementById('tablaTareas').querySelector('tbody');
    const fila = document.createElement('tr');
    fila.innerHTML = `
      <td><input type="text" placeholder="Nombre del módulo"></td>
      <td><input type="date"></td>
      <td><input type="text" placeholder="Sí/No"></td>
      <td><input type="text" placeholder="Observaciones"></td>
    `;
    tabla.appendChild(fila);
  }

  function agregarExamen() {
    const tabla = document.getElementById('tablaExamenes').querySelector('tbody');
    const fila = document.createElement('tr');
    fila.innerHTML = `
      <td><input type="date"></td>
      <td><input type="text" placeholder="Nombre del módulo"></td>
      <td><input type="text" placeholder="Tema"></td>
      <td><input type="number" min="0" max="10" step="0.1" oninput="calcularMedia()"></td>
    `;
    tabla.appendChild(fila);
  }

  function calcularMedia() {
    const notas = Array.from(document.querySelectorAll('#tablaExamenes tbody input[type="number"]'))
      .map(n => parseFloat(n.value))
      .filter(n => !isNaN(n));

    if (notas.length === 0) {
      document.getElementById('resultadoMedia').textContent = 'Nota media: -';
      document.getElementById('resultadoMedia').style.background = '';
      return;
    }

    const media = notas.reduce((a, b) => a + b, 0) / notas.length;
    const resultado = document.getElementById('resultadoMedia');
    resultado.textContent = `Nota media: ${media.toFixed(2)} `;

    let color = "", mensaje = "";
    if (media >= 9) { color = "blue"; mensaje = "Vas estupendamente"; }
    else if (media >= 7) { color = "green"; mensaje = "Bien, si puedes dar más, hazlo"; }
    else if (media >= 5) { color = "yellow"; mensaje = "Vas un poco justo, intenta hacerlo mejor"; }
    else if (media >= 3) { color = "orange"; mensaje = "Creo que te debes centrar más en los estudios"; }
    else if (media >= 1) { color = "red"; mensaje = "Ponte las pilas pero ya"; }
    else { color = "black"; mensaje = "Replanteate lo que haces"; }

    resultado.style.background = color;
    resultado.style.color = (color === "yellow") ? "black" : "white";
    resultado.textContent += ` → ${mensaje}`;
  }
</script>
</body>
</html>
