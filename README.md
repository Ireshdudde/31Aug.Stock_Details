<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Stock Dashboard</title>
  <style>
    :root {
      --primary: #2c3e50;
      --secondary: #34495e;
      --accent: #16a085;
      --bg: #ecf0f1;
      --white: #ffffff;
      --text-dark: #2d3436;
      --text-light: #7f8c8d;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: var(--bg);
      display: flex;
      min-height: 100vh;
    }

    .container {
      display: flex;
      flex-direction: row;
      width: 100%;
    }

    .sidebar {
      background-color: var(--primary);
      color: var(--white);
      width: 220px;
      padding: 20px;
      flex-shrink: 0;
    }

    .date {
      text-align: center;
      font-size: 1.4rem;
      font-weight: bold;
      margin-bottom: 10px;
      color: #bdc3c7;
    }

    .sidebar h2 {
      text-align: center;
      font-size: 1.2rem;
      margin-bottom: 20px;
    }

    .location {
      padding: 12px 15px;
      margin-bottom: 12px;
      background-color: var(--secondary);
      border-radius: 6px;
      cursor: pointer;
      transition: 0.3s;
      font-size: 0.95rem;
    }

    .location:hover {
      background-color: var(--accent);
    }

    .content {
      flex: 1;
      padding: 20px;
      overflow-y: auto;
    }

    .content h2 {
      margin-bottom: 20px;
      color: var(--text-dark);
      font-size: 1.4rem;
    }

    .section {
      margin-bottom: 30px;
    }

    .section h3 {
      font-size: 1.3rem;
      color: #ffffff;
      background-color: #16a085;
      padding: 10px 15px;
      border-radius: 6px;
      margin-bottom: 15px;
    }

    .cards {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
    }

    .card {
      background-color: var(--white);
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
      padding: 10px 15px;
      width: 160px;
      transition: transform 0.2s;
    }

    .card:hover {
      transform: scale(1.03);
    }

    .card h4 {
      font-size: 0.85rem;
      color: var(--text-dark);
      margin-bottom: 6px;
    }

    .card p {
      font-size: 0.95rem;
      color: var(--accent);
      font-weight: 600;
    }

    @media (max-width: 768px) {
      .container {
        flex-direction: column;
      }

      .sidebar {
        width: 100%;
        display: flex;
        flex-direction: row;
        overflow-x: auto;
        padding: 10px;
      }

      .sidebar h2 {
        display: none;
      }

      .location {
        margin: 0 8px;
        padding: 10px 12px;
        font-size: 0.85rem;
        white-space: nowrap;
      }

      .card {
        width: 48%;
      }
    }

    @media (max-width: 480px) {
      .card {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="sidebar">
      <div class="date">31 Aug 2025</div>
      <h2>Locations</h2>
      <div class="location" onclick="loadStock('savda')">1. Savda</div>
      <div class="location" onclick="loadStock('jamner')">2. Jamner</div>
      <div class="location" onclick="loadStock('shirpurchopada')">3. Shirpur_Chopada</div>
    </div>

    <div class="content">
      <h2 id="locationTitle">Select a location to view stock</h2>

      <div class="section" id="inwardSection">
        <h3>Inward</h3>
        <div class="cards" id="inwardCards"></div>
      </div>

      <div class="section" id="outwardSection">
        <h3>Outward</h3>
        <div class="cards" id="outwardCards"></div>
      </div>

      <div class="section" id="balanceSection">
        <h3>Balance</h3>
        <div class="cards" id="balanceCards"></div>
      </div>
    </div>
  </div>

  <script>
    const stockData = {
      shirpurchopada: {
        Inward: {},
        Outward: {
          "Roshana Box (Nos)": "4328 nos",
          "Vaccum Bag 13kg": "33 KG",
          "PE Form 1.5 MM (Nos)": "4368 nos",
          "Sachets (Nos)": "728 nos",
          "fevicol (kg)": "4 KG",
          "Germination Paper (kg)": "14.56 KG",
          "Fungicide (kg)": "1.5 KG",
          "Truti (kg)": "5 KG",
          "Bleaching Powder (g)": "0.1 G",
          "Rubber (kg)": "0.5 KG",
          "Roshana Sticker (Nos)": "10192"
        },
        Balance: {
          "Roshana Box (Nos)": "6474 nos",
          "Vaccum Bag 13kg": "497.95 KG",
          "PE Form 1.5 MM (Nos)": "39716 nos",
          "Sachets (Nos)": "9536 nos",
          "fevicol (kg)": "105 KG",
          "Germination Paper (kg)": "270.7 KG",
          "Fungicide (kg)": "30 KG",
          "Truti (kg)": "70 KG",
          "Bleaching Powder (g)": "2.4 G",
          "Rubber (kg)": "8.6 KG",
          "Roshana Sticker (Nos)": "799504 nos"
        }
      },

      jamner: {
        Inward: {
          "fevicol (kg)": "35 KG"
        },
        Outward: {
          "Roshana Box (Nos)": "1438 nos",
          "Vaccum Bag 13kg": "65.36363636 KG",
          "PE Form 1.5 MM (Nos)": "10066 nos",
          "Sachets (Nos)": "1438 nos",
          "fevicol (kg)": "8 KG",
          "Germination Paper (kg)": "28.76 KG",
          "Fungicide (kg)": "3 KG",
          "Truti (kg)": "10 KG",
          "Bleaching Powder (g)": "0.2 G",
          "Rubber (kg)": "1 KG",
          "Roshana Sticker (Nos)": "20132"
        },
        Balance: {
          "Roshana Box (Nos)": "4207 nos",
          "Vaccum Bag 13kg": "815.636 KG",
          "PE Form 1.5 MM (Nos)": "69743 nos",
          "Sachets (Nos)": "59107 nos",
          "fevicol (kg)": "75.5 KG",
          "Germination Paper (kg)": "47.14 KG",
          "Fungicide (kg)": "6 KG",
          "Truti (kg)": "75 KG",
          "Bleaching Powder (g)": "3.2 G",
          "Rubber (kg)": "10 KG",
          "Roshana Sticker (Nos)": "131526 nos"
        }
      },

      savda: {
        Inward: {},
        Outward: {},
        Balance: {
          "King Brown Box (Nos)": "2300 nos",
          "Bandhan Premium": "1500 nos",
          "Alaa white Box (Nos)": "500 nos",
          "Roshana Box (Nos)": "0 nos",
          "Laibaah Box (Nos)": "350 nos",
          "Haniya Box (Nos)": "800 nos",
          "Shabad Box(Nos)": "280 nos",
          "Vaccum Bag 13kg": "1720 KG",
          "PE Form 1.5 MM (Nos)": "7500 nos",
          "Sachets (Nos)": "0 nos",
          "Fevicol (kg)": "258 KG",
          "Germination Paper (kg)": "313 KG",
          "Fungicide (kg)": "4 KG",
          "Truti (kg)": "495 KG",
          "Bleaching Powder (g)": "8.9 G",
          "Rubber (kg)": "0 KG",
          "King Sticker (Nos)": "56000 nos",
          "Alaa Sticker (Nos)": "4650000 nos",
          "Roshana Sticker (Nos)": "2610000 nos",
          "Other Sticker": "2520000 nos"
        }
      }
    };

    function loadStock(locationKey) {
      const location = stockData[locationKey];
      const locationName = locationKey.charAt(0).toUpperCase() + locationKey.slice(1).replace("_", " ");
      document.getElementById("locationTitle").innerText = `📦 ${locationName} Stock Details`;

      const inwardContainer = document.getElementById("inwardCards");
      const outwardContainer = document.getElementById("outwardCards");
      const balanceContainer = document.getElementById("balanceCards");

      function renderCards(container, data) {
        container.innerHTML = "";
        if (!data || Object.keys(data).length === 0) {
          container.innerHTML = `<p style="color: gray;">No data</p>`;
          return;
        }
        for (let item in data) {
          const value = data[item];
          container.innerHTML += `
            <div class="card">
              <h4>${item}</h4>
              <p>${value}</p>
            </div>`;
        }
      }

      renderCards(inwardContainer, location.Inward);
      renderCards(outwardContainer, location.Outward);
      renderCards(balanceContainer, location.Balance);
    }
  </script>
</body>
</html>
