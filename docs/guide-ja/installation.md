インストール
============

## Composer パッケージを取得する

このエクステンションをインストールするのに推奨される方法は [composer](http://getcomposer.org/download/) によるものです。

下記のコマンドを実行してください。

```
php composer.phar require --prefer-dist yiisoft/yii2-faker
```

または、あなたの `composer.json` ファイルの `require` セクションに、下記を追加してください。

```
"yiisoft/yii2-faker": "~2.0.0"
```

## アプリケーションを構成する

このエクステンションを使用するためには、次のコードをあなたのアプリケーション構成情報 (console.php) に追加するだけで十分です。

```php
'controllerMap' => [
    'fixture' => [
        'class' => 'yii\faker\FixtureController',
    ],
],
```
