# Money Requests

```always
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
# Example 1: Get all pending requests
pp Dwolla::requests.get
```
```python
'''
    EXAMPLE 1:
      Get a list of pending requests
      for the user with the given oauth token
'''
pending_requests = DwollaUser.get_pending_requests()
print(pending_requests)
```
```js
/***
 * Example 1:
 *
 * List pending money requests for the user associated with the
 * OAuth token
 */

Dwolla.requests(function(err, data){
   if (err) { console.log(err); }
   console.log(data);
});
```

List the authorized user's pending requests.

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/requests/`

### Request Parameters

| Parameter   | Optional? | Description                                                                                          |
|-------------|-----------|------------------------------------------------------------------------------------------------------|
| startDate   | yes       | Earliest date and time for which to retrieve pending requests (defaults to 0300 UTC for current day) |
| endDate     | yes       | Latest date and time for which to retrieve pending requests (defaults to UTC current date/time)      |
| limit       | yes       | Number of pending requests to retrieve (defaults to 20, must be in range of 1-200)                   |

> If successful, you'll receive this response:

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

## Retrieve Money Request

```php
/**
 * EXAMPLE 1: 
 *   Get request by ID
 **/
$request = $Dwolla->requestById($requestId);
if(!$request) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($request); }
```
```ruby
# Example 1: Get request details for request '12345'
pp Dwolla::requests.get('12345')
```
```python
'''
    EXAMPLE 1:
      Get detailed information for the request
      with the given 'request_id'
'''
request_id = '2209516'
request = DwollaUser.get_request(request_id)
print(request)
```
```js
/***
 * Example 1:
 *
 * Fetch detailed information about a money request by specific id '12345678'
 */

Dwolla.requestById('12345678', function(err, data) {
   if (err) { console.log(err); }
   console.log(data);
});
```
### HTTP Request

`GET https://www.dwolla.com/oauth/rest/requests/{id}`

> If successful, you'll receive this response:

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


## Create Money Request

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Request money ($1.00) to a Dwolla ID 
 **/
$requestId = $Dwolla->request('812-713-9234', 1.00);
if(!$requestId) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { echo("Request ID: {$requestId} \n"); }
```
```ruby
# Example 1: Request 1.00 from a Dwolla ID
pp Dwolla::requests.create({:sourceId=> '812-114-1111', :amount=> '1.00'})
```
```python
'''
    EXAMPLE 1:
      Initiate a money request
'''
request = DwollaUser.request_funds('1.00', 'reflector@dwolla.com', _keys.pin, source_type='Email')
print(request)
```
```js
/***
 * Example 1:
 *
 * Request $5 from Source ID '812-111-1111'
 */

Dwolla.request('812-111-1111', '5.00', function(err, data) {
   if (err) { console.log(err); }
   console.log(data);
});
```

`POST https://www.dwolla.com/oauth/rest/requests/`

| Parameter                  | Optional? | Description                                                                                                                                                                                       |
|----------------------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| oauth_token                | no        | Valid OAuth token with the `Request` scope.                                                                                                                                                       |
| sourceId                   | no        | ID of user to request funds from. Must be either a Dwolla ID, FB ID, Twitter screen-name, phone number, or e-mail address.                                                                        |
| amount                     | no        | Amount of funds to request from user (please exclude currency symbols; this will cause your request to be rejected).                                                                              |
| sourceType                 | yes       | Type of source user (either Dwolla, Email or Phone).                                                                                                                                              |
| facilitatorAmount          | yes       | Amount of the facilitator feeto override. Only applicable if the facilitator fee feature is enabled. If set to 0, facilitator fee is disabled for transaction. Cannot exceed 50% of the 'amount'. |
| notes                      | yes       | Note to attach to request (250 character limit).                                                                                                                                                  |
| senderAssumeCosts          | yes       | Set to `true` if the fulfilling user is to assume the Dwolla fee. Defaults to `false`; does not affect facilitator fees.                                                                          |
| senderAssumeAdditionalFees | yes       | Set to `true` if the fulfilling user is to assume all facilitator fees. Defaults to `false`; does not affect the Dwolla transaction fee.                                                          |
| additionalFees             | yes       | Array of additional facilitator fees                                                                                                                                                              |
| --> destinationId          | yes       | Facilitator's Dwolla ID                                                                                                                                                                           |
| --> amount                 | yes       | Facilitator amount.                                                                                                                                                                               |
| metadata                   | yes       |                                                                                                                                                                                                   |

