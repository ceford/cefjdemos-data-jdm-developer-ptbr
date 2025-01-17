<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Startup_Files / Display title: Anatomia MVC: Arquivos de Inicialização do Administrador -->

## Visão Geral dos Arquivos do Administrador

Existem mais arquivos na parte administrativa do componente, em parte porque há duas tabelas, cada uma com uma visão de lista e edição, e em parte porque alguns dos arquivos controlam aspectos do comportamento do componente em ambas as interfaces, do site e do administrador.

A criação de código para uma visualização de lista foi abordada na parte do tutorial referente aos arquivos do site, portanto não será repetida aqui. Esta seção tratará dos arquivos comuns à maioria dos componentes. Um tutorial posterior abordará uma das visualizações de edição necessárias para atualizar ou adicionar novos registros.

## serviços/provedor.php

Este arquivo é usado quando a aplicação Joomla chama o ComponentHelper para renderizar o componente. É o primeiro código do componente a ser executado tanto para o Site quanto para o Administrador. Sua função é registrar o componente:

```php
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new RouterFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new CountrybaseComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
```

## src/Extension/CountrybaseComponent.php

A função register deste arquivo é chamada a partir da declaração \$component = new CountrybaseComponent(...); em services/provider.php. Seu objetivo é inicializar o componente nas interfaces do Site e do Administrador.

```php
       public function boot(ContainerInterface $container)
        {
        }
```

## access.xml

Este arquivo define os controles de acesso usados neste componente. Os itens listados aqui aparecem na guia de Permissões de Opções do componente.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<access component="com_countrybase">
    <section name="component">
        <action name="core.admin" title="JACTION_ADMIN" />
        <action name="core.options" title="JACTION_OPTIONS" />
        <action name="core.manage" title="JACTION_MANAGE" />
        <action name="core.create" title="JACTION_CREATE" />
        <action name="core.delete" title="JACTION_DELETE" />
        <action name="core.edit" title="JACTION_EDIT" />
    </section>
</access>
```

## config.xml

Este arquivo é usado para controlar se alguma opção aparece no formulário de *Opções* do componente e em qual aba elas aparecem. Para com_countrybase não há opções além das opções de permissões padrão do Joomla. Seria possível adicionar opções aqui, por exemplo, se deseja mostrar ou ocultar uma coluna específica na exibição de saída. Isso iria em um conjunto de campos separado chamado Opções.

```xml
<?xml version="1.0" encoding="utf-8"?>
<config>

    <fieldset
        name="permissions"
        label="JCONFIG_PERMISSIONS_LABEL"
        description="JCONFIG_PERMISSIONS_DESC"
        >

        <field
            name="rules"
            type="rules"
            label="JCONFIG_PERMISSIONS_LABEL"
            filter="rules"
            validate="rules"
            component="com_countrybase"
            section="component"
         />
    </fieldset>
</config>
```

*Traduzido por openai.com*

