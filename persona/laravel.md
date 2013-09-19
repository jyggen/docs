# Overview

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)
- [Authentication Driver](#authentication)

<a name="introduction"></a>
## Introduction

This library comes packaged with a service provider and an authentication driver for Laravel 4. The authentication driver allows you to seamlessly integrate Persona into Laravel's built-in authentication library.

<a name="installation"></a>
## Installation

Install it like any other package and after you've done a `composer update` you'll have to add the library's service provider (`Mozilla\Persona\Provider\Laravel\PersonaServiceProvider`) to the end of the providers array in `app/config/app.php`:

    'providers' => array(

        'Illuminate\Foundation\Providers\ArtisanServiceProvider',
        'Illuminate\Auth\AuthServiceProvider',
        ...
        'Illuminate\View\ViewServiceProvider',
        'Illuminate\Workbench\WorkbenchServiceProvider',
        'Mozilla\Persona\Provider\Laravel\PersonaServiceProvider'

    ),

<a name="usage"></a>
## Usage

The service provider bind two classes to Laravel's IoC container `persona.identity` and `persona.verifier`.

    $identity = App::make('persona.identity', Input::get('assertion'));
    $verifier = App::make('persona.verifier');

    $verifier->verify($identity);

Note that you don't have to (and shouldn't) provide an audience, the verifier will automatically detect it through the `Request` object.

<a name="authentication"></a>
## Authentication Driver
To use Laravel's authentication system with Persona all you have to do is to change the driver to `persona` in `app/config/auth.php`. By default the driver will use the table specified in the key `table`, so change it as well if necessary.

[More information on how to use Laravel's authentication](http://laravel.com/docs/security#authenticating-users).

### Advanced Usage

Persona is a bit special since there's not register functionality
After you've done this you also have to create an event observer that listens to `persona.login`.

    Event::listen('persona.login', function($identity) {
        /* code here */
    });

Why? Because

