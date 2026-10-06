# Guia de uso

Como trabalhar com o Claude no projeto Unity do **Golden Spur Hold'em**: abrir o projeto, pedir mudanças, testar no PC e
no celular, versionar e deixar o Claude trabalhando sozinho. Pressupõe o [guia de instalação](GUIA-DE-INSTALACAO.md)
concluído.

## 1. Abrir o projeto

1. Abra o Unity Hub e o projeto `golden-spur` (`C:\dev\golden-spur`). Espere o editor terminar de importar.
2. Abra o VS Code na mesma pasta e inicie o Claude Code. Ele lê o `CLAUDE.md` do projeto sozinho: decisões, estado e
   ferramentas.
3. Para conferir que o Claude enxerga o editor, peça "veja se o editor está conectado". No PowerShell, o mesmo é:

   ```powershell
   unity pipeline list
   ```

   O esperado é `golden-spur` com **true** em Running, Pipeline e Server Reachable.

## 2. O que o Claude faz sozinho

- **Controla o editor aberto** pelo Unity CLI: entra e sai do modo Play, roda código C# no editor, tira fotos da cena,
  recalcula a luz, roda os testes e gera o APK.
- **Monta as cenas por código.** A cena do jogo (`Assets/Scenes/Table.unity`) sai do `PocSceneBuilder` e do
  `TableGameBuilder`; não se edita à mão. Mudou algo na montagem, o Claude remonta a cena e recalcula a luz (cerca de
  3 minutos).
- **Modela no Blender por script** (D15): o Blender portátil roda sem janela e os scripts ficam em `Tools/blender/`.
- **Grava as artes e o som da versão web** com o próprio código dela, num Chrome sem janela: cartas, fichas, tampa do
  estojo e moeda (`Tools/gerar_texturas_web.js`) e a trilha de ragtime com o salão (`Tools/gravar_audio_web.js`).
- **Testa e documenta:** a cada etapa, testes verdes, fotos no modo Play, `CLAUDE.md` e `CRONOLOGIA.md` atualizados,
  commit e push.

## 3. Como pedir

Peça o resultado, não o caminho. Exemplos que funcionaram bem:

- "Aumente as cartas do board no canto superior direito em uns 50%."
- "A borda interna da maleta está listrada, consegue ver?" (com uma foto do celular)
- "Quando as fichas se juntam no centro, quero que continuem fiéis ao que saiu do estojo."
- "Crie uma lista de tarefas para a noite; quando eu acordar, peço pausa."

Decisão nova (regra, plataforma, arte, som) entra primeiro no fim do [DECISOES.md](DECISOES.md) e depois no `CLAUDE.md`.
O Claude propõe; quem aprova é você.

## 4. Testar no PC

O editor testa quase tudo sem o celular:

- **Jogar:** o botão Play do Unity, ou peça ao Claude. O título oferece Continue (partida salva) e New Game.
- **Partida automática:** a IA joga também no seu lugar, em velocidade 1 a 12. Serve para chegar ao fim de uma
  partida, ver a troca final e procurar erros em muitas mãos.
- **Câmera lenta e fotos:** o Claude baixa a escala de tempo e fotografa por câmeras provisórias (de perto, de cima,
  pelo seu lugar). As fotos ficam em `Assets/Logs/shots`, fora do Git.
- **Sinais de problema:** erros no console, aviso "as fichas mostradas saíram da contabilidade" (a mesa divergiu das
  fichas do motor) e fichas "do caixa" (o estojo não tinha o troco).

## 5. Testar no celular

Com o S24+ no cabo e desbloqueado:

```bash
Tools/testar_s24.sh [--sem-build] [minutos]
```

O script gera o APK (8 a 30 minutos, conforme o que mudou), confere a instalação, abre o app, mede fps, temperatura e
bateria e puxa o `perf.csv`. O resultado entra em `Medicoes/`. Cuidados:

- O app grava o `perf.csv` só com ele na tela, e **cada abertura apaga o anterior**: puxe antes de reabrir.
- O Android fecha o app quando outro vem para a frente; a partida está salva e o Continue retoma do ponto exato.
- Celular bloqueado faz a instalação falhar; o script espera o desbloqueio.

## 6. Opções do jogo

Velocidade da animação, volume da música com o salão, volume dos efeitos, nome da sua mão acima das ações, medidor de
desempenho e 60 fps só na tomada. No Windows, M silencia tudo. O volume geral é o do celular.

## 7. Versionar

- Projeto em `github.com/jvbarea/golden-spur`, **privado**, branch `main` (D12). Modelos, texturas, áudio, fontes e luz
  pré-calculada vão pelo Git LFS.
- O Claude faz commit e push ao fim de cada etapa, com o porquê na mensagem e as dificuldades na `CRONOLOGIA.md`.
- Este repositório (`discovery`) é **público**: só documentos, sem assets de terceiros, sem links privados. O Claude
  edita aqui e não faz commit; o commit é seu.

## 8. Deixar o Claude trabalhando sozinho

1. Combine a fila de tarefas; o Claude a escreve em `PLANO.md`, no projeto, com estado e horário de cada item.
2. Antes de sair: notebook na tomada, "suspender: nunca" e "fechar a tampa: nada fazer" nas opções de energia, Windows
   Update pausado, Unity e VS Code abertos e o Claude Code no modo automático (um pedido de permissão para tudo até de
   manhã).
3. Durante a noite o Claude mede o PC a cada 30 s (temperatura e uso da GPU, carga e desempenho da CPU, memória livre,
   memória do editor e do Gradle, disco, tomada) e marca o começo e o fim de cada etapa; quando algo passa do limite,
   ele age no menor alvo possível (encerra o Gradle parado, reinicia só o editor) e anota.
4. De manhã, o `PLANO.md`, a `CRONOLOGIA.md` e o relatório mostram o que foi feito, as dificuldades, quanto durou cada
   etapa, os picos de temperatura e de memória e o que conferir; o APK fica pronto em `Builds/golden-spur.apk`. Peça
   "pausa" quando quiser assumir.

## 9. Problemas comuns

| Sintoma | Causa | O que fazer |
| --- | --- | --- |
| "Main thread operation timed out" | o editor estava importando, compilando ou esperando um clique | esperar e repetir |
| O jogo engasga e o console enche de `NullReferenceException` | um script foi editado com o jogo rodando e o Unity recompilou | sair do Play; a preferência "Script Changes While Playing" já está em recompilar depois do Play |
| Pasta `Assets/_Recovery` apareceu | o editor fechou com o jogo rodando (ou o PC desligou) | apagar; a cena salva vale |
| Fontes `.asset` mudaram depois do Play | o TextMeshPro acrescentou letras às folhas | descartar com `git checkout` antes do commit |
| Teste de velocidade do avaliador falha | o PC estava ocupado (Google Drive sincronizando, por exemplo) | rodar de novo com o PC parado |
| `git push` falhou com "Could not resolve host" | queda de rede | repetir mais tarde |
| Cada pedido ao editor leva 15 a 30 s | pouca memória livre depois de horas de Play e builds (o editor passa de 6 GB e o Gradle do build Android fica parado na memória) | encerrar o Gradle e reiniciar o editor; o Claude faz os dois |
