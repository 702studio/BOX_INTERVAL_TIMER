<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FIGHTING INTERVAL TIMER</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
    <style>
        .progress-circle {
            stroke-linecap: round;
            transition: stroke-dashoffset 0.05s linear;
            stroke-width: 1px;
        }
        body {
            font-family: 'JetBrains Mono', monospace;
            background-color: #1A1A1A;
            color: white;
        }
        .gauge {
            transform: rotate(-90deg);
            origin: center;
        }
        .gauge circle {
            transform-origin: 50% 50%;
        }
        .thinner-border {
            stroke-width: 1px;
        }
        .gauge-black {
            stroke: #121212;
            stroke-width: 1px;
        }
        .slider {
            -webkit-appearance: none;
            width: 100%;
            height: 2px;
            background: #ddd;
            outline: none;
            border-radius: 1px;
        }
        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            background: #ff4444;
            border-radius: 50%;
            cursor: pointer;
            transition: background 0.2s;
        }
        .slider::-webkit-slider-thumb:hover {
            background: #ff3333;
        }
        .snap-point-labels {
            display: flex;
            justify-content: space-between;
            margin-top: 4px;
            color: #666;
            font-size: 0.6rem;
        }
        .bg-custom {
            background-color: #1A1A1A;
        }
        @keyframes glow {
            0% { box-shadow: 0 0 0 0 rgba(255, 68, 68, 0.7); }
            50% { box-shadow: 0 0 0 10px rgba(255, 68, 68, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 68, 68, 0); }
        }
        .glow {
            animation: glow 2s infinite;
        }
        .custom-hover\:border {
            border: none;
        }
        .custom-hover\:border:hover {
            border: 2px solid #ff4444;
        }
        .custom-hover\:stroke {
            transition: all 0.3s ease;
        }
        .custom-hover\:stroke:hover {
            transform: translateY(-1px);
        }
        .hover-stroke {
            position: relative;
            box-sizing: border-box;
        }
        .hover-stroke::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            border: 2px solid transparent;
            transition: all 0.3s ease;
        }
        .hover-stroke:hover::before {
            border: 2px solid #ff4444;
        }
    </style>
