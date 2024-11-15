<script>
    let remaining = 1500;
    let interval;
    let pomodoro = 1; 
    let currentMode = "Pomodoro";
    let visible = false;
    let buttonText = "START"; 

    // after a break, increase pomodoro by 1
    // long mode once pomodoro 4 finishes. 

    function formatTime(seconds) {
        const mins = Math.floor((seconds % 3600) / 60);
        const secs = seconds % 60;
        return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
    }

    function pomodoroTime() { // set it to specific mode
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

    function updateTimer() {
        if(buttonText === "START") {
            buttonText = "STOP";
            visible = true;
            interval = setInterval(() => {
                if (remaining > 0) {
                    remaining -= 1;
                } else {
                    clearInterval(interval); 
                    alert("Time's up!"); 
                }
            }, 1000);
        } else {
            buttonText = "START";
            visible = false;
            clearInterval(interval);  // Stop the interval
        }
    }

    

    function nextMode() {
        buttonText = "START";
        visible = false;
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

<div class="timer">
    <div class="header-buttons">
        <button on:click={pomodoroTime}>Pomodoro</button>
        <button on:click={shortBreak}>Short Break</button>
        <button on:click={longBreak}>Long Break</button>
    </div>

    <div class="time-remaining">
        {formatTime(remaining)}
    </div>

    <button class="start-button" on:click={updateTimer}>{buttonText}</button>
    {#if visible} 
        <button on:click={nextMode}>Next Mode</button>
    {/if}
</div>

<p>#{pomodoro}</p>
<p>Time to Focus!</p>

<link href='https://fonts.googleapis.com/css?family=Varela Round' rel='stylesheet'>

<style>
    .time-remaining {
        font-size: 120px;
        font-weight: bold;
        margin-top: 20px;
        font-family: 'Varela Round';
    }

    .timer {
        background-color: rgb(193,92,92);
    }

    div {
      text-align: center;
      margin: 25px;
      border-radius: 10px;
    }


    button {
        font-family: 'Arial Rounded MT Regular';
        margin: 10px;
        padding: 5px 20px;
        font-size: 16px;
        color: white;
        cursor: pointer;
        background-color: rgb(164,78,78);
        border-radius: 10px;
    }

    .start-button {
        background-color: white;
        color: rgb(164,78,78);
    }
</style>
