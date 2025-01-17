<!-- Filename: J4.x:Developer:_File_Structure / Display title: Estrutura de Arquivo Exemplo -->

## Introdução

Como uma pessoa que está começando a trabalhar no desenvolvimento de extensões Joomla em um computador pessoal, laptop ou desktop, você precisa configurar uma estrutura de arquivos adequada para o código da sua extensão. A localização do seu site Joomla é pré-definida pelo seu sistema operacional e pela instalação do Apache. A localização do código que você cria é de sua escolha.

## Localização do Site de Teste

Neste exemplo, os sites de teste do Joomla estão localizados em subpastas da raiz do documento. Isso permite a criação de muitos sites para todos os tipos de projetos diferentes, sem a necessidade de criar hosts virtuais. Alguns sites de teste podem não estar relacionados ao Joomla. A raiz do site para diferentes plataformas:

- Mac: /Usuários/nomedeusuario/Sites
- Linux: /home/nomedeusuario/public_html
- Windows: ...

Esta é uma captura de tela de parte de uma lista da pasta Sites mostrando uma seleção de muitos sites de teste:

![multiple sites on mac](../../../en/images/getting-started/developer-file-structure-mac-sites.png)

Cada um é acessado pelo nome de sua subpasta. Exemplos:

- `http://localhost/j4test`
- `http://localhost/j520`

Existem circunstâncias nas quais você pode preferir criar sites virtuais separados. Isso não é abordado aqui.

## Estrutura de Arquivos do Site de Teste

Se você ainda não fez isso, precisará se familiarizar com a estrutura de um site Joomla. A ilustração a seguir mostra um exemplo típico de árvore de arquivos e pastas do Joomla, com a pasta Administrator expandida para mostrar seu conteúdo.

![joomla file structure with administrator expanded](../../../en/images/getting-started/developer-file-structure-mac-joomla.png)

Aqui é onde o código funcional será instalado. O código-fonte está em outro lugar.

## Localização do Código da Extensão do Desenvolvedor

A localização do código da sua extensão é uma escolha pessoal. Eu gosto de manter o código da minha extensão em uma árvore de arquivos adequada para a criação de um arquivo zip instalável. A base da minha árvore é /Users/username/git porque eu sei digitar git e uso git para controle de versão. Você não precisa fazer isso - git será abordado em um tutorial separado. Minha pasta pai do git contém muitas subpastas que podem usar pastas git separadas para controle de versão. Esta é uma captura de tela mostrando uma lista parcial de projetos:

![joomla file structure project folders](../../../en/images/getting-started/developer-file-structure-mac-project-folders.png)

Observe que alguns dos nomes de pastas começam com `j4xdemos`, que adotei para usar como a primeira parte do namespace utilizado para meus projetos criados para fins de tutoriais do Joomla 4. Não é necessário como parte do nome da pasta, mas é algo a se considerar: a primeira parte do seu namespace precisa ser algo único para você ou sua organização. Desde então, adotei `cefjdemos` como meu prefixo de namespace, pois é mais pessoal e não específico a uma versão do Joomla.

### Estrutura de Pastas do Projeto

Tomando o j4xdemos-com-mywalks como exemplo, todo o código que irá para a extensão está dentro da pasta com_mywalks. Quando isso é compactado, o arquivo com_mywalks.zip criado é utilizado para instalação no Joomla. Nenhum dos outros arquivos na raiz é incluído no arquivo zip, mas podem ser incluídos no repositório GitHub. Por exemplo, README.md é um arquivo de texto em formato Markdown contendo uma breve descrição da extensão. O nome da pasta j4xdemos-com-mywalks não é significativo. Ele não é usado em nenhum outro lugar além de ordenar as pastas em ordem alfabética.

Na ilustração a seguir, a pasta j4xdemos-com-mywalks foi aberta no VSCodium para mostrar a estrutura do código do projeto. O arquivo mywalks.xml é um arquivo de manifesto que informa ao Joomla o que instalar e onde. As pastas admin e site contêm código que irá para administrator/components/com_mywalks e components/com_mywalks.

![Project folder open in vscodium](../../../en/images/getting-started/developer-file-structure-mac-vscodium.png)

Deve ser óbvio que mesmo um pequeno componente precisa de uma quantidade considerável de pastas e arquivos. Existem ferramentas de modelo pré-definido para extensões disponíveis para criar rapidamente um componente esqueleto. Elas são abordadas em outro lugar. ToDo

Pronto para criar algum código?

*Traduzido por openai.com*

