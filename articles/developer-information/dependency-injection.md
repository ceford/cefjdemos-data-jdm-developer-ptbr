<!-- Filename: J4.x:Dependency_Injection_in_Joomla_4 / Display title: Injeção de Dependência -->

## Introdução

Injeção de dependência (DI) é uma técnica de desenvolvimento de software que fornece a uma classe suas dependências de fora, em vez de criá-las internamente. Ela melhora a flexibilidade, a manutenibilidade e a testabilidade do código. No entanto, é difícil de entender!

Há uma explicação sobre DI e Containers de Injeção de Dependência na documentação do [Joomla Framework DI](https://github.com/joomla-framework/di/blob/4.x-dev/docs/why-dependency-injection.md). Também há uma [Visão Geral](https://github.com/joomla-framework/di/blob/4.x-dev/docs/overview.md) do seu uso.

As explicações na seção [Injeção de Dependência](jdocmanual?article=docus/dependency-injection/index) da Documentação para Programadores do Joomla! não são tão fáceis de entender!

## Exemplos

Se seu objetivo é começar a construir uma extensão, os exemplos a seguir, retirados das extensões principais do Joomla, podem ajudar.

O código de injeção de dependência está localizado em services/provider.php, mas a localização exata desse arquivo depende do tipo de extensão.

### Exemplo de Componente

O caminho completo para o arquivo services/provider.php é administrator/components/com_example/services/provider.php. Este exemplo é com_tags, que não usa categorias. Se o seu componente usar categorias, dê uma olhada no código services/provider.php para com_content. Você precisará de várias instruções relacionadas a Categorias.

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  com_tags
 *
 * @copyright   (C) 2018 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Component\Router\RouterFactoryInterface;
use Joomla\CMS\Dispatcher\ComponentDispatcherFactoryInterface;
use Joomla\CMS\Extension\ComponentInterface;
use Joomla\CMS\Extension\Service\Provider\ComponentDispatcherFactory;
use Joomla\CMS\Extension\Service\Provider\MVCFactory;
use Joomla\CMS\Extension\Service\Provider\RouterFactory;
use Joomla\CMS\MVC\Factory\MVCFactoryInterface;
use Joomla\Component\Tags\Administrator\Extension\TagsComponent;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The tags service provider.
 *
 * @since  4.0.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new RouterFactory('\\Joomla\\Component\\Tags'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new TagsComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
};
```

### Exemplo de Módulo

Este é o módulo de Versão que mostra a versão do Joomla na barra de título das páginas do Administrador. O código está em administrator/modules/mod_version/services/provider.php.

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  mod_version
 *
 * @copyright   (C) 2024 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\HelperFactory;
use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   5.1.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Joomla\\Module\\Version'));
        $container->registerServiceProvider(new HelperFactory('\\Joomla\\Module\\Version\\Administrator\\Helper'));

        $container->registerServiceProvider(new Module());
    }
};
```

### Exemplo de Plugin

Este é o plugin que fornece informações de contato nas páginas de conteúdo. O código está localizado em plugins/content/contact/services/provider.php

```php
<?php

/**
 * @package     Joomla.Plugin
 * @subpackage  Content.contact
 *
 * @copyright   (C) 2022 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\Database\DatabaseInterface;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Joomla\Plugin\Content\Contact\Extension\Contact;

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
                $plugin     = new Contact(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'contact')
                );
                $plugin->setApplication(Factory::getApplication());
                $plugin->setDatabase($container->get(DatabaseInterface::class));

                return $plugin;
            }
        );
    }
};
```

*Traduzido por openai.com*

