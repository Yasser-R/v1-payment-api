# Scheduled Payments

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
  "Notes": "This is a scheduled payment",
  "Status": "scheduled",
  "CreatedDate": "2014-09-12T20:37:37Z",
  "Metadata": {
    "foo": "bar"
  }
}
```

Payments can be scheduled to be created at a future date, up to 3 years into the future.  When a Scheduled Payment is created, no transaction is created (and thus, no funds are transferred) until the `ScheduledDate`. 

Scheduled Payments can be one-time or recurring. Options for recurring payments are flexible.  You can create a recurring payment that pays weekly, monthly, daily, or every 2 weeks, every week on Tuesdays and Fridays, every 12th and 18th and 26th each month, and so on.

If there is an error creating the transaction, the Scheduled Payment will have a `Status` of `failed`.  Only bank-sourced payments are supported at this time.  

### Scheduled Payment Resource

Property | Description
---------|------------
Id | A unique ID for the scheduled payment
ScheduledDate | Date when transaction will be created.  Must be within 3 years from when the scheduled payment was created.  Format: `YYYY-MM-DD`
ExpectedClearingDate | Estimated date when the funds will be available to the recipient.  Typically 1-3 business days after the `ScheduledDate`.
TransactionId | Transaction ID of resulting Transaction.  `null` until the transaction is successfully created.
Amount | Amount of the transaction to be created.
FundingSource | ID of the bank funding source which will fund the transaction
AssumeCosts | Boolean.  Set to `true` if the sender of the funds will assume the $0.25 transaction fee (only applies if transaction is greater than $10.00).
Destination | JSON object describing the recipient.  `Id` is the recipient's Dwolla ID or email address, `Name` is the recipient's full name, `Type` is either `Dwolla` or `Email` and `Image` is a URL to the recipient's avatar, if the recipient is an existing Dwolla user.
Notes | Optional notes field.  Max 250 chars.
Status | Possible values: 'scheduled', 'processed', 'failed'
Metadata | JSON object with max 10 key-value pairs. Keys and values are strings of max length 255. Omitted if `null`, if not provided, or visible to application.  [Read more](#metadata)

## Create Scheduled Payment

```shell
{
  "amount":10,
  "assumeCosts":true,
  "destinationId":"812-114-7461",
  "destinationType":"Dwolla",
  "fundsSource":"33be0db8a99c1a03e01e7c6dd54dff26",
  "notes":"Weekly recurring membership",
  "pin":"7890",
  "scheduleDate":"2015-05-04",
  "metadata":{
    "foo": "bar",
    "bar": "baz"
  },
  "recurrence":{
    "frequency":"weekly",
    "onDays":"2"
  }
}
```
```php
/**
 * Send $300 to a Dwolla ID,
 * on 2018-01-01, from funding source with ID
 * "ashfdjh8f9df89"
 */
print_r($Transactions->schedule('812-197-4121', 300, '2018-01-01', 'ashfdjh8f9df89'));
```
```js
/**
 * Schedule a payment for 300 on 2015-09-09 to 812-111-1234
 */

Dwolla.schedule(cfg.pin, '812-111-1234', 300, '2015-09-09', function(err, data) {
  if (err) {console.log(err);}
  console.log(data);
});
```
```ruby
#   Send money ($1.00) to a Dwolla ID
#   on 2015-09-09 from funding source "abcdef"
pp Dwolla::Transactions.schedule({
                                :pin => @pin,
                                :amount => 1.00,
                                :destinationId => '812-111-1111',
                                :fundsSource => 'abcdef',
                                :scheduleDate => '2015-09-09'}) 
```
```python
# Schedule a payment for 2018-01-01 with
# amount $300

print(transactions.schedule('812-111-1111', 300, '2018-01-01', '5da016f7769bcc1de9998a30d194d5a7'))
```

> Response:

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": "876a72fb-e817-4bec-a781-a97b1c41091e",
        "ScheduledDate": "2015-05-04",
        "ExpectedClearingDate": "2015-05-07",
        "Amount": 10,
        "FundingSource": "33be0db8a99c1a03e01e7c6dd54dff26",
        "AssumeCosts": true,
        "Destination": {
            "Id": "812-114-7461",
            "Name": "WidgetCorp",
            "Type": "Dwolla",
            "Image": "https://dwolla-avatars-uat.s3.amazonaws.com/812-114-7461/dec1fee7"
        },
        "Notes": "Weekly recurring membership",
        "Status": "scheduled",
        "CreatedDate": "2015-05-01T19:51:30Z",
        "Metadata": {
            "foo": "bar",
            "bar": "baz"
        },
        "Recurrence": {
            "Frequency": "Weekly",
            "RepeatEvery": 1,
            "OnDays": "2",
            "NextRunDate": "2015-05-04",
            "Status": "Enabled"
        }
    }
}
```

