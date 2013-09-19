# Overview

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)

<a name="introduction"></a>
## Introduction

This library is a framework agnostic implementation of Mozilla Persona, the cross-browser login system for the Web that's easy to use and easy to deploy. It is installed through [Composer](http://getcomposer.org/) and you can [find it on Packagist here](https://packagist.org/packages/Mozila/Persona).

<a name="installation"></a>
## Installation

As with any Composer package, you must first include Composer's autoloader. It's also a good idea to alias/import the namespaces for quicker usage.

    require 'vendor/autoload.php';
    use Mozilla\Persona\Identity;
    use Mozilla\Persona\Verifier;

<a name="usage"></a>
## Usage
When you log in with Persona you receive an assertion which you're supposed to send to your backend for verification. In order to verify the assertion you have to instantiate an indentity for the assertion.

    $identity = new Identity($_POST['assertion']);

Now that you have an identity you have to verify it through the verifier to make sure it's a valid assertion:

    $verifier = new Verifier('http://example.com');

    $verifier->verify($identity);

Your identity is now verified and you have "unlocked" new methods on your `Identity` object:

    // If the identity is valid return the user's email address.
    if ($identity->isValid()) {
        return $identity->getEmail();
    // Otherwise throw an exception with the reason why the assertion isn't valid.
    } else {
        throw new Exception('Error: '.$identity->getReason());
    }
