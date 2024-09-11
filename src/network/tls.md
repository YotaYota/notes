# TLS

TLS (formerly SSL).

TLS 1.3 [RFC 8446](https://datatracker.ietf.org/doc/html/rfc8446).

TLS 1.3 is a hybrid cryptosystem

* TLS Handshake with assymetric encryptions, using ephemeral Diffie-Hellman.
* Use session keys (symmetric encryptions) for the rest of the session

**Note**: Symmetric encryption is much faster than assymetric.

## Handshake

1. **Client hello**: protocol version, "client random" (string of random bytes), list of cipher suites. Also parameters for calculating the premaster secret.
2. **Server generates master secret**: creates the master secret using client random, cipher suites and a server random.
3. **Server hello and "Finished"**: the server hello includes the server's certificate, digital signature, server random and chosen cipher suite. Also sends "Finished".
4. **Final steps and client "Finished"**: client verifies signature and certificate, generates master secret. Sends "Finished".

0-RTT mode for session resumption is also supported.

## Certificate Signing Request (CSR)

To obtain an SSL certificate from a Certificate Authority (**CA**), you must generate a Certificate Signing Request (**CSR**).

The information provided in the CSR is called Distinguished Name (**DN**). One of the fields in DN is Common Name (**CN**), which should be the FQDN.

CSR -> CA = SSL certificate

### Generate Private Key and CSR

To create a new key and a CSR with `openssl`:

```bash
openssl req \
    -newkey rsa:2048 \  # 2048-bit RSA key
    -nodes \            # private key not encrypted with pass phrase
    -keyout domain.key \
    -out domain.csr
```

**Note**: `-new` is implied, which indicates that a CSR should be generated.

### Generate CSR from Private Key

To create a CSR from existing key

```bash
openssl req \
    -key domain.key \   # existing private key
    -new \              # generate CSR
    -out domain.csr
```

### Genereate CSR from Certificate and Private Key

```bash
openssl x509 \
    -in domain.crt \        # existing CSR
    -signkey domain.key \   # existing private key
    -x509toreq \            # using an X509 cert to make a CSR
    -out domain.csr
```

## Self-Signed Certificates

A _self-signed certificate_ is not signed by a CA. A self-signed certificate is a certificate that is signed with its own private key.

Self-signed certificates should only be used if you do not need to prove your service's identity to its users.

### Generate Self-Signed Certificate

```bash
openssl req \
    -newkey rsa:2048 \
    -nodes \
    -keyout domain.key \
    -x509 \                 # generates self-signed when together with req option 
    -days 365 \
    -out domain.crt         # temporary CSR
```

### Generate Self-Signed Certificate from Private Key

```bash
openssl req \
    -key domain.key \   # existing private key
    -new \
    -x509 \
    -days 365 \
    -out domain.crt
```

### Generate Self-Signed Certificate from Private Key and CSR

```bash
openssl x509 \
    -signkey domain.key \   # existing private key
    -in domain.csr \        # existing CSR
    -req \
    -days 365 \
    -out domain.crt
```

## Viewing Certificates

Certificate and CSR files are encoded in PEM format. `openssl` can be used to convert to and from other formats, such as DER, PKCS7, PKCS12.

To view a CSR

```bash
openssl req -text -noout -verify -in domain.csr
```

To view a Certificate

```bash
openssl x509 -text -noout -in domain.crt
```

Verify a certificate was signed by a CA

```bash
openssl verify -verbose -CAFile ca.crt domain.crt
```

