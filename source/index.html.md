---
title: API Reference

language_tabs:
  - shell: cURL
  - javascript: NodeJS

toc_footers:
  - <a href='https://support.instantmerchant.io/'>Get Support</a>

includes:
  - errors

search: true
---

# Introduction

> API Endpoint:

```shell
  https://api.instantmerchant.io/
```
```javascript
  https://api.instantmerchant.io/
```

Welcome to the InstantMerchant API! You can use our API to access InstantMerchant payment endpoints, which you can use to develop mobile app or website that allow you to process payments.

We have language bindings in curl. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

The InstantMerchant API is organized around REST. Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. 

We support cross-origin resource sharing, allowing you to interact securely with our client-side web application API. JSON is returned by all API responses, including errors, although our API libraries convert responses to appropriate language-specific objects.


# Authentication

> To authorize, use this code:

```shell
# With curl, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "X-Api-Key: meowmeowmeow"
  -H "X-Api-Secret: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key and API secret.


InstantMerchant API authenticate your API key/secret in the request. You can get a new InstantMerchant API key by emailing [developer support](mailto:support@instantmerchant.io).

Your API key/secret pair carry many privileges, so be sure to keep them secret! Do not share your secret API key/secret in publicly accessible areas such GitHub, client-side code, and so forth.

InstantMerchant expects the API key and secret to be included in all API requests to the server in a header that looks like the following:

`X-Api-Key: meowmeowmeow`
`X-Api-Secret: meowmeowmeow`

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.


<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key and API secret.
</aside>

# Invoice

## Create Invoice

```shell
curl https://api.instantmerchant.io/api/v1/invoices \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d customer=209 \
  -d description='test description' \
  -d date_due='08/12/2017' \
  -d items[]='apple' \
  -d items_price[]=20 \
  -d items[]='orange' \
  -d items_price[]=12 \
  -d send_now=1 \
  -d payment_mode='auth_and_capture' \
  -d cardholder_name='Jim' \
  -d card_number=4242424242424242 \
  -d exp_month=12 \
  -d exp_year=2020 \
  -d cvc=123 \
  -d invoice_to_staff=254 \
  -d send_invoice_to='jim@instantmerchant.io' \
  -d payment_type='recurring' \
  -d interval='quarterly' \
  -d save_card='true' \
  -d currency=usd \
  -d is_default='true'
```
```javascript
//Configuration
var Imn = require('instantmerchant-node');
var instant = new Imn({
    key : "your api key",
    secret : "your api secret"
});



/*Create Invoice*/

//Request
var params = {
    'customer' : 209,
    'description' : 'test description',
    'date_due' : '08/12/2017',
    'payment_type' : 'one_time',
    'items' : ['apple','test'],
    'items_price' : [20,10],
    'send_email' : 1,
    'payment_mode' : 'auth_and_capture',
    'cardholder_name' : 'Jim',
    'card_number' : 4242424242424242,
    'exp_month' : 12,
    'exp_year' : 2020,
    'cvc' : 123,
    'invoice_to_staff' : 254,
    'send_invoice_to' : 'jim@instantmerchant.io',
    'payment_type' : 'recurring',
    'interval' : 'quarterly',
    'save_card' : 'true',
    'currency' : 'usd',
    'is_default' : 'true'
};

instant.invoice.create(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": " invoice created successfully ",
    "invoice_num": 303,
    "subscription_id": "sub_5893571433d8c1",
    "expiry_date": 1493701200,
    "charge_id": "cha_58935717308ee1",
    "card_id": "card_589357173094d",
    "card_last_4": "4242"
  }
]
```

This endpoint creates invoice and optionally charges it immediately.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/invoices`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [optional] | none | Required `customer`, only for existing customers.
description [required] | none | Payment description
date_due [required] | none | Invoice due date in format mm/dd/yyyy
items[] [required] | none | Item name
items_price[] [required] | none | Item price
send_now [optional] | 0 | If set to 1, customer will receive invoice email
payment_mode [required] | pay_later | If set to `auth_and_capture`, given credit card will be charged immediately. If set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [optional] | none | Actual cardholder name. Required, when `card_id` is not present. 
card_number [optional] | none | The card number, as a string without any separators. Required, when `card_id` is not present.
exp_month [optional] | none | Two digit number representing the card's expiration month. Required, when `card_id` is not present.
exp_year [optional] | none | Two or four digit number representing the card's expiration year. Required, when `card_id` is not present.
cvc [optional] | none | Card security code. Required, when `card_id` is not present.
currency [required] | none | Only allowed currency is `USD` or `usd`.
invoice_to_staff [optional] | none | The identifier of the staff.
send_invoice_to [optional] | none | Email address to notify about the invoice
address [optional] | none | Required, when the `customer` is new.
city [optional] | none | Required, when the `customer` is new.
state [optional] | none | Required, when the `customer` is new.
zip [optional] | none | Required, when the `customer` is new.
country [optional] | US | Only allowed country is `US`. Required, when the `customer` is new.
payment_type [required] | one_time | If set to `recurring` , subscription will be added to the charge.
interval [optional] | false | Required, when payment_type is set to `recurring`.
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.
card_id [optional] | none | Required, when card details are not present.

## Send Invoice

```shell
curl https://api.instantmerchant.io/api/v1/invoices/send \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d invoice_num=10
```
```javascript
//Request
var params = {
    invoice_num: 10
};

instant.invoice.send(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
  
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

`POST https://api.instantmerchant.io/api/v1/invoices/send`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The identifier of the invoice

## Archive Invoice

```shell
curl https://api.instantmerchant.io/api/v1/invoices/10/archive \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    invoice_num: 10
};

instant.invoice.archive(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
  
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Invoice archive have been updated successfully..!"
  }
]
```

This endpoint allows you to archive an invoice.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/{invoice_num}/archive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The identifier of the invoice

## Unarchive Invoice

```shell
curl https://api.instantmerchant.io/api/v1/invoices/10/unarchive \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    invoice_num: 10
};

instant.invoice.archive(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
  
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Invoice unarchive have been updated successfully..!"
  }
]
```

This endpoint allows you to unarchive an invoice.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/{invoice_num}/unarchive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The identifier of the invoice


## Retrieve an Invoice

```shell
curl https://api.instantmerchant.io/api/v1/invoices?invoice_num=11 \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    invoice_num: '11'
};

instant.invoice.retrieve(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Invoice data retrieved successfully..!",
    "invoice_data": {
        "id": "11",
        "client_id": "1",
        "customer_id": "20",
        "customer_name": "customer.test1",
        "email": "abc@test.com",
        "invoice_num": "11",
        "status": "Refund",
        "amount": "55.00",
        "archived": "0",
        "date_created": "2016-12-21 04:28:38"
    }
}
]
```

This endpoint allows you to retrieve an invoice.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices?invoice_num={invoice_num}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The Identifier of the Invoice.


## List all Paid Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices/paid \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Invoice retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 277,
        "invoices": [{
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "fghgfhfg",
            "date_paid": "2016-12-21 02:15:35",
            "invoice_num": "1",
            "status": "Paid",
            "amount": "5.00",
            "date_created": "2016-12-21 02:15:33"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": "0",
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "fgdfgfg",
            "date_paid": "2016-12-20 02:18:38",
            "invoice_num": "2",
            "status": "Paid",
            "amount": "1000.00",
            "date_created": "2016-12-20 02:18:37"
        }, {
            "client_id": "1",
            "customer_id": "333",
            "created_by": null,
            "customer_name": "test21-01",
            "email": "test21-01@gmail.com",
            "description": "test21-01 newcard save default onetime paynow",
            "date_paid": "2016-12-21 02:30:31",
            "invoice_num": "3",
            "status": "Paid",
            "amount": "211.00",
            "date_created": "2016-12-21 02:30:30"
        }, {
            "client_id": "1",
            "customer_id": "333",
            "created_by": null,
            "customer_name": "test21-01",
            "email": "test21-01@gmail.com",
            "description": "test21-01 onetime auth",
            "date_paid": "2016-12-21 02:33:32",
            "invoice_num": "4",
            "status": "Paid",
            "amount": "212.00",
            "date_created": "2016-12-21 02:33:31"
        }, {
            "client_id": "1",
            "customer_id": "333",
            "created_by": null,
            "customer_name": "test21-01",
            "email": "test21-01@gmail.com",
            "description": "test21-01 onetime paylater",
            "date_paid": "2016-12-21 02:43:52",
            "invoice_num": "6",
            "status": "Paid",
            "amount": "214.00",
            "date_created": "2016-12-21 02:41:17"
        }, {
            "client_id": "1",
            "customer_id": "334",
            "created_by": null,
            "customer_name": "test21-03",
            "email": "test21_03@gmail.com",
            "description": "test21-03 recurring paylater ",
            "date_paid": "2016-12-21 02:44:55",
            "invoice_num": "7",
            "status": "Paid",
            "amount": "215.00",
            "date_created": "2016-12-21 02:42:17"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": "0",
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "ghghfg",
            "date_paid": "2016-12-21 03:31:43",
            "invoice_num": "9",
            "status": "Paid",
            "amount": "500.00",
            "date_created": "2016-12-21 03:31:41"
        }, {
            "client_id": "1",
            "customer_id": "287",
            "created_by": "2",
            "customer_name": "saranya",
            "email": "sarantest2@test.com",
            "description": "testing",
            "date_paid": "2017-01-30 05:24:06",
            "invoice_num": "13",
            "status": "Paid",
            "amount": "34.00",
            "date_created": "2016-12-21 05:25:58"
        }, {
            "client_id": "1",
            "customer_id": "287",
            "created_by": null,
            "customer_name": "saranya",
            "email": "sarantest2@test.com",
            "description": "testing",
            "date_paid": "2016-12-21 06:29:13",
            "invoice_num": "16",
            "status": "Paid",
            "amount": "47.00",
            "date_created": "2016-12-21 06:29:11"
        }, {
            "client_id": "1",
            "customer_id": "334",
            "created_by": null,
            "customer_name": "test21-03",
            "email": "test21_03@gmail.com",
            "description": "test21-03 recurring paylater ",
            "date_paid": "2016-12-21 02:44:55",
            "invoice_num": "22",
            "status": "Paid",
            "amount": "215.00",
            "date_created": "2016-12-21 02:42:17"
        }]
    }
}
]
```

This endpoint allows you to retrieves a paid invoices.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/paid`

