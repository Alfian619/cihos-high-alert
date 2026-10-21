<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ciputra Hospital - High Alert Calculator v3</title>
<style>
:root{--navy:#0a2f4a;--teal:#0f5a6d;}
*{box-sizing:border-box;font-family:'Segoe UI',Arial}
body{margin:0;background:#0a2f4a;padding:12px}
.header{background:white;border-radius:18px;padding:14px 20px;display:flex;gap:14px;align-items:center}
.header h1{margin:0;color:var(--navy);font-size:18px}
.header p{margin:0;color:var(--teal);font-size:11px}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:12px}
.card{background:white;border-radius:18px;padding:14px}
.card h3{margin:0 0 10px;color:var(--navy);font-size:14px}
label{font-size:11px;display:block;margin-top:7px;color:#333;font-weight:700}
input,select{width:100%;padding:7px;border:1.5px solid #bcdcdc;border-radius:10px;margin-top:3px;font-size:13px}
.row2{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.row3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:6px}
.btns{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:8px;margin:12px 0}
.btn{border:none;border-radius:20px;padding:11px;font-weight:800;color:white;font-size:11px;cursor:pointer}
.blue{background:#2a7de1}.green{background:#1a9e5e}.red{background:#e23a3a}.dark{background:#12283f}
.table-box{background:white;border-radius:18px;padding:14px}
table{width:100%;border-collapse:collapse;margin-top:8px}
th{background:#cde9e9;padding:7px;font-size:11px;color:var(--navy)}
td{padding:6px;text-align:center;font-size:11px;border-bottom:1px solid #eee}
.highlight{background:#ffe8d0}
.badge{background:#ff4d1f;color:white;border-radius:6px;padding:2px 8px;font-size:10px}
.prints{display:flex;flex-wrap:wrap;gap:10px;margin-top:12px}
.label{background:white;border-radius:12px;padding:12px;width:300px;border:2.5px solid #000;font-size:11px;line-height:1.35}
.label-syringe{border:2.5px solid #d32f2f !important}
</style>
</head>
<body>
<div class="header">
<div style="background:var(--navy);color:white;width:44px;height:44px;display:grid;place-items:center;border-radius:10px;font-weight:900;font-size:9px;text-align:center">CIPUTRA<br>HOSPITAL</div>
<div><h1>Precision in High Alert Calculator v3</h1><p>SOP 50ML NANO 8mg/50mL = 160.000 ng/mL | PZ/D5 + Giver/Checker + No RM</p></div>
</div>

<div class="grid">
<div class="card">
<h3>👤 DATA PASIEN</h3>
<label>No RM / Medical Record *</label><input id="norm" placeholder="00123456" value="00123456">
<div class="row2">
<div><label>Nama Pasien</label><input id="nama" value="John Doe"></div>
<div><label>Umur</label><input id="umur" value="58 Th"></div>
</div>
<div class="row2">
<div><label>Berat Badan (kg) *</label><input id="bb" type="number" value="65" oninput="hitung()"></div>
<div><label>Ruangan / Bed</label><input id="ruang" value="ICU - Bed 04"></div>
</div>
</div>

<div class="card">
<h3>💊 OBAT TITRASI SOP 50ML + PENGOPLOSAN</h3>
<label>Obat High Alert</label><select id="obat" onchange="hitung()"><option value="NE">Norepinephrine 8 mg/50 mL NANO</option><option value="EPI">Epinephrine 8 mg/50 mL NANO</option><option value="DOPA">Dopamine 200 mg/50 mL</option><option value="DOBU">Dobutamine 250 mg/50 mL</option><option value="VASO">Vasopressin 20U/20mL</option></select>

<div class="row3" style="background:#e8f5e9;border:1.5px solid #2e7d32;border-radius:10px;padding:6px;margin-top:8px">
<div><label>Total Obat</label><input id="totalObat" value="8 mg" readonly></div>
<div><label>Pengencer</label><select id="pengencer"><option value="PZ">PZ (NaCl 0.9%)</option><option value="D5">D5 (Dextrose 5%)</option></select></div>
<div><label>Total Volume</label><select id="totalVol"><option>50 mL</option><option>50 mL Spuit</option><option>100 mL</option></select></div>
</div>

<div class="row2" style="margin-top:6px">
<div><label>Laju Target μg/kg/min</label><input id="target" type="number" step="0.01" value="0.05" oninput="hitung()"></div>
<div><label>Konsentrasi (auto)</label><input id="kons" readonly></div>
</div>

<div style="margin-top:8px;background:#fff3cd;border:1.5px solid #e6a800;border-radius:10px;padding:7px">
<label style="margin-top:0;color:#7a5a00">👥 DOUBLE CHECK - WAJIB</label>
<div class="row2">
<div><label>Giver (Pemberi)</label><input id="giver" placeholder="Nama perawat 1"></div>
<div><label>Checker (Pengecek)</label><input id="checker" placeholder="Nama perawat 2"></div>
</div>
</div>
<div style="margin-top:6px;background:#f0f8ff;border:1px solid #2a7de1;border-radius:8px;padding:5px;font-size:10px" id="mode"></div>
</div>
</div>

<div class="btns">
<button class="btn blue" onclick="tambah('titrasi')">+ Label Titrasi</button>
<button class="btn green" onclick="tambah('spuit')">💉 Label Spuit + PZ/D5</button>
<button class="btn red" onclick="document.getElementById('prints').innerHTML=''">🗑️ Bersihkan</button>
<button class="btn dark" onclick="window.print()">🖨️ Cetak Semua</button>
</div>

<div class="table-box">
<span class="badge">HIGH ALERT</span> <b id="judul">Norepinephrine - Tabel Dosis</b>
<table><thead><tr><th>Dosis (μg/kg/min)</th><th>mL/jam</th><th>μg/menit</th><th>Keterangan</th></tr></thead><tbody id="tbody"></tbody></table>
</div>
<div id="prints" class="prints"></div>

<script>
const SOP={NE:{n:'Norepinephrine 8 mg/50 mL',mg:8,vol:50,ug:160,mode:'NANO 160.000 ng/mL'},EPI:{n:'Epinephrine 8 mg/50 mL',mg:8,vol:50,ug:160,mode:'NANO 160.000 ng/mL'},DOPA:{n:'Dopamine 200 mg/50 mL',mg:200,vol:50,ug:4000,mode:'2-20 mcg/kg/mnt'},DOBU:{n:'Dobutamine 250 mg/50 mL',mg:250,vol:50,ug:5000,mode:'2-20 mcg/kg/mnt
