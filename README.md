```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Rastrear Encomenda</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Inter',sans-serif;
}

body{
background:radial-gradient(circle at top,#0b2a55,#020617);
color:white;
display:flex;
justify-content:center;
min-height:100vh;
}

.container{
width:100%;
max-width:420px;
padding:20px;
}

.header{
text-align:center;
font-size:22px;
font-weight:700;
margin-bottom:20px;
}

.cards{
display:grid;
grid-template-columns:1fr 1fr;
gap:12px;
margin-bottom:15px;
}

.card{
background:linear-gradient(145deg,#0f172a,#020617);
border-radius:16px;
padding:15px;
text-align:center;
box-shadow:0 10px 25px rgba(0,0,0,0.6);
}

.card span{
display:block;
font-size:13px;
opacity:.8;
}

.card b{
font-size:22px;
}

.input-box{
background:#0f172a;
border-radius:14px;
display:flex;
align-items:center;
padding:10px;
margin-bottom:10px;
}

.input-box input{
flex:1;
background:none;
border:none;
color:white;
font-size:14px;
outline:none;
}

.button{
width:100%;
padding:14px;
border-radius:14px;
border:none;
background:linear-gradient(90deg,#1d4ed8,#2563eb);
color:white;
font-weight:700;
font-size:16px;
cursor:pointer;
margin-bottom:15px;
}

.progress{
height:12px;
background:#020617;
border-radius:20px;
overflow:hidden;
margin-bottom:15px;
}

.bar{
height:100%;
width:0%;
background:linear-gradient(90deg,#facc15,#f59e0b);
transition:1s;
}

.status{
background:linear-gradient(145deg,#0f172a,#020617);
padding:15px;
border-radius:16px;
margin-bottom:15px;
}

.map{
height:160px;
border-radius:16px;
background:url("https://i.imgur.com/6YV6G8k.jpg") center/cover;
margin-bottom:15px;
}

.history{
background:linear-gradient(145deg,#0f172a,#020617);
border-radius:16px;
padding:15px;
font-size:14px;
opacity:.8;
}

.footer{
text-align:center;
margin-top:20px;
opacity:.7;
font-size:13px;
}

</style>
</head>

<body>

<div class="container">

<div class="header">
📦 RASTREAR ENCOMENDA
</div>

<div class="cards">

<div class="card">
<span>Em rota</span>
<b id="rota">0</b>
</div>

<div class="card">
<span>Em trânsito</span>
<b id="transito">0</b>
</div>

<div class="card">
<span>Entregues</span>
<b id="entregue">0</b>
</div>

<div class="card">
<span>Total</span>
<b id="total">0</b>
</div>

</div>

<div class="input-box">
<input type="text" id="codigo" placeholder="Digite o código de rastreio">
</div>

<button class="button" onclick="rastrear()">RASTREAR</button>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div class="status" id="status">
Digite um código de rastreio para consultar.
</div>

<div class="map"></div>

<div class="history">
Nenhum código rastreado ainda.
</div>

<div class="footer">
🇧🇷 Segurança Garantida
</div>

</div>

<script>

function rastrear(){

const codigo=document.getElementById("codigo").value;

if(!codigo){
alert("Digite um código");
return;
}

document.getElementById("rota").innerHTML="1";
document.getElementById("transito").innerHTML="1";
document.getElementById("entregue").innerHTML="0";
document.getElementById("total").innerHTML="2";

document.getElementById("bar").style.width="60%";

document.getElementById("status").innerHTML=
"📦 Código: "+codigo+
"<br>Status: Objeto em trânsito"+
"<br>Local: Recife / PE";

}

</script>

</body>
</html>
```
