# Money Requests

```always
    __  ___                      
   /  |/  /___  ____  ___  __  __
  / /|_/ / __ \/ __ \/ _ \/ / / /
 / /  / / /_/ / / / /  __/ /_/ / 
/_/  /_/\____/_/ /_/\___/\__, /  
                        /____/   
    ____                             __      
   / __ \___  ____ ___  _____  _____/ /______
  / /_/ / _ \/ __ `/ / / / _ \/ ___/ __/ ___/
 / _, _/  __/ /_/ / /_/ /  __(__  ) /_(__  ) 
/_/ |_|\___/\__, /\__,_/\___/____/\__/____/  
              /_/      
```

```shell
{
    "Id": 1636,
    "Source": {
        "Id": "812-742-8722",
        "Name": "Cafe Kubal",
        "Type": "Dwolla",
        "Image": "http://uat.dwolla.com/avatars/812-742-8722"
    },
    "Destination": {
        "Id": "812-742-3301",
        "Name": "Gordon Zheng",
        "Type": "Dwolla",
        "Image": "http://uat.dwolla.com/avatars/812-742-3301"
    },
    "Amount": 0.01,
    "Notes": "",
    "DateRequested": "2014-10-01T19:50:54Z",
    "Status": "Paid",
    "Transaction": {
        "RequestId": 1636,
        "Id": 335741,
        "Source": {
            "Id": "812-742-3301",
            "Name": "Gordon Zheng",
            "Type": "Dwolla",
            "Image": "http://uat.dwolla.com/avatars/812-742-3301"
        },
        "Destination": {
            "Id": "812-742-8722",
            "Name": "Cafe Kubal",
            "Type": "Dwolla",
            "Image": "http://uat.dwolla.com/avatars/812-742-8722"
        },
        "Amount": 0.01,
        "SentDate": "2014-10-01T19:51:52Z",
        "ClearingDate": "2014-10-01T19:51:52Z",
        "Status": "processed"
    },
    "CancelledBy": null,
    "DateCancelled": "",
    "SenderAssumeFee": false,
    "SenderAssumeAdditionalFees": false,
    "AdditionalFees": [],
    "Metadata": null
}
```

Money Requests are sent from a Dwolla user to another Dwolla user, phone number or email address.  A Money Request is essentially an invoice, requesting payment from the recipient of the request.

Money Requests can be fulfilled for the amount of the request or greater whenever the payer decides to do so.  When fulfilled, a RequestFulfilled webhook notification is sent.  Money Requests can also be cancelled by either the sender of the request or the recipient of the request.  When cancelled, a RequestCancelled webhook notification is sent.

Money requests do not expire.

### Money Request Statuses

Status | Description
-------|------------
Pending | Intitial state upon creation.  Not yet fulfilled.
Paid | Payer has fulfilled the request and a payment has been sent for the request amount or greater.
Cancelled | Request was cancelled by either payer or requester.

### Money Request Resource

Parameter | Description
----------|------------
Id | Unique Request ID
Source | JSON object describing the requester: Dwolla ID, full name, user type (will always be `Dwolla`, since only Dwolla user accounts can request money), and profile avatar URL
Destination | JSON object describing the payer: DestinationId (a Dwolla ID, email, or phone number), full name (if Dwolla account, otherwise, the email or phone number), user type, and profile avatar URL (`null` if not a user yet)
Amount | Amount of funds requested
Notes | Optional notes attached to Money Request
DateRequested | Date and time when Money Request was created
Status | Status of Money Request.  [See statuses](#money-request-statuses).
Transaction | If fulfilled, a JSON object representing the resulting Transaction.  Otherwise, `null`.
CancelledBy | If cancelled, a JSON object representing the user who cancelled the request.  Otherwise, `null`.
DateCancelled | If cancelled, the date and time when the request was cancelled.  Otherwise, empty string: `""`.
SenderAssumeFee | Boolean.  `true` if sender will assume the $0.25 transaction fee if applicable.
SenderAssumeAdditionalFees | Boolean.  `true` if sender will assume all facilitator fees.
AdditionalFees | Any additional facilitator fees attached to the request.
Metadata | An optional [metadata](#metadata) object.

## Create Money Request

```php
<?php

