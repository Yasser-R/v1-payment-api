# Contacts

The Dwolla API exposes functionality to allow users to look-up the contacts of another user or users near a certain location (latitude and longitude).

In order to retrieve a user's contacts, an OAuth token is required with the `Contacts` permission scope. 

## Get a user's contacts

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/contacts/?oauth_token={token}`

> Examples with Dwolla Libraries:

```json
```
```php
<?php
/**
 * EXAMPLE 1: 
 *   Fetch last 10 contacts from the 
 *   account associated with the provided
 *   OAuth token
 **/
$contacts = $Dwolla->contacts('Ben');
if(!$contacts) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($contacts); } // Print contacts

/**
 * EXAMPLE 2: 
 *   Search through the contacts of the
 *   account associated with the provided
 *   OAuth token for 'David', return 20 
 *   "Dwolla" type results. 
 **/

$contacts = $Dwolla->contacts('David', array('dwolla'), 20);
if(!$contacts) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($contacts); } // Print contacts
?>
```
```ruby
# EXAMPLE 1: 
#   Fetch last 10 contacts from the 
#   account associated with the provided
#   OAuth token
pp Dwolla::Contacts.get


# EXAMPLE 2: 
#   Search through the contacts of the
#   account associated with the provided
#   OAuth token
pp Dwolla::Contacts.get({:search => 'Ben'})
```
```python
'''
    EXAMPLE 1: 
      Fetch last 10 contacts from the 
      account associated with the provided
      OAuth token
'''
contacts = DwollaUser.get_contacts()
print(contacts)


'''
    EXAMPLE 2: 
      Search through the contacts of the
      account associated with the provided
      OAuth token
'''
contacts = DwollaUser.get_contacts('Ben')
print(contacts)
```
```js
/**
 * EXAMPLE 1: 
 *   Fetch last 10 contacts from the 
 *   account associated with the provided
 *   OAuth token
 **/

Dwolla.contacts(function(err, data){
   if (err) { console.log(err); }
   console.log(data);
});

/**
 * EXAMPLE 2: 
 *   Search through the contacts of the
 *   account associated with the provided
 *   OAuth token
 **/

Dwolla.contacts({search: 'Ben'}, function(err, data){
   if (err) { console.log(err); }
   console.log(data);
});

```

| Parameter   | Optional? | Description                                   |
|-------------|-----------|-----------------------------------------------|
| oauth_token | no        | A valid OAuth token with `Contacts` scope.    |
| search      | yes       | Search term used to search contacts.          |
| types       | yes       | Account types to return.                      |
| limit       | yes       | Number of contacts to return (default is 10). |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Name": "Joe Schmoe",
            "Id": "812-999-0090",
            "Type": "Dwolla",
            "Image": "https://www.dwolla.com/avatars/812-999-0090",
            "City": "St Louis",
            "State": "MO"
        },
        {
            "Name": "Sam Schmoe Super Foods",
            "Id": "812-999-0091",
            "Type": "Dwolla",
            "Image": "https://www.dwolla.com/avatars/812-999-0091",
            "City": "Des Moines",
            "State": "IA"
        },
        {
            "Name": "Moe Schmoe",
            "Id": "812-999-0092",
            "Type": "Dwolla",
            "Image": "https://www.dwolla.com/avatars/812-999-0092",
            "City": "Simpsonville",
            "State": "SC"
        }
    ]
}
```

## Find contacts by location

Please note that this will not return any personal account results; only businesses ('spots') will be returned by searching via this endpoint.

`GET https://www.dwolla.com/oauth/rest/contacts/nearby?client_id={client_id}&client_secret={client_secret}`

> Examples with Dwolla Libraries:

```json
```
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
 * EXAMPLE 1: 
 * Fetch all spots near lat 45 and 
 * long 45
 **/

Dwolla.nearby(cfg.apiKey, cfg.apiSecret, 45, 45, function(err, data){
   if (err) { console.log(err); }
   console.log(data);
});
```

### HTTP Request

| Parameter     | Optional? | Description                                                      |
|---------------|-----------|------------------------------------------------------------------|
| client_id     | no        | Consumer key for the application                                 |
| client_secret | no        | Consume secret for the application                               |
| latitude      | no        | Latitude coordinates (between -90.0 and 90.0)                    |
| longitude     | no        | Longitude coordinates (between -180.0 and 180.0)                 |
| range         | yes       | Range to retrieve spots for in miles (default 10, minimum 1)     |
| limit         | yes       | Number of spots to retrieve (default 10, minimum 1, maximum 100) |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Address": "111 West 5th St. Suite A",
            "City": "Des Moines",
            "Group": "812-111-2222",
            "Id": "812-111-1111",
            "Image": "https://www.dwolla.com/avatar.aspx?u=812-111-1111",
            "Latitude": 41.5852257,
            "Longitude": -93.6245059,
            "Name": "Test Spot 1",
            "PostalCode": "50309",
            "State": "IA"
        },
        {
            "Address": "111 West 5th St. Suite B",
            "City": "Des Moines",
            "Group": "812-111-1111",
            "Id": "812-111-2222",
            "Image": "https://www.dwolla.com/avatar.aspx?u=812-111-2222",
            "Latitude": 41.5852257,
            "Longitude": -93.6245059,
            "Name": "Test Spot 2",
            "PostalCode": "50309",
            "State": "IA"
        }
    ]
}
```