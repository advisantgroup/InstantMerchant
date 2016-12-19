---
title: API Reference

language_tabs:
  - shell: cURL

toc_footers:
  - <a href='https://support.instantmerchant.io/'>Get Support</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the InstantMerchant API! You can use our API to access InstantMerchant payment endpoints, which you can use to develop mobile app or website that allow you to process payments.

We have language bindings in curl. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```shell
# With curl, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "X-Api-Key: meowmeowmeow"
  -H "X-Api-Secret: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key and API secret.

InstantMerchant uses API key/secret pair to allow access to the API. You can get a new InstantMerchant API key at [developer support](mailto:support@instantmerchant.io).

InstantMerchant expects for the API key / secret to be included in all API requests to the server in a header that looks like the following:

`X-Api-Key: meowmeowmeow`
`X-Api-Secret: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key and API secret.
</aside>

# Invoice

## Create Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d customer=1 \
  -d description='test description' \
  -d date_due='08/12/2017' \
  -d payment_type='one_time' \
  -d items[]='apple' \
  -d items_price[]=20 \
  -d items[]='orange' \
  -d items_price[]=18 \
  -d send_email=1 \
  -d payment_mode=auth_and_capture \
  -d cardholder_name='Jim' \
  -d card_number=4242424242424242 \
  -d exp_month=12 \
  -d exp_year=2020 \
  -d cvc=123
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"invoice created successfully",
    "invoice_num": 12,
    "charge_id":"18gd5UB0o5NJGO1rgAoygmCU"
  }
]
```

This endpoint creates invoice and optionally charges it immediately.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | customer id if created already
description [required] | none | payment description
date_due [required] | none | invoice due date in format mm/dd/yyyy
payment_type [required] | none | only allowed payment type is `one_time`
items[] [required] | none | item name
items_price[] [required] | none | Item price
send_now [optional] | 0 | if set to 1, customer will receive invoice email
payment_mode [optional] | pay_later | if set to `auth_and_capture`, given credit card will be charged immediately. if set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [optional] | none | actual cardholder name
card_number [optional] | none | the card number, as a string without any separators.
exp_month [optional] | none | two digit number representing the card's expiration month.
exp_year [optional] | none | two or four digit number representing the card's expiration year.
cvc [optional] | none | card security code
currency [optional] | usd | only allowed currency is usd

## Send Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/send \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d invoice_num=10
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Invoice notification sent successfully"
  }
]
```

This endpoint allows you to email the invoice to customer.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice/send`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | the identifier of the invoice

## Charge Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/charge \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d payment_mode=auth_and_capture \
  -d invoice_num=30
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"payment processed successfully",
    "charge_id":"8ac9l0o54KJGsDhK3gmCU"
  }
]
```

This endpoint allow you to charge a credit or a debit card against an invoice, that has previously been created but not yet paid.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice/charge`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | the id of the invoice to charge
send_now [optional] | 0 | if set to 1, customer will receive invoice email
payment_mode [optional] | auth_and_capture | if set to `auth_and_capture`, given credit card will be charged immediately. if set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [optional] | none | actual cardholder name
card_number [optional] | none | the card number, as a string without any separators.
exp_month [optional] | none | two digit number representing the card's expiration month.
exp_year [optional] | none | two or four digit number representing the card's expiration year.
cvc [optional] | none | card security code
currency [optional] | usd | only allowed currency is usd

## Capture Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/capture \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d invoice_num=30
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"payment captured successfully",
    "charge_id":"8ac9l0o54KJGsDhK3gmCU",
    "invoice_num":145
  }
]
```

This endpoint allows you to capture the payment of an existing, uncaptured, charge.

Uncaptured payments expire exactly seven days after they are created. If they are not captured by that point in time, they will be marked as refunded and will no longer be capturable.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice/capture`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
charge_id [required] | none | the transaction id of the charge to capture

## Refund

```shell
curl https://api.instantmerchant.io/api/v2/invoice/refund \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d charge_id='1gd5UB0o5NJGO1rgAoygm' \
  -d amount=10
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Refund has been initiated successfully",
    "refund_id":"8ac9l0o54KJGsDhK3gmCU"
  }
]
```

This endpoint allow you to refund a charge that has previously been created but not yet refunded. Funds will be refunded to the credit or debit card that was originally charged. The fees you were originally charged are also refunded.

You can optionally refund only part of a charge. You can do so as many times as you wish until the entire charge has been refunded.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice/refund`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
charge_id [required] | none | the transaction id of the charge to refund
amount [optional] | entire charge | A positive integer representing how much of this charge to refund. Can only refund up to the unrefunded amount remaining of the charge.

