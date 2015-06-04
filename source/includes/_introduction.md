# Introduction
```
  _          _ _             
 | |__   ___| | | ___        
 | '_ \ / _ \ | |/ _ \       
 | | | |  __/ | | (_) |      
 |_| |_|\___|_|_|\___/     _ 
 __      _____  _ __| | __| |
 \ \ /\ / / _ \| '__| |/ _` |
  \ V  V / (_) | |  | | (_| |
   \_/\_/ \___/|_|  |_|\__,_|

```

Welcome to the Dwolla API documentation.  Dwolla's Web API enables you to send money, retrieve transaction data, request money, add bank accounts, accept payments, and a whole lot more.

Sample code is available in PHP, Ruby, Python, and Node.JS in the dark area to the right. JSON is shown by default but the tabs on the top right can be used to switch the language in which the examples are displayed in.

## Helper Libraries
```
We'll show you sample code on this pane!
```
```
Quote of the day:

<erno> hm. I've lost a machine.. literally _lost_. it responds to ping, it works completely, I just can't figure out where in my apartment it is.

```

A handful of libraries are officially maintained, and others are community maintained.

- PHP: [dwolla-php](https://github.com/Dwolla/dwolla-php)
- Ruby: [dwolla-ruby](https://github.com/Dwolla/dwolla-ruby)
- Python: [dwolla-python](https://github.com/Dwolla/dwolla-python)
- Node.JS: [dwolla-node](https://github.com/Dwolla/dwolla-node)
- Java: [dwolla-java](https://github.com/Dwolla/dwolla-java)
- Clojure: [dwolla-clojure](https://github.com/Dwolla/dwolla-clojure)
- Scala: [dwolla-scala](https://github.com/Dwolla/dwolla-scala)
- Windows 8 SDK
	- [Phone](http://www.nuget.org/packages/Dwolla.InAppSDKWP8/)
	- [XAML](http://www.nuget.org/packages/Dwolla.InAppSDK/)
	- [JS](http://www.nuget.org/packages/Dwolla.InAppSDK.JS/)
- iOS: [dwolla-ios](https://github.com/Dwolla/dwolla-ios)
- OS X Cocoa: [dwolla-cocoa](https://github.com/Dwolla/dwolla-cocoa)
- Perl: [WebService-Dwolla](http://search.cpan.org/dist/WebService-Dwolla/)
- C# / .NET: [dwolla.net](http://github.com/Dwolla/dwolla.net)

## Making Requests

```shell
POST https://www.dwolla.com/oauth/rest/transactions/send
Content-Type: application/json

{
	amount: 15.23,
	destinationId: 'gordon@dwolla.com',
	destinationType: 'Email',
	pin: 1234
}
```

```shell
...

GET https://www.dwolla.com/oauth/rest/transactions?client_id=XYZ&client_secret=JJJ&limit=10
```

`POST` requests must have a JSON encoded body and the 
`Content-Type: application/json` header.

`GET` requests have parameters provided in the querysting.

All requests must be made over HTTPS.  Any HTTP requests are met with a HTTP 302 to its HTTPS equivalent.

<aside class="notice">
Remember to [url-encode](http://en.wikipedia.org/wiki/Percent-encoding) all GET querystring parameters!
</aside>

## Responses

> Happy response

```shell
{
    "Success": true,
    "Message": "Success",
    "Response": 71332,
    "_links": null
}
```

> Unsuccessful response

```shell
{
	"Success": false,
	"Message": "Invalid access token.",
	"Response": null,
    "_links": null
}
```

Responses are always JSON encoded and are contained in an _envelope_.  

That means every API response contains: 

- `Success`, a boolean indicating whether or not the call was successful, or resulted in an error
- `Message`, an error message if the API call was unsuccessful, or `"Success"` otherwise
- `Response`, the actual data returned by the API call.

Most API responses will contain:

- `_links`, object that contains links (to URIs). Keys within _links are the name of the link and describe the relationship between the current resource and the link.

<!---
## Errors

TODO: document errors
-->

## Facilitator Fees

By [enabling](https://developers.dwolla.com/dev/pages/guides/facilitator_fee) the facilitator fee application feature, you can set your own percentage or flat fee from incoming transactions.  Transactions created via the Send endpoint and  transactions resulting from an [Off-Site Gateway](#checkouts) checkout can have a facilitator fee attached to it.  Facilitator fees can also be attached to [Money Requests](#money-requests) created by your application.  When the Money Request is fulfilled, the facilitator fee will be paid out.

Facilitator fees can be up to 50% of the transaction amount and must be at least $0.01.  They do not affect the original transaction amount, and exist as a separate [Transaction](#transactions) resource with a unique Transaction ID.

<aside class="notice">
**Note**: facilitator fee is a premium application feature. To try it out, please [contact us](https://www.dwolla.com/contact) or call 1-888-289-8744.
</aside>

## Metadata

Metadata can be supplied for Send, MassPay Items, Money Requests, Refunds, and Checkouts in the metadata property. The `metadata` property is a JSON object (a collection of "key": "value" pairs). A maximum of 10 key-value pairs can be stored. Keys and values must be strings of maximum length 255 characters.

```shell
{
    "Shipment ID": "d289e89ej893r3r",
    "TShirtSize": "Small",
    "DeliveryOption": "Rush Shipping",
    "InvoiceDate": "12-06-2014",
    "Priority": "High",
    "blah": "blah"
}
```

### Visibility / Access

Metadata is intended as an expansion of the existing `notes` field (a string of max length 255), in order to allow applications to store more data with resources.

Unlike the `notes` field, which is visible to:

1. the application that created the transaction
2. the recipient and sender of the transaction (through the Dwolla.com dashboard)
3. any future applications that view the transaction

`metadata` is only visible to the application that created the transaction. No other application, nor the sender or receiver may access the metadata field.

### Warnings

Currently, there are 2 bugs we are aware of:

1. If an metadata collection contains two duplicate keys, a HTTP 500 XML response will be thrown instead of a proper error response.
2. Keys which start with a number or symbol (ex. "10", "10abc", "$abc") are rejected.

## Errors

When an API request results in an error, Dwolla *typically* responds with a HTTP status code of `200` and response body containing a JSON object containing an error `Message`, with `Success` set to `false` and `Response` being `null`. 

However, in special cases, where there is an uncaught exception on our system, or when the API request is malformed (JSON syntax error, duplicated keys) Dwolla will return an HTTP `500` with an XML response body.  We realize that having two different response formats for errors is far from ideal, and is something we aim to fix in future versions of the API.

```shell
Standard HTTP 200 JSON response:
{
    "Success": false,
    "Message": "Access token is empty.",
    "Response": null
}
```


```shell
HTTP 500 XML response:
```
```xml
<Fault>
    xmlns="http://schemas.microsoft.com/ws/2005/05/envelope/none">
    <Code>
        <Value>Receiver</Value>
        <Subcode>
            <Value 
                xmlns:a="http://schemas.microsoft.com/net/2005/12/windowscommunicationfoundation/dispatcher">a:InternalServiceFault
            </Value>
        </Subcode>
    </Code>
    <Reason>
        <Text xml:lang="en-US">The server was unable to process the request due to an internal error.  For more information about the error, either turn on IncludeExceptionDetailInFaults (either from ServiceBehaviorAttribute or from the &lt;serviceDebug&gt; configuration behavior) on the server in order to send the exception information back to the client, or turn on tracing as per the Microsoft .NET Framework 3.0 SDK documentation and inspect the server trace logs.</Text>
    </Reason>
</Fault>
```


| HTTP Status | Response Type | Meaning |
|----------|------------|---------------|
200 | JSON | Normal errors |
500 | XML | Results from an invalid request or uncaught server exception |

### General Errors
The following errors are common across all API endpoints.

| Error String | Description |
|--------------|-------------|
| Access token is empty. | No OAuth access token has been supplied. |
| Invalid access token. | No OAuth access token has been supplied. |
| Token does not have access to requested resource. Token must have {...} scope access. | The given OAuth token does not have access to the requested scope used. Please review the scopes list. |
| Invalid account status for user of this access token. | The account associated with the given OAuth token is in an invalid status. |
| Application is either not verified or suspended. | The application used to generate the OAuth token has been unverified or suspended. |
| Account temporarily locked | Too many failed attempts have been made to access this account. The account has been temporarily locked for 30 minutes. |
| Unexpected error has occurred. | Something unexpected happened. It has been logged on our end and will be reviewed shortly. |