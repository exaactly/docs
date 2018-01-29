# Exaactly JS Web SDK

The Exaactly JS SDK is a work in progress, pre-release solution that allows 3rd party integration partners to access an Exaactly user's list of locations.

## Usage

### Acquiring the SDK

In proper releases, the SDK will be made available through NPM packages and as a standalone distributed package, but until such a release, please ask for the newest version from the Exaactly developers.

In order to use the SDK, you must apply for the proper credentials, and you will need to have a Client ID and a Client Secret to proceed.

## Authorization

The SDK uses the OAuth2 protocol for authorizing a retailer / carrier partner to access a user's information.

### Types of authorization

#### Implicit Flow (currently unavailable)
Your app does not use a server, such as a client-side JavaScript app or mobile app. This approach does not require a server that must make requests to the API.

#### Authorization Code Flow
Your application uses a server, can securely store a client secret, and can make server-to-server requests.

### Acquiring the access token (Authorization Code Flow)

#### Navigate the user to `[/authorize]` with the following query parameters: 
    
| Parameter | Value |
|-----------|--------|
|client_id | Your application's client id |
|redirect_uri | The url to redirect to after a successful authorization |
|response_type | `code` - There aren't any other types of flows at the moment |
|scope | `profile` |
    
#### Once the user has successfully authorized your application, they will be redirected to the `redirect_uri` endpoint of your application, with a `code` query parameter.
e.g. `http://example.com/connected?code=ABCDEFF1235243`
    
#### Using that `code` in your backend, you must construct a `POST` request to `[/token]` with the following

##### Headers

| Header | Value |
|-------|-------|
| Authorization | A Base 64 encoded version of the `<ClientID>:<ClientSecret>` string |
| Content-Type | application/x-www-form-urlencoded |
| Accept-Encoding | gzip |
    
##### Payload
    
| Parameter | Value |
|-----------|-------|
| code | The access code you've received in step 2 |
| grant_type | `authorization_code` |
| redirect_uri | The same `redirect_uri` that you used in step 1 |

#### As a response to the post, you will receive a JSON structure with the authorization details, as follows:

```json
{
   "access_token": "<user access token>",
   "refresh_token": "<refresh token>",
   "expires_in": <number of seconds until the token expires>,
}
```
    
### Using the Refresh Token
A Refresh Token is a special kind of token that can be used to obtain a renewed access token - that allows accessing a user's Exaactly data - at any time. You can request new access tokens until the refresh token is blacklisted. Refresh tokens **must be stored securely** by an application because they essentially allow a user to remain authenticated forever.

#### You must construct a `POST` request to `[/token]` with the following

##### Headers

| Header | Value |
|-------|-------|
| Authorization | A Base 64 encoded version of the `<ClientID>:<ClientSecret>` string |
| Content-Type | application/x-www-form-urlencoded |
| Accept-Encoding | gzip |
    
##### Payload
    
| Parameter | Value |
|-----------|-------|
| refresh_token | The refresh_token you've received previously |
| grant_type | `refresh_token` |

#### As a response to the post, you will receive a JSON structure with the authorization details, as follows:

```json
{
   "access_token": "<user access token>",
   "refresh_token": "<refresh token>",
   "expires_in": <number of seconds until the token expires>,
}
```

## SDK Functions

### Connecting to Exaactly

Once you've obtained an access token, you can initialize the SDK by:

```javascript
var exaactly = new ExaactlySDK(accessToken);
```

### listLocations(callback) - Listing the user's locations
A method that returns the connected user's Exaactly locations. It expects a callback as its only parameter. The callback will receive an array of locations.

```javascript
exaactly.listLocations(function(locations) {
    console.log(locations);
});
```
