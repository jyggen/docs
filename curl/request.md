# Overview

- [Introduction](#introduction)
- [Usage](#usage)
- [Public API](#public-api)
- [Extending](#extending)

<a name="introduction"></a>
## Introduction

`Request` is basically just a wrapper around the `curl_*` functions, so any cURL code you use today can easily be ported to this class.

<a name="usage"></a>
## Usage

The `Request` class is the main component of the library and it's used to make requests (unbelievable, I know).

To retrieve an URL, simply create a new request by specifying the URL and execute it (don't forget the namespace if you're not aliasing it):

    $request = new Request('http://example.com/');
    $request->execute();
    $response = $request->getResponse();

Of course you could easily use the [static helpers](/curl/overview) to do something like this, so let's look at some more advanced stuff.

    $request = new Request('http://example.com');

    $request->setOption(CURLOPT_FOLLOWLOCATION, true);
    $request->setOption(CURLOPT_POST, true);
    $request->setOption(CURLOPT_POSTFIELDS, ['username' => 'jyggen', 'password' => 'password']);
    $request->execute();

    if ($request->isSuccessful()) {
        return $request->getRawResponse();
    } else {
        throw new Exception($resquest->getErrorMessage());
    }

It's pretty self-explanatory, right? This will execute a request through POST towards example.com and follow any 3xx redirects. If the request was successful we'll return the raw response retrieved (not an instance of `Response`), otherwise we'll throw an exception with the error cURL encountered.

<a name="public-api"></a>
## Public API

### `__construct(string $url)`

Create a new instance of `Request`. Requires one (1) parameter, the URL the class should request.

### `getErrorMessage()`

Retrieve the last error message returned by the cURL handle. If no error message has been returned it will return `null`.

Internally it uses `curl_error()`.

### `getHandle()`

Retrieve the cURL handle attached to the request. The handle is automatically created with `curl_init()` and attached during the class construct.

### `getInfo(int $key = null)`

Get information regarding the request. Requires one (1) optional parameter, a `CURLINFO_*` constant which represent what information you want. Default, or if `null` is passed, it will return all available information.

Internally it uses `curl_getinfo()`.

### `getRawResponse()`

Retrieve the response exactly how it was returned from the request. Will return null if the request hasn't been executed yet.

### `getResponse()`

Retrieve a `Response` object based on the raw response. Will return null if the request hasn't been executed yet.

### `setOption(mixed $option, mixed $value = null)`

Set one or multple options for the request. It works like a combined version of `curl_setopt()` and `curl_setopt_array()`. You can either supply a `CURLOPT_*` constant and a value as two parameters, or a single parameter with an array of options with the constant as key and value as value.

The class got two (2) protected default values that can't be changed to ensure that the class works as intended. These options are `CURLOPT_RETURNTRANSFER` and `CURLOPT_HEADER`, which are both set to `true`.

Internally it uses `curl_setopt()`.

### `addMultiHandle($multiHandle)`

Attached the request to a cURL multi handle which allows you to execute this request in parallel with other requests. As long as you use the included `Dispatcher` class you should never have to use this method ever since it will set this up automatically.

Internally it uses `curl_multi_add_handle()`.

### `execute()`

This method is somewhat magical and doesn't really live up to its name when you're attached to a multi handle. During a "normal" request it will execute the request and retrieve the content, but on a request attached to a mutli handle **it will only retrieve the content**. This is due to the fact that the request has already been executed in the dispatcher, so executing it again would be pretty stupid.

Internally it uses curl_exec()` **or** `curl_multi_getcontent()`.

### `hasMulti()`

Returns a boolean whether or not a multi handle has been attached to the request.

### `isExecuted()`

Returns a boolean whether or not the request has been executed. Will be somewhat misleading if you attach more than one multi handle to the request.

### `isSuccessful()`

Returns a boolean whether or not the last request was successful or not.

### `removeMultiHandle()`

Remove an attached multi handle from the request. As long as you use the included `Dispatcher` class you should never have to use this method ever since it will handle this automatically.

Internally it uses `curl_multi_remove_handle()`.

## Extending

`Request` implements `RequestInterface` and should be relatively easy to extend and interact with the other classes.