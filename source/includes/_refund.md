# Refund

```json
{
  "amount": 11.25,
  "fundsSource": "Balance",
  "pin": 9999,
  "transactionId": 2427171
}
```

```js
/**
 * Process a refund for transaction '123456',
 * funding the refund from funding source ID '7654321'
 * for amount $10.00
 */

Dwolla.refund(pin, '12345', '7654321', 10.00, function(err, data) {
  console.log(data);
});
```

```ruby
puts Dwolla::Transactions.refund({
  :transactionId => "341617",
  :fundsSource => "Balance",
  :pin => "9999",
  :amount => 20
})
```

```php
<?php

/*
 * Refund transaction ID 347940 for the amount of $20.00,
 * and fund the refund with the merchant's account balance.
 */

$Transactions = new Dwolla\Transactions();
$Transactions->settings->oauth_token = "foo";
$Transactions->settings->pin = 9999;

$transactionId = '347940';
$fundingSource = 'Balance';
$result = $Transactions->refund($transactionId, $fundingSource, 20.00);

var_export($result);
?>
```
```python
# Refund $2 from "Balance" from transaction
# '123456'

print(transactions.refund('3452346', 'Balance', 2.00))
```

> Response: 

```php
array (
  'TransactionId' => 347944,
  'RefundDate' => '10/24/2014 23:23:20',
  'Amount' => 20,
)
```
```python
{
    "TransactionId": 4532,
    "RefundDate": "2014-12-10T12:01:09Z",
    "Amount": 2.00
}
```
```ruby
{
  "TransactionId" => 341621,
  "RefundDate"    => "10/20/2014 05:51:43",
  "Amount"        => 20
}
```

```js
{
  "TransactionId": 123458,
  "RefundDate": "2014-01-22T17:47:04Z",
  "Amount": 10
}
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "TransactionId": 8522,
        "RefundDate": "2014-01-22T17:47:04Z",
        "Amount": 20
    }
}
```

Commercial, Non-Profit, and Government type accounts can refund payments that have been received.  There is no limit on the window of time in which a payment can be refunded.  Individual accounts currently cannot refund payments.

A refund creates a new transaction. The refund amount can be up to the total payment amount, plus any facilitator fees assumed by the sender.

The Refund API requires the [_Recipient's_ Transaction ID](#how-transactions-work) to be provided.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Send` scope.</aside>

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/transactions/refund`

### Request Parameters

| Param | Optional? | Description |
|-------|-----------|-------------|
| fundsSource | no | ID of [funding source](#funding-sources) from which to fund the payments
| pin | no | User account PIN
| transactionId | no | [_Recipient's_ Transaction ID](#how-transactions-work) of the payment to be refunded.
| notes | yes | Note to attach to the refund.  Visible to refunder and recipient of refund.  Max 250 chars.
| metadata | yes | An optional [metadata](#metadata) object.

### Response Parameters
|Param| Description
|-----|------------
TransactionId | Transaction ID of the newly created refund payment
RefundDate | Timestamp when refund was created
Amount | Amount refunded

### Errors
| Error String | Description |
|------------------|-------------|
| Invalid access token. | Access token not valid. |
| Transaction ineligible for refund. | Cannot refund the transaction, for one of the following reasons: user is not recipient of a transaction, user is not a Commercial or Non-Profit type account |
| Invalid refund amount. | Refund amount is either less than 0.01 or not equal to or less than the original transaction amount. |