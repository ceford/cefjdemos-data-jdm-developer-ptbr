<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Edit_Files / Display title: Anatomia do MVC: Editar Arquivos do Administrador -->

## Arquivos de Dados do País

Os arquivos do administrador incluem quatro visualizações: duas visualizações de lista para os dados de países e dados de moedas e duas visualizações de edição para gerenciar a inserção de dados em cada uma das duas tabelas envolvidas. As visualizações de lista são quase idênticas à visualização de lista de países descrita para a interface do site, portanto, não serão abordadas novamente aqui. Da mesma forma, as visualizações de edição são quase idênticas, então apenas uma será abordada, a visão de inserção de dados para um único país.

## src/Controller/CountryController.php

Isso é imensamente simples! Além da declaração da classe, está desprovido de conteúdo. Tudo é cuidado pela classe pai.

```php
    class CountryController extends FormController
    {
    }
```

## src/View/Country/HtmlView.php

Este é o local onde os dados são buscados do modelo para uso no formulário de entrada de dados. Há uma diferença significativa em relação à exibição de lista descrita anteriormente: é uma prática comum usar uma barra de ferramentas com botões para Salvar, Cancelar, Ajuda, etcétera.

Quando você invoca o formulário de edição selecionando o título de um item da lista, o URL contém um ID, por exemplo, option=com_countrybase&view=country&layout=edit&id=1. O ID é o número do registro da tabela. Para um novo registro invocado através do botão Novo da lista, o ID está ausente. Se estiver presente, o modelo preenche o formulário de entrada de dados com os dados do registro existente.

```php
    public function display($tpl = null)
    {
        $this->form  = $this->get('Form');
        $this->item  = $this->get('Item');
        $this->state = $this->get('State');

        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        $this->addToolbar();

        return parent::display($tpl);
    }
```

### A Barra de Ferramentas

A função addToolbar() é normalmente utilizada para definir o título da página e adicionar botões apropriados para as permissões de acesso da pessoa que está usando o formulário. Observe o uso da variável \$isNew com base no id do registro para modificar levemente a saída.

```php
    protected function addToolbar()
    {
        Factory::getApplication()->input->set('hidemainmenu', true);
        $isNew      = ($this->item->id == 0);

        $canDo = ContentHelper::getActions('com_countrybase');

        $toolbar = Toolbar::getInstance();

        ToolbarHelper::title(
            Text::_('COM_COUNTRYBASE_COUNTRY_PAGE_TITLE_' . ($isNew ? 'ADD' : 'EDIT'))
        );

        if ($canDo->get('core.create')) {
            if ($isNew) {
                $toolbar->apply('country.save');
            } else {
                $toolbar->apply('country.apply');
            }
            $toolbar->save('country.save');
        }
        $toolbar->cancel('country.cancel', 'JTOOLBAR_CLOSE');

        ToolbarHelper::inlinehelp();
    }
```

**Notas:**

- **hidemainmenu** é definido como verdadeiro para todos os formulários de edição para evitar que você saia do formulário através do menu do Administrador sem salvar.
- **ToolbarHelper::title()** define o título na barra de título da página, neste caso para Editar País ou Adicionar País.
- Botões **toolbar-\>action**, onde a ação pode ser salvar, aplicar, cancelar ou uma de muitas outras, levam (controller.function) como argumentos. Então, neste caso, quando um botão é selecionado, a ação é passada para uma função save ou apply na classe CountryController.
- **cancel** leva um argumento adicional, a chave de string usada para rotular o botão.
- **inlinehelp()** mostra um botão que alterna as descrições dos campos abaixo de cada campo de entrada de dados.

## forms/country.xml

Este arquivo contém a especificação dos campos do formulário, geralmente na ordem apresentada no formulário de edição. A maioria dos campos é autoexplicativa, portanto, apenas dois são mostrados abaixo. Os valores de nome são os nomes das colunas usados nas tabelas do banco de dados.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<form>
        <field
            name="title"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_TITLE"
            required="true"
            maxlength="128"
        />

        <field
            name="iso_2"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_ISO_2"
            description="COM_COUNTRYBASE_COUNTRY_ISO_2_DESC"
            required="true"
            maxlength="3"
        />
