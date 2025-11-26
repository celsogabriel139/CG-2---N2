# 📘 Documentação do Projeto: A Corrida do Ligeirinho

## 1. Visão Geral
**A Corrida do Ligeirinho** é uma aplicação web interativa desenvolvida em HTML, CSS e JavaScript puro. O projeto simula um ambiente 2D com uma câmera virtual navegável e implementa um sistema de iluminação dinâmica baseado no **Modelo de Reflexão de Phong**.

O objetivo do jogo é localizar e clicar sobre o personagem "Ligeirinho" (um rato que se move aleatoriamente) dentro de um mapa virtual maior que a tela visível.

## 2. Funcionalidades Principais

### 🎮 Mecânicas de Jogo
* **Mundo Virtual:** Um espaço de 1600x1200 pixels, onde a câmera visualiza apenas uma porção de 800x600.
* **IA do Personagem:** O Ligeirinho se move para coordenadas aleatórias, com preferência por permanecer próximo à área visível da câmera.
* **Sistema de Captura:** O jogador deve clicar no personagem para "capturá-lo", incrementando um contador de pontuação.

### 🖥️ Computação Gráfica
* **Câmera Virtual:** Suporte a *panning* (movimento XY) e *zoom* (escala).
* **Iluminação Phong:** Renderização de luz em tempo real aplicada a objetos 2D, calculando componentes Ambiente, Difusa e Especular.
* **Transformação de Coordenadas:** Conversão bidirecional entre coordenadas de mundo (World Space) e coordenadas de tela (Screen Space).

---

## 3. Guia do Usuário

### Controles de Teclado
| Tecla | Ação |
| :--- | :--- |
| **W / Seta Cima** | Move a câmera para cima |
| **S / Seta Baixo** | Move a câmera para baixo |
| **A / Seta Esquerda** | Move a câmera para a esquerda |
| **D / Seta Direita** | Move a câmera para a direita |
| **+ / =** | Aumentar Zoom (Zoom In) |
| **- / _** | Diminuir Zoom (Zoom Out) |

### Interação com Mouse
* **Clique no Personagem:** Captura o Ligeirinho (vitória).
* **Clique no Cenário:** Teletransporta o centro da câmera para o local clicado.

### Painel de Controle (UI)
Localizado abaixo do canvas, permite ajustar a iluminação em tempo real:
* **Intensidade:** Brilho geral da luz.
* **Direção X/Y:** Altera o vetor de direção da luz simulada.
* **Cor da Luz:** Seletor de cor (RGB) para a fonte de luz.

---

## 4. Arquitetura Técnica

O código é contido em um único arquivo HTML para facilidade de portabilidade. Abaixo estão os componentes principais do script.

### 4.1 Variáveis Globais
* `camera`: Objeto que armazena `x`, `y` (posição) e `zoom` (escala).
* `light`: Configurações do modelo Phong (direção, cor, coeficientes de reflexão).
* `speedy`: Estado do personagem (posição, velocidade, alvo, status de captura).
* `obstacles`: Array de objetos estáticos renderizados no cenário.

### 4.2 Funções Core (Engine)

#### `gameLoop()`
O laço principal de animação que roda continuamente via `requestAnimationFrame`.
1.  `updateCamera()`: Processa input do teclado.
2.  `updateSpeedy()`: Calcula a nova posição do personagem.
3.  Renderização: Limpa a tela e desenha fundo, obstáculos e personagem.

#### `worldToScreen(worldX, worldY)` & `screenToWorld(screenX, screenY)`
Responsáveis pela matemática da câmera.
* **Fórmula:** `Tela = (Mundo - Câmera) * Zoom`

#### `applyPhongLighting(baseColor, normalZ)`
A função mais complexa do projeto, simulando 3D em um canvas 2D.
1.  **Ambiente:** Cor base multiplicada por uma constante mínima de luz.
2.  **Difusa (Lambert):** Baseada no ângulo entre a luz e a superfície.
3.  **Especular (Blinn-Phong):** Calcula o brilho intenso (highlight) baseado na direção da visão.

### 4.3 Lógica do Personagem (`updateSpeedy`)
O Ligeirinho possui uma lógica de movimento autônoma:
* Calcula a distância até o alvo (`targetX`, `targetY`).
* Ao chegar no alvo, escolhe um novo ponto aleatório.
* **Heurística:** Existe 70% de chance do novo alvo ser gerado próximo à câmera atual, garantindo que o personagem não fique perdido longe do jogador por muito tempo.

---

## 5. Estilização (CSS)
O design utiliza uma abordagem moderna:
* **Background:** Gradiente linear azul (`linear-gradient`).
* **Container:** Efeito de vidro (`rgba(0, 0, 0, 0.3)`) com bordas arredondadas.
* **Canvas:** Cursor personalizado (`crosshair`) para indicar precisão.

---

## 6. Como Executar
1.  Salve o código fornecido como um arquivo `.html` (ex: `Ligeirinho.html`).
2.  Abra o arquivo em qualquer navegador moderno (Chrome, Firefox, Edge, Safari).
3.  Não é necessário servidor web; o jogo roda localmente.

## 7. Possíveis Melhorias (Sugestões)
Para versões futuras, o código está preparado para receber:
* **Texturas:** Substituir cores sólidas por sprites.
* **Colisão:** Implementar lógica para que o Ligeirinho desvie dos obstáculos (`obstacles`).
* **Sistema de Som:** Adicionar efeitos sonoros ao capturar.
