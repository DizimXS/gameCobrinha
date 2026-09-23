# MONOGRAFIA_PLANO.md

> Plano de capítulos, metas de extensão e plano de expansão.
> Ver também: `MONOGRAFIA_STATUS.md`.

## 1. Título provisório

**DESENVOLVIMENTO DE SOFTWARE ORIENTADO POR ESPECIFICAÇÕES COM AGENTES DE INTELIGÊNCIA ARTIFICIAL:
UM ESTUDO DE CASO APLICADO A UM JOGO DIGITAL EM ARTEFATO ÚNICO**

`[VALIDAR COM O AUTOR]` — confirmar título, subtítulo e aderência à linha de pesquisa do curso.

## 2. Problema de engenharia

Agentes de IA generativa produzem código rapidamente, mas sem um processo que force
especificação prévia, critérios de aceitação verificáveis e evidência de teste, o resultado é
difícil de auditar e de manter. O problema investigado é:

> **Como estruturar um processo de desenvolvimento assistido por agentes de IA de modo que cada
> entrega seja rastreável a um requisito, verificável por um critério de aceitação e sustentada
> por evidência reproduzível?**

## 3. Objetivos

**Geral:** definir, aplicar e avaliar um processo de desenvolvimento orientado por especificações
(SDD) executado por agentes de IA, usando como estudo de caso a construção de um jogo digital
completo.

**Específicos:**

| ID | Objetivo específico | Método | Evidência | Status |
|---|---|---|---|---|
| OE-1 | Projetar o framework de agentes, instruções e artefatos de controle | Síntese a partir de lacunas identificadas na avaliação do material de origem | `.github/agents/`, `docs/agentic/`, `AVALIACAO.md` | ATINGIDO |
| OE-2 | Especificar formalmente o sistema-alvo com requisitos e critérios de aceitação verificáveis | Redação de especificação com IDs `FR/NFR/CA` | `.specs/features/snake-game/spec.md` | ATINGIDO |
| OE-3 | Registrar as decisões arquiteturais do alvo | Documento de decisões `ADR-###` | `.specs/features/snake-game/design.md` | ATINGIDO |
| OE-4 | Implementar o alvo sob as restrições especificadas | Implementação em artefato único | `index.html` | ATINGIDO |
| OE-5 | Verificar cada critério de aceitação com evidência empírica | Automação em navegador + inspeção estática | `docs/05-testing/snake-game-evidence.md` | ATINGIDO |
| OE-6 | Avaliar o processo, suas limitações e ameaças à validade | Análise crítica dos resultados | Capítulo 9 | PARCIAL |

## 4. Estrutura e metas de extensão

| Cap. | Título | Meta (páginas) | Status |
|---|---|---|---|
| — | Pré-textuais (capa, aprovação, resumo, abstract, listas) | 8 | RASCUNHO v1 |
| 1 | Introdução | 8 | RASCUNHO v1 |
| 2 | Fundamentação teórica | 16 | RASCUNHO v1 |
| 3 | Trabalhos relacionados | 8 | RASCUNHO v1 |
| 4 | Materiais e métodos | 9 | RASCUNHO v1 |
| 5 | O framework SDD agêntico | 12 | RASCUNHO v1 |
| 6 | Especificação do sistema-alvo | 9 | RASCUNHO v1 |
| 7 | Implementação | 13 | RASCUNHO v1 |
| 8 | Experimentos e resultados | 12 | RASCUNHO v1 |
| 9 | Discussão | 10 | RASCUNHO v1 |
| 10 | Conclusão | 5 | RASCUNHO v1 |
| — | Pós-textuais (referências, apêndices) | — | FORA DA CONTAGEM |
| | **Total** | **110** | — |

Meta institucional típica: 65–100 páginas de conteúdo. O capítulo 2 é intencionalmente o mais
longo por concentrar a fundamentação de duas áreas (engenharia de software e agentes de IA).

## 5. Diagramas e figuras produzidos

Onze diagramas foram produzidos nas duas representações previstas (TikZ e Mermaid), e uma
captura de tela real do produto foi incluída como registro visual.

