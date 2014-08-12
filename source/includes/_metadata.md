# Metadata

Metadata can be supplied for Send, MassPay Items, Money Requests, Refunds, and Checkouts in the metadata property. The `metadata` property is a JSON object (a collection of "key": "value" pairs). A maximum of 10 key-value pairs can be stored. Keys and values must be strings of maximum length 255 characters.

```always
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