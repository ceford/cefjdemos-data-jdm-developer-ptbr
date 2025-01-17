<!-- Filename: Working_with_git_and_github / Display title: Trabalhando com git e github -->

## Introdução

Este documento fornecerá informações sobre como contribuir para o Joomla! CMS com a ajuda do Git e GitHub. Se você gostaria de fazer uma alteração simples (apenas um arquivo), é mais fácil usar esta documentação: [Using the Github UI to Make Pull Requests](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests). Se você quer adicionar alterações mais complexas ou está apenas interessado nisso, continue lendo!

## O que são Git e GitHub?

Git é um sistema de controle de versão distribuído. É um sistema que registra mudanças em arquivos e mantém essas mudanças em um arquivo de histórico. Você pode sempre voltar a uma versão anterior do seu código e restaurar as alterações, se desejar. Por causa do arquivo de histórico, o Git é muito útil quando você trabalha com muitas pessoas juntas no mesmo projeto.

Aqui está como o GitHub pode ser usado. [GitHub](https://www.github.com) é um site que ajuda a gerenciar Projetos Git de maneira visual. Como proprietário de um projeto, você pode alterar o código e comparar diferentes versões. Como usuário do projeto, você pode contribuir fazendo Pull Requests. Um pull request é uma solicitação ao proprietário para incorporar algum código ao projeto. Você está oferecendo um pedaço de código (talvez uma solução para um bug) e perguntando se o proprietário do projeto gostaria de usá-lo. Se o proprietário gostar, ele pode mesclá-lo (adicioná-lo) ao seu projeto.

O Joomla! usa o GitHub e o Git para manter seu código. Qualquer pessoa pode contribuir para o software Joomla! através do seu [repositório no GitHub](https://github.com/joomla/joomla-cms).

## Inscreva-se no GitHub e instale o Git

Primeiro, você precisará [registrar-se no Github](http://www.github.com). É gratuito e fácil de fazer. Basta seguir os passos fornecidos.

Uma vez inscrito, você precisa instalar o Git no computador que você usa para desenvolvimento (estação de trabalho ou laptop). Siga as instruções de instalação no site do [git](http://git-scm.com). O Git é um programa CLI (Interface de Linha de Comando). No início, isso pode ser confuso e um pouco assustador, mas este documento irá guiá-lo pelo processo.

Se você não é um usuário avançado, basta executar o instalador e pressionar os botões "próximo" até que o programa seja instalado. O Git não danificará seu sistema. Mais tarde, você pode removê-lo como qualquer outro programa.

Com o Git instalado, abra um aplicativo de Terminal. Comece configurando seu nome e endereço de e-mail do Git. O Git usará essas informações quando você contribuir para um projeto:

```sh
    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com
```

Nos comandos acima, assim como em todos os outros comandos fornecidos nesta documentação, cada linha é um novo comando. Portanto, você digita a primeira linha, pressiona enter e depois digita a segunda linha e pressiona enter.

Você agora está pronto para usar o Git e avançar com a configuração da sua instalação de teste.

## Configurando uma instalação de teste

Para a sua instalação de teste, você precisa de um conjunto de software para que possa instalar e executar o Joomla! no seu computador. Conjuntos como MAMP, XAMP ou WAMP são abordados em outra parte desta documentação.

Após a instalação do seu stack de software, você precisa instalar a versão mais recente do Joomla!. Esta não é a última versão estável. A última versão do Joomla! é o branch de staging no GitHub.

## Fork e Clone Joomla!

No GitHub, você pode encontrar projetos em chamados Repositórios. Dentro de um projeto, você pode encontrar várias versões. Tal versão é chamada de Branch. Joomla! possui as seguintes branches:

- **4.2-dev** Arquivos para desenvolvimento da versão atual.
- **4.3-dev** Arquivos para desenvolvimento da próxima versão menor.
- **4.4-dev** Arquivos para desenvolvimento da segunda próxima versão menor.
- **5.0-dev** Arquivos para desenvolvimento da próxima versão principal.

No seu computador de teste, você vai usar o branch **4.2-dev**. No entanto, você não pode modificar este branch porque não é o proprietário. Você precisa fazer uma cópia dele. No GitHub isso é chamado de Fork. Você é o proprietário dessa cópia, então pode modificá-la. Após modificar o seu fork, você pode fazer um Pull Request para as alterações que fez. Falaremos mais sobre isso depois. Você pode fazer Fork de um branch pressionando o botão Fork no [Repositório do Joomla! CMS no Github](https://github.com/joomla/joomla-cms). Este botão está localizado no canto superior direito da página.

![Fork joomla in github](../../../en/images/getting-started/core-git-fork-joomla.png)

Após fazer o fork, você precisa instalar o Joomla! no seu computador local. Vá para a pasta onde você pode colocar arquivos usados pelo seu servidor Web. Muitos programas usam uma pasta chamada `htdocs`. Alguns usam `www` e outros usam pastas completamente diferentes. Tudo depende de se você está usando Windows, Mac ou Linux. Com o tempo, sua raiz web conterá diferentes pastas para diferentes sites. Uma vez dentro da sua pasta raiz web. Ou, em uma janela de Terminal aberta, use o comando cd para mudar o diretório atual para a raiz web. Ou, em seu explorador de arquivos GUI, localize a pasta raiz web, pressione o botão direito do mouse e clique em: "Git Bash Here" ou "Open Terminal" ou algo semelhante.

No Terminal, com a pasta raiz do site definida como a pasta atual, digite o seguinte comando para baixar os arquivos do seu Fork do Joomla! CMS para o seu computador. Substitua *username* pelo nome de usuário que você está usando no GitHub.

```sh
    git clone https://github.com/username/joomla-cms.git
```

O processo de clonagem pode demorar um pouco. Quando concluído, sua raiz web conterá uma pasta chamada joomla-cms. Você precisa tornar essa pasta a pasta atual para executar comandos git para essa pasta:

```sh
    cd joomla-cms
```

Lembre-se disso para outros comandos nesta documentação.

## Instalar o Joomla!

Seu clone baixado do seu repositório bifurcado precisa de mais ações antes de estar pronto para uso. Um dos arquivos baixados é chamado README.md. Abra-o com um editor de texto e siga as instruções em **How to get a working installation from the source**.

Quando estiver pronto, abra seu navegador e acesse a instalação no seu localhost. Normalmente, a URL é algo como: `http://localhost/joomla-cms`. Agora você verá o processo de instalação padrão do Joomla!.

## Fazer Alterações de Código

Todas as alterações que você fizer no código Joomla no seu site local serão registradas e monitoradas pelo Git. A qualquer momento, você pode digitar o comando `git status` para ver quais arquivos estão modificados ou não rastreados. Não rastreado significa que o arquivo nessa localização é novo para o Git.

Neste estágio, é provavelmente melhor usar um Ambiente de Desenvolvimento Integrado (IDE) para trabalhar no código do Joomla. O Visual Studio Code (VSCode) é altamente recomendado. É gratuito e funciona em todas as plataformas. Usando o VSCode, você pode fazer alterações, **Commit** essas alterações no seu clone local do Git e **Push** elas para seu fork remoto no GitHub.

## Enviar Alterações para o Fork

Fazer upload das alterações é chamado de `push` em termos do Git. Antes de fazer isso, você precisa fazer algo muito importante. Você deve criar um novo branch para suas alterações. (Um branch é uma versão do seu projeto, lembra?) Se você não fizer isso e fizer sua alteração diretamente no branch atual, na primeira vez não haverá problema. Mas se você fizer alterações pela segunda vez, e as alterações que você fez na primeira vez ainda não foram mescladas, todas essas alterações também serão registradas como novas alterações.

Você pode configurar o VSCode para realizar todos os seguintes comandos com alguns cliques. Mas se você quiser usar comandos git a partir da linha de comando do terminal:

Portanto, o primeiro comando que você vai executar criará um novo branch. Substitua nome-novo-branch pelo nome do novo branch. Esse nome deve ser curto e pode conter apenas letras minúsculas e números. **NÃO** use espaços. Em vez de espaços, use - (menos).

```sh
    git checkout -b name-new-branch
```

O próximo comando informa ao git que todas as alterações estão prontas para o commit.

```sh
    git add --all
```

O seguinte comando adiciona sua alteração ao branch. Por favor, substitua a mensagem por uma breve descrição de suas alterações. Esta descrição será o título da pull request que você irá fazer.

```sh
    git commit -m "description"
```

O comando final vai enviar (fazer upload) das alterações para o seu fork. Por favor, substitua name-new-branch pelo nome da branch que você criou alguns passos acima.

```sh
    git push origin name-new-branch
```

## Compare Forks e Faça um Pull Request

Após enviar sua alteração para o GitHub, vá para o seu fork do Joomla! CMS no site do GitHub. Selecione seu branch e faça um pull request.

Quando terminar, em seu clone local, volte para a ramificação original:

```sh
git checkout 4.2-dev
```

Agora você pode fazer novas alterações sem afetar as mudanças no seu branch anterior.

## Mantendo-se atualizado

Como a versão atual do Joomla! pode mudar todos os dias, é importante manter o seu fork atualizado. Você pode fazer isso adicionando o repositório original do Joomla ao seu projeto fork:

```sh
    git remote add upstream https://github.com/joomla/joomla-cms.git
```

Em seguida, atualize seu clone local com o seguinte comando:

```sh
    git pull upstream 4.2-dev
```

E atualize seu fork remoto:

```sh
    git push
```

*Traduzido por openai.com*

