<!-- Filename: J4.x:Moving_Joomla_To_Prepared_Statements / Display title: Movendo o Joomla para instruções preparadas -->

Joomla!  <span class="small">≥ </span>4.0 version

No Joomla 4, você verá a mudança do Joomla lentamente para o uso de
instruções preparadas para todas as nossas consultas. Este artigo tem
como objetivo definir por que e como estamos fazendo isso.

## Motivação

O PHP 5.3 introduziu o conceito de instruções preparadas. No Joomla 4
vamos começar a tirar vantagem delas. A principal vantagem das
instruções preparadas é reduzir a exposição de uma base de código a
ataques de injeção de SQL enviando a consulta e os dados separadamente.
Você pode imaginar o PHP enviando a consulta como o seguinte

    Prepared Statements
    Query: SELECT foobar FROM bar WHERE foo = ?
    Data:  [? = 'bar']

E o próprio banco de dados fará o trabalho difícil de citar sua
instrução. Como o banco de dados é responsável pela cotação, reduz a
complexidade da base de código do Joomla e também reduz a chance de
erros de codificação introduzindo quaisquer falhas.

## Implementando instruções preparadas por meio do JDatabaseDriver

A implementação de instruções preparadas no Joomla é muito simples e
multiplataforma. Por exemplo, aqui está uma consulta simples do plugin
de autenticação do Joomla
<img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.x" />.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . '=' . $this->db->quote($credentials['username']));

Para convertê-la em uma instrução preparada, substituiremos o nome de
usuário por um valor nomeado e, em seguida, vincularemos nossos dados
reais à consulta.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $credentials['username']);

Como você pode ver, ao adicionar outra linha à consulta, não precisamos
mais citar os dados - deixamos nosso banco de dados mysql/postgresql
lidar com isso para nós, facilitando muito a leitura e o gerenciamento
do código.

Algumas funções no JDatabaseDriver usarão instruções preparadas
automaticamente. Por exemplo <a
href="https://github.com/joomla-framework/database/blob/2.0-dev/src/DatabaseQuery.php#L1701"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">whereIn()</a> e <a
href="https://github.com/joomla-framework/database/blob/2.0-dev/src/DatabaseQuery.php#L1724"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">whereNotIn()</a> usará
automaticamente os valores e adicionará instruções preparadas à
consulta.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id, password')))
        ->from($this->db->quoteName('#__users'))
        ->whereIn($this->db->quoteName('id'), [ 1, 2, 3 ]);

This query will be converted to a sql prepared statement.

    SELECT 
      `id`, `password`
    FROM
      `#__users`
    WHERE
      `id` IN (
        :preparedArray1,
        :preparedArray2,
        :preparedArray3
      );

Os espaços reservados :preparedArray1-3 serão preenchidos com 1,2,3 na
execução.

As funções a seguir aceitam arranjos para reduzir a sobrecarga de
chamadas de função.

- bind()
- bindArray()
- whereIn()
- whereNotIn()

Se possível, você deve usar instruções preparadas.

## Leitura adicional sobre instruções preparadas

- [Selecting data using
  JDatabase](https://docs.joomla.org/J4.x:Selecting_data_using_JDatabase "Special:MyLanguage/J4.x:Selecting data using JDatabase")
- <a href="https://php.net/manual/en/pdo.prepared-statements.php"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">O manual do PHP sobre instruções
  preparadas</a>
- <a href="https://github.com/joomla/joomla-cms/pull/25049/files"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Solicitação de puxada (pull) simples
  implementando uma instrução preparada no Joomla</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">API de
  estrutura do Joomla</a>
