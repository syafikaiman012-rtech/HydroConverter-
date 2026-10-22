# HydroConverter-
Easy for convert 
```html
<!DOCTYPE html>
<html lang="en" class="dark h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#0284c7">
    <meta name="description" content="HydroConverter - Professional Dynamic Water Flow Converter, Custom Unit Builder & Pipe Velocity Calculator.">
    
    <!-- PWA & Mobile Web App Meta Tags -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="HydroConverter">
    <meta name="application-name" content="HydroConverter">

    <title>HydroConverter - Dynamic Water Flow & Hydraulic Suite</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        navy: {
                            800: '#0b192c',
                            900: '#070f1e',
                            950: '#030712'
                        },
                        hydro: {
                            50: '#f0f9ff',
                            100: '#e0f2fe',
                            200: '#bae6fd',
                            300: '#7dd3fc',
                            400: '#38bdf8',
                            500: '#0ea5e9',
                            600: '#0284c7',
                            700: '#0369a1',
                            800: '#075985',
                            900: '#0c4a6e',
                            950: '#082f49',
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
                        mono: ['JetBrains Mono', 'Fira Code', 'Consolas', 'monospace']
                    }
                }
            }
        }
    </script>
    
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 9999px;
        }
        .dark ::-webkit-scrollbar-thumb {
            background: #1e293b;
        }
        
        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { 
            -webkit-appearance: none; 
            margin: 0; 
        }
        input[type=number] {
            -moz-appearance: textfield;
        }

        .glass-header {
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
        }

        button, input, select {
            touch-action: manipulation;
        }

        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 22px;
            width: 22px;
            border-radius: 50%;
            background: #38bdf8;
            cursor: pointer;
            box-shadow: 0 0 12px rgba(56, 189, 248, 0.6);
            border: 2px solid #ffffff;
        }
    </style>
</head>
<body class="bg-slate-100 dark:bg-navy-900 text-slate-800 dark:text-slate-100 font-sans min-h-full flex flex-col transition-colors duration-300 selection:bg-hydro-500 selection:text-white pb-12 sm:pb-0">

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-6 right-6 z-50 transform translate-y-24 opacity-0 transition-all duration-300 ease-out bg-slate-900 dark:bg-slate-100 text-white dark:text-slate-900 px-5 py-3.5 rounded-2xl shadow-2xl font-medium text-sm flex items-center gap-3 border border-slate-700 dark:border-slate-300 pointer-events-none">
        <div class="w-6 h-6 rounded-full bg-hydro-500 text-white flex items-center justify-center text-xs shadow">
            <i class="fa-solid fa-check"></i>
        </div>
        <span id="toast-message">Copied to clipboard!</span>
    </div>

    <!-- Custom Modal Dialog (Replaces Browser Alerts) -->
    <div id="custom-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-slate-950/70 backdrop-blur-sm opacity-0 pointer-events-none transition-opacity duration-300 p-4">
        <div class="bg-white dark:bg-navy-800 border border-slate-200 dark:border-slate-700 rounded-3xl max-w-md w-full p-6 shadow-2xl transform scale-95 transition-transform duration-300" id="modal-container">
            <div class="flex items-center gap-3 text-amber-500 mb-3">
                <i class="fa-solid fa-triangle-exclamation text-2xl" id="modal-icon"></i>
                <h3 class="text-lg font-bold text-slate-900 dark:text-white" id="modal-title">Confirmation Required</h3>
            </div>
            <p class="text-xs text-slate-600 dark:text-slate-300 mb-6 leading-relaxed" id="modal-body">Are you sure you want to proceed?</p>
            <div class="flex justify-end gap-3">
                <button id="modal-cancel-btn" class="px-4 py-2 text-xs font-semibold rounded-xl bg-slate-200 dark:bg-slate-700 text-slate-700 dark:text-slate-200 hover:bg-slate-300 dark:hover:bg-slate-600 transition">Cancel</button>
                <button id="modal-confirm-btn" class="px-4 py-2 text-xs font-semibold rounded-xl bg-rose-600 text-white hover:bg-rose-500 transition shadow">Confirm</button>
            </div>
        </div>
    </div>

    <!-- Header / Navbar -->
    <header class="border-b border-slate-200 dark:border-slate-800 bg-white/85 dark:bg-navy-800/85 glass-header sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between gap-4">
            
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-hydro-700 via-hydro-500 to-cyan-400 flex items-center justify-center text-white shadow-lg shadow-hydro-500/30">
                    <i class="fa-solid fa-droplet text-lg"></i>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="text-xl font-extrabold tracking-tight text-slate-900 dark:text-white leading-tight">HydroConverter</h1>
                        <span class="text-[10px] font-black uppercase tracking-wider px-2 py-0.5 rounded-full bg-cyan-100 dark:bg-cyan-950/80 text-cyan-700 dark:text-cyan-300 border border-cyan-300 dark:border-cyan-800/60">Dynamic Suite</span>
                    </div>
                    <p class="text-xs text-slate-500 dark:text-slate-400 font-medium">Water Flow Conversion & Dynamic Hydraulics</p>
                </div>
            </div>

            <div class="flex items-center gap-2 sm:gap-3">
                <button id="theme-toggle" class="p-2.5 sm:px-3 sm:py-2 text-xs font-semibold rounded-xl text-slate-600 dark:text-slate-300 bg-slate-200/80 dark:bg-slate-800 hover:bg-slate-300 dark:hover:bg-slate-700 transition flex items-center gap-2" title="Toggle Light/Dark Theme">
                    <i class="fa-solid fa-moon dark:hidden text-slate-700"></i>
                    <i class="fa-solid fa-sun hidden dark:inline text-amber-400"></i>
                    <span class="hidden sm:inline">Theme</span>
                </button>

                <button id="factory-reset-btn" class="p-2.5 sm:px-3 sm:py-2 text-xs font-semibold rounded-xl text-slate-600 dark:text-slate-300 bg-slate-200/80 dark:bg-slate-800 hover:bg-rose-100 dark:hover:bg-rose-950/50 hover:text-rose-600 dark:hover:text-rose-400 transition flex items-center gap-2" title="Reset all data to defaults">
                    <i class="fa-solid fa-rotate-left"></i>
                    <span class="hidden sm:inline">Reset Defaults</span>
                </button>

                <button onclick="copyFullReport()" class="px-3.5 py-2 text-xs font-bold rounded-xl text-white bg-hydro-600 hover:bg-hydro-500 shadow-md shadow-hydro-600/25 transition flex items-center gap-2">
                    <i class="fa-solid fa-share-nodes"></i>
                    <span class="hidden sm:inline">Export Report</span>
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8 flex-1 w-full space-y-6 sm:space-y-8">

        <!-- Prominent Spotlight Focus Pair -->
        <section class="bg-gradient-to-br from-navy-800 via-slate-900 to-navy-950 text-white rounded-3xl p-6 sm:p-8 shadow-2xl relative overflow-hidden border border-slate-800">
            <div class="absolute -right-20 -top-20 w-80 h-80 bg-hydro-500/15 rounded-full blur-3xl pointer-events-none"></div>
            <div class="absolute -left-20 -bottom-20 w-80 h-80 bg-cyan-400/15 rounded-full blur-3xl pointer-events-none"></div>

            <div class="relative z-10 space-y-6">
                <div class="flex flex-wrap items-center justify-between gap-3">
                    <div class="flex items-center gap-2">
                        <span class="w-2.5 h-2.5 rounded-full bg-cyan-400 animate-pulse"></span>
                        <span class="text-xs font-bold uppercase tracking-wider text-cyan-300 font-mono">Core Flow Focus Pair</span>
                    </div>
                    <button onclick="copySpotlightPair()" class="px-3 py-1.5 bg-slate-800/90 hover:bg-slate-700 text-slate-200 text-xs font-semibold rounded-lg border border-slate-700/80 transition flex items-center gap-1.5">
                        <i class="fa-solid fa-copy text-cyan-400"></i> Copy Spotlight
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-5 relative">
                    <!-- Unit 1: Liter per second (L/s) -->
                    <div class="bg-navy-900/90 p-5 rounded-2xl border border-cyan-500/30 hover:border-cyan-400/60 transition shadow-inner group">
                        <div class="flex justify-between items-center mb-2">
                            <label for="spot-ls" class="text-sm font-bold text-slate-200 flex items-center gap-2 cursor-pointer">
                                <i class="fa-solid fa-faucet-drip text-cyan-400"></i> Liter per second
                            </label>
                            <span class="text-xs font-mono font-bold text-cyan-400 bg-cyan-950/80 border border-cyan-800/60 px-2.5 py-0.5 rounded-md">L/s</span>
                        </div>
                        <div class="relative flex items-center">
                            <input type="number" id="spot-ls" step="any" placeholder="0.00" 
                                class="w-full bg-slate-900/90 border border-slate-700 group-hover:border-slate-600 focus:border-cyan-400 focus:ring-2 focus:ring-cyan-400/20 rounded-xl py-3 px-4 text-2xl sm:text-3xl font-mono text-white font-semibold transition outline-none">
                            <button onclick="copyUnitValue('spot-ls', 'L/s')" class="absolute right-3 text-slate-500 hover:text-cyan-400 transition p-2" title="Copy value">
                                <i class="fa-regular fa-copy"></i>
                            </button>
                        </div>
                        <div class="mt-2.5 flex justify-between items-center text-[11px] font-mono text-slate-400">
                            <span>1 L/s = 0.0864 ML/d</span>
                            <span id="ls-daily-m3" class="text-slate-300">86.4 m³/day</span>
                        </div>
                    </div>

                    <div class="hidden md:flex absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 w-9 h-9 rounded-full bg-slate-800 border border-slate-700 items-center justify-center text-cyan-400 text-sm shadow-lg pointer-events-none z-20">
                        <i class="fa-solid fa-arrows-left-right"></i>
                    </div>

                    <!-- Unit 2: Megaliter per day (ML/d) -->
                    <div class="bg-navy-900/90 p-5 rounded-2xl border border-hydro-500/30 hover:border-hydro-400/60 transition shadow-inner group">
                        <div class="flex justify-between items-center mb-2">
                            <label for="spot-mld" class="text-sm font-bold text-slate-200 flex items-center gap-2 cursor-pointer">
                                <i class="fa-solid fa-water text-hydro-400"></i> Megaliter per day
                            </label>
                            <span class="text-xs font-mono font-bold text-hydro-400 bg-hydro-950/80 border border-hydro-800/60 px-2.5 py-0.5 rounded-md">ML/d</span>
                        </div>
                        <div class="relative flex items-center">
                            <input type="number" id="spot-mld" step="any" placeholder="0.00" 
                                class="w-full bg-slate-900/90 border border-slate-700 group-hover:border-slate-600 focus:border-hydro-400 focus:ring-2 focus:ring-hydro-400/20 rounded-xl py-3 px-4 text-2xl sm:text-3xl font-mono text-white font-semibold transition outline-none">
                            <button onclick="copyUnitValue('spot-mld', 'ML/d')" class="absolute right-3 text-slate-500 hover:text-hydro-400 transition p-2" title="Copy value">
                                <i class="fa-regular fa-copy"></i>
                            </button>
                        </div>
                        <div class="mt-2.5 flex justify-between items-center text-[11px] font-mono text-slate-400">
                            <span>1 ML/d = 11.574 L/s</span>
                            <span id="mld-hourly-m3" class="text-slate-300">41.67 m³/h</span>
                        </div>
                    </div>
                </div>

                <!-- Custom Presets Manager & Shortcuts -->
                <div class="pt-2">
                    <div class="flex items-center justify-between mb-3">
                        <div class="text-xs font-semibold text-slate-400 uppercase tracking-wider flex items-center gap-2">
                            <i class="fa-solid fa-bolt text-amber-400"></i> Dynamic Flow Shortcuts
                        </div>
                        <button id="add-preset-btn" class="text-xs text-cyan-400 hover:text-cyan-300 font-semibold flex items-center gap-1 bg-slate-800/80 px-2.5 py-1 rounded-lg border border-slate-700 hover:border-slate-600 transition">
                            <i class="fa-solid fa-plus text-[10px]"></i> Add Preset
                        </button>
                    </div>
                    
                    <!-- Dynamic Presets Container -->
                    <div id="presets-container" class="flex flex-wrap gap-2.5">
                        <!-- Rendered dynamically -->
                    </div>
                </div>
            </div>
        </section>

        <!-- Multi-Unit Conversion Matrix & Custom Unit Builder -->
        <section class="bg-white dark:bg-navy-800 rounded-3xl border border-slate-200 dark:border-slate-800 p-6 sm:p-8 shadow-xl transition-colors duration-300">
            <div class="flex flex-wrap items-center justify-between gap-4 mb-6">
                <div>
                    <h2 class="text-xl font-bold text-slate-900 dark:text-white flex items-center gap-2">
                        <i class="fa-solid fa-table-cells text-hydro-500"></i> Multi-Unit Conversion Matrix
                    </h2>
                    <p class="text-xs text-slate-500 dark:text-slate-400 mt-1">Real-time bi-directional conversion across built-in and user-customized flow units</p>
                </div>
                <div class="flex items-center gap-2">
                    <button id="add-custom-unit-btn" class="text-xs font-semibold text-white bg-hydro-600 hover:bg-hydro-500 px-3 py-1.5 rounded-xl transition flex items-center gap-1.5 shadow">
                        <i class="fa-solid fa-plus"></i> Custom Unit Builder
                    </button>
                    <span class="text-xs font-mono font-medium text-slate-500 dark:text-slate-400 bg-slate-100 dark:bg-navy-900 px-3 py-1.5 rounded-xl border border-slate-200 dark:border-slate-800">
                        <i class="fa-solid fa-arrows-rotate text-hydro-500 animate-spin mr-1"></i> Sync
                    </span>
                </div>
            </div>

            <!-- Dynamic Unit Grid -->
            <div id="units-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <!-- Rendered dynamically -->
            </div>
        </section>

        <!-- Smart Pipe Velocity & Custom Pipe Library -->
        <section class="bg-white dark:bg-navy-800 rounded-3xl border border-slate-200 dark:border-slate-800 p-6 sm:p-8 shadow-xl transition-colors duration-300">
            <div class="flex flex-wrap items-center justify-between gap-4 mb-6">
                <div>
                    <h3 class="text-lg font-bold text-slate-900 dark:text-white flex items-center gap-2">
                        <i class="fa-solid fa-circle-nodes text-cyan-500"></i> Pipe Velocity & Dynamic Library
                    </h3>
                    <p class="text-xs text-slate-500 dark:text-slate-400 mt-1">Calculates internal water velocity based on customizable engineering pipe sizes</p>
                </div>
                <span id="velocity-status-pill" class="text-xs font-bold font-mono px-3 py-1.5 rounded-full bg-emerald-100 dark:bg-emerald-950/80 text-emerald-700 dark:text-emerald-300 border border-emerald-300 dark:border-emerald-800">
                    <i class="fa-solid fa-circle-check mr-1"></i> Optimal Range
                </span>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div class="space-y-4 md:col-span-1 bg-slate-50 dark:bg-navy-900 p-5 rounded-2xl border border-slate-200 dark:border-slate-800">
                    <div class="flex justify-between items-center">
                        <label class="block text-xs font-bold text-slate-700 dark:text-slate-300">Pipe Selection</label>
                        <button id="add-pipe-btn" class="text-[11px] font-bold text-cyan-500 hover:text-cyan-400 flex items-center gap-1">
                            <i class="fa-solid fa-plus text-[9px]"></i> Add Pipe Size
                        </button>
                    </div>

                    <select id="pipe-preset-select" class="w-full bg-white dark:bg-slate-900 border border-slate-300 dark:border-slate-700 text-slate-900 dark:text-white rounded-xl p-2.5 text-xs font-semibold focus:border-hydro-500 outline-none">
                        <!-- Populated dynamically -->
                    </select>

                    <div id="custom-diameter-container" class="space-y-2">
                        <div class="flex items-center justify-between">
                            <label for="custom-diameter-val" class="text-xs font-bold text-slate-700 dark:text-slate-300">Inner Diameter Value</label>
                            <div class="flex gap-2 text-[10px] font-mono">
                                <label class="inline-flex items-center"><input type="radio" name="pipe-unit" value="mm" checked class="mr-1 accent-hydro-500"> mm</label>
                                <label class="inline-flex items-center"><input type="radio" name="pipe-unit" value="inch" class="mr-1 accent-hydro-500"> in</label>
                            </div>
                        </div>
                        <input type="number" id="custom-diameter-val" value="150" step="any" class="w-full bg-white dark:bg-slate-900 border border-slate-300 dark:border-slate-700 rounded-xl p-2.5 text-sm font-mono text-slate-900 dark:text-white font-bold outline-none">
                    </div>
                </div>

                <div class="md:col-span-2 bg-slate-50 dark:bg-navy-900 p-5 rounded-2xl border border-slate-200 dark:border-slate-800 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="text-xs font-bold text-slate-500 dark:text-slate-400 uppercase font-mono">Hydraulic Pipe Velocity Analysis</span>
                            <span class="text-xs font-mono text-slate-400" id="pipe-area-display">Area: 0.0177 m²</span>
                        </div>

                        <div class="flex items-baseline gap-3 mb-4">
                            <span id="pipe-velocity-val" class="text-3xl sm:text-4xl font-extrabold font-mono text-slate-900 dark:text-white">0.85</span>
                            <span class="text-sm font-bold text-slate-500 dark:text-slate-400">meters / sec (m/s)</span>
                        </div>

                        <div class="space-y-1.5">
                            <div class="flex justify-between text-[11px] font-mono text-slate-500 dark:text-slate-400">
                                <span>0.0 m/s (Stagnant)</span>
                                <span class="font-bold text-emerald-600 dark:text-emerald-400">Optimal: 0.6 - 2.0 m/s</span>
                                <span>3.0+ m/s (Scour/Erosion)</span>
                            </div>
                            <div class="w-full bg-slate-200 dark:bg-slate-950 h-3 rounded-full overflow-hidden p-0.5 border border-slate-300 dark:border-slate-800 relative">
                                <div class="absolute left-[20%] right-[33.3%] top-0 bottom-0 bg-emerald-500/20 border-x border-emerald-500/40"></div>
                                <div id="velocity-bar" class="bg-emerald-500 h-full rounded-full transition-all duration-300" style="width: 28%"></div>
                            </div>
                        </div>
                    </div>

                    <p id="velocity-explanation" class="text-xs text-slate-600 dark:text-slate-300 mt-4 leading-relaxed bg-white dark:bg-slate-950 p-3 rounded-xl border border-slate-200 dark:border-slate-800">
                        <!-- Text dynamically set -->
                    </p>
                </div>
            </div>
        </section>

        <!-- Visual Benchmarks & Interactive Slider -->
        <section class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <div class="lg:col-span-2 bg-white dark:bg-navy-800 rounded-3xl border border-slate-200 dark:border-slate-800 p-6 sm:p-8 shadow-xl flex flex-col justify-between transition-colors duration-300">
                <div>
                    <div class="flex items-center justify-between mb-5">
                        <div>
                            <h3 class="text-lg font-bold text-slate-900 dark:text-white flex items-center gap-2">
                                <i class="fa-solid fa-shapes text-hydro-500"></i> Visual Impact Benchmarks
                            </h3>
                            <p class="text-xs text-slate-500 dark:text-slate-400 mt-1">Understanding current flow magnitude through practical equivalents</p>
                        </div>
                        <span id="viz-scale-badge" class="text-xs font-semibold px-2.5 py-1 rounded-lg bg-hydro-100 dark:bg-hydro-950 text-hydro-700 dark:text-hydro-300 border border-hydro-200 dark:border-hydro-800/60">
                            Domestic Scale
                        </span>
                    </div>

                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-3.5">
                        <div class="bg-slate-50 dark:bg-navy-900 p-4 rounded-2xl border border-slate-200 dark:border-slate-800 flex flex-col items-center text-center">
                            <div class="w-10 h-10 rounded-xl bg-cyan-100 dark:bg-cyan-950 text-cyan-600 dark:text-cyan-400 border border-cyan-200 dark:border-cyan-800/60 flex items-center justify-center text-lg mb-2 shadow-sm">
                                <i class="fa-solid fa-water-ladder"></i>
                            </div>
                            <span class="text-xl sm:text-2xl font-extrabold font-mono text-slate-900 dark:text-white" id="viz-pools">0</span>
                            <span class="text-[11px] font-bold text-slate-700 dark:text-slate-300 mt-1">Olympic Pools/Day</span>
                            <span class="text-[9px] text-slate-400 dark:text-slate-500 mt-0.5">2.5 ML / Pool</span>
                        </div>

                        <div class="bg-slate-50 dark:bg-navy-900 p-4 rounded-2xl border border-slate-200 dark:border-slate-800 flex flex-col items-center text-center">
                            <div class="w-10 h-10 rounded-xl bg-hydro-100 dark:bg-hydro-950 text-hydro-600 dark:text-hydro-400 border border-hydro-200 dark:border-hydro-800/60 flex items-center justify-center text-lg mb-2 shadow-sm">
                                <i class="fa-solid fa-house-user"></i>
                            </div>
                            <span class="text-xl sm:text-2xl font-extrabold font-mono text-slate-900 dark:text-white" id="viz-homes">0</span>
                            <span class="text-[11px] font-bold text-slate-700 dark:text-slate-300 mt-1">Homes Supplied</span>
                            <span class="text-[9px] text-slate-400 dark:text-slate-500 mt-0.5">800 L/Home/Day</span>
                        </div>

                        <div class="bg-slate-50 dark:bg-navy-900 p-4 rounded-2xl border border-slate-200 dark:border-slate-800 flex flex-col items-center text-center">
                            <div class="w-10 h-10 rounded-xl bg-indigo-100 dark:bg-indigo-950 text-indigo-600 dark:text-indigo-400 border border-indigo-200 dark:border-indigo-800/60 flex items-center justify-center text-lg mb-2 shadow-sm">
                                <i class="fa-solid fa-bath"></i>
                            </div>
                            <span class="text-xl sm:text-2xl font-extrabold font-mono text-slate-900 dark:text-white" id="viz-tubs">0</span>
                            <span class="text-[11px] font-bold text-slate-700 dark:text-slate-300 mt-1">Bathtubs / Min</span>
                            <span class="text-[9px] text-slate-400 dark:text-slate-500 mt-0.5">150 L / Tub</span>
                        </div>

                        <div class="bg-slate-50 dark:bg-navy-900 p-4 rounded-2xl border border-slate-200 dark:border-slate-800 flex flex-col items-center text-center">
                            <div class="w-10 h-10 rounded-xl bg-teal-100 dark:bg-teal-950 text-teal-600 dark:text-teal-400 border border-teal-200 dark:border-teal-800/60 flex items-center justify-center text-lg mb-2 shadow-sm">
                                <i class="fa-solid fa-bottle-water"></i>
                            </div>
                            <span class="text-xl sm:text-2xl font-extrabold font-mono text-slate-900 dark:text-white" id="viz-bottles">0</span>
                            <span class="text-[11px] font-bold text-slate-700 dark:text-slate-300 mt-1">Bottles / Sec</span>
                            <span class="text-[9px] text-slate-400 dark:text-slate-500 mt-0.5">500 mL / Bottle</span>
                        </div>
                    </div>
                </div>

                <div class="mt-6 pt-5 border-t border-slate-200 dark:border-slate-800">
                    <div class="flex justify-between items-center text-xs text-slate-500 dark:text-slate-400 font-mono mb-2">
                        <span>Relative Hydraulic Scale Magnitude</span>
                        <span id="viz-progress-pct" class="font-bold text-hydro-600 dark:text-hydro-400">15%</span>
                    </div>
                    <div class="w-full bg-slate-100 dark:bg-navy-900 h-3 rounded-full overflow-hidden border border-slate-200 dark:border-slate-800 p-0.5">
                        <div id="viz-progress-bar" class="bg-gradient-to-r from-cyan-500 via-hydro-500 to-indigo-600 h-full rounded-full transition-all duration-300" style="width: 15%"></div>
                    </div>
                </div>
            </div>

            <div class="bg-white dark:bg-navy-800 rounded-3xl border border-slate-200 dark:border-slate-800 p-6 sm:p-8 shadow-xl flex flex-col justify-between transition-colors duration-300">
                <div>
                    <div class="flex items-center justify-between mb-2">
                        <h3 class="text-lg font-bold text-slate-900 dark:text-white flex items-center gap-2">
                            <i class="fa-solid fa-sliders text-hydro-500"></i> Logarithmic Flow Slider
                        </h3>
                        <span class="text-[10px] font-mono px-2 py-1 rounded bg-slate-100 dark:bg-navy-900 text-slate-600 dark:text-slate-300 border border-slate-200 dark:border-slate-800">
                            Continuous Scale
                        </span>
                    </div>
                    <p class="text-xs text-slate-500 dark:text-slate-400 mb-6">Drag to adjust flow from 0.01 L/s up to 10,000 L/s</p>

                    <div class="space-y-6">
                        <div>
                            <div class="flex justify-between items-center text-xs font-mono mb-3">
                                <span class="text-slate-500 dark:text-slate-400">Selected Rate</span>
                                <span id="slider-val-display" class="font-bold text-slate-900 dark:text-white bg-slate-100 dark:bg-navy-900 px-2.5 py-1 rounded-lg border border-slate-200 dark:border-slate-800">10 L/s</span>
                            </div>
                            <input type="range" id="flow-slider" min="0" max="1000" step="1" value="300" 
                                class="w-full h-3 bg-slate-200 dark:bg-navy-900 rounded-lg appearance-none cursor-pointer border border-slate-300 dark:border-slate-800">
                        </div>

                        <div class="space-y-2 text-xs font-mono text-slate-500 dark:text-slate-400 bg-slate-50 dark:bg-navy-900 p-3.5 rounded-2xl border border-slate-200 dark:border-slate-800">
                            <div class="flex justify-between">
                                <span>Micro Flow:</span>
                                <span class="text-slate-800 dark:text-slate-200">0.01 L/s</span>
                            </div>
                            <div class="flex justify-between">
                                <span>Medium Main:</span>
                                <span class="text-slate-800 dark:text-slate-200">100 L/s (8.64 ML/d)</span>
                            </div>
                            <div class="flex justify-between">
                                <span>Massive Supply:</span>
                                <span class="text-slate-800 dark:text-slate-200">10,000 L/s (864 ML/d)</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Conversion History Log Section -->
        <section class="bg-white dark:bg-navy-800 rounded-3xl border border-slate-200 dark:border-slate-800 p-6 sm:p-8 shadow-xl transition-colors duration-300">
            <div class="flex flex-wrap items-center justify-between gap-4 mb-6">
                <div>
                    <h3 class="text-lg font-bold text-slate-900 dark:text-white flex items-center gap-2">
                        <i class="fa-solid fa-clock-rotate-left text-hydro-500"></i> Conversion & Velocity History Log
                    </h3>
                    <p class="text-xs text-slate-500 dark:text-slate-400 mt-1">Saves recent calculation logs automatically to local storage</p>
                </div>
                <div class="flex items-center gap-2">
                    <button id="export-history-csv-btn" class="text-xs font-semibold px-3 py-1.5 rounded-xl bg-emerald-600 text-white hover:bg-emerald-500 transition shadow flex items-center gap-1.5">
                        <i class="fa-solid fa-file-csv"></i> Export CSV
                    </button>
                    <button id="clear-history-btn" class="text-xs font-semibold px-3 py-1.5 rounded-xl bg-slate-200 dark:bg-slate-700 text-slate-700 dark:text-slate-200 hover:bg-rose-100 dark:hover:bg-rose-950/50 hover:text-rose-600 dark:hover:text-rose-400 transition">
                        <i class="fa-solid fa-trash"></i> Clear Log
                    </button>
                </div>
            </div>

            <div class="bg-slate-50 dark:bg-navy-900 rounded-2xl border border-slate-200 dark:border-slate-800 overflow-hidden">
                <div class="overflow-x-auto max-h-60">
                    <table class="w-full text-left text-xs font-mono text-slate-700 dark:text-slate-300">
                        <thead class="bg-slate-100 dark:bg-slate-950 text-slate-500 dark:text-slate-400 uppercase text-[10px] sticky top-0 border-b border-slate-200 dark:border-slate-800">
                            <tr>
                                <th class="p-3">Timestamp</th>
                                <th class="p-3">Flow Rate (L/s)</th>
                                <th class="p-3">Flow Rate (ML/d)</th>
                                <th class="p-3">Pipe Size</th>
                                <th class="p-3">Velocity</th>
                            </tr>
                        </thead>
                        <tbody id="history-log-tbody" class="divide-y divide-slate-200 dark:divide-slate-800">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- Dynamic Formulas Reference & Table -->
        <section class="bg-white dark:bg-navy-800 rounded-3xl border border-slate-200 dark:border-slate-800 overflow-hidden shadow-xl transition-colors duration-300">
            <button id="formula-toggle-btn" class="w-full p-6 text-left flex items-center justify-between bg-white dark:bg-navy-800 hover:bg-slate-50 dark:hover:bg-slate-800/50 transition">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-2xl bg-hydro-100 dark:bg-hydro-950 text-hydro-600 dark:text-hydro-400 border border-hydro-200 dark:border-hydro-800/60 flex items-center justify-center text-lg shadow-sm">
                        <i class="fa-solid fa-square-root-variable"></i>
                    </div>
                    <div>
                        <h3 class="text-base font-bold text-slate-900 dark:text-white">Formulas & Live Unit Multiplier Reference</h3>
                        <p class="text-xs text-slate-500 dark:text-slate-400">Mathematical derivation factors updating dynamically with active units</p>
                    </div>
                </div>
                <i id="formula-chevron" class="fa-solid fa-chevron-down text-slate-400 transition-transform duration-300"></i>
            </button>

            <div id="formula-content" class="hidden border-t border-slate-200 dark:border-slate-800 p-6 bg-slate-50/50 dark:bg-navy-900/50 space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-5">
                    <div class="bg-white dark:bg-navy-800 p-5 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm">
                        <h4 class="text-sm font-bold text-cyan-600 dark:text-cyan-400 mb-2 flex items-center gap-2">
                            <i class="fa-solid fa-calculator"></i> L/s to ML/d Derivation
                        </h4>
                        <p class="text-xs text-slate-600 dark:text-slate-300 leading-relaxed mb-3">
                            86,400 seconds in a full day (24h × 60m × 60s) divided by 1,000,000 Liters per Megaliter.
                        </p>
                        <div class="bg-slate-100 dark:bg-navy-900 p-3 rounded-xl font-mono text-xs text-slate-800 dark:text-slate-200 border border-slate-200 dark:border-slate-800 space-y-1">
                            <div>Flow (ML/d) = (L/s × 86,400) / 1,000,000</div>
                            <div class="text-cyan-600 dark:text-cyan-400 font-bold">1 L/s = 0.0864 ML/d</div>
                        </div>
                    </div>

                    <div class="bg-white dark:bg-navy-800 p-5 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm">
                        <h4 class="text-sm font-bold text-hydro-600 dark:text-hydro-400 mb-2 flex items-center gap-2">
                            <i class="fa-solid fa-calculator"></i> ML/d to L/s Derivation
                        </h4>
                        <p class="text-xs text-slate-600 dark:text-slate-300 leading-relaxed mb-3">
                            1,000,000 Liters divided by 86,400 seconds yields the required continuous discharge rate per second.
                        </p>
                        <div class="bg-slate-100 dark:bg-navy-900 p-3 rounded-xl font-mono text-xs text-slate-800 dark:text-slate-200 border border-slate-200 dark:border-slate-800 space-y-1">
                            <div>Flow (L/s) = (ML/d × 1,000,000) / 86,400</div>
                            <div class="text-hydro-600 dark:text-hydro-400 font-bold">1 ML/d = 11.574074 L/s</div>
                        </div>
                    </div>
                </div>

                <div class="bg-white dark:bg-navy-800 rounded-2xl border border-slate-200 dark:border-slate-800 overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs text-slate-700 dark:text-slate-300 font-mono">
                            <thead class="bg-slate-100 dark:bg-navy-900 text-slate-500 dark:text-slate-400 uppercase text-[10px] border-b border-slate-200 dark:border-slate-800">
                                <tr>
                                    <th class="p-3">Unit Name</th>
                                    <th class="p-3">Symbol</th>
                                    <th class="p-3">Multiplier to L/s</th>
                                    <th class="p-3">Multiplier to ML/d</th>
                                </tr>
                            </thead>
                            <tbody id="formula-multipliers-tbody" class="divide-y divide-slate-100 dark:divide-slate-800">
                                <!-- Rendered dynamically -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <footer class="border-t border-slate-200 dark:border-slate-800 bg-white dark:bg-navy-950 py-6 text-xs text-slate-500 dark:text-slate-400 mt-auto transition-colors duration-300">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row items-center justify-between gap-3 text-center sm:text-left">
            <div>
                <p class="font-bold text-slate-700 dark:text-slate-300">HydroConverter • Persistent State Hydraulic Application</p>
                <p class="text-[11px] text-slate-400 mt-0.5">Fully synchronized localStorage architecture</p>
            </div>
            <div class="font-mono text-[11px] bg-slate-100 dark:bg-navy-900 px-3 py-1.5 rounded-xl border border-slate-200 dark:border-slate-800">
                1 m³ = 1,000 L = 264.172 US Gal = 219.969 Imperial Gal
            </div>
        </div>
    </footer>

    <!-- Application Script -->
    <script>
        // Default Configuration State Definitions
        const DEFAULT_UNITS = [
            { id: 'ls', name: 'Liter per second', symbol: 'L/s', factorToLps: 1.0, isBuiltIn: true, note: 'Standard Metric Flow' },
            { id: 'mld', name: 'Megaliter per day', symbol: 'ML/d', factorToLps: 1000000 / 86400, isBuiltIn: true, note: '1 ML = 1,000,000 Liters' },
            { id: 'm3h', name: 'Cubic meter per hour', symbol: 'm³/h', factorToLps: 1000 / 3600, isBuiltIn: true, note: 'Pump Utility Rating' },
            { id: 'm3s', name: 'Cubic meter per sec', symbol: 'm³/s', factorToLps: 1000.0, isBuiltIn: true, note: 'Cumec / River Discharge' },
            { id: 'usgpm', name: 'US Gallons per min', symbol: 'US GPM', factorToLps: 3.785411784 / 60, isBuiltIn: true, note: '1 US Gal ≈ 3.785 L' },
            { id: 'ukgpm', name: 'Imperial Gallons/min', symbol: 'UK GPM', factorToLps: 4.54609 / 60, isBuiltIn: true, note: '1 UK Gal ≈ 4.546 L' },
            { id: 'lmin', name: 'Liter per minute', symbol: 'L/min', factorToLps: 1 / 60, isBuiltIn: true, note: 'Fixtures & Water Meters' },
            { id: 'cfs', name: 'Cubic feet per second', symbol: 'cfs', factorToLps: 28.316846592, isBuiltIn: true, note: '1 cfs ≈ 28.317 L/s' }
        ];

        const DEFAULT_PRESETS = [
            { id: 'p1', name: '🚰 Household Tap', valueLps: 0.2, displayVal: '~0.2 L/s' },
            { id: 'p2', name: '💧 DMA Leak / Night Flow', valueLps: 2.5, displayVal: '~2.5 L/s' },
            { id: 'p3', name: '⚙️ Booster Station', valueLps: 15, displayVal: '~15 L/s' },
            { id: 'p4', name: '🏭 Medium WTP', valueLps: 578.7, displayVal: '~50 ML/d' },
            { id: 'p5', name: '🌊 Major Main / River', valueLps: 2314.8, displayVal: '~200 ML/d' }
        ];

        const DEFAULT_PIPES = [
            { id: '100', name: 'DN100 (100 mm / ~4")', mm: 100 },
            { id: '150', name: 'DN150 (150 mm / ~6")', mm: 150 },
            { id: '200', name: 'DN200 (200 mm / ~8")', mm: 200 },
            { id: '250', name: 'DN250 (250 mm / ~10")', mm: 250 },
            { id: '300', name: 'DN300 (300 mm / ~12")', mm: 300 },
            { id: '400', name: 'DN400 (400 mm / ~16")', mm: 400 },
            { id: '500', name: 'DN500 (500 mm / ~20")', mm: 500 }
        ];

        // Persistent Application State Variables
        let state = {
            currentLps: 10.0,
            units: JSON.parse(JSON.stringify(DEFAULT_UNITS)),
            presets: JSON.parse(JSON.stringify(DEFAULT_PRESETS)),
            pipes: JSON.parse(JSON.stringify(DEFAULT_PIPES)),
            selectedPipeId: '150',
            customPipeMm: 150,
            historyLog: []
        };

        // Load Persistent State from localStorage
        function loadStorageState() {
            try {
                const saved = localStorage.getItem('hydroconverter_app_state');
                if (saved) {
                    const parsed = JSON.parse(saved);
                    if (parsed.currentLps !== undefined) state.currentLps = parsed.currentLps;
                    if (parsed.units && parsed.units.length) state.units = parsed.units;
                    if (parsed.presets && parsed.presets.length) state.presets = parsed.presets;
                    if (parsed.pipes && parsed.pipes.length) state.pipes = parsed.pipes;
                    if (parsed.selectedPipeId) state.selectedPipeId = parsed.selectedPipeId;
                    if (parsed.customPipeMm) state.customPipeMm = parsed.customPipeMm;
                    if (parsed.historyLog) state.historyLog = parsed.historyLog;
                }
            } catch (e) {
                console.error("Storage loading error:", e);
            }
        }

        function saveStorageState() {
            try {
                localStorage.setItem('hydroconverter_app_state', JSON.stringify(state));
            } catch (e) {
                console.error("Storage saving error:", e);
            }
        }

        // Custom Modal UI Handler (replaces alert/confirm)
        let modalConfirmCallback = null;
        function showModal(title, message, confirmText = 'Confirm', onConfirm = null) {
            document.getElementById('modal-title').textContent = title;
            document.getElementById('modal-body').textContent = message;
            document.getElementById('modal-confirm-btn').textContent = confirmText;
            modalConfirmCallback = onConfirm;

            const modal = document.getElementById('custom-modal');
            const container = document.getElementById('modal-container');
            modal.classList.remove('opacity-0', 'pointer-events-none');
            container.classList.remove('scale-95');
            container.classList.add('scale-100');
        }

        function closeModal() {
            const modal = document.getElementById('custom-modal');
            const container = document.getElementById('modal-container');
            container.classList.remove('scale-100');
            container.classList.add('scale-95');
            modal.classList.add('opacity-0', 'pointer-events-none');
            modalConfirmCallback = null;
        }

        document.getElementById('modal-cancel-btn').addEventListener('click', closeModal);
        document.getElementById('modal-confirm-btn').addEventListener('click', () => {
            if (modalConfirmCallback) modalConfirmCallback();
            closeModal();
        });

        // Precision Formatter
        function formatVal(num) {
            if (num === 0 || isNaN(num)) return '0';
            const abs = Math.abs(num);
            if (abs >= 1000000 || abs < 0.0001) {
                return num.toExponential(4);
            } else if (abs >= 100) {
                return num.toFixed(2);
            } else if (abs >= 1) {
                return num.toFixed(3);
            } else {
                return num.toFixed(4);
            }
        }

        // Render Dynamic Presets
        function renderPresets() {
            const container = document.getElementById('presets-container');
            container.innerHTML = '';
            state.presets.forEach(p => {
                const btn = document.createElement('div');
                btn.className = "inline-flex items-center rounded-xl bg-slate-800/80 border border-slate-700/80 text-slate-200 text-xs shadow-sm overflow-hidden group";
                btn.innerHTML = `
                    <button onclick="setFlowLps(${p.valueLps})" class="px-3.5 py-2 hover:bg-slate-700 active:scale-95 transition flex items-center gap-2">
                        <span>${p.name}</span>
                        <span class="font-mono text-cyan-400 font-bold">${p.displayVal}</span>
                    </button>
                    <button onclick="removePreset('${p.id}')" class="px-2 py-2 text-slate-500 hover:text-rose-400 hover:bg-slate-700 transition border-l border-slate-700/60" title="Delete Preset">
                        <i class="fa-solid fa-xmark text-xs"></i>
                    </button>
                `;
                container.appendChild(btn);
            });
        }

        function removePreset(id) {
            state.presets = state.presets.filter(p => p.id !== id);
            saveStorageState();
            renderPresets();
            showToast("Preset removed");
        }

        // Render Dynamic Unit Matrix
        function renderUnitGrid() {
            const grid = document.getElementById('units-grid');
            grid.innerHTML = '';

            state.units.forEach(u => {
                const card = document.createElement('div');
                card.className = "bg-slate-50 dark:bg-navy-900 p-4 rounded-2xl border border-slate-200 dark:border-slate-800 hover:border-hydro-500/50 transition flex flex-col justify-between group";
                card.innerHTML = `
                    <div>
                        <div class="flex justify-between items-center mb-1.5">
                            <label for="unit-${u.id}" class="text-xs font-bold text-slate-700 dark:text-slate-300 flex items-center gap-1.5">
                                ${u.name}
                                ${!u.isBuiltIn ? `<button onclick="removeCustomUnit('${u.id}')" class="text-slate-400 hover:text-rose-400 transition" title="Delete custom unit"><i class="fa-solid fa-circle-xmark text-xs"></i></button>` : ''}
                            </label>
                            <span class="text-[10px] font-mono font-bold text-hydro-600 dark:text-hydro-400 bg-hydro-50 dark:bg-hydro-950/80 px-2 py-0.5 rounded border border-hydro-200 dark:border-hydro-800/60">${u.symbol}</span>
                        </div>
                        <div class="relative">
                            <input type="number" id="unit-${u.id}" step="any" placeholder="0" class="unit-input w-full bg-white dark:bg-slate-900 border border-slate-300 dark:border-slate-700 focus:border-hydro-500 focus:ring-2 focus:ring-hydro-500/20 rounded-xl py-2.5 pl-3 pr-9 text-lg font-mono text-slate-900 dark:text-white font-semibold outline-none transition">
                            <button onclick="copyUnitValue('unit-${u.id}', '${u.symbol}')" class="absolute right-2.5 top-3 text-slate-400 hover:text-hydro-500 transition" title="Copy ${u.symbol}">
                                <i class="fa-regular fa-copy text-xs"></i>
                            </button>
                        </div>
                    </div>
                    <span class="text-[10px] text-slate-400 dark:text-slate-500 mt-2 font-mono">${u.note || ''}</span>
                `;
                grid.appendChild(card);

                // Add Input Listener
                const inputElem = card.querySelector(`#unit-${u.id}`);
                inputElem.addEventListener('input', (e) => {
                    const val = parseFloat(e.target.value);
                    state.currentLps = isNaN(val) ? 0 : val * u.factorToLps;
                    updateAll(`unit-${u.id}`);
                });
            });

            renderFormulasTable();
        }

        function removeCustomUnit(id) {
            state.units = state.units.filter(u => u.id !== id);
            saveStorageState();
            renderUnitGrid();
            updateAll();
            showToast("Custom unit deleted");
        }

        // Render Pipes Dropdown
        function renderPipesDropdown() {
            const select = document.getElementById('pipe-preset-select');
            select.innerHTML = '';
            state.pipes.forEach(p => {
                const opt = document.createElement('option');
                opt.value = p.id;
                opt.textContent = p.name;
                if (p.id === state.selectedPipeId) opt.selected = true;
                select.appendChild(opt);
            });
            const customOpt = document.createElement('option');
            customOpt.value = 'custom';
            customOpt.textContent = 'Custom Diameter Input';
            if (state.selectedPipeId === 'custom') customOpt.selected = true;
            select.appendChild(customOpt);
        }

        // Render Formulas Multiplier Reference Table
        function renderFormulasTable() {
            const tbody = document.getElementById('formula-multipliers-tbody');
            tbody.innerHTML = '';
            state.units.forEach(u => {
                const factorToMld = u.factorToLps * 0.0864;
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50 dark:hover:bg-slate-800/50";
                tr.innerHTML = `
                    <td class="p-3 font-sans font-medium text-slate-900 dark:text-white">${u.name}</td>
                    <td class="p-3 text-hydro-600 dark:text-hydro-400 font-bold">${u.symbol}</td>
                    <td class="p-3">${u.factorToLps.toFixed(6)}</td>
                    <td class="p-3">${factorToMld.toFixed(6)}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        // Render History Log Table
        function renderHistoryTable() {
            const tbody = document.getElementById('history-log-tbody');
            tbody.innerHTML = '';
            if (state.historyLog.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="p-4 text-center text-slate-400 italic">No calculation logs recorded yet.</td></tr>`;
                return;
            }
            state.historyLog.slice().reverse().forEach(log => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-100 dark:hover:bg-slate-800/50";
                tr.innerHTML = `
                    <td class="p-3 text-slate-400">${log.time}</td>
                    <td class="p-3 font-bold text-cyan-500">${log.lps} L/s</td>
                    <td class="p-3 text-hydro-500">${log.mld} ML/d</td>
                    <td class="p-3">${log.pipe}</td>
                    <td class="p-3 font-bold ${log.velColor}">${log.velocity} m/s</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function addHistoryLog(velocityMs, velColorClass) {
            const now = new Date();
            const timeStr = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });
            const mld = state.currentLps * 0.0864;
            
            state.historyLog.push({
                time: timeStr,
                lps: formatVal(state.currentLps),
                mld: formatVal(mld),
                pipe: `${state.customPipeMm} mm`,
                velocity: velocityMs.toFixed(2),
                velColor: velColorClass
            });

            if (state.historyLog.length > 20) state.historyLog.shift(); // Limit to 20 logs
            saveStorageState();
            renderHistoryTable();
        }

        // Master Application Sync Function
        let logDebounceTimer = null;
        function updateAll(sourceId = null) {
            // 1. Primary Spotlight Inputs
            const spotLsInput = document.getElementById('spot-ls');
            const spotMldInput = document.getElementById('spot-mld');
            const lsDailyM3 = document.getElementById('ls-daily-m3');
            const mldHourlyM3 = document.getElementById('mld-hourly-m3');

            if (sourceId !== 'spot-ls') {
                spotLsInput.value = state.currentLps === 0 ? '' : formatVal(state.currentLps);
            }
            const currentMld = state.currentLps * 0.0864;
            if (sourceId !== 'spot-mld') {
                spotMldInput.value = currentMld === 0 ? '' : formatVal(currentMld);
            }

            lsDailyM3.textContent = `${formatVal(state.currentLps * 86.4)} m³/day`;
            mldHourlyM3.textContent = `${formatVal(currentMld * 1000 / 24)} m³/h`;

            // 2. Multi-Unit Matrix Sync
            state.units.forEach(u => {
                const elementId = `unit-${u.id}`;
                if (sourceId !== elementId) {
                    const inputElem = document.getElementById(elementId);
                    if (inputElem) {
                        const converted = state.currentLps / u.factorToLps;
                        inputElem.value = converted === 0 ? '' : formatVal(converted);
                    }
                }
            });

            // 3. Visual Impact Benchmarks
            const dailyLiters = state.currentLps * 86400;
            const poolsPerDay = dailyLiters / 2500000;
            const homesCount = dailyLiters / 800;
            const tubsPerMin = (state.currentLps * 60) / 150;
            const bottlesPerSec = state.currentLps / 0.5;

            document.getElementById('viz-pools').textContent = poolsPerDay < 0.01 && poolsPerDay > 0 ? '< 0.01' : poolsPerDay.toLocaleString(undefined, {maximumFractionDigits: 2});
            document.getElementById('viz-homes').textContent = homesCount < 1 && homesCount > 0 ? '< 1' : Math.round(homesCount).toLocaleString();
            document.getElementById('viz-tubs').textContent = tubsPerMin < 0.1 && tubsPerMin > 0 ? '< 0.1' : formatVal(tubsPerMin);
            document.getElementById('viz-bottles').textContent = bottlesPerSec < 0.1 && bottlesPerSec > 0 ? '< 0.1' : formatVal(bottlesPerSec);

            // Scale Bar
            let pct = 0;
            let badgeText = 'Domestic Scale';
            if (state.currentLps <= 1) {
                pct = (state.currentLps / 1) * 15;
                badgeText = 'Tap / Fixture Scale';
            } else if (state.currentLps <= 20) {
                pct = 15 + ((state.currentLps - 1) / 19) * 25;
                badgeText = 'Sub-Main / Booster';
            } else if (state.currentLps <= 200) {
                pct = 40 + ((state.currentLps - 20) / 180) * 30;
                badgeText = 'Treatment / District Main';
            } else {
                pct = Math.min(100, 70 + (Math.log10(state.currentLps / 200) * 15));
                badgeText = 'River / Regional Intake';
            }

            const clampedPct = Math.max(3, Math.min(100, Math.round(pct)));
            document.getElementById('viz-progress-bar').style.width = `${clampedPct}%`;
            document.getElementById('viz-progress-pct').textContent = `${clampedPct}%`;
            document.getElementById('viz-scale-badge').textContent = badgeText;

            // 4. Logarithmic Slider Sync
            const flowSlider = document.getElementById('flow-slider');
            if (sourceId !== 'flow-slider') {
                const minLog = Math.log(0.01);
                const maxLog = Math.log(10000);
                const currentLog = Math.log(Math.max(0.01, Math.min(10000, state.currentLps)));
                const sliderPos = ((currentLog - minLog) / (maxLog - minLog)) * 1000;
                flowSlider.value = isNaN(sliderPos) ? 0 : sliderPos;
            }
            document.getElementById('slider-val-display').textContent = `${formatVal(state.currentLps)} L/s (${formatVal(currentMld)} ML/d)`;

            // 5. Compute Pipe Velocity
            updatePipeVelocity();

            // Save state & debounce history log creation
            saveStorageState();
            clearTimeout(logDebounceTimer);
            logDebounceTimer = setTimeout(() => {
                const diameterMeters = state.customPipeMm / 1000;
                if (diameterMeters > 0 && state.currentLps > 0) {
                    const areaM2 = Math.PI * Math.pow(diameterMeters / 2, 2);
                    const velocityMs = (state.currentLps / 1000) / areaM2;
                    let colorClass = "text-emerald-500";
                    if (velocityMs < 0.6) colorClass = "text-amber-500";
                    if (velocityMs > 2.0) colorClass = "text-rose-500";
                    addHistoryLog(velocityMs, colorClass);
                }
            }, 1000);
        }

        // Pipe Velocity Logic
        function updatePipeVelocity() {
            const diameterMeters = state.customPipeMm / 1000;
            if (diameterMeters <= 0) return;

            const areaM2 = Math.PI * Math.pow(diameterMeters / 2, 2);
            const flowM3s = state.currentLps / 1000;
            const velocityMs = flowM3s / areaM2;

            document.getElementById('pipe-area-display').textContent = `Area: ${areaM2.toFixed(4)} m² (${state.customPipeMm} mm ID)`;
            document.getElementById('pipe-velocity-val').textContent = velocityMs.toFixed(2);

            const barPct = Math.max(2, Math.min(100, (velocityMs / 3.5) * 100));
            const velocityBar = document.getElementById('velocity-bar');
            velocityBar.style.width = `${barPct}%`;

            const velocityStatusPill = document.getElementById('velocity-status-pill');
            const velocityExplanation = document.getElementById('velocity-explanation');

            if (velocityMs < 0.6) {
                velocityStatusPill.className = "text-xs font-bold font-mono px-3 py-1.5 rounded-full bg-amber-100 dark:bg-amber-950/80 text-amber-700 dark:text-amber-300 border border-amber-300 dark:border-amber-800";
                velocityStatusPill.innerHTML = `<i class="fa-solid fa-triangle-exclamation mr-1"></i> Low Velocity (<0.6 m/s)`;
                velocityBar.className = "bg-amber-400 h-full rounded-full transition-all duration-300";
                velocityExplanation.innerHTML = `<i class="fa-solid fa-circle-info text-amber-500 mr-1"></i> Velocity is low (<0.6 m/s). Sediment deposition or water stagnation may occur in mains. Consider a smaller pipe size.`;
            } else if (velocityMs <= 2.0) {
                velocityStatusPill.className = "text-xs font-bold font-mono px-3 py-1.5 rounded-full bg-emerald-100 dark:bg-emerald-950/80 text-emerald-700 dark:text-emerald-300 border border-emerald-300 dark:border-emerald-800";
                velocityStatusPill.innerHTML = `<i class="fa-solid fa-circle-check mr-1"></i> Optimal Range (0.6 - 2.0 m/s)`;
                velocityBar.className = "bg-emerald-500 h-full rounded-full transition-all duration-300";
                velocityExplanation.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-500 mr-1"></i> Optimal hydraulics! Velocity is within standard engineering limits (0.6 - 2.0 m/s), preventing sedimentation while keeping headloss minimal.`;
            } else if (velocityMs <= 3.0) {
                velocityStatusPill.className = "text-xs font-bold font-mono px-3 py-1.5 rounded-full bg-orange-100 dark:bg-orange-950/80 text-orange-700 dark:text-orange-300 border border-orange-300 dark:border-orange-800";
                velocityStatusPill.innerHTML = `<i class="fa-solid fa-gauge-high mr-1"></i> High Velocity (>2.0 m/s)`;
                velocityBar.className = "bg-orange-500 h-full rounded-full transition-all duration-300";
                velocityExplanation.innerHTML = `<i class="fa-solid fa-circle-info text-orange-500 mr-1"></i> Velocity is elevated (>2.0 m/s). This increases friction headloss and pumping energy requirements. Consider a larger pipe diameter.`;
            } else {
                velocityStatusPill.className = "text-xs font-bold font-mono px-3 py-1.5 rounded-full bg-rose-100 dark:bg-rose-950/80 text-rose-700 dark:text-rose-300 border border-rose-300 dark:border-rose-800";
                velocityStatusPill.innerHTML = `<i class="fa-solid fa-radiation mr-1"></i> Excessive (>3.0 m/s)`;
                velocityBar.className = "bg-rose-500 h-full rounded-full transition-all duration-300";
                velocityExplanation.innerHTML = `<i class="fa-solid fa-circle-xmark text-rose-500 mr-1"></i> Excessive velocity (>3.0 m/s)! Severe risk of pipe erosion, water hammer surge pressures, and energy loss. Select a larger pipe diameter.`;
            }
        }

        // Spotlight Input Listeners
        document.getElementById('spot-ls').addEventListener('input', (e) => {
            const val = parseFloat(e.target.value);
            state.currentLps = isNaN(val) ? 0 : val;
            updateAll('spot-ls');
        });

        document.getElementById('spot-mld').addEventListener('input', (e) => {
            const val = parseFloat(e.target.value);
            state.currentLps = isNaN(val) ? 0 : val / 0.0864;
            updateAll('spot-mld');
        });

        // Slider Listener
        document.getElementById('flow-slider').addEventListener('input', (e) => {
            const val = parseFloat(e.target.value);
            const minLog = Math.log(0.01);
            const maxLog = Math.log(10000);
            const logVal = minLog + (val / 1000) * (maxLog - minLog);
            state.currentLps = Math.exp(logVal);
            updateAll('flow-slider');
        });

        // Add Custom Preset Handler
        document.getElementById('add-preset-btn').addEventListener('click', () => {
            const name = prompt("Enter preset label (e.g., '🚰 Wash Pump'):");
            if (!name) return;
            const lpsStr = prompt("Enter flow rate in L/s:");
            const lpsVal = parseFloat(lpsStr);
            if (isNaN(lpsVal) || lpsVal <= 0) {
                showToast("Invalid flow rate input.");
                return;
            }

            const newPreset = {
                id: 'p_' + Date.now(),
                name: name,
                valueLps: lpsVal,
                displayVal: `~${formatVal(lpsVal)} L/s`
            };

            state.presets.push(newPreset);
            saveStorageState();
            renderPresets();
            showToast("Custom preset added!");
        });

        // Add Custom Unit Handler
        document.getElementById('add-custom-unit-btn').addEventListener('click', () => {
            const name = prompt("Enter unit full name (e.g., 'Barrel per day'):");
            if (!name) return;
            const symbol = prompt("Enter unit symbol (e.g., 'BPD'):");
            if (!symbol) return;
            const factorStr = prompt(`How many L/s is 1 ${symbol}? (e.g. 0.00184):`);
            const factorVal = parseFloat(factorStr);
            if (isNaN(factorVal) || factorVal <= 0) {
                showToast("Invalid conversion multiplier.");
                return;
            }

            const newUnit = {
                id: 'u_' + Date.now(),
                name: name,
                symbol: symbol,
                factorToLps: factorVal,
                isBuiltIn: false,
                note: `User Custom Unit (1 ${symbol} = ${factorVal} L/s)`
            };

            state.units.push(newUnit);
            saveStorageState();
            renderUnitGrid();
            updateAll();
            showToast("Custom unit added to matrix!");
        });

        // Add Custom Pipe Size Handler
        document.getElementById('add-pipe-btn').addEventListener('click', () => {
            const mmStr = prompt("Enter new pipe internal diameter in millimeters (mm):");
            const mmVal = parseFloat(mmStr);
            if (isNaN(mmVal) || mmVal <= 0) {
                showToast("Invalid pipe diameter.");
                return;
            }
            const name = `DN${Math.round(mmVal)} (${mmVal} mm Custom)`;
            const pipeId = 'pipe_' + Date.now();

            state.pipes.push({ id: pipeId, name: name, mm: mmVal });
            state.selectedPipeId = pipeId;
            state.customPipeMm = mmVal;

            saveStorageState();
            renderPipesDropdown();
            document.getElementById('custom-diameter-val').value = mmVal;
            updateAll();
            showToast("New pipe diameter added!");
        });

        // Pipe Selection Event Handlers
        const pipePresetSelect = document.getElementById('pipe-preset-select');
        const customDiameterVal = document.getElementById('custom-diameter-val');

        pipePresetSelect.addEventListener('change', (e) => {
            state.selectedPipeId = e.target.value;
            if (e.target.value === 'custom') {
                getCustomDiameter();
            } else {
                const found = state.pipes.find(p => p.id === e.target.value);
                if (found) {
                    state.customPipeMm = found.mm;
                    customDiameterVal.value = found.mm;
                }
            }
            updateAll();
        });

        customDiameterVal.addEventListener('input', getCustomDiameter);

        function getCustomDiameter() {
            const val = parseFloat(customDiameterVal.value) || 0;
            const unitRadio = document.querySelector('input[name="pipe-unit"]:checked').value;
            if (unitRadio === 'inch') {
                state.customPipeMm = val * 25.4;
            } else {
                state.customPipeMm = val;
            }
            state.selectedPipeId = 'custom';
            pipePresetSelect.value = 'custom';
            updateAll();
        }

        document.querySelectorAll('input[name="pipe-unit"]').forEach(radio => {
            radio.addEventListener('change', getCustomDiameter);
        });

        function setFlowLps(lps) {
            state.currentLps = lps;
            updateAll();
        }

        // Factory Reset Handler
        document.getElementById('factory-reset-btn').addEventListener('click', () => {
            showModal(
                "Reset to Factory Defaults",
                "Are you sure you want to reset all custom units, user presets, custom pipe sizes, and conversion logs?",
                "Yes, Reset Everything",
                () => {
                    localStorage.removeItem('hydroconverter_app_state');
                    state = {
                        currentLps: 10.0,
                        units: JSON.parse(JSON.stringify(DEFAULT_UNITS)),
                        presets: JSON.parse(JSON.stringify(DEFAULT_PRESETS)),
                        pipes: JSON.parse(JSON.stringify(DEFAULT_PIPES)),
                        selectedPipeId: '150',
                        customPipeMm: 150,
                        historyLog: []
                    };
                    renderPresets();
                    renderUnitGrid();
                    renderPipesDropdown();
                    renderHistoryTable();
                    customDiameterVal.value = 150;
                    updateAll();
                    showToast("App reset to factory defaults!");
                }
            );
        });

        // Clear Log Handler
        document.getElementById('clear-history-btn').addEventListener('click', () => {
            state.historyLog = [];
            saveStorageState();
            renderHistoryTable();
            showToast("Conversion history cleared!");
        });

        // Export History CSV
        document.getElementById('export-history-csv-btn').addEventListener('click', () => {
            if (state.historyLog.length === 0) {
                showToast("History log is empty.");
                return;
            }
            let csv = "Timestamp,Flow (L/s),Flow (ML/d),Pipe Diameter,Velocity (m/s)\n";
            state.historyLog.forEach(row => {
                csv += `"${row.time}","${row.lps}","${row.mld}","${row.pipe}","${row.velocity}"\n`;
            });
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `HydroConverter_Log_${Date.now()}.csv`;
            a.click();
            URL.revokeObjectURL(url);
            showToast("CSV Log Downloaded!");
        });

        // Formula Accordion
        const formulaToggleBtn = document.getElementById('formula-toggle-btn');
        const formulaContent = document.getElementById('formula-content');
        const formulaChevron = document.getElementById('formula-chevron');

        formulaToggleBtn.addEventListener('click', () => {
            const isHidden = formulaContent.classList.contains('hidden');
            if (isHidden) {
                formulaContent.classList.remove('hidden');
                formulaChevron.classList.add('rotate-180');
            } else {
                formulaContent.classList.add('hidden');
                formulaChevron.classList.remove('rotate-180');
            }
        });

        // Theme Manager
        const themeToggleBtn = document.getElementById('theme-toggle');
        function setTheme(isDark) {
            if (isDark) {
                document.documentElement.classList.add('dark');
                localStorage.setItem('hydroconverter_theme', 'dark');
            } else {
                document.documentElement.classList.remove('dark');
                localStorage.setItem('hydroconverter_theme', 'light');
            }
        }

        const savedTheme = localStorage.getItem('hydroconverter_theme');
        if (savedTheme === 'dark' || (!savedTheme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            setTheme(true);
        } else {
            setTheme(false);
        }

        themeToggleBtn.addEventListener('click', () => {
            const isDark = document.documentElement.classList.contains('dark');
            setTheme(!isDark);
        });

        // Toast Feedback
        function showToast(message) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toast-message');
            toastMsg.textContent = message;
            toast.classList.remove('opacity-0', 'translate-y-24');
            toast.classList.add('opacity-100', 'translate-y-0');
            setTimeout(() => {
                toast.classList.remove('opacity-100', 'translate-y-0');
                toast.classList.add('opacity-0', 'translate-y-24');
            }, 2500);
        }

        function copyToClipboard(text) {
            const textarea = document.createElement('textarea');
            textarea.value = text;
            textarea.style.position = 'fixed';
            document.body.appendChild(textarea);
            textarea.select();
            try {
                document.execCommand('copy');
                showToast("Copied to clipboard!");
            } catch (err) {
                console.error("Copy failed", err);
            }
            document.body.removeChild(textarea);
        }

        function copyUnitValue(inputId, label) {
            const input = document.getElementById(inputId);
            if (input && input.value) {
                copyToClipboard(`${input.value} ${label}`);
            }
        }

        function copySpotlightPair() {
            const mld = state.currentLps * 0.0864;
            copyToClipboard(`Water Flow Rate: ${formatVal(state.currentLps)} L/s = ${formatVal(mld)} ML/d`);
        }

        function copyFullReport() {
            const mld = state.currentLps * 0.0864;
            let report = `HYDROCONVERTER - WATER FLOW & HYDRAULIC REPORT\n` +
                `====================================================\n`;
            
            state.units.forEach(u => {
                const converted = state.currentLps / u.factorToLps;
                report += `• ${u.name} (${u.symbol}): ${formatVal(converted)}\n`;
            });

            const velocityMs = (state.currentLps / 1000) / (Math.PI * Math.pow((state.customPipeMm / 1000) / 2, 2));

            report += `----------------------------------------------------\n` +
                `PIPE HYDRAULIC ANALYSIS:\n` +
                `• Selected Pipe Size: ${state.customPipeMm} mm ID\n` +
                `• Computed Water Velocity: ${velocityMs.toFixed(2)} m/s\n` +
                `----------------------------------------------------\n` +
                `DAILY BENCHMARKS:\n` +
                `• Daily Volume: ${formatVal(state.currentLps * 86.4)} m³/day\n` +
                `• Households Supplied: ${Math.round((state.currentLps * 86400) / 800).toLocaleString()}\n` +
                `• Olympic Swimming Pools/Day: ${((state.currentLps * 86400) / 2500000).toFixed(2)}`;

            copyToClipboard(report);
        }

        // Initialize App
        window.addEventListener('DOMContentLoaded', () => {
            loadStorageState();
            renderPresets();
            renderUnitGrid();
            renderPipesDropdown();
            renderHistoryTable();
            document.getElementById('custom-diameter-val').value = state.customPipeMm;
            updateAll();
        });
    </script>
</body>
</html>
```
