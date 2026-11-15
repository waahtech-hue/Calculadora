<!DOCTYPE html>
<html lang="pt-BR" id="pagina">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora</title>
    <style>
        :root {
            --fundo: #f0f0f0;
            --fundo-calc: #ffffff;
            --tela: #e9e9e9;
            --texto: #333333;
            --botao: #dddddd;
            --botao-hover: #bbbbbb;
            --cor-operacao: #ff9500;
            --cor-operacao-hover: #e68600;
            --cor-igual: #007aff;
            --cor-igual-hover: #0066cc;
            --cor-limpar: #ff3b30;
            --cor-limpar-hover: #e03025;
        }

        [data-tema="escuro"] {
            --fundo: #121212;
            --fundo-calc: #1e1e1e;
            --tela: #2c2c2c;
            --texto: #ffffff;
            --botao: #3a3a3a;
            --botao-hover: #555555;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: sans-serif;
            transition: background-color 0.3s, color 0.3s;
        }
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: var(--fundo);
            padding: 20px;
        }
        .botoes-topo {
            position: fixed;
            top: 20px;
            right: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
        }
        .botao-tema, .botao-paleta {
            padding: 10px 16px;
            border: none;
            border-radius: 20px;
            background: var(--botao);
            color: var(--texto);
            cursor: pointer;
            font-size: 14px;
        }
        .botao-tema:hover, .botao-paleta:hover {
            background: var(--botao-hover);
        }
        .painel-cores {
            background: var(--fundo-calc);
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            margin-bottom: 20px;
            display: none;
            max-width: 315px;
        }
        .painel-cores.visivel {
            display: block;
        }
        .painel-cores h3 {
            color: var(--texto);
            margin-bottom: 12px;
            font-size: 16px;
        }
        .grupo-cores {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-bottom: 12px;
        }
        .opcao-cor {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border: 3px solid transparent;
            cursor: pointer;
            transition: transform 0.2s;
        }
        .opcao-cor:hover {
            transform: scale(1.15);
        }
        .opcao-cor.selecionada {
            border-color: var(--texto);
        }
        .linha-cor {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 8px;
        }
        .linha-cor label {
            color: var(--texto);
            font-size: 13px;
            width: 80px;
        }
        .calculadora {
            background: var(--fundo-calc);
            padding: 18px;
            border-radius: 16px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
            width: 315px;
            height: 560px;
            display: flex;
            flex-direction: column;
        }
        .tela {
            width: 100%;
            height: 110px;
            background: var(--tela);
            border-radius: 10px;
            margin-bottom: 18px;
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            justify-content: center;
            padding: 15px;
            color: var(--texto);
            word-wrap: break-word;
            word-break: break-all;
        }
        .tela .expressao {
            font-size: 18px;
            opacity: 0.7;
            margin-bottom: 5px;
        }
        .tela .resultado {
            font-size: 38px;
            font-weight: 500;
        }
        .botoes {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            flex: 1;
        }
        button {
            padding: 0;
            font-size: 20px;
            border: none;
            border-radius: 12px;
            background: var(--botao);
            color: var(--texto);
            cursor: pointer;
            transition: 0.2s;
        }
        button:active {
            transform: scale(0.96);
        }
        button:hover {
            background: var(--botao-hover);
        }
        .operador {
            background: var(--cor-operacao);
            color: white;
        }
        .operador:hover {
            background: var(--cor-operacao-hover);
        }
        .limpar {
            background: var(--cor-limpar);
            color: white;
        }
        .limpar:hover {
            background: var(--cor-limpar-hover);
        }
        .igual {
            background: var(--cor-igual);
            color: white;
        }
        .igual:hover {
            background: var(--cor-igual-hover);
        }
        .porcentagem {
            background: #a2a2a2;
            color: white;
        }
        .porcentagem:hover {
            background: #888888;
        }
        .zero {
            grid-column: span 2;
        }
    </style>
