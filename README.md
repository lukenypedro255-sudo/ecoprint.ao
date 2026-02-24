<!DOCTYPE html>
<html lang="pt-ao">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoPrint - Revolução Sustentável</title>
    <style>
        :root { --primary: #00b894; --dark: #1e272e; --gray: #f4f7f6; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--gray); margin: 0; padding: 20px; color: var(--dark); }
        .container { max-width: 500px; background: white; padding: 25px; border-radius: 15px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); margin: auto; }
        .header { text-align: center; border-bottom: 2px solid var(--primary); margin-bottom: 20px; }
        .step { display: none; } .step.active { display: block; }
        input, select { width: 100%; padding: 12px; margin: 10px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; }
        button { width: 100%; padding: 15px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; transition: 0.3s; }
        .btn-next { background: var(--primary); color: white; margin-top: 10px; }
        .eco-badge { background: #e8f8f5; color: #009432; padding: 15px; border-radius: 8px; font-size: 0.9em; margin: 15px 0; border-left: 5px solid var(--primary); }
        .price-tag { font-size: 1.2em; font-weight: bold; color: var(--primary); text-align: center; display: block; }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h2 style="color: var(--primary);">EcoPrint 🌿</h2>
        <p>Impressão Inteligente e Sustentável</p>
    </div>

    <div class="step active" id="step1">
        <input type="text" placeholder="Nome do Cliente">
        <input type="tel" placeholder="Telemóvel (Ex: 923000000)">
        <select id="plano">
            <option value="avulso">Pedido Avulso</option>
            <option value="student">Plano Eco-Student (Assinatura)</option>
        </select>
        <button class="btn-next" onclick="nextStep(2)">Próximo: Detalhes da Impressão</button>
    </div>

    <div class="step" id="step2">
        <input type="number" id="paginas" placeholder="Número de Páginas" oninput="calc()">
        <select id="tipo" onchange="calc()">
            <option value="10">Preto e Branco (Econômico)</option>
            <option value="50">Colorido Premium</option>
        </select>
        <input type="file" id="fileInput">
        
        <div class="eco-badge" id="ecoInfo">
            Sua impressão economizará <b>0 litros</b> de água.
        </div>
        
        <span class="price-tag" id="totalDisplay">Total: 0 Kz</span>
        <button class="btn-next" onclick="nextStep(3)">Próximo: Pagamento</button>
    </div>

    <div class="step" id="step3">
        <p>Selecione o Método:</p>
        <button style="background: #0984e3; color: white; margin-bottom: 10px;">Pagar via Referência/App</button>
        <button style="background: #2d3436; color: white;">Pagar na Retirada (QR-Code)</button>
        <button class="btn-next" style="background: #ccc;" onclick="nextStep(1)">Voltar</button>
    </div>
</div>

<script>
    function nextStep(s) {
        document.querySelectorAll('.step').forEach(el => el.classList.remove('active'));
        document.getElementById('step' + s).classList.add('active');
    }

    function calc() {
        const pags = document.getElementById('paginas').value || 0;
        const preco = document.getElementById('tipo').value;
        const plano = document.getElementById('plano').value;
        
        let total = pags * preco;
        if(plano === 'student') total *= 0.8; // 20% de desconto na assinatura

        document.getElementById('totalDisplay').innerText = `Total: ${total.toFixed(2)} Kz`;
        // Lógica Revolucionária: Cada folha poupa aprox 0.5L de água no processo EcoPrint
        document.getElementById('ecoInfo').innerHTML = `Sua impressão poupará <b>${(pags * 0.5).toFixed(1)} litros</b> de água comparado ao processo comum.`;
    }
</script>

</body>
</html>
