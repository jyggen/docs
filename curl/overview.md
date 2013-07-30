# Overview

- [Introduction](#introduction)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)

<a name="introduction"></a>
## Introduction

Curl is simple and lightweight cURL library with support for multiple requests in parallel. It is installed through [Composer](http://getcomposer.org/) and you can [find it on Packagist here](https://packagist.org/packages/jyggen/Curl).

<a name="installation"></a>
## Installation

As with any Composer package, you must first include Composer's autoloader. If you plan to use the static helper it's recommended to import the namespace for quicker usage.

    require 'vendor/autoload.php';
    use jyggen\Curl;

<a name="configuration"></a>
## Configuration

You shouldn't have to do any configuration most of the time since the static helpers uses some sane defaults out of the box.

<a name="usage"></a>
## Usage

This section will cover the usage of the static helpers. This library was created with simplicity in mind, so in most cases you can use the class simply named `Curl`. The class consists of four methods, one for each REST verb: GET, PUT, POST and DELETE.

    Curl::get();
    Curl::post();
    Curl::put();
    Curl::delete();

If you request a single URL the helper will return a `Response` object, and if you request multiple URLs you'll get an array of objects. For more information about `Response`, [read its documentation](/curl/response).

    // Single URL
    var_dump(Curl::get('http://example.com/'));

    // Will output something like:
    class jyggen\Curl\Response#5 (6) {
        ...
    }

    // Multiple URLs
    var_dump(Curl::get(array('http://example.com/', 'http://example.org/')));

    // Will output something like:
    array(2) {
        [0] =>
        class jyggen\Curl\Response#7 (6) {
            ...
        }
        [1] =>
        class jyggen\Curl\Response#9 (6) {
            ...
        }
    )

You call `Curl::get()` and `Curl::delete()` the same way, with either an URL to request or an array of multiple URLs. With `Curl::post()` and `Curl::put()` you've got a second parameter to work with, the actual data you want to include in the request. This data is URL-encoded through `http_build_query()`, so the data may be an array or object containing properties.

    Curl::post('http://example.com/', array('username' => 'jyggen', 'password' => 'seaponies'));

    Curl::post(array(
        'http://example.com/' => array('username' => 'jyggen', 'password' => 'seaponies'),
        'http://example.org/' => array('username' => 'jyggen', 'password' => 'seaponies')
    ));

That's pretty much all there is to know about the static helpers. For more advanced usage, such as headers, cookies and cURL configuration, there's more in-depth class documentation available:

- [Dispatcher](/curl/dispatcher)
- [HeaderBag](/curl/headerbag)
- [Request](/curl/request)
- [Response](/curl/response)