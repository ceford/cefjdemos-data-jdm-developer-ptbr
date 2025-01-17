<!-- Filename: J4.x:Developer:_Required_Software / Display title: Configuração do Banco de Dados -->

## Sobre MySQL e MariaDB

Novatos podem ter a impressão de que MySQL e MariaDB são bancos de dados e podem ficar confusos quando as instruções dizem *criar um novo banco de dados*. Na verdade, ambos são Sistemas de Gerenciamento de Bancos de Dados Relacionais (RDBMS) que podem gerenciar muitos bancos de dados individuais. Seu site de teste local pode ter muitas instalações individuais do Joomla, cada uma com seu próprio banco de dados. Por exemplo, você pode querer testar sua extensão na versão atual do Joomla e separadamente em uma versão candidata futura.

A captura de tela a seguir mostra parte de uma lista de mais de 30 bancos de dados criados para testar várias instalações e projetos de extensões do Joomla.

![Phypadmin screenshot of list of databases](../../../en/images/getting-started/phpmyadmin-databases.png)

Uma observação: as comparações são majoritariamente utf8mb4_0900_ai_ci:

- utf8mb4 significa que cada caractere é armazenado com um máximo de 4 bytes no esquema de codificação UTF-8.
- 0900 refere-se à versão do Algoritmo de Colação Unicode.
- ai refere-se à insensibilidade a acentos, não há diferença entre e, è, é, ê e ë ao ordenar.
- ci refere-se à insensibilidade a maiúsculas e minúsculas, não há diferença entre p e P ao ordenar.

Tabelas e colunas individuais podem ter uma collation diferente. As tabelas do Joomla geralmente têm a collation utf8mb4_unicode_ci.

## Configuração do Banco de Dados com phpMyAdmin

Antes de instalar o Joomla, você precisa de um banco de dados vazio pronto para ser populado. Você também precisa de um usuário de banco de dados.

### Criar um Banco de Dados

- **Iniciar phpMyAdmin** Digite localhost/phpmyadmin na barra de URL do seu navegador.
- **Login** Dependendo de como foi instalado, o nome de usuário será root ou admin e pode ou não haver uma senha.
- Selecione **Bancos de Dados** no menu superior da página inicial do phpMyAdmin.
- Em **Criar Banco de Dados** insira um nome curto no lugar da dica **Nome do Banco de Dados**, por exemplo, jtest.
- Selecione **utf8mb4_0900_ai_ci** na lista suspensa de Interação.
- Selecione **Criar** para criar o banco de dados.

Isso o levará a um banco de dados vazio. No topo, há uma mensagem: *Nenhuma tabela encontrada no banco de dados.*

### Criar um Usuário

- Selecione o ícone **Início** no canto superior esquerdo do phpMyAdmin para ir à página inicial.
- Selecione **Contas de Usuário** no menu superior da página inicial.
- Selecione **Adicionar Conta de Usuário** no painel Novo, abaixo da lista de usuários atuais (se houver).
- Insira um **Nome de usuário**, que pode ser um nome curto que você precisará mais tarde durante a instalação do Joomla. Exemplo: jtest. Você pode usar este mesmo usuário para outros bancos de dados criados posteriormente!
- Selecione **Local** na lista do campo Nome do host. Isso definirá localhost no campo de valor adjacente.
- Use o botão **Gerar** para gerar uma senha. Você precisa copiar o valor gerado e colá-lo em um editor de texto para uso posterior. Você não precisa se lembrar dele a longo prazo, pois será armazenado como texto simples no arquivo configuration.php do Joomla. Exemplo: 8t8mGQq.gw\[\]8lp(
- **Salvar** pule as seções Banco de dados para usuário e Privilégios globais do formulário. O botão **Ir** (Salvar) está na parte inferior.

### Atribuir Permissões de Banco de Dados ao Usuário

- Na página de **Contas de Usuário**, selecione o usuário (jtest), se necessário, via Início / Contas de Usuário / Nome de Usuário.
- Selecione **Banco de Dados** perto do topo da página. Isso mostrará uma lista de bancos de dados aos quais este usuário recebeu permissões de acesso e, na parte inferior, uma lista de bancos de dados aos quais o usuário não recebeu acesso.
- **Selecione** o banco de dados ao qual o acesso deve ser concedido e selecione o botão **Ir**.
- **Privilégios específicos do banco de dados** Selecione **Marcar todos** e em seguida **Ir** para salvar.

É isso aí! Agora você pode sair do phpMyAdmin. Esqueceu de anotar a senha do usuário do banco de dados? Volte e edite a conta de usuário que você criou para alterá-la. Se você usa o mesmo usuário de banco de dados para vários bancos de dados Joomla, encontrará a senha em um arquivo configuration.php do Joomla.

*Traduzido por openai.com*

