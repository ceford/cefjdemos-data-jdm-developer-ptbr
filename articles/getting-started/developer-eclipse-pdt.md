<!-- Filename: J4.x:Developer:_Eclipse_PDT / Display title: Eclipse PDT -->

## Introdução

Como Desenvolvedor PHP, você precisará de algumas ferramentas para ajudar em seu trabalho. Eclipse é um IDE (Ambiente de Desenvolvimento Integrado) que pode ser usado para todos os tipos de projetos em todas as linguagens de programação. Eclipse PDT (Ferramentas para Desenvolvedores PHP) inclui as extensões necessárias para o desenvolvimento em PHP. Algumas das funcionalidades mencionadas na sua página inicial incluem:

- Destaque de Sintaxe
- Validação de Sintaxe
- Assistência de Conteúdo
- Navegação de Código
- Depuração PHP (Zend Debugger / Xdebug)
- Perfil de PHP (Zend Debugger / Xdebug)

Adicione a isso a capacidade de copiar arquivos para onde eles precisam estar em seu site de teste local e criar um arquivo zip, há material suficiente para trabalhar durante anos. Aprender a usar o Eclipse PDT leva tempo, um dia ou mais, mas vale a pena. Existem outras IDEs e editores de código disponíveis!

### Alternativas

**IDEs** têm todos os recursos necessários para construir projetos PHP. Exemplos multiplataforma conhecidos por serem usados por alguns desenvolvedores do Joomla incluem:

- **JetBrains PhpStorm** Comercial, Multiplataforma.
- **Apache NetBeans** Gratuito, Código Aberto, Multiplataforma.
- **Visual Studio Code** Gratuito, Multiplataforma.

**Editores PHP** são bons para editar código, mas faltam alguns dos recursos avançados necessários para grandes projetos. Exemplos incluem:

- **Notepad++** Gratuito, apenas para Windows.

## Instalar Eclipse PDT

