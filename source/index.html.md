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
> "A sample test API key/secret is included in all the examples on this page, so you can test any example right away. To test requests using your account, replace the sample API key/secret with your actual API key/secret."

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
curl https://api.instantmerchant.io/api/v2/invoice \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d customer=1 \
  -d description='test description' \
  -d date_due='08/12/2017' \
  -d items[]='apple' \
  -d items_price[]=20 \
  -d items[]='orange' \
  -d items_price[]=18 \
  -d send_now=1 \
  -d payment_mode='auth_and_capture' \
  -d cardholder_name='Jim' \
  -d card_number=4242424242424242 \
  -d exp_month=12 \
  -d exp_year=2020 \
  -d cvc=123 \
  -d invoice_to_staff = 254 \
  -d send_invoice_to = 'jim@instantmerchant.io' \
  -d payment_type='recurring' \
  -d interval='quarterly' \
  -d save_card='true' \
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
    'customer' : 290,
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

`POST https://api.instantmerchant.io/api/v2/invoice`

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
currency [optional] | USD | Only allowed currency is `USD`.
invoice_to_staff [optional] | none | The identifier of the staff.
send_invoice_to [optional] | none | Email address to notify about the invoice
address [optional] | none | Required, when the customer is new.
city [optional] | none | Required, when the customer is new.
state [optional] | none | Required, when the customer is new.
zip [optional] | none | Required, when the customer is new.
country [optional] | US | Only allowed country is `US`.
payment_type [required] | one_time | If set to `recurring` , subscription will be added to the charge.
interval [optional] | false | Required, when payment_type is set to `recurring`.
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.
card_id [optional] | none | Required, when card details are not present.

## Send Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/send \
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

`POST https://api.instantmerchant.io/api/v2/invoice/send`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The identifier of the invoice

## Archive Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/10/archive \
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

`GET https://api.instantmerchant.io/api/v2/invoice/{invoice_num}/archive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The identifier of the invoice

## Unarchive Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/10/unarchive \
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

`GET https://api.instantmerchant.io/api/v2/invoice/{invoice_num}/unarchive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The identifier of the invoice

## Invoice Charge

```shell
curl https://api.instantmerchant.io/api/v2/invoice/charge \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d payment_mode='auth_and_capture' \
  -d invoice_num=30 \
  -d cardholder_name='Jim' \
  -d card_number=4242424242424242 \
  -d exp_month=11 \
  -d exp_year=2019 \
  -d cvc=123 \
  -d send_email=1 \
  -d save_card='true' \
  -d is_default='true'
```
```javascript
//Request
var params = {
    payment_mode : 'auth_and_capture',
    invoice_num: 30,
    cardholder_name : 'Jim',
    card_number : 4242424242424242,
    exp_month : 12,
    exp_year : 2020,
    cvc : 123,
    send_email : 1,
    save_card : 'true',
    is_default : 'true'
};

instant.invoice.charge(params).then(function(res){
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
    "card_id": "card_585d2a6c7c5d4",
    "card_last_4": "4242",
    "charge_id": "cha_585d2a6f0e02f1",
    "invoice_num": "31",
    "subscription_id": "sub_585d2a6c7c5711"
  }
]
```

This endpoint allow you to charge a credit or a debit card against an invoice, that has previously been created but not yet paid.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice/charge`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The id of the invoice to charge
send_email [optional] | 0 | If set to 1, customer will receive invoice email
payment_mode [optional] | auth_and_capture | If set to `auth_and_capture`, given credit card will be charged immediately. If set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [required] | none | Actual cardholder name. Required, when `card_id` is not present.
card_number [required] | none | The card number, as a string without any separators.Required, when `card_id` is not present.
exp_month [required] | none | Two digit number representing the card’s expiration month. Required, when `card_id` is not present.
exp_year [required] | none | Two or four digit number representing the card’s expiration year. Required, when `card_id` is not present.
cvc [required] | none | Card security code. Required, when `card_id` is not present.
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.
card_id [optional] | none | Required, when card details are not present.


## Retrieve an Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/invoice?invoice_num=11 \
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

`GET https://api.instantmerchant.io/api/v2/invoice/invoice?invoice_num={invoice_num}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
invoice_num [required] | none | The Identifier of the Invoice.

