# Balance

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

```json
{
    "Success": true,
    "Message": "Success",
    "Response": 55.76
}
```

Fetch a user's account balance.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Balance` scope.</aside>

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/balance/`