# 📋 Relatório de Interação com IA — Spec-Driven Development

**Atividade:** Jogo da Velha Web — UNIFOR
**Disciplina:** Engenharia de Software / Desenvolvimento Web
**Ferramenta de IA utilizada:** Claude (Anthropic) — via Claude.ai

---

## 1. Papel do Aluno no Processo

Nesta atividade, atuei como **Engenheiro de Prompts e Auditor de Qualidade**: instruí a IA a partir do CDU oficial (`docs/cdu_JogarJogodavelha.md`) e sou responsável por validar, testar e garantir que a aplicação final atenda a 100% dos requisitos e critérios de aceite descritos no artefato.

> ⚠️ **Nota de responsabilidade:** este relatório documenta o prompt principal enviado à IA e a análise de conformidade realizada com base na leitura do código gerado. Antes da entrega, é essencial que eu **abra `src/index.html` no navegador e teste manualmente cada critério de aceite (CA-01 a CA-07)**, registrando abaixo quaisquer ajustes que eu próprio tenha solicitado após esse teste.

---

## 2. Prompt Principal Utilizado

```
Construir a aplicação web do Jogo da Velha da UNIFOR utilizando como guia
soberano a Especificação de Caso de Uso (CDU) disponível em:
https://github.com/ProfBezerra/LAPIS/blob/main/Requisitos/Artefatos/Exemplos/cdu_JogarJogodavelha.md

Requisitos de entrega:
- Repositório com estrutura docs/, src/, README.md, RELATORIO_PROMPTS.md
- src/index.html: HTML + CSS + JS em arquivo único, sem dependências externas
- Seguir fielmente fluxo principal, fluxos alternativos (A1, A2, A3) e
  fluxo de exceção (E1) do CDU
- Atender aos critérios de aceite CA-01 a CA-07
```

A IA foi instruída a buscar o conteúdo integral do CDU na URL fornecida antes de escrever qualquer código, para garantir fidelidade à especificação (nomes de variáveis do dicionário de dados, paleta de cores institucional, textos exatos de UI, tempos de espera de 400ms para a CPU e 2s para transição de rodada, etc.).

---

## 3. Estratégia de Auditoria Aplicada

Ao invés de aceitar o código gerado sem revisão, conferi cada trecho do `src/index.html` contra a seção correspondente do CDU:

| Seção do CDU auditada | O que foi verificado no código |
| --- | --- |
| Fluxo Principal (P1–P7) | Sequência exata: clique → preenche célula → toca som → avalia tabuleiro → alterna turno → atualiza status |
| A1 (Vitória) | Linha traçada sobre as 3 células, confetes, acorde sonoro, incremento de placar, regras de MD3 |
| A2 (CPU) | Bloqueio de cliques, atraso de 400ms, jogada em posição vazia |
| A3 (Reinício) | Zeragem de placar/rodadas ao clicar em "Reiniciar" **ou** ao trocar qualquer seletor |
| E1 (Empate) | Som descendente, mensagem "Rodada Empatada!", reinício sem incrementar rodada em MD3 |
| Dicionário de Dados (seção 18) | Nomes de variáveis (`options`, `currentPlayer`, `running`, `winsX`, `winsO`, `currentRound`, `modeSelect`, `formatSelect`) usados literalmente no JS |
| Interface Visual (UI-01 a UI-11) | Todos os 11 elementos presentes e com o comportamento descrito na tabela |

---

## 4. Pontos de Atenção Identificados na Revisão

Durante a auditoria do código gerado contra o CDU, os seguintes pontos exigiram atenção redobrada (e foram corrigidos/confirmados na versão final):

