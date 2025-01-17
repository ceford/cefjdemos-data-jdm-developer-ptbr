<!-- Filename: J4.x:Http_Header_Management / Display title: Cabeçalhos HTTP -->

## Introdução

Joomla 4 introduziu um sistema de Cabeçalhos HTTP projetado para ajudar os proprietários de sites a configurarem Cabeçalhos de Segurança HTTP. Isso é implementado usando um plugin *System - HTTP Headers*. Há um tutorial abrangente para [Usuários](jdocmanual?article=user/security/http-headers) sobre este tópico. Este tutorial é mais curto e cobre alguns pontos relevantes para desenvolvedores.

## O Sistema - Plugin de Cabeçalhos HTTP

### Guia de plugin

Navegue até **Sistema → Plugins → Sistema - HTTP Headers** para acessar o formulário de configuração do plugin.

![System http headers plugin form](../../../en/images/security/security-http-headers-plugin.png)

- **X-Frame Options** Este é ativado por padrão, mas a [documentação](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) diz que está obsoleto e uma política de *frame-ancestors* deve ser usada em seu lugar.
- **Referrer-Policy** O padrão é *strict-origin-when-cross-origin*.
- **Cross-Origin-Opener-Policy** O valor padrão do Joomla é `same-origin`.
- **Force HTTP Headers** Não há cabeçalhos forçados definidos por padrão. É aqui que uma alternativa ao *X-Frame Options* pode ser especificada. O valor de `Content-Security-Policy` pode ser um dos seguintes:
    - `'none'`
    - `'self' https://www.example.org;`
    - `'self' https://example.org https://example.com https://store.example.com;`

Usando o subformulário **Force HTTP Headers**, você também pode forçar os seguintes cabeçalhos:

- [Content-Security-Policy](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Content-Security-Policy-Report-Only](https://scotthelme.co.uk/content-security-policy-an-introduction/#testingapolicy)
- [Cross-Origin-Opener-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy)
- [Expect-CT](https://scotthelme.co.uk/a-new-security-header-expect-ct/)
- [Feature-Policy & Permissions-Policy](https://scotthelme.co.uk/a-new-security-header-feature-policy/)
- [NEL (Registro de Resposta de Rede)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/NEL)
- [Referrer-Policy](https://scotthelme.co.uk/a-new-security-header-referrer-policy/)
- [Report-to](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
- [Strict-Transport-Security](https://scotthelme.co.uk/hsts-the-missing-link-in-tls/)
- [X-Frame-Options](https://scotthelme.co.uk/hardening-your-http-response-headers/#x-frame-options)

### Aba Strict-Transport-Security (HSTS)

![strict transport security settings](../../../en/images/security/security-http-headers-hsts.png)

Use o botão *Alternar Ajuda Inline* para obter informações sobre cada parâmetro. Referência ilustrada:

[Formulário para verificar ou definir o status e a elegibilidade de pré-carregamento HSTS](https://hstspreload.org/)

### Guia de Política de Segurança de Conteúdo (CSP)

![Content security policy options](../../../en/images/security/security-http-headers-csp.png)

Uma vez habilitado, você pode definir o cliente onde deseja impor a CSP configurada, permitindo que você ajuste `site`, `administrador` ou `ambos`. Uma CSP deve ser aplicada tanto no frontend quanto no backend. Referências ilustradas:

- [Política de Segurança de Conteúdo](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [Nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce)
- [Hashes de Script e Estilo](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)
- Descrições de src de [Diretiva de Política](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/child-src) estão disponíveis no menu desta página.

A opção final chamada *Add Directive* permite que você configure a lista de permissões por diretiva conforme necessário. Por exemplo, para a diretiva *script-src*, o campo *Value* deve conter as origens das quais você deseja permitir o carregamento de scripts.

## Notas

Quando você configurou alguns cabeçalhos de segurança HTTP diretamente no servidor, pode haver entradas duplicadas.

Verifique a saída dos seus Cabeçalhos HTTP no console do navegador. No Google Chrome ou Firefox: Inspecionar → Rede → a saída em Cabeçalhos. Você pode então desativar os cabeçalhos que causam entradas duplas. Verifique também o console do seu navegador para possíveis erros.

## Desenvolvedores de Extensão

A grande vantagem de uma Política de Segurança de Conteúdo ocorre quando o Cabeçalho bloqueia todo o JavaScript embutido e CSS embutido, afetando manipuladores de eventos JavaScript via atributos HTML. Com a proteção do navegador ativada, JavaScript embutido e CSS embutido são bloqueados para extensões também. Essa proteção não é ativada por padrão, mas pode ser habilitada pelos usuários.

Onde JavaScript e CSS em linha são necessários, suporte a nonce e hash estão incluídos nas APIs de Documento. Quando usados, o núcleo garantirá que eles estejam na lista de permissões, mas ainda bloqueará código malicioso.

### Notas importantes para desenvolvedores de extensões

A partir do Joomla 4.0, a Política de Segurança de Conteúdo:

- é enviado com o núcleo.
- está desativado por padrão.
- pode ser habilitado por administradores do site.
- é fortemente recomendado que o frontend e backend de sua extensão funcionem com a Política de Segurança de Conteúdo habilitada.

Com a Política de Segurança de Conteúdo estrita ativada, os seguintes recursos serão bloqueados:

- A execução de JavaScript através dos manipuladores de eventos HTML (manipuladores onXXX como onClick e similares).
- A execução de JavaScript na página não passado para a página através da API do Documento.
- A execução de código JavaScript injetado em APIs DOM, como eval().
- O uso de CSS inline na página não passado para a página através da API do Documento.
- O uso de CSS inline usando o atributo de estilo HTML.

Para fazer suas extensões funcionarem mesmo com uma Política de Segurança de Conteúdo rigorosa habilitada, a maneira mais fácil é usar a API do Documento para aplicar seu JavaScript e CSS inline. Veja os exemplos abaixo.

### Adicionando JavaScript usando a API do Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add JavaScript from URL
    $wa->registerAndUseScript('com_example.sample', 'https://example.org/sample.js', [], ['defer' => true]);

    // Add inline JavaScript
    $wa->addInlineScript('
        document.addEventListener("DOMContentLoaded", function(event) {
            alert("An inline JavaScript Declaration");
        });
    ');
```

### Adicionando CSS usando a API do Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add Style from URL
    $wa->registerAndUseStyle('com_example.sample', 'https://example.org/sample.css');

    // Add inline Style
    $wa->addInlineStyle('
        body {
            background: #00ff00;
            color: rgb(0,0,255);
        }
    ');
```

## Recursos adicionais

- [CSP Cheat Sheet](https://scotthelme.co.uk/csp-cheat-sheet/)
- [Content Security Policy - Uma Introdução](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Security Headers](https://securityheaders.com/) é parte do Probely e foi criado originalmente por Scott Helme! É uma ferramenta gratuita e fácil de usar, projetada para ajudar você a implantar e entender melhor os recursos modernos de segurança disponíveis para seu site.
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [Web Fundamentals Content Security Policy](https://developers.google.com/web/fundamentals/security/csp)
- [Documentação CSP do Google](https://csp.withgoogle.com/docs/index.html)
- [CSP Is Dead, Long Live CSP!](https://research.google/pubs/pub45542/) Sobre a Insegurança de Listas Brancas e o Futuro da Política de Segurança de Conteúdo
- [web.dev busca por Segurança](https://web.dev/s/results?q=security#gsc.tab=0&gsc.q=security&gsc.sort=)

*Traduzido por openai.com*

