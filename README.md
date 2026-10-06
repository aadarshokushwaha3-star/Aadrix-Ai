<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Aadrix – AI Robot</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background: #111;
      color: white;
      margin: 0;
      padding: 20px;
    }

    h1 {
      color: #00ff66;
    }

    .box {
      max-width: 500px;
      margin: auto;
      background: #222;
      padding: 20px;
      border-radius: 20px;
    }

    button {
      font-size: 20px;
      padding: 15px 25px;
      margin: 7px;
      border: none;
      border-radius: 12px;
      background: #00aa55;
      color: white;
    }

    button:active {
      background: #007733;
    }

    #answer {
      margin-top: 20px;
      padding: 15px;
      background: #333;
      border-radius: 12px;
      min-height: 40px;
    }
  </style>
</head>

<body>

  <div class="box">

    <h1>🤖 AADRIX – AI</h1>

    <p>My AI Robot Assistant</p>

    <button onclick="startVoice()">🎤 बोलिए</button>

    <div id="answer">
      Aadrix तैयार है...
    </div>

    <h2>🚗 Robot Control</h2>

    <button onclick="sendCommand('forward')">⬆️ आगे</button><br>

    <button onclick="sendCommand('left')">⬅️ बायाँ</button>

    <button onclick="sendCommand('stop')">⏹️ Stop</button>

    <button onclick="sendCommand('right')">➡️ दायाँ</button><br>

    <button onclick="sendCommand('back')">⬇️ पीछे</button>

  </div>

<script>

function sendCommand(command) {

  document.getElementById("answer").innerText =
    "Command: " + command;

  // ESP32 robot control
  fetch("http://192.168.4.1/" + command)
    .catch(() => {
      console.log("ESP32 से connection नहीं हुआ");
    });
}


function speak(text) {

  const speech = new SpeechSynthesisUtterance(text);

  speech.lang = "hi-IN";

  speech.rate = 0.9;

  window.speechSynthesis.speak(speech);
}


function startVoice() {

  const SpeechRecognition =
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

  if (!SpeechRecognition) {

    document.getElementById("answer").innerText =
      "इस browser में voice recognition support नहीं है।";

    return;
  }

  const recognition = new SpeechRecognition();

  recognition.lang = "hi-IN";

  recognition.continuous = false;

  recognition.interimResults = false;

  document.getElementById("answer").innerText =
    "🎤 Aadrix सुन रहा है...";

  recognition.start();

  recognition.onresult = function(event) {

    const text =
      event.results[0][0].transcript.toLowerCase();

    document.getElementById("answer").innerText =
      "आपने कहा: " + text;

    processCommand(text);
  };

  recognition.onerror = function() {

    document.getElementById("answer").innerText =
      "आवाज़ समझ नहीं आई। फिर से बोलिए।";
  };
}


function processCommand(text) {

  if (
    text.includes("आगे") ||
    text.includes("forward")
  ) {

    sendCommand("forward");

    speak("ठीक है, आगे चल रहा हूँ।");

  }

  else if (
    text.includes("पीछे") ||
    text.includes("back")
  ) {

    sendCommand("back");

    speak("ठीक है, पीछे चल रहा हूँ।");

  }

  else if (
    text.includes("बाएं") ||
    text.includes("बाय") ||
    text.includes("left")
  ) {

    sendCommand("left");

    speak("बाएं मुड़ रहा हूँ।");

  }

  else if (
    text.includes("दाएं") ||
    text.includes("दायें") ||
    text.includes("right")
  ) {

    sendCommand("right");

    speak("दाएं मुड़ रहा हूँ।");

  }

  else if (
    text.includes("रुक") ||
    text.includes("stop")
  ) {

    sendCommand("stop");

    speak("ठीक है, रुक गया।");

  }

  else if (
    text.includes("हेलो") ||
    text.includes("hello") ||
    text.includes("नमस्ते")
  ) {

    speak("नमस्ते! मैं Aadrix हूँ। मैं आपकी मदद के लिए तैयार हूँ।");

  }

  else {

    speak(
      "मैंने आपकी बात सुनी, लेकिन इस command का जवाब अभी मेरे अंदर नहीं है।"
    );

  }
}

</script>

</body>
</html>
