<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: MVC Anatomia: Estrutura de Arquivos -->

## Configuração do Desenvolvedor

Existem várias maneiras de organizar arquivos para fins de desenvolvimento. Um método é usar um clone de um repositório do GitHub como na seguinte estrutura esquemática:

```
cefjdemos-com-countrybase
|-- .vscode (folder for build settings)
|-- com_countrybase (folder compressed to create an installable zip file)
    |-- admin (the administrator files)
        |-- forms
        |-- help
            |-- countrybase.html (this is a Help screen used in the backend module edit form)
        |-- language
            |-- en-GB (language folder, kept with the extension code)
                |-- mod_downmsg.ini
                |-- mod_downmsg.sys.ini
        |-- services (folder for dependency injection)
            |-- provider.php (the DI code)
        |-- sql
            |-- updates
                |-- mysql
                    |-- index.html (an empty file required to avoid an installation error if there are no sql updates)
        |-- src (folder for namespaced classes)
            |-- more folders: Controller, Extension, Helper, Model, Table and View
        |-- tmpl (folder for layouts)
            |-- more folders: countries, country, currencies and currency
        |-- access.xml (standard list of access permissions)
        |-- config.xml (options parameters)
    |-- media
        |-- css (placeholder for CSS files - contains an empty index.html file)
        |-- js (placeholder for JavaScript files - contains an empty index.html file)
    |-- site (the site files, abbreviated here)
        |-- forms
        |-- language
        |-- src
        |-- tmpl
    |-- countrybase.xml (the manifest file)
    |-- script.php (a script to run on install, update or uninstall)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Esta é a estrutura no IDE VSCodium:

![Vscodium file structure view](../../../en/images/mvc-anatomy/com-countrybase-vscodium.png)

Na instalação, as partes do componente `com_countrybase` são distribuídas em diferentes locais na estrutura de arquivos do Joomla:
- Os arquivos do administrador vão para `root/administrator/components/com_countrybase`.
- Os arquivos do site vão para `root/components/com_countrybase`.
- Arquivos de mídia, javascript e css (se houver), vão para `root/media/com_countrybase`.
- Arquivos de idioma vão para as estruturas de componentes do administrador e do site. Uma convenção anterior os colocava nas pastas de idiomas do núcleo.

Onde os itens são colocados é controlado pelo arquivo de manifesto do componente, neste caso countrybase.xml.

Observe que o arquivo zip contém tudo dentro da pasta com_countrybase. Você pode criar um zip apenas comprimindo essa pasta. Fora dessa pasta estão o build.xml, que é um arquivo usado para construir o componente sempre que uma alteração é feita, e README.md, que é um arquivo padrão Markdown no formato do Github que descreve o componente.

## Notas

- Existem mais de 40 arquivos neste componente simples!
- Na instalação, o arquivo countrybase.xml é copiado para administrador/componentes/com_countrybase, onde é necessário para fins de atualização ou desinstalação.

*Traduzido por openai.com*

