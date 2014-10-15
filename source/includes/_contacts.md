# Contacts

```always
[
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
    }
]
```

Any Dwolla user which a user sends money to or receives money from becomes a `Contact` of that user.

### Contact Resource

Property | Description
---------|------------
Name | User's full name (business name, if business account)
Id | Dwolla ID of user
Type | Will always be `Dwolla`
Image | URL of the user's avatar image
City | User's resident city
State | User's resident state

## Get a user's contacts


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

Use the following endpoint to fetch the authorized user's contacts.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Contacts` scope.</aside>

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/contacts/`

### Optional Paramters

| Parameter   | Description                                   |
|-------------|-----------------------------------------------|
| search      | Search term used to search contacts.          |
| types       | Account types to return.                      |
| limit       | Number of contacts to return (default is 10). |