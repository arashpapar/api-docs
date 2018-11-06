# Margin Trading

**Endpoint URL**

**`https://api.wcex.com/trading`**

## Authenticated Endpoints

{% api-method method="post" host="https://api.wcex.com/trading" path="/trade/new" %}
{% api-method-summary %}
Place a New Order
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="product" type="string" required=true %}
A valid product.
{% endapi-method-parameter %}

{% api-method-parameter name="side" type="string" required=true %}
Can be `buy` or `sell`.
{% endapi-method-parameter %}

{% api-method-parameter name="type" type="string" required=true %}
Can be `market` or `pending`.
{% endapi-method-parameter %}

{% api-method-parameter name="leverage" type="number" required=true %}
Leverage to use.
{% endapi-method-parameter %}

{% api-method-parameter name="amount" type="integer" required=true %}
Amount in contracts.
{% endapi-method-parameter %}

{% api-method-parameter name="base\_currency" type="string" required=true %}
Must be `BTC`.
{% endapi-method-parameter %}

{% api-method-parameter name="price" type="string" required=false %}
Required only if type is `pending`.
{% endapi-method-parameter %}

{% api-method-parameter name="take\_profit" type="string" required=false %}
The trade's take-profit price.
{% endapi-method-parameter %}

{% api-method-parameter name="stop\_loss" type="string" required=false %}
The trade's stop-loss price.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Order successfully submitted.
{% endapi-method-response-example-description %}

