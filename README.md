<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kalkulator Efisiensi Bensin & Energi</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      background-color: #0f172a;
      color: #f8fafc;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }

    .container {
      background-color: #1e293b;
      padding: 28px;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
      width: 100%;
      max-width: 480px;
      border: 1px solid #334155;
    }

    h2 {
      font-size: 1.5rem;
      margin-bottom: 20px;
      text-align: center;
      color: #38bdf8;
    }

    .form-group {
      margin-bottom: 16px;
    }

    label {
      display: block;
      font-size: 0.9rem;
      margin-bottom: 6px;
      color: #cbd5e1;
    }

    .input-wrapper {
      display: flex;
      gap: 8px;
    }

    input[type="number"], select {
      width: 100%;
      padding: 12px;
      background-color: #0f172a;
      border: 1px solid #475569;
      border-radius: 8px;
      color: #ffffff;
      font-size: 1rem;
      outline: none;
      transition: border-color 0.2s;
    }

    input[type="number"]:focus, select:focus {
      border-color: #38bdf8;
    }

    select {
      width: auto;
      cursor: pointer;
    }

    button {
      width: 100%;
      padding: 14px;
      margin-top: 10px;
      background-color: #0284c7;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.2s;
    }

    button:hover {
      background-color: #0369a1;
    }

    .result-container {
      margin-top: 24px;
      padding: 16px;
      background-color: #0f172a;
      border-radius: 8px;
      border-left: 4px solid #38bdf8;
      display: none;
    }

    .result-item {
      margin-bottom: 12px;
    }

    .result-item:last-child {
      margin-bottom: 0;
    }

    .result-label {
      font-size: 0.85rem;
      color: #94a3b8;
    }

    .result-value {
      font-size: 1.25rem;
      font-weight: bold;
      color: #4ade80;
    }

    .note {
      font-size: 0.75rem;
      color: #64748b;
      margin-top: 12px;
      line-height: 1.4;
    }
  </style>
</head>
<body>

  <div class="container">
    <h2>Kalkulator Efisiensi</h2>

    <div class="form-group">
      <label for="bensin">Volume Bensin Awal (ml)</label>
      <input type="number" id="bensin" step="any" placeholder="Contoh: 15" required>
    </div>

    <div class="form-group">
      <label for="energi">Energi Listrik</label>
      <div class="input-wrapper">
        <input type="number" id="energi" step="any" placeholder="Contoh: 5.6" required>
        <select id="satuanEnergi">
          <option value="Wh">Wh</option>
          <option value="Joule">Joule</option>
        </select>
      </div>
    </div>

    <div class="form-group">
      <label for="jarak">Jarak Tempuh (km)</label>
      <input type="number" id="jarak" step="any" placeholder="Contoh: 10" required>
    </div>

    <button onclick="hitung()">Hitung Efisiensi</button>

    <div id="hasil" class="result-container">
      <div class="result-item">
        <div class="result-label">Total Volume Bensin:</div>
        <div id="totalBensinVal" class="result-value">0 ml</div>
      </div>
      <div class="result-item" style="margin-top: 12px;">
        <div class="result-label">Hasil Efisiensi:</div>
        <div id="efisiensiVal" class="result-value">0 km/liter</div>
      </div>
    </div>

    <p class="note">* Faktor konversi Wh: 0,5854 | Faktor konversi Joule: 0,0001626</p>
  </div>

  <script>
    function hitung() {
      const bensinAwal = parseFloat(document.getElementById('bensin').value);
      const energi = parseFloat(document.getElementById('energi').value);
      const satuan = document.getElementById('satuanEnergi').value;
      const jarak = parseFloat(document.getElementById('jarak').value);

      if (isNaN(bensinAwal) || isNaN(energi) || isNaN(jarak) || jarak <= 0) {
        alert("Harap masukkan semua angka dengan benar!");
        return;
      }

      // 1. Rumus Total Volume Bensin
      let konsumsiEnergiBensin = 0;
      if (satuan === 'Wh') {
        konsumsiEnergiBensin = energi * 0.5854;
      } else if (satuan === 'Joule') {
        konsumsiEnergiBensin = energi * 0.0001626;
      }

      const totalBensin = bensinAwal + konsumsiEnergiBensin;

      // 2. Rumus Hasil Efisiensi (km/l)
      const hasilEfisiensi = (jarak * 1000) / totalBensin;

      // Tampilkan Hasil
      document.getElementById('totalBensinVal').innerText = totalBensin.toLocaleString('id-ID', { maximumFractionDigits: 3 }) + ' ml';
      document.getElementById('efisiensiVal').innerText = hasilEfisiensi.toLocaleString('id-ID', { maximumFractionDigits: 2 }) + ' km/liter';
      
      document.getElementById('hasil').style.display = 'block';
    }
  </script>

</body>
</html>