```

## src/Modelo/ModeloPais.php

As funções neste arquivo são bastante padrão. No entanto, a função getTable() requer alguma explicação.

```php
    public function getTable($name = '', $prefix = '', $options = array())
    {
        $name = 'Countries';
        $prefix = 'Table';

        if ($table = $this->_createTable($name, $prefix, $options)) {
            return $table;
        }

        throw new \Exception(Text::sprintf('JLIB_APPLICATION_ERROR_TABLE_NAME_NOT_SUPPORTED', $name), 0);
    }
```

Esta função não se refere à tabela em si! Ela se refere ao construtor de src/Table/CountriesTable.php. Isso pode ser um pouco confuso porque está sendo chamada a partir da classe CountryModel.

## src/Table/CountriesTable.php

Então é aqui que o nome da tabela para a lista de países é definido.

```php
    class CountriesTable extends Table
    {
        /**
         * Constructor
         *
         * @param   DatabaseDriver  $db  Database connector object
         *
         * @since   1.6
         */
        public function __construct(DatabaseDriver $db)
        {
            parent::__construct('#__countrybase_countries', 'id', $db);

            $this->setColumnAlias('published', 'state');
        }
    }
```

Observe a declaração do alias da coluna. Isso é usado porque o formulário utiliza um campo de entrada de dados chamado **published**, mas o campo usado para armazenamento é chamado **status**.

## tmpl/country/edit.php

Este é o arquivo que cria a saída HTML. Ele começa com dois auxiliares: HTMLHelper::_('behavior.formvalidator'); carrega o JavaScript necessário para a validação de formulário no lado do cliente. Embora os navegadores modernos apliquem validação de formulário, isso não impede o envio do formulário. HTMLHelper::_('behavior.keepalive'); carrega um JavaScript para pingar o servidor e evitar a expiração da sessão enquanto preenche formulários complexos.

**Notas:**

- LayoutHelper::render('joomla.edit.title_alias', \$this); renderiza um campo de título padrão do Joomla.
- \$this-\>form-\>renderField('iso_2'); renderiza o campo especificado, neste caso iso_2.
- LayoutHelper::render('joomla.edit.global', \$this); renderiza os campos do lado direito da aba Detalhes.

```php
<?php

/**
 * @package     Countrybase.Administrator
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;

HTMLHelper::_('behavior.formvalidator');
HTMLHelper::_('behavior.keepalive');

?>

<form action="<?php echo Route::_('index.php?option=com_countrybase&view=country&layout=edit&id=' .
        (int) $this->item->id); ?>"
    method="post" name="adminForm" id="country-form" class="form-validate">

    <?php echo LayoutHelper::render('joomla.edit.title_alias', $this); ?>

    <div>
        <?php echo HTMLHelper::_('uitab.startTabSet', 'myTab', array('active' => 'details')); ?>

        <?php echo HTMLHelper::_('uitab.addTab', 'myTab', 'details', Text::_('COM_COUNTRYBASE_COUNTRY_TAB_DETAILS')); ?>
        <div class="row">
            <div class="col-md-9">
                <div class="row">
                    <div class="col-md-6">
                        <?php echo $this->form->renderField('iso_2'); ?>
                        <?php echo $this->form->renderField('iso_3'); ?>
                        <?php echo $this->form->renderField('country_code'); ?>
                        <?php echo $this->form->renderField('region_code'); ?>
                        <?php echo $this->form->renderField('subregion_code'); ?>
                        <?php echo $this->form->renderField('phone_prefix'); ?>
                        <?php echo $this->form->renderField('currency_code'); ?>
                        <?php echo $this->form->renderField('id'); ?>
                    </div>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card card-light">
                    <div class="card-body">
                        <?php echo LayoutHelper::render('joomla.edit.global', $this); ?>
                    </div>
                </div>
            </div>
        </div>
        <?php echo HTMLHelper::_('uitab.endTab'); ?>

        <?php echo HTMLHelper::_('uitab.endTabSet'); ?>
    </div>
    <input type="hidden" name="task" value="">
    <?php echo HTMLHelper::_('form.token'); ?>
</form>
```

Você pode alternar entre conjuntos de campos de formulário e campos dentro de cada conjunto. Isso pode simplificar a saída de formulários complexos com várias abas.

![country edit form](../../../en/images/mvc-anatomy/com-countrybase-edit-country.png)

*Traduzido por openai.com*