// Request $20 from email gordon@dwolla.com

$Requests = new Dwolla\Requests();
$Requests->settings->oauth_token = "foo";

$result = $Requests->create('gordon@dwolla.com', 20.00, [
  'sourceType' => 'Email',
  'notes' => 'You owe me money!'
]);

var_export($result);
?>
```
```ruby
# Request 1.00 from a Dwolla ID
puts Dwolla::Requests.create({:sourceId=> '812-742-8722', :amount=> '1.00'})
```
```python
# Request $5 from 812-740-3809

print(request.create('812-740-3809', 5.00))
```
```js
/***
 * Request $5 from Source ID '812-111-1111'
 */

Dwolla.request('812-111-1111', '5.00', function(err, data) {
   console.log(data);
});
```

> If successful, you'll receive this response:

```php
1670        // request ID
```
```python
1470 # Request ID
```

```ruby
1652      # request ID
```

```js
1288        // Request ID
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": 996
}
```

Send a Money Request from the authorized user to a destination Dwolla user, email address or phone number.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Request` scope.</aside>

### HTTP Request

`POST https://www.dwolla.com/oauth/rest/requests/`

| Parameter                  | Optional? | Description |
|----------------------------|-----------|-------------|
| sourceId | no | ID of user to request funds from. Must be either a Dwolla ID, phone number, or email address.
| amount | no | Amount of funds to request 
| sourceType | yes | Type of source user (either `Dwolla`, `Email` or `Phone`).  Defaults to `Dwolla`.|
| facilitatorAmount | yes | Amount of the facilitator feeto override. Only applicable if the facilitator fee feature is enabled for your API application. If set to 0, facilitator fee is disabled for transaction. Cannot exceed 50% of the `amount`. 
| notes | yes | Note to attach to request (250 character limit).
| senderAssumeCosts | yes | Set to `true` if the fulfilling user is to assume the Dwolla fee. Defaults to `false`; does not include facilitator fees.
| senderAssumeAdditionalFees | yes | Set to `true` if the fulfilling user is to assume all facilitator fees. Defaults to `false`; does not include the Dwolla transaction fee.
| additionalFees             | yes | Array of additional facilitator fee objects.  `[{"destinationId": "", "amount": 0.01}, ...]` |
| metadata                   | yes | Optional JSON object of a maximum of 10 key-value pairs (each key and value must be less than 255 characters).  [Read more](#metadata)  |

### Errors
| Error String | Description |
|--------------|-------------|
| Notes length is too long. Maximum of 250 character is allowed. | The 'notes' parameter is too long. See requests parameters for further details. |
| The user to send to could not be found. | Could not find the speicified source user + source type combination. |
| You can only send money to one of your Twitter followers. | You can only request money from one of your Twitter followers. |
| Invalid facilitator user. |  |
| Facilitator fee too large. | Facilitator fee cannot exceed 50% of the amount. |
| Facilitator fee too small. |  |
| Money request failed. {..} | An unexpecetd error occured while trying to request funds. See additional response message. |

## List Money Requests

```php
/**
 * EXAMPLE 1: 
 *   Get list of pending requests
 **/
$requests = $Dwolla->requests();
if(!$requests) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($requests); }
```
```ruby
# Get all pending requests
puts Dwolla::requests.get
```
```python
# Get all pending requests from the user
# associated with the current OAuth token.
print(request.get())
```
```js
/***
 * List pending money requests for the authorized user
 */

Dwolla.requests(function(err, data){
   console.log(data);
});
```

```php
<?php
$Requests = new Dwolla\Requests();
$Requests->settings->oauth_token = "foo";

$result = $Requests->get();

var_export($result);
?>
```

> If successful, you'll receive this response:


```php
array (
  0 =>
  array (
    'Id' => 1452,
    'Source' =>
    array (
      'Id' => '812-742-8722',
      'Name' => 'Cafe Kubal',
      'Type' => 'Dwolla',
      'Image' => 'http://uat.dwolla.com/avatars/812-742-8722',
    ),
    'Destination' =>
    array (
      'Id' => '812-443-2936',
      'Name' => ' ',
      'Type' => 'Dwolla',
      'Image' => 'http://uat.dwolla.com/avatars/812-443-2936',
    ),
    'Amount' => 1,
    'Notes' => '',
    'DateRequested' => '2014-08-27T03:47:39Z',
    'Status' => 'Pending',
    'Transaction' => NULL,
    'CancelledBy' => NULL,
    'DateCancelled' => '',
    'SenderAssumeFee' => false,
    'SenderAssumeAdditionalFees' => false,
    'AdditionalFees' =>
    array (
    ),
    'Metadata' => NULL,
  ),
  1 => array ( ... ),
  2 => array ( ... ),
  ...
)
```
```python
[
        {
            "Id": 640,
            "Source": {
                "Id": "812-693-9484",
                "Name": "Spencer Hunter",
                "Type": "Dwolla",
                "Image": null
            },
            "Destination": {
                "Id": "812-706-1396",
                "Name": "Jane Doe",
                "Type": "Dwolla",
                "Image": null
            },
            "Amount": 5.00,
            "Notes": "",
            "DateRequested": "2014-07-23T21:49:06Z",
            "Status": "Pending" ,
            "Transaction": null,
            "CancelledBy": null,
            "DateCancelled": "",
            "SenderAssumeFee": false,
            "SenderAssumeAdditionalFees": false,
            "AdditionalFees": [],
            "Metadata": null 
        },
        ...
]
```
```ruby
[
  {
    "Id"                         => 1452,
    "Source"                     => {
      "Id"    => "812-742-8722",
      "Name"  => "Cafe Kubal",
      "Type"  => "Dwolla",
      "Image" => "http://uat.dwolla.com/avatars/812-742-8722"
    },
    "Destination"                => {
      "Id"    => "812-443-2936",
      "Name"  => " ",
      "Type"  => "Dwolla",
      "Image" => "http://uat.dwolla.com/avatars/812-443-2936"
    },
    "Amount"                     => 1.0,
    "Notes"                      => "",
    "DateRequested"              => "2014-08-27T03:47:39Z",
    "Status"                     => "Pending",
    "Transaction"                => nil,
    "CancelledBy"                => nil,
    "DateCancelled"              => "",
    "SenderAssumeFee"            => false,
    "SenderAssumeAdditionalFees" => false,
    "AdditionalFees"             => [],
    "Metadata"                   => nil
  },
  { ... }
]
```

```js
[
  {
    "Id": 640,
    "Source": {
      "Id": "812-693-9484",
      "Name": "Spencer Hunter",
      "Type": "Dwolla",
      "Image": null
    },
    "Destination": {
      "Id": "812-706-1396",
      "Name": "Jane Doe",
      "Type": "Dwolla",
      "Image": null
    },
    "Amount": 0.1,
    "Notes": "",
    "DateRequested": "2014-07-23T21:49:06Z",
    "Status": "Pending" ,
    "Transaction": null,
    "CancelledBy": null,
    "DateCancelled": "",
    "SenderAssumeFee": false,
    "SenderAssumeAdditionalFees": false,
    "AdditionalFees": [],
    "Metadata": null 
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
            "Id": 640,
            "Source": {
                "Id": "812-693-9484",
                "Name": "Spencer Hunter",
                "Type": "Dwolla",
                "Image": null
            },
            "Destination": {
                "Id": "812-706-1396",
                "Name": "Jane Doe",
                "Type": "Dwolla",
                "Image": null
            },
            "Amount": 0.1,
            "Notes": "",
            "DateRequested": "2014-07-23T21:49:06Z",
            "Status": "Pending" ,
            "Transaction": null,
            "CancelledBy": null,
            "DateCancelled": "",
            "SenderAssumeFee": false,
            "SenderAssumeAdditionalFees": false,
            "AdditionalFees": [],
            "Metadata": null 
        },
        
        ...

    ]
}
```

List the authorized user's pending requests.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Request` scope.</aside>

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/requests/`

### Request Parameters

| Parameter   | Optional? | Description|
|-------------|-----------|------------------------------------------------------------------------------------------------------|
| startDate   | yes       | Earliest date and time for which to retrieve pending requests (defaults to 0300 UTC for current day) |
| endDate     | yes       | Latest date and time for which to retrieve pending requests (defaults to UTC current date/time)      |
| limit       | yes       | Number of pending requests to retrieve (defaults to 20, must be in range of 1-200)                   |

## Retrieve Money Request

```php
<?php
$Requests = new Dwolla\Requests();
$Requests->settings->oauth_token = "foo";

$requestId = '1452';
$result = $Requests->info($requestId);

var_export($result);
?>
```
```ruby
# Example 1: Get request details for request '12345'
puts Dwolla::requests.get('12345')
```
```python
# Get info regarding a pending money request.
print(request.info(1470))
```
```js
/***
 * Fetch a money request by id '12345678'
 */

Dwolla.requestById('12345678', function(err, data) {
   console.log(data);
});
```

> If successful, you'll receive this response:

```php
array (
  'Id' => 1452,
  'Source' =>
  array (
    'Id' => '812-742-8722',
    'Name' => 'Cafe Kubal',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-742-8722',
  ),
  'Destination' =>
  array (
    'Id' => '812-443-2936',
    'Name' => ' ',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-443-2936',
  ),
  'Amount' => 1,
  'Notes' => '',
  'DateRequested' => '2014-08-27T03:47:39Z',
  'Status' => 'Pending',
  'Transaction' => NULL,
  'CancelledBy' => NULL,
  'DateCancelled' => '',
  'SenderAssumeFee' => false,
  'SenderAssumeAdditionalFees' => false,
  'AdditionalFees' =>
  array (
  ),
  'Metadata' => NULL,
)
```

```ruby
{
    "Id"                         => 12345,
    "Source"                     => {
      "Id"    => "812-742-8722",
      "Name"  => "Cafe Kubal",
      "Type"  => "Dwolla",
      "Image" => "http://uat.dwolla.com/avatars/812-742-8722"
    },
    "Destination"                => {
      "Id"    => "812-443-2936",
      "Name"  => " ",
      "Type"  => "Dwolla",
      "Image" => "http://uat.dwolla.com/avatars/812-443-2936"
    },
    "Amount"                     => 1.0,
    "Notes"                      => "",
    "DateRequested"              => "2014-08-27T03:47:39Z",
    "Status"                     => "Pending",
    "Transaction"                => nil,
    "CancelledBy"                => nil,
    "DateCancelled"              => "",
    "SenderAssumeFee"            => false,
    "SenderAssumeAdditionalFees" => false,
    "AdditionalFees"             => [],
    "Metadata"                   => nil
  }
```
```python
        {
            "Id": 1470,
            "Source": {
                "Id": "812-693-9484",
                "Name": "Spencer Hunter",
                "Type": "Dwolla",
                "Image": null
            },
            "Destination": {
                "Id": "812-706-1396",
                "Name": "Jane Doe",
                "Type": "Dwolla",
                "Image": null
            },
            "Amount": 5.00,
            "Notes": "",
            "DateRequested": "2014-07-23T21:49:06Z",
            "Status": "Pending" ,
            "Transaction": null,
            "CancelledBy": null,
            "DateCancelled": "",
            "SenderAssumeFee": false,
            "SenderAssumeAdditionalFees": false,
            "AdditionalFees": [],
            "Metadata": null 
        }
```
```js
{
  "Id": 660,
  "Source": {
    "Id": "812-693-9484",
    "Name": "Spencer Hunter",
    "Type": "Dwolla",
    "Image": null
  },
  "Destination": {
    "Id": "812-706-1396",
    "Name": "Jane Doe",
    "Type": "Dwolla",
    "Image": null
  },
  "Amount": 10,
  "Notes": "",
  "DateRequested": "2014-07-25T17:38:26Z",
  "Status": "Pending" ,
  "Transaction": null,
  "CancelledBy": null,
  "DateCancelled": "",
  "SenderAssumeFee": false,
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

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": 660,
        "Source": {
            "Id": "812-693-9484",
            "Name": "Spencer Hunter",
            "Type": "Dwolla",
            "Image": null
        },
        "Destination": {
            "Id": "812-706-1396",
            "Name": "Jane Doe",
            "Type": "Dwolla",
            "Image": null
        },
        "Amount": 10,
        "Notes": "",
        "DateRequested": "2014-07-25T17:38:26Z",
        "Status": "Pending" ,
        "Transaction": null,
        "CancelledBy": null,
        "DateCancelled": "",
        "SenderAssumeFee": false,
        "SenderAssumeAdditionalFees": false,
        "AdditionalFees": [],
        "Metadata": {
            "InvoiceDate": "12-06-2014",
            "Priority": "High",
            "TShirtSize": "Small",
            "blah": "blah"
        }
    }
}
```

Retrieve a single Money Request by its ID.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Request` scope.</aside>

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/requests/{id}`

### Errors
| Error String | Description |
|--------------|-------------|
| No verified funding sources. | Could not find any funding sources for the given OAuth token. |

## Fulfill Money Request

```php
/**
 *   Fulfill request ID 1671 for $29.99
 **/
