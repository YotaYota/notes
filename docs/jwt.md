# JWT

Contains 

- header (base64 encoded string of json object)
- payload (base64 encoded string of json object)
- key (signed and digested)

```
let header = { typ: 'JWT', alg: 'HS256'}
header = new Buffer(JSON.stringify(header)).toString('base64')
```

```
let payload = { iat: Date}
payload = new Buffer(JSON.stringify(payload)).toString('base64')
```

Different kinds of claims:

- public
- private
- reserved

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
