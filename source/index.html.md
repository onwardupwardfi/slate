---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to Upward - a seamless integration to setting up automated payments directly from your customer's income. You can inititiate enrollment by calling Upward's API endpoints and displaying the widget.

This API reference provides information on available endpoints and how to interact with it.

Upward's API is organized around REST. Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. All requests should be over SSL. All request and response bodies, including errors are encoded in JSON.

# API Keys

Upward authenticates your API requests using your account’s API keys. If you do not include your key when making an API request, or use one that is incorrect or outdated, Upward returns an error.

The API key should be kept confidential and only stored on your own servers. Your account’s API key can perform any API request to Upward without restriction.

Each account has a total of two keys: a key pair for test mode and live mode.

# Authentication

Authentication is done via the API key which you can find in your accounts settings endpoint. Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

Test mode secret keys have the prefix sk_test_ and live mode secret keys have the prefix sk_live_. Alternatively, you can use restricted API keys for granular permissions.

Authentication to the API is performed via HTTP Basic Auth. Provide your API key as the basic auth username value. You do not need to provide a password.

If you need to authenticate via bearer auth (e.g., for a cross-origin request), use -H "Authorization: Bearer base64(ai_test_app_id:sk_test_8fD39MqLyjWBarjtP1zdp7bc)"

> Example Request:

```shell
curl -X GET https://api.upwardfi.com/enrollments \
  -H "Authorization: Bearer base64(ai_test_app_id:sk_test_8fD39MqLyjWBarjtP1zdp7bc)"
  ```

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

```shell
# With shell, you can just pass the correct header with each request
curl "http://api.upwardfi.com/enrollments" \
  -H "Authorization: your_api_key"
```

# Rate limiting

Upward's API are rate limited to prevent abuse that would degrade our ability to maintain consistent API performance for all users. By default, each API key or app is rate limited at 10,000 requests per hour. If your requests are being rate limited, HTTP response code 429 will be returned with an rate_limit_exceeded error.

<!-- # Employer Eligibility Check API

This API can be used to check if the customer's employer is supported by Upward before moving onto enrollment.

### HTTP Request

`GET http://api.upwardfi.com/check-employer-eligibility`

### Arguments

Parameter | Type | Description
--------- | ------- | -----------
employer_name *required* | string | Customer employer name

### Response

Parameter | Type | Description
--------- | ------- | -----------
access_employment_data | boolean | Returns whether payroll data can be accessed.
support_direct_deposit | boolean | Returns whether payments can be set up.

```shell
curl "http://api.upwardfi.com/check-employer-eligibility" \
  -H "Authorization Bearer: base64(app_id:app_secret)" \
  -H "Content-Type: application/json" \
  -d $'{
    "employer_name": "Walmart"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "access_employment_data": true,
  "support_direct_deposit": true
}
``` -->

# Customer Enrollment API

This API initiates customer enrollment via Upward.By passing basic customer information Upward will return an enrollment key unique to the customer and current enrollment.

### HTTP Request

`POST http://api.upwardfi.com/enrollments`

### Arguments

Parameter | Type | Description
--------- | ------- | -----------
`email` *required* | string | Email id of customer
`first_name` *required* |string | First Name
`last_name` *required* | string | Last Name
`ssn` *required* | string | Social Security Number
`street` *required* | string | Street address
`city` *required* | string | City
`state` *required* | string | State
`zip5` *required* | string | Zip code
`country` *required* | string | Country
`partner_product_id` *required* | string | Your product id as seen in your portal
`employer` *optional* | string | Customer employer name
`phone_number` *optional* | string | Phone number
`date_of_birth` *optional* | string | Date of birth
`payment_amount` *optional* | string | Recurring amount to be paid to you by the customer
`payment_frequency` *optional* | string | Frequency interval that customer will make payments
`first_payment_date` *optional* | string | Date of first payment
`application_reference_number` *optional* | string | Loan application number
`account_reference_number` *optional* | string | Your account reference number
`bank_routing_number` *optional* | string | Your bank routing number
`bank_account_number` *optional* | string | Your bank account number
`bank_account_type` *optional* | string | Your bank account type
`days_until_expires` *optional* | string | Number of days before this enrollment request expires
`required_employment_start_date` *optional* | string | start date of employment
`required_gross_income` *optional* | number | customer gross income
`required_net_income` *optional* | number | customer net income
`return_w2_data` *optional* | boolean | Specify true if customer w2 data must be returned
`return_paystubs` *optional* | boolean | Specify true if link to customer paystubs must be returned

### Response

Parameter | Type | Description
--------- | ------- | -----------
`enrollment_id` | string | Returns enrollment id.


```shell
curl "http://api.upwardfi.com/enrollments" \
  -H "Authorization Bearer: base64(app_id:app_secret)" \
  -H "Content-Type: application/json" \
  -d $'{
    "partner_product_id": "key_linked_to_partner_product",
    "first_name": "John",
    "last_name": "Doe",
    "ssn": "123456789",
    “street": "Suite 300, 400 TX-121",
    "city": "Lewisville",
    "state": "TX",
    "zip5": "75056",
    "country": "USA",
    "email": "xyz@abc.com",
    "payment_amount": 200.00
  }'
```

> The above command returns JSON structured like this:

```json
{
  "enrollment_id": "YtMXJzGzJcht38SCJuMhzC"
}
```

# Widget Installation

Upward's widget is a front-end UI element that allows customers to grant your application access to their work accounts and to set up automated payments directly from their paychecks. It can be displayed on any part of your application.

### Config parameters

Parameter | Type | Description
--------- | ------- | -----------
`plugin_key` *required* | string | unique key corresponding to their application
`api_host` *required* | string | Link to API environment (Sandbox/Production)
`enrollment_id` *required* | string | Enrollment key that is returned from calling the Customer Enrollment API
`distribution_amount` *required* | string | Amount allocated from income towards your product.

<!-- ### Response -->

<!-- Parameter | Type | Description
--------- | ------- | -----------
customer_id | string | unique customer id
status | string | response status -->

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
      plugin_key: 'your_plugin_key',
      api_host: 'https://api-sandbox.upwardfi.com',
      enrollment_id: 'key_from_enrollment_api',
      distribution_amount: 'distribution_dollar_amount'
    });
    upwardLink.open();
  </script>
</body>
</html>
```
<!-- > The above command returns JSON structured like this:

```json
{
    "customer_ID": "erj4021",
    "status": "success" 
}
``` -->