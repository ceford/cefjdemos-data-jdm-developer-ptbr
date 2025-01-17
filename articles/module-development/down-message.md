<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: Exemplo: Mensagem de Site Fora do Ar -->

## Introdução

Este artigo apresenta um módulo completo de site Joomla para novos desenvolvedores desmontarem e verem sua estrutura e como ele funciona. Ele substitui um módulo anterior que usava convenções de codificação mais antigas. Este é projetado para Joomla 5 e versões posteriores.

Existem artigos introdutórios sobre *Conceitos Gerais* e *Módulos* na Documentação para Programadores Joomla e reproduzidos no Jdocmanual para sua conveniência. Este artigo é semelhante ao JPD [Módulo Básico](jdocmanual?article=docus/modules/basic-module), mas possui algumas funcionalidades extras.

## Propósito

O módulo é projetado para exibir uma mensagem temporária em um dos vários idiomas por um curto período antes do fechamento do site para manutenção do sistema. Isso dá aos usuários a oportunidade de terminar o que estão fazendo antes que o fechamento ocorra.

O nome do módulo é `mod_downmsg` e a mensagem que ele exibe em inglês é semelhante a *Este site será fechado por um curto período às 12:00 GMT*. O horário e a mensagem podem ser selecionados para se adequar ao fechamento iminente do site.

O módulo pode ser baixado do GitHub para instalar ou examinar conforme apropriado.

## Estrutura do Código-Fonte

A seguir está uma ilustração esquemática da estrutura do código-fonte usado para criar a extensão:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- mod_downmsg (folder compressed to create an installable zip file)
    |-- help
        |-- en-GB
            |-- downmsg.html (this is Help screen used in the backend module edit form)
    |-- language
        |-- de-DE
            |-- mod_downmsg.ini (only frontend strings are included)
        |-- en-GB (language folder, kept with the extension code)
            |-- mod_downmsg.ini
            |-- mod_downmsg.sys.ini
        |-- fr-FR
            |-- mod_downmsg.ini (only frontend strings are included)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Dispatcher (folder for extension specific code)
            |-- Dispatcher.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- mod_downmsg.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Esta é a estrutura vista no IDE VSCode ou VSCodium:

![Plugin development file structure in vscodium](../../../en/images/modules/downmsg-module-vscodium.png)

## O Arquivo Manifesto

O arquivo de manifesto controla os processos de instalação, atualização e desinstalação da extensão:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" method="upgrade" client="site">
    <name>mod_downmsg</name>
    <creationDate>December 2024</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <authorUrl>http://jdocmanual.org/</authorUrl>
    <copyright>Copyright (C) 2024 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.1.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>MOD_DOWNMSG_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Module\Downmsg</namespace>
    <files>
        <folder>help</folder>
        <folder>language</folder>
        <folder module="mod_downmsg">services</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <help url="MOD_DOWNMSG_HELP_URL" />
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Mod Downmsg Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="msg_id"
                    type="list"
                    label="MOD_DOWNMSG_PARAMS_MSG_ID_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MSG_ID_DESC"
                    default="v1"
                    required="true"
                >
                    <option value="v1">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
                    <option value="v2">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
                </field>
                <field
                    name="hour"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_HOUR_LABEL"
                    description="MOD_DOWNMSG_PARAMS_HOUR_DESC"
                    default="12"
                    min="0"
                    max="23"
                    required="true"
                />
                <field
                    name="minute"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_MINUTE_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MINUTE_DESC"
                    default="00"
                    min="00"
                    max="59"
                    required="true"
                />
                <field
                    name="tz"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_TZ_LABEL"
                    description="MOD_DOWNMSG_PARAMS_TZ_DESC"
                    default="0"
                    min="-11"
                    max="11"
                    required="true"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

### Anotações de Elemento

- Os atributos **extension**:
   - **type="module"** indica o tipo de extensão.
   - **method="upgrade"** significa que esta extensão pode ser instalada e atualizada se versões mais recentes estiverem disponíveis.
   - **client="site"** informa ao Joomla que este é um módulo *Site* ao invés de um módulo *Administrator*.
- A instrução **version** é usada para controlar a atualização. Se um servidor de atualização estiver disponível, ela é comparada com a versão registrada lá. Isso também impede que uma versão mais antiga sobrescreva uma versão mais recente.
- O atributo de caminho **namespace** informa ao Joomla onde procurar as classes do namespace.
- A seção **files** lista pastas e arquivos para instalação. Se um item não estiver presente aqui, ele não será instalado mesmo que esteja presente no arquivo de instalação compactado.
- O atributo url **help** permite que um arquivo de ajuda personalizado seja usado no formulário de edição do módulo. O caminho para o arquivo de ajuda é um valor para a chave dada no arquivo en-GB/mod_downmsg.ini.
- A seção **config** mostra quais parâmetros este módulo precisa. Todos devem ter padrões porque são configurados na instalação.

