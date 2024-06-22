<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Configurando seu ambiente local -->

<span id="main-portal-heading">Tutorial  
Como configurar um ambiente local para o Joomla 4</span> Joomla!  4.x

Com o Joomla! 4 nós mudamos o processo de desenvolvimento. Não é mais
possível clonar o repositório e ter uma instalação utilizável do Joomla.
Seguimos as melhores práticas e implementamos um processo de construção
para o sistema de gerenciamento de conteúdo (cms).

## Guia de início rápido

Os passos para configurar seu ambiente de desenvolvimento dependem de
seu sistema operacional. Não podemos escrever documentação para cada
sistema operacional (os), use seu mecanismo de pesquisa favorito para
encontrar um tutorial.

### Ferramentas que você precisa

1.  PHP - basicamente o mesmo que você precisa para executar um site
    Joomla, mas você precisa da versão CLI (interface de linha de
    comando) do PHP. (Veja a página [Configurando um servidor LAMPP para
    desenvolvimento
    PHP](https://docs.joomla.org/Configuring_a_LAMPP_server_for_PHP_development "Special:MyLanguage/Configuring a LAMPP server for PHP development").)
2.  Composer - para gerenciar as dependências PHP do Joomla. Para obter
    ajuda na instalação do Composer, leia a documentação em
    <a href="https://getcomposer.org/doc/00-intro.md" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">https://getcomposer.org/doc/00-intro.md</a>.
3.  Node.js - para compilar os arquivos JavaScript e SASS do Joomla.
    Para obter ajuda na instalação do Node.js, siga as instruções
    disponíveis em
    <a href="https://nodejs.org/pt-br/" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">https://nodejs.org/pt-br/</a>.
    Observe que você precisará do NodeJS 12 ou superior para instalar o
    Joomla.
4.  Git - para gerenciamento de versões.

### Passos para configurar o ambiente local

1.  Clonar o repositório;
2.  selecionar a ramificação *4.1-dev*;
3.  executar `composer install` a partir da raiz do repositório git;
    (Você pode adicionar *--ignore-platform-reqs* se você não tiver o
    PHP-LDAP instalado localmente e não precisar dele.)
4.  executar `npm ci` na raiz do repositório git. (Observe que você
    precisa do npm 6.13.4 ou superior para isso. Execute
    `npm install -g npm@lts` para atualizar sua versão do npm para a
    versão LTS.)

Usuários do Linux e do OSX podem configurar o seguinte alias bash
colocando o seguinte dentro do arquivo *~/.bashrc*:

    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install; npm ci"

Isso excluirá todos os arquivos compilados em seu sistema e executará
uma nova instalação como um comando chamando `jinstall` dentro de sua
instalação do Joomla. Você também pode usar o comando `jclean` para
voltar para uma ramificação do Joomla 3.x

## Guia de início um pouco mais longo

O Joomla é semelhante à muitas outras ferramentas da web nos dias de
hoje. Tem uma grande parte PHP e tem cada vez mais código JavaScript.
Embora a codificação PHP não precise de muita preparação, o JavaScript
precisa de muitas ferramentas. A principal razão é que ninguém escreve
código de uma maneira que todos os navegadores entendam, então o código
precisa ser transpilado de, por exemplo, ES6 para uma versão compatível
de JavaScript. O mesmo vale para CSS. Para Joomla, estamos usando SASS e
isso será convertido em CSS nativo para que qualquer navegador o
entenda. No lado negativo, configurar um ambiente de desenvolvimento é
um pouco mais complicado, mas as ferramentas tornam a codificação mais
conveniente. Graças aos observadores e ao recarregamento automático do
navegador, você pode ver suas alterações em tempo real.

### PHP

Deve ser o suficiente para executar `composer install`, pois isso
instalará as dependências do PHP salvas no arquivo *composer.lock*. Você
pode fazer isso quantas vezes quiser. Ele só instalará novos pacotes
quando o arquivo *composer.lock* for alterado. Não execute
`composer update` pois isso atualizará todos os pacotes para versões
mais recentes e atualizará o arquivo *composer.lock*.

**Observação:** Você pode precisar executar `composer install` com a
opção `--ignore-platform-reqs` para ignorar os requisitos de plataforma
especificados no Composer. Ou seja, se você não tiver a extensão LDAP do
PHP instalada.

### Scripts node/npm

O Node.js vem com um gerenciador de pacotes chamado NPM (em alguns
aspectos o mesmo que o Composer). O NPM possui um comando `run` e
preparamos alguns scripts para facilitar sua vida. Você precisa executar
os comandos para a raiz do repositório ao alterar os arquivos JS ou
SASS. Anteriormente, você precisa(va) executar `npm ci` uma vez, para
instalar dependências.

#### npm run build:css

Ele compilará arquivos SASS para CSS e também criará os arquivos
minificados.

#### npm run build:js

Ele irá compilar e transpilar os arquivos JavaScript para o formato
correto e criar arquivos minificados.

#### npm run watch

Este é o mesmo que o comando `build:js`, mas observará as alterações e
criará automaticamente os arquivos atualizados no diretório de mídia. Os
arquivos SASS ainda não estão incluídos.

#### npm run lint:js

Isso executará uma verificação de sintaxe em todos os arquivos
JavaScript ES6 em relação ao padrão de código javascript (para obter
mais informações sobre o padrão de estilo de código do Joomla 4, leia o
<a
href="https://developer.joomla.org/coding-standards/introduction.html%7C"
class="external text" target="_blank" rel="noreferrer noopener">manual
de padrões de codificação</a>.

#### npm run test

Isso executará um conjunto de testes JavaScript.

## Possíveis problemas

Ao executar o composer install você pode encontrar esses erros

    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.

A solução é executar composer install com a opção
`--ignore-platform-reqs` para ignorar os requisitos de plataforma
especificados no Composer. Ou seja, se você não tiver a extensão LDAP do
PHP instalada.

    composer install --ignore-platform-reqs

Se você receber um erro de início de sessão como mostrado abaixo, exclua
o arquivo `library/autoload_psr4.php` conforme mostrado na segunda
imagem.

<img
src="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/400px-Install-error.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/600px-Install-error.png 1.5x, https://docs.joomla.org/images/thumb/b/b3/Install-error.png/800px-Install-error.png 2x"
data-file-width="1920" data-file-height="1080" width="400" height="225"
alt="Login After Install Error" />
