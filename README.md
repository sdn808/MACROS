<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Macro Tracker</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- Rubik Font -->
  <link href="https://fonts.googleapis.com/css2?family=Rubik:wght@400;600&display=swap" rel="stylesheet">
  
  <style>
    body {
      font-family: 'Rubik', sans-serif;
      font-size: 1.2rem;
      max-width: 100%;
      margin: 0;
      padding: 20px;
      background: #f9f9f9;
    }

    h1 {
      text-align: center;
      font-size: 2rem;
      margin-bottom: 20px;
    }

    form {
      display: flex;
      flex-direction: column;
      gap: 12px;
      margin-bottom: 20px;
    }

    input {
      font-size: 1.2rem;
      padding: 14px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-family: 'Rubik', sans-serif;
    }

    button,
    .reset {
      background: linear-gradient(135deg, #3498db, #3cb2ff);
      color: white;
      font-weight: 600;
      border: none;
      border-radius: 12px;
      padding: 12px 16px;
      font-size: 1.1rem;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.3s;
      box-shadow: 0 4px 12px rgba(60, 178, 255, 0.3);
      font-family: 'Rubik', sans-serif;
    }

    button:hover,
    .reset:hover {
      transform: scale(1.05);
      box-shadow: 0 6px 18px rgba(60, 178, 255, 0.5);
    }

    /* Unique style for Reset button */
    .reset {
      background: linear-gradient(135deg, #ff4b2b, #ff416c);
      box-shadow: 0 4px 12px rgba(255, 65, 108, 0.3);
      margin-top: 20px;
    }

    .reset:hover {
      box-shadow: 0 6px 18px rgba(255, 65, 108, 0.5);
    }

    .entry {
      background: white;
      border-radius: 8px;
      padding: 12px;
      margin-bottom: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    .totals {
      background: #fff;
      border-radius: 8px;
      padding: 16px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      font-weight: bold;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Macro Track</h1>

  <form id="macro-form">
    <input type="text" id="description" placeholder="Meal description" required>
    <input type="number" id="protein" placeholder="Protein (g)" required>
    <input type="number" id="carbs" placeholder="Carbs (g)" required>
    <input type="number" id="fats" placeholder="Fats (g)">
    <input type="number" id="calories" placeholder="Calories">
    <button type="submit">Add Entry</button>
  </form>

  <div id="entries"></div>

  <div class="totals" id="totals"></div>

  <button class="reset" onclick="resetData()">Reset Day</button>

  <script>
    let entries = JSON.parse(localStorage.getItem('macroEntries')) || [];

    function saveEntries() {
      localStorage.setItem('macroEntries', JSON.stringify(entries));
    }

    function renderEntries() {
      const entriesDiv = document.getElementById('entries');
      entriesDiv.innerHTML = '';
      let totalProtein = 0, totalCarbs = 0, totalFats = 0, totalCalories = 0;

      entries.forEach(entry => {
        totalProtein += entry.protein;
        totalCarbs += entry.carbs;
        totalFats += entry.fats;
        totalCalories += entry.calories;

        const div = document.createElement('div');
        div.className = 'entry';
        div.innerHTML = `
          <strong>${entry.description}</strong><br>
          Protein: ${entry.protein}g | Carbs: ${entry.carbs}g | Fats: ${entry.fats}g | Calories: ${entry.calories}
        `;
        entriesDiv.appendChild(div);
      });

      document.getElementById('totals').innerHTML = `
        Total Protein: ${totalProtein}g<br>
        Total Carbs: ${totalCarbs}g<br>
        Total Fats: ${totalFats}g<br>
        Total Calories: ${totalCalories}
      `;
    }

    document.getElementById('macro-form').addEventListener('submit', function (e) {
      e.preventDefault();
      const description = document.getElementById('description').value.trim();
      const protein = parseInt(document.getElementById('protein').value) || 0;
      const carbs = parseInt(document.getElementById('carbs').value) || 0;
      const fats = parseInt(document.getElementById('fats').value) || 0;
      const calories = parseInt(document.getElementById('calories').value) || 0;

      entries.push({ description, protein, carbs, fats, calories });
      saveEntries();
      renderEntries();
      this.reset();
    });

    function resetData() {
      if (confirm('Are you sure you want to reset today\'s entries?')) {
        entries = [];
        saveEntries();
        renderEntries();
      }
    }

    renderEntries();
  </script>
</body>
</html>