## List all Uncaptured Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices/uncaptured \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Invoice retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 22,
        "invoices": [{
            "client_id": "1",
            "customer_id": "259",
            "created_by": null,
            "customer_name": "client_123",
            "email": "teclient_123@test.com",
            "description": "test",
            "date_paid": "2016-12-22 09:54:54",
            "invoice_num": "28",
            "status": "uncaptured",
            "amount": "21.00",
            "date_created": "2016-12-22 09:54:29"
        }, {
            "client_id": "1",
            "customer_id": "19",
            "created_by": null,
            "customer_name": "Somnium Labs",
            "email": "raja@somniumlabs.com",
            "description": "test draft",
            "date_paid": "2016-12-22 10:25:12",
            "invoice_num": "32",
            "status": "uncaptured",
            "amount": "11.00",
            "date_created": "2016-12-22 10:25:11"
        }, {
            "client_id": "1",
            "customer_id": "347",
            "created_by": null,
            "customer_name": "test_customer13",
            "email": "test_customer13@test.com",
            "description": "test draft",
            "date_paid": "2016-12-22 10:30:22",
            "invoice_num": "36",
            "status": "uncaptured",
            "amount": "11.00",
            "date_created": "2016-12-22 10:27:34"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "hfghfgh",
            "date_paid": "2016-12-26 03:44:38",
            "invoice_num": "50",
            "status": "uncaptured",
            "amount": "452.00",
            "date_created": "2016-12-26 03:43:42"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "hgfghfghgfh",
            "date_paid": "2016-12-26 04:11:10",
            "invoice_num": "54",
            "status": "uncaptured",
            "amount": "16.00",
            "date_created": "2016-12-26 04:11:09"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "dsgdsg",
            "date_paid": "2016-12-26 04:13:41",
            "invoice_num": "57",
            "status": "uncaptured",
            "amount": "24.00",
            "date_created": "2016-12-26 04:13:40"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "jghjghjh",
            "date_paid": "2016-12-26 04:14:46",
            "invoice_num": "58",
            "status": "uncaptured",
            "amount": "18.00",
            "date_created": "2016-12-26 04:13:45"
        }, {
            "client_id": "1",
            "customer_id": "357",
            "created_by": null,
            "customer_name": "t002",
            "email": "t002@gmail.com",
            "description": "t002",
            "date_paid": "2016-12-26 07:29:40",
            "invoice_num": "61",
            "status": "uncaptured",
            "amount": "102.00",
            "date_created": "2016-12-26 07:29:39"
        }, {
            "client_id": "1",
            "customer_id": "335",
            "created_by": "286",
            "customer_name": "testing",
            "email": "test@advisantgroup.com",
            "description": "sadsaf",
            "date_paid": "2017-01-25 07:56:03",
            "invoice_num": "75",
            "status": "uncaptured",
            "amount": "45.00",
            "date_created": "2016-12-28 06:31:49"
        }, {
            "client_id": "1",
            "customer_id": "25",
            "created_by": null,
            "customer_name": "Newuser36 lastname",
            "email": "Newuser36@test.com",
            "description": "testing",
            "date_paid": "2017-01-04 10:00:13",
            "invoice_num": "90",
            "status": "uncaptured",
            "amount": "16.00",
            "date_created": "2017-01-04 10:00:11"
        }]
    }
  }
]
```

This endpoint allows you to retrieves a uncaptured/pre-auth invoices.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/uncaptured`


## List all Refunded Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices/refunds \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Invoice retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 72,
        "invoices": [{
            "client_id": "1",
            "customer_id": "334",
            "created_by": "0",
            "customer_name": "test21-03",
            "email": "test21_03@gmail.com",
            "description": "test21-03 recurring paynow newcard save ",
            "date_paid": "2016-12-21 02:37:36",
            "invoice_num": "5",
            "status": "Refund",
            "amount": "213.00",
            "date_created": "2016-12-21 02:37:34"
        }, {
            "client_id": "1",
            "customer_id": "335",
            "created_by": "0",
            "customer_name": "testing",
            "email": "test@advisantgroup.com",
            "description": "sdfdsf",
            "date_paid": "2016-12-21 02:51:38",
            "invoice_num": "8",
            "status": "Refund",
            "amount": "20.00",
            "date_created": "2016-12-21 02:51:37"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": "0",
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "ghfghfghg",
            "date_paid": "2016-12-21 04:28:39",
            "invoice_num": "11",
            "status": "Refund",
            "amount": "55.00",
            "date_created": "2016-12-21 04:28:38"
        }, {
            "client_id": "1",
            "customer_id": "287",
            "created_by": "0",
            "customer_name": "saranya",
            "email": "saran@testing.com",
            "description": "testing",
            "date_paid": "2016-12-21 06:30:23",
            "invoice_num": "18",
            "status": "Refund",
            "amount": "40.00",
            "date_created": "2016-12-21 06:30:22"
        }, {
            "client_id": "1",
            "customer_id": "287",
            "created_by": "0",
            "customer_name": "saranya",
            "email": "saran@testing.com",
            "description": "testing",
            "date_paid": "2016-12-21 06:32:55",
            "invoice_num": "20",
            "status": "Refund",
            "amount": "20.00",
            "date_created": "2016-12-21 06:32:52"
        }, {
            "client_id": "1",
            "customer_id": "343",
            "created_by": "0",
            "customer_name": "raja01",
            "email": "raja@gmail.com",
            "description": "test-renewal",
            "date_paid": "2016-12-21 09:57:16",
            "invoice_num": "23",
            "status": "Refund",
            "amount": "123.00",
            "date_created": "2016-12-21 09:52:57"
        }, {
            "client_id": "1",
            "customer_id": "259",
            "created_by": "254",
            "customer_name": "client_123",
            "email": "teclient_123@test.com",
            "description": "test",
            "date_paid": "2016-12-22 09:53:58",
            "invoice_num": "26",
            "status": "Refund",
            "amount": "12.00",
            "date_created": "2016-12-22 09:38:36"
        }, {
            "client_id": "1",
            "customer_id": "256",
            "created_by": "254",
            "customer_name": "client_staff1",
            "email": "uclient_staff1@test.com",
            "description": "test",
            "date_paid": "2016-12-22 09:40:13",
            "invoice_num": "27",
            "status": "Refund",
            "amount": "21.00",
            "date_created": "2016-12-22 09:40:12"
        }, {
            "client_id": "1",
            "customer_id": "348",
            "created_by": "0",
            "customer_name": "test_customer14",
            "email": "test_customer14@test.com",
            "description": "test draft",
            "date_paid": "2016-12-22 11:15:23",
            "invoice_num": "37",
            "status": "Refund",
            "amount": "11.00",
            "date_created": "2016-12-22 10:27:50"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": "0",
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "fgffgfg",
            "date_paid": "2016-12-22 11:07:11",
            "invoice_num": "42",
            "status": "Refund",
            "amount": "15.00",
            "date_created": "2016-12-22 11:07:10"
        }]
    }
  }
]
```

This endpoint allows you to retrieves refunded invoices.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/refunds`


## List all Chargeback Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices/chargebacks \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Invoice retrieved successfully. ",
    "has_more": false,
    "data": {
        "total_count": 2,
        "invoices": [{
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "fghgfhfg",
            "date_paid": "2016-12-21 02:15:35",
            "invoice_num": "1",
            "status": "Chargeback",
            "amount": "5.00",
            "date_created": "2016-12-21 02:15:33"
        }, {
            "client_id": "1",
            "customer_id": "24",
            "created_by": null,
            "customer_name": "bps1.somniumlabs",
            "email": "bps1@somniumlabs.com",
            "description": "newdesc",
            "date_paid": "2016-12-21 02:19:25",
            "invoice_num": "3",
            "status": "Chargeback",
            "amount": "52.00",
            "date_created": "2016-12-21 02:19:23"
        }]
  
    }
  }
]
```

This endpoint allows you to retrieves chargedback invoices.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/chargebacks`

