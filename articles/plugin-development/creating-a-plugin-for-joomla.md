<!-- Filename: J4.x:Creating_a_Plugin_for_Joomla / Display title: Criando um plugin para o Joomla -->

<span id="main-portal-heading">Tutorial
Como criar um plugin para o Joomla 4</span> Joomla!  4.x series

A estrutura de plugins para o Joomla! 1.5, 2.5 e 3.x era muito flexível
e poderosa. Os plug-ins podem ser usados, não apenas, para lidar com
eventos acionados pelo aplicativo principal e pelas extensões, mas
também podem ser usados para tornar as extensões de terceiros
extensíveis e poderosas. No Joomla 4.x nós reescrevemos muito do sistema
dispatcher por trás disso para aumentar ainda mais a flexibilidade
quando você modifica os parâmetros passados como eventos enquanto
simultaneamente aumenta o desempenho dos plugins.

Este tutorial deve fornecer o básico do que você precisa saber para
desenvolver seu próprio plugin. A maioria dos plugins consiste em apenas
um único arquivo de código, mas para instalar corretamente o código do
plugin, ele deve ser empacotado em um arquivo de instalação que pode ser
processado pelo instalador do Joomla.

### Criando o arquivo de instalação

Tal como acontece com todas as extensões no Joomla, os plugins são
facilmente instalados como um arquivo .zip (.tar.gz também é suportado),
mas um arquivo XML formatado corretamente deve ser incluído.
Como exemplo, aqui está o arquivo de instalação XML para o plugin de
pesquisa de categorias:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="search" method="upgrade">
	<name>plg_search_categories</name>
	<author>Joomla! Project</author>
	<creationDate>November 2005</creationDate>
	<copyright>Copyright (C) 2005 - 2018 Open Source Matters. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<authorEmail>admin@joomla.org</authorEmail>
	<authorUrl>www.joomla.org</authorUrl>
	<version>3.0.0</version>
	<description>PLG_SEARCH_CATEGORIES_XML_DESCRIPTION</description>
	<files>
		<filename plugin="categories">categories.php</filename>
	</files>
	<languages>
		<language tag="en-GB">en-GB.plg_search_categories.ini</language>
		<language tag="en-GB">en-GB.plg_search_categories.sys.ini</language>
	</languages>
	<config>
		<fields name="params">

			<fieldset name="basic">
				<field
					name="search_limit"
					type="number"
					label="JFIELD_PLG_SEARCH_SEARCHLIMIT_LABEL"
					default="50"
				/>

				<field
					name="search_content"
					type="radio"
					label="JFIELD_PLG_SEARCH_ALL_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>

				<field
					name="search_archived"
					type="radio"
					label="JFIELD_PLG_SEARCH_ARCHIVED_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>
			</fieldset>

		</fields>
	</config>
</extension>
```

Como você pode ver, o sistema é semelhante a outros arquivos de
instalação XML do Joomla. Você só precisa procurar a entrada
`group="xxx"` na marcação e as informações estendidas na marcação . Esta
informação diz ao Joomla em qual pasta copiar o arquivo e em qual grupo
o plugin deve ser adicionado.

Se você estiver criando um plug-in que responde a eventos principais
existentes, o atributo `group="xxx"` será alterado para refletir o nome
da pasta de plug-in existente para o tipo de evento que você deseja
aumentar. Por exemplo, `group="authentication"` ou `group="user"`.
Consulte
[Plugin/Eventos](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
para obter uma lista completa das categorias de eventos principais
existentes. Ao criar um novo plug-in para responder a eventos
principais, é importante que o nome do seu plug-in seja exclusivo e não
entre em conflito com nenhum dos outros plug-ins que também possam estar
respondendo ao evento principal que você deseja atender.

Se você estiver criando um plug-in para responder a eventos não
essenciais do sistema, sua escolha para a marcação `group="xxx"` deve
ser diferente de qualquer uma das categorias principais existentes.

**Dica:** *Se você adicionar o atributo `method="upgrade"` à tag
`extension`, este plugin pode ser instalado sem desinstalar uma versão
anterior. Todos os arquivos existentes serão substituídos, mas os
arquivos antigos não serão excluídos.*

### Criando o plug-in

No Joomla 4 você também pode ter código com espaço de nomes em seu
plugin - por favor leia [Convenções de espaço de nomes no Joomla
4](https://docs.joomla.org/J4.x:Namespace_Conventions_In_Joomla "J4.x:Namespace Conventions In Joomla").
Observe que o arquivo de entrada principal não tem espaço de nomes, mas
os campos e qualquer outro código de suporte têm.

A maneira orientada a objetos de escrever plugins envolve escrever uma
subclasse do <a
href="https://api.joomla.org/cms-4/classes/Joomla.CMS.Plugin.CMSPlugin.html"
class="external text" target="_blank"
rel="noreferrer noopener">CMSPlugin</a>, uma classe base que implementa
as propriedades básicas de plug-ins. Em seus métodos, as seguintes
propriedades estão disponíveis:

- `$this->params`: os
  [parâmetros](https://docs.joomla.org/Parameter "Special:MyLanguage/Parameter")
  definidos para este plugin pelo administrador
- `$this->_name`: o nome do plug-in
- `$this->_type`: o grupo (tipo) do plugin
- `$this->db`: o objeto db
- `$this->app`: o objeto de aplicação

**DICA:** *Para usar `$this->db` e `$this->app`, o `CMSPlugin` testa se
a propriedade existe e não é privada. Se desejar que os objetos padrões
sejam usados, crie propriedades não instanciadas na classe do plugin (ou
seja, `protected $db; protected $app;` na mesma área que
`protected $autoloadLanguage = true;`). As propriedades não existirão a
menos que sejam explicitamente criadas.*

No exemplo de código a seguir, representa o grupo (tipo) do plug-in e
representa seu nome. Observe que os nomes de classes e funções em PHP
não diferenciam maiúsculas de minúsculas.

Também implementamos a <a
href="https://api.joomla.org/framework-2/classes/Joomla.Event.SubscriberInterface.html"
class="external text" target="_blank"
rel="noreferrer noopener">SubscriberInterface</a> aqui, que é a
principal mudança do Joomla 1.5-3.x. Em vez do nome da função ser
detectado automaticamente e ser o mesmo que o nome do evento, isso
permite que você tenha nomes de funções personalizados. Isso nos permite
dizer quais plugins estão implementando quais funções e como a análise
de métodos públicos no código PHP é lenta, dá um aumento significativo
no desempenho.

Observe que em toda a série Joomla 4 há uma camada obsoleta que cobrirá
plugins usando a antiga estratégia de nomenclatura de nomes de plugins
sendo os mesmos que o nome do evento quando a SubscriberInterface não é
implementada.

```php
<?php
// no direct access
defined( '_JEXEC' ) or die;

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\Event;
use Joomla\Event\SubscriberInterface;

