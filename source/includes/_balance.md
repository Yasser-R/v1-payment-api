# Balance

This method can be used to access the account balance for a user with an authorized OAuth token. Please note that the application must request a token with the `Balance` scope or else the endpoint will return an error. 

## Get Balance

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Fetch account balance for the 
 *   account associated with the provided
 *   OAuth token
 **/
$balance = $Dwolla->balance();
if($balance == NULL) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { echo $balance; } // Print user's balance
```
```ruby
# EXAMPLE 1: 
#   Get the balance of the authenticated user
pp Dwolla::Balance.get
```
```python
balance = DwollaUser.get_balance()
print(balance)
```
```js
/***
 * Example 1:
 *
 * Get balance of the user associated with the OAuth token.
 */

Dwolla.balance(function(err, data){
   if (err) { console.log(err); }
   console.log(data);
});
```

`GET https://www.dwolla.com/oauth/rest/balance/?oauth_token={token}`

| Parameter   | Optional? | Description                                   |
|-------------|-----------|-----------------------------------------------|
| oauth_token | no        | A valid OAuth token with `Balance` scope.     |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": 55.76
}
```