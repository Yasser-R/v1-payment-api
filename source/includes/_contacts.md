# Contacts

```shell
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


```php
<?php
$Contacts = new Dwolla\Contacts();
$Contacts->settings->oauth_token = "foo";

$result = $Contacts->get();

var_export($result);
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
# Get the first 10 contacts from the user
# associated with the current OAuth token.

print(contacts.get())
```
```js
/**
 * EXAMPLE 1: 
 *   Fetch last 10 contacts from the 
 *   account associated with the provided
 *   OAuth token
 **/

Dwolla.contacts(function(err, data){
   console.log(data);
});

/**
 * EXAMPLE 2: 
 *   Search through the contacts of the
 *   account associated with the provided
 *   OAuth token
 **/

Dwolla.contacts({search: 'Ben'}, function(err, data){
   console.log(data);
});

```

> If successful, you'll receive this response:

```ruby
[
  {
    "Name"  => "Gordon Zheng",
    "Id"    => "812-742-3301",
    "Type"  => "Dwolla",
    "Image" => "http://uat.dwolla.com/avatars/812-742-3301",
    "City"  => "Elmhurst",
    "State" => "NY"
  },
  { ... },
  { ... }
]
```

```php
array (
  0 =>
  array (
    'Name' => 'GORDCORP',
    'Id' => '812-740-4294',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-740-4294',
    'City' => 'Test',
    'State' => 'NY',
  ),
  1 =>
  array (
    'Name' => 'Gordon Zheng',
    'Id' => '812-742-3301',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-742-3301',
    'City' => 'Elmhurst',
    'State' => 'NY',
  ),
  2 =>
  array (
    'Name' => 'Joe Schmoe',
    'Id' => '812-741-3440',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-741-3440',
    'City' => 'Test',
    'State' => 'NY',
  ),
)
```

```js
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