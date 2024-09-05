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

