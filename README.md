fxa-keyserver
=============


# Firefox Accounts Key Server API

## Overview

### URL Structure

```
https://<server-url>/v1/<api-endpoint>
```

Note that:

- All API access must be over HTTPS
- The URL embeds a version identifier "v1"; future revisions of this API may introduce new version numbers.
- The base URL of the server may be configured on a per-client basis

### Authorization

Most endpoints that return user data require authorization from the [OAuth][]
server. After a bearer token is received for the user, you can pass it to these
endpoints as a header:

```
Authorization: Bearer 558f9980ad5a9c279beb52123653967342f702e84d3ab34c7f80427a6a37e2c0
```

Some endpoints may require certain scopes as well; these will be listed in each endpoint. The general scope `profile` automatically has all scopes for this server.

### Errors

Invalid requests will return 4XX responses. Internal failures will return 5XX. Both will include JSON responses describing the error.

Example error:

```js
{
  "code": 400, // matches the HTTP status code
  "errno": 101, // stable application-level error number
  "error": "Bad Request", // string description of error type
  "message": "Unknown client"
}
```

The currently-defined error responses are:

- status code, errno: description
- 403, 100: Unauthorized
- 400, 101: Invalid request parameter
- 400, 102: Unsupported image provider
- 500, 103: Image processing error
- 503, 104: OAuth service unavailable
- 500, 999: internal server error

## API Endpoints


- [GET /v1/search][search]
- [GET /v1/users/:uid/keys][keys]
- [GET /v1/users/:uid/key/:fingerprint][key]
- [POST /v1/users/:uid/key/:fingerprint (:lock: BearerToken)][post]
- [DELETE /v1/users/:uid/key/:fingerprint (:lock: BearerToken)][delete]


### GET /v1/search

- scope: `search`

Search for a user. 

__Parameters__

- **email** email to look for.


### GET /v1/users/:uid/keys

- scope: `keys`

Lists all keys for an uid - ordered by creation date.

### GET /v1/users/:uid/keys/:fingerprint

- scope: `key`

Retrieves a user's key

### GET /v1/users/:uid/keys/:fingerprint

- scope: `post`

Adds a key


### DELETE /v1/users/:uid/keys/:fingerprint

- scope: `delete`

Deletes a user's key


[keys]: #get-v1usersuidkeys
[key]: #get-v1usersuidkeysfingerprint
[delete]: #delete-v1usersuidkeysfingerprint
[post]: #post-v1usersuidkeysfingerprint
[OAuth]: https://github.com/mozilla/fxa-oauth-server

