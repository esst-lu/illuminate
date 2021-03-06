Logging
=======

Application logs should be consistent, e.g. log entries from Laravel and Yii should be written into the same channel.
The best way to achieve this will be usage of `\Yii2tech\Illuminate\Yii\Log\Logger` instead of the standard Yii logger.
It can be setup at application entry script:

```php
<?php

Yii::setLogger(new Yii2tech\Illuminate\Yii\Log\Logger); // replace standard logger

$config = require(__DIR__ . '/../config/web.php');

(new yii\web\Application($config))->run();
```

This will make all log entries written by Yii to be passed to the Laravel logger.
It allows you to have single log configuration and access to [Monolog](https://github.com/Seldaek/monolog) logging tools.

You may configure Yii logger to be used via "yii.middleware.logger" configuration key. If it is set, `\Yii2tech\Illuminate\Http\YiiApplicationMiddleware`
will bootstrap it automatically before running Yii application. Laravel configuration example:

```php
<?php
// file "config/yii.php"

return [
    'middleware' => [
        // ..
        'logger' => [
            '__class' => Yii2tech\Illuminate\Yii\Log\Logger::class,
        ],
    ],
];
```

Note that Yii extensions, which utilizes logs, like [yiisoft/yii2-debug](https://github.com/yiisoft/yii2-debug), will cease to
function with `\Yii2tech\Illuminate\Yii\Log\Logger` applied. Consider usage of laravel development tools like [laravel/telescope](https://github.com/laravel/telescope)
and [barryvdh/laravel-debugbar](https://github.com/barryvdh/laravel-debugbar) instead.

Alternatively you can use `\Yii2tech\Illuminate\Yii\Log\Target` as a target for standard Yii logger. It will transfer log
entries to the Laravel just the same, still allowing usage of other log targets. Application configuration example:

```php
<?php

return [
    'components' => [
        'log' => [
            'targets' => [
                [
                    'class' => Yii2tech\Illuminate\Yii\Log\Target::class,
                ],
                // ...
            ],
        ],
        // ...
    ],
    // ...
];
```
