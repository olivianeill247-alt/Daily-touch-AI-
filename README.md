<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Touch AI - Creative Companion</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Plus Jakarta Sans"', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#f0fdfa',
                            100: '#ccfbf1',
                            500: '#14b8a6', // Teal/Cyan friendly mix
                            600: '#0d9488',
                            900: '#134e4a',
                        },
                        accent: {
                            500: '#6366f1', // Indigo
                            600: '#4f46e5',
                        },
                        dark: {
                            800: '#1e1b4b',
                            900: '#0f172a',
                        }
                    },
                    animation: {
                        'pulse-slow': 'pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                        'float': 'float 6s ease-in-out infinite',
                    },
                    keyframes: {
                        float: {
                            '0%, 100%': { transform: 'translateY(0)' },
                            '50%': { transform: 'translateY(-10px)' },
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #0f172a; /* Slate 900 */
            background-image: 
                radial-gradient(at 0% 0%, hsla(253,16%,7%,1) 0, transparent 50%), 
                radial-gradient(at 50% 0%, hsla(225,39%,30%,1) 0, transparent 50%), 
                radial-gradient(at 100% 0%, hsla(339,49%,30%,1) 0, transparent 50%);
            color: #e2e8f0;
            min-height: 100vh;
        }
        
        /* Glassmorphism Card */
        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .gradient-text {
            background: linear-gradient(to right, #2dd4bf, #818cf8);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a; 
        }
        ::-webkit-scrollbar-thumb {
            background: #334155; 
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569; 
        }

        .loader {
            border: 3px solid rgba(255, 255, 255, 0.1);
            border-radius: 50%;
            border-top: 3px solid #2dd4bf;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Image hover effect */
        .image-container:hover .image-overlay {
            opacity: 1;
        }
    </style>
</head>
<body class="antialiased selection:bg-brand-500 selection:text-white">

    <!-- Navbar -->
    <nav class="w-full glass-card sticky top-0 z-50 border-b border-white/10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
                <div class="flex items-center gap-3">
                    <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-brand-500 to-accent-500 flex items-center justify-center text-white font-bold shadow-lg shadow-brand-500/20">
                        DT
                    </div>
                    <span class="text-xl font-bold tracking-tight text-white">Daily Touch <span class="text-brand-500">AI</span></span>
                </div>
                <div class="flex items-center gap-4">
                    <button onclick="clearGallery()" class="text-sm text-slate-400 hover:text-white transition-colors">Clear History</button>
                    <div class="hidden md:flex items-center px-3 py-1 rounded-full bg-white/5 border border-white/10 text-xs text-brand-500">
                        <span class="w-2 h-2 rounded-full bg-green-500 mr-2 animate-pulse"></span>
                        System Online
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            <!-- Left Panel: Controls -->
            <div class="lg:col-span-5 space-y-6">
                
                <!-- Hero Text -->
                <div class="space-y-2 mb-8 animate-float">
                    <h1 class="text-4xl md:text-5xl font-bold text-white leading-tight">
                        Visualize your <br />
                        <span class="gradient-text">Daily Dreams.</span>
                    </h1>
                    <p class="text-slate-400 text-lg">
                        Free, unlimited AI image generation. No API key required. Just type and create.
                    </p>
                </div>

                <!-- Control Card -->
                <div class="glass-card rounded-2xl p-6 shadow-xl shadow-black/20">
                    <div class="space-y-4">
                        
                        <!-- Prompt Input -->
                        <div class="space-y-2">
                            <label class="block text-sm font-medium text-slate-300">Prompt</label>
                            <textarea id="promptInput" rows="4" 
                                class="w-full bg-slate-900/50 border border-slate-700 rounded-xl p-4 text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-brand-500 focus:border-transparent transition-all resize-none"
                                placeholder="A futuristic city with flying cars, neon lights, highly detailed, 8k resolution..."></textarea>
                        </div>

                        <!-- Settings Grid -->
                        <div class="grid grid-cols-2 gap-4">
                            <!-- Style Selector -->
                            <div class="space-y-2">
                                <label class="block text-sm font-medium text-slate-300">Style</label>
                                <div class="relative">
                                    <select id="styleSelect" class="w-full appearance-none bg-slate-900/50 border border-slate-700 rounded-xl px-4 py-3 text-white focus:outline-none focus:ring-2 focus:ring-brand-500 cursor-pointer">
                                        <option value="">No Filter (Default)</option>
                                        <option value="cinematic">Cinematic</option>
                                        <option value="anime">Anime</option>
                                        <option value="photorealistic">Photorealistic</option>
                                        <option value="digital-art">Digital Art</option>
                                        <option value="oil-painting">Oil Painting</option>
                                        <option value="cyberpunk">Cyberpunk</option>
                                        <option value="3d-render">3D Render</option>
                                    </select>
                                    <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-4 text-slate-400">
                                        <i class="fas fa-chevron-down text-xs"></i>
                                    </div>
                                </div>
                            </div>

                            <!-- Aspect Ratio -->
                            <div class="space-y-2">
                                <label class="block text-sm font-medium text-slate-300">Size</label>
                                <div class="relative">
                                    <select id="sizeSelect" class="w-full appearance-none bg-slate-900/50 border border-slate-700 rounded-xl px-4 py-3 text-white focus:outline-none focus:ring-2 focus:ring-brand-500 cursor-pointer">
                                        <option value="1024x1024">Square (1:1)</option>
                                        <option value="1280x720">Landscape (16:9)</option>
                                        <option value="720x1280">Portrait (9:16)</option>
                                    </select>
                                    <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-4 text-slate-400">
                                        <i class="fas fa-chevron-down text-xs"></i>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Generate Button -->
                        <button id="generateBtn" onclick="generateImage()" 
                            class="w-full group relative overflow-hidden bg-gradient-to-r from-brand-600 to-accent-600 hover:from-brand-500 hover:to-accent-500 text-white font-semibold py-4 rounded-xl shadow-lg shadow-brand-500/25 transition-all duration-200 transform hover:scale-[1.02] disabled:opacity-70 disabled:cursor-not-allowed mt-4">
                            <span class="relative z-10 flex items-center justify-center gap-2">
                                <span id="btnText">Generate Masterpiece</span>
                                <i id="btnIcon" class="fas fa-magic"></i>
                                <div id="btnLoader" class="loader hidden"></div>
                            </span>
                            <!-- Shine effect -->
                            <div class="absolute inset-0 h-full w-full scale-0 rounded-xl transition-all duration-300 group-hover:scale-100 group-hover:bg-white/10"></div>
                        </button>
                    </div>
                </div>

                <!-- Tips -->
                <div class="glass-card rounded-xl p-4 flex items-start gap-3">
                    <div class="p-2 bg-brand-500/10 rounded-lg text-brand-500">
                        <i class="fas fa-lightbulb"></i>
                    </div>
                    <div>
                        <h3 class="text-sm font-semibold text-white">Daily Tip</h3>
                        <p class="text-xs text-slate-400 mt-1">Try adding "lighting", "4k", or "highly detailed" to your prompt for better results.</p>
                    </div>
                </div>
            </div>

            <!-- Right Panel: Preview & Gallery -->
            <div class="lg:col-span-7 space-y-6">
                
                <!-- Main Preview Area -->
                <div class="glass-card rounded-2xl p-2 min-h-[400px] flex flex-col items-center justify-center relative overflow-hidden group border-2 border-dashed border-slate-700/50 hover:border-brand-500/50 transition-colors" id="previewContainer">
                    
                    <!-- Placeholder State -->
                    <div id="placeholderState" class="text-center p-8">
                        <div class="w-24 h-24 mx-auto bg-slate-800 rounded-full flex items-center justify-center mb-4 text-slate-600 group-hover:text-brand-500 transition-colors">
                            <i class="fas fa-image text-4xl"></i>
                        </div>
                        <h3 class="text-xl font-semibold text-white mb-2">Ready to Create</h3>
                        <p class="text-slate-400 max-w-sm mx-auto">Your imagination is the limit. Generated images will appear here in high quality.</p>
                    </div>

                    <!-- Loading State -->
                    <div id="loadingState" class="hidden absolute inset-0 bg-slate-900/90 z-20 flex flex-col items-center justify-center">
                        <div class="relative w-24 h-24">
                            <div class="absolute inset-0 rounded-full border-4 border-slate-700"></div>
                            <div class="absolute inset-0 rounded-full border-4 border-t-brand-500 animate-spin"></div>
                        </div>
                        <p id="loadingText" class="mt-6 text-brand-400 font-medium animate-pulse">Initializing AI...</p>
                    </div>

                    <!-- Image Result -->
                    <img id="mainImage" class="hidden w-full h-auto rounded-xl shadow-2xl object-cover max-h-[600px]" alt="Generated Art" />
                    
                    <!-- Action Overlay (Download/Expand) -->
                    <div id="imageActions" class="hidden absolute bottom-6 right-6 flex gap-3 z-10">
                        <button onclick="downloadImage()" class="bg-slate-900/80 hover:bg-brand-600 text-white p-3 rounded-full backdrop-blur-md shadow-lg transition-all transform hover:scale-110 border border-white/10" title="Download High Res">
                            <i class="fas fa-download"></i>
                        </button>
                    </div>
                </div>

                <!-- Recent Gallery -->
                <div>
                    <h3 class="text-lg font-semibold text-white mb-4 flex items-center gap-2">
                        <i class="fas fa-history text-slate-500"></i> Recent Creations
                    </h3>
                    <div id="galleryGrid" class="grid grid-cols-2 md:grid-cols-4 gap-4">
                        <!-- Gallery items will be injected here -->
                    </div>
                </div>
            </div>
        </div>
    </main>

    <!-- Footer -->
    <footer class="mt-12 border-t border-white/5 py-8">
        <div class="max-w-7xl mx-auto px-4 text-center">
            <p class="text-slate-500 text-sm">© 2024 Daily Touch AI. Designed for creativity.</p>
        </div>
    </footer>

    <!-- Notification Toast -->
    <div id="toast" class="fixed bottom-5 left-1/2 transform -translate-x-1/2 bg-slate-800 text-white px-6 py-3 rounded-full shadow-2xl border border-brand-500/30 flex items-center gap-3 transition-all duration-300 translate-y-20 opacity-0 z-50">
        <i class="fas fa-check-circle text-brand-500"></i>
        <span id="toastMsg">Action completed</span>
    </div>

    <script>
        // --- Configuration ---
        const API_BASE = "https://image.pollinations.ai/prompt/";
        const loadingMessages = [
            "Dreaming up your concept...",
            "Mixing colors...",
            "Consulting the digital muse...",
            "Rendering pixels...",
            "Adding finishing touches..."
        ];

        // --- Elements ---
        const promptInput = document.getElementById('promptInput');
        const styleSelect = document.getElementById('styleSelect');
        const sizeSelect = document.getElementById('sizeSelect');
        const generateBtn = document.getElementById('generateBtn');
        const btnText = document.getElementById('btnText');
        const btnIcon = document.getElementById('btnIcon');
        const btnLoader = document.getElementById('btnLoader');
        const mainImage = document.getElementById('mainImage');
        const placeholderState = document.getElementById('placeholderState');
        const loadingState = document.getElementById('loadingState');
        const loadingText = document.getElementById('loadingText');
        const imageActions = document.getElementById('imageActions');
        const galleryGrid = document.getElementById('galleryGrid');

        // --- State ---
        let isGenerating = false;
        let currentImageUrl = "";

        // --- Functions ---

        async function generateImage() {
            const prompt = promptInput.value.trim();
            if (!prompt) {
                showToast("Please describe your image first!");
                promptInput.focus();
                return;
            }

            if (isGenerating) return;

            // UI Update: Start
            isGenerating = true;
            generateBtn.disabled = true;
            btnText.textContent = "Generating...";
            btnIcon.classList.add('hidden');
            btnLoader.classList.remove('hidden');
            
            placeholderState.classList.add('hidden');
            mainImage.classList.add('hidden');
            imageActions.classList.add('hidden');
            loadingState.classList.remove('hidden');

            // Construct Prompt
            const style = styleSelect.value ? ` ${styleSelect.value} style` : "";
            const [width, height] = sizeSelect.value.split('x');
            const enhancedPrompt = encodeURIComponent(`${prompt}${style}, high quality, detailed, 8k`);
            
            // Random Seed to prevent caching
            const seed = Math.floor(Math.random() * 100000);
            
            // Pollinations URL Structure
            const finalUrl = `${API_BASE}${enhancedPrompt}?width=${width}&height=${height}&seed=${seed}&nologo=true`;

            // Loading Text Animation
            let msgIdx = 0;
            const msgInterval = setInterval(() => {
                loadingText.textContent = loadingMessages[msgIdx % loadingMessages.length];
                msgIdx++;
            }, 1500);

            try {
                // Preload image
                const img = new Image();
                img.src = finalUrl;
                
                await new Promise((resolve, reject) => {
                    img.onload = resolve;
                    img.onerror = reject;
                });

                // Success
                currentImageUrl = finalUrl;
                mainImage.src = finalUrl;
                mainImage.classList.remove('hidden');
                imageActions.classList.remove('hidden');
                addToGallery(finalUrl, prompt);
                showToast("Image generated successfully!");

            } catch (error) {
                console.error(error);
                showToast("Failed to generate image. Try again.");
                placeholderState.classList.remove('hidden');
            } finally {
                // UI Update: End
                clearInterval(msgInterval);
                isGenerating = false;
                generateBtn.disabled = false;
                btnText.textContent = "Generate Masterpiece";
                btnIcon.classList.remove('hidden');
                btnLoader.classList.add('hidden');
                loadingState.classList.add('hidden');
            }
        }

        // Add to Gallery History
        function addToGallery(url, prompt) {
            const div = document.createElement('div');
            div.className = 'group relative aspect-square rounded-xl overflow-hidden cursor-pointer border border-white/5';
            div.innerHTML = `
                <img src="${url}" class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110" loading="lazy">
                <div class="absolute inset-0 bg-black/50 opacity-0 group-hover:opacity-100 transition-opacity flex items-center justify-center gap-2">
                    <button onclick="restoreImage('${url}')" class="p-2 bg-white/20 hover:bg-brand-500 rounded-full text-white backdrop-blur-sm transition-colors">
                   # Daily-touch-AI-
Generate any image here
