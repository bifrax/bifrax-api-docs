# Account Rest API for Bifrax (2019-03-10)
# General API Information
* The base endpoint is: **http://127.0.0.1**
* All endpoints return either a JSON object or array.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests;
  the issue is on the sender's side.
* HTTP `5XX` return codes are used for internal errors; the issue is on Bifrax's side.
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.
* Any endpoint can return an ERROR; the error payload is as follows:
```javascript
{
  "code": 500,
  "msg": "Invalid symbol."
}
```

* Specific error codes and messages defined in another document.
* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters may be sent as a
  `query string` or in the `request body` with content type
  `application/x-www-form-urlencoded` or `application/json`.

# Endpoint security type
* Each endpoint has a security type that determines the how you will
  interact with it.
* API-keys are passed into the Rest API via the `X-BIFRAX-APIKEY`
  header.
* API-keys and secret-keys **are case sensitive**.
* API-keys can be configured to only access certain types of secure endpoints.
* By default, API-keys can access all secure routes.

# SIGNED (Private) Endpoint security
* `SIGNED` endpoints require an additional parameter, `signature`, to be
  sent in the  `query string` or `request body`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation.
  Use your `secretKey` as the key and `totalParams` as the value for the HMAC operation.
* `totalParams` is must be sorted `alphabetically` for signatures.
* The `signature` is **not case sensitive**.
* `totalParams` is defined as the `query string` concatenated with the
  `request body` include `application/json`.
* reference for order-rest-api

## Account endpoints

### Infomation

* ExchangeType

`LOCAL`: local currency exchanges

`SPOT`: exchange between coins

`FUTURE`: future exchanges


### Balance information (USER_DATA)
```
GET /v1/balances (HMAC SHA256)
```
Get current balance information.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
exchangeType | STRING | NO | 
currency | STRING | NO | if not set, all coins

**Response:**
```
  "currency": "PHP",
  "totalAmount": "9999978134.79999924",
  "amount": "9999957126.5",
  "evaluationAmount": "10000000144.79999924",
  "balances": [
    {
      "symbol": "BTC/PHP",
      "quantity": "0.1",
      "avgPrice": "218302.8301886",
      "currentPrice": "220100",
      "porfitOrLoss": "179.71698114",
      "polRate": "0.82",
      "usingQuantity": "0",
      "marginQuantity": "0",
      "marginPrice": "0",
      "evaluationPrice": "22010"
    }
  ]
}
```
