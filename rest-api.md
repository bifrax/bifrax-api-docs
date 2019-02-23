# Public Rest API for Bifrax (2019-02-23)
# General API Information
* The base endpoint is: **http://127.0.0.1**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests;
  the issue is on the sender's side.
* HTTP `5XX` return codes are used for internal errors; the issue is on Bifrax's side.
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.

* Specific error codes and messages defined in another document.
* For `GET` endpoints, parameters must be sent as a `query string`.

Security Type | Description
------------ | ------------
NONE | Endpoint can be accessed freely.
TRADE | Endpoint requires sending a valid API-Key and signature.
USER_DATA | Endpoint requires sending a valid API-Key and signature.
USER_STREAM | Endpoint requires sending a valid API-Key.
MARKET_DATA | Endpoint requires sending a valid API-Key.


### Check server time
```
GET /api/v1/time
```
Test connectivity to the Rest API and get the current server time.

**Parameters:**
NONE

**Response:**
```javascript
{
  "timezone": "UTC",
  "serverTime": 1550926234362
}```

### Exchange information
```
GET /api/v1/exchangeInfo
```
Current exchange trading rules and symbol information

**Parameters:**
NONE

**Response:**
```javascript

{
  "timezone": "UTC",
  "serverTime": 1550925268628,
  "symbols": [
    {
      "symbol": "BTC/KRW",
      "baseName": "Bitcoin",
      "baseAsset": "BTC",
      "quoteAsset": "KRW",
      "minPrice": "1000",
      "minQty": ".003"
    }
}
```

## Market Data endpoints
### Order book
```
GET /api/v1/depth
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |


**Caution:** maxium list count is 20 

**Response:**
```javascript
{
  "time": 1550926537351,
  "bids": [
    [
      "3995000.00000000",
      "0.56770000"
    ],
    [
      "3996000.00000000",
      "0.80580000"
    ]
  ],
  "asks": [
    [
      "3994000.00000000",
      "1.35058000"
    ],
    [
      "3991000.00000000",
      "1.60000000"
    ]
  ]
}
```


### Kline/Candlestick data
```
GET /api/v1/klines
```
Kline/candlestick bars for a symbol.
Klines are uniquely identified by their open time.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
interval | STRING | YES | ['1m', '3m', '5m', '15m', '30m', '60m', '180m', '1d', '3d', '1w', '2w', '3w', '4w', '1M', '3M', '6M', '1y']
limit | INT | NO | Default 500; max 1000.
nextKey | STRING | NO | for paging

* If startTime and endTime are not sent, the most recent klines are returned.

**Response:**
```javascript
{
  "serverTime": 1550930645577,
  "nextKey": "20190220063000",
  "openPrice": "0.00000000",
  "highPrice": "0.00000000",
  "lowPrice": "0.00000000",
  "data": [
    {
      "time": 1550834460577,
      "openPrice": "3994000.00000000",
      "lowPrice": "3994000.00000000",
      "closedPrice": "3994000.00000000",
      "volume": "0.00300000",
      "sign": ""
    },
  ]
}
```



### Symbol price ticker
```
GET /api/v1/ticker/price
```
Latest price for a symbol or symbols.

**Weight:**
1 for a single symbol; **2** when the symbol parameter is omitted

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING ARRAY | NO |


**Response:**
```javascript

```
OR
```javascript
{
  "data": [
    {
      "symbol": "BTC/KRW",
      "price": "3994000.00000000"
    },
    {
      "symbol": "ETH/KRW",
      "price": "115500.00000000"
    },
    {
      "symbol": "EOS/KRW",
      "price": "2525.00000000"
    }
  ]
}
```

