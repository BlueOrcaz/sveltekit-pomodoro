<script>
    let remaining = 1500;
    let interval;
    let pomodoro = 1; 
    let currentMode = "Pomodoro";
    let visible = false;

    // after a break, increase pomodoro by 1
    // long mode once pomodoro 4 finishes. 

    function formatTime(seconds) {
        const mins = Math.floor((seconds % 3600) / 60);
        const secs = seconds % 60;
        return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
    }

    function pomodoroTime() {
        clearInterval(interval);
        remaining = 1500; 
        currentMode = "Pomodoro"; 
    }

    function shortBreak() {
        clearInterval(interval);
        remaining = 300;   
        currentMode = "Short"; 
    }

    function longBreak() {
        clearInterval(interval);
        remaining = 900;  
        currentMode = "Long"; 
    }

    function startTimer() {
        visible = true;
        interval = setInterval(() => {
            if (remaining > 0) {
                remaining -= 1;
            } else {
                clearInterval(interval); 
                alert("Time's up!"); 
            }
        }, 1000);
    }

    function stopTimer() {
        visible = false;
        clearInterval(interval);  // Stop the interval
    }
    

    function nextMode() {
        clearInterval(interval); 
        // If we're in a break mode, switch to Pomodoro
        if (currentMode === "Short" || currentMode === "Long") {
            pomodoroTime();
            pomodoro += 1;  // Increment Pomodoro count
        } 
        // If we're in Pomodoro, check if it's the 4th Pomodoro
        else if (currentMode === "Pomodoro") {
            if (pomodoro % 4 === 0) {
                longBreak();
            } else {
                shortBreak();
            }
        }
    }

    
</script>

<div>
    <button on:click={pomodoroTime}>Pomodoro</button>
    <button on:click={shortBreak}>Short Break</button>
    <button on:click={longBreak}>Long Break</button>

    <div>{formatTime(remaining)}</div>
    <button on:click={startTimer}>Start</button>
    <button on:click={stopTimer}>Stop</button>
    {#if visible} 
        <button on:click={nextMode}>Next Mode</button>
    {/if}
    

    <p>Pomodoro: {pomodoro}</p>
</div>

<style>
    div {
      text-align: center;
      margin: 20px;
    }

    button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
</style>
