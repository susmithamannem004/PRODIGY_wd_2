<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="stopwatch-container">
        <h1>Stopwatch</h1>
        <div id="time-display">00:00:00.00</div>
        <div class="buttons">
            <button id="start-pause-btn">Start</button>
            <button id="reset-btn">Reset</button>
            <button id="lap-btn">Lap</button>
        </div>
        <div id="laps-container">
            <h2>Laps</h2>
            <ul id="laps"></ul>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>


body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f5f5f5;
    font-family: Arial, sans-serif;
    margin: 0;
}

.stopwatch-container {
    text-align: center;
    background: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

#time-display {
    font-size: 3em;
    margin: 20px 0;
    color: #333;
}

.buttons button {
    font-size: 1em;
    padding: 10px 20px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

#start-pause-btn {
    background-color: #4caf50;
    color: white;
}

#reset-btn {
    background-color: #f44336;
    color: white;
}

#lap-btn {
    background-color: #2196f3;
    color: white;
}

#laps-container {
    margin-top: 20px;
}

#laps {
    list-style-type: none;
    padding: 0;
}

#laps li {
    background: #eee;
    margin: 5px 0;
    padding: 10px;
    border-radius: 5px;
}


let startTime, updatedTime, difference, tInterval;
let running = false;
let lapCounter = 0;

const timeDisplay = document.getElementById('time-display');
const startPauseBtn = document.getElementById('start-pause-btn');
const resetBtn = document.getElementById('reset-btn');
const lapBtn = document.getElementById('lap-btn');
const laps = document.getElementById('laps');

startPauseBtn.addEventListener('click', startPause);
resetBtn.addEventListener('click', reset);
lapBtn.addEventListener('click', recordLap);

function startPause() {
    if (!running) {
        startTime = new Date().getTime() - difference;
        tInterval = setInterval(updateTime, 10);
        startPauseBtn.textContent = 'Pause';
        running = true;
    } else {
        clearInterval(tInterval);
        difference = new Date().getTime() - startTime;
        startPauseBtn.textContent = 'Start';
        running = false;
    }
}

function reset() {
    clearInterval(tInterval);
    difference = 0;
    running = false;
    startPauseBtn.textContent = 'Start';
    timeDisplay.textContent = '00:00:00.00';
    lapCounter = 0;
    laps.innerHTML = '';
}

function updateTime() {
    updatedTime = new Date().getTime() - startTime;
    timeDisplay.textContent = formatTime(updatedTime);
}

function formatTime(time) {
    let diffInHrs = time / 3600000;
    let hh = Math.floor(diffInHrs);

    let diffInMin = (diffInHrs - hh) * 60;
    let mm = Math.floor(diffInMin);

    let diffInSec = (diffInMin - mm) * 60;
    let ss = Math.floor(diffInSec);

    let diffInMs = (diffInSec - ss) * 100;
    let ms = Math.floor(diffInMs);

    let formattedHH = hh.toString().padStart(2, '0');
    let formattedMM = mm.toString().padStart(2, '0');
    let formattedSS = ss.toString().padStart(2, '0');
    let formattedMS = ms.toString().padStart(2, '0');

    return `${formattedHH}:${formattedMM}:${formattedSS}.${formattedMS}`;
}

function recordLap() {
    if (running) {
        lapCounter++;
        const lapTime = formatTime(updatedTime);
        const lapElement = document.createElement('li');
        lapElement.textContent = `Lap ${lapCounter}: ${lapTime}`;
        laps.appendChild(lapElement);
    }
}
