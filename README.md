# 🎮 Jogo da Velha — UNIFOR

Aplicação web do Jogo da Velha desenvolvida como atividade prática de **Spec-Driven Development** (Desenvolvimento Guiado por Requisitos), a partir do Caso de Uso (CDU) *"Jogar Jogo da Velha"* fornecido pela disciplina de Engenharia de Software / Desenvolvimento Web da Universidade de Fortaleza (UNIFOR).

**Aluno:** Luis Gustavo
**Matrícula:** _[preencher com sua matrícula]_
**Instituição:** Universidade de Fortaleza (UNIFOR)

---

## 🔗 Link da Aplicação (GitHub Pages)

> _(https://tuex777ty-commits.github.io/Jogo-da-velha/)_
>

---

## 📁 Estrutura do Repositório

```
jogo-da-velha-unifor/
├── docs/cdu_JogarJogodavelha.md   # Especificação (CDU) fornecida pela disciplina
├── src/index.html                 # Aplicação completa (HTML + CSS + JS)
├── README.md                      # Este arquivo
└── RELATORIO_PROMPTS.md           # Relatório de interação com a IA
```

---

## ▶️ Como Executar

### Opção 1 — Localmente
1. Clone o repositório:
   ```bash
   git clone https://github.com/<seu-usuario>/jogo-da-velha-unifor.git
   cd jogo-da-velha-unifor
   ```
2. Abra o arquivo `src/index.html` diretamente no navegador (duplo clique ou arraste para a janela do navegador).
   - Não é necessário servidor, build ou instalação de dependências — o app é 100% autocontido.

### Opção 2 — GitHub Pages (online)
Acesse o link publicado na seção [🔗 Link da Aplicação](#-link-da-aplicação-github-pages) acima.

---

## 🕹️ Funcionalidades

- **Modo de Jogo:** 2 Jogadores (PVP) ou Contra o Computador (CPU com heurística de vitória/bloqueio).
- **Formato da Partida:** Partida Única ou Melhor de 3 (MD3), com transição automática entre rodadas.
- **Placar persistente** durante a sessão, com pontuação por jogador/CPU.
- **Linha de vitória animada**, calculada dinamicamente sobre as 3 células vencedoras.
- **Confetes** disparados via `<canvas>` ao final de cada rodada vencida.
- **Efeitos sonoros 100% sintetizados** via Web Audio API — nenhum arquivo de áudio externo.
- **Identidade visual institucional** da UNIFOR (Azul `#003366`, Azul Destaque `#0056b3`, Laranja `#d97706`, Fundo `#f4f6f9`).

---

## 📄 Especificação de Origem

O desenvolvimento seguiu integralmente o Caso de Uso disponível em [`docs/cdu_JogarJogodavelha.md`](./docs/cdu_JogarJogodavelha.md), incluindo fluxo principal, fluxos alternativos (A1, A2, A3), fluxo de exceção (E1), matriz de rastreabilidade e critérios de aceite (CA-01 a CA-07).

Veja o relatório completo do processo de engenharia de prompts e a autoavaliação dos critérios de aceite em [`RELATORIO_PROMPTS.md`](./RELATORIO_PROMPTS.md).
