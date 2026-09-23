---
title: Jogo Snake em arquivo HTML único
version: 1.0
date_created: 2026-09-23
owner: William
tags: [game, canvas, frontend, single-file]
---

# Introdução

Especificação do jogo da cobrinha (Snake) entregue como **um único arquivo `index.html`**
contendo HTML, CSS (`<style>`) e JavaScript (`<script>`) embutidos. O jogo deve rodar
diretamente no navegador, sem servidor, sem build e sem dependências externas.

Fonte original do produto: `docs/memorial.txt` (não editar).

## 1. Propósito e Escopo

**Propósito:** definir requisitos verificáveis para um jogo Snake completo, jogável em
desktop e dispositivos móveis, com placar persistente e apresentação retro-arcade.

**Escopo (dentro):**
- Arquivo único `index.html` autocontido.
- Renderização em HTML5 Canvas 2D.
- Loop de jogo com `requestAnimationFrame` + controle de tempo por `millis` equivalente.
- Controles por teclado (setas e WASD) e D-Pad virtual na tela.
- Pontuação atual, melhor pontuação persistida em `localStorage`, tela de game over com reinício.

**Escopo (fora):**
- Backend, multiplayer, placar online, áudio obrigatório, níveis de dificuldade selecionáveis,
  itens especiais. Recursos bônus só entram se declarados na seção "Recursos Bônus (Opcionais)"
  do memorial — que, na versão atual, não define nenhum.

**Público-alvo do documento:** agentes e desenvolvedores que implementam ou revisam o jogo.

**Premissas:**
- Navegador moderno com suporte a Canvas 2D, `requestAnimationFrame` e `localStorage`.
- A grade do tabuleiro é discreta (movimento em células), não contínua.

## 2. Definições

| Termo | Definição |
|---|---|
| Célula | Menor unidade da grade lógica do tabuleiro. |
| Grade | Matriz `COLS × ROWS` de células que define o tabuleiro. |
| Tick | Um passo lógico de simulação, no qual a cobra avança uma célula. |
| Cobra | Lista ordenada de células; índice 0 é a cabeça. |
| Direção | Vetor unitário de movimento em uma das quatro direções cardinais. |
| D-Pad | Conjunto de botões direcionais virtuais exibidos na tela para toque. |
| High Score | Maior pontuação já obtida, persistida em `localStorage`. |
| Game loop | Ciclo contínuo de atualização + renderização. |
| Fila de entrada | Buffer de no máximo 2 direções pendentes aplicadas uma por tick. |

## 3. Requisitos, Restrições e Diretrizes

### Requisitos Funcionais (FR)

- **FR-001**: O produto deve ser entregue como um único arquivo `index.html` com CSS em `<style>` e JS em `<script>`, sem arquivos auxiliares.
- **FR-002**: O jogo deve usar HTML5 Canvas para renderizar o tabuleiro, a cobra e a comida.
- **FR-003**: O jogador deve controlar a cobra com as setas do teclado (`ArrowUp`/`ArrowDown`/`ArrowLeft`/`ArrowRight`) e com as teclas `W`/`A`/`S`/`D` (maiúsculas e minúsculas).
- **FR-004**: O jogo deve exibir um D-Pad virtual com quatro botões direcionais, operável por toque e por mouse.
- **FR-005**: A cobra não pode inverter instantaneamente a direção (ex.: mover-se para a esquerda quando já está indo para a direita).
- **FR-006**: A comida deve surgir em uma célula aleatória livre, nunca sobre o corpo da cobra nem sobre a cabeça.
- **FR-007**: Ao consumir a comida, a cobra deve crescer em uma célula e a pontuação deve aumentar em 10 pontos.
- **FR-008**: O jogo deve encerrar quando a cabeça colidir com a parede do tabuleiro ou com qualquer célula do próprio corpo.
- **FR-009**: A interface deve exibir a Pontuação Atual e a Melhor Pontuação.
- **FR-010**: A Melhor Pontuação deve ser lida e gravada em `localStorage` de forma tolerante a falhas (modo privado/`localStorage` indisponível não pode quebrar o jogo).
- **FR-011**: Deve existir uma tela de Game Over com a pontuação final e um botão de reinício que restaura o estado inicial.
- **FR-012**: A partida deve começar em estado "pronto"; o primeiro tick ocorre após a primeira entrada direcional válida do jogador.
- **FR-013**: O jogo deve ser pausável e retomável (tecla `Espaço` ou `P`, e ao perder o foco da janela).
- **FR-014**: O loop de jogo deve usar `requestAnimationFrame` com acumulador de tempo para desacoplar a simulação (tick) da taxa de quadros de renderização.
- **FR-015**: A velocidade de movimento deve aumentar progressivamente conforme a pontuação, respeitando um piso mínimo de intervalo por tick.

### Requisitos Não Funcionais (NFR)

