
<!DOCTYPE html>
<html lang="tr" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ATÖLYEMODA - 3D Studio v3.0 | Kıyafet Tasarım Stüdyosu</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome 6 Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700&family=Inter:wght@300;400;500;600;700;800&family=Montserrat:wght@700;800;900&family=Pacifico&family=Space+Grotesk:wght@500;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        studio: {
                            950: '#0b0e14',
                            900: '#11151c',
                            850: '#161b24',
                            800: '#1e2430',
                            700: '#2b3342',
                            600: '#3d475a',
                            border: '#262f3d',
                            card: '#151922',
                            cardHover: '#1c222e'
                        },
                        amberBrand: {
                            400: '#fbbf24',
                            500: '#f59e0b',
                            600: '#d97706',
                            700: '#b45309'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        heading: ['Montserrat', 'sans-serif'],
                        tech: ['Space Grotesk', 'sans-serif'],
                        luxury: ['Cinzel', 'serif'],
                        cursive: ['Pacifico', 'cursive']
                    }
                }
            }
        }
    </script>
    <style>
        /* 3D Perspective Studio Grid */
        .studio-viewport {
            perspective: 1200px;
        }

        .perspective-floor-grid {
            position: absolute;
            bottom: -65px;
            left: 50%;
            transform: translateX(-50%) perspective(480px) rotateX(68deg);
            width: 1000px;
            height: 520px;
            background-size: 38px 38px;
            background-image: 
                linear-gradient(to right, rgba(255, 255, 255, 0.07) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(255, 255, 255, 0.07) 1px, transparent 1px);
            mask-image: radial-gradient(ellipse 65% 55% at 50% 35%, rgba(0, 0, 0, 0.95) 15%, transparent 75%);
            -webkit-mask-image: radial-gradient(ellipse 65% 55% at 50% 35%, rgba(0, 0, 0, 0.95) 15%, transparent 75%);
            pointer-events: none;
            z-index: 1;
        }

        /* Spotlight illumination behind 3D model */
        .studio-spotlight {
            position: absolute;
            top: 42%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 680px;
            height: 680px;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.07) 0%, rgba(30, 41, 59, 0.03) 45%, transparent 72%);
            pointer-events: none;
            z-index: 1;
        }

        /* 3D Garment container with smooth interactive tilt */
        .garment-3d-stage {
            transform-style: preserve-3d;
            transition: transform 0.25s cubic-bezier(0.2, 0.8, 0.2, 1);
        }

        /* Custom scrollbar */
        .custom-scrollbar::-webkit-scrollbar {
            width: 5px;
            height: 5px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #11151c;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #2b3342;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #f59e0b;
        }

        /* Active Navigation Tab */
        .nav-tab.active {
            color: #f59e0b;
            border-bottom: 2px solid #f59e0b;
            background-color: rgba(245, 158, 11, 0.06);
        }

        /* Interactive Canvas Element */
        .design-element {
            cursor: move;
            user-select: none;
            touch-action: none;
            transition: outline 0.15s ease;
        }
        .design-element.selected {
            outline: 2px dashed #f59e0b;
            outline-offset: 4px;
            box-shadow: 0 0 12px rgba(245, 158, 11, 0.35);
        }

        /* Printable area dashed border matching the photo */
        .print-boundary {
            border: 1.5px dashed rgba(245, 158, 11, 0.45);
            background: rgba(245, 158, 11, 0.02);
            transition: all 0.2s ease;
        }
        .print-boundary:hover {
            border-color: rgba(245, 158, 11, 0.85);
            background: rgba(245, 158, 11, 0.04);
        }

        /* Resize Handles for Design Elements */
        .design-element .resize-handle {
            position: absolute;
            width: 12px;
            height: 12px;
            background: #f59e0b;
            border: 2px solid #0b0e14;
            border-radius: 3px;
            cursor: nwse-resize;
            display: none;
            z-index: 50;
            touch-action: none;
        }
        .design-element.selected .resize-handle {
            display: block;
        }
        .design-element .resize-handle.rh-tl { top: -6px; left: -6px; cursor: nwse-resize; }
        .design-element .resize-handle.rh-tr { top: -6px; right: -6px; cursor: nesw-resize; }
        .design-element .resize-handle.rh-bl { bottom: -6px; left: -6px; cursor: nesw-resize; }
        .design-element .resize-handle.rh-br { bottom: -6px; right: -6px; cursor: nwse-resize; }

        /* Product Card Active Highlight */
        .product-card.active {
            border-color: #f59e0b !important;
            box-shadow: 0 0 0 1px #f59e0b, 0 8px 24px -4px rgba(245, 158, 11, 0.35);
            background: linear-gradient(180deg, rgba(245, 158, 11, 0.12) 0%, rgba(21, 25, 34, 0.95) 100%) !important;
        }
    </style>
