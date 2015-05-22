# Transactions
```shell
{
    "Id": 4443952,
    "Amount": 0.01,
    "Date": "2014-01-22T13:09:38Z",
    "Type": "money_sent",
    "UserType": "Dwolla",
    "DestinationId": "812-713-9234",
    "DestinationName": "Peter Douglas",
    "Destination": {
        "Id": "812-713-9234",
        "Name": "Peter Douglas",
        "Type": "Dwolla",
        "Image": "http://uat.dwolla.com/avatars/812-713-9234"
    },
    "SourceId": "812-629-2658",
    "SourceName": "Tom Walker",
    "Source": {
        "Id": "812-713-9234",
        "Name": "Tom Walker",
        "Type": "Dwolla",
        "Image": "http://uat.dwolla.com/avatars/812-713-9234"
    },
    "ClearingDate": "",
    "Status": "processed" ,
    "Notes": "don't spend it all in one place!",
    "OriginalTransactionId": null,
    "Metadata": null ,
    "Fees": null
}
```
```
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$$,,,$$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$"OOO"$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$!OOO!$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$,"OOO",$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$,"OOOOOOO",$$$$$$$$$$$$$
$$$$$$$$$$$$!"OOOOOOOOOOO"!$$$$$$$$$$$
$$$$$$$$$$,"OOOOOOOOOOOOOOO",$$$$$$$$$
$$$$$$$$,1OOOOOOOOOOOOOOOOOOO7,$$$$$$$
$$$$$$,!OOOOOOOOOOOOOOOOOOOOOO"$$$$$$$
$$$$$$!OOOOOOOOO/""""'\OOOOOOOO1$$$$$$
$$$$$$"OOOOOOOOOO"$$$$!OOOOOOOO"$$$$$$
$$$$$$"OOOOOOOOOOO!$$$",OOOOOOO!$$$$$$
$$$$$$$",OOOOOOOOOO",$$$"""""""$$$$$$$
$$$$$$$$"!OOOOOOOOOOO",$$$$$$$$$$$$$$$
$$$$$$$$$$",OOOOOOOOOOO",$$$$$$$$$$$$$
$$$$$$$$$$$$"OOOOOOOOOOOO",$$$$$$$$$$$
$$$$$$$$$$$$$"!OOOOOOOOOOOO"+$$$$$$$$$
$$$$$$$$$$$$$$$"==OOOOOOOOOOO,$$$$$$$$
$$$$$$$$,,,,,,$$$$",OOOOOOOOOO",$$$$$$
$$$$$$,"OOOOOO',$$$$",OOOOOOOOO!$$$$$$
$$$$$$1OOOOOOOO."~~~"OOOOOOOOOO!$$$$$$
$$$$$$!OOOOOOOOOOOOOOOOOOOOOOO1$$$$$$$
$$$$$$",OOOOOOOOOOOOOOOOOOOOO,"$$$$$$$
$$$$$$$",OOOOOOOOOOOOOOOOOOO!$$$$$$$$$
$$$$$$$$$1OOOOOOOOOOOOOOOO,1$$$$$$$$$$
$$$$$$$$$$"~~,OOOOOOOOOO,"$$$$$$$$$$$$
$$$$$$$$$$$$$$"""1OOO1"'$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$!OOO!$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$,OOO,$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$$"""$$$$$$$$$$$$$$$$$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
$$$$$ "DWOLLA DWOLLA BILL YA'LL" $$$$$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
```

Any movement of money, whether it's a payment, fee, withdrawal, or adding funds to a Dwolla balance, is recorded as an individual Transaction.  For instance, adding money from a funding source into your Dwolla account balance creates a `Transaction` of type `deposit`.

### How Transactions Work
When a payment is sent, 2 or more transactions are created.  At least one is created to credit the recipient's account balance, and another one to debit the sender's account balance.  

