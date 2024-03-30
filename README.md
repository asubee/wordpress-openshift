# WordpressをopenShift上で動かす
本リポジトリ自体は、wordpressのgitソースコードをFetchしている。編集した箇所は以下の通り。

## wp-config.php
元々のファイル名```wp-config-example.php```をリネームして、```wp-config.php```にする。

次に、データベースアクセスなどを外だし（環境変数として指定）できるように、以下記述を変更する。

```php
// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
/** The name of the database for WordPress */
//define( 'DB_NAME', 'database_name_here' );
define( 'DB_NAME', getenv("DB_NAME") );

/** Database username */
//define( 'DB_USER', 'username_here' );
define( 'DB_USER', getenv("DB_USER") );

/** Database password */
//define( 'DB_PASSWORD', 'password_here' );
define( 'DB_PASSWORD', getenv("DB_PASSWORD") );

/** Database hostname */
//define( 'DB_HOST', 'localhost' );
define( 'DB_HOST', getenv("DB_HOST") );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/** parameter */
define( 'WP_HOME', getenv("WP_HOME") );
define( 'WP_SITEURL', getenv("WP_SITEURL") );
```

OpenShiftで以下環境変数を指定することで、DBアクセス情報などを埋め込むことなく利用可能になる。

| 環境変数名(env） | 値 | 備考 |
| --- | --- | --- |
| DB_NAME | データベース名 | wordpressで使うデータベースの名前を入力する |
| DB_USER | データベースの一般ユーザ名 | データベースへアクセスするユーザ名を入力する。DBのSecretに指定している場合はSecretを指定する |
| DB_PASSWORD | ユーザのパスワード | DBのSecretを指定する。|
| DB_HOST | データベースコンテナ名 | DBのホスト名(IPアドレス)に該当する記載 |
| WP_HOME | WordpressサイトURL | routerで指定されたURLを記載する。Wordpressはドメイン名を指定する必要があるため |
| WP_SITEURL | WordpressサイトURL | routerで指定されたURLを記載する。Wordpressはドメイン名を指定する必要があるため |