</head>
<body class="bg-studio-950 text-zinc-100 font-sans h-full flex flex-col antialiased selection:bg-amberBrand-500 selection:text-black overflow-hidden">

    <!-- Top Application Bar -->
    <header class="border-b border-studio-border bg-studio-900/90 backdrop-blur-md sticky top-0 z-40 shrink-0">
        <div class="w-full px-4 sm:px-6 h-16 flex items-center justify-between">
            <!-- Brand Logo with exact typography from photo -->
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-amber-600 to-amber-400 flex items-center justify-center text-white shadow-lg shadow-amber-500/25">
                    <i class="fa-solid fa-shirt text-xl"></i>
                </div>
                <div class="flex items-baseline space-x-2">
                    <span class="font-heading text-xl font-extrabold tracking-wider text-white">ATÖLYE<span class="text-amberBrand-500">MODA</span></span>
                    <span class="text-[11px] font-semibold bg-studio-800 text-zinc-400 px-2.5 py-0.5 rounded-md border border-studio-border">3D Studio v3.0</span>
                </div>
            </div>

            <!-- Top Right Action Controls from photo -->
            <div class="flex items-center space-x-2.5">
                <button id="btn-size-chart" class="px-3.5 py-2 text-xs font-semibold text-zinc-300 hover:text-white bg-studio-850 hover:bg-studio-800 border border-studio-border rounded-xl transition-all flex items-center gap-2 shadow-sm">
                    <i class="fa-solid fa-ruler-horizontal text-amberBrand-500"></i>
                    <span>Beden Rehberi</span>
                </button>
                <button id="btn-reset-design" class="px-3.5 py-2 text-xs font-semibold text-zinc-300 hover:text-rose-400 bg-studio-850 hover:bg-studio-800 border border-studio-border rounded-xl transition-all flex items-center gap-2 shadow-sm">
                    <i class="fa-solid fa-rotate-left"></i>
                    <span>Sıfırla</span>
                </button>
                <button id="btn-save-design" class="px-4 py-2 text-xs font-bold text-emerald-400 hover:text-emerald-300 bg-studio-850 hover:bg-studio-800 border border-studio-border rounded-xl transition-all flex items-center gap-2 shadow-sm">
                    <i class="fa-solid fa-download"></i>
                    <span>Tasarımı İndir</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Studio Layout -->
    <main class="flex-1 flex flex-col lg:flex-row overflow-hidden relative">

        <!-- LEFT PANEL: CUSTOMIZATION & GARMENT CARDS -->
        <aside class="w-full lg:w-[380px] bg-studio-900 border-r border-studio-border flex flex-col z-20 shrink-0 h-[46vh] lg:h-auto border-b lg:border-b-0">
            <!-- Navigation Tabs: Ürün, Renk, Yazı, Grafik, Desen -->
            <div class="flex border-b border-studio-border bg-studio-950/60 text-xs font-semibold overflow-x-auto custom-scrollbar shrink-0">
                <button class="nav-tab active flex-1 min-w-[70px] py-3 px-2 border-b-2 border-transparent text-zinc-400 hover:text-zinc-200 transition-all flex flex-col items-center gap-1.5" data-tab="garment">
                    <i class="fa-solid fa-shirt text-base text-amberBrand-500"></i>
                    <span>Ürün</span>
                </button>
                <button class="nav-tab flex-1 min-w-[70px] py-3 px-2 border-b-2 border-transparent text-zinc-400 hover:text-zinc-200 transition-all flex flex-col items-center gap-1.5" data-tab="color">
                    <i class="fa-solid fa-palette text-base"></i>
                    <span>Renk</span>
                </button>
                <button class="nav-tab flex-1 min-w-[70px] py-3 px-2 border-b-2 border-transparent text-zinc-400 hover:text-zinc-200 transition-all flex flex-col items-center gap-1.5" data-tab="text">
                    <i class="fa-solid fa-font text-base"></i>
                    <span>Yazı</span>
                </button>
                <button class="nav-tab flex-1 min-w-[70px] py-3 px-2 border-b-2 border-transparent text-zinc-400 hover:text-zinc-200 transition-all flex flex-col items-center gap-1.5" data-tab="graphics">
                    <i class="fa-solid fa-icons text-base"></i>
                    <span>Grafik</span>
                </button>
                <button class="nav-tab flex-1 min-w-[70px] py-3 px-2 border-b-2 border-transparent text-zinc-400 hover:text-zinc-200 transition-all flex flex-col items-center gap-1.5" data-tab="pattern">
                    <i class="fa-solid fa-chess-board text-base"></i>
                    <span>Desen</span>
                </button>
            </div>

            <!-- Tab Content Area -->
            <div class="flex-1 overflow-y-auto p-4 custom-scrollbar">
                
                <!-- TAB 1: PRODUCT CATALOG CARDS (Exact match to photo: 8 items, 2-column grid, detailed illustrations) -->
                <div id="tab-garment" class="tab-pane space-y-3.5">
                    <div class="flex items-center justify-between">
                        <h3 class="text-xs uppercase font-extrabold tracking-wider text-zinc-400">KIYAFET TİPİ SEÇİN</h3>
                        <span class="text-[11px] text-zinc-500">8 Model Mevcut</span>
                    </div>

                    <!-- 2-Column Product Grid matching user photo -->
                    <div class="grid grid-cols-2 gap-3" id="garment-cards-grid">
                        <!-- Generated dynamically by JS -->
                    </div>
                </div>

                <!-- TAB 2: FABRIC COLOR & TECHNIQUE -->
                <div id="tab-color" class="tab-pane hidden space-y-5">
                    <div>
                        <h3 class="text-xs uppercase font-extrabold tracking-wider text-zinc-400 mb-3">KUMAŞ RENGİ</h3>
                        <div class="grid grid-cols-6 gap-2.5" id="color-swatches">
                            <!-- Populated by JS -->
                        </div>
                    </div>

                    <div class="pt-4 border-t border-studio-border">
                        <label for="custom-color-input" class="text-xs font-semibold text-zinc-400 mb-2.5 block">Özel Renk Seçici</label>
                        <div class="flex items-center space-x-3 bg-studio-850 p-2.5 rounded-xl border border-studio-border">
                            <input type="color" id="custom-color-input" value="#32373d" class="w-10 h-10 rounded-lg border-0 bg-transparent cursor-pointer p-0">
                            <div class="flex flex-col">
                                <span class="text-[11px] text-zinc-400">HEX Kodu</span>
                                <span id="custom-color-code" class="text-sm font-mono font-bold text-white uppercase">#32373D</span>
                            </div>
                        </div>
                    </div>

                    <div class="pt-4 border-t border-studio-border">
                        <label class="text-xs font-semibold text-zinc-400 mb-2.5 block">Baskı / Dikiş Tekniği</label>
                        <div class="grid grid-cols-2 gap-2.5">
                            <button class="technique-btn active p-3 rounded-xl border border-amberBrand-500 bg-amberBrand-500/10 text-left transition-all" data-tech="print" data-price="0">
                                <div class="font-bold text-xs text-white">Dijital Baskı</div>
                                <div class="text-zinc-400 text-[10px] mt-0.5">Yüksek Çözünürlüklü</div>
                                <div class="text-amberBrand-400 text-[11px] font-bold mt-1">Ücretsiz</div>
                            </button>
                            <button class="technique-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all" data-tech="embroidery" data-price="45">
                                <div class="font-bold text-xs text-white">Nakış İşleme</div>
                                <div class="text-zinc-400 text-[10px] mt-0.5">Premium Kabartma</div>
                                <div class="text-amberBrand-400 text-[11px] font-bold mt-1">+45 ₺</div>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- TAB 3: CUSTOM TEXT CREATION -->
                <div id="tab-text" class="tab-pane hidden space-y-4">
                    <h3 class="text-xs uppercase font-extrabold tracking-wider text-zinc-400">ÖZEL YAZI EKLE</h3>
                    
                    <div class="space-y-3.5">
                        <div>
                            <label class="text-xs text-zinc-400 block mb-1.5 font-medium">Metniniz</label>
                            <input type="text" id="input-text-content" placeholder="Örn: VOGUE, MODA 2026..." class="w-full bg-studio-850 border border-studio-border rounded-xl px-3.5 py-2.5 text-sm text-white placeholder-zinc-500 focus:outline-none focus:border-amberBrand-500 transition-all">
                        </div>

                        <div class="grid grid-cols-2 gap-2.5">
                            <div>
                                <label class="text-xs text-zinc-400 block mb-1.5 font-medium">Font Stili</label>
                                <select id="select-font-family" class="w-full bg-studio-850 border border-studio-border rounded-xl px-3 py-2 text-xs text-white focus:outline-none focus:border-amberBrand-500">
                                    <option value="Inter">Modern (Sans-Serif)</option>
                                    <option value="Montserrat">Kalın & Bold</option>
                                    <option value="Space Grotesk">Tech / Sokak</option>
                                    <option value="Cinzel">Klasik & Lüks</option>
                                    <option value="Pacifico">El Yazısı (Vintage)</option>
                                </select>
                            </div>

                            <div>
                                <label class="text-xs text-zinc-400 block mb-1.5 font-medium">Yazı Rengi</label>
                                <div class="flex items-center space-x-2 bg-studio-850 border border-studio-border rounded-xl px-2.5 py-1">
                                    <input type="color" id="input-text-color" value="#ffffff" class="w-8 h-8 rounded-lg bg-transparent cursor-pointer p-0 border-0">
                                    <span id="text-color-hex" class="text-xs font-mono text-zinc-300">#FFF</span>
                                </div>
                            </div>
                        </div>

                        <div>
                            <div class="flex justify-between text-xs text-zinc-400 mb-1.5">
                                <span>Yazı Boyutu</span>
                                <span id="text-size-display" class="font-bold text-amberBrand-500">26px</span>
                            </div>
                            <input type="range" id="input-text-size" min="14" max="72" value="26" class="w-full accent-amberBrand-500 bg-studio-800">
                        </div>

                        <button id="btn-submit-text" class="w-full py-3 bg-gradient-to-r from-amberBrand-500 to-amber-600 hover:from-amberBrand-600 hover:to-amber-700 text-black font-extrabold rounded-xl text-xs transition-all shadow-lg shadow-amber-500/20 flex items-center justify-center gap-2">
                            <i class="fa-solid fa-plus"></i>
                            <span>Yazıyı Tasarıma Ekle</span>
                        </button>
                    </div>
                </div>

                <!-- TAB 4: GRAPHICS & UPLOAD -->
                <div id="tab-graphics" class="tab-pane hidden space-y-4">
                    <h3 class="text-xs uppercase font-extrabold tracking-wider text-zinc-400">GRAFİK & ETİKETLER</h3>
                    
                    <div class="grid grid-cols-4 gap-2" id="graphics-sticker-grid">
                        <!-- Populated by JS -->
                    </div>

                    <div class="pt-4 border-t border-studio-border">
                        <label class="text-xs font-semibold text-zinc-400 block mb-2">Kendi Logonu / Görselini Yükle</label>
                        <label class="flex flex-col items-center justify-center border-2 border-dashed border-studio-border hover:border-amberBrand-500 rounded-2xl p-4 cursor-pointer bg-studio-850/60 hover:bg-studio-850 transition-all text-center group">
                            <i class="fa-solid fa-cloud-arrow-up text-2xl text-amberBrand-500 group-hover:scale-110 transition-transform mb-2"></i>
                            <span class="text-xs text-zinc-200 font-semibold">Görsel Seç veya Sürükle</span>
                            <span class="text-[10px] text-zinc-500 mt-1">PNG, JPG, SVG veya WebP (Max 10MB)</span>
                            <input type="file" id="input-file-upload" accept="image/*" class="hidden">
                        </label>
                    </div>
                </div>

                <!-- TAB 5: FABRIC PATTERNS -->
                <div id="tab-pattern" class="tab-pane hidden space-y-4">
                    <div class="flex items-center justify-between">
                        <h3 class="text-xs uppercase font-extrabold tracking-wider text-zinc-400">KUMAŞ DESENİ</h3>
                        <span id="pattern-active-name" class="text-[11px] text-amberBrand-400 font-bold">Düz Kumaş</span>
                    </div>

                    <!-- 6 Realistic Pattern Presets -->
                    <div class="grid grid-cols-2 gap-2.5" id="pattern-selector-grid">
                        <button class="pattern-btn active p-3 rounded-xl border border-amberBrand-500 bg-amberBrand-500/10 text-left transition-all" data-pattern="none">
                            <div class="w-full h-11 bg-studio-800 rounded-lg mb-2 flex items-center justify-center text-zinc-400 border border-studio-border">
                                <i class="fa-solid fa-ban text-lg"></i>
                            </div>
                            <span class="text-xs font-bold text-white block">Düz Kumaş</span>
                            <span class="text-[10px] text-zinc-500">Desensiz Standart</span>
                        </button>

                        <button class="pattern-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all" data-pattern="stripes">
                            <div class="w-full h-11 rounded-lg mb-2 flex items-center justify-center text-[11px] font-mono font-bold text-white border border-studio-border" style="background: repeating-linear-gradient(45deg, #1e2430, #1e2430 6px, #3d475a 6px, #3d475a 12px);">
                                STRIPES
                            </div>
                            <span class="text-xs font-bold text-white block">Çizgili Stil</span>
                            <span class="text-[10px] text-zinc-500">Diyagonal Çizgi</span>
                        </button>

                        <button class="pattern-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all" data-pattern="camo">
                            <div class="w-full h-11 bg-emerald-950/60 rounded-lg mb-2 flex items-center justify-center text-[11px] font-mono font-bold text-emerald-400 border border-emerald-800/40">
                                CAMO
                            </div>
                            <span class="text-xs font-bold text-white block">Kamuflaj</span>
                            <span class="text-[10px] text-zinc-500">Urban Taktik</span>
                        </button>

                        <button class="pattern-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all" data-pattern="dots">
                            <div class="w-full h-11 rounded-lg mb-2 flex items-center justify-center text-[11px] font-mono font-bold text-amber-400 border border-amber-800/40" style="background: radial-gradient(#f59e0b 2px, #181e29 2px); background-size: 10px 10px;">
                                DOTS
                            </div>
                            <span class="text-xs font-bold text-white block">Puantiyeli</span>
                            <span class="text-[10px] text-zinc-500">Nokta Desen</span>
                        </button>

                        <button class="pattern-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all" data-pattern="plaid">
                            <div class="w-full h-11 rounded-lg mb-2 flex items-center justify-center text-[11px] font-mono font-bold text-indigo-300 border border-indigo-800/40" style="background: repeating-linear-gradient(0deg, #1e2330 0px, #1e2330 8px, #384566 8px, #384566 16px), repeating-linear-gradient(90deg, transparent 0px, transparent 8px, rgba(255,255,255,0.1) 8px, rgba(255,255,255,0.1) 16px);">
                                PLAID
                            </div>
                            <span class="text-xs font-bold text-white block">Ekose / Kareli</span>
                            <span class="text-[10px] text-zinc-500">Streetwear Flanel</span>
                        </button>

                        <button class="pattern-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all" data-pattern="houndstooth">
                            <div class="w-full h-11 rounded-lg mb-2 flex items-center justify-center text-[11px] font-mono font-bold text-rose-300 border border-rose-800/40 bg-studio-800">
                                PATTERN
                            </div>
                            <span class="text-xs font-bold text-white block">Kazayağı</span>
                            <span class="text-[10px] text-zinc-500">Lüks Geometrik</span>
                        </button>
                    </div>

                    <!-- Pattern Customization Controls (Color, Scale, Opacity) -->
                    <div id="pattern-adjustments" class="pt-3 border-t border-studio-border space-y-3.5">
                        <!-- Pattern Color -->
                        <div>
                            <label class="text-xs font-semibold text-zinc-400 mb-2 block">Desen Rengi</label>
                            <div class="flex items-center gap-2">
                                <div class="flex gap-1.5 flex-1" id="pattern-color-presets">
                                    <button data-pcolor="#ffffff" class="w-7 h-7 rounded-lg border-2 border-amberBrand-500 bg-white hover:scale-110 transition-transform shadow" title="Beyaz"></button>
                                    <button data-pcolor="#181b20" class="w-7 h-7 rounded-lg border border-studio-border bg-zinc-900 hover:scale-110 transition-transform" title="Siyah"></button>
                                    <button data-pcolor="#f59e0b" class="w-7 h-7 rounded-lg border border-studio-border bg-amber-500 hover:scale-110 transition-transform" title="Altın"></button>
                                    <button data-pcolor="#ef4444" class="w-7 h-7 rounded-lg border border-studio-border bg-red-500 hover:scale-110 transition-transform" title="Kırmızı"></button>
                                    <button data-pcolor="#3b82f6" class="w-7 h-7 rounded-lg border border-studio-border bg-blue-500 hover:scale-110 transition-transform" title="Mavi"></button>
                                    <button data-pcolor="#059669" class="w-7 h-7 rounded-lg border border-studio-border bg-emerald-600 hover:scale-110 transition-transform" title="Haki Yeşil"></button>
                                </div>
                                <input type="color" id="pattern-custom-color" value="#ffffff" class="w-8 h-8 rounded-lg bg-transparent cursor-pointer p-0 border-0 shrink-0" title="Özel Renk Seç">
                            </div>
                        </div>

                        <!-- Pattern Scale (Size) -->
                        <div>
                            <div class="flex justify-between items-center text-xs text-zinc-400 mb-1.5">
                                <span>Desen Boyutu / Sıklığı</span>
                                <span id="pattern-scale-label" class="text-amberBrand-400 font-bold">Standart</span>
                            </div>
                            <div class="grid grid-cols-3 gap-2">
                                <button class="pattern-scale-btn py-1.5 px-2 rounded-lg border border-studio-border bg-studio-850 text-xs font-semibold hover:border-zinc-500 transition-all text-zinc-300" data-scale="0.7">İnce</button>
                                <button class="pattern-scale-btn active py-1.5 px-2 rounded-lg border border-amberBrand-500 bg-amberBrand-500/10 text-xs font-bold text-white transition-all" data-scale="1.0">Standart</button>
                                <button class="pattern-scale-btn py-1.5 px-2 rounded-lg border border-studio-border bg-studio-850 text-xs font-semibold hover:border-zinc-500 transition-all text-zinc-300" data-scale="1.5">Geniş</button>
                            </div>
                        </div>

                        <!-- Pattern Opacity -->
                        <div>
                            <div class="flex justify-between items-center text-xs text-zinc-400 mb-1.5">
                                <span>Desen Belirginliği</span>
                                <span id="pattern-opacity-label" class="text-amberBrand-400 font-bold">40%</span>
                            </div>
                            <input type="range" id="pattern-opacity-slider" min="15" max="85" value="40" class="w-full accent-amberBrand-500 bg-studio-800 h-1.5">
                        </div>
                    </div>
                </div>

            </div>
        </aside>

        <!-- CENTER PANEL: 3D REALISTIC WORKSPACE WITH PERSPECTIVE FLOOR GRID -->
        <section class="flex-1 bg-studio-950 flex flex-col items-center justify-center p-4 sm:p-8 relative min-h-[400px] overflow-hidden studio-viewport" id="viewport-stage">
            
            <!-- Perspective 3D Floor Grid matching user photo -->
            <div class="perspective-floor-grid"></div>

            <!-- Soft Studio Spotlight behind garment -->
            <div class="studio-spotlight"></div>

            <!-- Selected Element Action Bar (Sil, Renk, Boyut) -->
            <div id="element-action-bar" class="absolute top-5 right-5 z-30 hidden bg-studio-900/95 border border-studio-border px-3.5 py-2.5 rounded-2xl shadow-2xl backdrop-blur-md" style="min-width:280px;">
                <div class="flex items-center justify-between mb-2">
                    <span class="text-zinc-400 font-bold text-xs uppercase tracking-wider">Seçili Öğe</span>
                    <button id="btn-delete-active" class="px-2.5 py-1.5 bg-rose-500/20 text-rose-400 hover:bg-rose-500 hover:text-white rounded-lg transition-all flex items-center gap-1.5 font-bold text-xs">
                        <i class="fa-solid fa-trash-can"></i>
                        <span>Sil</span>
                    </button>
                </div>

                <!-- Element Color Picker -->
                <div class="flex items-center gap-2.5 mb-2 pb-2 border-b border-studio-border/60">
                    <span class="text-[11px] text-zinc-400 font-semibold whitespace-nowrap"><i class="fa-solid fa-droplet mr-1 text-amberBrand-500"></i>Renk:</span>
                    <input type="color" id="element-color-picker" value="#f59e0b" class="w-7 h-7 rounded-lg bg-transparent cursor-pointer p-0 border-0 shrink-0">
                    <div class="flex gap-1 flex-wrap" id="element-quick-colors">
                        <button data-qc="#ffffff" class="w-5 h-5 rounded-md border border-studio-border bg-white hover:scale-110 transition-transform"></button>
                        <button data-qc="#000000" class="w-5 h-5 rounded-md border border-studio-border bg-black hover:scale-110 transition-transform"></button>
                        <button data-qc="#f59e0b" class="w-5 h-5 rounded-md border border-studio-border bg-amber-500 hover:scale-110 transition-transform"></button>
                        <button data-qc="#ef4444" class="w-5 h-5 rounded-md border border-studio-border bg-red-500 hover:scale-110 transition-transform"></button>
                        <button data-qc="#3b82f6" class="w-5 h-5 rounded-md border border-studio-border bg-blue-500 hover:scale-110 transition-transform"></button>
                        <button data-qc="#22c55e" class="w-5 h-5 rounded-md border border-studio-border bg-green-500 hover:scale-110 transition-transform"></button>
                        <button data-qc="#a855f7" class="w-5 h-5 rounded-md border border-studio-border bg-purple-500 hover:scale-110 transition-transform"></button>
                        <button data-qc="#ec4899" class="w-5 h-5 rounded-md border border-studio-border bg-pink-500 hover:scale-110 transition-transform"></button>
                    </div>
                </div>

                <!-- Element Size Slider -->
                <div class="flex items-center gap-2.5">
                    <span class="text-[11px] text-zinc-400 font-semibold whitespace-nowrap"><i class="fa-solid fa-up-right-and-down-left-from-center mr-1 text-amberBrand-500"></i>Boyut:</span>
                    <input type="range" id="element-size-slider" min="12" max="160" value="42" class="flex-1 accent-amberBrand-500 bg-studio-800 h-1.5">
                    <span id="element-size-label" class="text-[11px] text-amberBrand-400 font-bold w-8 text-right">42</span>
                </div>
            </div>

            <!-- 3D Garment Stage Container with interactive tilt -->
            <div class="relative w-full max-w-[460px] aspect-square flex items-center justify-center select-none garment-3d-stage z-10" id="garment-stage">
                
                <!-- Realistic 3D Garment Vector Engine (SVG) -->
                <svg id="garment-vector-canvas" class="w-full h-full filter drop-shadow-[0_25px_35px_rgba(0,0,0,0.85)] transition-all duration-300" viewBox="0 0 600 600" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <!-- SVG Defs & Shading Layers injected dynamically -->
                </svg>

                <!-- Interactive Printable Area Overlay (aligned with chest & torso) -->
                <div id="garment-print-box" class="print-boundary absolute w-[40%] h-[50%] top-[25%] left-[30%] rounded-xl overflow-hidden flex items-center justify-center pointer-events-auto">
                    <!-- Front design layer -->
                    <div id="design-layer-front" class="w-full h-full relative"></div>
                    <!-- Back design layer -->
                    <div id="design-layer-back" class="w-full h-full relative hidden"></div>
                </div>
            </div>

            <!-- Center Bottom Controls matching user photo: Hint Badge & View Switch Toggle -->
            <div class="absolute bottom-6 flex flex-col items-center gap-2.5 z-20 pointer-events-auto">
                
                <!-- Hint Badge from photo: "🔥 Öğeleri sürükleyip boyutlandırabilirsiniz" -->
                <div class="bg-studio-900/90 border border-studio-border/80 px-4 py-1.5 rounded-full shadow-xl flex items-center gap-2 text-xs font-semibold text-zinc-300 backdrop-blur-md">
                    <i class="fa-solid fa-fire text-amberBrand-500"></i>
                    <span>Öğeleri sürükleyip boyutlandırabilirsiniz</span>
                </div>

                <!-- Pill Switch Toggle from photo: [ Ön Yüz ] [ Arka Yüz ] -->
                <div class="bg-studio-900/90 border border-studio-border p-1 rounded-2xl shadow-2xl flex items-center space-x-1 backdrop-blur-md">
                    <button id="toggle-view-front" class="px-5 py-2 text-xs font-extrabold rounded-xl bg-amberBrand-500 text-black transition-all shadow-md shadow-amber-500/20">
                        Ön Yüz
                    </button>
                    <button id="toggle-view-back" class="px-5 py-2 text-xs font-semibold rounded-xl text-zinc-400 hover:text-white transition-all">
                        Arka Yüz
                    </button>
                </div>

            </div>

        </section>

        <!-- RIGHT PANEL: SIZE SELECTOR, DESIGN SUMMARY & CHECKOUT CTA -->
        <aside class="w-full lg:w-80 bg-studio-900 border-l border-studio-border p-5 flex flex-col justify-between z-20 shrink-0">
            <div class="space-y-6">
                
                <!-- Beden Seçimi matching photo -->
                <div>
                    <div class="flex justify-between items-center mb-2.5">
                        <label class="text-xs uppercase font-extrabold tracking-wider text-zinc-400">BEDEN SEÇİMİ</label>
                        <span id="stock-badge" class="text-[10px] text-emerald-400 font-bold bg-emerald-950/70 border border-emerald-800/40 px-2 py-0.5 rounded-md">Stokta Var</span>
                    </div>
                    <div class="grid grid-cols-5 gap-1.5" id="size-button-group">
                        <button class="size-pill py-2.5 text-xs font-bold rounded-xl border border-studio-border bg-studio-850 text-zinc-300 hover:border-zinc-500 transition-all" data-size="S">S</button>
                        <button class="size-pill active py-2.5 text-xs font-bold rounded-xl border border-amberBrand-500 bg-amberBrand-500 text-black transition-all shadow-md shadow-amber-500/20" data-size="M">M</button>
                        <button class="size-pill py-2.5 text-xs font-bold rounded-xl border border-studio-border bg-studio-850 text-zinc-300 hover:border-zinc-500 transition-all" data-size="L">L</button>
                        <button class="size-pill py-2.5 text-xs font-bold rounded-xl border border-studio-border bg-studio-850 text-zinc-300 hover:border-zinc-500 transition-all" data-size="XL">XL</button>
                        <button class="size-pill py-2.5 text-xs font-bold rounded-xl border border-studio-border bg-studio-850 text-zinc-300 hover:border-zinc-500 transition-all" data-size="2XL">2XL</button>
                    </div>
                </div>

                <!-- Tasarım Özeti matching photo -->
                <div class="border-t border-studio-border pt-4 space-y-3">
                    <h4 class="text-xs uppercase font-extrabold tracking-wider text-zinc-400">TASARIM ÖZETİ</h4>
                    
                    <div class="flex justify-between text-xs">
                        <span class="text-zinc-500">Seçilen Ürün:</span>
                        <span id="summary-product-name" class="font-bold text-white">Oversized Tişört</span>
                    </div>
                    <div class="flex justify-between text-xs">
                        <span class="text-zinc-500">Teknik:</span>
                        <span id="summary-technique-name" class="font-semibold text-zinc-200">Dijital Baskı</span>
                    </div>
                    <div class="flex justify-between text-xs">
                        <span class="text-zinc-500">Ekstra Detaylar:</span>
                        <span id="summary-extras-count" class="font-semibold text-zinc-400">Yok</span>
                    </div>
                </div>

                <!-- Price Breakdown matching photo -->
                <div class="border-t border-studio-border pt-4 space-y-2.5 bg-studio-950/60 p-4 rounded-2xl border border-studio-border">
                    <div class="flex justify-between text-xs text-zinc-400">
                        <span>Taban Ürün Fiyatı:</span>
                        <span id="summary-base-price" class="font-semibold text-zinc-200">350 ₺</span>
                    </div>
                    <div class="flex justify-between text-xs text-zinc-400">
                        <span>Baskı / Özelleştirme:</span>
                        <span id="summary-custom-price" class="font-semibold text-zinc-200">0 ₺</span>
                    </div>
                    <div class="border-t border-studio-border pt-3 flex justify-between items-baseline">
                        <span class="text-xs font-extrabold text-white uppercase tracking-wider">Toplam Tutar:</span>
                        <div class="text-right">
                            <span id="summary-total-price" class="text-2xl font-black text-amberBrand-500">350 ₺</span>
                            <span class="block text-[10px] text-zinc-500 mt-0.5">KDV Dahil / Ücretsiz Kargo</span>
                        </div>
                    </div>
                </div>

            </div>

            <!-- Big Bottom CTA Button matching photo: "🔒 Siparişi Tamamla" -->
            <div class="pt-4 border-t border-studio-border">
                <button id="btn-checkout-cta" class="w-full py-4 bg-gradient-to-r from-amberBrand-500 to-amber-600 hover:from-amber-400 hover:to-amber-500 text-black font-black rounded-2xl text-sm shadow-xl shadow-amber-500/25 transition-all transform active:scale-[0.98] flex items-center justify-center gap-2">
                    <i class="fa-solid fa-lock text-base"></i>
                    <span>Siparişi Tamamla</span>
                </button>
            </div>
        </aside>

    </main>

    <!-- MODAL 1: CHECKOUT MODAL -->
    <div id="modal-checkout-window" class="fixed inset-0 bg-black/85 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-studio-900 border border-studio-border rounded-3xl max-w-lg w-full p-6 space-y-5 relative shadow-2xl animate-fade-in">
            <button class="modal-dismiss absolute top-5 right-5 text-zinc-400 hover:text-white text-lg">
                <i class="fa-solid fa-xmark"></i>
            </button>

            <div class="flex items-center space-x-3.5">
                <div class="w-11 h-11 rounded-2xl bg-amberBrand-500/20 text-amberBrand-500 flex items-center justify-center text-xl">
                    <i class="fa-solid fa-circle-check"></i>
                </div>
                <div>
                    <h3 class="text-lg font-black text-white">Tasarımınız Hazır!</h3>
                    <p class="text-xs text-zinc-400">Özel tasarımınız atölyemizde özenle üretilecek.</p>
                </div>
            </div>

            <div class="bg-studio-950 p-4 rounded-2xl border border-studio-border space-y-2.5 text-xs">
                <div class="flex justify-between text-zinc-300">
                    <span>Kıyafet:</span>
                    <span id="modal-item-title" class="font-bold text-white">-</span>
                </div>
                <div class="flex justify-between text-zinc-300">
                    <span>Beden:</span>
                    <span id="modal-item-size" class="font-bold text-white">M</span>
                </div>
                <div class="flex justify-between text-zinc-300">
                    <span>Kumaş Rengi:</span>
                    <div class="flex items-center gap-2">
                        <span id="modal-color-preview" class="w-3.5 h-3.5 rounded-full border border-zinc-600 inline-block"></span>
                        <span id="modal-item-color" class="font-bold text-white">-</span>
                    </div>
                </div>
                <div class="flex justify-between text-zinc-300 border-t border-studio-border pt-2.5">
                    <span class="text-sm font-bold text-white">Ödenecek Tutar:</span>
                    <span id="modal-item-total" class="text-lg font-black text-amberBrand-500">0 ₺</span>
                </div>
            </div>

            <form id="form-order" class="space-y-3">
                <div>
                    <label class="text-xs text-zinc-400 block mb-1 font-medium">Ad Soyad</label>
                    <input type="text" required placeholder="Adınız Soyadınız" class="w-full bg-studio-850 border border-studio-border rounded-xl px-3.5 py-2.5 text-xs text-white focus:outline-none focus:border-amberBrand-500">
                </div>
                <div class="grid grid-cols-2 gap-2.5">
                    <div>
                        <label class="text-xs text-zinc-400 block mb-1 font-medium">E-Posta</label>
                        <input type="email" required placeholder="ornek@mail.com" class="w-full bg-studio-850 border border-studio-border rounded-xl px-3.5 py-2.5 text-xs text-white focus:outline-none focus:border-amberBrand-500">
                    </div>
                    <div>
                        <label class="text-xs text-zinc-400 block mb-1 font-medium">Telefon</label>
                        <input type="tel" required placeholder="05xx xxx xx xx" class="w-full bg-studio-850 border border-studio-border rounded-xl px-3.5 py-2.5 text-xs text-white focus:outline-none focus:border-amberBrand-500">
                    </div>
                </div>
                <div>
                    <label class="text-xs text-zinc-400 block mb-1 font-medium">Teslimat Adresi</label>
                    <textarea rows="2" required placeholder="Tam teslimat adresiniz..." class="w-full bg-studio-850 border border-studio-border rounded-xl px-3.5 py-2 text-xs text-white focus:outline-none focus:border-amberBrand-500"></textarea>
                </div>

                <button type="submit" class="w-full py-3.5 bg-gradient-to-r from-amberBrand-500 to-amber-600 hover:from-amber-400 hover:to-amber-500 text-black font-black rounded-xl text-sm transition-all shadow-lg shadow-amber-500/25 mt-2">
                    Siparişi Onayla & Üretime Başla
                </button>
            </form>
        </div>
    </div>

    <!-- MODAL 2: SIZE CHART MODAL -->
    <div id="modal-size-chart-window" class="fixed inset-0 bg-black/85 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-studio-900 border border-studio-border rounded-3xl max-w-md w-full p-6 space-y-4 relative shadow-2xl">
            <button class="modal-dismiss absolute top-5 right-5 text-zinc-400 hover:text-white text-lg">
                <i class="fa-solid fa-xmark"></i>
            </button>

            <h3 class="text-base font-black text-white flex items-center gap-2">
                <i class="fa-solid fa-ruler-combined text-amberBrand-500"></i>
                <span>Beden Ölçü Tablosu (cm)</span>
            </h3>

            <div class="overflow-x-auto rounded-xl border border-studio-border">
                <table class="w-full text-xs text-left text-zinc-300 border-collapse">
                    <thead class="bg-studio-850 text-zinc-400 uppercase text-[10px] font-bold">
                        <tr>
                            <th class="p-3 border-b border-studio-border">Beden</th>
                            <th class="p-3 border-b border-studio-border">Göğüs (En)</th>
                            <th class="p-3 border-b border-studio-border">Boy</th>
                            <th class="p-3 border-b border-studio-border">Omuz</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y divide-studio-border/60">
                        <tr>
                            <td class="p-3 font-bold text-amberBrand-500">S</td>
                            <td class="p-3">52 cm</td>
                            <td class="p-3">71 cm</td>
                            <td class="p-3">48 cm</td>
                        </tr>
                        <tr class="bg-studio-850/40">
                            <td class="p-3 font-bold text-amberBrand-500">M</td>
                            <td class="p-3">55 cm</td>
                            <td class="p-3">73 cm</td>
                            <td class="p-3">50 cm</td>
                        </tr>
                        <tr>
                            <td class="p-3 font-bold text-amberBrand-500">L</td>
                            <td class="p-3">58 cm</td>
                            <td class="p-3">75 cm</td>
                            <td class="p-3">52 cm</td>
                        </tr>
                        <tr class="bg-studio-850/40">
                            <td class="p-3 font-bold text-amberBrand-500">XL</td>
                            <td class="p-3">61 cm</td>
                            <td class="p-3">77 cm</td>
                            <td class="p-3">55 cm</td>
                        </tr>
                        <tr>
                            <td class="p-3 font-bold text-amberBrand-500">2XL</td>
                            <td class="p-3">65 cm</td>
                            <td class="p-3">79 cm</td>
                            <td class="p-3">58 cm</td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <p class="text-[11px] text-zinc-500 italic">
                * Ölçüler AtölyeModa rahat kesim (oversize) kalıplarımıza aittir. Rahat salaş görünüm için standart bedeninizi tercih ediniz.
            </p>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="studio-toast" class="fixed bottom-6 right-6 bg-studio-850 border border-studio-border text-white px-4 py-3 rounded-2xl shadow-2xl flex items-center space-x-3 transition-all transform translate-y-24 opacity-0 z-50">
        <i id="toast-icon-status" class="fa-solid fa-circle-check text-amberBrand-500 text-base"></i>
        <span id="toast-text-status" class="text-xs font-semibold">İşlem başarıyla tamamlandı</span>
    </div>

    <!-- JAVASCRIPT STUDIO ENGINE -->
    <script>
        // --- Catalog Items matching the 8 cards in user photo ---
        const CATALOG_ITEMS = [
            {
                id: 'card-1',
                type: 'tshirt',
                name: 'Oversized Tişört',
                color: '#32373d', // Dark gray / Anthracite
                price: 350,
                selected: true
            },
            {
                id: 'card-2',
                type: 'hoodie',
                name: 'Kapüşonlu Sweatshirt',
                color: '#2e343b', // Slate dark hoodie
                price: 650,
                selected: false
            },
            {
                id: 'card-3',
                type: 'tshirt',
                name: 'Oversized Tişört',
                color: '#00a8ff', // Vibrant cyan / blue
                price: 350,
                selected: false
            },
            {
                id: 'card-4',
                type: 'hoodie',
                name: 'Kapüşonlu Sweatshirt',
                color: '#fbc531', // Vibrant yellow hoodie
                price: 650,
                selected: false
            },
            {
                id: 'card-5',
                type: 'tshirt',
                name: 'Oversized Tişört',
                color: '#f8c291', // Peach / Beige t-shirt
                price: 350,
                selected: false
            },
            {
                id: 'card-6',
                type: 'hoodie',
                name: 'Kapüşonlu Sweatshirt',
                color: '#2563eb', // Royal blue hoodie
                price: 650,
                selected: false
            },
            {
                id: 'card-7',
                type: 'classic-tshirt',
                name: 'Mayon Tişört',
                color: '#f8fafc', // Crisp white classic t-shirt
                price: 320,
                selected: false
            },
            {
                id: 'card-8',
                type: 'hoodie',
                name: 'Kapüşonlu Sweatshirt',
                color: '#d1fae5', // Mint / soft light green hoodie
                price: 650,
                selected: false
            }
        ];

        const COLOR_SWATCHES = [
            '#121417', '#2c323d', '#4b5563', '#fcfcfd',
            '#e8dec8', '#0099e5', '#1e40af', '#0f172a',
            '#f59e0b', '#dc2626', '#881337', '#059669',
            '#3f4f34', '#7c3aed', '#ec4899', '#f8c291',
            '#a7f3d0', '#38bdf8'
        ];

        const STICKER_LIST = [
            'fa-fire', 'fa-bolt', 'fa-crown', 'fa-dragon',
            'fa-skull', 'fa-star', 'fa-heart', 'fa-gem',
            'fa-cat', 'fa-compass', 'fa-feather', 'fa-face-smile'
        ];

        // --- Application State ---
        let studioState = {
            activeCardId: 'card-1',
            activeType: 'tshirt',
            activeColor: '#32373d',
            activeSize: 'M',
            activePattern: 'none',
            patternColor: '#ffffff',
            patternScale: 1.0,
            patternOpacity: 0.40,
            currentView: 'front', // 'front' | 'back'
            techniquePrice: 0,
            techniqueName: 'Dijital Baskı',
            selectedElement: null,
            elementsCount: 0
        };

        // --- DOM Cache ---
        const svgCanvas = document.getElementById('garment-vector-canvas');
        const printBox = document.getElementById('garment-print-box');
        const layerFront = document.getElementById('design-layer-front');
        const layerBack = document.getElementById('design-layer-back');
        const actionControls = document.getElementById('element-action-bar');
        const garmentStage = document.getElementById('garment-stage');
        const viewportStage = document.getElementById('viewport-stage');

        // --- Startup Initialization ---
        window.addEventListener('DOMContentLoaded', () => {
            renderCardCatalog();
            renderColorSwatches();
            renderStickerGrid();
            render3DGarment();
            initInteractiveTilt();
            initEventHandlers();
            recalculateSummary();
        });

        // --- Render the 8 Product Selection Cards with realistic micro-illustrations ---
        function renderCardCatalog() {
            const container = document.getElementById('garment-cards-grid');
            container.innerHTML = '';

            CATALOG_ITEMS.forEach(item => {
                const isSelected = item.id === studioState.activeCardId;
                const card = document.createElement('div');
                card.className = `product-card cursor-pointer rounded-2xl p-3 border transition-all duration-200 flex flex-col items-center justify-between h-44 group ${
                    isSelected ? 'active border-amberBrand-500' : 'border-studio-border bg-studio-card hover:bg-studio-cardHover hover:border-zinc-600'
                }`;
                card.onclick = () => selectCatalogItem(item);

                // SVG mini-illustration based on type & color
                const miniSvg = generateMiniatureSvg(item.type, item.color);

                card.innerHTML = `
                    <div class="w-full flex-1 flex items-center justify-center p-1 pointer-events-none group-hover:scale-105 transition-transform">
                        ${miniSvg}
                    </div>
                    <div class="w-full text-center mt-2 pt-2 border-t border-studio-border/50">
                        <span class="text-xs font-bold text-white block truncate">${item.name}</span>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // --- Generate Mini SVG for Cards ---
        function generateMiniatureSvg(type, color) {
            const isWhite = color === '#f8fafc' || color === '#ffffff';
            const strokeColor = isWhite ? '#cbd5e1' : '#1e293b';

            if (type === 'hoodie') {
                return `
                <svg viewBox="0 0 120 120" class="w-24 h-24 filter drop-shadow-md">
                    <!-- Base Hoodie Body -->
                    <path d="M35 30 L45 18 C55 14 65 14 75 18 L85 30 L102 52 L88 58 L86 102 L34 102 L32 58 L18 52 Z" fill="${color}" stroke="${strokeColor}" stroke-width="2"/>
                    <!-- Hood Neck Opening & Folds -->
                    <path d="M46 20 C46 36 74 36 74 20" fill="none" stroke="${strokeColor}" stroke-width="2"/>
                    <path d="M48 24 C52 42 68 42 72 24" fill="rgba(0,0,0,0.22)"/>
                    <!-- Drawstrings -->
                    <path d="M54 34 L54 52 M66 34 L66 52" stroke="#ffffff" stroke-width="1.8" stroke-linecap="round"/>
                    <!-- Kangaroo Pocket -->
                    <path d="M42 74 L78 74 L84 94 L36 94 Z" fill="none" stroke="${strokeColor}" stroke-width="1.8"/>
                    <!-- Shading Highlights -->
                    <path d="M35 30 L85 30" stroke="rgba(255,255,255,0.25)" stroke-width="1.5"/>
                    <path d="M36 58 C38 75 38 90 36 102" stroke="rgba(0,0,0,0.18)" stroke-width="2"/>
                    <path d="M84 58 C82 75 82 90 84 102" stroke="rgba(0,0,0,0.18)" stroke-width="2"/>
                </svg>`;
            } else if (type === 'classic-tshirt') {
                return `
                <svg viewBox="0 0 120 120" class="w-24 h-24 filter drop-shadow-md">
                    <!-- Base Fitted T-Shirt -->
                    <path d="M36 28 L48 22 C56 26 64 26 72 22 L84 28 L98 46 L86 52 L84 100 L36 100 L34 52 L22 46 Z" fill="${color}" stroke="${strokeColor}" stroke-width="2"/>
                    <!-- Crew Neck -->
                    <path d="M48 22 C52 30 68 30 72 22" fill="none" stroke="${strokeColor}" stroke-width="2.2"/>
                    <path d="M50 23 C54 28 66 28 70 23" fill="rgba(0,0,0,0.18)"/>
                    <!-- Sleeve & Body folds -->
                    <path d="M36 50 C40 70 40 85 38 98" stroke="rgba(0,0,0,0.12)" stroke-width="2"/>
                    <path d="M84 50 C80 70 80 85 82 98" stroke="rgba(0,0,0,0.12)" stroke-width="2"/>
                </svg>`;
            } else {
                // Oversized T-Shirt
                return `
                <svg viewBox="0 0 120 120" class="w-24 h-24 filter drop-shadow-md">
                    <!-- Oversized Boxy T-Shirt Silhouette -->
                    <path d="M34 26 L46 20 C55 24 65 24 74 20 L86 26 L102 48 L88 54 L86 102 L34 102 L32 54 L18 48 Z" fill="${color}" stroke="${strokeColor}" stroke-width="2"/>
                    <!-- Deep Crew Rib Neck -->
                    <path d="M46 20 C50 31 70 31 74 20" fill="none" stroke="${strokeColor}" stroke-width="2.5"/>
                    <path d="M48 21 C52 28 68 28 72 21" fill="rgba(0,0,0,0.25)"/>
                    <!-- Shoulder highlight -->
                    <path d="M34 26 L46 20 M74 20 L86 26" stroke="rgba(255,255,255,0.3)" stroke-width="1.8"/>
                    <!-- Side 3D Folds -->
                    <path d="M35 52 C40 72 40 88 36 100" stroke="rgba(0,0,0,0.18)" stroke-width="2.5"/>
                    <path d="M85 52 C80 72 80 88 84 100" stroke="rgba(0,0,0,0.18)" stroke-width="2.5"/>
                </svg>`;
            }
        }

        // --- Select Catalog Item ---
        function selectCatalogItem(item) {
            studioState.activeCardId = item.id;
            studioState.activeType = item.type;
            studioState.activeColor = item.color;

            // Sync color picker UI
            document.getElementById('custom-color-input').value = item.color;
            document.getElementById('custom-color-code').textContent = item.color.toUpperCase();
            
            // Adjust printable box position slightly for hoodie vs tshirt
            if (item.type === 'hoodie') {
                printBox.style.top = '27%';
                printBox.style.height = '42%';
            } else {
                printBox.style.top = '25%';
                printBox.style.height = '50%';
            }

            renderCardCatalog();
            renderColorSwatches();
            render3DGarment();
            recalculateSummary();
        }

        // --- Render Color Swatches ---
        function renderColorSwatches() {
            const container = document.getElementById('color-swatches');
            container.innerHTML = '';

            COLOR_SWATCHES.forEach(hex => {
                const btn = document.createElement('button');
                const isSelected = hex.toLowerCase() === studioState.activeColor.toLowerCase();
                btn.className = `w-9 h-9 rounded-xl border-2 transition-all transform hover:scale-110 flex items-center justify-center ${
                    isSelected ? 'border-amberBrand-500 scale-105 shadow-md shadow-amber-500/25' : 'border-studio-border hover:border-zinc-400'
                }`;
                btn.style.backgroundColor = hex;
                btn.onclick = () => {
                    studioState.activeColor = hex;
                    document.getElementById('custom-color-input').value = hex;
                    document.getElementById('custom-color-code').textContent = hex.toUpperCase();
                    renderColorSwatches();
                    render3DGarment();
                    recalculateSummary();
                };

                if (isSelected) {
                    const checkColor = (hex === '#f8fafc' || hex === '#ffffff' || hex === '#d1fae5') ? '#000000' : '#ffffff';
                    btn.innerHTML = `<i class="fa-solid fa-check text-xs" style="color: ${checkColor}"></i>`;
                }
                container.appendChild(btn);
            });
        }

        // --- Render Stickers in Tab 4 ---
        function renderStickerGrid() {
            const container = document.getElementById('graphics-sticker-grid');
            container.innerHTML = '';

            STICKER_LIST.forEach(icon => {
                const btn = document.createElement('button');
                btn.className = 'h-14 rounded-2xl bg-studio-850 border border-studio-border hover:border-amberBrand-500 flex items-center justify-center text-2xl text-zinc-300 hover:text-amberBrand-500 transition-all hover:scale-105';
                btn.innerHTML = `<i class="fa-solid ${icon}"></i>`;
                btn.onclick = () => addIconElement(icon);
                container.appendChild(btn);
            });
        }

        // --- Render 3D Realistic Garment (Ultra-realistic SVG with Multi-stop Shading & Texture) ---
        // --- Helper to test if a color is light or dark for optimal contrast ---
        function isColorLight(hex) {
            if (!hex || typeof hex !== 'string') return false;
            let c = hex.replace('#', '');
            if (c.length === 3) c = c[0] + c[0] + c[1] + c[1] + c[2] + c[2];
            if (c.length !== 6) return false;
            const r = parseInt(c.substring(0, 2), 16);
            const g = parseInt(c.substring(2, 4), 16);
            const b = parseInt(c.substring(4, 6), 16);
            const brightness = (r * 299 + g * 587 + b * 114) / 1000;
            return brightness > 155;
        }

        // --- Pattern SVG Generator with True Garment Conformance ---
        function generatePatternDefs(pattern, patternColor, scale) {
            if (!pattern || pattern === 'none') return '';
            const s = scale || 1.0;

            if (pattern === 'stripes') {
                const size = 18 * s;
                const width = 4 * s;
                return `
                <pattern id="activeFabricPattern" width="${size}" height="${size}" patternTransform="rotate(45 0 0)" patternUnits="userSpaceOnUse">
                    <line x1="0" y1="0" x2="0" y2="${size}" stroke="${patternColor}" stroke-width="${width}" />
                </pattern>`;
            } else if (pattern === 'dots') {
                const size = 16 * s;
                const r = 2.6 * s;
                return `
                <pattern id="activeFabricPattern" width="${size}" height="${size}" patternUnits="userSpaceOnUse">
                    <circle cx="${size/2}" cy="${size/2}" r="${r}" fill="${patternColor}" />
                </pattern>`;
            } else if (pattern === 'camo') {
                const size = 64 * s;
                return `
                <pattern id="activeFabricPattern" width="${size}" height="${size}" patternUnits="userSpaceOnUse">
                    <rect width="${size}" height="${size}" fill="transparent"/>
                    <path d="M${10*s} ${12*s} Q${26*s} ${4*s} ${38*s} ${16*s} T${48*s} ${36*s} T${22*s} ${46*s} Z" fill="${patternColor}" opacity="0.6"/>
                    <path d="M${32*s} ${28*s} Q${50*s} ${18*s} ${58*s} ${34*s} T${40*s} ${54*s} T${16*s} ${50*s} Z" fill="${patternColor}" opacity="0.35"/>
                    <circle cx="${20*s}" cy="${20*s}" r="${8*s}" fill="${patternColor}" opacity="0.45"/>
                    <circle cx="${45*s}" cy="${48*s}" r="${6*s}" fill="#000000" opacity="0.25"/>
                    <path d="M${2*s} ${42*s} Q${14*s} ${32*s} ${28*s} ${44*s} Z" fill="#000000" opacity="0.3"/>
                </pattern>`;
            } else if (pattern === 'plaid') {
                const size = 32 * s;
                const thick = 8 * s;
                return `
                <pattern id="activeFabricPattern" width="${size}" height="${size}" patternUnits="userSpaceOnUse">
                    <rect width="${size}" height="${size}" fill="transparent"/>
                    <rect x="0" y="0" width="${size}" height="${thick}" fill="${patternColor}" opacity="0.45"/>
                    <rect x="0" y="0" width="${thick}" height="${size}" fill="${patternColor}" opacity="0.45"/>
                    <line x1="0" y1="${20*s}" x2="${size}" y2="${20*s}" stroke="${patternColor}" stroke-width="${1.5*s}" opacity="0.75"/>
                    <line x1="${20*s}" y1="0" x2="${20*s}" y2="${size}" stroke="${patternColor}" stroke-width="${1.5*s}" opacity="0.75"/>
                </pattern>`;
            } else if (pattern === 'houndstooth') {
                const size = 20 * s;
                return `
                <pattern id="activeFabricPattern" width="${size}" height="${size}" patternUnits="userSpaceOnUse">
                    <rect width="${size}" height="${size}" fill="transparent"/>
                    <polygon points="0,0 ${10*s},0 0,${10*s}" fill="${patternColor}" opacity="0.55"/>
                    <polygon points="${10*s},${10*s} ${20*s},${10*s} ${10*s},${20*s}" fill="${patternColor}" opacity="0.55"/>
                    <polygon points="${5*s},${5*s} ${15*s},${5*s} ${15*s},${15*s} ${5*s},${15*s}" fill="${patternColor}" opacity="0.35"/>
                </pattern>`;
            }
            return '';
        }

        // --- Render 3D Realistic Garment (Ultra-realistic SVG with Multi-stop Shading & Texture) ---
        function render3DGarment() {
            const type = studioState.activeType;
            const color = studioState.activeColor;
            const isFront = studioState.currentView === 'front';
            const pattern = studioState.activePattern;
            const patternColor = studioState.patternColor;
            const patternScale = studioState.patternScale;
            const patternOpacity = studioState.patternOpacity;

            const patternDefs = generatePatternDefs(pattern, patternColor, patternScale);

            let garmentContent = '';

            if (type === 'hoodie') {
                garmentContent = generateRealisticHoodieSvg(color, isFront, pattern, patternDefs, patternOpacity);
            } else {
                garmentContent = generateRealisticTShirtSvg(color, isFront, pattern, patternDefs, patternOpacity, type === 'classic-tshirt');
            }

            svgCanvas.innerHTML = garmentContent;
        }

        // --- Realistic 3D T-Shirt SVG Engine ---
        function generateRealisticTShirtSvg(baseColor, isFront, pattern, patternDefs, patternOpacity, isClassic) {
            const isLight = isColorLight(baseColor);
            const patternBlend = isLight ? 'multiply' : 'overlay';

            const bodyPath = isClassic
                ? `M175 125 C215 105 240 100 270 115 C285 125 315 125 330 115 C360 100 385 105 425 125 C455 145 490 200 480 250 C465 265 435 270 415 255 C420 310 422 410 425 515 C360 528 240 528 175 515 C178 410 180 310 185 255 C165 270 135 265 120 250 C110 200 145 145 175 125 Z`
                : `M170 120 C215 95 242 90 270 110 C285 122 315 122 330 110 C358 90 385 95 430 120 C465 142 505 205 495 260 C475 275 440 280 420 260 C424 320 426 425 430 530 C365 545 235 545 170 530 C174 425 176 320 180 260 C160 280 125 275 105 260 C95 205 135 142 170 120 Z`;

            return `
            <defs>
                ${patternDefs}
                
                <!-- Fabric Micro-Texture Filter -->
                <filter id="fabricTexture" x="0%" y="0%" width="100%" height="100%">
                    <feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="3" result="noise" />
                    <feColorMatrix type="matrix" values="0.33 0.33 0.33 0 0  0.33 0.33 0.33 0 0  0.33 0.33 0.33 0 0  0 0 0 0.15 0" />
                    <feBlend mode="multiply" in="SourceGraphic" result="blend" />
                </filter>

                <!-- Ground Shadow Blur -->
                <filter id="floorShadowBlur" x="-30%" y="-30%" width="160%" height="160%">
                    <feGaussianBlur stdDeviation="16" />
                </filter>

                <!-- Collar Depth Gradient -->
                <linearGradient id="collarInnerGrad" x1="0" y1="0" x2="0" y2="1">
                    <stop offset="0%" stop-color="#000000" stop-opacity="0.9"/>
                    <stop offset="100%" stop-color="#000000" stop-opacity="0.25"/>
                </linearGradient>

                <!-- 3D Volumetric Torso Lighting -->
                <radialGradient id="torsoHighlight" cx="48%" cy="36%" r="52%">
                    <stop offset="0%" stop-color="#ffffff" stop-opacity="${isLight ? '0.18' : '0.35'}"/>
                    <stop offset="60%" stop-color="#ffffff" stop-opacity="0.04"/>
                    <stop offset="100%" stop-color="#000000" stop-opacity="${isLight ? '0.28' : '0.45'}"/>
                </radialGradient>

                <!-- Ambient Occlusion Shadow Gradient -->
                <linearGradient id="ambientShading" x1="0" y1="0" x2="0" y2="1">
                    <stop offset="0%" stop-color="#ffffff" stop-opacity="0.08"/>
                    <stop offset="70%" stop-color="#000000" stop-opacity="0.1"/>
                    <stop offset="100%" stop-color="#000000" stop-opacity="0.35"/>
                </linearGradient>
            </defs>

            <!-- 1. Realistic Floor Ambient Occlusion Shadow -->
            <ellipse cx="300" cy="555" rx="180" ry="24" fill="#000000" opacity="0.65" filter="url(#floorShadowBlur)" />

            <!-- 2. Inside Back Collar Depth (Front view only) -->
            ${isFront ? `
            <path d="M265 110 C280 92 320 92 335 110 C325 125 275 125 265 110 Z" fill="url(#collarInnerGrad)"/>
            <path d="M265 110 C280 95 320 95 335 110" stroke="rgba(255,255,255,0.18)" stroke-width="1.5" fill="none"/>
            <rect x="290" y="102" width="20" height="8" rx="2" fill="rgba(255,255,255,0.35)"/>
            ` : ''}

            <!-- 3. Base Colored T-Shirt Body -->
            <path d="${bodyPath}" fill="${baseColor}" stroke="rgba(0,0,0,0.45)" stroke-width="2.5" />

            <!-- 4. Pattern Layer (STRICTLY CONFORMED TO GARMENT SILHOUETTE - ZERO SPILLOVER!) -->
            ${pattern !== 'none' ? `
            <path d="${bodyPath}" fill="url(#activeFabricPattern)" opacity="${patternOpacity}" style="mix-blend-mode: ${patternBlend}; pointer-events: none;" />
            ` : ''}

            <!-- 5. 3D Body Lighting & Volumetric Curvature -->
            <path d="${bodyPath}" fill="url(#torsoHighlight)" style="mix-blend-mode: ${isLight ? 'multiply' : 'screen'}; pointer-events: none;" />
            <path d="${bodyPath}" fill="url(#ambientShading)" style="mix-blend-mode: multiply; pointer-events: none;" />

            <!-- 6. Sleeve Seams & Depth (Separates sleeve visually from torso!) -->
            <path d="M170 120 C182 165 180 215 180 260" stroke="rgba(0,0,0,0.35)" stroke-width="2" fill="none"/>
            <path d="M172 120 C184 165 182 215 182 260" stroke="rgba(255,255,255,0.18)" stroke-width="1.2" fill="none"/>
            <path d="M430 120 C418 165 420 215 420 260" stroke="rgba(0,0,0,0.35)" stroke-width="2" fill="none"/>
            <path d="M428 120 C416 165 418 215 418 260" stroke="rgba(255,255,255,0.18)" stroke-width="1.2" fill="none"/>

            <!-- 7. Natural Armpit Tension Creases & Fabric Folds -->
            <path d="M180 240 Q210 270 230 290" stroke="rgba(0,0,0,0.35)" stroke-width="3" stroke-linecap="round" fill="none"/>
            <path d="M182 250 Q205 278 220 298" stroke="rgba(255,255,255,0.2)" stroke-width="1.5" stroke-linecap="round" fill="none"/>
            <path d="M420 240 Q390 270 370 290" stroke="rgba(0,0,0,0.35)" stroke-width="3" stroke-linecap="round" fill="none"/>
            <path d="M418 250 Q395 278 380 298" stroke="rgba(255,255,255,0.2)" stroke-width="1.5" stroke-linecap="round" fill="none"/>

            <!-- Waist Tension Waves -->
            <path d="M186 385 Q235 408 290 402" stroke="rgba(0,0,0,0.22)" stroke-width="2" stroke-linecap="round" fill="none"/>
            <path d="M414 395 Q365 418 310 412" stroke="rgba(0,0,0,0.22)" stroke-width="2" stroke-linecap="round" fill="none"/>

            <!-- 8. Shoulder Top Ridge Highlights -->
            <path d="M170 120 C215 95 242 90 270 110" stroke="rgba(255,255,255,0.35)" stroke-width="2" fill="none"/>
            <path d="M330 110 C358 90 385 95 430 120" stroke="rgba(255,255,255,0.35)" stroke-width="2" fill="none"/>

            <!-- 9. Sleeve Hem Seams & Highlights -->
            <path d="M120 250 C140 268 165 270 180 260" stroke="rgba(0,0,0,0.4)" stroke-width="2" fill="none"/>
            <path d="M121 247 C141 265 166 267 181 257" stroke="rgba(255,255,255,0.2)" stroke-width="1.2" fill="none"/>
            <path d="M420 260 C435 270 460 268 480 250" stroke="rgba(0,0,0,0.4)" stroke-width="2" fill="none"/>
            <path d="M419 257 C434 267 459 265 479 247" stroke="rgba(255,255,255,0.2)" stroke-width="1.2" fill="none"/>

            <!-- 10. Collar Ribbed Band -->
            ${isFront ? `
            <path d="M265 110 C280 135 320 135 335 110 C338 116 335 124 320 138 C280 138 262 116 265 110 Z" fill="rgba(0,0,0,0.3)"/>
            <path d="M265 110 C280 135 320 135 335 110" stroke="rgba(0,0,0,0.55)" stroke-width="3" fill="none"/>
            <path d="M263 113 C280 138 320 138 337 113" stroke="rgba(255,255,255,0.22)" stroke-width="1.5" fill="none"/>
            ` : `
            <path d="M265 110 C280 102 320 102 335 110 C335 118 320 124 300 124 C280 124 265 118 265 110 Z" fill="rgba(0,0,0,0.25)"/>
            <path d="M265 110 C280 102 320 102 335 110" stroke="rgba(0,0,0,0.55)" stroke-width="3" fill="none"/>
            <path d="M210 160 Q300 175 390 160" stroke="rgba(0,0,0,0.3)" stroke-width="1.8" stroke-dasharray="4,3" fill="none"/>
            `}

            <!-- 11. Bottom Hem Curve & Twin Stitch -->
            <path d="M170 530 C235 545 365 545 430 530" stroke="rgba(0,0,0,0.55)" stroke-width="2.5" fill="none"/>
            <path d="M172 522 C236 537 364 537 428 522" stroke="rgba(0,0,0,0.25)" stroke-width="1.2" stroke-dasharray="5,3" fill="none"/>
            <path d="M173 518 C236 533 364 533 427 518" stroke="rgba(0,0,0,0.25)" stroke-width="1.2" stroke-dasharray="5,3" fill="none"/>
            `;
        }

        // --- Realistic 3D Hoodie SVG Engine ---
        function generateRealisticHoodieSvg(baseColor, isFront, pattern, patternDefs, patternOpacity) {
            const isLight = isColorLight(baseColor);
            const patternBlend = isLight ? 'multiply' : 'overlay';

            const bodyPath = `M175 140 C210 90 260 70 300 70 C340 70 390 90 425 140 C465 185 500 240 485 295 C465 310 430 315 410 295 C415 360 418 435 420 530 C360 545 240 545 180 530 C182 435 185 360 190 295 C170 315 135 310 115 295 C100 240 135 185 175 140 Z`;

            return `
            <defs>
                ${patternDefs}
                <filter id="hoodieFloorBlur" x="-30%" y="-30%" width="160%" height="160%">
                    <feGaussianBlur stdDeviation="18" />
                </filter>
                <radialGradient id="hoodieHighlight" cx="48%" cy="38%" r="52%">
                    <stop offset="0%" stop-color="#ffffff" stop-opacity="${isLight ? '0.2' : '0.35'}"/>
                    <stop offset="60%" stop-color="#ffffff" stop-opacity="0.04"/>
                    <stop offset="100%" stop-color="#000000" stop-opacity="${isLight ? '0.3' : '0.45'}"/>
                </radialGradient>
                <linearGradient id="hoodieAmbient" x1="0" y1="0" x2="0" y2="1">
                    <stop offset="0%" stop-color="#ffffff" stop-opacity="0.08"/>
                    <stop offset="70%" stop-color="#000000" stop-opacity="0.12"/>
                    <stop offset="100%" stop-color="#000000" stop-opacity="0.38"/>
                </linearGradient>
            </defs>

            <!-- 1. Floor Shadow -->
            <ellipse cx="300" cy="555" rx="190" ry="26" fill="#000000" opacity="0.75" filter="url(#hoodieFloorBlur)" />

            <!-- 2. Base Colored Hoodie Body -->
            <path d="${bodyPath}" fill="${baseColor}" stroke="rgba(0,0,0,0.45)" stroke-width="2.5"/>

            <!-- 3. Pattern Layer (STRICTLY CONFORMED TO GARMENT SILHOUETTE - ZERO SPILLOVER!) -->
            ${pattern !== 'none' ? `
            <path d="${bodyPath}" fill="url(#activeFabricPattern)" opacity="${patternOpacity}" style="mix-blend-mode: ${patternBlend}; pointer-events: none;" />
            ` : ''}

            <!-- 4. 3D Volumetric Body Lighting -->
            <path d="${bodyPath}" fill="url(#hoodieHighlight)" style="mix-blend-mode: ${isLight ? 'multiply' : 'screen'}; pointer-events: none;"/>
            <path d="${bodyPath}" fill="url(#hoodieAmbient)" style="mix-blend-mode: multiply; pointer-events: none;"/>

            <!-- 5. Sleeve Drop Shoulder Seams -->
            <path d="M175 140 C185 185 188 240 190 295" stroke="rgba(0,0,0,0.35)" stroke-width="2.2" fill="none"/>
            <path d="M177 140 C187 185 190 240 192 295" stroke="rgba(255,255,255,0.18)" stroke-width="1.2" fill="none"/>
            <path d="M425 140 C415 185 412 240 410 295" stroke="rgba(0,0,0,0.35)" stroke-width="2.2" fill="none"/>
            <path d="M423 140 C413 185 410 240 408 295" stroke="rgba(255,255,255,0.18)" stroke-width="1.2" fill="none"/>

            <!-- 6. 3D Hood Construction & Folds -->
            ${isFront ? `
            <!-- Hood Drape on Shoulders -->
            <path d="M220 120 C240 85 270 75 300 75 C330 75 360 85 380 120 C360 160 330 180 300 180 C270 180 240 160 220 120 Z" fill="rgba(0,0,0,0.4)"/>
            <path d="M235 110 C250 85 275 80 300 80 C325 80 350 85 365 110 C350 145 325 160 300 160 C275 160 250 145 235 110 Z" fill="${baseColor}"/>
            ${pattern !== 'none' ? `
            <path d="M235 110 C250 85 275 80 300 80 C325 80 350 85 365 110 C350 145 325 160 300 160 C275 160 250 145 235 110 Z" fill="url(#activeFabricPattern)" opacity="${patternOpacity * 0.75}" style="mix-blend-mode: ${patternBlend};"/>
            ` : ''}
            <!-- Deep Hood Opening Shadow -->
            <path d="M255 110 C270 95 330 95 345 110 C335 140 265 140 255 110 Z" fill="#080a0e"/>
            <!-- Metal Grommets & White Braided Drawstrings -->
            <circle cx="275" cy="170" r="3.5" fill="#cbd5e1" stroke="#334155" stroke-width="1.5"/>
            <circle cx="325" cy="170" r="3.5" fill="#cbd5e1" stroke="#334155" stroke-width="1.5"/>
            <path d="M275 174 C275 220 265 240 270 270" stroke="#f8fafc" stroke-width="3.5" stroke-linecap="round" fill="none" filter="drop-shadow(0 2px 3px rgba(0,0,0,0.4))"/>
            <rect x="268" y="266" width="4.5" height="11" rx="1.5" fill="#94a3b8"/>
            <path d="M325 174 C325 220 335 240 330 270" stroke="#f8fafc" stroke-width="3.5" stroke-linecap="round" fill="none" filter="drop-shadow(0 2px 3px rgba(0,0,0,0.4))"/>
            <rect x="328" y="266" width="4.5" height="11" rx="1.5" fill="#94a3b8"/>

            <!-- Front Kangaroo Pouch Pocket with 3D Depth -->
            <path d="M210 380 L390 380 L415 480 L185 480 Z" fill="${baseColor}" stroke="rgba(0,0,0,0.35)" stroke-width="2"/>
            ${pattern !== 'none' ? `
            <path d="M210 380 L390 380 L415 480 L185 480 Z" fill="url(#activeFabricPattern)" opacity="${patternOpacity}" style="mix-blend-mode: ${patternBlend};"/>
            ` : ''}
            <!-- Pocket Entry Openings -->
            <path d="M210 380 L185 480" stroke="rgba(0,0,0,0.5)" stroke-width="3"/>
            <path d="M390 380 L415 480" stroke="rgba(0,0,0,0.5)" stroke-width="3"/>
            <path d="M210 380 L390 380" stroke="rgba(255,255,255,0.22)" stroke-width="1.8"/>
            <path d="M212 390 L388 390" stroke="rgba(0,0,0,0.25)" stroke-width="1.2" stroke-dasharray="4,3"/>
            <!-- Bar-Tack Pocket Stitches -->
            <line x1="208" y1="380" x2="216" y2="380" stroke="#f59e0b" stroke-width="2"/>
            <line x1="384" y1="380" x2="392" y2="380" stroke="#f59e0b" stroke-width="2"/>
            ` : `
            <!-- Back Hood Drape -->
            <path d="M220 100 C250 60 350 60 380 100 C370 195 230 195 220 100 Z" fill="rgba(0,0,0,0.4)"/>
            <path d="M230 105 C255 75 345 75 370 105 C360 180 240 180 230 105 Z" fill="${baseColor}" stroke="rgba(0,0,0,0.35)" stroke-width="2"/>
            <path d="M300 85 L300 175" stroke="rgba(0,0,0,0.35)" stroke-width="2" stroke-linecap="round"/>
            `}

            <!-- Ribbed Hem & Waistband with Rib Lines -->
            <path d="M180 500 C240 515 360 515 420 500 L420 530 C360 545 240 545 180 530 Z" fill="rgba(0,0,0,0.3)"/>
            <path d="M180 500 C240 515 360 515 420 500" stroke="rgba(0,0,0,0.45)" stroke-width="2.5" fill="none"/>
            <path d="M180 530 C240 545 360 545 420 530" stroke="rgba(0,0,0,0.45)" stroke-width="2.5" fill="none"/>
            <!-- Waistband Rib texture stitches -->
            <path d="M220 508 L220 535 M260 512 L260 538 M300 515 L300 542 M340 512 L340 538 M380 508 L380 535" stroke="rgba(0,0,0,0.25)" stroke-width="1.5"/>
            `;
        }

        // --- Recalculate Pricing & Summary ---
        function recalculateSummary() {
            const currentItem = CATALOG_ITEMS.find(i => i.id === studioState.activeCardId) || CATALOG_ITEMS[0];
            const basePrice = currentItem.price;
            const customPrice = (studioState.elementsCount * 25) + studioState.techniquePrice;
            const totalPrice = basePrice + customPrice;

            document.getElementById('summary-product-name').textContent = currentItem.name;
            document.getElementById('summary-technique-name').textContent = studioState.techniqueName;
            document.getElementById('summary-extras-count').textContent = studioState.elementsCount > 0 
                ? `${studioState.elementsCount} Öğe (+${studioState.elementsCount * 25} ₺)` 
                : 'Yok';

            document.getElementById('summary-base-price').textContent = `${basePrice} ₺`;
            document.getElementById('summary-custom-price').textContent = `${customPrice} ₺`;
            document.getElementById('summary-total-price').textContent = `${totalPrice} ₺`;
        }

        // --- Interactive 3D Parallax Tilt Effect ---
        function initInteractiveTilt() {
            viewportStage.addEventListener('mousemove', (e) => {
                const rect = viewportStage.getBoundingClientRect();
                const x = e.clientX - rect.left - rect.width / 2;
                const y = e.clientY - rect.top - rect.height / 2;
                
                const rotateX = -(y / rect.height) * 10; // max 5 deg tilt
                const rotateY = (x / rect.width) * 12;

                garmentStage.style.transform = `rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
            });

            viewportStage.addEventListener('mouseleave', () => {
                garmentStage.style.transform = `rotateX(0deg) rotateY(0deg)`;
            });
        }

        // --- Active Canvas Layer ---
        function getActiveCanvasLayer() {
            return studioState.currentView === 'front' ? layerFront : layerBack;
        }

        // --- Add Text Element to Canvas ---
        function addTextElement(text, font, color, size) {
            if (!text.trim()) return;

            const el = document.createElement('div');
            el.className = 'design-element absolute p-1 font-bold';
            el.style.left = '25%';
            el.style.top = '35%';
            el.style.color = color;
            el.style.fontSize = `${size}px`;
            el.style.fontFamily = font;
            el.dataset.elType = 'text';
            el.dataset.elColor = color;
            el.dataset.elSize = size;
            el.textContent = text;

            addResizeHandles(el);
            makeDraggable(el);
            getActiveCanvasLayer().appendChild(el);
            selectElement(el);

            studioState.elementsCount++;
            recalculateSummary();
            showStudioToast('Metin tasarıma eklendi');
        }

        // --- Add Sticker Icon to Canvas ---
        function addIconElement(iconClass) {
            const el = document.createElement('div');
            el.className = 'design-element absolute p-1';
            el.style.left = '38%';
            el.style.top = '35%';
            el.style.fontSize = '42px';
            el.style.color = '#f59e0b';
            el.dataset.elType = 'icon';
            el.dataset.elColor = '#f59e0b';
            el.dataset.elSize = '42';
            el.innerHTML = `<i class="fa-solid ${iconClass}"></i>`;

            addResizeHandles(el);
            makeDraggable(el);
            getActiveCanvasLayer().appendChild(el);
            selectElement(el);

            studioState.elementsCount++;
            recalculateSummary();
            showStudioToast('Grafik etiketi eklendi');
        }

        // --- Add uploaded image element ---
        function addImageElement(dataUrl) {
            const el = document.createElement('div');
            el.className = 'design-element absolute';
            el.style.left = '32%';
            el.style.top = '32%';
            el.style.width = '96px';
            el.style.height = '96px';
            el.dataset.elType = 'image';
            el.dataset.elSize = '96';
            el.innerHTML = `<img src="${dataUrl}" class="w-full h-full object-contain pointer-events-none filter drop-shadow" draggable="false">`;

            addResizeHandles(el);
            makeDraggable(el);
            getActiveCanvasLayer().appendChild(el);
            selectElement(el);

            studioState.elementsCount++;
            recalculateSummary();
            showStudioToast('Görsel başarıyla yüklendi');
        }

        // --- Resize Handles for all design elements ---
        function addResizeHandles(el) {
            const positions = ['tl', 'tr', 'bl', 'br'];
            positions.forEach(pos => {
                const handle = document.createElement('div');
                handle.className = `resize-handle rh-${pos}`;
                handle.dataset.handlePos = pos;
                handle.addEventListener('pointerdown', (e) => {
                    e.stopPropagation();
                    e.preventDefault();
                    startResize(el, handle, e);
                });
                el.appendChild(handle);
            });
        }

        function startResize(el, handle, e) {
            const startX = e.clientX;
            const startY = e.clientY;
            const startWidth = el.offsetWidth;
            const startHeight = el.offsetHeight;
            const startFontSize = parseFloat(el.style.fontSize) || 42;
            const pos = handle.dataset.handlePos;
            const isImage = el.dataset.elType === 'image';

            handle.setPointerCapture(e.pointerId);

            const onMove = (ev) => {
                let deltaX = ev.clientX - startX;
                let deltaY = ev.clientY - startY;

                // Reverse delta for left/top handles
                if (pos === 'tl' || pos === 'bl') deltaX = -deltaX;
                if (pos === 'tl' || pos === 'tr') deltaY = -deltaY;

                const delta = Math.max(deltaX, deltaY);

                if (isImage) {
                    const newWidth = Math.max(24, startWidth + delta);
                    const ratio = startHeight / startWidth;
                    el.style.width = `${newWidth}px`;
                    el.style.height = `${Math.round(newWidth * ratio)}px`;
                    el.dataset.elSize = String(newWidth);
                } else {
                    const newSize = Math.max(12, Math.min(160, startFontSize + delta * 0.5));
                    el.style.fontSize = `${Math.round(newSize)}px`;
                    el.dataset.elSize = String(Math.round(newSize));
                }

                // Sync action bar slider
                updateActionBarFromElement(el);
            };

            const onUp = (ev) => {
                handle.removeEventListener('pointermove', onMove);
                handle.removeEventListener('pointerup', onUp);
                try { handle.releasePointerCapture(ev.pointerId); } catch(err){}
            };

            handle.addEventListener('pointermove', onMove);
            handle.addEventListener('pointerup', onUp);
        }

        // --- Apply color to selected element ---
        function applyColorToElement(el, color) {
            if (!el) return;
            el.dataset.elColor = color;
            if (el.dataset.elType === 'image') return; // images don't change color

            el.style.color = color;
        }

        // --- Apply size to selected element ---
        function applySizeToElement(el, size) {
            if (!el) return;
            el.dataset.elSize = String(size);

            if (el.dataset.elType === 'image') {
                el.style.width = `${size}px`;
                el.style.height = `${size}px`;
            } else {
                el.style.fontSize = `${size}px`;
            }
        }

        // --- Update action bar controls from the selected element ---
        function updateActionBarFromElement(el) {
            if (!el) return;
            const color = el.dataset.elColor || '#f59e0b';
            const size = parseInt(el.dataset.elSize || '42');

            document.getElementById('element-color-picker').value = color;
            document.getElementById('element-size-slider').value = size;
            document.getElementById('element-size-label').textContent = size;

            // Disable color controls for images
            const colorRow = document.getElementById('element-color-picker').closest('.flex');
            if (el.dataset.elType === 'image') {
                colorRow.style.opacity = '0.35';
                colorRow.style.pointerEvents = 'none';
            } else {
                colorRow.style.opacity = '1';
                colorRow.style.pointerEvents = 'auto';
            }
        }

        // --- Draggable & Interactive Element Behavior ---
        function makeDraggable(el) {
            let isDragging = false;
            let startX, startY, initLeft, initTop;

            el.addEventListener('pointerdown', (e) => {
                e.stopPropagation();
                selectElement(el);

                isDragging = true;
                startX = e.clientX;
                startY = e.clientY;
                initLeft = el.offsetLeft;
                initTop = el.offsetTop;
                el.setPointerCapture(e.pointerId);
            });

            el.addEventListener('pointermove', (e) => {
                if (!isDragging || studioState.selectedElement !== el) return;
                const deltaX = e.clientX - startX;
                const deltaY = e.clientY - startY;

                el.style.left = `${initLeft + deltaX}px`;
                el.style.top = `${initTop + deltaY}px`;
            });

            el.addEventListener('pointerup', (e) => {
                isDragging = false;
                try { el.releasePointerCapture(e.pointerId); } catch(err){}
            });
        }

        function selectElement(el) {
            document.querySelectorAll('.design-element').forEach(i => i.classList.remove('selected'));
            if (el) {
                el.classList.add('selected');
                studioState.selectedElement = el;
                actionControls.classList.remove('hidden');
                updateActionBarFromElement(el);
            } else {
                studioState.selectedElement = null;
                actionControls.classList.add('hidden');
            }
        }

        // --- Event Handlers Setup ---
        function initEventHandlers() {
            // Navigation tabs
            document.querySelectorAll('.nav-tab').forEach(tab => {
                tab.addEventListener('click', () => {
                    document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
                    document.querySelectorAll('.tab-pane').forEach(p => p.classList.add('hidden'));

                    tab.classList.add('active');
                    const tabId = tab.getAttribute('data-tab');
                    document.getElementById(`tab-${tabId}`).classList.remove('hidden');
                });
            });

            // Front/Back Toggle Buttons matching photo
            const btnFront = document.getElementById('toggle-view-front');
            const btnBack = document.getElementById('toggle-view-back');

            btnFront.addEventListener('click', () => {
                studioState.currentView = 'front';
                btnFront.className = 'px-5 py-2 text-xs font-extrabold rounded-xl bg-amberBrand-500 text-black transition-all shadow-md shadow-amber-500/20';
                btnBack.className = 'px-5 py-2 text-xs font-semibold rounded-xl text-zinc-400 hover:text-white transition-all';
                layerFront.classList.remove('hidden');
                layerBack.classList.add('hidden');
                render3DGarment();
            });

            btnBack.addEventListener('click', () => {
                studioState.currentView = 'back';
                btnBack.className = 'px-5 py-2 text-xs font-extrabold rounded-xl bg-amberBrand-500 text-black transition-all shadow-md shadow-amber-500/20';
                btnFront.className = 'px-5 py-2 text-xs font-semibold rounded-xl text-zinc-400 hover:text-white transition-all';
                layerBack.classList.remove('hidden');
                layerFront.classList.add('hidden');
                render3DGarment();
            });

            // Custom color picker
            const colorPicker = document.getElementById('custom-color-input');
            colorPicker.addEventListener('input', (e) => {
                const hex = e.target.value;
                studioState.activeColor = hex;
                document.getElementById('custom-color-code').textContent = hex.toUpperCase();
                renderColorSwatches();
                render3DGarment();
                recalculateSummary();
            });

            // Add Text Submit
            document.getElementById('btn-submit-text').addEventListener('click', () => {
                const text = document.getElementById('input-text-content').value;
                const font = document.getElementById('select-font-family').value;
                const color = document.getElementById('input-text-color').value;
                const size = document.getElementById('input-text-size').value;
                addTextElement(text, font, color, size);
                document.getElementById('input-text-content').value = '';
            });

            // Text font size slider
            document.getElementById('input-text-size').addEventListener('input', (e) => {
                document.getElementById('text-size-display').textContent = `${e.target.value}px`;
            });

            // Text color picker
            document.getElementById('input-text-color').addEventListener('input', (e) => {
                document.getElementById('text-color-hex').textContent = e.target.value.toUpperCase();
            });

            // Upload custom graphic
            document.getElementById('input-file-upload').addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (event) => {
                        addImageElement(event.target.result);
                    };
                    reader.readAsDataURL(file);
                }
            });

            // --- Element Action Bar: Color Picker ---
            document.getElementById('element-color-picker').addEventListener('input', (e) => {
                if (studioState.selectedElement) {
                    applyColorToElement(studioState.selectedElement, e.target.value);
                }
            });

            // --- Element Action Bar: Quick Color Buttons ---
            document.querySelectorAll('#element-quick-colors button[data-qc]').forEach(btn => {
                btn.addEventListener('click', () => {
                    const color = btn.getAttribute('data-qc');
                    if (studioState.selectedElement) {
                        applyColorToElement(studioState.selectedElement, color);
                        document.getElementById('element-color-picker').value = color;
                    }
                });
            });

            // --- Element Action Bar: Size Slider ---
            document.getElementById('element-size-slider').addEventListener('input', (e) => {
                const size = parseInt(e.target.value);
                document.getElementById('element-size-label').textContent = size;
                if (studioState.selectedElement) {
                    applySizeToElement(studioState.selectedElement, size);
                }
            });

            // Patterns Selector
            const patternNames = {
                'none': 'Düz Kumaş',
                'stripes': 'Çizgili Stil',
                'camo': 'Kamuflaj (Camo)',
                'dots': 'Puantiyeli',
                'plaid': 'Ekose Kareli',
                'houndstooth': 'Kazayağı'
            };

            document.querySelectorAll('.pattern-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    document.querySelectorAll('.pattern-btn').forEach(b => {
                        b.className = 'pattern-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all';
                    });
                    btn.className = 'pattern-btn active p-3 rounded-xl border border-amberBrand-500 bg-amberBrand-500/10 text-left transition-all';
                    const pat = btn.getAttribute('data-pattern');
                    studioState.activePattern = pat;
                    const nameLabel = document.getElementById('pattern-active-name');
                    if (nameLabel) nameLabel.textContent = patternNames[pat] || pat;
                    render3DGarment();
                });
            });

            // Pattern Color Presets
            document.querySelectorAll('#pattern-color-presets button[data-pcolor]').forEach(btn => {
                btn.addEventListener('click', () => {
                    document.querySelectorAll('#pattern-color-presets button[data-pcolor]').forEach(b => {
                        b.className = 'w-7 h-7 rounded-lg border border-studio-border hover:scale-110 transition-transform';
                    });
                    btn.className = 'w-7 h-7 rounded-lg border-2 border-amberBrand-500 hover:scale-110 transition-transform shadow';
                    const color = btn.getAttribute('data-pcolor');
                    studioState.patternColor = color;
                    const customPicker = document.getElementById('pattern-custom-color');
                    if (customPicker) customPicker.value = color;
                    render3DGarment();
                });
            });

            // Pattern Custom Color Picker
            const customPatternPicker = document.getElementById('pattern-custom-color');
            if (customPatternPicker) {
                customPatternPicker.addEventListener('input', (e) => {
                    studioState.patternColor = e.target.value;
                    render3DGarment();
                });
            }

            // Pattern Scale Buttons
            document.querySelectorAll('.pattern-scale-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    document.querySelectorAll('.pattern-scale-btn').forEach(b => {
                        b.className = 'pattern-scale-btn py-1.5 px-2 rounded-lg border border-studio-border bg-studio-850 text-xs font-semibold hover:border-zinc-500 transition-all text-zinc-300';
                    });
                    btn.className = 'pattern-scale-btn active py-1.5 px-2 rounded-lg border border-amberBrand-500 bg-amberBrand-500/10 text-xs font-bold text-white transition-all';
                    studioState.patternScale = parseFloat(btn.getAttribute('data-scale'));
                    const scaleLabel = document.getElementById('pattern-scale-label');
                    if (scaleLabel) scaleLabel.textContent = btn.textContent;
                    render3DGarment();
                });
            });

            // Pattern Opacity Slider
            const patternOpacitySlider = document.getElementById('pattern-opacity-slider');
            if (patternOpacitySlider) {
                patternOpacitySlider.addEventListener('input', (e) => {
                    const val = parseInt(e.target.value);
                    studioState.patternOpacity = val / 100;
                    const opacityLabel = document.getElementById('pattern-opacity-label');
                    if (opacityLabel) opacityLabel.textContent = `${val}%`;
                    render3DGarment();
                });
            }

            // Delete selected element
            document.getElementById('btn-delete-active').addEventListener('click', () => {
                if (studioState.selectedElement) {
                    studioState.selectedElement.remove();
                    selectElement(null);
                    studioState.elementsCount = Math.max(0, studioState.elementsCount - 1);
                    recalculateSummary();
                    showStudioToast('Öğe silindi');
                }
            });

            // Deselect element on background click
            viewportStage.addEventListener('click', (e) => {
                if (!e.target.closest('.design-element') && !e.target.closest('#element-action-bar')) {
                    selectElement(null);
                }
            });

            // Size selection pills
            document.querySelectorAll('.size-pill').forEach(pill => {
                pill.addEventListener('click', () => {
                    document.querySelectorAll('.size-pill').forEach(p => {
                        p.className = 'size-pill py-2.5 text-xs font-bold rounded-xl border border-studio-border bg-studio-850 text-zinc-300 hover:border-zinc-500 transition-all';
                    });
                    pill.className = 'size-pill active py-2.5 text-xs font-bold rounded-xl border border-amberBrand-500 bg-amberBrand-500 text-black transition-all shadow-md shadow-amber-500/20';
                    studioState.activeSize = pill.getAttribute('data-size');
                });
            });

            // Technique selection
            document.querySelectorAll('.technique-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    document.querySelectorAll('.technique-btn').forEach(b => {
                        b.className = 'technique-btn p-3 rounded-xl border border-studio-border bg-studio-850 text-left hover:border-zinc-500 transition-all';
                    });
                    btn.className = 'technique-btn active p-3 rounded-xl border border-amberBrand-500 bg-amberBrand-500/10 text-left transition-all';

                    studioState.techniquePrice = parseInt(btn.getAttribute('data-price'));
                    studioState.techniqueName = btn.querySelector('.font-bold').textContent;
                    recalculateSummary();
                });
            });

            // Reset Design button
            document.getElementById('btn-reset-design').addEventListener('click', () => {
                layerFront.innerHTML = '';
                layerBack.innerHTML = '';
                studioState.elementsCount = 0;
                selectElement(null);
                recalculateSummary();
                showStudioToast('Tasarım sıfırlandı');
            });

            // Size Chart Modal
            document.getElementById('btn-size-chart').addEventListener('click', () => {
                document.getElementById('modal-size-chart-window').classList.remove('hidden');
            });

            // Checkout CTA button
            document.getElementById('btn-checkout-cta').addEventListener('click', () => {
                const currentItem = CATALOG_ITEMS.find(i => i.id === studioState.activeCardId) || CATALOG_ITEMS[0];
                document.getElementById('modal-item-title').textContent = currentItem.name;
                document.getElementById('modal-item-size').textContent = studioState.activeSize;
                document.getElementById('modal-item-color').textContent = studioState.activeColor.toUpperCase();
                document.getElementById('modal-color-preview').style.backgroundColor = studioState.activeColor;
                document.getElementById('modal-item-total').textContent = document.getElementById('summary-total-price').textContent;

                document.getElementById('modal-checkout-window').classList.remove('hidden');
            });

            // Dismiss Modals
            document.querySelectorAll('.modal-dismiss').forEach(btn => {
                btn.addEventListener('click', () => {
                    document.querySelectorAll('.fixed').forEach(m => {
                        if (!m.id.includes('toast')) m.classList.add('hidden');
                    });
                });
            });

            // Order form submission
            document.getElementById('form-order').addEventListener('submit', (e) => {
                e.preventDefault();
                document.getElementById('modal-checkout-window').classList.add('hidden');
                showStudioToast('Siparişiniz başarıyla alındı! Atölyede hazırlanıyor.');
            });

            // Download Design Mockup button
            document.getElementById('btn-save-design').addEventListener('click', () => {
                showStudioToast('Tasarım yüksek çözünürlükte kaydediliyor...');
            });
        }

        // --- Notification Toast ---
        function showStudioToast(msg) {
            const toast = document.getElementById('studio-toast');
            document.getElementById('toast-text-status').textContent = msg;
            toast.classList.remove('translate-y-24', 'opacity-0');

            setTimeout(() => {
                toast.classList.add('translate-y-24', 'opacity-0');
            }, 3200);
        }
    </script>
</body>
</html>
