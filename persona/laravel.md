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
Since an identity is just an email address, and not an actual user in your application, it's impossible for the library to know how you manage your users. This section will tell you how the library works out of the box and how you can modify it trough [Laravel's event system](http://laravel.com/docs/events).

Out of the box the library will pretty much mimic Laravel's "Database" authentication driver. It will use the table defined in `app/config/auth.php` and use the column `email` to look for a user.

    DB::table(Config::get('auth.table'))->where('email', $email)->first();

If a user doesn't exist it will go on and create one for you through a simple insert query with the email address.

    DB::table(Config::get('auth.table'))->insert(array('email' => $email));

As you can imagine this is quite limited. What if you want to send a email during creation? Or maybe use your [Eloquent model](http://laravel.com/docs/eloquent) instead? This is where Laravel's event system come in handy.

The library fire two events during the login process, `persona.login` and `persona.register`, that you can use to override the default functionality. If you want to future-proof your application you may use the constants defined in `Mozilla\Persona\Provider\Laravel\PersonaUserProvider`.

    use Mozilla\Persona\Provider\Laravel\PersonaUserProvider;

    Event::listen(PersonaUserProvider::EVENT_LOGIN, function($email) { });
    Event::listen(PersonaUserProvider::EVENT_REGISTER, function($email) { });

The first event, `persona.login` or `EVENT_LOGIN`, is fired when we're trying to find a user. It passes one parameter to the method, the email address that identifies the current user. Your subscriber is expected to return either `null`, if no user where found, or an instance of `Illuminate\Auth\UserInterface` if one was.

A basic implementation using Eloquent would look like this. Please note that the object you return (whether you're using Eloquent or not) must implement `Illuminate\Auth\UserInterface`  and `getAuthIdentifier()` must return the user's email address.

    Event::listen('persona.login', function($email) {
        return User::where('email', $email)->first();
    });

The other event, `persona.register` or `EVENT_REGISTER`, is fired when we couldn't find the user and must create a new one. Just like the first event it passes one parameter to the method, the email address that identifies the current user, and once again you're expected to return an instance of `Illuminate\Auth\UserInterface` - this time if you successfully created a new user.

A basic implementation using Eloquent:

    Event::listen('persona.register', function($email) {
        $user = new User;
        $user->email = $email;
        $user->save();
        return $user;
    });
