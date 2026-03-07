
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Suas Encomendas</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

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
border:none;
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
display:none;
}

#map{
height:200px;
border-radius:12px;
margin-bottom:15px;
display:none;
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
</div>

<div id="map"></div>

</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

function rastrear(){

let codigo = document.getElementById("codigo").value.trim()

if(codigo === "BR123456789BR"){

document.getElementById("rota").innerText="1"
document.getElementById("transito").innerText="1"
document.getElementById("entregue").innerText="0"
document.getElementById("total").innerText="2"

document.getElementById("bar").style.width="60%"

let status = document.getElementById("status")
status.style.display="block"

status.innerHTML=
"📦 Código: "+codigo+"<br>"+
"Status: Objeto em trânsito (60%)<br>"+
"Local: Recife PE<br>"+
"Data: 06/03/2026"

let mapDiv=document.getElementById("map")
mapDiv.style.display="block"

setTimeout(function(){

var map = L.map('map').setView([-8.0476,-34.8770],13)

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
maxZoom:19
}).addTo(map)

L.marker([-8.0476,-34.8770]).addTo(map)
.bindPopup("🚚 Objeto em Recife PE - 2026")
.openPopup()

},300)

}else{

alert("Código não encontrado")

}

}

</script>

</body>
</html>
