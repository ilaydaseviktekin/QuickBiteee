<!DOCTYPE html>

<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>QuickBite - Akıllı Tarif Asistanı</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      padding: 20px;
      color: #333;
    }
    .container {
      max-width: 1000px;
      margin: auto;
      background: white;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
    }
    h1 {
      text-align: center;
      color: #e74c3c;
    }
    .input-group {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }
    input, select, button {
      padding: 12px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 16px;
    }
    .tag {
      display: inline-block;
      background: #3498db;
      color: white;
      padding: 5px 10px;
      border-radius: 15px;
      margin: 5px;
    }
    .recipes {
      margin-top: 30px;
    }
    .recipe {
      border-bottom: 1px solid #ddd;
      padding: 20px 0;
    }
    .recipe img {
      width: 100%;
      max-width: 300px;
      border-radius: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>QuickBite - Tarif Bul</h1>
    <div class="input-group">
      <input type="text" id="ingredientInput" placeholder="Malzeme girin (örn: yumurta, domates)">
      <button onclick="addIngredient()">Ekle</button>
    </div>
    <div id="selectedTags"></div>
    <div class="input-group">
      <select id="dietFilter">
        <option value="">Diyet Seçimi (Opsiyonel)</option>
        <option value="vegan">Vegan</option>
        <option value="vegetarian">Vejetaryen</option>
        <option value="lowcarb">Düşük Karbonhidrat</option>
        <option value="keto">Keto</option>
      </select>
      <button onclick="findRecipes()">Tarifleri Bul</button>
    </div>
    <div class="recipes" id="recipeResults"></div>
  </div>

  <script>
    const recipes = [
      {
        name: "Menemen",
        ingredients: ["yumurta", "domates", "biber"],
        diet: "vegetarian",
        image: "https://i.imgur.com/JY3qN5a.jpg",
        steps: "Soğanı ve biberi kavur, domates ekle, yumurta kır."
      },
      {
        name: "Sebzeli Makarna",
        ingredients: ["makarna", "biber", "kabak", "patlıcan"],
        diet: "vegetarian",
        image: "https://i.imgur.com/mQ6cB9x.jpg",
        steps: "Makarnayı haşla, sebzeleri sotele, birleştir."
      }
    ];

    const selectedIngredients = [];

    function addIngredient() {
      const input = document.getElementById("ingredientInput");
      const value = input.value.trim().toLowerCase();
      if (value && !selectedIngredients.includes(value)) {
        selectedIngredients.push(value);
        renderTags();
        input.value = "";
      }
    }

    function renderTags() {
      const tagDiv = document.getElementById("selectedTags");
      tagDiv.innerHTML = selectedIngredients.map(ing => `<span class='tag'>${ing}</span>`).join(" ");
    }

    function findRecipes() {
      const diet = document.getElementById("dietFilter").value;
      const container = document.getElementById("recipeResults");

      const matched = recipes.filter(r => {
        const hasAnyMatch = r.ingredients.some(recipeIng =>
          selectedIngredients.some(userIng =>
            recipeIng.includes(userIng) || userIng.includes(recipeIng)
          )
        );

        return hasAnyMatch && (!diet || r.diet === diet);
      });

      if (matched.length === 0) {
        container.innerHTML = `<p>Uygun tarif bulunamadı.</p>`;
        return;
      }

      container.innerHTML = matched.map(r => `
        <div class='recipe'>
          <h2>${r.name}</h2>
          <img src='${r.image}' alt='${r.name}'>
          <p><strong>Malzemeler:</strong> ${r.ingredients.join(", ")}</p>
          <p><strong>Yapılışı:</strong> ${r.steps}</p>
        </div>
      `).join("");
    }
  </script>

</body>
</html> BU KOD İÇİN ANSIL WEB SAYFASI AÇABİLİRM KENDİME AİT html dosyası oluştur
