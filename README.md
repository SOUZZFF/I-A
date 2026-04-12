<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OMK IA - A Mais Braba do Rio</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

        body {
            margin: 0;
            background: linear-gradient(135deg, #0a0a0a, #1a0000);
            font-family: 'Arial', sans-serif;
            display: flex;
            flex-direction: column;
            height: 100vh;
            color: white;
            overflow: hidden;
        }

        header {
            background: #000;
            color: #00ff88;
            padding: 18px;
            text-align: center;
            font-weight: bold;
            font-size: 1.5em;
            text-shadow: 0 0 10px #00ff88;
            border-bottom: 3px solid #00ff88;
        }

        #chat {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            background: rgba(0,0,0,0.6);
        }

        .msg {
            max-width: 78%;
            padding: 14px 18px;
            border-radius: 15px;
            word-wrap: break-word;
            line-height: 1.5;
            font-size: 15.5px;
        }

        .user {
            align-self: flex-end;
            background: #00ff88;
            color: black;
            border-bottom-right-radius: 5px;
            box-shadow: 0 4px 10px rgba(0, 255, 136, 0.3);
        }

        .bot {
            align-self: flex-start;
            background: #222;
            border: 1px solid #00ff88;
            border-bottom-left-radius: 5px;
        }

        #area {
            display: flex;
            padding: 15px;
            background: #111;
            gap: 10px;
            border-top: 2px solid #00ff88;
        }

        input {
            flex: 1;
            padding: 16px;
            border: none;
            background: #1f1f1f;
            color: white;
            border-radius: 12px;
            font-size: 16px;
        }

        input:focus {
            outline: none;
            box-shadow: 0 0 0 3px #00ff88;
        }

        button {
            padding: 16px 24px;
            border: none;
            background: #00ff88;
            color: black;
            font-weight: bold;
            border-radius: 12px;
            cursor: pointer;
            font-size: 16px;
        }

        button:hover {
            background: #00cc6a;
            transform: scale(1.05);
        }

        .digitando {
            font-style: italic;
            color: #888;
        }
    </style>
</head>
<body>

    <header>OMK IA 🤖 - A Rainha do RJ</header>

    <div id="chat"></div>

    <div id="area">
        <input id="input" placeholder="Fala aí, porra... o que tu quer hoje?" autocomplete="off" />
        <button onclick="enviar()">MANDAR VER</button>
    </div>

    <script>
        const chat = document.getElementById("chat");

        function addMsg(text, cls) {
            const div = document.createElement("div");
            div.className = `msg ${cls}`;
            div.innerText = text;
            chat.appendChild(div);
            chat.scrollTop = chat.scrollHeight;
        }

        async function enviar() {
            const input = document.getElementById("input");
            const texto = input.value.trim();
            if (!texto) return;

            addMsg(texto, "user");
            input.value = "";

            // Mensagem de digitando
            const digitandoDiv = document.createElement("div");
            digitandoDiv.className = "msg bot digitando";
            digitandoDiv.innerText = "OMK tá pensando, peraí caralho...";
            digitandoDiv.id = "digitando";
            chat.appendChild(digitandoDiv);
            chat.scrollTop = chat.scrollHeight;

            try {
                const res = await fetch("https://api.openai.com/v1/chat/completions", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                        "Authorization": "Bearer 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"
                    },
                    body: JSON.stringify({
                        model: "gpt-4o-mini",
                        messages: [
                            {
                                role: "system",
                                content: "Você é a OMK IA, uma IA gamer, malandra e com personalidade forte do Rio de Janeiro. Fala com gíria pesada do RJ, xinga de leve quando der, é direta, inteligente, sarcástica e engraçada pra caralho. Sempre responde em português bem carioca."
                            },
                            {
                                role: "user",
                                content: texto
                            }
                        ]
                    })
                });

                const data = await res.json();
                document.getElementById("digitando").remove();

                const resposta = data.choices?.[0]?.message?.content || "Deu ruim aqui, tenta de novo meu irmão 😤";

                addMsg(resposta, "bot");

            } catch (error) {
                document.getElementById("digitando").remove();
                addMsg("Porra, deu erro na conexão! Verifica se a chave ainda tá viva ou se a internet tá de sacanagem.", "bot");
            }
        }

        // Enter pra enviar
        document.getElementById("input").addEventListener("keydown", (e) => {
            if (e.key === "Enter") enviar();
        });

        // Mensagem de boas-vindas
        window.onload = () => {
            setTimeout(() => {
                addMsg("E aí, seu filho da puta! Bem-vindo à OMK IA, a mais braba do Rio. Manda tua pergunta que eu tô pronta pra te humilhar ou te ajudar, dependendo do teu nível.", "bot");
            }, 600);
        };
    </script>

</body>
</html>
