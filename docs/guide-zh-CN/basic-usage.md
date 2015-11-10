基础用法
===========

使用 ```Faker\Factory::create()``` 创建并初始化一个假数据生成器，然后就可用通过该对象不同的属性来生产不同类型的假数据。

```php

// 使用工厂模式生成 Faker\Generator 实例
$faker = Faker\Factory::create();

// 通过属性获取假数据
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

在测试里使用
-----------

在命令行应用里定义 `tests` 别名。比如，在 `basic` 为模板的项目里，别名应该加在 `console.php` 配置文件中：
`Yii::setAlias('tests', __DIR__ . '/../tests');`
在使用这个命令前，你需要对 [Faker](https://github.com/fzaninotto/Faker) 库有所了解（阅读手册），比如怎么通过给定的格式生成fixture模板文件。

```php
// users.php 在模板目录里 (默认在 @tests/unit/templates/fixtures)
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
    'intro' => $faker->sentence(7, true),  // generate a sentence with 7 words
];
```

正如你看到的，模板文件只是普通的PHP脚本，模板脚本应该返回包含一组键值对的数组，键代表表中的字段名，值代表假数据值。
当你运行 `fixture/generate` 命令，每一条假数据都会执行一次模板中定义的假数据生成格式。在模板中你有两个预定义变量可以使用。

* `$faker`: 假数据生成器实例
* `$index`: 当前假数据的索引。比如你要为user表生成3个假数据，索引将是 0，1，2。

有了模板文件，就可以通过下面的命令生成假数据了：

```
# 通过users的假数据模板生成假数据
php yii fixture/generate users

# 通过假数据模板同时生成多张表的假数据
php yii fixture/generate users profiles teams
```

在上面的代码中 `users` 是假数据模板的模板名称。运行上面的命令后，假数据会在假数据目录中生成 (默认在 `@tests/unit/fixtures` 文件夹)。

```
php yii fixture/generate-all
```

上面这个命令将会扫描模板目录下存放的所有模板并生成假数据，通过 `--count` 参数指定你想要生成多少行假数据。下面的命令将会使用所有的假数据
模板，每一个模板生成3条假数据。

```
php yii fixture/generate-all --count=3
```
使用不同的参数运行命令

```
# 生成俄语的假数据
php yii fixture/generate users --count=5 --language='ru_RU'

# 从其它文件夹读取模板
php yii fixture/generate-all --templatePath='@app/path/to/my/custom/templates'

# 生成假数据到其它目录
php yii fixture/generate-all --fixtureDataPath='@tests/acceptance/fixtures/data'
```

运行下面的命令查看所有可用的模板

```
# 列出默认模板目录的所有模板（默认使用 '@tests/unit/templates/fixtures' 目录）
php yii fixture/templates

# 列出指定目录下可用的模板
php yii fixture/templates --templatePath='@app/path/to/my/custom/templates'
```

你可以为表的字段创建自己的数据生成策略，参见 [Faker](https://github.com/fzaninotto/Faker) 手册获取更多的信息；
你创建的数据生成策略大概像下面这个样子：

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

在 console.php 配置文件中的 `$providers` 属性里加上你自己的数据生成策略提供器：

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
