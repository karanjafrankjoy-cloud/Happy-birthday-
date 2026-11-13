<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#06b6d4">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>J.A.R.V.I.S. — Ultimate Cross-Platform AI HUD</title>
    
    <!-- Dynamic PWA Manifest Link -->
    <link id="pwa-manifest" rel="manifest" href="">

    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter, Orbitron (HUD Display) & JetBrains Mono -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&family=Orbitron:wght@500;700;900&display=swap" rel="stylesheet">
    <!-- Marked.js for Markdown Parsing -->
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <!-- Highlight.js for Syntax Highlighting -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/atom-one-dark.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        hud: ['Orbitron', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace'],
                    },
                    colors: {
                        jarvis: {
                            cyan: '#06b6d4',
                            glow: '#22d3ee',
                            obsidian: '#05080f',
                            panel: '#0a101d',
                            border: '#16243d'
                        }
                    }
                }
            }
        }
    </script>

    <style>
        ::-webkit-scrollbar { width: 5px; height: 5px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: rgba(6, 182, 212, 0.25); border-radius: 9999px; }
        ::-webkit-scrollbar-thumb:hover { background: rgba(6, 182, 212, 0.5); }

        .dark .hud-bg {
            background-color: #05080f;
            background-image: 
                radial-gradient(circle at 50% 0%, rgba(6, 182, 212, 0.08) 0%, transparent 55%),
                linear-gradient(to right, rgba(6, 182, 212, 0.03) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(6, 182, 212, 0.03) 1px, transparent 1px);
            background-size: 100% 100%, 32px 32px, 32px 32px;
        }

        @keyframes reactorSpin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        @keyframes reactorSpinReverse {
            0% { transform: rotate(360deg); }
            100% { transform: rotate(0deg); }
        }
        .arc-ring-outer { animation: reactorSpin 14s linear infinite; }
        .arc-ring-inner { animation: reactorSpinReverse 9s linear infinite; }

        @keyframes voicePulse {
            0% { box-shadow: 0 0 0 0 rgba(244, 63, 94, 0.65); }
            70% { box-shadow: 0 0 0 14px rgba(244, 63, 94, 0); }
            100% { box-shadow: 0 0 0 0 rgba(244, 63, 94, 0); }
        }
        .listening-active { animation: voicePulse 1.5s infinite; }

        .prose-content p { margin-bottom: 0.75rem; line-height: 1.65; }
        .prose-content p:last-child { margin-bottom: 0; }
        .prose-content ul { list-style-type: disc; padding-left: 1.5rem; margin-bottom: 0.75rem; }
        .prose-content ol { list-style-type: decimal; padding-left: 1.5rem; margin-bottom: 0.75rem; }
        .prose-content li { margin-bottom: 0.25rem; }
        .prose-content h1, .prose-content h2, .prose-content h3 { font-weight: 600; margin-top: 1.15rem; margin-bottom: 0.45rem; }
        .prose-content h1 { font-size: 1.2rem; }
        .prose-content h2 { font-size: 1.08rem; }
        .prose-content h3 { font-size: 0.98rem; }
        .prose-content strong { font-weight: 600; }
        .prose-content a { color: #06b6d4; text-decoration: underline; }
        .prose-content blockquote { border-left: 3px solid #06b6d4; padding-left: 0.875rem; font-style: italic; opacity: 0.85; margin: 0.75rem 0; }
        .prose-content table { width: 100%; border-collapse: collapse; margin: 0.875rem 0; font-size: 0.85rem; overflow-x: auto; display: block; }
        .prose-content th, .prose-content td { border: 1px solid rgba(6, 182, 212, 0.25); padding: 0.5rem 0.75rem; text-align: left; }
        .prose-content th { background-color: rgba(6, 182, 212, 0.12); font-weight: 600; }
        .prose-content code:not(pre code) { font-family: 'JetBrains Mono', monospace; font-size: 0.85em; padding: 0.15rem 0.35rem; border-radius: 0.375rem; background-color: rgba(6, 182, 212, 0.15); color: #22d3ee; }
        html:not(.dark) .prose-content code:not(pre code) { color: #0e7490; background-color: rgba(6, 182, 212, 0.12); }
        .prose-content pre { margin: 0.875rem 0; border-radius: 0.75rem; overflow: hidden; background-color: #070b14; border: 1px solid rgba(6, 182, 212, 0.25); }
        .prose-content pre code { font-family: 'JetBrains Mono', monospace; font-size: 0.84rem; padding: 1rem; display: block; overflow-x: auto; }

        @keyframes pulseDot {
            0%, 80%, 100% { transform: scale(0.6); opacity: 0.4; }
            40% { transform: scale(1); opacity: 1; }
        }
        .typing-dot { animation: pulseDot 1.4s infinite ease-in-out both; }
        .typing-dot:nth-child(1) { animation-delay: -0.32s; }
        .typing-dot:nth-child(2) { animation-delay: -0.16s; }
    </style>
</head>
<body class="bg-slate-100 text-slate-900 dark:bg-jarvis-obsidian dark:text-slate-100 transition-colors duration-200 h-screen w-screen overflow-hidden flex flex-col select-none sm:select-auto">

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed top-16 right-4 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- Device Command Execution Modal -->
    <div id="command-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-black/70 backdrop-blur-sm p-4">
        <div class="bg-white dark:bg-jarvis-panel border border-slate-200 dark:border-cyan-500/30 rounded-2xl max-w-lg w-full p-5 sm:p-6 shadow-2xl shadow-cyan-500/10 space-y-4">
            <div class="flex items-center justify-between border-b border-slate-200 dark:border-jarvis-border pb-3">
                <div class="flex items-center gap-2.5">
                    <div class="w-9 h-9 rounded-xl bg-cyan-500/15 border border-cyan-500/30 flex items-center justify-center text-cyan-500">
                        <i data-lucide="terminal" class="w-5 h-5"></i>
                    </div>
                    <div>
                        <h3 class="font-hud font-bold text-sm tracking-wider text-slate-900 dark:text-cyan-400">HARDWARE & DEVICE COMMAND ENGINE</h3>
                        <p class="text-xs text-slate-500 dark:text-slate-400">Android 12 & Windows 11 Pro Integration</p>
                    </div>
                </div>
                <button onclick="toggleCommandModal(false)" class="p-2 rounded-xl text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-800 transition">
                    <i data-lucide="x" class="w-4 h-4"></i>
                </button>
            </div>

            <div id="command-modal-body" class="space-y-3 text-xs">
                <!-- Dynamically populated hardware stats or command output -->
            </div>

            <div class="flex justify-end pt-2">
                <button onclick="toggleCommandModal(false)" class="px-4 py-2 rounded-xl bg-slate-200 dark:bg-slate-800 hover:bg-slate-300 dark:hover:bg-slate-700 text-xs font-medium transition">
                    Close
                </button>
            </div>
        </div>
    </div>

    <!-- Install PWA Modal (Android 12 & Windows 11 Pro Guide) -->
    <div id="install-modal" class="fixed inset-0 z-50 hidden items-center justify-center bg-black/70 backdrop-blur-sm p-4">
        <div class="bg-white dark:bg-jarvis-panel border border-slate-200 dark:border-cyan-500/30 rounded-2xl max-w-lg w-full p-5 sm:p-6 shadow-2xl shadow-cyan-500/10 space-y-5 relative">
            <div class="flex items-center justify-between border-b border-slate-200 dark:border-jarvis-border pb-3">
                <div class="flex items-center gap-2.5">
                    <div class="w-9 h-9 rounded-xl bg-cyan-500/15 border border-cyan-500/30 flex items-center justify-center text-cyan-500">
                        <i data-lucide="download-cloud" class="w-5 h-5"></i>
                    </div>
                    <div>
                        <h3 class="font-hud font-bold text-sm tracking-wider text-slate-900 dark:text-cyan-400">INSTALL J.A.R.V.I.S.</h3>
                        <p class="text-xs text-slate-500 dark:text-slate-400">Cross-Platform Progressive Web App</p>
                    </div>
                </div>
                <button onclick="toggleInstallModal(false)" class="p-2 rounded-xl text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-800 transition">
                    <i data-lucide="x" class="w-4 h-4"></i>
                </button>
            </div>

            <div id="native-install-banner" class="hidden p-3.5 rounded-xl bg-cyan-500/10 border border-cyan-500/30 flex items-center justify-between gap-3">
                <div class="text-xs">
                    <div class="font-semibold text-cyan-600 dark:text-cyan-300">Direct App Installation Ready</div>
                    <div class="text-slate-600 dark:text-slate-400">Install J.A.R.V.I.S. directly to your home screen or taskbar.</div>
                </div>
                <button onclick="triggerNativeInstall()" class="px-3.5 py-2 rounded-xl bg-cyan-600 hover:bg-cyan-500 text-white text-xs font-semibold shrink-0 shadow-md shadow-cyan-500/20 transition">
                    Install Now
                </button>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-xs">
                <div class="p-3.5 rounded-xl bg-slate-50 dark:bg-jarvis-obsidian border border-slate-200 dark:border-jarvis-border space-y-2">
                    <div class="flex items-center gap-2 font-semibold text-emerald-600 dark:text-emerald-400">
                        <i data-lucide="smartphone" class="w-4 h-4"></i>
                        <span>Android 12 (Chrome / Edge)</span>
                    </div>
                    <ol class="list-decimal pl-4 space-y-1.5 text-slate-600 dark:text-slate-300 leading-relaxed">
                        <li>Open this app in <strong>Chrome</strong> or <strong>Edge</strong> on your Android 12 device.</li>
                        <li>Tap the <strong>⋮ (Three Dots)</strong> menu in the top-right corner.</li>
                        <li>Select <strong>"Add to Home screen"</strong> or <strong>"Install app"</strong>.</li>
                        <li>Launch J.A.R.V.I.S. full-screen from your app drawer.</li>
                    </ol>
                </div>

                <div class="p-3.5 rounded-xl bg-slate-50 dark:bg-jarvis-obsidian border border-slate-200 dark:border-jarvis-border space-y-2">
                    <div class="flex items-center gap-2 font-semibold text-sky-600 dark:text-cyan-400">
                        <i data-lucide="monitor" class="w-4 h-4"></i>
                        <span>Windows 11 Pro (Edge / Chrome)</span>
                    </div>
                    <ol class="list-decimal pl-4 space-y-1.5 text-slate-600 dark:text-slate-300 leading-relaxed">
                        <li>Open in <strong>Microsoft Edge</strong> or <strong>Google Chrome</strong>.</li>
                        <li>Click the <strong>App Install icon</strong> in the right side of the address bar.</li>
                        <li>Click <strong>Install</strong> to create a standalone desktop window.</li>
                        <li>Pin J.A.R.V.I.S. to your <strong>Windows 11 Taskbar</strong>.</li>
                    </ol>
                </div>
            </div>

            <div class="flex justify-end pt-1">
                <button onclick="toggleInstallModal(false)" class="px-4 py-2 rounded-xl bg-slate-200 dark:bg-slate-800 hover:bg-slate-300 dark:hover:bg-slate-700 text-xs font-medium transition">
                    Acknowledge
                </button>
            </div>
        </div>
    </div>

    <!-- Main Layout Wrapper -->
    <div class="flex h-full w-full overflow-hidden relative">

        <!-- Mobile Sidebar Backdrop -->
        <div id="sidebar-backdrop" onclick="toggleSidebar()" class="fixed inset-0 bg-black/60 backdrop-blur-xs z-20 hidden lg:hidden transition-opacity"></div>

        <!-- Collapsible Left HUD Configuration & Knowledge Base Sidebar -->
        <aside id="sidebar" class="fixed lg:static inset-y-0 left-0 z-30 w-80 sm:w-88 bg-white dark:bg-jarvis-panel border-r border-slate-200 dark:border-jarvis-border flex flex-col transform -translate-x-full lg:translate-x-0 transition-transform duration-300 ease-in-out shrink-0">
            
            <div class="p-4 border-b border-slate-200 dark:border-jarvis-border flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <div class="relative w-9 h-9 rounded-full bg-cyan-500/10 border border-cyan-500/40 flex items-center justify-center shadow-md shadow-cyan-500/20">
                        <div class="w-6 h-6 rounded-full border-2 border-dashed border-cyan-400 arc-ring-outer"></div>
                        <div class="w-2.5 h-2.5 rounded-full bg-cyan-400 shadow-[0_0_10px_#22d3ee] absolute"></div>
                    </div>
                    <div>
                        <h1 class="font-hud font-bold text-sm tracking-widest text-slate-900 dark:text-cyan-400">J.A.R.V.I.S.</h1>
                        <p id="device-os-tag" class="text-[10px] font-mono text-slate-500 dark:text-cyan-500/70">DETECTING PLATFORM...</p>
                    </div>
                </div>
                <button onclick="toggleSidebar()" class="lg:hidden p-2 rounded-xl text-slate-500 hover:bg-slate-100 dark:hover:bg-slate-800">
                    <i data-lucide="x" class="w-4 h-4"></i>
                </button>
            </div>

            <div class="flex-1 overflow-y-auto p-4 space-y-5">
                
                <!-- Quick Hardware Action Tray -->
                <div class="space-y-2">
                    <label class="text-[11px] font-hud uppercase tracking-wider text-slate-500 dark:text-cyan-400/80 flex items-center gap-1.5">
                        <i data-lucide="cpu" class="w-3.5 h-3.5 text-cyan-500"></i>
                        Hardware & Device Quick Actions
                    </label>
                    <div class="grid grid-cols-2 gap-2">
                        <button onclick="runDeviceCommand('battery')" class="p-2.5 rounded-xl border border-slate-200 dark:border-jarvis-border hover:border-cyan-500/50 bg-slate-50 dark:bg-jarvis-obsidian text-left transition flex items-center gap-2">
                            <i data-lucide="battery-charging" class="w-4 h-4 text-emerald-400"></i>
                            <span class="text-xs font-medium">Battery Status</span>
                        </button>
                        <button onclick="runDeviceCommand('network')" class="p-2.5 rounded-xl border border-slate-200 dark:border-jarvis-border hover:border-cyan-500/50 bg-slate-50 dark:bg-jarvis-obsidian text-left transition flex items-center gap-2">
                            <i data-lucide="wifi" class="w-4 h-4 text-sky-400"></i>
                            <span class="text-xs font-medium">Network Check</span>
                        </button>
                        <button onclick="runDeviceCommand('camera')" class="p-2.5 rounded-xl border border-slate-200 dark:border-jarvis-border hover:border-cyan-500/50 bg-slate-50 dark:bg-jarvis-obsidian text-left transition flex items-center gap-2">
                            <i data-lucide="camera" class="w-4 h-4 text-purple-400"></i>
                            <span class="text-xs font-medium">Launch Camera</span>
                        </button>
                        <button onclick="runDeviceCommand('clipboard')" class="p-2.5 rounded-xl border border-slate-200 dark:border-jarvis-border hover:border-cyan-500/50 bg-slate-50 dark:bg-jarvis-obsidian text-left transition flex items-center gap-2">
                            <i data-lucide="clipboard" class="w-4 h-4 text-amber-400"></i>
                            <span class="text-xs font-medium">Clipboard Vault</span>
                        </button>
                    </div>
                </div>

                <!-- Operational Mode / Protocol Selector -->
                <div class="space-y-2">
                    <label class="text-[11px] font-hud uppercase tracking-wider text-slate-500 dark:text-cyan-400/80 flex items-center gap-1.5">
                        <i data-lucide="sliders" class="w-3.5 h-3.5 text-cyan-500"></i>
                        Operational Protocol
                    </label>
                    <div class="grid grid-cols-2 gap-2" id="protocol-grid">
                        <!-- Populated via JS -->
                    </div>
                </div>

                <!-- Voice & Audio Controls Card -->
                <div class="p-3 rounded-xl bg-slate-50 dark:bg-jarvis-obsidian border border-slate-200 dark:border-jarvis-border space-y-3">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center gap-2">
                            <i data-lucide="volume-2" class="w-4 h-4 text-cyan-500"></i>
                            <div>
                                <div class="text-xs font-medium">Jarvis Voice Output (TTS)</div>
                                <div class="text-[10px] text-slate-500 dark:text-slate-400">Speak responses aloud</div>
                            </div>
                        </div>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="tts-toggle" onchange="handleTtsToggle()" class="sr-only peer">
                            <div class="w-9 h-5 bg-slate-300 dark:bg-slate-750 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-

