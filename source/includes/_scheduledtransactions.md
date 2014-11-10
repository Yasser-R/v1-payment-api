# Scheduled Transactions

```always
        _____
     _.'_____`._
   .'.-'  12 `-.`.
  /,' 11      1 `.\
 // 10      /   2 \\
;;         /       ::
|| 9  ----O      3 ||
::                 ;;
 \\ 8           4 //
  \`. 7       5 ,'/
   '.`-.__6__.-'.'
    ((-._____.-))
    _))       ((_
   '--'SSt    '--'

```

```shell
{
  "Id": "3bfaf7fb-b5e9-4a6e-ab09-1ef30d30bbef",
  "ScheduledDate": "2014-09-13",
  "ExpectedClearingDate": "2014-09-18",
  "TransactionId": null,
  "Amount": 10,
  "FundingSource": "3eacc3e3f6da404aa5735911219f277",
  "AssumeCosts": false,
  "Destination": {
      "Id": "812-111-1111",
      "Name": "Jane Doe",
      "Type": "Dwolla",
      "Image": "http://uat.dwolla.com/avatars/812-111-1111"
  },
  "Notes": "This is a scheduled transaction",
  "Status": "scheduled",
  "CreatedDate": "2014-09-12T20:37:37Z",
  "Metadata": {
    "foo": "bar"
  }
}
```

Payments can be scheduled to be created at a future date, up to 3 years into the future.  Only bank-sourced payments are supported at this time.  When a Scheduled Transaction is created, no transaction is created (and thus, no funds are transferred) until the `ScheduledDate`.  If there is an error creating the transaction, the Scheduled Transaction will have a `Status` of `failed`.

### Scheduled Transaction Resource

Property | Description
---------|------------
Id | A unique ID for the scheduled transaction
ScheduledDate | Date when transaction will be created.  Must be within 3 years from when the scheduled transaction was created.  Format: `YYYY-MM-DD`
ExpectedClearingDate | Estimated date when the funds will be available to the recipient.  Typically 1-3 business days after the `ScheduledDate`.
TransactionId | Transaction ID of resulting Transaction.  `null` until the transaction is successfully created.
Amount | Amount of the transaction to be created.
FundingSource | ID of the bank funding source which will fund the transaction
AssumeCosts | Boolean.  Set to `true` if the sender of the funds will assume the $0.25 transaction fee (only applies if transaction is greater than $10.00).
Destination | JSON object describing the recipient.  `Id` is the recipient's Dwolla ID or email address, `Name` is the recipient's full name, `Type` is either `Dwolla` or `Email` and `Image` is a URL to the recipient's avatar, if the recipient is an existing Dwolla user.
Notes | Optional notes field.  Max 250 chars.
Status | Possible values: 'scheduled', 'processed', 'failed'
Metadata | JSON object with max 10 key-value pairs. Keys and values are strings of max length 255. `null` if not provided or visible to application.  [Read more](#metadata)

## Create Scheduled Transaction

```shell
{
  "pin": "9999",
  "destinationId": "gordon@dwolla.com",
  "destinationType": "Email",
  "amount": 300.00,
  "scheduleDate": "2015-09-13",
  "assumeCosts": true,
  "fundsSource": "5da016f7769bcc1de9998a30d194d5a7",
  "notes": "here's this month's rent!",
  "metadata": {
    "foo": "bar"
  }
}
```

> Response:

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "3bfaf7fb-b5e9-4a6e-ab09-1ef30d30bbef",
        "ScheduledDate": "2015-09-13",
        "ExpectedClearingDate": "2015-09-18",
        "TransactionId": null,
        "Amount": 300.00,
        "FundingSource": "5da016f7769bcc1de9998a30d194d5a7",
        "AssumeCosts": false,
        "Destination": {
            "Id": "812-111-1111",
            "Name": "Jane Doe",
            "Type": "Dwolla",
            "Image": "http://www.dwolla.com/avatars/812-111-1111"
        },
        "Notes": "here's this month's rent!",
        "Status": "scheduled",
        "CreatedDate": "2014-09-12T20:37:37Z",
        "Metadata": {
          "foo": "bar"
      }
    }
}
```

Create a new Scheduled Transaction on behalf of the authorized user.  Initial `Status` will be `scheduled`.  Must fund payment with a bank-funding source.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Send` scope.</aside>

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/transactions/scheduled`

Parameter | Optional? | Description
----------|-----------|------------
amount | | Amount of transaction.  Must be within the sender's account transaction limit.
destinationId | | Recipient's Dwolla ID or email address.  
destinationType | yes | Recipient type: `Dwolla` or `Email`.  Must set this to `Email` if you provided an email as the `destinationId`.
pin | | Sender's account PIN
fundsSource | | ID of the [Funding Source](#funding-sources) to fund this payment.
scheduleDate | | Date to initiate payment.  If the transaction is funded by an ACH bank funding source, funds will be available to the recipient typically 1-3 business days after this date.
assumeCosts | | Boolean.  Set to `true` if the sender of the funds will assume the $0.25 transaction fee (only applies if transaction is greater than $10.00).
notes | yes | Optional note to attach to transaction.  Max 250 chars.
metadata | yes | Optional JSON object of a maximum of 10 key-value pairs (each key and value must be less than 255 characters).  [Read more](#metadata)  

## List Scheduled Transactions
```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Total": 2,
        "Count": 2,
        "Results": [
            {
                "Id": "59be7670-343b-46a1-bbb0-7070c2ba1806",
                "ScheduledDate": "2014-09-10",
                "ExpectedClearingDate": "2014-09-15",
                "TransactionId": 329870,
                "Amount": 12,
                "FundingSource": "3eacc3e3f6da404aa5735911219f277",
                "AssumeCosts": true,
                "Destination": {
                    "Id": "812-111-1111",
                    "Name": "Jane Doe",
                    "Type": "Dwolla",
                    "Image": "http://uat.dwolla.com/avatars/812-111-1111"
                },
                "Notes": "scheduled 1",
                "Status": "processed",
                "CreatedDate": "2014-09-09T22:00:03Z"
            },
            {
                "Id": "1d7c80c6-8dcd-4219-8ddc-440909eccc0d",
                "ScheduledDate": "2014-09-10",
                "ExpectedClearingDate": "2014-09-15",
                "TransactionId": 329867,
                "Amount": 1,
                "FundingSource": "3eacc3e3f6da404aa5735911219f277",
                "AssumeCosts": false,
                "Destination": {
                    "Id": "812-111-1111",
                    "Name": "Jane Doe",
                    "Type": "Dwolla",
                    "Image": "http://uat.dwolla.com/avatars/812-111-1111"
                },
                "Notes": "This is a scheduled",
                "Status": "processed",
                "CreatedDate": "2014-09-09T21:58:14Z"
            }
        ]
    }
}
```

List the authorized user's scheduled transactions.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions/scheduled/?limit=10`

### Optional Parameters

Parameter | Description
----------|------------
status | Filter scheduled transactions by their status.  Possible values: `scheduled`, `processed`, `failed`.
limit | Pagination: number of results to return.
skip | Pagination: number of results to skip.

### Response

Parameter | Description
----------|------------
Total | Total number of query results
Count | Number of query results included in response.  Can be less than `Total`, if paginating with `limit` set in query.
Results | JSON array of [Scheduled Transactions](#scheduled-transaction-resource)

## Retrieve Scheduled Transaction

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "d72b9f82-307e-4a35-9559-7889ea929db7",
        "ScheduledDate": "2014-09-11",
        "ExpectedClearingDate": "2014-09-16",
        "TransactionId": 330295,
        "Amount": 100,
        "FundingSource": "3eacc3e3f6da404aa5735911219f277",
        "AssumeCosts": false,
        "Destination": {
            "Id": "812-111-1111",
            "Name": "Jane Doe",
            "Type": "Dwolla",
            "Image": "http://uat.dwolla.com/avatars/812-111-1111"
        },
        "Notes": "This is a scheduled",
        "Status": "processed",
        "CreatedDate": "2014-09-10T21:05:39Z"
    }
}
```

Retrieve a scheduled transaction by its ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions/scheduled/{id}`

## Edit Scheduled Transaction

> Edit the amount:

```shell
{
  "pin": 9999,
  "amount": 16
}
```

> Response:

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "08f9b638-0f8e-4e00-abe5-41c8fd24fbe8",
        "ScheduledDate": "2014-09-11",
        "ExpectedClearingDate": "2014-09-16",
        "TransactionId": null,
        "Amount": 16,
        "FundingSource": "3eacc3e3f6da404aa5735911219f277",
        "AssumeCosts": true,
        "Destination": {
            "Id": "812-111-1111",
            "Name": "Jane Doe",
            "Type": "Dwolla",
            "Image": "http://uat.dwolla.com/avatars/812-111-1111"
        },
        "Notes": "This is a scheduled test update",
        "Status": "scheduled",
        "CreatedDate": "2014-09-10T18:28:20Z"
    }
}
```

Edit an existing scheduled transaction. Partial editing is allowed: just supply the field(s) to change, along with the sender's PIN.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`PUT https://www.dwolla.com/oauth/rest/transactions/scheduled/{id}`

Parameter | Optional? | Description
----------|-----------|------------
pin | required | Sender's account PIN
amount | yes | Amount of transaction.  Must be within the sender's account transaction limit.
destinationId | yes | Recipient's Dwolla ID or email address.  
destinationType | yes | Recipient type: `Dwolla` or `Email`.  Must set this to `Email` if you provided an email as the `destinationId`.
fundsSource | yes | ID of the [Funding Source](#funding-sources) to fund this payment.
scheduleDate | yes | Date to initiate payment.  If the transaction is funded by an ACH bank funding source, funds will be available to the recipient typically 1-3 business days after this date.
assumeCosts | yes | Boolean.  Set to `true` if the sender of the funds will assume the $0.25 transaction fee (only applies if transaction is greater than $10.00).
notes | yes | Optional note to attach to transaction.  Max 250 chars.

## Delete Scheduled Transaction

```shell
DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled/d856d2ec-4098-4b3b-b20f-b2561ec85871
```

> If successful, response contains the ID of the deleted scheduled transaction:

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": "d856d2ec-4098-4b3b-b20f-b2561ec85871"
}
```

Delete a *pending* scheduled transaction created by the authorized user.  Status must be `scheduled`.  Cannot delete a `processed` or `failed` scheduled transaction.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled/{id}`

## Delete all Scheduled Transactions

```shell
DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled
```

> If successful, response contains JSON array of IDs of scheduled transactions deleted:

```shell
{
  "Success": true,
  "Message": "Success",
  "Response": [
    "09e66fa3-1542-40b0-8143-67f72c75ae77",
    "1646c924-c6e0-46f1-b33d-5b999b408cab",
    "48c93710-5cb0-424b-b0b0-3a692a0ba840"
  ]
}
```

Delete **all** of an authorized user's *pending* scheduled transactions.  Will not delete any `processed` or `failed` scheduled transactions.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled`