<!-- Filename: Creating_a_Smart_Search_plug-in / Display title: Exemplo: Pesquisa Inteligente -->

## Introdução

Os plugins de Busca Inteligente estão localizados no grupo `finder` (em plugins/finder/xxxx). A codificação dos plugins de finder mudou significativamente no Joomla 4 e 5. Para criar um novo plugin de finder, provavelmente é melhor copiar um plugin central existente e modificar seu conteúdo para atender ao seu novo propósito.

Este artigo descreve um plugin de busca projetado para dar suporte à extensão *Jdocmanual*. O conteúdo a ser indexado está localizado na tabela `#__jdm_articles`. O código pode ser encontrado no [GitHub](https://github.com/ceford/cefjdemos-plg-finder-jdocmanual).

## Configuração da IDE - VSCode ou VSCodium

No meu ambiente de desenvolvimento local, o código-fonte desta extensão está localizado em ~/git/cefjdemos-plg-jdm-finder. Este nome de pasta não tem significado; é apenas a minha maneira de manter minhas próprias extensões organizadas. A estrutura esquemática da pasta é a seguinte:

```
cefjdemos-plg-finder-jdocmanual
|-- .vscode (folder for build settings)
|-- plg_finder_jdocmanual (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_finder_jdocmanual.ini
        |-- plg_finder_jdocmanual.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Jdocmanual.php (the extension class code)
    |-- jdocmanual.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

No VSCodium, parece assim:

![Plugin development file structure in vscodium](../../../en/images/plugins/jdocmanual-vscodium.png)

## Personalizar o Código

Tendo começado com um plugin de localizador principal, a primeira tarefa é mudar todas as referências do plugin original para valores adequados para o novo plugin. Neste caso, há 63 instâncias da palavra `jdocmanual` em 7 arquivos. Existe uma grande chance de que algumas sejam perdidas na primeira passagem e o plugin não funcione.

## Construir a Extensão

Existem duas partes na construção. Eu tenho um arquivo build.xml que contém instruções para o [`phing`](https://www.phing.info/), uma ferramenta de construção para projetos PHP. Ele pode ser chamado a partir do VSCode/VSCodium ou Eclipse ou qualquer outro IDE.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="jdocmanual" basedir="." default="main">

    <property name="joomladir" value="/Users/ceford/public_html/jdm3"  override="true" />
    <property name="sitecomdir" value="${joomladir}/components" override="true" />
    <property name="admincomdir" value="${joomladir}/administrator/components" override="true" />
    <property name="adminmediadir" value="${joomladir}/media/com_jdocmanual" override="true" />
    <property name="plgdir" value="${joomladir}/plugins/finder/jdocmanual" override="true" />
    <property name="srcdir" value="${project.basedir}" override="true" />

    <property name="zipsdir" value="/Users/ceford/git/zips/cefjdemos"  override="true" />

    <!-- Fileset for plugin files -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- fileset for zip -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">

        <copy todir="${plgdir}">
            <fileset refid="plgfiles" />
        </copy>

        <zip destfile="${zipsdir}/plg_finder_jdocmanual.zip">
            <fileset refid="plgfiles" />
        </zip>

    </target>
</project>
```

A segunda parte é um arquivo `tasks.json` na pasta .vscode. Ele informa ao VSCode onde encontrar o arquivo phing.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
      {
        "label": "Build plg_finder_jdocmanual",
        "type": "shell",
        "command": "php ~/bin/phing-latest.phar",
        "windows": {
          "command": "php ~/bin/phing-latest.phar"
        },
        "group": "build",
        "presentation": {
          "reveal": "always",
          "panel": "shared"
        }
      }
    ]
}
```

No VSCode/VScodium, eu seleciono **Terminal / Executar Tarefa de Construção...** na barra de menu e escolho a tarefa apropriada.

## Instalar a Extensão

Após a construção, o arquivo zip da extensão deve ser instalado da mesma forma que qualquer outra extensão. Ele precisa ser ativado e todas as opções precisam ser configuradas. O Jdocmanual possui três opções de *Taxonomias para Indexar*:

- **Manual** um ou todos os Manuais disponíveis (Usuário, Desenvolvedor, ...)
- **Idioma** as pesquisas são restritas a artigos no mesmo idioma que o idioma da página.
- **Tipo** um ou todos os tipos de conteúdo (Jdocmanual, Conteúdo, ...)

No caso do site Jdocmanual, outros plugins de busca estão desativados porque não há outro conteúdo a ser indexado.

Após a primeira instalação, normalmente é suficiente executar a tarefa de build novamente após a correção do código. A instalação do arquivo zip é necessária apenas se houver mudanças no arquivo manifest.xml.

## Indexar o Conteúdo

Jdocmanual está configurado para indexar seus artigos quando solicitado por um Super Usuário. Isso é diferente do plugin de Conteúdo normal, que indexa o conteúdo sempre que um artigo é salvo. A primeira execução leva alguns minutos para indexar os ~5000 artigos do Jdocmanual em 8 idiomas.

## Criar um Módulo de Busca Inteligente

A partir de **Módulos de Conteúdo / Site** selecione **Novo** e instale um novo módulo de **Pesquisa Inteligente**. Ajuste as configurações para obter uma aparência adequada.

## Testando

Eventualmente, seu plugin de busca inteligente personalizado deve funcionar. Esta é uma página de resultados de exemplo para o Jdocmanual que pesquisa um termo nesta página. A página de resultados omite o formulário de pesquisa da barra de título porque ele está presente no corpo da página.

![Smart search result](../../../en/images/plugins/jdocmanual-search-result.png)

Um parêntese: o plugin System - Joomla Accessibility Checker mostra que há 3 erros relacionados ao formulário de entrada de dados *Termos de Busca*. Isso precisa de uma correção no núcleo ou substituição.

*Traduzido por openai.com*