## List all Invoices

```shell
curl https://api.instantmerchant.io/api/v2/invoice/invoice \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Invoice found successfully",
    "total_rows": 33,
    "invoices_data": [{
        "id": "1",
        "client_id": "1",
        "customer_id": "20",
        "created_by": null,
        "customer_name": "customer1",
        "email": "customer1@test.com",
        "description": "charge1",
        "date_paid": "2016-12-21 02:15:35",
        "date_due": "2016-12-24",
        "archived": "0",
        "invoice_num": "1",
        "status": "Paid",
        "amount": "5.00",
        "date_created": "2016-12-21 02:15:33",
        "unique_id": "585a3a25d687c"
    }, {
        "id": "2",
        "client_id": "1",
        "customer_id": "20",
        "created_by": "0",
        "customer_name": "customer2",
        "email": "customer2@test.com",
        "description": "charge2",
        "date_paid": "2016-12-20 02:18:38",
        "date_due": "2016-12-30",
        "archived": "0",
        "invoice_num": "2",
        "status": "Paid",
        "amount": "1000.00",
        "date_created": "2016-12-20 02:18:37",
        "unique_id": "585a3add404af"
    }, {
        "id": "3",
        "client_id": "1",
        "customer_id": "333",
        "created_by": null,
        "customer_name": "customer3",
        "email": "customer3@testl.com",
        "description": "onetime paynow",
        "date_paid": "2016-12-21 02:30:31",
        "date_due": "2016-12-28",
        "archived": "0",
        "invoice_num": "3",
        "status": "Paid",
        "amount": "211.00",
        "date_created": "2016-12-21 02:30:30",
        "unique_id": "585a3da60ee2a"
    }, {
        "id": "4",
        "client_id": "1",
        "customer_id": "333",
        "created_by": null,
        "customer_name": "customer4",
        "email": "customer4@testl.com",
        "description": "charge4",
        "date_paid": "2016-12-21 02:33:32",
        "date_due": "2016-12-28",
        "archived": "0",
        "invoice_num": "4",
        "status": "Paid",
        "amount": "212.00",
        "date_created": "2016-12-21 02:33:31",
        "unique_id": "585a3e5b28cb3"
    }, {
        "id": "5",
        "client_id": "1",
        "customer_id": "334",
        "created_by": "0",
        "customer_name": "customer5",
        "email": "customer5@test.com",
        "description": "charge5 ",
        "date_paid": "2016-12-21 02:37:36",
        "date_due": "2016-12-28",
        "archived": "0",
        "invoice_num": "5",
        "status": "Refund",
        "amount": "213.00",
        "date_created": "2016-12-21 02:37:34",
        "unique_id": "585a3f4eb5743"
    }, {
        "id": "6",
        "client_id": "1",
        "customer_id": "333",
        "created_by": null,
        "customer_name": "customer6",
        "email": "customer6@test.com",
        "description": "charg6",
        "date_paid": "2016-12-21 02:43:52",
        "date_due": "2016-12-28",
        "archived": "0",
        "invoice_num": "6",
        "status": "Paid",
        "amount": "214.00",
        "date_created": "2016-12-21 02:41:17",
        "unique_id": "585a402d38634"
    }, {
        "id": "7",
        "client_id": "1",
        "customer_id": "334",
        "created_by": null,
        "customer_name": "customer7",
        "email": "customer7@test.com",
        "description": "charge7 ",
        "date_paid": "2016-12-21 02:44:55",
        "date_due": "2016-12-28",
        "archived": "0",
        "invoice_num": "7",
        "status": "Paid",
        "amount": "215.00",
        "date_created": "2016-12-21 02:42:17",
        "unique_id": "585a406948ad0"
    }, {
        "id": "8",
        "client_id": "1",
        "customer_id": "335",
        "created_by": "0",
        "customer_name": "customer8",
        "email": "customer8@test.com",
        "description": "charge8",
        "date_paid": "2016-12-21 02:51:38",
        "date_due": "2016-12-31",
        "archived": "0",
        "invoice_num": "8",
        "status": "Refund",
        "amount": "20.00",
        "date_created": "2016-12-21 02:51:37",
        "unique_id": "585a4299975e2"
    }, {
        "id": "9",
        "client_id": "1",
        "customer_id": "20",
        "created_by": "0",
        "customer_name": "customer9",
        "email": "customer9@test.com",
        "description": "ghghfg",
        "date_paid": "2016-12-21 03:31:43",
        "date_due": "2016-12-22",
        "archived": "0",
        "invoice_num": "9",
        "status": "Paid",
        "amount": "500.00",
        "date_created": "2016-12-21 03:31:41",
        "unique_id": "585a4bfde8c9d"
    }, {
        "id": "10",
        "client_id": "1",
        "customer_id": "20",
        "created_by": "0",
        "customer_name": "customer10",
        "email": "customer10@test.com",
        "description": "charge10",
        "date_paid": "2016-12-21 03:43:13",
        "date_due": "2016-12-22",
        "archived": "0",
        "invoice_num": "10",
        "status": "Void",
        "amount": "100.00",
        "date_created": "2016-12-21 03:43:12",
        "unique_id": "585a4eb024087"
    }]
  }
]
```

