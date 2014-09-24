# Funding Sources

```
[some cool ascii art]

[
  {
      "Id": "Balance",
      "Name": "My Dwolla Balance",
      "Type": "",
      "Verified": "true",
      "ProcessingType": ""
  },
  {
      "Id": "c58bb9f7f1d51d5547e1987a2833f4fa",
      "Name": "Donations Collection Fund - Savings",
      "Type": "Savings",
      "Verified": "true",
      "ProcessingType": "ACH"
  },
  {
      "Id": "c58bb9f7f1d51d5547e1987a2833f4fb",
      "Name": "Donations Payout Account - Checking",
      "Type": "Checking",
      "Verified": "true",
      "ProcessingType": "FiSync"
  }
]

```

Dwolla accounts have funding sources, such as linked bank accounts or a line of credit.  Funds can be deposited from a bank funding source (`ACH` or `FiSync`) into an user's account `Balance`, and vice versa, funds can be withdrawn from a user's account balance to a bank funding source.

Payments funded by a real-time funding source such as the user's account balance, a Fi-Sync enabled bank account, or line of `Credit` clear instantly.  On the other hand, transactions funded by ACH bank funding sources require 2-5 business days before the funds will be made available to the recipient.

ACH funding sources can be added and verified via the API.  

### Funding Source Types

| Type | Description | Clearing time |
-------|-------------|---------------|
| Balance | Every Dwolla user has an account balance. Payments funded by a user's balance clear instantly. | Instant |
| ACH | A traditional bank account associated with the account. | 2-5 business days
| FiSync | A linked bank account with a participating FiSync bank.  Currently, only bank accounts with Veridian Credit Union support FiSync. | Instant
| Credit | A line of credit via Dwolla Credit.  Currently, only business accounts that have undergone review may accept credit-funded payments.  | Instant
| Sweep | A special funding source type available to accounts with Sweep functionality. | Instant

## List Funding Sources

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Id": "Balance",
            "Name": "My Dwolla Balance",
            "Type": "",
            "Verified": "true",
            "ProcessingType": ""
        },
        {
            "Id": "c58bb9f7f1d51d5547e1987a2833f4fa",
            "Name": "Donations Collection Fund - Savings",
            "Type": "Savings",
            "Verified": "true",
            "ProcessingType": "ACH"
        },
        {
            "Id": "c58bb9f7f1d51d5547e1987a2833f4fb",
            "Name": "Donations Payout Account - Checking",
            "Type": "Checking",
            "Verified": "true",
            "ProcessingType": "FiSync"
        }
    ]
}
```

Enumerate a user's funding sources.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Funding` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/fundingsources/`

### Request Parameters
The following querystring parameters are optional:

| Param | Description |
|-------|-------------|
| destinationId | Only show funding sources that a particular recipient can accept a payment from.  See the section below.
| destinationType | The type of the recipient.  Possible values: `Email`, `Dwolla`, `Phone`.  Defaults to `Dwolla`

### Filtering funding sources
You can limit the results of this query to only the funding sources that a particular user can receive a payment from.  This is helpful, for instance, if the user account you're trying to send money to has opted-in to not accept payments funded via an ACH funding source.  In another case, only select merchant accounts have the ability to receive a Credit funded transaction, so Credit would not appear in the list of funding sources when filtered.

To do so, provide the `destinationId` and `destinationType` params in your request.  For example, if I only want to see funding sources that the user account ID 812-713-9234 can accept:

`GET https://www.dwolla.com/oauth/rest/fundingsources/?destinationId=812-713-9234&destinationType=Dwolla`

## Get a Funding Source

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "c58bb9f7f1d51d5547e1987a2833f4fb",
        "Name": "Donations Payout Account - Checking",
        "Type": "Checking",
        "Verified": "true",
        "ProcessingType": "FiSync"
    }
}
```

Look up a particular funding source by its funding source ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Funding` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/fundingsources/{id}`

## Withdraw to a funding source
```json

