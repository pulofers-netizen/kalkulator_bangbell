<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bang Bell - BEP Ads Auto</title>
    <style>
        :root { --primary: #00ff88; --bg: #0a0a0a; --card: #141414; --accent: #1e1e1e; }
        body { font-family: 'Inter', sans-serif; background: var(--bg); color: #fff; margin: 0; padding: 15px; display: flex; justify-content: center; }
        .container { width: 100%; max-width: 420px; background: var(--card); padding: 25px; border-radius: 20px; border: 1px solid #252525; box-shadow: 0 15px 35px rgba(0,0,0,0.7); position: relative; }
        .watermark { position: absolute; top: 10px; right: 20px; font-size: 10px; color: var(--primary); letter-spacing: 2px; opacity: 0.5; }
        header { border-bottom: 1px solid #252525; margin-bottom: 20px; padding-bottom: 10px; }
        header h1 { font-size: 20px; margin: 0; color: var(--primary); letter-spacing: 1px; }
        
        label { display: block; font-size: 10px; color: #777; margin-top: 15px; font-weight: bold; text-transform: uppercase; }
        select, input { width: 100%; padding: 12px; border-radius: 10px; border: 1px solid #252525; background: #080808; color: #fff; margin-top: 5px; box-sizing: border-box; font-size: 14px; outline: none; }
        select:focus, input:focus { border-color: var(--primary); }
        
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        
        /* Checkbox Styling */
        .toggle-group { display: flex; align-items: center; justify-content: space-between; background: #080808; padding: 10px; border-radius: 10px; margin-top: 10px; border: 1px solid #252525; }
        .toggle-label { font-size: 12px; color: #ccc; }
        .switch { position: relative; display: inline-block; width: 40px; height: 20px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #333; transition: .4s; border-radius: 20px; }
        .slider:before { position: absolute; content: ""; height: 14px; width: 14px; left: 3px; bottom: 3px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--primary); }
        input:checked + .slider:before { transform: translateX(20px); }

        button { width: 100%; padding: 16px; background: var(--primary); color: #000; border: none; border-radius: 12px; font-weight: 800; margin-top: 25px; cursor: pointer; transition: 0.3s; }
        button:active { transform: scale(0.97); }

        .result-panel { display: none; margin-top: 20px; padding: 20px; border-radius: 15px; background: linear-gradient(180deg, #1a1a1a, #111); border: 1px solid #333; text-align: center; }
        .bep-val { font-size: 40px; font-weight: 900; color: var(--primary); margin: 5px 0; }
        .breakdown { text-align: left; margin-top: 15px; font-size: 12px; border-top: 1px solid #222; padding-top: 10px; color: #888; }
        .row { display: flex; justify-content: space-between; margin-bottom: 5px; }
        .row b { color: #fff; }
    </style>
</head>
<body>

<div class="container">
    <div class="watermark">BANG BELL PRO</div>
    <header>
        <h1>AUTOMATED BEP</h1>
    </header>

    <label>Marketplace</label>
    <select id="mp" onchange="updateKat()">
        <option value="shopee">Shopee (Star/Star+)</option>
        <option value="tiktok">TikTok Shop</option>
        <option value="tokopedia">Tokopedia (PM)</option>
    </select>

    <label>Kategori Produk</label>
    <select id="kat"></select>

    <div class="toggle-group">
        <span class="toggle-label">Ikut Program (Grtis Ongkir/Cashback)</span>
        <label class="switch">
            <input type="checkbox" id="prog" checked>
            <span class="slider"></span>
        </label>
    </div>

    <div class="grid">
        <div>
            <label>Harga Jual</label>
            <input type="number" id="hj" placeholder="0">
        </div>
        <div>
            <label>Modal (HPP)</label>
            <input type="number" id="modal" placeholder="0">
        </div>
    </div>

    <label>Voucher Diskon (Rp)</label>
    <input type="number" id="voc" placeholder="0">

    <label>Budget Iklan Harian</label>
    <input type="number" id="iklan" placeholder="0">

    <button onclick="hitungAuto()">ANALISA TARGET</button>

    <div id="result" class="result-panel">
        <span style="font-size: 10px; color: #666;">HARUS LAKU MINIMAL:</span>
        <div class="bep-val" id="outUnit">0</div>
        <div id="outStatus" style="font-size: 12px; color: #aaa;"></div>
        
        <div class="breakdown">
            <div class="row"><span>Total Admin MP:</span> <b id="detAdmin">Rp 0</b></div>
            <div class="row"><span>Potongan Voucher:</span> <b id="detVoc">Rp 0</b></div>
            <div class="row"><span>Margin Bersih:</span> <b id="detMargin" style="color:var(--primary)">Rp 0</b></div>
        </div>
    </div>
</div>

<script>
    const dbAdmin = {
        shopee: [ {n: "Fashion (8%)", r: 0.08}, {n: "Elektronik (5%)", r: 0.05}, {n: "Umum (7%)", r: 0.07} ],
        tiktok: [ {n: "Fashion (10%)", r: 0.10}, {n: "Elektronik (6%)", r: 0.06}, {n: "Umum (8%)", r: 0.08} ],
        tokopedia: [ {n: "Fashion (7.5%)", r: 0.075}, {n: "Elektronik (4%)", r: 0.04}, {n: "Umum (6%)", r: 0.06} ]
    };

    function updateKat() {
        const mp = document.getElementById('mp').value;
        const kat = document.getElementById('kat');
        kat.innerHTML = dbAdmin[mp].map(k => `<option value="${k.r}">${k.n}</option>`).join('');
    }

    function hitungAuto() {
        const hj = parseFloat(document.getElementById('hj').value) || 0;
        const modal = parseFloat(document.getElementById('modal').value) || 0;
        const iklan = parseFloat(document.getElementById('iklan').value) || 0;
        const voc = parseFloat(document.getElementById('voc').value) || 0;
        const rateAdmin = parseFloat(document.getElementById('kat').value);
        
        // Program Biaya Otomatis: 6% (Rata-rata Gratis Ongkir + Cashback Xtra)
        const rateProg = document.getElementById('prog').checked ? 0.06 : 0;

        const totalBiayaMP = hj * (rateAdmin + rateProg);
        const marginClean = hj - modal - totalBiayaMP - voc;

        if(marginClean <= 0) {
            alert("RUGI! Margin minus, cek lagi harga modal/jual.");
            return;
        }

        const unit = Math.ceil(iklan / marginClean);
        
        document.getElementById('outUnit').innerText = unit + " UNIT";
        document.getElementById('outStatus').innerText = "Target harian agar modal iklan kembali.";
        document.getElementById('detAdmin').innerText = "Rp " + Math.round(totalBiayaMP).toLocaleString();
        document.getElementById('detVoc').innerText = "Rp " + voc.toLocaleString();
        document.getElementById('detMargin').innerText = "Rp " + Math.round(marginClean).toLocaleString();
        document.getElementById('result').style.display = 'block';
    }

    window.onload = updateKat;
</script>

</body>
</html>
