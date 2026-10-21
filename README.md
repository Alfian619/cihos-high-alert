<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ciputra Hospital - High Alert Calculator</title>
<style>
:root{--navy:#0a2f4a;--teal:#0f5a6d;--light:#e6f7f7}
*{box-sizing:border-box;font-family:'Segoe UI',Arial}
body{margin:0;background:#0a2f4a;padding:12px}
.header{background:white;border-radius:18px;padding:14px 20px;display:flex;gap:14px;align-items:center}
.header h1{margin:0;color:var(--navy);font-size:20px}
.header p{margin:0;color:var(--teal);font-size:11px}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:12px}
.card{background:white;border-radius:18px;padding:14px}
.card h3{margin:0 0 10px;color:var(--navy);font-size:15px}
label{font-size:12px;display:block;margin-top:8px;color:#333}
input,select{width:100%;padding:8px;border:1.5px solid #bcdcdc;border-radius:10px;margin-top:4px}
.btns{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:8px;margin:12px 0}
.btn{border:none;border-radius:20px;padding:11px;font-weight:800;color:white;font-size:12px}
.blue{background:#2a7de1}.green{background:#1a9e5e}.red{background:#e23a3a}.dark{background:#12283f}
.table-box{background:white;border-radius:18px;padding:14px}
table{width:100%;border-collapse:collapse;margin-top:8px}
th{background:#cde9e9;padding:8px;font-size:12px;color:var(--navy)}
td{padding:7px;text-align:center;font-size:12px;border-bottom:1px solid #eee}
.highlight{background:#ffe8d0}
.badge{background:#ff4d1f;color:white;border-radius:6px;padding:2px 8px;font-size:11px}
.prints{display:flex;flex-wrap:wrap;gap:8px;margin-top:12px}
.label{background:white;border-radius:12px;padding:10px;width:230px;border:1.5px solid #333;font-size:11px}
</style>
</head>
<body>
<div class="header">
<div style="background:var(--navy);color:white;width:44px;height:44px;display:grid;place-items:center;border-radius:10px;font-weight:900;font-size:9px;text-align:center">CIPUTRA<br>HOSPITAL</div>
<div><h1>Precision in High Alert Medication Calculator</h1><p>SOP 50ML NANO VERSION - Norepinephrine 8mg/50mL = 160.000 ng/mL</p></div>
</div>

<div class="grid">
<div class="card">
<h3>👤 DATA PASIEN</h3>
<label>Nama Pasien</label><input id="nama" value="John Doe">
<label>Berat Badan (kg)</label><input id="bb" type="number" value="65" oninput="hitung()">
<label>Ruangan</label><input id="ruang" value="ICU - Bed 04">
<label>Status</label><select><option>Stabil • Terpantau</option></select>
</div>
<div class="card">
<h3>💊 OBAT TITRASI SOP 50ML</h3>
<label>Obat</label><select id="obat" onchange="hitung()"><option value="NE">Norepinephrine 8 mg/50 mL NANO</option><option value="EPI">Epinephrine 8 mg/50 mL NANO</option><option value="DOPA">Dopamine 200 mg/50 mL</option><option value="DOBU">Dobutamine 250 mg/50 mL</option><option value="VASO">Vasopressin 20U/20mL - 1 U/mL</option></select>
<label>Konsentrasi</label><input id="kons" readonly>
<label>Laju Target μg/kg/min</label><input id="target" type="number" step="0.01" value="0.05" oninput="hitung()">
<div style="margin-top:8px;background:#f0f8ff;border:1px solid #2a7de1;border-radius:8px;padding:6px;font-size:11px" id="mode"></div>
</div>
</div>

<div class="btns">
<button class="btn blue" onclick="tambah()">+ Tambah Label Titrasi</button>
<button class="btn green" onclick="tambah()">💉 Label Spuit</button>
<button class="btn red" onclick="document.getElementById('prints').innerHTML=''">🗑️ Bersihkan</button>
<button class="btn dark" onclick="window.print()">🖨️ Cetak Semua</button>
</div>

<div class="table-box">
<span class="badge">HIGH ALERT</span> <b id="judul">Norepinephrine 8 mg/50 mL - Tabel Dosis & Laju Infus</b>
<table><thead><tr><th>Dosis (μg/kg/min)</th><th>Laju Infus (mL/jam)</th><th>Dosis (μg/menit)</th><th>Keterangan</th></tr></thead><tbody id="tbody"></tbody></table>
<p style="font-size:10px;color:#666">Catatan: Validasi ulang sesuai protokol medis. HIGH ALERT MEDICATION - ikuti 6 benar.</p>
</div>
<div id="prints" class="prints"></div>

<script>
const SOP={NE:{n:'Norepinephrine 8 mg/50 mL',mg:8,vol:50,ug:160,mode:'NANO 10-500 ng/kg/mnt - 160.000 ng/mL'},EPI:{n:'Epinephrine 8 mg/50 mL',mg:8,vol:50,ug:160,mode:'NANO 10-500 ng/kg/mnt - 160.000 ng/mL'},DOPA:{n:'Dopamine 200 mg/50 mL',mg:200,vol:50,ug:4000,mode:'2-20 mcg/kg/mnt'},DOBU:{n:'Dobutamine 250 mg/50 mL',mg:250,vol:50,ug:5000,mode:'2-20 mcg/kg/mnt'},VASO:{n:'Vasopressin 20U/20mL - 1 U/mL',mg:20,vol:20,ug:1,mode:'0.01-0.1 U/mnt',vaso:true}}
function hitung(){let k=document.getElementById('obat').value;let s=SOP[k];let bb=parseFloat(document.getElementById('bb').value)||65;document.getElementById('kons').value=s.mg+' mg / '+s.vol+' mL = '+s.ug+' ug/mL';document.getElementById('mode').innerText=s.mode;document.getElementById('judul').innerText=s.n+' - Tabel Dosis & Laju Infus';let target=parseFloat(document.getElementById('target').value)||0.05;let list=s.vaso?[0.01,0.02,0.03,0.04]:[0.05,0.1,0.2,0.3];let tb=document.getElementById('tbody');tb.innerHTML='';list.forEach(d=>{let ml=s.vaso?d*60:(d*bb*60)/s.ug;let ugm=d*bb;let tr=document.createElement('tr');if(d==0.2)tr.className='highlight';tr.innerHTML=`<td>${d}</td><td>${ml.toFixed(2)}</td><td>${ugm.toFixed(2)}</td><td>${d==0.05?'Dosis minimum':d==0.2?'⭐ Target ICU':'Titrasi'}</td>`;tb.appendChild(tr)})}
function tambah(){let s=SOP[document.getElementById('obat').value];let d=document.createElement('div');d.className='label';d.innerHTML=`<b style="color:red">HIGH ALERT</b> ${s.n}<br>👤 ${document.getElementById('nama').value} ${document.getElementById('bb').value}kg ${document.getElementById('ruang').value}<br>Target ${document.getElementById('target').value} mcg/kg/min<div style="border:1.5px solid red;color:red;text-align:center;border-radius:8px;font-weight:900;margin-top:6px">HIGH ALERT - DOUBLE CHECK</div>`;document.getElementById('prints').appendChild(d)}
hitung();
</script>
</body>
</html>
