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
  -d pay_now=yes \
  -d cardholder_name='Jim' \
  -d card_number=4242424242424242 \
  -d exp_month=12 \
  -d exp_year=2017 \
  -d cvc=123
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"invoice created successfully",
    "data":"18gd5UB0o5NJGO1rgAoygmCU"
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
payment_type [required] | none | only allowed payment type is 'one_time'
items[] [required] | none | item name
items_price[] [required] | none | Item price
send_now [optional] | 0 | if set to 1, customer will receive invoice email
pay_now [optional] | no | if set to yes, given credit card will be charged immediately
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
    "message":"payment processed successfully",
    "data":"8ac9l0o54KJGsDhK3gmCU"
  }
]
```

This endpoint allow you to charge a credit or a debit card against an invoice, that has previously been created but not yet paid.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice/refund`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | the id of the invoice to charge
send_now [optional] | 0 | if set to 1, customer will receive invoice email
cardholder_name [optional] | none | actual cardholder name
card_number [optional] | none | the card number, as a string without any separators.
exp_month [optional] | none | two digit number representing the card's expiration month.
exp_year [optional] | none | two or four digit number representing the card's expiration year.
cvc [optional] | none | card security code
currency [optional] | usd | only allowed currency is usd

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
    "data":"8ac9l0o54KJGsDhK3gmCU"
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
    "data":23
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

# Transfer

Use Transfer to send funds from your InstantMerchant account to a third-party recipient or to your own bank account or debit card.

## Transfer to card

```shell
curl https://api.instantmerchant.io/api/v2/transfer/card \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d recipient_name='Jim' \
  -d recipient_email='jim@instantmerchant.io' \
  -d type='card' \
  -d transfer_amount='125' \
  -d transfer_description='gift' \
  -d card_number=4000056655665556 \
  -d exp_month='08' \
  -d exp_year='2017' \
  -d cvc='123' \
  -d currency='usd' \
  -d country='US'
```

> The above command returns JSON structured like this:

```json
[
  {
  "status":true,
  "message":"transaction success",
  "data":"18eOMKB0o5NJGO1rCrCX7RpE"
  }
]
```

This endpoint allows you to transfer $ to debit cards.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/transfer/card`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
recipient_name [required] | none | recipient name
recipient_email [required] | none | recipient email address
type [required] | none | card or bank
transfer_amount [required] | none | A positive integer representing how much to transfer
transfer_description [required] | none | description about the transfer
card_number [required] | none | debit card number
exp_month [required] | none | expiry month
exp_year [required] | none | expiry year
cvc  [required] | none | card security code
currency  [required] | none | 3-letter ISO code for currency
country [required] | none | 2-letter country code

## Transfer to bank account

```shell
curl https://api.instantmerchant.io/api/v2/transfer/bank \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -d recipient_name='Jim' \
  -d recipient_email='jim@instantmerchant.io' \
  -d type='bank' \
  -d transfer_amount='1200' \
  -d transfer_description='donation' \
  -d account_number=000123456789 \
  -d routing_number='110000000' \
  -d account_holder_type='individual' \
  -d bank_name='State bank' \
  -d currency='usd' \
  -d country='US'
```

> The above command returns JSON structured like this:

```json
[
  {
  "status":true,
  "message":"transaction success",
  "data":"18eOKJB0o5NJGO1rZ0F37nod"
  }
]
```

This endpoint allows you to fund any bank account from your InstantMerchant account.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/transfer/bank`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
recipient_name [required] | none | recipient name
recipient_email [required] | none | recipient email address
type [required] | none | card or bank
transfer_amount [required] | none | A positive integer representing how much to transfer
transfer_description [required] | none | description about the transfer
account_number [required] | none | bank account number
routing_number [required] | none | routing number
account_holder_type [required] | none | account type either invidual or company
bank_name [required] | none | name of the bank
currency  [required] | none | 3-letter ISO code for currency
country [required] | none | 2-letter country code