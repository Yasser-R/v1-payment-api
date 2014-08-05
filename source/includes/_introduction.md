# Introduction

Welcome to the Dwolla API. This is a listing of all publically available endpoints which allow developers to modify their accounts, process transactions, find other users, and modify funding sources. All endpoints are listed in the sidebar to the left. 

Example requests are available in JSON, PHP, Ruby, Python, and Node.js in the dark area to the right. JSON is shown by default, however, the tabs in the top right can be used to switch the language in which the examples are displayed in.

## Helper Libraries

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

```
POST https://www.dwolla.com/oauth/rest/transactions/send
Content-Type: application/json

{
	amount: 15.23,
	destinationId: 'gordon@dwolla.com',
	destinationType: 'Email',
	pin: 1234
}
```

`POST` requests must have a JSON encoded body and the 
`Content-Type: application/json` header.


`GET` requests have parameters provided in the querysting.

```
GET https://www.dwolla.com/oauth/rest/transactions?client_id=XYZ&client_secret=JJJ&limit=10
```

<aside class="notice">
Remember to [url-encode](http://en.wikipedia.org/wiki/Percent-encoding) all GET querystring parameters!
</aside>

## Responses

Responses are contained in an _envelope_.

```
{
    "Success": true,
    "Message": "Success",
    "Response": 71332
}
```