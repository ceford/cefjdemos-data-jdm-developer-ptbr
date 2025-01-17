<!-- Filename: J4.x:MVC_Anatomy:_Getting_Started / Display title: Anatomia do MVC: Introdução -->

## Introdução

Os componentes Joomla seguem a abordagem Modelo, Visualização, Controlador, ou MVC para encurtar. O Modelo deve lidar com o carregamento e armazenamento de dados. A Visualização deve lidar com a exibição de dados. E o Controlador deve lidar com o fluxo do programa, a interação entre o código do Modelo e da Visualização do componente.

Se você quiser criar seu próprio componente, há duas abordagens para aprendizado que você pode adotar:

- **Construa passo a passo:** seguindo um tutorial simples que descreve cada etapa.
- **Desmonte arquivo por arquivo:** desmontando um componente funcional para aprender como cada parte funciona.

Este tutorial utiliza a última abordagem. O componente usado para o tutorial é chamado de com_countrybase. Ele é usado para gerenciar e exibir alguns dados básicos sobre os países do mundo. Na verdade, é útil por si só e pode ser usado como base para construir algo mais sofisticado.

## Objetivos do Esboço

Seja qual for o seu projeto, você deve começar com um esboço de seus objetivos e talvez se perguntar se esses objetivos podem ser alcançados com os componentes principais do Joomla ou uma extensão disponível. Para o com_countrybase é necessária uma tabela de dados de países, junto com páginas de entrada e saída. E talvez um módulo para exibir uma taxa de câmbio monetária diária. E até mesmo um plugin chamado por um comando cli para atualizar dados de moeda em intervalos regulares. Este tutorial cobre apenas o componente.

O componente precisa de uma tabela de banco de dados para armazenar dados de países e uma tabela para armazenar dados de moedas. Cada um precisará de uma visão de lista e uma visão de edição. Isso não é algo que possa ser realizado com componentes core do Joomla, portanto, é claramente um caso para um componente personalizado.

## Tabelas Iniciais

Há muitos dados sobre países disponíveis de todos os tipos de fontes e em todos os tipos de formatos. Como existem pelo menos 250 países no mundo, inserir dados para cada país um por um usando um formulário de entrada de dados seria bastante tedioso. Então, preparei tabelas iniciais coletando dados de várias fontes, combinando-os em uma planilha e, em seguida, exportando instruções SQL para criar as tabelas.

Os dados são baseados nos códigos de países ISO 3166, mas alguns países muito conhecidos não estão incluídos, como Inglaterra, Escócia e País de Gales. Você não precisa se preocupar com isso. As tabelas são fornecidas com o componente com_countrybase.

Os arquivos para o componente com_countrybase estão disponíveis no GitHub. Você pode fazer o download de um arquivo [ZIP](https://github.com/ceford/j4xdemos-com-countrybase/archive/refs/heads/master.zip) e instalá-lo para vê-lo funcionando no menu do Administrador. Crie um item de menu se desejar vê-lo funcionando no modelo do seu Site. Além disso, descompacte o arquivo zip no espaço de arquivos do seu projeto, não na árvore do seu site de teste, para examinar a estrutura de arquivos do componente e o conteúdo dos arquivos com seu IDE ou ferramenta de edição de texto favorita.

## Captura de Tela

Esta lista de Administradores de Países tem cinco itens para minimizar o tamanho da imagem. O Joomla normalmente exibe 20 itens.

![List of countries](../../../en/images/mvc-anatomy/com-countrybase-countries.png)

## Componente em Template

Para ajudá-lo a começar com seu próprio componente, há um [componente modelo](https://github.com/ceford/j4xdemos-com-bpsrc/archive/refs/heads/master.zip) disponível no Github. Baixe e descompacte isso no espaço de arquivos do seu projeto, e não na árvore do seu site de teste. Após o download, faça todas as alterações indicadas no README e você estará pronto para começar.

Existem também vários geradores de extensão gratuitos e comerciais que você pode querer experimentar para gerar um componente esqueleto para seus próprios fins. [Joomla! Component Builder](https://www.joomlacomponentbuilder.com/) é gratuito e parece ser abrangente.

*Traduzido por openai.com*

