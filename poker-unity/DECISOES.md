# Decisões

Registro do que foi decidido para a versão Unity do Golden Spur Hold'em e por quê.
Cada decisão nova entra no fim, com data. Uma decisão revista ganha uma entrada nova que aponta para a antiga; nada é apagado.

## 29/09/2026: estudo de plataforma

Origem: conversa de estudo paralela à construção da versão web (`../poker-saloon`), que segue em andamento como experimento "feito do zero".

### D1. Sair do WebGL feito do zero e usar um motor de jogo

- **Decisão:** a versão realista do jogo é feita em motor de jogo, com assets prontos (modelos, texturas, animações capturadas).
- **Por quê:** o objetivo passou a ser naturalidade e realismo. Personagens esculpidos em código ficam com cara de maquete; realismo vem de modelos feitos por artistas e movimento gravado de gente de verdade.
- **Alternativas:** continuar no WebGL puro (mantido só como experimento em `../poker-saloon`); three.js com Blender (abre por link, mas perde desempenho no navegador).

### D2. Unity 6.3 LTS com URP

- **Decisão:** Unity 6.3 LTS, pipeline URP.
- **Por quê:** é o motor mais usado em jogos de celular (o Genshin Impact é feito nele); qualquer animação do Mixamo funciona em qualquer personagem humanoide; IK pronta; plugin oficial do Unity para o Claude Code.
- **Alternativas:** Godot 4 (mais leve e com cenas em texto, mas animação menos madura e menos desempenho no celular); Unreal 5 (mais realista, pesado demais para o notebook atual).

### D3. Celular primeiro, a 30 quadros por segundo

- **Decisão:** alvo principal é o Galaxy S24+ (Exynos 2400, GPU Xclipse 940, 12 GB), app Android instalado, 30 fps estáveis; 60 fps como opção com o celular na tomada. A versão Windows vem de brinde do Unity.
- **Por quê:** poker é calmo e 30 fps constantes parecem naturais; esquenta menos e gasta menos bateria em sessões longas. A tela de 120 Hz divide certinho por 30.
- **Números que embasaram:** no 3DMark Wild Life Extreme o S24+ faz ~4.300 pontos no pico e ~2.400 depois de esquentar (55% de estabilidade); uma Iris Xe 96EU fica perto de 3.400 e a GeForce MX450 empata com ela. O notebook (Dell Inspiron 15 5510, i7-11390H, 16 GB) esquenta com facilidade.

### D4. Jogo em inglês

- **Decisão:** interface, cartas, nomes das mãos e falas em inglês ("Full House, Kings over Sixes"). A documentação continua em português.
- **Por quê:** os termos de poker nascem em inglês, o clima de 1899 fica mais convincente e modelos de IA pequenos escrevem melhor em inglês. Português pode entrar depois pelo pacote de localização.

### D5. Naturalidade: movimento gravado com camadas por código

- **Decisão:** base de animações capturadas (Mixamo, pacote de animações de cartas, vídeos convertidos em animação), com camadas por código por cima: IK das mãos até fichas e cartas, cabeça e olhos seguindo quem age, piscadas, respiração, trocas de postura, cada personagem num ritmo próprio. Rostos com expressões (blend shapes) e reações disparadas pelos eventos do jogo.

### D6. Personagens e assets

- **Prova de conceito:** Microsoft Rocketbox (grátis, MIT, com expressões faciais) e animações do Mixamo; saloon gratuito do Sketchfab; texturas do Poly Haven e do ambientCG.
- **Versão final (candidatos):** Character Creator 5 para todos os personagens (US$ 299, exporta para Unity com expressões e níveis de detalhe); MetaHuman só para um personagem de destaque, se valer o trabalho de conversão; personagens de faroeste da Asset Store se trouxerem expressões faciais; HQ Western Saloon para o cenário.
- **Sob medida:** mesa, fichas, cartas e estojo, feitos a partir das referências da especificação original.
- **Evitar:** Ready Player Me (encerrado em 31/01/2026) e sites que distribuem pacotes pagos de graça.

### D7. IA dos oponentes: cérebro e boca

