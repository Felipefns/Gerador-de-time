<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Time</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #28a745; /* Fundo verde */
            color: #FFFFFF; /* Texto branco */
            margin: 0;
            padding: 20px;
        }
        h1 {
            text-align: center;
            color: #FFFFFF; /* Título em branco */
            text-transform: uppercase; /* Letras maiúsculas */
        }
        #team-form {
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: rgba(0, 0, 0, 0.8); /* Fundo preto para o formulário */
            padding: 20px;
            border-radius: 10px;
        }
        input[type="text"], select {
            margin: 5px;
            padding: 10px;
            width: 200px;
            border-radius: 5px;
            border: 1px solid #FFD700;
            background-color: #333; /* Fundo dos inputs */
            color: #FFFFFF; /* Texto dos inputs em branco */
        }
        button {
            padding: 10px 20px;
            margin-top: 10px;
            border: none;
            border-radius: 5px;
            background-color: #FFD700;
            color: #000;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #FFC107; /* Amarelo mais claro */
        }
        #results {
            margin-top: 20px;
            text-align: center;
            background-color: rgba(0, 0, 0, 0.8); /* Fundo preto para os resultados */
            padding: 10px;
            border-radius: 5px;
        }
        #playerList {
            margin-top: 20px;
            text-align: center;
        }
        #teamCount {
            margin: 10px;
            display: flex;
            align-items: center; /* Alinhar verticalmente */
        }
        /* Estilo para a lista de jogadores */
        #list {
            background-color: rgba(0, 0, 0, 0.8); /* Fundo preto para a lista de jogadores */
            padding: 10px;
            border-radius: 5px;
        }
        #list li {
            color: #FFFFFF; /* Nomes dos jogadores em branco */
            font-weight: bold; /* Nomes em negrito */
        }
        #notification {
            display: none;
            margin-left: 20px; /* Espaçamento à esquerda */
            color: #FFD700; /* Texto da notificação em amarelo */
            font-weight: bold;
            font-size: 20px; /* Aumentar o tamanho da fonte */
            background-color: rgba(0, 0, 0, 0.7); /* Fundo escuro para destaque */
            padding: 5px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h1>GERADOR DE TIME</h1>
    <div id="team-form">
        <input type="text" id="playerName" placeholder="Digite o nome do jogador" />
        <button onclick="addPlayer()">Adicionar Jogador</button>
        <div id="teamCount">
            <label for="numTeams">Escolha o número de times:</label>
            <select id="numTeams">
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
            </select>
            <div id="notification">Jogador adicionado!</div> <!-- Mensagem de notificação -->
        </div>
        <button onclick="generateTeams()">Gerar Times</button>
        <button onclick="restart()">Reiniciar</button>
    </div>
    <div id="playerList">
        <h3>Jogadores Adicionados:</h3>
        <ul id="list"></ul>
    </div>
    <div id="results">
        <h2>Times Gerados:</h2>
    </div>

    <script>
        const players = [];

        function addPlayer() {
            const playerName = document.getElementById('playerName').value.trim();
            if (playerName) {
                players.push(playerName);
                document.getElementById('playerName').value = '';
                updatePlayerList();
                showNotification(); // Mostrar notificação
            } else {
                alert("Por favor, digite um nome válido.");
            }
        }

        function updatePlayerList() {
            const list = document.getElementById('list');
            list.innerHTML = '';
            players.forEach((player, index) => {
                const listItem = document.createElement('li');
                listItem.textContent = `${index + 1}. ${player}`;
                list.appendChild(listItem);
            });
        }

        function showNotification() {
            const notification = document.getElementById('notification');
            notification.style.display = 'block';
            setTimeout(() => {
                notification.style.display = 'none';
            }, 2000); // Ocultar notificação após 2 segundos
        }

        function generateTeams() {
            const playerCount = players.length;
            const numTeams = parseInt(document.getElementById('numTeams').value);

            if (playerCount < 4) {
                alert("Você precisa de pelo menos 4 jogadores para formar um time!");
                return;
            }

            if (numTeams > playerCount) {
                alert("O número de times não pode ser maior que o número de jogadores!");
                return;
            }

            const teamSize = Math.ceil(playerCount / numTeams);
            const shuffledPlayers = players.sort(() => 0.5 - Math.random());
            const teams = [];
            for (let i = 0; i < playerCount; i += teamSize) {
                teams.push(shuffledPlayers.slice(i, i + teamSize));
            }

            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = `<h2>Times Gerados:</h2>`;
            teams.forEach((team, index) => {
                resultsDiv.innerHTML += `<h3>Time ${index + 1}:</h3><ul>${team.map((player, idx) => `<li>${idx + 1}. ${player}</li>`).join('')}</ul>`;
            });
        }

        function restart() {
            players.length = 0;
            document.getElementById('playerName').value = '';
            document.getElementById('list').innerHTML = '';
            document.getElementById('results').innerHTML = `<h2>Times Gerados:</h2>`;
            document.getElementById('numTeams').value = '2';
            alert("O formulário foi reiniciado!");
        }
    </script>
</body>
</html>