- **NFR-001**: Zero dependências externas: sem bibliotecas, sem frameworks, sem `CDN`, sem fontes ou imagens remotas.
- **NFR-002**: Layout moderno, limpo, centralizado horizontal e verticalmente na viewport.
- **NFR-003**: Tema escuro estilo retro-arcade: fundo escuro com elementos em tons neon.
- **NFR-004**: Layout responsivo: o canvas e os controles devem se adaptar a telas de 320 px a 1920 px de largura, sem rolagem horizontal indesejada.
- **NFR-005**: Código comentado (pt-BR), organizado em seções e com nomes de identificadores em inglês.
- **NFR-006**: O canvas deve ser renderizado com dimensões de desenho ajustadas ao `devicePixelRatio` para nitidez em telas de alta densidade.
- **NFR-007**: A entrada do usuário deve ser tratada sem bloquear o loop; `preventDefault` deve impedir a rolagem da página provocada pelas setas.
- **NFR-008**: A página deve carregar e iniciar sem erros no console do navegador.
- **NFR-009**: Deve haver suporte a `prefers-reduced-motion` para reduzir animações decorativas.

### Restrições (CON)

- **CON-001**: Apenas `index.html`; é proibido criar `.css` ou `.js` separados para o jogo.
- **CON-002**: Apenas Vanilla CSS e Vanilla JavaScript (ES2015+). Sem TypeScript, sem transpilação.
- **CON-003**: A grade e as constantes de jogo devem ser centralizadas em um único objeto de configuração no topo do script.
- **CON-004**: O estado do jogo não pode depender de temporização por `setInterval` para a simulação.

### Diretrizes (GUD)

- **GUD-001**: Preferir células lógicas em vez de pixels na simulação; pixels apenas na renderização.
- **GUD-002**: Manter uma única fonte de verdade para cada dado (pontuação, direção, high score).
- **GUD-003**: Manter o histórico de entradas em fila curta para evitar direções perdidas em ticks rápidos.

### Padrões (PAT)

- **PAT-001**: Separação em funções `update(dt)` (simulação) e `render()` (desenho), acionadas pelo game loop.
- **PAT-002**: Renderização por célula derivada de `cellSize = canvasSize / COLS`.

## 4. Interfaces e Contratos de Dados

### 4.1 Configuração (constante `CONFIG`)

| Campo | Tipo | Descrição |
|---|---|---|
| `cols` | inteiro | Número de colunas da grade. |
| `rows` | inteiro | Número de linhas da grade. |
| `baseStepMs` | inteiro | Intervalo inicial entre ticks (ms). |
| `minStepMs` | inteiro | Intervalo mínimo entre ticks (ms). |
| `speedUpPerFood` | inteiro | Redução do intervalo por comida consumida (ms). |
| `pointsPerFood` | inteiro | Pontos por comida. |
| `storageKey` | string | Chave usada no `localStorage`. |

### 4.2 Estado do jogo (objeto `state`)

| Campo | Tipo | Descrição |
|---|---|---|
| `snake` | `Array<{x,y}>` | Corpo da cobra; índice 0 = cabeça. |
| `dir` | `{x,y}` | Direção atual aplicada no tick. |
| `queue` | `Array<{x,y}>` | Fila de direções pendentes (máx. 2). |
| `food` | `{x,y}` | Posição da comida. |
| `score` | inteiro | Pontuação atual. |
| `highScore` | inteiro | Melhor pontuação persistida. |
| `status` | `"ready" \| "running" \| "paused" \| "over"` | Estado da partida. |
| `stepMs` | inteiro | Intervalo atual entre ticks. |
| `acc` | número | Acumulador de tempo em ms. |

### 4.3 Persistência (`localStorage`)

| Chave | Valor | Observação |
|---|---|---|
| `CONFIG.storageKey` | inteiro (string) | Valor inválido ou ausente é tratado como `0`, sem lançar exceção. |

### 4.4 Eventos de entrada

| Origem | Evento | Ação |
|---|---|---|
| Teclado | `keydown` setas / WASD | Enfileira direção |
| Teclado | `keydown` `Espaço`/`P` | Alterna pausa |
| Teclado | `keydown` `Enter` | Inicia/reinicia |
| D-Pad | `pointerdown`/`click` | Enfileira direção |
| Janela | `blur` | Pausa |

## 5. Critérios de Aceitação

