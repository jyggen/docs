# Milieu [![Build Status](https://secure.travis-ci.org/jyggen/Milieu.png?branch=master)](https://travis-ci.org/jyggen/Milieu)

A lightweight environment initialization, detection and configuration library.

[Find Milieu on Packagist/Composer](https://packagist.org/packages/jyggen/Milieu)

## Usage

*This section assumes that you want to utilize all of Milieu's classes/features. For standalone usage, see the class' documentation.*

As with any Composer package, you must first include the autoloader. It's also recommended to import Milieu's namespace for quicker usage.

    require 'vendor/autoload.php';
    use Milieu\Milieu;

Let's get going! The first step is to define all your environments. This is done through the static variable `environments`.

    Milieu::$environments = array(
        'dev'  => array('localhost', '127.0.01'),
        'test' => array('test.example.com')
    );

Milieu is more than just static hostnames and IPs though, you can also put some wildcards and closures in there to make it more dynamic.

    Milieu::$environments = array(
        'dev' => array('dev.*.com', '192.168.*', (function(){ return (PHP_VERSION === 5); })),
    );

A cool thing about the support for closures is that, if you add a parameter, you can access the Milieu instance.

    Milieu::$environments = array(
        'mobile' => array('m.example.com', (function($milieu){ return $milieu->device()->isMobile(); })),
    );

## Documentation

Milieu is built upon four classes: `Configuration`, `Device`, `Environment` and `Milieu`.

### Milieu

`Milieu` is the main class and is basically a wrapper around the other three.

## About

Milieu is the word for environment in French, and, for hundreds of years, also in Dutch, German, Swedish, English, and other languages that were strongly influenced by French culture and French language, primarily during the 17th and 18th centuries.

### Requirements

* PHP 5.3 or above.
* PHPUnit to execute the test suite (optional).

### Author

Jonas Stendahl  
jonas.stendahl@gmail.com  
http://twitter.com/jyggen

[See the list of contributors here](https://github.com/jyggen/Milieu/contributors).

### License

Milieu is licensed under the MIT License - see the `LICENSE` file for details.