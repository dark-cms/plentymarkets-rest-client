# plentymarkets-rest-client

[![Latest Version on Packagist](https://img.shields.io/packagist/v/repat/plentymarkets-rest-client.svg?style=flat-square)](https://packagist.org/packages/repat/plentymarkets-rest-client)
[![Total Downloads](https://img.shields.io/packagist/dt/repat/plentymarkets-rest-client.svg?style=flat-square)](https://packagist.org/packages/repat/plentymarkets-rest-client)

This is a PHP package for Plentymarkets new REST API. The API is relatively new (March 2017), so not everything might work correctly and this package might also be out of date at some point.

I'm not in anyway affiliated with Plentymarkets, nor do I get paid for this by anybody. As it says in the license, this software is 'as-is'. If you want/need more features, open a GitHub ticket or write a pull request. I'll do my best :) That said, I don't work for the company I developed this for anymore, so if you have any interest in becoming a contributor on this repo, let me know.

You can find the Plentymarkets documentation [here](https://developers.plentymarkets.com/):

## Overview

* Functions for the 4 HTTP verbs: GET, POST, PUT, DELETE
* Automatic login and refresh if login is not valid anymore
* Simple one-time configuration with PHP array (will be saved serialized in a file)
* Functions return an associative array
* Handle rate limiting (thanks [hepisec](http://github.com/hepisec))

## Installation

Available via composer on [Packagist](https://packagist.org/packages/repat/plentymarkets-rest-client):

`composer require repat/plentymarkets-rest-client`

## Usage

```php
use repat\PlentymarketsRestClient\PlentymarketsRestClient;

// path to store the configuration in
$configFilePath = ".plentymarkets-rest-client.config.php";
// $config only has to be set once like this
$config = [
    "username" => "PM_USERNAME",
    "password" => "PM_PASSWORD",
    "url" => "https://www.plentymarkets-system.tld",
];

// Handle (Guzzle) Exceptions yourself - optional 3rd parameter
$handleExceptions = PlentymarketsRestClient::HANDLE_EXCEPTIONS; // true
$handleExceptions = PlentymarketsRestClient::DONT_HANDLE_EXCEPTIONS; // false (default)

// Init
$client = new PlentymarketsRestClient($configFilePath, $config, $handleExceptions);

// After that just use it like this:
$client = new PlentymarketsRestClient($configFilePath);
```

It's possible to use the 4 HTTP verbs like this

```php
$client->get($path, $parameterArray);
$client->post($path, $parameterArray);
$client->put($path, $parameterArray);
$client->delete($path, $parameterArray);

// $parameterArray has to be a PHP array. It will be transformed into JSON automatically in case
// of POST, PUT and DELETE or into query parameters in case of GET.
// You don't _have_ to specify it, it will then just be empty
$parameterArray = [
    "createdAtFrom" => "2016-10-24T13:33:23+02:00"
];

// $path is the path you find in the Plentymarkets documentation
$path = "rest/orders/";
```

It's also possible to use the function like this. It gives you more freedom, since
you can specify the method and the $parameters given are directly given to the [guzzle
object](http://docs.guzzlephp.org/en/latest/quickstart.html).

```php
$client->singleCall("GET", $guzzleParameterArray);
```

### Errors

* If there was an error with the call (=> guzzle throws an exception) all functions will return false
* If the specified config file doesn't exist or doesn't include username/password/url, an exception will be thrown

## TODO

* Refresh without new login but refresh-token

## Dependencies

* [https://packagist.org/packages/nesbot/carbon](nesbot/carbon) for date comparison
* [https://packagist.org/packages/guzzlehttp/guzzle](guzzlehttp/guzzle) for HTTP calls.

## License

* see [LICENSE](https://github.com/repat/plentymarkets-rest-client/blob/master/LICENSE) file

## Changelog

* 0.1.13 Change from [Laravels `str_contains`](https://github.com/laravel/framework/blob/8.x/src/Illuminate/Support/Str.php#L181) to [PHPs `stripos()`](https://www.php.net/manual/de/function.stripos.php) because [it doesn't require `ext-mbstring`](https://github.com/repat/plentymarkets-rest-client/pull/16#issuecomment-880731813) (thx DanMan)
* 0.1.12 Remove `danielstjules/stringy` dependency, copy over [Laravels `str_contains`](https://github.com/laravel/framework/blob/8.x/src/Illuminate/Support/Str.php#L181) instead
* 0.1.11 PHP 8 Support & wait in case of _short period read limit reached_ error (thx [fwehrhausen](https://github.com/repat/plentymarkets-rest-client/pull/15))
* 0.1.10 Allow dealing with Exceptions yourself by passing `true` as 3rd parameter
* 0.1.9 Bugfix `isAccessTokenValid()` (thx [hochdruckspezialist](https://github.com/repat/plentymarkets-rest-client/pull/14))
* 0.1.8 Update, so Carbon 2.0 can be used (thx [stefnats](https://github.com/repat/plentymarkets-rest-client/pull/12))
* 0.1.7 Fix constructor according to README (thx [daniel-mannheimer](https://github.com/repat/plentymarkets-rest-client/pull/11))
* 0.1.6 Support for HTTP `PATCH` (thx [hepisec](https://github.com/repat/plentymarkets-rest-client/pull/10))
* 0.1.5 Remove check for `www.` as it breaks subdomains (thx [daniel-mannheimer](https://github.com/repat/plentymarkets-rest-client/pull/9))
* 0.1.4 Automatic rate limiting (thx [hepisec](https://github.com/repat/plentymarkets-rest-client/pull/8))
* 0.1.3 Fix PHP 7.2 dependency
* 0.1.2 Fix Carbon dependency
* 0.1.1 Update Guzzle for PHP 7.2
* 0.1 initial release

## Contact

* Homepage: [https://repat.de](https://repat.de)
* e-mail: repat@repat.de
* Twitter: [@repat123](https://twitter.com/repat123 "repat123 on twitter")

[![Flattr this git repo](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=repat&url=https://github.com/repat/plentymarkets-rest-client&title=plentymarkets-rest-client&language=&tags=github&category=software)
