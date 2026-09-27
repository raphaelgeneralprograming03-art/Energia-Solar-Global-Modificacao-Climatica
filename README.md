
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GEO-CORE 9000 // Centro de Comando Climático Global</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        hud: {
                            bg: '#050a14',
                            panel: 'rgba(10, 20, 38, 0.75)',
                            border: '#00f0ff',
                            text: '#a0f0ff',
                            accent: '#00ffcc',
                            danger: '#ff3366',
                            warning: '#ffaa00',
                            gold: '#ffd700'
                        }
                    },
                    fontFamily: {
                        mono: ['Courier New', 'monospace'],
                        sans: ['Inter', 'sans-serif'],
                        hud: ['Orbitron', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts: Orbitron & Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&family=Orbitron:wght@400;600;800;900&display=swap" rel="stylesheet">
    <!-- FontAwesome icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        body {
            background-color: #030712;
            color: #d1feff;
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
            background-image: 
                radial-gradient(circle at 50% 50%, rgba(0, 240, 255, 0.05) 0%, transparent 80%),
                linear-gradient(rgba(0, 240, 255, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 240, 255, 0.03) 1px, transparent 1px);
            background-size: 100% 100%, 30px 30px, 30px 30px;
        }

        .hud-font {
            font-family: 'Orbitron', sans-serif;
        }

        /* HUD Glass Panel Styling */
        .hud-panel {
            background: rgba(8, 16, 32, 0.82);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(0, 240, 255, 0.25);
            box-shadow: 0 0 20px rgba(0, 240, 255, 0.08), inset 0 0 15px rgba(0, 240, 255, 0.03);
            position: relative;
        }

        /* Holographic Corner Accent Accents */
        .hud-panel::before {
            content: '';
            position: absolute;
            top: -1px; left: -1px;
            width: 10px; height: 10px;
            border-top: 2px solid #00f0ff;
            border-left: 2px solid #00f0ff;
        }
        .hud-panel::after {
            content: '';
            position: absolute;
            bottom: -1px; right: -1px;
            width: 10px; height: 10px;
            border-bottom: 2px solid #00f0ff;
            border-right: 2px solid #00f0ff;
        }

        .hud-danger-panel {
            border-color: rgba(255, 51, 102, 0.4);
            box-shadow: 0 0 20px rgba(255, 51, 102, 0.15);
        }
        .hud-danger-panel::before { border-color: #ff3366; }
        .hud-danger-panel::after { border-color: #ff3366; }

        /* Custom Hologram Scanline Effect */
        .scanlines {
            position: relative;
            overflow: hidden;
        }
        .scanlines::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.03), rgba(0, 255, 0, 0.01), rgba(0, 0, 255, 0.03));
            z-index: 10;
            background-size: 100% 4px, 6px 100%;
            pointer-events: none;
            opacity: 0.6;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(5, 10, 20, 0.8);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(0, 240, 255, 0.4);
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(0, 240, 255, 0.8);
        }

        /* Sci-Fi Sliders */
        input[type=range] {
            -webkit-appearance: none;
            background: rgba(0, 240, 255, 0.1);
            border: 1px solid rgba(0, 240, 255, 0.3);
            border-radius: 4px;
            height: 6px;
        }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 16px;
            width: 16px;
            border-radius: 2px;
            background: #00f0ff;
            cursor: pointer;
            box-shadow: 0 0 10px #00f0ff;
        }

        /* Pulse Animations */
        @keyframes radar-pulse {
            0% { transform: scale(1); opacity: 0.8; }
            50% { transform: scale(1.05); opacity: 0.3; }
            100% { transform: scale(1); opacity: 0.8; }
        }
        .animate-radar {
            animation: radar-pulse 3s infinite ease-in-out;
        }

        @keyframes glow-red {
            0%, 100% { box-shadow: 0 0 10px rgba(255, 51, 102, 0.4); }
            50% { box-shadow: 0 0 25px rgba(255, 51, 102, 0.8); }
        }
        .glow-danger {
            animation: glow-red 2s infinite;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between p-2 md:p-4 select-none">

    <!-- TOP HEADER / STATUS BAR -->
    <header class="hud-panel p-3 mb-3 rounded-lg flex flex-wrap items-center justify-between gap-4">
        <div class="flex items-center gap-3">
            <div class="p-2 bg-cyan-950 border border-cyan-400 rounded-lg text-cyan-400 animate-pulse">
                <i class="fa-solid fa-earth-americas text-2xl"></i>
            </div>
            <div>
                <h1 class="hud-font font-black text-xl md:text-2xl text-cyan-300 tracking-wider flex items-center gap-2">
                    GEO-CORE <span class="text-xs px-2 py-0.5 bg-cyan-500/20 text-cyan-400 border border-cyan-400/40 rounded">v4.8 HOLOGRAPHIC</span>
                </h1>
                <p class="text-xs text-cyan-400/70 font-mono">SISTEMA PLANETÁRIO DE MODIFICAÇÃO CLIMÁTICA E GEOENGENHARIA</p>
            </div>
        </div>

        <!-- Global Status Indicators -->
        <div class="flex items-center gap-6 text-xs font-mono">
            <div class="flex items-center gap-2">
                <span class="w-2.5 h-2.5 rounded-full bg-emerald-400 animate-ping"></span>
                <span class="text-gray-400">ESTADO REDE:</span>
                <span class="text-emerald-400 font-bold" id="sys-status">OPERACIONAL</span>
            </div>
            <div class="hidden sm:flex flex-col">
                <span class="text-gray-400 text-[10px]">TEMPO SIMULADO</span>
                <span class="text-cyan-300 font-bold" id="sim-clock">2088-09-27 12:00:00 UTC</span>
            </div>
            <div class="flex gap-2">
                <button id="audio-toggle-btn" onclick="toggleAudio()" class="px-3 py-1.5 bg-cyan-900/40 hover:bg-cyan-800/60 border border-cyan-500/40 rounded text-cyan-300 text-xs flex items-center gap-1.5 transition">
                    <i class="fa-solid fa-volume-high" id="audio-icon"></i> <span id="audio-label">SOM ON</span>
                </button>
                <button onclick="openInfoModal()" class="px-3 py-1.5 bg-cyan-500/20 hover:bg-cyan-500/40 border border-cyan-400 rounded text-cyan-200 text-xs font-bold transition flex items-center gap-1">
                    <i class="fa-solid fa-circle-info"></i> MANUAL TÉCNICO
                </button>
            </div>
        </div>
    </header>

    <!-- MAIN GRID CONTAINER -->
    <main class="grid grid-cols-1 lg:grid-cols-12 gap-4 flex-1">

        <!-- LEFT SIDEBAR: Geoengineering Controls Matrix -->
        <aside class="lg:col-span-3 flex flex-col gap-3">
            <div class="hud-panel p-4 rounded-lg flex-1 flex flex-col justify-between">
                <div>
                    <h2 class="hud-font text-sm font-bold text-cyan-300 border-b border-cyan-500/30 pb-2 mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-sliders text-cyan-400"></i> CONTROLES DE ATUAÇÃO
                    </h2>

                    <!-- SRM Aerosol Injection Slider -->
                    <div class="mb-5 bg-cyan-950/30 p-3 rounded border border-cyan-500/20">
                        <div class="flex justify-between items-center mb-1">
                            <label class="text-xs font-bold text-cyan-200 flex items-center gap-1.5">
                                <i class="fa-solid fa-cloud-sun text-yellow-400"></i> Aerossóis Estratosféricos (SRM)
                            </label>
                            <span class="text-xs font-mono text-cyan-400 font-bold" id="srm-val">15%</span>
                        </div>
                        <p class="text-[10px] text-gray-400 mb-2">Reflete radiação solar para resfriamento planetário direto.</p>
                        <input type="range" id="srm-slider" min="0" max="100" value="15" class="w-full">
                    </div>

                    <!-- Cloud Seeding Control -->
                    <div class="mb-5 bg-cyan-950/30 p-3 rounded border border-cyan-500/20">
                        <div class="flex justify-between items-center mb-1">
                            <label class="text-xs font-bold text-cyan-200 flex items-center gap-1.5">
                                <i class="fa-solid fa-cloud-showers-heavy text-blue-400"></i> Semeadura de Nuvens Agro
                            </label>
                            <span class="text-xs font-mono text-cyan-400 font-bold" id="seeding-val">40%</span>
                        </div>
                        <p class="text-[10px] text-gray-400 mb-2">Injeta iodeto de prata para chuvas em secas agrícolas.</p>
                        <input type="range" id="seeding-slider" min="0" max="100" value="40" class="w-full">
                    </div>

                    <!-- DAC Carbon Capture Grid -->
                    <div class="mb-5 bg-cyan-950/30 p-3 rounded border border-cyan-500/20">
                        <div class="flex justify-between items-center mb-1">
                            <label class="text-xs font-bold text-cyan-200 flex items-center gap-1.5">
                                <i class="fa-solid fa-filter text-emerald-400"></i> Potência Torres DAC (CO2)
                            </label>
                            <span class="text-xs font-mono text-cyan-400 font-bold" id="dac-val">60%</span>
                        </div>
                        <p class="text-[10px] text-gray-400 mb-2">Captura direta de CO2 atmosférico e mineralização.</p>
                        <input type="range" id="dac-slider" min="0" max="100" value="60" class="w-full">
                    </div>

                    <!-- Interactive Weapon/Ionization Mode Selection -->
                    <div class="mb-4">
                        <label class="text-xs font-bold text-cyan-300 block mb-2">MODO DE INTERVENÇÃO MANUAL</label>
                        <div class="grid grid-cols-2 gap-2">
                            <button id="mode-laser" onclick="setInterventionMode('laser')" class="px-2 py-2 bg-cyan-500/20 border border-cyan-400 rounded text-xs text-cyan-200 font-bold flex flex-col items-center gap-1 hover:bg-cyan-500/30 transition">
                                <i class="fa-solid fa-bolt text-amber-400 text-sm"></i>
                                Feixe Ionizante
                            </button>
                            <button id="mode-seed" onclick="setInterventionMode('seed')" class="px-2 py-2 bg-cyan-950/50 border border-cyan-500/30 rounded text-xs text-gray-400 flex flex-col items-center gap-1 hover:bg-cyan-500/20 transition">
                                <i class="fa-solid fa-droplet text-blue-400 text-sm"></i>
                                Induzir Chuva
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Immediate Action Tactical Triggers -->
                <div class="pt-3 border-t border-cyan-500/30 space-y-2">
                    <button onclick="triggerPulseCooling()" class="w-full py-2 bg-gradient-to-r from-cyan-600 to-blue-600 hover:from-cyan-500 hover:to-blue-500 text-white font-bold text-xs rounded border border-cyan-300 shadow-lg shadow-cyan-500/20 flex items-center justify-center gap-2 transition">
                        <i class="fa-solid fa-snowflake"></i> PULSO DE RESFRIAMENTO
                    </button>
                    <button onclick="triggerGlobalStormNeutralize()" class="w-full py-2 bg-gradient-to-r from-red-600 to-amber-600 hover:from-red-500 hover:to-amber-500 text-white font-bold text-xs rounded border border-red-300 shadow-lg shadow-red-500/20 flex items-center justify-center gap-2 transition">
                        <i class="fa-solid fa-shield-halved"></i> DISSIPAR FURACÕES ATIVOS
                    </button>
                </div>
            </div>
        </aside>

        <!-- CENTER PANEL: Sci-Fi Interactive Globe Radar -->
        <section class="lg:col-span-6 flex flex-col gap-3">
            <div class="hud-panel p-3 rounded-lg flex-1 flex flex-col relative scanlines overflow-hidden">
                <!-- Canvas Title Overlay -->
                <div class="flex justify-between items-center mb-2 z-20">
                    <div class="flex items-center gap-2">
                        <span class="w-2 h-2 rounded-full bg-cyan-400 animate-pulse"></span>
                        <h2 class="hud-font text-xs font-bold text-cyan-300">RADAR ATMOSFÉRICO GLOBAL // WAR-ROOM 2D</h2>
                    </div>
                    <div class="flex items-center gap-2 text-[10px] font-mono">
                        <button onclick="toggleLayer('hurricanes')" id="btn-layer-storms" class="px-2 py-0.5 bg-cyan-500/30 border border-cyan-400 text-cyan-200 rounded">Furacões</button>
                        <button onclick="toggleLayer('droughts')" id="btn-layer-droughts" class="px-2 py-0.5 bg-cyan-500/30 border border-cyan-400 text-cyan-200 rounded">Secas</button>
                        <button onclick="toggleLayer('dac')" id="btn-layer-dac" class="px-2 py-0.5 bg-cyan-500/30 border border-cyan-400 text-cyan-200 rounded">Torres DAC</button>
                    </div>
                </div>

                <!-- Radar Canvas Element -->
                <div class="relative w-full flex-1 bg-black/80 rounded border border-cyan-500/30 overflow-hidden cursor-crosshair" id="canvas-container">
                    <canvas id="radarCanvas" class="w-full h-full block"></canvas>
                    
                    <!-- Canvas Instruction Overlay -->
                    <div class="absolute bottom-2 left-2 pointer-events-none bg-black/60 backdrop-blur border border-cyan-500/30 px-2 py-1 rounded text-[10px] font-mono text-cyan-300">
                        <i class="fa-solid fa-crosshairs text-amber-400"></i> Clique no radar para disparar <span id="active-mode-label" class="text-amber-300 font-bold">Feixe Ionizante</span>
                    </div>

                    <!-- Dynamic Target Reticle Overlay -->
                    <div id="target-info" class="hidden absolute pointer-events-none bg-cyan-950/90 border border-cyan-400 px-3 py-1.5 rounded text-xs font-mono text-cyan-200 shadow-xl">
                        <div class="font-bold text-amber-300 text-[11px]" id="target-title">ALVO DETECTADO</div>
                        <div class="text-[10px]" id="target-coords">LAT: 0° N | LON: 0° W</div>
                    </div>
                </div>

                <!-- Quick Legend Below Radar -->
                <div class="mt-2 grid grid-cols-4 gap-2 text-[10px] text-center font-mono border-t border-cyan-500/20 pt-2">
                    <div class="flex items-center justify-center gap-1">
                        <span class="w-2.5 h-2.5 rounded-full bg-red-500 inline-block"></span>
                        <span class="text-gray-300">Ciclone/Furacão</span>
                    </div>
                    <div class="flex items-center justify-center gap-1">
                        <span class="w-2.5 h-2.5 rounded-full bg-amber-500 inline-block"></span>
                        <span class="text-gray-300">Zona de Seca</span>
                    </div>
                    <div class="flex items-center justify-center gap-1">
                        <span class="w-2.5 h-2.5 rounded-full bg-emerald-400 inline-block"></span>
                        <span class="text-gray-300">Rede DAC Active</span>
                    </div>
                    <div class="flex items-center justify-center gap-1">
                        <span class="w-2.5 h-2.5 rounded-full bg-cyan-400 inline-block"></span>
                        <span class="text-gray-300">Nuvem Semeada</span>
                    </div>
                </div>
            </div>
        </section>

        <!-- RIGHT SIDEBAR: Telemetry Metrics & Event Logs -->
        <aside class="lg:col-span-3 flex flex-col gap-3">
            <!-- Telemetry Metrics Gauges -->
            <div class="hud-panel p-4 rounded-lg">
                <h2 class="hud-font text-sm font-bold text-cyan-300 border-b border-cyan-500/30 pb-2 mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-chart-line text-cyan-400"></i> TELEMETRIA PLANETÁRIA
                </h2>

                <div class="grid grid-cols-2 gap-2 mb-3">
                    <!-- Thermal Anomaly -->
                    <div class="bg-cyan-950/40 p-2.5 rounded border border-cyan-500/30">
                        <span class="text-[10px] text-gray-400 font-mono block">ANOMALIA TÉRMICA</span>
                        <div class="flex items-baseline gap-1">
                            <span class="hud-font text-xl font-black text-amber-400" id="metric-temp">+2.45</span>
                            <span class="text-xs text-amber-300">°C</span>
                        </div>
                        <div class="w-full bg-gray-800 h-1.5 rounded mt-1 overflow-hidden">
                            <div id="bar-temp" class="bg-amber-400 h-full w-[60%] transition-all duration-500"></div>
                        </div>
                    </div>

                    <!-- Atmospheric CO2 -->
                    <div class="bg-cyan-950/40 p-2.5 rounded border border-cyan-500/30">
                        <span class="text-[10px] text-gray-400 font-mono block">CO2 ATMOSFÉRICO</span>
                        <div class="flex items-baseline gap-1">
                            <span class="hud-font text-xl font-black text-emerald-400" id="metric-co2">432</span>
                            <span class="text-xs text-emerald-300">PPM</span>
                        </div>
                        <div class="w-full bg-gray-800 h-1.5 rounded mt-1 overflow-hidden">
                            <div id="bar-co2" class="bg-emerald-400 h-full w-[70%] transition-all duration-500"></div>
                        </div>
                    </div>

                    <!-- Disasters Prevented -->
                    <div class="bg-cyan-950/40 p-2.5 rounded border border-cyan-500/30">
                        <span class="text-[10px] text-gray-400 font-mono block">DESASTRES DISSIPADOS</span>
                        <div class="flex items-baseline gap-1">
                            <span class="hud-font text-xl font-black text-cyan-300" id="metric-disasters">14</span>
                            <span class="text-[10px] text-cyan-400">eventos</span>
                        </div>
                    </div>

                    <!-- Agricultural Efficiency -->
                    <div class="bg-cyan-950/40 p-2.5 rounded border border-cyan-500/30">
                        <span class="text-[10px] text-gray-400 font-mono block">EFICIÊNCIA AGRO</span>
                        <div class="flex items-baseline gap-1">
                            <span class="hud-font text-xl font-black text-blue-400" id="metric-agri">88.5</span>
                            <span class="text-xs text-blue-300">%</span>
                        </div>
                    </div>
                </div>

                <!-- Clean Power Consumption -->
                <div class="bg-cyan-950/40 p-2.5 rounded border border-cyan-500/30">
                    <div class="flex justify-between text-[10px] font-mono text-gray-400 mb-1">
                        <span>CONSUMO DE ENERGIA LIMPA</span>
                        <span class="text-cyan-300 font-bold" id="metric-power">1.42 TW</span>
                    </div>
                    <div class="w-full bg-gray-800 h-2 rounded overflow-hidden">
                        <div id="bar-power" class="bg-gradient-to-r from-blue-500 to-cyan-300 h-full w-[45%] transition-all duration-300"></div>
                    </div>
                </div>
            </div>

            <!-- Historical Mini-Chart Canvas -->
            <div class="hud-panel p-3 rounded-lg flex-1 flex flex-col">
                <h3 class="hud-font text-xs font-bold text-cyan-300 mb-2 flex items-center justify-between">
                    <span><i class="fa-solid fa-wave-square text-cyan-400"></i> EVOLUÇÃO TÉRMICA X CO2</span>
                    <span class="text-[9px] font-mono text-gray-400">HISTÓRICO 60s</span>
                </h3>
                <div class="flex-1 w-full min-h-[100px] bg-black/60 rounded border border-cyan-500/20 relative">
                    <canvas id="chartCanvas" class="w-full h-full block"></canvas>
                </div>
            </div>

            <!-- Real-time Tactical Event Log Terminal -->
            <div class="hud-panel p-3 rounded-lg h-44 flex flex-col">
                <h3 class="hud-font text-xs font-bold text-cyan-300 mb-2 flex items-center gap-1.5">
                    <i class="fa-solid fa-terminal text-emerald-400"></i> TERMINAL DE EVENTOS CLIMÁTICOS
                </h3>
                <div id="event-log" class="flex-1 font-mono text-[10px] overflow-y-auto space-y-1.5 pr-1 text-cyan-200">
                    <!-- Dynamic logs added here -->
                </div>
            </div>
        </aside>
    </main>

    <!-- INFO & TECHNICAL MANUAL MODAL -->
    <div id="info-modal" class="fixed inset-0 bg-black/80 backdrop-blur-md z-50 hidden flex items-center justify-center p-4">
        <div class="hud-panel max-w-2xl w-full p-6 rounded-xl border-cyan-400 max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-cyan-500/40 pb-3 mb-4">
                <h3 class="hud-font text-lg font-bold text-cyan-300 flex items-center gap-2">
                    <i class="fa-solid fa-book-bookmark text-cyan-400"></i> MANUAL DE GEOENGENHARIA E CONTROLE CLIMÁTICO
                </h3>
                <button onclick="closeInfoModal()" class="text-gray-400 hover:text-white text-xl">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <div class="space-y-4 text-xs leading-relaxed text-gray-300">
                <div class="bg-cyan-950/40 p-3 rounded border border-cyan-500/30">
                    <h4 class="font-bold text-cyan-300 text-sm mb-1"><i class="fa-solid fa-bolt text-amber-400"></i> 1. Feixes de Ionização de Alta Frequência</h4>
                    <p>Disparos Orbitais/Terrestres de feixes de micro-ondas ionizantes que aquecem ou alteram a pressão local das camadas troposféricas superiores, desestruturando a circulação ciclônica e dissolvendo furacões e tempestades severas em tempo real.</p>
                </div>

                <div class="bg-cyan-950/40 p-3 rounded border border-cyan-500/30">
                    <h4 class="font-bold text-cyan-300 text-sm mb-1"><i class="fa-solid fa-sun text-yellow-400"></i> 2. Injeção Estratosférica de Aerossóis (SRM)</h4>
                    <p>Dispersão de partículas refletoras (dióxido de enxofre ou nanopartículas de albedo elevado) na estratosfera. Aumenta a reflexão de radiação solar para o espaço, reduzindo a Anomalia Térmica global rapidamente.</p>
                </div>

                <div class="bg-cyan-950/40 p-3 rounded border border-cyan-500/30">
                    <h4 class="font-bold text-cyan-300 text-sm mb-1"><i class="fa-solid fa-cloud-showers-heavy text-blue-400"></i> 3. Semeadura Agrícola de Nuvens</h4>
                    <p>Dispersão de núcleos de condensação (Iodeto de Prata / Sais higroscópicos) sobre regiões atingidas por seca extrema para forçar precipitação, restaurando a umidade do solo e otimizando a produtividade de lavouras globais.</p>
                </div>

                <div class="bg-cyan-950/40 p-3 rounded border border-cyan-500/30">
                    <h4 class="font-bold text-cyan-300 text-sm mb-1"><i class="fa-solid fa-filter text-emerald-400"></i> 4. Captura Direta de Carbono (DAC Grid)</h4>
                    <p>Rede global de reatores gigantescos que aspiram o ar atmosférico, sequestram as moléculas de CO2 e as mineralizam no subsolo de forma permanente, reduzindo gradualmente a concentração de gases do efeito estufa (PPM).</p>
                </div>
            </div>

            <div class="mt-6 pt-4 border-t border-cyan-500/30 flex justify-end">
                <button onclick="closeInfoModal()" class="px-5 py-2 bg-cyan-500/20 hover:bg-cyan-500/40 border border-cyan-400 rounded text-cyan-200 font-bold text-xs">
                    ENTENDIDO // RETORNAR AO COMANDO
                </button>
            </div>
        </div>
    </div>

    <script>
        /* Web Audio API Sound Synthesizer for Sci-Fi FX */
        let audioCtx = null;
        let audioMuted = false;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        function playSound(type) {
            if (audioMuted || !audioCtx) return;
            try {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                const now = audioCtx.currentTime;

                if (type === 'laser') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(880, now);
                    osc.frequency.exponentialRampToValueAtTime(110, now + 0.3);
                    gain.gain.setValueAtTime(0.3, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.3);
                    osc.start(now);
                    osc.stop(now + 0.3);
                } else if (type === 'cooling') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(200, now);
                    osc.frequency.linearRampToValueAtTime(600, now + 0.5);
                    gain.gain.setValueAtTime(0.2, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.5);
                    osc.start(now);
                    osc.stop(now + 0.5);
                } else if (type === 'dissolve') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(300, now);
                    osc.frequency.exponentialRampToValueAtTime(80, now + 0.4);
                    gain.gain.setValueAtTime(0.25, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.4);
                    osc.start(now);
                    osc.stop(now + 0.4);
                } else if (type === 'rain') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(450, now);
                    osc.frequency.linearRampToValueAtTime(350, now + 0.2);
                    gain.gain.setValueAtTime(0.15, now);
                    gain.gain.linearRampToValueAtTime(0.01, now + 0.2);
                    osc.start(now);
                    osc.stop(now + 0.2);
                }
            } catch (e) { console.error(e); }
        }

        function toggleAudio() {
            audioMuted = !audioMuted;
            const icon = document.getElementById('audio-icon');
            const label = document.getElementById('audio-label');
            if (audioMuted) {
                icon.className = 'fa-solid fa-volume-xmark text-red-400';
                label.innerText = 'SOM OFF';
            } else {
                icon.className = 'fa-solid fa-volume-high text-cyan-400';
                label.innerText = 'SOM ON';
                initAudio();
            }
        }

        // Modal Controls
        function openInfoModal() {
            document.getElementById('info-modal').classList.remove('hidden');
        }
        function closeInfoModal() {
            document.getElementById('info-modal').classList.add('hidden');
        }

        // Simulation State Data
        const simState = {
            thermalAnomaly: 2.45,   // °C above baseline
            co2Ppm: 432.0,           // PPM
            disastersDissipated: 14,
            agriEfficiency: 88.5,   // %
            cleanPowerTW: 1.42,     // Terawatts
            
            // Slider Values
            srmLevel: 15,           // %
            cloudSeedingLevel: 40,  // %
            dacPowerLevel: 60,      // %

            interventionMode: 'laser', // 'laser' or 'seed'
            
            // Visibility layers
            layers: {
                hurricanes: true,
                droughts: true,
                dac: true
            },

            // Historical Data Buffer for Mini Chart
            history: []
        };

        // Geographic & Entity Collections for Canvas
        let hurricanes = [];
        let droughts = [];
        let dacTowers = [];
        let activeBeams = [];
        let rainParticles = [];
        let cloudParticles = [];

        // Simplified World Continents Geo Lines for Holographic Canvas Render
        const worldContinents = [
            // North America
            [{x:0.15, y:0.25}, {x:0.25, y:0.20}, {x:0.32, y:0.35}, {x:0.25, y:0.50}, {x:0.18, y:0.45}, {x:0.12, y:0.35}],
            // South America
            [{x:0.28, y:0.55}, {x:0.35, y:0.58}, {x:0.38, y:0.75}, {x:0.32, y:0.88}, {x:0.27, y:0.68}],
            // Europe
            [{x:0.48, y:0.22}, {x:0.58, y:0.20}, {x:0.56, y:0.35}, {x:0.48, y:0.32}],
            // Africa
            [{x:0.46, y:0.38}, {x:0.58, y:0.40}, {x:0.60, y:0.65}, {x:0.52, y:0.75}, {x:0.44, y:0.55}],
            // Asia
            [{x:0.60, y:0.20}, {x:0.85, y:0.22}, {x:0.88, y:0.48}, {x:0.75, y:0.52}, {x:0.65, y:0.40}],
            // Australia
            [{x:0.78, y:0.68}, {x:0.88, y:0.66}, {x:0.86, y:0.82}, {x:0.76, y:0.80}]
        ];

        function initSimulationEntities() {
            // Generate Initial DAC Towers
            dacTowers = [
                {x: 0.22, y: 0.38, name: "DAC-Alpha (EUA)"},
                {x: 0.52, y: 0.28, name: "DAC-Beta (Europa)"},
                {x: 0.32, y: 0.65, name: "DAC-Gamma (Brasil)"},
                {x: 0.72, y: 0.35, name: "DAC-Delta (Ásia)"},
                {x: 0.50, y: 0.60, name: "DAC-Epsilon (África)"}
            ];

            // Generate Dynamic Hurricanes
            spawnHurricane(0.20, 0.40, "Furacão Aegis (Cat 4)");
            spawnHurricane(0.78, 0.45, "Tufão Raijin (Cat 5)");
            spawnHurricane(0.35, 0.30, "Ciclone Tiamat (Cat 3)");

            // Generate Drought Zones
            droughts = [
                {x: 0.52, y: 0.48, radius: 35, severity: 0.8, name: "Seca do Saara Expandida"},
                {x: 0.80, y: 0.72, radius: 28, severity: 0.7, name: "Seca Bacia Outback"},
                {x: 0.28, y: 0.32, radius: 25, severity: 0.6, name: "Seca do Sudoeste Americano"}
            ];

            // Atmospheric Cloud background particles
            for (let i = 0; i < 60; i++) {
                cloudParticles.push({
                    x: Math.random(),
                    y: Math.random(),
                    size: Math.random() * 20 + 10,
                    speed: Math.random() * 0.0003 + 0.0001,
                    alpha: Math.random() * 0.15 + 0.05
                });
            }
        }

        function spawnHurricane(x, y, name) {
            hurricanes.push({
                x: x || Math.random() * 0.8 + 0.1,
                y: y || Math.random() * 0.6 + 0.2,
                radius: Math.random() * 20 + 25,
                intensity: Math.random() * 40 + 60, // Health/Intensity
                maxIntensity: 100,
                angle: 0,
                rotationSpeed: Math.random() * 0.05 + 0.03,
                name: name || `Ciclone Neo-${Math.floor(Math.random()*900 + 100)}`
            });
        }

        const canvas = document.getElementById('radarCanvas');
        const ctx = canvas.getContext('2d');
        const targetInfo = document.getElementById('target-info');

        function resizeCanvas() {
            const container = document.getElementById('canvas-container');
            canvas.width = container.clientWidth;
            canvas.height = container.clientHeight;
        }
        window.addEventListener('resize', resizeCanvas);

        // Interaction Mode Switching
        function setInterventionMode(mode) {
            simState.interventionMode = mode;
            const laserBtn = document.getElementById('mode-laser');
            const seedBtn = document.getElementById('mode-seed');
            const modeLabel = document.getElementById('active-mode-label');

            if (mode === 'laser') {
                laserBtn.className = "px-2 py-2 bg-cyan-500/20 border border-cyan-400 rounded text-xs text-cyan-200 font-bold flex flex-col items-center gap-1 transition";
                seedBtn.className = "px-2 py-2 bg-cyan-950/50 border border-cyan-500/30 rounded text-xs text-gray-400 flex flex-col items-center gap-1 hover:bg-cyan-500/20 transition";
                modeLabel.innerText = "Feixe Ionizante";
                modeLabel.className = "text-amber-300 font-bold";
            } else {
                seedBtn.className = "px-2 py-2 bg-blue-500/20 border border-blue-400 rounded text-xs text-blue-200 font-bold flex flex-col items-center gap-1 transition";
                laserBtn.className = "px-2 py-2 bg-cyan-950/50 border border-cyan-500/30 rounded text-xs text-gray-400 flex flex-col items-center gap-1 hover:bg-cyan-500/20 transition";
                modeLabel.innerText = "Semeadura de Chuva";
                modeLabel.className = "text-blue-300 font-bold";
            }
        }

        // Layer Toggles
        function toggleLayer(layerName) {
            simState.layers[layerName] = !simState.layers[layerName];
            const btnMap = {
                hurricanes: 'btn-layer-storms',
                droughts: 'btn-layer-droughts',
                dac: 'btn-layer-dac'
            };
            const btn = document.getElementById(btnMap[layerName]);
            if (simState.layers[layerName]) {
                btn.className = "px-2 py-0.5 bg-cyan-500/30 border border-cyan-400 text-cyan-200 rounded";
            } else {
                btn.className = "px-2 py-0.5 bg-gray-800 border border-gray-600 text-gray-400 rounded";
            }
        }

        // Radar Canvas Mouse Hover & Click Logic
        canvas.addEventListener('mousemove', (e) => {
            initAudio();
            const rect = canvas.getBoundingClientRect();
            const mouseX = e.clientX - rect.left;
            const mouseY = e.clientY - rect.top;

            const normX = mouseX / canvas.width;
            const normY = mouseY / canvas.height;

            // Check hover near hurricanes
            let hoveredEntity = null;
            if (simState.layers.hurricanes) {
                hurricanes.forEach(h => {
                    const hPx = h.x * canvas.width;
                    const hPy = h.y * canvas.height;
                    const dist = Math.hypot(mouseX - hPx, mouseY - hPy);
                    if (dist < h.radius + 10) {
                        hoveredEntity = { title: h.name, type: 'CRÍTICO: Ciclone Atmosférico' };
                    }
                });
            }

            if (simState.layers.droughts && !hoveredEntity) {
                droughts.forEach(d => {
                    const dPx = d.x * canvas.width;
                    const dPy = d.y * canvas.height;
                    const dist = Math.hypot(mouseX - dPx, mouseY - dPy);
                    if (dist < d.radius) {
                        hoveredEntity = { title: d.name, type: 'ALERTA: Estresse Hídrico Agro' };
                    }
                });
            }

            if (hoveredEntity) {
                targetInfo.classList.remove('hidden');
                targetInfo.style.left = `${mouseX + 15}px`;
                targetInfo.style.top = `${mouseY - 15}px`;
                document.getElementById('target-title').innerText = hoveredEntity.title;
                document.getElementById('target-coords').innerText = hoveredEntity.type;
            } else {
                targetInfo.classList.add('hidden');
            }
        });

        canvas.addEventListener('click', (e) => {
            const rect = canvas.getBoundingClientRect();
            const clickX = e.clientX - rect.left;
            const clickY = e.clientY - rect.top;
            const normX = clickX / canvas.width;
            const normY = clickY / canvas.height;

            if (simState.interventionMode === 'laser') {
                // Fire Ionization Beam
                activeBeams.push({
                    x: clickX,
                    y: clickY,
                    alpha: 1.0,
                    radius: 40
                });
                playSound('laser');

                // Check collision with hurricanes
                hurricanes.forEach((h, index) => {
                    const hPx = h.x * canvas.width;
                    const hPy = h.y * canvas.height;
                    const dist = Math.hypot(clickX - hPx, clickY - hPy);
                    if (dist < h.radius + 30) {
                        h.intensity -= 35;
                        playSound('dissolve');
                        addEventLog(`[ION-BEAM] Disparo Ionizante atingiu ${h.name}. Intensidade -35%.`, 'amber');
                        if (h.intensity <= 0) {
                            addEventLog(`[SUCESSO] ${h.name} DISSIPADO COMPLETAMENTE!`, 'emerald');
                            simState.disastersDissipated++;
                            hurricanes.splice(index, 1);
                        }
                    }
                });
            } else if (simState.interventionMode === 'seed') {
                // Trigger Cloud Seeding locally
                playSound('rain');
                for (let i = 0; i < 30; i++) {
                    rainParticles.push({
                        x: clickX + (Math.random() - 0.5) * 60,
                        y: clickY + (Math.random() - 0.5) * 60,
                        vy: Math.random() * 3 + 2,
                        alpha: 1.0
                    });
                }
                
                // Relieve drought if clicked nearby
                droughts.forEach((d, idx) => {
                    const dPx = d.x * canvas.width;
                    const dPy = d.y * canvas.height;
                    const dist = Math.hypot(clickX - dPx, clickY - dPy);
                    if (dist < d.radius + 30) {
                        d.severity -= 0.3;
                        addEventLog(`[SEMEADURA] Chuva induzida sobre ${d.name}. Umidade solo +30%`, 'cyan');
                        if (d.severity <= 0.1) {
                            addEventLog(`[RECUPERADO] ${d.name} neutralizada por irrigação atmosférica.`, 'emerald');
                            droughts.splice(idx, 1);
                        }
                    }
                });
            }
        });

        function triggerPulseCooling() {
            playSound('cooling');
            simState.thermalAnomaly = Math.max(0, simState.thermalAnomaly - 0.25);
            addEventLog("[SRM OVERDRIVE] Carga estática de aerossóis liberada! Anomalia Térmica -0.25°C.", 'cyan');
        }

        function triggerGlobalStormNeutralize() {
            if (hurricanes.length === 0) {
                addEventLog("[INFO] Nenhuma tempestade severa detectada em órbita.", 'cyan');
                return;
            }
            playSound('laser');
            let count = hurricanes.length;
            simState.disastersDissipated += count;
            hurricanes = [];
            addEventLog(`[DEFESA GLOBAL] Rede Ionizante Orbitária descarregada! ${count} ciclones dissipados.`, 'emerald');
        }

        function addEventLog(msg, color = 'cyan') {
            const logContainer = document.getElementById('event-log');
            const entry = document.createElement('div');
            const now = new Date();
            const timeStr = now.toTimeString().split(' ')[0];
            
            let colorClass = 'text-cyan-300';
            if (color === 'emerald') colorClass = 'text-emerald-400 font-bold';
            if (color === 'amber') colorClass = 'text-amber-400';
            if (color === 'red') colorClass = 'text-red-400 font-bold';

            entry.className = colorClass;
            entry.innerHTML = `<span class="text-gray-500">[${timeStr}]</span> ${msg}`;
            logContainer.prepend(entry);

            // Limit log history to 50 items
            if (logContainer.children.length > 50) {
                logContainer.removeChild(logContainer.lastChild);
            }
        }

        // Slider Event Listeners
        document.getElementById('srm-slider').addEventListener('input', (e) => {
            simState.srmLevel = parseInt(e.target.value);
            document.getElementById('srm-val').innerText = `${simState.srmLevel}%`;
        });

        document.getElementById('seeding-slider').addEventListener('input', (e) => {
            simState.cloudSeedingLevel = parseInt(e.target.value);
            document.getElementById('seeding-val').innerText = `${simState.cloudSeedingLevel}%`;
        });

        document.getElementById('dac-slider').addEventListener('input', (e) => {
            simState.dacPowerLevel = parseInt(e.target.value);
            document.getElementById('dac-val').innerText = `${simState.dacPowerLevel}%`;
        });

        let radarAngle = 0;

        function renderRadar() {
            if (!canvas.width) return;
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // 1. Draw Background Grid Lines
            ctx.strokeStyle = 'rgba(0, 240, 255, 0.08)';
            ctx.lineWidth = 1;
            const gridSize = 40;
            for (let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, canvas.height);
                ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(canvas.width, y);
                ctx.stroke();
            }

            // 2. Draw World Map Holographic Outlines
            ctx.strokeStyle = 'rgba(0, 240, 255, 0.35)';
            ctx.lineWidth = 1.5;
            ctx.fillStyle = 'rgba(0, 240, 255, 0.03)';

            worldContinents.forEach(polygon => {
                ctx.beginPath();
                polygon.forEach((pt, i) => {
                    const px = pt.x * canvas.width;
                    const py = pt.y * canvas.height;
                    if (i === 0) ctx.moveTo(px, py);
                    else ctx.lineTo(px, py);
                });
                ctx.closePath();
                ctx.stroke();
                ctx.fill();
            });

            // 3. Render Atmospheric Clouds
            cloudParticles.forEach(c => {
                c.x += c.speed;
                if (c.x > 1) c.x = -0.1;
                ctx.fillStyle = `rgba(160, 240, 255, ${c.alpha})`;
                ctx.beginPath();
                ctx.arc(c.x * canvas.width, c.y * canvas.height, c.size, 0, Math.PI * 2);
                ctx.fill();
            });

            // 4. Render Stratospheric Aerosol Injection (SRM Layer Shimmer)
            if (simState.srmLevel > 0) {
                const srmOpacity = (simState.srmLevel / 100) * 0.12;
                ctx.fillStyle = `rgba(220, 245, 255, ${srmOpacity})`;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
            }

            // 5. Render Drought Zones
            if (simState.layers.droughts) {
                droughts.forEach(d => {
                    const px = d.x * canvas.width;
                    const py = d.y * canvas.height;
                    
                    const grad = ctx.createRadialGradient(px, py, 5, px, py, d.radius);
                    grad.addColorStop(0, `rgba(255, 170, 0, ${d.severity * 0.5})`);
                    grad.addColorStop(1, 'rgba(255, 170, 0, 0)');

                    ctx.fillStyle = grad;
                    ctx.beginPath();
                    ctx.arc(px, py, d.radius, 0, Math.PI * 2);
                    ctx.fill();

                    ctx.strokeStyle = `rgba(255, 170, 0, ${d.severity})`;
                    ctx.lineWidth = 1;
                    ctx.setLineDash([4, 4]);
                    ctx.stroke();
                    ctx.setLineDash([]);
                });
            }

            // 6. Render DAC Towers
            if (simState.layers.dac) {
                dacTowers.forEach(dac => {
                    const px = dac.x * canvas.width;
                    const py = dac.y * canvas.height;

                    ctx.fillStyle = '#00ffcc';
                    ctx.beginPath();
                    ctx.arc(px, py, 4, 0, Math.PI * 2);
                    ctx.fill();

                    // Suction ring pulse
                    ctx.strokeStyle = 'rgba(0, 255, 204, 0.4)';
                    ctx.lineWidth = 1;
                    ctx.beginPath();
                    ctx.arc(px, py, 8 + Math.sin(Date.now() * 0.005) * 4, 0, Math.PI * 2);
                    ctx.stroke();
                });
            }

            // 7. Render Hurricanes / Cyclones
            if (simState.layers.hurricanes) {
                hurricanes.forEach(h => {
                    const px = h.x * canvas.width;
                    const py = h.y * canvas.height;
                    h.angle += h.rotationSpeed;

                    // Vortex swirl effect
                    ctx.save();
                    ctx.translate(px, py);
                    ctx.rotate(h.angle);

                    ctx.strokeStyle = `rgba(255, 51, 102, ${h.intensity / 100})`;
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    for (let i = 0; i < 3; i++) {
                        ctx.arc(0, 0, (h.radius / 3) * (i + 1), i * 1.5, i * 1.5 + Math.PI);
                    }
                    ctx.stroke();

                    // Eye of the hurricane
                    ctx.fillStyle = '#ff3366';
                    ctx.beginPath();
                    ctx.arc(0, 0, 4, 0, Math.PI * 2);
                    ctx.fill();

                    ctx.restore();

                    // Name Tag
                    ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
                    ctx.font = '9px Orbitron';
                    ctx.fillText(`${h.name} (${Math.round(h.intensity)}%)`, px + 12, py + 4);
                });
            }

            // 8. Render Fired Ionization Beams Effect
            activeBeams.forEach((beam, index) => {
                ctx.strokeStyle = `rgba(255, 170, 0, ${beam.alpha})`;
                ctx.fillStyle = `rgba(0, 240, 255, ${beam.alpha * 0.3})`;
                ctx.lineWidth = 3;

                ctx.beginPath();
                ctx.arc(beam.x, beam.y, beam.radius, 0, Math.PI * 2);
                ctx.stroke();
                ctx.fill();

                // Vertical Beam line from orbital top
                ctx.strokeStyle = `rgba(0, 240, 255, ${beam.alpha})`;
                ctx.beginPath();
                ctx.moveTo(beam.x, 0);
                ctx.lineTo(beam.x, beam.y);
                ctx.stroke();

                beam.alpha -= 0.04;
                beam.radius += 2;
                if (beam.alpha <= 0) activeBeams.splice(index, 1);
            });

            // 9. Render Rain / Cloud Seeding Particles
            rainParticles.forEach((r, idx) => {
                ctx.strokeStyle = `rgba(0, 200, 255, ${r.alpha})`;
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.moveTo(r.x, r.y);
                ctx.lineTo(r.x, r.y + 6);
                ctx.stroke();

                r.y += r.vy;
                r.alpha -= 0.02;
                if (r.alpha <= 0) rainParticles.splice(idx, 1);
            });

            // 10. Radar Sweeping Line Effect
            radarAngle += 0.015;
            const sweepX = canvas.width / 2 + Math.cos(radarAngle) * canvas.width;
            const sweepY = canvas.height / 2 + Math.sin(radarAngle) * canvas.width;

            const sweepGrad = ctx.createRadialGradient(
                canvas.width / 2, canvas.height / 2, 10,
                canvas.width / 2, canvas.height / 2, canvas.width / 1.5
            );
            sweepGrad.addColorStop(0, 'rgba(0, 240, 255, 0.05)');
            sweepGrad.addColorStop(1, 'rgba(0, 240, 255, 0.0)');

            ctx.fillStyle = sweepGrad;
            ctx.beginPath();
            ctx.moveTo(canvas.width / 2, canvas.height / 2);
            ctx.arc(canvas.width / 2, canvas.height / 2, canvas.width, radarAngle - 0.2, radarAngle);
            ctx.closePath();
            ctx.fill();
        }

        const chartCanvas = document.getElementById('chartCanvas');
        const chartCtx = chartCanvas.getContext('2d');

        function renderChart() {
            if (!chartCanvas.width) {
                const container = chartCanvas.parentElement;
                chartCanvas.width = container.clientWidth;
                chartCanvas.height = container.clientHeight;
            }

            chartCtx.clearRect(0, 0, chartCanvas.width, chartCanvas.height);
            const w = chartCanvas.width;
            const h = chartCanvas.height;

            if (simState.history.length < 2) return;

            // Draw Thermal Anomaly Line (Red/Amber)
            chartCtx.strokeStyle = '#ffaa00';
            chartCtx.lineWidth = 2;
            chartCtx.beginPath();

            const stepX = w / (60 - 1);
            simState.history.forEach((pt, i) => {
                const x = i * stepX;
                // Normalize temp between 0°C and 4°C to canvas height
                const normTemp = 1 - (pt.temp / 4.0);
                const y = normTemp * (h - 10) + 5;
                if (i === 0) chartCtx.moveTo(x, y);
                else chartCtx.lineTo(x, y);
            });
            chartCtx.stroke();

            // Draw CO2 Line (Green)
            chartCtx.strokeStyle = '#00ffcc';
            chartCtx.lineWidth = 1.5;
            chartCtx.beginPath();

            simState.history.forEach((pt, i) => {
                const x = i * stepX;
                // Normalize CO2 between 350 and 500 PPM
                const normCo2 = 1 - ((pt.co2 - 350) / 150);
                const y = normCo2 * (h - 10) + 5;
                if (i === 0) chartCtx.moveTo(x, y);
                else chartCtx.lineTo(x, y);
            });
            chartCtx.stroke();
        }

        function updateSimulationLogic() {
            // 1. SRM Effect on Thermal Anomaly
            const srmCoolingRate = (simState.srmLevel / 100) * 0.003;
            // CO2 heating effect
            const co2HeatingRate = (simState.co2Ppm / 400) * 0.001;
            simState.thermalAnomaly = Math.max(0.1, simState.thermalAnomaly - srmCoolingRate + co2HeatingRate + (Math.random() - 0.5) * 0.001);

            // 2. DAC Power Effect on CO2 PPM
            const dacReduction = (simState.dacPowerLevel / 100) * 0.04;
            simState.co2Ppm = Math.max(280, simState.co2Ppm - dacReduction + 0.01);

            // 3. Cloud Seeding effect on Agricultural Efficiency
            const seedingBonus = (simState.cloudSeedingLevel / 100) * 15;
            const tempPenalty = simState.thermalAnomaly * 8;
            simState.agriEfficiency = Math.min(100, Math.max(10, 95 + seedingBonus - tempPenalty));

            // 4. Power Consumption Calculation (TW)
            const powerSrm = (simState.srmLevel / 100) * 0.3;
            const powerDac = (simState.dacPowerLevel / 100) * 0.9;
            const powerSeeding = (simState.cloudSeedingLevel / 100) * 0.2;
            simState.cleanPowerTW = (0.2 + powerSrm + powerDac + powerSeeding).toFixed(2);

            // 5. Random Hurricane Spawning based on high thermal anomaly
            if (Math.random() < 0.008 * simState.thermalAnomaly && hurricanes.length < 5) {
                spawnHurricane();
                addEventLog("[ALERTA] Novo ciclone formado devido a superaquecimento oceânico!", 'red');
            }

            // Update UI Telemetry Displays
            document.getElementById('metric-temp').innerText = (simState.thermalAnomaly >= 0 ? '+' : '') + simState.thermalAnomaly.toFixed(2);
            document.getElementById('bar-temp').style.width = `${Math.min(100, (simState.thermalAnomaly / 4) * 100)}%`;

            document.getElementById('metric-co2').innerText = Math.round(simState.co2Ppm);
            document.getElementById('bar-co2').style.width = `${Math.min(100, ((simState.co2Ppm - 300) / 200) * 100)}%`;

            document.getElementById('metric-disasters').innerText = simState.disastersDissipated;
            document.getElementById('metric-agri').innerText = simState.agriEfficiency.toFixed(1);
            document.getElementById('metric-power').innerText = `${simState.cleanPowerTW} TW`;

            // Record History Buffer
            simState.history.push({
                temp: simState.thermalAnomaly,
                co2: simState.co2Ppm
            });
            if (simState.history.length > 60) simState.history.shift();

            // Update Simulated Clock
            const now = new Date();
            document.getElementById('sim-clock').innerText = `2088-09-27 ${now.toTimeString().split(' ')[0]} UTC`;
        }

        window.onload = function() {
            resizeCanvas();
            initSimulationEntities();

            // Initial Event Logs
            addEventLog("[SISTEMA] Centro de Comando GEO-CORE 9000 Inicializado.", 'emerald');
            addEventLog("[INFO] Sensores orbitais e rede de captura DAC em sincronia.", 'cyan');
            addEventLog("[ALERTA] 3 anomalias ciclônicas ativas detectadas.", 'amber');

            // Main Animation & Physics Loop
            function mainLoop() {
                renderRadar();
                renderChart();
                requestAnimationFrame(mainLoop);
            }
            mainLoop();

            // Simulation Logic Interval (1 sec)
            setInterval(updateSimulationLogic, 1000);
        };
    </script>
</body>
</html>