- **CA-001**: Dado o arquivo `index.html` aberto localmente, quando `grep` é aplicado, então existe exatamente um arquivo com `<style>` e `<script>` embutidos e nenhuma referência `http(s)://` externa de script/estilo/fonte.
- **CA-002**: Dado o jogo carregado, quando o jogador pressiona `ArrowRight` e depois `ArrowLeft` no mesmo tick, então a cobra continua indo para a direita (inversão bloqueada).
- **CA-003**: Dado o jogo em execução, quando a cobra consome a comida, então o comprimento da cobra aumenta em 1 e a pontuação aumenta em 10.
- **CA-004**: Dado qualquer estado de jogo, quando a comida é reposicionada, então sua célula pertence à grade e não pertence a nenhuma célula do corpo da cobra.
- **CA-005**: Dado que a cabeça atinge uma célula fora da grade, quando o tick é processado, então o status passa a `"over"` e a tela de Game Over é exibida.
- **CA-006**: Dado que a cabeça atinge uma célula ocupada pelo corpo, quando o tick é processado, então o status passa a `"over"`.
- **CA-007**: Dado que a pontuação supera a Melhor Pontuação anterior, quando a partida termina, então o novo valor é gravado em `localStorage` e exibido na reinicialização.
- **CA-008**: Dado o D-Pad visível, quando o jogador toca em um botão direcional, então a cobra muda de direção conforme o botão (respeitando CA-002).
- **CA-009**: Dado o status `"ready"`, quando o jogador pressiona uma direção válida, então a cobra começa a se mover.
- **CA-010**: Dado o status `"running"`, quando o jogador pressiona `Espaço`, então o status passa a `"paused"` e a cobra para; pressionar novamente retoma.
- **CA-011**: Dado o jogo aberto sem erros, quando o console do navegador é inspecionado após 5 s de execução, então não há erros nem avisos de exceção.
- **CA-012**: Dado o lucro de pontos, quando a pontuação aumenta, então o intervalo entre ticks diminui até o mínimo definido em `CONFIG.minStepMs`.

## 6. Estratégia de Teste

- **Níveis**: sistema (navegador real) e inspeção estática do artefato único.
- **Frameworks**: nenhum framework de teste automatizado (restrição CON-001/CON-002). A verificação
  usa o navegador integrado + *DevTools*.
- **Verificação estática**: inspeção do `index.html` para confirmar ausência de dependências externas
  (CA-001) e presença das seções comentadas.
- **Verificação manual/funcional**: execução do jogo no navegador, exercitando cada CA, com captura de tela.
- **Sem hardware**: o alvo é software de navegador; níveis de evidência `HOST`/`SIMULADOR` não se aplicam.
  A evidência é `NAVEGADOR`.
- **Critério de aceite global**: CAs 001–012 verificados, com registro do que não pôde ser verificado.

## 7. Justificativa e Contexto

O memorial exige um único arquivo sem dependências, o que torna o Canvas 2D a escolha mais direta:
não requer bibliotecas gráficas e permite desenhar a grade com primitivas 2D. O `requestAnimationFrame`
é exigido explicitamente e evita o acúmulo de atrasos do `setInterval`. A fila de entrada única
(GUD-003) resolve o caso clássico de perda de direção quando duas teclas são pressionadas dentro
do mesmo tick, sem permitir inversão (FR-005).

## 8. Dependências e Integrações Externas

### Sistemas Externos

- **EXT-001**: Navegador web moderno — executa o arquivo localmente via `file://`. Requer Canvas 2D e `requestAnimationFrame`.

### Serviços de Terceiros

- Nenhum. Ver NFR-001.

### Dependências de Infraestrutura

- **INF-001**: Nenhum servidor/build. O arquivo deve abrir com duplo clique.

### Dependências de Dados

- **DAT-001**: `localStorage` do navegador — chave `CONFIG.storageKey`, valor inteiro. Pode estar indisponível; tratar como falha silenciosa.

### Dependências de Plataforma

- **PLT-001**: JavaScript ES2015+ (`const`/`let`, arrow functions, template literals) sem transpilação.

### Dependências de Conformidade

- Nenhuma regulação aplicável. Não há coleta de dados pessoais.

## 9. Exemplos e Casos de Borda

```js
// Caso de borda 1: inversão bloqueada no mesmo tick
// dir = {x:1,y:0} (direita); entrada ArrowLeft => ignorada
// Resultado esperado: dir permanece {x:1,y:0}  (CA-002)

// Caso de borda 2: comida nunca no corpo
// Ao reposicionar, sorteia célula livre; se a cobra ocupa toda a grade,
// o jogo é considerado vitória e encerra sem travar.  (CA-004)

// Caso de borda 3: localStorage indisponível
// getItem/lança exceção => highScore permanece 0 e o jogo continua.  (FR-010)

// Caso de borda 4: duas teclas no mesmo tick
// runLoop que "norte" e depois "leste" a partir de "leste" => fila [leste]; vira norte no próximo tick.

// Caso de borda 5: colisão com a própria cauda em movimento
// A cauda é removida antes da checagem; mover para a célula que a cauda
// acabou de deixar NÃO deve matar a cobra.
```

## 10. Critérios de Validação

1. `index.html` abre em `file://` sem erro de console (CA-011).
2. Inspeção estática confirma arquivo único e zero dependências (CA-001).
3. Exercício manual de cada CA-002 … CA-010 no navegador, com captura de tela.
4. Registro em `docs/05-testing/` do resultado de cada CA e das limitações (ex.: teste touch real em dispositivo físico).

## 11. Especificações Relacionadas / Leitura Complementar

- `docs/memorial.txt` — fonte original do produto (não editar).
- `.specs/features/snake-game/design.md` — decisões arquiteturais e de renderização.
- `.specs/features/snake-game/tasks.md` — decomposição de tarefas.
