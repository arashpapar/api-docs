# Overview

## Introduction

WCX offers REST and WebSocket APIs for both the exchange and our margin trading service. This documentation outlines the API's functionality.

You can use the API to programmatically place new orders, check your balance, or stream real-time price updates, among other things.

For margin trading, both real and demo trading are supported.

All request and response bodies are encoded in JSON \(`application/json`\). All requests must be made over SSL.

## Data Types

**Timestamp**  
Timestamps are in milliseconds since UNIX epoch. All date libraries in modern languages should be able to handle this without issue.

**Numbers**  
We recommend passing all numbers as strings in your requests, instead of integers or floats, to preserve precision and accurate processing.

Numbers returned by the API, particularly decimals, will be returned as strings.

## Rate Limits

WCX rate limits its API to prevent abuse that would degrade the exchange's ability to maintain top-notch performance for all traders.

By default, all endpoints are limited to 5 requests per second per IP. Requests that exceed this limit will receive a Rate Limit Exceeded error with status code `429`.

For real-time data, we highly recommend you subscribe to WebSocket events instead of polling REST endpoints.

## Errors

Bad requests receive an HTTP `4xx` response with a `message` describing the error.

| Code | Description |
| :--- | :--- |
| 401 | Unauthorized – Authentication error |
| 402 | Failed Request |
| 403 | Forbidden |
| 404 | Resource Not Found |
| 422 | Unprocessable Entity – there was an issue parsing your request |
| 429 | Rate Limit Exceeded – too many requests |
| 5xx | Internal Server Error – there was a problem with our servers |

If the request is successful, a `200` \(OK\) status code is returned, along with an optional body as documented in this API reference.

## Endpoints

#### **REST Endpoint URL**

**`https://api.wcex.com`**

#### WebSocket Endpoint URL

**`wss://data.wcex.com`**

{% api-method method="get" host="https://api.wcex.com" path="/time" %}
{% api-method-summary %}
Get Server Time
{% endapi-method-summary %}

{% api-method-description %}
Returns the server time.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="client\_time" type="number" required=false %}
Send client\_time \(in ms since Epoch\) to include the time drift between your client and our server in the response.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "timestamp": 1511572042589
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