</head>
<body>

    <div class="botoes-topo">
        <button class="botao-paleta" onclick="alternarPainelCores()">🎨 Cores</button>
        <button class="botao-tema" onclick="alternarTema()">🌙 Tema Escuro</button>
    </div>

    <div class="painel-cores" id="painelCores">
        <h3>🎨 Personalizar Cores</h3>
        
        <div class="linha-cor">
            <label>Operações:</label>
            <div class="grupo-cores" id="grupoOperacoes">
                <div class="opcao-cor selecionada" style="background: #ff9500" onclick="definirCorOperacao('#ff9500', '#e68600', this)"></div>
                <div class="opcao-cor" style="background: #ff2d55" onclick="definirCorOperacao('#ff2d55', '#e02047', this)"></div>
                <div class="opcao-cor" style="background: #5856d6" onclick="definirCorOperacao('#5856d6', '#4846c0', this)"></div>
                <div class="opcao-cor" style="background: #34c759" onclick="definirCorOperacao('#34c759', '#28a745', this)"></div>
                <div class="opcao-cor" style="background: #00c7be" onclick="definirCorOperacao('#00c7be', '#00a8a1', this)"></div>
                <div class="opcao-cor" style="background: #af52de" onclick="definirCorOperacao('#af52de', '#9038c0', this)"></div>
            </div>
        </div>

        <div class="linha-cor">
            <label>Igual (=):</label>
            <div class="grupo-cores" id="grupoIgual">
                <div class="opcao-cor selecionada" style="background: #007aff" onclick="definirCorIgual('#007aff', '#0066cc', this)"></div>
                <div class="opcao-cor" style="background: #34c759" onclick="definirCorIgual('#34c759', '#28a745', this)"></div>
                <div class="opcao-cor" style="background: #ff9500" onclick="definirCorIgual('#ff9500', '#e68600', this)"></div>
                <div class="opcao-cor" style="background: #5856d6" onclick="definirCorIgual('#5856d6', '#4846c0', this)"></div>
                <div class="opcao-cor" style="background: #ff2d55" onclick="definirCorIgual('#ff2d55', '#e02047', this)"></div>
                <div class="opcao-cor" style="background: #00c7be" onclick="definirCorIgual('#00c7be', '#00a8a1', this)"></div>
            </div>
        </div>
    </div>

    <div class="calculadora">
        <div class="tela" id="tela">
            <div class="expressao" id="expressao"></div>
            <div class="resultado" id="resultado">0</div>
        </div>
        <div class="botoes">
            <button class="limpar" onclick="limpar()">C</button>
            <button class="porcentagem" onclick="calcularPorcentagem()">%</button>
            <button onclick="adicionarValor('/')" class="operador">÷</button>
            <button onclick="apagar()">⌫</button>

            <button onclick="adicionarValor('7')">7</button>
            <button onclick="adicionarValor('8')">8</button>
            <button onclick="adicionarValor('9')">9</button>
            <button onclick="adicionarValor('*')" class="operador">×</button>

            <button onclick="adicionarValor('4')">4</button>
            <button onclick="adicionarValor('5')">5</button>
            <button onclick="adicionarValor('6')">6</button>
            <button onclick="adicionarValor('-')" class="operador">−</button>

            <button onclick="adicionarValor('1')">1</button>
            <button onclick="adicionarValor('2')">2</button>
            <button onclick="adicionarValor('3')">3</button>
            <button onclick="adicionarValor('+')" class="operador">+</button>

            <button class="zero" onclick="adicionarValor('0')">0</button>
            <button onclick="adicionarValor('.')">,</button>
            <button class="igual" onclick="calcular()">=</button>
        </div>
    </div>

    <script>
        let expressao = '';
        let resultado = '0';
        const telaExpressao = document.getElementById('expressao');
        const telaResultado = document.getElementById('resultado');
        const pagina = document.getElementById('pagina');
        const botaoTema = document.querySelector('.botao-tema');
        const painelCores = document.getElementById('painelCores');
        const estilos = document.documentElement.style;

        const temaSalvo = localStorage.getItem('tema');
        if (temaSalvo === 'escuro') {
            pagina.setAttribute('data-tema', 'escuro');
            botaoTema.textContent = '☀️ Tema Claro';
        }

        const corOpSalva = localStorage.getItem('corOperacao');
        const corOpHoverSalva = localStorage.getItem('corOperacaoHover');
        const corIgualSalva = localStorage.getItem('corIgual');
        const corIgualHoverSalva = localStorage.getItem('corIgualHover');

        if (corOpSalva) estilos.setProperty('--cor-operacao', corOpSalva);
        if (corOpHoverSalva) estilos.setProperty('--cor-operacao-hover', corOpHoverSalva);
        if (corIgualSalva) estilos.setProperty('--cor-igual', corIgualSalva);
        if (corIgualHoverSalva) estilos.setProperty('--cor-igual-hover', corIgualHoverSalva);

        function alternarTema() {
            if (pagina.getAttribute('data-tema') === 'escuro') {
                pagina.removeAttribute('data-tema');
                botaoTema.textContent = '🌙 Tema Escuro';
                localStorage.setItem('tema', 'claro');
            } else {
                pagina.setAttribute('data-tema', 'escuro');
                botaoTema.textContent = '☀️ Tema Claro';
                localStorage.setItem('tema', 'escuro');
            }
        }

        function alternarPainelCores() {
            painelCores.classList.toggle('visivel');
        }

        function definirCorOperacao(cor, corHover, elemento) {
            document.querySelectorAll('#grupoOperacoes .opcao-cor').forEach(el => el.classList.remove('selecionada'));
            elemento.classList.add('selecionada');
            estilos.setProperty('--cor-operacao', cor);
            estilos.setProperty('--cor-operacao-hover', corHover);
            localStorage.setItem('corOperacao', cor);
            localStorage.setItem('corOperacaoHover', corHover);
        }

        function definirCorIgual(cor, corHover, elemento) {
            document.querySelectorAll('#grupoIgual .opcao-cor').forEach(el => el.classList.remove('selecionada'));
            elemento.classList.add('selecionada');
            estilos.setProperty('--cor-igual', cor);
            estilos.setProperty('--cor-igual-hover', corHover);
            localStorage.setItem('corIgual', cor);
            localStorage.setItem('corIgualHover', corHover);
        }

        function atualizarTela() {
            telaExpressao.textContent = expressao;
            telaResultado.textContent = resultado;
        }

        function adicionarValor(valor) {
            if (resultado === 'Erro') {
                resultado = valor;
                expressao = '';
            } else if (resultado === '0' && !['.', '-'].includes(valor)) {
                resultado = valor;
            } else if (['+', '-', '*', '/'].includes(valor)) {
                expressao = expressao + resultado + ' ' + valor + ' ';
                resultado = '0';
            } else {
                resultado += valor;
            }
            atualizarTela();
        }

        function calcularPorcentagem() {
            try {
                let valor = parseFloat(resultado.replace(',', '.'));
                resultado = (valor / 100).toString();
                atualizarTela();
            } catch {
                resultado = 'Erro';
                atualizarTela();
            }
        }

        function limpar() {
            expressao = '';
            resultado = '0';
            atualizarTela();
        }

        function apagar() {
            if (resultado.length > 1) {
                resultado = resultado.slice(0, -1);
            } else if (expressao.length > 0) {
                const partes = expressao.trim().split(' ');
                if (partes.length >= 2) {
                    resultado = partes[partes.length - 2];
                    expressao = partes.slice(0, -2).join(' ') + (partes.length > 2 ? ' ' : '');
                } else {
                    resultado = '0';
                    expressao = '';
                }
            } else {
                resultado = '0';
            }
            atualizarTela();
        }

        function calcular() {
            try {
                let conta = expressao + resultado;
                conta = conta.replace(/,/g, '.');
                resultado = eval(conta).toString();
                expressao = '';
                atualizarTela();
            } catch {
                resultado = 'Erro';
                expressao = '';
                atualizarTela();
            }
        }
    </script>
</body>
</html>
