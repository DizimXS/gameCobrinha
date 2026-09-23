---
title: Design — Jogo Snake em arquivo HTML único
version: 1.0
date_created: 2026-09-23
owner: William
tags: [design, canvas, game-loop]
---

# Design

Design técnico do jogo Snake especificado em `spec.md`. Documento de nível arquitetural e de
algoritmos; não contém o código final.

## 1. Decisões Arquiteturais

- **ADR-001 — Canvas 2D em vez de DOM grid.** Renderizar `cols × rows` como valores posicionais
  em um único `<canvas>` evita centenas de elementos no DOM e mantém a renderização estável a 60 fps.
  Alternativa rejeitada: `<div>` por célula (custo de layout alto, difícil de escalar).
- **ADR-002 — Simulação por tick discreto com acumulador.** O `requestAnimationFrame` fornece o
  delta de tempo; um acumulador dispara `update()` quando `acc >= stepMs`. Desacopla a lógica da
  taxa de quadros (FR-014) e permite desenhar interpolação e efeitos sem afetar a jogabilidade.
  Alternativa rejeitada: `setInterval` (proibido por CON-004).
- **ADR-003 — Fila de entrada curta.** Cada direção entra em uma fila (máx. 2). No início de cada tick,
  retira-se a primeira direção **válida** (não oposta à última aplicada). Isso evita inversão (FR-005)
  e perda de comandos em ticks rápidos (GUD-003).
- **ADR-004 — Estado imutável por tick, cauda removida antes da colisão.** No tick: calcula-se a nova
  cabeça, remove-se a cauda quando não há crescimento e só então se testa a colisão. Isso permite
  mover para a célula que a cauda acabou de liberar (caso de borda 5).
- **ADR-005 — Config centralizada.** Um único objeto `CONFIG` congela grade, tempos, pontos e chave de
  armazenamento (CON-003).
- **ADR-006 — Persistência tolerante.** Acesso a `localStorage` encapsulado em funções `loadHighScore()`
  e `saveHighScore()` com `try/catch` (FR-010, DAT-001).
- **ADR-007 — CSS embutido com variáveis.** Tema neon definido via custom properties CSS
  (`--neon`, `--bg`, ...) para manter consistência visual (NFR-003) e permitir `prefers-reduced-motion` (NFR-009).

## 2. Estrutura do Artefato Único

```text
index.html
├── <head>  (meta viewport, title)
├── <style> (variáveis de tema, layout, HUD, D-Pad, overlay)
├── <body>
│   ├── header (título)
│   ├── div.hud  (score, high score)
│   ├── div.stage
│   │   ├── canvas
│   │   └── div.overlay  (ready | paused | game over)
│   └── div.dpad  (4 botões direcionais)
└── <script>
    ├── CONFIG
    ├── estado + DOM refs
    ├── persistência
    ├── regras puras (collision, pushDirection, spawnFood)
    ├── update(dt) / tick()
    ├── render()
    ├── loop(ts)
    ├── input handlers (keyboard, dpad, blur)
    └── bootstrap
```

## 3. Modelo de Dados

```text
CONFIG = { cols, rows, baseStepMs, minStepMs, speedUpPerFood, pointsPerFood, storageKey }

state = {
  snake: Cell[],  cell = {x:int, y:int},  cell[0] = cabeça
  dir: Cell,          // direção aplicada no último tick
  queue: Cell[],      // entradas pendentes (máx. 2)
  food: Cell,
  score: int,
  highScore: int,
  status: "ready" | "running" | "paused" | "over",
  stepMs: int,
  acc: float
}
```

## 4. Algoritmos

### 4.1 Colisão / inversão

```text
isOpposite(a, b)  = (a.x + b.x === 0 && a.y + b.y === 0)

pushDirection(next):
  if status == "ready" -> inicia partida
  lastRef = (queue não vazia) ? queue[queue.length-1] : dir
  if isOpposite(next, lastRef) or next == lastRef -> ignora
  if queue.length < 2 -> queue.push(next)
```

### 4.2 Reposicionamento da comida

```text
spawnFood():
  builds occupancy set from snake (string "x,y")
  loop até maxAttempts (ex.: 500):
    c = { x: rand(0..cols-1), y: rand(0..rows-1) }
    if c not in occupancy -> return c
  varredura determinística como fallback (garante CA-004)
  se nenhuma célula livre -> vitória (fim de jogo sem travar)
```

### 4.3 Tick

```text
tick():
  1. aplica a próxima direção válida da fila (se houver)
  2. head = { x: snake[0].x + dir.x, y: snake[0].y + dir.y }
  3. if head fora da grade -> gameOver(); return
  4. growing = (head == food)
  5. if not growing -> snake.pop()          // ADR-004
  6. if head ∈ snake -> gameOver(); return
  7. snake.unshift(head)
  8. if growing:
       score += pointsPerFood
       stepMs = max(minStepMs, baseStepMs - (score/pointsPerFood) * speedUpPerFood)
       food = spawnFood()   // ou vitória se não houver célula livre
       atualizaHighScore()
```

### 4.4 Loop

```text
loop(ts):
  dt = ts - lastTs ; lastTs = ts
  if status == "running":
      acc += dt
      while acc >= stepMs: acc -= stepMs; tick()
  render()
  requestAnimationFrame(loop)
```

Observação: o `while` limita o número de ticks por quadro para evitar "espiral da morte"
quando a aba fica em segundo plano. Um teto (ex.: 5 ticks/quadro) é aplicado.

### 4.5 Renderização

- `cellSize = canvas.width / cols` (canvas em pixels de dispositivo).
- Fundo com grade discreta; cabeça com brilho neon e olhos; corpo com gradiente;
  comida pulsante (desativada sob `prefers-reduced-motion`).
- Renderização usa apenas primitivas 2D (`fillRect`, `roundRect` quando disponível, `arc`).

## 5. Interação e Acessibilidade

| Elemento | Comportamento |
|---|---|
| `keydown` | mapeia setas e WASD; `preventDefault` para evitar rolagem (NFR-007) |
| `keydown` Espaço/P | alterna pausa (FR-013) |
| `keydown` Enter | inicia/reinicia |
| D-Pad | `pointerdown` + `click` como fallback; `touch-action: none` para evitar zoom/rolagem |
| `blur` da janela | pausa automática (FR-013) |
| Overlay | contém o botão de reinício e textos de estado; `role="status"` |
| Foco | D-Pad e botão navegáveis por teclado (`:focus-visible`) |

## 6. Riscos e Mitigações

| Risco | Mitigação |
|---|---|
| Inversão por duplo toque rápido | fila única com validação de opostos (ADR-003) |
| Comida sobre o corpo | `spawnFood` com conjunto de ocupação + fallback (4.2) |
| Falsa colisão na cauda | ordem ADR-004 |
| `localStorage` bloqueado | `try/catch` (ADR-006) |
| Tela de alta densidade borrada | escala por `devicePixelRatio` (NFR-006) |
| Ticks acumulados ao voltar de segundo plano | teto de ticks por quadro + pausa no `blur` |

## 7. Rastreabilidade Design → Requisitos

| Decisão | Requisitos atendidos |
|---|---|
| ADR-001 | FR-002, NFR-006 |
| ADR-002 | FR-014, CON-004 |
| ADR-003 | FR-005, FR-003, FR-004 |
| ADR-004 | FR-008, CA-006 |
| ADR-005 | CON-003 |
| ADR-006 | FR-010 |
| ADR-007 | NFR-003, NFR-002, NFR-009 |

## 8. Leitura Complementar

- `.specs/features/snake-game/spec.md`
- `.specs/features/snake-game/tasks.md`
