<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rastrear Encomenda</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Inter, sans-serif;
}

body{
background:linear-gradient(180deg,#0f1d35,#091424);
color:#fff;
padding:20px;
}

h1{
text-align:center;
margin-bottom:20px;
color:#3b82f6;
}

.container{
max-width:500px;
margin:auto;
}

.cards{
display:grid;
grid-template-columns:1fr 1fr;
gap:10px;
margin-bottom:20px;
display:none;
}

.card{
background:#1e293b;
padding:15px;
border-radius:10px;
text-align:center;
}

input{
width:100%;
padding:15px;
border-radius:10px;
border:none;
margin-bottom:10px;
font-size:16px;
}

button{
width:100%;
padding:15px;
border:none;
border-radius:10px;
background:#3b82f6;
color:#fff;
font-size:16px;
cursor:pointer;
}

button:hover{
background:#2563eb;
}

.resultado{
margin-top:20px;
background:#1e293b;
padding:15px;
border-radius:10px;
display:none;
}

.barra{
width:100%;
height:10px;
background:#1f2937;
border-radius:10px;
margin:10px 0;
overflow:hidden;
display:none;
}

.progresso{
height:10px;
width:60%;
background:#f59e0b;
}

#map{
height:300px;
margin-top:15px;
border-radius:10px;
display:none;
}

</style>
</head>

<body>

<div class="container">

<h1>rastreiros-2</h1>

<h2 style="text-align:center;margin-bottom:20px;">📦 Rastrear Encomenda</h2>

<div class="cards" id="cards">

<div class="card">
<div>Em rota</div>
<strong>1</strong>
</div>

<div class="card">
<div>Em trânsito</div>
<strong>1</strong>
</div>

<div class="card">
<div>Entregues</div>
<strong>0</strong>
</div>

<div class="card">
<div>Total</div>
<strong>2</strong>
</div>

</div>

<input id="codigo" placeholder="Digite o código de rastreio">

<button onclick="rastrear()">Rastrear</button>

<div class="barra" id="barra">
<div class="progresso"></div>
</div>

<div class="resultado" id="resultado">
<p><strong>Código:</strong> BR123456789BR</p>
<p><strong>Status:</strong> Objeto em trânsito (60%)</p>
<p><strong>Local:</strong> Recife PE</p>
</div>

<div id="map"></div>

</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

function rastrear(){

let codigo = document.getElementById("codigo").value
let resultado = document.getElementById("resultado")
let cards = document.getElementById("cards")
let barra = document.getElementById("barra")
let mapDiv = document.getElementById("map")

if(codigo == "BR123456789BR"){

resultado.style.display = "block"
cards.style.display = "grid"
barra.style.display = "block"
mapDiv.style.display = "block"

setTimeout(function(){

var map = L.map('map').setView([-8.05,-34.88], 13);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
maxZoom: 19
}).addTo(map);

L.marker([-8.05,-34.88]).addTo(map)
.bindPopup("Objeto em trânsito - Recife PE")
.openPopup();

},500)

}else{

alert("Código não encontrado")

}

}

</script>

</body>
</html>
