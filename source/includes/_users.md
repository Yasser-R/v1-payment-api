# Users

```always
USERS!

 o   \ o /  _ o         __|    \ / 
/|\    |     /\   ___\o   \o    |  
/ \   / \   | \  /)  |    ( \  /o\ 

- "Tonja Hickey"
```

Dwolla users can be looked up via the API.

### Account Types

Type | Description
-------------|------------
Personal | Account belonging to an individual.  Default transaction limit of $5000.
Commercial | Merchant/business account.  Default transaction limit of $10000.
NonProfit | Account belonging to a non-profit organization. Default transaction limit of $10000.

## Get Basic Account Info

```php
/**
 *   Fetch basic account information
 *   for a given Dwolla ID
 **/

<?php

$Account = new Dwolla\Account();
$Account->settings->client_id = $apiKey;
$Account->settings->client_secret = $apiSecret;

$result = $Account->basic('812-121-7199');
var_export($result);
?>
```
```ruby
#   Fetch basic account information
#   for a given email address
puts Dwolla::Users.get('gordon@dwolla.com')
```
```python
# Fetch basic information for a user via their Dwolla ID
print(accounts.basic('812-202-3784'))
```
```js
/**
 *   Fetch basic account information
 *   for a given Dwolla ID (or email)
 **/

Dwolla.basicAccountInfo('812-546-3855', function(err, data) {
    console.log(data);
});
```

> If successful, you'll receive this response:

```php
array (
  'Id' => '812-121-7199',
  'Name' => 'David Stancu',
  'Latitude' => 0,
  'Longitude' => 0,
)
```

```ruby
{
  "Id"        => "812-742-3301",
  "Name"      => "Gordon Zheng",
  "Latitude"  => 0,
  "Longitude" => 0
}
```

```js
{
    "Id": "812-111-1111",
    "Latitude": 0,
    "Longitude": 0,
    "Name": "Joe Schmoe"
}
```

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

Retrieve basic information about **any** Dwolla user, given their Dwolla ID or email address.

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/users/{account_identifier}?client_id={}&client_secret={}`

`account_identifier` must be a Dwolla ID or email address.

#### Example

To lookup the user with email `gordon@dwolla.com`:

`GET https://www.dwolla.com/oauth/rest/users/gordon@dwolla.com?client_id={}&client_secret={}`

### Response

Parameter | Description
----------|-------------
Id | User's Dwolla ID
Latitude | If this is a business account, latitude of the account's location.  Otherwise, always `0` for individual accounts.
Longitude | If this is a business account, longitude of the account's location.  Otherwise, always `0` for individual accounts.
Name | Full account name (or business name for a business account)

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid account identifier. | Could not find an account with the specified ID. |

## Get Full Account Info

```php
/**
 *   Fetch account information for the
 *   account associated with the provided
 *   OAuth access token
 **/
<?php
$Account = new Dwolla\Account();
$Account->settings->oauth_token = "foo";

$result = $Account->full();

var_export($result);
?>
```
```ruby
#   Fetch account information for the
#   authorized user
puts Dwolla::Users.get
```
```python
# Print FULL account information for the user associated with the currently set OAuth token.
print(accounts.full())
```
```js
/**
 *   Fetch account information for the
 *   authorized user
 **/

Dwolla.fullAccountInfo(function(err, data) {
    console.log(data);
});
```

> If successful, you'll receive this response:

```ruby
{
  "City"      => "Test",
  "State"     => "NY",
  "Type"      => "Commercial",
  "Id"        => "812-742-8722",
  "Name"      => "Cafe Kubal",
  "Latitude"  => 41.58975983,
  "Longitude" => -93.61564636
}
```

```js
{
    "City": "Des Moines",
    "Id": "812-111-1111",
    "Latitude": 41.584546,
    "Longitude": -93.634167,
    "Name": "Test User",
    "State": "IA",
    "Type": "Personal"
}
```

```php
array (
  'City' => 'Test',
  'State' => 'NY',
  'Type' => 'Commercial',
  'Id' => '812-742-8722',
  'Name' => 'Cafe Kubal',
  'Latitude' => -1,
  'Longitude' => -1,
)
```

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

Retrieve information about the authorized user.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `AccountInfoFull` scope.</aside>

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/users/`

### Response

Parameter | Description
----------|-------------
Id | User's Dwolla ID
Latitude | If this is a business account, latitude of the account's location.  Otherwise, always `0` for individual accounts.
Longitude | If this is a business account, longitude of the account's location.  Otherwise, always `0` for individual accounts.
Name | Full account name (or business name for a business account)
City | User's home city
State | User's home state
Type | `Personal` or `Commercial`

## Get Avatar

```shell
GET https://www.dwolla.com/avatars/812-713-9234
```

Retrieve the avatar image for a Dwolla user, given their Dwolla ID.  This requires no authentication and will return a 220px x 220px PNG file.

### HTTP Request

`GET https://www.dwolla.com/avatars/{dwolla_id}`

