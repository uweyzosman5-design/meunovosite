<!DOCTYPE html>
<html lang="pt" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pulso — O teu ginásio, a bater certo</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        pulso: {
                            50: '#fff1f2',
                            100: '#ffe4e6',
                            500: '#f43f5e',
                            600: '#e11d48',
                            700: '#be123c',
                            900: '#881337',
                            dark: '#0f172a',
                            card: '#1e293b',
                            accent: '#10b981'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
    <!-- Three.js CDN para Renderização 3D e WebGL -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        body {
            font-family: 'Plus Jakarta Sans', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: #0b0f19;
            color: #f8fafc;
            letter-spacing: -0.01em;
            position: relative;
            overflow-x: hidden;
        }
        /* 3D Canvas Background fixed to fill screen behind all content */
        #webgl-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            z-index: 0;
            pointer-events: none;
        }
        .content-layer {
            position: relative;
            z-index: 10;
        }
        .font-mono {
            font-family: 'JetBrains Mono', monospace;
        }
        .glass-panel {
            background: rgba(15, 23, 42, 0.70);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
        }
        /* Clean Professional Focus & Hover State */
        .btn-primary {
            background-color: #e11d48;
            color: #ffffff;
            font-weight: 600;
            border-radius: 0.5rem;
            transition: all 0.15s ease-in-out;
            box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
        }
        .btn-primary:hover {
            background-color: #be123c;
        }
        .btn-secondary {
            background-color: #1e293b;
            color: #f1f5f9;
            font-weight: 600;
            border: 1px solid #334155;
            border-radius: 0.5rem;
            transition: all 0.15s ease-in-out;
        }
        .btn-secondary:hover {
            background-color: #334155;
            border-color: #475569;
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f19;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569;
        }
    </style>
