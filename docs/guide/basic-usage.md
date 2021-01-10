Basic Usage
===========

Use `Faker\Factory::create()` to create and initialize a faker generator, which can generate data by accessing properties named after the type of data you want.

```php
// use the factory to create a Faker\Generator instance
$faker = Faker\Factory::create();

// generate data by accessing properties
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

Testing
-----------

Define a `tests` alias in your console config. For example, for the `basic` project template, this should be added
to the `console.php` configuration: `Yii::setAlias('tests', dirname(__DIR__) . '/tests/codeception');`
To start using this command you need to be familiar (read guide) with the [Faker](https://github.com/FakerPHP/Faker) library and
generate fixture template files, according to the given format:

```php
// users.php file under the template path (by default @tests/unit/templates/fixtures)
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

As you can see, the template file is just a regular PHP script. The script should return an array of key-value
pairs, where the keys represent the table column names and the values the corresponding value. When you run
the `fixture/generate` command, the script will be executed once for every data row being generated.
In this script, you can use the following two predefined variables:

* `$faker`: the Faker generator instance
* `$index`: the current fixture index. For example if user need to generate 3 fixtures for user table, it will be 0..2.

With such a template file, you can generate your fixtures using the commands like the following:

```
# generate fixtures from users fixture template
php yii fixture/generate users

# to generate several fixture data files
php yii fixture/generate users profiles teams
```

In the code above `users` is a template name. After running this command, a new file with the same template name
will be created under the fixture path (`@tests/unit/fixtures` folder).

```
php yii fixture/generate-all
```

This command will generate fixtures for all template files that are stored under the template path and
store fixtures under the fixtures path with file names same as templates names.
You can specify how many fixtures per file you need by the `--count` option. In the code below we generate
all fixtures and in each file there will be 3 rows (fixtures).

```
php yii fixture/generate-all --count=3
```

You can specify different options of this command:

```
# generate fixtures in russian language
php yii fixture/generate users --count=5 --language="ru_RU"

# read templates from the other path
php yii fixture/generate-all --templatePath='@app/path/to/my/custom/templates'

# generate fixtures into other directory.
php yii fixture/generate-all --fixtureDataPath='@tests/acceptance/fixtures/data'
```

You can see all available templates by running command:

```
# list all templates under the default template path (i.e. '@tests/unit/templates/fixtures')
php yii fixture/templates

# list all templates under the specified template path
php yii fixture/templates --templatePath='@app/path/to/my/custom/templates'
```

You also can create your own data providers for custom tables fields, see [Faker](https://github.com/fzaninotto/Faker) library guide for more info;
After you created custom provider, for example:

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

You can use it by adding it to the `$providers` property of the current command. In your console.php config:

```php
return [
    'controllerMap' => [
        'fixture' => [
            'class' => 'yii\faker\FixtureController',
            'providers' => [
                'app\tests\unit\faker\providers\Book',
            ],
        ],
        // ...
    ],
    // ...
];
```


Yii 2 Advanced Template
-----------------------

If you want to run faker in the Yii 2 advanced template, you need to set `templatePath` and `fixtureDataPath`. For example if you want to setup common fixtures, use the following config in `console/config/main.php`:

```php
return [
    'controllerMap' => [
        'fixture' => [
            'class' => 'yii\faker\FixtureController',
            'templatePath' => '@common/tests/templates/fixtures',
            'fixtureDataPath' => '@common/tests/fixtures/data',
        ],
        // ...
    ],
    // ...
];
```