Vá para o site do [Eclipse PDT](https://www.eclipse.org/pdt/) e faça o download da versão disponível para sua plataforma (Linux, Mac ou Windows).

Siga as instruções de instalação para sua plataforma.

## O Espaço de Trabalho do Eclipse

Há pelo menos três lugares diferentes onde o código do seu projeto estará localizado:

- Arquivos de **origem do projeto** são aqueles que você cria e edita por conta própria. Eles não devem estar na sua árvore web. Eles devem estar no seu espaço de arquivos pessoal. Exemplos, onde cada projeto estará em uma subpasta separada:
  - /Users/username/git (Mac)
  - /home/username/git (Linux)
  - ... (Windows)
- A localização da **árvore web** depende da instalação do seu conjunto de softwares. Você pode usar seu próprio espaço de arquivos. Exemplos:
  - /Users/username/Sites/j4test (Mac)
  - /home/username/public_html/j4test (Linux)
  - ... (Windows).
- O **Espaço de Trabalho do Eclipse** é onde o Eclipse armazena informações sobre projetos individuais. A localização padrão é a raiz do seu próprio espaço de arquivos. Exemplos:
  - /Users/username/eclipse-workspace (Mac)
  - /home/username/eclipse-workspace (Linux)
  - ... (Windows).

Pronto para começar o Eclipse?

## Iniciar Eclipse

Após a inicialização, você será solicitado a confirmar várias configurações. Em algum momento, você será solicitado a escolher um espaço de trabalho. O padrão é /home/username/eclipse-workspace, mas você deseja criar uma subpasta para cada projeto, então selecione o botão Procurar e, em seguida, o botão Nova pasta. No exemplo abaixo, criei uma pasta chamada j4tutorials.

![Choose eclipse workspace location](../../../en/images/getting-started/eclipse-pdt-choose-workspace.png)

Selecione o botão **Iniciar**. Se algo estiver errado, você pode selecionar Arquivo / Alternar Espaço de Trabalho em um estágio posterior e fazê-lo novamente. Você também pode excluir espaços de trabalho não utilizados depois.

Ao iniciar pela primeira vez, você verá uma tela de boas-vindas. Você também pode acessar essa tela através do menu **Ajuda / Boas-vindas**.

![Eclipse welcome screen](../../../en/images/getting-started/eclipse-pdt-welcome.png)

Comece com Revisar Configurações de IDE. Você pode marcar ou desmarcar qualquer uma que pareça apropriada, ou desmarcar, desfazer a marcação ou seguir em frente se não tiver certeza. Você também pode mudar as configurações novamente mais tarde.

## Criar um novo projeto PHP

O primeiro projeto a ser adicionado é o código do site de teste. Você precisará disso para verificar se seus próprios arquivos estão sendo copiados para os lugares corretos e para depuração mais tarde. Selecione o item na tela de boas-vindas. Se você avançou para a área de trabalho do Eclipse, selecione Criar um Projeto PHP no Project Explorer. No formulário Novo Projeto PHP:

- Insira um Nome de Projeto adequado. Deve ser uma palavra curta que aparecerá no Explorador de Projetos.
- Selecione o botão de rádio **Criar projeto em localização existente (de fonte existente)**.
- **Navegue** até a localização do seu site de teste Joomla e selecione-o.
- Selecione **Concluir**.

![Eclipse new php project](../../../en/images/getting-started/eclipse-pdt-new-project.png)

Seus arquivos do site serão adicionados ao Project Explorer. Pode levar um ou dois minutos para o Eclipse concluir esse processo, pois ele faz algumas preparações nos bastidores. Você pode selecionar o Projeto para expandir o primeiro nível da pasta.

Dois arquivos serão adicionados à sua pasta do site: .buildpath e .project. Como eles começam com um ponto (.), você pode não vê-los e não precisa se preocupar com eles. Eles são arquivos xml contendo informações usadas pelo Eclipse.

## A Perspectiva do PHP

A coleção de painéis na tela do Eclipse é conhecida como uma perspectiva. As duas mais comumente usadas são a Perspectiva PHP e a Perspectiva de Depuração. Você pode alternar entre elas usando os ícones no canto superior direito da Barra de Ferramentas. Você também pode usar o menu: Janela / Perspectiva / Abrir Perspectiva / PHP ou Depuração.

As ilustrações a seguir mostram a Perspectiva PHP com alguns formulários de edição abertos.

![Eclipse php perspective](../../../en/images/getting-started/eclipse-pdt-php-perspective.png)

Os painéis do Eclipse:

- O painel à esquerda é o Explorador de Projetos, onde você pode navegar pela estrutura de arquivos.
- O painel central superior é onde aparecem os editores de texto abertos.
- O painel superior direito normalmente mostra um esboço do painel de edição selecionado. Ele pode mostrar outras informações.
- O painel inferior direito mostra qualquer uma de várias Visualizações, selecionáveis no menu Janela / Mostrar Visualização.

A visualização de Problemas indica Erros (100 de 791 itens) e Avisos (100 de 11629). Parece muito, mas não há motivo para se preocupar.

## Um novo projeto Hello Universe em PHP

Você está agora pronto para criar um novo Projeto PHP para a extensão que deseja desenvolver. Para ilustrar, você pode começar com um componente simples chamado com_hellouniverse. Para mim, o código do Hello Universe estará localizado em /Users/username/git/j4xtutorials-com-hellouniverse e usarei j4xtutorials como minha afiliação de namespace (a primeira palavra no espaço de nome do componente).

No menu do Eclipse, selecione Arquivo / Novo / Projeto PHP. O formulário é o ilustrado acima. Meus dados:

- Nome do projeto: HelloUniverse
- Conteúdo: selecione Criar projeto em local existente (de fonte existente).
- Navegar: Navegue até /Home/username/git e selecione o botão Nova Pasta.
- No diálogo Nova Pasta, insira j4xtutorials-com-hellouniverse e pressione Criar.
- Com essa pasta vazia aberta, selecione Abrir.
- O diretório selecionado deve ser: /Users/username/git/j4xtutorials-com-hellouniverse
- Selecione Concluir.

Agora você verá o novo projeto no Project Explorer junto com o projeto do seu site. O novo projeto contém dois itens, ambos relacionados ao suporte PHP. Existem alguns itens ocultos usados pelo Eclipse: .settings (pasta), .buildpath e .project.

## Adicionar pasta com_hellouniverse

Com o projeto HelloUniverse selecionado:

- Selecione o item de menu Arquivo ou clique com o botão direito no nome do projeto.
- Selecione os itens de menu Novo / Pasta.
- No formulário Nova Pasta, insira com_hellouniverse no campo Nome da pasta:.
- Selecione Concluir.

A pasta com_hellouniverse é onde o código da sua extensão será colocado. Qualquer coisa dentro dessa pasta acabará em um arquivo zip que você pode usar para instalar no Joomla. Qualquer coisa fora dessa pasta é usada para outros fins. Dois arquivos comuns neste nível:

- README.md é um arquivo de texto formatado em Markdown que descreve o componente. Esses arquivos são comumente usados em conjunto com armazenamento de código remoto, como no Github (mais sobre isso em outro lugar).
- build.xml é um arquivo que contém instruções sobre o que fazer após você ter feito alterações no código. Mais comumente, você desejará copiar arquivos alterados para os locais corretos na estrutura do seu site e regenerar o arquivo zip. Então, para a maioria das alterações, você pode simplesmente recarregar a página da web em que está trabalhando. Você não precisa reinstalar o componente. Isso economiza um tempo enorme!

## Adicionar pastas de administrador, site e mídia

- Selecione a pasta com_hellouniverse e use os itens de menu Arquivo / Novo / Pasta para criar uma nova pasta chamada admin.
- Novamente, desta vez crie uma nova pasta chamada media. CUIDADO: certifique-se de que o pai seja com_hellouniverse e não HelloUniverse.
- Novamente, desta vez crie uma nova pasta chamada site. Se criada no lugar errado, ela pode ser movida para o lugar correto.

## build.xml

O código a seguir contém um exemplo de arquivo build.xml para ser colocado na raiz do seu projeto HelloUniverse.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="hellouniverse" basedir="." default="main">
    <property file=".project" />

    <!-- Source and Destinations -->

    <property name="package"  value="${phing.project.name}" override="true" />
    <property name="srcdir" value="${project.basedir}/com_hellouniverse" override="true" />
    <property name="sitedir" value="/Users/username/Sites/j4tutorials"  override="true" />
    <property name="zipsdir" value="/Users/username/zips"  override="true" />

    <!-- Filesets -->

    <fileset dir="${srcdir}/admin" id="adminfiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/media" id="mediafiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/site" id="sitefiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}" id="allfiles">
        <include name="**" />
    </fileset>

    <!-- Targets -->

    <target name="main" description="main target">
        <copy todir="${sitedir}/administrator/components/com_hellouniverse">
            <fileset refid="adminfiles" />
        </copy>
        <copy todir="${sitedir}/media/com_hellouniverse">
            <fileset refid="mediafiles" />
        </copy>
        <copy todir="${sitedir}/components/com_hellouniverse">
            <fileset refid="sitefiles" />
        </copy>
        <zip destfile="${zipsdir}/com_hellouniverse.zip">
            <fileset refid="allfiles" />
        </zip>
    </target>
</project>
```

Algumas explicações:

- **srcdir** é a localização no sistema de arquivos do seu código-fonte, de onde os arquivos são copiados.
- **sitedir** é a localização no sistema de arquivos do seu site de teste, para onde os arquivos são copiados.
- **zipsdir** é uma localização para o arquivo zip - acho conveniente manter arquivos zip de projetos diferentes juntos em uma pasta de zips.
- **Filesets** são grupos de origem a serem copiados para diferentes destinos. significa todas as pastas e arquivos.
- **Targets** são os destinos e o que precisa ser copiado para lá.

Crie um arquivo build.xml contendo o código acima:

- Selecione Arquivo / Novo / Arquivo XML no menu do Eclipse.
- Certifique-se de que HelloUniverse esteja selecionado como o pai e insira o nome build.xml (exatamente assim).
- Selecione Concluir
- Na Visualização de Código-Fonte (escolha no canto inferior esquerdo), copie e cole o código acima no lugar da linha que já está lá.
- Salve.

Para fazer isso funcionar, você precisa de uma ferramenta de build: Phing.

## A Ferramenta de Construção Phing¶

Vá para o site do [Phing](https://www.phing.info/) e baixe o arquivo Phar. Você pode usar este [link direto](https://www.phing.info/get/phing-latest.phar). Salve o arquivo em uma pasta phing na sua pasta pessoal bin /Users/username/bin/phing. Altere as permissões do arquivo para 755 para que ele possa ser executado a partir da linha de comando. Para testar, abra um terminal, mude para sua pasta bin/phing e digite ./phing-latest.phar -v para ver o número da versão. Lembre-se do caminho completo para o arquivo:

- /Usuários/nomeusuario/bin/phing/phing-ultimo.phar

Você precisará disso mais tarde.

## Preferências do Eclipse - Construtores

Aqui é onde você configura o Phing para compilar seu projeto sempre que fizer alterações no código.

- No Project Explorer, selecione o projeto HelloUniverse.
- Selecione os itens de menu Arquivo / Propriedades.
- Na caixa de diálogo Propriedades, selecione Construtores e depois o botão Novo.
- Na caixa de diálogo Tipo de Configuração, selecione Programa e depois OK.
- Na caixa de diálogo Editar propriedades de configuração de lançamento, insira um título adequado: Build HelloUniverse.
- No campo Localização, insira o caminho para o programa phing ou use o botão Navegar no Sistema de Arquivos para procurá-lo: /Users/ceford/bin/phing/phing-latest.phar
- Para o campo Diretório de trabalho, selecione o botão Navegar no espaço de trabalho e depois selecione HelloUniverse.
- Clique em OK, depois em Aplicar e Fechar.

Para testar: selecione os itens de menu Projeto / Construir Projeto. A visualização do Console na parte inferior da tela mostrará o progresso da construção. Nesta etapa, é provável que mostre BUILD FAILED em vermelho. Isso é esperado - as pastas de componentes serão criadas na instalação do arquivo zip. Ainda não existe porque não há código. Mas isso mostra que a ferramenta de construção Phing está funcionando.

## Como Depurar

Quando algo dá errado, você pode querer percorrer o código linha por linha para ver o que realmente está acontecendo. Se você configurou o Sistema de Depuração como Sim e Relatório de Erros como Máximo na Configuração Global do Joomla, pode ter um rastreamento de pilha para mostrar onde um erro está ocorrendo no código. Ou você pode estar vendo algo incomum na saída e ter uma boa ideia de onde a falha pode estar. Em qualquer dos casos, você precisa definir pontos de interrupção onde a execução irá pausar para permitir que você veja os valores das variáveis e avance pelo código linha por linha.

### Definir um Ponto de Interrupção

Os pontos de interrupção devem ser definidos no projeto do site, J4Tutorials. Como exemplo, no painel Project Explorer, expanda o projeto J4Tutorials e encontre administrator / components / com_users / src / Model e clique duas vezes em UsersModel.php para abri-lo em uma janela de edição. Vá para a linha 87 e clique duas vezes no número 87. Um pequeno marcador azul aparece para indicar que um ponto de interrupção foi definido. A execução do código deve parar aqui quando você visualizar a lista de Usuários através dos itens de menu Administrator / Users / Manage.

### Criar uma Configuração de Depuração

No menu superior, selecione Executar / Configurações de Depuração. No formulário, insira um título reconhecível, por exemplo Debug Admin, para selecionar mais tarde em uma lista suspensa. Certifique-se de que o arquivo selecionado seja /J4Tutorials/administrator/index.php. No painel Comum do formulário, selecione Depurar na lista de menu Exibir nos favoritos. Verifique se o Depurador está configurado para Xdebug. Selecione Aplicar e Fechar.

Você pode agora selecionar Debug Admin na lista suspensa de depuração da Barra de Ferramentas, o ícone do símbolo de bug.

![Eclipse debug tools](../../../en/images/getting-started/eclipse-pdt-debug-tools.png)

Ícones de depuração, passe o mouse para ver os rótulos:

- **Pular Todos os Pontos de Interrupção** Um botão para pular pontos de interrupção, exceto a primeira linha de index.php.
- **Retomar** Continuar a execução normal até o próximo ponto de interrupção.
- **Finalizar** Parar a depuração (mas você também precisa remover o cookie do Xdebug).
- **Entrar** Em uma linha contendo uma chamada de função, entrar nessa função.
- **Passar Sobre** Em uma linha contendo uma chamada de função, não entrar nessa função.
- **Sair de** Em uma função, pular a parada em cada linha e retornar ao chamador.

### Procedimento de Depuração

Ao iniciar uma sessão de depuração, a execução é pausada na primeira linha de administrator/index.php. Essa é a página do Painel de Controle Inicial. Você precisa selecionar o símbolo Retomar (barra amarela e triângulo verde para a direita). Após um atraso de cerca de um segundo, há mais atividade e mais lançamentos remotos pausados no painel esquerdo Depurar Admin. Esta é a página Inicial usando ajax para buscar informações de atualização. Continue clicando em Retomar até que a atividade pare. Então, em seu navegador, selecione o item Usuários no Painel de Controle Inicial. O Eclipse entra em ação e para a execução no ponto de interrupção que você definiu.

![Eclipse PHP debug perspective](../../../en/images/getting-started/eclipse-pdt-debug-perspective.png)

Recursos a serem observados:

- A linha de código atual está marcada com um fundo verde pálido.
- O painel à esquerda mostra um rastreamento de pilha - as chamadas de função anteriores que levaram à função atual. Selecione qualquer item para ver o código pai.
- O painel à direita pode mostrar variáveis ativas ou pontos de interrupção. Expanda os objetos conforme necessário para ver detalhes.

### Alguns problemas

- Falha ao parar em pontos de interrupção: Isso acontece se você abrir outro programa PHP, como o phpMyAdmin. Para corrigir:
  - Vá para Executar / Configurações de Depuração e selecione seu item Debug Admin.
  - Na aba Depurador, selecione Configurar...
  - Em Mapeamento de Caminho, selecione e remova qualquer caminho não relacionado ao seu site de testes.
  - Selecione Concluir - você pode precisar Terminar e recarregar a sessão de depuração.
- Depuração retoma após selecionar o botão Terminar. Para corrigir:
  - Use as Ferramentas de Desenvolvedor do seu navegador para abrir uma janela de inspeção.
  - Encontre os cookies que seu navegador está usando (Armazenamento no Firefox)
  - Selecione e exclua o cookie XDebug.

## Usando Git

Git é um sistema de controle de versão amplamente utilizado para gerenciar qualquer tipo de texto, principalmente código, mas pode ser qualquer coisa. Ele funciona a partir de uma linha de comando, mas geralmente é invocado por ferramentas gráficas como Eclipse. Se você gostaria de ler sobre o git, veja o [site do git](https://git-scm.com/).

Se você quiser usar o git, você precisa instalá-lo para sua plataforma. Em seguida, no Eclipse, selecione seu projeto HelloUniverse, clique com o botão direito e selecione Team / Share Project. No diálogo Share Project, selecione a opção **Use or create repository in the parent folder of project** e selecione o projeto HelloUniverse da lista. Há um campo para indicar onde o repositório será criado (/Users/ceford/git/j4xtutorials-com-hellouniverse) - selecione o botão Criar Repositório adjacente. Finalmente, selecione o botão Concluir.

![Configure Git Repository](../../../en/images/getting-started/eclipse-pdt-configure-git.png)

Você notará que algumas novas decorações apareceram junto aos itens na visualização do Explorador de Projetos:

- O marcador \> indica que há conteúdo que foi alterado, mas não foi enviado para o repositório.
- O marcador ? indica que este item não foi adicionado à lista de itens a serem rastreados.

![Git Markers](../../../en/images/getting-started/eclipse-pdt-git-markers.png)

Você agora está configurado para controle de versão. Clique com o botão direito no projeto e veja como é o menu do Time. Muito para cobrir aqui!

## Finalmente

Pronto para começar a trabalhar no seu próprio código?

*Traduzido por openai.com*
