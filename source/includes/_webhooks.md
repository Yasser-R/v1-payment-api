# Webhooks

[Webhook notifications](https://developers.dwolla.com/dev/pages/guides/using_webhooks) are used to notify your application when particular events occur relating to Transactions or Requests that your application facilitated / created.  There are four kinds of webhook notifications:

- TransactionStatus
- TransactionReturned
- RequestFulfilled
- RequestCancelled

Webhook notifications are JSON-encoded POST requests, and will contain the `Content-Type: application/json` header.  There is currently no way to retrieve past Webhook notifications.

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

```shell
POST https://www.example.com/payments/webhook
Content-Type: application/json
X-Dwolla-Signature: d0a74b17a31334edc4e6698c59e460c61bb7f21a

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

```shell
POST https://www.example.com/payments/webhook
Content-Type: application/json
X-Dwolla-Signature: d0a74b17a31334edc4e6698c59e460c61bb7f21a

{
    "Type": "Transaction",
    "Subtype": "Returned",
    "SenderTransactionId": "489977",
    "ReceiverTransactionId": "489978",
    "ReturnCode": "01",
    "ReturnDescription": "Insufficient Funds"
}
```

Notifies your application when a bank-funded transaction facilitated by your application is returned by the financial institution.

## RequestFulfilled

```shell
POST https://www.example.com/requests/callback
Content-Type: application/json
X-Dwolla-Signature: f71c2f49a87b6d0a662f449b7dbb83a5c918e237

{
    "Id": 2254043,
    "Source": {
        "Id": "812-552-5953",
        "Name": "Gordon Zheng",
        "Type": "Dwolla"
    },
    "Destination": {
        "Id": "812-687-7049",
        "Name": "Gordon Zheng",
        "Type": "Dwolla"
    },
    "Amount": 0.0100,
    "Notes": "",
    "DateRequested": "2014-01-21T19:07:12Z",
    "Status": "Paid",
    "Transaction": {
        "RequestId": 2254043,
        "Id": 4439897,
        "Source": {
            "Id": "812-687-7049",
            "Name": "Gordon Zheng",
            "Type": "Dwolla"
        },
        "Destination": {
            "Id": "812-552-5953",
            "Name": "Ben Milne",
            "Type": "Dwolla"
        },
        "Amount": 0.01,
        "SentDate": "2014-01-21T19:07:44Z",
        "ClearingDate": "2014-01-21T19:07:44Z",
        "Status": "processed"
    },
    "DateCancelled": "",
    "SenderAssumeAdditionalFees": false,
    "AdditionalFees": [],
    "Metadata": {
        "InvoiceDate": "12-06-2014",
        "Priority": "High",
        "TShirtSize": "Small",
        "blah": "blah"
    }
}
```

Notifies your application when a Money Request created by your application is fulfilled.

## RequestCancelled

Notifies your application when a Money Request created by your application is cancelled.

```shell
POST https://www.example.com/requests/callback
Content-Type: application/json
X-Dwolla-Signature: f71c2f49a87b6d0a662f449b7dbb83a5c918e237

{
    "Id": 2254042,
    "Source": {
        "Id": "812-552-5953",
        "Name": "Gordon Zheng",
        "Type": "Dwolla"
    },
    "Destination": {
        "Id": "812-687-7049",
        "Name": "Ben Milne",
        "Type": "Dwolla"
    },
    "Amount": 0.0100,
    "Notes": "",
    "DateRequested": "2014-01-21T19:06:51Z",
    "Status": "Cancelled",
    "CancelledBy": "812-687-7049",
    "DateCancelled": "2014-01-21T19:07:30Z",
    "SenderAssumeAdditionalFees": false,
    "AdditionalFees": [],
    "Metadata": {
        "InvoiceDate": "12-06-2014",
        "Priority": "High",
        "TShirtSize": "Small",
        "blah": "blah"
    }
}
```

