# MONOGRAFIA_STATUS.md

> Arquivo de controle de continuidade. Um novo agente deve ler este arquivo primeiro.
> Última atualização: 2026-09-23

## 1. Estágio atual do pipeline

```text
AUDITORIA ................ CONCLUÍDA
COMPREENSÃO DO PROJETO ... CONCLUÍDA
IDENTIFICAÇÃO DO PROBLEMA  CONCLUÍDA
DEFINIÇÃO DOS OBJETIVOS .. CONCLUÍDA
MAPA DE EVIDÊNCIAS ....... CONCLUÍDO   (MONOGRAFIA_EVIDENCIAS.md)
ESTRUTURA ................ CONCLUÍDA
PLANO DOS CAPÍTULOS ...... CONCLUÍDO   (MONOGRAFIA_PLANO.md)
FUNDAMENTAÇÃO ............ RASCUNHO v1
METODOLOGIA .............. RASCUNHO v1
DESENVOLVIMENTO .......... RASCUNHO v1
EXPERIMENTOS ............. RASCUNHO v1
RESULTADOS ............... RASCUNHO v1
DISCUSSÃO ................ RASCUNHO v1
CONCLUSÃO ................ RASCUNHO v1
REVISÃO TÉCNICA .......... NÃO INICIADA
REVISÃO ACADÊMICA ........ NÃO INICIADA
REVISÃO ABNT ............. NÃO INICIADA
COMPILAÇÃO LaTeX ......... **PENDENTE — TOOLCHAIN AUSENTE**
VALIDAÇÃO FINAL .......... NÃO INICIADA
```

## 2. Decisões tomadas sem consulta ao autor

O autor não estava disponível para responder. Decisões registradas para revisão:

| ID | Decisão | Justificativa | Reversibilidade |
|---|---|---|---|
| D-01 | Objeto de estudo = framework SDD agêntico **com o jogo Snake como estudo de caso** | É o único produto com evidência empírica real no workspace (spec, código, 12 CA verificados). Documentar o firmware ESP8266 citado no `AGENTS.md` exigiria inventar código, medições e resultados. | Alta — trocar o estudo de caso não afeta o framework |
| D-02 | Autor = `William` | Único nome de titular presente no workspace (`AGENTS.md` §9 e `LICENSE`). | Alta |
| D-03 | Ano = 2026, idioma = pt-BR, norma = ABNT | Ano do contexto da sessão; idioma do restante do workspace; `AGENTS.md` determina documentação em pt-BR. | Alta |
| D-04 | Classe LaTeX = `abntex2` + `abntex2cite` (alf) | Escolha canônica para monografias ABNT no Brasil. | Média — trocar a classe exige reescrever `main.tex` |
| D-05 | Diagramas em **duas representações**: TikZ (compila no PDF) e Mermaid (`.mmd`, legível no GitHub) | `mmdc` não está instalado; TikZ não depende de ferramenta externa e o Mermaid preserva a fonte editável exigida pela skill. | Alta |
| D-06 | Gráfico do intervalo de tick usa **dados medidos** (2 médias por faixa de pontuação) e, separadamente, a **curva do modelo** definido na especificação | São as únicas grandezas quantitativas realmente medidas. A curva é rotulada como modelo, nunca como medição. | Alta |

## 3. Bloqueio ativo

**A compilação do PDF não foi executada.** Verificado no ambiente:

```text
latexmk   AUSENTE
pdflatex  AUSENTE
xelatex   AUSENTE
tectonic  AUSENTE
mmdc      AUSENTE
```

Consequência: o código LaTeX **não foi validado por compilador**. Erros de sintaxe, pacotes
ausentes e problemas de composição podem existir. Nenhuma afirmação de "PDF gerado" ou
"compilação limpa" foi feita em nenhum documento, conforme a regra de não fabricar evidência.

Instalação de LaTeX via `sudo` não é possível no modo atual. Instalação em nível de usuário
(TinyTeX) foi oferecida ao autor e **não autorizada** até o momento.

### 3.1 Validações que PUDERAM ser executadas sem compilador

