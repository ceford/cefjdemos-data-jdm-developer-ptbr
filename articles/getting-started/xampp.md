<!-- Filename: XAMPP / Display title: XAMPP -->

## Introdução

O XAMPP é um pacote fácil de instalar que inclui o servidor web Apache,
o PHP, o XDEBUG e o banco de dados MySQL. Isso permite que você crie o
ambiente que você precisa para executar o Joomla! em sua máquina local.
A versão mais recente do XAMPP está disponível no
<a href="https://www.apachefriends.org/pt_br/index.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">site do XAMPP</a>. Os downloads estão
disponíveis para Linux, Windows, Mac OS X e Solaris. Baixe o pacote para
sua plataforma.

*Nota importante sobre o XAMPP e o Skype:* O Apache e o Skype usam a
porta 80 como alternativa para conexões de entrada. Se você usa o Skype,
vá para o painel Ferramentas-Opções-Avançado-Conexão e desmarque a opção
"Usar 80 e 443 como alternativas para conexões de entrada". Se o Apache
iniciar como um serviço, ele usará a porta 80 antes que o Skype seja
iniciado e você não verá nenhum problema. Mas, por segurança, desative a
opção no Skype.

### Instalação no Windows

A instalação para o Windows é muito simples. Você pode usar o executável
do instalador do XAMPP (por exemplo,
"xampp-windows-x64-7.4.4-0-VC15-installer.exe"). Instruções detalhadas
de instalação para o Windows estão disponíveis
<a href="https://www.apachefriends.org/pt_br/download.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">aqui</a>.

Se você estiver no Windows XP ou 2003, eles não são suportados pelo
pacote principal, mas existem versões compatíveis do XAMPP para essas
plataformas listadas na página de downloads (mas você só poderá executar
o PHP versão 5.4 ou inferior e, portanto, só será capaz de testar o
Joomla versão 3.x ou inferior).

Para o Windows, é recomendado instalar o XAMPP em "c:\xampp" (não em
"c:\arquivos de programas"). Se você fizer isso, seu Joomla! (e
quaisquer outras pastas de sites locais) irão para a pasta
"c:\xampp\htdocs". (Por convenção, todo o conteúdo web vai para a pasta
"htdocs".)

Se você tiver vários servidores http (como o IIS), poderá alterar a
porta de escuta do xampp. Em \apache\conf\httpd.conf, modifique a linha
Listen 80 para Listen \[número da porta\] (ex: "Listen 8080").

Tutorial da revista da comunidade Joomla

Você pode encontrar um tutorial detalhado sobre como instalar o XAMPP no
Windows, junto com o Joomla 4 Beta, o Testador de correções Joomla e o
Git neste <a
href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git"
class="external text" target="_blank" rel="noreferrer noopener">artigo
da revista da comunidade Joomla</a>.

### Instalação no Linux

#### Instalar o XAMPP

Abra o Terminal e digite:

    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt

(substitua *xampp-linux-1.7.7.tar.gz* pela versão do xammp que você
baixou). Foi relatado que o banco de dados MYSQL do xampp 1.7.4 não
funciona com o Joomla 1.5.22

Isso instala ... o Apache2, o mysql e o php5, bem como um servidor ftp.

    sudo /opt/lampp/lampp start

e

    sudo /opt/lampp/lampp stop

inicia/para todos os serviços

#### Teste seu servidor XAMPP local

Abra seu navegador e aponte para

    http://localhost

O index.php irá redirecionar para

    http://localhost/xampp

Lá você encontrará instruções sobre como alterar nomes de
usuários/senhas padrão. Em um computador pessoal que não fornece
arquivos para a Internet ou para a rede de área local, alterar os
padrões é uma decisão pessoal.

#### Baixe o Joomla

Baixe o último zip de instalação do Joomla
<a href="https://www.joomla.org/download.html"
class="external autonumber" target="_blank"
rel="noreferrer noopener">[1]</a>

Descompacte no seu disco rígido

Conecte-se ao localhost com um cliente FTP padrão

    nobody
    lampp

Crie uma pasta para o seu Joomla no servidor localhost

Transfira, via FTP, os arquivos de instalação do Joomla descompactados
para a pasta Joomla recém-criada.

**Importante:**

- A instalação do xammp define a propriedade correta dos arquivos e as
  permissões.
- Usar o **comando chown** irá **causar problemas de propriedades com o
  xampp**.
- **Usar o nautilus** para manipular pastas/arquivos no host local irá
  **causar problemas de propriedades com o xampp**.

**Informações do banco de dados**

Host

    localhost

Nome do banco de dados padrão

    test

Usuário padrão do banco de dados

    root

    Não existe uma senha padrão.

A senha do administrador é sua escolha.

A instalação dos dados de amostra é recomendada para o usuário
iniciante.

Após a instalação, exclua o diretório de instalação e aponte seu
navegador para:

    http://localhost/yournewjoomlafolder

ou

    http://localhost/yournewjoomlafolder/administrator

#### Criando um link no menu do Ubuntu

