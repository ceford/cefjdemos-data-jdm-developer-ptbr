<!-- Filename: How_to_use_JDate / Display title: Como usar a classe Date -->

## Introdução
A classe Date do Joomla é uma classe auxiliar, estendida da classe DateTime do PHP, que permite aos desenvolvedores lidar com formatação de datas de maneira mais eficiente. A classe permite que os desenvolvedores formatem datas para strings legíveis, interação com MySQL, cálculo de timestamps UNIX, e também fornece métodos auxiliares para trabalhar em diferentes fusos horários.

## Criando uma Instância de Data

Todos os métodos auxiliares de data exigem uma instância da classe Date. Para começar, você deve criar uma. Um objeto Date pode ser criado de duas maneiras. Uma delas é o método nativo típico de simplesmente criar uma nova instância:

```php
use Joomla\CMS\Date\Date;

$date = new Date(); // Creates a new Date object equal to the current time.
```

Você também pode criar uma instância usando o método estático definido em Date:

```php
use Joomla\CMS\Date\Date;

$date = Date::getInstance(); // Alias of 'new Date();'
```

Não há diferença entre esses métodos, pois Date::getInstance simplesmente cria uma nova instância de Date exatamente como o primeiro método mostrado.

Alternativamente, você também pode obter a data atual (como um objeto Date) do objeto Application, usando:
```php
use Joomla\CMS\Factory;

$date = Factory::getDate();
```

## Argumentos

O construtor Date (e o método estático getInstance) aceita dois parâmetros opcionais: uma string de data para formatar e um fuso horário. Não passar uma string de data criará um objeto Date com a data e hora atuais, enquanto não passar um fuso horário permitirá que o objeto Date use o fuso horário padrão configurado.

O primeiro argumento, se utilizado, deve ser uma string que pode ser analisada usando o construtor nativo DateTime do PHP. Exemplo:
```php
use Joomla\CMS\Date\Date;

$currentTime = new Date('now'); // Current date and time
$tomorrowTime = new Date('now +1 day'); // Current date and time, + 1 day.
$plus1MonthTime = new Date('now +1 month'); // Current date and time, + 1 month.
$plus1YearTime = new Date('now +1 year'); // Current date and time, + 1 year.
$plus1YearAnd1MonthTime = new Date('now +1 year +1 month'); // Current date and time, + 1 year and 1 month.
$plusTimeToTime = new Date('now +1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$plusTimeToTime = new Date('now -1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$combinedTimeToTime = new Date('now -1 hour -30 minutes 23 seconds'); // Current date and time, - 1 hour, +30 minutes and +23 seconds

$date = new Date('2012-12-1 15:20:00'); // 3:20 PM, December 1st, 2012
```

Um carimbo de data/hora Unix (em segundos) também pode ser passado como o primeiro argumento, caso em que ele será internamente transformado em uma data. Se um fuso horário tiver sido especificado como o segundo argumento para o construtor, ele será convertido para esse fuso horário.

## Exibindo Datas

Uma nota de cautela ao exibir objetos Date em um contexto de usuário: não os imprima diretamente na tela. O método toString() do objeto Date simplesmente chama o método format() do seu pai, sem consideração para fusos horários ou formatação de data localizada. Isso não resultará em uma boa experiência do usuário e levará a inconsistências entre a formatação na sua extensão e em outras partes do Joomla. Em vez disso, você deve sempre exibir Datas usando os métodos mostrados abaixo.

### Formatos Comuns de Data

Vários formatos de data são predefinidos no Joomla como parte dos pacotes de idioma base. Isso é benéfico porque significa que os formatos de data podem ser facilmente internacionalizados. Um exemplo das strings de formato disponíveis está abaixo, do pacote de idioma en-GB. É altamente recomendado utilizar essas strings de formatação ao exibir datas, para que suas datas sejam automaticamente reformatadas de acordo com o local do usuário. Elas podem ser recuperadas da mesma forma que qualquer string de idioma (veja abaixo para exemplos).