## Find Nearby Businesses

```php
<?php
$Contacts = new Dwolla\Contacts();
$Contacts->settings->client_id = $apiKey;
$Contacts->settings->client_secret = $apiSecret;

$lat = "41.585";
$long = "-93.624";

$result = $Contacts->nearby($lat, $long);

var_export($result);
?>
```
```ruby
#   Get a list of nearby Dwolla spots
#   for a given set of coordinates (lat, long)
pp Dwolla::Contacts.nearby(41.58975983, -93.61564636)
```
```python
# Fetch Dwolla spots near 40.7127, 74.0059
print(contacts.nearby(40.7127, 74.0059))
```
```js
/**
 * Fetch all spots near lat 41.585 and 
 * long -93.624
 **/

Dwolla.nearby('41.585', '-93.624', function(err, data){
   console.log(data);
});
```

> If successful, you'll receive this response:

```php
array (
  0 =>
  array (
    'Name' => 'Alan\'s Brew',
    'Id' => '812-198-4099',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-198-4099',
    'Latitude' => 41.582946999999997,
    'Longitude' => -93.622444000000002,
    'Address' => '333 SW 5th st',
    'City' => 'Des Moines\n',
    'State' => 'IA',
    'PostalCode' => '50309',
    'Group' => '812-198-4099',
    'Delta' => 0.0036089999999973088,
  ),
  1 =>
  array (
    'Name' => 'Rocket Gear',
    'Id' => '812-742-6826',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-742-6826',
    'Latitude' => 41.589759829999998,
    'Longitude' => -93.61564636,
    'Address' => '123 Test Ave\n',
    'City' => 'Des Moines',
    'State' => 'IA',
    'PostalCode' => '50169',
    'Group' => '812-742-6826',
    'Delta' => 0.013113469999993299,
  )
)
```

```ruby
[
  {
    "Name"       => "Rocket Gear",
    "Id"         => "812-742-6826",
    "Type"       => "Dwolla",
    "Image"      => "http://uat.dwolla.com/avatars/812-742-6826",
    "Latitude"   => 41.58975983,
    "Longitude"  => -93.61564636,
    "Address"    => "123 Test Ave\n",
    "City"       => "Des Moines",
    "State"      => "IA",
    "PostalCode" => "50169",
    "Group"      => "812-742-6826",
    "Delta"      => 0.0045938100000100235
  },
  {
    "Name"       => "Alan's Brew",
    "Id"         => "812-198-4099",
    "Type"       => "Dwolla",
    "Image"      => "http://uat.dwolla.com/avatars/812-198-4099",
    "Latitude"   => 41.582947,
    "Longitude"  => -93.622444,
    "Address"    => "120 SW 5th st\n",
    "City"       => "Des Moines",
    "State"      => "IA",
    "PostalCode" => "50309",
    "Group"      => "812-198-4099",
    "Delta"      => 0.009497000000003197
  },
  { ... }
]
```

```js
[
  {
    "Name": "ThelmasTreats",
    "Id": "812-608-8758",
    "Type": "Dwolla",
    "Image": "https://www.dwolla.com/avatars/812-608-8758",
    "Latitude": 41.590043,
    "Longitude": -93.62095,
    "Address": "615 3rd Street\n",
    "City": "Des Moines",
    "State": "IA",
    "PostalCode": "50309",
    "Group": "812-608-8758",
    "Delta": 0.0009069999999908873
  },
  {
    "Name": "IKONIX Studio",
    "Id": "812-505-4939",
    "Type": "Dwolla",
    "Image": "https://www.dwolla.com/avatars/812-505-4939",
    "Latitude": 41.5887958,
    "Longitude": -93.6215057,
    "Address": "506 3rd St\nSuite 206",
    "City": "Des Moines",
    "State": "IA",
    "PostalCode": "50309",
    "Group": "812-505-4939",
    "Delta": 0.0027098999999992657
  }
]
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Name": "ThelmasTreats",
            "Id": "812-608-8758",
            "Type": "Dwolla",
            "Image": "https://www.dwolla.com/avatars/812-608-8758",
            "Latitude": 41.590043,
            "Longitude": -93.62095,
            "Address": "615 3rd Street\n",
            "City": "Des Moines",
            "State": "IA",
            "PostalCode": "50309",
            "Group": "812-608-8758",
            "Delta": 0.0009069999999908873
        },
        {
            "Name": "IKONIX Studio",
            "Id": "812-505-4939",
            "Type": "Dwolla",
            "Image": "https://www.dwolla.com/avatars/812-505-4939",
            "Latitude": 41.5887958,
            "Longitude": -93.6215057,
            "Address": "506 3rd St\nSuite 206",
            "City": "Des Moines",
            "State": "IA",
            "PostalCode": "50309",
            "Group": "812-505-4939",
            "Delta": 0.0027098999999992657
        }
    ]
}
```

