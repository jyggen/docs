# Overview

- [Introduction](#introduction)
- [Usage](#usage)
- [Public API](#public-api)
- [Extending](#extending)

<a name="introduction"></a>
## Introduction

`Dispatcher` is basically just a wrapper around the `curl_multi_*` functions, so any cURL code you use today can easily be ported to this class.

<a name="usage"></a>
## Usage

This class is used to execute multiple requests in parallel (commonly referred to as threaded requests, which isn't really true). To use `Dispatcher` you first have to create a couple of requests, and after we've done that we create a new dispatcher, add our requests and execute them.

    $req1 = new Request('http://example.com');
    $req2 = new Request('http://example.org');
    $req3 = new Request('http://jyggen.com');

    $dispatcher = new Dispatcher();

    $dispatcher->add($req1);
    $dispatcher->add($req2);
    $dispatcher->add($req3);

    $dispatcher->execute();

    print $req1->getRawResponse();
    print $req2->getRawResponse();
    print $req3->getRawResponse();

<a name="public-api"></a>
## Public API

### `__construct(string $url)`

Create a new instance of `Dispatcher`.

### `add(RequestInterface $request)`

Add a request to the dispatcher. Requires one (1) parameter, an object implementing `RequestInterface`.

Returns the key assigned to the attached request.

### `clear()`

Remove all requests attached to the dispatcher.

### `execute()`

Execute all requests attached to the dispatcher.

### `get(int $key = null)`

Retrieve all requests attached to the dispatcher. Optionally you can specify the key of the request you want to retrieve.

Returns an array of `RequestInterface` objects or a single `RequestInterface` object depending on usage.

### `remove(int $key)`

Remove a request from the dispatcher. Requires one (1) parameter, the key of the request you want to remove.

## Extending

`Dispatcher` implements `DispatcherInterface` and should be relatively easy to extend and interact with the other classes.