<?php
$Requests = new Dwolla\Requests();
$Requests->settings->oauth_token = "foo";
$Requests->settings->pin = 9999;

$requestId = '1671';
$result = $Requests->fulfill($requestId, '29.99');

var_export($result);
?>
```
```ruby
# Example 1: Fulfill a request, ID 1653, of amount 20.00
puts Dwolla::Requests.fulfill('1653', {
  :pin => 9999,
  :amount => 20.00,
})
```
```python
# Fulfill a pending money request.

print(request.fulfill(1475, 10.00))
```
```js
/***
 * Fulfill money request ID '12345678'.  Pay '10.00'
 */

Dwolla.fulfillRequest(pin, '12345678', '10.00', function(err, data) {
   console.log(data);
});
```

> If successful, you'll receive this response:

```php
array (
  'RequestId' => 1671,
  'Id' => 347947,
  'Source' =>
  array (
    'Id' => '812-742-8722',
    'Name' => 'Cafe Kubal',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-742-8722',
  ),
  'Destination' =>
  array (
    'Id' => '812-742-3301',
    'Name' => 'Gordon Zheng',
    'Type' => 'Dwolla',
    'Image' => 'http://uat.dwolla.com/avatars/812-742-3301',
  ),
  'Amount' => 29.989999999999998,
  'SentDate' => '2014-10-24T23:37:18Z',
  'ClearingDate' => '2014-10-24T23:37:18Z',
  'Status' => 'processed',
)
```
```python
{
        "Id": 147659,
        "RequestId": 1475,
        "Amount": 10.00,
        "SentDate": "2014-01-22T13:11:10Z",
        "ClearingDate": "2014-01-22T13:11:10Z",
        "Status": "processed" ,
        "Source": {
            "Id": "812-693-9484",
            "Name": "Michael Schonfeld",
            "Type": "Dwolla",
            "Image": null
        },
        "Destination": {
            "Id": "812-600-6705",
            "Name": "Michael Schonfeld",
            "Type": "Dwolla",
            "Image": null
        }
}
```
```ruby
{
  "RequestId"    => 1653,
  "Id"           => 341624,
  "Source"       => {
    "Id"    => "812-742-8722",
    "Name"  => "Cafe Kubal",
    "Type"  => "Dwolla",
    "Image" => "http://uat.dwolla.com/avatars/812-742-8722"
  },
  "Destination"  => {
    "Id"    => "812-742-3301",
    "Name"  => "Gordon Zheng",
    "Type"  => "Dwolla",
    "Image" => "http://uat.dwolla.com/avatars/812-742-3301"
  },
  "Amount"       => 20.0,
  "SentDate"     => "2014-10-20T06:00:02Z",
  "ClearingDate" => "2014-10-20T06:00:02Z",
  "Status"       => "processed"
}
```

```js
{ 
  RequestId: 12345678,
  Id: 328969,
  Source: { 
    Id: '812-740-4294',
    Name: 'GORDCORP',
    Type: 'Dwolla',
    Image: 'http://uat.dwolla.com/avatars/812-740-4294' 
  },
  Destination: { 
    Id: '812-742-8722',
    Name: 'Cafe Kubal',
    Type: 'Dwolla',
    Image: 'http://uat.dwolla.com/avatars/812-742-8722'
  },
  Amount: 10,
  SentDate: '2014-09-09T04:30:46Z',
  ClearingDate: '2014-09-09T04:30:46Z',
  Status: 'processed' 
}
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": {
        "Id": 147659,
        "RequestId": 999,
        "Amount": 1,
        "SentDate": "2014-01-22T13:11:10Z",
        "ClearingDate": "2014-01-22T13:11:10Z",
        "Status": "processed" ,
        "Source": {
            "Id": "812-693-9484",
            "Name": "Michael Schonfeld",
            "Type": "Dwolla",
            "Image": null
        },
        "Destination": {
            "Id": "812-600-6705",
            "Name": "Michael Schonfeld",
            "Type": "Dwolla",
            "Image": null
        }
    }
}
```

Fulfill an authorized user's pending money request.  Request must have a status of `Pending`.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Request` scope.</aside>

