# Especificação dos testes

O que confere o Golden Spur Hold'em em quatro camadas (regras, editor, celular e visual), como rodar cada uma e quando
repetir. Os critérios vêm das decisões em [DECISOES.md](DECISOES.md) e da especificação original.

## 1. Regras e IA (testes automáticos)

**Onde:** `Assets/Tests/EditMode/Poker`, testes NUnit do assembly do motor (`GoldenSpur.Poker`, C# puro, sem Unity).
**Como rodar:** `Tools/rodar_testes.sh` com o editor aberto e fora do Play; cerca de 1 minuto, 61 testes.

| Grupo | O que garante |
| --- | --- |
| Avaliador (13) | ordem das mãos (roda do ás, flush × sequência, kickers, board que joga para os dois), 10.000 mãos sorteadas iguais a um avaliador ingênuo independente, velocidade de 2 milhões de avaliações por segundo, nomes das mãos em inglês ("Full House, Kings over Sixes") e dinheiro em centavos ("$3.05", "65¢") |
| Regras (11) | potes laterais (o exemplo da especificação, duas e três camadas), centavo da divisão para o primeiro vencedor à esquerda do botão, aposta não paga devolvida, quem desiste paga e não disputa, aumento mínimo igual ao último aumento completo, all-in incompleto que não reabre a ação, mano a mano com o botão na small blind, botão e blinds pulando eliminados, opção da big blind |
| IA (7) | tabela pré-flop (ases contra 1 e 4 oponentes, pior e melhor mão), tabela pré-calculada igual ao cálculo, equidade de trinca no flop, contagem de outs, decisão que não depende das cartas fechadas alheias (D7, D8) e as personas acertando os alvos de VPIP e PFR |
| Partidas (9) | 20.000 mãos IA contra IA sem quebrar invariantes (soma das fichas, nenhum stack negativo, pote pago inteiro, botão sempre num jogador ativo, toda ação legal, toda partida termina), mesma semente dá o mesmo registro, cinco partidas iguais **evento por evento** às da versão web (`Reference/`) e cada jogador só vê as próprias cartas (D8) |
| Partida salva (3) | refazer as jogadas registradas leva ao mesmo estado; com o estado do gerador da IA, a partida continua idêntica depois de 60 e de 150 jogadas |
| Fichas reais, D18 (6) | estoque da compra (4 × $1, 2 × 50¢, 16 × 25¢, 20 × 5¢), troco pelo estojo, pote dividido com centavo ímpar usando fichas de 1¢, troca de cor quando a blind chega a 5× o valor, mesma semente dá as mesmas fichas e 5.000 mãos em que cada pilha vale o que o motor diz, o pote bate, nada sobra no fim da mão, o total de cada valor não muda e o estojo volta cheio ao fim da partida |

**Quando rodar:** a cada mudança no motor, na IA ou nas fichas, e antes de todo commit. Mudança de regra ou de IA de
propósito exige mudar a versão web também ou regenerar as referências (`node Tools/gerar_referencia_web.js`) de forma
consciente. O teste de velocidade do avaliador é sensível a carga: com o PC ocupado (Google Drive sincronizando, por
exemplo) cai para 1,1 a 1,8 milhão por segundo; repita com o PC parado antes de concluir que piorou.

## 2. Jogo no editor (modo Play)

Testes manuais ou dirigidos pelo Claude, no editor aberto, sem o celular.

| O que | Como | Critério |
| --- | --- | --- |
| Partida inteira | partida automática em velocidade 6 a 12 até o fim | nenhum erro no console; nenhum aviso de fichas fora da contabilidade; nenhuma ficha "do caixa" |
| Compra e troca no fim (D17) | partida nova; fim de partida automática | moedas entram no recorte do estojo, fichas saem dos rolos; no fim as fichas voltam e as moedas vão para a frente do vencedor |
| Troco e troca de cor (D18) | câmera lenta (escala de tempo 0,02 a 0,1) com fotos de perto | fichas vão ao estojo e voltam outras de mesmo valor; a pilha do Lucien não entra no estojo |
| Partida salva | sair da mesa no meio da mão e usar Continue | mesma mão, mesmas cartas, mesmas fichas, sem engasgo ao continuar |
| Falas e rostos (♦5, D5) | ler a legenda durante uma partida automática; fotos de perto dos rostos | falas na cor do personagem, uma a cada 20 s por personagem e nunca na sua vez; comentário de vitória depois do aviso do pote; piscar, sorriso, cara fechada e boca falando |
| Tells (♦5) | disparar cada gesto em câmera lenta | Coronel com a mão na têmpora, Edgar tamborilando na borda da mesa, Adelaide imóvel; nada atravessando o rosto ou a mesa de forma visível |
| Espiada e pote | fotos de perto em câmera lenta (escala de tempo 0,05) | o oponente levanta a borda de perto das próprias cartas, com a do centro no feltro (a face nunca vira para a mesa); quem ganha puxa o pote com as duas mãos |
| Saída do eliminado | disparar a saída de cada jogador no título e fotografar da sua cadeira e do alto; partida automática até o fim e revanche | levanta para o lado longe de você, vira de costas e sai andando para o escuro; some a ~2,4 m da cadeira, que fica vazia; na revanche todos voltam sentados no lugar certo |
| Som (D19) | estado das fontes de áudio durante a partida | trilha e salão em laço; efeitos de cartas, fichas e moedas disparando nos eventos |
| HUD e menus | fotos na proporção do S24+ (1560 × 720) | textos legíveis, nada sobreposto, opções com as 6 linhas |

## 3. Celular (Galaxy S24+)

**Como rodar:** `Tools/testar_s24.sh [--sem-build] [minutos]` (build, instalação conferida, abertura, medição pelo
`adb` e o `perf.csv` do app). Medições em `Medicoes/` no projeto.

| Medida | Critério (D3, D11) | Resultado em 30/09/2026 |
| --- | --- | --- |
| Quadros por segundo | 30 fps estáveis por 30 minutos | 30 fps por 33 min (prova de conceito); mesa jogável com saloon, estojo e móveis: 30 fps |
| Temperatura | sem limitação térmica (estado térmico 0) | estado 0; índice `getThermalHeadroom` de 0,62 a 0,76 (1,0 = começa a limitar) |
| Bateria | sessão longa sem cair dos 30 fps | 27 min a 30 fps, pele a 33–34 °C, ~10% de carga por hora |
| Engasgos | quadros acima de 50 ms raros e explicados | raros, de ~75 ms, ainda sem causa (próximo passo: Profiler num build de desenvolvimento); 117 ms no Continue, corrigido em 01/10 |
| Memória | o Android não fecha o app com ele na tela | o app ocupa ~675 MB; o Android o fecha quando outro app vem para a frente |

**Quando repetir:** depois de mudar luz, materiais, número de objetos na tela, personagens, som ou pós-processamento, e
antes de qualquer versão para outras pessoas. A sessão de 30 minutos vale como teste de regressão de desempenho.

## 4. Visual

O celular mostra a cena em 1872 × 864 (escala de render 0,8) e detalhes finos se comportam diferente do monitor.

- **Tremor e cintilação (D16):** renderizar a câmera do jogo em 1872 × 864 sem antisserrilhamento várias vezes e comparar
  os quadros pixel a pixel. Com a câmera parada, o desvio entre quadros nas pilhas de fichas deve ficar perto de 0,3
  (com a respiração da câmera era ~6).
- **Linhas finas:** filetes e chanfros com menos de 1 pixel na tela saem picotados ou listrados; conferir no celular
  depois de qualquer arte com linhas finas (a tampa do estojo precisou de filetes de 8 e 5 px e ASTC 4×4).
- **Cores sob a luz do lampião:** materiais metálicos refletem a sonda de reflexo (paredes escuras e feltro verde);
  conferir de perto e da sua cadeira (a moeda de ouro saía preta com metal puro).
- **O usuário no celular** é o teste final: tudo que muda a imagem passa por ele antes de ser dado como pronto.
