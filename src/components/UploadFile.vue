<template>
    <div class="upload-container">
      <h2 class="upload-title">Subir Archivo</h2>
      <input type="file" @change="onFileChange" class="file-input" />
      <button @click="uploadFile" class="upload-button">Subir</button>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    name: 'UploadFile',
    setup() {
      const file = ref(null);
  
      const onFileChange = (event) => {
        file.value = event.target.files[0];
      };
  
      const uploadFile = async () => {
        if (!file.value) {
          alert('Selecciona un archivo primero');
          return;
        }
  
        const reader = new FileReader();
        reader.onload = async (event) => {
          const fileContent = event.target.result;
          
          
          const base64FileContent = btoa(
            new Uint8Array(fileContent).reduce((data, byte) => data + String.fromCharCode(byte), '')
          );
  
          const soapMessage = `
            <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
              <soapenv:Header/>
              <soapenv:Body>
                <tem:UploadFile>
                  <tem:fileBytes>${base64FileContent}</tem:fileBytes>
                  <tem:fileName>${file.value.name}</tem:fileName>
                </tem:UploadFile>
              </soapenv:Body>
            </soapenv:Envelope>
          `;
  
          try {
            const response = await fetch('http://localhost:5177/FileService.asmx', {
              method: 'POST',
              headers: {
                'Content-Type': 'text/xml; charset=utf-8',
                'SOAPAction': 'http://tempuri.org/IFileService/UploadFile'
              },
              body: soapMessage
            });
  
            if (response.ok) {
              alert('Archivo subido exitosamente');
            } else {
              alert(`Error al subir el archivo: ${response.statusText}`);
            }
          } catch (error) {
            console.error('Error:', error);
            alert('Error al subir el archivo');
          }
        };
  
        reader.readAsArrayBuffer(file.value);
      };
  
      return { onFileChange, uploadFile };
    }
  };
  </script>
  
  <style scoped>
  .upload-container {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    margin: auto;
    margin-top: 20px;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    background-color: #f9f9f9;
    text-align: center;
  }
  
  .upload-title {
    font-size: 24px;
    margin-bottom: 20px;
    color: #333;
  }
  
  .file-input {
    margin-bottom: 20px;
  }
  
  .upload-button {
    padding: 10px 20px;
    font-size: 16px;
    color: #fff;
    background-color: #28a745;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .upload-button:hover {
    background-color: #218838;
  }
  </style>
  