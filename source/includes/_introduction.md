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
We'll you show sample code on this pane!
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
- C# / .NET: [dwolla.net](https://github.com/justinsoliz/dwolla.net)

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
    "Response": 71332
}
```

> Unsuccessful response

```shell
{
	"Success": false,
	"Message": "Invalid access token.",
	"Response": null
}
```

Responses are always JSON encoded and are contained in an _envelope_.  

That means every API response contains: 

- `Success`, a boolean indicating whether or not the call was successful, or resulted in an error
- `Message`, an error message if the API call was unsuccessful, or `"Success"` otherwise
- `Response`, the actual data returned by the API call.

<!---
## Errors

TODO: document errors
-->

## Facilitator Fees

By [enabling](https://developers.dwolla.com/dev/pages/guides/facilitator_fee) the Facilitator Fee application feature, your application can take a cut of any transactions that it facilitates.  Transactions created via the Send endpoint and  transactions resulting from an [Off-Site Gateway](#checkouts) checkout can have a facilitator fee attached to it.  Facilitator fees can also be attached to [Money Requests](#money-requests) created by your application.  When the Money Request is fulfilled, the facilitator fee will be paid out.

Facilitator fees can be up to 50% of the transaction amount and must be at least $0.01.  They do not affect the total transaction amount, and exist as a separate [Transaction](#transactions) resource with a unique Transaction ID.

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