## List all Void Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices/voids \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Invoice retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 22,
        "invoices": [{
            "client_id": "1",
            "customer_id": "20",
            "created_by": "0",
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "ghfhfghg",
            "date_paid": "2016-12-21 03:43:13",
            "invoice_num": "10",
            "status": "Void",
            "amount": "100.00",
            "date_created": "2016-12-21 03:43:12"
        }, {
            "client_id": "1",
            "customer_id": "385",
            "created_by": "1",
            "customer_name": "api_cus01",
            "email": "api_cus01@test.com",
            "description": "authcharge",
            "date_paid": "2017-01-30 05:25:38",
            "invoice_num": "86",
            "status": "Void",
            "amount": "12.00",
            "date_created": "2017-01-04 08:21:10"
        }, {
            "client_id": "1",
            "customer_id": "385",
            "created_by": "384",
            "customer_name": "api_cus01",
            "email": "api_cus01@test.com",
            "description": "authcharge",
            "date_paid": "2017-01-04 08:21:20",
            "invoice_num": "87",
            "status": "Void",
            "amount": "13.00",
            "date_created": "2017-01-04 08:21:18"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": "1",
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test",
            "date_paid": "2017-01-07 01:37:54",
            "invoice_num": "162",
            "status": "Void",
            "amount": "25.00",
            "date_created": "2017-01-07 01:37:14"
        }, {
            "client_id": "1",
            "customer_id": "256",
            "created_by": "2",
            "customer_name": "client_staff1",
            "email": "uclient_staff1@test.com",
            "description": "test",
            "date_paid": "2017-01-30 03:44:18",
            "invoice_num": "270",
            "status": "Void",
            "amount": "12.00",
            "date_created": "2017-01-30 03:44:16"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": "1",
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test",
            "date_paid": "2017-01-30 04:16:19",
            "invoice_num": "277",
            "status": "Void",
            "amount": "12.00",
            "date_created": "2017-01-30 04:16:18"
        }, {
            "client_id": "1",
            "customer_id": "335",
            "created_by": "1",
            "customer_name": "testing",
            "email": "test@advisantgroup.com",
            "description": "fdmhf",
            "date_paid": "2017-01-30 04:25:30",
            "invoice_num": "283",
            "status": "Void",
            "amount": "34.00",
            "date_created": "2017-01-30 04:25:29"
        }, {
            "client_id": "1",
            "customer_id": "21",
            "created_by": "2",
            "customer_name": "Jim test",
            "email": "jim@test.com",
            "description": "asda",
            "date_paid": "2017-01-31 04:24:25",
            "invoice_num": "318",
            "status": "Void",
            "amount": "23.00",
            "date_created": "2017-01-31 04:24:23"
        }, {
            "client_id": "1",
            "customer_id": "21",
            "created_by": "2",
            "customer_name": "Jim test",
            "email": "jim@test.com",
            "description": "asda",
            "date_paid": "2017-01-31 04:26:09",
            "invoice_num": "319",
            "status": "Void",
            "amount": "400.00",
            "date_created": "2017-01-31 04:25:28"
        }, {
            "client_id": "1",
            "customer_id": "23",
            "created_by": "1",
            "customer_name": "Newuser36 lastname",
            "email": "Newuser36@test.com",
            "description": "erwerw",
            "date_paid": "2017-01-31 04:29:55",
            "invoice_num": "320",
            "status": "Void",
            "amount": "235.00",
            "date_created": "2017-01-31 04:29:53"
        }]
    }
  }
]
```

This endpoint allows you to retrieves void invoices.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/voids`


## List all Pending Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices/pending \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[ 
    {
    "status": true,
    "message": "Invoice retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 25,
        "invoices": [{
            "client_id": "1",
            "customer_id": "287",
            "created_by": null,
            "customer_name": "saranya",
            "email": "test3@testing.com",
            "description": "testing",
            "date_paid": null,
            "invoice_num": "12",
            "status": "Pending",
            "amount": "34.00",
            "date_created": "2016-12-21 05:24:15"
        }, {
            "client_id": "1",
            "customer_id": "287",
            "created_by": null,
            "customer_name": "saranya",
            "email": "test3@testing.com",
            "description": "testing",
            "date_paid": null,
            "invoice_num": "17",
            "status": "Pending",
            "amount": "47.00",
            "date_created": "2016-12-21 06:30:09"
        }, {
            "client_id": "1",
            "customer_id": "287",
            "created_by": null,
            "customer_name": "saranya",
            "email": "test3@testing.com",
            "description": "testing",
            "date_paid": null,
            "invoice_num": "19",
            "status": "Pending",
            "amount": "40.00",
            "date_created": "2016-12-21 06:32:41"
        }, {
            "client_id": "1",
            "customer_id": "22",
            "created_by": null,
            "customer_name": "Newuser35 test",
            "email": "Newuser35@test.com",
            "description": "refb",
            "date_paid": null,
            "invoice_num": "21",
            "status": "Pending",
            "amount": "7.00",
            "date_created": "2016-12-21 07:14:00"
        }, {
            "client_id": "1",
            "customer_id": "394",
            "created_by": null,
            "customer_name": "test06-01",
            "email": "test06_01@gmail.com",
            "description": "t-10",
            "date_paid": null,
            "invoice_num": "195",
            "status": "Pending",
            "amount": "41.00",
            "date_created": "2017-01-10 04:28:01"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test draft",
            "date_paid": null,
            "invoice_num": "244",
            "status": "Pending",
            "amount": "11.00",
            "date_created": "2017-01-22 03:00:05"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test draft",
            "date_paid": null,
            "invoice_num": "245",
            "status": "Pending",
            "amount": "11.00",
            "date_created": "2017-01-23 03:00:02"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test",
            "date_paid": null,
            "invoice_num": "327",
            "status": "Pending",
            "amount": "55.00",
            "date_created": "2017-01-31 08:10:52"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test",
            "date_paid": null,
            "invoice_num": "328",
            "status": "Pending",
            "amount": "56.00",
            "date_created": "2017-01-31 08:13:45"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test",
            "date_paid": null,
            "invoice_num": "329",
            "status": "Pending",
            "amount": "56.00",
            "date_created": "2017-01-31 08:14:27"
        }]
    }
  }
]
```

This endpoint allows you to retrieves pending invoices.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/pending`

## List all Draft Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices/draft \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[ 
   {
    "status": true,
    "message": "Invoice retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 53,
        "invoices": [{
            "client_id": "1",
            "customer_id": "287",
            "created_by": null,
            "customer_name": "saranya",
            "email": "saranya.v@test.com",
            "description": "testing",
            "date_paid": null,
            "invoice_num": "14",
            "status": "draft",
            "amount": "47.00",
            "date_created": "2016-12-21 06:28:23"
        }, {
            "client_id": "1",
            "customer_id": "287",
            "created_by": null,
            "customer_name": "saranya",
            "email": "saranya.v@test.com",
            "description": "testing",
            "date_paid": null,
            "invoice_num": "15",
            "status": "draft",
            "amount": "47.00",
            "date_created": "2016-12-21 06:28:46"
        }, {
            "client_id": "1",
            "customer_id": "19",
            "created_by": null,
            "customer_name": "Somnium Labs",
            "email": "raja@somniumlabs.com",
            "description": "test draft",
            "date_paid": null,
            "invoice_num": "29",
            "status": "draft",
            "amount": "11.00",
            "date_created": "2016-12-22 10:23:19"
        }, {
            "client_id": "1",
            "customer_id": "19",
            "created_by": null,
            "customer_name": "Somnium Labs",
            "email": "raja@somniumlabs.com",
            "description": "test draft",
            "date_paid": null,
            "invoice_num": "30",
            "status": "draft",
            "amount": "11.00",
            "date_created": "2016-12-22 10:23:49"
        }, {
            "client_id": "1",
            "customer_id": "345",
            "created_by": null,
            "customer_name": "test_customer11",
            "email": "test_customer11@test.com",
            "description": "test draft",
            "date_paid": null,
            "invoice_num": "34",
            "status": "draft",
            "amount": "11.00",
            "date_created": "2016-12-22 10:26:27"
        }, {
            "client_id": "1",
            "customer_id": "358",
            "created_by": null,
            "customer_name": "t003",
            "email": "t003@gmail.com",
            "description": "t003",
            "date_paid": null,
            "invoice_num": "62",
            "status": "draft",
            "amount": "103.00",
            "date_created": "2016-12-26 07:30:47"
        }, {
            "client_id": "1",
            "customer_id": "22",
            "created_by": null,
            "customer_name": "Newuser35 test",
            "email": "Newuser35@test.com",
            "description": "testing data",
            "date_paid": null,
            "invoice_num": "76",
            "status": "draft",
            "amount": "14.00",
            "date_created": "2016-12-29 05:46:43"
        }, {
            "client_id": "1",
            "customer_id": "20",
            "created_by": null,
            "customer_name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "description": "test",
            "date_paid": null,
            "invoice_num": "81",
            "status": "draft",
            "amount": "33.00",
            "date_created": "2017-01-03 08:19:32"
        }, {
            "client_id": "1",
            "customer_id": "387",
            "created_by": null,
            "customer_name": "Jim",
            "email": "jimr@instantmerchant.io",
            "description": "test description",
            "date_paid": null,
            "invoice_num": "92",
            "status": "draft",
            "amount": "38.00",
            "date_created": "2017-01-05 03:47:57"
        }, {
            "client_id": "1",
            "customer_id": "388",
            "created_by": null,
            "customer_name": "Jim",
            "email": "rashi@instantmerchant.io",
            "description": "test description",
            "date_paid": null,
            "invoice_num": "93",
            "status": "draft",
            "amount": "38.00",
            "date_created": "2017-01-05 03:49:17"
        }]
    }
  }
]
```

This endpoint allows you to retrieves draft invoices.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices/draft`


