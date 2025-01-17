<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Adicionando um Changelog -->

Desde o Joomla 4.0, os desenvolvedores de extensões podem aproveitar a capacidade do Joomla de ler um arquivo de changelog e fornecer uma representação visual do changelog. Se uma versão específica não for encontrada no changelog, o botão do changelog não será exibido.

As mudanças em uma versão serão apresentadas da seguinte maneira:

![changelog modal view](../../../en/images/developer-information/adding-changelog-example-1.png)

O changelog é usado em dois lugares diferentes.

## Atualizar Visualização

O instalador mostrará o changelog da versão que pode ser instalada se disponível.

![changelog installer view](../../../en/images/developer-information/adding-changelog-update-view.png)

Clicar no botão Changelog aqui mostrará o registro de alterações da nova versão disponível.

## Gerenciar Visualização

O gerenciador de extensões exibirá o log de alterações da extensão atualmente instalada, se disponível.

![changelog installer view](../../../en/images/developer-information/adding-changelog-extension-view.png)

Clicar no número da versão aqui mostrará o changelog da versão atualmente instalada.

## Adicionar a tag changelogurl aos arquivos de manifesto

O primeiro passo é atualizar seus arquivos de manifesto que informam ao Joomla onde encontrar os detalhes do changelog. Adicione o seguinte nó aos seus arquivos XML de manifesto:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Por favor, observe: A URL na tag changelogurl não deve ter espaços ou quebras de linha antes ou depois dela. Veja exemplos de código.

### Atualizar manifesto do servidor

Veja este exemplo de arquivo de manifesto do servidor de atualização que informa o Joomla sobre uma atualização de um componente chamado "com_lists". Assim, você verá o botão de Registro de Alterações na visualização de atualizações.

```
<?xml version="1.0" encoding="utf-8"?>
<updates>
 <update>
  <name>Student List</name>
  <description>List of students</description>
  <element>com_lists</element>
  <type>component</type>
  <version>4.0.0</version>

  <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

  <tags>
   <tag>stable</tag>
  </tags>
  <maintainer>Example Miller</maintainer>
  <maintainerurl>https://example.com/</maintainerurl>
  <section>Updates</section>
  <targetplatform name="joomla" version="4.?" />
  <client>1</client>
  <folder></folder>
 </update>
</updates>
```

### Manifesto de extensão

Além disso, adicione a tag changelogurl ao manifesto de extensão XML. Assim, a versão da extensão será vinculada aos registros de alterações na vista de gerenciamento.

```
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>COM_LISTS</name>

... Other stuff ...

    <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

    <updateservers>
        <server type="extension" name="My Extension's Updates">https://example.com/lists-updates.xml</server>
    </updateservers>
</extension>
```

## Criar arquivo de registro de alterações

O arquivo de changelog deve conter os seguintes 3 nós:

* elemento
* tipo
* versão

Essas informações são usadas para identificar o changelog correto para uma determinada extensão.

Um nó de versão dentro de qualquer nó de changelog é sempre obrigatório. Caso contrário, você verá uma mensagem de erro como SyntaxError: JSON.parse: unexpected character at line 1 column 1 of the JSON data.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

Além disso, o changelog é preenchido com um ou mais tipos de mudanças. Os seguintes tipos de mudanças são suportados:

* segurança: Quaisquer problemas de segurança que foram corrigidos
* correção: Quaisquer erros que foram corrigidos
* idioma: Isto é para alterações de idioma
* adição: Quaisquer novos recursos adicionados
* mudança: Quaisquer alterações
* remoção: Quaisquer recursos removidos
* nota: Qualquer informação extra para informar o usuário

Cada nó pode ser repetido quantas vezes for necessário.

O formato do texto pode ser texto simples ou HTML, mas no caso de HTML, deve ser delimitado por tags CDATA conforme mostrado no exemplo.

```
<changelogs>
    <changelog>
        <element>com_lists</element>
        <type>component</type>
        <version>4.0.0</version>
        <security>
            <item>Item A</item>
            <item><![CDATA[<h2>You MUST replace this file</h2>]]></item>
        </security>
        <fix>
            <item>Item A</item>
            <item>Item b</item>
        </fix>
        <language>
            <item>Item A</item>
            <item>Item b</item>
        </language>
        <addition>
            <item>Item A</item>
            <item>Item b</item>
        </addition>
        <change>
            <item>Item A</item>
            <item>Item b</item>
        </change>
        <remove>
            <item>Item A</item>
            <item>Item b</item>
        </remove>
        <note>
            <item>Item A</item>
            <item>Item b</item>
        </note>
</changelog>
<changelog>
    <element>com_lists</element>
    <type>component</type>
    <version>0.0.2</version>
    <security>
        <item>Big issue</item>
    </security>
</changelog>
</changelogs>
```

Este arquivo contém 2 registros de alterações:

* Versão 0.0.2 (para testar a visualização de gerenciamento)
* Versão 4.0.0 (para testar a visualização de atualização)

Um changelog pode ter quantas versões forem necessárias

*Traduzido por openai.com*