### HTTP Request

`POST https://www.dwolla.com/oauth/rest/requests/{request_id}/fulfill`

| Parameter         | Optional? | Description|
|-------------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| pin               | no        | The authorized user's PIN.
| amount  | no | Amount to pay for request (must be greater than or equal to the request amount).
| notes | yes | Note to attach to transaction (250 character limit). |
| fundsSource | yes | ID of funding source to use for transaction (defaults to `Balance`). 
| assumeCosts | yes | Set to `true` for the fulfilling user to assume the Dwolla fee, `false` for the destination user to assume the fee. |
| metadata | yes | An optional [metadata](#metadata) object.

### Errors
| Error String | Description |
|--------------|-------------|
| Invalid request ID. | The specific request ID is either empty or invalid. |
| Request not found for account. | Could not find a request with the specified ID for the given OAuth token. |
| Account can not pay their own request. | Account can not pay their own request. |
| Request has already been paid | Request has already been paid |
| Amount may not be less than request amount of {0} | The fulfilled amount cannot be less than the requested amount. |
| Notes length is too long. Maximum of 250 character is allowed. | The 'notes' parameter is too long. See requests parameters for further details. |
| Invalid funding source provided: {..} | The specified funding source is missing, empty, or invalid. |
| Insufficient funds. | The are not enough funds in the specified funding source to fulfill this transaction. |

## Cancel a Money Request

Cancel a money request which the authorized user created or received.  Request must have a status of `Pending`.

```php
/**
 *   Cancel request with ID 1672
 **/
<?php
$Requests = new Dwolla\Requests();
$Requests->settings->oauth_token = "foo";

$requestId = '1672';
$result = $Requests->cancel($requestId);

var_export($result);
?>
```
```ruby
# Cancel request with ID '12345'
puts Dwolla::Requests.delete('1652')
```
```python
# Cancel a pending money request.

print(request.cancel(1470))
```
```js
/***
 * Cancel money request with ID '12345678'
 */

Dwolla.cancelRequest('12345678', function(err, data){
   console.log(data);
});
```

> If successful, you'll recieve this response:

```
''      // empty string
```

```js
true    // boolean.
```

```ruby
""      // empty string
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": ""
}
```

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `Request` scope.</aside>

### HTTP Request

`POST https://www.dwolla.com/oauth/rest/requests/{request_id}/cancel`

| Error String | Description |
|--------------|-------------|
| Invalid request ID. | The specific request ID is either empty or invalid. |
| Request not found for account. | Could not find a request with the specified ID for the given OAuth token. |
| Request is not in Pending status. | You may only cancel requests that are in the 'pending' state. |