## List all Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message":"Invoice data retrieved successfully",
    "has_more":true,
    "data": {
          "total_count":8,
          "invoices": [
          {
              "client_id": "1",
              "customer_id": "20",
              "created_by": null,
              "customer_name": "customer1",
              "email": "customer1@test.com",
              "description": "charge1",
              "date_paid": "2016-12-21 02:15:35",
              "invoice_num": "1",
              "status": "Paid",
              "amount": "5.00",
              "date_created": "2016-12-21 02:15:33"
          }, {
              "client_id": "1",
              "customer_id": "20",
              "created_by": "0",
              "customer_name": "customer2",
              "email": "customer2@test.com",
              "description": "charge2",
              "date_paid": "2016-12-20 02:18:38",
              "invoice_num": "2",
              "status": "Paid",
              "amount": "1000.00",
              "date_created": "2016-12-20 02:18:37"
          }, {
              "client_id": "1",
              "customer_id": "333",
              "created_by": null,
              "customer_name": "customer3",
              "email": "customer3@testl.com",
              "description": "onetime paynow",
              "date_paid": "2016-12-21 02:30:31",
              "invoice_num": "3",
              "status": "Paid",
              "amount": "211.00",
              "date_created": "2016-12-21 02:30:30"
          }, {
              "client_id": "1",
              "customer_id": "333",
              "created_by": null,
              "customer_name": "customer4",
              "email": "customer4@testl.com",
              "description": "charge4",
              "date_paid": "2016-12-21 02:33:32",
              "invoice_num": "4",
              "status": "Paid",
              "amount": "212.00",
              "date_created": "2016-12-21 02:33:31"
          }, {
              "client_id": "1",
              "customer_id": "334",
              "created_by": "0",
              "customer_name": "customer5",
              "email": "customer5@test.com",
              "description": "charge5 ",
              "date_paid": "2016-12-21 02:37:36",
              "invoice_num": "5",
              "status": "Refund",
              "amount": "213.00",
              "date_created": "2016-12-21 02:37:34"
          }, {
              "client_id": "1",
              "customer_id": "333",
              "created_by": null,
              "customer_name": "customer6",
              "email": "customer6@test.com",
              "description": "charg6",
              "date_paid": "2016-12-21 02:43:52",
              "invoice_num": "6",
              "status": "Paid",
              "amount": "214.00",
              "date_created": "2016-12-21 02:41:17"
          }, {
              "client_id": "1",
              "customer_id": "334",
              "created_by": null,
              "customer_name": "customer7",
              "email": "customer7@test.com",
              "description": "charge7 ",
              "date_paid": "2016-12-21 02:44:55",
              "invoice_num": "7",
              "status": "Paid",
              "amount": "215.00",
              "date_created": "2016-12-21 02:42:17"
          }, {
              "client_id": "1",
              "customer_id": "335",
              "created_by": "0",
              "customer_name": "customer8",
              "email": "customer8@test.com",
              "description": "charge8",
              "date_paid": "2016-12-21 02:51:38",
              "invoice_num": "8",
              "status": "Refund",
              "amount": "20.00",
              "date_created": "2016-12-21 02:51:37"
          }]
    }
  }
]
```

This endpoint allows you to list all the invoices with the limit of 10.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices`

## List all archived Invoices

```shell
curl https://api.instantmerchant.io/api/v1/invoices?archived=1 \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
{
    "status": true,
    "message": "Invoice data retrieved successfully",
    "has_more": false,
    "data": {
        "total_count": 2,
        "invoices": [{
            "client_id": "1",
            "customer_id": "256",
            "created_by": "254",
            "customer_name": "client_staff1",
            "email": "uclient_staff1@test.com",
            "description": "manojj",
            "date_paid": "2017-02-17 08:06:44",
            "invoice_num": "396",
            "status": "Refund",
            "amount": "11.00",
            "date_created": "2017-02-17 08:06:42"
        }, {
            "client_id": "1",
            "customer_id": "471",
            "created_by": "1",
            "customer_name": "ustomer3",
            "email": "customer3@yahoo.com",
            "description": "invoice testing",
            "date_paid": "2017-03-03 16:56:23",
            "invoice_num": "424",
            "status": "Paid",
            "amount": "10.56",
            "date_created": "2017-03-03 16:56:21"
        }]
    }
}
```

This endpoint allows you to list all the archived invoices with the limit of 10.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/invoices?archived={archived_status}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
archived [required] | none | Archive status.

# Charges

## Create Charge

```shell
curl https://api.instantmerchant.io/api/v1/charges \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d name='Jim' \
  -d username='jim_test' \
  -d email='jim@instantmerchant.io' \
  -d amount=100 \
  -d payment_mode='auth_and_capture' \
  -d currency='usd' \
  -d address='my address here' \
  -d city='nashville' \
  -d zip=37251 \
  -d state='TN' \
  -d country='US' \
  -d description='first payment' \
  -d cardholder_name='Jim' \
  -d card_number=4242424242424242 \
  -d exp_month=12 \
  -d exp_year=2020 \
  -d cvc=123 \
  -d invoice_num=26 \
  -d send_email=1 \
  -d payment_type='recurring' \
  -d interval='quarterly' \
  -d customer=29 \
  -d create_customer=true \
  -d save_card='true' \
  -d is_default='true'
```
```javascript
//Request
var params = {
    name: 'Jim',
    username: 'jim_test',
    email: 'jim@instantmerchant.io',
    amount: 100,
    payment_mode: 'auth_and_capture',
    payment_type: 'recurring',
    currency: 'usd',
    address: 'my address here',
    city: 'nashville',
    zip: 37251,
    state: 'TN',
    country: 'US',
    description: 'first payment',
    cardholder_name: 'Jim',
    card_number: 4242424242424242,
    exp_month: 12,
    exp_year: 2020,
    cvc: 123,
    invoice_num:26,
    customer:29,
    interval: 'quarterly',
    create_customer: 'true',
    save_card: 'true',
    is_default: 'true'
};

instant.direct.charge(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "payment processed successfully",
    "customer_id": 363,
    "card_id": "card_58626a702e23c",
    "subscription_id": "sub_58626a702eb9e1",
    "expiry_date": 1490590800,
    "charge_id": "cha_689d0tf96536d1",
    "card_last_4": "4242",
  }
]
```

Use this endpoint to charge a credit card.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/charges`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
name [optional] | none | Customer name. Required, when `customer` is not set.
email [optional] | none | Customer email address. Required, when `customer` is not set.
description [required] | none | Payment description
amount [required] | none | A positive integer representing how much to charge the card. The minimum amount is $1 USD
address [optional] | none | Customer address. Required, when `customer` is not set.
city [optional] | none | City/Suburb/Town/Village. Required, when `customer` is not set.
zip [optional] | none | Zip code or postal code. Required, when `customer` is not set.
state [optional] | none | 2-letter state code. Required, when `customer` is not set.
country [optional] | none | 2-letter country code. Required, when `customer` is not set.
invoice_num [optional] | none | Required for the id of the invoice to charge
payment_mode [required] | auth_and_capture | If set to `auth_and_capture`, given credit card will be charged immediately. if set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [required] | none | Actual cardholder name. when `card_id` is not present.
card_number [required] | none | The card number, as a string without any separators. when `card_id` is not present.
exp_month [required] | none | Two digit number representing the card's expiration month. when `card_id` is not present.
exp_year [required] | none | Two or four digit number representing the card's expiration year. when `card_id` is not present.
cvc [required] | none | Card security code. when `card_id` is not present.
send_email [optional] | 0 | If set to 1, customer will receive payment email
currency [optional] | usd | Only allowed currency is usd
customer [optional] | new | Identifier of the customer.
create_customer [optional] | none | Required, when `customer` is not set.
interval [optional] | false | Required, when payment_type is set to `recurring`.
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.
card_id [optional] | none | Required, when card details are not present.


## Archive Charge

```shell
curl https://api.instantmerchant.io/api/v1/charges/cha_689d0tf96536d1/archive \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    unique_id: 'cha_689d0tf96536d1'
};

instant.direct.archive(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Archive have been updated successfully..!"
  }
]
```

This endpoint allows you to archive the payment transaction.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges/{unique_id}/archive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
unique_id [required] | none | The unique Identifier of the payment.


## Unarchive Charge

```shell
curl https://api.instantmerchant.io/api/v1/charges/cha_689d0tf96536d1/unarchive \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    unique_id: 'cha_689d0tf96536d1'
};

instant.direct.unarchive(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Unarchive have been updated successfully..!"
  }
]
```

This endpoint allows you to unarchive the payment transaction.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges/{unique_id}/unarchive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
unique_id [required] | none | The Identifier of the payment.

## Paid Charges

```shell
curl https://api.instantmerchant.io/api/v1/charges/paid \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Charge data is retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 47,
        "transactions": [{
            "invoice_id": null,
            "created_by": "1",
            "customer_id": null,
            "name": "test",
            "email": "test@test.com",
            "amount": "2.00",
            "balance": "2.00",
            "subscription_id": "0",
            "description": "desc",
            "status": "Paid",
            "city": "city",
            "state": "sttae",
            "zip": "56456464",
            "date_created": "2017-03-01 09:50:18",
            "expiry_date": null,
            "transaction_id": "cha_58b6edba0bebe1"
        }, {
            "invoice_id": null,
            "created_by": "1",
            "customer_id": null,
            "name": "Raja",
            "email": "rajacse10@gmail.com",
            "amount": "10.00",
            "balance": "10.00",
            "subscription_id": "0",
            "description": "test",
            "status": "Paid",
            "city": "city here",
            "state": "state here",
            "zip": "1234567890",
            "date_created": "2017-03-01 11:47:52",
            "expiry_date": null,
            "transaction_id": "cha_58b709480baad1"
        }, {
            "invoice_id": "386",
            "created_by": "1",
            "customer_id": "19",
            "name": "Somnium Labs",
            "email": "raja@somniumlabs.com",
            "amount": "12.00",
            "balance": "12.00",
            "subscription_id": "133",
            "description": "test draft",
            "status": "Paid",
            "city": "my city",
            "state": "my state",
            "zip": "9898",
            "date_created": "2017-03-08 10:39:40",
            "expiry_date": null,
            "transaction_id": "cha_58c033c8525f11"
        }, {
            "invoice_id": "400",
            "created_by": "1",
            "customer_id": "287",
            "name": "saranya",
            "email": "saranya.v@advisantgroup.com",
            "amount": "40.00",
            "balance": "40.00",
            "subscription_id": "0",
            "description": "testing",
            "status": "Paid",
            "city": "Brentwood",
            "state": "TN",
            "zip": "37027",
            "date_created": "2017-02-28 03:43:59",
            "expiry_date": null,
            "transaction_id": "cha_58b5465dae1a31"
        }, {
            "invoice_id": "402",
            "created_by": "1",
            "customer_id": "279",
            "name": "saran",
            "email": "saran@test.com",
            "amount": "56.00",
            "balance": "56.00",
            "subscription_id": "0",
            "description": "test",
            "status": "Paid",
            "city": "Brentwood",
            "state": "TN",
            "zip": "37027",
            "date_created": "2017-03-01 05:48:42",
            "expiry_date": null,
            "transaction_id": "cha_58b6b51af226d1"
        }, {
            "invoice_id": "408",
            "created_by": "1",
            "customer_id": "466",
            "name": "saran1",
            "email": "saranktvr@gmail.com",
            "amount": "12.00",
            "balance": "12.00",
            "subscription_id": "0",
            "description": "test",
            "status": "Paid",
            "city": "city",
            "state": "KY",
            "zip": "654321",
            "date_created": "2017-03-08 08:58:12",
            "expiry_date": null,
            "transaction_id": "cha_58c01c046efa11"
        }, {
            "invoice_id": "411",
            "created_by": "1",
            "customer_id": "20",
            "name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "amount": "12.00",
            "balance": "12.00",
            "subscription_id": "0",
            "description": "test",
            "status": "Paid",
            "city": "city1",
            "state": "Nashville",
            "zip": "37221",
            "date_created": "2017-03-02 10:59:38",
            "expiry_date": null,
            "transaction_id": "cha_58b84f78e58c11"
        }, {
            "invoice_id": "412",
            "created_by": "1",
            "customer_id": "20",
            "name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "amount": "14.00",
            "balance": "14.00",
            "subscription_id": "0",
            "description": "test",
            "status": "Paid",
            "city": "city1",
            "state": "Nashville",
            "zip": "37221",
            "date_created": "2017-03-02 11:00:00",
            "expiry_date": null,
            "transaction_id": "cha_58b84f8f264fd1"
        }, {
            "invoice_id": "413",
            "created_by": "1",
            "customer_id": "19",
            "name": "Somnium Labs",
            "email": "raja@somniumlabs.com",
            "amount": "10.00",
            "balance": "10.00",
            "subscription_id": "0",
            "description": "testing",
            "status": "Paid",
            "city": "my city",
            "state": "my state",
            "zip": "9898",
            "date_created": "2017-03-02 11:02:23",
            "expiry_date": null,
            "transaction_id": "cha_58b8501d080541"
        }, {
            "invoice_id": "414",
            "created_by": "1",
            "customer_id": "20",
            "name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "amount": "12.00",
            "balance": "12.00",
            "subscription_id": "0",
            "description": "test",
            "status": "Paid",
            "city": "city1",
            "state": "Nashville",
            "zip": "37221",
            "date_created": "2017-03-03 05:21:47",
            "expiry_date": null,
            "transaction_id": "cha_58b951c8ef8f61"
        }]
    }
  }
]
```

This endpoint allows you to list paid transactions.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges/paid`


