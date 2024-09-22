# JWT

[RFC7519](https://datatracker.ietf.org/doc/html/rfc7519)

JWT is used for *Authorization* and is usually sent in the **Authorization** header as `Authorization: Bearer <token>`.

Contains 

- header (base64 encoded string of json object)
- payload (base64 encoded string of json object)
- signature (signed and digested)

in the format `header.payload.signature`.

Can be signed with a secret using **HMAC**, or a key pair using **RSA** or **ECDSA**. 

Different kinds of claims:

- public (should be defined in [IANA](https://www.iana.org/assignments/jwt/jwt.xhtml), or defined as a URI that contains a collision resistant namespace)
- private
- [registered](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1) (predefined and recommended)

The signature is created with a secret

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

## Simple Implementation

```
let header = { typ: 'JWT', alg: 'HS256'}
header = new Buffer(JSON.stringify(header)).toString('base64')
```

```
let payload = { iat: Date}
payload = new Buffer(JSON.stringify(payload)).toString('base64')
```


```
let key = header + '.' + payload
let signature = crypto.createHmac('sha356', 'secret_key')
signature.update(key)
key = signature.digest('base64')
```

The token is the header, payload and the signed digested key

```
let token = header + '.' + payload + '.' key
```
