# Snake Neon

Jogo da cobrinha (Snake) completo, entregue em **um único arquivo `index.html`** com HTML, CSS e JavaScript embutidos. Sem build, sem servidor, sem dependências externas — abra o arquivo e jogue.

Renderizado em HTML5 Canvas 2D com tema escuro estilo retro-arcade, placar persistente e controles para teclado e toque.

## Funcionalidades

- **Arquivo único e autocontido** — `<style>` e `<script>` embutidos, zero requisições de rede.
- **Canvas 2D** com nitidez ajustada ao `devicePixelRatio` da tela.
- **Placar** com Pontuação Atual e Melhor Pontuação, persistida em `localStorage`.
- **Controles duplos** — setas do teclado e WASD, além de um D-Pad virtual para dispositivos de toque.
- **Movimentação segura** — inversão de 180° bloqueada e fila de entrada curta, para não perder comandos em ticks rápidos.
- **Comida inteligente** — sempre sorteada em célula livre, nunca sobre o corpo da cobra.
- **Dificuldade progressiva** — a velocidade aumenta a cada comida, até um intervalo mínimo (`minStepMs`).
- **Pausa e reinício** — `Espaço` pausa/retoma, `Enter` reinicia, perder o foco da janela pausa automaticamente.
- **Game Over amigável** com pontuação final e botão de reinício.
- **Acessibilidade** — foco visível, região viva para leitores de tela e suporte a `prefers-reduced-motion`.

## Como jogar

| Ação | Teclado | Toque |
|---|---|---|
| Mover | `↑` `↓` `←` `→` ou `W` `A` `S` `D` | Botões do D-Pad |
| Pausar / retomar | `Espaço` ou `P` | Botão do overlay |
| Iniciar / reiniciar | `Enter` | Botão do overlay |
| Reiniciar direto | `R` | — |

O jogo começa em estado de espera: a cobra só se move após a primeira entrada direcional válida. Cada comida vale **10 pontos** e aumenta o comprimento em uma célula. A partida termina ao colidir com as paredes do tabuleiro ou com o próprio corpo.

## Como executar

Basta abrir o arquivo no navegador:

```bash
# Linux
xdg-open index.html

# macOS
open index.html
```

Ou sirva o diretório localmente, se preferir:

```bash
python3 -m http.server 8000
# acesse http://localhost:8000/index.html
```

**Requisitos:** qualquer navegador moderno com Canvas 2D, `requestAnimationFrame` e `localStorage`. Nenhuma instalação é necessária.

## Estrutura do projeto

```text
.
├── index.html                          # o jogo completo (arquivo único)
├── STATUS.md                           # estado atual, pendências e riscos
├── AGENTS.md                          # regras permanentes do projeto
├── .gitignore
├── LICENSE                             # MIT
├── .specs/features/snake-game/
│   ├── spec.md                         # requisitos FR/NFR e critérios de aceitação CA
│   ├── design.md                       # decisões arquiteturais (ADR) e algoritmos
│   └── tasks.md                        # decomposição T-###
├── docs/
│   ├── memorial.txt                    # solicitação original do produto (não editar)
│   ├── descricao.txt
│   ├── 05-testing/                     # evidências de teste e capturas de tela
│   └── agentic/                        # protocolo, workflow e DoD dos agentes
└── .github/                            # skills, instructions e agentes
```

## Especificação e rastreabilidade

O projeto segue um fluxo de **Spec-Driven Development (SDD)**: nenhuma mudança de comportamento é feita sem requisito e critério de aceitação correspondentes.

- Requisitos funcionais e não funcionais: `.specs/features/snake-game/spec.md` (`FR-###`, `NFR-###`)
- Critérios de aceitação: `CA-001` a `CA-012` na mesma especificação
- Decisões arquiteturais: `ADR-001` a `ADR-007` em `.specs/features/snake-game/design.md`
- Tarefas: `T-001` a `T-016` em `.specs/features/snake-game/tasks.md`
- Estado consolidado e pendências: `STATUS.md`

## Testes e evidências

O artefato é verificado em **navegador real** (`file://`), por inspeção estática e automação de teclado, toque e leitura de pixels do canvas. Os 12 critérios de aceitação foram validados:

| Verificação | Resultado |
|---|---|
| Arquivo único, sem dependências externas (`CA-001`) | PASS |
| Inversão de direção bloqueada (`CA-002`) | PASS |
| Crescimento e pontuação ao comer (`CA-003`) | PASS |
| Comida nunca sobre o corpo (`CA-004`) | PASS |
| Colisão com parede (`CA-005`) e com o próprio corpo (`CA-006`) | PASS |
| Melhor pontuação persistida em `localStorage` (`CA-007`) | PASS |
| D-Pad, início, pausa e ausência de erros no console (`CA-008`…`CA-011`) | PASS |
| Velocidade crescente com a pontuação (`CA-012`) | PASS — 133 ms → 105 ms |

Detalhes, método e capturas de tela: [`docs/05-testing/snake-game-evidence.md`](docs/05-testing/snake-game-evidence.md).

> [!NOTE]
> A validação do D-Pad em **dispositivo móvel físico** segue pendente (`T-016`). A verificação atual usa emulação de `pointerdown` em navegador de desktop, o que não equivale a validação em hardware real.

## Limitações conhecidas

- Não há testes automatizados versionados: a restrição de artefato único e zero dependências impede adicionar um framework de teste ao projeto.
- Responsividade verificada por cálculo de layout e pontos de quebra em CSS, não em todas as larguras de 320 px a 1920 px.
- Sem áudio, sem níveis de dificuldade selecionáveis e sem itens especiais — esses recursos não foram solicitados na especificação.

> [!IMPORTANT]
> O `AGENTS.md` descreve um firmware ESP8266 (DS18B20, PWM, PlatformIO), enquanto o produto
> implementado aqui é o jogo descrito em `docs/memorial.txt`. As regras de processo do `AGENTS.md`
> (SDD, rastreabilidade, evidência) foram aplicadas; as regras de hardware/embarcado não se aplicam.
> A decisão sobre qual produto o repositório deve conter está registrada em `STATUS.md`.

## Licença

Distribuído sob a licença **MIT**. Consulte o arquivo [`LICENSE`](LICENSE) para o texto completo.

```text
MIT License

Copyright (c) 2026 William
```

<!--
Nota de manutenção: o AGENTS.md (seção 9) ainda declara Apache License 2.0.
A licença efetiva do repositório é MIT, conforme solicitado. Os dois documentos
precisam ser reconciliados — ver seção 6 de STATUS.md.
-->
