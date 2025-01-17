<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: Exemplo: Índice -->

## Introdução

Este artigo fornece um exemplo de um plugin de conteúdo para ilustrar a codificação usando as convenções de codificação mais recentes do Joomla. O código completo está disponível no [GitHub](https://github.com/ceford/cefjdemos-plg-content-toc). Você pode baixá-lo e instalá-lo ou apenas descompactá-lo para inspecionar o código.

Há uma seção sobre [Desenvolvimento de Plugins](https://manual.joomla.org/docs/building-extensions/plugins/) e mais exemplos na Documentação para Programadores do Joomla, [copiada para este site](jdocmanual?article=docus/plugins/index) para conveniência.

Este plugin gera um *Sumário* a partir dos títulos de qualquer artigo que contenha um marcador `{cefjdemostoc}`. O resultado é ilustrado na última captura de tela deste artigo.

## Estrutura do Código Fonte

A seguir está uma ilustração esquemática da estrutura do código-fonte usado para criar a extensão:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- plg_content_cefjdemostoc (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_content_cefjdemostoc.ini
        |-- plg_content_cefjdemostoc.sys.ini
    |-- media
        |-- css
            |-- cefjdemostoc.css (layout styles)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Cefjdemostoc.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- cefjdemostoc.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

Esta é a estrutura vista no IDE VSCode ou VSCodium:

![Plugin development file structure in vscodium](../../../en/images/plugins/cefjdemostoc-vscodium.png)

## O Arquivo Manifesto

Cada extensão deve ter um arquivo de manifesto em formato xml. Ele é usado pelo Joomla para fins de instalação, atualização e remoção. Este é o arquivo de manifesto para esta extensão:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="plugin" group="content" method="upgrade">
    <name>plg_content_cefjdemostoc</name>
    <author>Clifford E Ford</author>
    <creationDate>2024-12-23</creationDate>
    <copyright>(C) 2024 Clifford E Ford</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>cliff@xxxx.yyyy.co.uk</authorEmail>
    <authorUrl>jdocmanual.org</authorUrl>
    <version>0.2.2</version>
    <description>PLG_CONTENT_CEFJDEMOSTOC_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Content\Cefjdemostoc</namespace>
    <files>
        <folder plugin="cefjdemostoc">services</folder>
        <folder>src</folder>
        <folder>language</folder>
    </files>
    <media destination="plg_content_cefjdemostoc" folder="media">
        <folder>css</folder>
    </media>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemostoc Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="help"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DESC"
                    default="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                >
                    <option value="">PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT</option>
                </field>

                <field
                    name="toc_class"
                    type="text"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                />

                <field
                    name="toc_head_class"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                    default="center"
                >
                    <option value="text-center">text-center</option>
                    <option value="text-start">text-start</option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## O arquivo services/provider.php

O objetivo deste arquivo é registrar um provedor de serviços e declarar quaisquer ferramentas que ele precise. Este plugin específico não precisa de muito. Se o seu plugin personalizado precisasse fazer consultas ao banco de dados, você declararia aqui. Dê uma olhada em um plugin de autenticação. Eles requerem ferramentas de banco de dados e usuário.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjdemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford <https://jdocmanual.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.3.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin     = new Cefjdemostoc(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'cefjdemostoc')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## A Classe de Extensão

O objetivo do arquivo src/Extension/Cefjdemostoc.php é preparar os dados para a saída. O arquivo tem 91 linhas, portanto, não está reproduzido aqui. No entanto, algumas partes dele merecem explicações adicionais.

### Diretivas do Codesniffer

Linhas 16 a 18 têm isso:

```php
// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

As linhas comentadas evitam relatórios de erro do CodeSniffer. Elas não têm outros propósitos.

### função pública onContentPrepare()

Este é o evento ao qual o plugin responde. É passado um *contexto*, os dados do artigo e os parâmetros do artigo. Um *Sumário* deve ser gerado apenas se a página renderizada for um único artigo. Caso contrário, o espaço reservado `{cefjdemostoc}` precisa ser removido.

Para gerar um índice, alguns parâmetros do artigo são definidos e cada título no artigo é alterado para adicionar um número de sequência **id**, assim `<h2>Introdução</h2>` se torna `<h2 id="cefjemostoc0">Introdução</h2>`. Em seguida, um modelo é carregado para adicionar o índice renderizado.

### O arquivo tmpl/default.php

O objetivo deste arquivo é renderizar a saída com os dados fornecidos. O arquivo pode ser substituído e você o verá entre as substituições de **plugins/contents** para o modelo Cassiopeia.

O arquivo é reproduzido abaixo. Observe que alguns elementos têm classes CSS codificadas e alguns obtidos dos parâmetros do artigo. As classes **fs-** são tamanhos de fonte do Bootstrap. Os cabeçalhos são armazenados no array `$headings2`.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

/**
 * @var Joomla\CMS\WebAsset\WebAssetManager $wa
 * @var \Joomla\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc $this
 */
$wa = $this->getApplication()->getDocument()->getWebAssetManager();
$wa->registerAndUseStyle('plg_content_cefjdemostoc', 'plg_content_cefjdemostoc/cefjdemostoc.css');

?>
<div class="card cefjdemostoc <?php echo $toc_class; ?>">
    <div class="card-header">
        <div class="<?php echo $toc_head_class; ?> fs-2">
            <?php echo Text::_('PLG_CONTENT_CEFJDEMOSTOC_CONTENTS'); ?>
        </div>
    </div>
    <div class="card-body">
        <ul class="list-group">
            <?php foreach ($headings2 as $i => $heading) : ?>
                <li class="list-group-item fs-<?php echo $heading[1] + 2; ?> ps-<?php echo $heading[1] + 2; ?>">
                    <a href="#cefjemostoc<?php echo $i; ?>" class="link-underline-light">
                        <?php echo $heading[2]; ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </div>
</div>
```

#### O Gerenciador de Ativos da Web

Sem estilização adicional, o índice (ToC) ocuparia toda a largura da tela na localização do espaço reservado `{cefjemostoc}` no texto fonte. Isso às vezes é visto em publicações técnicas, mas pode não ser o que você deseja. Em telas largas, muitas vezes é melhor flutuar o ToC e permitir que o texto do artigo flua ao redor dele. No entanto, isso pode se tornar inutilizável em telas estreitas. A melhor solução é fornecer uma folha de estilo com consultas de mídia personalizadas. Em telas largas, o ToC flutua para a direita, mas em telas estreitas não.

O código `default.php` acima mostra como incluir uma folha de estilo personalizada para o plugin. O CSS está no arquivo `media/css/cefjdemostoc.css`. Os valores ali são para ilustração deste tutorial.

## Resultado

![The resulting table of contents](../../../en/images/plugins/cefjdemostoc-result.png)

*Traduzido por openai.com*

