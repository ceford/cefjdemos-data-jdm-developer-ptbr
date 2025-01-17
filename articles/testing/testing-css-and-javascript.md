<!-- Filename: J4.x:SCSS_and_Sass / Display title: Testando CSS e JavaScript -->

## Introdução

Em um site Joomla de produção, o Joomla entrega arquivos CSS e Javascript comprimidos (aqueles com `.min` no nome do arquivo). Os servidores enviarão as versões gzipped se estiverem disponíveis (aquelas terminando com um sufixo `.gz`). Esses arquivos são criados a partir de fontes não comprimidas. No caso dos arquivos CSS, eles são frequentemente criados processando precursores SCSS. Para testar o código que inclui CSS ou JavaScript novo ou atualizado, é necessário reconstruir os arquivos CSS e as versões comprimidas e minificadas a partir das fontes alteradas.

## Testar Instalação

Para fins de teste, você precisa de um servidor de teste com Git, Node.js e Composer instalados. Vá para o repositório [Joomla CMS](https://github.com/joomla/joomla-cms) no GitHub e siga as instruções em *Como obter uma instalação funcional a partir do código-fonte* próximo ao final do texto README.

Após a conclusão da instalação do Joomla, não exclua o Diretório de Instalação! Isso também irá excluir o diretório `build`, que é necessário para reconstruir os arquivos CSS e JavaScript.

Os arquivos principais `.scss` estão nos seguintes diretórios:

- midia/modelos/site/cassiopeia/scss
- midia/modelos/administrador/atum/scss

O script de geração de CSS, o compilador SCSS e outras ferramentas de compilação semelhantes estão localizados no diretório `build/`. O diretório de build está disponível apenas a partir do código-fonte do Joomlaǃ, não está incluído em uma versão oficial do Joomlaǃ.

As instruções de instalação incluem o seguinte:

```sh
git checkout 5.2-dev (or whatever branch you wish to work with)
composer install
npm ci
```

O comando `npm ci` é um sinônimo para `npm clean-install` usado em qualquer situação onde você queira garantir que está fazendo uma instalação limpa das suas dependências.

## Uso diário

Existem comandos alternativos para uso em situações nas quais apenas arquivos SCSS ou JavaScript foram alterados:

```sh
npm run build:css¶
    This command compiles SCSS files to CSS and also creates the minified files.

npm run build:js¶
    This command compiles and transpiles the JavaScript files to the correct format and creates minified files.
```

Os comandos varrem os diretórios de mídia e modelos e constroem as versões finais dos arquivos exigidos por uma instalação do Joomla.

## Sass, SCSS e CSS

> Sass é uma linguagem de folhas de estilo que é compilada para CSS. Ela permite o uso de variáveis, regras aninhadas, mixins, funções e muito mais, tudo com uma sintaxe totalmente compatível com CSS. O Sass ajuda a manter grandes folhas de estilo bem organizadas e facilita o compartilhamento de design dentro e entre projetos. Arquivos Sass têm o sufixo `scss`.

### CSS

O código CSS é expresso da seguinte formaː

```css
#header {
    margin: 0;
    border: 1px solid blue;
}
#header p {
    font-size: 14px;
    color: blue;
}
#header a {
    text-decoration: none;
}
```

### SCSS

`SCSS` é uma extensão da sintaxe do CSS. É usado no núcleo do Joomlaǃ

```css
$color:    blue;
#header {
    margin: 0;
    border: 1px solid $color;
    p {
        color: $colorRed;
        font: {
            size: 14px;
        }
    }
    a {
        text-decoration: none;
    }
}
```

Uma sintaxe mais antiga do `Sass` oferece uma maneira mais concisa de escrever CSS.

```css
$color:    blue
#header
    margin: 0
    border: 1px solid $color
        p
            color: $colorRed
            font:
                size: 14px
        a
            text-decoration: none
```

Você pode encontrar mais informações na [Documentação Sass](http://sass-lang.com/documentation/syntax/).

## De CSS existente para SCSS

Se você deseja personalizar o template Cassiopeia, é uma boa ideia copiar o template e personalizá-lo depois, para que as atualizações do Joomla! não substituam suas personalizações. Para adicionar seus próprios arquivos CSS a uma cópia do template Cassiopeia, basta renomear seus arquivos `.css` para `.scss`. Assim, você poderá aproveitar os recursos que o SCSS oferece:

- Extensões de linguagem como variáveis, aninhamento e mixins
- Muitas funções úteis para manipular cores e outros valores
- Recursos avançados como diretivas de controle para bibliotecas
- Saída bem formatada e personalizável

Consulte o [site do Sass](https://sass-lang.com/) para obter todos os detalhes.

### Importe seus arquivos .css como arquivos .scss

Supondo que você queira incluir seus arquivos personalizados e que eles sejam analisados pelo compilador SCSS - você NÃO precisa reescrever seu .css para SCSS porque o `.css` simples também funciona. O que você tem que fazer:

- renomeie seus arquivos `.css` para `.scss`
- prefixe-os com um `_`
- adicione uma declaração `@import` no final de `media/templates/site/copy_of_cassiopeia/scss/template.scss` para que ele sobrescreva as declarações existentes:

```css
// Bootstrap functions
@import "../../../media/vendor/bootstrap/scss/functions";
//...
// 50 or more lines of import statements
// Finally add the cassiopeia colours
:root {
  @each $color, $value in $cassiopeia-colors {
    --#{$color}: #{$value};
  }
// More lines containing scss color statements, then your own styles:
@import "mystyles";
}
```

Se você agora compilar o seu arquivo main template.scss, acabará com um main template.css otimizado e minificado. Você também reduziu suas solicitações HTTP e o site deve carregar mais rápido.

*Traduzido por openai.com*