- **Cérebro (decide as jogadas):** algoritmo de poker, não modelo de linguagem. Tabelas de mãos iniciais, equidade contra as mãos prováveis do oponente, apostas e blefes equilibrados, leitura do estilo do jogador durante a partida. O oponente de nível alto é esse cérebro no modo mais forte.
- **Boca (conversa):** modelo de linguagem só para falas e provocações, alimentado pelo que o cérebro percebeu. Primeiro teste: Gemini Nano pelo Prompt API do ML Kit, que roda no processador de IA do S24+. Plano B: modelo aberto (Gemma 3 ou Qwen3) pela biblioteca LLM for Unity.
- **Regra:** o modelo de linguagem nunca decide jogadas nem vê cartas escondidas. Fala atrasada é trocada por uma fala pronta.

### D8. Arquitetura pronta para online

- **Decisão:** o dealer funciona como servidor desde o começo: embaralha, distribui e valida as jogadas; cada jogador só recebe as próprias cartas. As regras ficam separadas da parte visual e conversam por eventos, como na especificação original.
- **Por quê:** quando o online chegar (mesas com amigos entre celular e PC), o dealer local vira um servidor de verdade sem reescrever o jogo, e ninguém vê as cartas dos outros.
- **Limite:** só fichas de mentira. Dinheiro de verdade exige licença de jogo de azar.

### D9. Ray tracing fica de fora

- **Decisão:** sem ray tracing em tempo real. A luz realista vem de mapas de luz calculados com antecedência (o "ray tracing" acontece na preparação).
- **Por quê:** navegadores não dão acesso ao ray tracing do celular; no nativo seria outro projeto; o ganho visual numa mesa sob um lampião é pequeno.

### D10. Onde cada coisa mora

- **Decisão:** o projeto Unity fica em `C:\dev\golden-spur`, fora do OneDrive e fora deste repositório. Este repositório guarda só documentação.
- **Por quê:** o OneDrive trava os arquivos temporários do Unity; o projeto fica grande; assets comprados não podem ir para repositório público.

### D11. Primeiro marco: prova de conceito no S24+

- **Decisão:** antes de migrar o jogo, montar uma sala simples com a mesa, cinco personagens sentados com animações do Mixamo e luz pré-calculada, instalar no S24+ e medir quadros por segundo e temperatura durante 30 minutos.
- **Se passar:** a lógica de poker, a IA e os testes da versão web são convertidos para C#.

### D12. Versionamento: Git com LFS num repositório privado do GitHub

- **Decisão:** o projeto Unity é versionado com Git em `github.com/jvbarea/golden-spur`, **privado**, com branch `main`. Modelos, texturas, áudio, vídeo, fontes, luz pré-calculada e pacotes vão pelo Git LFS (regras no `.gitattributes`); o que o Unity gera sozinho (`Library`, `Temp`, `Logs`, `UserSettings`) e os builds (`Builds`, APKs) ficam fora (`.gitignore`). Cenas e assets do Unity ficam como texto (Force Text), com fim de linha LF.
- **Por quê:** ter como voltar atrás antes de importar os assets da prova de conceito; privado porque assets comprados não podem ir para repositório público (D10); LFS porque modelos e texturas incham o histórico do Git.
- **Alternativas:** Unity Version Control (lida bem com binários e trava arquivos, mas é mais um serviço para aprender; faz mais sentido com equipe); Git sem LFS (o repositório fica pesado com os primeiros personagens).
- **Limite:** o LFS gratuito do GitHub tem cota de armazenamento e de download por mês; acompanhar em Settings → Billing quando entrarem os assets grandes.

## 30/09/2026: resultado da prova de conceito

### D13. A prova de conceito passou; começa a conversão para C#

- **Resultado (D11):** a cena com o saloon (~1 milhão de triângulos), cinco personagens do Rocketbox sentados com animações do Mixamo, luz pré-calculada e o lampião com sombra em tempo real segurou **30 fps por 33 minutos** no S24+, com o estado térmico do Android em 0 o tempo todo, a folga térmica estável em ~0,60 (1,0 é onde começa a limitação) e a bateria subindo de 27,3 °C para 30,9 °C, com o celular carregando. Uma segunda rodada com luz nos personagens manteve os 30 fps. Detalhes e dados em `C:\dev\golden-spur\Medicoes`.
- **Decisão:** seguir com o Unity e converter para C# a lógica de poker, a IA e os testes da versão web (`../poker-saloon/index.html`, §3, §4 e §16). O motor e a IA ficam em C# puro, num assembly sem dependência do Unity, com os testes rodando no editor; é o que deixa o dealer virar servidor depois (D8).
- **Pendências:** um teste de 30 minutos na bateria, fora da tomada (uso real); engasgos raros de ~75 ms que não vêm do código do jogo, a investigar com o Profiler; personagens da versão final ainda não testados.

