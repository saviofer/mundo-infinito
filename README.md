# 🌲 Sobrevivência: Biomas e Evolução

Um jogo de sobrevivência em 2D desenvolvido com **HTML5 Canvas** e **JavaScript**, focado em exploração, coleta de recursos e progressão de equipamentos.

## 🚀 Funcionalidades Principais

* **Ciclo Dia/Noite:** O jogo escurece gradualmente. Ao dormir na cama (no bioma inicial), o dia reseta e o jogador recebe um relatório de coleta.
* **Resumo de Coleta:** Sempre que amanhece, um alerta exibe a quantidade exata de madeira, pedra e carne coletadas durante o dia anterior.
* **Sistema de Fome e Vida:** A fome diminui constantemente. Se chegar a zero, o jogador começa a perder vida. A carne pode ser consumida para restaurar a saciedade.
* **Bússola (Seta Branca):** Uma seta branca flutua ao redor do jogador, apontando permanentemente a direção da cama (ponto inicial), facilitando o retorno à base.

## 🗺️ Biomas e Mapa

O mundo é dividido em coordenadas `(X, Y)`. Cada região possui características únicas:

| Coordenada | Bioma | Características |
| --- | --- | --- |
| **(0, 0)** | **Floresta** | Ponto inicial. Contém a **Cama**, árvores e porcos. |
| **(1, 0)** | **Pedreira** | Solo rochoso com abundância de pedras para coleta. |
| **(0, 1)** | **Caverna** | Ambiente subterrâneo com **escuridão constante** e pedras. |
| **(1, 1)** | **Deserto** | Solo arenoso com **Cactos** que causam dano ao contato. |

## 🛠️ Evolução e Itens

O jogador pode evoluir suas ferramentas usando os recursos coletados:

1. **Mão:** Coleta básica de recursos.
2. **Espada (5 Madeiras):** Permite derrotar zumbis.
3. **Picareta (5 Pedras):** Aumenta a eficiência na coleta de pedras.
4. **Machado (10 Madeiras):** Aumenta a eficiência na coleta de madeira.
5. **Escudo (10 Pedras):** Fornece proteção total contra ataques de zumbis.

## 🕹️ Comandos

* **Movimentação:** Teclas `W, A, S, D` ou `Setas do Teclado`.
* **Controles Touch:** Botões direcionais na interface para dispositivos móveis.
* **Comer:** Botão "COMER CARNE" (restaura fome).
* **Crafting:** Botão "CRIAR" (evolui o equipamento atual).
* **Dormir:** Encostar na cama amarela (Bioma 0,0) durante o final do dia.

---

### 📝 Notas de Desenvolvimento

* O sistema de colisão é baseado em distância euclidiana ().
* O renderizador utiliza o `requestAnimationFrame` para garantir fluidez a 60 FPS.
* A escuridão é aplicada via camada de preenchimento `rgba` sobre o canvas principal.

### Proximos updates

#### 1. Sistema de Iluminação na Caverna

Como a caverna agora é escura, você pode criar um item de **Tocha**.

* **Mecânica:** Enquanto o jogador não tiver uma tocha, ele só enxerga o que está em volta dele (um pequeno círculo de luz).
* **Crafting:** 2 Madeiras + 1 novo item (Carvão).

#### 2. Novos Inimigos por Bioma

Para tornar a exploração mais perigosa e recompensadora:

* **Deserto:** Adicionar **Escorpiões** que se movem rápido mas têm pouca vida.
* **Caverna:** Adicionar **Morcegos** que ignoram obstáculos.
* **Pedreira:** Um **Golem de Pedra** lento, mas que precisa de uma Picareta para ser derrotado.

#### 3. Coleta de Ouro e Loja

Introduzir um recurso raro para permitir melhorias permanentes:

* **Ouro:** Encontrado raramente na Caverna ou Pedreira.
* **Vendedor:** Um NPC que aparece na Floresta a cada 5 dias para trocar Ouro por "Botas de Velocidade" ou "Poções de Vida".

#### 4. Clima Dinâmico

* **Chuva:** Na Floresta, pode começar a chover, diminuindo a velocidade do jogador.
* **Tempestade de Areia:** No Deserto, a visibilidade diminui e o jogador é empurrado levemente para os lados.

---

### 🛠️ Exemplo de como seria a evolução do Crafting no README:

| Item | Receita | Efeito Especial |
| --- | --- | --- |
| **Tocha** | 2 Mad + 1 Carvão | Revela o mapa na Caverna |
| **Botas** | 4 Carnes (Couro) | Aumenta a velocidade em +2 |
| **Picareta de Ouro** | 10 Ped + 5 Ouro | Coleta 5 pedras por vez |

