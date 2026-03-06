<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Suas Encomendas</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Inter',sans-serif}

body{
background:linear-gradient(135deg,#0f172a,#1e293b);
color:#fff;
display:flex;
justify-content:center;
}

.container{
width:100%;
max-width:420px;
padding:20px;
}

h2{
text-align:center;
margin-bottom:15px;
}

.card-grid{
display:grid;
grid-template-columns:1fr 1fr;
gap:10px;
margin-bottom:15px;
}

.card{
background:rgba(255,255,255,0.05);
padding:15px;
border-radius:12px;
text-align:center;
}

input{
width:100%;
padding:12px;
border-radius:10px;
border:none;
margin-bottom:10px;
}

.rastrear{
width:100%;
padding:12px;
border-radius:10px;
background:#3b82f6;
color:#fff;
font-weight:bold;
}

.progress{
width:100%;
height:10px;
background:#1e293b;
border-radius:10px;
overflow:hidden;
margin:15px 0;
}

.bar{
width:0%;
height:100%;
background:#f59e0b;
transition:1s;
}

.status-box{
background:rgba(255,255,255,0.05);
padding:12px;
border-radius:10px;
margin-bottom:15px;
font-size:14px;
}

#map{
height:200px;
border-radius:12px;
margin-bottom:15px;
display:none;
}

.history-item{
background:rgba(255,255,255,0.05);
padding:8px;
border-radius:8px;
margin-bottom:5px;
font-size:13px;
}
</style>
</head>

<body>

<div class="container">

<h2>📦 Rastrear Encomenda</h2>

<div class="card-grid">
<div class="card">Em rota<br><b id="rota">0</b></div>
<div class="card">Em trânsito<br><b id="transito">0</b></div>
<div class="card">Entregues<br><b id="entregue">0</b></div>
<div class="card">Total<br><b id="total">0</b></div>
</div>

<input type="text" id="codigo" placeholder="Digite o código de rastreio">

<button class="rastrear" onclick="rastrear()">Rastrear</button>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div class="status-box" id="status">
Digite um código para rastrear.
</div>

<div id="map"></div>

<h3>Histórico</h3>
<div id="historico"></div>

</div>

<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

<script>

let map;

function rastrear(){

const codigo=document.getElementById("codigo").value.trim();

if(!codigo){
alert("Digite um código");
return;
}

const status="Objeto em trânsito";
const cidade="Recife PE";

/* CONTADORES */
document.getElementById("rota").innerHTML="1";
document.getElementById("transito").innerHTML="1";
document.getElementById("entregue").innerHTML="0";
document.getElementById("total").innerHTML="2";

document.getElementById("bar").style.width="60%";

document.getElementById("status").innerHTML=
"📦 Código: "+codigo+"<br>"+
"Status: "+status+" (60%)<br>"+
"Local: "+cidade;

/* MOSTRAR MAPA */
document.getElementById("map").style.display="block";

iniciarMapa(cidade);
salvarHistorico(codigo);

}

async function iniciarMapa(cidade){

if(map){map.remove();}

try{

const resposta=await fetch(
"https://nominatim.openstreetmap.org/search?format=json&q="+encodeURIComponent(cidade)
);

const dados=await resposta.json();

if(!dados.length){
alert("Cidade não encontrada");
return;
}

const lat=dados[0].lat;
const lon=dados[0].lon;

map=L.map('map').setView([lat,lon],13);

L.tileLayer(
'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
{maxZoom:19}
).addTo(map);

L.marker([lat,lon])
.addTo(map)
.bindPopup("🚚 Objeto em Recife PE")
.openPopup();

}catch(e){

alert("Erro ao carregar mapa");

}

}

function salvarHistorico(codigo){

const historico=document.getElementById("historico");

const item=document.createElement("div");

item.className="history-item";

item.innerHTML="Código rastreado: "+codigo;

historico.prepend(item);

}

</script>

</body>
</html>
