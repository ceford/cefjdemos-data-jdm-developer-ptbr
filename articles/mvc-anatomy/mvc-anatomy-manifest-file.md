<!-- Filename: J4.x:MVC_Anatomy:_Manifest_File / Display title: Anatomia do MVC: Arquivo Manifesto -->

## Metadados

O arquivo de manifesto do componente deve ser nomeado como manifest.xml ou componentname.xml, neste caso countrybase.xml. Note que a parte com_ não está incluída. A primeira parte do arquivo contém metadados:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>com_countrybase</name>
    <author>Clifford E Ford</author>
    <authorEmail>john.doe@example.com</authorEmail>
    <authorUrl>example.com</authorUrl>
    <creationDate>2025-01-02</creationDate>
    <copyright>(C) 2025 Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later; see LICENSE.txt</license>
    <version>0.0.1</version>
    <description>COM_COUNTRYBASE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Component\Countrybase</namespace>
```

A maioria dos metadados é autoexplicativa. Itens a serem observados:

- O tipo de extensão neste caso é **componente**. Existem outros tipos de extensões, como módulo ou plugin.
- O método pode ser instalação ou atualização. Este último permite a reinstalação após a atualização do código.
- O número da versão é importante! Deve impossibilitar a reinstalação de uma versão mais antiga. E é usado para controlar se atualizações no banco de dados são necessárias.
- O caminho do namespace src tem três partes:
  - A primeira parte é um prefixo definido pelo fornecedor, então Joomla para código fornecido pelo Joomla, Acme para código fornecido pelo fornecedor de extensões de terceiros Acme e, neste caso, Cefjdemos, um nome escolhido para demonstrações de código Joomla.
  - A segunda parte indica o tipo de extensão. Pode ser Componente ou Módulo ou Plugin ou ...
  - A terceira parte é o nome específico da extensão, neste caso Countrybase.

## Banco de Dados

O arquivo de manifesto define os locais de quaisquer arquivos sql de instalação, atualização ou remoção. Eles acabam em uma pasta sql dentro da pasta do administrador.

```xml
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.sql</file>
        </sql>
    </uninstall>
    <update>
        <schemas>
            <schemapath type="mysql">sql/updates/mysql</schemapath>
        </schemas>
    </update>
```

## Arquivo de Script

Um arquivo de script pode ser usado para finalidades de pré-instalação ou pós-instalação. Por exemplo, ele pode ser usado para verificar os requisitos mínimos do sistema antes da instalação ou para definir parâmetros após a instalação.

## Arquivos de Mídia

O componente com_countrybase não necessita de css ou javascript específicos do componente, mas o código para instalá-los está incluído por precaução. As pastas css e js contêm apenas arquivos index.html vazios. Sem esses arquivos, as pastas não são criadas.

```xml
    <scriptfile>script.php</scriptfile>

    <media destination="com_countrybase" folder="media">
        <file>joomla.asset.json</file>
        <folder>css</folder>
        <folder>js</folder>
    </media>
```

Observe que o arquivo joomla.asset.json é usado pelo gerenciador de ativos da web, geralmente chamado a partir de um arquivo de modelo.

## Arquivos do Site

Estes são os arquivos que são copiados para root/components/com_countrybase. Note que a pasta de idioma é copiada para a pasta do componente e não para a pasta root/language:

```xml
    <files folder="site">
        <folder>language</folder>
        <folder>forms</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
```

## Arquivos do Administrador

Há mais arquivos na parte de administração do arquivo manifest porque há mais a fazer:

```xml
    <administration>
        <files folder="admin">
            <filename>access.xml</filename>
            <filename>config.xml</filename>
            <folder>forms</folder>
            <folder>help</folder>
            <folder>language</folder>
            <folder>layouts</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <menu img="class:default">countrybase</menu>
        <submenu>
            <!--
                Note that all & must be escaped to &amp; for the file to be valid
                XML and be parsed by the installer
            -->
            <menu
                link="option=com_countrybase&amp;view=countries"
                img="default"
            >
                COM_COUNTRYBASE_COUNTRIES
            </menu>
            <menu
                link="option=com_countrybase&amp;view=currencies"
                img="default"
            >
                COM_COUNTRYBASE_CURRENCIES
            </menu>
        </submenu>
    </administration>
```

Observe o método para criar os itens de menu do Administrador do Joomla. Há um item de menu que não possui um link. E dois itens de menu que levam às duas visualizações de administração disponíveis: a lista de Países e a lista de Moedas. Além disso, as traduções das chaves de string devem estar no arquivo com_countrybase.sys.ini.

## Registro de Alterações

O changelog é usado para manter um registro das alterações em cada versão da extensão. Consulte o artigo [Changelogs](jdocmanual?article=docus/install-update/installation-change-log) para obter informações sobre o conteúdo do changelog.

```xml
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/changelog.xml</changelogurl>
```

## Atualizar Servidor

O código do servidor de atualização informa ao Joomla onde encontrar as informações de atualização. Ele é executado em intervalos regulares para verificar se há uma atualização disponível. Deixe esta seção de fora se você não estiver usando um servidor de atualização para o seu próprio componente.

```xml
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" name="Countrybase Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/updates.xml</server>
    </updateservers>
```

*Traduzido por openai.com*

