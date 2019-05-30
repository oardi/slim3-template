# Slim - Manual Setup


## Composer setup
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```


## Install Slim3
```
php composer.phar composer require slim/slim "^3.0"
```

This will create the folder "vendor" with Slim as a dependency in composer.json.


## Create index.php File
In the root folder create another folder called "public".
Inside public create the index.php file with the following content.

```php
<?php
use \Psr\Http\Message\ServerRequestInterface as Request;
use \Psr\Http\Message\ResponseInterface as Response;

$app = new \Slim\App;
$app->get('/', function (Request $request, Response $response) {
    return $response->withJson("Hello Slim3 API");
});
$app->run();
```


## .htaccess 
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ index.php [QSA,L]
```


## Serve
`php -S localhost:8000 -t ./public`


## Display API Errors
app constructor erweitern
```php
$app = new \Slim\App(
    ['settings' => [
        'displayErrorDetails' => true,
    ]]
);
```
