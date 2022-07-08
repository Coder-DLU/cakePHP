# Tìm hiểu cake php
- Tạo mới một project: composer create-project --prefer-dist cakephp/app:~4.0 my_app_name
- Tạo mới một controller: bin\cake bake controller posts
- Tạo mới một model: bin\cake bake model posts  
- Tạo một teamplates: bin\cake bake template posts 
- Chạy serve: bin/cake server
- Sữa lại file app_local.php:
``Docker file

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


``