### D14. Controles por toque

- **Decisão:** as ações ficam em linhas tocáveis no canto inferior direito, como a HUD da Ref D (Fold, Check ou Call, Bet ou Raise com o valor escolhido por atalhos de ½ pote, ¾ pote, pote e all-in e por ajuste fino). Segurar o dedo nas suas cartas as ergue para espiar (substitui o "segurar Q"); segurar no board faz você se debruçar sobre a mesa (substitui o "segurar E"). Desistir quando passar é de graça continua exigindo segurar por 0,5 s.
- **Por quê:** o jogo é celular primeiro (D3) e a especificação original foi feita para teclado, mouse e controle. As linhas da HUD crescem em relação às medidas da Ref D (4,5 vh por linha) para caber o dedo; o resto da HUD mantém o estilo da Ref D.
- **Alternativas:** barra de botões fixa embaixo, como nos apps de poker (mais prática, mas foge da Ref D); gestos de deslizar para desistir ou aumentar (rápidos, mas fáceis de disparar sem querer).

### D15. Blender para assets, controlado por scripts

- **Decisão:** Blender 5.2 LTS, na versão portátil em `C:\dev\tools\blender-5.2.2-windows-x64`, controlado pelo Claude por scripts Python sem janela (`blender --background --python`), com imagens renderizadas para conferir o resultado. Usos: deixar leves os assets de terceiros (começando pelo saloon, com ~1 milhão de triângulos em 942 objetos), modelar os objetos sob medida do D6 (estojo da Ref C, cadeiras de madeira curvada, lampião, mesa) e tratar animações capturadas. O movimento natural dos personagens (IK das mãos, olhar, respiração, reações do D5) continua no Unity.
- **Por quê:** a prova de conceito mostrou que o celular aguenta mais, mas o saloon desperdiça orçamento com peças que quase não aparecem; e os objetos da mesa, hoje blocos gerados em código, precisam de modelagem de verdade para ficarem fiéis às referências.
- **Alternativas:** modelar tudo por código no Unity (limitado, como a mesa provisória); comprar objetos prontos (menos fiéis às Refs A a D).
- **Limite relacionado (D6):** o Character Creator 5 pede 8 GB de memória de vídeo e 230 GB livres (instalação e cache); este notebook tem uma GeForce MX450 de 2 GB e 152 GB livres. Os personagens finais precisam de outro computador para o CC5 ou de personagens prontos (lojas da Reallusion, Asset Store). O MetaHuman também exige o Unreal instalado e, no celular, usa o LOD 3 (cabeça de 2.500 vértices, texturas de 2048, cabelo em cards).

## 30/09/2026: primeiro teste da mesa jogável no celular

### D16. Câmera parada em repouso (revê o ♥5 da especificação)

- **Decisão:** a câmera nos seus olhos fica parada em repouso; sai a respiração de ±4 mm a 0,25 Hz do ♥5 (e da versão
  web). As poses de espiar as cartas e de se debruçar no board continuam. A cintilação de ±3% do lampião (♥6) fica.
- **Por quê:** no primeiro teste no S24+, as pilhas de fichas "cintilavam". Cada ficha de uma pilha ocupa uns 2,5
  pixels de altura na tela, e a respiração movia a imagem menos de um pixel, o que fazia as listras das bordas
  "andarem". A medição no editor, na resolução interna do celular (1872 × 864, sem antisserrilhamento), comparou quadros
  em quatro combinações. Com a respiração, o desvio da luminância nas pilhas entre quadros foi de 5,4 a 7,0, com até 29%
  dos pixels variando mais de 8 níveis. Com a câmera parada, 0,3. A cintilação do lampião, a primeira suspeita, quase
  não pesa (0,3).
- **Alternativas:** antisserrilhamento (MSAA ou TAA), que custa desempenho no celular e só reduz o efeito; respiração
  menor, que só diminui o tremor. Os personagens continuam respirando nas animações; só a câmera ficou parada.

### D17. Compra de fichas na entrada e troca no fim, com o estojo como banco

- **Decisão:** no começo de cada Sit & Go, cada jogador paga $10 com uma moeda de ouro de $10 da época (a "Eagle",
  cunhada até 1907), e o Silas entrega as fichas do estoque inicial tiradas dos rolos do estojo: 4 de $1, 2 de 50¢,
  16 de 25¢ e 20 de 5¢. As moedas ficam no recorte redondo do estojo durante a partida. No fim, as fichas do vencedor
  voltam para o estojo e ele recebe as cinco moedas ($50). Sem recompra: o formato continua Sit & Go de cinco. O
  dinheiro é de mentira, como as fichas (D8).
