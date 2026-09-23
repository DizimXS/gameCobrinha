---
title: Tarefas — Jogo Snake em arquivo HTML único
version: 1.0
date_created: 2026-09-23
owner: William
tags: [tasks, snake]
---

# Tarefas

Decomposição da implementação descrita em `spec.md` + `design.md`.

| ID | Tarefa | Requisitos | Evidência | Status |
|---|---|---|---|---|
| T-001 | Definir esqueleto do `index.html` (head, viewport, `<style>`, `<script>`) | FR-001, NFR-004 | Inspeção do arquivo | CONCLUÍDA |
| T-002 | Implementar tema neon e layout centralizado responsivo | NFR-002, NFR-003, NFR-009, ADR-007 | Captura de tela | CONCLUÍDA |
| T-003 | Construir HUD (pontuação atual e melhor pontuação) | FR-009 | Captura de tela | CONCLUÍDA |
| T-004 | Centralizar `CONFIG` e o modelo de `state` | CON-003, ADR-005 | Inspeção do código | CONCLUÍDA |
| T-005 | Implementar persistência tolerante de high score | FR-010, ADR-006 | Teste manual/inspeção | CONCLUÍDA |
| T-006 | Implementar regras puras (inversão, comida, colisão) | FR-005, FR-006, FR-008, ADR-003, ADR-004 | Revisão de código | CONCLUÍDA |
| T-007 | Implementar `tick()` e progressão de velocidade | FR-007, FR-008, FR-015 | Teste manual | CONCLUÍDA |
| T-008 | Implementar game loop com `requestAnimationFrame` + acumulador | FR-014, CON-004, ADR-002 | Teste manual | CONCLUÍDA |
| T-009 | Implementar renderização no Canvas 2D com escala por DPR | FR-002, NFR-006, ADR-001 | Captura de tela | CONCLUÍDA |
| T-010 | Implementar controles de teclado (setas + WASD) | FR-003, NFR-007 | Teste manual | CONCLUÍDA |
| T-011 | Implementar D-Pad virtual para toque | FR-004, CA-008 | Teste manual + captura | CONCLUÍDA |
| T-012 | Implementar estados ready/running/paused/over com overlay e reinício | FR-011, FR-012, FR-013 | Teste manual | CONCLUÍDA |
| T-013 | Comentar o código e revisar limpeza/estrutura | NFR-005 | Revisão | CONCLUÍDA |
| T-014 | Verificar CAs no navegador e registrar evidência | CA-001..CA-012 | `docs/05-testing/snake-game-evidence.md` | CONCLUÍDA (com limitações) |
| T-015 | Atualizar `STATUS.md` e rastreabilidade | Processo SDD | `STATUS.md` | CONCLUÍDA |

## Pendências / Limitações conhecidas

- **T-016 (PENDENTE)**: validação do D-Pad em dispositivo móvel físico (evidência `BANCADA`/dispositivo real).
  A verificação atual é por navegador de desktop com emulação de toque.
- `SPEC_DEVIATION`: nenhuma.

## Rastreabilidade

- Cada tarefa aponta para `FR`/`NFR`/`CA` correspondentes.
- A rastreabilidade consolidada está em `docs/agentic/TRACEABILITY.md` (quando aplicável) e em `STATUS.md`.
