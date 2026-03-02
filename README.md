<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EcoPrint Pro</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{
font-family:'Segoe UI',sans-serif;
background:#0c0c0c;
color:white;
display:flex;
justify-content:center;
}

.app{
width:100%;
max-width:430px;
min-height:100vh;
background:#121212;
display:flex;
flex-direction:column;
}

header{
background:linear-gradient(135deg,#00c896,#006b4f);
padding:25px;
text-align:center;
position:relative;
box-shadow:0 5px 20px rgba(0,0,0,.5);
}

header h1{font-size:24px;}

.ecoia{
position:absolute;
top:20px;
right:20px;
width:60px;
height:60px;
border-radius:50%;
background:white;
color:#006b4f;
display:flex;
align-items:center;
justify-content:center;
font-weight:bold;
cursor:pointer;
box-shadow:0 5px 15px rgba(0,0,0,.4);
}

.container{padding:20px;flex:1;}

.card{
background:#1e1e1e;
padding:20px;
border-radius:20px;
margin-bottom:20px;
box-shadow:0 10px 30px rgba(0,0,0,.5);
}

input,select{
width:100%;
padding:14px;
margin-top:12px;
border-radius:12px;
border:none;
background:#2a2a2a;
color:white;
}

button{
width:100%;
padding:15px;
margin-top:18px;
border:none;
border-radius:12px;
background:linear-gradient(135deg,#00c896,#006b4f);
color:white;
font-weight:bold;
cursor:pointer;
}

#total{
margin-top:15px;
font-size:17px;
color:#00ffae;
font-weight:bold;
}

.payment-box{
background:#111;
padding:15px;
border-radius:15px;
margin-top:15px;
font-size:13px;
line-height:1.6;
}

.status{
margin-top:10px;
font-size:14px;
color:orange;
}

footer{
text-align:center;
padding:15px;
font-size:12px;
opacity:.6;
background:#0a0a0a;
}

/* CHAT */
.chat{
display:none;
position:fixed;
bottom:20px;
right:20px;
width:330px;
max-width:90%;
background:#1f1f1f;
border-radius:20px;
padding:15px;
box-shadow:0 10px 30px rgba(0,0,0,.6);
}

.chat textarea{
width:100%;
height:80px;
border:none;
border-radius:10px;
padding:10px;
background:#2a2a2a;
color:white;
resize:none;
}

.chat-messages{
max-height:150px;
overflow-y:auto;
font-size:13px;
margin-bottom:10px;
}
</style>
</head>

<body>

<div class="app">

<header>
<h1>EcoPrint PRO 🌱</h1>
<div class="ecoia" onclick="toggleChat()">EcoIA</div>
</header>

<div class="container">

<div class="card">
<h3>Agendamento Profissional</h3>

<input type="text" id="nome" placeholder="Seu Nome">
<input type="number" id="paginas" placeholder="Número de Páginas">
<select id="tipo">
<option value="25">Preto e Branco (25 AOA)</option>
<option value="50">Colorido (50 AOA)</option>
</select>

<input type="date">
<input type="time">

<div id="total">Total: 0 AOA</div>

<div class="payment-box">
<strong>Pagamento Oficial:</strong><br><br>
📱 Multicaixa Express: <b>933664451</b><br>
🏦 IBAN: <b>006600001051553810128</b><br><br>
Após pagamento, envie o comprovativo abaixo:
</div>

<input type="file" id="comprovativo">

<div class="status" id="statusPagamento">
Status: Aguardando pagamento...
</div>

<button onclick="verificarPagamento()">Confirmar Pagamento</button>

</div>

</div>

<footer>
Criadores: Lukeny, Gariel e Elmir
</footer>

</div>

<!-- ECOIA -->
<div class="chat" id="chat">
<div class="chat-messages" id="mensagens"></div>
<textarea id="inputIA" placeholder="Pergunte algo ou escreva seu trabalho..."></textarea>
<button onclick="responderIA()">Enviar</button>
</div>

<script>
const { jsPDF } = window.jspdf;

/* CALCULO */
paginas.oninput=tipo.onchange=function(){
let total=(parseInt(paginas.value)||0)*parseInt(tipo.value);
document.getElementById("total").innerText="Total: "+total+" AOA";
}

/* PAGAMENTO */
function verificarPagamento(){
let file=document.getElementById("comprovativo").files[0];

if(!file){
alert("Envie o comprovativo primeiro.");
return;
}

document.getElementById("statusPagamento").innerText="Status: Pagamento confirmado ✅ Pedido Liberado.";
document.getElementById("statusPagamento").style.color="lightgreen";
}

/* ECOIA */
function toggleChat(){
chat.style.display=chat.style.display==="block"?"none":"block";
}

function responderIA(){
let texto=inputIA.value;
if(texto==="")return;

mensagens.innerHTML+="<div><b>Você:</b> "+texto+"</div>";

let resposta="";

if(texto.toLowerCase().includes("preço"))
resposta="Os preços são 25 AOA por página preto e branco e 50 AOA colorido.";
else if(texto.toLowerCase().includes("formato"))
resposta="Posso organizar seu trabalho em formato acadêmico com capa, índice e conclusão.";
else if(texto.toLowerCase().includes("capa"))
resposta="Posso gerar uma capa profissional com nome, disciplina e data.";
else
resposta="Sou a EcoIA Pro 🤖 Assistente inteligente do EcoPrint. Posso formatar trabalhos, gerar PDFs e responder dúvidas.";

mensagens.innerHTML+="<div style='color:#00ffae'><b>EcoIA:</b> "+resposta+"</div>";

if(confirm("Deseja gerar PDF do texto?")){
let doc=new jsPDF();
doc.text(texto,10,20,{maxWidth:180});
doc.save("EcoPrint_Pro.pdf");
}

inputIA.value="";
mensagens.scrollTop=mensagens.scrollHeight;
}
</script>

</body>
</html>
