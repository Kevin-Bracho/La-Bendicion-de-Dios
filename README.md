<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>La Bendición de Dios</title>
  
  <style>
    /* Estilos generales */
    body {
      margin: 0;
      padding: 0;
      background: url('quesillo.png') no-repeat center center fixed;
      background-size: cover;
      min-height: 100vh;
      font-family: 'Poppins', sans-serif;
      text-align: center;
      color: white;
    }
    .title-container {
      width: 100%;
      background: linear-gradient(90deg, #8B4513, #D2691E);
      padding: 20px 0;
      position: fixed;
      top: 0;
      left: 0;
      z-index: 1000;
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
    }
    h1 {
      font-size: 1.8rem;
      margin: 10px 0 0 0;
      font-weight: bold;
      letter-spacing: 1px;
      text-transform: uppercase;
      color: white;
    }
    p {
      font-size: 1rem;
      margin-top: 15px;
      font-style: italic;
      opacity: 0.9;
      color: white;
    }
    .content-container {
      margin-top: 100px;
      font-size: 1.5rem;
      font-weight: bold;
      color: white;
    }
    .price-container {
      margin: 20px auto 0;
      padding: 15px;
      background: linear-gradient(90deg, #D2691E, #FFA07A);
      border-radius: 10px;
      width: 60%;
      display: inline-block;
      font-size: 1.2rem;
      font-weight: bold;
      color: white;
    }
    .price-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 5px 0;
      cursor: pointer;
      border-bottom: 1px solid rgba(255, 255, 255, 0.3);
    }
    .price-item:last-child {
      border-bottom: none;
    }
    .custom-checkbox {
      width: 16px;
      height: 16px;
      border: 2px solid white;
      display: inline-block;
      margin-right: 6px;
      flex-shrink: 0;
      text-align: center;
      line-height: 14px;
      font-size: 12px;
    }
    .price-item input[type="checkbox"] {
      display: none;
    }
    .price-item input[type="checkbox"]:checked + .custom-checkbox::after {
      content: "✓";
      color: white;
    }
    .price-text {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      color: white;
    }
    label:focus, input:focus, .custom-checkbox:focus {
      outline: none;
      box-shadow: none;
    }
    #order-message {
      margin: 15px auto 0;
      width: 60%;
      font-size: 0.9rem;
      font-weight: bold;
      text-align: center;
      color: white;
      display: none;
    }
    .input-container {
      margin: 10px auto 0;
      padding: 10px;
      background: linear-gradient(90deg, #D2691E, #FFA07A);
      border-radius: 10px;
      width: 60%;
      display: inline-block;
      color: white;
      opacity: 0;
      transition: opacity 0.5s ease;
    }
    .input-container input[type="number"] {
      width: 90%;
      padding: 8px;
      border: none;
      background: transparent;
      font-family: 'Poppins', sans-serif;
      font-size: 1rem;
      text-align: center;
      color: white;
    }
    .visible {
      opacity: 1;
    }
    #total-display {
      margin: 15px auto 0;
      width: 60%;
      font-size: 1rem;
      font-weight: bold;
      text-align: center;
      color: white;
      display: none;
    }
    /* Contenedor para datos de copiado */
    #copy-info {
      margin: 20px auto;
      width: 60%;
      display: none;
      text-align: center;
    }
    #copy-info p {
      margin: 10px 0 5px 0;
      font-size: 1rem;
      font-weight: bold;
    }
    #copy-info button {
      background: linear-gradient(90deg, #D2691E, #FFA07A);
      border: none;
      padding: 5px 10px;
      border-radius: 5px;
      font-size: 0.9rem;
      color: white;
      cursor: pointer;
      margin-bottom: 15px;
    }
    /* Contenedor y estilos para imágenes */
    #image-container {
      margin: 20px auto;
      width: 60%;
      display: none;
      text-align: center;
    }
    #image-container img {
      width: 80%;
      display: block;
      margin: 15px auto;
      cursor: pointer;
      border: 2px solid transparent;
      transition: border 0.3s ease;
    }
    #image-container img.selected {
      border: 2px solid #FFFFFF;
    }
    /* Contenedor para el botón "Siguiente" */
    #next-container {
      margin: 20px auto;
      width: 60%;
      display: none;
      text-align: center;
    }
    #next-container button {
      background: linear-gradient(90deg, #D2691E, #FFA07A);
      border: none;
      padding: 8px 16px;
      border-radius: 5px;
      font-size: 1rem;
      color: white;
      cursor: pointer;
    }
  </style>
  
  <script>
    // Variables de precio
    const COMBO_PRICE = 2.70;
    const QUESILLO_PRICE = 1.70;
    const AREQUIPE_PRICE = 0.60;
    
    // URLs asociadas a cada imagen
    const urlVenezuela = "https://www.bancodevenezuela.com/";
    const urlProvincial = "https://www.provincial.com/personas.html";
    const urlBicentenario = "https://www.bdt.com.ve";
    
    // Variable para almacenar la URL de la imagen seleccionada
    let selectedImageUrl = "";
    
    // Variables bandera para el copiado
    let copied04128187076 = false;
    let copied13519438 = false;
    
    // Actualiza el mensaje dinámico según la selección de productos
    function updateOrderMessage() {
      const quesilloChecked = document.getElementById("quesillo").checked;
      const arequipeChecked = document.getElementById("arequipe").checked;
      let message = "";
      if (quesilloChecked && arequipeChecked) {
        message = "Cuántos Combos Desea Comprar";
      } else if (quesilloChecked) {
        message = "Cuántos Quesillos Desea Comprar";
      } else if (arequipeChecked) {
        message = "Cuántos Arequipes Desea Comprar";
      }
      document.getElementById("order-message").innerText = message;
      if (message !== "") {
        document.getElementById("order-message").style.display = "block";
        document.querySelector(".input-container").classList.add("visible");
      } else {
        document.getElementById("order-message").style.display = "none";
        document.querySelector(".input-container").classList.remove("visible");
      }
      updateTotal();
    }
    
    // Calcula el total según la cantidad ingresada y la selección
    function updateTotal() {
      const quantity = parseFloat(document.querySelector(".input-container input[type='number']").value) || 0;
      const quesilloChecked = document.getElementById("quesillo").checked;
      const arequipeChecked = document.getElementById("arequipe").checked;
      let unitPrice = 0;
      if (quesilloChecked && arequipeChecked) {
        unitPrice = COMBO_PRICE;
      } else if (quesilloChecked) {
        unitPrice = QUESILLO_PRICE;
      } else if (arequipeChecked) {
        unitPrice = AREQUIPE_PRICE;
      }
      const total = unitPrice * quantity;
      if (unitPrice > 0 && quantity > 0) {
        document.getElementById("total-display").innerText = "Total: $" + total.toFixed(2);
        document.getElementById("total-display").style.display = "block";
      } else {
        document.getElementById("total-display").style.display = "none";
      }
      if (quantity <= 0) {
        document.getElementById("copy-info").style.display = "none";
        copied04128187076 = false;
        copied13519438 = false;
        document.getElementById("image-container").style.display = "none";
        document.getElementById("next-container").style.display = "none";
        selectedImageUrl = "";
        deselectAllImages();
      }
    }
    
    // Al presionar "Enter" en el campo de cantidad, se muestra el contenedor de copiado
    document.addEventListener("DOMContentLoaded", function() {
      const inputField = document.querySelector(".input-container input[type='number']");
      inputField.addEventListener("keydown", function(event) {
        if (event.key === "Enter") {
          inputField.blur();
          const quantity = parseFloat(inputField.value) || 0;
          if (quantity > 0) {
            document.getElementById("copy-info").style.display = "block";
          } else {
            document.getElementById("copy-info").style.display = "none";
          }
        }
      });
    });
    
    // Adjunta listeners a las imágenes para hacerlas seleccionables
    function attachImageListeners() {
      const images = document.querySelectorAll("#image-container img");
      images.forEach(img => {
        img.addEventListener("click", function() {
          deselectAllImages();
          img.classList.add("selected");
          if (img.alt === "Venezuela") {
            selectedImageUrl = urlVenezuela;
          } else if (img.alt === "Provincial") {
            selectedImageUrl = urlProvincial;
          } else if (img.alt === "Bicentenario") {
            selectedImageUrl = urlBicentenario;
          }
        });
      });
    }
    
    // Quita la clase "selected" de todas las imágenes
    function deselectAllImages() {
      const images = document.querySelectorAll("#image-container img");
      images.forEach(img => {
        img.classList.remove("selected");
      });
    }
    
    // Función para copiar texto (los números) al portapapeles y activar la secuencia de copiado
    function copyToClipboard(text) {
      navigator.clipboard.writeText(text);
      if (text === "04128187076") {
        copied04128187076 = true;
      } else if (text === "13519438") {
        copied13519438 = true;
      }
      if (copied04128187076 && copied13519438) {
        document.getElementById("image-container").style.display = "block";
        document.getElementById("next-container").style.display = "block";
        attachImageListeners();
      }
    }
    
    // Función que se invoca al presionar "Siguiente". Aquí se redirige a la URL de la imagen seleccionada.
    function goNext() {
      if (selectedImageUrl !== "") {
        window.location.href = selectedImageUrl;
      } else {
        alert("Por favor, selecciona una imagen.");
      }
    }
  </script>
