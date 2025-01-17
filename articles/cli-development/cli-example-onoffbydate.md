<!-- Filename: J4.x:CLI_example_-_Onoffbydate / Display title: Exemplo de CLI - Onoffbydate -->

## Introdução

Há ocasiões em que um site precisa exibir ou ocultar um módulo dependendo da data. Um exemplo pode ser um módulo HTML personalizado exibindo uma mensagem durante o inverno. Outro exemplo pode ser a alternância de módulos personalizados dependendo do dia da semana, por exemplo, um para os dias úteis e outro para os fins de semana. O exemplo apresentado aqui utiliza um plugin, um comando CLI e um cron para alcançar o efeito desejado.

O código está disponível em [GitHub](https://github.com/ceford/cefjdemos-plg-onoffbydate)

Há mais artigos sobre o uso da CLI no [Manual do Usuário](http://localhost/jdm4/jdocmanual?article=user/command-line-interface/using-the-cli) e na Documentação do Joomla! para Programadores [cópia local](jdocmanual?article=docus/plugins/plugin-examples-console-plugin-sqlfile) ou [fonte original](https://manual.joomla.org/docs/building-extensions/plugins/plugin-examples/console-plugin-sqlfile/);

## Padrões do Joomla

Nas versões anteriores do Joomla, o sistema de plugins utilizava uma implementação do padrão Observável/Observador. Como resultado, cada plugin carregado teria imediatamente todos os seus métodos públicos registrados como observadores. Isso poderia causar problemas.

Joomla 4 e posteriores utilizam o pacote Joomla Framework Event para gerenciar eventos de plugins. Isso oferece melhor desempenho e segurança. Em termos práticos, isso significa que a estrutura de arquivos de um plugin Joomla 4/5/6 é diferente da estrutura de plugins legados das versões anteriores.

## A Estrutura do Plugin

O plugin é chamado **onoffbydate**. O diagrama esquemático a seguir mostra a estrutura de arquivos usada para desenvolvimento local do plugin:

```
cefjdemos-plg-onoffbydate
|-- .vscode (folder for build settings)
|-- cefjdemos-plg-onoffbydate (folder compressed to create an installable zip file)
    |-- language
        |-- en-GB (language folder, kept with the extension code)
            |-- plg_system_onoffbydate.ini
            |-- plg_system_onoffbydate.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Console (folder for extension specific code)
            |-- OnoffbydateCommand.php (the plugin execution code)
        |-- Extension
            |-- Onoffbydate.php (the plugin register code)
    |-- onoffbydate.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (a record of changes in each release)
|-- README.md (brief description and instructions on use)
|-- updates.xml (the update server specification)
```

## O Arquivo de Manifesto onoffbydate.xml

Este é o arquivo de instalação - um item padrão para qualquer extensão.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="console" method="upgrade">
    <name>plg_console_onoffbydate</name>
    <author>Clifford E Ford</author>
    <creationDate>Jamuary 2025</creationDate>
    <copyright>(C) Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later</license>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <version>0.3.0</version>
    <description>PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Console\Onoffbydate</namespace>
    <files>
        <folder>language</folder>
        <folder plugin="onoffbydate">services</folder>
        <folder>src</folder>
    </files>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemosonoffbydate Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/updates.xml</server>
    </updateservers>
    <config>
        <fields>

        </fields>
    </config>
</extension>
```

- A linha **namespace** informa ao Joomla onde encontrar o código com namespace para este plugin.
- A pasta **language** é mantida junto com o código do plugin.
- Um **updateserver** é necessário para atender aos requisitos do JED.
- Não há opções de **config** para este plugin, mas isso pode ser alterado no futuro. Por exemplo, o usuário pode preferir definir os meses de inverno usando caixas de seleção em vez de tê-los codificados.

## Registro: serviços/provider.php

Este é o ponto de entrada para o código do plugin.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\CMS\Factory;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Extension\Onoffbydate;

return new class implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.2.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $dispatcher = $container->get(DispatcherInterface::class);
                $plugin     = new Onoffbydate(
                    $dispatcher,
                    (array) PluginHelper::getPlugin('console', 'onoffbydate')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Assinatura de Evento: src/Extension/Onoffbydate.php

Este é o local onde o evento que aciona o plugin é registrado e o local onde o código que irá lidar com o evento é definido.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Extension;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\SubscriberInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Console\OnoffbydateCommand;

final class Onoffbydate extends CMSPlugin implements SubscriberInterface
{
    /**
     * Returns the event this subscriber will listen to.
     *
     * @return  array
     */
    public static function getSubscribedEvents(): array
    {
        return [
                \Joomla\Application\ApplicationEvents::BEFORE_EXECUTE => 'registerCommands',
        ];
    }

    /**
     * Returns the command class for the Onoffbydate CLI plugin.
     *
     * @return  void
     */
    public function registerCommands(): void
    {
        $myCommand = new OnoffbydateCommand();
        $myCommand->setParams($this->params);
        $this->getApplication()->addCommand($myCommand);
    }
}
```

A classe **OnoffbydateCommand** está localizada na pasta *src/Console*, assim como os comandos embutidos do Joomla Console estão localizados em uma pasta Console. Poderia ter sido colocada em uma pasta *Extension*.

## O Arquivo de Comando: src/Console/OnoffbydateCommand.php

Este arquivo contém o código que realiza todo o trabalho para esta extensão.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Console;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Factory;
use Joomla\CMS\Language\Text;
use Joomla\Console\Command\AbstractCommand;
use Joomla\Database\DatabaseInterface;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Style\SymfonyStyle;
use Joomla\Database\DatabaseAwareTrait;

class OnoffbydateCommand extends AbstractCommand
{
    use DatabaseAwareTrait;

    /**
     * The default command name
     *
     * @var    string
     *
     * @since  4.0.0
     */
    protected static $defaultName = 'onoffbydate:action';

    /**
     * @var InputInterface
     * @since version
     */
    private $cliInput;

    /**
     * SymfonyStyle Object
     * @var SymfonyStyle
     * @since 4.0.0
     */
    private $ioStyle;

    /**
     * The params associated with the plugin, plus getter and setter
     * These are injected into this class by the plugin instance
     */
    protected $params;

    protected function getParams()
    {
        return $this->params;
    }

    public function setParams($params)
    {
        $this->params = $params;
    }

    /**
     * Initialise the command.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    protected function configure(): void
    {
        $lang = Factory::getApplication()->getLanguage();
        $test = $lang->load(
            'plg_console_onoffbydate',
            JPATH_BASE . '/plugins/console/onoffbydate'
        );

        $this->addArgument(
            'season',
            InputArgument::REQUIRED,
            'winter or weekend'
        );
        $this->addArgument(
            'action',
            InputArgument::REQUIRED,
            'publish or unpublish'
        );

        $this->addArgument(
            'module_id',
            InputArgument::REQUIRED,
            'module id'
        );

        $help = Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_1');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_2');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_3');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_4');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_5');

        $this->setDescription(Text::_('PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION'));
        $this->setHelp($help);
    }

    /**
     * Internal function to execute the command.
     *
     * @param   InputInterface   $input   The input to inject into the command.
     * @param   OutputInterface  $output  The output to inject into the command.
     *
     * @return  integer  The command exit code
     *
     * @since   4.0.0
     */
    protected function doExecute(InputInterface $input, OutputInterface $output): int
    {
        $this->configureIO($input, $output);

        $season = $this->cliInput->getArgument('season');
        $action = $this->cliInput->getArgument('action');
        $module_id = $this->cliInput->getArgument('module_id');

        switch ($season) {
            case 'winter':
                $result = $this->winter($module_id, $action);
                break;
            case 'weekend':
                $result = $this->weekend($module_id, $action);
                break;
            default:
                $result = Text::_('PLG_CONSOLE_ONOFFBYDATE_ERROR', $season);
                $this->ioStyle->error($result);
                return 0;
        }

        return 1;
    }

    protected function publish($module_id, $published)
    {
        $db = Factory::getContainer()->get(DatabaseInterface::class);
        //$db    = $this->getDatabase();
        $query = $db->getQuery(true);
        $query->update('#__modules')
            ->set('published = ' . $published)
            ->where('id = ' . $module_id);
        $db->setQuery($query);
        $db->execute();
    }

    protected function weekend($module_id, $action)
    {
        // get the day of the week
        $day = date('w');
        if (in_array($day, array(0,6))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    protected function winter($module_id, $action)
    {
        // get the month of the month
        $month = date('n');
        if (in_array($month, array(1,2,11,12))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    /**
     * Configures the IO
     *
     * @param   InputInterface   $input   Console Input
     * @param   OutputInterface  $output  Console Output
     *
     * @return void
     *
     * @since 4.0.0
     *
     */
    private function configureIO(InputInterface $input, OutputInterface $output)
    {
        $this->cliInput = $input;
        $this->ioStyle = new SymfonyStyle($input, $output);
    }
}
```

- A função `configure` estabelece que são necessários três argumentos de linha de comando:
    - `season` deve ser um dos `weekend` ou `winter`.
    - `action` deve ser um dos `publish` ou `unpublish`.
    - `module_id` deve ser o ID inteiro do módulo a ser publicado ou retirado de publicação.
- A função `doExecute` é onde o trabalho é feito. Duas opções de ação são reconhecidas:
    - `winter` chama uma função para publicar ou retirar de publicação um módulo se hoje estiver nos meses de inverno.
    - `weekend` chama uma função para publicar ou retirar de publicação um módulo se hoje for um dia de fim de semana.
- A função `publish` faz uma chamada de banco de dados para definir o campo `publishes` do módulo como `0` ou `1` conforme apropriado.
- As funções `winter` e `weekend` fazem alguns cálculos de data para determinar se hoje está no inverno (ou não) ou em um fim de semana (ou não) para passar parâmetros apropriados para a função publicar.
- A chamada da função `$this->ioStyle->success()` produz uma mensagem de texto no console com texto preto e fundo verde. `$this->ioStyle->error()` produz uma mensagem de ERRO com texto branco em um fundo marrom.

## arquivos de idioma

Existem dois arquivos de idioma usados durante a instalação e configuração do plugin. O arquivo en-GB/plg_console_onoffbydate.sys.ini contém as strings vistas durante a instalação e gerenciamento:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
```

O arquivo en-GB/plg_console_onoffbydate.ini contém as strings vistas em uso:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
PLG_CONSOLE_ONOFFBYDATE_ERROR="Unknown season: %s."
PLG_CONSOLE_ONOFFBYDATE_HELP_1="<info>%command.name%</info> Toggles module Enabled/Disabled state\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_2="Usage: <info>php %command.full_name% season action module_id where\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_3="    season is one of winter or weekend,\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_4="    action is one of publish or unpublish and\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_5="    module_id is the id of the module to publish or unpublish.</info>\n"
PLG_CONSOLE_ONOFFBYDATE_SUCCESS="That seemed to work. %s Module %s has been %s."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND="Today is in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER="Today is in winter."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND="Today is not in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER="Today is not in winter."
```

Observe que a saída do comando é vista apenas por você na sua instalação do Joomla.

## O Módulo

Este código simplesmente altera o estado publicado de um módulo dependendo de alguma função da data. Portanto, você precisa do ID do módulo conforme ilustrado na coluna da direita da página de listagem de Módulos (Site).

## A Linha de Comando

Instale o plugin na sua instalação de teste do Joomla e ative-o. Se ele causar falhas no seu site, use o phpMyAdmin para encontrar o plugin recém-instalado na tabela #__extensions e definir seu parâmetro enabled para 0. Corrija o problema e tente novamente.

Em uma janela de terminal, mude o diretório para a raiz do seu site e use o seguinte comando:

```sh
php cli/joomla.php list
```

Se algo der errado nesta etapa, verifique se a versão do PHP invocada é a versão de linha de comando e não a usada pelo servidor web. Você pode suprimir mensagens de descontinuação com esta versão da linha de comando.

```sh
php -d error_reporting="E_ALL & ~E_DEPRECATED" cli/joomla.php onoffbydate:action garbage publish 133
```

Se o código funcionar, você verá `onoffbydate` entre a lista de comandos disponíveis e poderá invocar a ajuda para ver como deve ser usado:

```sh
php cli/joomla.php onoffbydate:action --help
Usage:
  onoffbydate:action <season> <action> <module_id>

Arguments:
  season                       winter or summer or weekday or weekend
  action                       publish or unpublish
  module_id                    module id

Options:
  -h, --help                   Display the help information
  -q, --quiet                  Flag indicating that all output should be silenced
  -V, --version                Displays the application version
      --ansi                   Force ANSI output
      --no-ansi                Disable ANSI output
  -n, --no-interaction         Flag to disable interacting with the user
      --live-site[=LIVE-SITE]  The URL to your site, e.g. https://www.example.com
  -v|vv|vvv, --verbose         Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  onoffbydate:action Toggles module Enabled/Disabled state
  Usage: php joomla.php onoffbydate:action season action module_id where
      season is one of winter or summer or weekday or weekend,
      action is one of publish or unpublish and
      module_id is the id of the module to publish or unpublish.
```

E então, apenas experimente:

```sh
php cli/joomla.php onoffbydate:action weekend publish 133

     [OK] That seemed to work. Today is not a weekend. Module 133 has been Unpublished
```

### Em Uso

A lógica de uso é um pouco ilógica! Se você tem um módulo que deve ser publicado somente no fim de semana, os parâmetros a serem usados todos os dias são `weekend publish module_id`. Isso publica o módulo nos dias de fim de semana e o despublica nos dias que não são de fim de semana. Para um módulo que precisa ser publicado em dias úteis, os parâmetros a serem usados todos os dias são `weekend unpublish module_id`. Isso despublica o módulo nos dias de fim de semana e o publica nos dias que não são de fim de semana. A mesma lógica se aplica à versão `winter`.

## O cronogram

O comando pode ser testado em uma janela de terminal, mas você provavelmente vai querer usá-lo a partir de um cron. A opção `winter` poderia ser executada no primeiro dia de cada mês. A opção `weekend` seria executada diariamente. O ponto importante é que você pode ter tantos crons quanto precisar para alterar o estado publicado de quantos módulos quiser em quaisquer intervalos apropriados. O mesmo código funciona para todos.

Em um serviço de hospedagem, você precisa fornecer os caminhos completos para o executável php e o comando cli do joomla. Exemplo:

```sh
/usr/local/bin/php /home/username/public_html/pathtojoomla/cli/joomla.php onoffbydate:action winter publish 130
```

Dependendo de como você configurou seu cron e seu sistema, você pode receber um e-mail reconfortante contendo exatamente as mesmas informações que vê na linha de comando.

## Verificar

E, claro: vá para a sua página inicial e verifique se o módulo realmente foi publicado ou despublicado.

*Traduzido por openai.com*

