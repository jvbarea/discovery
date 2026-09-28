# Túnel de Vento DSC-26

Um carro de F1 genérico (proporções do regulamento 2026) num túnel de vento 3D, visto em perspectiva.
Mostra o ar contornando o carro com fumaça, partículas e linhas de corrente, o mapa de pressão na carroceria
e uma balança aerodinâmica com downforce, arrasto e balanço ao vivo.

**Abrir:** [jvbarea.github.io/discovery/f1-air-simulation](https://jvbarea.github.io/discovery/f1-air-simulation/)
ou dê duplo clique no `index.html` (precisa de internet para carregar o three.js e as fontes).

## O que dá para fazer

| Controle | Efeito |
| --- | --- |
| Velocidade do vento (60–340 km/h) | Escala forças e a velocidade da animação. O formato do escoamento não muda, porque o campo é normalizado por V∞. |
| Modo curva / modo reta | Asas ativas de 2026: no modo reta os flaps abrem, o downforce e o arrasto caem e o campo é recalculado. |
| Altura do assoalho (15–60 mm) | Efeito solo: o downforce do assoalho sobe até ~22 mm; abaixo de ~18 mm o assoalho estola e aparece o aviso de *porpoising*. |
| Fumaça, partículas, linhas de corrente | Três formas de ver o mesmo campo de velocidade. A altura da sonda de fumaça muda em tempo real. |
| Pressão (Cp) | Pinta a carroceria de azul (sucção) a vermelho (estagnação). A vista "Assoalho" mostra o fundo do carro. |
| Forças | Setas de downforce dianteiro/traseiro e de arrasto (1 m de seta = 1.000 kgf). |

## Como o escoamento é calculado

É uma representação didática, não CFD de engenharia. O campo de velocidade soma:

1. **Escoamento livre** em +x, normalizado (V∞ = 1).
2. **Bloqueio do corpo:** ~20 primitivas de distância com sinal (SDF) imitam o carro; perto da superfície
   o fluxo fica tangente, desacelera na frente (estagnação) e acelera nos "ombros".
3. **Downforce por vórtices em ferradura** (lei de Biot–Savart) na asa dianteira, na asa traseira e no assoalho,
   com núcleo que se alarga a jusante. Daí saem os vórtices de ponta de asa e a esteira subindo atrás do carro.
4. **Efeito solo** pelo método das imagens: cada vórtice ganha um espelho sob o piso com circulação oposta.

A circulação vem de Kutta–Joukowski (Γ = ½·V·CL·A / envergadura), usando a mesma tabela de coeficientes que
alimenta o painel, então números e visual são coerentes. As linhas de corrente são integradas com RK2 em passo de
tempo fixo; as partículas apenas percorrem essas trajetórias, o que permite milhares delas a 60 fps.
O Cp é 1 − (V/V∞)² (Bernoulli).

Os coeficientes são ilustrativos, na ordem de grandeza de um carro atual: −CL·A ≈ 3,5 m² e CD·A ≈ 1,0 m²
no modo curva. A 300 km/h isso dá ~1.500 kgf de downforce, e o carro "anda no teto" acima de ~220 km/h.

## Técnica

- Um único `index.html`, sem build: three.js 0.147 (UMD) via cdnjs e OrbitControls via jsDelivr.
- Carro modelado por código: loft de seções superelípticas, perfis NACA invertidos para as asas, pneus por revolução.
- Tema claro/escuro automático; a cena do túnel acompanha o tema.

## Limitações

- Não resolve Navier–Stokes: não há separação real, camada-limite nem turbulência calculada
  (a agitação da esteira é um ruído visual aplicado onde o fluxo tocou o carro).
- O carro é genérico e simétrico, sem guinada (yaw) nem esterçamento.