class Plg<PluginGroup><PluginName> extends CMSPlugin implements SubscriberInterface
{
	/**
	 * Load the language file on instantiation
	 *
	 * @var    boolean
	 * @since  3.1
	 */
	protected $autoloadLanguage = true;

	/**
	 * Returns an array of events this subscriber will listen to.
	 *
	 * @return  array
	 */
	public static function getSubscribedEvents(): array
	{
		return [
			'<EventName>' => 'myFunctionName',
		];
	}

	/**
	 * Plugin method is the array value in the getSubscribedEvents method
	 * The plugin then modifies the Event object (if it's not immutable)
	 */
	 public function myFunctionName(Event $event)
	 {
		/*
		 * Plugin code goes here.
		 * You can access parameters via $this->params
		 */
		return true;
	}
}
?>
```

### Usando plugins em seu código

Se você estiver criando um plug-in para um novo evento não essencial,
lembre-se de ativar seu plug-in depois de instalá-lo. Preceda qualquer
referência ao seu novo plugin com o comando
`JPluginHelper::importPlugin()`.

Agora que você criou seu plug-in, provavelmente desejará chamá-lo em seu
código. Você pode não: o núcleo do Joomla tem vários eventos embutidos
nos quais você pode querer que seu código de plugin seja registrado (e
nesse caso você pode ignorar esta seção).

#### Nova maneira do Joomla 4

A nova maneira de fazer isso no Joomla 4 é obter o dispatcher e enviar
um evento nomeado.

```php
    use Joomla\CMS\Event\AbstractEvent;
    use Joomla\CMS\Factory;

    $dispatcher = Factory::getApplication()->getDispatcher();

    // Here we create an event however as long as you implement EventInterface you can create your own
    // custom classes
    $event = AbstractEvent::create(
        '',
        [
            'name' => $value,
        ]
    );

    $eventResult = $dispatcher->dispatch('', $event);
```

Se você deseja permitir que o usuário modifique valores, você pode usar
o resultado do evento e obter resultados (getResults) de volta. Você
pode olhar

```php
    defined('_JEXEC') or die;

    use BadMethodCallException;
    use Joomla\CMS\Event\AbstractImmutableEvent;
    use Joomla\CMS\Table\TableInterface;

    /**
     * Event class for an event
     */
    class MyCustomEvent extends AbstractImmutableEvent
    {
        /**
         * Constructor.
         *
         * @param   string  $name       The event name.
         * @param   array   $arguments  The event arguments.
         *
         * @throws  BadMethodCallException
         */
        public function __construct($name, array $arguments = array())
        {
            if (!array_key_exists('myProperty', $arguments))
            {
                throw new BadMethodCallException("Argument 'myProperty' is required for event $name");
            }

            parent::__construct($name, $arguments);
        }

        /**
         * Setter for the myProperty argument
         *
         * @param   mixed  $value  The value to set
         *
         * @return  mixed
         *
         * @throws  BadMethodCallException  if the argument is not of the expected type
         */
        protected function setMyProperty($value)
        {
            if (!empty($value) && !is_object($value) && !is_array($value))
            {
                throw new BadMethodCallException("Argument 'src' of event {$this->name} must be empty, object or array");
            }

            return $value;
        }
    }
```

Por que introduzimos essa classe de nome sobre parâmetros? Bem, facilita
a introdução de setters e getters personalizados para propriedades -
atualmente, um plug-in pode alterar completamente uma propriedade como
desejar - para um componente, não há como impor limitações. Além disso,
torna muito mais fácil para os desenvolvedores adicionar e remover
parâmetros em um evento sem ter grandes problemas b/c (já que agora você
está chamando métodos definidos e não está sujeito a uma propriedade
sendo o segundo argumento de sua função).

#### Como obter compatibilidade máxima com o Joomla 3

Se você deseja acionar um evento de maneira semelhante ao
JEventDispatcher do J3.x removido, use um código como este:

```php
    $results = \Joomla\CMS\Factory::getApplication()->triggerEvent( '',  );
```

É importante notar que os parâmetros devem estar em um arranjo. A
própria função do plug-in obterá os parâmetros como um objeto de Evento
se implementar a SubscriberInterface e como valores individuais se não,
mas esse método sempre retornará um arranjo que o plug-in retorna.

Observe que, se algum plug-in em um grupo não implementar a
SubscriberInterface, a propriedade de resultado (como um parâmetro
nomeado e resultado de um plug-in) será usada como uma propriedade
especial e não poderá ser usada.
