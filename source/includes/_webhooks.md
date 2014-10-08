# Webhooks

Webhook notifications are used to notify your application when particular events occur relating to Transactions or Requests that your application facilitated / created.  There are four kinds of webhook notifications:

- TransactionStatus
- TransactionReturned
- RequestFulfilled
- RequestCancelled

### Security

```js

function validate_webhook_signature(body, secret, signature) {
  var crypto = require('crypto');

  hash = crypto
    .createHmac('sha1', secret)
    .update(body)
    .digest('hex');

  return (hash == signature);
}

```

All webhook notifications include the `X-Dwolla-Signature` header so that you can verify that it originated from Dwolla.  The signature is a SHA-1 HMAC hash of the request body, with the secret key being the application secret (`client_secret`). 

## TransactionStatus

```always
{
    "Id": "4444962",
    "Type": "Transaction",
    "Subtype": "Status",
    "Created": "2014-01-22T16:55:04Z",
    "Triggered": "2014-01-22T16:55:04Z",
    "Value": "processed",
    "Transaction": {
        "Type": "money_sent",
        "Notes": "",
        "Fees": [],
        "Id": 4444962,
        "Source": {
            "Id": "812-687-7049",
            "Name": "Gordon Zheng",
            "Type": "Dwolla"
        },
        "Destination": {
            "Id": "812-713-9234",
            "Name": "Reflector by Dwolla",
            "Type": "Dwolla"
        },
        "Amount": 0.01,
        "SentDate": "2014-01-22T16:55:04Z",
        "ClearingDate": "2014-01-22T16:55:04Z",
        "Status": "processed"
    },
    "Metadata": {
        "InvoiceDate": "12-06-2014",
        "Priority": "High",
        "TShirtSize": "Small",
        "blah": "blah"
    }

}
```

When the status of a Transaction that your application facilitated changes (e.g. transaction clears successfully, or fails, or is cancelled), Dwolla will send a TransactionStatus webhook.

## TransactionReturned

## RequestFulfilled

## RequestCancelled