</head>
<body class="font-jetbrains bg-custom">
    <div class="container mx-auto px-4 py-8 max-w-2xl flex justify-center">
        <div class="bg-custom rounded-2xl shadow-lg p-8 border border-gray-700 w-full">
            <h1 class="text-4xl font-normal text-white mb-8 text-center uppercase">FIGHTING INTERVAL TIMER</h1>
            
            <div class="relative w-96 h-96 mx-auto mb-8">
                <svg class="w-full h-full" viewBox="0 0 100 100">
                    <circle class="gauge-black" stroke="currentColor" fill="transparent" r="45" cx="50" cy="50"/>
                    <circle id="progress" class="text-red-500 progress-circle thinner-border" stroke="currentColor" fill="transparent" r="45" cx="50" cy="50" transform="rotate(-90 50 50)"/>
                </svg>
                <div class="absolute inset-0 flex flex-col items-center justify-center space-y-2">
                    <span id="timer-status" class="text-2xl font-normal uppercase">READY</span>
                    <span id="timer" class="text-8xl font-bold">03:00</span>
                    <div class="flex flex-col items-center">
                        <span class="text-sm uppercase">ROUND</span>
                        <span id="round-info" class="text-2xl font-normal mt-1">01/03</span>
                    </div>
                </div>
            </div>

            <div class="flex flex-row gap-0">
                <button id="start" class="flex-1 px-4 py-2 bg-[#404040] text-white rounded-none font-normal hover-stroke transition-all uppercase">
                    START
                </button>
                <button id="reset" class="flex-1 px-4 py-2 bg-[#333333] text-white rounded-none font-normal hover-stroke transition-all uppercase">
                    RESET
                </button>
            </div>
            <div class="text-center">
                <button id="settingsBtn" class="w-full px-4 py-2 bg-[#262626] text-white rounded-none font-normal hover-stroke transition-all uppercase">
                    SETTINGS
                </button>
            </div>
        </div>
    </div>

    <audio id="notificationSound" src="https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3"></audio>

    <div id="settingsModal" class="fixed inset-0 bg-black bg-opacity-50 hidden">
        <div class="flex items-center justify-center min-h-screen">
            <div class="bg-custom p-4 rounded-xl w-80 border border-gray-700">
                <h2 class="text-xl font-bold mb-4 uppercase">SETTINGS</h2>
                <div class="space-y-4">
                    <div class="space-y-1">
                        <label class="block text-sm font-medium mb-1 uppercase">ROUND DURATION</label>
                        <div class="flex items-center space-x-2">
                            <input type="range" id="roundDuration" class="slider w-full" min="15" max="300" step="15" value="180">
                            <span id="roundDurationValue" class="w-16 text-right">03:00</span>
                        </div>
                        <div class="snap-point-labels">
                            <span>0.5M</span>
                            <span>1M</span>
                            <span>1.5M</span>
                            <span>2M</span>
                            <span>2.5M</span>
                            <span>3M</span>
                            <span>3.5M</span>
                            <span>4M</span>
                            <span>4.5M</span>
                            <span>5M</span>
                        </div>
                    </div>
                    <div class="space-y-1">
                        <label class="block text-sm font-medium mb-1 uppercase">REST DURATION</label>
                        <div class="flex items-center space-x-2">
                            <input type="range" id="restDuration" class="slider w-full" min="15" max="120" step="15" value="60">
                            <span id="restDurationValue" class="w-16 text-right">01:00</span>
                        </div>
                        <div class="snap-point-labels">
                            <span>0.5M</span>
                            <span>1M</span>
                            <span>2M</span>
                        </div>
                    </div>
                    <div class="space-y-1">
                        <label class="block text-sm font-medium mb-1 uppercase">TOTAL ROUNDS</label>
                        <div class="flex items-center space-x-1">
                            <input type="range" id="totalRounds" class="slider w-full" min="1" max="50" step="1" value="3">
                            <span id="totalRoundsValue" class="w-16 text-right">03</span>
                        </div>
                    </div>
                </div>
                <div class="mt-4 flex justify-between">
                    <button id="cancelSettings" class="flex-1 px-4 py-2 text-gray-400 hover:text-gray-300 transition-all hover-stroke uppercase">
                        CANCEL
                    </button>
                    <button id="saveSettings" class="flex-1 px-4 py-2 bg-[#262626] text-white font-normal hover-stroke transition-all uppercase">
                        SAVE
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        let timer;
        let timeLeft;
        let isRunning = false;
        let currentRound = 1;
        let isWork = true;

        let roundDuration = 180;
        let restDuration = 60;
        let totalRounds = 3;

        timeLeft = roundDuration;

        const timerElement = document.getElementById('timer');
        const statusElement = document.getElementById('timer-status');
        const roundElement = document.getElementById('round-info');
        const progressCircle = document.getElementById('progress');

        function updateDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            timerElement.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            
            roundElement.textContent = ` ${currentRound.toString().padStart(2, '0')} / ${totalRounds.toString().padStart(2, '0')}`;
            
            const radius = progressCircle.r.baseVal.value;
            const circumference = 2 * Math.PI * radius;
            progressCircle.style.strokeDasharray = `${(circumference * (timeLeft / (isWork ? roundDuration : restDuration)))} ${circumference}`;
            progressCircle.style.strokeDashoffset = '0';
        }

        function updateDisplayValue(sliderId, displayId) {
            const slider = document.getElementById(sliderId);
            const display = document.getElementById(displayId);
            
            if (sliderId === 'roundDuration' || sliderId === 'restDuration') {
                const minutes = Math.floor(slider.value / 60);
                const seconds = slider.value % 60;
                display.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            } else {
                display.textContent = slider.value;
            }
        }

        function showSettings() {
            document.getElementById('settingsModal').classList.toggle('hidden');
            updateDisplayValue('roundDuration', 'roundDurationValue');
            updateDisplayValue('restDuration', 'restDurationValue');
            updateDisplayValue('totalRounds', 'totalRoundsValue');
        }

        function toggleTimer() {
            if (!isRunning) {
                startTimer();
            } else {
                stopTimer();
            }
        }

        function startTimer() {
            isRunning = true;
            document.getElementById('start').textContent = 'PAUSE';
            timer = setInterval(countdown, 1000);
        }

        function stopTimer() {
            isRunning = false;
            document.getElementById('start').textContent = 'START';
            clearInterval(timer);
        }

        function resetTimer() {
            stopTimer();
            isRunning = false;
            timeLeft = isWork ? roundDuration : restDuration;
            currentRound = 1;
            isWork = true;
            document.getElementById('start').textContent = 'START';
            updateDisplay();
        }

        function countdown() {
            if (timeLeft <= 0) {
                clearInterval(timer);
                isRunning = false;
                document.getElementById('start').textContent = 'START';
                
                if (isWork) {
                    document.getElementById('notificationSound').play();
                    
                    if (currentRound < totalRounds) {
                        currentRound++;
                        isWork = false;
                        timeLeft = restDuration;
                        updateDisplay();
                        statusElement.textContent = "REST";
                        startTimer();
                    } else {
                        statusElement.textContent = "FINISHED";
                    }
                } else {
                    isWork = true;
                    timeLeft = roundDuration;
                    updateDisplay();
                    statusElement.textContent = "WORK";
                    startTimer();
                }
            } else {
                timeLeft--;
                updateDisplay();
            }
        }

        document.getElementById('start').addEventListener('click', toggleTimer);
        document.getElementById('reset').addEventListener('click', resetTimer);
        document.getElementById('settingsBtn').addEventListener('click', showSettings);
        
        document.getElementById('roundDuration').addEventListener('input', function() {
            updateDisplayValue('roundDuration', 'roundDurationValue');
        });
        document.getElementById('restDuration').addEventListener('input', function() {
            updateDisplayValue('restDuration', 'restDurationValue');
        });
        document.getElementById('totalRounds').addEventListener('input', function() {
            updateDisplayValue('totalRounds', 'totalRoundsValue');
        });

        document.getElementById('saveSettings').addEventListener('click', function() {
            roundDuration = parseInt(document.getElementById('roundDuration').value);
            restDuration = parseInt(document.getElementById('restDuration').value);
            totalRounds = parseInt(document.getElementById('totalRounds').value);
            resetTimer();
            document.getElementById('settingsModal').classList.add('hidden');
        });

        document.getElementById('cancelSettings').addEventListener('click', function() {
            document.getElementById('settingsModal').classList.add('hidden');
        });

        updateDisplay();
    </script>
</body>
</html>