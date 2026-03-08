<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CekPack PRO | Rastreamento & Admin</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
<style>
:root {
    --bg:#020617; --card:rgba(15,23,42,0.7); --panel:#0f172a;
    --brand:#fbbf24; --text:#f8fafc; --muted:#94a3b8;
    --border:rgba(255,255,255,0.08); --ok:#10b981; --danger:#f43f5e;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:'Plus Jakarta Sans',sans-serif;}
body{background:var(--bg);color:var(--text);min-height:100vh;padding:20px;}
.container{max-width:1200px;margin:0 auto;}
header{display:flex;justify-content:space-between;align-items:center;margin-bottom:40px;}
.logo{font-size:24px;font-weight:800;color:var(--brand);text-decoration:none;}
.live-dot{display:flex;align-items:center;gap:8px;font-size:12px;color:var(--ok);font-weight:600;background: rgba(16,185,129,0.1);padding:6px 12px;border-radius:20px;}
.dot{width:8px;height:8px;background:var(--ok);border-radius:50%;box-shadow:0 0 10px var(--ok);animation:pulse 2s infinite;}
@keyframes pulse{0%{opacity:1;}50%{opacity:0.4;}100%{opacity:1;}}
.hero{text-align:center;padding:40px 0;}
.hero h1{font-size:clamp(32px,5vw,56px);font-weight:800;margin-bottom:16px;line-height:1.1;}
.hero p{color:var(--muted);font-size:18px;margin-bottom:40px;}
.search-box{background: rgba(255,255,255,0.03);backdrop-filter: blur(12px);border:1px solid var(--border);padding:10px;border-radius:24px;display:flex;gap:10px;max-width:600px;margin:0 auto;box-shadow:0 25px 50px -12px rgba(0,0,0,0.5);}
.search-box input{flex:1;background:transparent;border:none;padding:16px 20px;color:white;font-size:16px;outline:none;}
.search-box button{background:var(--brand);color:#000;border:none;padding:0 32px;border-radius:16px;font-weight:800;cursor:pointer;transition:0.3s;}
.search-box button:hover{transform:scale(1.02);}
#error{color:#f87171;display:none;margin-top:24px;font-weight:600;}
#results{display:none;margin-top:60px;padding-bottom:100px;}
.grid{display:grid;grid-template-columns:1.3fr 0.7fr;gap:24px;}
.card{background:var(--card);backdrop-filter:blur(20px);border:1px solid var(--border);padding:32px;border-radius:32px;box-shadow:0 10px 30px rgba(0,0,0,0.2);}
.res-id{font-size:40px;font-weight:800;color:var(--brand);margin-bottom:30px;}
.info-item{display:flex;justify-content:space-between;padding:16px 0;border-bottom:1px solid var(--border);font-size:15px;}
.info-item span{color:var(--muted);}
.info-item strong{color:#fff;}
#map{height:400px;border-radius:24px;margin-top:24px;border:1px solid var(--border);}
.step{position:relative;padding-left:32px;margin-bottom:24px;}
.step::before{content:'';position:absolute;left:0;top:6px;width:12px;height:12px;border-radius:50%;background:var(--brand);box-shadow:0 0 12px var(--brand);}
.step p{font-weight:700;font-size:15px;}
.step span{color:var(--muted);font-size:13px;}

/* PAINEL ADMIN ESCONDIDO */
.dashboard{display:none;margin-top:60px;grid-template-columns:400px 1fr;gap:32px;}
.admin-card{background:var(--panel);border:1px solid var(--border);border-radius:24px;padding:32px;box-shadow:0 20px 40px rgba(0,0,0,0.3);}
.admin-card h2{font-size:20px;margin-bottom:24px;}
.input-group{margin-bottom:20px;}
.input-group label{display:block;font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:8px;font-weight:600;}
.input-group input{width:100%;background: rgba(0,0,0,0.2);border:1px solid var(--border);padding:14px 16px;color:#fff;border-radius:12px;font-size:14px;outline:none;}
.input-group input:focus{border-color:var(--brand);box-shadow:0 0 0 4px rgba(251,191,36,0.1);}
.btn{background:var(--brand);color:#000;border:none;padding:16px;width:100%;border-radius:14px;font-weight:800;cursor:pointer;margin-top:10px;}
.btn:hover{transform:translateY(-2px);box-shadow:0 10px 20px rgba(251,191,36,0.2);}
table{width:100%;border-collapse:collapse;}
th{text-align:left;color:var(--muted);padding:16px;border-bottom:1px solid var(--border);font-size:12px;text-transform:uppercase;}
td{padding:20px 16px;border-bottom:1px solid var(--border);font-size:14px;}
.code-tag{background:rgba(251,191,36,0.1);color:var(--brand);padding:4px 8px;border-radius:6px;font-weight:800;font-size:12px;}
.del-btn{color:var(--danger);font-weight:800;cursor:pointer;background:none;border:none;font-size:12px;}
#loader{font-size:12px;color:var(--brand);display:none;margin-top:10px;font-weight:600;}
@media(max-width:950px){.grid,.dashboard{grid-template-columns:1fr;}}
</style>
</head>
<body>

<header class="container">
<a href="#" class="logo">CekPack <span style="font-weight:400;color:#fff">PRO</span></a>
<div class="live-dot"><div class="dot"></div> SISTEMA: ATIVO</div>
</header>

<div class="container">
<section class="hero">
<h1>Sua carga, nossa prioridade.</h1>
<p>Rastreamento global em tempo real com precisão de 99.9%.</p>
<div class="search-box">
<input type="text" id="trackInput" placeholder="Código de rastreamento">
<button onclick="buscar()">Rastrear</button>
</div>
<p id="error">Objeto não localizado.</p>
</section>

<section id="results">
<div class="grid">
<div class="card">
<div id="res-id" class="res-id">--</div>
<div class="info-item"><span>Status</span> <strong id="res-status">--</strong></div>
<div class="info-item"><span>Localização</span> <strong id="res-local">--</strong></div>
<div id="map"></div>
</div>
<div class="card">
<h3 style="margin-bottom:30px;font-size:20px;">Rota e Logística</h3>
<div class="info-item"><span>Origem</span> <strong id="res-from">--</strong></div>
<div class="info-item"><span>Destino</span> <strong id="res-to">--</strong></div>
<div class="info-item"><span>Previsão</span> <strong id="res-eta">--</strong></div>
<div style="margin-top:30px;">
<div class="step">
<p id="st-status">Sincronizado</p>
<span id="st-local">Aguardando servidor</span>
</div>
</div>
</div>
</div>
</section>

<!-- PAINEL ADMIN -->
<div class="dashboard">
<div class="admin-card">
<h2>Nova Atualização</h2>
<div class="input-group"><label>Código de Rastreio</label><input type="text" id="code" placeholder="Ex: BR100"></div>
<div class="input-group"><label>Status Atual</label><input type="text" id="status" placeholder="Ex: Saiu para entrega"></div>
<div class="input-group"><label>Cidade / Localização</label><input type="text" id="local" placeholder="Ex: Curitiba, PR"><div id="loader">● Buscando coordenadas GPS...</div></div>
<div class="input-group"><label>Previsão de Entrega</label><input type="text" id="eta" placeholder="Ex: Hoje às 18:00"></div>
<div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
<div class="input-group"><label>Origem</label><input type="text" id="origin" placeholder="Cidade"></div>
<div class="input-group"><label>Destino</label><input type="text" id="destination" placeholder="Cidade"></div>
</div>
<button class="btn" onclick="salvar()">PUBLICAR NA REDE</button>
</div>

<div class="admin-card" style="padding:0;overflow:hidden;">
<div style="padding:32px 32px 0 32px;"><h2>Histórico de Operações</h2></div>
<table>
<thead><tr><th>Código</th><th>Status</th><th>Localização</th><th>Ação</th></tr></thead>
<tbody id="tbody"></tbody>
</table>
</div>
</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
let map, marker;
const DB_KEY='cekpack_database_final';

function buscar(){
    const code=document.getElementById('trackInput').value.trim().toUpperCase();

    // SE FOR SENHA ADMIN
    if(code==='ADMIN2233'){
        document.querySelector('.dashboard').style.display='grid';
        document.getElementById('error').style.display='none';
        document.getElementById('results').scrollIntoView({behavior:'smooth'});
        return;
    }

    const db=JSON.parse(localStorage.getItem(DB_KEY))||{};
    const data=db[code];
    if(data){
        document.getElementById('error').style.display='none';
        document.getElementById('results').style.display='block';
        document.getElementById('res-id').innerText=data.code;
        document.getElementById('res-status').innerText=data.status;
        document.getElementById('res-local').innerText=data.local;
        document.getElementById('res-from').innerText=data.from;
        document.getElementById('res-to').innerText=data.to;
        document.getElementById('res-eta').innerText=data.eta;
        document.getElementById('st-status').innerText=data.status;
        document.getElementById('st-local').innerText=data.local;

        const lat=parseFloat(data.lat),lng=parseFloat(data.lng);
        if(!map){
            map=L.map('map').setView([lat,lng],13);
            L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png',{attribution:'©OpenStreetMap'}).addTo(map);
            marker=L.marker([lat,lng]).addTo(map).bindPopup(data.local).openPopup();
        }else{
            map.setView([lat,lng],13);
            marker.setLatLng([lat,lng]).setPopupContent(data.local).openPopup();
            setTimeout(()=>map.invalidateSize(),200);
        }
        document.getElementById('results').scrollIntoView({behavior:'smooth'});
    }else{
        document.getElementById('error').style.display='block';
        document.getElementById('results').style.display='none';
    }
}

async function salvar(){
    const code=document.getElementById('code').value.trim().toUpperCase();
    const local=document.getElementById('local').value.trim();
    const loader=document.getElementById('loader');
    if(!code||!local)return alert("Código e Localização são obrigatórios.");
    loader.style.display='block';
    try{
        const res=await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(local)}`);
        const data=await res.json();
        if(data&&data.length>0){
            const db=JSON.parse(localStorage.getItem(DB_KEY))||{};
            db[code]={code,status:document.getElementById('status').value||"Processando",local:local,lat:data[0].lat,lng:data[0].lon,from:document.getElementById('origin').value||"N/A",to:document.getElementById('destination').value||"N/A",eta:document.getElementById('eta').value||"Em análise"};
            localStorage.setItem(DB_KEY,JSON.stringify(db));
            alert("Sucesso! Código publicado.");
            listar();
        }else alert("Cidade não encontrada no mapa. Tente: Nome da Cidade, Estado.");
    }catch(err){alert("Erro ao conectar com o servidor satelital.");}
    finally{loader.style.display='none';}
}

function excluir(code){
    if(confirm("Confirmar exclusão do código "+code+"?")){
        const db=JSON.parse(localStorage.getItem(DB_KEY));
        delete db[code];
        localStorage.setItem(DB_KEY,JSON.stringify(db));
        listar();
    }
}

function listar(){
    const db=JSON.parse(localStorage.getItem(DB_KEY))||{};
    const tbody=document.getElementById('tbody');
    tbody.innerHTML=Object.values(db).map(e=>`
        <tr>
            <td><span class="code-tag">${e.code}</span></td>
            <td style="font-weight:700">${e.status}</td>
            <td style="color:#94a3b8">${e.local}</td>
            <td><button class="del-btn" onclick="excluir('${e.code}')">REMOVER</button></td>
        </tr>
    `).join('');
}
listar();
</script>
</body>
</html>
