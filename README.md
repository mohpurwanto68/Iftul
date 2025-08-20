<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Budidaya Ikan</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .question-card {
            transition: all 0.3s ease;
        }
        .option-button {
            transition: all 0.2s ease;
        }
        .option-button:hover {
            transform: translateY(-1px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        .correct {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
        }
        .incorrect {
            background: linear-gradient(135deg, #ef4444, #dc2626);
            color: white;
        }
        .progress-bar {
            transition: width 0.5s ease;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-gray-800 mb-2">🐟 Kuis Budidaya Ikan</h1>
            <p class="text-gray-600">Uji pengetahuan Anda tentang budidaya ikan</p>
        </div>



        <!-- Registration Form (Shown initially) -->
        <div id="registration-form" class="bg-white rounded-xl shadow-lg p-8 mb-6">
            <h2 class="text-2xl font-semibold text-gray-800 mb-6 text-center">Informasi Peserta</h2>
            <div class="space-y-6">
                <div>
                    <label for="group-name" class="block text-sm font-medium text-gray-700 mb-2">
                        Nama Kelompok <span class="text-red-500">*</span>
                    </label>
                    <input type="text" id="group-name" 
                           class="w-full px-4 py-3 border-2 border-gray-200 rounded-lg focus:border-blue-500 focus:outline-none transition-colors"
                           placeholder="Masukkan nama kelompok Anda">
                </div>
                <div>
                    <label for="material" class="block text-sm font-medium text-gray-700 mb-2">
                        Materi <span class="text-red-500">*</span>
                    </label>
                    <select id="material" 
                            class="w-full px-4 py-3 border-2 border-gray-200 rounded-lg focus:border-blue-500 focus:outline-none transition-colors">
                        <option value="">Pilih materi yang dikerjakan</option>
                        <option value="Wadah Budidaya Ikan">Wadah Budidaya Ikan</option>
                        <option value="Sistem Budidaya">Sistem Budidaya</option>
                        <option value="Jenis Kolam">Jenis Kolam</option>
                        <option value="Konstruksi Kolam">Konstruksi Kolam</option>
                        <option value="Pengelolaan Air">Pengelolaan Air</option>
                    </select>
                </div>
                <button id="start-quiz-btn" 
                        class="w-full px-6 py-3 bg-blue-600 text-white rounded-lg font-medium hover:bg-blue-700 transition-colors disabled:opacity-50 disabled:cursor-not-allowed"
                        disabled>
                    🚀 Mulai Kuis
                </button>
            </div>
        </div>

        <!-- Progress Bar (Hidden initially) -->
        <div id="progress-section" class="hidden bg-white rounded-lg shadow-md p-4 mb-6">
            <div class="flex justify-between items-center mb-2">
                <span class="text-sm font-medium text-gray-600">Progress</span>
                <span class="text-sm font-medium text-gray-600" id="progress-text">0/5</span>
            </div>
            <div class="w-full bg-gray-200 rounded-full h-3">
                <div class="progress-bar bg-gradient-to-r from-blue-500 to-indigo-600 h-3 rounded-full" style="width: 0%" id="progress-bar"></div>
            </div>
        </div>

        <!-- Participant Info Display (Hidden initially) -->
        <div id="participant-info" class="hidden bg-blue-50 rounded-lg p-4 mb-6">
            <div class="flex justify-between items-center text-sm">
                <div>
                    <span class="font-medium text-blue-800">Kelompok:</span>
                    <span class="text-blue-700" id="display-group-name"></span>
                </div>
                <div>
                    <span class="font-medium text-blue-800">Materi:</span>
                    <span class="text-blue-700" id="display-material"></span>
                </div>
            </div>
        </div>

        <!-- Quiz Container -->
        <div id="quiz-container" class="question-card bg-white rounded-xl shadow-lg p-8 mb-6">
            <!-- Questions will be loaded here -->
        </div>

        <!-- Navigation Buttons -->
        <div class="flex justify-between items-center">
            <button id="prev-btn" class="px-6 py-3 bg-gray-300 text-gray-700 rounded-lg font-medium hover:bg-gray-400 transition-colors disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                ← Sebelumnya
            </button>
            <button id="next-btn" class="px-6 py-3 bg-blue-600 text-white rounded-lg font-medium hover:bg-blue-700 transition-colors disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                Selanjutnya →
            </button>
        </div>

        <!-- Results Section (Hidden initially) -->
        <div id="results" class="hidden bg-white rounded-xl shadow-lg p-8 text-center">
            <div class="mb-6">
                <div class="text-6xl mb-4" id="result-emoji">🎉</div>
                <h2 class="text-3xl font-bold text-gray-800 mb-2">Kuis Selesai!</h2>
                <p class="text-xl text-gray-600" id="final-score">Skor Anda: 0/5</p>
            </div>
            <div class="flex justify-center mb-6">
                <div class="bg-purple-50 p-8 rounded-lg">
                    <div class="text-4xl font-bold text-purple-600" id="total-score">0</div>
                    <div class="text-xl text-purple-700">Total Skor</div>
                </div>
            </div>
            <button id="restart-btn" class="px-8 py-3 bg-blue-600 text-white rounded-lg font-medium hover:bg-blue-700 transition-colors">
                🔄 Mulai Ulang
            </button>
        </div>
    </div>

    <script>
        const originalQuestions = [
            {
                question: "Berapa kedalaman rata-rata kolam pada umumnya?",
                options: ["1,0 M", "1,3 M", "1,5 M", "2,2 M", "2,5 M"],
                correctAnswer: "1,5 M"
            },
            {
                question: "Ada bawah ini yang bukan termasuk wadah budidaya ikan?",
                options: ["Bak", "Akuarium", "KJA", "Kolam", "Kombongan"],
                correctAnswer: "Kombongan"
            },
            {
                question: "\"Dapat dibedakan berdasarkan sistem budidaya yang akan diterapkan dan sumber air yang digunakan\" pernyataan yang benar diatas adalah:",
                options: ["Jenis – jenis bak", "Jenis – jenis kolam", "Jenis – jenis Akuarium", "Jenis – jenis KJA", "Jenis – jenis Tanki"],
                correctAnswer: "Jenis – jenis kolam"
            },
            {
                question: "\"Kolam yang digunakan adalah kolam yang bagian kolamnya (dinding pematang) terbuat dari tembok sedangkan dasar kolamnya terbuat dari tanah\" pernyataan yang benar diatas adalah:",
                options: ["Tradisional/ekstensif", "Intensif", "Semi Intensif", "Super intensif", "Semi ekstensif"],
                correctAnswer: "Semi Intensif"
            },
            {
                question: "Dibawah ini yang bukan jenis kolam untuk budidaya ikan:",
                options: ["Kolam penetasan", "Kolam pemijahan", "Kolam pemberokan", "Kolam pemijahan", "Kolam pembuangan"],
                correctAnswer: "Kolam pembuangan"
            }
        ];

        let questions = [];
        let currentQuestion = 0;
        let userAnswers = [];
        let score = 0;
        let groupName = '';
        let material = '';

        // Function to shuffle array
        function shuffleArray(array) {
            const shuffled = [...array];
            for (let i = shuffled.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
            }
            return shuffled;
        }

        // Function to randomize questions and answers
        function randomizeQuestions() {
            // First shuffle the questions order
            const shuffledQuestions = shuffleArray(originalQuestions);
            
            // Then shuffle the options for each question
            questions = shuffledQuestions.map(q => {
                const shuffledOptions = shuffleArray(q.options);
                const correctIndex = shuffledOptions.indexOf(q.correctAnswer);
                
                return {
                    question: q.question,
                    options: shuffledOptions,
                    correct: correctIndex
                };
            });
        }

        // Function to validate registration form
        function validateForm() {
            const groupNameInput = document.getElementById('group-name');
            const materialSelect = document.getElementById('material');
            const startBtn = document.getElementById('start-quiz-btn');
            
            const isValid = groupNameInput.value.trim() !== '' && materialSelect.value !== '';
            startBtn.disabled = !isValid;
            
            return isValid;
        }

        // Function to start quiz after registration
        function startQuiz() {
            groupName = document.getElementById('group-name').value.trim();
            material = document.getElementById('material').value;
            
            if (!validateForm()) return;
            
            // Hide registration form
            document.getElementById('registration-form').classList.add('hidden');
            
            // Show quiz elements
            document.getElementById('progress-section').classList.remove('hidden');
            document.getElementById('participant-info').classList.remove('hidden');
            document.getElementById('quiz-container').style.display = 'block';
            document.querySelector('.flex.justify-between').style.display = 'flex';
            
            // Display participant info
            document.getElementById('display-group-name').textContent = groupName;
            document.getElementById('display-material').textContent = material;
            
            // Initialize quiz
            randomizeQuestions();
            loadQuestion();
        }

        function loadQuestion() {
            const container = document.getElementById('quiz-container');
            const question = questions[currentQuestion];
            
            container.innerHTML = `
                <div class="mb-6">
                    <div class="flex items-center justify-between mb-4">
                        <span class="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-medium">
                            Soal ${currentQuestion + 1} dari ${questions.length}
                        </span>
                    </div>
                    <h2 class="text-xl font-semibold text-gray-800 mb-6 leading-relaxed">
                        ${question.question}
                    </h2>
                </div>
                <div class="space-y-3">
                    ${question.options.map((option, index) => `
                        <button class="option-button w-full text-left p-4 border-2 border-gray-200 rounded-lg hover:border-blue-300 hover:bg-blue-50 transition-all" 
                                onclick="selectAnswer(${index})" data-index="${index}">
                            <span class="font-medium text-gray-700">${String.fromCharCode(97 + index)}.</span>
                            <span class="ml-2 text-gray-800">${option}</span>
                        </button>
                    `).join('')}
                </div>
            `;

            // Restore previous answer if exists
            if (userAnswers[currentQuestion] !== undefined) {
                const buttons = container.querySelectorAll('.option-button');
                buttons[userAnswers[currentQuestion]].classList.add('border-blue-500', 'bg-blue-50');
                document.getElementById('next-btn').disabled = false;
            }

            updateProgress();
            updateNavigationButtons();
        }

        function selectAnswer(index) {
            const buttons = document.querySelectorAll('.option-button');
            
            // Remove previous selection
            buttons.forEach(btn => {
                btn.classList.remove('border-blue-500', 'bg-blue-50');
            });
            
            // Add selection to clicked button
            buttons[index].classList.add('border-blue-500', 'bg-blue-50');
            
            // Store answer
            userAnswers[currentQuestion] = index;
            
            // Enable next button
            document.getElementById('next-btn').disabled = false;
        }

        function updateProgress() {
            const answered = userAnswers.filter(answer => answer !== undefined).length;
            const percentage = (answered / questions.length) * 100;
            
            document.getElementById('progress-bar').style.width = percentage + '%';
            document.getElementById('progress-text').textContent = `${answered}/${questions.length}`;
        }

        function updateNavigationButtons() {
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            
            prevBtn.disabled = currentQuestion === 0;
            
            if (currentQuestion === questions.length - 1) {
                nextBtn.textContent = 'Selesai 🏁';
                nextBtn.disabled = userAnswers[currentQuestion] === undefined;
            } else {
                nextBtn.textContent = 'Selanjutnya →';
                nextBtn.disabled = userAnswers[currentQuestion] === undefined;
            }
        }

        function nextQuestion() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                loadQuestion();
            } else {
                showResults();
            }
        }

        function prevQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                loadQuestion();
            }
        }

        function showResults() {
            // Calculate score (20 points per correct answer)
            score = 0;
            let correctAnswers = 0;
            for (let i = 0; i < questions.length; i++) {
                if (userAnswers[i] === questions[i].correct) {
                    correctAnswers++;
                    score += 20;
                }
            }

            const percentage = Math.round((correctAnswers / questions.length) * 100);
            const incorrect = questions.length - correctAnswers;

            // Hide quiz container and show results
            document.getElementById('quiz-container').style.display = 'none';
            document.querySelector('.flex.justify-between').style.display = 'none';
            document.getElementById('results').classList.remove('hidden');

            // Update results
            document.getElementById('final-score').textContent = `Skor Anda: ${score}/100`;
            document.getElementById('total-score').textContent = score;

            // Set emoji based on score
            const resultEmoji = document.getElementById('result-emoji');
            if (percentage >= 80) {
                resultEmoji.textContent = '🎉';
            } else if (percentage >= 60) {
                resultEmoji.textContent = '👍';
            } else {
                resultEmoji.textContent = '📚';
            }
        }

        function restartQuiz() {
            currentQuestion = 0;
            userAnswers = [];
            score = 0;
            groupName = '';
            material = '';
            
            // Hide quiz elements and results
            document.getElementById('quiz-container').style.display = 'none';
            document.querySelector('.flex.justify-between').style.display = 'none';
            document.getElementById('results').classList.add('hidden');
            document.getElementById('progress-section').classList.add('hidden');
            document.getElementById('participant-info').classList.add('hidden');
            
            // Show registration form again
            document.getElementById('registration-form').classList.remove('hidden');
            
            // Clear form inputs
            document.getElementById('group-name').value = '';
            document.getElementById('material').value = '';
            document.getElementById('start-quiz-btn').disabled = true;
        }

        // Event listeners
        document.getElementById('next-btn').addEventListener('click', nextQuestion);
        document.getElementById('prev-btn').addEventListener('click', prevQuestion);
        document.getElementById('restart-btn').addEventListener('click', restartQuiz);
        document.getElementById('start-quiz-btn').addEventListener('click', startQuiz);
        
        // Form validation listeners
        document.getElementById('group-name').addEventListener('input', validateForm);
        document.getElementById('material').addEventListener('change', validateForm);



        // Initialize - hide quiz elements initially
        document.getElementById('quiz-container').style.display = 'none';
        document.querySelector('.flex.justify-between').style.display = 'none';
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'971e745bb6f6e9d4',t:'MTc1NTY1Njg3Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
