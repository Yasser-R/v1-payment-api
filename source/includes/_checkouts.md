# Checkouts

```shell
 _        ,
(_\______/________
   \-|-|/|-|-|-|-|/
    \==/-|-|-|-|-/
     \/|-|-|-|,-'
      \--|-'''
       \_j________
       (_)     (_)

"Shopping Trolley" - hjw
```

Create a checkout on our [Off-Site Gateway](https://developers.dwolla.com/dev/pages/gateway). 

Checkout sessions must be completed by the user within __15 minutes__ of their creation. 

There are two types of checkouts:

### Standard Checkout

A standard checkout means the payment is placed automatically and immediately after the user clicks the *Submit* button on the confirmation screen.

### PayLater Checkout

A PayLater checkout means the user is redirected back to your site after clicking *Continue* on the confirmation screen.  No payment is created yet.  

You will have 15 minutes to [complete the Checkout](#complete-a-paylater-checkout) in order to place the payment.

To create a PayLater Checkout, set `checkoutWithApi` to `true`.

## Create a Checkout

```php
<?php

$Checkouts = new Dwolla\Checkouts();
$Checkouts->settings->client_id = $apiKey;
$Checkouts->settings->client_secret = $apiSecret;

$purchaseOrder = [
  'destinationId' => '812-742-3301',
  'total' => '33.99'
];

$result = $Checkouts->create($purchaseOrder, [
  'redirect' => 'https://myredirect.com/redirect'
]);

print_r($result);

?>
```


```json
{
  "client_id": "JCGQXLrlfuOqdUYdTcLz3rBiCZQDRvdWIUPkw++GMuGhkem9Bo",
  "client_secret": "g7QLwvO37aN2HoKx1amekWi8a2g7AIuPbD5C/JSLqXIcDOxfTr",
  "callback": "http://www.something.com/callback",
  "redirect": "http://www.something.com/return",
  "allowGuestCheckout": "true",
  "checkoutWithApi": "false",
  "orderId": "foo123",
  "allowFundingSources": "true",
  "additionalFundingSources": "true",
  "purchaseOrder": {
		"orderItems": [
			{
				"name": "Prime Rib Sandwich", 
				"description": "A somewhat tasty non-vegetarian sandwich", 
				"quantity": "1", 
				"price": "15.00"
			}
		],
    "destinationId": "812-740-4294",
    "total": 15.00,
    "notes": "blahhh",
    "metadata": {
    	"key1": "something",
    	"key2": "another thing"
    }
  }
}
```

```js
var purchaseOrder = {
 destinationId: '812-740-4294',
 total: '5.00'
};

var params = {
 allowFundingSources: true,
 orderId: 'blah',
};

dwolla.createCheckout(redirect_uri, purchaseOrder, params, function(err, checkout) {
 if (err) console.log(err);

 console.log(checkout.checkoutURL);
});
```

```python
# Create checkout session with items from
# http://docs.dwolla.com/#checkouts

test = checkouts.create({
    'orderItems': {
        frozenset({
            'name': 'Prime Rib Sandwich',
            'description': 'A somewhat tasty non-vegetarian sandwich',
            'quantity': '1',
            'price': '15.00'
        })
    },
    'destinationId': '812-740-4294',
    'total': 15.00,
    'notes': 'blahhh',
    'metadata': frozenset({
        'key1': 'something',
        'key2': 'another thing'
    })})
```

```ruby
# Set redirect URL, callback URL
Dwolla::OffsiteGateway.redirect = 'http://dwolla.com/payment_redirect'
Dwolla::OffsiteGateway.callback = 'http://dwolla.com/payment_callback'

# Add products
Dwolla::OffsiteGateway.add_product('Macbook Air', '13" Macbook Air; Model #0001', 499.99, 1)

# Generate a checkout sesssion
pp Dwolla::OffsiteGateway.get_checkout_url('812-734-7288')
```

> If successful, you'll get this back:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "CheckoutId": "684862bc-8d94-4e53-9c41-26398b4b7fac"
    }
}
```

```js
https://www.dwolla.com/payment/checkout/684862bc-8d94-4e53-9c41-26398b4b7fac
```

```php
array (
  'CheckoutId' => 'f3f454bd-89a9-422f-8fef-5eb9f82ccc8f',
  'URL' => 'https://uat.dwolla.com/payment/checkout/f3f454bd-89a9-422f-8fef-5eb9f82ccc8f',
)
```

```ruby
https://www.dwolla.com/payment/checkout/684862bc-8d94-4e53-9c41-26398b4b7fac
```

> Simply append the resulting `CheckoutId` to this URI: `https://www.dwolla.com/payment/checkout/`

```
https://www.dwolla.com/payment/checkout/684862bc-8d94-4e53-9c41-26398b4b7fac
```

> Then send your user there!


