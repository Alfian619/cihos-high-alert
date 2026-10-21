<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>CIHOS v3.3 FIX</title><style>body{background:#0a2f4a;padding:10px;font-family:Arial} .c{background:#fff;border-radius:12px;padding:12px;margin-bottom:10px} input,select{width:100%;padding:8px;border:1px solid #aaa;border-radius:8px;margin-top:3px} .b{border:none;border-radius:20px;padding:12px;color:#fff;font-weight:900;width:48%;margin:1%} .bl{background:#2a7de1} .gr{background:#1a9e5e} .lb{background:#fff;border:3px solid #d32f2f;border-radius:10px;padding:10px;margin:8px;width:340px;font-size:11px}</style></head><body>
<div class="c"><b>DATA PASIEN</b><br>No RM <input id="norm" value="00123456"> Nama <input id="nama" value="John Doe"> BB <input id="bb" type="number" value="65"> Ruangan <input id="ruang" value="ICU - Bed 04"></div>
<div class="c"><b>OBAT + PENGOPLOSAN PZ/D5</b><br>Obat <select id="obat"><option value="8">Norepinephrine 8mg/50mL</option><option value="200">Dopamine 200mg/50mL</option><option value="250">Dobutamine 250mg/50mL</option></select><br>
Total Obat <input id="tObat" value="8 mg"> Pengencer <select id="peng"><option>PZ (NaCl 0.9%)</option><option>D5 (Dextrose 5%)</option></select><br>
Total Vol <select id="tVol"><option>50 mL Spuit</option><option>50 mL</option><option>100 mL</option></select> Target <input id="tar" type="number" value="0.05" step="0.01"><br>
Giver <input id="giv" value="a"> Checker <input id="chk" value="b"><br>
Konsentrasi <input id="kons" readonly placeholder="otomatis"></div>
<div class="c" style="text-align:center"><button class="b bl" onclick="buat('titrasi')">+ Label Titrasi AUTO</button><button class="b gr" onclick="buat('spuit')">💉 Label Spuit + PZ/D5 + Tgl AUTO</button><button onclick="document.getElementById('area').innerHTML=''" style="width:98%;margin-top:6px;background:#e23a3a;color:#fff;border:none;border-radius:20px;padding:10px">Bersihkan</button></div>
<div id="area" style="display:flex;flex-wrap:wrap"></div>
<script>
function buat(jenis){
let norm=document.getElementById('norm').value;let nama=document.getElementById('nama').value;let bb=parseFloat(document.getElementById('bb').value)||65;let ruang=document.getElementById('ruang').value;
let obatMg=parseFloat(document.getElementById('obat').value)||8;let tObat=document.getElementById('tObat').value;let peng=document.getElementById('peng').value;let tVol=document.getElementById('tVol').value;
let tar=parseFloat(document.getElementById('tar').value)||0.05;let giv=document.getElementById('giv').value;let chk=document.getElementById('chk').value;
if(!giv||!chk){alert('Isi Giver & Checker!');return;}
let ugPerMl = (obatMg*1000)/50; // 8mg=8000mcg/50=160
document.getElementById('kons').value = tObat+' / 50 mL = '+ugPerMl+' mcg/mL';
let mlJam = ((tar*bb*60)/ugPerMl).toFixed(2);
let now=new Date();let tgl=now.toLocaleString('id-ID',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'});
let exp=new Date(now.getTime()+24*60*60*1000);let tglExp=exp.toLocaleString('id-ID',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'});
let d=document.createElement('div');d.className='lb';
if(jenis=='spuit'){
d.innerHTML='<div style="background:red;color:#fff;text-align:center;font-weight:900">⚠️ HIGH ALERT - LABEL SPUIT</div><b>No RM: '+norm+'</b> | '+tgl+'<br><b>Norepinephrine '+tObat+' dalam '+tVol+'</b><br><div style="background:#e3f2fd;padding:3px"><b>PENGENCER: '+peng+'</b></div>Kons: '+ugPerMl+' mcg/mL<br>👤 '+nama+' '+bb+'kg '+ruang+'<br>🎯 Dosis: '+tar+' mcg/kg/min = <b>'+mlJam+' mL/jam (AUTO HITUNG)</b><br><hr>Giver: '+giv+' | Checker: '+chk+'<br>⏰ BUAT: '+tgl+'<br>⏰ EXP: '+tglExp+' (24 jam)<br>📦 Oplosan: '+tObat+' + '+peng+' ad '+tVol+'<br><div style="border:2px solid red;text-align:center;color:red;font-weight:900">DOUBLE CHECK OK</div>';
}else{
d.innerHTML='<b style="color:red">HIGH ALERT - LABEL TITRASI</b><br>No RM: '+norm+'<br>Obat: '+tObat+' + '+peng+' ad '+tVol+'<br><b>PENGENCER: '+peng+'</b><br>👤 '+nama+' '+bb+'kg<br>🎯 '+tar+' = <b>'+mlJam+' mL/jam (AUTO)</b><br>Kons: '+ugPerMl+' mcg/mL<br>⏰ '+tgl+'<br>Giver: '+giv+' | Checker: '+chk+'<br><div style="border:2px solid red;text-align:center;color:red;font-weight:900">DOUBLE CHECK</div>';
}
document.getElementById('area').appendChild(d);
}
</script></body></html>
