# Tìm hiểu cake php
- Tạo mới một project: composer create-project --prefer-dist cakephp/app:~4.0 my_app_name
-   Tạo bản posts trong database
- Tạo mới một controller: bin\cake bake controller posts
- Tạo mới một model: bin\cake bake model posts  
- Tạo một teamplates: bin\cake bake template posts 
- Chạy serve: bin/cake server
- Sữa lại file app_local.php:
```Dockerfile
    'Datasources' => [
        'default' => [
            'host' => 'localhost',
            'username' => 'root',
            'password' => '',

            'database' => 'mycode',

            'url' => env('DATABASE_URL', null),
        ],

        /*
         * The test connection is used during the test suite.
         */
        'test' => [
            'host' => 'localhost',
            //'port' => 'non_standard_port_number',
            'username' => 'my_app',
            'password' => 'secret',
            'database' => 'test_myapp',
            //'schema' => 'myapp',
            'url' => env('DATABASE_TEST_URL', 'sqlite://127.0.0.1/tests.sqlite'),
        ],
    ],
```
-    sữa lại app.php:
```Dockerfile
  'default' => [
            'className' => Connection::class,
            'driver' => Mysql::class,
            'persistent' => false,
            'timezone' => 'UTC',

            /*
             * For MariaDB/MySQL the internal default changed from utf8 to utf8mb4, aka full utf-8 support, in CakePHP 3.6
             */
            //'encoding' => 'utf8mb4',
            'host' => 'localhost',
            'username' => 'root',
            'password' => '',

            'database' => 'mycode',
            /*
             * If your MySQL server is configured with `skip-character-set-client-handshake`
             * then you MUST use the `flags` config to set your charset encoding.
             * For e.g. `'flags' => [\PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES utf8mb4']`
             */
            'flags' => [],
            'cacheMetadata' => true,
            'log' => false,

            /*
             * Set identifier quoting to true if you are using reserved words or
             * special characters in your table or column names. Enabling this
             * setting will result in queries built using the Query Builder having
             * identifiers quoted when creating SQL. It should be noted that this
             * decreases performance because each query needs to be traversed and
             * manipulated before being executed.
             */
            'quoteIdentifiers' => false,

            /*
             * During development, if using MySQL < 5.6, uncommenting the
             * following line could boost the speed at which schema metadata is
             * fetched from the database. It can also be set directly with the
             * mysql configuration directive 'innodb_stats_on_metadata = 0'
             * which is the recommended value in production environments
             */
            //'init' => ['SET GLOBAL innodb_stats_on_metadata = 0'],
        ],
```
-    Tạo trang login: composer require "cakephp/authentication:^2.0"
-   Tạo bản users trong database
-   thêm mật khẩu băm : bin/cake bake all users
- Tạo mới một controller: bin\cake bake controller users
- Tạo mới một model: bin\cake bake model users  
- Tạo một teamplates: bin\cake bake template users
- Chạy serve: bin/cake server
-   Vào user.php 
```Dockerfile
<?php
declare(strict_types=1);

namespace App\Model\Entity;

// use Authentication\PasswordHasher\DefaultPasswordHasher; // Add this line
use Cake\Auth\DefaultPasswordHasher as AuthDefaultPasswordHasher;
use Cake\ORM\Entity;


/**
 * User Entity
 *
 * @property int $id
 * @property string $username
 * @property string $email
 * @property string $password
 * @property \Cake\I18n\FrozenTime $created
 * @property \Cake\I18n\FrozenTime $modified
 */
class User extends Entity
{
    /**
     * Fields that can be mass assigned using newEntity() or patchEntity().
     *
     * Note that when '*' is set to true, this allows all unspecified fields to
     * be mass assigned. For security purposes, it is advised to set '*' to false
     * (or remove it), and explicitly make individual fields accessible as needed.
     *
     * @var array<string, bool>
     */
    protected $_accessible = [
        'username' => true,
        'email' => true,
        'password' => true,
        'created' => true,
        'modified' => true,
    ];

    /**
     * Fields that are excluded from JSON versions of the entity.
     *
     * @var array<string>
     */
    protected $_hidden = [
        'password',
    ];
    // protected function _setPassword(string $password) : ?string
    //{
        //if ( strlen($password) > 0 ) {
            //$hashedPW = (new DefaultPasswordHasher())->hash($password);
           // $this->Flash->success(__('Original password: {0}, hashed: {1}', $password, $hashedPW));
           // return $hashedPW;
        //}
        //$this->Flash->error('Original password is empty!');
    //}
    // Add this method
    // protected function _setPassword(string $password) : ?string
    // {
    //     if (strlen($password) > 0) {
    //         // return (new DefaultPasswordHasher())->hash($password);
    //         $hasher = new DefaultPasswordHasher();
    //         return $hasher->hash($password);
    //     }
    // }\
    protected function _setPassword($value) {
        return sha1($value);
    }
}
```
Chạy lại vào http://localhost:8765/users thêm user mới vào database check lại password đã được mã hóa khi thêm vào.
