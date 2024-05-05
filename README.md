<!DOCTYPE html>
<html lang="nl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Producten Toevoegen</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
    }
    .navbar {
      background-color: #333;
      overflow: hidden;
    }
    .navbar a {
      float: left;
      display: block;
      color: white;
      text-align: center;
      padding: 14px 20px;
      text-decoration: none;
    }
    .product-input {
      margin-top: 20px;
    }
    .product-item {
      margin-bottom: 5px;
    }
    .delete-btn {
      background-color: #f44336;
      color: white;
      border: none;
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
    }
    .delete-btn:hover {
      background-color: #d32f2f;
    }
  </style>
</head>
<body>

<div class="navbar">
  <a href="#" onclick="showProductsFor('duitsland')">Producten voor Duitsland!</a>
  <a href="#" onclick="showProductsFor('boodschappen')">Boodschappenlijst of ideeën!</a>
</div>

<div class="product-input">
  <input type="text" id="product" placeholder="Voer een productnaam in">
  <button onclick="addProduct()">Toevoegen</button>
</div>

<div id="product-list"></div>

<script>
  // Functie om de huidige geselecteerde categorie op te halen
  function currentCategory() {
    var pageTitle = document.title;
    if (pageTitle === "Producten voor Duitsland!") {
      return "duitsland";
    } else if (pageTitle === "Boodschappenlijst of ideeën!") {
      return "boodschappen";
    }
    return "";
  }

  // Functie om een product toe te voegen aan de lijst en op te slaan in localStorage
  function addProduct() {
    var productInput = document.getElementById("product");
    var productName = productInput.value;
    var productList = document.getElementById("product-list");
    var newItem = document.createElement("div");
    newItem.classList.add("product-item");
    newItem.textContent = productName;
    
    var deleteBtn = document.createElement("button");
    deleteBtn.textContent = "Verwijderen";
    deleteBtn.classList.add("delete-btn");
    deleteBtn.onclick = function() {
      deleteProduct(productName);
      productList.removeChild(newItem);
    };
    
    newItem.appendChild(deleteBtn);
    productList.appendChild(newItem);
    saveProduct(productName, currentCategory()); // Opslaan in localStorage met de huidige categorie
    productInput.value = ""; // Invoerveld leegmaken na toevoegen
  }

  // Functie om een product op te slaan in localStorage met de categorie
  function saveProduct(productName, category) {
    var products = JSON.parse(localStorage.getItem(category)) || [];
    products.push(productName);
    localStorage.setItem(category, JSON.stringify(products));
  }

  // Functie om een product te verwijderen uit localStorage
  function deleteProduct(productName) {
    var category = currentCategory();
    var products = JSON.parse(localStorage.getItem(category)) || [];
    var index = products.indexOf(productName);
    if (index !== -1) {
      products.splice(index, 1);
      localStorage.setItem(category, JSON.stringify(products));
    }
  }

  // Functie om producten weer te geven op basis van de geselecteerde categorie
  function showProductsFor(category) {
    var pageTitle = "";
    if (category === 'duitsland') {
      pageTitle = "Producten voor Duitsland!";
    } else if (category === 'boodschappen') {
      pageTitle = "Boodschappenlijst of ideeën!";
    }
    document.title = pageTitle; // Pas de paginatitel aan
    
    var productList = document.getElementById("product-list");
    productList.innerHTML = ''; // Leeg de productenlijst
    
    var products = JSON.parse(localStorage.getItem(category)) || [];
    products.forEach(function(productName) {
      var newItem = document.createElement("div");
      newItem.classList.add("product-item");
      newItem.textContent = productName;
      
      var deleteBtn = document.createElement("button");
      deleteBtn.textContent = "Verwijderen";
      deleteBtn.classList.add("delete-btn");
      deleteBtn.onclick = function() {
        deleteProduct(productName);
        productList.removeChild(newItem);
      };
      
      newItem.appendChild(deleteBtn);
      productList.appendChild(newItem);
    });
  }

  // Functie om opgeslagen producten weer te geven bij het laden van de pagina
  window.onload = function() {
    // Controleer of er producten zijn opgeslagen voor beide categorieën
    var duitseProducten = JSON.parse(localStorage.getItem('duitsland')) || [];
    var boodschappenProducten = JSON.parse(localStorage.getItem('boodschappen')) || [];

    // Als er geen producten zijn opgeslagen, creëer dan lege arrays voor beide categorieën
    if (duitseProducten.length === 0) {
      localStorage.setItem('duitsland', JSON.stringify([]));
    }
    if (boodschappenProducten.length === 0) {
      localStorage.setItem('boodschappen', JSON.stringify([]));
    }

    showProductsFor(currentCategory()); // Toon producten voor de huidige categorie
  }
</script>

</body>
</html>
