<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>NOVA Assistant</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    background: #05070d;
    color: white;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
}

.container {
    width: 100%;
    max-width: 600px;
    padding: 25px;
    text-align: center;
}

h1 {
    font-size: 42px;
    margin-bottom: 5px;
    letter-spacing: 5px;
}

.subtitle {
    color: #888;
    margin-bottom: 35px;
}

.orb {
    width: 190px;
    height: 190px;
    margin: auto;
    border-radius: 50%;
    background: radial-gradient(circle at center, #ffffff 0%, #6d7cff 15%, #273cff 45%, #080b25 70%);
    box-shadow: 0 0 30px #394cff, 0 0 80px #1e2cff;
    animation: pulse 3s infinite;
}

@keyframes pulse {
    0%,100% {
        transform: scale(1);
        box-shadow: 0 0 30px #394cff, 0 0 80px #1e2cff;
    }

    50% {
        transform: scale(1.08);
        box-shadow: 0 0 50px #6570ff, 0 0 120px #293cff;
    }
}

button {
    margin-top: 35px;
    padding: 18px 35px;
    border: none;
    border-radius: 40px;
    background: white;
    color: black;
    font-size: 18px;
    font-weight: bold;
    cursor: pointer;
}

button:active {
    transform: scale(.95);
}

#status {
    margin-top: 20px;
    color: #aaa;
}

.chat {
    margin-top: 30px;
    background: #10131d;
    border-radius: 18px;
    padding: 18px;
    min-height: 70px;
    text-align: left;
}

#response {
    color: #ddd;
    line-height: 1.5;
}
</style>
</head>

<body>

<div class="container">

    <h1>NOVA</h1>

    <div class="subtitle">
        Tu asistente inteligente
    </div>

    <div class="orb"></div>

    <button onclick="startNOVA()">
        🎙️ Hablar con NOVA
    </button>

    <div id="status">
        Pulsa el botón para hablar
    </div>

    <div class="chat">
        <div id="response">
            NOVA está lista.
        </div>
    </div>

</div>

<script>

const statusText = document.getElementById("status");
const responseText = document.getElementById("response");

const SpeechRecognition =
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

let recognition;

if (SpeechRecognition) {

    recognition = new SpeechRecognition();

    recognition.lang = "es-CO";
    recognition.continuous = false;
    recognition.interimResults = false;

    recognition.onstart = function() {
        statusText.innerText = "🎙️ Escuchando...";
    };

    recognition.onresult = function(event) {

        const text =
            event.results[0][0].transcript.toLowerCase();

        statusText.innerText = "Procesando...";

        responseText.innerText =
            "Tú: " + text;

        processCommand(text);
    };

    recognition.onerror = function(event) {

        statusText.innerText =
            "Error del micrófono: " + event.error;
    };

    recognition.onend = function() {

        if (statusText.innerText === "🎙️ Escuchando...") {
            statusText.innerText = "Pulsa nuevamente para hablar";
        }
    };

} else {

    statusText.innerText =
        "Tu navegador no permite reconocimiento de voz.";
}


function startNOVA() {

    if (!recognition) {

        speak("Tu navegador no permite reconocimiento de voz.");

        return;
    }

    recognition.start();
}


function processCommand(text) {

    let answer = "";

    if (
        text.includes("hola") ||
        text.includes("buenas")
    ) {

        answer =
            "Hola. Soy NOVA. ¿En qué puedo ayudarte?";

    }

    else if (
        text.includes("cómo estás") ||
        text.includes("como estas")
    ) {

        answer =
            "Estoy funcionando correctamente.";

    }

    else if (
        text.includes("qué puedes hacer") ||
        text.includes("que puedes hacer")
    ) {

        answer =
            "Puedo escucharte, responder comandos y ayudarte a interactuar con tu sistema.";

    }

    else if (
        text.includes("hora")
    ) {

        const now = new Date();

        answer =
            "Son las " +
            now.toLocaleTimeString("es-CO", {
                hour: "2-digit",
                minute: "2-digit"
            });

    }

    else if (
        text.includes("abre youtube")
    ) {

        answer = "Abriendo YouTube.";

        speak(answer);

        setTimeout(function() {
            window.open(
                "https://www.youtube.com",
                "_blank"
            );
        }, 1000);

        return;
    }

    else if (
        text.includes("abre google")
    ) {

        answer = "Abriendo Google.";

        speak(answer);

        setTimeout(function() {
            window.open(
                "https://www.google.com",
                "_blank"
            );
        }, 1000);

        return;
    }

    else {

        answer =
            "Escuché: " +
            text +
            ". Todavía estoy aprendiendo a responder esa solicitud.";
    }

    responseText.innerText = "NOVA: " + answer;

    speak(answer);

    statusText.innerText =
        "Pulsa el botón para hablar";
}


function speak(text) {

    if (!("speechSynthesis" in window)) {
        return;
    }

    window.speechSynthesis.cancel();

    const voice =
        new SpeechSynthesisUtterance(text);

    voice.lang = "es-CO";
    voice.rate = 1;
    voice.pitch = 1;

    window.speechSynthesis.speak(voice);
}

</script>

</body>
</html>
