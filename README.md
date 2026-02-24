<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>EcoPrint</title>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
*{
box-sizing:border-box;
margin:0;
padding:0;
}

html,body{
width:100%;
overflow-x:hidden;
font-family:Arial, sans-serif;
}

:root{
--verde:#0e8f61;
--bg-light:#f4f4f4;
--bg-dark:#121212;
--card-light:#ffffff;
--card-dark:#1e1e1e;
--text-light:#000;
--text-dark:#fff;
}

body{
background:var(--bg-light);
color:var(--text-light);
display:flex;
justify-content:center;
}

.app{
width:100%;
max-width:420px;
min-height:100vh;
position:relative;
}

header{
background:var(--verde);
color:white;
padding:15px;
display:flex;
justify-content:space-between;
align-items:center;
flex-direction:column;
text-align:center;
}

header h1{
font-size:20px;
}

#dataHora{
font-size:13px;
margin-top:5px;
}

.container{
padding:15px;
}

.card{
background:var(--card-light);
padding:15px;
border-radius:12px;
margin-bottom:15px;
box-shadow:0 4px 8px rgba(0,0,0,0.05);
}

input,select,textarea{
width:100%;
padding:12px;
margin-top:10px;
border-radius:8px;
border:1px solid #ddd;
font-size:15px;
}

button{
width:100%;
padding:14px;
margin-top:15px;
border:none;
border-radius:8px;
background:var(--verde);
color:white;
font-size:16px;
font-weight:bold;
}

#total{
margin-top:10px;
font-weight:bold;
}

/* DARK MODE */
.dark{
background:var(--bg-dark);
color:var(--text-dark);
}

.dark .card{
background:var(--card-dark);
}

/* BOTÃO DARK */
.toggleMode{
position:fixed;
bottom:20px;
right:20px;
width:55px;
height:55px;
border-radius:50%;
background:var(--verde);
color:white;
display:flex;
justify-content:center;
align-items:center;
font-size:20px;
cursor:pointer;
box-shadow:0 4px 10px rgba(0,0,0,0.3);
}

/* ECOIA */
.ecoia{
position:absolute;
top:15px;
right:15px;
width:50px;
height:50px;
border-radius:50%;
background:white;
display:flex;
justify-content:center;
align-items:center;
cursor:pointer;
font-size:11px;
font-weight:bold;
color:#0e8f61;
box-shadow:0 4px 8px rgba(0,0,0,0.2);
}

.chat{
display:none;
position:fixed;
bottom:90px;
right:20px;
width:300px;
max-width:90%;
background:white;
border-radius:12px;
box-shadow:0 4px 10px rgba(0,0,0,0.3);
padding:10px;
}

.chat textarea{
height:80px;
}
</style>
</head>

<body>

<div class="app">

<header>
<h1>EcoPrint 🌱</h1>
<div id="dataHora"></div>
<div class="ecoia" onclick="abrirIA()">EcoIA</div>
</header>

<div class="container">

<div class="card">
<h3>Agendamento</h3>

<input type="text" id="nome" placeholder="Seu Nome">
<input type="number" id="paginas" placeholder="Número de Páginas">
<select id="tipo">
<option value="25">Preto e Branco (25 AOA)</option>
<option value="50">Colorido (50 AOA)</option>
</select>

<input type="date" id="data">
<input type="time" id="hora">

<h3 id="total">Total: 0 AOA</h3>

<button onclick="confirmar()">Confirmar Pedido</button>
</div>

</div>

</div>

<div class="toggleMode" onclick="toggleMode()">🌙</div>

<!-- CHAT IA -->
<div class="chat" id="chatBox">
<textarea id="textoIA" placeholder="Faça uma pergunta ou escreva seu trabalho..."></textarea>
<button onclick="responderIA()">Perguntar à EcoIA</button>
</div>

<script>
const { jsPDF } = window.jspdf;

/* DATA E HORA */
function atualizarDataHora(){
let agora=new Date();
let texto=agora.toLocaleString('pt-PT',{
day:'2-digit',
month:'2-digit',
year:'numeric',
hour:'2-digit',
minute:'2-digit',
second:'2-digit'
});
document.getElementById("dataHora").innerText=texto;
}
setInterval(atualizarDataHora,1000);
atualizarDataHora();

/* CÁLCULO */
document.getElementById("paginas").addEventListener("input",calcular);
document.getElementById("tipo").addEventListener("change",calcular);

function calcular(){
let tipo=parseInt(document.getElementById("tipo").value);
let paginas=parseInt(document.getElementById("paginas").value)||0;
let total=tipo*paginas;
document.getElementById("total").innerText="Total: "+total+" AOA";
}

/* CONFIRMAR */
function confirmar(){
alert("Agendamento confirmado ✅");
}

/* DARK MODE */
function toggleMode(){
document.body.classList.toggle("dark");
}

/* ECOIA */
function abrirIA(){
let chat=document.getElementById("chatBox");
chat.style.display = chat.style.display==="block"?"none":"block";
}

/* IA SIMULADA INTELIGENTE */
function responderIA(){
let texto=document.getElementById("textoIA").value.trim();

if(texto===""){
alert("Digite algo.");
return;
}

let resposta="";

if(texto.toLowerCase().includes("capa")){
resposta="Posso criar uma capa formatada para seu trabalho.";
}
else if(texto.toLowerCase().includes("resumo")){
resposta="Posso gerar um resumo estruturado com introdução, desenvolvimento e conclusão.";
}
else{
resposta="Posso organizar seu texto e formatar pronto para impressão.";
}

if(confirm(resposta+"\n\nDeseja transformar em PDF?")){
let doc=new jsPDF();
doc.setFontSize(12);
doc.text(texto,10,20,{maxWidth:180});
doc.save("EcoPrint_Trabalho.pdf");
}
}
</script>

</body>
</html>
