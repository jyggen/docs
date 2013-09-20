# Verifier

- [Introduction](#introduction)
- [Public API](#public-api)

<a name="introduction"></a>
## Introduction

`Verifier` is used to verify an identity's assertion against a remote verifier, usually Mozilla's Verifier API.

<a name="public-api"></a>
## Public API

### `const ENDPOINT`

This constant contains the URL to Mozilla's Verifier API. This endpoint is used unless another one is specified in the constructor.

### `__construct($audience, $endpoint = self::ENDPOINT)`

Create a new instance of `Verifier`. Requires one (1) parameter, audience*, and have one (1) optional parameter which specifies the API endpoint to verify against. Default is to use Mozilla's Verifier API.

*\* The protocol, domain name, and port of your site. For example, "https://example.com:443".*

### `getAudience()`

Retrieve the audience used by the verifier.

### `getEndpoint()`

Retrieve the endpoint used by the verifier.

### `verify(Identity $identity)`

Verify an `Identity` against the endpoint. Requires one (1) parameter, the identity you want to verify. This method will make a POST request through cURL against the endpoint used by the identifier. When the request is executed it will decode the JSON response and pass it to `Identity::parse()`.
