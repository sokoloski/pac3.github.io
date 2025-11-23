<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PAC - Direito</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            /* FIX DE LAYOUT PARA DESKTOP/LANDSCAPE: Define o espaço do corpo para o dado fixo na esquerda */
            display: block; 
            padding-left: 240px; 
            
            /* --- INCLUSÃO E RESPONSIVIDADE DA IMAGEM DE FUNDO --- */
            background-image: url('image.jpg');
            background-size: cover; 
            background-position: center; 
            background-repeat: no-repeat;
            background-attachment: fixed; 
            
            background-color: #f0f0f0; 
        }


        /* --- PRIMEIRA TELA (MENU) --- */
        #menu {
            text-align: center;
            margin: 50px auto; 
            background-color: rgba(255, 255, 255, 0.95); /* Fundo semi-transparente para leitura */
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            max-width: 350px;
            width: 90%;
        }

        /* Estilos dos campos de texto e botões (Mantidos/Melhorados) */
        h1 { color: #333; margin-bottom: 5px; }
        #player-inputs { margin: 20px 0; }
        .player-input { margin-bottom: 10px; }
        #player-inputs input[type="text"] { width: 90%; padding: 12px; border: 2px solid #ccc; border-radius: 10px; box-sizing: border-box; font-size: 16px; }
        #player-inputs input[type="text"]:focus { border-color: #007bff; box-shadow: 0 0 5px rgba(0, 123, 255, 0.5); outline: none; }
        #add-player, #start-game { width: 100%; padding: 12px 25px; margin-top: 10px; font-size: 16px; font-weight: bold; color: white; border: none; cursor: pointer; transition: background-color 0.3s, transform 0.1s; border-radius: 20px; }
        #add-player { background-color: #6c757d; }
        #add-player:hover { background-color: #5a6268; transform: translateY(-1px); }
        #start-game { background-color: #28a745; margin-top: 20px; }
        #start-game:hover { background-color: #218838; transform: translateY(-1px); }

        /* --- ESTILOS DO JOGO E TABULEIRO (FIX) --- */
        #game {
            display: none;
            width: 100%;
            max-width: 800px;
            /* Centraliza o tabuleiro no espaço restante */
            margin: 20px auto; 
            background-color: rgba(255, 255, 255, 0.9); /* Fundo semi-transparente para leitura */
            padding: 20px;
            border-radius: 10px;
            box-sizing: border-box; 
        }

        #board {
            display: grid;
            grid-template-columns: repeat(5, 1fr); 
            gap: 5px;
            margin: 20px 0;
            position: relative;
        }

        .house {
            width: 100%;
            height: 150px;
            background-color: #ddd;
            border: 1px solid #aaa;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            text-align: center;
            position: relative;
            transition: background-color 0.3s ease; 
        }

        .house-icon { font-size: 40px; margin-bottom: 10px; }
        .house.finish { background-color: gold !important; border: 3px solid #ccaa00; }
        .player-pin {
            width: 40px; height: 40px; border-radius: 50%; position: absolute;
            top: 5px; left: 5px; color: white; font-size: 20px;
            display: flex; align-items: center; justify-content: center;
        }

        /* --- ESTILOS DO DADO 3D (FIXO NA ESQUERDA - Desktop) --- */
        #dice-container {
            position: fixed;
            top: 20px;
            left: 20px;
            text-align: center;
            width: 180px;
            perspective: 1000px; 
            background-color: rgba(255, 255, 255, 0.9); /* Fundo semi-transparente para leitura */
            padding: 15px;
            border-radius: 10px;
        }
        
        #dice {
            width: 100px; height: 100px; position: relative;
            transform-style: preserve-3d;
            transition: transform 1.5s ease-out; 
            margin: 0 auto 15px auto; 
            color: transparent; 
        }
        
        .face {
            position: absolute; width: 100px; height: 100px;
            background-color: #ffffff; border: 1px solid #333;
            border-radius: 10px;
        }
        
        .ponto {
            width: 15px; height: 15px;
            background-color: #333; border-radius: 50%;
            margin: 3px;
        }

        /* Definição das posições das faces no cubo (Desktop) */
        .face-1 { transform: rotateY(0deg) translateZ(50px); }
        .face-6 { transform: rotateY(180deg) translateZ(50px); }
        .face-2 { transform: rotateY(90deg) translateZ(50px); }
        .face-5 { transform: rotateY(-90deg) translateZ(50px); }
        .face-3 { transform: rotateX(90deg) translateZ(50px); }
        .face-4 { transform: rotateX(-90deg) translateZ(50px); }
        
        .face-1 { display: flex; justify-content: center; align-items: center; }
        .face-2, .face-3, .face-4, .face-5, .face-6 {
            display: grid;
            grid-template-columns: repeat(3, 1fr); 
            grid-template-rows: repeat(3, 1fr);   
            padding: 5px; 
            width: 100%; 
            height: 100%;
        }
        .face-2 .ponto, .face-3 .ponto, .face-4 .ponto, .face-5 .ponto, .face-6 .ponto {
            margin: auto; 
            align-self: center; 
            justify-self: center;
        }

        #roll-button { margin-top: 10px; padding: 10px 20px; font-size: 16px; width: 100%; border-radius: 20px; background-color: #007bff; color: white; border: none; cursor: pointer; transition: background-color 0.3s; }
        #roll-button:hover { background-color: #0056b3; }
        #status { text-align: center; font-size: 18px; margin-top: 20px; color: #333; }

        /* Estilos Modais (Perguntas e Feedback) */
        #question-modal { display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background-color: white; padding: 20px; border: 1px solid black; z-index: 1000; width: 80%; max-width: 400px; border-radius: 10px; }
        #question-options button { display: block; width: 100%; margin: 5px 0; padding: 10px; border-radius: 5px;}
        
        /* Estilos de Feedback */
        #feedback { 
            display: none; 
            position: fixed; 
            top: 20%; 
            left: 50%; 
            transform: translate(-50%, -50%); 
            padding: 20px; 
            font-size: 24px; 
            font-weight: bold; 
            z-index: 1001; 
            border-radius: 10px; 
            text-align: center; 
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2); 
        }
        #feedback.correct { background-color: green; color: white; animation: bounce 1s; }
        #feedback.wrong { background-color: red; color: white; animation: shake 1s; }
        @keyframes bounce { 0%, 20%, 50%, 80%, 100% { transform: translateY(0); } 40% { transform: translateY(-30px); } 60% { transform: translateY(-15px); } }
        @keyframes shake { 0%, 100% { transform: translateX(0); } 10%, 30%, 50%, 70%, 90% { transform: translateX(-10px); } 20%, 40%, 60%, 80% { transform: translateX(10px); } }


        /* --- ESTILOS DA ROLETA (Mantidos) --- */
        #roulette-modal { 
            display: none; 
            position: fixed; 
            top: 50%; 
            left: 50%; 
            transform: translate(-50%, -50%); 
            background-color: white; 
            padding: 30px; 
            border: 1px solid #ccc; 
            z-index: 1002; 
            width: 90%; 
            max-width: 500px; 
            border-radius: 10px; 
            text-align: center; 
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
        }

        #wheel-container {
            position: relative;
            width: 300px;
            height: 300px;
            margin: 20px auto;
        }

        #roulette-wheel {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            position: relative;
            transition: transform 5s cubic-bezier(0.25, 0.1, 0.25, 1); 
            border: 5px solid gold;
            box-shadow: 0 0 10px rgba(0,0,0,0.2);
            overflow: hidden; 
        }

        #wheel-pointer {
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 0;
            height: 0;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-bottom: 25px solid red;
            z-index: 10;
        }
        
        #spin-button {
            padding: 15px 30px; 
            font-size: 18px; 
            margin-top: 20px; 
            background-color: #ffc107; 
            border: none; 
            border-radius: 30px; 
            cursor: pointer; 
            font-weight: bold;
        }


        /* AJUSTE DE RESPONSIVIDADE PARA TABLETS (max-width: 768px) E CELULARES */
        @media (max-width: 768px) { 
            body {
                padding-left: 0; 
                display: flex; 
                flex-direction: column;
                align-items: center;
                overflow-y: auto; 
            }
            #menu { margin-top: 20px; }
            #game { 
                margin: 20px auto; 
                padding: 10px; 
                max-width: 95%; 
            }
            
            /* Tabuleiro com 5 colunas para reduzir altura total */
            #board { grid-template-columns: repeat(5, 1fr); } 
            
            /* Reduz a altura das casas */
            .house { height: 90px; font-size: 10px; } 
            .house-icon { font-size: 30px; margin-bottom: 5px; } 

            .player-pin { width: 25px; height: 25px; font-size: 14px; top: 3px; left: 3px; }

            /* **AJUSTE PRINCIPAL: Contêiner do Dado Reduzido** */
            #dice-container { 
                position: static; 
                width: 100%; 
                max-width: 100px; /* Reduz para o tamanho de uma casa */
                margin: 5px auto; /* Centraliza */
                display: flex; 
                flex-direction: column; 
                align-items: center; 
                padding: 5px; /* Reduz o preenchimento */
                order: -1; 
            }

            /* **AJUSTE PRINCIPAL: Redução Extrema do Dado 3D** */
            #dice {
                width: 60px; /* Novo tamanho do dado */
                height: 60px;
                margin: 0 auto 5px auto; 
            }

            /* Ajuste as faces para o novo tamanho (60px) */
            .face {
                width: 60px; 
                height: 60px;
            }

            /* Ajuste o translateZ para metade do novo tamanho (30px) */
            .face-1 { transform: rotateY(0deg) translateZ(30px); }
            .face-6 { transform: rotateY(180deg) translateZ(30px); }
            .face-2 { transform: rotateY(90deg) translateZ(30px); }
            .face-5 { transform: rotateY(-90deg) translateZ(30px); }
            .face-3 { transform: rotateX(90deg) translateZ(30px); }
            .face-4 { transform: rotateX(-90deg) translateZ(30px); }

            /* Reduza os pontos do dado */
            .ponto {
                width: 10px; 
                height: 10px;
            }
            
            /* Ajuste do botão para caber no contêiner estreito */
            #roll-button {
                padding: 5px 10px; 
                font-size: 14px;
                margin-top: 5px;
            }
            
            /* Ajuste do status para ser mais discreto */
            #status {
                font-size: 14px;
                margin-top: 5px;
            }
        }
    </style>
