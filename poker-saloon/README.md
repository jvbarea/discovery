# Poker Saloon

**Golden Spur Hold'em**: poker Texas Hold'em No-Limit em 3D, em primeira pessoa, numa mesa de saloon de 1899, com
clima inspirado em Red Dead Redemption 2. Um Sit & Go de cinco lugares contra quatro oponentes com personalidade, num
único `index.html` de WebGL2 e JavaScript escritos do zero: sem bibliotecas nem arquivos externos. Texturas, modelos,
personagens e sons são gerados em código; só as fontes vêm do Google Fonts, e sem internet o jogo usa Georgia e Arial
Narrow.

**Jogar:** [jvbarea.github.io/discovery/poker-saloon](https://jvbarea.github.io/discovery/poker-saloon/) ou dê duplo clique
em `index.html` (Chrome, Edge ou Firefox, com WebGL2).

O jogo foi construído a partir da especificação em slides que está na mesma pasta:
[especificacao.html](https://jvbarea.github.io/discovery/poker-saloon/especificacao.html).

## O jogo

Feito fase a fase (M0 a M7, slide ♣6), cada uma conferida contra os critérios de aceite antes da seguinte. As escolhas
que a especificação deixou em aberto estão registradas como `// DECISÃO:` no topo do `index.html`.

| Fase | O que entrou |
| --- | --- |
| M0 · Esqueleto | WebGL2, câmera em primeira pessoa, mesa com feltro e aro, lampião, ACES; `?guide=1` confere o enquadramento |
| M1 · Motor de poker | avaliador de 7 cartas, No-Limit com side pots, estados e eventos; `?test=1` com 20.000 mãos simuladas |
| M2 · Cartas e fichas | versos da Ref A, frentes com figuras em gravura, 7 fichas da Ref B, botão da Ref C, instancing, sombra PCSS e decalques de contato |
| M3 · Jogável | Sit & Go inteiro do título ao fim; um Diretor anima cada evento do motor; HUD no estilo da Ref D; teclado, mouse e controle; pausa, opções e tela de fim |
| M4 · IA | equidade por Monte Carlo, quatro personas calibradas (VPIP e PFR medidos em 500 mãos), um oponente que se adapta a você e 140 falas em legenda |
| M5 · Ambiente e luz | salão com mezanino como no fundo da Ref D, estojo da Ref C, arandelas; bloom, profundidade de campo, gradação e grão; poeira e fumaça; qualidade Baixa, Média, Alta e Auto |
| M6 · Personagens | os cinco esculpidos em SDF e malhados por Surface Nets na carga, esqueleto rígido com IK, respiração, olhar e gestos que seguem fichas e cartas; tells; o Coronel bebe e fuma; retratos em sépia na HUD; as suas mãos segurando as cartas |
| M7 · Áudio e acabamento | som todo sintetizado em WebAudio (fichas, cartas, embaralhar, murmúrio do salão e um ragtime procedural ao piano); volumes, sensibilidade e inversão do olhar, vozes opcionais e toque para olhar; passe de desempenho; testado no Chrome, no Edge e no Firefox |

Controles: <kbd>C</kbd> passa ou paga, <kbd>R</kbd> aposta ou aumenta, <kbd>F</kbd> desiste (segure 0,5 s quando passar
é de graça), <kbd>↑</kbd>/<kbd>↓</kbd> ou a roda mudam o valor (<kbd>Shift</kbd> 5 BB, <kbd>Ctrl</kbd> 5¢), <kbd>1</kbd>–<kbd>4</kbd>
são ½ pote, ¾ pote, pote e all-in; segure <kbd>Q</kbd> para ver as suas cartas, <kbd>E</kbd> para o board, <kbd>H</kbd>
para o ranking e <kbd>Espaço</kbd> para acelerar; botão direito e arrastar olha em volta; <kbd>Esc</kbd> pausa e
<kbd>M</kbd> silencia. O controle (Gamepad API) e o toque também funcionam.

Parâmetros de URL: `?seed=123` (partida reproduzível), `?quality=low|med|high` (fixa a qualidade; o padrão é Auto),
`?fast=1` (animações 4×), `?autoplay=1` (a IA joga no seu lugar e emenda partidas), `?test=1` (bateria de testes),
`?guide=1` (linhas de enquadramento com medição), `?gallery=1` (texturas geradas) e `?debug=1` (fps, tempo de GPU por
passe, cartas das IAs e log de decisões).

## A especificação

1. Navegue com ← e →, ou troque para o modo **Documento** para rolar tudo. **Índice** (tecla G) lista os 35 slides.
2. **Copiar prompt** transforma a especificação inteira em Markdown, com o prompt de abertura no topo.
3. Para construir de novo: cole num chat com o Claude e anexe as quatro imagens de referência (Ref A, verso das cartas;
   Ref B, fichas; Ref C, estojo de nogueira; Ref D, mesa do RDR2).

O documento cobre ♠ direção (visão, restrições e prioridades), ♥ arte e cena (paleta, mesa, câmera, luz, cartas, fichas,
estojo, elenco e áudio), ♦ jogo (regras, avaliador, IA, coreografia, HUD e controles) e ♣ engenharia (arquitetura,
renderizador, personagens em SDF, orçamento de desempenho, testes e fases).

## Limitações

- Os personagens são estilizados, esculpidos em código; de perto (tecla Q), as suas mãos ainda parecem luvas.
- As vozes usam a síntese de fala do sistema: sem uma voz em português instalada, a opção fica muda e restam as legendas.
- As imagens de referência não estão no repositório porque são de terceiros; o documento as descreve.
- A primeira carga compila os shaders e esculpe os personagens: leva alguns segundos, mais num computador ocupado.
