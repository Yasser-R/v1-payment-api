# Users

The API exposes methods for applications to look up users. Either [full](#get-full-account-info) or [basic](#get-basic-account-info) info is accessible. Full account information requires the `AccountInfoFull` scope to be set for the user's OAuth token. 

## Lookup Users By Location

This method helps retrieve Dwolla users near a location (lat and lon).

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Get users nearby a given geolocation
 **/
$lat = "40.708322";
$long = "-74.0147477";

$users = $Dwolla->usersNearby($lat, $long);
if(!$users) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($users); }
```
```python
'''
    EXAMPLE 1: 
      Fetch users near lat 45, lon 45
'''
me = Dwolla.get_nearby_users('45', '45')
print(me)
```
```ruby
# EXAMPLE 1: 
#   Fetch users near account associated with
#   the configured OAuth token
pp Dwolla::Users.nearby()
```
```js
// Example 1: Users near lat 45, lon 45
Dwolla.nearbyUsers('45', '45', function(err, data) {
    if (err) { console.log(err); }
    console.log(data);
});
```

`GET https://www.dwolla.com/oauth/rest/users/nearby?client_id={client_id}&client_secret={client_secret}`

| Parameter     | Optional? | Description                                              |
|---------------|-----------|----------------------------------------------------------|
| client_id     | no        | Consumer key for the application.                        |
| client_secret | no        | Consumer secret for the application.                     |
| latitude      | no        | Latitude coordinates (must be between -90.0 and 90.0)    |
| longitude     | no        | Longitude coordinates (must be between -180.0 and 180.0) |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Id": "812-734-7288",
            "Latitude": 40.708448,
            "Name": "Michael Schonfeld",
            "Longitude": -74.014429,
            "Delta": 114.72287700000001,
            "Image": "https://www.dwolla.com/avatars/812-734-7288"
        }
    ]
}
```

## Get Basic Account Info

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Fetch basic account information
 *   for a given Dwolla ID
 **/
$user = $Dwolla->getUser('812-546-3855');
if(!$user) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($user); } // Print user information
```
```ruby
# EXAMPLE 1: 
#   Fetch basic account information
#   for a given Dwolla ID
pp Dwolla::Users.get('812-626-8794')
```
```python
'''
    EXAMPLE 1: 
      Fetch basic account information
      for a given Dwolla ID
'''
me = Dwolla.get_account_info('812-626-8794')
print(me)
```
```js
/**
 * EXAMPLE 1: 
 *   Fetch basic account information
 *   for a given Dwolla ID
 **/

Dwolla.basicAccountInfo('812-546-3855', function(err, data) {
    if (err) { console.log(err); }
    console.log(data);
});
```

`GET https://www.dwolla.com/oauth/rest/users/{account_identifier}?client_id={client_id}&client_secret={client_secret}`

| Parameter          | Optional? | Description                                                                  |
|--------------------|-----------|------------------------------------------------------------------------------|
| client_id          | no        | Consumer key for the application.                                            |
| client_secret      | no        | Consumer secret for the application.                                         |
| account_identifier | no        | Dwolla ID or e-mail address associated with the account desired for lookup.  |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "812-111-1111",
        "Latitude": 41.584546,
        "Longitude": -93.634167,
        "Name": "Test User"
    }
}
```

## Get Full Account Info

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Fetch account information for the
 *   account associated with the provided
 *   OAuth token
 **/
$me = $Dwolla->me();
if(!$me) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($me); } // Print user information
```
```ruby
# EXAMPLE 1:
#   Fetch account information for the
#   account associated with the provided
#   OAuth token
pp Dwolla::Users.get
```
```python
'''
    EXAMPLE 1: 
      Fetch account information for the
      account associated with the provided
      OAuth token
'''
me = DwollaUser.get_account_info()
print(me)
```
```js
/**
 * EXAMPLE 1: 
 *   Fetch account information for the
 *   account associated with the provided
 *   OAuth token
 **/

Dwolla.fullAccountInfo(function(err, data) {
    if (err) { console.log(err); }
    console.log(data);
});
```

`GET https://www.dwolla.com/oauth/rest/users/?oauth_token={token}`

| Parameter   | Optional? | Description                                       |
|-------------|-----------|---------------------------------------------------|
| oauth_token | no        | A valid OAuth token with `AccountInfoFull` scope. |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "City": "Des Moines",
        "Id": "812-111-1111",
        "Latitude": 41.584546,
        "Longitude": -93.634167,
        "Name": "Test User",
        "State": "IA",
        "Type": "Personal"
    }
}
```
## Get Avatar

User avatars can also be retrieved using the API. This requires no authentication and will return a 220px x 220px PNG file.

### HTTP Request

`GET https://www.dwolla.com/avatars/{dwolla_id}`

| Parameter   | Optional? | Description                                       |
|-------------|-----------|---------------------------------------------------|
| dwolla_id   | no        | Target user's Dwolla ID 						  |