<!-- Filename: J4.x:Joomla_4_Tips_and_Tricks:_Form_Validation_Basics / Display title: Validação de Formulário -->

## Introdução

Joomla possui um script de validação no lado do cliente que pode ser utilizado e estendido por qualquer componente. Este artigo é sobre como ele funciona e como usá-lo. Existem quatro validadores padrão para tipos de campo de nome de usuário, senha, numérico e e-mail. Há também um validador de padrão de uso mais geral e um validador de campo obrigatório.

Observe que navegadores modernos também oferecem validação de formulários por padrão. A validação do navegador pode ser desativada incluindo novalidate na tag do formulário.

## Como invocar a validação

No início do arquivo edit.php que contém o código de geração do formulário, inclua a chamada para carregar o arquivo javascript do validador:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();
$wa->useScript('keepalive')
    ->useScript('form.validate');
```

no elemento form, certifique-se de incluir class="form-validate". Este exemplo é do formulário de edição do usuário:

```html
<form action="<?php echo Route::_('index.php?option=com_users&layout=edit&id=' . (int) $this->item->id); ?>"
    method="post" name="adminForm" id="user-form"
    enctype="multipart/form-data"
    aria-label="<?php echo Text::_('COM_USERS_USER_FORM_' . ( (int) $this->item->id === 0 ? 'NEW' : 'EDIT'), true); ?>"
    class="form-validate">
```

No arquivo de formulário XML, inclua as instruções que invocam a validação de campo individual. Este exemplo é o campo de e-mail do usuário:

```xml
        <field
            name="email"
            type="email"
            label="JGLOBAL_EMAIL"
            required="true"
            size="30"
            validate="email"
            validDomains="com_users.domains"
        />
```

## As Expressões de Validação

Esta seção tem o objetivo de fornecer um entendimento das expressões de validação para ajudar a explicar o uso. Todas estão incluídas no código validator.js.

### Nome de usuário

```php
this.setHandler('username', value => {
      const regex = new RegExp('[<|>|"|\'|%|;|(|)|&]', 'i');
      return !regex.test(value);
    });
```

Aqui, a expressão procura a ocorrência de qualquer um dos caracteres alternativos dentro da classe de caracteres [] e retorna falso (inválido) se algum for encontrado.

### Senha

```pnp
this.setHandler('password', value => {
      const regex = /^\S[\S ]{2,98}\S$/;
      return regex.test(value);
    });
```

Aqui a expressão está procurando um caractere inicial que não seja um espaço em branco seguido por 2 a 98 caracteres que não sejam espaço em branco ou um espaço e terminando com um caractere que não seja espaço em branco. O Joomla possui um verificador de senha mais sofisticado que garante uma mistura de tipos de caracteres e comprimento mínimo.

### Email

```php
this.setHandler('email', value => {
      const newValue = punycode.toASCII(value);
      const regex = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
      return regex.test(newValue);
    }); // Attach all forms with a class 'form-validate'
```

Aqui a expressão regular aceita qualquer string que comece com um ou mais dos caracteres na classe de caracteres [], seguida por @, seguida por qualquer um ou mais caracteres na segunda classe de caracteres [], seguida por zero ou mais pontos finais seguidos por um ou mais caracteres no terceiro grupo da classe de caracteres, e terminando com um dos grupos após o @.

Embora tecnicamente correto, este validador permite alguns endereços de e-mail bastante bobos, como !@me - você pode querer criar um padrão mais restritivo.

### Numérico

```php
this.setHandler('numeric', value => {
      const regex = /^(\d|-)?(\d|,)*\.?\d*$/;
      return regex.test(value);
    });
```

Aqui, o valor deve começar com um dígito ou sinal de menos zero ou uma vez, ser seguido por um dígito ou uma vírgula zero ou mais vezes, seguido por um ponto final zero ou uma vez, terminando com um dígito zero ou mais vezes. Assim, isso corresponderá a 3.142 ou -10 ou 100.000,0 e assim por diante. O número real precisa corresponder ao tipo de campo no banco de dados. A expressão não corresponderá a números com expoentes, então 10e2 será rejeitado.

### Padrão

Suponha que você queira que um campo de entrada seja um número inteiro positivo. Você poderia usar seu próprio padrão colocado no arquivo xml do formulário:

```xml
        <field
            name="number"
            type="text"
            label="COM_MYCOMPONENT_CAMP_NUMBER_LABEL"
            description="COM_MYCOMPONENT_CAMP_NUMBER_DESC"
            class="w-auto"
            required="true"
            pattern="\d+"
        />
```

Aqui o padrão permite um ou mais dígitos. Portanto, -1 seria inválido, assim como 3.142.

### Obrigatório

Certifique-se de incluir required="true" na especificação do campo, ou a etiqueta do formulário omitirá o marcador * comumente usado para indicar campos obrigatórios. Incluir required na classe não é suficiente para validação.

*Traduzido por openai.com*

