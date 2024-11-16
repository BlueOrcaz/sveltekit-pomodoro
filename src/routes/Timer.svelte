<script>
    let remaining = 1500;
    let interval;
    let pomodoro = 1; 
    let currentMode = "Pomodoro";
    let visible = false;
    let buttonText = "START"; 

    let backgroundColour;
    let timerColour;
    let buttonColour; 


    // after a break, increase pomodoro by 1
    // long mode once pomodoro 4 finishes. 

    function formatTime(seconds) {
        const mins = Math.floor((seconds % 3600) / 60);
        const secs = seconds % 60;
        return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
    }

    function pomodoroTime() { // set it to specific mode
        buttonText = "START";
        visible = false;
        clearInterval(interval);
        remaining = 1500; 
        currentMode = "Pomodoro"; 
        backgroundColour = "rgb(186, 73, 73)";
        timerColour = "rgb(193,92,92)";
        buttonColour = "rgb(164,78,78)";
        updateUI();
    }

    function shortBreak() {
        buttonText = "START";
        visible = false;
        clearInterval(interval);
        remaining = 300;   
        currentMode = "Short"; 
        backgroundColour = "rgb(56,133,138)";
        timerColour = "rgb(76,145,150)";
        buttonColour = "rgb(65,123,128)";
        updateUI();
    }

    function longBreak() {
        buttonText = "START";
        visible = false;
        clearInterval(interval);
        remaining = 900;  
        currentMode = "Long"; 
        backgroundColour = "rgb(57,112,151)";
        timerColour = "rgb(77,127,162)";
        buttonColour = "rgb(66,108,138)";
        updateUI();
    }

    function updateUI() {
        document.body.style.backgroundColor = backgroundColour;
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

<div class="timer" style="background-color: {timerColour}">
    <div class="header-buttons">
        <button on:click={pomodoroTime} style="background-color: {buttonColour}">Pomodoro</button>
        <button on:click={shortBreak} style="background-color: {buttonColour}">Short Break</button>
        <button on:click={longBreak} style="background-color: {buttonColour}">Long Break</button>
    </div>

    <div class="time-remaining">
        {formatTime(remaining)}
    </div>

    <button class="start-button" on:click={updateTimer} style="color: {buttonColour}">{buttonText}</button>
    {#if visible} 
        <button on:click={nextMode} style="background-color: {backgroundColour}">Next Mode</button>
    {/if}
</div>

<p class="font-varela">#{pomodoro}</p>
<p class="font-varela">Time to Focus!</p>
<br/>
<p class="font-varela">Simple Pomodoro Timer by BlueOrcaz. Inspired by Pomofocus.io</p>
<br/>
<a href="https://github.com/BlueOrcaz" target="_blank">GitHub</a>

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
        transition: background-color 0.3s ease; 
    }

    div {
      text-align: center;
      margin: 45px;
      border-radius: 10px;
    }


    button {
        font-family: 'Varela Round';
        margin: 10px;
        padding: 5px 25px;
        font-size: 18px;
        color: white;
        cursor: pointer;
        background-color: rgb(164,78,78);
        border-radius: 10px;
        transition: background-color 0.3s ease; 
    }

    .start-button {
        background-color: white;
        color: rgb(164,78,78);
    }
</style>
