# Server API for Live System
**V.0.0.1**

By Michael Yang

Oct. 11, 2019

Revisions
-----------

# 1. Overview

This API is designed to provide the access for other services to manager , create, and query products in basket. It is a RESTful API based on standard HTTP request/response with UTF-8 encoded JSON data for communication.

##1.1 Design Convention

**Request Format**

The request is based on standard HTTP request with standard HTTP 
method like GET or POST. The format of request is as following:

| https://&lt;host>/api/&lt;resourcePath>|
|-----------------------------------------------------------------------|


**Definition**

| Arguments    | Description                                                                                                                                                                                                |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Host         | This is a host domain name of this api. (Ex: example.com)                                                                                                                                                  |
| ResourcePath | This is a path combined with a chain of individual data entities including its target ID(Ex: /organizations/&lt;organization-id>/licenses/&lt;license-id>) to handle some particular action of the request |

**Response Format**

The response format for all requests is a JSON object.

The API will response the request with JSON object with standard HTTP status code to represent the result of the request.

If there is no any item in a response, it is not an error. A empty JSON object/array should be returned.

Empty response example
```javascript
{}
```

**Request Example**

```curl
curl –u “https://xxx.xxx.xxx.xxx/api/products/1/” 
```


**Response Example**

 **HTTP/1.1 200**
```javascript
{
  "id": 1,
  "name": "Samsung Galaxy S10+ 6.4吋智慧型手機 8G/128G",
  "price": 100,
  "image": "https://ehs-shop.tyson711.now.sh/static/images/470x600-02.jpg",
  "url": "",
  "description": "This is description",
  "qty": 10
}
```
##1.2 Rules for token, ID and name

### access_token
* A User Identifier to checkout products in cart

### product ID
* Uppercase and lowercase Latin letters (a–z, A–Z) (ASCII: 65–90, 97–122)
* Digits 0 to 9 (ASCII: 48–57)
* Special characters: +-.@_ (ASCII: 43, 45,46, 64,95)
* The maximum length is 64.

### Order ID
* Uppercase and lowercase Latin letters (a–z, A–Z) (ASCII: 65–90, 97–122)
* Digits 0 to 9 (ASCII: 48–57)
* Special characters: +-.@_ (ASCII: 43, 45,46, 64,95)
* The maximum length is 64.


## 1.3 Request
### Method Summary

| URL                        | HTTP Method | Description                               |
| -------------------------- | ----------- | ----------------------------------------- |
| /products                  | GET         | Get all products of this set of APIs.     |
| /products/&lt;products-id> | GET         | Get specific product of this set of APIs. |
| /cart                      | GET         | Get customer shopping cart information    |
| /cart                      | POST        | Add products to shopping cart             |
| /cartProduct               | DELETE      | Delete product from shopping cart         |
| /order                     | POST        | Create a order                            |
# 2. Methods Details

This section describes the methods provided by this API. 

## 2.1. General
 
### 2.1.1. Get all products of this set of APIs.

#### Request Method

| GET | /products |
| --- | --------- |



#### Request Example

```curl
curl –u "https://xxx.xxx.xxx/api/products" \
-X GET \
```

#### Response Data

| Response Code        | Description                    |
| -------------------- | ------------------------------ |
| Normal Response code | 200                            |
| Errors Response code | 404-01, 405-01, 420-01, 420-02 |

| Parameter | Type    | Description                                                            |
| --------- | ------- | ---------------------------------------------------------------------- |
| category  | String  | the category name of products.                                         |
| idCatgory | Integer | the category id of products.                                           |
| products  | JSON    | the product details including id, name, price, image, url, description |



#### Response Example
Here return version number for both API and Software.

**HTTP/1.1 200**
```javascript
{
  [
    {
      "category": "時尚",
      "idCatgory": 11,
      "products": [
        {
            "idProduct": 1,
            "name": "Samsung Galaxy S10+ 6.4吋智慧型手機 8G/128G",
            "price": 100,
            "image": "https://ehs-shop.tyson711.now.sh/static/images/470x600-02.jpg",
            "url": "",
            "description": "This is description",
            "qty": 10
        },
        {
            "idProduct": 2,
            "name": "Samsung Galaxy S10+ 6.4吋智慧型手機 16G/128G",
            "price": 150,
            "image": "https://ehs-shop.tyson711.now.sh/static/images/470x600-02.jpg",
            "url": "",
            "description": "This is description",
            "qty": 1
          
        }
      ]
    }
  ]
}

```

**HTTP/1.1 420**
```javascript
{
    "errors": {
        "code": "420-01",
        "message": "Method Failure. The method 'xxx' has some program errors"
    }
}
```

### 2.1.2. Get specific product by product id.

#### Request Method

| GET | /products/&lt;products-id> |
| --- | -------------------------- |



#### Request Example

```curl
curl –u "https://xxx.xxx.xxx/api/products/1" \
-X GET \
```

#### Response Data

| Response Code        | Description                    |
| -------------------- | ------------------------------ |
| Normal Response code | 200                            |
| Errors Response code | 404-01, 405-01, 420-01, 420-02 |

| Parameter   | Type    | Description                    |
| ----------- | ------- | ------------------------------ |
| category    | String  | the category name of products. |
| idCatgory   | Integer | the category id of products.   |
| id          | Integer |                                |
| name        | String  |                                |
| price       | Float   |                                |
| image       | String  | Product's image url            |
| url         | String  |                                |
| description | String  |                                |
| qty         | Integer |                                |


#### Response Example
Here return version number for both API and Software.

