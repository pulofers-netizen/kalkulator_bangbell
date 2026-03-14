<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulator BEP Ads - Bang Bell</title>
    <style>
        :root { --bg: #0d1117; --card: #161b22; --primary: #2ecc71; --text: #c9d1d9; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: var(--bg); color: var(--text); margin: 0; padding: 15px; }
        .container { max-width: 480px; margin: auto; }
        .card { background: var(--card); padding: 25px; border-radius: 12px; border: 1px solid #30363d; box-shadow: 0 8px 24px rgba(0,0,0,0.5); }
        h2 { text-align: center; color: var(--primary); margin-top: 0; font-size: 1.5rem; text-transform: uppercase; }
        label { display: block; font-size: 12px; color: #8b949e; margin-top: 15px; font-weight: bold; }
        select, input { width: 100%; padding: 12px; border-radius: 6px; border: 1px solid #30363d; background: #0d1117; color: white; margin-top: 5px; box-sizing: border-box; font-size: 15px; }
        .row { display: flex; gap: 10px; }
        .row div { flex: 1; }
        button { width: 100%; padding: 16px; background: var(--primary); color: #fff; border: none; border-radius: 6px; font-weight: bold; margin-top: 25px; cursor: pointer; font-size: 16px; text-shadow: 0 1px 2px rgba(0,0,0,0.2); }
        .result-panel { display: none; margin-top: 25px; padding: 20px; background: #21262d; border-radius: 10px; border: 1px solid var(--primary); text-align: center; }
        .bep-big { font-size: 3rem; font-weight: 800; color: var(--primary); margin: 5px 0; }
        .breakdown { text-align: left; font-size: 13px; color: #8b949e; background: #0d1117; padding: 15px; border-radius: 8px; margin-top: 15px; border: 1px solid #30363d; }
        .breakdown div { display: flex; justify-content: space-between; margin-bottom: 8px; }
    </style>
</head>
<body>

<div class="container">
    <div class="card">
        <h2>ADS TARGET UNIT</h2>
        
        <label>Pilih Marketplace</label>
        <select id="mp" onchange="updateKategori()">
            <option value="shopee">Shopee</option>
            <option value="tiktok">TikTok Shop</option>
            <option value="tokopedia">Tokopedia</option>
        </select>

        <label>Tipe Seller</label>
        <select id="tipe">
            <option value="star">Star / Star+ / Power Merchant</option>
            <option value="mall">Official Store / Mall</option>
            <option value="biasa">Non-Star (Biasa)</option>
        </select>

        <label>Kategori Produk</label>
        <select id="kat"></select>

        <div class="row">
            <div>
                <label>Harga Jual (Rp)</label>
                <input type="number" id="hj" placeholder="0">
            </div>
            <div>
                <label>Modal/HPP (Rp)</label>
                <input type="number" id="modal" placeholder="0">
            </div>
        </div>

        <label>Total Budget Iklan (Rp)</label>
        <input type="number" id="iklan" placeholder="0">

        <button onclick="hitungBEP()">HITUNG SEKARANG</button>

        <div id="result" class="result-panel">
            <span style="font-size: 12px;">TARGET PENJUALAN MINIMAL:</span>
            <div id="bepUnit" class="bep-big">0</div>
            <div id="statusText" style="font-size: 14px; font-weight: bold; color: #fff;"></div>
            
            <div class="breakdown">
                <div><span>Estimasi Admin Marketplace:</span> <span id="detAdmin" style="color: #ff7b72;">Rp 0</span></div>
                <div><span>Keuntungan per Produk:</span> <span id="detMargin" style="color: var(--primary);">Rp 0</span></div>
                <hr style="border: 0.5px solid #30363d;">
                <div style="font-weight: bold; color: #fff;"><span>Balik Modal Iklan di:</span> <span id="detTotal">Rp 0</span></div>
            </div>
        </div>
    </div>
</div>

<script>
    // Database Admin Fee (Sederhana berdasarkan rata-rata 2026)
    const kategoriData = [
        { nama: "Fashion, Aksesoris, Tas, Sepatu", rate: 0.085 },
        { nama: "Skincare, Kosmetik, Kesehatan", rate: 0.08 },
        { nama: "Elektronik, Gadget, Kamera", rate: 0.045 },
        { nama: "Perlengkapan Rumah & Hobi", rate: 0.07 },
        { nama: "Makanan & Minuman (F&B)", rate: 0.06 },
        { nama: "Otomotif & Olahraga", rate: 0.065 },
        { nama: "Ibu & Bayi, Mainan", rate: 0.075 }
    ];

    function updateKategori() {
        const katSelect = document.getElementById('kat');
        katSelect.innerHTML = kategoriData.map((k, index) => 
            `<option value="${index}">${k.nama}</option>`
        ).join('');
    }

    function hitungBEP() {
        const hj = parseFloat(document.getElementById('hj').value);
        const modal = parseFloat(document.getElementById('modal').value);
        const iklan = parseFloat(document.getElementById('iklan').value);
        const tipe = document.getElementById('tipe').value;
        
        let rateBase = kategoriData[document.getElementById('kat').value].rate;

        // Penyesuaian Tipe Seller
        if (tipe === 'mall') rateBase += 0.02; // Mall biasanya lebih mahal
        if (tipe === 'biasa') rateBase -= 0.01; // Non-star lebih murah dikit adminnya

        if(!hj || !modal || !iklan) {
            alert("Lengkapi data bosku!");
            return;
        }

        const potAdmin = hj * rateBase;
        const marginClean = hj - modal - potAdmin;

        if(marginClean <= 0) {
            document.getElementById('bepUnit').innerText = "RUGI";
            document.getElementById('statusText').innerText = "Margin habis untuk admin & modal!";
            document.getElementById('result').style.display = 'block';
            return;
        }

        const unitBEP = Math.ceil(iklan / marginClean);
        
        document.getElementById('bepUnit').innerText = unitBEP + " UNIT";
        document.getElementById('statusText').innerText = `Iklan aman setelah laku ${unitBEP} paket.`;
        
        document.getElementById('detAdmin').innerText = `Rp ${Math.round(potAdmin).toLocaleString()}`;
        document.getElementById('detMargin').innerText = `Rp ${Math.round(marginClean).toLocaleString()}`;
        document.getElementById('detTotal').innerText = `Rp ${Math.round(iklan).toLocaleString()}`;
        
        document.getElementById('result').style.display = 'block';
    }

    window.onload = updateKategori;
</script>

</body>
</html>