## Pre-auth Charges

```shell
curl https://api.instantmerchant.io/api/v1/charges/uncaptured \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
    {
    "status": true,
    "message": "Charge data is retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 38,
        "transactions": [{
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@advisantgroup.com",
            "amount": "120.00",
            "balance": "0.00",
            "subscription_id": "0",
            "description": "wordpress video - Order #127",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-21 05:48:35",
            "expiry_date": "2016-12-28 05:48:33",
            "transaction_id": "cha_585a6c137bfbf1"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@advisantgroup.com",
            "amount": "120.00",
            "balance": "120.00",
            "subscription_id": "0",
            "description": "wordpress video - Order #174",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-23 04:14:14",
            "expiry_date": "2016-12-30 04:14:12",
            "transaction_id": "cha_585cf8f69ff521"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@advisantgroup.com",
            "amount": "30.00",
            "balance": "30.00",
            "subscription_id": "0",
            "description": "wordpress video - Order #264",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-26 04:03:12",
            "expiry_date": "2017-01-02 04:03:11",
            "transaction_id": "cha_5860eae0a8a141"
        }, {
            "invoice_id": null,
            "created_by": "1",
            "customer_id": null,
            "name": "test",
            "email": "test@test.com",
            "amount": "3.00",
            "balance": "3.00",
            "subscription_id": "0",
            "description": "desc",
            "status": "uncaptured",
            "city": "city",
            "state": "sttae",
            "zip": "56456464",
            "date_created": "2017-03-01 09:43:17",
            "expiry_date": "2017-03-08 09:43:17",
            "transaction_id": "cha_58b6ec134e2351"
        }, {
            "invoice_id": null,
            "created_by": "1",
            "customer_id": null,
            "name": "test",
            "email": "test@test.com",
            "amount": "3.00",
            "balance": "3.00",
            "subscription_id": "0",
            "description": "desc",
            "status": "uncaptured",
            "city": "city",
            "state": "sttae",
            "zip": "56456464",
            "date_created": "2017-03-01 09:46:38",
            "expiry_date": "2017-03-08 09:46:38",
            "transaction_id": "cha_58b6ecdbc245e1"
        }, {
            "invoice_id": null,
            "created_by": "1",
            "customer_id": null,
            "name": "Raja",
            "email": "rajacse10@gmail.com",
            "amount": "10.00",
            "balance": "10.00",
            "subscription_id": "0",
            "description": "test",
            "status": "uncaptured",
            "city": "city here",
            "state": "state here",
            "zip": "1234567890",
            "date_created": "2017-03-01 11:47:17",
            "expiry_date": "2017-03-08 11:47:17",
            "transaction_id": "cha_58b7092402e491"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@advisantgroup.com",
            "amount": "40.00",
            "balance": "40.00",
            "subscription_id": "0",
            "description": "wordpress video - Order #287",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-28 04:28:15",
            "expiry_date": "2017-01-04 04:28:15",
            "transaction_id": "cha_586393bf396481"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@advisantgroup.com",
            "amount": "10.00",
            "balance": "10.00",
            "subscription_id": "0",
            "description": "wordpress video - Order #292",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-28 05:27:22",
            "expiry_date": "2017-01-04 05:27:22",
            "transaction_id": "cha_5863a19a24ab21"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@advisantgroup.com",
            "amount": "30.00",
            "balance": "30.00",
            "subscription_id": "0",
            "description": "wordpress video - Order #300",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-28 06:36:37",
            "expiry_date": "2017-01-04 06:36:37",
            "transaction_id": "cha_5863b1d516bfb1"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "363",
            "name": "saran v",
            "email": "saran@gmail.com",
            "amount": "10.00",
            "balance": "10.00",
            "subscription_id": "0",
            "description": "wordpress video - Order #309",
            "status": null,
            "city": "asfdcgbfvb",
            "state": "CA",
            "zip": "90002",
            "date_created": "2016-12-28 07:32:29",
            "expiry_date": "2017-01-04 07:32:29",
            "transaction_id": "cha_5863beed279911"
        }]
    }
  }
]
```

This endpoint allows you to list pre-auth transactions.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges/uncaptured`


## Chargeback Charges

```shell
curl https://api.instantmerchant.io/api/v1/chargebacks \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
    {
    "status": true,
    "message": "Chargeback data is retrieved successfully. ",
    "has_more": false,
    "data": {
        "total_count": 1,
        "transactions": [{
            "invoice_id": "1",
            "created_by": null,
            "customer_id": "20",
            "name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "amount": "5.00",
            "balance": "5.00",
            "subscription_id": "0",
            "description": "fghgfhfg",
            "status": "Chargeback",
            "city": "city1",
            "state": "Nashville",
            "zip": "37221",
            "date_created": "2016-12-21 02:15:35",
            "expiry_date": null,
            "transaction_id": "cha_585a3a274aae31"
        }]
      }
    }
]
```

This endpoint allows you to list chargeback transactions.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/chargebacks`


## Void Charges

```shell
curl https://api.instantmerchant.io/api/v1/voids \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
    {
    "status": true,
    "message": "void data is retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 23,
        "transactions": [{
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@test.com",
            "amount": "120.00",
            "balance": null,
            "subscription_id": "0",
            "description": "wordpress video - Order #127",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-21 07:33:44",
            "expiry_date": null,
            "transaction_id": "ref_585a84b8936171"
        }, {
            "invoice_id": "10",
            "created_by": null,
            "customer_id": "20",
            "name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "amount": "100.00",
            "balance": null,
            "subscription_id": "0",
            "description": "ghfhfghg",
            "status": null,
            "city": "city1",
            "state": "Nashville",
            "zip": "37221",
            "date_created": "2016-12-21 03:43:39",
            "expiry_date": null,
            "transaction_id": "ref_585a4ecb556941"
        }, {
            "invoice_id": "86",
            "created_by": "1",
            "customer_id": "385",
            "name": "api_cus01",
            "email": "api_cus01@test.com",
            "amount": "12.00",
            "balance": null,
            "subscription_id": "0",
            "description": "authcharge",
            "status": null,
            "city": "Bangalore",
            "state": "TN",
            "zip": "37252",
            "date_created": "2017-02-07 09:00:02",
            "expiry_date": null,
            "transaction_id": "ref_5899e0f207b4f1"
        }, {
            "invoice_id": "87",
            "created_by": "384",
            "customer_id": "385",
            "name": "api_cus01",
            "email": "api_cus01@test.com",
            "amount": "13.00",
            "balance": null,
            "subscription_id": "0",
            "description": "authcharge",
            "status": null,
            "city": "Bangalore",
            "state": "TN",
            "zip": "37252",
            "date_created": "2017-01-04 09:12:16",
            "expiry_date": null,
            "transaction_id": "ref_586d10d03c6d01"
        }, {
            "invoice_id": "162",
            "created_by": "1",
            "customer_id": "20",
            "name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "amount": "25.00",
            "balance": null,
            "subscription_id": "0",
            "description": "test",
            "status": null,
            "city": "city1",
            "state": "Nashville",
            "zip": "37221",
            "date_created": "2017-01-07 01:38:03",
            "expiry_date": null,
            "transaction_id": "ref_58709adb6d1a41"
        }, {
            "invoice_id": "270",
            "created_by": "2",
            "customer_id": "256",
            "name": "client_staff1",
            "email": "uclient_staff1@test.com",
            "amount": "12.00",
            "balance": null,
            "subscription_id": "0",
            "description": "test",
            "status": null,
            "city": "bangalore",
            "state": "PA",
            "zip": "37251",
            "date_created": "2017-01-30 04:21:34",
            "expiry_date": null,
            "transaction_id": "ref_588f13ae8875e1"
        }, {
            "invoice_id": "277",
            "created_by": "1",
            "customer_id": "20",
            "name": "bps.somniumlabs",
            "email": "bps@somniumlabs.com",
            "amount": "12.00",
            "balance": null,
            "subscription_id": "0",
            "description": "test",
            "status": null,
            "city": "city1",
            "state": "Nashville",
            "zip": "37221",
            "date_created": "2017-01-30 04:18:25",
            "expiry_date": null,
            "transaction_id": "ref_588f12f1a5abc1"
        }, {
            "invoice_id": "283",
            "created_by": "1",
            "customer_id": "335",
            "name": "testing",
            "email": "test@advisantgroup.com",
            "amount": "34.00",
            "balance": null,
            "subscription_id": "0",
            "description": "fdmhf",
            "status": null,
            "city": "Brentwood",
            "state": "TN",
            "zip": "37027",
            "date_created": "2017-01-30 04:39:19",
            "expiry_date": null,
            "transaction_id": "ref_588f17d7df34b1"
        }, {
            "invoice_id": "318",
            "created_by": "2",
            "customer_id": "21",
            "name": "Jim test",
            "email": "jim@test.com",
            "amount": "23.00",
            "balance": null,
            "subscription_id": "0",
            "description": "asda",
            "status": null,
            "city": "Test city",
            "state": "Tennessee",
            "zip": "987654321",
            "date_created": "2017-01-31 04:28:28",
            "expiry_date": null,
            "transaction_id": "ref_589066ccbdb481"
        }, {
            "invoice_id": "319",
            "created_by": "2",
            "customer_id": "21",
            "name": "Jim test",
            "email": "jim@test.com",
            "amount": "400.00",
            "balance": null,
            "subscription_id": "0",
            "description": "asda",
            "status": null,
            "city": "Test city",
            "state": "Tennessee",
            "zip": "987654321",
            "date_created": "2017-01-31 04:27:03",
            "expiry_date": null,
            "transaction_id": "ref_58906677eb13d1"
        }]
    }
  }
]
```