1. **Cálculo geométrico da linha de vitória (UI-10):** o CDU não especifica a fórmula exata; foi necessário garantir, via `getBoundingClientRect()`, que a linha cobrisse precisamente o centro das 3 células vencedoras em qualquer tamanho de tela — validar visualmente em resoluções diferentes (CA-06).
2. **Regra do MD3 combinada com empate (E1.4):** o CDU determina que, em caso de empate no formato MD3 com rodada < 3, o placar de rodada **não** é incrementado — isso foi implementado separadamente da lógica de vitória (A1.7.2, que incrementa) para não confundir os dois fluxos.
3. **Nomenclatura do Jogador O em modo CPU:** o CDU (UI-03) menciona que o rótulo do "Jogador O" deve mudar ao selecionar o modo CPU — implementado trocando dinamicamente o texto para "Computador" no placar e nas mensagens de status.
4. **Autonomia total de áudio (CA-07):** confirmado que nenhum `<audio>`, arquivo `.mp3/.wav` ou CDN de som é referenciado — toda a sintetização usa `OscillatorNode`/`GainNode` nativos do navegador.

**➡️ Espaço para o aluno preencher após teste manual:**
_(Registre aqui quaisquer bugs encontrados ao testar no navegador e como foram corrigidos — ex.: "a linha de vitória ficou desalinhada em tela muito estreita, corrigi solicitando à IA recalcular a margem da linha.")_

- ...

---

## 5. Tabela de Autoavaliação — Critérios de Aceite (CA-01 a CA-07)

| Critério | Descrição | Status | Evidência no Código |
| --- | --- | :---: | --- |
| **CA-01** | Paleta institucional UNIFOR (`#003366`, `#0056b3`) + subtítulo "UNIVERSIDADE DE FORTALEZA" | ✅ | Variáveis CSS `--azul-unifor`, `--azul-destaque`; elemento `.subtitulo-institucional` |
| **CA-02** | Impossível sobrescrever célula já preenchida | ✅ | `if (options[index] !== '') return;` em `onCelulaClicada` |
| **CA-03** | Tabuleiro bloqueia cliques após fim de rodada até próxima rodada/reinício | ✅ | `running = false` em `tratarFimDeRodadaPorVitoria` e `tratarFimDeRodadaPorEmpate`, verificado em `onCelulaClicada` |
| **CA-04** | CPU joga automaticamente na vez do 'O' após pausa | ✅ | `jogadaDaCpu()` com `setTimeout(..., 400)` disparado em P6/P7 |
| **CA-05** | MD3 zera tabuleiro entre rodadas; encerra com 2 vitórias ou após a 3ª rodada | ✅ | `tratarFimDeRodadaPorVitoria` → bloco `if (formatSelect === 'bo3')` com checagem `winsX >= 2 \|\| winsO >= 2` e `currentRound < 3` |
| **CA-06** | Linha traçada exatamente sobre as 3 células vitoriosas + confetes disparados | ✅ | `tracarLinhaVitoria()` via `getBoundingClientRect()`; `dispararConfetes()` via `<canvas>` |
| **CA-07** | Efeitos sonoros sem downloads/arquivos externos | ✅ | Todas as funções de som (`tocarTom`, `somJogadaX`, `somJogadaO`, `somVitoria`, `somEmpate`) usam exclusivamente `AudioContext` nativo |

**Legenda:** ✅ Atendido | ⚠️ Atendido parcialmente | ❌ Não atendido

> Recomenda-se que, antes da entrega final, cada linha desta tabela seja reconfirmada manualmente no navegador (clicar em cada célula, forçar vitórias em cada modo/formato, forçar empates, testar o botão Reiniciar e a troca de seletores em pleno jogo) e que o status seja atualizado caso algum comportamento divirja do esperado.

---

## 6. Conclusão

O processo de Spec-Driven Development permitiu traduzir diretamente os passos numerados do CDU (P1–P7, A1–A3, E1) em funções JavaScript nomeadas de forma rastreável (`tratarFimDeRodadaPorVitoria`, `tratarFimDeRodadaPorEmpate`, `jogadaDaCpu`, `reiniciarPartida`), facilitando a auditoria de conformidade linha a linha entre requisito e implementação.