{
    "Success": true,
    "Message": "Success",
    "Response": { 
        "Id": 327843,
        "Amount": 5,
        "Date": "2014-09-05T06:40:56Z",
        "Type": "withdrawal",
        "UserType": "Dwolla",
        "DestinationId": "XXX9999",
        "DestinationName": "Blah",
        "Destination": { 
            "Id": "XXX9999", 
            "Name": "Blah", 
            "Type": "Dwolla", 
            "Image": ""
        },
        "SourceId": "812-742-8722",
        "SourceName": "Cafe Kubal",
        "Source": {
            "Id": "812-742-8722",
            "Name": "Cafe Kubal",
            "Type": "Dwolla",
            "Image": "http://uat.dwolla.com/avatars/812-742-8722" 
        },
        "ClearingDate": "2014-09-08T00:00:00Z",
        "Status": "pending",
        "Notes": null,
        "Fees": null,
        "OriginalTransactionId": null,
        "Metadata": null 
      }
}

```

Withdraw funds from a user's account balance to one of the user's bank funding sources.  A new [Transaction](#transactions) of type `withdrawal` is created as a result.

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/fundingsources/{id}/withdraw`

### Request Parameters
Parameter | Description
----------|------------
amount | Amount to withdraw from balance to funding source
pin | User account PIN

## Deposit from a funding source
```json

{
    "Success": true,
    "Message": "Success",
    "Response": { 
        "Id": 327843,
        "Amount": 5,
        "Date": "2014-09-05T06:40:56Z",
        "Type": "deposit",
        "UserType": "Dwolla",
        "DestinationId": "XXX9999",
        "DestinationName": "Blah",
        "Destination": {
            "Id": "812-742-8722",
            "Name": "Cafe Kubal",
            "Type": "Dwolla",
            "Image": "http://uat.dwolla.com/avatars/812-742-8722" 
        },
        "SourceId": "812-742-8722",
        "SourceName": "Cafe Kubal",
        "Source": { 
            "Id": "XXX9999", 
            "Name": "Blah", 
            "Type": "Dwolla", 
            "Image": ""
        },
        "ClearingDate": "2014-09-08T00:00:00Z",
        "Status": "pending",
        "Notes": null,
        "Fees": null,
        "OriginalTransactionId": null,
        "Metadata": null 
      }
}

```

Deposit funds from a user's bank funding source to the user's account balance.  A new [Transaction](#transactions) of type `deposit` is created as a result.

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/fundingsources/{id}/deposit`

### Request Parameters
Parameter | Description
----------|------------
amount | Amount to withdraw from balance to funding source
pin | User account PIN


## Add new Funding Source

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "34da835f235cd25302ef0c5c1cb1d4b9",
        "Name": "My Checking Account - Checking",
        "Type": "Checking",
        "Verified": false,
        "ProcessingType": "ACH"
    }
}
```

Add a new ACH bank account as a funding source for the authenticated user.  Once created, the funding source will need to be verified before it can be used.  Two micro-deposits will be made, and their amounts must be verified using the [Verify API](#verify-a-funding-source) or manually by the user on [www.dwolla.com](https://www.dwolla.com/).

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/fundingsources/`

### Request Parameters
Parameter | Description
----------|------------
account_number | The bank account number
routing_number | The bank account's routing number.
account_type | Type of bank account: `Checking` or `Savings`.
name | Arbitrary nickname for the funding source

## Verify a Funding Source

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "34da835f235cd25302ef0c5c1cb1d4b9",
        "Name": "My Checking Account - Checking",
        "Type": "Checking",
        "Verified": true,
        "ProcessingType": "ACH"
    }
}
```

Verify the amounts of the 2 micro-deposits sent to a funding source.

### HTTP Request
`POST https://www.dwolla.com/oauth/rest/fundingsources/{id}/verify`

### Request Parameters
Parameter | Description
----------|------------
deposit1 | First amount
deposit2 | Second amount