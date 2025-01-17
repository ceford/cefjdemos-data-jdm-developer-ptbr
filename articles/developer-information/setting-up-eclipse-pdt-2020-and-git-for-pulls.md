<!-- Filename: Setting_up_Eclipse_PDT_2020_and_Git_for_Pulls / Display title: Configurando o Eclipse PDT 2020 e o Git para Pulls -->

## Introdução

Comecei a usar o Eclipse há muitos anos para um projeto privado no Joomla e um repositório git remoto privado que não estava no Github. Li os tutoriais disponíveis e continuei feliz até este ano, quando passei a ajudar com testes de núcleo e correção de bugs para o Joomla! 4. E, em seguida, alguns pull requests. Foi então que percebi que não tinha entendido exatamente o que estava fazendo. Finalmente, decidi começar de novo e documentar o processo para outros desenvolvedores semi-experientes que são novos no Joomla, especialmente as partes que eu não valorizei completamente da primeira vez. Primeiro na lista:

## Estrutura de Arquivos

Não é óbvio antes de você baixar e instalar o Eclipse que existem pelo menos dois lugares onde os dados são armazenados, além de onde o código do Eclipse está localizado.

### O Espaço de Trabalho

Este é o local onde o Eclipse armazena seus próprios dados para cada projeto. Não deve estar na árvore da web! Como eu trabalho principalmente em um laptop Mac, o local padrão para mim é **/Users/username/eclipse-workspace**. Há uma subpasta para cada projeto. Então, um poderia ser **/Users/username/eclipse-workspace/joomla-cms**. Ela contém uma pasta: .metadata. Usuários de Linux provavelmente saberão substituir home por Users.

### O repositório local do git e o site de teste

Aqui é onde o código do projeto é armazenado. Ele pode estar na árvore web local, se você quiser usá-lo para testes. Para mim, é **/Users/username/Sites/joomla-cms-4**. Usuários de Linux provavelmente saberão substituir Sites por public_html. Fique atento aos passos adicionais necessários para fazer o repositório local funcionar como um site Joomla.

### Um site de teste separado

Isso é opcional, mas requer que o repositório git tenha um arquivo build.xml personalizado, por exemplo, build-local.xml, abordado no Apêndice.

## Software Necessário

Para configurar um site de desenvolvimento e teste funcional, você precisará primeiro instalar o seguinte software:

