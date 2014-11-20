fxa-keyserver
=============

Public key server for FxA


# API Endpoints

* Account
    * [POST /v1/<email>/keys/<fingerprint> (:lock: keyFetchToken) (verf-required)](#post-v1keyscreate)
    * [GET  /v1/<email>/keys](#get-v1accountkeys)


## POST /v1/<email>/keys/<name>

Adds a key to the specified FxA email.

___Parameters___

* email - the primary email for this account
* fingerprint - the fingerprint of the key

The body contains the raw GPG public key.

A key can be attached to several e-mails, but it has to be linked through
a POST call for each e-mail.

### Request

```sh
curl -v \
-X POST \
-H "Content-Type: application/json" \
"https://host/v1/tarek@mozilla.com/keys/65E4-0D24-CEF9-5E85-DF41-6484-D260-D234-9E58-FF96" \
-H 'Authorization: Hawk id="d4c5b1e3f5791ef83896c27519979b93a45e6d0da34c7509c5632ac35b28b48d", ts="1373391043", nonce="ohQjqb", mac="LAnpP3P2PXelC6hUoUaHP72nCqY5Iibaa3eeiGBqIIU="' \
-d '
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1

mQINBFRf+wcBEADirtAVu9DEMifLfZwmez459hscKk4Cp8a3XQ0dh2I47As4zd6I
FONkxOUCLc+sy9O742Qu1WhkaJyaTJeork7Prs9lPO0DTNvangGLfm10hZ19e1Ad
/mKTk31ZipWYId70bAVtk/JSbDrrIeVaGdE+Ttz2hrKlqVQDEQGA4Jspj3Dp/n2/
tgXvJMskUbO8n3nsXznqTLOEeweDR7NQQz3Gv691DovL9hvwFPau3NmJWcJPCwc2
TibjWWN7342
...
=EVSy
-----END PGP PUBLIC KEY BLOCK-----
'
```

### Response

Successful requests will produce a "200 OK" response with the FxA account's unique identifier in the JSON body:

```json
{
  "uid": "4c352927cd4f4a4aa03d7d1893d950b8"
}
```

## GET /v1/<email>/keys

Returns a list of all public keys associated with a e-mail, sorted by date.


### Request

```sh
curl -v \
-X GET \
-H "Content-Type: application/json" \
https://host/v1/tarek@mozilla.com/keys
```

### Response

Successful requests will produce a "200 OK" response with the FxA account's list of fingerprint keys
order by creation date.

```json
{
  "keys": [
    {"createdAt": 1392144866, 
     "fingerprint": "65E4-0D24-CEF9-5E85-DF41-6484-D260-D234-9E58-FF96"
     }
  ]
}
```

## GET /v1/<email>/keys/<fingerprint>

Returns the public key associated with the e-mail.


### Request

```sh
curl -v \
-X GET \
-H "Content-Type: application/json" \
https://host/v1/tarek@mozilla.com/keys/65E4-0D24-CEF9-5E85-DF41-6484-D260-D234-9E58-FF96
```

### Response

Successful requests will produce a "200 OK" response with the raw GPG key in the body.

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1

mQINBFRf+wcBEADirtAVu9DEMifLfZwmez459hscKk4Cp8a3XQ0dh2I47As4zd6I
FONkxOUCLc+sy9O742Qu1WhkaJyaTJeork7Prs9lPO0DTNvangGLfm10hZ19e1Ad
/mKTk31ZipWYId70bAVtk/JSbDrrIeVaGdE+Ttz2hrKlqVQDEQGA4Jspj3Dp/n2/
tgXvJMskUbO8n3nsXznqTLOEeweDR7NQQz3Gv691DovL9hvwFPau3NmJWcJPCwc2
TibjWWN7342
...
=EVSy
-----END PGP PUBLIC KEY BLOCK-----
```