As a simple example, let's say Bob sends $5 to Alice from his account balance.  From this action, two `Transactions` are created:

 1. Transaction `512010`: credits Alice's account with $5
 2. Transaction `512011`: deducts $5 from Bob's account

It's important to keep in mind that when Bob looks at this payment, he will see the credit transaction's ID: `512010`.  This is called the __Sender's Transaction ID__. 

Similarly, when Alice looks up this payment, she will see a different transaction of ID `512011`, which we call the __Recipient's Transaction ID__.

#### Additional Transactions
For a particular payment, additional transactions are created: 

 - if the payment is bank sourced, to fund the payment
 - if the payment is over $10 it will incur a $0.25 _Dwolla Fee_
 - if there is a facilitator fee

Learn how to correlate the sender's and recipient's transaction ID with [this guide](https://developers.dwolla.com/dev/pages/guides/correlate_transactions).

### Transaction Types

Type | Description
-----------------|------------
money_sent | Payment sent by a user
money_received | Payment received by a user
deposit | Funds deposited from a Funding Source (e.g. bank account) to the user's Dwolla account balance
withdrawal | Funds withdrawn from the user's Dwolla account balance to an associated Funding Source
fee | Transaction fee of $0.25 or Facilitator Fee

### Transaction Statuses

Status | Description
-------|------------
pending | Bank-funded payment awaiting clearance; typically requires 3 - 5 business days to clear.
processed | Transaction has cleared successfully.
failed | Transaction failed to clear successfully.  Usually, this is a result of an ACH failure (insufficient funds, etc.).
cancelled | A pending transaction has been canceled, and will not process further.
reclaimed | The payment was returned to the sender after being unclaimed by the recipient for a period of time.

<aside class="notice">The initial transaction state for all real-time (e.g. Balance, FiSync, Credit) funded transfers will always be "processed".</aside>

### Transaction Resource
```always
{
  "Amount": 1,
  "Date": "2014-01-22T13:11:10Z",
  "UserType": "Email",
  "DestinationId": "bob@dwolla.com",
  "DestinationName": "bob@dwolla.com,
  "Destination": {
      "Id": "bob@dwolla.com",
      "Name": "bob@dwolla.com",
      "Type": "Email",
      "Image": ""
  },
  "Id": 12345,
  "SourceId": "812-111-2222",
  "SourceName": "Alice",
  "Source": {
      "Id": "812-111-2222",
      "Name": "Alice",
      "Type": "Dwolla",
      "Image": "http://uat.dwolla.com/avatars/812-111-2222"
  },
  "Type": "money_sent",
  "Status": "processed" ,
  "ClearingDate": "",
  "Notes": "Thank you for lunch!",
  "OriginalTransactionId": null,
  "Metadata": {
      "some meta info":  "foobar",
      "aProfoundRealization": "pretzels"
  },
  "Fees": [
      {
          "Id": 1646163,
          "Amount": 0.1,
          "Type": "Facilitator Fee"
      }
  ]
}
```

Property | Description
---------|-------------
Amount | Amount of funds involved.
Date | Timestamp of when the transaction was created.  ISO-8601 format.
UserType | Type of destination.  Can be `Dwolla` if money was sent to a Dwolla ID, `Email`, or `PhoneNumber`.
DestinationId | Dwolla ID, email address, or phone number of recipient.
DestinationName | Full name of recipient Dwolla user, if available.  Otherwise, email or phone number of non-user.
Destination | JSON object describing the DestinationId, DestinationName, recipient type (possible values: `Dwolla`, `Email`, `Phone`), and avatar URL of recipient, if available.
Id | Unique Transaction ID.  Incrementally generated integer.  Currently, Transaction IDs are 7 digits long in production, but will eventually grow longer.
SourceId | Dwolla ID of the sender's account
SourceName | Full name of the sender
Source | JSON object describing the SourceId, DestinationName, and avatar URL of sender, if available.  `Type` will always be `Dwolla`.
Type | Transaction type [See above](#transaction-types)
Status | Transaction Status.  [See above](#transaction-statuses)
ClearingDate | For bank funded payments, this is the expected clearing date.  Otherwise, for real time payments (e.g. funded by `Balance`), this will be an empty string: `""`
Notes | Note attached to the payment.  Max 250 characters.
OriginalTransactionId | If the transaction is a refund, this is the transaction ID of the original transaction which was refunded. Otherwise, `null`.
Metadata | JSON object with max 10 key-value pairs. Keys and values are strings of max length 255. `null` if not provided or visible to application.  [Read more](#metadata)
Fees | Array of any facilitator fees or transaction fees incurred by this payment.  Otherwise, `null`.

## List a user's transactions

```js
/**
 * Retrieve the 10 most recent transactions for
 * the authorized user
 */

Dwolla.transactions(function(err, data) {
  console.log(data);
});
```

```ruby
#   Get 10 most recent transactions
puts Dwolla::Transactions.get
```

```php
<?php
$Transactions = new Dwolla\Transactions();
$Transactions->settings->oauth_token = "foo";

$result = $Transactions->get();

var_export($result);
?>
```

```python
# Fetch the 10 most recent transactions
print(transactions.get())
```

> Response: 

```php
array (
  0 =>
  array (
    'Id' => 346099,
    'Amount' => 5,
    'Date' => '2014-10-22T19:51:01Z',
    'Type' => 'deposit',
    'UserType' => 'Dwolla',
    'DestinationId' => '812-742-8722',
    'DestinationName' => 'Cafe Kubal',
    'Destination' =>
    array (
      'Id' => '812-742-8722',
      'Name' => 'Cafe Kubal',
      'Type' => 'Dwolla',
      'Image' => 'http://uat.dwolla.com/avatars/812-742-8722',
    ),
    'SourceId' => '33be0db8a99c1a03e01e7c6dd54dff26',
    'SourceName' => 'Blah',
    'Source' =>
    array (
      'Id' => '33be0db8a99c1a03e01e7c6dd54dff26',
      'Name' => 'Blah',
      'Type' => 'Dwolla',
      'Image' => '',
    ),
    'ClearingDate' => '2014-10-28T00:00:00Z',
    'Status' => 'pending',
    'Notes' => NULL,
    'Fees' => NULL,
    'OriginalTransactionId' => NULL,
    'Metadata' => NULL,
  ),
  1 =>
  array (
  ...
  ),
)
```
```python
[
    {
        "Id": 113533,
        "Amount": 5.50,
        "Date": "2014-12-13T05:07:15Z",
        "Type": "money_sent",
        "UserType": "Email",
        "DestinationId": "812-197-4121",
        "DestinationName": "Some Name",
        "Destination": {
            "Id": "812-197-4121",
            "Name": "Some Name",
            "Type": "dwolla",
            "Image": ""
        },
        "SourceId": "812-202-3784",
        "SourceName": "David Stancu",
        "Source": {
            "Id": "812-202-3784",
            "Name": "David Stancu",
            "Type": "Dwolla",
            "Image": "https://dwolla-avatars.s3.amazonaws.com/812-202-3784/ac045522"
        },
        "ClearingDate": "",
        "Status": "cancelled",
        "Notes": "",
        "Fees": null,
        "OriginalTransactionId": null,
        "Metadata": null
    },
    ...
]
```
```ruby
[
  {
    "Id"                    => 331506,
    "Amount"                => 15.25,
    "Date"                  => "2014-09-17T18:18:46Z",
    "Type"                  => "withdrawal",
    "UserType"              => "Dwolla",
    "DestinationId"         => "33be0db8a99c1a03e01e7c6dd54dff26",
    "DestinationName"       => "Blah",
    "Destination"           => {
      "Id"    => "33be0db8a99c1a03e01e7c6dd54dff26",
      "Name"  => "Blah",
      "Type"  => "Dwolla",
      "Image" => ""
    },
    "SourceId"              => "812-742-8722",
    "SourceName"            => "Cafe Kubal",
    "Source"                => {
      "Id"    => "812-742-8722",
      "Name"  => "Cafe Kubal",
      "Type"  => "Dwolla",
      "Image" => "http://uat.dwolla.com/avatars/812-742-8722"
    },
    "ClearingDate"          => "2014-09-19T00:00:00Z",
    "Status"                => "pending",
    "Notes"                 => nil,
    "Fees"                  => nil,
    "OriginalTransactionId" => nil,
    "Metadata"              => nil
  },
  { ... },
  { ... }
]
```

```js
[
   {
      "Id":328979,
      "Amount":10.01,
      "Date":"2014-09-09T04:49:41Z",
      "Type":"money_sent",
      "UserType":"Dwolla",
      "DestinationId":"812-232-2342",
      "DestinationName":"Joe Schmoe",
      "Destination":{
         "Id":"812-232-2342",
         "Name":"Joe Schmoe",
         "Type":"Dwolla",
         "Image":"http://uat.dwolla.com/avatars/812-232-2342"
      },
      "SourceId":"812-742-8722",
      "SourceName":"Cafe Kubal",
      "Source":{
         "Id":"812-742-8722",
         "Name":"Cafe Kubal",
         "Type":"Dwolla",
         "Image":"http://uat.dwolla.com/avatars/812-742-8722"
      },
      "ClearingDate":"",
      "Status":"processed",
      "Notes":"",
      "Fees":null,
      "OriginalTransactionId":null,
      "Metadata":null
  },
  { 
    ... 
  },
  { 
    ... 
  }
]
```

```json
{
  "Success": true,
  "Message": "Success",
  "Response": [
    {
      "Id": 5569196,
      "Amount": 0.01,
      "Date": "2014-08-13T05:17:15Z",
      "Type": "money_sent",
      "UserType": "Email",
      "DestinationId": "ejfwefjwfk02022@gmail.com",
      "DestinationName": "ejfwefjwfk02022@gmail.com",
      "Destination": {
        "Id": "ejfwefjwfk02022@gmail.com",
        "Name": "ejfwefjwfk02022@gmail.com",
        "Type": "Email",
        "Image": ""
      },
      "SourceId": "812-687-7049",
      "SourceName": "Gordon Zheng",
      "Source": {
        "Id": "812-687-7049",
        "Name": "Gordon Zheng",
        "Type": "Dwolla",
        "Image": "https://dwolla-avatars.s3.amazonaws.com/812-687-7049/ac044552"
      },
      "ClearingDate": "",
      "Status": "cancelled",
      "Notes": "",
      "Fees": null,
      "OriginalTransactionId": null,
      "Metadata": null
    },
    ...
  ]
}
```

Retrieve the transaction history of the authenticated user.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions`

#### Example:
To get the latest 200 transactions of type `money_sent`, `fee`, and `deposit`:

`GET https://www.dwolla.com/oauth/rest/transactions?types=money_sent,fee,deposit&limit=200`

### Request Parameters
Parameter | Description
----------|------------
types | Types of transactions to return.  By default, all types returned.  Possible values: `money_sent`, `money_received`, `deposit`, `withdrawal`, `fee`.  Delimited by `,`
limit | Number of transactions to return.  Max 200.  Defaults to 10.
skip | Number of transactions to skip.
sinceDate | Earliest date and time for which to retrieve transactions.  Must be ISO-8601 formatted.  Example: `2014-08-13T21:05:23.797Z`
endDate | Latest date and time for which to retrieve transactions.  Must be ISO-8601 formatted.  Example: `2014-08-13T21:05:23.797Z`

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid since date. | Date specified to retrieve transactions is invalid |
| Invalid end date. | Date specified to retrieve transactions is invalid |
| Invalid limit. | Invalid number of transactions to return |
| Invalid skip. | Invalid number of transactions to skip |

## List an app's transactions 

```js
/**
 * Retrieve the 10 recentmost transactions which
 * have been facilitated by this application.
 */

Dwolla.transactionsByApp(function(err, data) {
  console.log(data);
});
```

```php
<?php
$Transactions = new Dwolla\Transactions();
$Transactions->settings->client_id = $apiKey;
$Transactions->settings->client_secret = $apiSecret;

// set access token to null and get() will use key and secret
$Transactions->settings->oauth_token = null;

$result = $Transactions->get();

var_export($result);
?>
```

```ruby
puts Dwolla::Transactions.get(nil, {}, false)
```

```python
print(transactions.get())
```

> Response:

```php
array (
  0 =>
  array (
    'Id' => 346099,
    'Amount' => 5,
    'Date' => '2014-10-22T19:51:01Z',
    'Type' => 'deposit',
    'UserType' => 'Dwolla',
    'DestinationId' => '812-742-8722',
    'DestinationName' => 'Cafe Kubal',
    'Destination' =>
    array (
      'Id' => '812-742-8722',
      'Name' => 'Cafe Kubal',
      'Type' => 'Dwolla',
      'Image' => 'http://uat.dwolla.com/avatars/812-742-8722',
    ),
    'SourceId' => '33be0db8a99c1a03e01e7c6dd54dff26',
    'SourceName' => 'Blah',
    'Source' =>
    array (
      'Id' => '33be0db8a99c1a03e01e7c6dd54dff26',
      'Name' => 'Blah',
      'Type' => 'Dwolla',
      'Image' => '',
    ),
    'ClearingDate' => '2014-10-28T00:00:00Z',
    'Status' => 'pending',
    'Notes' => NULL,
    'Fees' => NULL,
    'OriginalTransactionId' => NULL,
    'Metadata' => NULL,
  ),
  1 =>
  array (
  ...
  ),
)
```

```ruby
[
  {
    "Id"                    => 331506,
    "Amount"                => 15.25,
    "Date"                  => "2014-09-17T18:18:46Z",
    "Type"                  => "withdrawal",
    "UserType"              => "Dwolla",
    "DestinationId"         => "33be0db8a99c1a03e01e7c6dd54dff26",
    "DestinationName"       => "Blah",
    "Destination"           => {
      "Id"    => "33be0db8a99c1a03e01e7c6dd54dff26",
      "Name"  => "Blah",
      "Type"  => "Dwolla",
      "Image" => ""
    },
    "SourceId"              => "812-742-8722",
    "SourceName"            => "Cafe Kubal",
    "Source"                => {
      "Id"    => "812-742-8722",
      "Name"  => "Cafe Kubal",
      "Type"  => "Dwolla",
      "Image" => "http://uat.dwolla.com/avatars/812-742-8722"
    },
    "ClearingDate"          => "2014-09-19T00:00:00Z",
    "Status"                => "pending",
    "Notes"                 => nil,
    "Fees"                  => nil,
    "OriginalTransactionId" => nil,
    "Metadata"              => nil
  },
  { ... },
  { ... }
]
```

```js
[
   {
      "Id":328979,
      "Amount":10.01,
      "Date":"2014-09-09T04:49:41Z",
      "Type":"money_sent",
      "UserType":"Dwolla",
      "DestinationId":"812-232-2342",
      "DestinationName":"Joe Schmoe",
      "Destination":{
         "Id":"812-232-2342",
         "Name":"Joe Schmoe",
         "Type":"Dwolla",
         "Image":"http://uat.dwolla.com/avatars/812-232-2342"
      },
      "SourceId":"812-742-8722",
      "SourceName":"Cafe Kubal",
      "Source":{
         "Id":"812-742-8722",
         "Name":"Cafe Kubal",
         "Type":"Dwolla",
         "Image":"http://uat.dwolla.com/avatars/812-742-8722"
      },
      "ClearingDate":"",
      "Status":"processed",
      "Notes":"",
      "Fees":null,
      "OriginalTransactionId":null,
      "Metadata":null
  },
  { 
    ... 
  },
  { 
    ... 
  }
]
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": [
        {
            "Id": 5569196,
            "Amount": 0.01,
            "Date": "2014-08-13T05:17:15Z",
            "Type": "money_sent",
            "UserType": "Email",
            "DestinationId": "ejfwefjwfk02022@gmail.com",
            "DestinationName": "ejfwefjwfk02022@gmail.com",
            "Destination": {
                "Id": "ejfwefjwfk02022@gmail.com",
                "Name": "ejfwefjwfk02022@gmail.com",
                "Type": "Email",
                "Image": ""
            },
            "SourceId": "812-687-7049",
            "SourceName": "Gordon Zheng",
            "Source": {
                "Id": "812-687-7049",
                "Name": "Gordon Zheng",
                "Type": "Dwolla",
                "Image": "https://dwolla-avatars.s3.amazonaws.com/812-687-7049/ac044552"
            },
            "ClearingDate": "",
            "Status": "cancelled",
            "Notes": "",
            "Fees": null,
            "OriginalTransactionId": null,
            "Metadata": null
        },
        ...
    ]
}
```

List the transactions which your application has created / facilitated.

### HTTP Request
`GET https://www.dwolla.com/oauth/rest/transactions?client_id={}&client_secret={}`

#### Example:
To get the latest 200 transactions of type `money_sent`, `fee`, and `deposit`:

`GET https://www.dwolla.com/oauth/rest/transactions?types=money_sent,fee,deposit&limit=200`

### Request Parameters
Parameter | Description
----------|------------
client_id | Application key
client_secret | Application secret
types | Types of transactions to return.  By default, all types returned.  Possible values: `money_sent`, `money_received`, `deposit`, `withdrawal`, `fee`.  Delimited by `,`
limit | Number of transactions to return.  Max 200.  Defaults to 10.
skip | Number of transactions to skip.
sinceDate | Earliest date and time for which to retrieve transactions.  Must be ISO-8601 formatted.  Example: `2014-08-13T21:05:23.797Z`
endDate | Latest date and time for which to retrieve transactions.  Must be ISO-8601 formatted.  Example: `2014-08-13T21:05:23.797Z`

## Get a specific transaction

```js
/**
 * Retrieve transaction ID '12345'
 */

Dwolla.transactionById('12345', function(err, data) {
  console.log(data);
});
```

```ruby
#   Retrieve a transaction by ID
puts Dwolla::Transactions.get(331506)
```

```python
# Get info for transaction ID 123456
print(transactions.info('123456'))
```

```php
<?php
$Transactions = new Dwolla\Transactions();

// you may provide key and secret...
$Transactions->settings->client_id = $apiKey;
$Transactions->settings->client_secret = $apiSecret;

// OR an access token... 
$Transactions->settings->oauth_token = "foo";

$result = $Transactions->info('346099');

var_export($result);
?>

```

> Response:

```php
array (
  'Id' => 346099,
  'Amount' => 5,
  'Date' => '2014-10-22T19:51:01Z',
  'Type' => 'deposit',
  'UserType' => 'Dwolla',
  'DestinationId' => '812-742-8722',
  'DestinationName' => 'Cafe Kubal',
  'Destination' =>
  array (
    'Id' => '812-742-8722',
    'Name' => 'Cafe Kubal',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-742-8722',
  ),
  'SourceId' => '33be0db8a99c1a03e01e7c6dd54dff26',
  'SourceName' => 'Blah',
  'Source' =>
  array (
    'Id' => '33be0db8a99c1a03e01e7c6dd54dff26',
    'Name' => 'Blah',
    'Type' => 'Dwolla',
    'Image' => '',
  ),
  'ClearingDate' => '2014-10-28T00:00:00Z',
  'Status' => 'pending',
  'Notes' => NULL,
  'Fees' => NULL,
  'OriginalTransactionId' => NULL,
  'Metadata' => NULL,
)
```
```python
Return:
    {
        "Id": 123456,
        "Amount": 5.50,
        "Date": "2014-12-13T05:07:15Z",
        "Type": "money_sent",
        "UserType": "Email",
        "DestinationId": "812-197-4121",
        "DestinationName": "Some Name",
        "Destination": {
            "Id": "812-197-4121",
            "Name": "Some Name",
            "Type": "dwolla",
            "Image": ""
        },
        "SourceId": "812-202-3784",
        "SourceName": "David Stancu",
        "Source": {
            "Id": "812-202-3784",
            "Name": "David Stancu",
            "Type": "Dwolla",
            "Image": "https://dwolla-avatars.s3.amazonaws.com/812-202-3784/ac045522"
        },
        "ClearingDate": "",
        "Status": "cancelled",
        "Notes": "",
        "Fees": null,
        "OriginalTransactionId": null,
        "Metadata": null
    }
```

```ruby
{
    "Id"                    => 331506,
    "Amount"                => 15.25,
    "Date"                  => "2014-09-17T18:18:46Z",
    "Type"                  => "withdrawal",
    "UserType"              => "Dwolla",
    "DestinationId"         => "33be0db8a99c1a03e01e7c6dd54dff26",
    "DestinationName"       => "Blah",
    "Destination"           => {
      "Id"    => "33be0db8a99c1a03e01e7c6dd54dff26",
      "Name"  => "Blah",
      "Type"  => "Dwolla",
      "Image" => ""
    },
    "SourceId"              => "812-742-8722",
    "SourceName"            => "Cafe Kubal",
    "Source"                => {
      "Id"    => "812-742-8722",
      "Name"  => "Cafe Kubal",
      "Type"  => "Dwolla",
      "Image" => "http://uat.dwolla.com/avatars/812-742-8722"
    },
    "ClearingDate"          => "2014-09-19T00:00:00Z",
    "Status"                => "pending",
    "Notes"                 => nil,
    "Fees"                  => nil,
    "OriginalTransactionId" => nil,
    "Metadata"              => nil
  }
```

```js
{
      "Id":328979,
      "Amount":10.01,
      "Date":"2014-09-09T04:49:41Z",
      "Type":"money_sent",
      "UserType":"Dwolla",
      "DestinationId":"812-232-2342",
      "DestinationName":"Joe Schmoe",
      "Destination":{
         "Id":"812-232-2342",
         "Name":"Joe Schmoe",
         "Type":"Dwolla",
         "Image":"http://uat.dwolla.com/avatars/812-232-2342"
      },
      "SourceId":"812-742-8722",
      "SourceName":"Cafe Kubal",
      "Source":{
         "Id":"812-742-8722",
         "Name":"Cafe Kubal",
         "Type":"Dwolla",
         "Image":"http://uat.dwolla.com/avatars/812-742-8722"
      },
      "ClearingDate":"",
      "Status":"processed",
      "Notes":"",
      "Fees":null,
      "OriginalTransactionId":null,
      "Metadata":null
  }
```

```json
{
  "Success": true,
  "Message": "Success",
  "Response": {
    "Id": 331506,
    "Amount": 15.25,
    "Date": "2014-09-17T18:18:46Z",
    "Type": "withdrawal",
    "UserType": "Dwolla",
    "DestinationId": "33be0db8a99c1a03e01e7c6dd54dff26",
    "DestinationName": "Blah",
    "Destination": {
      "Id": "33be0db8a99c1a03e01e7c6dd54dff26",
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
    "ClearingDate": "2014-09-19T00:00:00Z",
    "Status": "pending",
    "Notes": null,
    "Fees": null,
    "OriginalTransactionId": null,
    "Metadata": null
  }
}
```

Look up a particular transaction by its ID (Either Sender's or Recipient's.  Read about transaction IDs [here](#how-transactions-work).)  Either an OAuth access token can be supplied, or if your API application facilitated this transaction, you can supply an application key and secret instead.

<aside class="reminder">This endpoint can take either an [an OAuth access token](#authentication) with the `Transactions` scope or an API application's `client_id` and `client_secret`.</aside>

### HTTP Request
To fetch a transaction which belongs to the authorized user:

`GET https://www.dwolla.com/oauth/rest/transactions/{id}`

or to fetch a transaction which belongs to an application:

`GET https://www.dwolla.com/oauth/rest/transactions/{id}?client_id={}&client_secret={}`

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid transaction ID. | The specified transaction ID is empty, missing, or invalid. |
| Transaction not found for account. | Could not find a transaction with the specified ID for the given OAuth token. |

## Search transactions

```shell
GET https://www.dwolla.com/oauth/rest/transactions/search?limit=20
```

> Response:

```shell
{
  "Success": true,
  "Message": "Success",
  "Response": {
    "TotalHits": 13502,
    "Results": [
      {
        "Id": 346099,
        "Amount": 5,
        "Date": "2014-10-22T19:51:01Z",
        "Type": "deposit",
        "UserType": "Dwolla",
        "DestinationId": "812-742-8722",
        "DestinationName": "Cafe Kubal",
        "Destination": {
          "Id": "812-742-8722",
          "Name": "Cafe Kubal",
          "Type": "Dwolla",
          "Image": "http://www.dwolla.com/avatars/812-742-8722"
        },
        "SourceId": "33be0db8a99c1a03e01e7c6dd54dff26",
        "SourceName": "Blah",
        "Source": {
          "Id": "33be0db8a99c1a03e01e7c6dd54dff26",
          "Name": "Blah",
          "Type": "Dwolla",
          "Image": ""
        },
        "ClearingDate": "2014-10-28T00:00:00Z",
        "Status": "pending",
        "Notes": null,
        "Fees": null,
        "OriginalTransactionId": null,
        "Metadata": null
      },

      { ... }
    ]
  }
}
```

Search a user's transactions, filtering results based on an amount range, date range, transaction type, transaction status, individual's name, business name, Dwolla ID, email address, or phone number.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Transactions` scope.</aside>

### HTTP Request
To fetch a transaction which belongs to the authorized user:

`GET https://www.dwolla.com/oauth/rest/transactions/search?limit={}`

### Optional Querystring Parameters
Parameter | Description
----------|------------
searchTerm | A string to be matched with recipient/sender names (individual or business/organizaton), Dwolla ID, email address, or phone number. 
startAmount | Only include transactions with an amount equal to or greater than `startAmount`.  Can optionally be used with `endAmount` to specify an amount range.
endAmount | Only include transactions with an amount equal to or less than `endAmount`.  Can optionally be used with `startAmount` to specify an amount range.
startDate | Only include transactions created after this date.  ISO-8601 format: `YYYY-MM-DD`.  Can optionally be used with `endDate` to specify a date range.
endDate | Only include transactions created before than this date.  ISO-8601 format: `YYYY-MM-DD`.  Can optionally be used with `startDate` to specify a date range.
type | Filter results on [Transaction type](#transaction-types).  Possible values: `money_sent`, `money_received`, `deposit`, `withdraw`
status | Filter results on [Transaction status](#transaction-statuses). Possible values: `pending`, `processed`, `failed`, `reclaimed`, `cancelled`
limit | Number of search results to return.  Defaults to `50`.
offset | Number of search results to skip.  Used for pagination.

### Response

Parameter | Description
----------|------------
TotalHits | Total number of search results.  Does not necessarily equal the number of results contained in the response `Results`.
Results | JSON array of [Transaction](#transaction-resource) resources.