# Transaction Stats

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "TransactionsCount": 5,
        "TransactionsTotal": 116.92
    }
}
```

Transaction statistics can be retrieved for the authenticated user.   Currently, the report contains a count of transactions and sum of transactions over a given period of time.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions/stats`

### Request Parameters
Parameter | Optional? | Description
----------|-----------|------------
startDate | yes | Starting date and time for which to select transactions for report. Defaults to 0300 UTC, today.
endDate | yes | Ending date and time for which to select transactions for report.  Defaults to current date and time.