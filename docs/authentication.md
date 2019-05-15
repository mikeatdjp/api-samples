# Digital DJ Pool Authentication

DigitalDJPool uses the OAuth 2.0 protocol for authentication and authorization.

## The Basics

OAuth defines four specific roles or parties involved in the various "flows":

* **Resource Owner**: The end user. It's the party that owns the data and has the power to allow third parties to access that data or resource.
* **Resource Server**: The services and APIs providing data. It trusts the Authorization Server to securely authenticate and authorize the OAuth Client, and uses Bearer access_tokens to ensure that access to a resource can be granted.
* **Client**: Your application identified by its application ID. The OAuth client is usually the party that the end user interacts with, and it requests tokens from the authorization server. The client must be granted permission to access the resource by the resource owner.
* **Authorization Server**: The endpoint responsible for ensuring the user's identity, granting and revoking access to resources, and issuing tokens. The authorization server, or identity provider, securely handles the user's information, their access, and the trust between parties in a flow.

## App Registration

Applications that intend to use Digital DJ Pool's OAuth endpoint must be registered with Digital DJ Pool. The registration process will collect and/or assign values for your application including:

* An Application, or Client, ID
* A Client Secret, which is similar to a password and should be kept secure
* A Redirect URI that is used for directing users back to your application

The app registration process is currently by invite only. For more details on creating your own application please reach out to Digital DJ Pool.

## Endpoints

Once your application is registered, it will communicate with Digital DJ Pool by sending HTTP requests to our OAuth endpoint:

* https://digitaldjpool.com/auth
* https://digitaldjpool.com/token

## OAuth Flows

We support common OAuth grant types, or flows, that correspond to a specific authentications scenario. The supported grants, or flows, are:

* [Authorization Code Grant](auth-code-grant.md)
* [Authorization Code PKCE Grant](auth-code-pkce-grant.md)
* [Resource Owner Password Credentials Grant](auth-resource-owners-credentials-grant.md)

## Tokens

A token is returned to the client with an OAuth request. An example would be:

```
{
  access_token: h34FQbl3SDF,
  refresh_token: FD#!GBsdbvWW
}
```

## API Scopes

Scopes let you specify what type of access your application requires. Scopes limit access for the OAuth tokens issued. Requested scopes are displayed to the user on the authorization form.

### Available Scopes

| Name  | Description                                                        |
| ----- | ------------------------------------------------------------------ |
| user  | Grants read & write access to the profile information of the user. |
| crate | Grants read & write access to the user's crates.                   |
| file  | Grants read & write access to the user's files.                    |

> Note: Your OAuth App requests scopes in the initial authentication step as documented in one of the available flows above. You can specify multiple scopes by separating them with a space.
