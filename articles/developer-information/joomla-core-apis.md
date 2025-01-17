<!-- Filename: J4.x:Joomla_Core_APIs / Display title: APIs do Núcleo do Joomla -->

Esta página lista os endpoints disponíveis no Joomla por meio de exemplos de comandos curl. Foi preparada para o Joomla 4 e requer verificação de conformidade com as versões atuais do Joomla.

Cada URL requer autenticação, a menos que seja designada como uma URL pública. Para
segurança no Joomla 4.0.0 planejamos fazer com que a Aplicação Api padrão
requeira uma conta de Super Usuário (já que a Aplicação API é completamente nova), isso
pode ser relaxado à medida que a API se estabiliza e é bem testada na
comunidade. Se você estiver usando o plugin de autenticação básica (atualmente
o único plugin fornecido a partir do Joomla 4 alpha 10) é necessário
adicionar aos comandos curl abaixo usando --user user_name:password

Cada URL precisa ser precedido pelo host do Joomla antes do caminho (por exemplo, em vez de `/api/index.php/v1/article`, você precisa de `http://example.com/api/index.php/v1/article`)

Os nomes das propriedades entre chaves ({}) indicam que a propriedade é uma variável que deve ser substituída.

A menos que indicado de outra forma, essas APIs foram introduzidas no Joomla 4. Para mais informações sobre a Especificação da API do Joomla (em oposição a esta lista de URLs e opções), por favor, visite a [Especificação da API do Joomla](https://docs.joomla.org/Joomla_Api_Specification).

Onde os pontos finais exigem o valor de {app}, a variável atualmente assume dois valores: site (front end) ou administrador (back end).

## Recursos Úteis

Um número de recursos complementares foi criado para apresentar os Serviços Web do Joomla e ajudá-lo a aprender como implementar os Serviços Web em seu projeto.

- Coleção Postman de chamadas [Joomla Web Services API](https://github.com/alexandreelise/j4x-api-collection) por Alexandre Elise.
- Tutorial do Youtube Joomla 4: [Using the Web Services API](https://www.youtube.com/watch?v=lT9qodsvfZg) com Eoin Oliver.
- Revista da Comunidade Joomla: [Joomla Web Services API 101](https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test) por Patrick Jackson.

## Componente de Banners

### Banners

#### Obter Lista de Banners

Não traduzir blocos de código.

#### Obter Banner Único

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Excluir Banner

curl -X DELETE /api/index.php/v1/banners/{banner_id}

#### Criar Banner

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d
```

```json
{
    "catid": 3,
    "clicks": 0,
    "custombannercode": "",
    "description": "Texto",
    "metakey": "",
    "name": "Nome",
    "params": {
        "alt": "",
        "height": "",
        "imageurl": "",
        "width": ""
    }
}
```

#### Atualizar Banner

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/{banner_id} -d

```json
{
    "alias": "nome",
    "catid": 3,
    "description": "Novo Texto",
    "name": "Novo Nome"
}
```

### Clientes

#### Obter Lista de Clientes

curl -X GET /api/index.php/v1/banners/clients

#### Obter Cliente Único

curl -X GET /api/index.php/v1/banners/clients/{client_id}

#### Excluir Cliente

curl -X DELETE /api/index.php/v1/banners/clients/{client_id}

#### Criar Cliente

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/clients -d
```

```json
{
    "contact": "Nome",
    "email": "email@mail.com",
    "extrainfo": "",
    "metakey": "",
    "name": "Clientes",
    "state": 1
}
```

#### Atualizar Cliente

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/clients/{client_id} -d
```

```json
{
    "contact": "novo Nome",
    "email": "newemail@mail.com",
    "name": "Clientes"
}
```

### Categorias de Banners

#### Obter Lista de Categorias

`curl -X GET /api/index.php/v1/banners/categories`

#### Obter Categoria Única

curl -X GET /api/index.php/v1/banners/categories/{category_id}

#### Excluir Categoria

curl -X DELETE /api/index.php/v1/banners/categories/{category_id}

#### Criar Categoria

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/categories -d
```

```json
{
    "acesso": 1,
    "alias": "cat",
    "extensao": "com_banners",
    "idioma": "*",
    "nota": "",
    "id_pai": 1,
    "publicado": 1,
    "titulo": "Título"
}
```

#### Atualizar Categoria

```shell
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/banners/categorias/{category_id} -d
```

```json
{
    "alias": "gato",
    "note": "Algum Texto",
    "parent_id": 1,
    "title": "Novo Título"
}
```

### Histórico de Conteúdo dos Banners

#### Obter Lista de Históricos de Conteúdo

curl -X GET /api/index.php/v1/banners/contenthistory/{banner_id}

#### Alternar Manter Histórico de Conteúdo

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/contenthistory/keep/{contenthistory_id}

#### Excluir Histórico de Conteúdo

```
curl -X DELETE
/api/index.php/v1/banners/contenthistory/{contenthistory_id}
```

## Componente de Configuração

### Aplicativo

#### Obter Lista de Configurações de Aplicativos

```plaintext
curl -X GET /api/index.php/v1/config/application
```

#### Atualizar Configuração do Aplicativo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d
```

```json
{
    "debug": true,
    "sitename": "123"
}
```

### Componente

#### Obter Lista de Configurações de Componentes

curl -X GET /api/index.php/v1/config/{nome_do_componente}

Exemplo “component_name” é “com_content”.

#### Atualizar Configuração do Componente de Aplicativo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d
```

```json
{
    "link_titles": 1
}
```

## Componente de Contato

### Contato

#### Obter Lista de Contatos

```markdown
curl -X GET /api/index.php/v1/contact
```

#### Obter Contato Único

curl -X GET /api/index.php/v1/contact/{contact_id}

#### Excluir Contato

```
curl -X DELETE /api/index.php/v1/contact/{contact_id}
```

#### Criar Contato

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact -d
```

```json
{
    "alias": "contact",
    "catid": 4,
    "language": "*",
    "name": "Contato"
}
```

#### Atualizar Contato

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d
```

```json
{
    "alias": "contato",
    "catid": 4,
    "name": "Novo Contato"
}
```

#### Enviar Formulário de Contato

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/form/{contact_id} -d
```

```json
{
    "contact_email": "email@mail.com",
    "contact_message": "algum texto",
    "contact_name": "nome",
    "contact_subject": "assunto"
}
```

### Categorias de Contato

1. A rota Categorias de Contato é: "v1/contact/categories"
2. Trabalhar com isso é similar às Categorias de Banners.

### Campos de Contato

#### Obter Lista de Campos de Contato

```
curl -X GET /api/index.php/v1/fields/contact/contact
```

#### Obter Contato de Campo Único

curl -X GET /api/index.php/v1/fields/contact/contact/{field_id}

#### Excluir Campo de Contato

curl -X DELETE /api/index.php/v1/fields/contact/contact/{field_id}

#### Criar Contato de Campo

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact -d
```

```json
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "campo de contato",
    "language": "*",
    "name": "campo-de-contato",
    "note": "",
    "params": {
        "class": "",
        "display": "2",
        "display_readonly": "2",
        "hint": "",
        "label_class": "",
        "label_render_class": "",
        "layout": "",
        "prefix": "",
        "render_class": "",
        "show_on": "",
        "showlabel": "1",
        "suffix": ""
    },
    "required": 0,
    "state": 1,
    "title": "campo de contato",
    "type": "text"
}
```

#### Atualizar Campo de Contato

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact/{field_id} -d
```

```json
{
    "title": "novo campo de contato",
    "name": "campo-de-contato",
    "label": "campo de contato",
    "default_value": "",
    "type": "texto",
    "note": "",
    "description": "Algum Texto Novo"
}
```

### Campos Contato Correio

1. O Campo de Rota do Contato Mail é: "v1/fields/contact/mail"
2. Trabalhar com isso é semelhante a Campos de Contato.

### Categorias de Contato de Campos

1. Os Campos de Rota das Categorias de Contato são: "v1/fields/contact/categories"
2. Trabalhar com isso é semelhante a Campos de Contato.

### Grupos Campos Contato

#### Obter Lista de Campos de Grupos de Contato

curl -X GET /api/index.php/v1/fields/groups/contact/contact

#### Obter Campos de Contato de Grupo Único

```
curl -X GET /api/index.php/v1/fields/groups/contact/contact/{group_id}
```

#### Excluir Campos de Contato do Grupo

```markdown
curl -X DELETE
/api/index.php/v1/fields/groups/contact/contact/{group_id}
```

#### Criar Campos de Contato do Grupo

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact -d
```

```json
{
    "accesso": 1,
    "contexto": "com_contact.contact",
    "valor_padrao": "",
    "descrição": "",
    "grupo_id": 0,
    "rótulo": "campo de contato",
    "idioma": "*",
    "nome": "campo-de-contato3",
    "nota": "",
    "parâmetros": {
        "classe": "",
        "exibição": "2",
        "exibição_lei_somente": "2",
        "dica": "",
        "classe_rótulo": "",
        "classe_render_rótulo": "",
        "layout": "",
        "prefixo": "",
        "classe_render": "",
        "mostrar_em": "",
        "mostrar_rótulo": "1",
        "sufixo": ""
    },
    "obrigatório": 0,
    "estado": 1,
    "título": "campo de contato",
    "tipo": "texto"
}
```

#### Atualizar Campos de Contato do Grupo

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact/{group_id} -d

```json
{
    "title": "novo grupo de contato",
    "note": "",
    "description": "nova descrição"
}
```

### Contato de Campos do Grupo

1. Os campos do grupo de roteamento Contato Email são: "v1/fields/groups/contact/mail"
2. Trabalhar com ele é semelhante a Campos do Grupo Contato.

### Categorias de Contato dos Campos do Grupo

1. Os Campos do Grupo de Rota Categorias de Contato são: "v1/fields/groups/contact/categories"
2. Trabalhar com isso é semelhante a Campos do Grupo de Contato.

### Histórico de Conteúdo de Contato

1. O Histórico de Conteúdo da Rota é: "v1/contact/contenthistory"
2. Trabalhar com ele é similar ao Histórico de Conteúdo de Banners.

## Componente de Conteúdo

### Artigos

#### Obter Lista de Artigos

curl -X GET /api/index.php/v1/content/articles

#### Obter Artigo Único

curl -X GET /api/index.php/v1/content/articles/{article_id}

#### Excluir Artigo

`curl -X DELETE /api/index.php/v1/content/articles/{article_id}`

#### Criar Artigo

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/content/articles -d
```

```json
{
    "alias": "meu-artigo",
    "articletext": "Meu texto",
    "catid": 64,
    "language": "*",
    "metadesc": "",
    "metakey": "",
    "title": "Aqui está um artigo"
}
```

Atualmente, as opções mencionadas aqui são propriedades obrigatórias. No entanto, a intenção é, atualmente, tornar PELO MENOS metakey e metadesc opcionais na API.

#### Atualizar Artigo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/content/articles/{article_id} -d
```

```json
{
    "catid": 64,
    "title": "Artigo atualizado"
}
```

### Categorias de Conteúdo

1. As Categorias de Conteúdo da Rota são: "v1/content/categories"
2. Trabalhar com isso é semelhante a trabalhar com Categorias de Banners. Nota: se os fluxos de trabalho estiverem habilitados, então especificar um fluxo de trabalho é necessário (similarly to the UI)

### Artigos de Campos

1. Os artigos de campos de rota são: "v1/fields/content/articles"
2. Trabalhar com isso é similar ao Campos de Contato.

### Artigos dos Campos dos Grupos

1. As Rotas de Grupos de Campos Artigos são: "v1/fields/groups/content/articles"
2. Trabalhar com isso é semelhante a Grupos de Campos Contato.

### Categorias de Campos

1. As categorias dos campos de rota são: "v1/fields/groups/content/categories"
2. Trabalhar com isso é semelhante ao Campos de Contato.

### Histórico de Conteúdo do Componente de Conteúdo

1. O Histórico de Conteúdo da Rota é:
    "v1/content/articles/{article_id}/contenthistory"
2. Trabalhar com isso é semelhante ao Histórico de Conteúdo de Banners.

## Componente de Idiomas

### Idiomas

#### Obter Lista de Idiomas

`curl -X GET /api/index.php/v1/languages`

#### Instalar Idioma

```markdown
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/languages -d
```

{
    "package": "pkg_fr-FR"
}

### Idiomas de Conteúdo

#### Obter Lista de Idiomas de Conteúdo

`curl -X GET /api/index.php/v1/languages/content`

#### Obter Idioma Único do Conteúdo

curl -X GET /api/index.php/v1/v1/languages/content/{language_id}

#### Excluir Idioma do Conteúdo

curl -X DELETE /api/index.php/v1/idiomas/conteudo/{language_id}

#### Criar Idioma do Conteúdo

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/content -d
```

{
        "accesso": 1,
        "descrição": "",
        "image": "fr_FR",
        "lang_code": "fr-FR",
        "metadesc": "",
        "metakey": "",
        "ordem": 1,
        "publicado": 0,
        "sef": "fk",
        "nomesite": "",
        "título": "Francês (FR)",
        "título_nativo": "Français (France)"
    }

#### Atualizar Idioma do Conteúdo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/content/{language_id} -d
```

```json
{
    "description": "",
    "lang_code": "pt-BR",
    "metadesc": "",
    "metakey": "",
    "sitename": "",
    "title": "Português (pt-BR)",
    "title_native": "Português (Brasil)"
}
```

### Substitui Idiomas

#### Obter Lista de Constantes de Idiomas Substituídas

```
curl -X GET /api/index.php/v1/languages/overrides/{app}/{lang_code}
```

#### Obter Constante de Linguagem de Substituição Única

curl -X GET
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Excluir Substituições de Idioma de Conteúdo

```
curl -X DELETE
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}
```

#### Criar Substituições de Idioma de Conteúdo

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code} -d
```

```json
{
    "key":"new_key",
    "override": "texto"
}
```

#### Atualizar Substituições de Idioma de Conteúdo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id} -d
```

```json
{
    "key": "new_key",
    "override": "novo texto"
}
```

var app - enum {"site", "administrador"}

var lang_code - string Exemplo: “fr-FR“, “en-GB“ você pode obter lang_code
de v1/languages/content

#### Constante de Substituição de Pesquisa

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search -d
```

{
    "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
    "searchtype": "constant"
}

```markdown
var searchtype - enum {"constant", "value"}. "constant" busca por nome de constante, "value" - busca por valor de constante
```

#### Atualizar Cache de Busca de Substituição

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh
```

## Componente de Menus

### Menus

#### Obter Lista de Menus

```
curl -X GET /api/index.php/v1/menus/{app}
```

#### Obter Menu Único

curl -X GET /api/index.php/v1/menus/{app}/{menu_id}

#### Excluir Menu

```
curl -X DELETE /api/index.php/v1/menus/{app}/{menu_id}
```

#### Criar Menu

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app} -d
```

{
    "client_id": 0,
    "description": "O menu para o site",
    "menutype": "menu",
    "title": "Menu"
}

#### Atualizar Menu

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/{menu_id} -d

```json
{
    "menutype": "menu",
    "title": "Novo Menu"
}
```

### Itens de Menu

#### Obter Lista de Tipos de Itens do Cardápio

```
curl -X GET /api/index.php/v1/menus/{app}/items/types
```

#### Obter Lista de Itens do Menu

`curl -X GET /api/index.php/v1/menus/{app}/items`

#### Obter Item Único do Menu

curl -X GET /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Excluir Item do Menu

curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Criar Item de Menu

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d

```json
{
    "access": "1",
    "alias": "",
    "associations": {
        "en-GB": "",
        "fr-FR": ""
    },
    "browserNav": "0",
    "component_id": "20",
    "home": "0",
    "language": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "mainmenu",
    "note": "",
    "params": {
        "cancel_redirect_menuitem": "",
        "catid": "",
        "custom_cancel_redirect": "0",
        "enable_category": "0",
        "menu-anchor_css": "",
        "menu-anchor_title": "",
        "menu-meta_description": "",
        "menu-meta_keywords": "",
        "menu_image": "",
        "menu_image_css": "",
        "menu_show": "1",
        "menu_text": "1",
        "page_heading": "",
        "page_title": "",
        "pageclass_sfx": "",
        "redirect_menuitem": "",
        "robots": "",
        "show_page_heading": ""
    },
    "parent_id": "1",
    "publish_down": "",
    "publish_up": "",
    "published": "1",
    "template_style_id": "0",
    "title": "título",
    "toggle_modules_assigned": "1",
    "toggle_modules_published": "1",
    "type": "componente"
}
```

Exemplo para "Criar Página de Artigo"

#### Atualizar Item do Menu

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items/{menu_item_id} -d
```

```json
{
    "component_id": "20",
    "language": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "menuprincipal",
    "note": "",
    "title": "novo título",
    "type": "componente"
}
```

Exemplo para "Criar Página de Artigo"

## Componente de Mensagens

### Mensagens

#### Obter Lista de Mensagens

curl -X GET /api/index.php/v1/messages

#### Obter Mensagem Única

curl -X GET /api/index.php/v1/mensagens/{message_id}

#### Excluir Mensagem

curl -X DELETE /api/index.php/v1/mensagens/{message_id}

#### Criar Mensagem

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/messages -d
```

{
    "mensagem": "texto",
    "estado": 0,
    "assunto": "texto",
    "user_id_from": 773,
    "user_id_to": 772
}

#### Atualizar Mensagem

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/messages/{message_id} -d
```

```json
{
    "message": "novo texto",
    "subject": "novo texto",
    "user_id_from": 773,
    "user_id_to": 772
}
```

## Componente de Módulos

### Módulos

#### Obter Lista de Tipos de Módulos

curl -X GET /api/index.php/v1/módulos/tipos/{app}

#### Obter Lista de Módulos

curl -X GET /api/index.php/v1/modules/{app}

#### Obter Módulo Único

```
curl -X GET /api/index.php/v1/modules/{app}/{module_id}
```

#### Excluir Módulo

```
curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}
```

#### Criar Módulo

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/modules/{app} -d

```json
{
    "access": "1",
    "assigned": [
        "101",
        "105"
    ],
    "assignment": "0",
    "client_id": "0",
    "language": "*",
    "module": "mod_articles_archive",
    "note": "",
    "ordering": "1",
    "params": {
        "bootstrap_size": "0",
        "cache": "1",
        "cache_time": "900",
        "cachemode": "static",
        "count": "10",
        "header_class": "",
        "header_tag": "h3",
        "layout": "_:default",
        "module_tag": "div",
        "moduleclass_sfx": "",
        "style": "0"
    },
    "position": "",
    "publish_down": "",
    "publish_up": "",
    "published": "1",
    "showtitle": "1",
    "title": "Título"
}
```

Exemplo para "Artigos - Arquivados"

#### Atualizar Módulo

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d
```

```json
{
    "accesso": "1",
    "client_id": "0",
    "idioma": "*",
    "módulo": "mod_articles_archive",
    "nota": "",
    "ordem": "1",
    "título": "Novo Título"
}
```

Exemplo para "Artigos - Arquivados"

## Componente de Feeds de Notícias

### Feeds

#### Obter Lista de Feeds

`curl -X GET /api/index.php/v1/newsfeeds/feeds`

#### Obter Feed Único

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Excluir Feed

curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Criar Feed

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/newsfeeds/feeds -d
```

```json
{
    "acesso": 1,
    "apelido": "alias",
    "catid": 5,
    "descrição": "",
    "imagens": {
        "float_first": "",
        "float_second": "",
        "image_first": "",
        "image_first_alt": "",
        "image_first_caption": "",
        "image_second": "",
        "image_second_alt": "",
        "image_second_caption": ""
    },
    "idioma": "*",
    "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metadados": {
        "hits": "",
        "rights": "",
        "robots": "",
        "tags": {
            "tags": "",
            "typeAlias": null
        }
    },
    "metadesc": "",
    "metakey": "",
    "nome": "Name",
    "ordem": 1,
    "parâmetros": {
        "feed_character_count": "",
        "feed_display_order": "",
        "newsfeed_layout": "",
        "show_feed_description": "",
        "show_feed_image": "",
        "show_item_description": ""
    },
    "publicado": 1
}
```

#### Atualizar Feed

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/newsfeeds/feeds/{feed_id} -d
```

```json
{
    "acesso": 1,
    "alias": "teste2",
    "catid": 5,
    "descrição": "",
    "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metadesc": "",
    "metakey": "",
    "nome": "Teste"
}
```

### Categorias de Notícias

1. A rota de categorias de feeds de notícias é: "v1/newsfeeds/categories"
2. Trabalhar com isso é semelhante às categorias de banners.

## Componente de Privacidade

### Solicitação

#### Obter Lista de Solicitações

curl -X GET /api/index.php/v1/privacy/request

#### Obter Solicitação Única

curl -X GET /api/index.php/v1/privacy/request/{request_id}

#### Obter Dados de Exportação de Solicitação Única

```
curl -X GET /api/index.php/v1/privacy/request/export/{request_id}
```

#### Criar Solicitação

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/privacy/request -d
```

```json
{
    "email":"somenewemail@com.ua",
    "request_type":"exportar"
}
```

### Consentimento

#### Obter Lista de Consentimentos

curl -X GET /api/index.php/v1/privacidade/consentimento

#### Obter Consentimento Único

curl -X GET /api/index.php/v1/privacy/consent/{consent_id}

#### Excluir Consentimento

```
curl -X DELETE /api/index.php/v1/privacy/consent/{consent_id}
```

## Componente de Redirecionamento

### Redirecionar

#### Obter Lista de Redirecionamentos

curl -X GET /api/index.php/v1/redirecionar

#### Obter Redirecionamento Único

curl -X GET /api/index.php/v1/redirect/{redirect_id}

#### Excluir Redirecionamento

`curl -X DELETE /api/index.php/v1/redirect/{redirect_id}`

#### Criar Redirecionamento

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/redirect -d

{
        "comentário": "",
        "cabeçalho": 301,
        "acessos": 0,
        "nova_url": "/content/art/99",
        "antiga_url": "/content/art/12",
        "publicado": 1,
        "referente": ""
    }

#### Atualizar Redirecionamento

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d
```

{
        "new_url": "/content/art/4",
        "old_url": "/content/art/132"
    }

## Componente de Tags

### Tags

#### Obter Lista de Tags

```
curl -X GET /api/index.php/v1/tags
```

#### Obter Única Tag

curl -X GET /api/index.php/v1/tags/{tag_id}

#### Excluir Tag

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Criar Tag

```markdown
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags
-d
```

```json
{
    "access": 1,
    "access_title": "Público",
    "alias": "teste",
    "description": "",
    "language": "*",
    "note": "",
    "parent_id": 1,
    "path": "teste",
    "published": 1,
    "title": "teste"
}
```

#### Atualizar Tag

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/tags/{tag_id} -d
```

{
    "alias": "teste",
    "title": "novo título"
}

## Modelos

### Estilos de Modelos

#### Obter Lista de Estilos de Modelos

```
curl -X GET /api/index.php/v1/templates/styles/{app}
```

#### Obter Estilo de Modelo Único

curl -X GET /api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Excluir Estilo de Modelo

```
curl -X DELETE
/api/index.php/v1/templates/styles/{app}/{template_style_id}
```

#### Criar Estilo de Modelo

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app} -d
```

```json
{
    "home": "0",
    "params": {
        "fluidContainer": "0",
        "logoFile": "",
        "sidebarLeftWidth": "3",
        "sidebarRightWidth": "3"
    },
    "template": "cassiopeia",
    "title": "cassiopeia - Algum Texto"
}
```

#### Atualizar Estilo do Modelo

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app}/{template_style_id} -d
```

