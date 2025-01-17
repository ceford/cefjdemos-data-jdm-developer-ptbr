<!-- Filename: J4.x:Developer:_Required_Software / Display title: Instalar Joomla -->

## Resumo

Aqui está um breve resumo dos passos envolvidos:

- **Baixe** a versão completa mais recente da página [Joomla Downloads](https://downloads.joomla.org/).
- **Salve** o arquivo baixado no diretório raiz do seu Documento ou em uma subpasta do raiz.
- **Descompacte** o arquivo baixado no local. Alguns sistemas operacionais criarão uma pasta com o mesmo nome do arquivo zip baixado, menos o zip. Isso é bom - você pode apenas renomear a pasta com um nome curto e movê-la se necessário. Outros sistemas operacionais descompactarão os arquivos e pastas no local. Isso é ruim se você colocou o download em sua pasta raiz onde há pastas existentes com outros sites. Você precisará criar um nome de pasta curto e mover todos os arquivos e pastas recém-extraídos para ela. O objetivo é acabar com uma pasta que você possa acessar com seu navegador da web via localhost/j4test.
- **Instale** apontando seu navegador para localhost/j4test e preenchendo os formulários de instalação do Joomla.

## Configuração

Algumas sugestões:

- **Configuração Global / Site / Cookie / Caminho do Cookie** configurado para a pasta que contém a instalação do Joomla (/j4test).
- **Configuração Global / Sistema / Depuração / Sistema de Depuração** configurado para Sim.
- **Configuração Global / Sistema / Tempo de Vida da Sessão** configurado para 60 - caso contrário, será desconectado enquanto pensa.
- **Configuração Global / Servidor / Servidor / Relatórios de Erros** configurado para Máximo.
- **Plugin do Sistema - Depuração / Plugin / Atualizar Recursos** configurado para Não, a menos que você esteja depurando css ou javascript.

É isso aí. Se o Joomla estiver funcionando, você está pronto para usá-lo no desenvolvimento de extensões.

## Mais Sites

Você pode instalar quantos sites quiser em um único computador. Dependendo de como você configurou seu servidor, isso pode ser feito por diferentes subpastas acessadas através de localhost/test1, localhost/test2 e assim por diante, ou por hosts virtuais acessados por test1.localhost, test2.localhost e assim por diante. De qualquer forma, você pode ter um novo banco de dados e um novo site Joomla limpo em poucos minutos. Escolha nomes curtos e significativos! *test1* e *test2* logo se tornarão confusos.

## Instalação para Testes

Há um procedimento diferente se você quiser ajudar com o teste do Joomla. Nesse caso, você deve instalar o **git** e seguir as instruções no [Repositório do GitHub](https://github.com/joomla/joomla-cms).

- no GitHub, selecione o branch do Joomla para trabalhar. Isso altera as instruções observadas mais abaixo na página.
- Em seu laptop ou desktop, siga as instruções a partir de *Clone o repositório:* em diante.
- Não exclua o diretório de Instalação ao final da instalação do Joomla!
- Renomeie o clone de joomla-cms para outro nome, assim você poderá fazer tudo novamente mais tarde.
- Instale o Joomla Patchtester.

*Traduzido por openai.com*

