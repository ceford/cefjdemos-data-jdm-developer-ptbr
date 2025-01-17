<!-- Filename: XAMPP / Display title: XAMPP -->

## Introdução

XAMPP é um pacote fácil de instalar que agrupa o servidor web Apache, PHP, XDEBUG e o banco de dados MySQL. Isso permite que você crie o ambiente necessário para rodar o Joomla! em sua máquina local. A versão mais recente do XAMPP está disponível no <a href="http://www.apachefriends.org/en/index.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">site do XAMPP</a>. Os downloads estão disponíveis para Linux, Windows, Mac OS X e Solaris. Baixe o pacote para a sua plataforma.

*Nota Importante Sobre o XAMPP e o Skype:* Apache e Skype ambos utilizam a porta 80 como alternativa para conexões de entrada. Se você usa Skype, vá até o painel Ferramentas-Opções-Avançado-Conexão e desmarque a opção "Usar 80 e 443 como alternativas para conexões de entrada". Se o Apache iniciar como um serviço, ele tomará a porta 80 antes do Skype iniciar e você não verá um problema. Mas, para estar seguro, desative a opção no Skype.

### Instalação no Windows

A instalação para Windows é muito simples. Você pode usar o executável do instalador XAMPP (por exemplo, "xampp-windows-x64-7.4.4-0-VC15-installer.exe"). Instruções detalhadas de instalação para Windows estão disponíveis <a href="https://www.apachefriends.org/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">aqui</a>.

Se você estiver no Windows XP ou 2003, eles não são suportados pelo pacote principal, mas existem versões compatíveis do XAMPP para essas plataformas listadas na página de download (mas você só poderá executar o PHP 5.4 ou inferior - e, portanto, só poderá testar o Joomla 3.x ou inferior).

Para Windows, é recomendável instalar o XAMPP em "c:\xampp" (e não em "c:\program files"). Se você fizer isso, seu Joomla! (e quaisquer outras pastas de sites locais) irá para a pasta "c:\xampp\htdocs". (Por convenção, todo o conteúdo web vai para a pasta "htdocs".)

Se você tiver vários servidores http (como IIS), pode alterar a porta de escuta do xampp. No \apache\conf\httpd.conf, modifique a linha Listen 80 para Listen \[portnumber\] (ex: "Listen 8080").

Tutorial da Revista da Comunidade Joomla

Você pode encontrar um tutorial detalhado sobre como instalar o XAMPP no Windows, juntamente com o Joomla 4 Beta, o Joomla Patch Tester e o Git neste <a href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git" class="external text" target="_blank" rel="noreferrer noopener">artigo da Joomla Community Magazine</a>.

### Instalação no Linux

#### Instalar XAMPP

Abra o Terminal e insira:

```bash
    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt
```

(substitua *xampp-linux-1.7.7.tar.gz* pela versão do xampp que você baixou). Foi relatado que o banco de dados MYSQL do xampp 1.7.4 não funciona com o Joomla 1.5.22

Isso instala ... Apache2, mysql e php5, bem como um servidor ftp.

```bash
    sudo /opt/lampp/lampp start
```

e

```bash
    sudo /opt/lampp/lampp stop
```

inicia/para todos os serviços

#### Teste seu servidor local XAMPP

Abra seu navegador e aponte para

```bash
    http://localhost
```

O index.php redirecionará para

```bash
    http://localhost/xampp
```

Lá você encontrará instruções sobre como alterar os nomes de usuário/senhas padrão. Em um PC que não fornece arquivos para a Internet ou LAN, então alterar os padrões é uma decisão pessoal.

#### Obter Joomla

Baixe o arquivo zip da instalação mais recente do Joomla
<a href="https://www.joomla.org/download.html"
class="external autonumber" target="_blank"
rel="noreferrer noopener">[1]</a>

Descompacte para o seu disco rígido

Conectar-se ao localhost com um cliente FTP Padrão

```
    nobody
    lampp
```

Crie uma pasta para o seu Joomla no servidor localhost

Envie por FTP os arquivos de instalação do Joomla descompactados para a pasta Joomla recém-criada.

**Importante:**

- A instalação do xammp define a Propriedade correta dos arquivos e permissões.
- Usar o **comando CHOWN** **causará problemas de Propriedade com o xampp**.
- **Usar o nautilus** para manipular pastas/arquivos no localhost **causará problemas de Propriedade com o xampp**.

**Informações do banco de dados**

- Host: localhost
- Nome padrão do Banco de Dados: test
- Usuário padrão do Banco de Dados: root
- **Não** há Senha padrão.

A senha do Administrador é de sua escolha.

É recomendada a instalação de Dados de Exemplo para o usuário iniciante.

Após a instalação, exclua o diretório de instalação e aponte seu
navegador para:

```
    http://localhost/yournewjoomlafolder
```

por

```
    http://localhost/yournewjoomlafolder/administrator
```

#### Criando um link no menu do Ubuntu

**Para criar uma GUI para xammp conectada ao seu menu Ubuntu**

Abra o Terminal e digite

```bash
    sudo gedit /usr/share/applications/xampp-control-panel.desktop
```

Em seguida, copie o seguinte para o gedit e salve.

```bash
[Desktop Entry]
Encoding=UTF-8
Name=XAMPP Control Panel
Comment=Start and Stop XAMPP
Exec=gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
Icon=/usr/share/icons/Tango/scalable/devices/network-wired.svg
Terminal=false
Type=Application
Categories=GNOME;Application;Network;
StartupNotify=true
```

