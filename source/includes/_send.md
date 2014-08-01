# Send Money

The API allows users to easily send money to other users' Dwolla accounts via their Dwolla ID, Facebook ID, Twitter screen-name, phone number, or e-mail address from the account associated with an active OAuth token. In order for the transaction to successfully complete, the OAuth token must be granted with the `Send` scope. 

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Send money ($1.00) to a Dwolla ID 
 **/
$transactionId = $Dwolla->send($pin, '812-734-7288', 1.00);
if(!$transactionId) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { echo "Send transaction ID: {$transactionId} \n"; } // Print Transaction ID
```
```ruby
# EXAMPLE 1:
#   Send money ($1.00) to a Dwolla ID
transactionId = Dwolla::Transactions.send({:destinationId => '812-626-8794', :amount => 1.00, :pin => @pin})
pp transactionId
```
```python
'''
    EXAMPLE 1: 
      Send money ($1.00) to a Dwolla ID 
'''
transaction = DwollaUser.send_funds(0.01, '812-626-8794', _keys.pin)
print(transaction)
```
```js
/**
 * EXAMPLE 1: 
 *   Send money ($1.00) to a Dwolla ID 
 **/

Dwolla.send(cfg.pin, '812-626-8794', 1.00, function(err, data) {
   if (err) { console.log(err); }
   console.log(data);
});
```

`POST https://www.dwolla.com/oauth/rest/transactions/send`

| Parameter            | Optional? | Description                                                                                                                                                                                        |
|----------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| oauth_token          | no        | Valid OAuth token with the `Request` scope.                                                                                                                                                        |
| destinationId        | no        | Identification of the user to send funds to. Must be the Dwolla identifier, Facebook identifier, Twitter identifier, phone number, or email address.                                               |
| pin                  | no        | The PIN associated with the user's account.                                                                                                                                                        |
| amount               | no        | Amount of funds to transfer to destination user.                                                                                                                                                   |
| destinationType      | yes       | Type of destination user (defaults to 'Dwolla', possible values are 'Dwolla', 'Email', and 'Phone').                                                                                               |
| fundsSource          | yes       | ID of funding source to use for transaction (defaults to `Balance`).                                                                                                                               |
| assumeCosts          | yes       | Set to `true` for the fulfilling user to assume the Dwolla fee, `false` for the destination user to assume the fee.                                                                                |
| metadata             | yes       | Set to `true` if the fulfilling user is to assume all facilitator fees. Defaults to `false`; does not affect the Dwolla transaction fee.                                                           |
| additionalFees       | yes       | Array of additional facilitator fees                                                                                                                                                               |
| --> destinationId    | yes       | Facilitator's Dwolla ID                                                                                                                                                                            |
| --> amount           | yes       | Facilitator amount.                                                                                                                                                                                |
| metadata             | yes       | Optional JSON object of a maximum of 10 key-value pairs (each key and value must be less than 255 characters).                                                                                     |
| assumeAdditionalFees | yes       | Set to `true` if the sending user will assume all additional facilitator fees, `false` if the destination user will assume the fees. Defaults to `false`.                                          |
| facilitatorAmount    | yes       | Amount of the facilitator fee to override. Only applicable if the facilitator fee feature is enabled. If set to 0, facilitator fee is disabled for transaction. Cannot exceed 50% of the 'amount'. |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": 12345
}
```