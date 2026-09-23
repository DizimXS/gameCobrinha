# MONOGRAFIA_PENDENCIAS.md

> Registro de toda informação crítica ausente e de todo item que depende de decisão do autor.
> Marcadores usados no corpo da monografia: `[VALIDAR COM O AUTOR]`, `[INFORMAÇÃO AUSENTE]`,
> `[DADO NECESSÁRIO]`.

## 1. Bloqueadores — impedem a entrega final

| ID | Pendência | Impacto | Onde aparece | Ação necessária |
|---|---|---|---|---|
| P-01 | Instituição de ensino não informada | Capa, folha de aprovação, referência bibliográfica do trabalho | `main.tex`, `pre-textuais.tex` | Informar nome completo e sigla |
| P-02 | Curso de graduação não informado | Capa, folha de aprovação | idem | Informar nome do curso e grau (Bacharelado/Licenciatura) |
| P-03 | Nome do orientador não informado | Capa, folha de aprovação, agradecimentos | idem | Informar nome, titulação e instituição |
| P-04 | Coorientador (se houver) não informado | Folha de aprovação | idem | Informar ou confirmar inexistência |
| P-05 | Banca examinadora (membros e titulação) não informada | Folha de aprovação | idem | Informar após a defesa |
| P-06 | Cidade e data de apresentação não informadas | Capa, folha de aprovação | idem | Informar cidade e mês/ano |
| P-07 | Manual de normalização institucional não disponível no workspace | Ajustes de margens, espaçamento, elementos pré-textuais | `main.tex` | Fornecer o modelo oficial — ele prevalece |
| P-08 | **Toolchain LaTeX ausente** | PDF não gerado; código não validado por compilador | todo o projeto | Instalar TeX Live/MiKTeX e compilar |
| P-09 | `abntex2` não é instalado por padrão em distribuições mínimas | Falha de compilação | `main.tex` | Verificar dependências no `README.md` da monografia |

## 2. Dados experimentais ausentes

| ID | Dado necessário | Por que é necessário | Como obter |
|---|---|---|---|
| P-10 | Consumo de memória e taxa de quadros do alvo | Sustentar afirmações de eficiência no Cap. 7 | Instrumentar com `performance.memory` e *Performance* do DevTools |
| P-11 | Intervalo de tick com N execuções e intervalo de confiança | Rigor estatístico do Cap. 8 | Repetir a medição ≥ 10 vezes e aplicar estatística descritiva |
| P-12 | Custo de contexto/tokens por tarefa agêntica | Sustentar a análise de custo do framework no Cap. 5 e 9 | Instrumentar a sessão e registrar |
| P-13 | Tempo de parede e número de iterações por gate | Quantificar o processo | Registrar durante a execução |
| P-14 | Grupo de controle: mesmo alvo desenvolvido sem SDD | Sustentar a comparação do Cap. 9 | Executar experimento controlado |
| P-15 | Validação do D-Pad em dispositivo de toque físico | Fechar a pendência `T-016` do projeto | Testar em celular/tablet real |

## 3. Verificação bibliográfica obrigatória

**Todas as entradas de `references.bib` devem ser conferidas contra a fonte original antes da
entrega.** As entradas foram declaradas com os campos de que se tem certeza (autor, título, ano,
editora/veículo). Campos como **ISBN, DOI, número de páginas, volume e número de fascículo foram
deliberadamente omitidos** onde não havia certeza — campos omitidos são preferíveis a campos
inventados.

| ID | Item a verificar |
|---|---|
| P-16 | Conferir edição e ano de cada livro (edições brasileiras mudam ano e editora) |
| P-17 | Confirmar volume, número e páginas dos artigos de periódico |
| P-18 | Confirmar a edição vigente das normas ABNT citadas (NBR 14724, 6023, 10520, 6027, 6024, 6028) |
| P-19 | Confirmar ano e status das normas ISO/IEC e das recomendações W3C/WHATWG/ECMA |
| P-20 | Confirmar autoria completa dos artigos de arXiv (listas longas de autores) |
| P-21 | Verificar se há tradução brasileira preferível para as obras estrangeiras (NBR 10520) |

## 4. Decisões de conteúdo a validar

| ID | Decisão tomada | Alternativa | Marcador |
|---|---|---|---|
| P-22 | Objeto = framework agêntico com o jogo Snake como estudo de caso | Documentar o firmware ESP8266 descrito no `AGENTS.md` — **inviável sem inventar dados**, pois não há código, medição ou resultado de firmware no workspace | `[VALIDAR COM O AUTOR]` |
| P-23 | Título provisório | — | `[VALIDAR COM O AUTOR]` |
| P-24 | Uso de `abntex2` como classe | `book` + formatação manual; modelo institucional | `[VALIDAR COM O AUTOR]` |
| P-25 | Extensão atual (~35–45 pág.) abaixo da meta de 65–100 | Expandir conforme plano — requer novas medições e revisão de literatura | Registrado em `MONOGRAFIA_PLANO.md` § 6 |

