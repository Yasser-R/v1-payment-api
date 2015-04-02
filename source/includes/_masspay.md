# MassPay

```shell
$~ cowsay MOOOspay
```
```always
  _________
< MOOOspay >
  ---------
         \   ^__^ 
          \  (oo)\_______
             (__)\       )\/\
                 ||----w |
                 ||     ||
    
```

MassPay allows you to easily send up to 5,000 payments with a single API request. The payments are funded from a single user's specified funding source and processed asynchronously upon submission. 

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
```python
# Create a MassPay job with two items to
# the Balance of the user associated with the current
# OAuth token.

info = masspay.create('Balance',
               {
                   {
                       'amount': 1.00,
                       'destination': '812-197-4121'
                   },
                   {
                       'amount': 2.00,
                       'destination': '812-174-9528'
                   }
               })
```

```ruby
items = [
  {
    :amount => 9.99,
    :destination => 'gordon@dwolla.com',
    :destinationType => 'Email'
  },
  {
    :amount => 30,
    :destination => '812-742-3301',
    :notes => 'money!!!',
    :metadata => {
      'something_optional' => 'foobar'
    }
  }
]

puts Dwolla::MassPay.create({
  :fundsSource => "Balance",
  :pin => 9999,
  :items => items
})
```

```js
items = [
  {
    amount: 0.01,
    destination: 'a@example.com',
    destinationType: 'Email',
    notes: 'for your good work',
    metadata: {
      'arbitrary_info': 'the house tasted good'
    }
  },
  {
    amount: 0.01,
    destination: '812-740-4294',
    destinationType: 'Dwolla',
    notes: 'cause Im jenerus',
    metadata: {
      'anything_you_want': 'blahhhhh'
    }
  },
  {
    amount: 0.01,
    destination: 'a@example.com',
    destinationType: 'email'
  }
];

dwolla.createMassPayJob('Balance', '2908', items, {
  userJobId: 'Some arbitrary id you can specify',
  assumeCosts: true
}, console.log);
```

```php
<?php

/*
 * Send $3.40 to an email address and $12.40 to a Dwolla ID.
 * Assume transaction fees.
 */

$MassPay = new Dwolla\MassPay();
$MassPay->settings->oauth_token = "foo";
$MassPay->settings->pin = 9999;

$items = [
  [
    'destination' => 'gordon@dwolla.com',
    'destinationType' => 'Email',
    'amount' => 3.40,
    'notes' => 'enjoy this money!'
  ],
  [
    'destination' => '812-742-3301',
    'destinationType' => 'Dwolla',
    'amount' => 12.40
  ]
];

$fundsSource = '5da016f7769bcb1de9938a30d194d5a7';

$result = $MassPay->create($fundsSource, $items, [
  'assumeCosts' => true
]);

var_export($result);
?>
```

> Response: 

```php
array (
  'Id' => '8b116429-95c8-428f-be0f-a3ce017cba53',
  'UserJobId' => NULL,
  'AssumeCosts' => true,
  'FundingSource' => '5da016f7769bcb1de9938a30d194d5a7',
  'Total' => 15.80,
  'Fees' => 0.25,
  'CreatedDate' => '2014-10-24T23:06:08Z',
  'Status' => 'queued',
  'ItemSummary' =>
  array (
    'Count' => 2,
    'Completed' => 0,
    'Successful' => 0,
  ),
)
```

```ruby
{
  "Id"            => "d18a2589-409c-4d0d-8404-a3ca005d3bf2",
  "UserJobId"     => nil,
  "AssumeCosts"   => false,
  "FundingSource" => "Balance",
  "Total"         => 39.99,
  "Fees"          => 0,
  "CreatedDate"   => "2014-10-20T05:39:27Z",
  "Status"        => "queued",
  "ItemSummary"   => {
    "Count"      => 2,
    "Completed"  => 0,
    "Successful" => 0
  }
}
```

