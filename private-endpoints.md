# Exchange

**Endpoint URL**

**`https://api.wcex.com/exchange`**

## Authenticated Endpoints

{% api-method method="post" host="https://api.wcex.com/exchange" path="/order/new" %}
{% api-method-summary %}
Place a New Order
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to place a new order on the exchange.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="product" type="string" required=true %}
A valid product.
{% endapi-method-parameter %}

{% api-method-parameter name="side" type="string" required=true %}
Can be `buy` or `sell.`
{% endapi-method-parameter %}

{% api-method-parameter name="type" type="string" required=true %}
Should be `limit`.
{% endapi-method-parameter %}

{% api-method-parameter name="size" type="string" required=true %}
Amount to buy or sell.
{% endapi-method-parameter %}

{% api-method-parameter name="price" type="string" required=true %}
Price of your order.
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
    "id": "9d335cce-d581-45f6-8efc-bdab3d61c6e2",
    "product": "XT-BTC",
    "type": "limit",
    "side": "buy",
    "size": "10.00000000",
    "price": "0.04",
    "filled": "2.23000000",
    "timestamp": "1511467005839"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Currently, only limit orders are supported. To have your order execute immediately, simply set a price deep in the opposite book.

#### Order Lifecycle

Your account must have sufficient balance to successfully submit an order.

If any part of the order results in adding liquidity to the book, it will be marked as **open**.

An order that is completely filled and off the book will be marked as **done**.

#### Response

An order accepted by the matching engine will be assigned an order `id`which will appear in the response.

`filled` specifies the amount that was matched as a result of your order.

`timestamp` is the time your order was received by the matching engine.

Cancel

Get Orders

Get Fills

Get Transactions

Get Balances

Get Deposit Address

