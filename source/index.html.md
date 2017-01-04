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

InstantMerchant uses API key/secret pair to allow access to the API. You can get a new InstantMerchant API key by emailing [developer support](mailto:support@instantmerchant.io).

InstantMerchant expects the API key and secret to be included in all API requests to the server in a header that looks like the following:

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
  -d create_customer='true' \
  -d save_card='true' \
  -d is_default='true' \
  -d card_id='card_585a3da60deae'
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
    'create_customer' : 'true',
    'save_card' : 'true',
    'is_default' : 'true',
    'card_id' : 'card_585a3da60deae'
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
    "message": "invoice created successfully",
    "card_id": "card_585a3da60deae",
    "card_last_4": "4242",
    "subscription_id": "sub_585d0bf69f5b21",
    "expiry_date": 1485151200,
    "invoice_num": 29,
    "charge_id": "cha_585d0bf93573f1"
  }
]
```

This endpoint creates invoice and optionally charges it immediately.

### HTTP Request

`POST https://api.instantmerchant.io/api/v2/invoice`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already
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
create_customer [optional] | false | If set to `true`, Customer will be created.
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

## Charge Invoice

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
  -d is_default='true' \
  -d card_id='card_585d2a6c7c5d4'
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
    is_default : 'true',
    card_id : 'card_585d2a6c7c5d4'
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

## Capture Invoice

```shell
curl https://api.instantmerchant.io/api/v2/invoice/capture \
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

instant.invoice.capture(params).then(function(res){
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
  -X POST \
  -d charge_id='cha_585d0bf93573f1' \
  -d amount=10
```
```javascript
//Request
var params = {
    charge_id : 'cha_585d0bf93573f1',
    amount : 10
};

instant.invoice.refund(params).then(function(res){
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

`POST https://api.instantmerchant.io/api/v2/invoice/refund`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
charge_id [required] | none | The transaction id of the charge to refund
amount [optional] | entire charge | A positive integer representing how much of this charge to refund. Can only refund up to the un-refunded amount remaining of the charge.

# Direct Payment

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
  -d create_customer='true' \
  -d save_card='true' \
  -d is_default='true' \
  -d card_id='card_585a3da60deae'
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
    interval: 'quarterly',
    create_customer: 'true',
    save_card: 'true',
    is_default: 'true',
    card_id: 'card_585a3da60deae'
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
    "card_last_4": "1111",
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
name [required] | none | Customer name
email [required] | none | Customer email address
description [required] | none | Payment description
amount [required] | none | A positive integer representing how much to charge the card. The minimum amount is $1 USD
address [required] | none | Customer address
city [required] | none | City/Suburb/Town/Village
zip [required] | none | Zip code or postal code
state [required] | none | 2-letter state code
country [required] | none | 2-letter country code
payment_mode [required] | auth_and_capture | If set to `auth_and_capture`, given credit card will be charged immediately. if set to `auth_only` the charge issues an authorization (or pre-authorization), and will need to be captured later. Uncaptured charges expire in **7 days**.
cardholder_name [required] | none | Actual cardholder name. when `card_id` is not present.
card_number [required] | none | The card number, as a string without any separators. when `card_id` is not present.
exp_month [required] | none | Two digit number representing the card's expiration month. when `card_id` is not present.
exp_year [required] | none | Two or four digit number representing the card's expiration year. when `card_id` is not present.
cvc [required] | none | Card security code. when `card_id` is not present.
send_email [optional] | 0 | If set to 1, customer will receive payment email
currency [optional] | usd | Only allowed currency is usd
interval [optional] | false | Required, when payment_type is set to `recurring`.
create_customer [optional] | false | If set to `true`. Customer is created.
save_card [optional] | false | If set to `true`, card details will be stored.
is_default [optional] | false | If set to `true`. card details are saved and make it as default card.
card_id [optional] | none | Required, when card details are not present.

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
charge_id [required] | none | The transaction id of the charge to capture

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

# Customer

## Create Customer

```shell
curl https://api.instantmerchant.io/api/v2/customer \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET \
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

`GET https://api.instantmerchant.io/api/v2/customer/customer/?id=22`

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
    "card_last_4": "1111"
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
curl https://api.instantmerchant.io/api/v2/card \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET \
  -d customer=22 \
  -d card_id='card_585a3da60deae'
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

`GET https://api.instantmerchant.io/api/v2/card`

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
curl https://api.instantmerchant.io/api/v2/card \
  -H "X-Api-Key: meowmeowmeow" \
  -H "X-Api-Secret: meowmeowmeow" \
  -X GET \
  -d customer=22
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

`GET https://api.instantmerchant.io/api/v2/card`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer [required] | none | Customer id if created already.

# Subscription

## New Subscription

You can create subscription on <a href='#create-invoice'>Create Invoice</a> or <a href='#create-charge'>Direct Payment</a>.

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
curl https://api.instantmerchant.io/api/v2/subscription/cancel \
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

`POST https://api.instantmerchant.io/api/v2/subscription/cancel`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
subscription_id [required] | none | The identifier of the subscription.

## Update Card

```shell
curl https://api.instantmerchant.io/api/v2/subscription/update_card \
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

`POST https://api.instantmerchant.io/api/v2/subscription/update_card`



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
