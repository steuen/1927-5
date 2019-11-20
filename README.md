# 1927-5
Final Projects Software Development
<!DOCTYPE html>
<html>

<head>
<title>SAASBOT</title>
<style type="text/css">
summary {
cursor: pointer;
}
</style>
</head>

<body>
<div>
<button id="toggleRTS">toggleRTS</button>
<button id="reset">reset</button>
<button id="forward">forward</button>
<button id="safe">safe</button>
<button id="full">full</button>
<button id="start">start</button>
<button id="halt">halt</button>
<button id="cw" class="drive" data-command='{"left":-100,"right":100}'>cw</button>
<button id="nw" class="drive" data-command='{"left":100,"right":50}'>nw</button>
<button id="back" class="drive" data-command='{"left":-50,"right":-50}'>back</button>
<button class="drive" data-command='{"left":100,"right":-100}'>ccw</button>
<button class="drive" data-command='{"left":50,"right":100}'>ne</button>
<button class="drive" data-command='{"left":100,"right":50}'>nw</button>
<audio controls id="myAudio">
<source src="http://math.seattleacademy.org/colincole/New%20Recording%2022.m4a" type="audio/mpeg">
</audio>
</div>
Left <div id="leftSpeed">0</div>
Right <div id="rightSpeed">0</div>
<details open>
<summary>sensors</summary>
<pre id="sensors">0</pre>
</details>
Voltage <div id='millivolts'>.</div>
<script src='js/jquery-3.4.1.min.js'></script>
<script>
var latestDate = {};

function sensorUpdate(data) {
console.log(JSON.stringify(data, null, 4));
latestData = data;
console.log(data)


}
console.log(window.document.location)

// var host = window.document.location.host;
// var ws = new WebSocket('ws://' + host);
// //var ws = new WebSocket('ws://pi5:5001');
// ws.onmessage = function(event) {
// sensorUpdate(JSON.parse(event.data));
// };


function readSensors() {
fetch('/sensors')
.then((resp) => resp.json())
.then(function(data) {
latestData = data;
// console.log(latestData)
$('#millivolts').text(latestData.voltage);
let sensorsNow = JSON.stringify(latestData, null, 2)
//sensorsNow = sensorsNow.split(',').join('\n');
$('#sensors').text(sensorsNow);
console.log(sensorsNow)
if (sensorsNow.bumps > 0){
console.log("bumps")
function playAudio()}
})

}
let readSensorTimer = setInterval(readSensors, 200);

$(".drive").click(function(e) {
command = $(this).data('command');
$.post("/drive", JSON.stringify({
left: command.left,
right: command.right
}));
});

$("#reset").click(function() {
console.log('reset')
$.post("/reset");
});


$("#toggleRTS").click(function() {
console.log('toggleRTS')
$.post("/toggleRTS");
});
$("#forward").click(function() {
console.log(JSON.stringify({
left: 20,
right: 30
}))
$.post("/drive", JSON.stringify({
left: 20,
right: 30
}));
});
$("#halt").click(function() {
$.post("/drive", JSON.stringify({
left: 0,
right: 0
}));
});
$("textarea").keydown(function(e) {
e.stopPropagation()
});

$("body").keydown(function(e) {
// iv = 0;

console.log(e.keyCode);
var key = e.keyCode;
oldRight = $("#rightSpeed").text() * 1;
oldLeft = $("#leftSpeed").text() * 1;
if (oldRight > 500) {
oldRight = 500;
} else if (oldRight < -500) {
oldRight = -500;
}
if (oldLeft > 500) {
oldLeft = 500;
} else if (oldLeft < -500) {
oldLeft = -500;
}

function slowToStop() {;
if (oldLeft <= 50 && oldLeft >= -50) {
oldLeft = 0;
} else if (oldLeft > 50 || oldLeft < -50) {
oldLeft = Math.round(oldLeft / 2);
}
if (oldRight <= 50 && oldRight >= -50) {
oldRight = 0;
} else if (oldRight > 50 || oldRight < -50) {
oldRight = Math.round(oldRight / 2);
}
}
if (key == 87) {
oldLeft += 50;
oldRight += 50;
console.log("forward", oldLeft, oldRight);
$.post("/drive", JSON.stringify({
left: oldLeft,
right: oldRight
}));
}
if (key == 83) {
oldRight -= 50;
oldLeft -= 50;
console.log("backward", oldLeft, oldRight);
$.post("/drive", JSON.stringify({
left: oldLeft,
right: oldRight
}));
}
if (key == 65) {
oldRight -= 50;
oldLeft += 50;
console.log("rotateright", oldLeft, oldRight);
$.post("/drive", JSON.stringify({
left: oldLeft,
right: oldRight
}));
}
if (key == 68) {
oldRight += 50;
oldLeft -= 50;
console.log("rotateleft", oldLeft, oldRight);
$.post("/drive", JSON.stringify({
left: oldLeft,
right: oldRight
}));
}
if (key == 13) {
oldRight = 0;
oldLeft = 0;
console.log("rotateleft", oldLeft, oldRight);
$.post("/drive", JSON.stringify({
left: oldLeft,
right: oldRight
}));
}
if (key == 32) {
slowToStop();
console.log("stop");
$.post("/drive", JSON.stringify({
left: oldLeft,
right: oldRight
}));
}
$("#leftSpeed").text(oldLeft);
$("#rightSpeed").text(oldRight);

});

$("#sing").click(function() {
song = $("#song").val();
$.post("/sing", JSON.stringify({
song: song
}));
});
$("#safe").click(function() {
$.post("/safe");
});

$("#full").click(function() {
$.post("/full");
});
$("#start").click(function() {
$.post("/start");
});
$("button").focus(function(e) {
this.blur(); //Prevents space bar from clicking button
});
</script>
</body>

</html>