</head>
<body>
    <div id="menu">
        <h1>PAC 3</h1>
        <p>Insira os nomes dos jogadores (2 a 4):</p>
        <div id="player-inputs">
            <div class="player-input"><input type="text" placeholder="Nome do Jogador 1" id="player1"></div>
            <div class="player-input"><input type="text" placeholder="Nome do Jogador 2" id="player2"></div>
            <div class="player-input"><input type="text" placeholder="Nome do Jogador 3 (Opcional)" id="player3" disabled></div>
            <div class="player-input"><input type="text" placeholder="Nome do Jogador 4 (Opcional)" id="player4" disabled></div>
        </div>
        <button id="add-player">Adicionar Jogador</button>
        <button id="start-game">Iniciar Jogo</button>
    </div>

    <div id="game">
        <div id="dice-container">
            <div id="dice">
                <div class="face face-1"><span class="ponto"></span></div>
                <div class="face face-2">
                    <span class="ponto"></span><span></span><span></span>
                    <span></span><span></span><span></span>
                    <span></span><span></span><span class="ponto"></span>
                </div>
                <div class="face face-3">
                    <span class="ponto"></span><span></span><span></span>
                    <span></span><span class="ponto"></span><span></span>
                    <span></span><span></span><span class="ponto"></span>
                </div>
                <div class="face face-4">
                    <span class="ponto"></span><span></span><span class="ponto"></span>
                    <span></span><span></span><span></span>
                    <span class="ponto"></span><span></span><span class="ponto"></span>
                </div>
                <div class="face face-5">
                    <span class="ponto"></span><span></span><span class="ponto"></span>
                    <span></span><span class="ponto"></span><span></span>
                    <span class="ponto"></span><span></span><span class="ponto"></span>
                </div>
                <div class="face face-6">
                    <span class="ponto"></span><span></span><span class="ponto"></span>
                    <span class="ponto"></span><span></span><span class="ponto"></span>
                    <span class="ponto"></span><span></span><span class="ponto"></span>
                </div>
            </div>
            <button id="roll-button">Rolar Dado</button>
            <div id="status"></div>
        </div>
        <div id="board"></div>
    </div>

    <div id="question-modal">
        <p id="question-text"></p>
        <div id="question-options"></div>
    </div>

    <div id="feedback"></div>

    <div id="roulette-modal" style="display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background-color: white; padding: 30px; border: 1px #ccc; z-index: 1002; width: 90%; max-width: 500px; border-radius: 10px; text-align: center; box-shadow: 0 10px 25px rgba(0,0,0,0.3);">
        <h2>Parabéns, <span id="winner-name">Vencedor</span>!</h2>
        <h3>Gire a Roda para Ganhar seu Prêmio!</h3>
        <div id="wheel-container">
            <div id="roulette-wheel">
                </div>
            <div id="wheel-pointer"></div>
        </div>
        <button id="spin-button">GIRAR</button>
        <p id="prize-result" style="margin-top: 20px; min-height: 25px; font-weight: bold; font-size: 20px; color: #007bff;"></p>
    </div>

    <script>
        // Configurações do jogo (mantidas)
        const numHouses = 20; 
        const colors = ['red', 'blue', 'green', 'yellow'];
        const ANIMATION_DELAY = 500; 
        let players = [];
        let currentPlayer = 0;
        let positions = [];
        let questions = [
            { question: "O que é o Código Civil Brasileiro?", options: ["Lei de trânsito", "Lei de direitos humanos", "Lei que regula relações civis", "Lei penal", "Lei trabalhista"], correct: 2, icon: '📘', bgColor: '#e6d5b8' },
            { question: "Qual o princípio da presunção de inocência?", options: ["Todo mundo é culpado até provar o contrário", "Ninguém é culpado até decisão final", "Apenas para crimes leves", "Apenas para ricos", "Não existe"], correct: 1, icon: '⚖️', bgColor: '#b9e6b8' },
            { question: "O que é habeas corpus?", options: ["Remédio constitucional contra prisão ilegal", "Tipo de contrato", "Lei de herança", "Processo civil", "Nada"], correct: 0, icon: '🔒', bgColor: '#b8e6e1' },
            { question: "Qual a idade mínima para votar no Brasil?", options: ["14", "16", "18", "21", "25"], correct: 1, icon: '🗳️', bgColor: '#e6b8c9' },
            { question: "O que é o Estatuto da Criança e do Adolescente?", options: ["Lei para idosos", "Lei para proteção de menores", "Lei ambiental", "Lei de educação", "Lei de saúde"], correct: 1, icon: '🧒', bgColor: '#d5b8e6' },
            { question: "Qual o órgão máximo do Judiciário brasileiro?", options: ["Congresso", "Presidência", "STF", "STJ", "TSE"], correct: 2, icon: '🏛️', bgColor: '#b8e6c9' },
            { question: "O que é crime hediondo?", options: ["Crime leve", "Crime grave com penas altas", "Crime econômico", "Crime virtual", "Crime ambiental"], correct: 1, icon: '🚨', bgColor: '#e6b8b8' },
            { question: "Qual o prazo para prescrição de dívida?", options: ["1 ano", "5 anos", "10 anos", "20 anos", "Eterno"], correct: 1, icon: '⏰', bgColor: '#b8d5e6' },
            { question: "O que é direito constitucional?", options: ["Direito de família", "Estudo da Constituição", "Direito penal", "Direito trabalhista", "Direito internacional"], correct: 1, icon: '📜', bgColor: '#e6e1b8' },
            { question: "Quem pode declarar guerra no Brasil?", options: ["Presidente", "Congresso", "STF", "Exército", "Povo"], correct: 1, icon: '⚔️', bgColor: '#b8e6d5' },
            { question: "O que é usucapião?", options: ["Aquisição de propriedade por tempo", "Tipo de contrato", "Crime", "Lei de trânsito", "Nada"], correct: 0, icon: '🏠', bgColor: '#e6b8d5' },
            { question: "Qual a pena máxima no Brasil?", options: ["Morte", "Perpétua", "30 anos", "50 anos", "100 anos"], correct: 2, icon: '⛓️', bgColor: '#b8b8e6' },
            { question: "O que é CLT?", options: ["Código Civil", "Consolidação das Leis Trabalhistas", "Código Penal", "Lei Ambiental", "Lei Eleitoral"], correct: 1, icon: '💼', bgColor: '#e6c9b8' },
            { question: "Qual o direito à privacidade?", options: ["Garantido pela Constituição", "Não existe", "Apenas para famosos", "Somente online", "Somente offline"], correct: 0, icon: '🔐', bgColor: '#b8e6b8' },
            { question: "O que é improbidade administrativa?", options: ["Ato honesto", "Ato ilegal de agente público", "Contrato", "Lei de herança", "Nada"], correct: 1, icon: '🚫', bgColor: '#e6b8e1' },
            { question: "Qual a idade para maioridade penal?", options: ["16", "18", "21", "14", "12"], correct: 1, icon: '👮', bgColor: '#b8d5e6' },
            { question: "O que é lei complementar?", options: ["Lei simples", "Lei que complementa a Constituição", "Lei ordinária", "Decreto", "Portaria"], correct: 1, icon: '📋', bgColor: '#e6e1b8' },
            { question: "Qual o princípio da legalidade?", options: ["Fazer o que quiser", "Só fazer o que a lei permite", "Ignorar leis", "Apenas para governo", "Nada"], correct: 1, icon: '⚖️', bgColor: '#b8e6c9' },
            { question: "O que é mandado de segurança?", options: ["Proteção contra ato ilegal", "Tipo de prisão", "Contrato", "Lei de família", "Nada"], correct: 0, icon: '🛡️', bgColor: '#e6b8d5' },
            { question: "Qual o número de deputados federais?", options: ["100", "200", "513", "300", "400"], correct: 2, icon: '🏛️', bgColor: '#b8b8e6' },
            { question: "O que é direito tributário?", options: ["Estudo de impostos", "Direito penal", "Direito civil", "Direito internacional", "Direito ambiental"], correct: 0, icon: '💸', bgColor: '#e6c9b8' },
            { question: "Quem julga o Presidente?", options: ["STF", "Congresso", "STJ", "TSE", "Povo"], correct: 1, icon: '⚖️', bgColor: '#b8e6b8' },
            { question: "O que é posse?", options: ["Direito de propriedade", "Exercício de fato sobre coisa", "Contrato", "Crime", "Lei"], correct: 1, icon: '🏠', bgColor: '#e6b8e1' },
            { question: "Qual a duração do mandato presidencial?", options: ["2 anos", "4 anos", "5 anos", "6 anos", "8 anos"], correct: 1, icon: '⏳', bgColor: '#b8d5e6' },
            { question: "O que é Código de Defesa do Consumidor?", options: ["Lei para produtores", "Lei para consumidores", "Lei penal", "Lei trabalhista", "Lei ambiental"], correct: 1, icon: '🛒', bgColor: '#e6e1b8' },
            { question: "Qual o direito à educação?", options: ["Garantido", "Opcional", "Apenas para ricos", "Somente superior", "Nada"], correct: 0, icon: '📚', bgColor: '#b8e6c9' },
            { question: "O que é ação popular?", options: ["Ação contra dano ao patrimônio público", "Ação privada", "Contrato", "Lei de herança", "Nada"], correct: 0, icon: '🗣️', bgColor: '#e6b8d5' },
            { question: "Qual o número de senadores por estado?", options: ["1", "2", "3", "4", "5"], correct: 2, icon: '🏛️', bgColor: '#b8b8e6' },
            { question: "O que é direito ambiental?", options: ["Proteção ao meio ambiente", "Direito penal", "Direito civil", "Direito trabalhista", "Direito internacional"], correct: 0, icon: '🌳', bgColor: '#b8e6b8' },
            { question: "Quem aprova leis?", options: ["Presidente", "Congresso", "STF", "STJ", "Povo"], correct: 1, icon: '📜', bgColor: '#e6c9b8' }
        ];

        // Rotações para o dado 3D
        const BASE_ROTATIONS = { 1: { x: 0, y: 0 }, 2: { x: 0, y: -90 }, 3: { x: -90, y: 0 }, 4: { x: 90, y: 0 }, 5: { x: 0, y: 90 }, 6: { x: 0, y: 180 } };
        const ROTACAO_DADO_SHUFFLE = { 1: 'rotateX(0deg) rotateY(0deg)', 2: 'rotateX(-90deg) rotateY(0deg)', 3: 'rotateX(0deg) rotateY(90deg)', 4: 'rotateX(0deg) rotateY(-90deg)', 5: 'rotateX(90deg) rotateY(0deg)', 6: 'rotateX(0deg) rotateY(180deg)' };

        // Configuração da Roleta
        const prizes = [
            { name: "Sonho de valsa", color: "#FFD700" }, 
            { name: "Ouro branco", color: "#17A2B8" }, 
            { name: "Pipoca", color: "#28A745" }, 
            { name: "bala fine", color: "#DC3545" }, 
            { name: "chocolate", color: "#6F42C1" } 
        ];
        const sectorDegree = 360 / prizes.length; 

        // Inicializar tabuleiro (com Início e Fim)
        const board = document.getElementById('board');
        for (let i = 0; i <= numHouses; i++) {
            const house = document.createElement('div');
            house.classList.add('house');
            if (i === 0) {
                house.innerHTML = '<span class="house-icon">🏁</span><br>Início';
                house.style.backgroundColor = '#cccccc';
            } else if (i === numHouses) {
                house.innerHTML = '<span class="house-icon">🏆</span><br>Fim do Jogo';
                house.classList.add('finish');
            } else {
                const qIndex = (i - 1) % questions.length;
                house.innerHTML = `<span class="house-icon">${questions[qIndex].icon}</span><br>Casa ${i}`;
                house.style.backgroundColor = questions[qIndex].bgColor;
            }
            house.dataset.house = i;
            board.appendChild(house);
        }

        // Lógica do Menu (Mantida)
        let numPlayers = 2;
        document.getElementById('add-player').addEventListener('click', () => {
            if (numPlayers < 4) {
                numPlayers++;
                document.getElementById(`player${numPlayers}`).disabled = false;
                document.getElementById(`player${numPlayers}`).placeholder = `Nome do Jogador ${numPlayers}`;
            }
        });

        document.getElementById('start-game').addEventListener('click', () => {
            for (let i = 1; i <= numPlayers; i++) {
                const name = document.getElementById(`player${i}`).value || `Jogador ${i}`;
                players.push(name);
                positions.push(0);
            }
            if (players.length < 2) {
                alert('Pelo menos 2 jogadores!');
                return;
            }
            document.getElementById('menu').style.display = 'none';
            document.getElementById('game').style.display = 'block'; 
            updateStatus();
            renderPins();
        });

        // Funções do Jogo (Mantidas)
        function renderPins() {
            document.querySelectorAll('.player-pin').forEach(pin => pin.remove());
            positions.forEach((pos, index) => {
                const house = board.querySelector(`[data-house="${pos}"]`);
                const pin = document.createElement('div');
                pin.classList.add('player-pin');
                pin.style.backgroundColor = colors[index];
                pin.innerText = players[index][0];
                pin.dataset.playerIndex = index;
                house.appendChild(pin);
            });
        }

        function updateStatus() {
            document.getElementById('status').innerText = `Vez de ${players[currentPlayer]}`;
            document.getElementById('roll-button').disabled = false;
        }
        
        // LÓGICA CORRETA DO DADO 3D (Mantida)
        document.getElementById('roll-button').addEventListener('click', () => {
            document.getElementById('roll-button').disabled = true;
            const dice = document.getElementById('dice');
            dice.style.transition = 'none';
            let rolls = 0;
            const maxRolls = 15;
            const interval = setInterval(() => {
                rolls++;
                const randomRoll = Math.floor(Math.random() * 6) + 1;
                
                dice.style.transform = `${ROTACAO_DADO_SHUFFLE[randomRoll]} rotateZ(${Math.random() * 360}deg)`;

                if (rolls > maxRolls) {
                    clearInterval(interval);
                    const roll = Math.floor(Math.random() * 6) + 1; 
                    
                    dice.style.transition = 'transform 1.5s ease-out';
                    
                    const baseRotation = BASE_ROTATIONS[roll];
                    let rotacaoX = 360 * 3 + baseRotation.x;
                    let rotacaoY = 360 * 4 + baseRotation.y;
                    
                    dice.style.transform = `rotateX(${rotacaoX}deg) rotateY(${rotacaoY}deg)`;

                    movePlayer(roll);
                }
            }, 100);
        });

        function animateMove(stepsRemaining, finalPos, initialPos) {
            const pin = document.querySelector(`.player-pin[data-player-index="${currentPlayer}"]`);
            if (!pin) return;

            if (stepsRemaining === 0) {
                positions[currentPlayer] = finalPos; 
                if (finalPos > 0) {
                    showQuestion(finalPos, initialPos);
                } else {
                    nextPlayer();
                }
                return;
            }

            const tempCurrentPos = positions[currentPlayer]; 
            const nextPos = tempCurrentPos + 1;
            
            const oldHouse = board.querySelector(`[data-house="${tempCurrentPos}"]`);
            if (oldHouse) {
                oldHouse.removeChild(pin);
            }

            const targetHouse = board.querySelector(`[data-house="${nextPos}"]`);
            if (targetHouse) {
                targetHouse.appendChild(pin);
            }
            
            positions[currentPlayer] = nextPos;

            setTimeout(() => {
                animateMove(stepsRemaining - 1, finalPos, initialPos);
            }, ANIMATION_DELAY); 
        }

        function movePlayer(roll) {
            const initialPos = positions[currentPlayer];
            let finalPos = initialPos + roll;
            
            if (finalPos >= numHouses) { 
                finalPos = numHouses;
            }

            const stepsToMove = finalPos - initialPos;
            
            if (stepsToMove > 0) {
                animateMove(stepsToMove, finalPos, initialPos);
            } else {
                if (finalPos >= numHouses) {
                    endGame();
                } else if (finalPos > 0) {
                    showQuestion(finalPos, initialPos);
                } else {
                    nextPlayer();
                }
            }
        }

        function showQuestion(newPos, oldPos) {
            const qIndex = (newPos - 1) % questions.length;
            const q = questions[qIndex];
            document.getElementById('question-text').innerText = q.question;
            const optionsDiv = document.getElementById('question-options');
            optionsDiv.innerHTML = '';
            q.options.forEach((opt, index) => {
                const btn = document.createElement('button');
                btn.innerText = opt;
                btn.addEventListener('click', () => {
                    document.getElementById('question-modal').style.display = 'none';
                    if (index === q.correct) {
                        positions[currentPlayer] = newPos; 
                        showFeedback('Parabéns! Você acertou.', 'correct');
                    } else {
                        positions[currentPlayer] = oldPos > 0 ? oldPos : 0;
                        renderPins(); 
                        showFeedback('Errado! Volte para a casa anterior.', 'wrong');
                    }
                    
                    setTimeout(() => {
                        if (positions[currentPlayer] >= numHouses) {
                            endGame();
                        } else {
                            nextPlayer();
                        }
                    }, 2000);
                });
                optionsDiv.appendChild(btn);
            });
            document.getElementById('question-modal').style.display = 'block';
        }

        function showFeedback(text, type) {
            const feedback = document.getElementById('feedback');
            feedback.innerText = text;
            feedback.classList.remove('correct', 'wrong');
            feedback.classList.add(type);
            feedback.style.display = 'block';
            setTimeout(() => {
                feedback.style.display = 'none';
            }, 2000);
        }

        function nextPlayer() {
            currentPlayer = (currentPlayer + 1) % players.length;
            updateStatus();
        }

        // Função para configurar a roleta visualmente (Mantida)
        function setupRoulette() {
            const wheel = document.getElementById('roulette-wheel');
            wheel.innerHTML = '';
            
            let gradientStops = '';
            prizes.forEach((prize, index) => {
                const start = index * sectorDegree;
                const end = (index + 1) * sectorDegree;
                gradientStops += `${prize.color} ${start}deg ${end}deg, `;
            });
            wheel.style.backgroundImage = `conic-gradient(${gradientStops.slice(0, -2)})`;

            prizes.forEach((prize, index) => {
                const textContainer = document.createElement('div');
                textContainer.style.cssText = `
                    position: absolute;
                    width: 50%;
                    height: 50%;
                    top: 0;
                    left: 50%;
                    transform-origin: 0% 100%;
                    transform: rotate(${index * sectorDegree + sectorDegree / 2}deg);
                `;
                
                const textLabel = document.createElement('span');
                const shortName = prize.name.split(' ').slice(0, 2).join(' ').replace(/,/g, ''); 
                textLabel.innerHTML = prize.name.replace(/ /g, '<br>');
                textLabel.style.cssText = `
                    position: absolute;
                    top: 25px;
                    left: 50%;
                    transform: rotate(90deg) translate(-50%, -50%); 
                    font-size: 8px;
                    font-weight: bold;
                    color: black;
                    text-shadow: 0 0 1px white;
                    padding: 2px;
                    text-align: center;
                    width: 80px;
                    line-height: 1.1;
                `;
                
                textContainer.appendChild(textLabel);
                wheel.appendChild(textContainer);
            });
        }

        // Função para girar a roleta (COM 15 SEGUNDOS DE PRÊMIO)
        function spinWheel() {
            const wheel = document.getElementById('roulette-wheel');
            const spinButton = document.getElementById('spin-button');
            const resultElement = document.getElementById('prize-result');
            
            spinButton.disabled = true;
            resultElement.innerText = 'Girando...';
            resultElement.style.color = '#007bff';
            
            const winningIndex = Math.floor(Math.random() * prizes.length); 
            const prize = prizes[winningIndex];
            
            const targetCenterAngle = winningIndex * sectorDegree + (sectorDegree / 2);
            const rotationOffset = 360 - targetCenterAngle; 
            
            const extraTurns = 10 * 360; 
            const finalRotation = extraTurns + rotationOffset;
            
            wheel.style.transform = `rotate(${finalRotation}deg)`;
            
            // Espera a transição terminar (5 segundos)
            setTimeout(() => {
                // Exibe o resultado
                resultElement.innerText = `Prêmio: ${prize.name}! 🎉`;
                resultElement.style.color = prize.color;
                
                // Finaliza o jogo após o prêmio (15 segundos)
                setTimeout(() => {
                    document.getElementById('roulette-modal').style.display = 'none';
                    // Reset do jogo
                    players = [];
                    positions = [];
                    currentPlayer = 0;
                    document.getElementById('menu').style.display = 'block';
                }, 15000); 

            }, 5000); 
        }

        // Modifica endGame para mostrar a roleta (Mantida)
        function endGame() {
            const winnerName = players[currentPlayer];
            document.getElementById('winner-name').innerText = winnerName;
            
            document.getElementById('game').style.display = 'none';
            document.getElementById('feedback').style.display = 'none';

            const wheel = document.getElementById('roulette-wheel');
            wheel.style.transition = 'none'; 
            wheel.style.transform = 'rotate(0deg)'; 
            document.getElementById('prize-result').innerText = '';
            document.getElementById('spin-button').disabled = false;
            
            void wheel.offsetWidth; 
            wheel.style.transition = 'transform 5s cubic-bezier(0.25, 0.1, 0.25, 1)';
            
            document.getElementById('roulette-modal').style.display = 'block';
        }

        // Event listener para o botão de girar roleta
        document.addEventListener('DOMContentLoaded', () => {
            setupRoulette();
            document.getElementById('spin-button').addEventListener('click', spinWheel);
        });

    </script>
</body>
</html>
