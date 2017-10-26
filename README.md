# Libryo API Oauth2 docs

This is some basic documentation for Libryo partners to integrate their application with the Libryo API using Oauth2.

## Request Authorization
To request an access token to communicate with the API, a Libryo user needs to give your application approval to communicate with the Libryo API on their behalf.

The following is the endpoint for your app to send users to, to authorize your application:

https://my.libryo.com/oauth/authorize

The query parameters that need to be passed along with this request are:
* `client_id` - The client ID provided to you by Libryo
* `redirect_uri` - The redirect URI that your application is registered to redirect to after the user has approved/rejected the auth request
* `response_type` - This should be `code`, in order for you to receive an authorization code, with which an access token can be generated
* `scope` - The scopes that are currently available are `view-legal-report`, `search-legislation`, `list-libryos`


### PHP Example
```
$queryParams = http_build_query([
    'client_id' => 'client-id',
    'redirect_uri' => 'http://your-app.com/callback',
    'response_type' => 'code',
    'scope' => 'view-legal-report search-legislation list-libryos',
]);

$url = 'https://my.libryo.com/oauth/authorize?' . $query;
```

### Python Example
```
from urllib import urlencode
queryParams = {
    'client_id': 'your-client-id',
    'redirect_uri': 'http://your-app.com/callback',
    'response_type': 'code',
    'scope': 'view-legal-report search-legislation list-libryos',
}
url = 'https://my.libryo.com/oauth/authorize?' + urlencode(queryParams)

```

If the user is not logged into Libryo, they will need to log in first in order to approve the authorization request. Once logged in, they will be redirected back to the above URL.


## Redirect URI
Once the user approved/rejected your authorization request, they will be redirected back to your redirect URI.
If the user accepted the auth request, Libryo passes an authorization code as a query parameter to your redirect URI.

eg.
http://your-app.com/callback?code=the-authorization-code

If the user rejected your authorization request, Libryo passes an error as a query paramater.
eg.
http://your-app.com/callback?error=access_denied


## Request Access Token
Now that your application has an authorization code, you can use it to request an access token for the user:

### PHP Example
```
$code = $request->input('code');
$httpClient = new GuzzleHttp\Client;

$response = $httpClient->post('https://api.libryo.com/api/v1/oauth/token', [
    'form_params' => [
        'grant_type' => 'authorization_code',
        'client_id' => 'your-client-id',
        'client_secret' => 'your-client-secret',
        'redirect_uri' => 'http://your-app.com/callback',
        'code' => $code,
    ],
]);
```