## 5. Inconsistências documentais do workspace (não da monografia)

| ID | Inconsistência | Tratamento |
|---|---|---|
| P-26 | `AGENTS.md` descreve firmware ESP8266 (`platformio.ini`, GPIO, OneWire, PWM); o memorial e o produto implementado são um jogo HTML. São dois projetos distintos no mesmo repositório | Registrado no Cap. 4 (contexto do estudo de caso) e no `STATUS.md` do projeto |
| P-27 | O `.gitignore` contém regras de PlatformIO que não se aplicam ao produto atual | Mantidas para não quebrar o projeto de firmware; registrado como dívida de organização |
| P-28 | `AGENTS.md` §9 declara licença Apache 2.0; `README.md`/`LICENSE` declaram MIT | Registrado como pendência jurídica `LIC` no `STATUS.md`; fora do escopo da monografia |
| P-29 | Nove requisitos não funcionais do alvo não possuem critério de aceitação associado | Registrado como limitação metodológica e autocrítica (Cap. 9, `MONOGRAFIA_RASTREABILIDADE.md` § 4) |

## 6. Riscos de compilação não verificados

As validações possíveis sem compilador foram executadas e passaram: todos os `\input`
resolvem, todo `\ref` tem `\label`, toda citação tem entrada no `.bib`, e os ambientes
`\begin`/`\end` estão balanceados nos 28 arquivos LaTeX. Os itens abaixo **não** puderam ser
verificados e devem ser revisados na primeira compilação com sucesso.

| ID | Risco | Ação se falhar |
|---|---|---|
| P-30 | `newfloat` nomeia a lista de quadros a partir do ambiente, gerando `\listofquadro` | Ajustar o nome em `chapters/pre-textuais.tex` |
| P-31 | `newfloat` pode conflitar com `memoir` em versões antigas de TeX Live | Converter os 21 `quadro` em `table` com `\captionsetup{name=Quadro}` |
| P-32 | Ordem de carregamento entre `hyperref` e `abntex2cite` | Inverter a ordem no preâmbulo |
| P-33 | Layout dos 11 diagramas TikZ não inspecionado visualmente | Ajustar coordenadas após a primeira compilação |
| P-34 | Acentuação em `listings` com `pdflatex` | As 5 listagens foram escritas só com ASCII; se houver erro, confirmar que nenhum acento entrou |
| P-35 | A captura de tela `images/screenshot_gaming.png` é PNG de 8 bits com canal alfa (RGB**A**, color type 6, 601×849). O `pdflatex` pode não tratar o canal alfa corretamente, resultando em fundo preto ou canal descartado | Converter para RGB antes de compilar: `magick screenshot_gaming.png -background black -flatten screenshot_gaming.rgb.png` (ImageMagick está ausente neste ambiente) ou compilar com `xelatex`/`lualatex`. O canal alfa **não pôde ser inspecionado** (sem PIL, sem `pngcheck`) |
| P-36 | O caminho da imagem é relativo (`../images/`). Se a pasta `monografia/` for movida para outro local ou enviada isoladamente ao Overleaf, a figura não será encontrada | Copiar `images/screenshot_gaming.png` para `monografia/figures/` antes do envio isolado. `\graphicspath` já contempla `figures/` e `images/` |

## 7. Resumo executivo das pendências

- **Bloqueadores de entrega:** P-01 … P-09 (9 itens).
- **Dados ausentes:** P-10 … P-15 (6 itens) — nenhum será estimado.
- **Verificação bibliográfica:** P-16 … P-21 (6 itens).
- **Decisões a validar:** P-22 … P-25 (4 itens).
- **Inconsistências do workspace:** P-26 … P-29 (4 itens).
- **Riscos de compilação:** P-30 … P-36 (7 itens).

Nenhum item desta lista foi resolvido por suposição silenciosa. Todos estão visíveis no corpo
da monografia por meio de marcador explícito.

## 8. Decisão de diagramação em duas representações (DG)

Conforme a decisão D-05, cada diagrama existe em duas formas:

| Representação | Local | Função | Verificação |
|---|---|---|---|
| TikZ | `figures/*.tex` | Compila dentro do PDF, sem ferramenta externa | Layout **não** inspecionado (P-33) |
| Mermaid | `diagrams/*.mmd` | Fonte editável e legível no GitHub | Sintaxe **não** validada (sem `mmdc`) |

Nenhuma das duas formas foi renderizada e inspecionada visualmente. A correspondência entre as
duas versões é semântica: elas representam o mesmo modelo, mas **não** são idênticas em
disposição visual, porque as ferramentas têm capacidades de layout diferentes.

