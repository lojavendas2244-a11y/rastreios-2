
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CekPack PRO | Rastreamento</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">

<style>

:root{
--bg:#020617;
--card:rgba(15,23,42,0.7);
--brand:#fbbf24;
--text:#f8fafc;
--muted:#94a3b8;
--border:rgba(255,255,255,0.08);
--ok:#10b981;
}

*{
box-sizing:border-box;
margin:0;
padding:0;
font-family:'Plus Jakarta Sans',sans-serif;
}

body{
background:var(--bg);
color:var(--text);
min-height:100vh;
padding:20px;
}

.container{
max-width:1100px;
margin:auto;
}

header{
display:flex;
justify-content:space-between;
align-items:center;
margin-bottom:40px;
}

.logo{
font-size:24px;
font-weight:800;
color:#fbbf24;
}

.live-dot{
display:flex;
align-items:center;
gap:8px;
font-size:12px;
color:#10b981;
font-weight:600;
}

.dot{
width:8px;
height:8px;
background:#10b981;
border-radius:50%;
box-shadow:0 0 10px #10b981;
animation:pulse 2s infinite;
}

@keyframes pulse{
0%{opacity:1}
50%{opacity:0.4}
100%{opacity:1}
}

.hero{
text-align:center;
padding:40px 0;
}

.hero h1{
font-size:40px;
margin-bottom:10px;
}

.hero p{
color:#94a3b8;
margin-bottom:30px;
}

.search-box{
display:flex;
gap:10px;
max-width:500px;
margin:auto;
background:#0f172a;
padding:10px;
border-radius:20px;
border:1px solid rgba(255,255,255,0.08);
}

.search-box input{
flex:1;
background:none;
border:none;
color:white;
padding:12px;
outline:none;
}

.search-box button{
background:#fbbf24;
border:none;
padding:12px 24px;
border-radius:12px;
font-weight:800;
cursor:pointer;
}

#error{
display:none;
margin-top:20px;
color:red;
}

#results{
display:none;
margin-top:50px;
}

.card{
background:rgba(15,23,42,0.7);
border:1px solid rgba(255,255,255,0.08);
padding:30px;
border-radius:24px;
}

.res-id{
font-size:32px;
font-weight:800;
color:#fbbf24;
margin-bottom:20px;
}

.info-item{
display:flex;
justify-content:space-between;
padding:10px 0;
border-bottom:1px solid rgba(255,255,255,0.08);
}

.step{
margin-top:20px;
padding-left:20px;
border-left:3px solid #fbbf24;
}

.step p{
font-weight:700;
}

.step span{
font-size:13px;
color:#94a3b8;
}

#map{
height:300px;
margin-top:20px;
border-radius:20px;
}

</style>
</head>

<body>

<header class="container">

<div class="logo">CekPack PRO</div>

<div class="live-dot">
<div class="dot"></div>
SISTEMA ATIVO
</div>

</header>

<div class="container">

<section class="hero">

<h1>Sua carga, nossa prioridade.</h1>

<p>Rastreamento global em tempo real.</p>

<div class="search-box">

<input type="text" id="trackInput" placeholder="Código de rastreio">

<button onclick="buscar()">Rastrear</button>

</div>

<p id="error">Objeto não localizado.</p>

</section>

<section id="results">

<div class="card">

<div id="res-id" class="res-id"></div>

<div class="info-item">
<span>Status</span>
<strong id="res-status"></strong>
</div>

<div class="info-item">
<span>Localização</span>
<strong id="res-local"></strong>
</div>

<div class="info-item">
<span>Entrega prevista</span>
<strong>17/03/2026</strong>
</div>

<div id="map"></div>

<div class="step">
<p>Objeto em rota</p>
<span>Local: Itabaiana - PB • Data: 11/03/2026</span>
</div>

</div>

</section>

</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

let map

function buscar(){

let code=document.getElementById("trackInput").value.trim()

if(code==="BR987654321BR"){

document.getElementById("results").style.display="block"
document.getElementById("error").style.display="none"

document.getElementById("res-id").innerText=code
document.getElementById("res-status").innerText="Objeto em rota"
document.getElementById("res-local").innerText="Itabaiana - PB"

setTimeout(initMap,200)

}else{

document.getElementById("results").style.display="none"
document.getElementById("error").style.display="block"

}

}

function initMap(){

if(map){
map.remove()
}

map=L.map('map').setView([-7.3319,-35.3317],11)

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
maxZoom:19
}).addTo(map)

L.marker([-7.3319,-35.3317]).addTo(map)
.bindPopup("Objeto em Itabaiana - PB")
.openPopup()

}

</script>

</body>