```js
{ 
  Id: '9ed0f61a-5cc9-4f36-a7c1-a37e0001c55e',
  UserJobId: 'Some arbitrary id you can specify',
  AssumeCosts: true,
  FundingSource: 'Balance',
  Total: 0.03,
  Fees: 0,
  CreatedDate: '2014-08-05T00:01:06Z',
  Status: 'queued',
  ItemSummary: { 
    Count: 3, 
    Completed: 0, 
    Successful: 0 
  } 
}
```

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
| notes | yes | A note to attach to the payment.  Max 250 characters.
| metadata | yes | An optional [metadata](#metadata) object.

### Errors
| Error String | Description |
|--------------|-------------|
| The selected funding source doesn't have enough funds to cover this job's payout. | Insufficient funds to cover total job payout. Select alternative funding source. |

## List Jobs

```php
<?php
$MassPay = new Dwolla\MassPay();
$MassPay->settings->oauth_token = "foo";

$result = $MassPay->listJobs();

var_export($result);
?>
```

```js
dwolla.getMassPayJobs(function(err, response) {
  console.log(response);
});
```
```python
# Get all current MassPay jobs for the
# user associated with the current OAuth token.

print(masspay.listjobs())
```
```ruby
puts Dwolla::MassPay.get
```

> Response: 

```php
array (
  0 => array (
    'Id' => '8b116429-95c8-428f-be0f-a3ce017cba53',
    'UserJobId' => NULL,
    'AssumeCosts' => true,
    'FundingSource' => '5da016f7769bcb1de9938a30d194d5a7',
    'Total' => 15.80,
    'Fees' => 0.25,
    'CreatedDate' => '2014-10-24T23:06:08Z',
    'Status' => 'queued',
    'ItemSummary' =>
    array (
      'Count' => 2,
      'Completed' => 0,
      'Successful' => 0,
    ),
  ),

  1 => array ( ... ),
  2 => array ( ... ),
  ...
)
```

```ruby
[
  {
    "Id"            => "256f7554-9bcb-4399-97d8-a34c00bb20b1",
    "UserJobId"     => "efnwjkefnkwjnknkjnk",
    "AssumeCosts"   => true,
    "FundingSource" => "Balance",
    "Total"         => 0.02,
    "Fees"          => 0.0,
    "CreatedDate"   => "2014-06-16T18:21:18Z",
    "Status"        => "complete",
    "ItemSummary"   => {
      "Count"      => 2,
      "Completed"  => 2,
      "Successful" => 1
    }
  },
  { ... }
]
```

```js
[
   {
      Id: '9ed0f61a-5cc9-4f36-a7c1-a37e0001c55e',
      UserJobId: 'Some arbitrary id you can specify',
      AssumeCosts: true,
      FundingSource: 'Balance',
      Total: 0.03,
      Fees: 0,
      CreatedDate: '2014-08-05T00:01:06Z',
      Status: 'complete',
      ItemSummary: {
         Count: 3,
         Completed: 3,
         Successful: 3
      }
   },
   {
      Id: '68e22e63-c3cb-45e6-bf04-a37201717e5d',
      UserJobId: null,
      AssumeCosts: true,
      FundingSource: '76fe6c3ff2417eb02a9019c25c9a259d',
      Total: 10080000,
      Fees: 504,
      CreatedDate: '2014-07-24T22:25:18Z',
      Status: 'complete',
      ItemSummary: {
         Count: 2016,
         Completed: 2016,
         Successful: 1832
      }
   }
]
```

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

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid limit. | The value for limit is invalid. |
| Invalid skip. | The value for skip is invalid. |

## Retrieve Job

```php
<?php
$MassPay = new Dwolla\MassPay();
$MassPay->settings->oauth_token = "foo";

$result = $MassPay->getJob('8b116429-95c8-428f-be0f-a3ce017cba53');

var_export($result);
?>
```
```python

# Get information about a MassPay
# job with ID 'abcd123456'

print(masspay.getjob("abcd123456"))
```

```ruby
puts Dwolla::MassPay.getJob('68e22e63-c3cb-45e6-bf04-a37201717e5d')
```

```js
var jobId = '68e22e63-c3cb-45e6-bf04-a37201717e5d';

dwolla.getMassPayJob(jobId, function(err,result) {
  console.log(result);
});
```

> Response:

```php
array (
  'Id' => '8b116429-95c8-428f-be0f-a3ce017cba53',
  'UserJobId' => NULL,
  'AssumeCosts' => true,
  'FundingSource' => '5da016f7769bcb1de9938a30d194d5a7',
  'Total' => 15.80,
  'Fees' => 0.25,
  'CreatedDate' => '2014-10-24T23:06:08Z',
  'Status' => 'complete',
  'ItemSummary' =>
  array (
    'Count' => 2,
    'Completed' => 2,
    'Successful' => 2,
  ),
)
```

```ruby
{
  "Id"            => "256f7554-9bcb-4399-97d8-a34c00bb20b1",
  "UserJobId"     => "efnwjkefnkwjnknkjnk",
  "AssumeCosts"   => true,
  "FundingSource" => "Balance",
  "Total"         => 0.02,
  "Fees"          => 0.0,
  "CreatedDate"   => "2014-06-16T18:21:18Z",
  "Status"        => "complete",
  "ItemSummary"   => {
    "Count"      => 2,
    "Completed"  => 2,
    "Successful" => 1
  }
}
```

```js
{
  Id: '68e22e63-c3cb-45e6-bf04-a37201717e5d',
  UserJobId: null,
  AssumeCosts: true,
  FundingSource: '76fe6c3ff2417eb02a9019c25c9a259d',
  Total: 10080000,
  Fees: 504,
  CreatedDate: '2014-07-24T22:25:18Z',
  Status: 'complete',
  ItemSummary: {
     Count: 2016,
     Completed: 2016,
     Successful: 1832
  }
}
```

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

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid job id. | The value for job id is invalid. |

## List a Job's Items

```php
<?php
$MassPay = new Dwolla\MassPay();
$MassPay->settings->oauth_token = "foo";

$result = $MassPay->getJobItems('8b116429-95c8-428f-be0f-a3ce017cba53');

var_export($result);
?>
```

```js
var jobId = '643f2db9-5b45-4755-a881-a3100178b6d7';

dwolla.getMassPayJobItems(jobId, function(err,result) {
  console.log(result);
});
```
```python
# Get items from MassPay job ID with 'abcd1234'
items = masspay.getjobitems("abcd1234")
print(items)
```
```ruby
puts Dwolla::MassPay.getItems('256f7554-9bcb-4399-97d8-a34c00bb20b1')
```

> Response:

```php
array (
  0 =>
  array (
    'JobId' => '8b116429-95c8-428f-be0f-a3ce017cba53',
    'ItemId' => 482764,
    'Destination' => 'gordon@dwolla.com',
    'DestinationType' => 'email',
    'Amount' => 3.4,
    'Status' => 'success',
    'TransactionId' => 347927,
    'Error' => NULL,
    'CreatedDate' => '2014-10-24T23:06:08Z',
    'Metadata' => NULL,
  ),
  1 =>
  array (
    'JobId' => '8b116429-95c8-428f-be0f-a3ce017cba53',
    'ItemId' => 482765,
    'Destination' => '812-742-3301',
    'DestinationType' => 'dwolla',
    'Amount' => 12.4,
    'Status' => 'success',
    'TransactionId' => 347929,
    'Error' => NULL,
    'CreatedDate' => '2014-10-24T23:06:08Z',
    'Metadata' => NULL,
  ),
)
```

```ruby
[
  {
    "JobId"           => "256f7554-9bcb-4399-97d8-a34c00bb20b1",
    "ItemId"          => 17053,
    "Destination"     => "reflector@dwolla.com",
    "DestinationType" => "email",
    "Amount"          => 0.01,
    "Status"          => "success",
    "TransactionId"   => 61966,
    "Error"           => nil,
    "CreatedDate"     => "2014-06-16T18:21:18Z",
    "Metadata"        => {
      "lol" => "foo",
      "bar" => "qux"
    }
  },
  {
    "JobId"           => "256f7554-9bcb-4399-97d8-a34c00bb20b1",
    "ItemId"          => 17054,
    "Destination"     => "812-713-9234",
    "DestinationType" => "dwolla",
    "Amount"          => 0.01,
    "Status"          => "failed",
    "TransactionId"   => nil,
    "Error"           => "The user to send to could not be found.",
    "CreatedDate"     => "2014-06-16T18:21:18Z",
    "Metadata"        => nil
  }
]
```

```js
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
```

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
status | Filter results on [Item status](#item-statuses). Possible values: `notrun`, `success`, `failed`

You can optionally provide the `skip` and `limit` querystring parameters to limit the number of results returned, and to offset the results by `skip` number of items.  For example, to retrieve the first 20 results after the first 5 in the set of results:

`GET https://www.dwolla.com/oauth/rest/masspay/{id}/items?limit=20&skip=5`

### Item Statuses
In the response, each `Item` will contain an item `Status` that corresponds to the success or fail of that particular transaction. A MassPay Job Item can contain a status of either `notrun`, `success`, or `failed`.

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid limit. | The value for limit is invalid. |
| Invalid skip. | The value for skip is invalid. |
| Invalid status. | The value for status is invalid. |

## Retrieve a Job's Item

```js
var jobId = '9ed0f61a-5cc9-4f36-a7c1-a37e0001c55e';
var itemId = '424792';

dwolla.getMassPayJobItem(jobId, itemId, function(err, result) {
  console.log(result);
});
```
```python
# Retrieve item 43432 from job with ID aaad1324

print(masspay.getitem("aaad1324", "43432")
```
```ruby
# Retrieve item 17054 from Job 256f7554-9bcb-4399-97d8-a34c00bb20b1: 

puts Dwolla::MassPay.getItem('256f7554-9bcb-4399-97d8-a34c00bb20b1', '17054')
```

```php
<?php
$MassPay = new Dwolla\MassPay();
$MassPay->settings->oauth_token = "foo";

$result = $MassPay->getItem('8b116429-95c8-428f-be0f-a3ce017cba53', '482765');

var_export($result);
?>
```

> Response:

```php
array (
  'JobId' => '8b116429-95c8-428f-be0f-a3ce017cba53',
  'ItemId' => 482765,
  'Destination' => '812-742-3301',
  'DestinationType' => 'dwolla',
  'Amount' => 12.4,
  'Status' => 'success',
  'TransactionId' => 347929,
  'Error' => NULL,
  'CreatedDate' => '2014-10-24T23:06:08Z',
  'Metadata' => NULL,
)
```

```js
{ 
  JobId: '9ed0f61a-5cc9-4f36-a7c1-a37e0001c55e',
  ItemId: 424792,
  Destination: 'a@example.com',
  DestinationType: 'email',
  Amount: 0.01,
  Status: 'success',
  TransactionId: 290128,
  Error: null,
  CreatedDate: '2014-08-05T00:06:26Z',
  Metadata: null 
}
```

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

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid job id. | The value for job id is invalid. |
| Invalid job item id. | The value for item id is invalid. |