Se o painel de controle não iniciar, tente executar o comando Exec
diretamente no terminal:

```bash
    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
```

Se você receber o erro:

```bash
    Error importing pygtk2 and pygtk2-libglade
```

Instale as bibliotecas ausentes:

```bash
    sudo apt-get install python-glade2
```

#### Depurador PHP XDebug

O pacote XAMPP para Linux não inclui o depurador PHP XDebug.  
Para instalar o XDebug no Debian ou Ubuntu:

- Instale o pacote *build-essential*:
```bash
    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf
```

- Baixe o
<a href="http://www.apachefriends.org/en/xampp-linux.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">pacote de desenvolvimento</a> para sua
versão do XAMPP e extraia-o sobre a sua instalação existente:
```bash
    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 
```

- Compilar XDebug:
```bash
    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize
```

Após isso, você terá o seguinte resultado no seu console…

```bash
    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 
```

Então a saída será esta... por favor, monitore o diretório especificado.

```bash
    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 
```

Crie uma pasta em sua pasta temporária que irá armazenar o arquivo de dados gerado pelo XDebug:

```bash
    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 
```

Instalações alternativas:

Instalar usando a biblioteca de extensões comunitárias do PHP (PECL) incluída no xampp:

```bash
    sudo /opt/lampp/bin/pecl install xdebug
```

No Ubuntu/Debian você pode instalar usando:

```bash
    apt-get install php5-xdebug 
```

(aviso: isso também instalará Apache e PHP dos repositórios apt).

**Aviso para usuários de 64 bits**

Ao compilar o XDebug ou instalar via apt-get, você receberá um
erro ao (re)iniciar o xampp:

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64
```

Isso ocorre porque o xampp é executado em 32 bits, mas o XDebug é de 64 bits. Para superar esse problema, ou compile xdebug.so em uma máquina de 32 bits ou baixe-o de:

```bash
    http://code.activestate.com/komodo/remotedebugging/
```

Baixe o arquivo: "PHP Remote Debugging Client" para "Linux (x86)"  
Extraia o conteúdo do arquivo em seu computador, este arquivo compactado contém várias pastas com números de versão ex: 4.4, 5.0, 5.1 ... 5.3 e assim por diante, entre na pasta com o maior número de versão ou a que funciona para você, então copie manualmente o arquivo "xdebug.so" para o seguinte local, sobrescreva se necessário

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/
```

Lembre-se de que este local pode ser diferente no seu computador.

### Instalação no Mac OS X

O Mac OS X na verdade inclui um servidor Apache pronto para uso, mas a maioria dos desenvolvedores irá preferir usar as ferramentas integradas e a configurabilidade fornecida pelo XAMPP.

Como na maioria dos programas no Mac, a instalação é fácil. Visite o <a href="https://www.apachefriends.org/en/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> para o download do binário universal.

Depois que o arquivo terminar de ser baixado, basta abrir a imagem do disco e arrastar a pasta XAMPP para o atalho da pasta "Aplicativos".

Para iniciar o servidor, abra "XAMPP Control.app" e pressione o botão de iniciar ao lado de Apache.

##### Um Pouco de Resolução de Problemas

Muitos usuários de Mac têm um pouco de dificuldade nessa etapa ao tentar configurar outra instância do Apache em sua máquina. Se você não conseguir iniciar o Apache do XAMPP, você tem duas opções:  
**Você pode alterar a porta de escuta do XAMPP.** Em \Applications\XAMPP\xamppfiles\etc\httpd.conf, modifique a linha que diz "Listen 80" para Listen \[portNumber\]. Ex.:

```bash
    Listen 8080
```

**Você pode alterar a porta de escuta do servidor Apache pré-instalado.** No Finder, vá para "/etc" (CMD+SHIFT+G); a partir daqui você poderá navegar pelos arquivos normalmente ocultos do Apache. Encontre a pasta rotulada como Apache2 e edite o arquivo "http.conf". Modifique a linha que diz, "Listen 80" para Listen \[portNumber\]. Exemplo:

```bash
    Listen 8080
```

*Nota: Se você optar por alterar a porta do servidor Apache pré-instalado, pode ser necessário reiniciar o seu computador para que as alterações entrem em vigor. Você também precisará autenticar-se como administrador para alterar esses arquivos.*

### Testar Instalação do XAMPP

Após a instalação do XAMPP e o início do serviço Apache com a ferramenta XAMPP Control Panel, você pode testá-lo abrindo seu navegador e navegando até "<a href="http://localhost" class="external free" target="_blank" rel="nofollow noreferrer noopener">http://localhost</a>". Você deverá ver a tela de boas-vindas do XAMPP, semelhante à mostrada abaixo.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/800px-Phpinfo_on_xampp.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/1200px-Phpinfo_on_xampp.png 1.5x, https://docs.joomla.org/images/f/fc/Phpinfo_on_xampp.png 2x"
data-file-width="1498" data-file-height="883" width="800" height="472"
alt="Phpinfo no xampp.png" />

Selecione o link chamado "phpinfo()" no menu superior. Isso exibirá uma longa tela de informações sobre a configuração do PHP, conforme mostrado abaixo.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

Neste ponto, o XAMPP foi instalado com sucesso. Observe o "Arquivo de Configuração Carregado". Editaremos este arquivo na próxima seção para configurar o XDebug.

*Traduzido por openai.com*