Retrieve a list of Dwolla business accounts ([Spots](https://www.dwolla.com/spots)) near a given location.

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/contacts/nearby?client_id={}&client_secret={}&latitude={}&longitude={}`


| Parameter     | Optional? | Description                                                      |
|---------------|-----------|------------------------------------------------------------------|
| client_id     |           | Your Application Key                                             |
| client_secret |           | Your Application Secret                                          |
| latitude      |           | Latitude coordinates (between -90.0 and 90.0)                    |
| longitude     |           | Longitude coordinates (between -180.0 and 180.0)                 |
| range         | yes       | Range to retrieve spots for in miles (default 10, minimum 1)     |
| limit         | yes       | Number of spots to retrieve (default 10, minimum 1, maximum 100) |

### Response

| Parameter | Description
|-----------|------------|
Name | Business name
Id | Dwolla ID
Type | Will always be `Dwolla`
Image | URL to business's account avatar
Latitude | Business location
Longitude | Business location
Address | Business address.  Street address lines are separated by an escaped newline character: `\n`
City | Business address city
State | Business address state
PostalCode | Business address zip code
Group | Set to Dwolla ID of business account
Delta | Proximity to the lat / long specified in query

### Errors
| Error String | Description |
|--------------|-------------|
| Latitude must be between -90.0 and 90.0. | Invalid latitude. |
| Longitude must be between -180.0 and 180.0. | Invalid longitude. |

## Find Users Nearby

```php
<?php
$Account = new Dwolla\Account();
$Account->settings->client_id = $apiKey;
$Account->settings->client_secret = $apiSecret;

$lat = "40.7820";
$long = "-73.8317";

$result = $Account->nearby($lat, $long);

var_export($result);
?>
```
```ruby
#   Get a list of nearby Dwolla users
#   for a given set of coordinates (lat, long)
pp Dwolla::Users.nearby(:latitude => 40.7820, :longitude => -73.8317)
```
```python
# Get users near 40.7127, 74.0059
print(accounts.nearby(40.7127, 74.0059))
```
```js
/**
 * Fetch all spots near lat 40.7820 and 
 * long -73.8317
 **/

Dwolla.nearbyUsers('40.7820', '-73.8317', function(err, data){
   console.log(data);
});
```

> If successful, you'll receive this response:

```php
array (
  0 =>
  array (
    'Id' => '812-687-7049',
    'Image' => 'https://www.dwolla.com/avatars/812-687-7049',
    'Name' => 'Gordon Zheng',
    'Latitude' => 40.708448,
    'Longitude' => -74.014429,
    'Delta' => 114.72287700000001
  )
)
```

```ruby
[
  {
    "Id"         => "812-687-7049",
    "Image"      => "https://www.dwolla.com/avatars/812-687-7049",
    "Name"       => "Gordon Zheng",
    "Latitude"   => 40.708448,
    "Longitude"  => -74.014429,
    "Delta"      => 114.72287700000001
  },
  { ... }
]
```

```js
[
  {
    "Id": "812-687-7049",
    "Image": "https://www.dwolla.com/avatars/812-687-7049",
    "Name": "Gordon Zheng",
    "Latitude": 40.708448,
    "Longitude": -74.014429,
    "Delta": 114.72287700000001
  }
]
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Id": "812-687-7049",
            "Image": "https://www.dwolla.com/avatars/812-687-7049",
            "Name": "Gordon Zheng",
            "Latitude": 40.708448,
            "Longitude": -74.014429,
            "Delta": 114.72287700000001
        }
    ]
}
```

Retrieve a list of Dwolla users near a given location. Users must opt into allowing their location to be seen by enabling this setting in the Dwolla mobile app. Example on the ([web app](https://mobile.dwollalabs.com/location)).

### HTTP Request

`GET http://www.dwolla.com/oauth/rest/users/nearby?client_id={}&client_secret={}&latitude={}&longitude={}`


| Parameter     | Optional? | Description                                                      |
|---------------|-----------|------------------------------------------------------------------|
| client_id     |           | Your Application Key                                             |
| client_secret |           | Your Application Secret                                          |
| latitude      |           | Latitude coordinates (between -90.0 and 90.0)                    |
| longitude     |           | Longitude coordinates (between -180.0 and 180.0)                 |

### Response

| Parameter | Description
|-----------|------------|
Id | Dwolla ID
Image | URL to user's account avatar
Name | User's name
Latitude | User's location
Longitude | User's location
Delta | Proximity to the lat / long specified in query

### Errors
| Error String | Description |
|--------------|-------------|
| Latitude must be between -90.0 and 90.0. | Invalid latitude. |
| Longitude must be between -180.0 and 180.0. | Invalid longitude. |