# Auth Code Grant 

## Usages

The OAuth 2.0 authorization code flow is described in [section 4.1 of the OAuth 2.0 specification](https://tools.ietf.org/html/rfc6749#section-4.1). It is used to perform authentication and authorization typically in server-side web applications where the client_secret can be securely stored and kept safe. The flow enables apps to securely acquire access_tokens that can be used to access secured resources. 

## Flow Diagram

<img src="./AuthCodeGrantDiagram.png" />

<!--
Flow Sequence:
Native App->/auth: Pops a browser dialog. \nRequests authorization code.
Native App->/auth: User completes policy
/auth->Native App: Returns authorization code
Native App->/token: Requests an OAuth berer token
/token->Native App: Returns an access token and a refresh token
Native App->/api:Calls API with access token in Authorization header
/api->/api:Validate access token
/api->Native App:Returns data to app
alt Access token expires
end
Native App->/token: Request a new token using refresh_token
/token->Native App: Returns a new access token
Native App->/api:Calls API with access token in Authorization header
-->

## Request Authorization Code

Begin the authorization code flow by having your registered application direct the user to the https://digitaldjpool.com/auth endpoint in their browser.

```
https://digitaldjpool.com/auth?
client_id=[YOUR-CLIENT-ID]
&response_type=code
&state=123456
&redirect_uri=https://127.0.0.1/yourapp
&scope=crates
```

| Parameter       | Required    | Description                                                                                                                                                                                                                                                                                                                                                                                          |
| --------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `client_id`     | yes         | The unique application (client) ID provided to you from Digital DJ Pool                                                                                                                                                                                                                                                                                                                              |
| `response_type` | yes         | Must include code for the authorization code flow.                                                                                                                                                                                                                                                                                                                                                   |
| `state`         | recommended | A value included in the request that will also be returned in the token response. It can be a string of any content that you wish. A randomly generated unique value is typically used for preventing cross-site request forgery attacks. The value can also encode information about the user's state in the app before the authentication request occurred, such as the page or view they were on. |
| `redirect_uri`  | yes         | The redirect_uri of your app, where authentication responses can be sent and received by your app. It must exactly match one of the redirect_uris you registered in the portal, except it must be url encoded.                                                                                                                                                                                       |
| `scope`         | yes         | A comma-separated list of scopes that you want the user to consent to.                                                                                                                                                                                                                                                                                                                               |

At this point the user is presented with a page on DigitalDJPool.com that will allow the user to authenticate. Once authenticated the user will be prompted to grant access to your app for the requested scopes. The user is then sent back to your application at the configured `redirect_uri` with the specific one-time-use Auth Code in the querystring.

### Successful Response

```
GET https://127.0.0.1/yourapp?code=ABC123FFABD&state=123456
```

| Parameter | Description                                                                                                                                                                                                            |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `code`    | The authorization code that the app requested. The app can use the authorization code to request an access token for the target resource. Authorization codes are very short lived, typically expiring within minutes. |
| `state`   | If a state parameter is included in the request, the same value should appear in the response. The app should verify that the state values in the request and response are identical.                                  |

## Request Access Token

Once an authorization code has been obtained an access_token must be obtained. Do this by sending a POST request to the /token endpoint. This request must provide the `client_id` and `client_secret` in one of two ways.

### Authorization Header

Make a request that contains an Authorization header that is composed of your client_id as the username and your client_secret as your password.

```
POST /token HTTP/1.1
Host: https://digitaldjpool.com
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [BASE64 Encoded (`client_id`:`client_secret`)]

grant_type=authorization_code
&code=ABC123FFABD
&redirect_uri=https://127.0.0.1/yourapp

```

| Parameter      | Required | Description                                                                  |
| -------------- | -------- | ---------------------------------------------------------------------------- |
| `grant_type`   | Yes      | Must be authorization_code for the authorization code flow.                  |
| `code`         | Yes      | The authorization code provided to your application in step one.             |
| `redirect_uri` | Yes      | The same redirect_uri value that was used to acquire the authorization code. |

### Request Body

Make a request that provides the `client_id` and `client_secret` in the body.

```
POST /token HTTP/1.1
Host: https://digitaldjpool.com
Content-Type: application/x-www-form-urlencoded


client_id=[YOUR-CLIENT-ID]
&client_secret=[YOUR-CLIENT-SECRET]
&grant_type=authorization_code
&code=ABC123FFABD
&redirect_uri=https://127.0.0.1/yourapp

```

| Parameter       | Required | Description                                                                  |
| --------------- | -------- | ---------------------------------------------------------------------------- |
| `client_id`     | Yes      | The unique application (client) ID provided to you from Digital DJ Pool      |
| `client_secret` | Yes      | The unique client secret provided to you from Digital DJ Pool                |
| `grant_type`    | Yes      | Must be authorization_code for the authorization code flow.                  |
| `code`          | Yes      | The authorization code provided to your application in step one.             |
| `redirect_uri`  | Yes      | The same redirect_uri value that was used to acquire the authorization code. |

### Successful Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "access_token":"jhr44jhfjkfs...",
  "token_type":"bearer",
  "expires_in":59,
  "refresh_token":"fdfr4frfrr"
}
```

| Parameter       | Description                                                                                                                                                                                                                                                                                                           |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `access_token`  | The requested access token. The application can now make requests to the DigitalDJPool API on behalf of that user, by sending along the Access Token you have received as a header of every request like Authorization: Bearer {AccessToken}                                                                          |
| `refresh_token` | An OAuth 2.0 refresh token. The app can use this token acquire additional access tokens after the current access token expires. Refresh tokens are long-lived, and can be used to retain access to resources for extended periods of time. For more detail on refreshing an access token, refer to the section below. |
| `token_type`    | Always set to Bearer.                                                                                                                                                                                                                                                                                                 |
| `expires_in`    | The number of seconds that access token is valid for.                                                                                                                                                                                                                                                                 |


## Use Access Token

The acquired access token should be included in the Authorization header for all subsequent API calls

```
GET /api/users/ApiDemoUser/crates
Host: https://digitaldjpool.com
Authorization: Bearer jhr44jhfjkfs...
```

## Refresh Access Token

Access tokens are short lived, and you must refresh them after they expire to continue accessing resources. Once the access token expires the DigitalDJPool API will respond with an Http Status Code of 401 (Unauthorized). Your web server should be able to intercept this response and then use the Refresh Token to obtain a new Access Token. You can do so by submitting another POST request to the /token endpoint, this time providing the refresh_token instead of the code. 

Refresh tokens do not have specified lifetimes. Typically, the lifetimes of refresh tokens are relatively long. However, in some cases, refresh tokens expire, are revoked, or lack sufficient privileges for the desired action. Your application needs to expect and handle errors returned by the token issuance endpoint correctly. Note that refresh tokens are not revoked when used to acquire new access tokens.

```
POST /token HTTP/1.1
Host: https://digitaldjpool.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
&client_id=[YOUR-CLIENT-ID]
&client_secret=[YOUR-CLIENT-SECRET]
&refresh_token=[YOUR-REFRESH-TOKEN]

```

| Parameter     | Required | Description                                                             |
| ------------- | -------- | ----------------------------------------------------------------------- |
| `grant_type`    | Yes      | Must be refresh_token for this step of the authorization code flow.     |
| `client_id`     | Yes      | The unique application (client) ID provided to you from Digital DJ Pool |
| `client_secret` | Yes      | The unique client secret provided to you from Digital DJ Pool           |
| `refresh_token` | Yes      | The refresh_token that you acquired in the second step of the flow.     |

### Successful Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "access_token": "jhr44jhfjkfs",
  "token_type":"bearer",
  "expires_in":59
}
```

| Parameter       | Description                                                                                                                                                                                                                                  |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `access_token`  | The requested access token. The application can now make requests to the DigitalDJPool API on behalf of that user, by sending along the Access Token you have received as a header of every request like Authorization: Bearer {AccessToken} |
| `expires_in`    | The number of seconds that access token is valid for.                                                                                                                                                                                        |
| `refresh_token` | Token used to acquire new access tokens and refresh tokens as documented below.                                                                                                                                                              |
