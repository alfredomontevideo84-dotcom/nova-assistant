<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NOVA</title>

<style>
*{box-sizing:border-box}

body{
margin:0;
min-height:100vh;
background:#05070d;
color:white;
font-family:Arial,sans-serif;
display:flex;
justify-content:center;
align-items:center
}

.app{
width:100%;
max-width:600px;
padding:25px;
text-align:center
}

h1{
font-size:45px;
letter-spacing:8px;
margin:5px
}

.sub{
color:#888;
margin-bottom:30px
}

.orb{
width:180px;
height:180px;
margin:20px auto;
border-radius:50%;
background:radial-gradient(circle,#fff 0%,#7180ff 15%,#293cff 45%,#07091c 72%);
box-shadow:0 0 35px #4050ff,0 0 100px #202fff;
animation:pulse 3s infinite
}

@keyframes pulse{
50%{transform:scale(1.07);box-shadow:0 0 60px #6570ff,0 0 130px #293cff}
}

button{
border:0;
border-radius:40px;
padding:18px 30px;
font-size:18px;
font-weight:bold;
cursor:pointer
}

#status{
margin:20px;
color:#aaa
}

.chat{
background:#10131d;
border-radius:18px;
padding:20px;
text-align:left;
min-height:100px
}

.user{
color:#aaa;
margin-bottom:12px
}

.nova{
line-height:1.5
}
</style>
</head>

<body>

<div class="app">

<h1>NOVA</h1>
<div class="sub">Tu asistente inteligente</div>

<div class="orb"></div>

<button onclick="hablar()">🎙️ Hablar con NOVA</button>

<div id="status">Pulsa el botón para comenzar</div>

<div class="chat">
<div id="user" class="user"></div>
<div id="nova" class="nova">Hola. Soy NOVA. Estoy lista.</div>
</div>

</div>

<script>

const Recognition =
window.SpeechRecognition ||
window.webkitSpeechRecognition;

let recognition;

if(Recognition){

recognition=new Recognition();

recognition.lang="es-CO";
recognition.continuous=false;
recognition.interimResults=false;

recognition.onstart=()=>{
document.getElementById("status").innerText="🎙️ Escuchando...";
};

recognition.onresult=(event)=>{

const texto=event.results[0][0].transcript;

document.getElementById("user").innerText="Tú: "+texto;

responder(texto);

};

recognition.onerror=(event)=>{

document.getElementById("status").innerText=
"Error: "+event.error;

};

}else{

document.getElementById("status").innerText=
"Este navegador no admite reconocimiento de voz.";

}


function hablar(){

if(recognition){

try{
recognition.start();
}catch(e){}

}

}


function responder(texto){

const t=texto.toLowerCase().trim();

let respuesta="";


// SALUDOS

if(t.match(/hola|buenas|hey|buenos días|buenas tardes|buenas noches/)){

respuesta="Hola. Soy NOVA. ¿En qué puedo ayudarte?";

}


// IDENTIDAD

else if(t.includes("quién eres") || t.includes("quien eres")){

respuesta="Soy NOVA, tu asistente virtual. Esta es mi primera versión.";

}


// CAPACIDADES

else if(
t.includes("qué puedes hacer") ||
t.includes("que puedes hacer")
){

respuesta="Puedo escucharte, responder preguntas básicas, decirte la hora, hacer cálculos y abrir algunas páginas. Mi inteligencia artificial avanzada será añadida en la siguiente etapa.";

}


// HORA

else if(t.includes("hora")){

respuesta="En este momento son las "+
new Date().toLocaleTimeString("es-CO",{
hour:"2-digit",
minute:"2-digit"
});

}


// FECHA

else if(
t.includes("qué día es") ||
t.includes("que dia es") ||
t.includes("fecha")
){

respuesta="Hoy es "+
new Date().toLocaleDateString("es-CO",{
weekday:"long",
year:"numeric",
month:"long",
day:"numeric"
});

}


// CÁLCULO

else if(
t.includes("cuánto es") ||
t.includes("cuanto es") ||
t.includes("calcula")
){

let expresion=t
.replace("cuánto es","")
.replace("cuanto es","")
.replace("calcula","")
.replace(/x/g,"*");

try{

if(/^[0-9+\-*/().\s]+$/.test(expresion)){

let resultado=Function(
'"use strict";return ('+expresion+')'
)();

respuesta="El resultado es "+resultado;

}else{

respuesta="Puedo calcular operaciones como 25 por 4 o 100 dividido entre 5.";

}

}catch{

respuesta="No pude realizar ese cálculo.";

}

}


// YOUTUBE

else if(t.includes("abre youtube")){

respuesta="Abriendo YouTube.";

hablarRespuesta(respuesta);

setTimeout(()=>{
window.open("https://www.youtube.com","_blank");
},700);

return;

}


// GOOGLE

else if(t.includes("abre google")){

respuesta="Abriendo Google.";

hablarRespuesta(respuesta);

setTimeout(()=>{
window.open("https://www.google.com","_blank");
},700);

return;

}


// RESPUESTA GENERAL

else{

respuesta=
"Entiendo que me dices: "+texto+
". Todavía no tengo conectado mi cerebro de inteligencia artificial. Esa será nuestra siguiente mejora.";

}


document.getElementById("nova").innerText=respuesta;

hablarRespuesta(respuesta);

document.getElementById("status").innerText=
"Pulsa el botón para hablar";

}


function hablarRespuesta(texto){

if(!("speechSynthesis" in window))return;

speechSynthesis.cancel();

const voz=new SpeechSynthesisUtterance(texto);

voz.lang="es-CO";
voz.rate=1;
voz.pitch=1;

speechSynthesis.speak(voz);

}

</script>

</body>
</html>
