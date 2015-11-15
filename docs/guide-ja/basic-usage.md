基本的な使用方法
================

```Faker\Factory::create()``` を使って、faker ジェネレータを作成して初期化します。
この faker ジェネレータのプロパティの中から、あなたが必要とするデータのタイプを指定する名前のものを選んでアクセスすると、疑似データを生成することが出来ます。

```php

// factory を使って Faker\Generator のインスタンスを作成する
$faker = Faker\Factory::create();

// プロパティにアクセスして、データを生成する
echo $faker->name;
  // 'Lucy Cechtelar';
echo $faker->address;
  // "426 Jordy Lodge
  // Cartwrightshire, SC 88120-6700"
echo $faker->text;
  // Sint velit eveniet. Rerum atque repellat voluptatem quia rerum. Numquam excepturi
  // beatae sint laudantium consequatur. Magni occaecati itaque sint et sit tempore. Nesciunt
  // amet quidem. Iusto deleniti cum autem ad quia aperiam.
  // A consectetur quos aliquam. In iste aliquid et aut similique suscipit. Consequatur qui
  // quaerat iste minus hic expedita. Consequuntur error magni et laboriosam. Aut aspernatur
  // voluptatem sit aliquam. Dolores voluptatum est.
  // Aut molestias et maxime. Fugit autem facilis quos vero. Eius quibusdam possimus est.
  // Ea quaerat et quisquam. Deleniti sunt quam. Adipisci consequatur id in occaecati.
  // Et sint et. Ut ducimus quod nemo ab voluptatum.
```

テストでの使用方法
------------------

`tests` というエイリアスをコンソールの構成情報で定義してください。
例えば、`basic` プロジェクトテンプレートであれば、`Yii::setAlias('tests', dirname(__DIR__) . '/tests/codeception');` を `console.php` 構成情報ファイルに追加します。
このコマンドを使い始めるためには、[Faker](https://github.com/fzaninotto/Faker) ライブラリに慣れ親しんで (ガイドを読んでください)、指定された形式に従ってフィクスチャテンプレートファイルを生成しなければなりません。

```php
// テンプレートパス (デフォルトでは、@tests/unit/templates/fixtures の下) の users.php ファイル
/**
 * @var $faker \Faker\Generator
 * @var $index integer
 */
return [
    'name' => $faker->firstName,
    'phone' => $faker->phoneNumber,
    'city' => $faker->city,
    'password' => Yii::$app->getSecurity()->generatePasswordHash('password_' . $index),
    'auth_key' => Yii::$app->getSecurity()->generateRandomString(),
    'intro' => $faker->sentence(7, true),  // 単語 7 個の文を生成
];
```

御覧のように、テンプレートファイルは単純な通常の PHP スクリプトです。
スクリプトは「キー・値」ペアの配列を返さなければなりません。
ここで、キーはテーブルのカラム名を表し、値は対応する値です。
`fixture/generate` コマンドを実行すると、生成されるデータ行のすべてについてこのスクリプトが実行されます。
スクリプトの中では、次の二つの事前定義された変数を使用することが出来ます。

* `$faker`: Faker ジェネレータのインスタンス。
* `$index`: 現在のフィクスチャのインデックス。
   例えば、user テーブルのために三つのフィクスチャを生成する必要がある場合、インデックスは 0..2 です。

このようなテンプレートファイルを使って、次のようなコマンドでフィクスチャを生成することが出来ます。

```
# user のフィクスチャテンプレートからフィクスチャを生成する
php yii fixture/generate users

# いくつかのフィクスチャデータファイルを生成する
php yii fixture/generate users profiles teams
```

上記のコードで `users` がテンプレートの名前です。
このコマンドを実行した後、テンプレートと同じ名前のファイルがフィクスチャパス (`@tests/unit/fixtures` フォルダ) に生成されます。

```
php yii fixture/generate-all
```

上記のコマンドは、テンプレートパスに保存されている全てのテンプレートに対してフィクスチャを生成して、テンプレートの名前と同じファイル名でフィクスチャパスに保存します。
`--count` オプションによって、テンプレートごとに生成したいフィクスチャの数を指定することが出来ます。
下記のコードでは、全てのテンプレートに対してフィクスチャを生成し、各フィクスチャファイルについては 3 行 (3 個) のフィクスチャを得ることになります。

```
php yii fixture/generate-all --count=3
```

さらに、このコマンドには、さまざまなオプションがあります。

```
# ロシア語でフィクスチャを生成する
php yii fixture/generate users --count=5 --language="ru_RU"

# 他のパスからテンプレートを読む
php yii fixture/generate-all --templatePath='@app/path/to/my/custom/templates'

# 他のディレクトリにフィクスチャを生成する
php yii fixture/generate-all --fixtureDataPath='@tests/acceptance/fixtures/data'
```

次のコマンドを実行すると、利用可能な全てのテンプレートを見ることが出来ます。

```
# デフォルトのテンプレートパス (すなわち、'@tests/unit/templates/fixtures') の下にある全てのテンプレートをリストする
php yii fixture/templates

# 指定したテンプレートパスの下にある全てのテンプレートをリストする
php yii fixture/templates --templatePath='@app/path/to/my/custom/templates'
```

カスタムテーブルフィールドのためにあなた自身のデータプロバイダを作成することも出来ます。
詳細な情報は [Faker](https://github.com/fzaninotto/Faker) ライブラリのガイドを参照してください。
例えば、次のようにして、カスタムプロバイダを作成します。

```php
class Book extends \Faker\Provider\Base
{

    public function title($nbWords = 5)
    {
        $sentence = $this->generator->sentence($nbWords);
        return mb_substr($sentence, 0, mb_strlen($sentence) - 1);
    }

 }
```

そして、これを console.php 構成情報ファイルで現在のコマンドの `$provider` プロパティに追加することによって、使用することが出来るようになります。

```php
'controllerMap' => [
    'fixture' => [
        'class' => 'yii\faker\FixtureController',
        'providers' => [
            'app\tests\unit\faker\providers\Book',
        ],
    ],
]
```
