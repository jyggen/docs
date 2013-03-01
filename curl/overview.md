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

You shouldn't have to do any configuration most of the time since the static helpers uses some sane defaults out of the box. For cURL configuration and more advanced requests (like headers, cookies etc.), please [see the documentation](/curl/session) for `Session`.

## Usage

This section will cover the usage of the static helpers. Curl was created with simplicity in mind, so in most cases you can use the class simply named `Curl`. The class consists of four methods, one for each REST verb: GET, PUT, POST and DELETE.

    Curl::get();
    Curl::put();
    Curl::post();
    Curl::delete();

If you request a single URL the helper will return a `Response` object, and if you request multiple URLs you'll get an array of objects. For more information about `Response`, [read its documentation](/curl/response).

    // Single URL
    var_dump(Curl::get('http://example.com/'));

    // Will output something like:
    class jyggen\Curl\Response#4 (6) {
        ...
    }

    // Multiple URLs
    var_dump(Curl::get(array('http://example.com/', 'http://example.org/')));

    // Will output something like:
    array(2) {
        [0] =>
        class jyggen\Curl\Response#5 (6) {
            ...
        }
        [0] =>
        class jyggen\Curl\Response#7 (6) {
            ...
        }
    )
