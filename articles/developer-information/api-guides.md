<!-- Filename: API_Guides / Display title: Guias da API -->

Esta página contém um índice do conjunto de Guias da API Joomla. Esses guias têm como objetivo ajudá-lo a entender como usar essas funções do Joomla em suas próprias extensões do Joomla.

Cada guia de API inclui exemplos de código que você pode copiar e instalar em seu próprio ambiente de desenvolvimento. Geralmente, esses exemplos de código são escritos para serem incluídos e instalados como um módulo do Joomla, portanto, se você não estiver familiarizado com o desenvolvimento de módulos do Joomla, pode achar útil passar pela breve série [Criando um Módulo Simples](https://docs.joomla.org/Creating_a_simple_module "Creating a simple module").

Por volta do Joomla 3.8, a equipe de desenvolvimento do Joomla começou a mudar a convenção de nomenclatura das classes do Joomla para usar namespaces, de forma que, por exemplo, JFactory mudou para Factory no namespace Joomla\CMS. Ao ler o código e documentação existentes do Joomla, você pode encontrar as classes seguindo o novo ou o antigo padrão de nomenclatura. Você pode encontrar o mapeamento entre as duas convenções de nomenclatura no arquivo `libraries/classmap.php` na sua instância do Joomla.

- As classes básicas de Aplicação e sua hierarquia e propósitos são descritos em [Understanding the Application classes](https://docs.joomla.org/J3.x:Understanding_the_Application_classes "J3.x:Understanding the Application classes")
- O tratamento Ajax dentro dos Componentes Joomla é descrito em [JSON Responses with JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson"). Ajax também pode ser usado dentro de Módulos e Plugins Joomla como descrito em [Using Joomla Ajax Interface](https://docs.joomla.org/Using_Joomla_Ajax_Interface "Using Joomla Ajax Interface").
- [Cache](https://docs.joomla.org/Cache_Basic_API_Guide "Cache Basic API Guide") - como usar o cache de callback dentro do seu código.
- [Categorias](https://docs.joomla.org/Categories_and_CategoryNodes_API_Guide "Categories and CategoryNodes API Guide") Usando as classes `Categories` e `CategoryNode` para acessar dados relacionados a categorias do Joomla
- CSS pode ser adicionado como descrito em [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Banco de dados / JDatabase. Existem dois guias de API, abordando [Selecionando dados usando JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase "Selecting data using JDatabase")
  e [Inserindo, Atualizando e Removendo dados usando JDatabase](https://docs.joomla.org/Inserting,_Updating_and_Removing_data_using_JDatabase "Inserting, Updating and Removing data using JDatabase")
- [Data/JDate](https://docs.joomla.org/How_to_use_JDate "How to use JDate") é a classe de data do Joomla.
- Arquivos e Pastas. Veja [How to use the filesystem package](https://docs.joomla.org/How_to_use_the_filesystem_package "How to use the filesystem package").
- Form / JForm. Existe um [Guia Básico](https://docs.joomla.org/Basic_form_guide "Basic form guide") para usar a API de Formulários do Joomla e incorporar formulários em um componente Joomla, e também um [Guia Avançado de Formulários](https://docs.joomla.org/Advanced_form_guide "Advanced form guide") abordando aspectos mais avançados das APIs.
- FormField / JFormField. Esta classe e classes relacionadas como JFormFieldList que herdam dela são principalmente úteis para definir campos de formulário personalizados, como descrito em [Creating a custom form field type](https://docs.joomla.org/Creating_a_custom_form_field_type "Creating a custom form field type").
- [Input / JInput](https://docs.joomla.org/Retrieving_request_data_using_JInput "Retrieving request data using JInput") Usando a classe `Input` para obter os valores dos parâmetros em requisições HTTP GET e POST
- JavaScript pode ser adicionado como descrito em [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- O uso de Layouts no Joomla é descrito em [Sharing layouts across views or extensions with JLayout](https://docs.joomla.org/J3.x:Sharing_layouts_across_views_or_extensions_with_JLayout "J3.x:Sharing layouts across views or extensions with JLayout"). A flexibilidade foi aumentada no Joomla 3.2, conforme descrito em [JLayout Improvements for Joomla!](https://docs.joomla.org/J3.x:JLayout_Improvements_for_Joomla! "J3.x:JLayout Improvements for Joomla!").
- [Log / JLog](https://docs.joomla.org/Using_JLog "Using JLog") Registrar mensagens (por exemplo, mensagens de erro, mensagens de depuração) em um arquivo de log e, opcionalmente, no console de depuração
- [Menu e Itens de Menu](https://docs.joomla.org/Menu_and_Menuitems_API_Guide "Menu and Menuitems API Guide")
- [Conjuntos Aninhados](https://docs.joomla.org/Using_nested_sets "Using nested sets"), que permitem que uma hierarquia de árvore seja implementada na tabela do banco de dados, são usados por menus, artigos, categorias Joomla, etc.
- [Registro/JRegistry](https://github.com/joomla-framework/registry) é uma classe utilitária que é muito útil para manipular arrays PHP, converter para JSON, etc.
- [JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson") suporta responder em formato JSON a requisições Ajax.
- Rota / JRoute veja [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- Table / JTable fornece funcionalidade para realizar operações CRUD (e mais) em tabelas de banco de dados. O guia é dividido em um [Guia Básico de API](https://docs.joomla.org/Table_Basic_API_Guide "Table Basic API Guide")
  e um [Guia Avançado de API](https://docs.joomla.org/Table_Advanced_API_Guide "Table Advanced API Guide")
- Os [Controladores](https://docs.joomla.org/Controllers "Controllers") (BaseController, AdminController, FormController, ApiController) são responsáveis por analisar a requisição do usuário, verificar se o usuário tem permissão para realizar essa ação e determinar como satisfazer a requisição.
- Os [Modelos](https://docs.joomla.org/Models "Models") (BaseModel, BaseDatabaseModel, ItemModel, ListModel, FormModel, AdminModel) encapsulam os dados usados pelo componente. Eles também são responsáveis por atualizar o banco de dados quando apropriado.
- As [Visualizações](https://docs.joomla.org/Views "Views") (AbstractView, CategoriesView, CategoryFeedView, CategoryView, FormView, HtmlView, JsonApiView, JsonView, ListView) especificam o que deve aparecer na página da web e reúnem todos os dados necessários para gerar a resposta HTTP.
- [Tags](https://docs.joomla.org/Tags_API_Guide "Tags API Guide").
- Uri / JUri veja [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- [Usuário / JUser](https://docs.joomla.org/Accessing_the_current_user_object "Accessing the current user object") API relacionada ao Usuário do Joomla.

*Traduzido por openai.com*

