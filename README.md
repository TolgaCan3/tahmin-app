<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>Tahmin Paylaş</title>
  <style>
    body{font-family:sans-serif;padding:20px;background:#f7f7f7;margin:0}
    .row{display:flex;gap:8px;margin:6px 0}
    .col{flex:1;padding:6px}
    .btn{padding:10px 14px;border:0;border-radius:8px;background:#4caf50;color:#fff;cursor:pointer}
    .card{padding:10px;border:1px solid #ddd;border-radius:10px;margin:10px 0;position:relative;background:#fff}
    .del{position:absolute;top:6px;right:6px;background:#f44336;color:#fff;border:0;border-radius:50%;width:24px;height:24px;cursor:pointer}
    h2,h3{margin-top:18px}
  </style>
</head>
<body>

<h2>📝 Tahmin Paylaş</h2>
<div class="row">
  <input id="ev" class="col" placeholder="Ev sahibi">
  <input id="dep" class="col" placeholder="Deplasman">
</div>
<div class="row">
  <select id="tahmin" class="col">
    <option value="">Tahmin seç</option>
    <option>MS 1</option><option>MS 0</option><option>MS 2</option>
    <option>2.5 ÜST</option><option>2.5 ALT</option>
    <option>KG VAR</option><option>KG YOK</option>
  </select>
  <input id="oran" class="col" placeholder="Oran (opsiyonel)">
</div>
<div class="row">
  <select id="kategori" class="col">
    <option value="free">Ücretsiz</option>
    <option value="vip">VIP</option>
  </select>
  <button class="btn" style="flex:1" onclick="ekleTahmin()">➕ Ekle</button>
</div>

<h3>🎁 Ücretsiz Tahminler</h3>
<div id="listeFree">Henüz yok.</div>

<h3>🔒 VIP Tahminler</h3>
<div id="listeVip">Henüz yok.</div>

<script>
function ekleTahmin(){
  var ev=document.getElementById('ev').value.trim();
  var dep=document.getElementById('dep').value.trim();
  var th=document.getElementById('tahmin').value;
  var or=document.getElementById('oran').value.trim();
  var kat=document.getElementById('kategori').value;
  if(!ev||!dep||!th){alert('Lütfen tüm alanları doldur.');return;}
  var key='mr_tahminler_'+kat;
  var arr=[];try{arr=JSON.parse(localStorage.getItem(key)||'[]');}catch(e){}
  var id=Date.now().toString();
  arr.unshift({id:id,ev:ev,dep:dep,th:th,or:or,ts:Date.now()});
  localStorage.setItem(key,JSON.stringify(arr));
  document.getElementById('ev').value='';
  document.getElementById('dep').value='';
  document.getElementById('tahmin').value='';
  document.getElementById('oran').value='';
  renderLists();
}

function renderLists(){
  ['free','vip'].forEach(function(kat){
    var key='mr_tahminler_'+kat;
    var arr=[];try{arr=JSON.parse(localStorage.getItem(key)||'[]');}catch(e){}
    var html='';
    arr.forEach(function(it){
      var d=new Date(it.ts);
      html+='<div class="card">'
           +'<button class="del" onclick="silTahmin(\''+kat+'\',\''+it.id+'\')">×</button>'
           +'<b>'+it.ev+' - '+it.dep+'</b><br>'
           +it.th+(it.or?(' • Oran: '+it.or):'')+'<br>'
           +'<small>'+d.toLocaleDateString()+' '+d.toLocaleTimeString()+'</small>'
           +'</div>';
    });
    document.getElementById(kat==='free'?'listeFree':'listeVip').innerHTML= html || 'Henüz yok.';
  });
}

function silTahmin(kat,id){
  var key='mr_tahminler_'+kat;
  var arr=[];try{arr=JSON.parse(localStorage.getItem(key)||'[]');}catch(e){}
  arr=arr.filter(function(it){return it.id!==id;});
  localStorage.setItem(key,JSON.stringify(arr));
  renderLists();
}

// sayfa açıldığında listele
renderLists();
</script>

</body>
</html>
