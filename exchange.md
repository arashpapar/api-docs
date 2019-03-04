# Exchange

**Endpoint URL**

**`https://api.wcex.com/exchange`**

## Authenticated Endpoints

### Place a New Order

**`https://api.wcex.com/exchange/order/new`**

#### Request (POST)

| Body Parameter | Type | Description |
| :--- | :--- | :--- |
| product | `string` | A valid product. |
| side | `string` | Can be `buy` or `sell`. |
| type | `string` | Must be `limit`. |
| size | `string` | Amount to buy or sell. |
| price | `string` | Price of your order. |

Currently, only limit orders are supported. To have your order execute immediately (as a market order), simply set a price deep in the opposite book.

#### Order Lifecycle

Your account must have sufficient balance to successfully submit an order.

If any part of the order results in adding liquidity to the book, it will be marked as open.

An order that is completely filled and off the book will be marked as done.

#### Response

An order accepted by the matching engine will be assigned an order `id` which will appear in the response.

`filled` specifies the amount that was matched as a result of your order.

`timestamp` is the time your order was received by the matching engine.

```javascript
{
    "id": "9d335cce-d581-45f6-8efc-bdab3d61c6e2",
    "product": "XT-BTC",
    "type": "limit",
    "side": "buy",
    "size": "10.00000000",
    "price": "0.04",
    "filled": "2.23000000",
    "timestamp": 1511467005839
}
```

### Cancel Order

**`https://api.wcex.com/exchange/order/cancel`**

#### Request (POST)

| Body Parameter | Type | Description |
| :--- | :--- | :--- |
| id | `string` | The ID of the order to cancel. |
| product (optional) | `string` | The order's product. |

Providing `product` is optional, but it's recommended because it greatly speeds up the cancel operation by skipping the id-to-product lookup.

#### Response

If successful, the response will contain the ID of the cancelled order.

If an order could not be cancelled \(if it has been already filled, does not exist, etc.\), an error is returned.

```javascript
{
    "id": "9d335cce-d581-45f6-8efc-bdab3d61c6e2"
}
```

### Get Orders

**`https://api.wcex.com/exchange/orders`**

Lists your open orders, sorted by submission time \(most recent first\).

#### Request

| Query Parameter | Type | Description |
| :--- | :--- | :--- |
| limit (optional) | `number` | Number of orders to return (max is 100). |

You can optionally pass a `limit` query parameter to limit the number of orders returned. By default, 100 items are returned.


E.g. `/orders?limit=50.`

#### Keeping track of your orders

We recommend listening to streaming WebSocket order events to maintain an up-to-date view of your open orders. It's faster and in many cases more accurate than polling this endpoint, because an order's state may change between the time you make an HTTP request and receive a response.

Polling this endpoint is highly discouraged.

#### Response

An array of your open orders.

```javascript
[
    {
        "id": "b20265e7-8d28-476a-8758-c6113ad98c0b",
        "product": "XT-BTC",
        "size": "2.34977543",
        "filled": "0.00000000",
        "price": "0.053435",
        "side": "sell",
        "type": "limit",
        "timestamp": 1511252206104
    },
    {
        "id": "b361e636-46cd-4a34-a532-d454c9e7b4b6",
        "product": "XT-BTC",
        "size": "4.52120565",
        "filled": "0.00000000",
        "price": "0.053147",
        "side": "buy",
        "type": "limit",
        "timestamp": 1511252184729
    }
]
```

### Get Fills

**`https://api.wcex.com/exchange/fills/:product?`**

List your recent fills, ordered by fill time (most recent first).

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| product (optional) | `string` | Limit fills returned to this product. |

| Query Parameter | Type | Description |
| :--- | :--- | :--- |
| timestamp (optional) | `number` | Return fills that occurred before this time. |
| limit (optional) | `number` | Number of fills to return (max is 100). |

Omitting `product` will return fills across all products.

E.g. `/fills?limit=50&timestamp=1511481127561`

