# Transactions

### How Transactions Work
When a payment is placed, 2 or more transactions are created.  At least one is created to credit the recipient's account balance, and another one to debit the sender's account balance.  

As a simple example, let's say Bob sends $5 to Alice from his account balance.  From this action, two `Transactions` are created:

 1. Transaction `512010`: credits Alice's account with $5
 2. Transaction `512011`: deducts $5 from Bob's account

It's important to keep in mind that when Bob looks at this payment, he will see the credit transaction's ID: `512010`.  This is called the __Sender's Transaction ID__. 

Similarly, when Alice looks up this payment, she will see a different transaction of ID `512011`, which we call the __Recipient's Transaction ID__.

#### Additional Transactions
Additional transactions are created: 

 - if the payment is bank sourced, to fund the payment
 - if the payment is over $10 it will incur a $0.25 _Dwolla Fee_
 - if there is a facilitator fee

### Transaction Types

### Transaction Statuses

## Get all transactions

## Get a specific transaction