## Os Arquivos de Linguagem

Este módulo possui poucas cadeias de caracteres. Essas no arquivo **sys.ini** são usadas na instalação e para listagem entre outros módulos. Os arquivos **.ini** são usados no formulário do Administrador e na renderização do Site do módulo. Observe as convenções na identificação de cadeias de caracteres:

MOD\_\[nome\]\_\[propósito\]\_\[uso\]

Os arquivos abaixo mostram que as traduções em alemão e francês incluem apenas as strings a serem vistas pelos visitantes do site, pois o formulário de Parâmetros será sempre administrado em inglês. Caso você não soubesse: as strings en-GB são sempre carregadas primeiro por padrão para garantir que strings não traduzidas acidentalmente não apareçam como chaves de string. As strings do idioma necessário são então carregadas e substituem as strings equivalentes em inglês.

As versões em francês e alemão das frases aqui foram obtidas usando as ferramentas de tradução do Google. Isso pode funcionar melhor para frases do que para palavras isoladas!

### en-GB/mod_downmsg.ini

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"

MOD_DOWNMSG_MSG_V1="This site will close for a short period at %s"
MOD_DOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

MOD_DOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
MOD_DOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

MOD_DOWNMSG_PARAMS_HOUR_LABEL="Hour"
MOD_DOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

MOD_DOWNMSG_PARAMS_MINUTE_LABEL="Minute"
MOD_DOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

MOD_DOWNMSG_PARAMS_TZ_LABEL="Time Zone"
MOD_DOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### en-GB/mod_downmsg.sys.ini

Este arquivo é usado para gerenciamento do sistema.

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."
```

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
MOD_DOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

### fr-FR/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
MOD_DOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

## O Código do Módulo

O código do módulo é incrivelmente simples. Existem três arquivos PHP:

- services/provider.php (o código de injeção de dependência).
- src/Dispatcher/Dispatcher.php (monta os dados a serem exibidos).
- tmpl/default.php (o modelo para exibir os dados, que pode ser sobrescrito).

### serviços/provedor.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The module Downmsg service provider.
 *
 * @since  4.4.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.4.0
     */
    public function register(Container $container): void
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Cefjdemos\\Module\\Downmsg'));
        $container->registerServiceProvider(new Module());
    }
};
```

### src/Dispatcher/Dispatcher.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Cefjdemos\Module\Downmsg\Site\Dispatcher;

use Joomla\CMS\Dispatcher\AbstractModuleDispatcher;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Dispatcher class for mod_downmsg
 *
 * @since  4.4.0
 */
class Dispatcher extends AbstractModuleDispatcher
{
    /**
     * Returns the layout data.
     *
     * @return  array
     *
     * @since   4.4.0
     */
    protected function getLayoutData()
    {
        $data = parent::getLayoutData();

        return $data;
    }
}
```

### tmpl/default.php

```php
<?php

/**
 * @package     Cefjdemos.Module
 * @subpackage  mod_downmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_DOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0) {
    $tz = '(+' . $tz . ')';
} elseif ($tz < 0) {
    $tz = '(-' . $tz . ')';
} else {
    $tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning text-center" role="alert">
    <?php echo sprintf($msg, $tod); ?>
</div>
```

## Instalação

No menu Administrador, navegue para a página Sistema / Instalar / Extensões e use a aba Enviar Arquivo de Pacote para enviar o arquivo zip. Em seguida, navegue para Conteúdo / Módulos do Site e edite o módulo recém-instalado.

No formulário de edição do Módulo:

1. Insira um título. Lembre-se de que um módulo pode ter mais de uma instância, então eles podem aparecer em locais diferentes ou ter parâmetros diferentes.
2. Defina a hora em que o site ficará fora do ar.
3. Defina o fuso horário (provavelmente há uma maneira melhor do que usar GMT +- n horas).

Na lista de parâmetros comuns do lado direito

1. Defina o **Título** para **ocultar**.
2. Selecione uma **Posição** de template - em Cassiopeia, a **barra superior** coloca a mensagem acima do conteúdo da página, perfeito.
3. Defina o **Status** como **Publicado**.
4. Na aba **Atribuição de Menu**, selecione **Em todas as páginas**.
5. **Salve** e você estará pronto para verificar a aparência do Site.

![the module edit form](../../../en/images/modules/downmsg-module-edit-form.png)

## Testando

É assim que a mensagem aparece em inglês:

![site down message in english](../../../en/images/modules/downmsg-module-result-en.png)

Em alemão:

![site down message in english](../../../en/images/modules/downmsg-module-result-de.png)

E francês:

![site down message in english](../../../en/images/modules/downmsg-module-result-fr.png)

## Atualizar Site e Registro de Alterações

Esses itens estão incluídos na fonte, mas não são abordados aqui.

*Traduzido por openai.com*