**Para criar uma interface gráfica do usuário (GUI) para o xampp
conectado ao seu menu do Ubuntu**

Abra o Terminal e digite

    sudo gedit /usr/share/applications/xampp-control-panel.desktop

Em seguida, copie e salve o seguinte no gedit:

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

Se o painel de controle não iniciar, tente executar o comando Exec
diretamente no terminal:

    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"

Se você receber o erro:

    Error importing pygtk2 and pygtk2-libglade

Instale as bibliotecas que faltam:

    sudo apt-get install python-glade2

#### Depurador PHP XDebug

O pacote XAMPP para Linux não inclui o depurador PHP XDebug. Para
instalar o XDebug no Debian ou no Ubuntu:

\- Instale o pacote *build-essential*:

    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf

\- Baixe o <a href="http://www.apachefriends.org/en/xampp-linux.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">pacote de desenvolvimento</a> para
sua versão do XAMPP e extraia-o sobre sua instalação existente:

    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 

\- Construa o XDebug:

    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize

Depois disso, você terá a seguinte saída no seu console…

    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 

Então a saída será esta... por favor, monitore o diretório especificado.

    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 

Crie, em sua pasta temporária, uma pasta que conterá o arquivo de dados
gerado pelo XDebug:

    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 

Instalações alternativas:

Instale usando a biblioteca comunitária de extensões PHP (PECL)
empacotada com o xampp:

    sudo /opt/lampp/bin/pecl install xdebug

No Ubuntu/Debian você pode instalar usando:

    apt-get install php5-xdebug 

(aviso: isso também instalará o Apache e o PHP dos repositórios apt).

**Aviso para os usuários de 64 bits**

Ao compilar o XDebug ou instalar via apt-get, você receberá um erro ao
(re)iniciar o xampp:

    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64

Isso ocorre porque o xampp é executado em 32 bits, mas o XDebug é em 64
bits. Para superar esse problema, faça o xdebug.so em uma máquina de 32
bits ou baixe-o de:

    http://code.activestate.com/komodo/remotedebugging/

Baixe o arquivo: "Cliente de depuração remota do PHP" para "Linux (x86)"
Extraia o conteúdo do arquivo em seu computador, este arquivo compactado
contém várias pastas com números de versão ex: 4.4, 5.0, 5.1 ... 5.3 e
assim por diante, entre na pasta com o número de versão superior ou
aquela que funciona para você , copie manualmente o arquivo "xdebug.so"
para o seguinte local, substitua se necessário

    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/

Lembre-se que este local pode ser diferente no seu computador

### Installation on Mac OS X

Mac OS X actually includes an Apache server out-of-the-box, but most
developers will prefer to use the integrated tools and configurability
provided by XAMPP.

As with most programs on Mac, installation is a breeze. Visit
<a href="https://www.apachefriends.org/en/download.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> for the
universal binary download.

Once the file has finished downloading, just open the disk image, and
drag the XAMPP folder to the "Applications" folder alias.

To start the server, open "XAMPP Control.app" and press the start button
next to Apache.

##### A Little Troubleshooting

Many Mac users have a little difficulty at this stage when trying to set
up another instance of Apache on their machine. If you cannot start
XAMPP's Apache, you have two options:  
**You can change the listening port of XAMPP.** In
\Applications\XAMPP\xamppfiles\etc\httpd.conf, modify the line that
says, "Listen 80" to Listen \[portNumber\]. E.g.:

    Listen 8080

**You can change the listening port of the pre-installed Apache
server.** In finder, go to "/etc" (CMD+SHIFT+G); from here you will be
able to navigate through the normally hidden Apache files. Find the
folder labeled Apache2, and edit the "http.conf" file. Modify the line
that says, "Listen 80" to Listen \[portNumber\]. E.g.:

    Listen 8080

*Note: If you choose to change the port of the pre-installed Apache
server, you may need to restart your computer for changes to take
effect. You will also have to authenticate as an administrator to change
these files.*

### Testar a instalação do XAMPP

Depois que o XAMPP estiver instalado e você tiver iniciado o serviço
Apache com a ferramenta do Painel de controle do XAMPP, você poderá
testá-lo abrindo seu navegador e navegando até
"<a href="http://localhost" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://localhost</a>". Você deve ver
a tela de boas-vindas do XAMPP semelhante à abaixo.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/800px-Phpinfo_on_xampp.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/1200px-Phpinfo_on_xampp.png 1.5x, https://docs.joomla.org/images/f/fc/Phpinfo_on_xampp.png 2x"
data-file-width="1498" data-file-height="883" width="800" height="472"
alt="Phpinfo on xampp.png" />

Selecione o link chamado "phpinfo()" no menu superior. Isso exibirá uma
longa tela de informações sobre a configuração do PHP, conforme mostrado
abaixo.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

Neste ponto, o XAMPP está instalado com sucesso. Observe o "Arquivo de
configuração carregado". Estaremos editando este arquivo na próxima
seção para configurar o XDebug.