This endpoint allows you to list void transactions.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/voids`

## Partial Refund Charges

```shell
curl https://api.instantmerchant.io/api/v1/charges/partial_refund \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
    {
    "status": true,
    "message": "Charge data is retrieved successfully. ",
    "has_more": false,
    "data": {
        "total_count": 1,
        "transactions": [{
            "invoice_id": "1",
            "created_by": "1",
            "customer_id": "242",
            "name": "raja",
            "email": "rajacse10@gmail.com",
            "amount": "17.00",
            "balance": "15.00",
            "subscription_id": "0",
            "description": "client2",
            "status": "Partial_refund",
            "city": "city",
            "state": "TN",
            "zip": "37251",
            "date_created": "2017-03-10 07:36:20",
            "expiry_date": null,
            "transaction_id": "cha_58c2abceaed369"
        }]
    }
  }
]
```

This endpoint allows you to list partial refund transactions.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges/partial_refund`


## List all refunds for the Charge

```shell
curl https://api.instantmerchant.io/api/v1/charges/cha_58c2abceaed369/refunds \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
    {
    "status": true,
    "message": "Refunds are retrieved successfully. ",
    "charge_id": "cha_58c2abceaed369",
    "data": {
        "total_count": 3,
        "refunds": [{
            "invoice_id": "1",
            "created_by": "9",
            "customer_id": "242",
            "name": "raja",
            "email": "rajacse10@gmail.com",
            "amount": "2.00",
            "balance": null,
            "subscription_id": "0",
            "description": "client2",
            "status": "Refund",
            "city": "city",
            "state": "TN",
            "zip": "37251",
            "date_created": "2017-03-10 07:36:53",
            "expiry_date": null,
            "transaction_id": "ref_58c2abf516e419"
        }, {
            "invoice_id": "1",
            "created_by": "9",
            "customer_id": "242",
            "name": "raja",
            "email": "rajacse10@gmail.com",
            "amount": "3.00",
            "balance": null,
            "subscription_id": "0",
            "description": "client2",
            "status": "Refund",
            "city": "city",
            "state": "TN",
            "zip": "37251",
            "date_created": "2017-03-10 07:51:48",
            "expiry_date": null,
            "transaction_id": "ref_58c2af7491dae9"
        }, {
            "invoice_id": "1",
            "created_by": "9",
            "customer_id": "242",
            "name": "raja",
            "email": "rajacse10@gmail.com",
            "amount": "2.00",
            "balance": null,
            "subscription_id": "0",
            "description": "client2",
            "status": "Refund",
            "city": "city",
            "state": "TN",
            "zip": "37251",
            "date_created": "2017-03-10 07:51:59",
            "expiry_date": null,
            "transaction_id": "ref_58c2af7f50e0d9"
        }]
    }
  }
]
```

This endpoint allows you to list of partial refunds for the particular charge.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges/{charge_id}/refunds`


## Capture Charge

```shell
curl https://api.instantmerchant.io/api/v1/charges/cha_585d0bf93573f1/capture \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
```
```javascript
//Request
var params = {
    charge_id: 'cha_585d0bf93573f1'
};

instant.direct.capture(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"payment captured successfully",
    "charge_id":"cha_585d0bf93573f1",
    "invoice_num":"408"
  }
]
```

This endpoint allows you to capture the payment of an existing, uncaptured, charge.

Uncaptured payments expire exactly seven days after they are created. If they are not captured by that point in time, they will be marked as refunded and will no longer be capturable.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/charges/cha_585d0bf93573f1/capture`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
charge_id [required] | none | The unique id of the transaction to capture.

** If it is an Invoice capture, `invoice_num` will be returned along with JSON output.

## Refund

```shell
curl https://api.instantmerchant.io/api/v1/refunds \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d charge_id='cha_585d0bf93573f1' \
  -d amount=10
```
```javascript
//Request
var params = {
    charge_id: 'cha_585d0bf93573f1',
    amount: 10
};

instant.direct.refund(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})

```
> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Refund initiated successfully",
    "refund_id":"ref_397d0me53575f3"
  }
]
```

This endpoint allow you to refund a charge that has previously been created but not yet refunded. Funds will be refunded to the credit or debit card that was originally charged. The fees you were originally charged are also refunded.

You can optionally refund only part of a charge. You can do so as many times as you wish until the entire charge has been refunded.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/refunds`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
charge_id [required] | none | The transaction id of the charge to refund
amount [optional] | entire charge | A positive integer representing how much of this charge to refund. Can only refund up to the unrefunded amount remaining of the charge.


** If it is an Invoice refund, `invoice_num` will be returned along with JSON output.


## List all charges