```javascript
{
        "id": "bf2b704c-010a-48ca-93fb-d0193f24420a",
        "product": "EUR-USD",
        "price": "1.26635",
        "side": "buy",
        "leverage": "300",
        "amount": 1000000,
        "margin": "3.33333333",
        "base_size": "1000",
        "base_currency": "BTC",
        "type": "market",
        "liquidation_price": "1.26354",
        "decay_rate": "0.0002",
        "created_at": 1511482279492,
        "opened_at": 1511482876413
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

A trade with type `market` will result in an open position.

A trade with type `pending` will result in a pending order and behaves as either a limit or stop order depending on its price \(it works in both directions\).

#### Response

A trade accepted by the engine will be assigned a trade `id`which will appear in the response.

{% api-method method="post" host="https://api.wcex.com/trading" path="/trade/cancel" %}
{% api-method-summary %}
Cancel Order
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
The ID of the order to cancel.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "id": "9d335cce-d581-45f6-8efc-bdab3d61c6e2"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

#### Response

If successful, the response will contain the ID of the cancelled order.

If an order could not be cancelled \(if it has been already filled, does not exist, etc.\), an error is returned.

{% api-method method="post" host="https://api.wcex.com/trading" path="/trade/update" %}
{% api-method-summary %}
Update Order
{% endapi-method-summary %}

{% api-method-description %}
Updates an order or position.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
The ID of the order or position to update.s
{% endapi-method-parameter %}

{% api-method-parameter name="price" type="number" required=false %}
New price for the order.
{% endapi-method-parameter %}

{% api-method-parameter name="take\_profit" type="number" required=false %}
New take-profit price for the order.
{% endapi-method-parameter %}

{% api-method-parameter name="stop\_loss" type="number" required=false %}
New stop-loss price for the order.
{% endapi-method-parameter %}

{% api-method-parameter name="margin" type="number" required=false %}
Updated margin amount for the order.
{% endapi-method-parameter %}

{% api-method-parameter name="leverage" type="number" required=false %}
Updated leverage multiplier for the order.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
      "id": "bf2b704c-010a-48ca-93fb-d0193f24420a",
      "product": "EUR-USD",
      "price": "1.26635",
      "side": "buy",
      "leverage": "300",
      "amount": 1000000,
      "margin": "3.33333333",
      "base_size": "1000",
      "base_currency": "BTC",
      "type": "market",
      "liquidation_price": "1.26354",
      "take_profit": "1.28898",
      "stop_loss": "1.26477",
      "decay_rate": "0.0002",
      "created_at": 1511482279492,
      "updated_at": 1511482476135
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

For pending orders, `price`, `margin`, `leverage`, `amount`, `take_profit`, and `stop_loss` can be updated.

For open positions, `margin`, `leverage`, `take_profit`, and `stop_loss` can be updated.

To remove a `take_profit` or `stop_loss`, simply set it to `0`.

You can set both `margin` and `leverage` in the same request, but `margin` will always take precedence.

Margin can only be increased, not decreased. Accordingly, leverage can only be decreased, not increased.

{% api-method method="post" host="https://api.wcex.com/trading" path="/trade/close" %}
{% api-method-summary %}
Close Position
{% endapi-method-summary %}

{% api-method-description %}
Closes a position at market price. Sells close at the ask, buys at the bid.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
The ID of the position to close.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
        "id": "bf2b704c-010a-48ca-93fb-d0193f24420a",
        "product": "EUR-USD",
        "price": "1.26635",
        "side": "buy",
        "leverage": "300",
        "amount": 1000000,
        "margin": "3.33333333",
        "base_size": "1000",
        "base_currency": "BTC",
        "type": "market",
        "liquidation_price": "1.26354",
        "decay_rate": "0.0002",
        "close_price": "1.26995",
        "pnl": "2.84281590",
        "pnl_percent": "85.2845",
        "reason": "manual",
        "decay": "0",
        "created_at": 1511482279492,
        "opened_at": 1511482876413,
        "closed_at": 1511483816212
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.wcex.com/trading" path="/trade/split" %}
{% api-method-summary %}
Split Position
{% endapi-method-summary %}

{% api-method-description %}
Splits a position into two positions that can be managed separately.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
The ID of the position to split.
{% endapi-method-parameter %}

{% api-method-parameter name="ratio" type="string" required=true %}
A ratio of percents. E.g. `40:60`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.wcex.com/trading" path="/orders" %}
{% api-method-summary %}
Get Orders
{% endapi-method-summary %}

{% api-method-description %}
Lists your pending orders, sorted by submission time \(most recent first\).
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="limit" type="integer" required=false %}
Number of orders to return \(max is 100\).
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
    {
        "id": "bf2b704c-010a-48ca-93fb-d0193f24420a",
        "product": "EUR-USD",
        "price": "1.26635",
        "side": "buy",
        "leverage": "300",
        "amount": 1000000,
        "margin": "3.33333333",
        "base_size": "1000",
        "base_currency": "BTC",
        "type": "market",
        "liquidation_price": "1.26354",
        "decay_rate": "0.0002",
        "created_at": 1511482279492
    }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

You can optionally pass a `limit` query parameter to limit the number of orders returned. By default, 100 items are returned.

E.g. `/orders?limit=50`.

#### Keeping track of your orders

We recommend listening to streaming WebSocket order events to maintain an up-to-date view of your open orders. It's faster and in many cases more accurate than polling this endpoint, because an order's state may change between the time you make an HTTP request and receive a response.

Polling this endpoint is highly discouraged.

#### Response

An array of your pending orders.

{% api-method method="get" host="https://api.wcex.com/trading" path="/positions" %}
{% api-method-summary %}
Get Positions
{% endapi-method-summary %}

{% api-method-description %}
Lists your open positions, sorted by opening time.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="limit" type="integer" required=false %}
Number of positions to return \(max is 100\).
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
    {
        "id": "bf2b704c-010a-48ca-93fb-d0193f24420a",
        "product": "EUR-USD",
        "price": "1.26635",
        "side": "buy",
        "leverage": "300",
        "amount": 1000000,
        "margin": "3.33333333",
        "base_size": "1000",
        "base_currency": "BTC",
        "type": "market",
        "liquidation_price": "1.26354",
        "decay_rate": "0.0002",
        "created_at": 1511482279492,
        "opened_at": 1511483479492
    }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.wcex.com/trading" path="/history/:product?" %}
{% api-method-summary %}
Get History
{% endapi-method-summary %}

{% api-method-description %}
List your recent trades \(closed positions\), ordered by close time.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="product" type="string" required=false %}
Limit trades returned to this product.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="timestamp" type="number" required=false %}
Return trades that occurred before this time.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Number of trades to return \(max is 100\).
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
    {
        "id": "bf2b704c-010a-48ca-93fb-d0193f24420a",
        "product": "EUR-USD",
        "price": "1.26635",
        "side": "buy",
        "leverage": "300",
        "amount": 1000000,
        "margin": "3.33333333",
        "base_size": "1000",
        "base_currency": "BTC",
        "type": "market",
        "liquidation_price": "1.26354",
        "decay_rate": "0.0002",
        "created_at": 1511482279492,
        "opened_at" 1511483479492,
        "closed_at" 1511483979492
    }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Omitting `product` will return trades across all products.

E.g. `/history?limit=50&timestamp=1511481127561`

#### Keeping track of your trades

We recommend listening to streaming WebSocket match events to maintain an up-to-date view of your closed positions. It's faster and more accurate than polling this endpoint.

Polling this endpoint is highly discouraged.

#### Response

An array of your recent trades.

{% api-method method="get" host="https://api.wcex.com/trading" path="/balances/:asset?" %}
{% api-method-summary %}
Get Balances
{% endapi-method-summary %}

{% api-method-description %}
Lists your balances.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="asset" type="string" required=false %}
Limits balances returned to this asset.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "BTC": "11.88900211",
    "ETH": "2218.99900000"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Omitting `asset` will return all balances on your account. Only non-zero balances, or assets for which you have made at least one deposit will be returned.

{% api-method method="get" host="https://api.wcex.com/trading" path="/transactions/:type" %}
{% api-method-summary %}
Get Transactions
{% endapi-method-summary %}

{% api-method-description %}
Lists your deposits and withdrawals, sorted by most recent first.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="type" type="string" required=false %}
Can be `deposit` or `withdraw`.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="asset" type="string" required=false %}
Limits transactions returned to this asset.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="string" required=false %}
Number of transactions to return \(max 100\).
{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=false %}
Return transactions that occurred before this time.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

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
        "asset": "ETH",
        "amount": "200.00990000",
        "type": "withdraw",
        "address": "0xa93fae849449d2f9ddca15b861fcb329d39ad47c",
        "txid": "0xaaec962c014ab72eec8d4abb58326dca1c5c8a9425cd851ef8b9c390f2ad050e",
        "timestamp": 1511482279491,
        "confirmed_at": 1511482379491
    }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Omitting type will return all transactions on your account.

E.g. `/transactions?asset=BTC&limit=50&timestamp=1511481127561`

{% api-method method="get" host="https://api.wcex.com/trading" path="/deposit/:asset" %}
{% api-method-summary %}
Get Deposit Address
{% endapi-method-summary %}

{% api-method-description %}
Returns a deposit address for the provided asset.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="asset" type="string" required=true %}
A valid asset.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "address": "2MzeJNyp47Cugv85aq361AwBmaC4kxHofuu",
    "asset": "BTC",
    "confirmations_needed": 2
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Public Endpoints

{% api-method method="get" host="https://api.wcex.com/trading" path="/products/:product?" %}
{% api-method-summary %}
Get Products
{% endapi-method-summary %}

{% api-method-description %}
Lists products available to trade.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="product" type="string" required=false %}
Limits results returned to this products.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "forex": [
        [
            "EUR-USD", // product
            "300", // max leverage
            "0.00001", // tick
            "-0.0001", // decay rate
            {
                "BTC": "600000" // max size
            }
            "EUR/USD" // name
        ],
    ...
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.wcex.com/trading" path="/quotes/:product\(s\)" %}
{% api-method-summary %}
Get Quotes
{% endapi-method-summary %}

{% api-method-description %}
Returns quotes for the provided products\(s\).
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="product\(s\)" type="string" required=true %}
One or more comma-separated products.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
        "EUR-USD": [
                "1.12442", // mid price
                "1.12439", // bid price
                "1.12445", // ask price
                151143431932 // timestamp
        ],
        ...
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

E.g. `/quotes/EUR-USD,AAPL,SNAP,ETH-USD`

Polling this endpoint is highly discouraged in favor of subscribing to WebSocket events.

E.g. `/book/XT-BTC?limit=50`.

Polling this endpoint is discouraged in favor of subscribing to WebSocket events.

{% api-method method="get" host="https://api.wcex.com/trading" path="/candles/:product/:resolution" %}
{% api-method-summary %}
Get Candles
{% endapi-method-summary %}

{% api-method-description %}
Lists historical candles for a product. Candles returned are grouped by `resolution`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="product" type="string" required=true %}
A valid product.
{% endapi-method-parameter %}

{% api-method-parameter name="resolution" type="string" required=true %}
Can be 1, 5, 15, 60, 240, or 1D.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="signal" type="string" required=false %}
Can be `mid`, `bid`, or `ask`. Defaults to `mid`.
{% endapi-method-parameter %}

{% api-method-parameter name="start" type="number" required=false %}
The time after which to fetch candles, in ms.
{% endapi-method-parameter %}

{% api-method-parameter name="end" type="number" required=false %}
The time before which to fetch candles, in ms.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
Number of candles to return \(max 150\).
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
    [
        "1511481600",
        "0.051323",
        "0.051323",
        "0.05132",
        "0.05132"
    ],
    [
        "1511467200",
        "0.051323",
        "0.051323",
        "0.051323",
        "0.051323"
    ],
    [
        "1511251200",
        "0.044683",
        "0.053744",
        "0.042048",
        "0.045321"
    ]
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

E.g. `/candles/EUR-USD/60?signal=bid&start=1511480127561&end=1511480129513` fetches 1h bid candles on EUR/BTC between the start and end timestamps.

#### Response

Each item in the array returned represents a candle with granularity `resolution`:

* **timestamp** \(in seconds\)
* **open** \(first trade in this interval\)
* **high** \(highest trade in this interval\)
* **low** \(lowest trade in this interval\)
* **close** \(last trade in this interval\)

If the candle is active, `close` is the last price.

