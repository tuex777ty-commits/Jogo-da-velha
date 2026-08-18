# Relatório de Prompts — Jogo da Velha UNIFOR

Aluno: Luis Gustavo
Matrícula: [preencher]
IA utilizada: Claude (Anthropic)

## Como usei a IA

Passei o enunciado da atividade praticamente igual ao que a professora/monitoria mandou, incluindo o link do CDU no GitHub do Prof. Bezerra. Pedi pra IA seguir o CDU como guia principal e montar o repositório já na estrutura exigida (docs/, src/, README.md, RELATORIO_PROMPTS.md). A IA foi lá, abriu o link do CDU, leu a especificação inteira e só depois começou a escrever o código — isso foi importante porque boa parte das regras (tempo de 400ms da CPU, 2 segundos de pausa entre rodadas, nomes exatos dos elementos de tela) só tavam no documento, não dava pra IA "chutar".

### Prompt 1 (o principal, que gerou a primeira versão)

"Construir a aplicação web do Jogo da Velha da UNIFOR utilizando como guia soberano a Especificação de Caso de Uso (CDU) disponibilizada [link]. Formato de entrega: link do repositório GitHub, com estrutura docs/, src/, README.md e RELATORIO_PROMPTS.md."

A partir disso a IA já entregou o `index.html` completo com HTML+CSS+JS num arquivo só, o CDU salvo em docs/, e o README com instruções de execução.

### Prompt 2 (correção de bug)

Depois de gerar o app, testei jogando uma partida no navegador e forcei uma vitória do jogador O na coluna do meio. A linha laranja que devia passar em cima dos "O" apareceu deslocada pra esquerda, cobrindo uma faixa vazia do tabuleiro em vez dos símbolos. Tirei um print e mandei pra IA: "quero que a linha fique em cima dos números".

A IA me explicou o motivo do erro: o cálculo da linha de vitória usava um deslocamento fixo de -17px no eixo X pra dar aquela "sobra" nas pontas da linha (pra ela passar um pouco além do centro da primeira e da última célula). Isso funcionava certo quando a vitória era numa linha horizontal, porque nesse caso o deslocamento coincidia com a própria direção da linha. Mas numa vitória em coluna (vertical), esse mesmo deslocamento em X só empurrava a linha inteira pro lado, já que a direção real da linha ali é no eixo Y, não no X.

A correção que a IA aplicou foi trocar aquele deslocamento fixo por um cálculo vetorial: agora ela pega a direção real entre o centro da primeira e da última célula vencedora (funciona pra linha, coluna e diagonal) e estende a "sobra" das pontas nessa direção, não mais fixo em X. Testei de novo depois do ajuste e a linha ficou certinha em cima dos símbolos, tanto em coluna quanto testei depois em diagonal.

### Prompt 3

Pedi pra deixar a árvore de diretórios do README com as linhas retas (mesmo estilo `├──`/`└──` que já tava sendo usado pras outras entradas), porque o jeito anterior tava com uma ramificação aninhada (`│   └──`) que ficava com visual diferente do resto. Foi só ajuste de formatação no Markdown, sem mudança de código.

## Erros que a IA cometeu e como corrigi

| O que aconteceu | Onde eu percebi | Como pedi pra corrigir |
|---|---|---|
| Linha de vitória desalinhada em vitórias verticais (deslocamento fixo em X aplicado igual pra qualquer ângulo) | Testando manualmente uma vitória de coluna e comparando com o print | Mandei o print e descrevi o problema ("quero que a linha fique em cima dos números"); a IA identificou a causa raiz e reescreveu o cálculo usando vetor de direção |
| Árvore de diretórios do README com estilo inconsistente (aninhada em vez de reta) | Comparação visual direta com o resto do arquivo | Pedi pra deixar igual às outras linhas |

## Testes que fiz manualmente antes de considerar pronto

- Vitória em linha horizontal (topo, meio, base) — ok
- Vitória em coluna (depois do fix) — ok, linha em cima dos símbolos
- Vitória em diagonal — ok
- Empate (9 células preenchidas sem vencedor) — mensagem "Rodada Empatada!" aparece e some o clique nas células
- Trocar pra "Contra o Computador" no meio de uma partida — zera o placar e a CPU já joga sozinha na vez do O
- Melhor de 3 — o placar acumula entre rodadas e o jogo só declara campeão com 2 vitórias ou depois da 3ª rodada
- Clicar em "Reiniciar Jogo" no meio de uma rodada — zera tudo e volta pro X

## Autoavaliação dos critérios de aceite

| Critério | Atendido? | Observação |
|---|---|---|
| CA-01 — Paleta institucional + subtítulo UNIVERSIDADE DE FORTALEZA | Sim | Cores `#003366` e `#0056b3` usadas no cabeçalho e nos elementos de destaque |
| CA-02 — Não sobrescreve célula ocupada | Sim | Testei clicando várias vezes na mesma célula |
| CA-03 — Bloqueia cliques depois de fim de rodada | Sim | Testei clicar rápido depois de uma vitória, não registra jogada |
| CA-04 — CPU joga sozinha na vez do O | Sim | Tem uma pausa perceptível antes da jogada da CPU |
| CA-05 — Regra do Melhor de 3 | Sim | Testei até o fim de uma partida completa |
| CA-06 — Linha de vitória em cima das células + confetes | Sim (depois da correção) | Ver seção de erros corrigidos acima |
| CA-07 — Áudio sem arquivo externo | Sim | Não tem nenhum `<audio>` nem link de mp3 no código, os sons são gerados na hora |
