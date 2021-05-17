---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to Upward API! You can use our API to onboard your products and enroll customers via Upward's widget.

# API Keys

Upward authenticates your API requests using your account’s API keys. If you do not include your key when making an API request, or use one that is incorrect or outdated, Upward returns an error.

The API key should be kept confidential and only stored on your own servers. Your account’s API key can perform any API request to Upward without restriction.

Each account has a total of two keys: a key pair for test mode and live mode.

# Authentication

Authentication is done via the API key which you can find in your accounts settings endpoint. Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

Test mode secret keys have the prefix sk_test_ and live mode secret keys have the prefix sk_live_. Alternatively, you can use restricted API keys for granular permissions.

Authentication to the API is performed via HTTP Basic Auth. Provide your API key as the basic auth username value. You do not need to provide a password.

If you need to authenticate via bearer auth (e.g., for a cross-origin request), use -H "Authorization: Bearer base64(ai_test_app_id:sk_test_8fD39MqLyjWBarjtP1zdp7bc)"

curl -X GET https://api.onedb.xyz/accounts/193954b0-0ae8-5db7-b640-8110afe56438 \
  -H "Authorization: Bearer base64(ai_test_app_id:sk_test_8fD39MqLyjWBarjtP1zdp7bc)"
All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

> Example Request:

```shell
# With shell, you can just pass the correct header with each request
curl "http://api.upwardfi.com/" \
  -H "Authorization: your_api_key"
```

# Rate limiting

Upward's API are rate limited to prevent abuse that would degrade our ability to maintain consistent API performance for all users. By default, each API key or app is rate limited at 10,000 requests per hour. If your requests are being rate limited, HTTP response code 429 will be returned with an rate_limit_exceeded error.

# Widget Installation

Upward's widget is a front-end UI element that allows customers to grant your application access to their work accounts. It can be displayed on any part of your application.

```javascript
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
</head>
<body>
  <upward-link></upward-link>
  <script src="https://api.upwardfi.com/upward-link.js"></script>
  <script type="text/javascript">
    upwardLink.create({
      pluginKey: 'your_plugin_key',
      apiHost: 'https://api-sandbox.upward.io/v1',
      partnerEnrollmentId: 'key_from_enrollment_api'
    });
    upwardLink.open();
  </script>
</body>
</html>
```
# API calls

## Get partner product ID associated with the product name

### HTTP Request

`GET http://api.upwardfi.com/partner-product-id/<product_name>`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
product_name | null | Name of partner product.

```shell
curl "http://api.upwardfi.com/partner-product-id/auto-finance" \
  -H "Authorization: your_api_key"
```

> The above command returns JSON structured like this:

```json
{
  "partner_product_id": 1,
}
```
