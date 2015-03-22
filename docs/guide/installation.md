Installation
============

## Getting Composer package

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist yiisoft/yii2-faker
```

or add

```json
"yiisoft/yii2-faker": "~2.0.0"
```

to the require section of your composer.json.

## Configuring Application

To use this extension,  simply add the following code in your application configuration (console.php):

```php
'controllerMap' => [
    'fixture' => [
        'class' => 'yii\faker\FixtureController',
    ],
],
```
