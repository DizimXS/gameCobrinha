# Monografia — projeto LaTeX

Monografia de graduação em Engenharia sobre **desenvolvimento de software orientado por
especificações (SDD) com agentes de inteligência artificial**, tendo o jogo `Snake Neon`
como estudo de caso.

## ✅ Status da compilação

**O PDF está gerado: `monografia/main.pdf`, com 83 páginas.**

```text
latexmk -pdf -interaction=nonstopmode main.tex
exit code: 0
erros: 0
overfull/underfull hbox: 0
referências e citações não definidas: 0
Output written on main.pdf (83 pages)
```

A toolchain foi instalada em nível de usuário (**TinyTeX** em `~/.TinyTeX`), sem privilégios de
administrador. Ver `MONOGRAFIA_STATUS.md` § 3 para o registro completo, incluindo os três defeitos
que a compilação revelou e que foram corrigidos.

**Não verificado:** o layout dos 11 diagramas TikZ e a composição final página a página. Não há
visualizador nem ferramenta de inspeção neste ambiente (`pdfinfo`, `pdftotext`, `pdftoppm`, `gs` e
`qpdf` ausentes). Abra o PDF e confira os diagramas.

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

### Toolchain já instalada neste ambiente

O **TinyTeX** (TeX Live) está instalado em `~/.TinyTeX`, em nível de usuário, sem `sudo`.
Para usar em uma sessão de terminal:

```bash
export PATH="$HOME/.TinyTeX/bin/x86_64-linux:$PATH"
```

Para tornar permanente, acrescente a linha acima ao final de `~/.bashrc`.

### Pacotes instalados

```bash
tlmgr install abntex2 newfloat memoir booktabs multirow listings microtype \
    babel-portugues latexmk xcolor pgf caption enumitem \
    xpatch l3packages l3kernel needspace oberdiek iftex \
    lastpage geometry colortbl fancyvrb float placeins \
    hyphen-portuguese
fmtutil-sys --byfmt pdflatex   # pre-carrega os padrões de hifenização portugueses
```

O comando `fmtutil-sys` é seguro aqui porque a árvore do TinyTeX pertence ao próprio usuário.
Sem ele, o `babel` emite o aviso *"No hyphenation patterns were preloaded for the language
'Portuguese'"* e usa padrões ingleses para hifenizar.

### Em outra máquina

- TeX Live completo (`texlive-full` + `texlive-lang-portuguese`) ou MiKTeX com instalação
  automática de pacotes resolve tudo.
- `bibtex` é obrigatório: o estilo ABNT vem do `abntex2cite`, que usa o fluxo BibTeX. **Não use
  `biber`** neste projeto.

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

### Riscos conhecidos

A compilação já foi executada com sucesso, então os riscos previstos foram resolvidos ou
descartados. O status de cada um está em `MONOGRAFIA_PENDENCIAS.md`, seção 6:

- **Resolvidos:** P-30 (nome da lista de quadros), P-35 (canal alfa da captura de tela).
- **Não se confirmaram:** P-31, P-32, P-34.
- **Ainda válido:** P-36 (caminho relativo da imagem, quebra em envio isolado ao Overleaf).
- **Parcial:** P-33 (os diagramas TikZ compilam, mas o layout visual não foi conferido — ver P-37).

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

1. **Canal alfa (P-35) — resolvido.** O arquivo é PNG RGBA (color type 6) e o `pdflatex`
   **incluiu a imagem corretamente** (registro `<use ../images/screenshot_gaming.png>` no
   `main.log`, página 59). Nenhuma conversão foi necessária. Se você mudar para outra captura de
   tela com transparência, o procedimento de conversão seria
   `magick imagem.png -background black -flatten imagem.rgb.png`.
2. **Caminho relativo (P-36) — ainda válido.** Se você enviar apenas a pasta `monografia/` para o
   Overleaf, a imagem não será encontrada. Nesse caso, copie o arquivo para `monografia/figures/`.

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
