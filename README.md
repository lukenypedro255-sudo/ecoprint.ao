<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>EcoPrint</title>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
}

html,body{
width:100%;
overflow-x:hidden;
font-family: 'Segoe UI', sans-serif;
background:#0f0f0f;
color:white;
display:flex;
justify-content:center;
}

.app{
width:100%;
max-width:420px;
min-height:100vh;
background:#111;
position:relative;
display:flex;
flex-direction:column;
}

header{
background:linear-gradient(135deg,#0e8f61,#0a5f43);
padding:20px;
text-align:center;
position:relative;
}

header h1{
font-size:22px;
}

#dataHora{
font-size:13px;
opacity:0.8;
margin-top:5px;
}

.ecoia{
position:absolute;
top:15px;
right:15px;
width:55px;
height:55px;
border-radius:50%;
background:white;
color:#0e8f61;
display:flex;
justify-content:center;
align-items:center;
font-weight:bold;
cursor:pointer;
box-shadow:0 4px 10px rgba(0,0,0,0.4);
}

.container{
padding:15px;
flex:1;
}

.card{
background:#1b1b1b;
padding:15px;
border-radius:15px;
margin-bottom:15px;
box-shadow:0 5px 15px rgba(0,0,0,0.3);
}

input,select,textarea{
width:100%;
padding:12px;
margin-top:10px;
border-radius:10px;
border:none;
background:#2a2a2a;
color:white;
font-size:14px;
}

button{
width:100%;
padding:14px;
margin-top:15px;
border:none;
border-radius:10px;
background:#0e8f61;
color:white;
font-size:15px;
font-weight:bold;
cursor:pointer;
transition:0.3s;
}

button:hover{
background:#0a5f43;
}

#total{
margin-top:10px;
font-size:16px;
font-weight:bold;
color:#00ffae;
}

.toggleMode{
position:fixed;
bottom:20px;
right:20px;
width:60px;
height:60px;
border-radius:50%;
background:#0e8f61;
display:flex;
justify-content:center;
align-items:center;
font-size:22px;
cursor:pointer;
box-shadow:0 4px 15px rgba(0,0,0,0.5);
}

.chat{
display:none;
position:fixed;
bottom:90px;
right:20px;
width:320px;
max-width:90%;
background:#1c1c1c;
border-radius:15px;
box-shadow:0 5px 15px rgba(0,0,0,0.5);
padding:15px;
}

.chat textarea{
height:90px;
resize:none;
}

.chatResposta{
margin-top:10px;
font-size:14px;
color:#00ffae;
}

footer{
background:#0a0a0a;
text-align:center;
padding:15px;
font-size:13px;
opacity:0.7;
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

<input type="text" placeholder="Seu Nome">
<input type="number" id="paginas" placeholder="Número de Páginas">
<select id="tipo">
<option value="25">Preto e Branco (25 AOA)</option>
<option value="50">Colorido (50 AOA)</option>
</select>

<input type="date">
<input type="time">

<select>
<option>Pagamento por Transferência</option>
<option>Pagamento por Multicaixa</option>
<option>Pagamento por Referência</option>
</select>

<input type="tel" placeholder="Número do Responsável para Pagamento">

<div id="total">Total: 0 AOA</div>

<button onclick="confirmar()">Confirmar Pedido</button>
</div>

</div>

<footer>
Criadores: Lukeny, Gariel e Elmir
</footer>

</div>

<div class="toggleMode" onclick="toggleMode()">🌙</div>

<!-- CHAT IA -->
<div class="chat" id="chatBox">
<textarea id="textoIA" placeholder="Pergunte algo ou escreva seu trabalho..."></textarea>
<button onclick="responderIA()">Perguntar</button>
<div class="chatResposta" id="respostaIA"></div>
</div>

<script>
const { jsPDF } = window.jspdf;

/* DATA */
function atualizarDataHora(){
let agora=new Date();
document.getElementById("dataHora").innerText=
agora.toLocaleString("pt-PT");
}
setInterval(atualizarDataHora,1000);

/* TOTAL AUTOMÁTICO */
document.getElementById("paginas").addEventListener("input",calcular);
document.getElementById("tipo").addEventListener("change",calcular);

function calcular(){
let paginas=parseInt(document.getElementById("paginas").value)||0;
let valor=parseInt(document.getElementById("tipo").value);
let total=paginas*valor;
document.getElementById("total").innerText="Total: "+total+" AOA";
}

/* CONFIRMAR */
function confirmar(){
alert("Pedido enviado com sucesso ✅");
}

/* DARK MODE */
function toggleMode(){
document.body.classList.toggle("light");
}

/* ECOIA */
function abrirIA(){
let chat=document.getElementById("chatBox");
chat.style.display=chat.style.display==="block"?"none":"block";
}

/* IA MAIS INTELIGENTE */
function responderIA(){
let texto=document.getElementById("textoIA").value.toLowerCase();
let resposta="";

if(texto.includes("preço")){
resposta="O preço depende do tipo de impressão. Preto e Branco custa 25 AOA por página e Colorido 50 AOA.";
}
else if(texto.includes("horário")){
resposta="Funcionamos das 8h às 18h de segunda a sábado.";
}
else if(texto.includes("como imprimir")){
resposta="Basta preencher o agendamento, escolher o tipo de impressão e confirmar.";
}
else{
resposta="Posso ajudar com informações sobre impressão, preços, horários ou formatar seu trabalho.";
}

document.getElementById("respostaIA").innerText=resposta;

if(confirm("Deseja transformar seu texto em PDF formatado?")){
let doc=new jsPDF();
doc.text(document.getElementById("textoIA").value,10,20,{maxWidth:180});
doc.save("EcoPrint.pdf");
}
}
</script>

</body>
</html>