```json
{
    "template": "cassiopeia",
    "title": "novo cassiopeia - Padrão"
}
```

## Componente de Usuários

### Usuários

#### Obter Lista de Usuários

curl -X GET /api/index.php/v1/usuarios

#### Obter Usuário Único

```
curl -X GET /api/index.php/v1/users/{user_id}
```

#### Excluir Usuário

curl -X DELETE /api/index.php/v1/users/{user_id}

#### Criar Usuário

```markdown
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/users
-d
```

```json
{
    "block": "0",
    "email": "test@mail.com",
    "groups": [
        "2"
    ],
    "id": "0",
    "lastResetTime": "",
    "lastvisitDate": "",
    "name": "nnn",
    "params": {
        "admin_language": "",
        "admin_style": "",
        "editor": "",
        "helpsite": "",
        "language": "",
        "timezone": ""
    },
    "password": "qwertyqwerty123",
    "password2": "qwertyqwerty123",
    "registerDate": "",
    "requireReset": "0",
    "resetCount": "0",
    "sendEmail": "0",
    "username": "ad"
}
```

#### Atualizar Usuário

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/users/{user_id} -d
```

{
    "email": "novo@mail.com",
    "groups": [
        "2"
    ],
    "name": "nome",
    "username": "nome de usuário"
}

### Campos Usuários

1. A rota Fields Users é: "v1/fields/users"
2. Trabalhar com ela é similar a trabalhar com Fields Contact.

### Grupos Campos Usuários

1.  A rota Groups Fields Users é: "v1/fields/groups/users"
2.  Trabalhar com isso é similar a Groups Fields Contact.

*Traduzido por openai.com*

