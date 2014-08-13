# Transactions
```always
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

Any movement of money, whether payment, withdrawal, deposit, or fee, is recorded as an individual Transaction.  For instance, depositing money from a funding source into your Dwolla account balance creates a `Transaction` of type `deposit`.

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


## List a user's transactions

## List an app's transactions 

## Get a specific transaction