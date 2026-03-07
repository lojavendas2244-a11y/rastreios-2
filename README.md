<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rastreamento</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

<style>

body{
font-family:Arial;
background:#0f172a;
color:white;
display:flex;
justify-content:center;
}

.container{
width:400px;
padding:20px;
}

input{
width:100%;
padding:10px;
margin-bottom:10px;
border-radius:8px;
border:none;
}

button{
width:100%;
padding:10px;
background:#3b82f6;
border:none;
color:white;
border-radius:8px;
font-weight:bold;
}

#map{
height:220px;
margin-top:15px;
border-radius:10px;
display:none;
}

.status{
margin-top:10px;
display:none;
background:#1e293b;
padding:10px;
border-radius:8px;
}

</style>
</head>

<body>

<div class="container">

<h2>📦 Rastrear Encomenda</h2>

<input id="codigo" placeholder="Digite o código">

<button onclick="rastrear()">Rastrear</button>

<div class="status" id="status"></div>

<div id="map"></div>

</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

let map;

function rastrear(){

let codigo=document.getElementById("codigo").value

if(codigo==="BR123456789BR"){

let status=document.getElementById("status")
status.style.display="block"
status.innerHTML="📦 Status: Em trânsito<br>📍 Recife PE<br>📅 2026"

let mapDiv=document.getElementById("map")
mapDiv.style.display="block"

setTimeout(()=>{

if(map){
map.remove()
}

map=L.map("map").setView([-8.0476,-34.8770],13)

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",{
maxZoom:19
}).addTo(map)

const icon=L.icon({
iconUrl:"https://cdn-icons-png.flaticon.com/512/684/684908.png",
iconSize:[35,35],
iconAnchor:[17,35],
popupAnchor:[0,-30]
})

L.marker([-8.0476,-34.8770],{icon:icon})
.addTo(map)
.bindPopup("🚚 Encomenda em trânsito<br>Recife PE<br>2026")
.openPopup()

},300)

}else{

alert("Código não encontrado")

}

}

</script>

</body>
</html>
