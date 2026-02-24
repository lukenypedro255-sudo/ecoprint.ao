<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EcoPrint - Agendamento de Impressões</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #0f2027, #2c5364);
    color: white;
}

header {
    text-align: center;
    padding: 30px;
    background: rgba(0,0,0,0.4);
}

header h1 {
    margin: 0;
    font-size: 40px;
    color: #00ffcc;
}

.container {
    max-width: 800px;
    margin: 30px auto;
    background: white;
    color: black;
    padding: 30px;
    border-radius: 15px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.5);
}

input, select, button {
    width: 100%;
    padding: 12px;
    margin-top: 10px;
    border-radius: 8px;
    border: 1px solid #ccc;
    font-size: 16px;
}

button {
    background: #00b894;
    color: white;
    border: none;
    cursor: pointer;
    font-weight: bold;
}

button:hover {
    background: #019875;
}

.agendamentos {
    margin-top: 30px;
}

.agendamento-item {
    background: #f1f1f1;
    padding: 15px;
    border-radius: 10px;
    margin-top: 10px;
}

.cancelar {
    background: red;
    margin-top: 10px;
}
</style>
</head>

<body>

<header>
    <h1>EcoPrint</h1>
    <p>Agende a impressão do seu trabalho de forma rápida e ecológica 🌱</p>
</header>

<div class="container">
    <h2>Agendar Impressão</h2>

    <input type="text" id="nome" placeholder="Seu Nome" required>
    <input type="email" id="email" placeholder="Seu Email" required>
    
    <select id="tipo">
        <option value="">Tipo de Impressão</option>
        <option>Preto e Branco</option>
        <option>Colorida</option>
    </select>

    <input type="number" id="paginas" placeholder="Número de Páginas">
    <input type="date" id="data">
    <input type="time" id="hora">

    <button onclick="agendar()">Agendar Impressão</button>

    <div class="agendamentos">
        <h2>Agendamentos Marcados</h2>
        <div id="lista"></div>
    </div>
</div>

<script>
function carregarAgendamentos() {
    const lista = document.getElementById("lista");
    lista.innerHTML = "";

    const agendamentos = JSON.parse(localStorage.getItem("ecoprint")) || [];

    agendamentos.forEach((ag, index) => {
        lista.innerHTML += `
            <div class="agendamento-item">
                <strong>${ag.nome}</strong><br>
                Email: ${ag.email}<br>
                Tipo: ${ag.tipo}<br>
                Páginas: ${ag.paginas}<br>
                Data: ${ag.data} às ${ag.hora}<br>
                <button class="cancelar" onclick="cancelar(${index})">Cancelar</button>
            </div>
        `;
    });
}

function agendar() {
    const nome = document.getElementById("nome").value;
    const email = document.getElementById("email").value;
    const tipo = document.getElementById("tipo").value;
    const paginas = document.getElementById("paginas").value;
    const data = document.getElementById("data").value;
    const hora = document.getElementById("hora").value;

    if (!nome || !email || !tipo || !paginas || !data || !hora) {
        alert("Preencha todos os campos!");
        return;
    }

    const novoAgendamento = { nome, email, tipo, paginas, data, hora };

    const agendamentos = JSON.parse(localStorage.getItem("ecoprint")) || [];
    agendamentos.push(novoAgendamento);

    localStorage.setItem("ecoprint", JSON.stringify(agendamentos));

    alert("Agendamento realizado com sucesso!");

    document.querySelectorAll("input, select").forEach(el => el.value = "");

    carregarAgendamentos();
}

function cancelar(index) {
    const agendamentos = JSON.parse(localStorage.getItem("ecoprint"));
    agendamentos.splice(index, 1);
    localStorage.setItem("ecoprint", JSON.stringify(agendamentos));
    carregarAgendamentos();
}

carregarAgendamentos();
</script>

</body>
</html>
