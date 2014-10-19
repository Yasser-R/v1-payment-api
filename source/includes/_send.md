# Send Money

```php
/**
 *   Send money ($1.00) to Dwolla ID 812-734-7288
 **/
$transactionId = $Dwolla->send($pin, '812-734-7288', 1.00);
if(!$transactionId) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { echo "Send transaction ID: {$transactionId} \n"; } // Print Transaction ID
```
```ruby
#   Send money ($1.00) to a Dwolla ID 812-626-8794
transactionId = Dwolla::Transactions.send({:destinationId => '812-626-8794', :amount => 1.00, :pin => @pin})
pp transactionId
```
```python
'''
      Send money ($1.00) to a Dwolla ID 812-626-8794
'''
transaction = DwollaUser.send_funds(0.01, '812-626-8794', _keys.pin)
print(transaction)
```
```js
/**
 *   Send money ($1.00) to a Dwolla ID 812-626-8794
 **/

Dwolla.send(cfg.pin, '812-626-8794', 1.00, function(err, data) {
   if (err) { console.log(err); }
   console.log(data);
});
```
```json
{
  "amount": 0.01,
  "destinationId": "gordon@dwolla.com",
  "destinationType": "Email",
  "pin": 4321
}
```

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": 12345
}
```

Send money from an authorized user's account to any Dwolla account ID, phone number, or e-mail address. You can send money to any phone number or email address -- if the recipient doesn't yet have a Dwolla account, they'll be sent an SMS or email notifying them of the payment and prompting them to sign up to claim the funds.

You can optionally have the sender cover the transaction fee, if applicable, by setting `assumeCosts` to `true`.  By default, the recipient of the payment covers the $0.25 fee.

Facilitator fees allow you, as the application facilitating a payment, to take a cut up to 50% of the payment amount.  By default, the facilitator fee is sent to the application owner's account, but you can have them sent to other recipients by declaring additional facilitator fees in the optional `additionalFees` array.
	
### HTTP Request

`POST https://www.dwolla.com/oauth/rest/transactions/send`

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Send` scope.</aside>

| Parameter            | Optional? | Description                                                                                                                                                                                        |
|----------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| destinationId        | no        | Email, Dwolla ID, or phone number of recipient.                                              |
| pin                  | no        | The PIN associated with the sender's account.                                                                                                                                                        |
| amount               | no        | Amount of funds to transfer to destination user.                                                                                                                                                   |
| destinationType      | yes       | Type of destination user. Defaults to 'Dwolla'. Possible values are `Dwolla`, `Email`, and `Phone`.                                                                                               |
| fundsSource          | yes       | ID of [funding source](#funding-sources) to use for transaction (defaults to `Balance`). |
| notes | yes | Note to attach to the resulting transaction.  Will be visible from dwolla.com dashboard to both sender and recipient.  Max 250 characters.
| assumeCosts          | yes       | Set to `true` for the fulfilling user to assume the Dwolla fee, `false` for the destination user to assume the fee.                                                                                |
| additionalFees       | yes       | Array of additional facilitator fee objects.  `[{"destinationId": "", "amount": 0.01}, ...]`                                                                                                                                                               |
| metadata             | yes       | Optional JSON object of a maximum of 10 key-value pairs (each key and value must be less than 255 characters).  [Read more](#metadata)                                                                                   |
| assumeAdditionalFees | yes       | Set to `true` if the sending user will assume all additional facilitator fees, `false` if the destination user will assume the fees. Defaults to `false`.                                          |
| facilitatorAmount    | yes       | Amount of the facilitator fee to override. Only applicable if the facilitator fee feature is enabled. If set to 0, facilitator fee is disabled for transaction. Cannot exceed 50% of the `amount`. |

### Response 

If successful, the response is simply the [Sender's Transaction ID](#how-transactions-work) for the resulting payment.  Read more about [Transactions](#transactions).