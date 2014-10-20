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
 *   Fetch basic account information
 *   for a given Dwolla ID (or email)
 **/

Dwolla.basicAccountInfo('812-546-3855', function(err, data) {
    console.log(data);
});
```

> If successful, you'll receive this response:

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

## Get Full Account Info

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
 *   Fetch account information for the
 *   authorized user
 **/

Dwolla.fullAccountInfo(function(err, data) {
    console.log(data);
});
```

> If successful, you'll receive this response:

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
/**
 * EXAMPLE 1: 
 * Fetch contacts of businesses near lat 45, lon 45
 **/
$contacts = $Dwolla->nearby(45,45);
if(!$contacts) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($contacts); } // Print contacts
?>
```
```ruby
# EXAMPLE 1: 
#   Get a list of nearby Dwolla spots
#   for a given set of coordinates
pp Dwolla::Contacts.nearby({:latitude => 1, :longitude => 2})
```
```python
'''
    EXAMPLE 1: 
      Fetch spots near  41.59 and 
      -93.62 (default parameters in dwolla-python)
'''
contacts = DwollaUser.get_nearby_spots()
print(contacts)
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
| latitude      |         | Latitude coordinates (between -90.0 and 90.0)                    |
| longitude     |         | Longitude coordinates (between -180.0 and 180.0)                 |
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
