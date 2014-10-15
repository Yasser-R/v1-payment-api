# MassPay

```always
$~ cowsay MOOOspay

  _________
< MOOOspay >
  ---------
         \   ^__^ 
          \  (oo)\_______
             (__)\       )\/\
                 ||----w |
                 ||     ||
    
```

MassPay allows you to easily send up to 10,000 payments with a single API request. The payments are funded from a single user's specified funding source and processed asynchronously upon submission. 

Your MassPay `Job` will then be queued and processed.  As the service processes a `Job`, each `Item` is processed one ofter the other, at an average rate of about 0.5 sec. / item.  Therefore, you can expect a 1000-item MassPay Job to be completed in about 8 minutes.

MassPay offers a significant advantage over repeatedly calling the [Send](#send-money) endpoint for each individual transaction, which is the fact that a bank-funded MassPay Job only incurs a single ACH deposit to fund the entire batch of payments.

The alternative approach incurs an ACH deposit from the bank funding source for each individual payment.  Those who used this approach have reported incurring fees from their financial institutions for excessive ACH transactions, which inspired us to build the _Single Deposit_ feature.

## Create Job

```json
{
  "fundsSource": "76fe6c3ff2417eb02a9019c25c9a259d",
  "pin": "9999",
  "userJobId": "some ID",
  "assumeCosts": true,
  "items": [
    {
        "amount": 140.52,
        "destination": "812-713-9234"
    },
    {
        "amount": 100.00,
        "destination": "reflector@dwolla.com",
        "destinationType": "Email",
        "notes": "here's some money!",
        "metadata": {
            "foo": "bar"
        } 
    }
  ]
} 
```

> Response: 

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "47fe2f7c-8d6d-4f98-bcd9-a3100178062f",
        "UserJobId": "testjob",
        "AssumeCosts": true,
        "FundingSource": "Balance",
        "Total": 240.52,
        "Fees": 0,
        "CreatedDate": "2014-04-18T05:49:03Z",
        "Status": "queued",
        "ItemSummary": {
            "Count": 2,
            "Completed": 0,
            "Successful": 0
        }
    }
}
```

Create a new MassPay job.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Send` scope.</aside>

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/masspay/`

### Request Parameters

| Param | Optional? | Description |
|-------|-----------|-------------|
| fundsSource | no | ID of [funding source](#funding-sources) from which to fund the payments
| pin | no | User account PIN
| items | no | An array of `Item` objects.  See below.
| userJobId | yes | An optional custom identifer for this job
| assumeCosts | yes | If `true`, the sender will assume the $0.25 Dwolla fee for each payment, if applicable.  Defaults to `false`.

### Items
Each payment to be created in a MassPay job is represented as a JSON object with the following properties:

| Param | Optional? | Description |
|-------|-----------|-------------|
| amount | no | Payment amount | 
| destination | no | Payment destination.  Can be a Dwolla ID or email address. |
| destinationType | yes | If the destination is an email address, set this to `Email`.  Otherwise, defaults to `Dwolla`.
| notes | yes | A note to attach to the payment.  Max 255 characters.
| metadata | yes | An optional [metadata](#metadata) object.

## List Jobs
```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Id": "643f2db9-5b45-4755-a881-a3100178b6d7",
            "UserJobId": "I've got a job for ya",
            "AssumeCosts": true,
            "FundingSource": "Balance",
            "Total": 0.02,
            "Fees": 0,
            "CreatedDate": "2014-04-18T05:51:34Z",
            "Status": "complete",
            "ItemSummary": {
                "Count": 2,
                "Completed": 2,
                "Successful": 2
            }
        },
        {
            "Id": "634de23d-ab50-4a53-bcd2-a310017794a8",
            "UserJobId": "I've got another job for ya",
            "AssumeCosts": true,
            "FundingSource": "Balance",
            "Total": 2,
            "Fees": 0,
            "CreatedDate": "2014-04-18T05:47:26Z",
            "Status": "complete",
            "ItemSummary": {
                "Count": 2,
                "Completed": 2,
                "Successful": 2
            }
        }
    ]
}
```

List a user's existing MassPay jobs.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/masspay/`

| Optional Param | Description
-----------------|------------
limit | Retrieve up to `limit` number of results
skip | Pagination marker.  Start retrieving results after `skip` number of elements in the set of results.

You can optionally provide the `skip` and `limit` querystring parameters to limit the number of results returned, and to offset the results by `skip` number of items.  For example, to retrieve the first 20 results after the first 5 in the set of results:

`GET https://www.dwolla.com/oauth/rest/masspay/?limit=20&skip=5`


## Retrieve Job

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "643f2db9-5b45-4755-a881-a3100178b6d7",
        "UserJobId": "somejob",
        "AssumeCosts": true,
        "FundingSource": "Balance",
        "Total": 240.52,
        "Fees": 0,
        "CreatedDate": "2014-04-18T05:51:34Z",
        "Status": "complete",
        "ItemSummary": {
            "Count": 2,
            "Completed": 2,
            "Successful": 2
        }
    }
}
```

Look up a particular MassPay job by its ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/masspay/{id}`

## List a Job's Items

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "JobId": "643f2db9-5b45-4755-a881-a3100178b6d7",
            "ItemId": 13,
            "Destination": "reflector@dwolla.com",
            "DestinationType": "email",
            "Amount": 0.01,
            "Status": "success",
            "TransactionId": 4938766,
            "Error": null,
            "CreatedDate": "2014-04-18T05:51:34Z",
            "Metadata": {
                "some meta data":  "hello world!",
                "profound realization": "cats"
            }
        },
        {
            "JobId": "643f2db9-5b45-4755-a881-a3100178b6d7",
            "ItemId": 14,
            "Destination": "812-713-9234",
            "DestinationType": "dwolla",
            "Amount": 0.01,
            "Status": "success",
            "TransactionId": 4938768,
            "Error": null,
            "CreatedDate": "2014-04-18T05:51:34Z",
            "Metadata": null 
        }
    ]
}
```

Retrieve the items for a particular MassPay job.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/masspay/{id}/items`

| Optional Param | Description
-----------------|------------
limit | Retrieve up to `limit` number of results
skip | Pagination marker.  Start retrieving results after `skip` number of elements in the set of results.

You can optionally provide the `skip` and `limit` querystring parameters to limit the number of results returned, and to offset the results by `skip` number of items.  For example, to retrieve the first 20 results after the first 5 in the set of results:

`GET https://www.dwolla.com/oauth/rest/masspay/{id}/items?limit=20&skip=5`

## Retrieve a Job's Item

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "JobId": "643f2db9-5b45-4755-a881-a3100178b6d7",
        "ItemId": 14,
        "Destination": "812-713-9234",
        "DestinationType": "dwolla",
        "Amount": 0.01,
        "Status": "success",
        "TransactionId": 4938768,
        "Error": null,
        "CreatedDate": "2014-04-18T05:51:34Z",
        "Metadata": {
            "some meta data":  "hello world!",
            "profound realization": "cats"
        }
    }
}
```

Retrieve a particular MassPay Job Item, by the Job ID and Item ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/masspay/{job_id}/items/{item_id}`