This endpoint allows you to list all the invoices with the limit of 10.

### HTTP Request

`GET https://api.instantmerchant.io/api/v2/invoice/invoice`


# Charges

## Create Charge

```shell
curl https://api.instantmerchant.io/api/v2/charge \
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
  -d send_email=1 \
  -d payment_type='recurring' \
  -d interval='quarterly' \
  -d customer=29
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
    "card_id": "card_58626a702e23c",
    "card_last_4": "4242",
    "subscription_id": "sub_58626a702eb9e1",
    "expiry_date": 1490590800,
    "customer_id": 363,
    "charge_id": "cha_689d0tf96536d1"
  }
]
```

Use this endpoint to charge a credit card.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/charge`

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
payment_mode [required] | auth_and_capture | If set to `auth_and_capture`, given credit card will be charged immediately. if set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [required] | none | Actual cardholder name. when `card_id` is not present.
card_number [required] | none | The card number, as a string without any separators. when `card_id` is not present.
exp_month [required] | none | Two digit number representing the card's expiration month. when `card_id` is not present.
exp_year [required] | none | Two or four digit number representing the card's expiration year. when `card_id` is not present.
cvc [required] | none | Card security code. when `card_id` is not present.
send_email [optional] | 0 | If set to 1, customer will receive payment email
currency [optional] | usd | Only allowed currency is usd
customer [optional] | new | Identifier of the customer.
interval [optional] | false | Required, when payment_type is set to `recurring`.
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.
card_id [optional] | none | Required, when card details are not present.


## Archive Charge

```shell
curl https://api.instantmerchant.io/api/v2/customer/7/archive \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    payment_id: '7'
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
    "message":"archive have been updated successfully..!"
  }
]
```

This endpoint allows you to archive the payment transaction.

### HTTP Request

`GET https://api.instantmerchant.io/api/v2/customer/{payment_id}/archive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
payment_id [required] | none | The Identifier of the payment.


## Unarchive Charge

```shell
curl https://api.instantmerchant.io/api/v2/customer/7/unarchive \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```
```javascript
//Request
var params = {
    payment_id: '7'
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

`GET https://api.instantmerchant.io/api/v2/customer/{payment_id}/unarchive`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
payment_id [required] | none | The Identifier of the payment.


## Capture Charge

```shell
curl https://api.instantmerchant.io/api/v2/capture \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d charge_id='cha_585d0bf93573f1'
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
    "charge_id":"cha_585d0bf93573f1"
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
charge_id [required] | none | The transaction id of the charge to capture.

** If it is an Invoice capture, `invoice_num` will be returned along with JSON output.

## Refund

```shell
curl https://api.instantmerchant.io/api/v2/refund \
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
    "message":"Refund has been initiated successfully",
    "refund_id":"ref_397d0me53575f3"
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
charge_id [required] | none | The transaction id of the charge to refund
amount [optional] | entire charge | A positive integer representing how much of this charge to refund. Can only refund up to the unrefunded amount remaining of the charge.


** If it is an Invoice refund, `invoice_num` will be returned along with JSON output.


## List all charges

