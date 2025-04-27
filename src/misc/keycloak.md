# KeyCloak

Identity and Access Management.

Integrates with OpenID Connect, OAuth 2.0 and SAML.

Can be used as Identity Provider.

- Realm
- User
- Client (represent an application that can request authentication on behlaf of a user)

## OICD Login

Account management for individual user: `htts://${URL}:8080/realms/${realm}/account`.

When signing in a request is made to `htts://${URL}:8080/realms/${realm}/protocol/openid-connect/token` which will aquire a token.

`grant_type` can have different values, eg `refresh_token`.
