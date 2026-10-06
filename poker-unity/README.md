# Poker Unity

Documentação da versão Unity do **Golden Spur Hold'em**: poker Texas Hold'em em primeira pessoa num saloon de 1899,
com personagens realistas, feito primeiro para o Galaxy S24+ a 30 quadros por segundo.

Esta pasta guarda só documentos. O projeto Unity fica em `C:\dev\golden-spur`, fora do OneDrive e fora deste
repositório público (motivos em [DECISOES.md](DECISOES.md), D10). A versão web feita do zero continua como
experimento em [`../poker-saloon`](../poker-saloon/).

Desde 05/10/2026 o projeto está congelado no estado atual, jogável no S24+ (D22): era o teste de Android, e passou.

## Documentos

| Documento | Estado | Para quê |
| --- | --- | --- |
| [GUIA-DE-INSTALACAO.md](GUIA-DE-INSTALACAO.md) | pronto | preparar o notebook e o S24+: Unity, módulo Android, depuração USB, plugin do Claude |
| [DECISOES.md](DECISOES.md) | pronto, D1 a D22 até 05/10/2026 | o que foi decidido e por quê |
| [GUIA-DE-USO.md](GUIA-DE-USO.md) | pronto, 01/10/2026 | como trabalhar com o Claude no projeto: pedir mudanças, testar no PC e no celular, versionar, trabalho noturno |
| [ESPECIFICACAO-DOS-TESTES.md](ESPECIFICACAO-DOS-TESTES.md) | pronto, 01/10/2026 | testes em quatro camadas: regras e IA, editor, celular e visual |
| `ESPECIFICACAO.md` | não será feita (D22) | especificação completa do jogo na versão Unity |

## Como começar o chat de implementação

1. Siga o [guia de instalação](GUIA-DE-INSTALACAO.md) até o fim.
2. Abra o VS Code em `C:\dev\golden-spur`, inicie o Claude Code e mande como primeira mensagem:

   ```text
   Leia C:\Users\joaob\OneDrive\Documentos\2026\projetos\discovery\poker-unity\README.md e os documentos
   que ele indica. Depois crie o CLAUDE.md deste projeto com as decisões que valem para o código e o caminho
   dessa pasta de documentação, para os próximos chats começarem sabendo o contexto.
   ```

3. O `CLAUDE.md` do projeto é o que passa o contexto adiante: todo chat aberto em `C:\dev\golden-spur` lê esse
   arquivo sozinho. Quando uma decisão mudar, ela entra primeiro em [DECISOES.md](DECISOES.md) e depois no `CLAUDE.md`.

## Material de base

- [`../poker-saloon/especificacao.html`](../poker-saloon/especificacao.html): regras, direção de arte, planta da mesa,
  personagens e HUD. Continua valendo, com as mudanças de [DECISOES.md](DECISOES.md): Unity, celular primeiro, jogo em inglês.
- `../poker-saloon/refs/`: as quatro imagens de referência (verso das cartas, fichas, estojo, mesa do RDR2).
  Só existem neste computador, porque são de terceiros e não vão para o GitHub.
