# Let's Encrypt

2 steps

- the agent proves to the CA that the web server controls a domain.
- the agent can request, renew, and revoke certificates for that domain.

certbot is a utility that automates Let's Encrypt challanges and certificate handling.

## Domain Validation

Let’s Encrypt identifies the server administrator by public key.

The Let’s Encrypt CA will look at the domain name being requested and issue one or more sets of challenges:

- Provisioning a DNS record, or
- Provisioning an HTTP resource under a well-known URI

Let’s Encrypt CA also provides a nonce that the agent must sign with its private key pair to prove that it controls the key pair.

 Once the agent has completed these steps, it notifies the CA that it’s ready to complete validation.

 The CA verifies the signature on the nonce, and it attempts to download the file from the web server and make sure it has the expected content.

 The agent is then authorized to do certificate management for that domain.

 **Note**: The key pair used by the agent is calleda an *authorized key pair* for the domain.

## Obtain Certificate

The agent constructs a *PKCS#10* Certificate Signing Request ([RFC 2986](https://www.rfc-editor.org/rfc/rfc2986)) for Let's Encrypt CA to issue a certificate for the domain with a specified public key.

The CSR includes a signature by the private key corresponding to the public key in the CSR.

The agent also signs the whole CSR with the authorized key for the domain.

Let's Encrypt CA verifies both signatures and issues a certificate with the public key provided in the CSR.
