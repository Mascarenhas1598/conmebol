# conmebol

<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Informe del Partido</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-800 font-sans p-6">

  <div class="max-w-4xl mx-auto bg-white p-6 rounded-xl shadow-md">
    <h1 class="text-2xl font-bold mb-4">Informe del Partido</h1>

    <!-- Informações iniciais -->
    <div class="grid grid-cols-2 gap-4 mb-6">
      <div>
        <label class="font-semibold">Equipo A:</label>
        <input type="text" class="w-full border rounded p-2" placeholder="Nombre del Equipo A">
      </div>
      <div>
        <label class="font-semibold">Equipo B:</label>
        <input type="text" class="w-full border rounded p-2" placeholder="Nombre del Equipo B">
      </div>
      <div>
        <label class="font-semibold">Fecha:</label>
        <input type="date" class="w-full border rounded p-2">
      </div>
      <div>
        <label class="font-semibold">Hora:</label>
        <input type="time" class="w-full border rounded p-2">
      </div>
      <div class="col-span-2">
        <label class="font-semibold">Ciudad:</label>
        <input type="text" class="w-full border rounded p-2" placeholder="Ciudad del Partido">
      </div>
    </div>

    <!-- Uploads de Fotos -->
    <h2 class="text-xl font-bold mt-8 mb-4">Informe Fotográfico</h2>
    <div class="grid grid-cols-1 gap-4">
      <!-- Exemplo de upload -->
      <div>
        <label class="block font-semibold mb-1">1 - Arribo delegación visitante</label>
        <input type="file" accept="image/*" class="w-full border p-2 rounded">
      </div>
      <div>
        <label class="block font-semibold mb-1">2 - Llegada del equipo visitante al hotel y seguridad del hotel</label>
        <input type="file" accept="image/*" class="w-full border p-2 rounded">
      </div>
      <!-- ... repete até 23 -->
    </div>

    <button class="mt-8 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
      Enviar Informe
    </button>
  </div>

</body>
</html>
