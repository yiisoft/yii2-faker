安装
============

## 通过Composer安装

推荐的安装方式是通过 [composer](http://getcomposer.org/download/) 工具安装。

直接使用composer命令安装

```
php composer.phar require --prefer-dist yiisoft/yii2-faker
```

或者添加

```json
"yiisoft/yii2-faker": "~2.0.0"
```

到你项目里的composer.json文件中。

## 配置

要使用此扩展，只需要在你的项目配置里 (console.php) 添加如下配置：

```php
'controllerMap' => [
    'fixture' => [
        'class' => 'yii\faker\FixtureController',
    ],
],
```
