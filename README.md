<!DOCTYPE html>
<html>
<head>
    <title>Pomar na Grama</title>
    <script src="https://unpkg.com/kaplay@3001.0.0-alpha.20/dist/kaplay.js"></script>
</head>
<body style="margin: 0;">
    <script>
        kaplay();

        // Cor do fundo (Grama)
        setBackground(46, 139, 87);

        // --- JOGADOR ---
        const player = add([
            rect(30, 50),
            pos(center()),
            color(0, 100, 255),
            area(),
            body(), // Necessário para colisões
            z(10),  // Altura da camada (para ficar entre o tronco e as folhas)
            "player"
        ]);

        // --- FUNÇÃO PARA CRIAR ÁRVORE ---
        function addTree(x, y) {
            // Tronco (Obstáculo)
            add([
                rect(20, 40),
                pos(x, y),
                color(101, 67, 33),
                area(),
                body({ isStatic: true }), // Jogador não consegue empurrar
                "obstacle"
            ]);
            // Copas/Folhas (Visual)
            add([
                circle(40),
                pos(x + 10, y - 10),
                color(34, 139, 34),
                z(15), // Fica acima do jogador
                "leaves"
            ]);
        }

        // --- FUNÇÃO PARA CRIAR FRUTAS ---
        function addFruit(x, y) {
            add([
                circle(10),
                pos(x, y),
                color(255, 50, 50), // Maçã vermelha
                area(),
                "fruit"
            ]);
        }

        // Adicionando elementos no mapa
        addTree(200, 200);
        addTree(500, 150);
        addTree(300, 400);

        addFruit(250, 250);
        addFruit(550, 200);
        addFruit(100, 300);

        // --- MOVIMENTAÇÃO ---
        const SPEED = 250;
        onKeyDown("left", () => player.move(-SPEED, 0));
        onKeyDown("right", () => player.move(SPEED, 0));
        onKeyDown("up", () => player.move(0, -SPEED));
        onKeyDown("down", () => player.move(0, SPEED));

        // --- INTERAÇÃO: COLETAR FRUTAS ---
        player.onCollide("fruit", (fruit) => {
            destroy(fruit); // Remove a fruta do jogo
            add([
                text("Pegou uma fruta!", { size: 18 }),
                pos(player.pos),
                move(UP, 50),
                lifespan(1), // O texto some depois de 1 segundo
            ]);
        });

    </script>
</body>
</html># mundo-infinito
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Jogo de Celular - Grama</title>
    <style>
        body { margin: 0; overflow: hidden; background: #2e8b57; font-family: sans-serif; touch-action: none; }
        canvas { display: block; }
        #placar {
            position: absolute; top: 10px; left: 10px;
            color: white; font-size: 20px; font-weight: bold; pointer-events: none;
        }
        /* Botões para o celular */
        .controles {
            position: absolute; bottom: 20px; left: 20px;
            display: grid; grid-template-columns: repeat(3, 60px); grid-gap: 10px;
        }
        .btn {
            width: 60px; height: 60px; background: rgba(255,255,255,0.3);
            border-radius: 50%; display: flex; align-items: center; justify-content: center;
            color: white; font-weight: bold; user-select: none;
        }
    </style>
</head>
<body>

    <div id="placar">Frutas: 0</div>
    
    <div class="controles">
        <div></div><div class="btn" id="up">↑</div><div></div>
        <div class="btn" id="left">←</div><div class="btn" id="down">↓</div><div class="btn" id="right">→</div>
    </div>

    <canvas id="jogoCanvas"></canvas>

    <script>
        const canvas = document.getElementById("jogoCanvas");
        const ctx = canvas.getContext("2d");
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        let pontos = 0;
        const jogador = { x: canvas.width/2, y: canvas.height/2, tam: 30, vel: 4 };
        const teclas = {};

        // Itens do cenário
        const arvores = Array.from({length: 6}, () => ({ x: Math.random()*(canvas.width-40), y: Math.random()*(canvas.height-40) }));
        let frutas = Array.from({length: 4}, () => ({ x: Math.random()*(canvas.width-20), y: Math.random()*(canvas.height-20), colhida: false }));

        // Eventos de Teclado (PC)
        window.addEventListener("keydown", (e) => teclas[e.key] = true);
        window.addEventListener("keyup", (e) => teclas[e.key] = false);

        // Eventos de Toque (Celular)
        const configurarBotao = (id, tecla) => {
            const el = document.getElementById(id);
            el.addEventListener("touchstart", (e) => { e.preventDefault(); teclas[tecla] = true; });
            el.addEventListener("touchend", () => teclas[tecla] = false);
        };
        configurarBotao("up", "ArrowUp");
        configurarBotao("down", "ArrowDown");
        configurarBotao("left", "ArrowLeft");
        configurarBotao("right", "ArrowRight");

        function loop() {
            // Movimentação
            if (teclas["ArrowUp"] || teclas["w"]) jogador.y -= jogador.vel;
            if (teclas["ArrowDown"] || teclas["s"]) jogador.y += jogador.vel;
            if (teclas["ArrowLeft"] || teclas["a"]) jogador.x -= jogador.vel;
            if (teclas["ArrowRight"] || teclas["d"]) jogador.x += jogador.vel;

            // Limites da tela
            jogador.x = Math.max(0, Math.min(canvas.width - jogador.tam, jogador.x));
            jogador.y = Math.max(0, Math.min(canvas.height - jogador.tam, jogador.y));

            // Desenhar
            ctx.fillStyle = "#2e8b57";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Frutas
            frutas.forEach(f => {
                if(!f.colhida) {
                    ctx.fillStyle = "red";
                    ctx.beginPath(); ctx.arc(f.x, f.y, 10, 0, Math.PI*2); ctx.fill();
                    if(Math.hypot(jogador.x - f.x, jogador.y - f.y) < 25) {
                        f.colhida = true; pontos++;
                        document.getElementById("placar").innerText = "Frutas: " + pontos;
                        setTimeout(() => { f.x = Math.random()*canvas.width; f.y = Math.random()*canvas.height; f.colhida = false; }, 2000);
                    }
                }
            });

            // Arvores
            arvores.forEach(a => {
                ctx.fillStyle = "#5d3a1a"; ctx.fillRect(a.x + 10, a.y + 20, 10, 20);
                ctx.fillStyle = "#1b4d2e"; ctx.beginPath(); ctx.arc(a.x + 15, a.y + 15, 20, 0, Math.PI*2); ctx.fill();
            });

            // Jogador
            ctx.fillStyle = "white";
            ctx.fillRect(jogador.x, jogador.y, jogador.tam, jogador.tam);

            requestAnimationFrame(loop);
        }
        loop();
    </script>
</body>
</html>