Create a new Scheduled Payment on behalf of the authorized user.  Initial `Status` will be `scheduled`.  Must fund payment with a bank-funding source. Allows scheduling of a one-time payment, or recurring payments with use of the `recurrence` object.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Scheduled` scope.</aside>

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
recurrence | yes | A recurrence JSON object. [See below](#recurrence-object)
assumeCosts | yes | Boolean.  Set to `true` if the sender of the funds will assume the $0.25 transaction fee (only applies if transaction is greater than $10.00).
notes | yes | Optional note to attach to transaction.  Max 250 chars.
metadata | yes | Optional JSON object of a maximum of 10 key-value pairs (each key and value must be less than 255 characters). [Read more](#metadata)

### Recurrence Object
Parameter | Optional? | Description
--------- | -------   | -----------
frequency | | Either `weekly` or `monthly`.
endDate | yes | Date at which to stop the recurrence.
endAfter | yes | Stop the recurrence after this many payments
repeatEvery | yes | Defaults to `1`.  For example a frequency of `weekly` and repeatEvery set to `2` would mean "pay every 2 weeks".
onDays | | Day of the week or day of the month to initiate payments.  Valid values for `weekly` are `1-7`, where `1` is Sunday and `7` is Saturday.  For monthly, valid values are `1-31` and `-1`.  -1 is relative, indicating the last day of the month.  If you use 29, 30, or 31 and the month doesn't have that day, it will initiate on the last day of the month.  You can also pass combinations of days. `weekly` with onDays = `1,2,3,4,5,6,7` would initiate a payment every day of the week.  For `monthly`, you are not able to combine `28,29,30,31` and/or `-1` as this would cause an overlap.

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid account PIN | The supplied PIN is invalid or incorrect. |
| There was an error scheduling the transaction at this time. | There was an error scheduling this transaction |
| Invalid funding source. | The specified funding source doesn't exist or is invalid. |
| Funding source must be a bank. | The specified funding source must be a bank |
| Notes length is too long. Maximum of 250 character is allowed. | The 'notes' length is too long. |
| This user is unable to receive transactions from provided funding source. | The destination user is unable to receive funds from specified funding source |

## List Scheduled Payments
```php
/**
 * Get all scheduled payments
 */
print_r($Transactions->scheduled());
```
```js
/**
 * Get all scheduled payments.
 */
Dwolla.scheduled(function(err, data) {
  if (err) {console.log(err);}
  console.log(data);
});
```
```ruby
#  Get all scheduled payments
pp Dwolla::Transactions.scheduled() 
```
```python
# Get all scheduled payments
print(transactions.scheduled())
```

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Total": 3,
        "Count": 3,
        "Results": [{
            "Id": "62dc6dcb-cc88-476f-a5c9-ce20e4d9d5af",
            "ScheduledDate": "2015-05-04",
            "ExpectedClearingDate": "2015-05-07",
            "Amount": 5,
            "FundingSource": "33be0db8a99c1a03e01e7c6dd54dff26",
            "AssumeCosts": true,
            "Destination": {
                "Id": "812-114-7461",
                "Name": "WidgetCorp",
                "Type": "Dwolla",
                "Image": "https://uat.dwolla.com/avatars/812-114-7461/dec1fee7"
            },
            "Notes": "Daily recurring membership",
            "Status": "scheduled",
            "CreatedDate": "2015-05-01T19:35:04Z",
            "Recurrence": {
                "Frequency": "Weekly",
                "RepeatEvery": 1,
                "OnDays": "1,2,3,4,5,6,7",
                "NextRunDate": "2015-05-04",
                "Status": "Enabled"
            }
        }, {
            "Id": "c9cb400b-bb58-4d63-b000-662e93d22119",
            "ScheduledDate": "2015-05-04",
            "ExpectedClearingDate": "2015-05-07",
            "Amount": 10,
            "FundingSource": "33be0db8a99c1a03e01e7c6dd54dff26",
            "AssumeCosts": true,
            "Destination": {
                "Id": "812-114-7461",
                "Name": "WidgetCorp",
                "Type": "Dwolla",
                "Image": "https://uat.dwolla.com/avatars/812-114-7461/dec1fee7"
            },
            "Notes": "Weekly recurring membership",
            "Status": "scheduled",
            "CreatedDate": "2015-05-01T19:30:17Z",
            "Recurrence": {
                "Frequency": "Weekly",
                "RepeatEvery": 1,
                "OnDays": "2",
                "NextRunDate": "2015-05-04",
                "Status": "Enabled"
            }
        }, {
            "Id": "35921635-aeb4-4c83-b730-6b61b1749127",
            "ScheduledDate": "2015-06-01",
            "ExpectedClearingDate": "2015-06-04",
            "Amount": 900,
            "FundingSource": "33be0db8a99c1a03e01e7c6dd54dff26",
            "AssumeCosts": true,
            "Destination": {
                "Id": "812-114-7461",
                "Name": "WidgetCorp",
                "Type": "Dwolla",
                "Image": "https://uat.dwolla.com/avatars/812-114-7461/dec1fee7"
            },
            "Notes": "Rent payment",
            "Status": "scheduled",
            "CreatedDate": "2015-05-01T19:57:50Z",
            "Metadata": {
                "foo": "bar",
                "bar": "baz"
            }
        }]
    }
}
```

