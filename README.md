# conmebol
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
<body class="bg-gray-100 text-gray-800 font-sans p-6">

  <div id="formulario" class="max-w-5xl mx-auto bg-white p-6 rounded-xl shadow-md">
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
    <div id="foto-uploads" class="grid grid-cols-1 gap-4 mb-8"></div>

    <button onclick="gerarPDF()" class="mt-8 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
      Exportar como PDF
    </button>
  </div>

  <script>
    function gerarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF('p', 'pt', 'a4');

      const camposImagem = document.querySelectorAll('#foto-uploads input[type="file"]');
      const labels = document.querySelectorAll('#foto-uploads label');

      let y = 40;
      doc.setFontSize(16);
      doc.text("Informe Fotográfico", 40, y);
      y += 20;

      const processarImagens = async () => {
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

      function carregarImagem(file) {
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
      }

      processarImagens();
    }
  </script>

  <script>
    const descripciones = [
      "Arribo delegación visitante",
      "Llegada del equipo visitante al hotel y seguridad del hotel",
      "Inspección de seguridad",
      "Reunión de Seguridad",
      "Reunión de Coordinación",
      "Instalación de Objetos de animación",
      "Llegada utilería equipo Visitante",
      "Utileria equipo local",
      "Llegada Delegación visitante",
      "Llegada delegación local",
      "Llegada equipo de arbitraje",
      "Charla OSC con seguridad privada",
      "Primera revisión interna y externa",
      "Instalación de Recursos",
      "Situación das tribunas protocolo de juego",
      "Situacion externa a los 70’",
      "Refuerzo a los 75’",
      "Evacuación Completa en 45’",
      "Salida del equipo local",
      "Salida del arbitraje",
      "Salida del equipo local por sus medios",
      "Comer cerrado",
      "Regreso del equipo visitante"
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
  </script>

</body>
</html>

        
