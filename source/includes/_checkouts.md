# Checkouts

Create a checkout on our [Off-Site Gateway](/dev/pages/gateway#ux). 

Checkout sessions must be completed by the user within __15 minutes__ of their creation. 

There are two types of checkouts:

### Standard Checkout

A standard checkout means the payment is placed automatically and immediately after the user clicks the *Submit* button on the confirmation screen.

### PayLater Checkout

A PayLater checkout means the user is redirected back to your site after clicking *Continue* on the confirmation screen.  No payment is created yet.  

You will have 15 minutes to [complete the Checkout](#complete-a-paylater-checkout) in order to place the payment.

To create a PayLater Checkout, set `checkoutWithApi` to `true`.

## Create a Checkout


```json
{
  "client_id": "JCGQXLrlfuOqdUYdTcLz3rBiCZQDRvdWIUPkw++GMuGhkem9Bo",
  "client_secret": "g7QLwvO37aN2HoKx1amekWi8a2g7AIuPbD5C/JSLqXIcDOxfTr",
  "callback": "http://www.something.com/callback",
  "redirect": "http://www.something.com/return",
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
notes | yes | A note of max 255 chars. to attach to the resulting payment.
facilitatorAmount | yes | Amount of the facilitator fee to override. Only applicable if the facilitator fee feature is enabled. If set to `0`, facilitator fee is disabled for transaction.
metadata | yes | An object containing up to 10 key-value pairs of your choice which is persisted with the resulting transaction.  [Learn about Metadata](#metadata)
orderItems | yes | An array of JSON object(s) which contain a `name`, `description`, `quantity`, and `price`.


## Retrieve a Checkout

Fetch an existing checkout by its `checkoutId` to get its status and details.

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
                "Quantity": 1,
                "Total": 15
            }
        ],
        "Metadata": null
    }
}
```

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/offsitegateway/checkouts/{checkoutId}? client_id={key}&client_secret={secret}`

### Response Parameters
This returns the `CheckoutId`, `Discount`, `Shipping`, `Tax`, `Total`, `OrderItems`, `ProfileId`, `Metadata` of the checkout in addition to some additional information:

Parameter | Description
--------- | -----------
Status | Status of the checkout.  <br>Possible values: `Created`, `ReadyForApiCheckout`, `Expired`, `Completed`
FundingSource | Type of funding source used to complete the checkout.  <br>Possible values: `Balance`, `Bank`, `null`
TransactionId | For the resulting payment, this is the [_Sender's_ Transaction ID](#how-transactions-work).
DestinationTransactionId | For the resulting payment, this is the [_Recipient's_ Transaction ID](#how-transactions-work).

## Complete a PayLater Checkout