```shell
curl https://api.instantmerchant.io/api/v2/customer/transactions \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "transaction data found successfully",
    "total_rows": 323,
    "transactions": [{
        "invoice_id": "1",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "20",
        "name": "customer1",
        "email": "customer1@test.com",
        "amount": "5.00",
        "balance": "5.00",
        "fee_charged": "0.15",
        "stripe_fee": "0.45",
        "description": "charge1",
        "status": "Paid",
        "client_id": "1",
        "city": "city1",
        "state": "Nashville",
        "zip": "37221",
        "charge_id": "ch_19T5G6CneoHSBp0PxZIkqIru",
        "archived": "0",
        "date_created": "2016-12-21 02:15:35",
        "date_expired": null,
        "id": "1",
        "subscription_id": "0"
    }, {
        "invoice_id": "2",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "20",
        "name": "customer2",
        "email": "customer2@test.com",
        "amount": "1000.00",
        "balance": "895.00",
        "fee_charged": "30.00",
        "stripe_fee": "29.30",
        "description": "charge2",
        "status": "Paid",
        "client_id": "1",
        "city": "city1",
        "state": "Nashville",
        "zip": "37221",
        "charge_id": "ch_19T5J4CneoHSBp0PYDiKx65M",
        "archived": "0",
        "date_created": "2016-12-20 02:18:38",
        "date_expired": null,
        "id": "2",
        "subscription_id": "0"
    }, {
        "invoice_id": "3",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "333",
        "name": "customer3",
        "email": "customer3@test.com",
        "amount": "211.00",
        "balance": "211.00",
        "fee_charged": "6.33",
        "stripe_fee": "6.42",
        "description": "charge3",
        "status": "Paid",
        "client_id": "1",
        "city": "test-city01",
        "state": "TN",
        "zip": "125436",
        "charge_id": "ch_19T5UZCneoHSBp0Pt2OzyCwz",
        "archived": "0",
        "date_created": "2016-12-21 02:30:31",
        "date_expired": null,
        "id": "3",
        "subscription_id": "0"
    }, {
        "invoice_id": "4",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "333",
        "name": "customer4",
        "email": "customer4@test.com",
        "amount": "212.00",
        "balance": "212.00",
        "fee_charged": "6.36",
        "stripe_fee": "6.45",
        "description": "test charge",
        "status": "Paid",
        "client_id": "1",
        "city": "test-city01",
        "state": "TN",
        "zip": "125436",
        "charge_id": "ch_19T5XUCneoHSBp0PjgaWK4pY",
        "archived": "0",
        "date_created": "2016-12-21 02:39:42",
        "date_expired": null,
        "id": "6",
        "subscription_id": null
    }, {
        "invoice_id": "6",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "333",
        "name": "customer22",
        "email": "customer22@test.com",
        "amount": "214.00",
        "balance": "214.00",
        "fee_charged": "6.42",
        "stripe_fee": "6.51",
        "description": "test payment",
        "status": "Paid",
        "client_id": "1",
        "city": "test-city01",
        "state": "TN",
        "zip": "125436",
        "charge_id": "ch_19T5hTCneoHSBp0P1TCrxiX2",
        "archived": "0",
        "date_created": "2016-12-21 02:43:52",
        "date_expired": null,
        "id": "7",
        "subscription_id": "0"
    }, {
        "invoice_id": "7",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "334",
        "name": "customer31",
        "email": "customer31@test.com",
        "amount": "215.00",
        "balance": "215.00",
        "fee_charged": "6.45",
        "stripe_fee": "6.54",
        "description": "charge12 ",
        "status": "Paid",
        "client_id": "1",
        "city": "CBE",
        "state": "CA",
        "zip": "9845122",
        "charge_id": "ch_19T5iVCneoHSBp0PussT5gmf",
        "archived": "0",
        "date_created": "2016-12-21 02:44:55",
        "date_expired": null,
        "id": "8",
        "subscription_id": "2"
    }, {
        "invoice_id": "9",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "20",
        "name": "customer23",
        "email": "customer23@test.com",
        "amount": "500.00",
        "balance": "90.00",
        "fee_charged": "15.00",
        "stripe_fee": "14.80",
        "description": "charge32",
        "status": "Paid",
        "client_id": "1",
        "city": "city1",
        "state": "Nashville",
        "zip": "37221",
        "charge_id": "ch_19T6RmCneoHSBp0PHoi79Svu",
        "archived": "0",
        "date_created": "2016-12-21 03:31:43",
        "date_expired": null,
        "id": "10",
        "subscription_id": "0"
    }, {
        "invoice_id": "9",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "20",
        "name": "customer36",
        "email": "customer36@test.com",
        "amount": "400.00",
        "balance": null,
        "fee_charged": null,
        "stripe_fee": null,
        "description": "charge 7",
        "status": "Refund",
        "client_id": "1",
        "city": "city1",
        "state": "Nashville",
        "zip": "37221",
        "charge_id": "ch_19T6RmCneoHSBp0PHoi79Svu",
        "archived": "0",
        "date_created": "2016-12-21 03:32:06",
        "date_expired": null,
        "id": "11",
        "subscription_id": "0"
    }, {
        "invoice_id": "10",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "20",
        "name": "customer11",
        "email": "customer11@test.com",
        "amount": "100.00",
        "balance": null,
        "fee_charged": null,
        "stripe_fee": null,
        "description": "charge45",
        "status": "Void",
        "client_id": "1",
        "city": "city1",
        "state": "Nashville",
        "zip": "37221",
        "charge_id": "ch_19T6cvCneoHSBp0PVEkY4Pra",
        "archived": "0",
        "date_created": "2016-12-21 03:43:39",
        "date_expired": null,
        "id": "13",
        "subscription_id": "0"
    }, {
        "invoice_id": "11",
        "created_by": "1",
        "uncaptured": "0",
        "customer_id": "20",
        "name": "customer23",
        "email": "customer23@test.com",
        "amount": "55.00",
        "balance": null,
        "fee_charged": null,
        "stripe_fee": null,
        "description": "charge109",
        "status": "Refund",
        "client_id": "1",
        "city": "city1",
        "state": "Nashville",
        "zip": "37221",
        "charge_id": "nm_3413022421",
        "archived": "0",
        "date_created": "2016-12-21 04:30:44",
        "date_expired": null,
        "id": "15",
        "subscription_id": "0"
    }]
  }
]
```
This endpoint allow you to list all the transactions with the limit of 10.

