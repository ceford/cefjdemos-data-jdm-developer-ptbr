<!-- Filename: J4.x:Web_Assets / Display title: Ativos Web -->

## Sobre Ativos Web

Há uma descrição completa de Ativos Web na Documentação para Programadores do Joomla!, reproduzida [neste site](jdocmanual?article=docus/web-asset-manager/index) ou disponível na [fonte original](https://manual.joomla.org/docs/general-concepts/web-asset-manager/).

## Exemplos Adicionais

No Jdocmanual, muitos dos artigos contêm blocos de código que se beneficiam da realce de sintaxe. Aqui está como isso é feito:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();

// Register and attach a custom item in one run
$wa->registerAndUseStyle('highlight', 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css', [], [], []);

// Register and attach a custom item in one run
$wa->registerAndUseScript('highlight','https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js', [], [], ['core']);

// Add an inline content as usual, will be rendered in flow after all assets
$wa->addInlineScript('hljs.highlightAll();');
```

Este código está no arquivo `default.php` que é usado para exibir esta página! É um pouco complicado de implementar porque a chamada `hljs.highlightAll()` deve ser feita após os arquivos `css` e `js` terem sido carregados e novamente sempre que o conteúdo da página for alterado com uma chamada JavaScript.

Outros realçadores de sintaxe estão disponíveis!

*Traduzido por openai.com*

