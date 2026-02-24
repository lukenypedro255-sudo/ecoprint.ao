<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EcoPrint</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root{
--verde:#0e8f61;
--verde-claro:#14b77a;
--bg:#f5f7f6;
--dark:#121212;
}

body{
margin:0;
font-family:Arial, sans-serif;
background:var(--bg);
}

header{
background:var(--verde);
color:white;
padding:15px;
display:flex;
justify-content:space-between;
align-items:center;
}

h1{
font-size:20px;
margin:0;
}

.menu{
font-size:22px;
cursor:pointer;
}

.container{
padding:15px;
}

.card{
background:white;
padding:15px;
border-radius:12px;
box-shadow:0 4px 10px rgba(0,0,0,0.05);
margin-bottom:15px;
}

input,select{
width:100%;
padding:12px;
margin-top:10px;
border-radius:8px;
border:1px solid #ddd;
font-size:16px;
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

button:hover{
background:var(--verde-claro);
}

#menuPlano{
display:none;
position:fixed;
top:0;
right:0;
width:80%;
height:100%;
background:white;
box-shadow:-3px 0 10px rgba(0,0,0,0.2);
padding:20px;
}

.progress{
height:6px;
background:#ddd;
border-radius:10px;
margin-bottom:15px;
}

.bar{
height:100%;
width:33%;
background:var(--verde);
border-radius:10px;
transition:0.3s;
}

canvas{
margin-top:20px;
}
</style>
</head>

<body>

<header>
<h1>EcoPrint 🌱</h1>
<div class="menu" onclick="abrirMenu()">⋮</div>
</header>

<div id="menuPlano">
<h3>Plano Universitário</h3>
<p><strong>1000 páginas/mês</strong></p>
<p>20.000 AOA</p>
<button onclick="fecharMenu()">Fechar</button>
</div>

<div class="container">

<div class="progress">
<div class="bar" id="barra"></div>
</div>

<div class="card">
<input type="text" id="nome" placeholder="Seu Nome">
<input type="text" id="telefone" placeholder="Número do Responsável">
<input type="number" id="paginas" placeholder="Número de Páginas">
<select id="tipo">
<option value="25">Preto e Branco (25 AOA)</option>
<option value="50">Colorido (50 AOA)</option>
</select>
<select id="pagamento">
<option>Multicaixa Express</option>
<option>Transferência</option>
<option>Pagar na Retirada</option>
</select>

<h3 id="total">Total: 0 AOA</h3>
<p id="eco"></p>

<button onclick="confirmar()">Confirmar Pedido</button>
</div>

<div class="card">
<h3>Painel Administrativo</h3>
<button onclick="analytics()">Ver Dados</button>
<canvas id="grafico"></canvas>
</div>

</div>

<script>

/* MENU */
function abrirMenu(){
document.getElementById("menuPlano").style.display="block";
}
function fecharMenu(){
document.getElementById("menuPlano").style.display="none";
}

/* CÁLCULO AUTOMÁTICO */
document.getElementById("paginas").addEventListener("input",calcular);
document.getElementById("tipo").addEventListener("change",calcular);

function calcular(){
let tipo=parseInt(document.getElementById("tipo").value);
let paginas=parseInt(document.getElementById("paginas").value)||0;
let total=tipo*paginas;

document.getElementById("total").innerText="Total: "+total+" AOA";

/* Sugestão ecológica */
if(paginas>40){
let economia=total*0.3;
document.getElementById("eco").innerText=
"💡 Frente e verso economiza 30% ("+economia.toFixed(0)+" AOA)";
}else{
document.getElementById("eco").innerText="";
}
}

/* CONFIRMAR */
function confirmar(){
let total=document.getElementById("total").innerText;
let paginas=document.getElementById("paginas").value;

let pedidos=JSON.parse(localStorage.getItem("ecoMobile"))||[];
pedidos.push({total,paginas});
localStorage.setItem("ecoMobile",JSON.stringify(pedidos));

alert("Pedido enviado com sucesso 🌱");
}

/* ANALYTICS */
function analytics(){
let pedidos=JSON.parse(localStorage.getItem("ecoMobile"))||[];
let faturamento=0;
let paginasTotal=0;

pedidos.forEach(p=>{
faturamento+=parseInt(p.total.replace(/\D/g,''))||0;
paginasTotal+=parseInt(p.paginas)||0;
});

new Chart(document.getElementById("grafico"),{
type:"bar",
data:{
labels:["Faturamento","Páginas Impressas"],
datasets:[{
data:[faturamento,paginasTotal],
backgroundColor:["#0e8f61","#14b77a"]
}]
}
});
}

</script>

</body>
</html>
