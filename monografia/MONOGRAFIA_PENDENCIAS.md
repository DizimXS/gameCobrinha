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
| P-08 | ~~**Toolchain LaTeX ausente**~~ | ~~PDF não gerado~~ | — | **RESOLVIDO.** TinyTeX instalado em `~/.TinyTeX`; `main.pdf` gerado com 83 páginas e 0 erros. Ver `MONOGRAFIA_STATUS.md` § 3 |
| P-09 | ~~`abntex2` não é instalado por padrão~~ | ~~Falha de compilação~~ | — | **RESOLVIDO.** `tlmgr install abntex2 ...` executado; classe e estilo de citação presentes |
| P-37 | **Inspeção visual do PDF não executada** | Layout dos diagramas TikZ e composição final não conferidos | `main.pdf` | Abrir o PDF em um leitor; `pdfinfo`, `pdftotext`, `pdftoppm`, `gs` e `qpdf` estão ausentes no ambiente |

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

## 6. Riscos de compilação — status após a primeira compilação

A compilação foi executada com sucesso (`latexmk -pdf -interaction=nonstopmode main.tex`,
exit code 0, `main.pdf` com 83 páginas, 0 erros, 0 overfull/underfull hbox, 0 referências ou
citações não definidas). Os riscos previstos foram resolvidos ou reclassificados:

| ID | Risco previsto | Status após compilar |
|---|---|---|
| P-30 | `newfloat` geraria a lista de quadros com outro nome | **RESOLVIDO.** `\listofquadro` funcionou; `main.loq` gerado com 21 quadros |
| P-31 | `newfloat` conflitaria com `memoir` | **NÃO SE CONFIRMOU.** Coexistem sem erro |
| P-32 | Ordem entre `hyperref` e `abntex2cite` | **NÃO SE CONFIRMOU.** A ordem adotada funciona |
| P-33 | Layout dos 11 diagramas TikZ não inspecionado | **PARCIAL.** Os 11 diagramas **compilam sem erro**, mas a disposição visual segue não inspecionada (P-37) |
| P-34 | Acentuação em `listings` | **NÃO SE CONFIRMOU.** As 5 listagens compilaram; a decisão de usar só ASCII evitou o problema |
| P-35 | Canal alfa do PNG (RGBA) | **RESOLVIDO.** A imagem foi incluída com sucesso (`<use ../images/screenshot_gaming.png>`, página 59) |
| P-36 | Caminho relativo `../images/` quebra no Overleaf | **AINDA VÁLIDO.** Copiar a imagem para `monografia/figures/` antes de envio isolado |

Três defeitos **não previstos** foram encontrados e corrigidos pela compilação (D-01, D-02, D-03 em
`MONOGRAFIA_STATUS.md` § 3.4). Nenhuma validação estática os detectaria, porque são de semântica de
macro: colchete interpretado como argumento opcional, ambiente de lista usado como tabela e
colchete interno fechando argumento opcional.

## 7. Resumo executivo das pendências

- **Bloqueadores de entrega:** P-01 … P-07 (7 itens — P-08 e P-09 foram **resolvidos** pela
  instalação do TinyTeX e pela compilação bem-sucedida).
- **Dados ausentes:** P-10 … P-15 (6 itens) — nenhum será estimado.
- **Verificação bibliográfica:** P-16 … P-21 (6 itens).
- **Decisões a validar:** P-22 … P-25 (4 itens).
- **Inconsistências do workspace:** P-26 … P-29 (4 itens).
- **Riscos de compilação:** P-30 … P-36 (7 itens — dois resolvidos, quatro não se confirmaram,
  um ainda válido).
- **Novas pendências:** P-37 (inspeção visual do PDF).

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

