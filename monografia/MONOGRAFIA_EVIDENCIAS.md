# MONOGRAFIA_EVIDENCIAS.md

> Matriz de evidências. Formato: `Afirmação | Evidência | Arquivo/Fonte | Seção`.
> Toda afirmação técnica da monografia deve constar aqui com sustentação correspondente.

## 1. Evidências de código (produto)

| ID | Afirmação | Evidência | Arquivo/Fonte | Seção da monografia |
|---|---|---|---|---|
| E-01 | O produto é um único arquivo HTML, com CSS e JavaScript embutidos | `grep` por `src=`/`href=`/`http` retorna vazio; exatamente 1 `<style>` e 1 `<script>`; 1063 linhas, 32 305 bytes | `index.html` | 6.2, 7.2 |
| E-02 | Não há dependências externas | Ausência de referências de rede; uso exclusivo de Canvas 2D, CSS e JS nativos | `index.html` | 6.2, 8.4 |
| E-03 | A simulação é desacoplada da renderização por acumulador de tempo | Chamada `requestAnimationFrame(loop)`; acumulador `state.acc` comparado a `state.stepMs` com teto `maxTicksPerFrame` | `index.html` (`loop`, `tick`) | 7.4 |
| E-04 | A inversão de 180° é bloqueada por fila de entrada | Função `requestDirection`: descarta `sameCell` e `isOpposite`; fila limitada a `maxQueue = 2` | `index.html` (`requestDirection`) | 7.6 |
| E-05 | A cauda é liberada antes da verificação de colisão | `state.snake.pop()` ocorre antes da varredura de colisão | `index.html` (`tick`) | 7.5 |
| E-06 | A comida nunca é sorteada sobre o corpo | Conjunto de ocupação + varredura determinística de fallback em `spawnFood` | `index.html` (`spawnFood`) | 7.7 |
| E-07 | A persistência do recorde é tolerante a falha | `try/catch` em `loadHighScore`/`saveHighScore`, com retorno 0 | `index.html` | 7.8 |
| E-08 | A renderização respeita a densidade de tela | `canvas.width = cssSize × min(devicePixelRatio, 2)`; `ctx.setTransform(dpr, ...)` | `index.html` (`resizeCanvas`) | 7.9 |
| E-09 | Há suporte a movimento reduzido | Bloco `@media (prefers-reduced-motion: reduce)`; pulso da comida condicionado | `index.html` | 7.10 |
| E-10 | A velocidade aumenta com a pontuação, com piso | `state.stepMs = max(minStepMs, baseStepMs − eaten × speedUpPerFood)`; constantes 68 / 140 / 4 | `index.html` (`tick`), `spec.md` FR-015 | 7.11, 8.5 |

## 2. Evidências experimentais (verificação)

| ID | Afirmação | Evidência | Arquivo/Fonte | Seção |
|---|---|---|---|---|
| E-11 | CA-001 verificado: artefato único sem dependências | Inspeção estática por `grep` | `docs/05-testing/snake-game-evidence.md` | 8.4 |
| E-12 | CA-002 verificado: inversão bloqueada | `ArrowRight` + `ArrowLeft` → a cobra manteve o sentido e colidiu com a parede direita | idem | 8.4 |
| E-13 | CA-003 verificado: crescimento e pontuação | Pontuação 0 → 10 após consumo de comida, com agente de teste por busca em largura | idem | 8.4 |
| E-14 | CA-004 verificado: comida nunca sobre o corpo | 25 amostras da célula da comida, 0 sobreposições com células de corpo | idem | 8.4 |
| E-15 | CA-005 verificado: colisão com a parede | Colisão → título `Fim de jogo` e botão de reinício | idem | 8.4 |
| E-16 | CA-006 verificado: colisão com o próprio corpo | Após crescimento, giro fechado 2×2 terminou a partida com a cabeça em (6,6), célula interior | idem | 8.4 |
| E-17 | CA-007 verificado: recorde persistido | `localStorage['snake-neon:highscore']` = `"10"` e depois `"100"`; HUD manteve após recarregar | idem | 8.4 |
| E-18 | CA-008 verificado: D-Pad altera a direção | `pointerdown` no botão ▲ moveu a cabeça de (11,11) para (12,9) | idem | 8.4 |
| E-19 | CA-009 verificado: primeira direção inicia a partida | Overlay visível antes; `is-hidden` após `ArrowRight` | idem | 8.4 |
| E-20 | CA-010 verificado: pausa congela o estado | Cabeça imóvel em (12,9) por 600 ms com o título `Pausado`; retomada ocultou o overlay | idem | 8.4 |
| E-21 | CA-011 verificado: sem erros no console | Log de `pageerror`/`console.error`/`console.warn` vazio | idem | 8.4 |
| E-22 | CA-012 verificado: aumento de velocidade | Intervalo médio de tick 133 ms (pontuação < 40) → 105 ms (pontuação ≥ 80), sobre 184 movimentos amostrados | idem | 8.5 |

## 3. Evidências de especificação e processo

