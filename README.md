# Chynge API

Chynge public API is at [http://chynge-api-doc-js.s3-website-ap-southeast-1.amazonaws.com](http://chynge-api-doc-js.s3-website-ap-southeast-1.amazonaws.com/)

## Developer's API key and secret

We send out API key and Secret key to developers manually. Please send
email to info@chynge.com to request for the keys.

The API key is to be used with a sandbox. The sandbox comes with a 
preloaded sample database.

When developer is ready to deploy to production, please send email
to info@chynge.com and we will give you a production API and Secret key.

## Authentication

Chynge API follows OAuth 2.0 specification for authentication. The API 
supports client credential grant to be used for developers created
middleware applications and authorization code grant for mobile and SPA
type applications.

In the past the API supported the use of resource owner credential grant.
It is now discouraged and will not be supported in the future. Applications
that have been using resource owner credential grant will continue to work.

### Authenticate through client credential grant

This section is intended as how to for developer's trusted middleware
application (middleware-app) to authenticate to Chynge API through client
credential grant.

To aquire an access_token middleware-appe sends a POST request to 
`api.chynge.com/v1/auth` with the following body parameters:
- `client_id`
- `client_secret`

The authorization server will respond with a JSON object containing
following properties:
- `token_type` with the value of 'Bearer'
- `expires_in` with an integer representing the TTL of the access token
- `access_token` is the access token itself

```
# middleware-app request
curl -X POST --header 'Content-Type: application/json' -d '{ \ 
   "client_secret": "tUSfmzgzV0LMsuwgxxxxxxxxxxxxxxxxxxxxxxxxx", \ 
   "client_id": "U3kTfxxxxxxxxxxxxxxx" \ 
 }' 'https://api.chynge.com/v1/auth'
 
Response from authentication server

```
 {
  "access_token": "eyJ0eXAxxxxxxxxxx.asdfasdfxxxxxxxxxx.uiokdfhksjdxxxxxx",
  "expires_in": 86400,
  "token_type": "Bearer"
}
```

`middleware-app` can now use the returned `access_token` as the bearer token
to authenticate with Chynge API endpoints.

```
# request country list using access_token
curl -X GET --header 'Authorization: eyJ0eXAiOiJKV1QiLCxxxxxxxxxxxxxxxxx' 'https://api.chynge.com/v1/country'

# response
[
  {
    "country": "Indonesia",
    "countryCode": "ID"
  },
  {
    "country": "Malaysia",
    "countryCode": "MY"
  },
  {
    "country": "Singapore",
    "countryCode": "SG"
  }
]
```

When you're using our interactive API documentation, `access_token` is
the value that you want to copy to the api_key input in the 'Authorize button'
before you do the 'try it out'.
