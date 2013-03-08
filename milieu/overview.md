# Overview

- [Introduction](#introduction)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)

<a name="introduction"></a>
## Introduction

Milieu is a lightweight environment initialization, detection and configuration library. It is installed through [Composer](http://getcomposer.org/) and you can [find it on Packagist here](https://packagist.org/packages/jyggen/Milieu).

<a name="installation"></a>
## Installation

As with any Composer package, you must first include Composer's autoloader. It's also recommended to import Milieu's namespace for quicker usage.

    require 'vendor/autoload.php';
    use Milieu\Milieu;

<a name="configuration"></a>
## Configuration

Before you start utilizing Milieu you must define all your environments through the static variable `environments`.

    Milieu::$environments = array(
        'dev'  => array('localhost', '127.0.01'),
        'test' => array('test.example.com')
    );

This will tell Milieu (or actually [Environment](/milieu/environment)) that we've got two environments we want it to detect: `dev` and `test`.

For each environment we define we also need to supply an array of detection rules, and these rules will be matched against a wide variety of different values (such as hostname, server name, server address etc.).

You can use other detection rules than static strings though, both wildcards and closures are supported by Milieu to make your environments more dynamic.

    Milieu::$environments = array(
        'dev' => array('dev.*.com', '192.168.*', (function(){
            return (PHP_VERSION === 5);
        }))
    );

The asterisk (*) is the wildcard, which will match anything and everything at that position in the string. In the example above all of the following strings will trigger the environment:

    dev.example.com
    dev.something.else.com
    192.168.0
    192.168.13.37
    192.168.you.though.this.was.an.IP.com

The closure in the example (and any other closure obviously) will be executed during detection and is expected to return a boolean, where true will trigger the environment.

Milieu will also pass an instance of itself as a parameter to the closure, so you can e.g. do device detection in your closure.

    Milieu::$environments = array(
        'mobile' => array('m.example.com', (function($milieu){
            return $milieu->device()->isMobile();
        }))
    );

<a name="usage"></a>
## Usage

It's time to initiate our Milieu instance.

    $milieu = new Milieu();