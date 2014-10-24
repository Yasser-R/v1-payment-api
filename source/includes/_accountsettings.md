# Account Settings

With the `ManageAccount` OAuth scope, you can set and retrieve a user's account preferences.  

Currently, the only account feature you can enable or disable for a user via the API is Auto Withdrawal.  When Auto Withdrawal is enabled for a user, any payments received by the user will be automatically withdrawn to a bank funding source of their choice.

## Get Auto Withdrawal Status

```php
/**
 * Example 1:
 * Get autowithdrawal status for the 
 * account associted with the OAuth token
 */

<?php
$Account = new Dwolla\Account();
$Account->settings->oauth_token = "foo";

$result = $Account->getAutoWithdrawalStatus();

var_export($result);
?>
```
```ruby
#   Get autowithdrawal status of the user
#   associated with the current OAuth token

puts Dwolla::Accounts.get_auto_withdrawal_status
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
 * Find out whether or not auto-withdrawal is enabled for the 
 * authorized user
 */

Dwolla.getAutoWithdrawalStatus(function(err, data) {
    console.log(data);
});
```

> If enabled, you'll get:

```ruby
{
  "Enabled"   => true,
  "FundingId" => "5da016f7769bcb1de9938a30d194d5a7"
}
```

```js
{
    "Enabled": true,
    "FundingId": "5da016f7769bcb1de9938a30d194d5a7"
}
```

```json
{
  "Success": true,
  "Message": "Success",
  "Response": {
    "Enabled": true,
    "FundingId": "5da016f7769bcb1de9938a30d194d5a7"
  }
}
```

```php
array (
  'Enabled' => true,
  'FundingId' => '5da016f7769bcb1de9938a30d194d5a7',
)
```

> If disabled, you'll get this response:

```php
array (
  'Enabled' => false,
  'FundingId' => '',
)
```

```ruby
{
  "Enabled"   => false,
  "FundingId" => ""
}
```

```js
{
  "Enabled": false,
  "FundingId": ""
}
```

```json
{
  "Success": true,
  "Message": "Success",
  "Response": {
    "Enabled": false,
    "FundingId": ""
  }
}
```

Determine whether the Auto Withdrawal is enabled for the authorized user.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageAccount` scope.</aside>

### HTTP Request

`GET https://www.dwolla.com/oauth/rest/accounts/features/auto_withdrawl`

## Enable/Disable Auto Withdrawal

```php
/**
 * Example 1:
 * Enable autowithdraw for funding source '12345' under the
 * account associted with the OAuth token
 */

<?php
$Account = new Dwolla\Account();
$Account->settings->oauth_token = "foo";

$fundingSource = '5da016f7769bcb1de9938a30d194d5a7';
$result = $Account->toggleAutoWithdrawalStatus(true, $fundingSource);

var_export($result);
?>
```
```ruby
# EXAMPLE 1:
#   Enable autowithdraw for funding source '12345' under the
#   associated with the current OAuth token

puts Dwolla::Accounts.toggle_auto_withdrawl(true, '12345')
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
 * Enable auto-withdraw for the user associated with the OAuth token,
 * for the funding source ID '1234567'.
 */

Dwolla.toggleAutoWithdraw('true', '1234567', function(err, data) {
    console.log(data);
});
```

> If enabled, you'll get this response:

```php
'Enabled'
```

```ruby
"Enabled"
```

```js
"Enabled"
```

```json
{
    "Success": true,
    "Message": "Success",
    "Response": "Enabled"
}
```

Enable or disable Auto Withdrawal for the authorized user.

<aside class="reminder">This endpoint [requires](#authentication) an OAuth access token with the `ManageAccount` scope.</aside>

### HTTP Request

`POST https://www.dwolla.com/oauth/rest/accounts/features/auto_withdrawl`

| Parameter   |  Description                                       |
|-------------|----------------------------------------------------|
| enabled | Boolean value of autowithdrawal feature. |
| fundingId | [Funding source](#funding-sources) ID of account to autowithdraw funds to. |