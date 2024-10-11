## NOTE

Fork of https://github.com/fahlisaputra/laravel-minify


*************************************************************************************************



## Installation

Minify for Laravel requires PHP 7.2 or higher. This particular version supports Laravel 8.x, 9.x, 10.x, and 11.x.

To get the latest version, simply require the project using [Composer](https://getcomposer.org):

```sh
composer require djdigitalemedien/laravel-minify
```
## Configuration
Minify for Laravel supports optional configuration. To get started, you'll need to publish all vendor assets:

```sh
php artisan vendor:publish --provider="djdigitalemedien\Minify\MinifyServiceProvider"
```

This will create a config/minify.php file in your app that you can modify to set your configuration. Also, make sure you check for changes to the original config file in this package between releases.

## Register the Middleware (Laravel 10 or older)
In order Minify for Laravel can intercept your request to minify and obfuscate, you need to add the Minify middleware to the `app/Http/Kernel.php` file:

```php
protected $middleware = [
    ....
    // Middleware to minify CSS
    \djdigitalemedien\Minify\Middleware\MinifyCss::class,
    // Middleware to minify Javascript
    \djdigitalemedien\Minify\Middleware\MinifyJavascript::class,
    // Middleware to minify Blade
    \djdigitalemedien\Minify\Middleware\MinifyHtml::class,
];
```
You can choose which middleware you want to use. Put all of them if you want to minify html, css, and javascript at the same time.

## Register the Middleware (Laravel 11 or newer)
In order Minify for Laravel can intercept your request to minify and obfuscate, you need to add the Minify middleware to the `bootstrap/app.php` file:

```php
->withMiddleware(function (Middleware $middleware) {
    $middleware->web(append: [
        \djdigitalemedien\Minify\Middleware\MinifyHtml::class,
        \djdigitalemedien\Minify\Middleware\MinifyCss::class,
        \djdigitalemedien\Minify\Middleware\MinifyJavascript::class,
    ]);
})
```

## Usage

This is how you can use Minify for Laravel in your project.

### Minify Asset Files
You must set `true` on `assets_enabled` in the `config/minify.php` file to minify your asset files. For example:

```php
"assets_enabled" => env("MINIFY_ASSETS_ENABLED", true),
```

You can minify your asset files by using the `minify()` helper function. This function will minify your asset files and return the minify designed route. In order to work properly, you need to put your asset files in the `resources/js` or  `resources/css` directory. For example:

```html
<link rel="stylesheet" href="{{ minify('/css/test.css') }}">
```

where `test.css` is located in the `resources/css` directory.

```html
<script src="{{ minify('/js/test.js') }}"></script>
```

where `test.js` is located in the `resources/js` directory.

### Automatic Insert Semicolon on Javascript or CSS
Use this option if Minify for Laravel makes your javascript or css not working properly. You can enable automatic insert semicolon on javascript or css by setting `true` on `insert_semicolon` in the `config/minify.php` file. For example:

```php
"insert_semicolon" => [
    'css' => env("MINIFY_CSS_SEMICOLON", true),
    'js' => env("MINIFY_JS_SEMICOLON", true),
],
```
Caution: this option is experimental. If the code still not working properly, you can disable this option and add semicolon manually to your Javascript or CSS code.

### Skip Minify on Blade
You can skip minify on blade by using attribute `ignore--minify` inside script or style tag. For example:

```html
<style ignore--minify>
    /* css */
</style>

<script ignore--minify>
   /* javascript */
</script>
```

### Skip Minify when Rendering View
You can skip minify when rendering view by passing `ignore_minify = true` in the view data. For example:

```php
return view('welcome', ['ignore_minify' => true]);
```

### Skip Minify by Route
You can skip minify by route by adding the route name to the `ignore` array in the `config/minify.php` file. For example:

```php
"ignore" => [
    '/admin'
],
```

### Custom Directives Replacement
You can replace custom directives by adding the directive name to the `directives` array in the `config/minify.php` file. For example in AlpineJS you can write `@click="function()"`. Unfortunately, Minify for Laravel will remove the `@` symbol. You can replace it by adding `@ => x-on:`  to the `directives` array. For example:

```php
"directives" => [
    '@' => 'x-on:',
],
```

### Keep Directives
You can keep directives by adding the directive name to the `keep_directives` array in the `config/minify.php` file. For example when you use `@vite`, you can add `@vite` to the `keep_directives` array. For example:

```php
"keep_directives" => [
    '@vite'
],
```

## Known Issues

- Minify for Laravel will remove the `@` symbol in the blade file. This will make the blade directive not working properly. You can fix this by adding `@ => x-on:` to the `directives` array in the `config/minify.php` file.
- Does not support for some Javascript framework. You can try experiment by changing the `insert_semicolon` option to `true` or `false` in the `config/minify.php` file.

## Contributing

If you find an issue, or have a better way to do something, feel free to open an issue, or a pull request. The package is far from perfect, and any help is welcome. There are no formal contribution guidelines, and there should be no contribution too small. All coding styles will be fixed during the pull request by StyleCI. So, don't worry too much about the code style. We'd love to hear from you!

## Thanks
Big thanks to the people who have contributed to this package:
- [@SaeedHeydari](https://github.com/SaeedHeydari)

## License
Laravel Minify is licensed under the [MIT license](LICENSE).

## Support
If you are having general issues with this package, feel free to contact us on [saputra@fahli.net](mailto:saputra@fahli.net)

## Report Vulnerability
Please read [our security policy](https://github.com/fahlisaputra/laravel-minify/security/policy) for more details.
