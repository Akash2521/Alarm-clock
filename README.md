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
    <div class="clock">
      <div class="hour-hand hand"></div>
      <div class="minute-hand hand"></div>
      <div class="second-hand hand"></div>
    </div>
    <div class="form-group">
      <label for="alarmHour">Set Alarm Time:</label>
      <input type="number" class="form-control" id="alarmHour" placeholder="Hour" min="1" max="12">
      <input type="number" class="form-control" id="alarmMinute" placeholder="Minute" min="0" max="59">
      <input type="number" class="form-control" id="alarmSecond" placeholder="Second" min="0" max="59">
      <select class="form-control" id="alarmPeriod">
        <option value="AM">AM</option>
        <option value="PM">PM</option>
      </select>
    </div>
    <button class="btn btn-primary" onclick="setAlarm()">Set Alarm</button>
    <div id="alarmsList" class="mt-4"></div>
  </div>
  
  <script src="script.js"></script>
</body>
</html>


CSS CODE For Alarm Clock 

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
.form-control {
  width: 100%;
  padding: 10px;
  font-size: 18px;
  margin-top: 10px;
}

/* Button Styles */
.btn {
  display: inline-block;
  padding: 10px 20px;
  margin-top: 10px;
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

/* Clock Styles */
.clock {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background-color: #333;
  position: relative;
  display: inline-block;
  margin: 20px;
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

/* Alarms List Styles */
#alarmsList {
  margin-top: 20px;
  color: #fff;
}


JAVASCRIPT of the Simple Alarm Clock

var alarmSound = new Audio('alarm_sound.mp3');
var alarms = [];

function setAlarm() {
  var alarmHour = parseInt(document.getElementById('alarmHour').value);
  var alarmMinute = parseInt(document.getElementById('alarmMinute').value);
  var alarmSecond = parseInt(document.getElementById('alarmSecond').value);
  var alarmPeriod = document.getElementById('alarmPeriod').value;
  
  if (isNaN(alarmHour) || isNaN(alarmMinute) || isNaN(alarmSecond)) {
    alert("Please enter valid numbers for hour, minute, and second.");
    return;
  }
  
  if (alarmHour < 1 || alarmHour > 12 || alarmMinute < 0 || alarmMinute > 59 || alarmSecond < 0 || alarmSecond > 59) {
    alert("Please enter valid values for hour (1-12), minute (0-59), and second (0-59).");
    return;
  }

  var now = new Date();
  var alarmTime = new Date();

  alarmTime.setHours(alarmHour);
  alarmTime.setMinutes(alarmMinute);
  alarmTime.setSeconds(alarmSecond);
  alarmTime.setMilliseconds(0);

  if (alarmPeriod === "PM") {
    alarmTime.setHours(alarmTime.getHours() + 12);
  }

  if (alarmTime < now) {
    alarmTime.setDate(alarmTime.getDate() + 1);
  }

  var timeDifference = alarmTime - now;

  alarms.push({ time: alarmTime, id: alarms.length + 1 });

  setTimeout(function() {
    playAlarm();
  }, timeDifference);

  displayAlarms();
}

function playAlarm() {
  alarmSound.play();
  alert('Wake up!');
}

function displayAlarms() {
  var alarmsListDiv = document.getElementById('alarmsList');
  alarmsListDiv.innerHTML = "";

  alarms.forEach(function(alarm) {
    var alarmTime = alarm.time.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    var deleteButton = '<button class="btn btn-danger" onclick="deleteAlarm(' + alarm.id + ')">Delete</button>';
    alarmsListDiv.innerHTML += '<p>Alarm set for ' + alarmTime + ' ' + deleteButton + '</p>';
  });
}

function deleteAlarm(alarmId) {
  alarms = alarms.filter(function(alarm) {
    return alarm.id !== alarmId;
  });

  displayAlarms();
}
