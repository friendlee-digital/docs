# Helpers

Winter includes a variety of "helper" PHP functions. Many of these functions are used internally by Winter itself, however, you are free to use them in your own plugins and applications if you find them useful.

<style>
    .collection-method-list > p {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    .collection-method-list a {
        display: block;
    }
</style>

### Arrays

<div class="collection-method-list" markdown="1">

[Laravel `Arr::*()` Helpers](https://laravel.com/docs/9.x/helpers#arrays-and-objects-method-list)
[array_add](#method-array-add)
[array_divide](#method-array-divide)
[array_dot](#method-array-dot)
[array_undot](#method-array-undot)
[array_except](#method-array-except)
[array_first](#method-array-first)
[array_flatten](#method-array-flatten)
[array_forget](#method-array-forget)
[array_get](#method-array-get)
[array_only](#method-array-only)
[array_pluck](#method-array-pluck)
[array_pull](#method-array-pull)
[array_set](#method-array-set)
[array_sort](#method-array-sort)
[array_sort_recursive](#method-array-sort-recursive)
[array_where](#method-array-where)
[head](#method-head)
[last](#method-last)

</div>

### Paths

<div class="collection-method-list" markdown="1">

[Path Symbols](#path-symbols)
[app_path](#method-app-path)
[base_path](#method-base-path)
[config_path](#method-config-path)
[database_path](#method-database-path)
[media_path](#method-media-path)
[plugins_path](#method-plugins-path)
[public_path](#method-public-path)
[storage_path](#method-storage-path)
[temp_path](#method-temp-path)
[themes_path](#method-themes-path)
[uploads_path](#method-uploads-path)

</div>

### Strings

<div class="collection-method-list" markdown="1">

[Laravel `Str::*()` Helpers](https://laravel.com/docs/9.x/helpers#strings-method-list)
[camel_case](#method-camel-case)
[class_basename](#method-class-basename)
[e](#method-e)
[ends_with](#method-ends-with)
[snake_case](#method-snake-case)
[str_limit](#method-str-limit)
[starts_with](#method-starts-with)
[str_contains](#method-str-contains)
[str_finish](#method-str-finish)
[str_is](#method-str-is)
[str_plural](#method-str-plural)
[str_random](#method-str-random)
[str_singular](#method-str-singular)
[str_slug](#method-str-slug)
[studly_case](#method-studly-case)
[trans](#method-trans)
[trans_choice](#method-trans-choice)

</div>

### SVG

<div class="collection-method-list" markdown="1">

[extract](#method-svg-extract)

</div>

### Miscellaneous

<div class="collection-method-list" markdown="1">

[asset](#method-asset)
[config](#method-config)
[dd](#method-dd)
[env](#method-env)
[get](#method-get)
[input](#method-input)
[post](#method-post)
[redirect](#method-redirect)
[request](#method-request)
[response](#method-response)
[route](#method-route)
[secure_asset](#method-secure-asset)
[trace_log](#method-trace-log)
[trace_sql](#method-trace-sql)
[url](#method-url)

</div>

<style>
    #collection-method code {
        font-size: 14px;
    }

    #collection-method:not(.first-collection-method) {
        margin-top: 50px;
    }
</style>

<a name="arrays"></a>
## Arrays

<a name="method-array-add"></a>
#### `array_add()` {#collection-method .first-collection-method}

The `array_add` function adds a given key / value pair to the array if the given key doesn't already exist in the array:

```php
$array = array_add(['name' => 'Desk'], 'price', 100);

// ['name' => 'Desk', 'price' => 100]
```

<a name="method-array-divide"></a>
#### `array_divide()` {#collection-method}

The `array_divide` function returns two arrays, one containing the keys, and the other containing the values of the original array:

```php
list($keys, $values) = array_divide(['name' => 'Desk']);

// $keys: ['name']

// $values: ['Desk']
```

<a name="method-array-dot"></a>
#### `array_dot()` {#collection-method}

The `array_dot` function flattens a multi-dimensional array into a single level array that uses "dot" notation to indicate depth:

```php
$array = array_dot(['foo' => ['bar' => 'baz']]);

// ['foo.bar' => 'baz'];
```

<a name="method-array-undot"></a>
#### `array_undot()` {#collection-method}

The `array_undot` function is the counter-part to the `array_dot` method. It will convert a dot-notated array into a standard associative array:

```php
$array = array_undot([
    'foo.bar' => 'baz'
]);

// [
//    'foo' => [
//        'bar' => 'baz'
//    ]
// ]
```

<a name="method-array-except"></a>
#### `array_except()` {#collection-method}

The `array_except` method removes the given key / value pairs from the array:

```php
$array = ['name' => 'Desk', 'price' => 100];

$array = array_except($array, ['price']);

// ['name' => 'Desk']
```

<a name="method-array-first"></a>
#### `array_first()` {#collection-method}

The `array_first` method returns the first element of an array passing a given truth test:

```php
$array = [100, 200, 300];

$value = array_first($array, function ($key, $value) {
    return $value >= 150;
});

// 200
```

A default value may also be passed as the third parameter to the method. This value will be returned if no value passes the truth test:

```php
$value = array_first($array, $callback, $default);
```

<a name="method-array-flatten"></a>
#### `array_flatten()` {#collection-method}

The `array_flatten` method will flatten a multi-dimensional array into a single level.

```php
$array = ['name' => 'Joe', 'languages' => ['PHP', 'Ruby']];

$array = array_flatten($array);

// ['Joe', 'PHP', 'Ruby'];
```

<a name="method-array-forget"></a>
#### `array_forget()` {#collection-method}

The `array_forget` method removes a given key / value pair from a deeply nested array using "dot" notation:

```php
$array = ['products' => ['desk' => ['price' => 100]]];

array_forget($array, 'products.desk');

// ['products' => []]
```

<a name="method-array-get"></a>
#### `array_get()` {#collection-method}

The `array_get` method retrieves a value from a deeply nested array using "dot" notation:

```php
$array = ['products' => ['desk' => ['price' => 100]]];

$value = array_get($array, 'products.desk');

// ['price' => 100]
```

The `array_get` function also accepts a default value, which will be returned if the specific key is not found:

```php
$value = array_get($array, 'names.john', 'default');
```

<a name="method-array-only"></a>
#### `array_only()` {#collection-method}

The `array_only` method will return only the specified key / value pairs from the given array:

```php
$array = ['name' => 'Desk', 'price' => 100, 'orders' => 10];

$array = array_only($array, ['name', 'price']);

// ['name' => 'Desk', 'price' => 100]
```

<a name="method-array-pluck"></a>
#### `array_pluck()` {#collection-method}

The `array_pluck` method will pluck a list of the given key / value pairs from the array:

```php
$array = [
    ['developer' => ['name' => 'Brian']],
    ['developer' => ['name' => 'Stewie']]
];

$array = array_pluck($array, 'developer.name');

// ['Brian', 'Stewie'];
```

<a name="method-array-pull"></a>
#### `array_pull()` {#collection-method}

The `array_pull` method returns and removes a key / value pair from the array:

```php
$array = ['name' => 'Desk', 'price' => 100];

$name = array_pull($array, 'name');

// $name: Desk

// $array: ['price' => 100]
```

<a name="method-array-set"></a>
#### `array_set()` {#collection-method}

The `array_set` method sets a value within a deeply nested array using "dot" notation:

```php
$array = ['products' => ['desk' => ['price' => 100]]];

array_set($array, 'products.desk.price', 200);

// ['products' => ['desk' => ['price' => 200]]]
```

<a name="method-array-sort"></a>
#### `array_sort()` {#collection-method}

The `array_sort` method sorts the array by the results of the given Closure:

```php
$array = [
    ['name' => 'Desk'],
    ['name' => 'Chair'],
];

$array = array_values(array_sort($array, function ($value) {
    return $value['name'];
}));

/*
    [
        ['name' => 'Chair'],
        ['name' => 'Desk'],
    ]
*/
```

<a name="method-array-sort-recursive"></a>
#### `array_sort_recursive()` {#collection-method}

The `array_sort_recursive` function recursively sorts the array using the `sort` function:

```php
$array = [
    [
        'Brian',
        'Shannon',
        'Alec',
    ],
    [
        'PHP',
        'Ruby',
        'JavaScript',
    ],
];

$array = array_sort_recursive($array);

/*
    [
        [
            'Alec',
            'Brian',
            'Shannon',
        ],
        [
            'JavaScript',
            'PHP',
            'Ruby',
        ]
    ];
*/
```

<a name="method-array-where"></a>
#### `array_where()` {#collection-method}

The `array_where` function filters the array using the given Closure:

```php
$array = [100, '200', 300, '400', 500];

$array = array_where($array, function ($value, $key) {
    return is_string($value);
});

// [1 => 200, 3 => 400]
```

<a name="method-head"></a>
#### `head()` {#collection-method}

The `head` function simply returns the first element in the given array:

```php
$array = [100, 200, 300];

$first = head($array);

// 100
```

<a name="method-last"></a>
#### `last()` {#collection-method}

The `last` function returns the last element in the given array:

```php
$array = [100, 200, 300];

$last = last($array);

// 300
```

<a name="paths"></a>
## Paths

<a name="path-symbols"></a>
#### Path Symbols

Path prefix symbols can be used to create a dynamic path. For example, a path beginning with `~/` will create a path relative to the application:

```yaml
list: ~/plugins/acme/pay/models/invoiceitem/columns.yaml
```

These symbols are supported for creating dynamic paths:

Symbol | Description
------------- | -------------
`~` | Relative to the application directory
`$` | Relative to the plugins directory
`#` | Relative to the themes directory

<a name="method-app-path"></a>
#### `app_path()` {#collection-method}

The `app_path` function returns the fully qualified path to the `app` directory:

```php
$path = app_path();
```

You may also use the `app_path` function to generate a fully qualified path to a given file relative to the application directory:

```php
$path = app_path('Http/Controllers/Controller.php');
```

<a name="method-base-path"></a>
#### `base_path()` {#collection-method}

The `base_path` function returns the fully qualified path to the project root:

```php
$path = base_path();
```

You may also use the `base_path` function to generate a fully qualified path to a given file relative to the application directory:

```php
$path = base_path('vendor/bin');
```

<a name="method-config-path"></a>
#### `config_path($path = '')` {#collection-method}

The `config_path` function returns the fully qualified path to the application configuration directory:

```php
$path = config_path();
```

You may also use the `config_path` function to generate a fully qualified path to a given file relative to the config directory:

```php
$path = config_path('dev/cms.php');
```

<a name="method-database-path"></a>
#### `database_path()` {#collection-method}

The `database_path` function returns the fully qualified path to the application's database directory:

```php
$path = database_path();
```

<a name="method-media-path"></a>
#### `media_path($path = '')` {#collection-method}

The `media_path` function returns the fully qualified path to the application media directory:

```php
$path = media_path();
```

You may also use the `media_path` function to generate a fully qualified path to a given file relative to the media directory:

```php
$path = media_path('images/myimage.png');
```

<a name="method-plugins-path"></a>
#### `plugins_path($path = '')` {#collection-method}

The `plugins_path` function returns the fully qualified path to the application plugin directory:

```php
$path = plugins_path();
```

You may also use the `plugins_path` function to generate a fully qualified path to a given file relative to the plugins directory:

```php
$path = plugins_path('author/plugin/routes.php');
```

<a name="method-public-path"></a>
#### `public_path()` {#collection-method}

The `public_path` function returns the fully qualified path to the `public` directory:

```php
$path = public_path();
```

<a name="method-storage-path"></a>
#### `storage_path($path = '')` {#collection-method}

The `storage_path` function returns the fully qualified path to the `storage` directory:

```php
$path = storage_path();
```

You may also use the `storage_path` function to generate a fully qualified path to a given file relative to the storage directory:

```php
$path = storage_path('app/file.txt');
```

<a name="method-temp-path"></a>
#### `temp_path($path = '')` {#collection-method}

The `temp_path` function returns the fully qualified path to a writable directory for temporary files:

```php
$path = temp_path();
```

You may also use the `temp_path` function to generate a fully qualified path to a given file relative to the temp directory:

```php
$path = temp_path('app/file.txt');
```

<a name="method-themes-path"></a>
#### `themes_path($path = '')` {#collection-method}

The `themes_path` function returns the fully qualified path to the `themes` directory:

```php
$path = themes_path();
```

You may also use the `themes_path` function to generate a fully qualified path to a given file relative to the themes directory:

```php
$path = themes_path('mytheme/file.txt');
```

<a name="method-uploads-path"></a>
#### `uploads_path($path = '')` {#collection-method}

The `uploads_path` function returns the fully qualified path to the application uploads directory:

```php
$path = uploads_path();
```

You may also use the `uploads_path` function to generate a fully qualified path to a given file relative to the uploads directory:

```php
$path = uploads_path('public/file.txt');
```

<a name="strings"></a>
## Strings

<a name="method-camel-case"></a>
#### `camel_case()` {#collection-method}

The `camel_case` function converts the given string to `camelCase`:

```php
$camel = camel_case('foo_bar');

// fooBar
```

<a name="method-class-basename"></a>
#### `class_basename()` {#collection-method}

The `class_basename` returns the class name of the given class with the class' namespace removed:

```php
$class = class_basename('Foo\Bar\Baz');

// Baz
```

<a name="method-e"></a>
#### `e()` {#collection-method}

The `e` function runs `htmlentities` over the given string:

```php
echo e('<html>foo</html>');

// &lt;html&gt;foo&lt;/html&gt;
```

<a name="method-ends-with"></a>
#### `ends_with()` {#collection-method}

The `ends_with` function determines if the given string ends with the given value:

```php
$value = ends_with('This is my name', 'name');

// true
```

<a name="method-snake-case"></a>
#### `snake_case()` {#collection-method}

The `snake_case` function converts the given string to `snake_case`:

```php
$snake = snake_case('fooBar');

// foo_bar
```

<a name="method-str-limit"></a>
#### `str_limit()` {#collection-method}

The `str_limit` function limits the number of characters in a string. The function accepts a string as its first argument and the maximum number of resulting characters as its second argument:

```php
$value = str_limit('The CMS platform that gets back to basics.', 6);

// The CMS...
```

<a name="method-starts-with"></a>
#### `starts_with()` {#collection-method}

The `starts_with` function determines if the given string begins with the given value:

```php
$value = starts_with('The cow goes moo', 'The');

// true
```

<a name="method-str-contains"></a>
#### `str_contains()` {#collection-method}

The `str_contains` function determines if the given string contains the given value:

```php
$value = str_contains('The bird goes tweet', 'bird');

// true
```

<a name="method-str-finish"></a>
#### `str_finish()` {#collection-method}

The `str_finish` function adds a single instance of the given value to a string:

```php
$string = str_finish('this/string', '/');

// this/string/
```

<a name="method-str-is"></a>
#### `str_is()` {#collection-method}

The `str_is` function determines if a given string matches a given pattern. Asterisks may be used to indicate wildcards:

```php
$value = str_is('foo*', 'foobar');

// true

$value = str_is('baz*', 'foobar');

// false
```

<a name="method-str-plural"></a>
#### `str_plural()` {#collection-method}

The `str_plural` function converts a string to its plural form. This function currently only supports the English language:

```php
$plural = str_plural('car');

// cars

$plural = str_plural('child');

// children
```

<a name="method-str-random"></a>
#### `str_random()` {#collection-method}

The `str_random` function generates a random string of the specified length:

```php
$string = str_random(40);
```

<a name="method-str-singular"></a>
#### `str_singular()` {#collection-method}

The `str_singular` function converts a string to its singular form. This function currently only supports the English language:

```php
$singular = str_singular('cars');

// car
```

<a name="method-str-slug"></a>
#### `str_slug()` {#collection-method}

The `str_slug` function generates a URL friendly "slug" from the given string:

```php
$title = str_slug("Winter CMS", "-");

// winter-cms
```

<a name="method-studly-case"></a>
#### `studly_case()` {#collection-method}

The `studly_case` function converts the given string to `StudlyCase`:

```php
$value = studly_case('foo_bar');

// FooBar
```

<a name="method-trans"></a>
#### `trans()` {#collection-method}

The `trans` function translates the given language line using your [localization files](../plugin/localization):

```php
echo trans('validation.required'):
```

<a name="method-trans-choice"></a>
#### `trans_choice()` {#collection-method}

The `trans_choice` function translates the given language line with inflection:

```php
$value = trans_choice('foo.bar', $count);
```

<a name="svg-utility"></a>
## SVG

Winter includes a simple SVG utility that allows you to extract sanitized SVG markup from a given path. This can be
useful for sanitization, or for using SVG markup directly in your themes.

<a name="method-svg-extract"></a>
#### `Svg::extract()` {#collection-method}

The `extract` method allows you to extract the sanitized SVG markup in a given path. Sanitization prevents the use of
JavaScript, remote sources and CSS imports, stopping common attack vectors within SVG code.

```php
$svg = Svg::extract('/path/to/image.svg');
```

By default, the output SVG markup is minified. The second parameter allows you to disable this by setting it to `false`.

```php
$unminifiedSvg = Svg::extract('/path/to/image.svg', false);
```

<a name="miscellaneous"></a>
## Miscellaneous

<a name="method-asset"></a>
#### `asset()` {#collection-method}

Generate a URL for an asset using the current scheme of the request (HTTP or HTTPS):

```php
$url = asset('img/photo.jpg');
```

You can configure the asset URL host by setting the `ASSET_URL` variable in your `.env` file (or `asset_url` in your `config/app.php` file). This can be useful if you host your assets on an external service like Amazon S3 or another CDN:

```php
// ASSET_URL=http://example.com/assets

$url = asset('img/photo.jpg'); // http://example.com/assets/img/photo.jpg
```

<a name="method-config"></a>
#### `config()` {#collection-method}

The `config` function gets the value of a configuration variable. The configuration values may be accessed using "dot" syntax, which includes the name of the file and the option you wish to access. A default value may be specified and is returned if the configuration option does not exist:

```php
$value = config('app.timezone');

$value = config('app.timezone', $default);
```

The `config` helper may also be used to set configuration variables at runtime by passing an array of key / value pairs:

```php
config(['app.debug' => true]);
```

<a name="method-dd"></a>
#### `dd()` {#collection-method}

The `dd` function dumps the given variable and ends execution of the script:

```php
dd($value);
```

<a name="method-env"></a>
#### `env()` {#collection-method}

The `env` function gets the value of an environment variable or returns a default value:

```php
$env = env('APP_ENV');

// Return a default value if the variable doesn't exist...
$env = env('APP_ENV', 'production');
```

<a name="method-get"></a>
#### `get()` {#collection-method}

The `get` function obtains an input item from the request, restricted to GET variables only:

```php
$value = get('key', $default = null)
```

<a name="method-input"></a>
#### `input()` {#collection-method}

The `input` function obtains an input item from the request:

```php
$value = input('key', $default = null)
```

<a name="method-post"></a>
#### `post()` {#collection-method}

The `post` function obtains an input item from the request, restricted to POST variables only:

```php
$value = post('key', $default = null)
```

<a name="method-redirect"></a>
#### `redirect()` {#collection-method}

The `redirect` function return an instance of the redirector to do [redirect responses](../services/response-view#redirects):

```php
return redirect('/home');
```

<a name="method-request"></a>
#### `request()` {#collection-method}

The `request` function returns the current [request instance](../services/request-input):

```php
$referer = request()->header('referer');
```

<a name="method-response"></a>
#### `response()` {#collection-method}

The `response` function creates a [response](../services/response-view) instance or obtains an instance of the response factory:

```php
return response('Hello World', 200, $headers);

return response()->json(['foo' => 'bar'], 200, $headers);
```

<a name="method-route"></a>
#### `route()` {#collection-method}

The `route` function generates a URL for the given [named route](../services/router):

```php
$url = route('routeName');
```

If the route accepts parameters, you may pass them as the second argument to the method:

```php
$url = route('routeName', ['id' => 1]);
```

<a name="method-secure-asset"></a>
#### `secure_asset()` {#collection-method}

Generate a URL for an asset using HTTPS:

```php
echo secure_asset('foo/bar.zip', $title, $attributes = []);
```

<a name="method-trace-log"></a>
#### `trace_log()` {#collection-method}

The `trace_log` function writes a trace message to the log file.

```php
trace_log('This code has passed...');
```

The function supports passing exceptions, arrays and objects:

```php
trace_log($exception);

trace_log($array);

trace_log($object);
```

You may also pass multiple arguments to trace multiple messages:

```php
trace_log($value1, $value2, $exception, '...');
```

<a name="method-trace-sql"></a>
#### `trace_sql()` {#collection-method}

The `trace_sql` function enables database logging and begins to monitor all SQL output.

```php
trace_sql();

Db::table('users')->count();

// select count(*) as aggregate from users
```

<a name="method-url"></a>
#### `url()` {#collection-method}

The `url` function generates a fully qualified URL to the given path:

```php
echo url('user/profile');

echo url('user/profile', [1]);
```
