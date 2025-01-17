<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Namespaces -->

## Introdução

O uso de **Namespaces PHP** é uma maneira de agrupar classes, interfaces, funções e constantes relacionadas para evitar conflitos de nomes em projetos maiores. Eles permitem que os desenvolvedores organizem o código de forma mais eficaz, especialmente ao trabalhar com bibliotecas de terceiros ou bases de código grandes que podem ter nomes de classe duplicados.

Há mais informações na seção [Namespace](jdocmanual?article=docus/namespaces/index) da Documentação para Programadores do Joomla!.

### Características Principais dos Namespaces do PHP

1. **Evite Conflitos de Nomes**:
   - Sem namespaces, se duas classes tiverem o mesmo nome, o PHP gerará um erro. Namespaces fornecem um contexto para identificar exclusivamente classes ou outros elementos.
2. **Organizar Código**:
   - Namespaces permitem o agrupamento lógico de classes ou funções relacionadas, tornando o código mais fácil de ler e manter.
3. **Acesso via Nomes Completamente Qualificados**:
   - Classes e funções dentro de um namespace podem ser acessadas usando seu nome completamente qualificado, que inclui o caminho do namespace.

### Práticas Comuns:
- Use namespaces para espelhar a estrutura de pastas do seu projeto, mantendo a consistência.
- Importe namespaces usando a palavra-chave `use` para um código mais limpo.

## O Arquivo de Manifesto

A declaração inicial de um namespace de extensão está localizada em um arquivo de manifesto de extensão, como neste exemplo:

```xml
    <namespace path="src">Acme\Component\Wonderful</namespace>
```

Os itens nesta declaração são os seguintes:

- **path="src"** informa ao carregador de classes que as classes com namespace estão na pasta **src** da extensão, por exemplo com_wonderful/src.
- **Acme** é o prefixo do fornecedor. Para extensões do núcleo do Joomla, seria *Joomla*. Você precisa inventar seu próprio prefixo de fornecedor exclusivo.
- **Component** indica o tipo de extensão (componente, módulo, plugin, template, biblioteca, ...).
- **Wonderful** é o nome da extensão. Escolha um nome de extensão que seja curto, significativo e provavelmente único.

## O Arquivo de Classe

A declaração de namespace deve ser a primeira instrução no arquivo em que é usada. Por exemplo:

```php
<?php

/**
 * @package     Wonderful
 * @subpackage  Site
 *
 * @copyright   (C) 2024 Merlin. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Acme\Component\Wonderful\Controller;

use Joomla\CMS\MVC\Controller\BaseController;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Controller to display the default page - display of wonderful items
 *
 * @since  1.0.0
 */
class MagicController extends BaseController
{
    public function dosomething($wonderful)
    {
        // ... output something wonderful!
    }
}
```

## Usando o Arquivo de Classe

Quando você precisar usar a classe, por exemplo em um template, você deverá inserir uma declaração **use** no topo do template:

```
use Acme\Component\Wonderful\Controller\MagicController;
```

E então instancie a classe e chame a função:

```
$magic = new MagicController;
$result = $magic->dosomething('wonderful');
```

## O arquivo autoload_psr4.php

Este arquivo está localizado na pasta administrator/cache do site Joomla. Ele contém um mapa completo dos namespaces extraídos dos arquivos de manifesto. É gerado na instalação e regenerado sempre que uma extensão é instalada ou reinstalada. Você pode deletar este arquivo e ele será regenerado no próximo carregamento da página. Às vezes, é necessário fazer isso se a instalação da extensão tiver sido tentada por um método incomum. O arquivo contém 275 ou mais caminhos para locais de classes com namespace.

**PSR** é um acrônimo para [**Recomendação de Padrões PHP**](https://www.php-fig.org/psr/) e [**PSR-4: Autoloader**](https://www.php-fig.org/psr/psr-4/) é um padrão aceito para carregamento de classes PHP.

*Traduzido por openai.com*

