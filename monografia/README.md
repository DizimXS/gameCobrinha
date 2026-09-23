# Monografia — projeto LaTeX

Monografia de graduação em Engenharia sobre **desenvolvimento de software orientado por
especificações (SDD) com agentes de inteligência artificial**, tendo o jogo `Snake Neon`
como estudo de caso.

## ⚠️ Status da compilação

**O PDF NÃO foi gerado.** Não há toolchain LaTeX instalada no ambiente em que o projeto foi
escrito (`latexmk`, `pdflatex`, `xelatex` e `tectonic` estão ausentes). O código LaTeX **não foi
validado por compilador** — erros de sintaxe ou de dependência podem existir.

Nenhum documento deste projeto afirma que houve compilação bem-sucedida.

## Arquitetura do projeto

```text
monografia/
├── main.tex                        # preâmbulo, metadados e inclusão dos capítulos
├── references.bib                  # referências (verificar contra as fontes — ver P-16…P-21)
├── chapters/
│   ├── pre-textuais.tex            # capa, folha de aprovação, resumo, abstract, listas
│   ├── 01-introducao.tex
│   ├── 02-fundamentacao.tex
│   ├── 03-trabalhos-relacionados.tex
│   ├── 04-materiais-metodos.tex
│   ├── 05-framework-sdd-agentico.tex
│   ├── 06-especificacao.tex
│   ├── 07-implementacao.tex
│   ├── 08-resultados.tex
│   ├── 09-discussao.tex
│   └── 10-conclusao.tex
├── appendices/
│   ├── a-especificacao.tex         # apêndice A — requisitos e critérios de aceitação
│   └── b-lista-verificacao.tex     # apêndice B — lista de verificação de encerramento
├── diagrams/                       # fontes Mermaid (.mmd) — legíveis no GitHub
├── figures/                        # figuras TikZ nativas (compilam no PDF)
├── tables/                         # quadros e tabelas extensas
├── MONOGRAFIA_STATUS.md            # estágio do pipeline e decisões
├── MONOGRAFIA_PLANO.md             # capítulos, metas e plano de expansão
├── MONOGRAFIA_EVIDENCIAS.md        # matriz de evidências
├── MONOGRAFIA_RASTREABILIDADE.md   # matrizes de rastreabilidade
└── MONOGRAFIA_PENDENCIAS.md        # informação ausente e itens a validar
```

## Como compilar

### Dependências

- TeX Live (recomendado: `texlive-full`) ou MiKTeX com instalação automática de pacotes.
- Pacotes principais: `abntex2`, `abntex2cite`, `booktabs`, `longtable`, `tabularx`, `tikz`,
  `listings`, `microtype`, `hyperref`.
- `bibtex` para as referências (o estilo ABNT vem do `abntex2cite`).

Em TeX Live mínimo, instale o essencial:

```bash
# Debian/Ubuntu — requer privilégios de administrador
sudo apt install texlive-full texlive-lang-portuguese
```

### Compilação

```bash
cd monografia
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

Caso o `latexmk` não esteja disponível, a sequência manual é:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

O `bibtex` é necessário porque o `abntex2cite` usa o fluxo BibTeX, e não o Biber. Não use
`biber` neste projeto.

### Limpeza dos auxiliares

```bash
latexmk -c
```

### Riscos conhecidos na primeira compilação

O código nunca foi compilado. As validações possíveis sem compilador passaram (todos os
`\input` resolvem, todo `\ref` tem `\label`, toda citação tem entrada no `.bib`, ambientes
balanceados). Os riscos restantes, com a ação corretiva de cada um, estão em
`MONOGRAFIA_PENDENCIAS.md`, seção 6 (itens P-30 a P-36). Os mais prováveis são o P-30 (o nome
da lista de quadros gerada pelo `newfloat` pode não ser `\listofquadro`) e o P-35 (canal alfa
da captura de tela).

## Diagramas

Cada diagrama tem duas representações, conforme a decisão D-05 de `MONOGRAFIA_STATUS.md`:

| Representação | Local | Função |
|---|---|---|
| **TikZ** | `figures/` | Compila dentro do PDF, sem dependência de ferramenta externa |
| **Mermaid** | `diagrams/*.mmd` | Fonte editável, renderizável no GitHub e em editores de Markdown |

Para renderizar os Mermaid em imagem (opcional):

```bash
npm install -g @mermaid-js/mermaid-cli
mmdc -i diagrams/dg-01-fluxo-agentico.mmd -o figures/dg-01-fluxo-agentico.png
```

O TikZ é a via recomendada, pois não exige Node.js nem navegador headless.

### Captura de tela

A Figura 12 (interface implementada) usa uma imagem real, não um diagrama: o arquivo
`images/screenshot_gaming.png`, na raiz do repositório. Ele **não é duplicado** dentro da
monografia — o preâmbulo declara

```latex
\graphicspath{{figures/}{../images/}{images/}}
```

Dois cuidados antes de compilar:

1. **Canal alfa (P-35).** O arquivo é PNG RGBA (color type 6). O `pdflatex` pode não tratar o
   canal alfa corretamente. Se a figura aparecer com fundo preto, converta para RGB:
   `magick images/screenshot_gaming.png -background black -flatten images/screenshot_gaming.rgb.png`
   e ajuste o nome no `\includegraphics`. Alternativa: compilar com `xelatex` ou `lualatex`.
2. **Caminho relativo (P-36).** Se você enviar apenas a pasta `monografia/` para o Overleaf, a
   imagem não será encontrada. Nesse caso, copie o arquivo para `monografia/figures/`.

## Ordem de leitura para continuidade

1. `MONOGRAFIA_STATUS.md` — onde o trabalho parou
2. `MONOGRAFIA_PLANO.md` — o que falta e em que ordem
3. `MONOGRAFIA_EVIDENCIAS.md` — o que sustenta cada afirmação
4. `MONOGRAFIA_RASTREABILIDADE.md` — como objetivos, métodos e resultados se conectam
5. `MONOGRAFIA_PENDENCIAS.md` — o que precisa de decisão humana

## Regras de integridade aplicadas

- Nenhum dado, medição, resultado ou referência foi inventado.
- Informação ausente aparece marcada como `[VALIDAR COM O AUTOR]`, `[INFORMAÇÃO AUSENTE]`
  ou `[DADO NECESSÁRIO]` e está registrada em `MONOGRAFIA_PENDENCIAS.md`.
- A curva de velocidade do Cap. 8 é apresentada como **modelo da especificação**, e os dois
  valores medidos são apresentados separadamente como **medições**, nunca misturados.
- A extensão está abaixo da meta de 65–100 páginas e isso está declarado, com plano de
  expansão acadêmica em `MONOGRAFIA_PLANO.md` § 6. Não foi adicionado texto de preenchimento.
