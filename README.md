<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CekPack Global | Rastreamento Premium</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --bg: #020617; --card: rgba(15, 23, 42, 0.7); --brand: #fbbf24; --text: #f8fafc; --muted: #94a3b8; --border: rgba(255,255,255,0.08); --ok: #10b981; }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Plus Jakarta Sans', sans-serif; }
        body { background: var(--bg); color: var(--text); background-image: radial-gradient(circle at 50% -20%, #1e293b 0%, #020617 100%); min-height: 100vh; }
        .container { max-width: 1100px; margin: 0 auto; padding: 0 24px; }
        
        header { padding: 40px 0; display: flex; justify-content: space-between; align-items: center; }
        .logo { font-size: 24px; font-weight: 800; color: var(--brand); letter-spacing: -1px; text-decoration: none; }
        .live-dot { display: flex; align-items: center; gap: 8px; font-size: 12px; color: var(--ok); font-weight: 600; background: rgba(16, 185, 129, 0.1); padding: 6px 12px; border-radius: 20px; }
        .dot { width: 8px; height: 8px; background: var(--ok); border-radius: 50%; box-shadow: 0 0 10px var(--ok); animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }

        .hero { padding: 80px 0 40px; text-align: center; }
        .hero h1 { font-size: clamp(32px, 5vw, 56px); font-weight: 800; margin-bottom: 16px; letter-spacing: -2px; line-height: 1.1; }
        .hero p { color: var(--muted); font-size: 18px; margin-bottom: 40px; }

        .search-box { 
            background: rgba(255,255,255,0.03); 
            backdrop-filter: blur(12px); 
            border: 1px solid var(--border); 
            padding: 10px; border-radius: 24px; display: flex; gap: 10px; max-width: 600px; margin: 0 auto;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
        }
        .search-box input { flex: 1; background: transparent; border: none; padding: 16px 20px; color: white; font-size: 16px; outline: none; }
        .search-box button { background: var(--brand); color: #000; border: none; padding: 0 32px; border-radius: 16px; font-weight: 800; cursor: pointer; transition: 0.3s; }
        .search-box button:hover { transform: scale(1.02); }

        #results { display: none; margin-top: 60px; padding-bottom: 100px; animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes slideUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }

        .grid { display: grid; grid-template-columns: 1.3fr 0.7fr; gap: 24px; }
        .card { background: var(--card); backdrop-filter: blur(20px); border: 1px solid var(--border); padding: 32px; border-radius: 32px; box-shadow: 0 10px 30px rgba(0,0,0,0.2); }
        .res-id { font-size: 40px; font-weight: 800; color: var(--brand); margin-bottom: 30px; letter-spacing: -1px; }
        
        .info-item { display: flex; justify-content: space-between; padding: 16px 0; border-bottom: 1px solid var(--border); font-size: 15px; }
        .info-item span { color: var(--muted); }
        .info-item strong { color: #fff; }

        /* MAPA BRANCO (LIGHT MODE) */
        #map { height: 450px; border-radius: 24px; margin-top: 24px; border: 1px solid var(--border); z-index: 1; }
        
        .step { position: relative; padding-left: 32px; margin-bottom: 24px; }
        .step::before { content: ''; position: absolute; left: 0; top: 6px; width: 12px; height: 12px; border-radius: 50%; background: var(--brand); box-shadow: 0 0 12px var(--brand); }
        .step p { font-weight: 700; font-size: 15px; }
        .step span { color: var(--muted); font-size: 13px; }

        @media (max-width: 900px) { .grid { grid-template-columns: 1fr; } }
    </style>
</head>
<body>
    <header class="container">
        <a href="#" class="logo">CekPack <span style="font-weight:400; color:#fff">PRO</span></a>
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
            <p id="error" style="color: #f87171; display: none; margin-top: 24px; font-weight: 600;">Objeto não localizado.</p>
        </section>

        <section id="results">
            <div class="grid">
                <div class="card">
                    <div id="res-id" class="res-id">--</div>
                    <div class="info-item"><span>Status</span> <strong id="res-status" style="color: var(--ok)">--</strong></div>
                    <div class="info-item"><span>Localização</span> <strong id="res-local">--</strong></div>
                    <div id="map"></div>
                </div>
                <div class="card">
                    <h3 style="margin-bottom: 30px; font-size: 20px;">Rota e Logística</h3>
                    <div class="info-item"><span>Origem</span> <strong id="res-from">--</strong></div>
                    <div class="info-item"><span>Destino</span> <strong id="res-to">--</strong></div>
                    <div class="info-item"><span>Previsão</span> <strong id="res-eta">--</strong></div>
                    
                    <div style="margin-top: 30px;">
                        <div class="step">
                            <p id="st-status">Sincronizado</p>
                            <span id="st-local">Aguardando servidor</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        let map, marker;
        const DB_KEY = 'cekpack_database_final';

        function buscar() {
            const code = document.getElementById('trackInput').value.trim().toUpperCase();
            const db = JSON.parse(localStorage.getItem(DB_KEY)) || {};
            const data = db[code];

            if (data) {
                document.getElementById('error').style.display = 'none';
                document.getElementById('results').style.display = 'block';
                document.getElementById('res-id').innerText = data.code;
                document.getElementById('res-status').innerText = data.status;
                document.getElementById('res-local').innerText = data.local;
                document.getElementById('res-from').innerText = data.from;
                document.getElementById('res-to').innerText = data.to;
                document.getElementById('res-eta').innerText = data.eta;
                document.getElementById('st-status').innerText = data.status;
                document.getElementById('st-local').innerText = data.local;

                const lat = parseFloat(data.lat), lng = parseFloat(data.lng);
                if (!map) {
                    map = L.map('map').setView([lat, lng], 13);
                    // MAPA BRANCO (CLARO)
                    L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
                        attribution: '©OpenStreetMap'
                    }).addTo(map);
                    marker = L.marker([lat, lng]).addTo(map).bindPopup(data.local).openPopup();
                } else {
                    map.setView([lat, lng], 13);
                    marker.setLatLng([lat, lng]).setPopupContent(data.local).openPopup();
                    setTimeout(() => map.invalidateSize(), 200);
                }
                document.getElementById('results').scrollIntoView({ behavior: 'smooth' });
            } else {
                document.getElementById('error').style.display = 'block';
                document.getElementById('results').style.display = 'none';
            }
        }
    </script>
</body>
</html>
