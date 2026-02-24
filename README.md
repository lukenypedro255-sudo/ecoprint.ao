<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EcoPrint Pro</title>

<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.10.377/pdf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root{
--verde:#00a86b;
--cinza:#f4f4f4;
--dark:#1e1e1e;
}

body{
margin:0;
font-family:Arial;
transition:0.3s;
}

.light{
background:var(--cinza);
color:#000;
}

.dark{
background:var(--dark);
color:#fff;
}

header{
text-align:center;
padding:20px;
background:var(--verde);
color:white;
}

.container{
max-width:900px;
margin:auto;
padding:20px;
}

.step{
display:none;
}

.active{
display:block;
}

button{
padding:12px;
margin-top:10px;
border:none;
border-radius:8px;
background:var(--verde);
color:white;
cursor:pointer;
}

.progress{
height:10px;
background:#ccc;
border-radius:10px;
margin-bottom:20px;
}

.progress-bar{
height:100%;
width:33%;
background:var(--verde);
border-radius:10px;
transition:0.3s;
}

canvas{
margin-top:20px;
background:white;
padding:10px;
border-radius:10px;
}
</style>
</head>

<body>

<header>
<h1>EcoPrint 🌱</h1>
<p>Impressão Inteligente e Sustentável</p>
</header>

<div class="container">

<div class="progress">
<div class="progress-bar" id="bar"></div>
</div>

<!-- ETAPA 1 -->
<div class="step active" id="step1">
<h2>1️⃣ Identificação</h2>
<input type="text" id="nome" placeholder="Nome">
<input type="text" id="telefone" placeholder="Número do Responsável">
<input type="email" id="email" placeholder="Email">
<button onclick="next(2)">Próximo</button>
</div>

<!-- ETAPA 2 -->
<div class="step" id="step2">
<h2>2️⃣ Arquivo e Opções</h2>

<select id="tipo">
<option value="25">Preto e Branco (25 AOA)</option>
<option value="50">Colorido (50 AOA)</option>
</select>

<input type="number" id="paginas" placeholder="Número de Páginas">

<input type="file" id="arquivo" accept="application/pdf">

<div id="preview"></div>

<p id="ecoSugestao"></p>

<button onclick="next(3)">Próximo</button>
</div>

<!-- ETAPA 3 -->
<div class="step" id="step3">
<h2>3️⃣ Pagamento</h2>

<select id="pagamento">
<option>Multicaixa Express</option>
<option>Transferência Bancária</option>
<option>Pagamento na Retirada</option>
</select>

<h3 id="total">Total: 0 AOA</h3>

<h3>Plano Universitário</h3>
<p>1000 páginas/mês por 20.000 AOA</p>

<button onclick="finalizar()">Confirmar Pedido</button>
</div>

<hr>

<h2>Painel Administrativo</h2>
<button onclick="verDados()">Ver Analytics</button>
<canvas id="grafico"></canvas>

</div>

<script>
/* DARK MODE AUTOMÁTICO */
let hora = new Date().getHours();
if(hora >= 18 || hora <=6){
document.body.classList.add("dark");
}else{
document.body.classList.add("light");
}

/* ETAPAS */
function next(step){
document.querySelectorAll(".step").forEach(s=>s.classList.remove("active"));
document.getElementById("step"+step).classList.add("active");
document.getElementById("bar").style.width = (step*33)+"%";
}

/* CÁLCULO AUTOMÁTICO */
document.getElementById("paginas").addEventListener("input",calcular);
document.getElementById("tipo").addEventListener("change",calcular);

function calcular(){
let tipo = parseInt(document.getElementById("tipo").value);
let paginas = parseInt(document.getElementById("paginas").value)||0;
let total = tipo*paginas;
document.getElementById("total").innerText="Total: "+total+" AOA";

/* Simulação IA ecológica */
if(paginas>50){
let economia = total*0.3;
document.getElementById("ecoSugestao").innerText=
"💡 Se usar frente e verso economiza 30% = "+economia.toFixed(0)+" AOA";
}
}

/* PREVIEW PDF */
document.getElementById("arquivo").addEventListener("change",function(e){
let file=e.target.files[0];
let reader=new FileReader();
reader.onload=function(){
let typedarray=new Uint8Array(this.result);
pdfjsLib.getDocument(typedarray).promise.then(function(pdf){
pdf.getPage(1).then(function(page){
let scale=1;
let viewport=page.getViewport({scale});
let canvas=document.createElement("canvas");
let context=canvas.getContext("2d");
canvas.height=viewport.height;
canvas.width=viewport.width;
page.render({canvasContext:context,viewport});
let preview=document.getElementById("preview");
preview.innerHTML="";
preview.appendChild(canvas);
});
});
};
reader.readAsArrayBuffer(file);
});

/* FINALIZAR */
function finalizar(){
let total=document.getElementById("total").innerText;
let paginas=document.getElementById("paginas").value;

let pedidos=JSON.parse(localStorage.getItem("ecoPro"))||[];
pedidos.push({total,paginas});
localStorage.setItem("ecoPro",JSON.stringify(pedidos));

alert("Pedido Confirmado 🌱");
}

/* ANALYTICS */
function verDados(){
let pedidos=JSON.parse(localStorage.getItem("ecoPro"))||[];
let faturamento=0;
let paginasTotal=0;

pedidos.forEach(p=>{
faturamento+=parseInt(p.total.replace(/\D/g,''))||0;
paginasTotal+=parseInt(p.paginas)||0;
});

new Chart(document.getElementById("grafico"),{
type:"bar",
data:{
labels:["Faturamento","Total de Páginas"],
datasets:[{
label:"EcoPrint Analytics",
data:[faturamento,paginasTotal],
backgroundColor:["#00a86b","#004d40"]
}]
}
});
}
</script>

</body>
</html>
