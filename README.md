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
<body class="bg-gradient-to-br from-blue-50 to-blue-100 text-gray-800 font-sans p-6">

  <div id="formulario" class="max-w-5xl mx-auto bg-white p-8 rounded-3xl shadow-2xl border border-blue-200">
    <h1 class="text-3xl font-extrabold text-center text-blue-800 mb-6">📄 Informe del Partido</h1>

    <!-- Upload da Logo -->
    <div class="mb-8">
      <label class="block text-sm font-medium text-gray-700 mb-2">Logo (será usada na capa do PDF):</label>
      <input id="logoUpload" type="file" accept="image/*" class="block w-full text-sm text-gray-700 border border-gray-300 rounded-lg cursor-pointer bg-white shadow-sm focus:outline-none">
    </div>

    <!-- Informações iniciais -->
    <div class="border-l-4 border-blue-300 pl-4 mb-10">
      <h2 class="text-xl font-semibold text-blue-700 mb-4">🏟️ Datos del Partido</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label class="block text-sm font-semibold text-gray-700">Equipo A:</label>
          <input id="equipoA" type="text" class="mt-1 block w-full rounded-xl border-gray-300 shadow focus:border-blue-500 focus:ring-blue-500" placeholder="Nombre del Equipo A">
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-700">Equipo B:</label>
          <input id="equipoB" type="text" class="mt-1 block w-full rounded-xl border-gray-300 shadow focus:border-blue-500 focus:ring-blue-500" placeholder="Nombre del Equipo B">
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-700">📅 Fecha:</label>
          <input id="fecha" type="date" class="mt-1 block w-full rounded-xl border-gray-300 shadow focus:border-blue-500 focus:ring-blue-500">
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-700">🕒 Hora:</label>
          <input id="hora" type="time" class="mt-1 block w-full rounded-xl border-gray-300 shadow focus:border-blue-500 focus:ring-blue-500">
        </div>
        <div class="md:col-span-2">
          <label class="block text-sm font-semibold text-gray-700">📍 Ciudad:</label>
          <input id="ciudad" type="text" class="mt-1 block w-full rounded-xl border-gray-300 shadow focus:border-blue-500 focus:ring-blue-500" placeholder="Ciudad del Partido">
        </div>
      </div>
    </div>

    <!-- Informações do partido -->
    <div class="mb-10 border-l-4 border-blue-300 pl-4">
      <h2 class="text-xl font-semibold text-blue-700 mb-4">📝 Observaciones Generales</h2>
      <label class="block text-sm font-semibold text-gray-700 mb-2">Informaciones del Partido:</label>
      <textarea id="infoPartido" rows="5" class="block w-full rounded-xl border-gray-300 shadow focus:border-blue-500 focus:ring-blue-500" placeholder="Describa aquí los eventos y observaciones del partido..."></textarea>
    </div>

    <!-- Uploads de Fotos -->
    <div class="mb-10 border-l-4 border-blue-300 pl-4">
      <h2 class="text-xl font-semibold text-blue-700 mb-4">📷 Informe Fotográfico</h2>
      <div id="foto-uploads" class="grid grid-cols-1 gap-6"></div>
    </div>

    <div class="text-center">
      <button onclick="gerarPDF()" class="inline-flex items-center justify-center bg-blue-600 hover:bg-blue-700 text-white font-semibold px-6 py-3 rounded-xl shadow-md transition duration-300">
        📥 Exportar como PDF
      </button>
    </div>
  </div>

  <script>
    const descripciones = [
      "Arribo delegación visitante", "Llegada del equipo visitante al hotel y seguridad del hotel",
      "Inspección de seguridad", "Reunión de Seguridad", "Reunión de Coordinación", "Instalación de Objetos de animación",
      "Llegada utilería equipo Visitante", "Utileria equipo local", "Llegada Delegación visitante", "Llegada delegación local",
      "Llegada equipo de arbitraje", "Charla OSC con seguridad privada", "Primera revisión interna y externa",
      "Instalación de Recursos", "Situación das tribunas protocolo de juego", "Situacion externa a los 70’",
      "Refuerzo a los 75’", "Evacuación Completa en 45’", "Salida del equipo local", "Salida del arbitraje",
      "Salida del equipo local por sus medios", "Comer cerrado", "Regreso del equipo visitante"
    ];

    window.addEventListener("DOMContentLoaded", () => {
      const container = document.getElementById("foto-uploads");
      descripciones.forEach((desc, index) => {
        const campo = document.createElement("div");
        campo.className = "mb-4";
        campo.innerHTML = `
          <label class="block font-semibold mb-1">${index + 1} - ${desc}</label>
          <input type="file" accept="image/*" class="w-full border p-2 rounded">
        `;
        container.appendChild(campo);
      });
    });

    function gerarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF('p', 'pt', 'a4');

      const equipoA = document.getElementById("equipoA").value;
      const equipoB = document.getElementById("equipoB").value;
      const fecha = document.getElementById("fecha").value;
      const hora = document.getElementById("hora").value;
      const ciudad = document.getElementById("ciudad").value;
      const infoPartido = document.getElementById("infoPartido").value;
      const logoFile = document.getElementById("logoUpload").files[0];

      const gerarCapa = (logoBase64) => {
        if (logoBase64) {
          doc.addImage(logoBase64, 'JPEG', 200, 40, 200, 100);
        }
        doc.setFontSize(18);
        doc.text("Informe del Partido", 220, 160);
        doc.setFontSize(14);
        doc.text(`Equipo A: ${equipoA}`, 80, 200);
        doc.text(`Equipo B: ${equipoB}`, 80, 220);
        doc.text(`Fecha: ${fecha}`, 80, 240);
        doc.text(`Hora: ${hora}`, 80, 260);
        doc.text(`Ciudad: ${ciudad}`, 80, 280);

        doc.addPage();
        doc.setFontSize(16);
        doc.text("Informaciones del Partido", 40, 60);
        doc.setFontSize(12);
        const texto = doc.splitTextToSize(infoPartido, 500);
        doc.text(texto, 40, 80);
      };

      const gerarFotos = async () => {
        doc.addPage();
        doc.setFontSize(16);
        doc.text("Informe Fotográfico", 40, 40);

        const camposImagem = document.querySelectorAll('#foto-uploads input[type="file"]');
        const labels = document.querySelectorAll('#foto-uploads label');

        let y = 60;

        for (let i = 0; i < camposImagem.length; i++) {
          const file = camposImagem[i].files[0];
          if (file) {
            const img = await carregarImagem(file);
            doc.setFontSize(12);
            doc.text(labels[i].innerText, 40, y);
            y += 10;

            const canvas = document.createElement("canvas");
            const ctx = canvas.getContext("2d");
            canvas.width = img.width;
            canvas.height = img.height;
            ctx.drawImage(img, 0, 0);
            const imgData = canvas.toDataURL("image/jpeg", 0.8);

            const pdfWidth = 500;
            const ratio = img.height / img.width;
            const pdfHeight = pdfWidth * ratio;

            if (y + pdfHeight > 800) {
              doc.addPage();
              y = 40;
            }
            doc.addImage(imgData, 'JPEG', 40, y, pdfWidth, pdfHeight);
            y += pdfHeight + 20;
          }
        }
        doc.save("informe_partido_fotos.pdf");
      };

      const carregarImagem = (file) => {
        return new Promise((resolve, reject) => {
          const reader = new FileReader();
          reader.onload = e => {
            const img = new Image();
            img.onload = () => resolve(img);
            img.src = e.target.result;
          };
          reader.onerror = reject;
          reader.readAsDataURL(file);
        });
      };

      const carregarLogoBase64 = (file) => {
        return new Promise((resolve, reject) => {
          if (!file) return resolve(null);
          const reader = new FileReader();
          reader.onload = e => resolve(e.target.result);
          reader.onerror = reject;
          reader.readAsDataURL(file);
        });
      };

      carregarLogoBase64(logoFile).then(logoBase64 => {
        gerarCapa(logoBase64);
        gerarFotos();
      });
    }
  </script>

</body>
</html>