### HTTP Request

`POST https://www.dwolla.com/oauth/rest/offsitegateway/checkouts`

Parameter | Optional? | Description
--------- | -------   | -----------
client_id |  | Your Application Key
client_secret |  | Your Application Secret
callback |  | Dwolla will POST a Callback containing results of the checkout when it is completed.
redirect |  | User will be redirected to this URL after successfully checking out, or cancelling.
purchaseOrder | | A purchaseOrder JSON object. See below.
checkoutWithApi | yes | Create a *PayLater* checkout.  Defaults to `false`.
allowGuestCheckout | yes | Enable [Dwolla Direct](https://developers.dwolla.com/dev/pages/gateway/direct) experience for users without a Dwolla account
orderId | yes | Custom string to identify the checkout.  Not persisted in resulting transaction.
allowFundingSources | yes | Allow the user to pay with a funding source other than their account Balance.  Defaults to `false`.
additionalFundingSources | yes | If `allowFundingSources` enabled, use this to filter the types of funding sources allowed. <br> `credit`: Only allow Dwolla Credit, if both the merchant and user support it <br> `banks`: Only allow bank (ACH) funding sources <br> `fisync`: Only allow FiSync-enabled bank funding sources <br> `realtime`: Only allow `credit` and `fisync` sources <br> `true`: Allow all funding source types <br> `false`: Do not allow any funding sources
profileId | yes | This feature allows a recipient to display a different avatar and name, to collect money on a different person/entity's behalf. Requires special permission to use. Contact us if you're interested!

### PurchaseOrder Object

Parameter | Optional? | Description
--------- | -------   | -----------
destinationId | | Dwolla ID of the user who will receive the payment in this checkout.
total | | Total checkout amount.
tax | yes | Order tax total.  For order display purposes only, not persisted.
discount | yes | Order discount (must be negative).  For order display purposes only, not persisted.
shipping | yes | Order shipping total.  For order display purposes only, not persisted.
notes | yes | A note of max 250 chars. to attach to the resulting payment.
facilitatorAmount | yes | Amount of the facilitator fee to override. Only applicable if the facilitator fee feature is enabled. If set to `0`, facilitator fee is disabled for transaction.
metadata | yes | An object containing up to 10 key-value pairs of your choice which is persisted with the resulting transaction.  [Learn about Metadata](#metadata)
orderItems | yes | An array of JSON object(s) which contain a `name`, `description`, `quantity`, and `price`.

### Errors
| Error String | Description |
|--------------|-------------|
| Shipping rate cannot be less than $0. | Specified shipping must be greater than $0 |
| Tax cannot be less than $0. | Specified tax must be greater than $0 |
| Discount cannot be greater than $0. | Specified discount must be less than $0 |
| Total cannot be less than $.01. | Total amount of PO must be greater than $0.01 |
| Invalid total. | Invalid total in reference to the orderItems in the PO |
| Notes length is too long. Maximum of 250 character is allowed. | Specified notes is greater than the allowed 250 character limit |
| Price on all order items cannot be less than $0. |  |
| Quantity on all order items cannot be less than 1. | OrderItems specified must contain a quantity greater than 1 |
| Order item name length must be between 1 and 100 characters. | Order item name must be greater than 1 & less than 100 characters |
| Order item description length must not exceed 200 characters. | Order item description must be greater than 1 & less than 200 characters |

## Retrieve a Checkout

```php
<?php
$Checkouts = new Dwolla\Checkouts();
$Checkouts->settings->client_id = $apiKey;
$Checkouts->settings->client_secret = $apiSecret;

$result = $Checkouts->get("19bca7f7-d92a-4a98-b685-7f0e4a372994");

var_export($result);
?>
```

```python
# Get information about the checkout with ID abdflkjdf34443

checkouts.get("abdflkjdf34443")
```

```js
var checkoutId = 'a9d7c86a-0d0d-4466-b228-e15584a1315a';
dwolla.getCheckout(checkoutId, function(err, response) {
  console.log(response);
});
```

```ruby
This method is not yet supported in dwolla-ruby.
```

> Response: 

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "CheckoutId": "684862bc-8d94-4e53-9c41-26398b4b7fac",
        "Discount": null,
        "Shipping": null,
        "Tax": null,
        "Total": 15,
        "Status": "Completed",
        "FundingSource": "Balance",
        "TransactionId": 69960,
        "ProfileId": null,
        "DestinationTransactionId": 69959,
        "OrderItems": [
            {
                "Description": "A somewhat tasty non-vegetarian sandwich",
                "Name": "Prime Rib Sandwich",
                "Price": 15,
                "Quantity": 1
            }
        ],
        "Metadata": null
    }
}
```

```js
{ 
  CheckoutId: 'd0429a55-c338-4139-b273-48d2f8c45693',
  Discount: null,
  Shipping: null,
  Tax: null,
  Total: 5,
  Status: 'Completed',
  FundingSource: 'Balance',
  TransactionId: 290142,
  ProfileId: null,
  DestinationTransactionId: 290141,
  OrderItems: [],
  Metadata: null 
}
```

```php
array (
  'CheckoutId' => 'f3f454bd-89a9-422f-8fef-5eb9f82ccc8f',
  'Discount' => NULL,
  'Shipping' => NULL,
  'Tax' => NULL,
  'Total' => 33.99,
  'Status' => 'Created',
  'FundingSource' => NULL,
  'TransactionId' => NULL,
  'ProfileId' => NULL,
  'DestinationTransactionId' => NULL,
  'OrderItems' =>
  array (
  ),
  'Metadata' => NULL,
)
```

```ruby
This method is not yet supported in dwolla-ruby.
```

Fetch an existing checkout by its `checkoutId` to get its status and details.

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/offsitegateway/checkouts/{checkoutId}?client_id={key}&client_secret={secret}`

