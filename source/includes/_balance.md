# Balance

```php
<?php
$Account = new Dwolla\Account();
$Account->settings->oauth_token = "foo";

$result = $Account->balance();

var_export($result);
?>
```
```ruby
#   Get the balance of the authenticated user
puts Dwolla::Balance.get
```
```python
balance = DwollaUser.get_balance()
print(balance)
```
```js
/***
 * Get balance of the user associated with the OAuth token.
 */

Dwolla.balance(function(err, data){
   if (err) { console.log(err); }
   console.log(data);
});
```

> Response:

```php
55.76
```

```ruby
55.76
```

```js
55.76
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