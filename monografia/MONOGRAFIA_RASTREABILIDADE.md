# MONOGRAFIA_RASTREABILIDADE.md

> Duas matrizes de rastreabilidade exigidas pela skill:
> (a) `Problema | Objetivo geral | Objetivos específicos | Método | Resultado`
> (b) `Objetivo específico | Método | Evidência | Resultado | Status`

## 1. Cadeia problema → resultado

| Elo | Conteúdo | Documento |
|---|---|---|
| Problema | Como estruturar um processo de desenvolvimento assistido por agentes de IA de modo que cada entrega seja rastreável a um requisito, verificável por um critério de aceitação e sustentada por evidência reproduzível? | Cap. 1.2 |
| Objetivo geral | Definir, aplicar e avaliar um processo de desenvolvimento orientado por especificações (SDD) executado por agentes de IA, com um jogo digital como estudo de caso | Cap. 1.5 |
| Objetivo específico 1 | Projetar o framework de agentes, instruções e artefatos de controle | Cap. 5 |
| Objetivo específico 2 | Especificar formalmente o alvo com requisitos e critérios de aceitação verificáveis | Cap. 6 |
| Objetivo específico 3 | Registrar as decisões arquiteturais do alvo | Cap. 6.5, 7.3 |
| Objetivo específico 4 | Implementar o alvo sob as restrições especificadas | Cap. 7 |
| Objetivo específico 5 | Verificar cada critério de aceitação com evidência empírica | Cap. 8 |
| Objetivo específico 6 | Avaliar o processo, suas limitações e ameaças à validade | Cap. 9 |
| Método | Estudo de caso único, de natureza aplicada, com verificação empírica em navegador e inspeção estática | Cap. 4 |
| Resultado | 12 de 12 critérios de aceitação verificados; velocidade de simulação confirmada como crescente (133 ms → 105 ms); 1 pendência de validação em hardware físico | Cap. 8, 10 |

## 2. Objetivo específico → evidência

| OE | Método | Evidência | Resultado | Status |
|---|---|---|---|---|
| OE-1 | Síntese a partir das lacunas identificadas em `AVALIACAO.md`; redação de 15 agentes, 4 instruções e 5 skills | E-26, E-27, E-28, E-29, E-30, E-31 | Framework operacional com fluxo adaptativo e 6 gates | ATINGIDO |
| OE-2 | Especificação com IDs `FR/NFR/CA`, casos de borda e contratos de dados | E-23, E-24 | 15 FR, 9 NFR, 12 CA documentados | ATINGIDO |
| OE-3 | Registro de decisões no formato ADR | E-25 | 7 ADRs com alternativas rejeitadas | ATINGIDO |
| OE-4 | Implementação em artefato único sob as restrições `CON-001`/`CON-002` | E-01, E-02 | `index.html` com 1063 linhas, zero dependências | ATINGIDO |
| OE-5 | Inspeção estática + automação de navegador (teclado, ponteiro, leitura de pixels do canvas) | E-11 … E-22 | 12/12 CA com resultado PASS | ATINGIDO |
| OE-6 | Análise crítica com ameaças à validade | Cap. 9 | 6 ameaças identificadas; 6 dados ausentes marcados | PARCIAL — depende de novas medições |

## 3. Requisito → critério → evidência (produto)

| Requisito | Critério | Evidência | Resultado |
|---|---|---|---|
| FR-001, NFR-001 | CA-001 | E-01, E-11 | PASS |
| FR-005 | CA-002 | E-04, E-12 | PASS |
| FR-007 | CA-003 | E-13 | PASS |
| FR-006 | CA-004 | E-06, E-14 | PASS |
| FR-008 (paredes) | CA-005 | E-15 | PASS |
| FR-008 (corpo) | CA-006 | E-05, E-16 | PASS |
| FR-010 | CA-007 | E-07, E-17 | PASS |
| FR-003, FR-004 | CA-008 | E-18 | PASS |
| FR-012 | CA-009 | E-19 | PASS |
| FR-013 | CA-010 | E-20 | PASS |
| NFR-008 | CA-011 | E-21 | PASS |
| FR-015 | CA-012 | E-10, E-22 | PASS |
| NFR-004 | — | E-08 (parcial) | PASS PARCIAL |
| NFR-009 | — | E-09 | PASS |
| NFR-006 | — | E-08 | PASS |

**Requisitos sem critério de aceitação dedicado:** NFR-002, NFR-003, NFR-005, NFR-007, CON-003,
CON-004, GUD-001…003, PAT-001, PAT-002. A verificação desses itens é por inspeção de código
(Evidência E-03 … E-10) e não por experimento. Isso é registrado como limitação metodológica
no Capítulo 9.

## 4. Lacuna de rastreabilidade identificada (autocrítica)

O mapa de evidências revelou uma **falha no próprio artefato de especificação**: nove requisitos
não funcionais e de restrição não possuem critério de aceitação associado. Sem `CA`, não há
verificação automatizável e a rastreabilidade se rompe entre `NFR` e evidência.

Ações:

1. Registrar como limitação em `MONOGRAFIA_PENDENCIAS.md`.
2. Propor correção no Capítulo 10 como trabalho futuro (adicionar `CA-013`…`CA-021`).
3. **Não** corrigir a especificação retroativamente sem justificativa, pois o artefato é
   objeto de estudo e alterá-lo agora invalidaria o relato da seção 8.