```ini
DATE_FORMAT_LC="l, d F Y"
DATE_FORMAT_LC1="l, d F Y"
DATE_FORMAT_LC2="l, d F Y H:i"
DATE_FORMAT_LC3="d F Y"
DATE_FORMAT_LC4="Y-m-d"
DATE_FORMAT_LC5="Y-m-d H:i"
DATE_FORMAT_LC6="Y-m-d H:i:s"
DATE_FORMAT_JS1="y-m-d"
DATE_FORMAT_CALENDAR_DATE="%Y-%m-%d"
DATE_FORMAT_CALENDAR_DATETIME="%Y-%m-%d %H:%M:%S"
DATE_FORMAT_FILTER_DATE="Y-m-d"
DATE_FORMAT_FILTER_DATETIME="Y-m-d H:i:s"
```

### O Método HtmlHelper (Recomendado)

Assim como muitos itens comuns de saída, a classe HtmlHelper está aqui para... ajudar! O método date() do HtmlHelper aceitará qualquer string de data e hora que o construtor Date aceitaria, juntamente com uma string de formatação, e exibirá a data de forma adequada às configurações de fuso horário do usuário atual. Assim, este é o método recomendado para exibir datas para o usuário.

```php
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

$myDateString = '2012-12-1 15:20:00';
echo HtmlHelper::date($myDateString, Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Método format() do Objeto Date

Outra opção é formatar a Data manualmente. Se este método for usado, você terá que recuperar e definir manualmente o fuso horário do usuário. Este método é mais útil para formatar datas fora da interface do usuário, como em logs de sistema ou chamadas de API.

```php
use Joomla\CMS\Language\Text;
use Joomla\CMS\Date\Date;
use Joomla\CMS\Factory;

$myDateString = '2012-12-1 15:20:00';
$timezone = Factory::getUser()->getTimezone();

$date = new Date($myDateString);
$date->setTimezone($timezone);
echo $date->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

## Outros Exemplos Úteis de Código

### Exibindo Rapidamente a Hora Atual

Existem duas maneiras fáceis de fazer isso.
- O método date() do HtmlHelper, se nenhum valor de data for fornecido, irá usar por padrão o horário atual.
- Factory::getDate() obtém a data atual como um objeto Date, que podemos formatar em seguida.

```php
use Joomla\CMS\Factory;
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

// These two are functionally equivalent
echo HtmlHelper::date('now', Text::_('DATE_FORMAT_FILTER_DATETIME'));

$timezone = Factory::getUser()->getTimezone();
echo Factory::getDate()->setTimezone($timezone)->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Adicionando e Subtraindo de Datas

Como o objeto Joomla Date estende o objeto PHP DateTime, ele fornece métodos para adicionar e subtrair datas. O mais fácil desses métodos de usar é o método modify(), que aceita qualquer string de modificação relativa que o método PHP [strtotime](https://www.php.net/manual/en/function.strtotime.php) aceitaria. Por exemplo:

```php
use Joomla\CMS\Date\Date;

$date = new Date('2012-12-1 15:20:00');
$date->modify('+1 year');
echo $date->toSQL(); // 2013-12-01 15:20:00
```

Existem também os métodos separados add() e sub(), para adicionar ou subtrair tempo de um objeto de data, respectivamente. Estes aceitam objetos [DateInterval](https://www.php.net/manual/en/class.dateinterval.php) padrão do PHP:

```php
use Joomla\CMS\Date\Date;

$interval = new \DateInterval('P1Y1D'); // Interval represents 1 year and 1 day

$date1 = new Date('2012-12-1 15:20:00');
$date1->add($interval);
echo $date1->toSQL(); // 2013-12-02 15:20:00

$date2 = new Date('2012-12-1 15:20:00');
$date2->sub($interval);
echo $date2->toSQL(); // 2011-11-30 15:20:00
```

### Exibindo Datas no Formato ISO 8601

```php
$date = new Date('2012-12-1 15:20:00');
$date->toISO8601(); // 20121201T152000Z
```

### Exibindo Datas no Formato RFC 822

```php
$date = new Date('2012-12-1 15:20:00');
$date->toRFC822(); // Sat, 01 Dec 2012 15:20:00 +0000
```

### Emitindo Datas no Formato de Data e Hora do SQL

```php
$date = new Date('20121201T152000Z');
$date->toSQL(); // 2012-12-01 15:20:00
```

### Exibindo Datas como Timestamps Unix

Um timestamp Unix é expresso como o número de segundos que se passaram desde a Época Unix (o primeiro segundo de 1º de janeiro de 1970).

*Traduzido por openai.com*