#### Keeping track of your fills

We recommend listening to streaming WebSocket match events to maintain an up-to-date view of your fills. It's faster and more accurate than polling this endpoint.

Polling this endpoint is highly discouraged.

#### Response

An array of your recent fills.

```javascript
[
    { 
        "order_id_taker": "bf2b704c-010a-48ca-93fb-d0193f24420a",
        "order_id_maker": "31bf3a89-2a0a-40b0-9327-ec0a5cc3ce3c",
        "product": "XT-BTC",
        "size": "0.77942322",
        "price": "0.05132",
        "side": "buy",
        "timestamp": 1511482279491
    },
    {
        "order_id_taker": "c6023aca-333a-44d9-98fe-279e279f3a86",
        "order_id_maker": "7f66a16c-0b47-4bd6-8add-6b2c33366082",
        "product": "XT-BTC",
        "size": "0.97422208",
        "price": "0.051323",
        "side": "buy",
        "timestamp": 1511482127568
    }
]
```

### Get Balances

**`https://api.wcex.com/exchange/balances/:asset?`**

Lists your balances.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| asset (optional) | `string` | Limits balances returned to this asset. |

Omitting `asset` will return all balances on your account. Only non-zero balances, or assets for which you have made at least one deposit will be returned.

#### Response

```javascript
{
    "BTC": "11.88900211",
    "XT": "2218.99900000"
}
```

### Get Transactions

**`https://api.wcex.com/exchange/transactions/:type`**

Lists your deposits and withdrawals, sorted by most recent first.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| type (optional) | `string` | Can be `deposit` or `withdraw`. |

| Query Parameter | Type | Description |
| :--- | :--- | :--- |
| asset (optional) | `string` | Limits transactions returned to this asset. |
| limit (optional) | `string` | Number of transactions to return (max 100). |
| timestamp (optional) | `string` | Return transactions that occurred before this time. |

Omitting `type` will return all transactions on your account.


E.g. `/transactions?asset=XT&limit=50&timestamp=1511481127561`

#### Response

```javascript
[
    {
        "asset": "BTC",
        "amount": "0.2000000",
        "type": "deposit",
        "address": "2MzeJNyp47Cugv85aq361AwBmaC4kxHofuu",
        "txid": "8a0a2cd49b3bbcabc5a4066783e3aad47ea745a849450a865977b668ce514ec2",
        "timestamp": 1511482279491,
        "confirmed_at": 1511482379491
    },
    {
        "asset": "XT",
        "amount": "200.00990000",
        "type": "withdraw",
        "address": "0xa93fae849449d2f9ddca15b861fcb329d39ad47c",
        "txid": "0xaaec962c014ab72eec8d4abb58326dca1c5c8a9425cd851ef8b9c390f2ad050e",
        "timestamp": 1511482279491,
        "confirmed_at": 1511482379491
    }
]
```

### Get Deposit Address

**`https://api.wcex.com/exchange/deposit/:asset`**

Returns a deposit address for the provided asset.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| asset | `string` | A valid asset. |

#### Response

```javascript
{
    "address": "2MzeJNyp47Cugv85aq361AwBmaC4kxHofuu",
    "asset": "BTC",
    "confirmations_needed": 2
}
```


## Public Endpoints


### Get Products

**`https://api.wcex.com/exchange/products/:product?`**

Lists products available to trade.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| product (optional) | `string` | Limits results returned to this product. |

#### Response

```javascript
[
    [
        "XT-BTC", // product
        "0.001", // base tick
        "0.00001", // quote tick
        "0.01" // minimum size
    ]
]
```

### Get Quotes

**`https://api.wcex.com/exchange/quotes/:product(s)?`**

Lists products available to trade.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| product(s) | `string` | One or more comma-separated products. |

E.g. `/quotes/XT-BTC,XT-ETH`

Polling this endpoint is highly discouraged in favor of subscribing to WebSocket events.

#### Response