| ID | Diagrama | Tipo | Capítulo | Arquivos |
|---|---|---|---|---|
| DG-01 | Fluxo agêntico adaptativo por complexidade | Fluxograma | 5 | `fig-fluxo-agentico.tex`, `dg-01-fluxo-agentico.mmd` |
| DG-02 | Camadas do framework | Blocos | 5 | `fig-camadas.tex`, `dg-02-camadas.mmd` |
| DG-03 | Pipeline SDD com portões | Fluxograma | 5 | `fig-pipeline-gates.tex`, `dg-03-pipeline-gates.mmd` |
| DG-04 | Máquina de estados da partida | Estados | 7 | `fig-fsm.tex`, `dg-04-fsm-partida.mmd` |
| DG-05 | Laço de animação com acumulador | Fluxograma | 7 | `fig-game-loop.tex`, `dg-05-game-loop.mmd` |
| DG-06 | Algoritmo de um passo de simulação | Fluxograma | 7 | `fig-tick.tex`, `dg-06-tick.mmd` |
| DG-07 | Estrutura do artefato único | Blocos | 7 | `fig-artefato.tex`, `dg-07-estrutura-artefato.mmd` |
| DG-08 | Sequência de tratamento de entrada | Sequência | 7 | `fig-sequencia.tex`, `dg-08-sequencia-entrada.mmd` |
| DG-09 | Cadeia de rastreabilidade | Grafo | 5 | `fig-rastreabilidade.tex`, `dg-09-rastreabilidade.mmd` |
| DG-10 | Camadas de revisão da monografia | Camadas | 2 | `fig-verificacao.tex`, `dg-10-verificacao-camadas.mmd` |
| DG-11 | Modelo de velocidade e valores medidos | Gráfico | 8 | `fig-velocidade.tex`, `dg-11-velocidade-modelo.mmd` |
| F-12 | Interface implementada no estado de fim de jogo | Captura de tela real | 7 | `fig-interface-implementada.tex` → `images/screenshot_gaming.png` |

**Nenhum diagrama foi renderizado e inspecionado visualmente**: a versão TikZ depende de
compilação LaTeX (P-33) e a versão Mermaid depende de `mmdc`, ausente no ambiente. A
correspondência entre as duas representações é semântica, não visual — ver
`MONOGRAFIA_PENDENCIAS.md`, seção 8.

A captura de tela (F-12) é a única figura cuja fonte é uma imagem real, não um diagrama.
Ela é mantida em `images/` na raiz do repositório e referenciada por caminho relativo, sem
duplicação. Dois riscos associados: canal alfa (P-35) e caminho relativo (P-36).

## 6. Plano de expansão — META JÁ ATINGIDA

A skill exige 65–100 páginas. A compilação produziu **83 páginas** (`main.pdf`, medido pelo
compilador), portanto **a meta de extensão está cumprida** e o plano abaixo passa a ser
**opcional**: são frentes de aprofundamento acadêmico, não de preenchimento.

A expansão **não** deve ser feita com texto genérico. Se houver interesse em aprofundar, as
frentes acadêmicas específicas são as seguintes:

| Frente | O que falta | Ganho estimado |
|---|---|---|
| Cap. 2.3 | Revisão sistemática da literatura sobre agentes LLM para engenharia de software, com protocolo de busca explícito (bases, strings, critérios de inclusão/exclusão) | +6 pág. |
| Cap. 2.4 | Comparação crítica de modelos de qualidade (ISO/IEC 25010) aplicados ao alvo, com justificativa de métricas | +4 pág. |
| Cap. 3 | Ampliar de 5 para 10–12 trabalhos relacionados, com quadro comparativo multidimensional | +4 pág. |
| Cap. 4 | Detalhar o protocolo experimental: ameaças à validade (interna, externa, de construto, de conclusão) segundo Wohlin et al. e Runeson & Höst | +3 pág. |
| Cap. 6 | Derivação dos requisitos a partir do memorial com matriz de rastreabilidade textual completa | +2 pág. |
| Cap. 7 | Análise de complexidade dos algoritmos de tick e busca de célula livre, com cálculo de pior caso | +3 pág. |
| Cap. 7 | Medição de consumo de memória e desempenho de renderização (requer instrumentação — atualmente não medida) | +3 pág. |
| Cap. 8 | Repetição dos experimentos com múltiplas execuções e intervalo de confiança (requer novas medições) | +4 pág. |
| Cap. 9 | Discussão sobre reprodutibilidade, custo de contexto e confiabilidade de agentes, com literatura | +4 pág. |

**Dependências de dados não disponíveis:** as frentes que exigem novas medições (Cap. 7 e 8)
estão bloqueadas por `[DADO NECESSÁRIO]` até que a instrumentação seja executada. Não serão
preenchidas com valores estimados.

## 7. Aderência ABNT prevista

| Norma | Aplicação |
|---|---|
| NBR 14724 | Apresentação de trabalhos acadêmicos (estrutura, margens, elementos) |
| NBR 6023 | Elaboração de referências |
| NBR 10520 | Citações em documentos |
| NBR 6027 | Sumário |
| NBR 6024 | Numeração progressiva das seções |
| NBR 6028 | Resumo e abstract |
| NBR 6022 | Artigo científico (referência auxiliar) |

`[VALIDAR COM O AUTOR]` — confirmar o manual de normalização institucional, que **prevalece**
sobre a configuração genérica do `main.tex`. Verificar a edição vigente de cada norma antes da
entrega final.
