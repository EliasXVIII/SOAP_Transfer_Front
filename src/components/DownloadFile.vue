<template>
    <div class="container">
      <h2 class="title">Descargar Archivo</h2>
      <input type="text" v-model="fileName" placeholder="Nombre del archivo" class="input" />
      <button @click="downloadFile" class="button">Descargar</button>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    name: 'DownloadFile',
    setup() {
      const fileName = ref('');
  
      const downloadFile = async () => {
        if (!fileName.value) {
          alert('Introduce el nombre del archivo');
          return;
        }
  
        // Construir el mensaje SOAP Atencion a este paso 
        const soapMessage = `
          <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
            <soapenv:Header/>
            <soapenv:Body>
              <tem:DownloadFile>
                <tem:fileName>${fileName.value}</tem:fileName>
              </tem:DownloadFile>
            </soapenv:Body>
          </soapenv:Envelope>
        `;
  
        try {
          const response = await fetch('http://localhost:5177/FileService.asmx', {
            method: 'POST',
            headers: {
              'Content-Type': 'text/xml; charset=utf-8',
              'SOAPAction': 'http://tempuri.org/IFileService/DownloadFile'
            },
            body: soapMessage
          });
  
          if (response.ok) {
            const responseText = await response.text(); 
  
            
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(responseText, 'text/xml');
            
            // Extraer el contenido del archivo desde la respuesta SOAP
            const base64FileContent = xmlDoc.getElementsByTagName('DownloadFileResult')[0].textContent;
  
            // Decodificar el contenido base64
            const binaryData = atob(base64FileContent);
  
            
            const blob = new Blob([binaryData], { type: 'application/octet-stream' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.style.display = 'none';
            a.href = url;
            a.download = fileName.value;
            document.body.appendChild(a);
            a.click();
            window.URL.revokeObjectURL(url);
          } else {
            alert('Error al descargar el archivo');
          }
        } catch (error) {
          console.error('Error:', error);
        }
      };
  
      return { fileName, downloadFile };
    }
  };
  </script>
  
  <style scoped>
  .container {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    margin: auto;
    margin-top: 20px;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    background-color: #f9f9f9;
  }
  
  .title {
    font-size: 24px;
    margin-bottom: 20px;
    color: #333;
    text-align: center;
  }
  
  .input {
    width: 95%;
    padding: 10px;
    margin-bottom: 20px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
  }
  
  .button {
    display: block;
    width: 100%;
    padding: 10px;
    font-size: 16px;
    color: #fff;
    background-color: #007bff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .button:hover {
    background-color: #0056b3;
  }
  </style>
  