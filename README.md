<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Cihos High Alert - Ciputra</title>
<style>
*{box-sizing:border-box;font-family:Segoe UI,Arial}
body{margin:0;background:#0a2f4a;padding:12px}
.header{background:#fff;border-radius:16px;padding:14px 18px;display:flex;align-items:center;gap:14px}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:12px}
.card{background:#fff;border-radius:16px;padding:14px}
.btns{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:8px;margin:12px 0}
.btn{border:none;border-radius:20px;padding:11px;font-weight:700;color:#fff}
.b{background:#2a7de1}.g{background:#1a9e5e}.r{background:#e23a3a}.d{background:#12283f}
.tbl{background:#fff;border-radius:16px;padding:12px}
table{width:100%;border-collapse:collapse}th{background:#d6eeee;padding:8px}td{padding:6px;text-align:center;border-bottom:1px solid #eee}
.hi{background:#ff4d1f;color:#fff;padding:2px 8px;border-radius:6px;font-size:11px}
</style>
</head>
<body>
<div class="header"><b>Precision in High Alert Medication Calculator</b><small> - ICU Ciputra Hospital Surabaya - SOP 50ML NANO</small></div>
<div class="grid">
<div class="card"><h3>DATA PASIEN</h3>
Nama<input id="nama" value="John Doe"><br>BB kg<input id="bb" type="number" value="65" oninput="calc()"><br>Ruangan<input id="ruang" value="ICU - Bed 04">
</div>
<div class="card"><h3>OBAT TITRASI SOP 50ML - NANO</h3>
<select id="obat" onchange="calc()"><option value="NE">NE 8mg/50ml NANO 160.000 ng/ml</option><option value="EPI">EPI 8mg/50ml NANO 160.000 ng/ml</option><option value="DOPA">Dopamine 200mg/50ml</option><option value="DOBU">Dobutamine 250mg/50ml</option><option value="VASO">Vasopressin 20U/20ml 1U/ml</option></select>
<input id="kons" readonly><input id="mode" readonly>
</div>
</div>
<div class="btns"><button class="btn b" onclick="addLabel()">+ Tambah Label</button><button class="btn g" onclick="addSpuit()">Label Spuit</button><button class="btn r" onclick="document.getElementById('printArea').innerHTML=''">Bersihkan</button><button class="btn d" onclick="window.print()">Cetak</button></div>
<div class="tbl"><span class="hi">HIGH ALERT</span> <b id="titleObat">Norepinephrine</b><table><thead><tr><th>Dosis</th><th>mL/jam</th><th>ug/menit</th><th>Keterangan</th></tr></thead><tbody id="tbody"></tbody></table></div>
<div id="printArea" style="display:flex;flex-wrap:wrap;gap:8px;margin-top:12px"></div>
<script>
const SOP={NE:{n:'Norepinephrine 8 mg/50 mL',mg:8,vol:50,ug:160,mode:'NANO 160.000 ng/mL'},EPI:{n:'Epinephrine 8 mg/50 mL',mg:8,vol:50,ug:160,mode:'NANO 160.000 ng/mL'},DOPA:{n:'Dopamine 200 mg/50 mL',mg:200,vol:50,ug:4000,mode:'2-20 mcg/kg/mnt'},DOBU:{n:'Dobutamine 250 mg/50 mL',mg:250,vol:50,ug:5000,mode:'2-20'},VASO:{n:'Vasopressin 20U/20mL',mg:20,vol:20,ug:1,mode:'0.01-0.1 U/mnt',vaso:true}}
function calc(){let o=SOP[document.getElementById('obat').value];let bb=parseFloat(document.getElementById('bb').value)||65;document.getElementById('kons').value=o.mg+'mg/'+o.vol+'ml = '+o.ug+' ug/mL';document.getElementById('mode').value=o.mode;document.getElementById('titleObat').innerText=o.n;let doses=o.vaso?[0.01,0.02,0.03,0.04]:[0.05,0.1,0.2,0.3];let tb=document.getElementById('tbody');tb.innerHTML='';doses.forEach(d=>{let mlh=o.vaso?d*60:(d*bb*60)/o.ug;let ugm=d*bb;let tr=document.createElement('tr');tr.innerHTML=`<td>${d}</td><td>${mlh.toFixed(2)}</td><td>${ugm.toFixed(2)}</td><td>${d==0.2?'Target ICU':'Standar'}</td>`;tb.appendChild(tr)})}
function addLabel(){let o=SOP[document.getElementById('obat').value];let div=document.createElement('div');div.style.cssText='background:#fff;border-radius:12px;padding:10px;width:230px;border:1.5px solid #333;font-size:11px';div.innerHTML=`<b style="color:red">HIGH ALERT</b> ${o.n}<br>${document.getElementById('nama').value} - ${document.getElementById('bb').value}kg<br><div style="border:1.5px solid red;color:red;text-align:center;border-radius:8px;font-weight:900;margin-top:6px">HIGH ALERT</div>`;document.getElementById('printArea').appendChild(div)}
function addSpuit(){addLabel()}calc();
</script>
</body>
</html>