```javascript
{
        "XT-BTC": [
                "0.0400", // last price
                "buy", // last tick
                "1223322.9983", // 24h volume
                "0.0344", // price 24 hours ago
                1534104719613 // timestamp
        ]
}
```

### Get Book

**`https://api.wcex.com/exchange/book/:product`**

Returns list of open bids and asks for a product.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| product | `string` | A valid product. |

| Query Parameter | Type | Description |
| :--- | :--- | :--- |
| limit (optional) | `string` | Number of levels to return (max 100). |

E.g. `/book/XT-BTC?limit=50`.

Polling this endpoint is discouraged in favor of subscribing to WebSocket events.

#### Response

```javascript
{
    "bids":[
        ["0.04", "5.00000000", 1], // price, size, order count
        ["0.039465", "4.24022501", 1]
    ],
    "asks":[
        ["0.05132", "9.22057678", 1],
        ["0.051323", "3.52460275", 1]
    ]
}
```


### Get Trade History

**`https://api.wcex.com/exchange/trades/:product`**

Lists the latest trades for the provided product.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| product | `string` | A valid product. |

| Query Parameter | Type | Description |
| :--- | :--- | :--- |
| timestamp (optional) | `number` | 
Return trades that occurred before this time. |
| limit (optional) | `string` | Number of trades to return (max 100). |

E.g. `/trades/XT-BTC?limit=50&timestamp=1511481127561`.

Polling this endpoint is discouraged in favor of subscribing to WebSocket events.

#### Response

`side` indicates the taker order side. A `buy` indicates an up-tick and a `sell` indicates a down-tick.

```javascript
[
    {
        "product": "XT-BTC",
        "order_id_taker": "bf2b704c-010a-48ca-93fb-d0193f24420a",
        "order_id_maker": "31bf3a89-2a0a-40b0-9327-ec0a5cc3ce3c",
        "size": "1.77942322",
        "price": "0.05132",
        "side": "buy",
        "timestamp": 1511482279492
    },
    {
        "product": "XT-BTC",
        "order_id_taker": "c6023aca-333a-44d9-98fe-279e279f3a86",
        "order_id_maker": "7f66a16c-0b47-4bd6-8add-6b2c33366082",
        "size": "3.97422208",
        "price": "0.051323",
        "side": "buy",
        "timestamp": 1511482127569
    }
]
```


### Get Candles

**`https://api.wcex.com/exchange/candles/:product/:resolution`**

Lists historical candles for a product. Candles returned are grouped by `resolution`.

#### Request

| Path Parameter | Type | Description |
| :--- | :--- | :--- |
| product | `string` | A valid product. |
| resolution | `string` | Can be 1, 5, 15, 60, 240, or 1D. |

| Query Parameter | Type | Description |
| :--- | :--- | :--- |
| start (optional) | `number` | 
The time after which to fetch candles, in ms. |
| end (optional) | `number` | 
The time before which to fetch candles, in ms. |
| limit (optional) | `string` | 
Number of candles to return (max 150). |

E.g. `/candles/XT-BTC/60?start=1511480127561&end=1511480129513` fetches 1h candles on XT/BTC between the start and end timestamps.

#### Response

Each item in the array returned represents a candle with granularity `resolution`:

* **timestamp** \(in seconds\)
* **open** \(first trade in this interval\)
* **high** \(highest trade in this interval\)
* **low** \(lowest trade in this interval\)
* **close** \(last trade in this interval\)
* **volume** \(volume of trading during this interval\)

`volume` is returned as string to preserve float number precision. If the candle is active, `close` is the last price.

```javascript
[
    [
        "1511481600",
        "0.051323",
        "0.051323",
        "0.05132",
        "0.05132",
        "1009.753645"
    ],
    [
        "1511467200",
        "0.051323",
        "0.051323",
        "0.051323",
        "0.051323",
        "3201.906964"
    ],
    [
        "1511251200",
        "0.044683",
        "0.053744",
        "0.042048",
        "0.045321",
        "3310.527428"
    ]
]
```