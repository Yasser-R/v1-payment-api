# Auto Withdrawal

Dwolla allows users to withdraw funds from their Dwolla balance funding sources associated with the account. The API exposes methods to toggle auto-withdrawal functionality.

These endpoints require an OAuth token with the `ManageAccount` scope. 

## Get Auto Withdrawal Status

> Examples with Dwolla Libraries:

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

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/accounts/features/auto_withdrawl?oauth_token={token}`

| Parameter   | Optional? | Description                                       |
|-------------|-----------|---------------------------------------------------|
| oauth_token | no        | A valid OAuth token with `ManageAccount` scope.   |

## Set Auto Withdrawal Status

> Examples with Dwolla Libraries:

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

### HTTP Request

`POST https://www.dwolla.com/oauth/rest/accounts/features/auto_withdrawl
Content-Type: application/json
`

| Parameter   | Optional? | Description                                       |
|-------------|-----------|---------------------------------------------------|
| oauth_token | no        | A valid OAuth token with `ManageAccount` scope.   |
| enabled | no | Boolean value of autowithdrawal feature. |
| fundingId | no | Funding source ID of account to autowithdraw funds to. |