List the authorized user's scheduled payments which have been created by your application.  

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Scheduled` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions/scheduled?limit=10`

### Optional Parameters

Parameter | Description
----------|------------
status | Filter scheduled payments by their status.  Possible values: `scheduled`, `processed`, `failed`.
limit | Pagination: number of results to return.
skip | Pagination: number of results to skip.
fundingSource | Show only scheduled payments funded by a given funding source ID.

### Response

Parameter | Description
----------|------------
Total | Total number of query results
Count | Number of query results included in response.  Can be less than `Total`, if paginating with `limit` set in query.
Results | JSON array of [Scheduled Payments](#scheduled-payment-resource)

## Retrieve Scheduled Payment

```php
/**
 * Get scheduled payment with ID
 * 'd72b9f82-307e-4a35-9559-7889ea929db7'
 */
print_r($Transactions->scheduledById('d72b9f82-307e-4a35-9559-7889ea929db7'));
```
```js
/**
 * Get scheduled payment with ID 'd72b9f82-307e-4a35-9559-7889ea929db7'
 */
Dwolla.scheduledById('d72b9f82-307e-4a35-9559-7889ea929db7', function(err, data) {
  if (err) {console.log(err);}
  console.log(data);
});
```
```ruby
#   Get scheduled payment with ID
#   'd72b9f82-307e-4a35-9559-7889ea929db7'
pp Dwolla::Transactions.scheduled_by_id('d72b9f82-307e-4a35-9559-7889ea929db7') 
```
```python
# Get scheduled payment with 
# ID 'd72b9f82-307e-4a35-9559-7889ea929db7'
print transactions.scheduledbyid('d72b9f82-307e-4a35-9559-7889ea929db7')
```
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

Retrieve a scheduled payment by its ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Scheduled` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions/scheduled/{id}`

## Edit Scheduled Payment

> Edit the amount:

```shell
{
  "pin": 9999,
  "amount": 16
}
```
```php
/**
 * Edit scheduled payment with ID
 * '08f9b638-0f8e-4e00-abe5-41c8fd24fbe8' to reflect amount 16
 */
print_r($Transactions->editScheduled('08f9b638-0f8e-4e00-abe5-41c8fd24fbe8', ['amount' => 16]));
```
```js
/**
 * Edit scheduled payment with ID '08f9b638-0f8e-4e00-abe5-41c8fd24fbe8' 
 * to specify a new amount of 16
 */
Dwolla.editScheduled('08f9b638-0f8e-4e00-abe5-41c8fd24fbe8', cfg.pin, {amount: 16}, {function(err, data) {
  if (err) {console.log(err);}
  console.log(data);
});
```
```ruby
#   Edit scheduled payment with ID 
#   '08f9b638-0f8e-4e00-abe5-41c8fd24fbe8' to have new amount 16
pp Dwolla::Transactions.edit_scheduled_by_id('08f9b638-0f8e-4e00-abe5-41c8fd24fbe8', {:amount => 16})
```
```python
# Edit scheduled payment with ID
# '08f9b638-0f8e-4e00-abe5-41c8fd24fbe8' to reflect amount 16
print transactions.editscheduledbyid('08f9b638-0f8e-4e00-abe5-41c8fd24fbe8', {'amount': 16})
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

Edit an existing scheduled payment. Partial editing is allowed: just supply the field(s) to change, along with the sender's PIN.  

Note: the `recurrence` field *cannot* be modified.  If you need to change the recurrence of a scheduled payment, delete it and create a new one.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Scheduled` scope.</aside>

### HTTP Request
`PUT https://www.dwolla.com/oauth/rest/transactions/scheduled/{id}`

Parameter | Optional? | Description
----------|-----------|------------
pin | required | Sender's account PIN
amount | yes | Amount of payment.  Must be within the sender's account transaction limit.
destinationId | yes | Recipient's Dwolla ID or email address.  
destinationType | yes | Recipient type: `Dwolla` or `Email`.  Must set this to `Email` if you provided an email as the `destinationId`.
fundsSource | yes | ID of the [Funding Source](#funding-sources) to fund this payment.
scheduleDate | yes | Date to initiate payment.  If the payment is funded by an ACH bank funding source, funds will be available to the recipient typically 1-3 business days after this date.
assumeCosts | yes | Boolean.  Set to `true` if the sender of the funds will assume the $0.25 transaction fee (only applies if transaction is greater than $10.00).
notes | yes | Optional note to attach to transaction.  Max 250 chars.

### Errors
| Error String | Description |
|--------------|-------------|
| Transaction has already been processed. | Unable to edit a transaction that has alredy been processed |
| There was an error updating the scheduled payment. | Error occured when updating the scheduled payment |

## Delete Scheduled Payment

```shell
DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled/d856d2ec-4098-4b3b-b20f-b2561ec85871?pin={PIN}
```
```php
/**
 * Delete scheduled payment with ID
 * 'd856d2ec-4098-4b3b-b20f-b2561ec85871'
 */
print_r($Transactions->deleteScheduledById('d856d2ec-4098-4b3b-b20f-b2561ec85871'));
```
```js
/**
 * Delete scheduled payment with ID 'd856d2ec-4098-4b3b-b20f-b2561ec85871'
 */
Dwolla.deleteScheduledById('d856d2ec-4098-4b3b-b20f-b2561ec85871', function(err, data) {
  if (err) {console.log(err);}
  console.log(data);
});
```
```ruby
#   Delete the scheduled payment with ID 
#   'd856d2ec-4098-4b3b-b20f-b2561ec85871' 
pp Dwolla::Transactions.delete_scheduled_by_id('d856d2ec-4098-4b3b-b20f-b2561ec85871', {:pin => @pin})
```
```python
# Delete scheduled payment with ID
# 'd856d2ec-4098-4b3b-b20f-b2561ec85871'
print transactions.deletescheduledbyid('d856d2ec-4098-4b3b-b20f-b2561ec85871')
```

> If successful, response contains the ID of the deleted scheduled payment:

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": "d856d2ec-4098-4b3b-b20f-b2561ec85871"
}
```

Delete a *pending* scheduled payment created by your application.  Status must be `scheduled`.  Cannot delete a `processed` or `failed` scheduled payment.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Scheduled` scope.</aside>

### HTTP Request
`DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled/{id}?pin={PIN}`

### Querystring params
Parameter |  Description
----------|------------
pin | User's PIN

### Errors
| Error String | Description |
|--------------|-------------|
| Scheduled transaction must not have been processed to delete. | The scheduled payment must be deleted before the `ScheduleDate` |
| There was an error deleting the scheduled payment. | Generic error.  If you're not sure why this happened, contact us. |

## Delete all Scheduled Payments

```shell
DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled?pin={PIN}
```
```php
/**
 * Delete all scheduled payments
 */
print_r($Transactions->deleteAllScheduled());
```
```js
/**
 * Delete all scheduled payments.
 */
Dwolla.deleteAllScheduled(function(err, data) {
  if (err) {console.log(err);}
  console.log(data);
});
```
```ruby
#   Delete all scheduled payments 
pp Dwolla::Transactions.delete_all_scheduled({:pin => @pin})
```
```python
# Delete all scheduled payments
print transactions.deleteallscheduled()
```

> If successful, response contains JSON array of IDs of scheduled payments deleted:

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

Delete **all** of an authorized user's *pending* scheduled payments created by your application.  Will not delete any `processed` or `failed` scheduled payments.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Scheduled` scope.</aside>

### HTTP Request
`DELETE https://www.dwolla.com/oauth/rest/transactions/scheduled?pin={PIN}`

### Querystring params
Parameter |  Description
----------|------------
pin | User's PIN
