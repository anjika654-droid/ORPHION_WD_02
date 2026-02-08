# ORPHION_WD_02
Build a stopwatch with Start, Pause, Reset, and "Lap" functionality.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Stopwatch with Lap</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #1e1e2f;
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .stopwatch {
      background: #2b2b3d;
      padding: 25px;
      border-radius: 10px;
      width: 300px;
      text-align: center;
      box-shadow: 0 10px 25px rgba(0,0,0,0.4);
    }

    .time {
      font-size: 2.5em;
      margin-bottom: 20px;
      letter-spacing: 2px;
    }

    .buttons {
      display: flex;
      justify-content: space-between;
      margin-bottom: 15px;
    }

    button {
      flex: 1;
      margin: 0 5px;
      padding: 10px;
      font-size: 0.9em;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    .start { background: #4caf50; color: #fff; }
    .pause { background: #ff9800; color: #fff; }
    .reset { background: #f44336; color: #fff; }
    .lap   { background: #2196f3; color: #fff; }

    ul {
      list-style: none;
      padding: 0;
      max-height: 150px;
      overflow-y: auto;
      text-align: left;
    }

    li {
      padding: 5px;
      border-bottom: 1px solid #444;
      font-size: 0.9em;
    }
  </style>
</head>
<body>

  <div class="stopwatch">
    <div class="time" id="display">00:00:00.000</div>

    <div class="buttons">
      <button class="start" id="startBtn">Start</button>
      <button class="pause" id="pauseBtn" disabled>Pause</button>
    </div>

    <div class="buttons">
      <button class="lap" id="lapBtn" disabled>Lap</button>
      <button class="reset" id="resetBtn" disabled>Reset</button>
    </div>

    <ul id="laps"></ul>
  </div>

  <script>
    let startTime = 0;
    let elapsedTime = 0;
    let timerInterval;
    let running = false;
    let lapCount = 1;

    const display = document.getElementById("display");
    const startBtn = document.getElementById("startBtn");
    const pauseBtn = document.getElementById("pauseBtn");
    const resetBtn = document.getElementById("resetBtn");
    const lapBtn = document.getElementById("lapBtn");
    const laps = document.getElementById("laps");

    function formatTime(time) {
      const ms = time % 1000;
      const totalSeconds = Math.floor(time / 1000);
      const seconds = totalSeconds % 60;
      const minutes = Math.floor(totalSeconds / 60) % 60;
      const hours = Math.floor(totalSeconds / 3600);

      return (
        String(hours).padStart(2, "0") + ":" +
        String(minutes).padStart(2, "0") + ":" +
        String(seconds).padStart(2, "0") + "." +
        String(ms).padStart(3, "0")
      );
    }

    function updateDisplay() {
      const currentTime = Date.now() - startTime + elapsedTime;
      display.textContent = formatTime(currentTime);
    }

    startBtn.addEventListener("click", () => {
      if (!running) {
        startTime = Date.now();
        timerInterval = setInterval(updateDisplay, 10);
        running = true;

        startBtn.disabled = true;
        pauseBtn.disabled = false;
        resetBtn.disabled = false;
        lapBtn.disabled = false;
      }
    });

    pauseBtn.addEventListener("click", () => {
      if (running) {
        clearInterval(timerInterval);
        elapsedTime += Date.now() - startTime;
        running = false;

        startBtn.disabled = false;
        pauseBtn.disabled = true;
      }
    });

    resetBtn.addEventListener("click", () => {
      clearInterval(timerInterval);
      startTime = 0;
      elapsedTime = 0;
      running = false;
      lapCount = 1;

      display.textContent = "00:00:00.000";
      laps.innerHTML = "";

      startBtn.disabled = false;
      pauseBtn.disabled = true;
      resetBtn.disabled = true;
      lapBtn.disabled = true;
    });

    lapBtn.addEventListener("click", () => {
      if (running) {
        const lapTime = Date.now() - startTime + elapsedTime;
        const li = document.createElement("li");
        li.textContent = `Lap ${lapCount++}: ${formatTime(lapTime)}`;
        laps.prepend(li);
      }
    });
  </script>

</body>
</html>