> If successful, you'll receive this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": 996
}
```

## Fulfill Money Request

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Fulfill request with ID
 **/
$fulfilledRequest = $Dwolla->fulfillRequest($requestId, $pin);
if(!$fulfilledRequest) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($fulfilledRequest); }
```
```ruby
# Example 1: Fulfill a request to ID 12345 of amount 1.00
pp Dwolla::requests.fulfill('12345', {:pin =>: '1234', :amount => '1.00'})
```
```python
'''
    EXAMPLE 1:
      Fulfill (:pay) a pending payment request
'''
request_id = '2214710'
fulfilled_request = DwollaUser.fulfill_request(request_id, _keys.pin)
print(fulfilled)
```
```js
/***
 * Example 1:
 *
 * Fulfill money request '12345678' of amount '10.00'
 */

Dwolla.fulfillRequest(cfg.pin, '12345678', '10.00', function(err, data) {
   if (err) { console.log(err); }
   console.log(data);
});
```

`POST https://www.dwolla.com/oauth/rest/requests/{request_id}/fulfill`

| Parameter         | Optional? | Description                                                                                                         |
|-------------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| oauth_token       | no        | Valid OAuth token with the `Request` scope.                                                                         |
| request_id        | no        | The request ID to fulfill.                                                                                          |
| pin               | no        | The PIN associated with the user's account.                                                                         |
| amount            | no        | Amount to pay for request (must be greater than or equal to the request amount or the request will fail).           |
| notes             | yes       | Note to attach to transaction (250 character limit).                                                                |
| fundsSource       | yes       | ID of funding source to use for transaction (defaults to `Balance`).                                                |
| assumeCosts       | yes       | Set to `true` for the fulfilling user to assume the Dwolla fee, `false` for the destination user to assume the fee. |
| metadata          | yes       | Optional JSON object of a maximum of 10 key-value pairs (each key and value must be less than 255 characters).      |

> If successful, you'll receive this response:

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

## Cancel a Money Request

### HTTP Request

> Examples with Dwolla Libraries:

```php
/**
 * EXAMPLE 1: 
 *   Cancel request with ID
 **/
$canceledRequest = $Dwolla->cancelRequest($requestId);
if(!$canceledRequest) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { echo("Canceled? {$canceledRequest} \n"); }
```
```ruby
# Example 1: Cancel request with ID '12345'
pp Dwolla::requests.cancel('12345')
```
```python
'''
    EXAMPLE 1:
      Cancel a pending money request
'''
request_id = request
canceled_request = DwollaUser.cancel_request(request_id)
print(canceled_request)
```
```js
/***
 * Example 1:
 *
 * Cancel money request with ID '12345678'
 */

Dwolla.cancelRequest('12345678', function(err, data){
   if (err) { console.log(err); }
   console.log(data);
});
```

`POST https://www.dwolla.com/oauth/rest/requests/{request_id}/cancel`

| Parameter   | Optional? | Description                                                                                          |
|-------------|-----------|------------------------------------------------------------------------------------------------------|
| oauth_token | no        | Valid OAuth token with the `Request` scope.                                                          |
| request_id  | no 		  | The request ID to cancel/deny.																		 |

> If successful, you'll recieve this response:

```json
{
    "Success": true,
    "Message": "Success",
    "Response": ""
}
```