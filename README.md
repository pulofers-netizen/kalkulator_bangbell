<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BEP Tracker - Bang Bell</title>
    <style>
        :root { --bg: #0f0f0f; --card: #1a1a1a; --text: #f0f0f0; --primary: #00ff88; --danger: #ff4444; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--bg); color: var(--text); margin: 0; padding: 15px; }
        .container { max-width: 450px; margin: auto; }
        .card { background: var(--card); padding: 20px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); margin-bottom: 20px; }
        h2 { text-align: center; color: var(--primary); margin-top: 0; font-size: 1.4rem; }
        label { display: block; font-size: 11px; color: #888; margin: 10px 0 4px; }
        input { width: 100%; padding: 12px; border-radius: 6px; border: 1px solid #333; background: #222; color: white; box-sizing: border-box; font-size: 16px; }
        button { width: 100%; padding: 14px; background: var(--primary); color: #000; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; margin-top: 15px; font-size: 16px; }
        .res-box { text-align: center; border: 2px dashed #444; padding: 15px; border-radius: 8px; margin-top: 15px; display: none; }
        .bep-val { font-size: 2.5rem; font-weight: 800; display: block; margin: 5px 0; }
        
        /* Gaya Riwayat */
        .history-box { background: #1a1a1a; padding: 15px; border-radius: 12px; }
        .history-item { background: #252525; padding: 10px; border-radius: 6px; margin-bottom: 8px; font-size: 13px; display: flex; justify-content: space-between; align-items: center; }
        .clear-btn { background: none; color: var(--danger); border: 1px solid var(--danger); padding: 5px 10px; font-size: 11px; width: auto; margin: 0; }
    </style>
</head>
<body>

<div class="container">
    <div class="card">
        <h2>BEP ADS CALCULATOR</h2>
        <label>Harga Jual (Rp)</label>
        <input type="number" id="hj" placeholder="Contoh: 150000">
        <label>Modal Barang (HPP)</label>
        <input type="number" id="modal" placeholder="Contoh: 80000">
        <label>Biaya Admin Marketplace (%)</label>
        <input type="number" id="adm" value="6">
        <label>Budget Iklan (Rp)</label>
        <input type="number" id="iklan" placeholder="Contoh: 200000">
        
        <button onclick="prosesBEP()">HITUNG TARGET</button>

        <div id="result" class="res-box">
            <span style="font-size: 12px; color: #aaa;">TARGET MINIMAL LAKU:</span>
            <span id="outUnit" class="bep-val">0</span>
            <span id="outText" style="font-size: 13px;"></span>
        </div>
    </div>

    <div class="history-box">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
            <h3 style="margin: 0; font-size: 1rem;">Riwayat Perhitungan</h3>
            <button onclick="hapusRiwayat()" class="clear-btn">Hapus Semua</button>
        </div>
        <div id="historyList"></div>
    </div>
</div>

<script>
    // Load data saat halaman dibuka
    window.onload = tampilkanRiwayat;

    function prosesBEP() {
        const hj = parseFloat(document.getElementById('hj').value);
        const modal = parseFloat(document.getElementById('modal').value);
        const adm = parseFloat(document.getElementById('adm').value) / 100;
        const iklan = parseFloat(document.getElementById('iklan').value);

        if (!hj || !modal || !iklan) return alert("Lengkapi data dulu!");

        const margin = hj - modal - (hj * adm);
        const resBox = document.getElementById('result');
        const outUnit = document.getElementById('outUnit');
        const outText = document.getElementById('outText');

        if (margin <= 0) {
            outUnit.innerText = "RUGI";
            outUnit.style.color = "var(--danger)";
            outText.innerText = "Harga jual terlalu rendah!";
        } else {
            const bep = Math.ceil(iklan / margin);
            outUnit.innerText = bep + " PCS";
            outUnit.style.color = "var(--primary)";
            outText.innerText = "Minimal jual " + bep + " unit untuk balik modal iklan.";
            
            simpanRiwayat(bep, iklan);
        }
        resBox.style.display = 'block';
    }

    function simpanRiwayat(unit, budget) {
        let list = JSON.parse(localStorage.getItem('bepHistory')) || [];
        const data = {
            tgl: new Date().toLocaleDateString('id-ID'),
            unit: unit,
            budget: budget
        };
        list.unshift(data); // Tambah ke paling atas
        localStorage.setItem('bepHistory', JSON.stringify(list.slice(0, 5))); // Simpan 5 terakhir
        tampilkanRiwayat();
    }

    function tampilkanRiwayat() {
        const list = JSON.parse(localStorage.getItem('bepHistory')) || [];
        const container = document.getElementById('historyList');
        container.innerHTML = list.map(item => `
            <div class="history-item">
                <span><b>${item.tgl}</b> - Budget: ${parseInt(item.budget).toLocaleString()}</span>
                <span style="color: var(--primary)">${item.unit} Pcs</span>
            </div>
        `).join('');
    }

    function hapusRiwayat() {
        localStorage.removeItem('bepHistory');
        tampilkanRiwayat();
    }
</script>

</body>
</html>
