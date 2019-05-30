## slim - manuelles setup


### composer setup
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```


### slim installieren
```
php composer.phar composer require slim/slim "^3.0"
```

Dies erstellt den Ordner "vendor" (ähnlich dem Ordner "node_modules" bei Nodejs) und trägt Slim als Dependency in composer.json ein.


### public folder mit index.php erstellen
Inhalt von slim Seite kopieren

```php
<?php
use \Psr\Http\Message\ServerRequestInterface as Request;
use \Psr\Http\Message\ResponseInterface as Response;

require '../vendor/autoload.php'; //hier "../" hinzufügen, da die index.php innerhalb des public Folder besteht

$app = new \Slim\App;
$app->get('/hello/{name}', function (Request $request, Response $response, array $args) {
    $name = $args['name'];
    $response->getBody()->write("Hello, $name");
    return $response;
});
$app->run();
```


### .htaccess erstellen
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ index.php [QSA,L]
```


## Dependencies nachträglich installieren
`php composer.phar install`


## Seite aufrufen und api testen
localhost/slim-api-test/hello/some-name


### api erweitern - errors anzeigen
app constructor erweitern
```php
$app = new \Slim\App(
    ['settings' => [
        'displayErrorDetails' => true,
    ]]
);
```


### default route eintragen
```php
$app->get('/', function (Request $request, Response $response) {
    return $response->withJson("Hello API - this is a json response");
});
```


## Serve
`php -S localhost:8000 -t ./public`