- Apache
- MySQL ou Maridb
- PHP
- phpMyAdmin - [Página inicial do PhpMyAdmin](https://www.phpmyadmin.net/)
- Composer - [Página inicial do Composer](https://getcomposer.org/)
- Node.js - [Página inicial do Node.js](https://nodejs.org/en/)
- Eclipse PDT - [Ferramentas de Desenvolvimento PHP do Eclipse](https://www.eclipse.org/pdt/)
- git (opcional para usuários de git na linha de comando) - [Página inicial do git](https://git-scm.com/)
- phing (opcional para builds personalizados) - [Página inicial do phing](https://www.phing.info/)

Os primeiros três ou quatro geralmente vêm como um pacote para a sua plataforma, conhecido como um stack LAMP, WAMP ou XAMP. Basta usar o que sua plataforma oferece ou conferir o Apache Friends [XAMPP](https://www.apachefriends.org/)

Vamos assumir que você instalou tudo, exceto o Eclipse.

## Fazer um fork de joomla-cms no Github

Há um fluxo de trabalho descrito em [Meu primeiro pull request para o Joomla! no Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") que eu não posso elogiar demasiadamente. Ele mostra exatamente o que você precisa fazer:

![Github work flow](../../../en/images/getting-started/core-work-flow-joomla.png)

Passos

- Vá para [Github](https://github.com/) e crie uma conta, rápido e grátis.
- Na sua conta do Github, vá para o repositório joomla/joomla-cms: digite joomla/joo... na caixa de pesquisa no canto superior esquerdo e selecione quando as opções aparecerem.
- No repositório joomla/joomla-cms, clique no botão fork no canto superior direito. Isso dará a você uma cópia bifurcada de todo o código para Joomla 4, Joomla 5, ..., tudo, na sua própria conta do Github.

De volta à sua estação de trabalho.

## Instalar Eclipse

Já tenho duas versões do Eclipse instaladas, Oxygen de alguns anos atrás, e Cocoa de 2020. A partir daqui, estou instalando uma segunda instância do Cocoa. Vamos ver o que acontece:

- Vá para o [site do Eclipse](https://www.eclipse.org/pdt/) e baixe a versão para sua plataforma.
- Siga o procedimento de instalação e, eventualmente, inicie o aplicativo Eclipse, para mim **Eclipse 2.app**.
- Aviso: *Eclipse 2.app* é um aplicativo baixado da internet. Tem certeza de que deseja abri-lo? **Abrir**
- Selecione um diretório como espaço de trabalho - O padrão é /Users/username/eclipse-workspace. **Procurar** e criar uma subpasta joomla-cms-4.
- Lançamento: O Eclipse está instalado e exibe a página de Boas-Vindas.
- Revise as configurações de IDE. Configure os itens 1, 2, 5 e 6.
- Selecione o ícone Workbench no canto superior direito.

Em vez de instalar outra versão do Eclipse, eu poderia ter aberto um novo espaço de trabalho vazio.

Até agora tudo bem!

## Importar fork no Eclipse

Na sua nova instalação local do Eclipse:

- Abra Preferências e defina Equipe / Git / Pasta padrão do repositório para /Users/username/git (você pode ou não usar esse local)
- Abra Arquivo / Importar / Git / Projetos do Git (com importação inteligente). Avançar ...
- Clonar URI. Avançar ...
- Copie a barra de URL do **seu fork do Github** e cole no campo URL. Você não precisa adicionar credenciais de Autenticação. Avançar ...
- Branches para importar ... hum ... Provavelmente é melhor importar tudo. Avançar ...
- Diretório. Aqui você pode escolher um local diferente para manter o código. Eu naveguei até /Users/username/public_html/joomla-cms-4, criando-o se necessário.
- Branch inicial - se você importou todas as branches. Estou trabalhando no Joomla 4, então selecionei 4.0-dev. E notei que meu clone será **origin**. Avançar ...
- Um gole de café. O clone levará alguns minutos.
- Importar fonte. A fonte deve ser o local do código. Concluir.
- O Eclipse agora mostra a raiz da árvore de código no painel Project Explorer. Clique para ver mais da árvore. Observe que este código ainda não funcionará como um site Joomla. Entre outras coisas, não há pasta de mídia.

## Adicionar repositório original joomla-cms ao Eclipse

- Mostre a janela de Repositórios Git: selecione Janela / Mostrar Visualização / Outro
- Selecione Git / Repositórios Git - a janela aparece na parte inferior da tela.
- Expanda joomla-cms / remotos / origem - se você fizer alterações no seu código e enviar para a origem, é para onde ele vai.
- Clique com o botão direito em Remotos e selecione Criar Remoto...
- Defina o nome do Remoto como **joomla-origin** e selecione **Configurar busca**.
- Em Configurar Busca, selecione **Alterar**.
- Em Selecionar um URI, cole a URL do repositório original joomla-cms: `https://github.com/joomla/`
- Deixe as Credenciais em branco. Finalizar.
- em Configurar Busca: **Salvar e Buscar**.

## Criar um site funcional

Sua cópia do código joomla-cms precisa de mais etapas para ser usada como um site.

- Abra um terminal e mude para a pasta que contém seu código clonado.
- Execute composer install:
  - Usuários de Linux e OSX podem configurar o seguinte alias bash colocando o seguinte dentro do arquivo `~/.bash_profile` ou `~/.zsh` (`\$ source ~/.bash_profile` para efetuar imediatamente):
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
    alias jinstall="jclean; composer install --ignore-platform-reqs; npm ci"
```

- composer não está no meu path, então substituí por php ~/composer/composer.phar
- jinstall
- que busca todas as dependências PHP do Joomla, dependências JavaScript, compila todo o JavaScript ES6 e coloca os arquivos em seus locais apropriados.
- golinho de café enquanto as dependências são baixadas e os arquivos de mídia criados.
- No Eclipse, clique com o botão direito no projeto raiz e selecione Atualizar. Você verá que seu código agora tem uma pasta de mídia.
- Se estiver interessado, use um gerenciador de arquivos para ver que a raiz também contém um arquivo chamado .gitignore e a pasta de mídia é mencionada nele. Isso significa que ela não será enviada para os seus repositórios git locais ou remotos clonados.

Você está pronto para uma instalação do Joomla:

- Crie um banco de dados usando o phpMyAdmin (ou a linha de comando do mysql, se preferir).
- Eu criei um banco de dados chamado joomla-cms-4 com collation utf8mb4-unicode-ci.
- Crie um novo usuário: o meu é jcms4 com uma senha aleatória gerada (GAOC26r77bBLkkdA). Você precisa anotar a senha para usar durante a instalação. Ela acaba em texto simples no arquivo de configuração.
- Conceda todos os privilégios no banco de dados para este usuário - o padrão.
- No seu navegador favorito, vá para a raiz do novo site: localhost/joomla-cms-4/

### No meu Mac executando o macOS Catalina

- Configuração do Ambiente Incompleta: Mais detalhes. Bah - apenas conserte e verifique se a barra de URL contém o URL que você digitou - o meu, de alguma forma, adicionou caracteres extras.
- Caso contrário, ele acessa um site em funcionamento - não exclua a pasta de instalação.

### No meu workstation Linux rodando o Linux Mint 20

Oops! Algo deu errado!

O problema é que tenho meu código Joomla no meu espaço de arquivos pessoal para acesso de leitura/gravação pelo Eclipse. O servidor web também precisa de acesso de gravação para escrever arquivos de configuração, cache e log, mas está sendo executado como um usuário e grupo com poucos privilégios. Como estou em uma rede doméstica privada, editei /etc/apache2/apache2.conf para comentar as linhas User \${APACHE_RUN_USER} e Group \${APACHE_RUN_GROUP} e adicionei User myusername e Group mygroupname. Reinicie o apache, então...

- Ele leva a um site funcional - não exclua a pasta de instalação.

## Revisão de Código

Em resumo:

- Buscar do joomla-origin para garantir que meu clone local esteja atualizado.
- Equipe / Mesclar / Selecionar ramificação para mesclar - cuidado - Eu selecionei joomla-origin/4.0-dev
- Enviar para origin para garantir que meu fork remoto esteja atualizado.
- Criar uma ramificação para algumas alterações de código que quero fazer.
- Fazer as alterações de código
- Se as alterações forem em arquivos fonte css ou js (aqueles em sass ou es6) vá para uma janela de terminal e execute jinstall novamente.
- TESTE sua instalação local para ver se há algum problema.
- Cometer as alterações de código
- Enviar para origin para atualizar meu fork remoto com minhas alterações.
- Vá para sua própria conta no Github e selecione a ramificação que você criou com o código atualizado. Algo assustador: ele diz **Esta ramificação está 11084 commits à frente, 134 commits atrás de joomla:staging.** Eu fiz algo errado? Aparentemente não!
- Selecione o botão de Pull Request. Certifique-se de selecionar a ramificação correta do joomla para mesclar. Para mim é 4.0-dev. E certifique-se de ter selecionado sua ramificação com o código alterado. Vá em frente!
- O pull request precisa ser testado e aprovado e pode levar dias, semanas ou meses. E a alteração pode ser rejeitada!

## Enquanto isso

De volta à sua estação de trabalho:

- Volte para a sua ramificação original, para mim: Equipe / Mudar Para / 4.0-dev
- Reconstrua: para mim Projeto / Construir Projeto
- Veja os arquivos previamente alterados sendo copiados novamente para o seu site de trabalho.
- TESTE: seu site de trabalho está de volta ao estado que estava antes das suas alterações de código.

Agora você está pronto para criar outro branch para outro conjunto de alterações de código.

## Se o Desastre Acontecer

Em determinado momento, meu clone local de alguma forma foi corrompido e eu não tinha ideia de como consertá-lo. Então, excluí meu clone local e todos os arquivos associados, esvaziei o banco de dados e voltei para a etapa **Importar fork para o Eclipse** acima. Isso colocou meu clone local em sincronia com meu fork remoto, incluindo quaisquer branches que eu criei para pull requests. A nova instalação funcionou sem problemas e fiquei feliz!

## Outros Recursos

- [Configurando o Eclipse para desenvolvimento Joomla](https://docs.joomla.org/Configuring_Eclipse_for_joomla_development "Special:MyLanguage/Configuring Eclipse for joomla development") (2012-2013) Mas obtenha a versão mais recente do Eclipse PDT!
- [Minha primeira solicitação de pull para o Joomla! no Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") (2011-2014) Boa visão geral, embora um pouco desatualizada.
- [Trabalhando com git e github](https://docs.joomla.org/Working_with_git_and_github "Special:MyLanguage/Working with git and github") (2011-2015)
- [Configurando seu ambiente local](https://docs.joomla.org/J4.x:Setting_Up_Your_Local_Environment "Special:MyLanguage/J4.x:Setting Up Your Local Environment") (2018-2020)
- [Configurando o Eclipse e o Xdebug](https://docs.joomla.org/Configuring_Eclipse_and_Xdebug "Special:MyLanguage/Configuring Eclipse and Xdebug") (2013) Tudo sobre depuração.
- [Trabalhando com Git e Eclipse](https://docs.joomla.org/Working_with_Git_and_Eclipse "Special:MyLanguage/Working with Git and Eclipse") (2014) Método para edições antigas do Eclipse.
- [Executando Testes Automatizados a partir do Eclipse](https://docs.joomla.org/Running_Automated_Tests_from_Eclipse "Special:MyLanguage/Running Automated Tests from Eclipse") (2020) Para usuários avançados?
- [Configurando Xdebug para desenvolvimento PHP/Linux](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development/Linux "Special:MyLanguage/Configuring Xdebug for PHP development/Linux") (2016) Instalar e configurar.
- [Configurando Eclipse IDE para desenvolvimento PHP/Linux](https://docs.joomla.org/Configuring_Eclipse_IDE_for_PHP_development/Linux "Special:MyLanguage/Configuring Eclipse IDE for PHP development/Linux") (2019) Instalar e configurar para Linux.
- [Configurando Xdebug para desenvolvimento PHP](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development "Special:MyLanguage/Configuring Xdebug for PHP development") (2014) Etapas para Linux, Windows e Mac OS X.
- [Webinar: Usando o Eclipse para Desenvolvimento Joomla!](https://docs.joomla.org/Webinar:_Using_Eclipse_for_Joomla!_Development "Special:MyLanguage/Webinar: Using Eclipse for Joomla! Development") (2014) Este webinar em vídeo de 45 minutos, gravado em 30 de abril de 2009, fornece uma visão geral das funcionalidades do Eclipse para desenvolvimento Joomla!
- [Configurando sua estação de trabalho para desenvolvimento Joomla](https://docs.joomla.org/Setting_up_your_workstation_for_Joomla_development "Special:MyLanguage/Setting up your workstation for Joomla development") (2020) Breve visão geral do software necessário e IDEs alternativos.

## Apêndice

Em um determinado momento, eu tinha meu repositório git local fora do diretório raiz da web e precisava copiar quaisquer alterações feitas para um site local instalado separadamente. Isso exigia um arquivo de construção personalizado, mostrado aqui como referência:

### Criar um arquivo build-local.xml

O clone do Eclipse contém um arquivo build.xml, mas ele é usado para testar e criar um arquivo zip para download para uma nova instalação. O que quero fazer é copiar quaisquer alterações que eu fizer no meu código clonado para o meu site de teste no meu laptop. Observe que eu só quero fazer alterações no código PHP e não no Javascript ou CSS. Para isso, criei um arquivo separado chamado build-local.xml na raiz do projeto:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="joomla-cms" basedir="." default="main">
    <property file=".project" />

    <property name="joomladir" value="/Users/username/public_html/joomla-cms"  override="true" />

    <property name="srcdir" value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="${srcdir}" id="allfiles">
        <include name="administrator/**" />
        <include name="api/**" />
        <include name="cli/**" />
        <include name="components/**" />
        <include name="images/**" />
        <include name="includes/**" />
        <include name="language/**" />
        <include name="layouts/**" />
        <include name="libraries/**" />
        <include name="modules/**" />
        <include name="plugins/**" />
        <include name="templates/**" />
        <include name="index.php" />

        <exclude name="**/.*" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${joomladir}">
            <fileset refid="allfiles" />
        </copy>
    </target>
</project>
```

### Adicionar uma ferramenta de build

- Clique com o botão direito no diretório raiz do projeto e selecione Propriedades.
- Selecione Construções / Novo / Programa / OK.
- Nome: Compilação local com phing
- Localização: onde quer que o phing esteja instalado. Para mim, está em /usr/local/php5/bin/phing, mesmo estando usando o PHP 7.4.
- Diretório de Trabalho: Navegue pelo Espaço de Trabalho e selecione joomla-cms - aparece como \${workspace_loc:/joomla-cms}
- Argumentos: -f build-local.xml
- Aplicar / OK
- Em Construções: Aplicar e Fechar
- Projeto / Compilar Projeto
- Veja o que acontece:

```sh
    Buildfile: /Users/username/git/joomla-cms/build-local.xml
     [property] Loading /Users/username/git/joomla-cms/.project

    joomla-cms > main:

    BUILD FINISHED

    Total time: 0.2121 seconds
```

*Traduzido por openai.com*