</head>
<body class="antialiased selection:bg-pulso-600 selection:text-white">

    <!-- 3D INTERACTIVE CANVAS BACKGROUND -->
    <canvas id="webgl-bg"></canvas>

    <div class="content-layer">
    <!-- TOP ANNOUNCEMENT BAR -->
    <div class="bg-slate-900/90 backdrop-blur border-b border-slate-800 text-slate-300 text-xs py-2 px-4 text-center font-medium">
        <span class="bg-pulso-600/20 text-pulso-400 border border-pulso-500/30 px-2 py-0.5 rounded text-[11px] mr-2 font-semibold">Licenciamento</span>
        Plataforma em Meticais (MZN) com 7 dias de demonstração sem compromisso.
    </div>

    <!-- NAVBAR -->
    <nav class="sticky top-0 z-50 glass-panel border-b border-slate-800/80 px-4 sm:px-8 py-3.5">
        <div class="max-w-7xl mx-auto flex items-center justify-between">
            <!-- BRAND LOGO -->
            <a href="#" class="flex items-center gap-2 text-xl font-bold tracking-tight text-white">
                <span class="w-7 h-7 rounded bg-pulso-600 flex items-center justify-center text-white font-black text-base">S</span>
                <span>Pulso<span class="text-pulso-500">.fitness</span></span>
            </a>

            <!-- NAV LINKS -->
            <div class="hidden md:flex items-center gap-8 text-sm font-medium text-slate-300">
                <a href="#funcionalidades" class="hover:text-white transition-colors">Funcionalidades</a>
                <a href="#ecras" class="hover:text-white transition-colors">Três Ecrãs</a>
                <a href="#um-dia" class="hover:text-white transition-colors">Um dia no Pulso</a>
                <a href="#licencas" class="hover:text-white transition-colors">Licenças</a>
                <a href="#faq" class="hover:text-white transition-colors">Perguntas Frequentes</a>
                <a href="#contacto" class="hover:text-white transition-colors">Contacto</a>
            </div>

            <!-- CTAS -->
            <div class="flex items-center gap-3">
                <button onclick="openModal('loginModal')" class="text-sm font-semibold text-slate-300 hover:text-white px-3 py-2 transition-colors">
                    ENTRAR
                </button>
                <button onclick="openModal('signupModal')" class="btn-primary text-sm px-4 py-2 flex items-center gap-1.5">
                    Criar conta grátis
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"/></svg>
                </button>
            </div>
        </div>
    </nav>

    <!-- HERO SECTION -->
    <section class="relative pt-12 pb-20 lg:pt-20 lg:pb-32 overflow-hidden">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
            <div class="text-center max-w-3xl mx-auto mb-12">
                <div class="inline-flex items-center gap-2 px-3 py-1 rounded-md bg-slate-900 border border-slate-800 text-slate-300 text-xs font-medium mb-6">
                    <span class="w-2 h-2 rounded-full bg-emerald-500"></span>
                    Software de Gestão de Ginásios · Moçambique
                </div>
                
                <h1 class="text-4xl sm:text-6xl font-extrabold tracking-tight text-white leading-tight mb-6">
                    O teu ginásio, <br><span class="text-pulso-500">a bater certo.</span>
                </h1>
                
                <p class="text-base sm:text-lg text-slate-300 font-normal mb-8 leading-relaxed max-w-2xl mx-auto">
                    Membros, pagamentos, aulas e controlo de entradas no mesmo pulso. Sabes quem pagou, quem entrou e quanto facturaste, <strong>sem abrir o caderno.</strong>
                </p>

                <div class="flex flex-col sm:flex-row items-center justify-center gap-3 mb-8">
                    <button onclick="openModal('signupModal')" class="btn-primary w-full sm:w-auto px-6 py-3.5 text-base flex items-center justify-center gap-2">
                        Criar conta grátis
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"/></svg>
                    </button>
                    <a href="#ecras" class="btn-secondary w-full sm:w-auto px-6 py-3.5 text-base text-center">
                        Ver o sistema
                    </a>
                </div>

                <div class="flex flex-wrap items-center justify-center gap-6 text-xs text-slate-400 font-medium">
                    <span class="flex items-center gap-1.5">
                        <svg class="w-4 h-4 text-emerald-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                        Licença de 7 dias activa imediatamente
                    </span>
                    <span class="flex items-center gap-1.5">
                        <svg class="w-4 h-4 text-emerald-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                        Licenças de 1, 3, 6 ou 12 meses
                    </span>
                    <span class="flex items-center gap-1.5">
                        <svg class="w-4 h-4 text-emerald-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                        Sem taxa de instalação
                    </span>
                </div>
            </div>

            <!-- HERO DASHBOARD PREVIEW CARDS (INTERACTIVE SNAPSHOT) -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mt-8">
                <!-- Card 1: Presenças Ao Vivo -->
                <div class="glass-panel p-5 rounded-2xl border-l-4 border-l-emerald-500">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs uppercase font-bold text-slate-400 tracking-wider">Presenças ao vivo</span>
                        <span class="px-2 py-0.5 rounded-full bg-emerald-500/20 text-emerald-400 text-xs font-bold animate-pulse">EM DIRETO</span>
                    </div>
                    <div class="flex items-baseline justify-between mb-4">
                        <span class="text-3xl font-black text-white" id="heroEntryCount">99</span>
                        <span class="text-xs text-slate-400">entradas hoje</span>
                    </div>
                    <div class="space-y-2 text-xs border-t border-slate-700/60 pt-3">
                        <div class="flex justify-between items-center text-slate-300">
                            <span class="flex items-center gap-2">
                                <span class="w-6 h-6 rounded-full bg-slate-700 font-bold text-[10px] flex items-center justify-center text-emerald-400">SM</span>
                                Sónia Matsinhe
                            </span>
                            <span class="text-slate-500 font-mono">18:45</span>
                        </div>
                        <div class="flex justify-between items-center text-slate-300">
                            <span class="flex items-center gap-2">
                                <span class="w-6 h-6 rounded-full bg-slate-700 font-bold text-[10px] flex items-center justify-center text-emerald-400">NC</span>
                                Nelson Cuna
                            </span>
                            <span class="text-slate-500 font-mono">18:44</span>
                        </div>
                        <div class="flex justify-between items-center text-slate-300">
                            <span class="flex items-center gap-2">
                                <span class="w-6 h-6 rounded-full bg-slate-700 font-bold text-[10px] flex items-center justify-center text-emerald-400">IZ</span>
                                Inês Zandamela
                            </span>
                            <span class="text-slate-500 font-mono">18:43</span>
                        </div>
                    </div>
                </div>

                <!-- Card 2: Receita Outubro -->
                <div class="glass-panel p-5 rounded-2xl border-l-4 border-l-pulso-500">
                    <div class="flex items-center justify-between mb-2">
                        <span class="text-xs uppercase font-bold text-slate-400 tracking-wider">Receita Outubro</span>
                        <span class="text-xs text-emerald-400 font-semibold">+12% vs Set</span>
                    </div>
                    <div class="text-3xl font-black text-white mb-3">48.500 <span class="text-sm font-normal text-slate-400">MZN</span></div>
                    <div class="space-y-1.5 text-xs text-slate-300 border-t border-slate-700/60 pt-3">
                        <div class="flex justify-between">
                            <span class="text-slate-400">M-Pesa:</span>
                            <span class="font-semibold text-white">27.000 MZN</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-slate-400">e-Mola:</span>
                            <span class="font-semibold text-white">9.000 MZN</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-slate-400">Numerário:</span>
                            <span class="font-semibold text-white">8.500 MZN</span>
                        </div>
                    </div>
                </div>

                <!-- Card 3: O Meu Turno & Licença -->
                <div class="glass-panel p-5 rounded-2xl border-l-4 border-l-amber-500">
                    <div class="flex justify-between items-center mb-2">
                        <span class="text-xs uppercase font-bold text-slate-400 tracking-wider">O meu turno hoje</span>
                        <span class="w-2 h-2 rounded-full bg-emerald-400"></span>
                    </div>
                    <div class="flex justify-between items-baseline mb-2">
                        <span class="text-xs text-slate-400">Entrada: <strong class="text-slate-200">07:58</strong></span>
                        <span class="text-xs text-slate-400">Pausa: <strong class="text-slate-200">12:30</strong></span>
                    </div>
                    <div class="text-xl font-bold text-amber-400 mb-3">Em turno há 10h 46m</div>
                    <div class="border-t border-slate-700/60 pt-2 text-xs">
                        <div class="text-slate-400">Faturação Pulso: <span class="text-white font-medium">Licença anual</span></div>
                        <div class="text-slate-500 text-[11px]">1 Out 2026 → 30 Set 2027</div>
                        <div class="mt-1 font-semibold text-emerald-400">284 dias restantes</div>
                    </div>
                </div>

                <!-- Card 4: Alerta Porta Principal & Validação -->
                <div class="glass-panel p-5 rounded-2xl border-l-4 border-l-rose-500">
                    <div class="flex justify-between items-center mb-2">
                        <span class="text-xs uppercase font-bold text-slate-400 tracking-wider">Porta Principal</span>
                        <span class="text-xs font-mono text-slate-400">18:45</span>
                    </div>
                    <div class="bg-emerald-950/60 border border-emerald-600/40 rounded-xl p-2.5 mb-2">
                        <div class="flex items-center justify-between">
                            <span class="font-bold text-xs text-emerald-200">Ana Machava</span>
                            <span class="text-[10px] bg-emerald-500 text-slate-950 px-1.5 py-0.5 rounded font-bold">PERMITIDA</span>
                        </div>
                        <div class="text-[11px] text-emerald-400/80 mt-0.5">Mensal · até 31 Out</div>
                    </div>
                    <div class="bg-rose-950/60 border border-rose-600/40 rounded-xl p-2.5">
                        <div class="flex items-center justify-between">
                            <span class="font-bold text-xs text-rose-200">Rui Bila</span>
                            <span class="text-[10px] bg-rose-500 text-white px-1.5 py-0.5 rounded font-bold">NEGADA</span>
                        </div>
                        <div class="text-[11px] text-rose-300/80 mt-0.5">Plano acabou a 2 Out</div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- SECTION: PAGAMENTOS MZN -->
    <section class="py-12 bg-slate-900/60 border-y border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <p class="text-center text-sm font-semibold text-slate-400 uppercase tracking-widest mb-6">
                Os teus membros pagam como quiserem · Tudo em Meticais (MZN)
            </p>
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 max-w-4xl mx-auto text-center">
                <div class="glass-panel p-4 rounded-xl flex items-center justify-center gap-3">
                    <span class="w-3 h-3 rounded-full bg-red-500"></span>
                    <span class="font-bold text-slate-200">M-Pesa</span>
                </div>
                <div class="glass-panel p-4 rounded-xl flex items-center justify-center gap-3">
                    <span class="w-3 h-3 rounded-full bg-orange-500"></span>
                    <span class="font-bold text-slate-200">e-Mola</span>
                </div>
                <div class="glass-panel p-4 rounded-xl flex items-center justify-center gap-3">
                    <span class="w-3 h-3 rounded-full bg-emerald-500"></span>
                    <span class="font-bold text-slate-200">Numerário</span>
                </div>
                <div class="glass-panel p-4 rounded-xl flex items-center justify-center gap-3">
                    <span class="w-3 h-3 rounded-full bg-blue-500"></span>
                    <span class="font-bold text-slate-200">Transferência</span>
                </div>
            </div>
        </div>
    </section>

    <!-- INTERACTIVE THREE SCREENS SHOWCASE -->
    <section id="ecras" class="py-20 bg-slate-950">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center max-w-3xl mx-auto mb-12">
                <h2 class="text-xs uppercase tracking-widest font-bold text-pulso-500 mb-2">Três Ecrãs, Um Sistema</h2>
                <p class="text-3xl sm:text-4xl font-extrabold text-white">Cada pessoa vê o que precisa. Nada mais.</p>
                <p class="text-slate-400 mt-4 text-base">
                    A recepção despacha a fila. O dono acompanha o dinheiro. O membro leva o passe no bolso. Tudo no mesmo Pulso, cada um com o seu acesso.
                </p>
            </div>

            <!-- TAB CONTROLS -->
            <div class="flex justify-center mb-8">
                <div class="inline-flex p-1 rounded-lg bg-slate-900 border border-slate-800">
                    <button id="btn-tab-tablet" onclick="switchScreenTab('tablet')" class="px-4 py-2.5 rounded-md text-xs font-semibold transition-all bg-pulso-600 text-white">
                        📱 Tablet Recepção
                    </button>
                    <button id="btn-tab-desktop" onclick="switchScreenTab('desktop')" class="px-4 py-2.5 rounded-md text-xs font-semibold text-slate-400 hover:text-white transition-all">
                        💻 Escritório Dono
                    </button>
                    <button id="btn-tab-mobile" onclick="switchScreenTab('mobile')" class="px-4 py-2.5 rounded-md text-xs font-semibold text-slate-400 hover:text-white transition-all">
                        📲 Telemóvel Membro
                    </button>
                </div>
            </div>

            <!-- ECRÃ 1: RECEPÇÃO (TABLET) -->
            <div id="screen-tablet" class="screen-view transition-all">
                <div class="glass-panel p-6 sm:p-8 rounded-3xl max-w-4xl mx-auto border border-slate-700 shadow-2xl">
                    <div class="flex items-center justify-between pb-4 border-b border-slate-800 mb-6">
                        <div>
                            <div class="text-xs font-bold text-pulso-500 uppercase">A Recepção ao balcão (Tablet)</div>
                            <h3 class="text-xl font-bold text-white">Procura o membro, regista o pagamento e renova em segundos</h3>
                        </div>
                        <span class="px-3 py-1 bg-slate-800 text-slate-300 text-xs rounded-full font-mono">Turno · 18:44</span>
                    </div>

                    <!-- INTERACTIVE MEMBER PAYMENT REGISTRATION FORM -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="space-y-4">
                            <div>
                                <label class="block text-xs font-medium text-slate-400 mb-1">Nome ou telefone</label>
                                <div class="relative">
                                    <input type="text" value="Júlio Tembe" readonly class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-slate-200 font-semibold focus:outline-none">
                                    <span class="absolute right-3 top-2.5 text-xs bg-amber-500/20 text-amber-300 px-2 py-0.5 rounded">Mensal · acaba sexta, 10 Out (3 dias)</span>
                                </div>
                            </div>

                            <div>
                                <label class="block text-xs font-medium text-slate-400 mb-2">Método de Pagamento</label>
                                <div class="grid grid-cols-2 gap-2" id="paymentMethodSelector">
                                    <button type="button" onclick="selectPaymentMethod(this, 'M-Pesa')" class="pay-btn active border-2 border-pulso-500 bg-pulso-600/20 text-white font-bold p-2.5 rounded-xl text-center text-xs">M-Pesa</button>
                                    <button type="button" onclick="selectPaymentMethod(this, 'e-Mola')" class="pay-btn border border-slate-700 bg-slate-900 text-slate-300 hover:border-slate-500 p-2.5 rounded-xl text-center text-xs">e-Mola</button>
                                    <button type="button" onclick="selectPaymentMethod(this, 'Numerário')" class="pay-btn border border-slate-700 bg-slate-900 text-slate-300 hover:border-slate-500 p-2.5 rounded-xl text-center text-xs">Numerário</button>
                                    <button type="button" onclick="selectPaymentMethod(this, 'Transf.')" class="pay-btn border border-slate-700 bg-slate-900 text-slate-300 hover:border-slate-500 p-2.5 rounded-xl text-center text-xs">Transf.</button>
                                </div>
                            </div>

                            <div class="bg-slate-900 p-4 rounded-xl border border-slate-800">
                                <div class="flex justify-between text-xs text-slate-400 mb-1">
                                    <span>Renova até</span>
                                    <span class="text-white font-bold">9 Nov</span>
                                </div>
                                <div class="flex justify-between items-baseline">
                                    <span class="text-sm font-semibold text-slate-300">Valor a cobrar:</span>
                                    <span class="text-2xl font-black text-emerald-400">1.500 MZN</span>
                                </div>
                            </div>

                            <button onclick="registerPayment()" id="btnRegisterPay" class="w-full py-3 bg-emerald-600 hover:bg-emerald-500 text-white font-bold rounded-xl transition-all shadow-lg flex justify-center items-center gap-2">
                                Registar Pagamento
                            </button>
                            <div id="paySuccessMsg" class="hidden p-3 bg-emerald-950 border border-emerald-500/50 text-emerald-300 rounded-xl text-xs text-center font-bold">
                                ✓ Pago · plano renovado até 9 Nov!
                            </div>
                        </div>

                        <!-- LIVE RECEPTION QUEUE & PORTA FEED -->
                        <div class="bg-slate-900 p-5 rounded-2xl border border-slate-800 space-y-4">
                            <h4 class="text-xs font-bold uppercase text-slate-400 tracking-wider">Últimos Pagamentos no Balcão</h4>
                            <div class="space-y-2 text-xs">
                                <div class="flex items-center justify-between p-2.5 bg-slate-800/60 rounded-lg">
                                    <div>
                                        <div class="font-bold text-white">Carlos Nhantumbo</div>
                                        <div class="text-[10px] text-slate-400">M-Pesa · 1.500 MZN</div>
                                    </div>
                                    <span class="px-2 py-0.5 bg-emerald-500/20 text-emerald-400 font-mono text-[10px] rounded">18:40</span>
                                </div>
                                <div class="flex items-center justify-between p-2.5 bg-slate-800/60 rounded-lg">
                                    <div>
                                        <div class="font-bold text-white">Família Sitoe (Casal)</div>
                                        <div class="text-[10px] text-slate-400">e-Mola · 2.600 MZN</div>
                                    </div>
                                    <span class="px-2 py-0.5 bg-emerald-500/20 text-emerald-400 font-mono text-[10px] rounded">18:22</span>
                                </div>
                                <div class="flex items-center justify-between p-2.5 bg-slate-800/60 rounded-lg">
                                    <div>
                                        <div class="font-bold text-white">Hélio Muianga</div>
                                        <div class="text-[10px] text-slate-400">Numerário · 200 MZN</div>
                                    </div>
                                    <span class="px-2 py-0.5 bg-emerald-500/20 text-emerald-400 font-mono text-[10px] rounded">17:55</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- ECRÃ 2: DONO (DESKTOP) -->
            <div id="screen-desktop" class="screen-view hidden transition-all">
                <div class="glass-panel p-6 sm:p-8 rounded-3xl max-w-5xl mx-auto border border-slate-700 shadow-2xl">
                    <div class="flex items-center justify-between pb-4 border-b border-slate-800 mb-6">
                        <div>
                            <div class="text-xs font-bold text-pulso-500 uppercase">O dono no escritório (Computador)</div>
                            <h3 class="text-xl font-bold text-white">Receita do mês, membros activos e o que precisa de atenção hoje</h3>
                        </div>
                        <div class="flex items-center gap-2">
                            <span class="w-8 h-8 rounded-full bg-pulso-600 font-bold text-xs flex items-center justify-center text-white">IS</span>
                            <span class="text-xs text-slate-300 font-medium hidden sm:inline">Ismael (Dono)</span>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-6">
                        <div class="bg-slate-900 p-4 rounded-xl border border-slate-800">
                            <div class="text-xs text-slate-400">Membros activos</div>
                            <div class="text-2xl font-black text-white">212</div>
                            <div class="text-[10px] text-emerald-400 font-semibold">+6 esta semana</div>
                        </div>
                        <div class="bg-slate-900 p-4 rounded-xl border border-slate-800">
                            <div class="text-xs text-slate-400">Receita Outubro</div>
                            <div class="text-2xl font-black text-emerald-400">48.500 <span class="text-xs font-normal">MZN</span></div>
                            <div class="text-[10px] text-emerald-400 font-semibold">+12% vs Setembro</div>
                        </div>
                        <div class="bg-slate-900 p-4 rounded-xl border border-slate-800">
                            <div class="text-xs text-slate-400">Presenças hoje</div>
                            <div class="text-2xl font-black text-white">96</div>
                            <div class="text-[10px] text-slate-400">Pico às 18h</div>
                        </div>
                        <div class="bg-slate-900 p-4 rounded-xl border border-slate-800">
                            <div class="text-xs text-slate-400">Licença Pulso</div>
                            <div class="text-2xl font-black text-amber-400">284 <span class="text-xs font-normal text-slate-400">dias</span></div>
                            <div class="text-[10px] text-slate-400">Licença anual activa</div>
                        </div>
                    </div>

                    <!-- PENDING ACTION REVIEWS -->
                    <div class="bg-slate-900 p-5 rounded-2xl border border-slate-800 mb-6">
                        <h4 class="text-xs font-bold uppercase text-amber-400 tracking-wider mb-3">Precisa de ti hoje (4 itens)</h4>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-xs">
                            <div class="bg-slate-800/80 p-3 rounded-xl border border-slate-700/60 flex items-center justify-between" id="proofItem1">
                                <div>
                                    <div class="font-bold text-white">2 comprovativos por aprovar</div>
                                    <div class="text-[11px] text-slate-400">Fátima Uamusse · e-Mola 1.500 MZN · ref 8K2X91</div>
                                </div>
                                <button onclick="approveProof('proofItem1')" class="px-3 py-1.5 bg-emerald-600 hover:bg-emerald-500 text-white font-bold rounded-lg transition-colors text-[11px]">Aprovar</button>
                            </div>
                            <div class="bg-slate-800/80 p-3 rounded-xl border border-slate-700/60 flex items-center justify-between">
                                <div>
                                    <div class="font-bold text-white">3 planos acabam esta semana</div>
                                    <div class="text-[11px] text-slate-400">Júlio, Marta e Dércio</div>
                                </div>
                                <button onclick="alert('Lista de membros a expirar pronta para enviar WhatsApp!')" class="px-3 py-1.5 bg-slate-700 hover:bg-slate-600 text-white font-medium rounded-lg transition-colors text-[11px]">Ver</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- ECRÃ 3: MEMBRO (MOBILE) -->
            <div id="screen-mobile" class="screen-view hidden transition-all">
                <div class="glass-panel p-6 sm:p-8 rounded-3xl max-w-sm mx-auto border border-slate-700 shadow-2xl">
                    <div class="text-center pb-4 border-b border-slate-800 mb-4">
                        <div class="text-xs font-bold text-pulso-500 uppercase">O membro no bolso (Telemóvel)</div>
                        <h3 class="text-lg font-bold text-white">Passe para a porta, aulas e dias de treino</h3>
                    </div>

                    <!-- VIRTUAL MEMBER PASS CARD -->
                    <div class="bg-gradient-to-br from-slate-900 to-slate-800 p-5 rounded-2xl border border-pulso-500/40 relative overflow-hidden shadow-xl mb-4">
                        <div class="absolute -right-6 -bottom-6 w-24 h-24 bg-pulso-600/20 rounded-full blur-xl"></div>
                        <div class="flex justify-between items-start mb-4">
                            <div>
                                <div class="text-xs text-slate-400">Passe Nº 0142</div>
                                <div class="text-xl font-bold text-white">Ismael Sitoe</div>
                            </div>
                            <span class="px-2 py-1 bg-emerald-500/20 text-emerald-400 font-bold text-[10px] rounded-full uppercase">Activo</span>
                        </div>

                        <!-- SIMULATED QR/BARCODE SIMULATION -->
                        <div class="bg-white p-3 rounded-xl mb-4 text-center">
                            <div class="h-12 bg-slate-950 rounded flex items-center justify-around px-2">
                                <div class="w-1 h-full bg-white"></div>
                                <div class="w-2 h-full bg-white"></div>
                                <div class="w-0.5 h-full bg-white"></div>
                                <div class="w-3 h-full bg-white"></div>
                                <div class="w-1.5 h-full bg-white"></div>
                                <div class="w-0.5 h-full bg-white"></div>
                                <div class="w-2.5 h-full bg-white"></div>
                                <div class="w-1 h-full bg-white"></div>
                            </div>
                            <span class="text-[10px] font-mono text-slate-600 mt-1 block">Mostra no leitor da porta</span>
                        </div>

                        <div class="flex justify-between text-xs border-t border-slate-700/60 pt-3 text-slate-300">
                            <div>Plano: <strong class="text-white">Mensal</strong></div>
                            <div>Válido até: <strong class="text-emerald-400">8 Nov (22 dias)</strong></div>
                        </div>
                    </div>

                    <!-- NEXT CLASS PREVIEW -->
                    <div class="bg-slate-900 p-4 rounded-xl border border-slate-800 text-xs">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-white">A seguir, hoje</span>
                            <span class="text-pulso-500 font-mono font-bold">18:00</span>
                        </div>
                        <div class="text-slate-300 font-medium">CrossFit · Treinador Paulo</div>
                        <div class="text-slate-500 text-[11px] mt-1">16 de 20 lugares ocupados</div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- INTERACTIVE PRORATED CALCULATOR SECTION -->
    <section class="py-16 bg-slate-900/80 border-y border-slate-800">
        <div class="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
                <div>
                    <span class="px-3 py-1 bg-pulso-600/20 text-pulso-400 text-xs font-bold rounded-full uppercase">Flexibilidade Total</span>
                    <h2 class="text-2xl sm:text-3xl font-extrabold text-white mt-3 mb-4">
                        Entrou a meio do mês? Tu decides.
                    </h2>
                    <p class="text-slate-300 text-sm leading-relaxed mb-6">
                        Liga o primeiro mês proporcional e cobras só os dias que faltam. Desliga e cobras o mês inteiro. Sem complicações e calculado na hora.
                    </p>
                    <div class="bg-slate-950 p-4 rounded-xl border border-slate-800 text-xs text-slate-400 space-y-2">
                        <div class="flex items-center gap-2">
                            <svg class="w-4 h-4 text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                            <span>Opção de ajustar o valor base por mensalidade (Ex: 1.500 MZN/mês)</span>
                        </div>
                        <div class="flex items-center gap-2">
                            <svg class="w-4 h-4 text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                            <span>Registo automático da data proporcional no histórico do membro</span>
                        </div>
                    </div>
                </div>

                <!-- CALCULATOR INTERACTIVE CARD -->
                <div class="glass-panel p-6 rounded-2xl border border-slate-700">
                    <div class="flex justify-between items-center mb-4">
                        <span class="text-xs font-bold text-slate-300 uppercase">Calculadora Proporcional</span>
                        <label class="inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="toggleProrated" checked onchange="calculateProrated()" class="sr-only peer">
                            <div class="w-11 h-6 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-pulso-600 relative"></div>
                            <span class="ml-2 text-xs text-slate-300 font-medium">Cobrar proporcional</span>
                        </label>
                    </div>

                    <div class="space-y-4">
                        <div>
                            <div class="flex justify-between text-xs text-slate-400 mb-1">
                                <span>Dia de entrada no mês (Outubro)</span>
                                <span class="text-white font-bold" id="dayDisplay">Dia 19</span>
                            </div>
                            <input type="range" id="entryDaySlider" min="1" max="31" value="19" oninput="calculateProrated()" class="w-full accent-pulso-600 bg-slate-800 h-2 rounded-lg cursor-pointer">
                        </div>

                        <div class="bg-slate-900 p-4 rounded-xl border border-slate-800 space-y-2">
                            <div class="flex justify-between text-xs text-slate-400">
                                <span>Plano Mensal Padrão:</span>
                                <span class="text-white font-semibold">1.500 MZN</span>
                            </div>
                            <div class="flex justify-between text-xs text-slate-400">
                                <span>Dias a usufruir:</span>
                                <span class="text-white font-semibold" id="daysRemainingText">13 de 31 dias</span>
                            </div>
                            <div class="border-t border-slate-800 pt-2 flex justify-between items-baseline">
                                <span class="text-sm font-bold text-slate-200">Valor Final A Cobrar:</span>
                                <span class="text-3xl font-black text-emerald-400" id="calculatedPrice">629 MZN</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- TIMELINE SECTION: UM DIA NO PULSO -->
    <section id="um-dia" class="py-20 bg-slate-950">
        <div class="max-w-4xl mx-auto px-4 sm:px-6">
            <div class="text-center mb-12">
                <h2 class="text-xs uppercase tracking-widest font-bold text-pulso-500 mb-2">Um dia com o Pulso</h2>
                <p class="text-3xl sm:text-4xl font-extrabold text-white">Das 5h45 ao fecho do caixa.</p>
                <p class="text-slate-400 mt-2 text-sm">O que acontece num dia normal de ginásio e quem trata de quê.</p>
            </div>

            <!-- INTERACTIVE TIMELINE CARDS -->
            <div class="relative border-l-2 border-slate-800 ml-4 sm:ml-32 space-y-8">
                <!-- 05:45 -->
                <div class="relative pl-6 sm:pl-8">
                    <span class="absolute -left-[9px] top-1.5 w-4 h-4 rounded-full bg-pulso-600 border-4 border-slate-950"></span>
                    <span class="sm:absolute sm:-left-28 sm:top-1 text-xs font-mono font-bold text-slate-400 block mb-1 sm:mb-0">05:45</span>
                    <div class="glass-panel p-4 rounded-xl">
                        <h4 class="font-bold text-white text-sm">A primeira entrada do dia</h4>
                        <p class="text-xs text-slate-400 mt-1">O leitor reconhece a Ana e regista a presença. Ainda não está ninguém na recepção, nem precisa.</p>
                        <div class="mt-2 inline-flex items-center gap-2 bg-emerald-950/60 border border-emerald-500/30 text-emerald-300 text-xs px-3 py-1 rounded-lg">
                            <span class="font-bold">Ana Machava</span> · Entrada permitida
                        </div>
                    </div>
                </div>

                <!-- 07:10 -->
                <div class="relative pl-6 sm:pl-8">
                    <span class="absolute -left-[9px] top-1.5 w-4 h-4 rounded-full bg-pulso-600 border-4 border-slate-950"></span>
                    <span class="sm:absolute sm:-left-28 sm:top-1 text-xs font-mono font-bold text-slate-400 block mb-1 sm:mb-0">07:10</span>
                    <div class="glass-panel p-4 rounded-xl">
                        <h4 class="font-bold text-white text-sm">Uma renovação sem fila</h4>
                        <p class="text-xs text-slate-400 mt-1">O casal Sitoe paga por M-Pesa. Os dois lugares do plano renovam juntos.</p>
                        <div class="mt-2 inline-flex items-center gap-2 bg-slate-900 border border-slate-700 text-xs px-3 py-1 rounded-lg">
                            <span class="text-red-400 font-bold">M-PESA</span>
                            <span class="text-emerald-400 font-bold">+2.600 MZN</span>
                            <span class="text-slate-400">· Plano casal (2 lugares)</span>
                        </div>
                    </div>
                </div>

                <!-- 09:30 -->
                <div class="relative pl-6 sm:pl-8">
                    <span class="absolute -left-[9px] top-1.5 w-4 h-4 rounded-full bg-pulso-600 border-4 border-slate-950"></span>
                    <span class="sm:absolute sm:-left-28 sm:top-1 text-xs font-mono font-bold text-slate-400 block mb-1 sm:mb-0">09:30</span>
                    <div class="glass-panel p-4 rounded-xl">
                        <h4 class="font-bold text-white text-sm">Três inscrições antes do almoço</h4>
                        <p class="text-xs text-slate-400 mt-1">A recepção cola a lista de nomes, confirma os telefones e escolhe o plano. Feito.</p>
                        <div class="mt-2 text-xs text-slate-300 space-y-0.5">
                            <div>• Hélio Muianga</div>
                            <div>• Inês Zandamela</div>
                            <div>• Nelson Cuna</div>
                        </div>
                    </div>
                </div>

                <!-- 12:15 -->
                <div class="relative pl-6 sm:pl-8">
                    <span class="absolute -left-[9px] top-1.5 w-4 h-4 rounded-full bg-pulso-600 border-4 border-slate-950"></span>
                    <span class="sm:absolute sm:-left-28 sm:top-1 text-xs font-mono font-bold text-slate-400 block mb-1 sm:mb-0">12:15</span>
                    <div class="glass-panel p-4 rounded-xl">
                        <h4 class="font-bold text-white text-sm">O dono espreita pelo telemóvel</h4>
                        <p class="text-xs text-slate-400 mt-1">Receita do dia, entradas e planos que acabam esta semana, onde quer que esteja.</p>
                        <div class="mt-2 text-xs font-bold text-amber-400">
                            Até agora: <span class="text-white">18.400 MZN</span>
                        </div>
                    </div>
                </div>

                <!-- 17:45 -->
                <div class="relative pl-6 sm:pl-8">
                    <span class="absolute -left-[9px] top-1.5 w-4 h-4 rounded-full bg-pulso-600 border-4 border-slate-950"></span>
                    <span class="sm:absolute sm:-left-28 sm:top-1 text-xs font-mono font-bold text-slate-400 block mb-1 sm:mb-0">17:45</span>
                    <div class="glass-panel p-4 rounded-xl">
                        <h4 class="font-bold text-white text-sm">Hora de ponta, porta firme</h4>
                        <p class="text-xs text-slate-400 mt-1">Quem tem o plano em dia entra. O Rui não passa: o plano acabou a 2 de Outubro.</p>
                        <div class="mt-2 inline-flex items-center gap-2 bg-rose-950/60 border border-rose-500/30 text-rose-300 text-xs px-3 py-1 rounded-lg">
                            <span class="font-bold">Rui Bila</span> · Entrada negada
                        </div>
                    </div>
                </div>

                <!-- 21:30 -->
                <div class="relative pl-6 sm:pl-8">
                    <span class="absolute -left-[9px] top-1.5 w-4 h-4 rounded-full bg-pulso-600 border-4 border-slate-950"></span>
                    <span class="sm:absolute sm:-left-28 sm:top-1 text-xs font-mono font-bold text-slate-400 block mb-1 sm:mb-0">21:30</span>
                    <div class="glass-panel p-4 rounded-xl">
                        <h4 class="font-bold text-white text-sm">Fecho de caixa, sem calculadora</h4>
                        <p class="text-xs text-slate-400 mt-1">O dia separado por método de pagamento. O numerário no painel bate com o da gaveta.</p>
                        <div class="mt-2 grid grid-cols-2 sm:grid-cols-4 gap-2 text-xs">
                            <div class="bg-slate-900 p-2 rounded text-slate-300">M-Pesa: <strong class="text-white">6.000</strong></div>
                            <div class="bg-slate-900 p-2 rounded text-slate-300">e-Mola: <strong class="text-white">1.500</strong></div>
                            <div class="bg-slate-900 p-2 rounded text-slate-300">Numerário: <strong class="text-white">3.200</strong></div>
                            <div class="bg-slate-900 p-2 rounded text-slate-300">Transf.: <strong class="text-white">0</strong></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- SECTION: PRICING & LICENSES -->
    <section id="licencas" class="py-20 bg-slate-900">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center max-w-3xl mx-auto mb-12">
                <h2 class="text-xs uppercase tracking-widest font-bold text-pulso-500 mb-2">Licenças de Software</h2>
                <p class="text-3xl sm:text-4xl font-extrabold text-white">Quanto mais tempo, mais barato fica o mês.</p>
                <p class="text-slate-400 mt-3 text-sm">
                    Crias a conta e recebes logo <strong>7 dias grátis</strong>, já aplicados. Depois escolhes a duração conveniente para o teu ginásio.
                </p>
            </div>

            <!-- LICENSES GRID -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- 1 Mês -->
                <div class="glass-panel p-6 rounded-xl flex flex-col justify-between border-slate-800">
                    <div>
                        <div class="text-base font-bold text-white mb-1">1 Mês</div>
                        <p class="text-xs text-slate-400 mb-4">Para começar com os teus membros reais.</p>
                        <div class="text-xl font-bold text-white mb-2">Preço Base</div>
                        <span class="inline-block px-2.5 py-1 bg-slate-800 text-slate-300 text-xs rounded">Mensal flexível</span>
                    </div>
                    <button onclick="openModal('signupModal')" class="mt-6 btn-secondary w-full py-2.5 text-xs">
                        Escolher 1 mês
                    </button>
                </div>

                <!-- 3 Meses -->
                <div class="glass-panel p-6 rounded-xl flex flex-col justify-between border-slate-800">
                    <div>
                        <div class="flex justify-between items-center mb-1">
                            <div class="text-base font-bold text-white">3 Meses</div>
                            <span class="text-xs font-semibold text-emerald-400 bg-emerald-950/60 px-2 py-0.5 rounded border border-emerald-800/60">−5% / mês</span>
                        </div>
                        <p class="text-xs text-slate-400 mb-4">Uma época inteira de treinos.</p>
                        <div class="text-xl font-bold text-white mb-2">Trimestral</div>
                        <span class="inline-block px-2.5 py-1 bg-slate-800 text-slate-300 text-xs rounded">Economia trimestral</span>
                    </div>
                    <button onclick="openModal('signupModal')" class="mt-6 btn-secondary w-full py-2.5 text-xs">
                        Escolher 3 meses
                    </button>
                </div>

                <!-- 6 Meses -->
                <div class="glass-panel p-6 rounded-xl flex flex-col justify-between border-slate-800">
                    <div>
                        <div class="flex justify-between items-center mb-1">
                            <div class="text-base font-bold text-white">6 Meses</div>
                            <span class="text-xs font-semibold text-emerald-400 bg-emerald-950/60 px-2 py-0.5 rounded border border-emerald-800/60">−10% / mês</span>
                        </div>
                        <p class="text-xs text-slate-400 mb-4">Meio ano sem pensar em renovar.</p>
                        <div class="text-xl font-bold text-white mb-2">Semestral</div>
                        <span class="inline-block px-2.5 py-1 bg-slate-800 text-slate-300 text-xs rounded">Desconto semestral</span>
                    </div>
                    <button onclick="openModal('signupModal')" class="mt-6 btn-secondary w-full py-2.5 text-xs">
                        Escolher 6 meses
                    </button>
                </div>

                <!-- 12 Meses (POPULAR) -->
                <div class="glass-panel p-6 rounded-xl flex flex-col justify-between border-2 border-pulso-600 relative">
                    <div class="absolute -top-3 right-4 px-2.5 py-0.5 bg-pulso-600 text-white text-[10px] font-bold uppercase rounded">
                        Mais vantajoso
                    </div>
                    <div>
                        <div class="flex justify-between items-center mb-1">
                            <div class="text-base font-bold text-white">12 Meses</div>
                            <span class="text-xs font-semibold text-emerald-400 bg-emerald-950/60 px-2 py-0.5 rounded border border-emerald-800/60">−15% / mês</span>
                        </div>
                        <p class="text-xs text-slate-400 mb-4">O ano inteiro, ao melhor preço por mês.</p>
                        <div class="text-xl font-bold text-white mb-2">Anual</div>
                        <span class="inline-block px-2.5 py-1 bg-pulso-600/20 text-pulso-400 text-xs rounded font-semibold">Máxima eficiência</span>
                    </div>
                    <button onclick="openModal('signupModal')" class="mt-6 btn-primary w-full py-2.5 text-xs">
                        Escolher 12 meses
                    </button>
                </div>
            </div>

            <!-- PAYMENT NOTE -->
            <div class="mt-8 text-center text-xs text-slate-400 bg-slate-950/60 p-4 rounded-xl border border-slate-800 max-w-2xl mx-auto">
                <span class="font-bold text-slate-200">Nota de Pagamento:</span> A licença paga-se por transferência bancária, em meticais (MZN). Sem cartão, sem M-Pesa, sem débito directo.
            </div>
        </div>
    </section>

    <!-- FAQ SECTION (PERGUNTAS FREQUENTES) -->
    <section id="faq" class="py-20 bg-slate-950 border-t border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center max-w-3xl mx-auto mb-12">
                <div class="flex items-center justify-center gap-2 text-xs font-mono text-slate-400 mb-2">
                    <a href="#" class="hover:text-white">Início</a>
                    <span>/</span>
                    <span class="text-pulso-500 font-bold">Perguntas</span>
                </div>
                <h2 class="text-3xl sm:text-4xl font-extrabold text-white">Perguntas, com respostas directas.</h2>
                <p class="text-slate-400 mt-3 text-sm">
                    Licenças, pagamentos, a porta e os dados dos teus membros. Se a tua pergunta não estiver aqui, escreve-nos.
                </p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
                <!-- CATEGORY FILTER SIDEBAR -->
                <div class="space-y-3">
                    <div class="glass-panel p-3 rounded-xl border border-slate-800 space-y-1">
                        <button onclick="filterFaq('all')" id="faq-cat-all" class="faq-cat-btn w-full text-left px-3 py-2 rounded-lg text-xs font-semibold transition-all bg-pulso-600 text-white flex justify-between items-center">
                            <span>Todas as Perguntas</span>
                            <span class="bg-white/20 px-2 py-0.5 rounded text-[10px]">19</span>
                        </button>
                        <button onclick="filterFaq('licensa')" id="faq-cat-licensa" class="faq-cat-btn w-full text-left px-3 py-2 rounded-lg text-xs font-medium text-slate-400 hover:text-white transition-all flex justify-between items-center">
                            <span>Licença e preço</span>
                            <span class="bg-slate-800 text-slate-400 px-2 py-0.5 rounded text-[10px]">6</span>
                        </button>
                        <button onclick="filterFaq('comecar')" id="faq-cat-comecar" class="faq-cat-btn w-full text-left px-3 py-2 rounded-lg text-xs font-medium text-slate-400 hover:text-white transition-all flex justify-between items-center">
                            <span>Começar</span>
                            <span class="bg-slate-800 text-slate-400 px-2 py-0.5 rounded text-[10px]">3</span>
                        </button>
                        <button onclick="filterFaq('membros')" id="faq-cat-membros" class="faq-cat-btn w-full text-left px-3 py-2 rounded-lg text-xs font-medium text-slate-400 hover:text-white transition-all flex justify-between items-center">
                            <span>Membros e pagamentos</span>
                            <span class="bg-slate-800 text-slate-400 px-2 py-0.5 rounded text-[10px]">4</span>
                        </button>
                        <button onclick="filterFaq('porta')" id="faq-cat-porta" class="faq-cat-btn w-full text-left px-3 py-2 rounded-lg text-xs font-medium text-slate-400 hover:text-white transition-all flex justify-between items-center">
                            <span>Porta e presenças</span>
                            <span class="bg-slate-800 text-slate-400 px-2 py-0.5 rounded text-[10px]">3</span>
                        </button>
                        <button onclick="filterFaq('seguranca')" id="faq-cat-seguranca" class="faq-cat-btn w-full text-left px-3 py-2 rounded-lg text-xs font-medium text-slate-400 hover:text-white transition-all flex justify-between items-center">
                            <span>Dados e segurança</span>
                            <span class="bg-slate-800 text-slate-400 px-2 py-0.5 rounded text-[10px]">3</span>
                        </button>
                    </div>

                    <!-- SUPPORT CALLOUT BOX -->
                    <div class="glass-panel p-5 rounded-2xl border border-slate-800 bg-gradient-to-br from-slate-900 to-slate-900/60">
                        <div class="text-xs font-bold text-white mb-1">Não encontras?</div>
                        <p class="text-[11px] text-slate-400 mb-3">Liga ou manda WhatsApp para +258 84 202 5446. Respondemos em dias úteis.</p>
                        <a href="https://wa.me/258842025446" target="_blank" class="text-xs font-bold text-pulso-400 hover:text-pulso-300 flex items-center gap-1">
                            Ir para contacto →
                        </a>
                    </div>
                </div>

                <!-- ACCORDION ITEMS -->
                <div class="lg:col-span-3 space-y-3" id="faqAccordionContainer">
                    
                    <!-- LICENÇA E PREÇO -->
                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="licensa">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Quanto custa o Pulso?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Ao criares a conta recebes 7 dias grátis, já aplicados. Depois, o preço depende de quantos membros tens, do período da licença e dos módulos que entram. No fim do teste mandamos por email um valor fechado. <button onclick="openModal('signupModal')" class="text-pulso-400 font-bold hover:underline">Criar conta grátis.</button>
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="licensa">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Posso experimentar antes de pagar?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Sim! Tens 7 dias de acesso total grátis ao criar a conta. Podes registar a tua equipa, cadastrar membros e testar o controlo de presenças na prática sem pagar nada.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="licensa">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Que períodos posso escolher?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Disponibilizamos licenças flexíveis de 1, 3, 6 ou 12 meses. Quanto maior for o período escolhido, menor será o custo mensal (com descontos até 15% na licença anual).
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="licensa">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Há taxa de instalação ou contrato de fidelização?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Não existe qualquer taxa de instalação nem contratos obrigações de permanência longa. Renovas quando e se quiseres ao terminar a tua licença.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="licensa">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Como pago a licença?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            A licença do software paga-se por transferência bancária direta, em meticais (MZN). Não exigimos cartão de crédito nem débito direto.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="licensa">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Se renovar antes do fim, perco dias?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Nunca. Se renovares com antecedência, os novos dias são automaticamente acumulados e somados à data limite atual da tua licença.
                        </div>
                    </div>

                    <!-- COMEÇAR -->
                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="comecar">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Quanto tempo demora a pôr o ginásio a funcionar?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Menos de 10 minutos! Crias a conta, defines os teus planos de mensalidade e podes começar logo a registar os primeiros membros e pagamentos.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="comecar">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Posso importar os membros que já tenho?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Sim. Se já tens uma lista em Excel ou noutro sistema, ajudamos-te a importar todos os membros activos para o Pulso sem perder o histórico.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="comecar">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Funciona no telemóvel?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Sim, a 100%. O sistema adapta-se perfeitamente a computadores de secretária, tablets na recepção e telemóveis de proprietários ou membros.
                        </div>
                    </div>

                    <!-- MEMBROS E PAGAMENTOS -->
                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="membros">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Que métodos de pagamento posso registar?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Podes registar pagamentos em M-Pesa, e-Mola, numerário (dinheiro físico) e transferência bancária, mantendo o total segregado no fecho de caixa.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="membros">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Os membros podem enviar o comprovativo?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Sim. Os membros podem submeter comprovativos digitais de M-Pesa ou e-Mola. O dono ou recepção recebe a notificação no painel para aprovação imediata.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="membros">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Como funcionam os planos de casal ou família?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Permitem associar múltiplos membros a um único plano titular (ex: Titular + 1). Quando o titular realiza o pagamento, a validade renova para todos os dependentes.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="membros">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Quem entra a meio do mês paga o mês inteiro?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Tu decides! O Pulso tem uma funcionalidade de cobrança proporcional automática que calcula apenas os dias restantes do mês se assim o desejares.
                        </div>
                    </div>

                    <!-- PORTA E PRESENÇAS -->
                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="porta">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Que leitores funcionam com o Pulso?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            O Pulso é compatível com catracas eletrónicas padrão, leitores de QR Code, biometria e módulos de integração via rede local.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="porta">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>O que acontece quando o plano de um membro acaba?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            A entrada é automaticamente negada pela catraca/leitor, apresentando o aviso de plano expirado, eliminando discussões ou exceções manuais.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="porta">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>E se não tiver leitor na porta?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Podes usar o ecrã da recepção num tablet ou telemóvel para registar as presenças manualmente em 1 clique ao balcão.
                        </div>
                    </div>

                    <!-- DADOS E SEGURANÇA -->
                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="seguranca">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Outros ginásios conseguem ver os meus membros?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Absolutamente não. Cada ginásio opera numa instância isolada com subdomínio próprio (ex: <em>seuginasio.pulso.fitness</em>) e encriptação completa de dados.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="seguranca">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>A minha equipa vê tudo?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Não. Podes definir permissões de acesso: a recepção só vê o balcão e entradas, os treinadores só vêm as turmas e o dono tem acesso aos totais financeiros.
                        </div>
                    </div>

                    <div class="faq-item glass-panel rounded-2xl border border-slate-800 overflow-hidden" data-category="seguranca">
                        <button onclick="toggleFaq(this)" class="w-full p-5 text-left flex justify-between items-center text-sm font-bold text-white hover:text-pulso-400 transition-colors">
                            <span>Posso tirar os meus dados?</span>
                            <span class="faq-icon text-pulso-500 text-xl font-mono">+</span>
                        </button>
                        <div class="faq-content hidden px-5 pb-5 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 pt-3">
                            Sim. Os dados pertencem ao teu ginásio. Podes exportar a lista de membros e pagamentos em formato Excel/CSV a qualquer momento.
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </section>

    <!-- PROOF & SOCIAL PROOF -->
    <section class="py-16 bg-slate-950">
        <div class="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="glass-panel p-8 rounded-3xl border border-slate-800 flex flex-col md:flex-row items-center justify-between gap-8">
                <div>
                    <span class="text-xs font-bold text-pulso-500 uppercase tracking-wider">Quem já usa</span>
                    <h3 class="text-2xl font-extrabold text-white mt-1">Fit24Gym entra no Pulso</h3>
                    <p class="text-xs text-slate-400 mt-2">fit24gym.pulso.fitness · Porta ligada ao sistema · Membros e pagamentos no Pulso</p>
                </div>
                <div class="flex items-center gap-4">
                    <div class="w-14 h-14 rounded-2xl bg-slate-900 border border-slate-700 flex items-center justify-center font-black text-white text-xl">
                        F24
                    </div>
                    <div>
                        <div class="font-bold text-slate-200">Fit24Gym Maputo</div>
                        <div class="text-xs text-emerald-400 font-semibold">Em funcionamento contínuo em 2026</div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- FINAL CTA SECTION -->
    <section class="py-20 bg-slate-950 text-center border-t border-slate-900">
        <div class="max-w-4xl mx-auto px-4 sm:px-6">
            <h2 class="text-3xl sm:text-4xl font-extrabold text-white mb-4">
                Já sabes o que precisas?
            </h2>
            <p class="text-slate-300 text-base mb-8 max-w-2xl mx-auto">
                Cria a conta e tens 7 dias grátis. No fim do teste, mandamos o valor por email.
            </p>
            <div class="flex flex-col sm:flex-row items-center justify-center gap-3">
                <button onclick="openModal('signupModal')" class="btn-primary w-full sm:w-auto px-8 py-3.5 text-base">
                    Criar conta grátis →
                </button>
                <a href="https://wa.me/258842025446" target="_blank" class="btn-secondary w-full sm:w-auto px-8 py-3.5 text-base flex items-center justify-center gap-2">
                    <svg class="w-5 h-5 text-emerald-400" fill="currentColor" viewBox="0 0 24 24"><path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.946.003-6.556 5.338-11.891 11.893-11.891 3.181.001 6.167 1.24 8.413 3.488 2.245 2.248 3.481 5.236 3.48 8.414-.003 6.557-5.338 11.892-11.892-1.99-.001-3.951-.5-5.688-1.448l-6.305 1.654zm6.597-3.807c1.676.995 3.276 1.591 5.392 1.592 5.448 0 9.886-4.434 9.889-9.885.002-5.462-4.415-9.89-9.881-9.892-5.452 0-9.887 4.434-9.889 9.884-.001 2.225.651 3.891 1.746 5.634l-.999 3.648 3.742-.981z"/></svg>
                    Falar connosco
                </a>
            </div>
        </div>
    </section>

    <!-- FOOTER -->
    <footer id="contacto" class="bg-slate-950 border-t border-slate-800 pt-16 pb-12 text-xs text-slate-400">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-2 md:grid-cols-5 gap-8 mb-12">
            <div class="col-span-2">
                <a href="#" class="text-xl font-black text-white flex items-center gap-2 mb-3">
                    <span class="w-6 h-6 rounded bg-pulso-600 flex items-center justify-center text-white text-xs">S</span>
                    Pulso.fitness
                </a>
                <p class="text-slate-400 max-w-sm mb-4">
                    Licenças de software para ginásios em Moçambique. Feito em Maputo.
                </p>
                <div class="text-slate-300 font-semibold">
                    +258 84 202 5446 · WhatsApp
                </div>
            </div>

            <div>
                <h4 class="font-bold text-white uppercase text-[11px] tracking-wider mb-3">Produto</h4>
                <ul class="space-y-2">
                    <li><a href="#funcionalidades" class="hover:text-white transition-colors">Funcionalidades</a></li>
                    <li><a href="#ecras" class="hover:text-white transition-colors">Três ecrãs</a></li>
                    <li><a href="#licencas" class="hover:text-white transition-colors">Licenças</a></li>
                    <li><a href="#" onclick="openModal('loginModal')" class="hover:text-white transition-colors">Entrar</a></li>
                </ul>
            </div>

            <div>
                <h4 class="font-bold text-white uppercase text-[11px] tracking-wider mb-3">Ajuda</h4>
                <ul class="space-y-2">
                    <li><a href="#faq" class="hover:text-white transition-colors">Perguntas</a></li>
                    <li><a href="https://wa.me/258842025446" target="_blank" class="hover:text-white transition-colors">Contacto</a></li>
                    <li><a href="#ecras" class="hover:text-white transition-colors">Demonstração</a></li>
                </ul>
            </div>

            <div>
                <h4 class="font-bold text-white uppercase text-[11px] tracking-wider mb-3">Legal</h4>
                <ul class="space-y-2">
                    <li><a href="#" class="hover:text-white transition-colors">Termos</a></li>
                    <li><a href="#" class="hover:text-white transition-colors">Privacidade</a></li>
                    <li><a href="#" class="hover:text-white transition-colors">Segurança</a></li>
                </ul>
            </div>
        </div>

        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 border-t border-slate-900 pt-6 flex flex-col sm:flex-row justify-between items-center text-slate-500 text-[11px]">
            <div>© 2026 Pulso · Maputo, Moçambique</div>
            <div class="mt-2 sm:mt-0">O teu ginásio, a bater certo.</div>
        </div>
    </footer>
    </div> <!-- END content-layer -->

    <!-- MODAL: LOGIN -->
    <div id="loginModal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="glass-panel p-6 sm:p-8 rounded-3xl max-w-md w-full border border-slate-700 relative">
            <button onclick="closeModal('loginModal')" class="absolute top-4 right-4 text-slate-400 hover:text-white text-xl">✕</button>
            <div class="text-center mb-6">
                <div class="w-10 h-10 rounded-xl bg-pulso-600 flex items-center justify-center font-bold text-white mx-auto mb-2 text-xl">S</div>
                <h3 class="text-xl font-bold text-white">Bem-vindo de volta</h3>
                <p class="text-xs text-slate-400 mt-1">Usa o email com que te inscreveste neste ginásio.</p>
            </div>
            <form onsubmit="handleFormSubmit(event, 'Login efectuado com sucesso!')" class="space-y-4">
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Email *</label>
                    <input type="email" required placeholder="tu@exemplo.com" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-sm text-slate-200 focus:outline-none focus:border-pulso-500">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Palavra-passe *</label>
                    <input type="password" required placeholder="A tua palavra-passe" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-sm text-slate-200 focus:outline-none focus:border-pulso-500">
                </div>
                <div class="flex justify-between items-center text-xs">
                    <a href="#" class="text-pulso-400 hover:underline">Esqueci a palavra-passe</a>
                </div>
                <button type="submit" class="w-full py-3 bg-pulso-600 hover:bg-pulso-500 text-white font-bold rounded-xl transition-colors">
                    Entrar
                </button>
            </form>
            <div class="text-center text-xs text-slate-400 mt-4">
                Ainda não és membro? <button onclick="switchModal('loginModal', 'signupModal')" class="text-pulso-400 hover:underline font-bold">Inscreve-te</button>
            </div>
        </div>
    </div>

    <!-- MODAL: SIGNUP (CRIAR CONTA GRÁTIS) -->
    <div id="signupModal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="glass-panel p-6 sm:p-8 rounded-3xl max-w-md w-full border border-slate-700 relative">
            <button onclick="closeModal('signupModal')" class="absolute top-4 right-4 text-slate-400 hover:text-white text-xl">✕</button>
            <div class="text-center mb-6">
                <span class="px-2.5 py-0.5 bg-emerald-500/20 text-emerald-400 text-[10px] font-bold rounded-full uppercase">7 Dias Grátis</span>
                <h3 class="text-xl font-bold text-white mt-2">Cria a tua conta no Pulso</h3>
                <p class="text-xs text-slate-400 mt-1">Cada ginásio com o seu endereço, a sua cor e o seu logótipo.</p>
            </div>
            <form onsubmit="handleFormSubmit(event, 'Conta criada com sucesso! 7 dias grátis activados.')" class="space-y-3">
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Nome do Ginásio *</label>
                    <input type="text" required placeholder="Ex: Maputo Fitness Club" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2 text-sm text-slate-200 focus:outline-none focus:border-pulso-500">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Email do Responsável *</label>
                    <input type="email" required placeholder="dono@ginasio.co.mz" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2 text-sm text-slate-200 focus:outline-none focus:border-pulso-500">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Contacto Telefónico *</label>
                    <input type="tel" required placeholder="+258 84 000 0000" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2 text-sm text-slate-200 focus:outline-none focus:border-pulso-500">
                </div>
                <button type="submit" class="w-full py-3 bg-pulso-600 hover:bg-pulso-500 text-white font-bold rounded-xl transition-colors shadow-lg mt-2">
                    Activar 7 dias grátis agora →
                </button>
            </form>
            <div class="text-center text-xs text-slate-400 mt-4">
                Já tens uma conta? <button onclick="switchModal('signupModal', 'loginModal')" class="text-pulso-400 hover:underline font-bold">Entrar</button>
            </div>
        </div>
    </div>

    <!-- JAVASCRIPT FOR INTERACTIVITY -->
    <script>
        // Tab switching for 3 Screens Showcase
        function switchScreenTab(tabName) {
            const tabs = ['tablet', 'desktop', 'mobile'];
            tabs.forEach(t => {
                const screen = document.getElementById(`screen-${t}`);
                const btn = document.getElementById(`btn-tab-${t}`);
                if (t === tabName) {
                    screen.classList.remove('hidden');
                    btn.classList.add('bg-pulso-600', 'text-white', 'shadow-lg');
                    btn.classList.remove('text-slate-400');
                } else {
                    screen.classList.add('hidden');
                    btn.classList.remove('bg-pulso-600', 'text-white', 'shadow-lg');
                    btn.classList.add('text-slate-400');
                }
            });
        }

        // FAQ Toggle Logic
        function toggleFaq(button) {
            const content = button.nextElementSibling;
            const icon = button.querySelector('.faq-icon');
            const isHidden = content.classList.contains('hidden');

            // Close all open FAQs
            document.querySelectorAll('.faq-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.faq-icon').forEach(el => el.innerText = '+');

            if (isHidden) {
                content.classList.remove('hidden');
                icon.innerText = '−';
            }
        }

        // FAQ Category Filter Logic
        function filterFaq(category) {
            const buttons = document.querySelectorAll('.faq-cat-btn');
            buttons.forEach(btn => {
                btn.classList.remove('bg-pulso-600', 'text-white');
                btn.classList.add('text-slate-400');
            });

            const activeBtn = document.getElementById(`faq-cat-${category}`);
            if (activeBtn) {
                activeBtn.classList.add('bg-pulso-600', 'text-white');
                activeBtn.classList.remove('text-slate-400');
            }

            const items = document.querySelectorAll('.faq-item');
            items.forEach(item => {
                if (category === 'all' || item.dataset.category === category) {
                    item.classList.remove('hidden');
                } else {
                    item.classList.add('hidden');
                }
            });
        }

        // Payment method selection demo
        let currentMethod = 'M-Pesa';
        function selectPaymentMethod(btn, method) {
            document.querySelectorAll('#paymentMethodSelector button').forEach(b => {
                b.classList.remove('border-2', 'border-pulso-500', 'bg-pulso-600/20', 'text-white');
                b.classList.add('border-slate-700', 'bg-slate-900', 'text-slate-300');
            });
            btn.classList.remove('border-slate-700', 'bg-slate-900', 'text-slate-300');
            btn.classList.add('border-2', 'border-pulso-500', 'bg-pulso-600/20', 'text-white');
            currentMethod = method;
        }

        function registerPayment() {
            const msg = document.getElementById('paySuccessMsg');
            msg.classList.remove('hidden');
            setTimeout(() => {
                msg.classList.add('hidden');
            }, 3500);
        }

        function approveProof(itemId) {
            const item = document.getElementById(itemId);
            if (item) {
                item.style.opacity = '0.5';
                item.innerHTML = '<span class="text-emerald-400 font-bold text-xs">✓ Comprovativo Aprovado!</span>';
            }
        }

        // Prorated Fee Calculator Logic
        function calculateProrated() {
            const isProrated = document.getElementById('toggleProrated').checked;
            const entryDay = parseInt(document.getElementById('entryDaySlider').value);
            const totalDaysInMonth = 31;
            const monthlyRate = 1500;

            document.getElementById('dayDisplay').innerText = `Dia ${entryDay}`;

            if (!isProrated) {
                document.getElementById('daysRemainingText').innerText = `31 de 31 dias (Mês Inteiro)`;
                document.getElementById('calculatedPrice').innerText = `${monthlyRate.toLocaleString()} MZN`;
            } else {
                const daysLeft = Math.max(1, totalDaysInMonth - entryDay + 1);
                const calculated = Math.round((monthlyRate / totalDaysInMonth) * daysLeft);
                document.getElementById('daysRemainingText').innerText = `${daysLeft} de ${totalDaysInMonth} dias`;
                document.getElementById('calculatedPrice').innerText = `${calculated.toLocaleString()} MZN`;
            }
        }

        // Modal Handlers
        function openModal(id) {
            document.getElementById(id).classList.remove('hidden');
        }

        function closeModal(id) {
            document.getElementById(id).classList.add('hidden');
        }

        function switchModal(closeId, openId) {
            closeModal(closeId);
            openModal(openId);
        }

        function handleFormSubmit(e, message) {
            e.preventDefault();
            alert(message);
            closeModal('loginModal');
            closeModal('signupModal');
        }

        // ==========================================
        // THREE.JS INTERACTIVE 3D ANIMATED BACKGROUND
        // ==========================================
        window.addEventListener('load', () => {
            const canvas = document.getElementById('webgl-bg');
            if (!canvas || !window.Three || !THREE) return;

            // Check reduced motion preference
            const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

            // Scene, Camera & Renderer
            const scene = new THREE.Scene();
            // Fog for dynamic depth blur (azul escuro / roxo)
            scene.fog = new THREE.FogExp2(0x0b0f19, 0.035);

            const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.z = 25;

            const renderer = new THREE.WebGLRenderer({
                canvas: canvas,
                alpha: true,
                antialias: true,
                powerPreference: "high-performance"
            });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

            // Lighting Setup (Blue, Purple & White Glows)
            const ambientLight = new THREE.AmbientLight(0x3b82f6, 0.6);
            scene.add(ambientLight);

            const pointLight1 = new THREE.PointLight(0x8b5cf6, 2.5, 100); // Purple light
            pointLight1.position.set(15, 15, 15);
            scene.add(pointLight1);

            const pointLight2 = new THREE.PointLight(0x38bdf8, 2.0, 100); // Cyan/Blue light
            pointLight2.position.set(-15, -15, 10);
            scene.add(pointLight2);

            const topLight = new THREE.DirectionalLight(0xffffff, 1.2); // White highlight
            topLight.position.set(0, 20, 10);
            scene.add(topLight);

            // Floating 3D Geometries Group
            const floatingGroup = new THREE.Group();
            scene.add(floatingGroup);

            // Materials with futuristic wireframe and glass-like translucency
            const blueWireMat = new THREE.MeshStandardMaterial({
                color: 0x3b82f6,
                wireframe: true,
                transparent: true,
                opacity: 0.25,
                roughness: 0.2,
                metalness: 0.8
            });

            const purpleSolidMat = new THREE.MeshPhysicalMaterial({
                color: 0x7c3aed,
                roughness: 0.1,
                metalness: 0.2,
                transmission: 0.8,
                transparent: true,
                opacity: 0.45,
                reflectivity: 0.9
            });

            const cyanWireMat = new THREE.MeshStandardMaterial({
                color: 0x06b6d4,
                wireframe: true,
                transparent: true,
                opacity: 0.35
            });

            // Geometry 1: Central TorusKnot
            const knotGeo = new THREE.TorusKnotGeometry(4.5, 1.2, 100, 16);
            const torusKnot = new THREE.Mesh(knotGeo, blueWireMat);
            torusKnot.position.set(-8, 3, -5);
            floatingGroup.add(torusKnot);

            // Geometry 2: Icosahedron
            const icoGeo = new THREE.IcosahedronGeometry(3.5, 1);
            const icoMesh = new THREE.Mesh(icoGeo, purpleSolidMat);
            icoMesh.position.set(10, -2, -8);
            floatingGroup.add(icoMesh);

            // Geometry 3: Outer Ring
            const ringGeo = new THREE.TorusGeometry(6, 0.2, 16, 100);
            const ringMesh = new THREE.Mesh(ringGeo, cyanWireMat);
            ringMesh.position.set(10, -2, -8);
            ringMesh.rotation.x = Math.PI / 3;
            floatingGroup.add(ringMesh);

            // Geometry 4: Small Octahedrons
            const octGeo = new THREE.OctahedronGeometry(1.8);
            const octMesh1 = new THREE.Mesh(octGeo, blueWireMat);
            octMesh1.position.set(2, 8, -12);
            floatingGroup.add(octMesh1);

            const octMesh2 = new THREE.Mesh(octGeo, purpleSolidMat);
            octMesh2.position.set(-12, -8, -10);
            floatingGroup.add(octMesh2);

            // Particle Starfield Dust
            const isMobile = window.innerWidth < 768;
            const particleCount = isMobile ? 300 : 700;
            const particleGeo = new THREE.BufferGeometry();
            const positions = new Float32Array(particleCount * 3);
            const scales = new Float32Array(particleCount);

            for (let i = 0; i < particleCount * 3; i += 3) {
                positions[i] = (Math.random() - 0.5) * 80;
                positions[i + 1] = (Math.random() - 0.5) * 80;
                positions[i + 2] = (Math.random() - 0.5) * 60;
                scales[i / 3] = Math.random() * 1.5 + 0.5;
            }

            particleGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));

            const particleMat = new THREE.PointsMaterial({
                color: 0x93c5fd,
                size: 0.25,
                transparent: true,
                opacity: 0.7,
                blending: THREE.AdditiveBlending
            });

            const particleSystem = new THREE.Points(particleGeo, particleMat);
            scene.add(particleSystem);

            // Mouse Move & Parallax Controls
            let mouseX = 0;
            let mouseY = 0;
            let targetX = 0;
            let targetY = 0;

            window.addEventListener('mousemove', (e) => {
                mouseX = (e.clientX - window.innerWidth / 2) * 0.001;
                mouseY = (e.clientY - window.innerHeight / 2) * 0.001;
            });

            // Smooth Window Resizing
            window.addEventListener('resize', () => {
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
                renderer.setSize(window.innerWidth, window.innerHeight);
            });

            // Animation Loop
            let clock = new THREE.Clock();

            function animate3D() {
                requestAnimationFrame(animate3D);

                const elapsedTime = clock.getElapsedTime();

                // Smooth Parallax Interpolation
                targetX += (mouseX - targetX) * 0.05;
                targetY += (mouseY - targetY) * 0.05;

                if (!prefersReducedMotion) {
                    // Object Rotations
                    torusKnot.rotation.x = elapsedTime * 0.15;
                    torusKnot.rotation.y = elapsedTime * 0.2;

                    icoMesh.rotation.x = elapsedTime * -0.1;
                    icoMesh.rotation.y = elapsedTime * 0.25;

                    ringMesh.rotation.z = elapsedTime * 0.12;

                    octMesh1.rotation.y = elapsedTime * 0.3;
                    octMesh2.rotation.x = elapsedTime * 0.2;

                    // Particle Subtle Wave Movement
                    particleSystem.rotation.y = elapsedTime * 0.02;
                    particleSystem.rotation.x = elapsedTime * 0.01;

                    // Floating Vertical Oscillation
                    floatingGroup.position.y = Math.sin(elapsedTime * 0.8) * 0.8;
                }

                // Camera Reacts to Mouse Movement
                camera.position.x = targetX * 12;
                camera.position.y = -targetY * 12;
                camera.lookAt(0, 0, 0);

                renderer.render(scene, camera);
            }

            // Start 3D Loop
            animate3D();
        });
    </script>
</body>
</html
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <title>Pulso - O teu ginásio, a bater certo</title>
    <style>
        /* Cole aqui os estilos CSS globais (como a imagem de fundo) */
        body {
            background-color: #090d16;
            background-image: url('caminho-para-a-tua-imagem.jpg'); /* Substitui pelo caminho da tua imagem */
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            background-repeat: no-repeat;
            color: #ffffff;
            margin: 0;
            font-family: system-ui, -apple-system, sans-serif;
        }
        
        .bg-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.75);
            pointer-events: none;
            z-index: 0;
        }

        .content {
            position: relative;
            z-index: 10;
        }
    </style>
</head>
<body>
    <!-- Camada escura para manter o texto legível -->
    <div class="bg-overlay"></div>

    <!-- O conteúdo do teu site fica aqui dentro -->
    <div class="content">
        <h1>O teu ginásio, a bater certo</h1>
        <!-- Outros componentes do site -->
    </div>
</body>
</html>
