---
title: "Lição 1 - Criar o quiz pelo terminal"
description: "Peça ao Copilot CLI o Space Quiz completo em uma solicitação detalhada e faça uma melhoria com escopo bem definido."
authors:
  - jamesmontemagno
lastUpdated: 2026-09-28
---

Peça o projeto completo em uma solicitação detalhada, revise o que o agente propõe antes de aprovar e, depois, faça uma pequena alteração com escopo definido.

Nesta lição, você vai:

- criar um quiz sem dependências em `index.html`.
- abrir o quiz em um navegador a partir da sessão.
- fazer uma alteração com escopo bem definido e verificá-la.

## Criar o quiz

Envie o seguinte prompt na sessão do Copilot CLI:

```plaintext
Crie um quiz de exploração espacial com 10 perguntas, uma barra de progresso, um contador de pontos e feedback colorido e animado (verde para respostas corretas e tremor vermelho para respostas incorretas). Mostre uma tela de resultados com uma reação em emoji ao final. Use apenas um arquivo index.html, sem servidor nem dependências. Garanta acessibilidade, navegação pelo teclado e respeito a prefers-color-scheme.
```

1. Leia o plano proposto e aprove as alterações nos arquivos.
2. Abra `index.html` em um navegador e responda a algumas perguntas. Para abri-lo a partir da sessão, execute `!open index.html` no macOS, `!start index.html` no Windows ou `!xdg-open index.html` no Linux.

## Fazer uma pequena alteração e verificá-la

Peça uma melhoria com escopo bem definido para observar como uma solicitação delimitada se comporta:

```plaintext
A tela de resultados parece sem vida. Dê mais impacto à chegada: anime a contagem crescente da pontuação e aumente a reação em emoji. Não altere mais nada.
```

1. Recarregue a página no navegador e jogue até o fim.
2. Observe que nada está no Git ainda, então não há uma versão de referência para comparar o diff. Isso muda depois que você publica o projeto.

## Resumo e próximos passos

Você criou o quiz e o aprimorou com uma solicitação de escopo definido. Continue com a [Lição 2: Registrar instruções do projeto][next-lesson].

[next-lesson]: ../2-project-instructions/
