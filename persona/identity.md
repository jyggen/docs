# Overview

- [Introduction](#introduction)
- [Public API](#public-api)

<a name="introduction"></a>
## Introduction

`Identity` is basically a container for the assertion string and it's data once it's verified.

<a name="public-api"></a>
## Public API

### `__construct($assertion)`

Create a new instance of `Identity`. Requires one (1) parameter, the assertion this instance ìdentifies.

### `getAssertion()`

Retrieve the assertion identified by the instance.

### `getAudience()`

Retrieve the audience of this identity. Only available after the identity has successfully been verified.

### `getEmail()`

Retrieve the email of this identity's owner. Only available after the identity has successfully been verified.

### `getExpires()`

Retrieve the time this identity expires. Only available after the identity has successfully been verified.

### `getIssuer()`

Retrieve the issuer of this identity. Only available after the identity has successfully been verified.

### `getReason()`

Retrieve the reason why this identity is invalid. Only available after the identity has successfully been verified.

### `getStatus()`

Retrieve the status of this identity. Only available after the identity has successfully been verified.

### `isInvalid()`

If this identity is invalid or not. Only available after the identity has successfully been verified.

### `isValid()`

If this identity is valid or not. Only available after the identity has successfully been verified.

### `isVerified()`

If this identity has been verified through a verifier.

### `parse(stdClass $response)`

Parses the response from the verifier. Shouldn't really be called directly, let the verifier do its thing.
