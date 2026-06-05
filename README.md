# Grow-and-Learn
A website that helps you grow and learn!
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grow and Learn</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #f8fafc;
        }
        .active-tab {
            background-color: #1e3a8a;
            color: white;
        }
        .flashcard {
            perspective: 1000px;
        }
        .flashcard-inner {
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }
        .flashcard.flipped .flashcard-inner {
            transform: rotateY(180deg);
        }
        .flashcard-front, .flashcard-back {
            backface-visibility: hidden;
        }
        .flashcard-back {
            transform: rotateY(180deg);
        }
    </style>
</head>
<body class="text-slate-800 flex flex-col min-h-screen">

    <header class="bg-emerald-800 text-white shadow-md p-4 sticky top-0 z-50">
        <div class="max-w-4xl mx-auto flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <i data-lucide="sprout" class="w-8 h-8 text-emerald-300"></i>
                <h1 class="text-2xl font-bold tracking-tight">Grow and Learn</h1>
            </div>
            <div class="bg-emerald-700 px-3 py-1 rounded-full text-sm font-medium flex items-center space-x-1">
                <i data-lucide="flame" class="w-4 h-4 text-orange-400 fill-orange-400"></i>
                <span id="header-streak">5 Day Streak</span>
            </div>
        </div>
    </header>

    <main class="flex-grow max-w-4xl w-full mx-auto p-4 pb-24">
        
        <div id="tab-home" class="tab-content space-y-6">
            <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
                <h2 class="text-lg font-semibold text-slate-900 mb-4 flex items-center gap-2">
                    <i data-lucide="sparkles" class="text-emerald-600"></i> Start Your Learning Journey
                </h2>
                
                <div class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-slate-600 mb-1">Choose Language</label>
                        <select id="study-lang" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            <option value="English">English</option>
                            <option value="Hindi">Hindi</option>
                            <option value="Nepali">Nepali</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-sm font-medium text-slate-600 mb-1">Choose Subject</label>
                        <select id="study-subject" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-emerald-500">
                            <option value="English grammar">English Grammar</option>
                            <option value="English literature">English Literature</option>
                            <option value="Hindi grammar">Hindi Grammar</option>
                            <option value="Hindi literature">Hindi Literature</option>
                            <option value="Nepali grammar">Nepali Grammar</option>
                            <option value="Nepali Literature">Nepali Literature</option>
                            <option value="Science">Science</option>
                            <option value="Social science">Social Science</option>
                            <option value="Maths">Maths</option>
                            <option value="IT">IT</option>
                            <option value="AI">AI</option>
                            <option value="Psychology">Psychology</option>
                            <option value="Current affairs">Current Affairs</option>
                            <option value="Cultural studies">Cultural Studies</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-sm font-medium text-slate-600 mb-1">Topic or Chapter Name</label>
                        <input type="text" id="study-topic" placeholder="e.g., Photosynthesis, Algebraic Expressions, Tenses..." class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-emerald-500">
                    </div>

                    <div class="grid grid-cols-2 gap-3 pt-2">
                        <button onclick="generateStudyMaterials()" class="bg-emerald-600 hover:bg-emerald-700 text-white font-medium py-3 px-4 rounded-xl shadow-sm transition flex items-center justify-center gap-2 cursor-pointer">
                            <i data-lucide="book-open" class="w-5 h-5"></i> Generate Notes
                        </button>
                        <button onclick="triggerScan()" class="bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium py-3 px-4 rounded-xl transition flex items-center justify-center gap-2 cursor-pointer">
                            <i data-lucide="scan-text" class="w-5 h-5"></i> Scan Chapters
                        </button>
                    </div>
                    <input type="file" id="scan-file" class="hidden" accept="image/*" onchange="simulateScan(event)">
                </div>
            </div>

            <div id="loading-spinner" class="hidden text-center py-12">
                <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-emerald-600 mx-auto mb-3"></div>
                <p class="text-slate-500 font-medium">AI is crafting your custom learning bundle...</p>
            </div>

            <div id="output-workspace" class="hidden space-y-6">
                <div class="border-b border-slate-200 flex space-x-4">
                    <button onclick="switchOutputTab('notes')" id="btn-out-notes" class="border-b-2 border-emerald-600 px-4 py-2 font-medium text-emerald-600">Notes</button>
                    <button onclick="switchOutputTab('podcast')" id="btn-out-podcast" class="border-b-2 border-transparent px-4 py-2 font-medium text-slate-500 hover:text-slate-800">Podcast</button>
                    <button onclick="switchOutputTab('cards')" id="btn-out-cards" class="border-b-2 border-transparent px-4 py-2 font-medium text-slate-500 hover:text-slate-800">Flashcards</button>
                    <button onclick="switchOutputTab('quiz')" id="btn-out-quiz" class="border-b-2 border-transparent px-4 py-2 font-medium text-slate-500 hover:text-slate-800">Quiz</button>
                </div>

                <div id="out-notes" class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
                    <h3 class="text-xl font-bold text-slate-900 mb-4" id="render-title">Notes</h3>
                    <div class="prose text-slate-700 space-y-4 leading-relaxed" id="render-notes"></div>
                </div>

                <div id="out-podcast" class="hidden bg-white rounded-2xl p-6 shadow-sm border border-slate-100 text-center space-y-4">
                    <div class="w-16 h-16 bg-emerald-100 text-emerald-700 rounded-full flex items-center justify-center mx-auto">
                        <i data-lucide="headphones" class="w-8 h-8"></i>
                    </div>
                    <h3 class="text-lg font-bold text-slate-900">AI Generated Audio Summary</h3>
                    <p class="text-sm text-slate-500 max-w-sm mx-auto">Listen to a conversation explaining your topic like an engaging radio show episode.</p>
                    <div class="pt-2">
                        <audio id="podcast-audio" controls class="mx-auto w-full max-w-md"></audio>
                    </div>
                </div>

                <div id="out-cards" class="hidden space-y-4">
                    <p class="text-sm text-slate-500 text-center">Tap the card to flip and check the answer!</p>
                    <div class="flashcard w-full max-w-md mx-auto h-48 cursor-pointer" onclick="this.classList.toggle('flipped')">
                        <div class="flashcard-inner w-full h-full relative shadow-sm rounded-2xl border border-slate-100">
                            <div class="flashcard-front absolute inset-0 bg-white p-6 flex flex-col justify-center items-center text-center rounded-2xl">
                                <span class="text-xs font-bold uppercase tracking-wider text-emerald-600 mb-2">Question</span>
                                <p class="text-lg font-medium text-slate-800" id="render-card-q">What is this topic?</p>
                            </div>
                            <div class="flashcard-back absolute inset-0 bg-emerald-50 p-6 flex flex-col justify-center items-center text-center rounded-2xl border border-emerald-200">
                                <span class="text-xs font-bold uppercase tracking-wider text-emerald-700 mb-2">Answer</span>
                                <p class="text-base font-medium text-emerald-900" id="render-card-a">The complete detailed answer details.</p>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="out-quiz" class="hidden bg-white rounded-2xl p-6 shadow-sm border border-slate-100 space-y-4">
                    <h3 class="text-lg font-bold text-slate-900" id="render-quiz-q">Question</h3>
                    <div class="space-y-2" id="render-quiz-options"></div>
                    <button onclick="checkQuizAnswer()" class="w-full mt-4 bg-slate-900 text-white font-medium py-3 rounded-xl hover:bg-slate-800 transition">Submit Answer</button>
                    <p id="quiz-feedback" class="hidden font-semibold text-center p-2 rounded-xl"></p>
                </div>
            </div>
        </div>

        <div id="tab-storage" class="tab-content hidden space-y-4">
            <h2 class="text-xl font-bold text-slate-900 flex items-center gap-2">
                <i data-lucide="archive" class="text-indigo-600"></i> Vaulted History
            </h2>
            <p class="text-sm text-slate-500">Jump right back into any study materials you generated earlier.</p>
            <div id="storage-list" class="space-y-3 pt-2">
                </div>
        </div>

        <div id="tab-zen" class="tab-content hidden space-y-6">
            <div class="bg-gradient-to-tr from-sky-50 to-indigo-50 border border-sky-100 rounded-2xl p-6 text-center space-y-4">
                <div class="w-16 h-16 bg-sky-500 text-white rounded-full flex items-center justify-center mx-auto shadow-sm">
                    <i data-lucide="cloud-sun" class="w-8 h-8"></i>
                </div>
                <h2 class="text-xl font-bold text-slate-900">The Sanctuary</h2>
                <p class="text-sm text-slate-600 max-w-md mx-auto">Feeling exam stress, confusion, or overwhelming pressure? Write down what is on your mind. Let this space comfort you.</p>
                
                <textarea id="zen-input" rows="4" placeholder="Type whatever is causing you stress right now..." class="w-full p-4 bg-white border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-sky-500 text-slate-700"></textarea>
                
                <button onclick="getComfort()" class="w-full bg-sky-600 hover:bg-sky-700 text-white font-medium py-3 rounded-xl transition flex items-center justify-center gap-2 cursor-pointer shadow-sm">
                    <i data-lucide="heart" class="w-5 h-5"></i> Receive Comfort
                </button>
            </div>

            <div id="zen-output-box" class="hidden bg-white border border-emerald-100 rounded-2xl p-6 shadow-sm space-y-3 animate-fade-in">
                <div class="flex justify-between items-center">
                    <span class="text-xs font-bold text-emerald-700 bg-emerald-50 px-2 py-1 rounded-md uppercase tracking-wider">A Gentle Reminder</span>
                    <button onclick="speakComfort()" class="text-sky-600 hover:text-sky-800 p-1 flex items-center gap-1 text-sm font-medium">
                        <i data-lucide="volume-2" class="w-4 h-4"></i> Listen
                    </button>
                </div>
                <p id="zen-comfort-text" class="text-slate-700 leading-relaxed italic text-lg text-center font-medium"></p>
            </div>
        </div>

        <div id="tab-you" class="tab-content hidden space-y-6">
            <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100 flex items-center space-x-4">
                <div class="w-16 h-16 bg-emerald-800 text-white text-2xl font-bold rounded-full flex items-center justify-center">
                    G
                </div>
                <div>
                    <h2 class="text-xl font-bold text-slate-900">Super Learner</h2>
                    <p class="text-sm text-slate-500 font-medium">8th Grade Academic Workspace</p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <div class="bg-white p-4 rounded-xl border border-slate-100 shadow-sm text-center">
                    <span class="block text-2xl font-bold text-emerald-700" id="stat-count">3</span>
                    <span class="text-xs text-slate-500 font-medium uppercase tracking-wider">Topics Covered</span>
                </div>
                <div class="bg-white p-4 rounded-xl border border-slate-100 shadow-sm text-center">
                    <span class="block text-2xl font-bold text-orange-600" id="stat-streak">5 Days</span>
                    <span class="text-xs text-slate-500 font-medium uppercase tracking-wider">Continuous Streak</span>
                </div>
            </div>

            <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100 space-y-3">
                <h3 class="text-sm font-bold uppercase tracking-wider text-slate-400">Your Attendance Map</h3>
                <div class="grid grid-cols-7 gap-2 text-center text-xs font-semibold text-slate-400 mb-1">
                    <div>Mon</div><div>Tue</div><div>Wed</div><div>Thu</div><div>Fri</div><div>Sat</div><div>Sun</div>
                </div>
                <div class="grid grid-cols-7 gap-2" id="calendar-grid">
                    </div>
            </div>
        </div>

    </main>

    <nav class="bg-white border-t border-slate-200 fixed bottom-0 left-0 right-0 z-50 shadow-lg">
        <div class="max-w-4xl mx-auto flex justify-around items-center h-16">
            <button onclick="switchTab('home')" id="nav-home" class="flex flex-col items-center justify-center w-full h-full text-emerald-700 font-semibold text-xs gap-1">
                <i data-lucide="home" class="w-5 h-5"></i> Home
            </button>
            <button onclick="switchTab('storage')" id="nav-storage" class="flex flex-col items-center justify-center w-full h-full text-slate-400 font-medium text-xs gap-1">
                <i data-lucide="archive" class="w-5 h-5"></i> Storage
            </button>
            <button onclick="switchTab('zen')" id="nav-zen" class="flex flex-col items-center justify-center w-full h-full text-slate-400 font-medium text-xs gap-1">
                <i data-lucide="cloud-sun" class="w-5 h-5"></i> Zen
            </button>
            <button onclick="switchTab('you')" id="nav-you" class="flex flex-col items-center justify-center w-full h-full text-slate-400 font-medium text-xs gap-1">
                <i data-lucide="user" class="w-5 h-5"></i> You
            </button>
        </div>
    </nav>

    <script>
        // Local state/database simulation
        let studyHistory = [
            { id: 1, topic: 'Tenses', subject: 'English grammar', lang: 'English', date: '2 days ago' },
            { id: 2, topic: 'Kriya (Verbs)', subject: 'Hindi grammar', lang: 'Hindi', date: '3 days ago' },
            { id: 3, topic: 'Triangle Congruence', subject: 'Maths', lang: 'English', date: '5 days ago' }
        ];

        let selectedQuizAnswer = null;
        let correctQuizAnswer = "A option";

        // Tab Switching Engine
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            
            // Adjust Nav styling
            const navIds = ['home', 'storage', 'zen', 'you'];
            navIds.forEach(id => {
                const btn = document.getElementById(`nav-${id}`);
                if(id === tabId) {
                    btn.classList.add('text-emerald-700', 'font-semibold');
                    btn.classList.remove('text-slate-400', 'font-medium');
                } else {
                    btn.classList.remove('text-emerald-700', 'font-semibold');
                    btn.classList.add('text-slate-400', 'font-medium');
                }
            });
            
            if(tabId === 'storage') renderStorage();
            if(tabId === 'you') renderYouTab();
        }

        // Sub-tabs for output results
        function switchOutputTab(section) {
            ['notes', 'podcast', 'cards', 'quiz'].forEach(s => {
                document.getElementById(`out-${s}`).classList.add('hidden');
                document.getElementById(`btn-out-${s}`).className = "border-b-2 border-transparent px-4 py-2 font-medium text-slate-500 hover:text-slate-800";
            });
            document.getElementById(`out-${section}`).classList.remove('hidden');
            document.getElementById(`btn-out-${section}`).className = "border-b-2 border-emerald-600 px-4 py-2 font-medium text-emerald-600";
        }

        // Simulate Scanning Feature
        function triggerScan() {
            document.getElementById('scan-file').click();
        }

        function simulateScan(event) {
            if(event.target.files && event.target.files[0]) {
                document.getElementById('study-topic').value = "Scanned Chapter Page (" + event.target.files[0].name + ")";
                generateStudyMaterials();
            }
        }

        // AI Core Material Generator Mock
        function generateStudyMaterials() {
            const topic = document.getElementById('study-topic').value.trim();
            const subject = document.getElementById('study-subject').value;
            const lang = document.getElementById('study-lang').value;

            if(!topic) {
                alert("Please write a topic or scan a page first!");
                return;
            }

            document.getElementById('loading-spinner').classList.remove('hidden');
            document.getElementById('output-workspace').classList.add('hidden');

            setTimeout(() => {
                document.getElementById('loading-spinner').classList.add('hidden');
                document.getElementById('output-workspace').classList.remove('hidden');
                switchOutputTab('notes');

                // Build custom synthetic text response
                document.getElementById('render-title').innerText = `${subject}: ${topic} (${lang})`;
                
                let notesHTML = `
                    <p class="font-bold text-emerald-800">📌 Overview:</p>
                    <p>This study bundle has been carefully adapted for you regarding <strong>${topic}</strong> inside your <strong>${subject}</strong> course curriculum written fluently in <strong>${lang}</strong>.</p>
                    <p class="font-bold text-emerald-800 pt-2">🔑 Key Concepts:</p>
                    <ul c
/settings/pages
