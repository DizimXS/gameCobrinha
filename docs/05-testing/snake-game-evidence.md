# Evidências de teste — Jogo Snake (arquivo único)

- **Recurso**: `.specs/features/snake-game/`
- **Artefato sob teste**: `index.html` (raiz do projeto)
- **Data**: 2026-09-23
- **Nível de evidência**: `NAVEGADOR` (Chromium via VS Code Simple Browser / navegador integrado)
- **Ambiente**: Linux, `file:///home/lsertorio/Documentos/CODE%20IA/index.html`
- **Método**: inspeção estática (`grep`) + automação no navegador (teclado, D-Pad, leitura de pixels do canvas)
  + capturas de tela.

> Nota metodológica: a leitura de pixels foi feita pelo *harness* de teste, que gera um aviso
> `Canvas2D: Multiple readback operations using getImageData...`. Esse aviso é **causado pelo teste**,
> não pelo jogo, e não aparece na execução normal.

## 1. Resultado por critério de aceitação

| CA | Descrição | Evidência | Resultado |
|---|---|---|---|
| CA-001 | Arquivo único, sem dependências externas | `grep -nE 'src=\|href=\|https?://\|@import\|url\(http' index.html` → nenhuma ocorrência; 1 `<style>`, 1 `<script>` | **PASS** |
| CA-002 | Inversão de 180° bloqueada | `ArrowRight` seguido de `ArrowLeft`; a cobra **não** inverteu e colidiu com a parede direita (título `Fim de jogo`) | **PASS** |
| CA-003 | Crescer e pontuar ao comer | Bot dirigido por BFS até a comida: `Pontuação` passou de `0` para `10` | **PASS** |
| CA-004 | Comida nunca sobre o corpo | 25 amostras da célula da comida durante o jogo: 0 sobreposições com células verdes (corpo) | **PASS** |
| CA-005 | Colisão com a parede | `ArrowRight` sem desvio → `#overlayTitle` = `Fim de jogo`, botão `Jogar novamente` | **PASS** |
| CA-006 | Colisão com o próprio corpo | Após crescer (pontuação 10) e executar giro fechado 2×2, o jogo terminou com a última célula da cabeça em **(6,6)** (interior da grade, longe das bordas) → só pode ser autocolisão | **PASS** |
| CA-007 | Melhor pontuação persistida | `localStorage['snake-neon:highscore']` = `"10"` → após recarregar, HUD `Recorde` = `10`; em execução posterior, `Recorde` = `100` após reload | **PASS** |
| CA-008 | D-Pad muda a direção | Cabeça passou de `{x:11,y:11}` para `{x:12,y:9}` após `pointerdown` no botão ▲ | **PASS** |
| CA-009 | Primera direção inicia a partida | Overlay visível antes (`is-hidden=false`); após `ArrowRight`, `is-hidden=true` | **PASS** |
| CA-010 | Pausa/retomada | `Space` → título `Pausado`; cabeça congelada em `{x:12,y:9}` durante 600 ms; `Space` → overlay oculto | **PASS** |
| CA-011 | Sem erros no console | Log de `pageerror`/`console.error`/`console.warn` vazio em todas as execuções (exceto o aviso do *harness* citado acima) | **PASS** |
| CA-012 | Velocidade aumenta com a pontuação | Intervalo médio entre ticks: **133 ms** com pontuação `< 40` → **105 ms** com pontuação `>= 80` (184 movimentos amostrados, partida até 100 pontos) | **PASS** |

## 2. Requisitos não funcionais

| NFR | Evidência | Resultado |
|---|---|---|
| NFR-001 | Nenhuma referência externa (CA-001) | **PASS** |
| NFR-002 | Layout centralizado — captura `snake-game-over.png` | **PASS** |
| NFR-003 | Tema escuro/neon — capturas acima | **PASS** |
| NFR-004 | Canvas de 517,5 × 517,5 px, centralizado; pontos de quebra para telas baixas (CSS) | **PASS (parcial)** |
| NFR-005 | Código em 13 seções comentadas em pt-BR, identificadores em inglês | **PASS** |
| NFR-006 | `canvas.width = cssSize × min(devicePixelRatio, 2)` via `ResizeObserver` | **PASS** |
| NFR-007 | `preventDefault` em setas/WASD/Espaço; `touch-action: none` no D-Pad | **PASS** |
| NFR-008 | Nenhum erro no carregamento (CA-011) | **PASS** |
| NFR-009 | Bloco `@media (prefers-reduced-motion: reduce)` + pulso da comida desativado | **PASS** |

## 3. Comportamentos adicionais observados

- `Enter` na tela de fim de jogo reinicia e já inicia a movimentação (overlay oculto, região viva = `Partida iniciada.`).
- `R` reinicia a partida.
- Perder o foco da janela dispara pausa automática.
- A casa da cauda é liberada antes da checagem de colisão (ADR-004): a cobra sobrevive ao girar em 2×2
  enquanto o comprimento é 4, e só colide a partir do comprimento 5 — comportamento verificado no teste de CA-006.

## 4. Limitações e pendências

- **T-016 / PENDENTE**: D-Pad validado por emulação de `pointerdown` em navegador de desktop.
  **Não** houve validação em dispositivo móvel físico (toque real, gestos do sistema).
  A diretriz do projeto é explícita: `HOST`/emulação não equivale a validação em hardware real.
- Responsividade verificada por cálculo de layout (bounding boxes) e quebra de CSS; não foram testadas
  todas as resoluções de 320 px a 1920 px uma a uma.
- Não há teste automatizado versionado no repositório (restrição CON-001/CON-002: artefato único,
  sem dependências). A automação acima foi executada pelo *harness* de navegador, não versionada.

## 5. Capturas

| Arquivo | Estado |
|---|---|
| `docs/05-testing/snake-game-over.png` | Tela de fim de jogo com botão de reinício |
| `docs/05-testing/snake-game-restarted.png` | Partida reiniciada após `Enter` |