### Response Parameters
This returns the `CheckoutId`, `Discount`, `Shipping`, `Tax`, `Total`, `OrderItems`, `ProfileId`, `Metadata` of the checkout and some additional information:

Parameter | Description
--------- | -----------
Status | Status of the checkout.  <br>Possible values: `Created`, `ReadyForApiCheckout`, `Expired`, `Completed`
FundingSource | Type of funding source used to complete the checkout.  <br>Possible values: `Balance`, `Bank`, `null`
TransactionId | For the resulting payment, this is the [_Sender's_ Transaction ID](#how-transactions-work).
DestinationTransactionId | For the resulting payment, this is the [_Recipient's_ Transaction ID](#how-transactions-work).

## Complete a PayLater Checkout

```json
{
  "client_id": "JCGQXLrlfuOqdUYdTcLz3rBiCZQDRvdWIUPkw++GMuGhkem9Bo",
  "client_secret": "g7QLwvO37aN2HoKx1amekWi8a2g7AIuPbD5C/JSLqXIcDOxfTr"
}
```
```python
# Complete the checkout

checkouts.complete('Order ID here')
```

```js
var checkoutId = 'a9d7c86a-0d0d-4466-b228-e15584a1315a';

dwolla.completeCheckout(checkoutId, function(err, response) {
  console.log(response);
});
```

```php
<?php

$Checkouts = new Dwolla\Checkouts();
$Checkouts->settings->client_id = $apiKey;
$Checkouts->settings->client_secret = $apiSecret;

$result = $Checkouts->complete("19bca7f7-d92a-4a98-b685-7f0e4a372994");

print_r($result);
?>
```

```ruby
This method is not yet supported in dwolla-ruby.
```

> If successful, you'll get back:

```ruby
This method is not yet supported in dwolla-ruby.
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Amount": 15,
        "CheckoutId": "041f503b-bf68-4d35-83b3-bc6304a98e78",
        "ClearingDate": "",
        "OrderId": "foo",
        "TestMode": false,
        "TransactionId": 69958,
        "DestinationTransactionId": 69957
    }
}
```

```php
array (
  'CheckoutId' => 'f3f454bd-89a9-422f-8fef-5eb9f82ccc8f',
  'ClearingDate' => NULL,
  'Amount' => 33.99,
  'OrderId' => NULL,
  'TransactionId' => 290144,
  'DestinationTransactionId' => 290143,
  'TestMode' => false
)
```

```js
{ 
  Amount: 5,
  CheckoutId: 'a9d7c86a-0d0d-4466-b228-e15584a1315a',
  ClearingDate: '',
  OrderId: 'blah',
  TestMode: false,
  TransactionId: 290144,
  DestinationTransactionId: 290143 
}
```

> Otherwise, you might get an error like this one:

```
{
	"Success":false,
	"Message":"There are insufficient funds for this transaction.",
	"Response":null
}
```

```php
DwollaPHP: An API error was encountered.
Server Message:

Invalid purchase order.
```

```js
"There are insufficient funds for this transaction."
```

```ruby
This method is not yet supported in dwolla-ruby.
```

Finish a PayLater checkout.  This will attempt to create the payment.

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/offsitegateway/checkouts/{id}/complete`

### Response Parameters

Parameter | Description
--------- | -----------
Amount | Payment amount
CheckoutId | Same Checkout ID you passed in!
OrderId | Any order ID provided when the checkout was created.  Otherwise, `null`.
TestMode | Boolean.  True if `Checkout.test` was set to true when checkout was created.
TransactionId | For the resulting payment, this is the [_Sender's_ Transaction ID](#how-transactions-work).
DestinationTransactionId | For the resulting payment, this is the [_Recipient's_ Transaction ID](#how-transactions-work).