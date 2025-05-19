<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Informe del Partido</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
</head>
<body class="bg-gray-900 text-gray-100 font-sans p-6">

  <div id="formulario" class="max-w-5xl mx-auto bg-gray-800 p-8 rounded-3xl shadow-2xl border border-gray-700">
    <h1 class="text-3xl font-extrabold text-center text-blue-400 mb-6">📄 Informe del Partido</h1>

    <!-- Upload da Logo -->
    <div class="mb-8">
      <label class="block text-sm font-medium text-gray-200 mb-2">Logo (será usada na capa do PDF):</label>
      <input id="logoUpload" type="file" accept="image/*" class="block w-full text-sm text-gray-200 border border-gray-600 rounded-xl cursor-pointer bg-gray-700 shadow-sm focus:outline-none">
    </div>

    <!-- Informações iniciais -->
    <div class="border-l-4 border-blue-400 pl-4 mb-10">
      <h2 class="text-xl font-semibold text-blue-300 mb-4">🏟️ Datos del Partido</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label class="block text-sm font-semibold text-gray-200">Equipo A:</label>
          <input id="equipoA" type="text" class="mt-1 block w-full rounded-xl border border-gray-600 bg-gray-700 text-white shadow focus:border-blue-400 focus:ring-blue-400" placeholder="Nombre del Equipo A">
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-200">Equipo B:</label>
          <input id="equipoB" type="text" class="mt-1 block w-full rounded-xl border border-gray-600 bg-gray-700 text-white shadow focus:border-blue-400 focus:ring-blue-400" placeholder="Nombre del Equipo B">
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-200">📅 Fecha:</label>
          <input id="fecha" type="date" class="mt-1 block w-full rounded-xl border border-gray-600 bg-gray-700 text-white shadow focus:border-blue-400 focus:ring-blue-400">
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-200">🕒 Hora:</label>
          <input id="hora" type="time" class="mt-1 block w-full rounded-xl border border-gray-600 bg-gray-700 text-white shadow focus:border-blue-400 focus:ring-blue-400">
        </div>
        <div class="md:col-span-2">
          <label class="block text-sm font-semibold text-gray-200">📍 Ciudad:</label>
          <input id="ciudad" type="text" class="mt-1 block w-full rounded-xl border border-gray-600 bg-gray-700 text-white shadow focus:border-blue-400 focus:ring-blue-400" placeholder="Ciudad del Partido">
        </div>
      </div>
    </div>

    <!-- Informações do partido -->
    <div class="mb-10 border-l-4 border-blue-400 pl-4">
      <h2 class="text-xl font-semibold text-blue-300 mb-4">📝 Observaciones Generales</h2>
      <label class="block text-sm font-semibold text-gray-200 mb-2">Informaciones del Partido:</label>
      <textarea id="infoPartido" rows="5" class="block w-full rounded-xl border border-gray-600 bg-gray-700 text-white shadow focus:border-blue-400 focus:ring-blue-400" placeholder="Describa aquí los eventos y observaciones del partido..."></textarea>
    </div>

    <!-- Uploads de Fotos -->
    <div class="mb-10 border-l-4 border-blue-400 pl-4">
      <h2 class="text-xl font-semibold text-blue-300 mb-4">📷 Informe Fotográfico</h2>
      <div id="foto-uploads" class="grid grid-cols-1 gap-6"></div>
    </div>

    <div class="text-center">
      <button onclick="gerarPDF()" class="inline-flex items-center justify-center bg-blue-500 hover:bg-blue-600 text-white font-semibold px-6 py-3 rounded-xl shadow-md transition duration-300">
        📥 Exportar como PDF
      </button>
    </div>
  </div>

</body>
</html>
