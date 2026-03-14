<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bang Bell - Ultimate BEP Ads</title>
    <style>
        :root { --primary: #00ff88; --danger: #ff4444; --bg: #0a0a0a; --card: #141414; }
        body { font-family: 'Inter', sans-serif; background: var(--bg); color: #fff; margin: 0; padding: 15px; display: flex; justify-content: center; }
        .container { width: 100%; max-width: 450px; background: var(--card); padding: 25px; border-radius: 20px; border: 1px solid #252525; position: relative; }
        .watermark { position: absolute; top: 15px; right: 20px; font-size: 12px; color: var(--primary); font-weight: bold; opacity: 0.7; letter-spacing: 1px; }
        header { border-bottom: 1px solid #222; margin-bottom: 20px; padding-bottom: 10px; }
        header h1 { font-size: 22px; margin: 0; background: linear-gradient(to right, #00ff88, #00dbde); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        label { display: block; font-size: 10px; color: #666; margin-top: 15px; font-weight: bold; text-transform: uppercase; }
        select, input { width: 100%; padding: 12px; border-radius: 10px; border: 1px solid #252525; background: #080808; color: #fff; margin-top: 5px; box-sizing: border-box; font-size: 14px; outline: none; }
        
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .toggle-group { display: flex; align-items: center; justify-content: space-between; background: #0d0d0d; padding: 12px; border-radius: 10px; margin-top: 15px; border: 1px solid #222; }
        
        button { width: 100%; padding: 16px; background: var(--primary); color: #000; border: none; border-radius: 12px; font-weight: 800; margin-top: 25px; cursor: pointer; transition: 0.3s; }
        
        .result-panel { display: none; margin-top: 20px; padding: 20px; border-radius: 15px; background: #0d0d0d; border: 1px solid #333; }
        .bep-display { text-align: center; margin-bottom: 20px; }
        .bep-val { font-size: 45px; font-weight: 900; line-height: 1; }
        
        .breakdown { font-size: 13px; border-top: 1px solid #222; padding-top: 15px; }
        .row { display: flex; justify-content: space-between; margin-bottom: 8px; color: #999; }
        .row b { color: #fff; }
        .highlight { color: var(--primary); font-weight: bold; }
        .minus { color: var(--danger) !important; }
    </style>
</head>
<body>

<div class="container">
    <div class="watermark">BANG BELL</div>
    <header><h1>ANALISA ADS V2</h1></header>

    <label>Marketplace & Tipe Seller</label>
    <select id="mp">
        <option value="0.08">Shopee (Star/Star+)</option>
        <option value="0.10">Shopee (Mall)</option>
        <option value="0.07">TikTok Shop (Pro)</option>
        <option value="0.06">Tokopedia (Power Merchant)</option>
    </select>

    <label>Kategori Produk</label>
    <select id="kat">
        <option value="0.01">Kategori 1 (Fashion, Kecantikan, Aksesoris)</option>
        <option value="0.005">Kategori 2 (Elektronik, Gadget, Otomotif)</option>
        <option value="0.007">Kategori 3 (Rumah Tangga, Hobi, Olahraga)</option>
        <option value="0.004">Kategori 4 (Makanan & Minuman)</option>
        <option value="0.008">Kategori 5 (Ibu & Bayi, Mainan Anak)</option>
    </select>

    <div class="toggle-group">
        <span style="font-size: 12px;">Program Gratis Ongkir Xtra (4%)</span>
        <input type="checkbox" id="ongkir" style="width: 20px; margin: 0;" checked>
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

    <div class="grid">
        <div>
            <label>Voucher Diskon</label>
            <input type="number" id="voc" value="0">
        </div>
        <div>
            <label>Budget Iklan</label>
            <input type="number" id="iklan" placeholder="0">
        </div>
    </div>

    <button onclick="hitungTotal()">PROSES DATA</button>

    <div id="result" class="result-panel">
        <div class="bep-display">
            <span style="font-size: 10px; color: #555;">TARGET MINIMAL PENJUALAN</span>
            <div id="outUnit" class="bep-val">0</div>
            <div id="outStatus" style="font-size: 12px; margin-top: 5px;"></div>
        </div>
        
        <div class="breakdown">
            <div class="row"><span>Harga Jual:</span> <b>Rp <span id="resHj">0</span></b></div>
            <div class="row"><span>Modal (HPP):</span> <b>- Rp <span id="resModal">0</span></b></div>
            <div class="row"><span>Admin MP + Kategori:</span> <b>- Rp <span id="resAdmin">0</span></b></div>
            <div class="row"><span>Grtis Ongkir Xtra:</span> <b>- Rp <span id="resGrox">0</span></b></div>
            <div class="row"><span>Voucher Toko:</span> <b>- Rp <span id="resVoc">0</span></b></div>
            <hr style="border: 0.5px solid #222;">
            <div class="row highlight"><span>Untung Bersih / Unit:</span> <b id="resMargin">Rp 0</b></div>
        </div>
    </div>
</div>

<script>
    function hitungTotal() {
        const hj = parseFloat(document.getElementById('hj').value) || 0;
        const modal = parseFloat(document.getElementById('modal').value) || 0;
        const iklan = parseFloat(document.getElementById('iklan').value) || 0;
        const voc = parseFloat(document.getElementById('voc').value) || 0;
        
        const rateMP = parseFloat(document.getElementById('mp').value);
        const rateKat = parseFloat(document.getElementById('kat').value);
        const rateGrox = document.getElementById('ongkir').checked ? 0.04 : 0;

        // Rincian Kalkulasi
        const biayaAdmin = hj * (rateMP + rateKat);
        const biayaGrox = hj * rateGrox;
        const marginPerUnit = hj - modal - biayaAdmin - biayaGrox - voc;

        // Update Rincian Harga di Bawah
        document.getElementById('resHj').innerText = hj.toLocaleString();
        document.getElementById('resModal').innerText = modal.toLocaleString();
        document.getElementById('resAdmin').innerText = Math.round(biayaAdmin).toLocaleString();
        document.getElementById('resGrox').innerText = Math.round(biayaGrox).toLocaleString();
        document.getElementById('resVoc').innerText = voc.toLocaleString();
        
        const resMargin = document.getElementById('resMargin');
        resMargin.innerText = "Rp " + Math.round(marginPerUnit).toLocaleString();

        const outUnit = document.getElementById('outUnit');
        const outStatus = document.getElementById('outStatus');

        if(marginPerUnit <= 0) {
            // Jika Rugi
            outUnit.innerText = "RUGI";
            outUnit.className = "bep-val minus";
            resMargin.className = "minus";
            outStatus.innerText = "Anda rugi Rp " + Math.abs(Math.round(marginPerUnit)).toLocaleString() + " setiap 1 produk laku.";
            outStatus.style.color = "#ff4444";
        } else {
            // Jika Untung
            const unit = Math.ceil(iklan / marginPerUnit);
            outUnit.innerText = unit + " UNIT";
            outUnit.className = "bep-val";
            resMargin.className = "highlight";
            outStatus.innerText = "Laku " + unit + " unit untuk menutupi iklan Rp " + iklan.toLocaleString();
            outStatus.style.color = "#00ff88";
        }

        document.getElementById('result').style.display = 'block';
    }
</script>

</body>
</html>
