<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Configurando um Ambiente Local -->

## Guia de Início Rápido

Para uma configuração de teste, você precisará de uma pilha de software que inclua Apache, MySQL e PHP. Os passos necessários estão cobertos em outra parte deste manual. Em caso de dúvida, use uma pilha de software apropriada para sua plataforma.

## Ferramentas Adicionais

Para testar solicitações de pull do Joomla e contribuir para o código central do Joomla, você precisará de ferramentas adicionais, todas facilmente instaláveis, muitas vezes com um comando de uma linha:

1. **Composer** - para gerenciar as dependências PHP do Joomla. Leia a [documentação do Composer](https://getcomposer.org/doc/00-intro.md) para obter ajuda com a instalação.
2. **Node.js** - para compilar os arquivos JavaScript e SASS do Joomla. Leia a [documentação do Node.js](https://nodejs.org/en/) para obter ajuda com a instalação.
3. **Git** - para gerenciamento de versões. Leia a [documentação do git](https://git-scm.com/) para obter ajuda com a instalação.

## Etapas para Configurar um Site de Teste Local

1. Vá para o [repositório Joomla! no GitHub](https://github.com/joomla/joomla-cms).
2. Selecione o ramo do Joomla no qual deseja trabalhar. Isso seleciona as instruções apropriadas do README.
3. Role a página até a seção **Steps to setup the local environment:** do texto do README.
4. Siga as etapas listadas lá. Você pode mudar o nome da pasta clonada joomla-cms para fazer quantos clones quiser para diferentes propósitos.
5. Crie um banco de dados e instale o Joomla, mas não exclua a pasta de instalação ao final do processo de instalação.

## Atualizando o Joomla

De tempos em tempos, você pode precisar atualizar um clone do Joomla. Este é um comando de uma linha que geralmente é rápido. No entanto, normalmente é necessário reconstruir os arquivos CSS e JavaScript, o que é um processo mais complicado e demorado.

Usuários de Linux e OSX podem configurar o seguinte alias bash colocando o seguinte dentro do *arquivo ~/.bashrc*:

```sh
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
```

Isso apagará todos os arquivos compilados e executará uma nova instalação como um único comando chamando `jinstall` dentro da sua instalação Joomla. Você também pode usar o comando `jclean` para trocar de volta para um ramo diferente do Joomla.

**Nota:** Você pode precisar executar `composer install` com a opção `--ignore-platform-reqs` para ignorar os requisitos de plataforma especificados no Composer. Isso, caso você não tenha a extensão LDAP do PHP instalada.

### Alterações no banco de dados

Se uma atualização do Joomla incluir alterações no banco de dados, você pode precisar encontrar as alterações individuais e executá-las manualmente ou começar novamente com um novo clone.

## Scripts de Node/npm

A instalação do Joomla a partir de um clone de repositório usou dois comandos:

- **composer install** Instale todos os pacotes necessários do composer.
- **npm ci** Instale todos os pacotes necessários do npm.

Node.js vem com um gerenciador de pacotes chamado NPM que possui um comando `run`. Alguns scripts estão disponíveis para tornar o processo de construção mais rápido se apenas arquivos CSS ou JavaScript tiverem sido alterados.

### npm run build:css

Este comando compila arquivos SASS para CSS e também cria os arquivos minificados.

### npm run build:js

Este comando compila e transpila os arquivos JavaScript para o formato correto e cria arquivos minificados.

### npm run watch

Este comando é o mesmo que o comando `build:js`, mas irá monitorar alterações e construir automaticamente os arquivos atualizados no diretório de mídia. Arquivos SASS ainda não estão incluídos.

### npm run lint:js

Este comando realiza uma verificação de sintaxe em todos os arquivos JavaScript ES6 em relação ao padrão de código JavaScript. Para mais informações, consulte o [manual de padrões de codificação do Joomla](https://developer.joomla.org/coding-standards/introduction.html).

### npm run test

Este comando executará um conjunto de testes em JavaScript.

## Problemas Possíveis

Ao executar o comando composer install, você pode encontrar estes erros.

```sh
    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
```

A solução é executar o composer install com a opção `--ignore-platform-reqs` para ignorar os requisitos de plataforma especificados no Composer. Ou seja, se você não tiver a extensão LDAP do PHP instalada.

```sh
    composer install --ignore-platform-reqs
```

Se você receber um erro de login, exclua o arquivo `administrator/cache/autoload_psr4.php`.

*Traduzido por openai.com*

