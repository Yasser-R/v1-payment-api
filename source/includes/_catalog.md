# Catalog
```
      __...--~~~~~-._   _.-~~~~~--...__
    //               `V'               \\ 
   //                 |                 \\ 
  //__...--~~~~~~-._  |  _.-~~~~~~--...__\\ 
 //__.....----~~~~._\ | /_.~~~~----.....__\\
====================\\|//====================
                    `---`  
```

> Response: Full user with token that contains Send, Scheduled, and Requests.

```php
/**
 * Retrieve the catalog of endpoints that 
 * are available to the OAuth token which you just 
 * retrieved.
 */
print_r($OAuth->catalog("Your Access Token"));
```
```ruby
# Retrieve the catalog of endpoints that are 
# available to the OAuth token passed in
[ Dwolla::OAuth.catalog("Another access token")
```
```python
# Retrieve the catalog of endpoints that
# are available to the current OAuth token!

print(oauth.catalog())
```
```js
// Get links to the endpoints elligible for use with
// the passed OAuth token
Dwolla.catalog("Access token here!", function(error, links) {
    console.log(links);
});
```
```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {},
    "_links": {
        "self": {
            "href": "http://uat.dwolla.com/oauth/rest/catalog"
        },
        "send": {
            "href": "http://uat.dwolla.com/oauth/rest/transactions/send"
        },
        "createScheduled": {
            "href": "http://uat.dwolla.com/oauth/rest/transactions/scheduled"
        },
        "fulfill": {
            "href": "http://uat.dwolla.com/oauth/rest/requests/{request_id}/fulfill",
            "templated": true
        }
    }
}
```

> Response: Direct user with token that contains Send, Scheduled, and Requests.

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {},
    "_links": {
        "self": {
            "href": "http://uat.dwolla.com/oauth/rest/catalog"
        },
        "sendWithoutPin": {
            "href": "http://uat.dwolla.com/oauth/rest/transactions/send"
        },
        "createScheduledWithoutPin": {
            "href": "http://uat.dwolla.com/oauth/rest/transactions/scheduled"
        },
        "fulfillWithoutPin": {
            "href": "http://uat.dwolla.com/oauth/rest/requests/{request_id}/fulfill",
            "templated": true
        }
    }
}
```

Fetch a list of endpoints available to the user. The Catalog endpoint is an initial step toward supporting [HAL](http://stateless.co/hal_specification.html).  

Currently, it only includes links to the Send, Create Scheduled, and Fulfill Request endpoints.  In the future, it will be extended to include all endpoints available to a user.

Use this endpoint to determine whether or not the user needs to provide a PIN in order to use the Send, Create Scheduled, or Fulfill Request endpoints.  Since Direct users do not have a PIN, for instance, the `sendWithoutPin` key will be included in `_links` instead of the regular `send` which you would see for Full users.  The URI remains the same in either case, but for any link with "WithoutPin" appended to it, you will not need to ask the user for their PIN and may omit the `pin` parameter when calling the endpoint.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token but does not require a particular scope.</aside>

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/catalog`