<!DOCTYPE html><html lang="id"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>CIHOS SIMPLE OFFLINE</title><style>body{margin:0;background:#0a2f4a;padding:10px;font-family:Arial;color:#0a2f4a}.c{background:#fff;border-radius:14px;padding:12px;margin-bottom:10px;box-shadow:0 2px 8px rgba(0,0,0,.2)}label{font-weight:800;font-size:11px;display:block;margin-top:8px}input,select{width:100%;padding:9px;border:1.5px solid #b9d7d7;border-radius:10px;margin-top:3px;font-size:12px}.row{display:grid;grid-template-columns:1fr 1fr;gap:8px}.btn{border:none;border-radius:22px;padding:14px;color:#fff;font-weight:900;width:100%;margin-top:8px;font-size:12px}.blue{background:#2a7de1}.green{background:#1a9e5e}.red{background:#e23a3a}.lb{background:#fff;border:3px solid #d32f2f;border-radius:10px;padding:10px;margin:8px 0;width:100%;font-size:11px;line-height:1.4}</style></head><body>
<div class="c" style="display:flex;align-items:center;gap:12px">
<img src="logo.png" style="height:50px;width:auto">
<div>
<h3 style="margin:0;color:#0a2f4a">CIPUTRA HOSPITAL SURABAYA</h3>
<p style="margin:2px 0 0;font-size:10px">ICU - High Alert Calculator v3.3 - OFFLINE</p>
</div>
</div>
<div class="c"><b>👤 PASIEN</b><label>No RM *</label><input id="norm" value="00123456"><label>Nama</label><input id="nama" value="John Doe"><div class="row"><div><label>BB (kg)</label><input id="bb" type="number" value="65"></div><div><label>Ruangan</label><input id="ruang" value="ICU - Bed 04"></div></div></div>
<div class="c"><b>💊 OBAT + PENGOPLOSAN PZ/D5</b><label>Obat</label><select id="obat"><option value="8">Norepinephrine 8 mg / 50 mL</option><option value="200">Dopamine 200 mg / 50 mL</option><option value="250">Dobutamine 250 mg / 50 mL</option></select><div class="row" style="background:#e8f5e9;padding:6px;border-radius:8px;margin-top:6px"><div><label>Total Obat</label><input id="tObat" value="8 mg"></div><div><label>Pengencer WAJIB</label><select id="peng"><option>PZ (NaCl 0.9%)</option><option>D5 (Dextrose 5%)</option></select></div></div><div class="row"><div><label>Total Vol</label><select id="tVol"><option>50 mL Spuit</option><option>50 mL</option><option>100 mL</option></select></div><div><label>Target μg/kg/min</label><input id="tar" type="number" value="0.05" step="0.01"></div></div><label>Giver *</label><input id="giv" placeholder="Nama perawat 1" value="a"><label>Checker *</label><input id="chk" placeholder="Nama perawat 2" value="b"></div>
<div class="c"><button class="btn blue" onclick="buat('titrasi')">+ Label Titrasi AUTO MUNCUL</button><button class="btn green" onclick="buat('spuit')">💉 Label Spuit PZ/D5 + Tgl/Jam AUTO</button><button class="btn red" onclick="document.getElementById('area').innerHTML=''">🗑️ Bersihkan Semua Label</button></div>
<div id="area"></div>
<script>
function buat(jenis){
let norm=document.getElementById('norm').value||'-';let nama=document.getElementById('nama').value||'-';let bb=parseFloat(document.getElementById('bb').value)||65;let ruang=document.getElementById('ruang').value||'-';
let obatMg=parseFloat(document.getElementById('obat').value)||8;let tObat=document.getElementById('tObat').value||'8 mg';let peng=document.getElementById('peng').value||'PZ (NaCl 0.9%)';let tVol=document.getElementById('tVol').value||'50 mL Spuit';
let tar=parseFloat(document.getElementById('tar').value)||0.05;let giv=document.getElementById('giv').value||'-';let chk=document.getElementById('chk').value||'-';
if(giv=='-'||chk=='-'){alert('Wajib isi Giver & Checker!');return;}
let ugPerMl=(obatMg*1000)/50;let mlJam=((tar*bb*60)/ugPerMl).toFixed(2);
let now=new Date();let tgl=now.toLocaleString('id-ID',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'});let exp=new Date(now.getTime()+86400000);let tglExp=exp.toLocaleString('id-ID',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'});
let d=document.createElement('div');d.className='lb';
if(jenis==='spuit'){
d.innerHTML='<div style="background:#d32f2f;color:#fff;text-align:center;font-weight:900;padding:4px;border-radius:6px">⚠️ HIGH ALERT - LABEL SPUIT</div><div style="display:flex;justify-content:space-between;margin-top:6px;font-weight:800"><span>No RM: '+norm+'</span><span>'+tgl+'</span></div><b style="font-size:12px">Obat: '+tObat+' dalam '+tVol+'</b><br><div style="background:#e3f2fd;padding:4px;border-radius:5px;margin:4px 0"><b>TOTAL: '+tObat+' dalam '+tVol+'</b><br><b style="color:#0d47a1">PENGENCER: '+peng+'</b></div>Konsentrasi: '+ugPerMl+' mcg/mL<br>👤 '+nama+' | '+bb+'kg | '+ruang+'<br>🎯 Dosis: '+tar+' mcg/kg/min = <b>'+mlJam+' mL/jam (AUTO HITUNG)</b><hr style="margin:6px 0"><div style="font-size:9px">👩‍⚕️ Giver: <b>'+giv+'</b> | 👀 Checker: <b>'+chk+'</b><br>⏰ BUAT: <b>'+tgl+'</b> | ⏰ EXP: <b>'+tglExp+' (24 jam)</b><br>📦 Pengoplosan: <b>'+tObat+' + '+peng+' ad '+tVol+'</b></div><div style="border:2px solid #d32f2f;color:#d32f2f;text-align:center;border-radius:6px;font-weight:900;margin-top:6px;padding:3px">✅ DOUBLE CHECK OK - 6 BENAR</div>';
}else{
d.innerHTML='<div style="display:flex;justify-content:space-between;font-weight:900"><span style="color:red">HIGH ALERT</span><span>No RM: '+norm+'</span></div><b>TOTAL: '+tObat+' dalam '+tVol+'</b><br><b>PENGENCER: '+peng+'</b><br>👤 '+nama+' '+bb+'kg '+ruang+'<br>🎯 '+tar+' mcg/kg/min = <b>'+mlJam+' mL/jam AUTO</b><br>Kons: '+ugPerMl+' mcg/mL<br>⏰ '+tgl+'<br>📦 Oplosan: '+tObat+' + '+peng+' ad '+tVol+'<br>Giver: '+giv+' | Checker: '+chk+'<br><div style="border:1.5px solid red;color:red;text-align:center;border-radius:5px;font-weight:900;margin-top:4px">DOUBLE CHECK OK</div>';
}
document.getElementById('area').appendChild(d);d.scrollIntoView({behavior:'smooth'});
}
</script></body></html>
