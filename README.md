<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator & Timer Suite</title>
    <style>
        /* Modern Dark Theme Base Setup */
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #b21919;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            gap: 40px; /* Space between the two apps */
            flex-wrap: wrap; /* Wraps components cleanly on smaller screens */
            padding: 20px;
            box-sizing: border-box;
        }

        .container {
            background-color: #0d0101;
            padding: 25px;
            border-radius: 20px;
            box-shadow: 0 20px 50px rgba(19, 173, 50, 0.5);
            width: 320px;
            height: 480px; /* Kept uniform for symmetry */
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            box-sizing: border-box;
        }

        /* --- Common Screen Styles --- */
        .screen {
            width: 100%;
            height: 65px;
            font-size: 32px;
            text-align: right;
            padding: 10px;
            box-sizing: border-box;
            border: none;
            border-radius: 10px;
            background: #2d2d2d;
            color: #00ff88;
            outline: none;
        }

        /* --- Calculator Specific Styles --- */
        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
        }

        button {
            padding: 18px;
            font-size: 18px;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            background-color: #781010;
            color: white;
            transition: all 0.2s ease;
        }

        button:hover {
            background-color: #444;
            transform: translateY(-2px);
        }

        button:active {
            transform: translateY(0);
        }

        .operator {
            background-color: #3d3d3d;
            color: #00ff88;
            font-weight: bold;
        }

        .clear { background-color: #ff4757; color: white; }
        .delete { background-color: #ffa502; color: white; }
        .equals {
            background-color: #00ff88;
            color: #121212;
            font-weight: bold;
            grid-column: span 2;
        }

        /* --- Timer Specific Styles --- */
        .timer-display {
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: monospace; /* Keeps layout stable as numbers shift */
            font-size: 40px;
        }

        .timer-controls {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .inputs-row {
            display: flex;
            gap: 10px;
            justify-content: space-between;
        }

        .time-input-group {
            display: flex;
            flex-direction: column;
            align-items: center;
            flex: 1;
        }

        .time-input-group label {
            color: #888;
            font-size: 12px;
            margin-bottom: 5px;
            text-transform: uppercase;
        }

        .time-input-group input {
            width: 100%;
            background: #2d2d2d;
            border: 1px solid #444;
            border-radius: 8px;
            padding: 8px;
            color: white;
            text-align: center;
            font-size: 16px;
        }

        .action-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }

        .btn-start { background-color: #00ff88; color: #121212; font-weight: bold;}
        .btn-stop { background-color: #ff4757; color: white; }
        .btn-reset { background-color: #3d3d3d; color: #ffa502; grid-column: span 2;}
    </style>
</head>
<body>

    <!-- CONTAINER 1: THE CALCULATOR -->
    <div class="container">
        <input type="text" id="display" class="screen" readonly placeholder="0">
        <div class="buttons-grid">
            <button onclick="clearDisplay()" class="clear">C</button>
            <button onclick="deleteLast()" class="delete">DEL</button>
            <button onclick="appendToDisplay('/')" class="operator">÷</button>
            <button onclick="appendToDisplay('*')" class="operator">×</button>
            
            <button onclick="appendToDisplay('7')">7</button>
            <button onclick="appendToDisplay('8')">8</button>
            <button onclick="appendToDisplay('9')">9</button>
            <button onclick="appendToDisplay('-')" class="operator">−</button>
            
            <button onclick="appendToDisplay('4')">4</button>
            <button onclick="appendToDisplay('5')">5</button>
            <button onclick="appendToDisplay('6')">6</button>
            <button onclick="appendToDisplay('+')" class="operator">+</button>
            
            <button onclick="appendToDisplay('1')">1</button>
            <button onclick="appendToDisplay('2')">2</button>
            <button onclick="appendToDisplay('3')">3</button>
            <button onclick="appendToDisplay('.')">.</button>
            
            <button onclick="appendToDisplay('0')">0</button>
            <button onclick="calculate()" class="equals">=</button>
        </div>
    </div>

    <!-- CONTAINER 2: THE COUNTDOWN TIMER -->
    <div class="container">
        <div id="timerDisplay" class="screen timer-display">00:00:00</div>
        
        <div class="timer-controls">
            <!-- User Input Configuration -->
            <div class="inputs-row">
                <div class="time-input-group">
                    <label>Hours</label>
                    <input type="number" id="inputHours" min="0" max="23" value="0">
                </div>
                <div class="time-input-group">
                    <label>Mins</label>
                    <input type="number" id="inputMinutes" min="0" max="59" value="0">
                </div>
                <div class="time-input-group">
                    <label>Secs</label>
                    <input type="number" id="inputSeconds" min="0" max="59" value="0">
                </div>
            </div>

            <!-- Execution Buttons -->
            <div class="action-buttons">
                <button onclick="startTimer()" class="btn-start">Start</button>
                <button onclick="stopTimer()" class="btn-stop">Stop</button>
                <button onclick="resetTimer()" class="btn-reset">Reset</button>
            </div>
        </div>
    </div>

    <script>
        /* ==========================================================================
           CALCULATOR LOGIC
           ========================================================================== */
        const calcDisplay = document.getElementById('display');

        function appendToDisplay(input) {
            if (calcDisplay.value === "0" && input !== ".") {
                calcDisplay.value = input;
            } else {
                calcDisplay.value += input;
            }
        }

        function clearDisplay() {
            calcDisplay.value = "";
        }

        function deleteLast() {
            calcDisplay.value = calcDisplay.value.slice(0, -1);
        }

        function calculate() {
            try {
                calcDisplay.value = new Function('return ' + calcDisplay.value)();
            } catch (error) {
                calcDisplay.value = "Error";
                setTimeout(clearDisplay, 1500);
            }
        }

        /* ==========================================================================
           TIMER LOGIC
           ========================================================================== */
        let countdown;
        let totalSecondsRemaining = 0;
        let isRunning = false;

        const timerDisplay = document.getElementById('timerDisplay');
        const inHours = document.getElementById('inputHours');
        const inMinutes = document.getElementById('inputMinutes');
        const inSeconds = document.getElementById('inputSeconds');

        // Converts raw seconds into a structured HH:MM:SS format string
        function formatTime(totalSeconds) {
            const hrs = Math.floor(totalSeconds / 3600);
            const mins = Math.floor((totalSeconds % 3600) / 60);
            const secs = totalSeconds % 60;
            return `${String(hrs).padStart(2, '0')}:${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
        }

        function startTimer() {
            if (isRunning) return; // Prevent spawning duplicate intervals

            // If starting fresh (not unpausing), fetch inputs
            if (totalSecondsRemaining <= 0) {
                const hours = parseInt(inHours.value) || 0;
                const minutes = parseInt(inMinutes.value) || 0;
                const seconds = parseInt(inSeconds.value) || 0;

                totalSecondsRemaining = (hours * 3600) + (minutes * 60) + seconds;
            }

            if (totalSecondsRemaining <= 0) return; // Do nothing if timer is set to 0

            isRunning = true;
            timerDisplay.style.color = "#00ff88"; // Normal color

            countdown = setInterval(() => {
                totalSecondsRemaining--;
                timerDisplay.innerText = formatTime(totalSecondsRemaining);

                if (totalSecondsRemaining <= 0) {
                    clearInterval(countdown);
                    isRunning = false;
                    timerDisplay.innerText = "Time's Up!";
                    timerDisplay.style.color = "#ff4757"; // Flash red on finish
                }
            }, 1000);
        }

        function stopTimer() {
            clearInterval(countdown);
            isRunning = false;
        }

        function resetTimer() {
            clearInterval(countdown);
            isRunning = false;
            totalSecondsRemaining = 0;
            timerDisplay.innerText = "00:00:00";
            timerDisplay.style.color = "#00ff88";
            inHours.value = 0;
            inMinutes.value = 0;
            inSeconds.value = 0;
        }
    </script>
</body>
</html>


# cv
