# Authentication

> You'll need to provide your Application key and secret when you initialize our libraries.

```js
var Dwolla = require('dwolla')("key here", "secret here");

Dwolla.setToken("token goes here");
```

```ruby
require 'dwolla'

# set API keys
Dwolla::api_key = "blahblahblah"
Dwolla::api_secret = "shhhhhh"

# set an OAuth access token
Dwolla::token = "JRRTolkien"
```
```php
<?php
require 'dwolla.php';

// Instantiate a new Dwolla REST Client
$Dwolla = new DwollaRestClient("key goes here", "secret goes here");

// Set an OAuth access token:
$Dwolla->setToken("token goes here");
?>
```
```python
from dwolla import DwollaClientApp, DwollaUser

# Instantiate a new Dwolla client
Dwolla = DwollaClientApp("key goes here", "secret goes here")

# set an OAuth access token
DwollaUser = DwollaUser("token goes here")
```
> Invalid or Expired Access Token Response:

```shell
{
  "Success": false,
  "Message": "Invalid access token.",
  "Response": null
}
{
  "Success": false,
  "Message": "Expired access token.",
  "Response": null
}
```

To interact with the API, you will need API keys.  [Create a consumer application](https://www.dwolla.com/applications/create) in order to get an Application `key` and `secret`.

Some API endpoints only require your application `key` and `secret` (for instance, [creating a checkout](#create-a-checkout) or [looking up a user](#lookup-user)), but most require an OAuth `access_token`.

For endpoints that require an OAuth access token, it should be included in the Authorization HTTP header like so:

`Authorization: Bearer <T0K3N_H3R3>`

<aside class="notice">
Needless to say, you should replace `<T0K3N_H3R3>` with an OAuth `access_token`!
</aside>
