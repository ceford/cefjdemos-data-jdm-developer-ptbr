<!-- Filename: J4.x:Developer:_Required_Software / Display title: Software Necessário -->

## Introdução

Este tutorial é direcionado a iniciantes no desenvolvimento de código Joomla que desejam preparar extensões em um computador local. Não importa se você usa Windows, Mac ou Linux. Todo o software necessário está disponível para todas essas plataformas. Para começar no seu próprio laptop ou desktop, que geralmente tem o nome de domínio *localhost*, você precisará instalar um conjunto padrão de itens de software separados.

- **Apache**, um servidor web. Outros servidores web estão disponíveis, mas os novatos devem ficar com o Apache.
- **MySQL** ou **MariaDB**, servidores de banco de dados. PostgreSQL também é suportado, mas não é para novatos.
- **PHP**, a versão mais recente recomendada pelo Joomla ou a versão mínima para sua plataforma.
- **phpMyAdmin** uma ferramenta usada para gerenciar bancos de dados MySQL e MariaDB.
- **xDebug** uma extensão ao PHP usada para depuração.
- Um IDE como o **VSCode**. Outros estão disponíveis e são abordados em um artigo separado.

## Pilhas de Software

Os primeiros quatro itens desta lista são frequentemente referidos como uma pilha e podem ser nomeados como LAMP, MAMP ou WAMP, onde as letras no acrônimo significam o seguinte:

- **L, M ou W** plataforma. L para Linux, M para Mac e W para Windows.
- **A: Apache** servidor web.
- **M: MySQL ou MariaDB** banco de dados. Os dois são intercambiáveis.
- **P: PHP** linguagem de script. Uma linguagem de script amplamente utilizada. Não há alternativa, pois o Joomla é codificado em PHP.

## Pilhas Empacotadas

Uma boa maneira de começar é usando um pacote que combina o software essencial:

- **WAMP** para Windows é gratuito no site [Wampserver](https://www.wampserver.com/en/).
- **Bearsampp** para Windows é gratuito no site [Bearsampp](https://bearsampp.com/). Possui mais ferramentas.
- **XAMPP** para Windows, Mac e Linux é gratuito no site [Apache Friends](https://www.apachefriends.org/). Há um tutorial local para [XAMPP](jdocmanual?article=user/hosting/local-hosting-with-xampp).
- **MAMP** para Mac e Windows está disponível em versões gratuita e comercial no site [MAMP](https://www.mamp.info/en/mac/).

## Sem Pilhas

Se você tem um computador Linux ou Macintosh, verá que pode instalar cada um dos softwares necessários de forma independente a partir dos repositórios remotos que suportam seu sistema operacional. É possível que eles já estejam instalados e prontos para uso. Para testar, abra seu navegador web favorito e digite **localhost** na barra de URL. Você verá uma página de espaço reservado ou uma página de erro de conexão.

## Diretório Raiz de Documentos do Servidor Web

Na instalação, seu servidor Apache terá definido um diretório raiz de documentos padrão. A localização varia de plataforma para plataforma e você precisará saber onde ela está ou criar um host virtual para colocá-la onde desejar. Locais padrão de exemplo:

- Mac OS: "/Library/WebServer/Documents"
- Linux: /var/www/html
- Windows: ...

Para evitar problemas posteriores com permissões de arquivos, muitas vezes é conveniente criar um host virtual para apontar para o diretório public_html do seu próprio espaço de arquivos. Isso pode ser /home/username/public_html no Linux ou /Users/username/Sites no Mac.

Este é um exemplo de entrada de host virtual Mac no arquivo /etc/apache2/vhosts/localhost.conf:

```bash
<VirtualHost *:80>
        DocumentRoot "/Users/username/Sites"
        ServerName localhost
        ErrorLog "/private/var/log/apache2/localhost-error_log"
        CustomLog "/private/var/log/apache2/localhost-access_log" common
        <Directory "/Users/username/Sites">
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

Alternativamente, você pode criar um link simbólico do diretório raiz padrão do documento para a pasta public_html do seu espaço de arquivos pessoal.

*Traduzido por openai.com*