| Validação | Método | Resultado |
|---|---|---|
| Todos os `\input` apontam para arquivo existente | busca textual | OK — nenhum include quebrado |
| Todo `\ref` possui `\label` correspondente | comparação de conjuntos | OK |
| Toda citação possui entrada no `.bib` | comparação de conjuntos | OK — 41 entradas, nenhuma órfã |
| Ambientes `\begin`/`\end` balanceados por arquivo | contagem por ambiente | OK — 28 arquivos verificados |
| Inclusões de `\usepackage` sem dependência externa de rede | inspeção do preâmbulo | OK |

### 3.2 Riscos de compilação NÃO verificados (revisar na primeira compilação)

| ID | Risco | Ação se falhar |
|---|---|---|
| P-30 | `newfloat` gera a lista de quadros como `\listofquadro`. Se a distribuição usar outro nome, a chamada em `pre-textuais.tex` falha | Ajustar para o nome gerado, ex.: `\listofquadros` |
| P-31 | `newfloat` pode conflitar com `memoir` (base do `abntex2`) em versões antigas de TeX Live | Alternativa: converter os 21 `quadro` em `table` com `\captionsetup{name=Quadro}` |
| P-32 | `abntex2cite` e `hyperref` são carregados nesta ordem; a ordem inversa também é usada por alguns modelos | Se houver erro de comando já definido, inverter a ordem |
| P-33 | Layout dos 11 diagramas TikZ não foi inspecionado visualmente (depende de compilação) | Ajustar coordenadas após a primeira compilação |
| P-34 | `listings` com caracteres acentuados: as 5 listagens foram escritas apenas com caracteres ASCII justamente para evitar esse problema | Se houver erro, confirmar que nenhum acento entrou nas listagens |


## 4. Próximo passo recomendado

1. Preencher os metadados institucionais em `MONOGRAFIA_PENDENCIAS.md` (bloqueia capa e folha de aprovação).
2. Instalar a toolchain e compilar: `latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex`.
3. Corrigir os erros de compilação e inspecionar o PDF.
4. Verificar as referências do `references.bib` contra as fontes originais.
5. Ampliar a extensão conforme o plano de expansão de `MONOGRAFIA_PLANO.md`.

## 5. Contagem atual (medida por contagem de arquivos, NÃO verificada por compilação)

| Item | Atual (contado) | Meta da skill |
|---|---|---|
| Linhas de LaTeX (total) | 4\,228 | — |
| Capítulos textuais | 10 | — |
| Apêndices | 2 | — |
| Seções (`\section`) | 91 | — |
| Subseções | 5 | — |
| Figuras | 12 (11 diagramas TikZ + 1 captura de tela real) | — |
| Quadros | 21 | — |
| Tabelas | 9 | — |
| Tabelas longas (apêndices) | 2 | — |
| Equações numeradas | 6 | — |
| Listagens de código | 5 | — |
| Referências no `.bib` | 41 | — |
| Diagramas Mermaid | 11 | — |
| **Páginas estimadas** | **45–60** (estimativa grosseira, **não medida** — depende de compilação) | 65–100 |

A extensão está provavelmente **abaixo ou no limite inferior da meta**, o que é registrado
explicitamente. Como a paginação real não pôde ser medida, o valor é uma estimativa e não deve
ser citado como fato. O plano de expansão está em `MONOGRAFIA_PLANO.md`, seção "Plano de expansão
para a meta de extensão", com indicação de quais seções precisam de aprofundamento acadêmico — e
**não** de texto de preenchimento.

## 6. Inventário de arquivos produzidos

```text
main.tex                                    210 linhas
references.bib                              343 linhas, 41 entradas
chapters/pre-textuais.tex                   277 linhas
chapters/01-introducao.tex                  193 linhas
chapters/02-fundamentacao.tex               327 linhas
chapters/03-trabalhos-relacionados.tex      138 linhas
chapters/04-materiais-metodos.tex           288 linhas
chapters/05-framework-sdd-agentico.tex      383 linhas
chapters/06-especificacao.tex               326 linhas
chapters/07-implementacao.tex               391 linhas
chapters/08-resultados.tex                  283 linhas
chapters/09-discussao.tex                   271 linhas
chapters/10-conclusao.tex                   166 linhas
appendices/a-especificacao.tex              103 linhas
appendices/b-lista-verificacao.tex           77 linhas
figures/*.tex                                12 arquivos, 472 linhas
diagrams/*.mmd                               11 arquivos
tables/medicoes-velocidade.md               dados da medição
```
