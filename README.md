# Alarm-clock
<!DOCTYPE html>
<html>
<head>
  <title>Alarm Clock</title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
</head>
<body>
  <div class="container">
    <h1 class="mt-4">Alarm Clock</h1>
    <div class="form-group">
      <label for="alarmTime">Set Alarm Time:</label>
      <input type="time" class="form-control" id="alarmTime">
    </div>
    <button class="btn btn-primary" onclick="setAlarm()">Set Alarm</button>
    <button class="btn btn-danger" onclick="clearAlarm()">Clear Alarm</button>
    <div id="status"></div>
  </div>
  
  <script src="script.js"></script>
</body>
</html>

/* Container Styles */
.container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #333;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  position: relative;
}

/* Heading Styles */
h1 {
  text-align: center;
  color: #fff;
  font-family: 'Oswald', sans-serif;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin: 0;
  font-size: 28px;
}

/* Input Styles */
.input-time {
  width: 100%;
  padding: 10px;
  border: none;
  border-bottom: 2px solid #fff;
  background-color: transparent;
  font-size: 18px;
  color: #fff;
  margin-top: 20px;
  outline: none;
}

.input-time:focus {
  border-color: #f9c147;
}

/* Button Styles */
.btn {
  display: inline-block;
  padding: 10px 20px;
  margin-top: 20px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  text-align: center;
  text-decoration: none;
  background-color: #f9c147;
  color: #333;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #e1ae33;
}

.btn-danger {
  background-color: #e74c3c;
}

/* Status Styles */
.status {
  margin-top: 20px;
  text-align: center;
  font-size: 18px;
  color: #fff;
}

/* Clock Styles */
.clock {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background-color: #333;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  align-items: center;
  justify-content: center;
}

.clock::before {
  content: '';
  position: absolute;
  width: 6px;
  height: 90px;
  background-color: #f9c147;
  border-radius: 4px;
  transform-origin: bottom center;
  top: 10px;
  left: 50%;
  transform: translateX(-50%);
}

.clock::after {
  content: '';
  position: absolute;
  width: 10px;
  height: 10px;
  background-color: #f9c147;
  border-radius: 50%;
}

.hand {
  position: absolute;
  background-color: #f9c147;
  transform-origin: bottom center;
}

.hour-hand {
  width: 6px;
  height: 60px;
  border-radius: 4px;
  top: 40px;
  left: calc(50% - 3px);
}

.minute-hand {
  width: 4px;
  height: 80px;
  border-radius: 4px;
  top: 20px;
  left: calc(50% - 2px);
}

.second-hand {
  width: 2px;
  height: 100px;
  border-radius: 4px;
  top: 0;
  left: calc(50% - 1px);
  background-color: red;
}

// Store the alarm sound
var alarmSound = new Audio('alarm_sound.mp3');

// Function to set the alarm
function setAlarm() {
  var alarmInput = document.getElementById('alarmTime').value;
  var alarmTime = new Date(alarmInput);
  var now = new Date();

  // Check if the entered time is in the past
  if (alarmTime < now) {
    alert("Please select a future time for the alarm.");
    return;
  }

  var timeDifference = alarmTime - now;

  // Set a timeout to trigger the alarm at the specified time
  setTimeout(function() {
    playAlarm();
  }, timeDifference);
  
  document.getElementById('status').innerHTML = 'Alarm set for ' + alarmTime.toLocaleTimeString();
}

// Function to play the alarm sound
function playAlarm() {
  alarmSound.play();
  document.getElementById('status').innerHTML = 'Wake up!';
}

// Function to clear the alarm
function clearAlarm() {
  alarmSound.pause();
  alarmSound.currentTime = 0;
  document.getElementById('status').innerHTML = '';
}