| ID | Afirmação | Evidência | Arquivo/Fonte | Seção |
|---|---|---|---|---|
| E-23 | O alvo possui 15 requisitos funcionais e 9 não funcionais | Documento de especificação com IDs `FR-001`…`FR-015` e `NFR-001`…`NFR-009` | `spec.md` | 6.3 |
| E-24 | O alvo possui 12 critérios de aceitação rastreáveis | IDs `CA-001`…`CA-012` | `spec.md` | 6.4 |
| E-25 | Há 7 decisões arquiteturais registradas | IDs `ADR-001`…`ADR-007` | `design.md` | 6.5, 7.3 |
| E-26 | O framework possui 15 agentes especializados | Arquivos `00-task-router` a `14-handoff-manager` | `.github/agents/` | 5.3 |
| E-27 | Há 4 conjuntos de instruções permanentes | `00-context-economy`, `01-embedded-engineering`, `02-sdd-artifacts`, `03-change-control` | `.github/instructions/` | 5.4 |
| E-28 | Existem 5 skills, 2 delas de produção acadêmica | `create-specification`, `sdd-embarcado`, `artigo-cientifico-latex`, `monografia-engenharia-latex`, `create-readme` | `.github/skills/` | 5.5 |
| E-29 | O fluxo é adaptativo por complexidade, com 6 gates | Fluxograma de decisão e definição dos gates 0 a 5 | `docs/agentic/WORKFLOW.md` | 5.6 |
| E-30 | Existem condições de parada explícitas | Seção "Stop conditions" com 6 situações | `docs/agentic/AGENT-OPERATING-MODE.md` | 5.7 |
| E-31 | A avaliação de origem apontou 8 lacunas no material inicial | Diagnóstico com 8 oportunidades de melhoria numeradas | `AVALIACAO.md` | 5.2, 5.9 |
| E-32 | O fluxo de trabalho é registrado em 16 tarefas | IDs `T-001`…`T-016`, com pendência `T-016` | `tasks.md` | 8.3 |
| E-33 | A interface implementada exibe título, placar com pontuação e recorde, tabuleiro, sobreposição de fim de jogo com botão de reinício, quatro botões direcionais e linha de atalhos | Captura de tela real do produto em execução, no estado de fim de jogo com pontuação 40 e recorde 40 | `images/screenshot_gaming.png` | 7.9 |
| E-34 | A pasta `images/` na raiz é a fonte única da captura de tela, que não é duplicada dentro da monografia | `\graphicspath` em `main.tex` resolve `../images/`; `figures/fig-interface-implementada.tex` inclui o arquivo sem copiá-lo | `main.tex`, `figures/fig-interface-implementada.tex` | 7.9 |

## 3.1 Evidências de compilação do artefato textual

| ID | Afirmação | Evidência | Arquivo/Fonte | Seção |
|---|---|---|---|---|
| E-35 | O projeto LaTeX compila sem erros e produz um PDF de 83 páginas | `latexmk -pdf -interaction=nonstopmode main.tex` com exit code 0; linha `Output written on main.pdf (83 pages` no log | `monografia/main.pdf`, `main.log` | todo o documento |
| E-36 | A composição tipográfica não apresenta caixas transbordantes | Contagem de `Overfull \hbox` e `Underfull \hbox` no log: 0 e 0 | `main.log` | todo o documento |
| E-37 | Todas as referências cruzadas e citações foram resolvidas | Contagem de `undefined citation` / `undefined reference` no log: 0; `main.bbl` com 32 entradas | `main.log`, `main.bbl` | todo o documento |
| E-38 | As três listas de elementos gráficos foram geradas | `main.lof` com 12 figuras, `main.loq` com 21 quadros, `main.lot` com 11 tabelas | `main.lof`, `main.loq`, `main.lot` | pré-textuais |
| E-39 | A captura de tela real foi incorporada ao PDF | Registro `<use ../images/screenshot_gaming.png>` e página 59 no log | `main.log` | 7.9 |
| E-40 | Os 11 diagramas TikZ compilam sem erro | Compilação com 0 erros, com os 11 arquivos de `figures/` efetivamente incluídos | `figures/*.tex`, `main.log` | 2, 5, 7, 8 |

**Ressalva sobre E-40:** "compila sem erro" é diferente de "tem boa aparência". O layout visual dos
diagramas não foi inspecionado (P-37).


## 4. Afirmações sem evidência disponível (marcadas)

| ID | Afirmação pretendida | Situação | Marcador |
|---|---|---|---|
| N-01 | Consumo de memória RAM/CPU do alvo em execução | Não medido | `[DADO NECESSÁRIO]` |
| N-02 | Custo real de tokens/contexto por tarefa agêntica | Não instrumentado | `[DADO NECESSÁRIO]` |
| N-03 | Número de iterações e tempo de parede do ciclo completo | Não registrado | `[DADO NECESSÁRIO]` |
| N-04 | Distribuição do intervalo de tick com múltiplas execuções e intervalo de confiança | Amostra única por faixa | `[DADO NECESSÁRIO]` |
| N-05 | Validação do D-Pad em dispositivo móvel físico | Só emulação | `[PENDENTE T-016]` |
| N-06 | Comparação experimental com grupo de controle (processo sem SDD) | Não executada | `[DADO NECESSÁRIO]` |

## 5. Regra de integridade

Nenhum valor, medição, referência bibliográfica ou resultado desta monografia foi estimado,
interpolado ou reproduzido de memória sem fonte. Onde o dado não existe, há marcador explícito
e registro em `MONOGRAFIA_PENDENCIAS.md`.