- **Estojo:** as 7 canaletas guardam o banco da compra: duas de 5¢ (60 fichas cada), duas de 25¢ (55 cada), uma de 50¢
  (40), uma de $1 (40) e uma de $5 (30). Cada canaleta cabe ~73 fichas, e as cinco compras pedem 100 de 5¢ e 80 de
  25¢; as fichas de 1¢ e de $10 saem do estojo (continuam na mesa quando as pilhas pedem).
- **Por quê:** ideia do usuário: com a compra, o estojo faz sentido na mesa; a troca no fim fecha o ciclo da partida.
- **Alternativas:** recompra depois de quebrar (muda o formato para jogo a dinheiro); notas de papel da época (mais
  visíveis, mas difíceis de desenhar fiéis).

## 30/09/2026: fichas de verdade (noite de trabalho sozinho)

### D18. Fichas reais em cada pilha, com troco e troca de cor pelo estojo (revê a pilha como função do valor)

- **Decisão:** cada pilha (a de cada jogador, a aposta da rodada e o pote) guarda quantas fichas de cada valor tem.
  Apostar, juntar no pote, devolver aposta e pagar o vencedor movem as mesmas fichas, escolhidas das maiores para as
  menores; as pilhas deixam de ser recalculadas a partir do valor a cada mudança (♥9 e decisão da versão web).
  - **Troco:** quando o valor exato não sai das fichas da pilha, a menor ficha maior que o necessário vai ao estojo e
    volta em fichas menores. Vale para apostas, potes divididos e apostas devolvidas.
  - **Troca de cor:** quando a small blind chega a 5× um valor, o Silas troca as fichas desse valor por maiores, em
    grupos exatos (5 de 5¢ por uma de 25¢; o que sobra fica com o jogador): ninguém ganha nem perde centavo, e não há
    "corrida de fichas". Também quando uma pilha passa de 60 fichas.
  - **1¢:** reserva de 20 fichas no nicho vazio da frente do estojo, para os potes divididos com centavo ímpar.
  - O total de fichas de cada valor (estojo + mesa) nunca muda; no fim da partida o estojo volta exatamente como começou.
  - A contabilidade fica junto do motor (C# puro, determinística, testada em milhares de mãos): no jogo online o
    servidor também decide as fichas físicas (D8), e a retomada da partida salva refaz as fichas pelos mesmos eventos.
- **Por quê:** pedido do usuário: com o estojo como banco (D17), as fichas que saíram dele devem continuar fiéis na
  mesa ("aumenta muito mais o realismo"). Troca de cor e troco são o que um cassino de verdade faz.
- **Alternativas:** mandar as fichas ao estojo e trazer as equivalentes toda vez que as apostas vão ao pote (até 4
  vezes por mão, deixa o jogo lento e não existe no poker de verdade); continuar recalculando as pilhas pelo valor.

### D19. Som: a trilha da versão web gravada em arquivo e efeitos de cartas e fichas gravados de verdade

- **Decisão:** a música e o ambiente são os da versão web, que o usuário aprovou ("a música de fundo está muito boa"):
  o piano de ragtime (96 BPM, forma A A' B A', melodias sorteadas a cada volta), o murmúrio do salão e os copos
  tilintando. Em vez de sintetizar no celular, o próprio código da web grava cada parte em WAV, em laço sem emenda
  (`Tools/gravar_audio_web.js`, Chrome sem janela): 160 s de piano e 60 s de salão. Cartas, fichas e moedas, que na
  web "soavam mal", passam a ser gravações reais do pacote Casino Audio da Kenney (licença CC0, domínio público). Os
  efeitos saem do ponto da mesa onde acontecem (som 3D). Opções: "Music & saloon" e "Sound effects"; o volume geral é
  o do celular, e no Windows o M silencia tudo. As falas continuam só em legenda (especificação).
- **Por quê:** a especificação pedia áudio procedural, mas sintetizar a trilha em tempo real gastaria CPU do S24+ sem
  ganho audível; gravada, ela é igual e custa só um arquivo em streaming. Os efeitos sintetizados eram o ponto fraco.
- **Alternativas:** sintetizar tudo no Unity em tempo real (CPU e trabalho de conversão do WebAudio); comprar um
  pacote de áudio (não precisa: CC0 resolve os efeitos e a trilha já existe).

### D20. Som sem chiado: o salão sem o murmúrio e o piano mais seco (revê o D19, 01/10/2026)

- **Decisão:** a gravação para o Unity deixa de copiar dois pontos da web: o salão perde as duas camadas de murmúrio
  (ruído filtrado em 520 e 880 Hz subindo e descendo devagar) e o fundo grave fica 6 dB mais baixo; o piano fica mais
  seco (som direto 0,75 em vez de 0,55, reverb 0,3 em vez de 0,9), no mesmo volume de antes. A versão web não muda.
- **Por quê:** no celular o usuário ouviu "um xiado de fundo que aumenta e diminui, parece a ventoinha do PC". Medido:
  o salão ficava ~12 dB abaixo da música, com ondas de 7 dB a cada ~10 s, justo na faixa que o alto-falante do celular
  realça; e o reverb da web, feito de ruído decaindo em 1,8 s e mais alto que o som direto, enchia todos os intervalos.
  Depois: salão a −50 dBFS, parado; piano com o mesmo RMS (−20 dBFS) e o reverb ~10 dB mais baixo.
- **Alternativas:** baixar só o volume do salão (o vai e vem continuaria audível no fone); trocar o murmúrio por uma
  gravação real de salão (fica para quando houver uma CC0 boa).

### D21. Falas continuam prontas: o Gemini Nano não está liberado no S24+ (revê o D7, 01/10/2026)

- **Decisão:** as falas seguem as prontas da versão web (`PersonaLines`), sem modelo de linguagem por enquanto. O
  protótipo ficou registrado na história do Git do projeto (commit "Protótipo das falas pelo Gemini Nano") e saiu do
  projeto, com o Android mínimo de volta a 25 e sem Gradle personalizado.
- **Por quê:** o protótipo (plugin Kotlin com o Prompt API do ML Kit, `genai-prompt` 1.0.0-beta4) chegou ao AICore do
  S24+ (Exynos, AICore de 20/08/2026), que respondeu "error code 606-FEATURE_NOT_FOUND: Feature 636 is not available":
  o prompt livre não está liberado para o aparelho, embora o AICore esteja instalado. O plano B do D7 (Gemma 3 ou Qwen3
  pelo LLM for Unity, ou o MediaPipe) roda na CPU ou na GPU, não na NPU: a NPU do Exynos não é aberta a apps para
  modelos de linguagem, e o caminho oficial para ela era o próprio AICore. O usuário preferiu manter as falas prontas.
- **Alternativas:** LLM for Unity com Qwen3 0,6–1,7 B ou Gemma 3 1B na CPU (testável no PC, com custo de calor e bateria
  a medir); voltar ao Gemini Nano se uma atualização do AICore liberar o recurso, ou num aparelho que já o tenha (Pixel
  9 e 10, linha S25).

## 05/10/2026: o projeto fica como o teste de Android

### D22. Versão do S24+ congelada no estado atual

- **Decisão:** o Golden Spur Hold'em fica como está, jogável no S24+, sem trabalho novo. Não são feitos: os
  personagens da versão final (a fase com máquina potente dos próximos passos, candidatos do D6), a `ESPECIFICACAO.md`
  da versão Unity e a conversão do jogo para outro motor. O projeto em `C:\dev\golden-spur` e o repositório privado
  (D12) ficam guardados; nada é apagado e as decisões D1 a D21 continuam descrevendo o que existe. Os próximos
  experimentos (ray tracing, iluminação global, mundo aberto, física) são outros projetos, no Unreal 5, num PC potente
  na nuvem, em pasta e repositório próprios.
- **Por quê:** o poker era o teste do que dá para fazer no Android, e esse teste está respondido: a prova de conceito
  passou (D13) e a mesa jogável segura 30 fps no S24+ sem limitação térmica. O usuário quer agora testar o que só um
  PC potente permite, e isso não cabe no alvo do D3 nem neste notebook (MX450 de 2 GB, 16 GB de memória).
- **Alternativas:** converter o poker para o Unreal (meses de reescrita de motor, IA e parte visual para chegar ao
  ponto de hoje, e o usuário não quer o poker como veículo dos testes novos); levar o poker para o PC no próprio Unity
  com HDRP (mantinha código e testes, mas também não era o objetivo); seguir com a fase dos personagens finais no
  celular.
