# Less_BEM — Boas Práticas de CSS com BEM + LESS

Exercício de refatoração de HTML/CSS aplicando a metodologia **BEM** (Block, Element, Modifier), com os estilos escritos em **LESS** e compilados para CSS puro.

## Objetivo

Partindo de uma marcação com classes soltas e inconsistentes, o exercício consiste em:

1. Reorganizar as classes do HTML seguindo BEM (blocos, elementos e modificadores).
2. Reescrever o CSS respeitando essa mesma estrutura.
3. Usar um pré-processador (neste caso, LESS) para tornar os estilos mais organizados e reaproveitáveis.

## O problema no código original

```html
<div class="produto">
    <div class="produto__content">...</div>
    <div class="produto--em-destaque">...</div>
    <div class="produto__content">...</div>
</div>
```

Duas inconsistências de BEM:

- **Bloco mal posicionado**: `produto` nomeava o container da lista, não o card individual — mas os elementos (`produto__imagem`, `produto__nome`...) já pressupunham que `produto` fosse o card.
- **Modificador substituindo o bloco**: `produto--em-destaque` era usado sozinho no lugar de `produto__content`, perdendo borda, padding e sombra do card base. Um modificador em BEM **sempre acompanha** a classe que ele modifica, nunca a substitui.

## Estrutura BEM aplicada

| Papel | Classe | Descrição |
|---|---|---|
| Bloco | `.produtos` | Grid que lista os cards de produto |
| Bloco | `.produto` | Card individual de produto |
| Elemento | `.produto__imagem` | Imagem do produto |
| Elemento | `.produto__nome` | Nome do produto |
| Elemento | `.produto__descricao` | Descrição do produto |
| Elemento | `.produto__preco` | Preço do produto |
| Elemento | `.produto__botao` | Botão de compra |
| Elemento | `.produto__selo` | Selo "Destaque" |
| Modificador | `.produto--em-destaque` | Variação visual de um produto em destaque, **combinado** com `.produto` |

```html
<div class="produto produto--em-destaque">
    <span class="produto__selo">Destaque</span>
    <img class="produto__imagem" ...>
    <h4 class="produto__nome">...</h4>
    <p class="produto__descricao">...</p>
    <span class="produto__preco">...</span>
    <button class="produto__botao">Comprar</button>
</div>
```

## Por que LESS

O `style.less` usa três recursos do pré-processador para deixar o BEM explícito no próprio código-fonte:

- **Variáveis** para cores, espaçamentos e transições, evitando valores mágicos repetidos.
- **Aninhamento com `&`**, onde cada elemento (`&__nome`) e modificador (`&--em-destaque`) vive dentro do bloco que pertence — a hierarquia BEM fica visualmente óbvia no arquivo.
- **Funções de cor** (`darken`, `fade`) para gerar estados de hover e sombras a partir de uma única cor base, sem precisar declarar cada tom manualmente.

## Estrutura de arquivos

```
├── index.html     # marcação com classes BEM
├── style.less     # fonte dos estilos (variáveis, aninhamento, mixins)
├── style.css      # CSS compilado, referenciado pelo index.html
└── README.md
```

## Como compilar o LESS

```bash
npm install -g less
lessc style.less style.css
```

O `index.html` referencia diretamente `style.css`, então não é necessário nenhum processo de build para visualizar o resultado — basta abrir o arquivo no navegador. O `.less` é reprocessado apenas quando os estilos são alterados.

## Preview

Um grid com três cards de produto (imagem, nome, descrição, preço e botão de compra); o card do meio traz o selo "Destaque" e uma paleta dourada, aplicada via `.produto--em-destaque` sobre o card base — com efeito de elevação (`hover`) e layout responsivo (empilha em coluna única abaixo de 720px).
