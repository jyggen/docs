# Response

- [Introduction](#introduction)
- [Public API](#public-api)

<a name="introduction"></a>
## Introduction

`Response` is an extension of `Symfony\Component\HttpFoundation\Response` and is used by `Request` to create the response of the request.

<a name="public-api"></a>
## Public API

For a complete list of the class' API, please see [the Symfony2 API documentation](http://api.symfony.com/2.3/Symfony/Component/HttpFoundation/Response.html).

### `static forge(RequestInterface $request)`

Forge a new instance of `Response` based on a `Request` instance. Requires one (1) parameter, an object implementing `RequestInterface`.
