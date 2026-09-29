# Poker Saloon

Especificação em slides de **Golden Spur Hold'em**, um poker Texas Hold'em No-Limit em 3D, em primeira pessoa,
numa mesa de saloon de 1899, com clima inspirado em Red Dead Redemption 2. O documento foi escrito para outro chat
com o Claude Opus implementar o jogo do zero: um único `index.html`, WebGL2 puro, sem bibliotecas.

**Abrir:** [jvbarea.github.io/discovery/poker-saloon/especificacao.html](https://jvbarea.github.io/discovery/poker-saloon/especificacao.html)
ou dê duplo clique em `especificacao.html`.

## Como usar

1. Navegue com ← e →, ou troque para o modo **Documento** para rolar tudo. **Índice** (tecla G) lista os 35 slides.
2. Clique em **Copiar prompt**: a especificação inteira vira Markdown, com o prompt de abertura no topo.
3. Cole num chat novo com o Claude Opus e anexe as quatro imagens de referência com os nomes Ref A (verso das cartas),
   Ref B (fichas), Ref C (estojo de nogueira) e Ref D (mesa do RDR2). No Claude Code, basta pedir para ler este arquivo.
4. Quando o jogo existir, ele entra nesta pasta como `index.html`.

## O que tem dentro

- **♠ Direção:** visão, restrições (um arquivo, zero bibliotecas, tudo procedural) e ordem de prioridades.
- **♥ Arte e cena:** paleta amostrada das referências, planta da mesa, câmera, luz, cartas, fichas, estojo, elenco e áudio.
- **♦ Jogo:** regras No-Limit com side pots, avaliador de 7 cartas, IA com quatro personalidades, coreografia, HUD e controles.
- **♣ Engenharia:** arquitetura do arquivo, renderizador, personagens em SDF, orçamento de desempenho, testes e fases M0 a M7.

## Limitações

- As imagens de referência não estão no repositório porque são de terceiros; o documento as descreve e pede que sejam anexadas.
- As ilustrações dos slides são desenhadas em código e servem de alvo visual, não de asset do jogo.
- Com Ctrl+P no navegador, cada slide vira uma página, para salvar em PDF.
