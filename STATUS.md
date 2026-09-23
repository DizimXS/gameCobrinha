# STATUS.md — Estado do projeto

> Última atualização: 2026-09-23
> Fonte de verdade do progresso. Atualizado ao final de cada fase.

## 1. Fase atual

**Recurso `snake-game`**: especificado, implementado e verificado em navegador. **Concluído com 1 pendência.**

## 2. Entregáveis

| Artefato | Caminho | Estado |
|---|---|---|
| Especificação | `.specs/features/snake-game/spec.md` | CONCLUÍDO |
| Design | `.specs/features/snake-game/design.md` | CONCLUÍDO |
| Tarefas | `.specs/features/snake-game/tasks.md` | CONCLUÍDO (T-016 pendente) |
| Implementação | `index.html` | CONCLUÍDO |
| Evidência de teste | `docs/05-testing/snake-game-evidence.md` | CONCLUÍDO |
| Documentação de uso | `README.md` | CONCLUÍDO |
| Licença | `LICENSE` (MIT) | CONCLUÍDO — conflita com `AGENTS.md` §9 |
| Ignorados de VCS | `.gitignore` | CONCLUÍDO |

## 3. Rastreabilidade (resumo)

| Requisitos | Critérios de aceitação | Evidência | Resultado |
|---|---|---|---|
| FR-001, NFR-001 | CA-001 | inspeção estática | PASS |
| FR-005 | CA-002 | automação de teclado | PASS |
| FR-007 | CA-003 | bot + HUD | PASS |
| FR-006 | CA-004 | 25 amostras de pixel | PASS |
| FR-008 (paredes) | CA-005 | automação de teclado | PASS |
| FR-008 (corpo) | CA-006 | giro fechado + posição da cabeça | PASS |
| FR-010 | CA-007 | `localStorage` + reload | PASS |
| FR-004 | CA-008 | `pointerdown` no D-Pad | PASS |
| FR-012 | CA-009 | overlay + primeira direção | PASS |
| FR-013 | CA-010 | pausa/retomada | PASS |
| NFR-008 | CA-011 | log de console | PASS |
| FR-015 | CA-012 | 133 ms → 105 ms (média de 184 movimentos) | PASS |

Detalhamento completo: `docs/05-testing/snake-game-evidence.md`.

## 4. Nível de evidência

- **NAVEGADOR** (execução em `file://` no navegador integrado) — todos os CAs.
- **PENDENTE**: teste em dispositivo móvel físico (toque real) para o D-Pad (T-016).

## 5. Pendências e riscos residuais

| ID | Descrição | Impacto | Ação |
|---|---|---|---|
| T-016 | D-Pad não validado em dispositivo de toque físico | Baixo (validado por emulação de `pointerdown`) | Validar em celular/tablet real |
| — | Responsividade não testada em todas as larguras de 320 px a 1920 px | Baixo | Amostragem adicional se necessário |
| — | Ausência de testes automatizados versionados | Médio (regressão só detectável manualmente) | Restrição CON-001/CON-002 do artefato único |
| **LIC** | `LICENSE`/`README.md` declaram **MIT**, mas `AGENTS.md` §9 declara **Apache 2.0** | **Alto** (ambiguidade jurídica) | Decidir a licença única e reconciliar `AGENTS.md` §9 |

`SPEC_DEVIATION`: nenhuma.

## 6. Conflito de contexto identificado (requer decisão)

O `AGENTS.md` descreve um **sistema embarcado ESP8266** (DS18B20, PWM, OneWire, PlatformIO),
enquanto `docs/memorial.txt` e `docs/descricao.txt` definem um **jogo Snake em HTML único** —
tecnologias, alvos e comandos de build totalmente diferentes.

- O memorial foi tratado como a fonte do produto desta tarefa (instrução explícita do solicitante).
- As regras de **processo** do `AGENTS.md` (SDD, IDs `FR/NFR/CA/T-`, rastreabilidade, não declarar
  evidência sem prova) foram aplicadas integralmente.
- As regras de **hardware/embarcado** do `AGENTS.md` **não se aplicam** a este artefato.
- **Ação requerida**: decidir se o `AGENTS.md` deve ser atualizado para o produto Snake ou se o
  memorial é um exercício dentro do repositório de firmware.

## 7. Próximos passos

1. Validar o D-Pad em dispositivo móvel físico (fecha T-016).
2. Resolver o conflito entre `AGENTS.md` e `docs/memorial.txt` (seção 6).
3. **Reconciliar a licença**: confirmar MIT e atualizar `AGENTS.md` §9 (hoje Apache 2.0).
4. Definir `HANDOFF.md` apenas se houver transferência de validação pendente a outro responsável.
