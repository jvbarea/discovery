# discovery — convenções

Repositório de explorações feitas com o Claude: protótipos, simulações e testes de capacidade.
Idioma do projeto: português (Brasil).

## Estrutura

- Uma pasta por exploração, nome em kebab-case (`f1-air-simulation`, `outra-ideia`).
- Cada pasta tem um `README.md` curto: o que é, como abrir, limitações.
- Páginas web são autocontidas: `index.html` único que abre com duplo clique.
  - Bibliotecas só por CDN com versão fixa (cdnjs de preferência; jsDelivr/unpkg se preciso),
    porque o mesmo arquivo é publicado como artefato do Claude, cujo CSP só libera esses hosts.
  - HTML5 com as tags `html`/`head`/`body` omitidas; para publicar como artefato, remova a
    linha `<!doctype html>` numa cópia (a ferramenta de artefato adiciona o esqueleto).

## Ao terminar uma exploração

1. Adicionar a linha dela na tabela do `README.md` da raiz.
2. Commit + push para `github.com/jvbarea/discovery` (público, GitHub Pages ativo na raiz da `main`).
   Link público de cada página: `https://jvbarea.github.io/discovery/<pasta>/`.
3. Links de artefato do claude.ai são privados por padrão: não colocar no README público.
