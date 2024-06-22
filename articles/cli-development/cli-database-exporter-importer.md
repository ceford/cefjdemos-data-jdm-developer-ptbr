<!-- Filename: J4.x:CLI_Database_Exporter_Importer / Display title: Exportador e importador de banco de dados de interface de linha de comando (CLI) -->

Joomla!  4.0

## Sobre

Antes de atualizar o Joomla! ou [instalar uma extensão de
terceiros](https://docs.joomla.org/J4.x:CLI_Update "Special:MyLanguage/J4.x:CLI Update"),
é altamente recomendável que você faça uma cópia de segurança de seu
site.  
O console do Joomla! 4.x fornece comandos para exportar (cópia de
segurança) e importar (restaurar) sua base de dados do Joomla!. Observe
que ele não faz cópia de segurança do seu sistema de arquivos, o que
deve ser feito separadamente

## Requisitos

Para usar esses comandos, você precisa de um acesso seguro ao shell
(ssh) de seu provedor de hospedagem (host) no qual a interface de linha
de comando (cli) do php está instalada. Considere ter conhecimento
básico do uso de comandos de shell.

## Instruções

Inicie a sessão em seu provedor de hospedagem (host) e vá para a pasta
raiz do seu site.  
Eu recomendo usar a pasta "tmp" do Joomla em seu para ter permissões de
leitura/gravação.

- Listar todos os comandos disponíveis do console do Joomla:  
  `php cli/joomla.php list`
- Exportar o banco de dados para a pasta:  
  `php cli/joomla.php database:export --all --folder `
- Importar o banco de dados da pasta:  
  `php cli/joomla.php database:import --all --folder `

Você também pode:

- Exporte o banco de dados como um arquivo .zip:  
  `php cli/joomla.php database:export --all --zip`
- Exportar uma tabela:  
  `php cli/joomla.php database:export --table `
- Exportar uma tabela como um arquivo .zip:  
  `php cli/joomla.php database:export --table --zip`
- Importar uma tabela:  
  `php cli/joomla.php database:import --table `
- Se precisar de ajuda:  
  `php cli/joomla.php database:export --help`
  `php cli/joomla.php database:import --help`

## Cópia de segurança e restauração

Para fazer uma cópia de segurança completa (com pastas, arquivos e banco
de dados) do seu site, você pode executar estes comandos:

1.  Arquivar seu diretório raiz do Joomla:  
    `tar --exclude='./tmp/joomla_bak.*' -zcvf tmp/joomla_bak.tgz . > tmp/joomla_bak.log`
2.  Exportar todo o banco de dados do Joomla:  
    `php cli/joomla.php database:export --all --folder tmp/db_bak`

E para restaurar, execute estes comandos:

1.  Importar todo o banco de dados Joomla:  
    `php cli/joomla.php database:import --all --folder tmp/db_bak`
2.  Extrair o arquivo:  
    `tar --recursive-unlink -xvf tmp/joomla_bak.tgz .`
