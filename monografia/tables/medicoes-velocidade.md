# Medições do intervalo entre passos

> Dados brutos e derivados da medição de \texttt{CA-012}.
> Corresponde à Tabela 8.3 e à Figura 10 da monografia.

## Procedimento

Agente de teste autônomo em navegador com motor Chromium. A cada mudança de posição
da cabeça do jogador, registrou-se um par (instante, pontuação corrente). A partida
foi conduzida até a pontuação de 100 pontos, sem encerramento por colisão.

## Resultado principal

| Faixa de pontuação | Média medida | Previsto (mín.) | Previsto (máx.) | Coerente |
|---|---|---|---|---|
| Abaixo de 40 pontos | 133 ms | 128 ms | 140 ms | Sim |
| 80 pontos ou mais | 105 ms | 100 ms | 108 ms | Sim |

- Movimentos amostrados (total): **184**
- Diferença entre as faixas: **28 ms** medidos, 28 ms previstos
- Pontuação final alcançada: **100 pontos**

Previsto calculado por $t(s) = \max(68,\; 140 - 2s/5)$ nos extremos de cada faixa.

## O que NÃO foi registrado

- Número de amostras por faixa (apenas o total de 184 movimentos).
- Desvio padrão por faixa.
- Número de execuções independentes (houve **uma única** execução na medição final).

Esses itens estão registrados como `[DADO NECESSÁRIO]` em `MONOGRAFIA_PENDENCIAS.md` (P-11).

## Observação de execução preliminar (configuração diferente)

Numa execução preliminar, com metodologia distinta (intervalos individuais medidos com
ciclo de observação de aproximadamente 40 ms, em vez de médias por faixa), obteve-se
mediana de 152 ms para a faixa de pontuação inferior a 40, com mínimo de 100 ms e máximo
de 198 ms, sobre 49 intervalos.

**Esta observação não é comparável ao resultado principal**, porque a configuração
experimental era diferente. Ela é registrada apenas como consistência qualitativa:
em ambas as configurações, o intervalo observado na faixa baixa foi superior ao
previsto de 140 ms, o que é coerente com o viés positivo do ciclo de observação
descrito na Seção 4.7 da monografia.

## Rastreabilidade

- Requisito: `FR-015`
- Critério: `CA-012`
- Tarefa: `T-014`
- Evidência: `E-22`
- Registro bruto: `docs/05-testing/snake-game-evidence.md`