### HTTP Request

`GET https://api.instantmerchant.io/api/v2/customer/transactions`

# Customer

## Create Customer

```shell
curl https://api.instantmerchant.io/api/v2/customer \
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
  -d country='US'
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
    country: 'US'
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
    "message":"Customer created successfully",
    "customer_id":23
  }
]
```

This endpoint allows you to create your customers.

### HTTP Request

`GET https://api.instantmerchant.io/api/v2/customer`

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

## Retrieve Customer

```shell
curl https://api.instantmerchant.io/api/v2/customer/customer/?id=22 \
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

`GET https://api.instantmerchant.io/api/v2/customer/customer/?id={customer_id}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id [required] | none | The identifier of the customer to be retrieved.

## Update Customer

```shell
curl https://api.instantmerchant.io/api/v2/customer/update \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d id =20 \
  -d name='Jim' \
  -d password='bacabcdefgh' \
  -d address='new address here'
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
Updates the specified customer by setting the values of the parameters passed. Any parameters not provided will be left unchanged.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/customer/update`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id [required] | none | The identifier of the customer to be update.
name [optional] | none | New name of the customer.
password [optional] | none | New password of the customer.
address [optional] | none | New address of the customer.
city [optional] | none | New City/Suburb/Town/Village.
zip [optional] | none | New Zip code or postal code.
active [optional] | 1 | Active status of the customer.

## Delete Customer

```shell
curl https://api.instantmerchant.io/api/v2/customer \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X DELETE \
  -d customer_id=22
```

> The above command returns JSON structured like this:

```json
[
  {
    "status": true,
    "message": "Customer data is deleted successfully..!"
  }
]
```

This endpoint allows you to delete a customer.

### HTTP Request

`DELETE https://api.instantmerchant.io/api/v2/customer`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id [required] | none | The identifier of the Customer.

## List all customers