</head>
<body>
  <div class="title-container">
    <h1>La Bendición de Dios</h1>
    <p>Con nosotros en todo momento</p>
  </div>
  
  <div class="content-container">Tenemos</div>
  
  <div class="price-container">
    <!-- Opción Quesillos -->
    <label class="price-item" for="quesillo">
      <input type="checkbox" id="quesillo" onchange="updateOrderMessage()">
      <span class="custom-checkbox"></span>
      <div class="price-text">
        <span>Quesillos:</span>
        <span>$1.70</span>
      </div>
    </label>
    <!-- Opción Arequipe -->
    <label class="price-item" for="arequipe">
      <input type="checkbox" id="arequipe" onchange="updateOrderMessage()">
      <span class="custom-checkbox"></span>
      <div class="price-text">
        <span>Arequipe:</span>
        <span>$0.60</span>
      </div>
    </label>
  </div>
  
  <!-- Mensaje dinámico -->
  <div id="order-message"></div>
  
  <!-- Campo numérico -->
  <div class="input-container">
    <input type="number" placeholder="Introduce una cantidad..." min="0" oninput="updateTotal()">
  </div>
  
  <!-- Total calculado -->
  <div id="total-display"></div>
  
  <!-- Contenedor para datos de copiado -->
  <div id="copy-info">
    <p>04128187076</p>
    <button onclick="copyToClipboard('04128187076')">copiar</button>
    <p>13519438</p>
    <button onclick="copyToClipboard('13519438')">copiar</button>
    <p>0102</p>
  </div>
  
  <!-- Contenedor para imágenes -->
  <div id="image-container">
    <img src="Venezuela.png" alt="Venezuela">
    <img src="Provincial.png" alt="Provincial">
    <img src="Bicentenario.png" alt="Bicentenario">
  </div>
  
  <!-- Contenedor para el botón "Siguiente" -->
  <div id="next-container">
    <button onclick="goNext()">Siguiente</button>
  </div>
</body>
</html>
