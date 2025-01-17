<!-- Filename: Deploying_an_Update_Server / Display title: Atualizar servidores -->

## Antecedentes

Há uma boa descrição dos Servidores de Atualização na Documentação de Programadores do Joomla! copiada para [este site](jdocmanual?article=docus/install-update/update-server) ou disponível na [fonte original](https://manual.joomla.org/docs/building-extensions/install-update/update-server/).

## Solução de Problemas

- **Script de atualização SQL não é executado durante a atualização.**

Se o script de atualização SQL (por exemplo, na pasta `sql/updates/mysql`) não for executado durante o processo de atualização, pode ser porque não há um número de versão na tabela `#_schemas` para esta extensão *antes da atualização*. Esse valor é determinado pelo nome do último script na pasta de atualizações SQL. Se esse valor estiver em branco, nenhum script SQL será executado durante esse ciclo de atualização. Para garantir que esse valor seja definido corretamente, certifique-se de ter um script SQL nesta pasta com seu nome como número de versão (por exemplo, 1.2.3.sql se a versão for 1.2.3). O arquivo pode estar vazio ou apenas ter uma linha de comentário SQL. Isso deve ser feito na versão antiga — aquela antes da atualização. Alternativamente, você pode adicionar esse valor ao `#_schemas` usando uma consulta SQL.

*Traduzido por openai.com*

