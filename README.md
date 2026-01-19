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
