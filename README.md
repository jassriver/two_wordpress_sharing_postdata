# two_wordpress_sharing_postdata
O que fazer pra dois wordpress compartilharem o mesmo postdata?

(Depois eu atualizo aqui e deixo mais organizado)
A ideia é: configure dois wordpress normalmente, após isso, você vai nas configurações (wp-config.php) do site que você quer que use o postdata do outro e aponte a conexão dele pro banco de dados do site que você quer espelhar.

Depois, você vai criar uma tabela no banco de dados do site que você quer espelhar chamado (wp_)options2 (o wp_ aqui é o prefixo que você escolheu, ok?), nesse options2 você vai clonar TODO O CONTEÚDO do wp_options original e vai modificar com as informações do site que vai receber o postdata, como nome, tema e url e etc.

Após isso, no site que vai receber o post data, você vai precisar abrir o arquivo wp-db.php que se encontra na pasta wp-includes
Localize os códigos que falam sobre tabelas customizadas

algo do gênero: (isso pode mudar no futuro)
```
if ( isset( $tables['users'] ) && defined( 'CUSTOM_USER_TABLE' ) ) {
  $tables['users'] = CUSTOM_USER_TABLE;
}
```

a ideia é que antes dessa linha falando sobre a tabela custom de usuários você insira um código que modifique a tabela de opções do wordpress que vai receber o postdata
use o código a baixo e insira antes do trecho de código citado anterior:
```
if ( isset( $tables['options'] ) && defined( 'RIVER_ESPELHO_OPTIONS_TABLE' ) ) {
  $tables['options'] = RIVER_ESPELHO_OPTIONS_TABLE;
}
```

salve e feche o documento;

Após isso, volte na wp_config.php do wordpress que vai receber o postdata e abaixo do WP_DEBUG adicione o seguinte trecho de código:
```
/* Chama a tabela de configuração options2 */
define( 'RIVER_ESPELHO_OPTIONS_TABLE', 'wp_options2');
```

salve e feche;

Feito isso, se você seguiu todos os passos corretamente e o wordpress não mudou a sua estrutura, tudo deverá funcionar normalmente.
OBS: é importante ressaltar aqui que, todas as vezes que o wordpress atualizar, ele provavelmente irá atualizar a wp-db.php, então você terá que repetir o processo.

Caso descubra um jeito de modificar o núcleo do wp sem sofrer alterações pelas atualizações do wp, por favor, contribua aqui;

(Futuramente irei traduzir o material aqui contido para inglês)
