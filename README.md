index.html
<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EcoPrint - Sistema Profissional</title>

<style>
body{
margin:0;
font-family:Arial, sans-serif;
background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
color:white;
}

header{
text-align:center;
padding:30px;
}

h1{
color:#00ffcc;
}

.container{
max-width:900px;
margin:auto;
background:white;
color:black;
padding:30px;
border-radius:15px;
}

input,select,button{
width:100%;
padding:12px;
margin-top:10px;
border-radius:8px;
font-size:16px;
}

button{
background:#00b894;
color:white;
border:none;
cursor:pointer;
font-weight:bold;
}

button:hover{
background:#019875;
}

.resumo{
background:#f1f1f1;
padding:15px;
border-radius:10px;
margin-top:20px;
}

.admin{
margin-top:40px;
background:#222;
padding:20px;
border-radius:10px;
color:white;
}
</style>
</head>

<body>

<header>
<h1>EcoPrint</h1>
<p>Impressão Inteligente e Sustentável 🌱</p>
</header>

<div class="container">

<h2>Agendar Impressão</h2>

<input type="text" id="nome" placeholder="Nome do Cliente">
<input type="text" id="responsavel" placeholder="Número do Responsável (Ex: 923000000)">
<input type="email" id="email" placeholder="Email">

<select id="tipo">
<option value="">Tipo de Impressão</option>
<option value="25">Preto e Branco (25 AOA por página)</option>
<option value="50">Colorida (50 AOA por página)</option>
</select>

<select id="encadernacao">
<option value="0">Sem Encadernação</option>
<option value="500">Encadernação Simples (+500 AOA)</option>
<option value="1000">Encadernação Premium (+1000 AOA)</option>
</select>

<input type="number" id="paginas" placeholder="Número de Páginas">
<input type="file" id="arquivo">

<h3>Método de Pagamento</h3>
<select id="pagamento">
<option>Pagamento na Retirada</option>
<option>Transferência Bancária</option>
<option>Multicaixa Express</option>
</select>

<button onclick="calcular()">Calcular Total</button>

<div class="resumo" id="resumo"></div>

<button onclick="finalizar()">Confirmar Agendamento</button>

<div class="admin">
<h3>Painel Administrativo</h3>
<button onclick="verPedidos()">Ver Pedidos</button>
<div id="listaPedidos"></div>
</div>

</div>

<script>

function calcular(){
let tipo = parseInt(document.getElementById("tipo").value);
let paginas = parseInt(document.getElementById("paginas").value);
let encadernacao = parseInt(document.getElementById("encadernacao").value);

if(!tipo || !paginas){
alert("Preencha os dados corretamente");
return;
}

let total = (tipo * paginas) + encadernacao;

document.getElementById("resumo").innerHTML =
"<strong>Total a pagar:</strong> " + total + " AOA";
}

function finalizar(){

let nome = document.getElementById("nome").value;
let responsavel = document.getElementById("responsavel").value;
let email = document.getElementById("email").value;
let pagamento = document.getElementById("pagamento").value;
let resumo = document.getElementById("resumo").innerText;

if(!nome || !responsavel || !email){
alert("Preencha todos os campos!");
return;
}

let pedido = {nome,responsavel,email,pagamento,resumo};

let pedidos = JSON.parse(localStorage.getItem("ecoprintPro")) || [];
pedidos.push(pedido);

localStorage.setItem("ecoprintPro",JSON.stringify(pedidos));

alert("Pedido Confirmado com Sucesso!");

document.querySelectorAll("input").forEach(el=>el.value="");
document.getElementById("resumo").innerHTML="";
}

function verPedidos(){
let pedidos = JSON.parse(localStorage.getItem("ecoprintPro")) || [];
let lista = document.getElementById("listaPedidos");
lista.innerHTML="";

pedidos.forEach(p=>{
lista.innerHTML+=
"<div style='background:#333;padding:10px;margin-top:10px;border-radius:8px;'>"+
"<strong>"+p.nome+"</strong><br>"+
"Responsável: "+p.responsavel+"<br>"+
"Email: "+p.email+"<br>"+
p.resumo+"<br>"+
"Método: "+p.pagamento+
"</div>";
});
}

</script>

</body>
</html>
