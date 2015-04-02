# Transaction Stats

```js
/**
 * Retrieve transaction statistics for the 
 * authorized user.
 */

Dwolla.transactionsStats(function(err, data){
  console.log(data);
});
```

```ruby
puts Dwolla::Transactions.stats()
```

```php
<?php
$Transactions = new Dwolla\Transactions();
$Transactions->settings->oauth_token = "foo";

$result = $Transactions->stats();

var_export($result);
?>
```
```python
print(transactions.stats())
```

> Response:

```ruby
{
  "TransactionsCount" => 10,
  "TransactionsTotal" => 134.22
}
```

```js
{
  "TransactionsCount": 5,
  "TransactionsTotal": 116.92
}
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "TransactionsCount": 5,
        "TransactionsTotal": 116.92
    }
}
```

```php
array (
  'TransactionsCount' => 5,
  'TransactionsTotal' => 116.21,
)
```

Transaction statistics can be retrieved for the authenticated user.   Currently, the report contains a count of transactions and sum of transactions over a given period of time.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions/stats`

### Request Parameters
Parameter | Optional? | Description
----------|-----------|------------
startDate | yes | Starting date and time for which to select transactions for report. Defaults to 0300 UTC, today.
endDate | yes | Ending date and time for which to select transactions for report.  Defaults to current date and time.

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid start date format. | Should follow format of yyyy-mm-dd |
| Invalid end date format. | Should follow format of yyyy-mm-dd |