**HTTP/1.1 200**
```javascript
{
  [
    {
        "idProduct": 1,
        "name": "Samsung Galaxy S10+ 6.4吋智慧型手機 8G/128G",
        "price": 100,
        "image": "https://ehs-shop.tyson711.now.sh/static/images/470x600-02.jpg",
        "url": "",
        "description": "This is description",
        "qty": 10
        "category": "時尚",
        "idCatgory": 11,
    }
  ]
}

```

### 2.1.2. Get shopping cart information by cart id.

#### Request Method

| GET | /cart/&lt;cart-id> |
| --- | ------------------ |



#### Request Example

```curl
curl –u "https://xxx.xxx.xxx/api/cart/1" \
-X GET \
```

#### Response Data

| Response Code        | Description                    |
| -------------------- | ------------------------------ |
| Normal Response code | 200                            |
| Errors Response code | 404-01, 405-01, 420-01, 420-02 |

| Parameter  | Type    | Description                                                            |
| ---------- | ------- | ---------------------------------------------------------------------- |
| id         | String  | the category name of products.                                         |
| nb_produts | Integer | the category id of products.                                           |
| products   | JSON    | the product details including id, name, price, image, url, description |



#### Response Example
Here return version number for both API and Software.

**HTTP/1.1 200**
```javascript
{
  [
    {
        "idCart": 1,
        "nb_produts": 2,
        "carts": [
        {
            "idCartProduct":"1",
            "product": [
                "idProduct": 1,
                "name": "Samsung Galaxy S10+ 6.4吋智慧型手機 8G/128G",
                "price": 100,
                "image": "https://ehs-shop.tyson711.now.sh/static/images/470x600-02.jpg",
                "url": "",
                "description": "This is description",
                "qty": 10
            ]
        },
        {
            "idCartProduct":2,
            "product": [
                "idProduct": 2,
                "name": "Samsung Galaxy S10+ 6.4吋智慧型手機 16G/128G",
                "price": 150,
                "image": "https://ehs-shop.tyson711.now.sh/static/images/470x600-02.jpg",
                "url": "",
                "description": "This is description",
                "qty": 1
            ]
        }
      ]
    }
  ]
}

```

**HTTP/1.1 420**
```javascript
{
    "errors": {
        "code": "420-01",
        "message": "Method Failure. The method 'xxx' has some program errors"
    }
}
```

### 2.1.3. Remove product from shopping cart

#### Request Method

| DELETE | /cartProduct/&lt;cartPrdocut-id> |
| ------ | -------------------------------- |



#### Request Example

```curl
curl –u "https://xxx.xxx.xxx/api/cartProduct1" \
-X DELETE \
```
#### Response Data

| Response Code        | Description                    |
| -------------------- | ------------------------------ |
| Normal Response code | 200                            |
| Errors Response code | 404-01, 405-01, 420-01, 420-02 |

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| id        | String | Table cartProduct's id |




#### Response Example
Here return version number for both API and Software.

**HTTP/1.1 200**
```javascript
{
  [
    {
        "idCartProduct": 1,
    }
  ]
}

```

**HTTP/1.1 420**
```javascript
{
    "errors": {
        "code": "420-01",
        "message": "Method Failure. The method 'xxx' has some program errors"
    }
}
```

### 2.1.4. Add product to shopping cart

#### Request Method

| POST | /cart/ |
| ---- | ------ |



#### Request Example

```curl
curl –u "https://xxx.xxx.xxx/api/cart" \
-X POST \
```
#### Request body
| Parameter | Type    | Description        |
| --------- | ------- | ------------------ |
| idProduct | Integer | product's id       |
| idCart    | Integer | shopping cart's id |

#### Response Data

| Response Code        | Description                    |
| -------------------- | ------------------------------ |
| Normal Response code | 200                            |
| Errors Response code | 404-01, 405-01, 420-01, 420-02 |

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| id        | String | Table cartProduct's id |




#### Response Example
Here return version number for both API and Software.

**HTTP/1.1 200**
```javascript
{
  [
    {
        "idCartProduct": 1,
    }
  ]
}

```

**HTTP/1.1 420**
```javascript
{
    "errors": {
        "code": "420-01",
        "message": "Method Failure. The method 'xxx' has some program errors"
    }
}
```


### 2.1.5. Create order

#### Request Method

| POST | /order/ |
| ---- | ------- |



#### Request Example

```curl
curl –u "https://xxx.xxx.xxx/api/order" \
-X POST \
```
#### Request body
| Parameter  | Type    | Description        |
| ---------- | ------- | ------------------ |
| idCustomer | Integer | product's id       |
| idCity     | Integer | city's id          |
| idZone     | Integer | zone'sid           |
| idCart     | Integer | shopping cart's id |
| address    | String  | shipping address   |

#### Response Data

| Response Code        | Description                    |
| -------------------- | ------------------------------ |
| Normal Response code | 200                            |
| Errors Response code | 404-01, 405-01, 420-01, 420-02 |

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| id        | String | Table cartProduct's id |




#### Response Example
Here return version number for both API and Software.

**HTTP/1.1 200**
```javascript
{
  [
    {
        "idOrder": 1,
    }
  ]
}

```

**HTTP/1.1 420**
```javascript
{
    "errors": {
        "code": "420-01",
        "message": "Method Failure. The method 'xxx' has some program errors"
    }
}
```

# 4. Errors

**HTTP Response Code**

| Code | Description                                                    |
| ---- | -------------------------------------------------------------- |
| 404  | Not Found This represents the request target does not existed. |


