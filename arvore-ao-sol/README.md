# Árvore ao Sol

Uma árvore gerada por código, iluminada pelo sol e pelo céu com **path tracing** (traçado de caminhos de luz),
escrito do zero em WebGL2 puro. Não usa nenhuma biblioteca e não precisa de placa RTX.

**Abrir:** [jvbarea.github.io/discovery/arvore-ao-sol](https://jvbarea.github.io/discovery/arvore-ao-sol/)
ou dê duplo clique no `index.html`.

A imagem começa granulada e fica limpa em segundos: a cada quadro, cada pixel soma mais uma amostra de luz.
Num Intel Iris Xe, a cena chegou a ~430 amostras por pixel em 30 s, a 1088 × 612.

## Controles

- **Hora do dia:** move o sol do nascer ao pôr; o céu é recalculado (azul ao meio-dia, dourado no fim da tarde).
- **Nova árvore:** gera outra árvore com uma semente aleatória.
- **Arrastar / rolar / pinça:** gira e aproxima a câmera. Duplo clique volta à vista inicial.

## Como funciona

1. **Árvore por colonização do espaço** (Runions et al., 2007): ~2.400 pontos enchem o volume da copa e os galhos
   crescem na direção deles. A espessura segue o modelo de tubos (regra de Leonardo, expoente 2,4).
   Galhos viram cápsulas; folhas viram elipses pontudas.
2. **BVH:** as ~12 mil primitivas vão para uma hierarquia de caixas (construída com SAH) guardada em texturas float,
   para que cada raio teste só o que está no caminho.
3. **Path tracing no fragment shader:** cada pixel dispara um raio que quica até 4 vezes. Em cada ponto, um segundo raio
   vai até um ponto aleatório do disco do sol (sombras com penumbra real). As folhas refletem e também deixam passar
   luz, por isso brilham quando o sol está atrás delas. A roleta russa encerra caminhos que já contribuem pouco.
4. **Céu físico:** espalhamento Rayleigh (azul) e Mie (halo do sol) calculados na CPU numa textura de 128 × 48,
   com correção empírica no horizonte. A mesma atmosfera define a cor do sol e a névoa da distância.
5. **Acúmulo e exibição:** as amostras são somadas em buffers de ponto flutuante; a exibição aplica exposição,
   curva de tom ACES e gama. A resolução interna se ajusta à velocidade da GPU nos primeiros quadros.

## Limitações

- Cena estática (sem vento), porque o acúmulo de amostras precisa de uma imagem parada.
- A grama é um plano com textura procedural, sem folhas de grama de verdade.
- O céu usa espalhamento simples; o horizonte é clareado por uma correção, não por espalhamento múltiplo real.