```shell
curl https://api.instantmerchant.io/api/v1/charges \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
{
    "status": true,
    "message": "Charge data is retrieved successfully. ",
    "has_more": true,
    "data": {
        "total_count": 431,
        "transactions": [{
            "invoice_id": null,
            "created_by": null,
            "customer_id": "393",
            "name": "sai",
            "email": "sai9178213@test.com",
            "amount": "390.00",
            "balance": "390.00",
            "subscription_id": "0",
            "description": "jdhgfs",
            "status": null,
            "city": "cbe",
            "state": "tn",
            "zip": "641030",
            "date_created": "2017-01-05 04:25:28",
            "expiry_date": null,
            "transaction_id": "cha_586e1f18242f31"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@test.com",
            "amount": "100.00",
            "balance": "100.00",
            "subscription_id": "0",
            "description": "sfdsf",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-21 05:08:09",
            "expiry_date": null,
            "transaction_id": "cha_585a6299875b51"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "338",
            "name": "rashika",
            "email": "rashikak.r@test.com",
            "amount": "120.00",
            "balance": null,
            "subscription_id": "0",
            "description": "wordpress video - Order #127",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-21 07:33:44",
            "expiry_date": null,
            "transaction_id": "ref_585a84b8936171"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "299",
            "name": "test",
            "email": "rettline@gmail.comssasssss",
            "amount": "10.00",
            "balance": "0.00",
            "subscription_id": "0",
            "description": "Payment for Kryptobit Wallet",
            "status": null,
            "city": "ewew",
            "state": "wew",
            "zip": "1232",
            "date_created": "2016-12-21 09:58:35",
            "expiry_date": null,
            "transaction_id": "cha_585aa6ab83d381"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "373",
            "name": "Manoj tendulkar",
            "email": "jim@tester.io",
            "amount": "46.00",
            "balance": "46.00",
            "subscription_id": "31",
            "description": "new payment",
            "status": null,
            "city": null,
            "state": null,
            "zip": null,
            "date_created": "2017-01-06 09:28:08",
            "expiry_date": null,
            "transaction_id": "cha_586fb7888f5171"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": null,
            "name": "rashika",
            "email": "rashikak.r@test.com",
            "amount": "100.00",
            "balance": "0.00",
            "subscription_id": "0",
            "description": "sfdsf",
            "status": null,
            "city": "sfdg",
            "state": "sdafg",
            "zip": "641030",
            "date_created": "2016-12-22 03:05:01",
            "expiry_date": null,
            "transaction_id": "cha_585b973d5e6051"
        }, {
            "invoice_id": null,
            "created_by": "1",
            "customer_id": "299",
            "name": "test",
            "email": "rettline@gmail.comssasssss",
            "amount": "1.00",
            "balance": "0.00",
            "subscription_id": "0",
            "description": "Payment for Kryptobit Wallet",
            "status": null,
            "city": "ewew",
            "state": "wew",
            "zip": "1232",
            "date_created": "2017-01-31 16:40:26",
            "expiry_date": null,
            "transaction_id": "cha_5891125a1b4471"
        }, {
            "invoice_id": null,
            "created_by": "1",
            "customer_id": "299",
            "name": "test",
            "email": "rettline@gmail.comssasssss",
            "amount": "12.00",
            "balance": "12.00",
            "subscription_id": "0",
            "description": "Payment for Kryptobit Wallet",
            "status": null,
            "city": "ewew",
            "state": "wew",
            "zip": "1232",
            "date_created": "2017-02-07 08:09:44",
            "expiry_date": null,
            "transaction_id": "cha_5899d528ac58d1"
        }, {
            "invoice_id": null,
            "created_by": "1",
            "customer_id": "26",
            "name": "Newuser37 Newuser37",
            "email": "Newuser37@test.com",
            "amount": "100.00",
            "balance": "100.00",
            "subscription_id": "0",
            "description": "first payment",
            "status": null,
            "city": "Newuser37",
            "state": "Blaenau Gwent",
            "zip": "98765463",
            "date_created": "2017-02-13 05:22:24",
            "expiry_date": null,
            "transaction_id": "cha_58a196eed04761"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": null,
            "name": "Manojkumar02",
            "email": "manoj02@merchant.io",
            "amount": "12.00",
            "balance": "12.00",
            "subscription_id": "0",
            "description": "test description",
            "status": null,
            "city": "newyork",
            "state": "tamilnadu",
            "zip": "12345",
            "date_created": "2016-12-22 11:57:43",
            "expiry_date": null,
            "transaction_id": "cha_585c1417678391"
        }]
    }
}
```
This endpoint allow you to list all the transactions with the limit of 10.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges`

## List all archived charges

```shell
curl https://api.instantmerchant.io/api/v1/charges?archived=1 \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
{
    "status": true,
    "message": "Charge data is retrieved successfully. ",
    "has_more": false,
    "data": {
        "total_count": 4,
        "transactions": [{
            "invoice_id": null,
            "created_by": null,
            "customer_id": "373",
            "name": "Manoj tendulkar",
            "email": "jim@tester.io",
            "amount": "46.00",
            "balance": "46.00",
            "subscription_id": "31",
            "description": "new payment",
            "status": null,
            "city": null,
            "state": null,
            "zip": null,
            "date_created": "2017-01-06 09:28:08",
            "expiry_date": null,
            "transaction_id": "cha_586fb7888f5171"
        }, {
            "invoice_id": null,
            "created_by": null,
            "customer_id": "373",
            "name": "Manoj tendulkar",
            "email": "jim@tester.io",
            "amount": "13.00",
            "balance": "0.00",
            "subscription_id": "31",
            "description": "new payment",
            "status": null,
            "city": "nashville",
            "state": "TN",
            "zip": "37251",
            "date_created": "2017-01-03 08:07:03",
            "expiry_date": null,
            "transaction_id": "cha_586bb007b0bd71"
        }, {
            "invoice_id": "269",
            "created_by": "254",
            "customer_id": "256",
            "name": "client_staff1",
            "email": "uclient_staff1@test.com",
            "amount": "11.00",
            "balance": "11.00",
            "subscription_id": "0",
            "description": "test",
            "status": null,
            "city": "bangalore",
            "state": "PA",
            "zip": "37251",
            "date_created": "2017-01-30 03:44:07",
            "expiry_date": null,
            "transaction_id": "cha_588f0ae77e0411"
        }, {
            "invoice_id": "270",
            "created_by": "2",
            "customer_id": "256",
            "name": "client_staff1",
            "email": "uclient_staff1@test.com",
            "amount": "12.00",
            "balance": null,
            "subscription_id": "0",
            "description": "test",
            "status": null,
            "city": "bangalore",
            "state": "PA",
            "zip": "37251",
            "date_created": "2017-01-30 04:21:34",
            "expiry_date": null,
            "transaction_id": "ref_588f13ae8875e1"
        }]
    }
}
```
This endpoint allow you to list all the archived transactions with the limit of 10.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/charges?archived=1`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
archived [required] | none | Archived status

# Customer

## Create Customer

```shell
curl https://api.instantmerchant.io/api/v1/customers \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d name='Jim' \
  -d username='jim123' \
  -d email='jim@instantmerchant.io' \
  -d address='my address here' \
  -d city='nashville' \
  -d zip=37251 \
  -d state='TN' \
  -d country='US' \
  -d cardholder_name='chris martin' \
  -d card_number=4242424242424242 \
  -d exp_month=12 \
  -d exp_year=2022 \
  -d cvc=111 \
  -d save_card='true' \
  -d is_default='true'
```
```javascript
//Request
var params = {
    name: 'Jim',
    username: 'jim123',
    email: 'jim@instantmerchant.io',
    address: 'my address here',
    city: 'nashville',
    zip: 37251,
    state: 'TN',
    country: 'US',
    cardholder_name: 'chris martin',
    card_number: 4242424242424242,
    exp_month: 12,
    exp_year: 2022,
    cvc: 111,
    save_card:'true',
    is_default:'true'
};

instant.customer.create(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status":true,
    "message":"Customer created successfully and card saved successfully.",
    "customer_id":23,
    "card_id": "card_58626a702e23c",
    "card_last_4":4242
  }
]
```

This endpoint allows you to create your customers as well as customer's card.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/customers`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
name [required] | none | Customer name
email [required] | none | Customer email address
username [required] | none | Unique username
address [required] | none | Customer address
city [required] | none | City/Suburb/Town/Village
zip [required] | none | Zip code or postal code
state [required] | none | 2-letter state code
country [required] | none | 2-letter country code
cardholder_name [optional] | none | Actual cardholder name.
card_number [optional] | none | The card number, as a string without any separators.
exp_month [optional] | none | Two digit number representing the card's expiration month.
exp_year [optional] | none | Two or four digit number representing the card's expiration year.
cvc [optional] | none | Card security code
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.

## Retrieve Customer

```shell
curl https://api.instantmerchant.io/api/v1/customers?id=22 \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
{
    "status": true,
    "message": "Customer data is retrieved successfully..!",
    "customer": [{
        "id": "22",
        "client_id": "1",
        "username": "customer22",
        "name": "customer22_test",
        "email": "customer22@test.com",
        "active": "1",
        "address": "customer22 address",
        "city": "customer22 city",
        "state": "Caerphilly",
        "zip": "9876543210",
        "country": "us"
    }]
}

```
Retrieves the details of an existing customer. You need only supply the unique customer identifier that was returned upon customer creation.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/customers?id={customer_id}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id [required] | none | The identifier of the customer to be retrieved.

## Update Customer

```shell
curl https://api.instantmerchant.io/api/v1/customers \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X PUT \
  -d id=20 \
  -d name='Jim' \
  -d password='bacabcdefgh' \
  -d address='new address here' \
  -d city='nashville' \
  -d zip=37251 \
  -d active=1

```

> The above command returns JSON structured like this:

```json
{
    "status": true,
    "message": "Customer data is updated successfully..!"
}

```
Updates the specified customer by setting the values of the parameters passed. Any one of the parameters needs to be given along with `id`. Any parameters not provided will be left unchanged.

### HTTP Request

`PUT https://api.instantmerchant.io/api/v1/customers`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id [required] | none | The Identifier of the customer to be update.
name [optional] | none | New name of the customer.
password [optional] | none | New password of the customer.
address [optional] | none | New address of the customer.
city [optional] | none | New City/Suburb/Town/Village.
zip [optional] | none | New Zip code or postal code.
active [optional] | 1 | Active status of the customer.

## List all customers

```shell
curl https://api.instantmerchant.io/api/v1/customers \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
{
    "status": true,
    "message": "Customer data is retrieved successfully..!",
    "customer": [{
        "id": "494",
        "client_id": "1",
        "username": "jim123495",
        "name": "Jim",
        "email": "jim@stantmerchant.io",
        "active": "1"
    }, {
        "id": "493",
        "client_id": "1",
        "username": "jim_test120",
        "name": "Jim",
        "email": "jim@chandt.io",
        "active": "1"
    }, {
        "id": "492",
        "client_id": "1",
        "username": "jim_test",
        "name": "Jim",
        "email": "jim@chant.io",
        "active": "1"
    }, {
        "id": "491",
        "client_id": "1",
        "username": "r",
        "name": "t",
        "email": "r@yahoo.com",
        "active": "1"
    }, {
        "id": "490",
        "client_id": "1",
        "username": "Customer28",
        "name": "Customer28",
        "email": "customer28@yahoo.com",
        "active": "1"
    }, {
        "id": "487",
        "client_id": "1",
        "username": "staffname1",
        "name": "0703_s01",
        "email": "staffemail1@gmail.com",
        "active": "1"
    }, {
        "id": "486",
        "client_id": "1",
        "username": "username7",
        "name": "0703_07",
        "email": "useremail7@gmail.com",
        "active": "1"
    }, {
        "id": "485",
        "client_id": "1",
        "username": "username6",
        "name": "0703_06",
        "email": "useremail6@gmail.com",
        "active": "1"
    }, {
        "id": "484",
        "client_id": "1",
        "username": "username5",
        "name": "0703_05",
        "email": "useremail5@gmail.com",
        "active": "1"
    }, {
        "id": "483",
        "client_id": "1",
        "username": "username4",
        "name": "0703_04",
        "email": "useremail4@gmail.com",
        "active": "1"
    }],
    "total": 332
}

```
Returns a list of your customers with a limit of 10. The customers are returned sorted by `id`, with the most recent customers appearing first.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/customers`


# Card

## Create Card

```shell
curl https://api.instantmerchant.io/api/v1/card \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d customer=22 \
  -d description='newcard' \
  -d cardholder_name='Jim' \
  -d card_number='4242424242424242' \
  -d exp_month=03  \
  -d exp_year=2018 \
  -d cvc=123 \
  -d currency='usd' \
  -d save_card='true' \
  -d is_default='true'
```
```javascript

//Request
var params = {
    customer: 22,
    description: 'newcard',
    cardholder_name: 'Jim',
    card_number: 4242424242424242,
    exp_month: 03,
    exp_year: 2018,
    cvc: 123,
    currency: 'usd',
    save_card: 'true',
    is_default: 'true'
};