# Direct Payment

## Create Charge

```shell
curl https://api.instantmerchant.io/api/v2/charge \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d name='Jim' \
  -d email='jim@instantmerchant.io' \
  -d amount=100 \
  -d authorize_now=1 \
  -d pay_later=1 \
  -d payment_mode=auth_and_capture \
  -d currency='usd' \
  -d address='my address here' \
  -d city='nashville' \
  -d zip=37251 \
  -d state='TN' \
  -d country='US'
  -d description='first payment' \
  -d cardholder_name='Jim' \
  -d card_number=4242424242424242 \
  -d exp_month=12 \
  -d exp_year=2020 \
  -d cvc=123
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"payment processed successfully",
    "charge_id":"18gd5UB0o5NJGO1rgAoygmCU"
  }
]
```

Use this endpoint to charge a credit card.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/charge`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
name [required] | none | customer name
email [required] | none | customer email address
description [required] | none | payment description
amount [required] | none | a positive integer representing how much to charge the card. The minimum amount is $1 USD
address [required] | none | customer address
city [required] | none | city/suburb/town/village
zip [required] | none | zip code or postal code
state [required] | none | 2-letter state code
country [required] | none | 2-letter country code
payment_mode [optional] | auth_and_capture | if set to `auth_and_capture`, given credit card will be charged immediately. if set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [optional] | none | actual cardholder name
card_number [optional] | none | the card number, as a string without any separators.
exp_month [optional] | none | two digit number representing the card's expiration month.
exp_year [optional] | none | two or four digit number representing the card's expiration year.
cvc [optional] | none | card security code
currency [optional] | usd | only allowed currency is usd

## Capture Charge

```shell
curl https://api.instantmerchant.io/api/v2/capture \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d invoice_num=30
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"payment captured successfully",
    "charge_id":"8ac9l0o54KJGsDhK3gmCU",
    "invoice_num":145
  }
]
```

This endpoint allows you to capture the payment of an existing, uncaptured, charge.

Uncaptured payments expire exactly seven days after they are created. If they are not captured by that point in time, they will be marked as refunded and will no longer be capturable.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/capture`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
charge_id [required] | none | the transaction id of the charge to capture

## Refund

```shell
curl https://api.instantmerchant.io/api/v2/refund \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d charge_id='1gd5UB0o5NJGO1rgAoygm' \
  -d amount=10
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Refund has been initiated successfully",
    "refund_id":"8ac9l0o54KJGsDhK3gmCU"
  }
]
```

This endpoint allow you to refund a charge that has previously been created but not yet refunded. Funds will be refunded to the credit or debit card that was originally charged. The fees you were originally charged are also refunded.

You can optionally refund only part of a charge. You can do so as many times as you wish until the entire charge has been refunded.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/refund`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
charge_id [required] | none | the transaction id of the charge to refund
amount [optional] | entire charge | A positive integer representing how much of this charge to refund. Can only refund up to the unrefunded amount remaining of the charge.

# Customer

## Create Customer

```shell
curl https://api.instantmerchant.io/api/v2/customer \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d name='Jim' \
  -d username='jim123' \
  -d email='jim@instantmerchant.io' \
  -d address='my address here' \
  -d city='nashville' \
  -d zip=37251 \
  -d state='TN' \
  -d country='US'
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Customer created successfully",
    "customer_id":23
  }
]
```

This endpoint allows you to create your customers.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/customer`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
name [required] | none | customer name
email [required] | none | customer email address
username [required] | none | unique username
address [required] | none | customer address
city [required] | none | city/suburb/town/village
zip [required] | none | zip code or postal code
state [required] | none | 2-letter state code
country [required] | none | 2-letter country code