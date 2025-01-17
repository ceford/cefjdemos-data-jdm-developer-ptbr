<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Sistemas de Arquivos em Nuvem para Gerenciador de Mídia -->

<span id="main-portal-heading">GSoC 2017 Sistemas de Arquivos em Nuvem para Gerenciador de Mídia Documentação</span> Joomla! 4.x

## Introdução

**Joomla! 4.x** vem com sistemas de arquivos em nuvem para o **Gerenciador de Mídia** por padrão. Com a API anterior, criar sistemas de arquivos personalizados era uma tarefa difícil. Graças à nova API, agora é fácil criar um sistema de arquivos personalizado. Se você deseja usar um serviço de nuvem com o novo Gerenciador de Mídia, é recomendável usar **OAuth2.0**.

Este documento irá guiá-lo por etapas importantes para criar seu próprio **Plug-in de Sistema de Arquivos** para estender o **Gerenciador de Mídia**. Antes de prosseguir, certifique-se de que você possui o conhecimento básico sobre como desenvolver um plug-in para o Joomla. [Este tutorial](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla "Special:MyLanguage/J3.x:Creating a Plugin for Joomla") deve ajudar.

## Crie seu plugin de sistema de arquivos

### Configuração

Primeiramente, precisamos criar um plugin de **sistema de arquivos** para estender o Gerenciador de Mídia. Este plugin deve conter alguns atributos importantes para que possa funcionar com o Gerenciador de Mídia.

Certifique-se de que seu plugin contenha `group="filesystem"`. Então, no seu `[plugin-name].xml`, você deve ter:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="4.0" type="plugin" group="filesystem" method="upgrade">
    <name>plg_filesystem_myplugin</name>
    <author>Joomla! Project</author>
    <creationDate>April 2017</creationDate>
    <copyright>Copyright (C) 2005 - 2017 Open Source Matters. All rights reserved.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>__DEPLOY_VERSION__</version>
    <description>Description</description>
    <files>
        <filename plugin="myplugin">myplugin.php</filename>
        <folder>SomeFolder</folder>
    </files>

    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="display_name"
                    type="text"
                    label="YOUR_LABEL"
                    description="YOUR_DESCRIPTION"
                    default="DEFAULT_VALUE"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

O parâmetro **display_name** ajuda o Gerenciador de Mídia a exibir o nome do seu **Sistema de Arquivos** como um nó raiz no Navegador de Arquivos. Qualquer adaptador pertencente ao seu Sistema de Arquivos será exibido como filho sob a raiz.

Inclua quaisquer parâmetros necessários, como `App Secret`, com Campos de Formulário adequados.

### Eventos de Plugin

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

Qualquer plugin de sistema de arquivos deve conter um evento chamado `onFileSystemGetAdapters()` para funcionalidade. Este evento deve retornar um **array** de `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` quando for chamado. O evento é acionado quando você abre o Gerenciador de Mídia. Cada `AdapterInterface` será montado sob o nó raiz do seu sistema de arquivos.

#### onFileSystemOAuthCallback()

Este evento é disparado quando você usa o **OAuthCallbackHandler** do Media Manager para o processo de Autorização e Autenticação OAuth2.0 descrito abaixo no documento. O evento é disparado apenas no plugin requisitado. Portanto, não há necessidade de verificar o `$context` em um cenário típico.

Um exemplo de uso do evento é parecido com:

```php
    public function onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)
    {
        // Your context
        $context = $event->getContext();

        // Get the input
        $data = $event->getInput();

        // Your code goes here

        // Set result to be returned
        $result = [
            "action" => "control-panel"
        ];

        // Pass back the result to event
        $event->setArgument('result', $result);
    }
```

**OAuthCallbackEvent** contém a entrada encaminhada para o URI do Media Manager OAuthCallback. `$event->getInput()` retorna um Objeto Input.

`$result` define como o Joomla se comporta após um redirecionamento para o callback. Existem várias soluções possíveis disponíveis:

- `action`
  - close: Fecha uma janela que foi aberta por um JavaScript **usando
    window.open()**
  - redirect: Redireciona para a página especificada pelo **redirect_uri**
  - control-panel: Redireciona para o painel de controle
  - media-manager: Redireciona para o Gerenciador de Mídia
- `redirect_uri`
  - Usado com a ação **redirect** (obrigatório)
- `message`
  - Mensagem precisa ser exibida após uma ação
- `message_type`
  - warning
  - notice
  - error
  - message(ou deixar vazio)

Depois de configurar tudo, defina o argumento `$result` do `$event` para passá-lo de volta ao chamador.

Um exemplo para exibir uma mensagem ao usuário seria:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Isso redirecionará para o Gerenciador de Mídia e exibirá um aviso com o texto em **mensagem**.

Este método pode ser tipicamente usado para obter códigos de autorização para o processo **OAuth2.0**. Em caso de **erro**, o Joomla! retornará ao painel de controle e exibirá uma mensagem de erro.

## Autenticação e Autorização

O Joomla! 4.x aconselha o uso de OAuth2.0 para este processo. É um cenário comum que o OAuth2.0 exija uma **URL de redirecionamento** para passar dados de autenticação para a Aplicação. Isso é tipicamente feito por um manipulador de retorno. Para o seu plugin, não é necessário reinventar a roda, você pode usar o **Manipulador de Retorno** do **Gerenciador de Mídia** para realizar a tarefa.

Tudo o que você precisa fazer é definir o **URI de Redirecionamento** para o seguinte no seu
Fornecedor OAuth2.0 e implementar o
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
no seu plugin.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[your-plugin-name]`

Neste exemplo:  
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Agora, quando você fizer um redirecionamento do seu provedor uma vez que ele alcançar a URL fornecida, o **Controlador** do Gerenciador de Mídia garantirá que seu plugin implemente `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` antes de passar quaisquer dados para ele. Caso contrário, você será redirecionado para o Painel de Controle com uma mensagem de erro.

Se você implementou todas as entradas que são passadas, o Provedor de Nuvem será incluído no `$event`. Você pode acessá-los usando `$event->getInput()` e tratá-los como uma `input` comum para o Joomla.

Após receber o `input`, você pode usá-lo para obter os detalhes do seu serviço de nuvem, como **Token de Acesso**, **Token de Atualização**, etc.

Se você quiser distinguir várias contas, pode usar `Session` para isso.

**Nota**: É aconselhável verificar o **token CSRF** antes de prosseguir com sua solicitação. A maioria dos provedores de nuvem suporta o parâmetro `state` que pode ser usado para cumprir a tarefa.

### Armadilhas Comuns

É necessário usar `urlencode()` no seu URI de redirecionamento ao enviá-lo para o provedor de nuvem. Use isso para evitar comportamentos indesejados da sua nuvem.

## Servir Arquivo do seu Adaptador

Para servir seus arquivos de mídia do Media Manager para o **Joomla! Site** `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` ajuda você. Esta Interface contém um método especial chamado `getUrl($path)`.

`$path : O caminho é relativo à sua raiz`

A intenção do método é fornecer uma **URL Absoluta Pública** para o arquivo que você solicitou inserir no site. Por exemplo, suponha que o caminho do seu arquivo seja `/path/to/me.png` no servidor em nuvem. Agora uma URL pública pode ser algo como `mycloud.com/share/u/myusername/path/to/me.png`. Como ele será servido, depende completamente de você. Quando um usuário quiser inserir uma URL gerada de mídia, este método será utilizado.

Mais detalhes sobre `Adapter Interface` podem ser encontrados em
`administrator/componenents/com_media/Adapter/AdapterInterface.php`

*Traduzido por openai.com*

