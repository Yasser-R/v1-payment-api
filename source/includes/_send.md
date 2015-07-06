# Send Money

```always
  __                      ___    ___               
 /\ \                    /\_ \  /\_ \              
 \_\ \  __  __  __    ___\//\ \ \//\ \      __     
 /'_` \/\ \/\ \/\ \  / __`\\ \ \  \ \ \   /'__`\   
/\ \L\ \ \ \_/ \_/ \/\ \L\ \\_\ \_ \_\ \_/\ \L\.\_ 
\ \___,_\ \___x___/'\ \____//\____\/\____\ \__/.\_\
 \/__,_ /\/__//__/   \/___/ \/____/\/____/\/__/\/_/
                                                   

```

```php
/**
 *   Send money ($5.50) to email address gordon@dwolla.com
 **/

<?php
$Transactions = new Dwolla\Transactions();
$Transactions->settings->oauth_token = "foo";
$Transactions->settings->pin = 9999;

$result = $Transactions->send('gordon@dwolla.com', 5.50, [
  'destinationType' => 'Email'
]);

print_r($result);

?>
```

```ruby
#   Send money ($1.00) to a Dwolla ID 812-626-8794

transactionId = Dwolla::Transactions.send({
  :destinationId => '812-626-8794', 
  :amount => 1.00, 
  :pin => "9999"})

puts transactionId
```
```python
# Send $5.50 to a Dwolla ID and print the transaction ID
print(transactions.send('812-197-4121', 5.50))
```
```js
/**
 *   Send money ($1.00) to an email address, with a note
 **/

// optional params:
var params = {
  destinationType: 'Email', 
  notes: 'Thanks for the coffee!'
};

Dwolla.send(pin, 'gordon@dwolla.com', 1.00, params, function(err, data) {
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

```php
343947    // resulting Transacton ID
```
```python
113533 # Sender transaction ID
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": 12345
}
```

```ruby
341523   # resulting transaction ID
```

```js
341523   // resulting transaction ID
```

Send money from an authorized user's account to any Dwolla account ID, phone number, or e-mail address. You can send money to any phone number or email address -- if the recipient doesn't yet have a Dwolla account, they'll be sent an SMS or email notifying them of the payment and prompting them to sign up to claim the funds.

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
| notes | yes | Note to attach to the resulting transaction.  Will be visible from dwolla.com dashboard to both sender and recipient.  Max 250 characters.                                                                           |
| additionalFees       | yes       | Array of additional facilitator fee objects.  `[{"destinationId": "", "amount": 0.01}, ...]`                                                                                                                                                               |
| metadata             | yes       | Optional JSON object of a maximum of 10 key-value pairs (each key and value must be less than 255 characters).  [Read more](#metadata)                                                                                   |
| assumeAdditionalFees | yes       | Set to `true` if the sending user will assume all additional facilitator fees, `false` if the destination user will assume the fees. Defaults to `false`.                                          |
| facilitatorAmount    | yes       | Amount of the facilitator fee to override. Only applicable if the facilitator fee feature is enabled. If set to 0, facilitator fee is disabled for transaction. Cannot exceed 50% of the `amount`. |

### Response 

If successful, the response is simply the [Sender's Transaction ID](#how-transactions-work) for the resulting payment.  Read more about [Transactions](#transactions).

### Errors
| Error String | Description |
|--------------|-------------|
| Insufficient balance. | The source account does not have sufficient balance in the selected funding source. |
| The account is limited to $x per transaction. on: | The source account has exceeded it's sending transaction limit |
| Amount must be at least $0.01. on: | The amount to send must be greater than $0.01 |
| Invalid account PIN | The supplied PIN is invalid or incorrect. |
| Invalid SSN for this account. | The source account is pending SSN validation. |
| Invalid funding source provided: {...} | The specified funding source doesn't exist or is invalid. |