instant.card.add(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Card created successfully",
    "card_id": "card_585d4e7edcf93",
    "card_last_4": "4242"
  }
]
```

This endpoint allows you to store multiple cards on a customer in order to charge the customer later.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/card`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already
description [optional] | none | An arbitrary string which you can attach to a card object.
cardholder_name [required] | none | Actual cardholder name.
card_number [required] | none | The card number, as a string without any separators.
exp_month [required] | none | Two digit number representing the card's expiration month.
exp_year [required] | none | Two or four digit number representing the card's expiration year.
cvc [required] | none | Card security code
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.

## Retrieve Card

```shell
curl https://api.instantmerchant.io/api/v1/card/?customer=22&card_id=card_585a3da60deae \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    customer: 22,
    card_id: 'card_585a3da60deae'
};

instant.card.get(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Credit card details retrieved successfully",
    "data": [{
        "card_id": "card_585a3da60deae",
        "is_default": "0",
        "card_type": "",
        "cc_last_4": "4242",
        "cc_valid_thru": "1/2017"
    }]
  }
]
```

This endpoint allows you to retrieve details about a specific card stored on the customer.

### HTTP Request
`GET https://api.instantmerchant.io/api/v1/card/?customer={customer_id}&card_id={card_id}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already.
card_id [required] | none | existing stored card id.

## Delete Card

```shell
curl https://api.instantmerchant.io/api/v1/card \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X DELETE \
  -d customer=22 \
  -d card_id='card_585a3da60deae'
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Card had been deleted successfully..!"
  }
]
```

This endpoint allows you to delete cards from the customer.

### HTTP Request

`DELETE https://api.instantmerchant.io/api/v1/card`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already
card_id [optional] | none | Required, when card details are not present.


## List all Cards

```shell
curl https://api.instantmerchant.io/api/v1/card/?customer=22 \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    customer: 22
};

instant.card.get(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Credit card details retrieved successfully",
    "data": [{
        "card_id": "card_585a3da60deae",
        "is_default": "0",
        "card_type": "",
        "cc_last_4": "4242",
        "cc_valid_thru": "1/2017"
    }, {
        "card_id": "card_585a3e5b27e1c",
        "is_default": "0",
        "card_type": "",
        "cc_last_4": "4242",
        "cc_valid_thru": "2/2017"
    }, {
        "card_id": "card_585a40c6e8afb",
        "is_default": "1",
        "card_type": "",
        "cc_last_4": "4242",
        "cc_valid_thru": "5/2017"
    }]
  }
]
```

This endpoint allows you to list all the cards stored on the customer.

### HTTP Request
`GET https://api.instantmerchant.io/api/v1/card/?customer={customer_id}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already.

# Subscription

## New Subscription

You can create subscription on <a href='#create-invoice'>Create Invoice</a> or <a href='#create-charge'> Charges </a>.

## Renew Subscription

```shell
curl https://api.instantmerchant.io/api/v1/subscriptions/renew \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d subscription_id='sub_58611e30ae9131'
```
```javascript
//Request
var params = {
    subscription_id: 'sub_58611e30ae9131'
};

instant.subscription.renew(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "subscription renewed successfully",
    "charge_id": "cha_58612419c7e571",
    "subscription_id": "sub_58611e30ae9131",
    "expiry_date": 1485410400,
    "invoice_num": 36
  }
]
```

This endpoint allows you to renew subscription.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/subscriptions/renew`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## Cancel Subscription

```shell
curl https://api.instantmerchant.io/api/v1/subscription/sub_587616ba745ca1/cancel \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    subscription_id: 'sub_587616ba745ca1'
};

instant.subscription.cancel(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "subscription canceled successfully"
  }
]
```

This endpoint allows you to cancel your subscription.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/subscription/{subscription_id}/cancel`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## Suspend Subscription

```shell
curl https://api.instantmerchant.io/api/v1/subscription/sub_587616ba745ca1/suspend \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    subscription_id: 'sub_587616ba745ca1'
};

instant.subscription.cancel(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "subscription suspended successfully"
  }
]
```

This endpoint allows you to cancel your subscription.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/subscription/{subscription_id}/suspend`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## Resume Subscription

```shell
curl https://api.instantmerchant.io/api/v1/subscription/sub_587616ba745ca1/resume \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    subscription_id: 'sub_587616ba745ca1'
};

instant.subscription.cancel(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "subscription Resumed successfully"
  }
]
```

This endpoint allows you to cancel your subscription.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/subscription/{subscription_id}/resume`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.


## Update Card

```shell
curl https://api.instantmerchant.io/api/v1/subscription/sub_58611e30ae9131/update_card \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d subscription_id='sub_58611e30ae9131' \
  -d cardholder_name='JimTest' \
  -d card_number=4242424242424242 \
  -d exp_month=11 \
  -d exp_year=2019 \
  -d cvc=999 \
  -d amount=200 \
  -d save_card='true' \
  -d is_default='true'
```
```javascript
//Request
var params = {
    subscription_id: 'sub_58611e30ae9131',
    cardholder_name: 'Jim',
    card_number: 4242424242424242,
    exp_month: 11,
    exp_year: 2019,
    cvc: 999,
    amount:200,
    save_card: 'true',
    is_default: 'true'
};

instant.subscription.updateCard(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Customer card updated successfully",
    "card_id": "card_58612d5143b2a",
    "card_last_4": "07/2037"
  }
]
```

This endpoint allows you to update only card details, like the expiration date or expiration year, you can do so without having to re-enter the full card details.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/subscription/{subscription_id}/update_card`



### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.
cardholder_name [required] | none | Actual cardholder name.
card_number [required] | none | The card number, as a string without any separators.
exp_month [required] | none | Two digit number representing the card's expiration month.
exp_year [required] | none | Two or four digit number representing the card's expiration year.
cvc [required] | none | Card security code
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.

## Update Amount

```shell
curl https://api.instantmerchant.io/api/v1/subscription/sub_58611e30ae9131
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d subscription_id='sub_58611e30ae9131' \
  -d amount=250
```
```javascript
//Request
var params = {
    subscription_id: 'sub_58611e30ae9131',
    amount: 250
};

instant.subscription(params).then(function(res){
    //success
},function(err){
    //error
}).catch(function(err){
    //error
})
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Subscription and amount updated successfully"
  }
]
```

This endpoint allows you to update only amount details.

### HTTP Request

`POST https://api.instantmerchant.io/api/v1/subscription/{subscription_id}`



### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.
amount [required] | none | amount to update for the subscription.

## Retrieve Subscription

```shell
curl https://api.instantmerchant.io/api/v1/subscription?id=sub_587616ba745ca1 \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Subscription retrieved successfully",
    "amount": "213.00",
    "active": 1,
    "expires_at": "14823000001",
    "subscription": {
        "id": "1",
        "customer_id": "334",
        "client_id": "1",
        "created_by": null,
        "invoice_id": "5",
        "payment_subscription_id": "sub_587616ba745ca1",
        "payment_customer_id": "str_cus_9mevFsW1UDT7NL",
        "admin_payment_account": "1",
        "merchant_account_id": null,
        "plan_interval": "monthly",
        "plan_name": "$213.00 every 1 month(s)",
        "plan_id": "$213.00 every 1 month(s)",
        "name": "test21-03",
        "email": "test21_03@test.com",
        "address": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "description": "test21-03 recurring paynow newcard save ",
        "amount": "213.00",
        "status": "Active",
        "date_canceled": null,
        "date_created": "2016-12-21 02:37:34",
        "date_expired": "14823000001",
        "renew_attempt": "0",
        "standalone": "1",
        "runmode": "1"
    }
  }
]
```

This endpoint allows you to retrieve your subscription.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/subscription?id={subscription_id}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## List all Subscriptions

```shell
curl https://api.instantmerchant.io/api/v1/subscription \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Subscription retrieved successfully",
    "subscription": [{
        "id": "12",
        "customer_id": "334",
        "client_id": "1",
        "created_by": null,
        "invoice_id": "5",
        "payment_subscription_id": "sub_585a3f4eb3b591",
        "payment_customer_id": "str_cus_9mevFsW1UDT7NL",
        "admin_payment_account": "1",
        "merchant_account_id": null,
        "plan_interval": "monthly",
        "plan_name": "$213.00 every 1 month(s)",
        "plan_id": "$213.00 every 1 month(s)",
        "name": "test21-03",
        "email": "test21_03@gmail.com",
        "address": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "description": "test21-03 recurring paynow newcard save ",
        "amount": "213.00",
        "status": "Active",
        "date_canceled": null,
        "date_created": "2016-12-21 02:37:34",
        "date_expired": "14823000001",
        "renew_attempt": "0",
        "standalone": "1",
        "runmode": "1"
    }, {
        "id": "13",
        "customer_id": "20",
        "client_id": "1",
        "created_by": null,
        "invoice_id": "40",
        "payment_subscription_id": "sub_585bff2e27a091",
        "payment_customer_id": "str_cus_9n9kovq1l4jYu3",
        "admin_payment_account": "1",
        "merchant_account_id": null,
        "plan_interval": "monthly",
        "plan_name": "$11.00 every 1 month(s)",
        "plan_id": "$11.00 every 1 month(s)",
        "name": "bps.somniumlabs",
        "email": "bps@somniumlabs.com",
        "address": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "description": "test draft",
        "amount": "11.00",
        "status": "Active",
        "date_canceled": null,
        "date_created": "2016-12-22 10:28:49",
        "date_expired": "1487743200",
        "renew_attempt": "0",
        "standalone": "1",
        "runmode": "1"
    }],
    "total": 2
  }
]
```

This endpoint allows you to retrieve your subscription.

### HTTP Request

`GET https://api.instantmerchant.io/api/v1/subscription`
### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.
