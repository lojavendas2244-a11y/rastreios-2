```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Rastrear Encomenda</title>

<style>

body{
margin:0;
font-family:Arial;
background:#0b1d3a;
color:white;
display:flex;
justify-content:center;
}

.container{
width:100%;
max-width:420px;
padding:20px;
}

h1{
text-align:center;
margin-bottom:20px;
}

.cards{
display:grid;
grid-template-columns:1fr 1fr;
gap:10px;
}

.card{
background:#111c3a;
padding:15px;
border-radius:10px;
text-align:center;
}

.card b{
font-size:22px;
}

input{
width:100%;
padding:12px;
margin-top:15px;
border-radius:10px;
border:none;
}

button{
width:100%;
padding:14px;
margin-top:10px;
border:none;
border-radius:10px;
background:#1d4ed8;
color:white;
font-size:16px;
font-weight:bold;
}

.progress{
width:100%;
height:10px;
background:#000;
border-radius:20px;
margin-top:15px;
}

.bar{
height:100%;
width:0%;
background:orange;
border-radius:20px;
}

.status{
margin-top:15px;
background:#111c3a;
padding:15px;
border-radius:10px;
}

.map{
margin-top:15px;
height:150px;
border-radius:10px;
background:url("https://maps.gstatic.com/tactile/basepage/pegman_sherlock.png") center no-repeat;
background-color:#1e293b;
}

.history{
margin-top:15px;
background:#111c3a;
padding:15px;
border-radius:10px;
}

</style>
</head>

<body>

<div class="container">

<h1>📦 Rastrear Encomenda</h1>

<div class="cards">
<div class="card">Em rota<br><b id="rota">0</b></div>
<div class="card">Em trânsito<br><b id="transito">0</b></div>
<div class="card">Entregues<br><b id="entregue">0</b></div>
<div class="card">Total<br><b id="total">0</b></div>
</div>

<input type="text" id="codigo" placeholder="Digite o código">

<button onclick="rastrear()">RASTREAR</button>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div class="status" id="status">
Digite um código para rastrear.
</div>

<div class="map"></div>

<div class="history" id="historico">
Nenhum rastreio ainda.
</div>

</div>

<script>

function rastrear(){

let codigo=document.getElementById("codigo").value;

if(codigo==""){
alert("Digite o código");
return;
}

document.getElementById("rota").innerText="1";
document.getElementById("transito").innerText="1";
document.getElementById("entregue").innerText="0";
document.getElementById("total").innerText="2";

document.getElementById("bar").style.width="60%";

document.getElementById("status").innerHTML=
"📦 Código: "+codigo+
"<br>Status: Objeto em trânsito"+
"<br>Local: Recife / PE";

document.getElementById("historico").innerHTML=
"Código rastreado: "+codigo;

}

</script>

</body>
</html>
```
