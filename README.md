<script>
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
