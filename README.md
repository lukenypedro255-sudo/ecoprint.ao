<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>EcoPrint</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;}
html,body{
width:100%;
overflow-x:hidden;
font-family:'Segoe UI',sans-serif;
background:#0f0f0f;
color:white;
display:flex;
justify-content:center;
}

.app{
width:100%;
max-width:430px;
min-height:100vh;
background:#111;
display:flex;
flex-direction:column;
position:relative;
}

/* HEADER */
header{
background:linear-gradient(135deg,#00b37e,#007a55);
padding:25px 20px;
text-align:center;
position:relative;
box-shadow:0 4px 15px rgba(0,0,0,0.4);
}

header h1{
font-size:24px;
letter-spacing:1px;
}

#dataHora{
font-size:13px;
opacity:.8;
margin-top:6px;
}

/* ECOIA BUTTON */
.ecoia{
position:absolute;
top:20px;
right:20px;
width:60px;
height:60px;
border-radius:50%;
background:white;
color:#007a55;
display:flex;
justify-content:center;
align-items:center;
font-weight:bold;
cursor:pointer;
box-shadow:0 5px 20px rgba(0,0,0,.5);
transition:.3s;
}
.ecoia:hover{transform:scale(1.1);}

/* CONTENT */
.container{padding:20px;flex:1;}

.card{
background:#1c1c1c;
padding:20px;
border-radius:20px;
box-shadow:0 10px 30px rgba(0,0,0,.5);
animation:fadeIn .6s ease;
}

@keyframes fadeIn{
from{opacity:0;transform:translateY(15px);}
to{opacity:1;transform:translateY(0);}
}

input,select{
width:100%;
padding:14px;
margin-top:12px;
border:none;
border-radius:12px;
background:#2a2a2a;
color:white;
font-size:14px;
outline:none;
transition:.3s;
}
input:focus,select:focus{background:#333;}

button{
width:100%;
padding:15px;
margin-top:18px;
border:none;
border-radius:12px;
background:linear-gradient(135deg,#00b37e,#007a55);
color:white;
font-weight:bold;
font-size:15px;
cursor:pointer;
transition:.3s;
}
button:hover{opacity:.85;}

#total{
margin-top:15px;
font-size:17px;
font-weight:bold;
color:#00ffae;
}

/* CHAT */
.chat{
display:none;
position:fixed;
bottom:100px;
right:20px;
width:340px;
max-width:90%;
background:#1e1e1e;
border-radius:20px;
box-shadow:0 10px 30px rgba(0,0,0,.6);
padding:15px;
}

.chat textarea{
width:100%;
height:90px;
border:none;
border-radius:12px;
padding:10px;
background:#2a2a2a;
color:white;
resize:none;
}

.chat-messages{
max-height:150px;
overflow-y:auto;
font-size:13px;
margin-bottom:8px;
}

.msg-user{color:#00ffae;margin-bottom:4px;}
.msg-bot{color:#ccc;margin-bottom:8px;}

/* SUCCESS POPUP */
.popup{
position:fixed;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:#1e1e1e;
padding:30px;
border-radius:20px;
box-shadow:0 10px 40px rgba(0,0,0,.7);
display:none;
text-align:center;
}

/* FOOTER */
footer{
text-align:center;
padding:15px;
font-size:12px;
opacity:.6;
background:#0d0d0d;
}
</style>
</head>

<body>

<div class="app">

<header>
<h1>EcoPrint 🌱</h1>
<div id="dataHora"></div>
<div class="ecoia" onclick="toggleChat()">EcoIA</div>
</header>

<div class="container">
<div class="card">
<h3>Agendamento Inteligente</h3>

<input type="text" id="nome" placeholder="Seu Nome">
<input type="number" id="paginas" placeholder="Número de Páginas">
<select id="tipo">
<option value="25">Preto e Branco (25 AOA)</option>
<option value="50">Colorido (50 AOA)</option>
</select>

<input type="date" id="data">
<input type="time" id="hora">

<select id="pagamento">
<option value="">Método de Pagamento</option>
<option>Transferência Bancária</option>
<option>Multicaixa Express</option>
<option>Referência Bancária</option>
</select>

<input type="tel" id="telefone" placeholder="Número para Pagamento">

<div id="total">Total: 0 AOA</div>

<button onclick="confirmarPedido()">Confirmar Pedido</button>
</div>
</div>

<footer>
Criadores: Lukeny, Gariel e Elmir
</footer>

</div>

<!-- CHAT -->
<div class="chat" id="chat">
<div class="chat-messages" id="mensagens"></div>
<textarea id="inputIA" placeholder="Pergunte algo..."></textarea>
<button onclick="responderIA()">Enviar</button>
</div>

<div class="popup" id="popup">
<h3>Pedido Confirmado ✅</h3>
<p>Seu agendamento foi salvo com sucesso.</p>
<button onclick="fecharPopup()">OK</button>
</div>

<script>
const { jsPDF } = window.jspdf;

/* DATA TEMPO REAL */
function atualizarHora(){
document.getElementById("dataHora").innerText=new Date().toLocaleString("pt-PT");
}
setInterval(atualizarHora,1000);

/* CALCULO AUTOMATICO */
document.getElementById("paginas").addEventListener("input",calcular);
document.getElementById("tipo").addEventListener("change",calcular);

function calcular(){
let paginas=parseInt(paginas.value)||0;
let valor=parseInt(tipo.value);
document.getElementById("total").innerText="Total: "+(paginas*valor)+" AOA";
}

/* CONFIRMAÇÃO */
function confirmarPedido(){
if(!nome.value || !paginas.value || !pagamento.value){
alert("Preencha todos os campos obrigatórios.");
return;
}
document.getElementById("popup").style.display="block";
}

/* POPUP */
function fecharPopup(){
document.getElementById("popup").style.display="none";
}

/* CHAT IA */
function toggleChat(){
let chat=document.getElementById("chat");
chat.style.display=chat.style.display==="block"?"none":"block";
}

function responderIA(){
let texto=inputIA.value.trim();
if(texto==="") return;

mensagens.innerHTML+=`<div class='msg-user'>Você: ${texto}</div>`;

let resposta="";

if(texto.includes("preço"))
resposta="Preto e Branco custa 25 AOA por página e Colorido 50 AOA.";
else if(texto.includes("horário"))
resposta="Funcionamos das 8h às 18h.";
else if(texto.includes("imprimir"))
resposta="Basta preencher o agendamento e confirmar.";
else
resposta="Sou a EcoIA 🤖 Posso ajudar com preços, horários, impressão ou formatar seu texto.";

mensagens.innerHTML+=`<div class='msg-bot'>EcoIA: ${resposta}</div>`;

if(confirm("Deseja gerar PDF do texto?")){
let doc=new jsPDF();
doc.text(texto,10,20,{maxWidth:180});
doc.save("EcoPrint.pdf");
}

inputIA.value="";
mensagens.scrollTop=mensagens.scrollHeight;
}
</script>

</body>
</html>
