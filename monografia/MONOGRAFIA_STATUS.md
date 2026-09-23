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
REVISÃO TÉCNICA .......... PARCIAL (defeitos de compilação corrigidos)
REVISÃO ACADÊMICA ........ NÃO INICIADA
REVISÃO ABNT ............. PARCIAL (classe abntex2 aplicada; manual institucional pendente)
COMPILAÇÃO LaTeX ......... **CONCLUÍDA — 83 páginas, 0 erros**
INSPEÇÃO VISUAL DO PDF ... **NÃO EXECUTADA — sem ferramenta de inspeção**
VALIDAÇÃO FINAL .......... PARCIAL
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

## 3. Toolchain e compilação — RESOLVIDO

### 3.1 Estado inicial (antes desta sessão)

```text
latexmk   AUSENTE
pdflatex  AUSENTE
xelatex   AUSENTE
tectonic  AUSENTE
mmdc      AUSENTE
```

Sem `sudo` disponível (ambiente Flatpak), a instalação do sistema era impossível.

### 3.2 Instalação executada em nível de usuário

Instalado **TinyTeX** (TeX Live) em `~/.TinyTeX`, sem privilégios de administrador:

```bash
wget -qO- "https://yihui.org/tinytex/install-bin-unix.sh" | sh
tlmgr install abntex2 newfloat memoir booktabs multirow listings microtype \
    babel-portugues latexmk xcolor pgf caption enumitem \
    xpatch l3packages l3kernel needspace oberdiek iftex \
    lastpage geometry colortbl fancyvrb float placeins \
    hyphen-portuguese
fmtutil-sys --byfmt pdflatex        # pre-carrega os padrões de hifenização pt
```

Para usar na sessão de terminal:

```bash
export PATH="$HOME/.TinyTeX/bin/x86_64-linux:$PATH"
```

### 3.3 Resultado da compilação — SUCESSO

```bash
latexmk -pdf -interaction=nonstopmode main.tex
# exit code: 0
```

| Verificação | Resultado |
|---|---|
| Erros de LaTeX | **0** |
| Páginas do PDF | **83** |
| Overfull hbox | **0** |
| Underfull hbox | **0** |
| Citações não definidas | **0** |
| Referências cruzadas não definidas | **0** |
| Figuras na lista de ilustrações | 12 |
| Quadros na lista de quadros | 21 |
| Tabelas na lista de tabelas | 11 |
| Entradas na bibliografia | 32 (de 41 no `.bib`) |
| Tamanho do PDF | 793\,934 bytes |

O entregável `monografia/main.pdf` **existe e foi gerado por compilador**.

### 3.4 Defeitos reais encontrados pela compilação (e corrigidos)

A compilação revelou três defeitos que nenhuma validação estática havia detectado:

| ID | Defeito | Correção |
|---|---|---|
| D-01 | `[VALIDAR COM O AUTOR ...]` no início de linha após `\\` era interpretado como argumento opcional de espaço vertical, causando `Missing number` e `Illegal unit of measure` | Envolver em `\mbox{...}` (4 ocorrências em `pre-textuais.tex`) |
| D-02 | `siglas` e `simbolos` do `abntex2` são ambientes de **lista** (`\item[...]`), não tabelas: o uso de `&` produzia `Misplaced alignment tab character &` e `missing \item` | Convertidos para `\item[...]` |
| D-03 | `\item[$\mathrm{E}[T]$]` — o `]` interno fechava o argumento opcional prematuramente | Reescrito como `$\mathrm{E}{[}T{]}$` |

**Lição de processo:** validação estática de `\input`, `\ref`/`\label` e balanceamento de ambientes
é necessária mas **insuficiente**. Os três defeitos eram de semântica de macro, invisíveis sem
compilador.

### 3.5 Avisos remanescentes (benignos, documentados)

| Aviso | Origem | Impacto |
|---|---|---|
| `Name 'brazil' is deprecated` | `abntex2.cls` linha 138: `\RequirePackage[brazil]{babel}` — **codificado na classe**, não no projeto | Nenhum. Não corrigível sem alterar a classe |
| `Token not allowed in a PDF string (Unicode): removing '\uppercase'` | `abntex2` aplica `\uppercase` aos títulos de capítulo e o `hyperref` o remove ao gerar o marcador do PDF | Nenhum no texto; apenas o marcador do PDF perde o invólucro |

### 3.6 O que NÃO foi verificado

- **Inspeção visual do PDF.** Não há visualizador nem ferramenta de inspeção no ambiente
  (`pdfinfo`, `pdftotext`, `pdftoppm`, `gs` e `qpdf` estão ausentes e não são instaláveis sem
  privilégios). O PDF foi validado por contagem de páginas, listas geradas e ausência de erros e
  avisos de referência — **não** por leitura visual.
- **Layout dos 11 diagramas TikZ.** Eles **compilam sem erro**, mas a disposição visual
  (sobreposição de setas, posicionamento de rótulos) não foi inspecionada. Ver P-33.
- **Renderização das fontes no PDF final** e paginação real de cada seção.
- **Sintaxe dos 11 diagramas Mermaid** — o `mmdc` continua ausente e não foi instalado.


## 4. Próximo passo recomendado

1. **Inspecionar visualmente `monografia/main.pdf`** (83 páginas) em um leitor de PDF — o ambiente
   não possui ferramenta de inspeção. Verificar sobretudo o layout dos 11 diagramas TikZ.
2. Preencher os metadados institucionais em `MONOGRAFIA_PENDENCIAS.md` (bloqueia capa e folha de aprovação).
3. Verificar as referências do `references.bib` contra as fontes originais (P-16 a P-21).
4. Comparar o `main.tex` com o manual de normalização institucional, que prevalece (P-07).
5. Opcional: aprofundar as seções listadas em `MONOGRAFIA_PLANO.md` § 6 — a meta de extensão já foi atingida.

## 5. Contagem atual (figuras e tabelas contadas; **páginas medidas por compilador**)

| Item | Atual | Meta da skill |
|---|---|---|
| Linhas de LaTeX (total) | 4\,228 | — |
| Capítulos textuais | 10 | — |
| Apêndices | 2 | — |
| Seções (`\section`) | 91 | — |
| Subseções | 5 | — |
| Figuras | 12 (11 diagramas TikZ + 1 captura de tela real) | — |
| Quadros | 21 | — |
| Tabelas | 11 (9 `table` + 2 `longtable`) | — |
| Equações numeradas | 6 | — |
| Listagens de código | 5 | — |
| Referências no `.bib` | 41 (32 efetivamente citadas) | — |
| Diagramas Mermaid | 11 | — |
| **Páginas** | **83 — medido pelo compilador** | 65–100 |

**A meta de extensão de 65 a 100 páginas foi atingida.** O valor de 83 páginas é medido, não
estimado: consta da linha `Output written on main.pdf (83 pages` do `main.log`.

O plano de expansão de `MONOGRAFIA_PLANO.md` deixa de ser necessário para atingir a meta e passa a
ser **opcional**, voltado a aprofundar as seções indicadas. As frentes que exigiam novas medições
(P-10, P-11, P-14) continuam bloqueadas por ausência de dado, não por extensão.


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
