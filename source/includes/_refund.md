# Refund

```json
POST /oauth/rest/transactions/refund

{
  "amount": 11.25,
  "fundsSource": "Balance",
  "pin": 9999,
  "transactionId": 2427171
}
```

> Response: 

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

A refund creates a new transaction.  This transaction will not incur a $0.25 transaction fee.  The refund amount can be up to the total payment amount, plus any transaction fee assumed by the sender and any facilitator fees assumed by the sender.

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
| notes | yes | Note to attach to the refund.  Visible to refunder and recipient of refund.  Max 255 chars.
| metadata | yes | An optional [metadata](#metadata) object.

### Response Parameters
|Param| Description
|-----|------------
TransactionId | Transaction ID of the newly created refund payment
RefundDate | Timestamp when refund was created
Amount | Amount refunded