```shell
curl https://api.instantmerchant.io/api/v2/customer/customer \
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
        "id": "10",
        "client_id": "20",
        "username": "user10",
        "name": "user10",
        "email": "user10@test.com",
        "active": "1"
    }, {
        "id": "09",
        "client_id": "20",
        "username": "user09",
        "name": "user09",
        "email": "user09@test.com",
        "active": "1"
    }, {
        "id": "08",
        "client_id": "20",
        "username": "user08",
        "name": "user08",
        "email": "user08@test.com",
        "active": "1"
    }, {
        "id": "07",
        "client_id": "20",
        "username": "user07",
        "name": "user07",
        "email": "user07@test.com",
        "active": "1"
    }, {
        "id": "06",
        "client_id": "20",
        "username": "user06",
        "name": "user072016-02",
        "email": "raja2612@test.com",
        "active": "1"
    }, {
        "id": "05",
        "client_id": "20",
        "username": "user05",
        "name": "user05",
        "email": "user05@test.com",
        "active": "1"
    }, {
        "id": "04",
        "client_id": "20",
        "username": "user04",
        "name": "user04",
        "email": "user04@test.com",
        "active": "1"
    }, {
        "id": "03",
        "client_id": "20",
        "username": "user03",
        "name": "user03",
        "email": "user03@test.com",
        "active": "1"
    }, {
        "id": "02",
        "client_id": "20",
        "username": "user02",
        "name": "user02",
        "email": "user02@test.com",
        "active": "1"
    }, {
        "id": "01",
        "client_id": "20",
        "username": "user01",
        "name": "user01",
        "email": "user01@test.com",
        "active": "1"
    }]
}

```
Returns a list of your customers with a limit of 10. The customers are returned sorted by `id`, with the most recent customers appearing first.

### HTTP Request

`GET https://api.instantmerchant.io/api/v2/customer/customer`


# Card

## Create Card

```shell
curl https://api.instantmerchant.io/api/v2/card \
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

`POST https://api.instantmerchant.io/api/v2/card`

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
curl https://api.instantmerchant.io/api/v2/card/?customer=22&card_id=card_585a3da60deae \
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
`GET https://api.instantmerchant.io/api/v2/card/?customer={customer_id}&card_id={card_id}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already.
card_id [required] | none | existing stored card id.

## Delete Card

```shell
curl https://api.instantmerchant.io/api/v2/card \
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

`DELETE https://api.instantmerchant.io/api/v2/card`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already
card_id [optional] | none | Required, when card details are not present.


## List all Cards

```shell
curl https://api.instantmerchant.io/api/v2/card/?customer=22 \
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
`GET https://api.instantmerchant.io/api/v2/card/?customer={customer_id}`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already.

# Subscription

## New Subscription

You can create subscription on <a href='#create-invoice'>Create Invoice</a> or <a href='#create-charge'> Charges </a>.

## Renew Subscription

```shell
curl https://api.instantmerchant.io/api/v2/subscription/renew \
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

`POST https://api.instantmerchant.io/api/v2/subscription/renew`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## Cancel Subscription

```shell
curl https://api.instantmerchant.io/api/v2/subscription/sub_587616ba745ca1/cancel \
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

`GET https://api.instantmerchant.io/api/v2/subscription/{subscription_id}/cancel`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## Suspend Subscription

```shell
curl https://api.instantmerchant.io/api/v2/subscription/sub_587616ba745ca1/suspend \
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

`GET https://api.instantmerchant.io/api/v2/subscription/{subscription_id}/suspend`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## Resume Subscription

```shell
curl https://api.instantmerchant.io/api/v2/subscription/sub_587616ba745ca1/resume \
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

`GET https://api.instantmerchant.io/api/v2/subscription/{subscription_id}/resume`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.


## Update Card

```shell
curl https://api.instantmerchant.io/api/v2/subscription/sub_58611e30ae9131/update_card \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X POST \
  -d subscription_id='sub_58611e30ae9131' \
  -d cardholder_name='JimTest' \
  -d card_number=4242424242424242 \
  -d exp_month=11 \
  -d exp_year=2019 \
  -d cvc=999 \
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

`POST https://api.instantmerchant.io/api/v2/subscription/{subscription_id}/update_card`



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
