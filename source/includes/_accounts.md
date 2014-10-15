# Account Settings

With the `ManageAccount` OAuth scope, you can set and retrieve a user's account preferences.  

Currently, the only account feature you can enable or disable for a user via the API is Auto Withdrawal.  When Auto Withdrawal is enabled for a user, any payments received by the user will be automatically withdrawn to a bank funding source of their choice.

## Retrieve Auto Withdrawal Status

```php
/**
 * Example 1:
 * Get autowithdrawal status for the 
 * account associted with the OAuth token
 */

$status = $Dwolla->getAutoWithdrawalStatus();
if(!$status) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($status); } // Print status
```
```ruby
# EXAMPLE 1:
# 	Get autowithdrawal status of the user
#	associated with the current OAuth token

pp Dwolla.get_autowithdrawal_status
```
```python
'''
    EXAMPLE 1: 
    	Fetch autowithdraw status for the account
    	associated with the current OAuth token.
'''
status = DwollaUser.get_autowithdraw_status
print(status)
```
```js
/***
 * Example 1:
 *
 * Find out whether or not auto-withdrawal is enabled for the user
 * associated with the OAuth token
 */

Dwolla.getAutoWithdrawalStatus(function(err, data) {
    if (err) { console.log(err); }
    console.log(data);
});
```

> If disabled:

```always
{
    "Success": true,
    "Message": "Success",
    "Response": "Disabled"
}
```

Determine whether the Auto Withdrawal is enabled for the authorized user.

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/accounts/features/auto_withdrawl`

## Set Auto Withdrawal Status

```php
/**
 * Example 1:
 * Enable autowithdraw for funding source '12345' under the
 * account associted with the OAuth token
 */

$status = $Dwolla->toggleAutoWithdrawalStatus('true', '12345');
if(!$status) { echo "Error: {$Dwolla->getError()} \n"; } // Check for errors
else { print_r($status); } // Print status
```
```ruby
# EXAMPLE 1:
# 	Enable autowithdraw for funding source '12345' under the
#	associated with the current OAuth token

pp Dwolla.auto_withdrawal(true, '12345')
```
```python
'''
    EXAMPLE 1: 
    	nable autowithdraw for funding source '12345' under the
    	associated with the current OAuth token.
'''
status = DwollaUser.set_autowithdraw_status(true, '12345')
print(status)
```
```js
/***
 * Example 1:
 *
 * Enable auto-withdraw for the user associated with the OAuth token,
 * for the funding ID '1234567'.
 */

Dwolla.toggleAutoWithdraw('true', '1234567', function(err, data) {
    if (err) { console.log(err); }
    console.log(data);
});
```

> If enabled:

```always
{
    "Success": true,
    "Message": "Success",
    "Response": "Enabled"
}
```

Enable or disable Auto Withdrawal for the authorized user.

### HTTP Request

`POST https://www.dwolla.com/oauth/rest/accounts/features/auto_withdrawl`

| Parameter   |  Description                                       |
|-------------|----------------------------------------------------|
| enabled | Boolean value of autowithdrawal feature. |
| fundingId | [Funding source](#funding-sources) ID of account to autowithdraw funds to. |