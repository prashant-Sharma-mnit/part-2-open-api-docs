# 📘 5paisa Xtream Open APIs Documentation Part2

---

## Table of Contents

### 3. 📋 Order & Trade Tracking  
- [OrderStatus](#orderstatus)  
- [OrderBook](#orderbook)  
- [TradeBook](#tradebook)

### 4. 📈 Positions & Holdings  
- [NetPositionNetWise](#netpositionnetwise)  
- [Holding](#holding)  

### 5. 💰 Margin  
- [Margin](#margin)  

---

# OrderStatus

## 🔹 API Name
`v2/OrderStatus`

---

## 🎯 Purpose

The `OrderStatusV2` API allows you to **fetch real-time status of one or more orders** placed via the 5paisa trading system. Ideal for algorithmic trading, dashboards, and bot monitoring workflows.

---

## 📌 Endpoint

```
https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V2/OrderStatus
```


---

## 📥 Request Structure

### ➕ Method
`POST`

### ➕ Headers

| Key             | Value                        |
|----------------|------------------------------|
| Content-Type   | application/json             |
| Authorization  | Bearer `<YourAccessToken>`   |

---

### 🧾 Sample Request Body

```json
{
  "head": {
    "key": "<YourAppKey>"
  },
  "body": {
    "ClientCode": "<ClientCode>",
    "OrdStatusReqList": [
      {
        "Exch": "N",
        "RemoteOrderID": "0327020205139304480"
      },
      {
        "Exch": "N",
        "RemoteOrderID": "203051105331"
      }
    ]
  }
}
````

---

## 📤 Success Response Example

```json
{
  "body": {
    "Message": "Success",
    "OrdStatusResLst": [
      {
        "AveragePrice": 431.05,
        "Exch": "N",
        "ExchOrderID": 10000365323,
        "ExchOrderTime": "/Date(1715587966000+0530)/",
        "ExchType": "C",
        "OrderQty": 1,
        "OrderRate": 431.05,
        "PendingQty": 1,
        "ScripCode": 1660,
        "Status": "Modified",
        "Symbol": "ITC",
        "TradedQty": 0
      },
      {
        "AveragePrice": 431.05,
        "Exch": "N",
        "ExchOrderID": 100000365383,
        "ExchOrderTime": "/Date(1715587966000+0530)/",
        "ExchType": "C",
        "OrderQty": 1,
        "OrderRate": 431.05,
        "PendingQty": 0,
        "ScripCode": 1660,
        "Status": "Fully Executed",
        "Symbol": "ITC",
        "TradedQty": 1
      }
    ],
    "Status": 0
  },
  "head": {
    "responseCode": "5POrdStatusV2",
    "status": "0",
    "statusDescription": "Success"
  }
}
```

---

## ❌ Failure Response Example

```json
{
  "body": {
    "Message": "Success",
    "OrdStatusResLst": [],
    "Status": 0
  },
  "head": {
    "responseCode": "5POrdStatusV2",
    "status": "0",
    "statusDescription": "Success"
  }
}
```

---

## 📘 Field Definitions

### 🔸 Request Parameters

| Field           | Type   | Required | Description                               |
| --------------- | ------ | -------- | ----------------------------------------- |
| `Exch`          | string | Yes      | Exchange code (e.g. `"N"` for NSE)        |
| `RemoteOrderID` | string | Yes      | Order ID generated at the time of placing |
| `ClientCode`    | string | Yes      | User's client code                        |

---

### 🔸 Response Fields

| Field           | Type     | Description                                   |
| --------------- | -------- | --------------------------------------------- |
| `AveragePrice`  | float    | Average traded price                          |
| `Exch`          | string   | Exchange code                                 |
| `ExchOrderID`   | string   | Order ID from exchange                        |
| `ExchOrderTime` | datetime | Order entry timestamp                         |
| `ExchType`      | string   | Exchange type (e.g., C = Cash, D = Deriv.)    |
| `OrderQty`      | int      | Total quantity                                |
| `OrderRate`     | float    | Price at which order was placed               |
| `PendingQty`    | int      | Quantity yet to be traded                     |
| `ScripCode`     | int      | Unique instrument code                        |
| `Status`        | string   | Current status (e.g., "Modified", "Executed") |
| `Symbol`        | string   | Trading symbol (e.g., "ITC")                  |
| `TradedQty`     | int      | Quantity already traded                       |

---

## 📑 Order Status Values

| Status             | Meaning                             |
| ------------------ | ----------------------------------- |
| `Fully Executed`   | Order completed                     |
| `Modified`         | Order was modified                  |
| `Xmitted`          | Not reached or rejected by exchange |
| `Rejected By 5P`   | Rejected by 5paisa system           |
| `Rejected by Exch` | Rejected by exchange                |
| `Cancelled`        | Order was cancelled                 |
| `Pending`          | Order placed and awaiting execution |

---

## 🔁 Use Cases

* Checking real-time order status in trading dashboards
* Monitoring order lifecycle in algorithmic bots
* Integrating into RAG-based AI assistants for trading automation

---

## ⚙️ Best Practices

* Batch multiple order queries (up to \~50)
* Validate session token before calling
* Use proper exchange codes (`N`, `B`, `M`, `X`)

---

## 🔐 Authentication

* Requires a valid bearer token in the `Authorization` header
* App Key in the `head.key` field is mandatory
* Session tokens must be refreshed periodically

---

## 📎 Additional Notes

* Use this API instead of full Order Book API for specific order lookup
* Exchange Order ID from this API can be used for modifying/cancelling orders
* All timestamps returned are Unix/Epoch-style (can be converted)

---

## 🧪 Postman / cURL Example

```bash
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V2/OrderStatus' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <YourAccessToken>' \
--data '{
  "head": {
    "key": "<YourAppKey>"
  },
  "body": {
    "ClientCode": "<ClientCode>",
    "OrdStatusReqList": [
      {
        "Exch": "N",
        "RemoteOrderID": "0327020205139304480"
      }
    ]
  }
}'
```

---

# OrderBook

## Overview of Order Book V4 API
The **Order Book V4 API** enables partners and clients to retrieve the order book details of a user for the current trading day. It supports orders across multiple segments including cash, derivatives, currency, and commodities.

This API is essential for tracking the status of placed orders by providing detailed order information such as average price, trigger rates, traded and pending quantities. It also provides key identifiers like remote order ID, broker order ID, and exchange order ID for mapping and managing orders efficiently.

> **Note:** For real-time order status updates, consider using WebSocket connections or the dedicated Order Status API.

---

## API Endpoint
```
POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V4/OrderBook
```
---

## Request Parameters

| Parameter            | Type    | Required | Description                                                                                  |
|----------------------|---------|----------|----------------------------------------------------------------------------------------------|
| `ClientCode`         | String  | Yes      | Unique identifier for the client whose order book is being requested.                        |
| `updatedInLastSeconds` | Integer | No       | Filters orders updated in the last *x* seconds (valid range: 0 to 600).                      |

### Parameter Details

- **ClientCode**  
  Identifier for the client. Must be provided to fetch the relevant order book.

- **updatedInLastSeconds** (Optional)  
  - If provided and between 1 and 600, only orders updated within the last *x* seconds are returned.  
  - If `0` or not provided, the full order book is returned without filtering.  
  - If no orders are updated in the specified time frame, response will indicate:  
    `"No Order found for this Client."`  
  - If value is out of range or negative, an error is returned:  
    `"updatedInLastSeconds should be in range from 0 to 600"`.

---

## Request Example (JSON)

```json
{
  "head": {
    // Standard request header fields
  },
  "body": {
    "ClientCode": "YOUR_CLIENT_CODE",
    "updatedInLastSeconds": 60
  }
}
````

---

## Response Structure

| Field       | Type    | Description                                                      |
| ----------- | ------- | ---------------------------------------------------------------- |
| `status`    | Integer | Execution status code (0 = Success, other codes indicate errors) |
| `message`   | String  | Descriptive message related to the API call                      |
| `orderBook` | Array   | List of order details objects                                    |

### Order Details Object

| Field                | Type    | Description                        |
| -------------------- | ------- | ---------------------------------- |
| `Exch`               | String  | Exchange code                      |
| `ExchType`           | String  | Exchange segment/type              |
| `ExchOrderID`        | String  | Exchange order ID                  |
| `BrokerOrderId`      | String  | Broker's order ID                  |
| `BrokerOrderTime`    | String  | Timestamp of broker order          |
| `BuySell`            | String  | Buy or sell indicator              |
| `AveragePrice`       | Decimal | Average traded price for the order |
| `TriggerRate`        | Decimal | Trigger price (if applicable)      |
| `TradedQty`          | Integer | Quantity traded                    |
| `PendingQty`         | Integer | Quantity pending                   |
| `OrderStatus`        | String  | Current status of the order        |
| `AtMarket`           | Boolean | Indicates if order is at market    |
| `AfterHours`         | String  | After hours flag                   |
| `AHProcess`          | String  | After hours process status         |
| `DisClosedQty`       | Integer | Disclosed quantity                 |
| `OrderRequesterCode` | String  | Code of the requester of the order |
| `OldOrderQty`        | Integer | Previous order quantity            |
| `DelvIntra`          | String  | Order for delivery or intraday     |

> The list above is a representative sample of key fields returned. Additional fields may be present depending on implementation.

---

## Response Example (JSON)

```json
{
  "head": {
    "responseCode": "5POrdBkV4",
    "status": 0,
    "statusDescription": "Success"
  },
  "body": {
    "Status": 0,
    "Message": "Success",
    "orderBook": [
      {
        "Exch": "NSE",
        "ExchType": "C",
        "ExchOrderID": "123456789",
        "BrokerOrderId": "987654321",
        "BrokerOrderTime": "2025-05-19 10:30:15.000",
        "BuySell": "B",
        "AveragePrice": 1500.25,
        "TriggerRate": 0,
        "TradedQty": 100,
        "PendingQty": 50,
        "OrderStatus": "Open",
        "AtMarket": false,
        "AfterHours": "N",
        "AHProcess": "N",
        "DisClosedQty": 0,
        "OrderRequesterCode": "ORD123",
        "OldOrderQty": 150,
        "DelvIntra": "Delivery"
      }
    ]
  }
}
```

---

## Error Handling

| Status Code | Description                                        |
| ----------- | -------------------------------------------------- |
| 1           | Invalid value for `updatedInLastSeconds` parameter |
| 2           | Missing or invalid request parameters              |
| 3           | Invalid `ClientCode`                               |
| 9           | Session invalid or unauthorized access             |

---

## Notes

* The API requires valid authentication tokens in request headers.
* This API is ideal for fetching a snapshot of the order book.
* For real-time order status updates, use the WebSocket API or dedicated order status endpoints.
* The order mapping via `ExchOrderID`, `BrokerOrderId`, and `OrderRequesterCode` allows seamless order management like modifications or cancellations.

---

## Best Practices for Integration

* Use `updatedInLastSeconds` parameter to optimize API calls for recent order changes, reducing data transfer.
* Always verify response `status` and `message` fields before processing data.
* Keep client authentication credentials secure and refresh tokens as needed.
* Combine this API data with real-time WebSocket feeds for comprehensive order tracking.

---



# TradeBook

## Overview of TradeBookV1 API

The `TradeBookV1` API allows clients to fetch their trade book for the current trading day. It returns comprehensive trade details across **Cash**, **F&O**, **Currency**, and **Commodity** segments from **all supported exchanges** (e.g., NSE, BSE, MCX).

## 📌 Endpoint

```

POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V1/TradeBook

````

---

## 📄 Purpose

The API is designed for:

- **Monitoring** executed trades.
- **Trade analysis** based on quantity, rate, and exchange.
- **Mapping** trades to respective orders using `ExchangeOrderID` and `ExchangeTradeID`.

A single order may be split into multiple trades, and this mapping enables precise trade-order relationship tracking.

---

## ✅ Features

- Returns all executed trades for the current day.
- Includes buy/sell type, exchange details, scrip info, and rates.
- Compatible across multiple asset classes and exchanges.
- Designed for integration into **trading assistants**, **portfolio tools**, or **RAG systems**.

---

## 🧾 Request Format (JSON)

```json
{
  "head": {
    "key": "<your-app-key>"
  },
  "body": {
    "ClientCode": "<your-client-code>"
  }
}
````

---

## 📤 Response Format (Success)

```json
{
  "head": {
    "responseCode": "5PTrdBkV1",
    "status": 0,
    "statusDescription": "Success"
  },
  "body": {
    "Status": 0,
    "Message": "",
    "TradeBookDetail": [
      {
        "Exch": "N",
        "ExchType": "C",
        "ScripCode": 500112,
        "ScripName": "SBIN",
        "BuySell": "B",
        "Qty": 100,
        "PendingQty": 0,
        "OrgQty": 100,
        "Rate": 540.25,
        "ExchOrderID": "ABC12345678",
        "ExchangeTradeID": "TRD987654",
        "ExchangeTradeTime": "2025-05-19T12:34:56Z",
        "DelvIntra": "D",
        "TradeType": "Online",
        "Multiplier": 1
      }
    ]
  }
}
```

---

## ❗ Error Response Example

```json
{
  "head": {
    "responseCode": "5PTrdBkV1",
    "status": 9,
    "statusDescription": "Invalid session"
  },
  "body": {
    "Status": 9,
    "Message": "Invalid session"
  }
}
```

---

## 📚 Response Field Description

| Field               | Type     | Description                              |
| ------------------- | -------- | ---------------------------------------- |
| `Exch`              | `char`   | Exchange code (e.g., `N` for NSE)        |
| `ExchType`          | `char`   | Segment type (e.g., `C` for Cash)        |
| `ScripCode`         | `int`    | Unique code of the instrument            |
| `ScripName`         | `string` | Instrument name                          |
| `BuySell`           | `char`   | `B` for Buy, `S` for Sell                |
| `Qty`               | `int`    | Traded quantity                          |
| `PendingQty`        | `int`    | Remaining quantity (if any)              |
| `OrgQty`            | `int`    | Original quantity of the order           |
| `Rate`              | `double` | Trade execution price                    |
| `ExchOrderID`       | `string` | Order reference ID from the exchange     |
| `ExchangeTradeID`   | `string` | Trade reference ID from the exchange     |
| `ExchangeTradeTime` | `string` | ISO timestamp of the trade               |
| `DelvIntra`         | `char`   | Delivery (`D`) or Intraday (`I`) flag    |
| `TradeType`         | `string` | Trade origin (`Online`, `Offline`, etc.) |
| `Multiplier`        | `int`    | Lot multiplier, useful in derivatives    |

---

## 🔐 Authorization

This API requires **JWT token-based authentication**. Ensure the token is passed in the `Authorization` header as:

```
Authorization: Bearer <your-jwt-token>
```

---

## 💡 Best Practices

* 🔍 **Map trades to orders** using `ExchOrderID` and `ExchangeTradeID`.

---

# NetPositionNetWise

## Overview of NetPosition_NetWiseV3 API
The **NetPosition_NetWiseV3** API provides detailed net position data for a client across multiple exchanges and product types. It aggregates positions in equity and commodity segments, delivering net quantities, average rates, and related metrics in a structured response.

---

## API Endpoint
```
POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V3/NetPositionNetWise
```
---

## Request Structure

```json
{
  "head": {
    "key": "string"
  },
  "body": {
    "ClientCode": "string"
  }
}
````

### Request Parameters

| Field              | Type   | Description                         |
| ------------------ | ------ | ----------------------------------- |
| `head.key`         | string | Authentication/authorization key    |
| `body.ClientCode`  | string | Unique client identifier            |

---

## Response Structure

```json
{
  "head": {
    "responseCode": "string",
    "status": int,
    "statusDescription": "string"
  },
  "body": {
    "Status": int,
    "Message": "string",
    "NetPositions": [
      {
        "Exch": "string",
        "ExchType": "string",
        "ScripCode": int,
        "ScripName": "string",
        "BuyQty": int,
        "SellQty": int,
        "NetQty": int,
        "AvgRate": double,
        "LastRate": double,
        "Multiplier": double,
        "Product": "string",
        "AdditionalFields": "..."
      }
    ]
  }
}
```

### Response Fields Description

| Field                    | Type   | Description                                               |
| ------------------------ | ------ | --------------------------------------------------------- |
| `head.responseCode`      | string | Code representing the API response                        |
| `head.status`            | int    | Status code (0 for success, other values indicate errors) |
| `head.statusDescription` | string | Text description of the status                            |
| `body.Status`            | int    | Business status code                                      |
| `body.Message`           | string | Response message or error description                     |
| `body.NetPositions`      | array  | List of net position details for various scrips           |

Each element in `NetPositions` includes:

| Field              | Type   | Description                                             |
| ------------------ | ------ | ------------------------------------------------------- |
| `Exch`             | string | Exchange code (e.g., N for NSE, M for MCX)              |
| `ExchType`         | string | Exchange segment/type                                   |
| `ScripCode`        | int    | Unique code identifying the trading instrument          |
| `ScripName`        | string | Name of the trading instrument                          |
| `BuyQty`           | int    | Total quantity bought                                   |
| `SellQty`          | int    | Total quantity sold                                     |
| `NetQty`           | int    | Net quantity (BuyQty - SellQty plus adjustments)        |
| `AvgRate`          | double | Average rate of the position                            |
| `LastRate`         | double | Last traded price                                       |
| `Multiplier`       | double | Multiplier used for value calculation                   |
| `Product`          | string | Product type indicator (e.g., Delivery, Intraday)       |
| `AdditionalFields` | varies | Other position-related metrics (e.g., BodQty, BuyValue) |

---

## Status Codes

| Code | Meaning                         |
| ---- | ------------------------------- |
| 0    | Success                         |
| 1    | No records found                |
| 2    | Validation error                |
| 9    | Invalid session or unauthorized |

---

## Example Request

```json
{
  "head": {
    "key": "your_auth_key"
  },
  "body": {
    "ClientCode": "CLIENT1234"
  }
}
```

---

## Example Successful Response

```json
{
  "head": {
    "responseCode": "5PNPNWV3",
    "status": 0,
    "statusDescription": "Success"
  },
  "body": {
    "Status": 0,
    "Message": "Success",
    "NetPositions": [
      {
        "Exch": "N",
        "ExchType": "Y",
        "ScripCode": 12345,
        "ScripName": "ABC Ltd",
        "BuyQty": 100,
        "SellQty": 50,
        "NetQty": 50,
        "AvgRate": 150.25,
        "LastRate": 152.00,
        "Multiplier": 1,
        "Product": "D"
      }
    ]
  }
}
```

---

## Notes

* Authorization is required; requests without valid authorization will be rejected.
* The API aggregates net positions by product type and exchange.
* If no data is found for the client, the response will indicate "No record found."
* Input validation is performed on request headers and body fields.
* The API supports multiple product types including Delivery (D), Intraday (I), and others.

---



## Best Practices

* Use secure storage and transmission for authorization tokens.
* Always check response status before processing data.
* Handle empty or partial data scenarios gracefully.

---
# Holding

## Overview Holding_V4 API

The **HoldingV4** API retrieves detailed holdings data for a client’s stock portfolio. It provides essential information such as instrument codes, quantity held, current price, pledge details, and other metadata related to each holding. This API requires authenticated access and is typically used to build the holdings section in trading or portfolio management applications.

---

## Endpoint

```
POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V4/Holding
```

---

## Request Headers

| Header        | Description                     | Example                       |
| ------------- | ------------------------------- | ----------------------------- |
| Content-Type  | Must be `application/json`      | `application/json`            |
| Authorization | Bearer token for authentication | `Bearer <JWT-token>`          |
| Cookie        | Optional session cookie         | `5paisacookie=<cookie_value>` |

---

## Request Body

```json
{
  "head": {
    "key": "<api-key>"
  },
  "body": {
    "ClientCode": "<client-code>"
  }
}
```

### Request Body Properties

| Property        | Type   | Description                | Required |
| --------------- | ------ | -------------------------- | -------- |
| head.key        | string | API key for access control | Yes      |
| body.ClientCode | string | Unique client identifier   | Yes      |

---

## cURL Example

```bash
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V4/Holding' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <your-jwt-token>' \
--header 'Cookie: 5paisacookie=<your-cookie>' \
--data '{
    "head": {
        "key": "<api-key>"
    },
    "body": {
        "ClientCode": "<client-code>"
    }
}'
```

---

## Response

### Success Response (HTTP 200)

```json
{
  "head": {
    "responseCode": "5PHoldingV4",
    "status": "0",
    "statusDescription": "Success"
  },
  "body": {
    "Status": 0,
    "Message": "Success",
    "CacheTime": 300,
    "Data": [
      {
        "Exch": "B",
        "ExchType": "C",
        "NseCode": 14366,
        "BseCode": 532822,
        "Symbol": "IDEA",
        "FullName": "VODAFONE IDEA LIMITED",
        "Quantity": 52,
        "CurrentPrice": 7.09,
        "PoolQty": 0,
        "DPQty": 44,
        "POASigned": "N",
        "ScripMultiplier": 1,
        "AvgRate": 7.2338,
        "ISIN": "INE669E01016",
        "MTFPledge": 0,
        "MTFQty": 0,
        "MarginPledge": 8,
        "PledgeQty": 8
      }
    ]
  }
}
```

### Response Properties

| Property               | Type   | Description                                     |
| ---------------------- | ------ | ----------------------------------------------- |
| head.responseCode      | string | API response identifier                         |
| head.status            | string | Status code (0 = success)                       |
| head.statusDescription | string | Status message                                  |
| body.Status            | int    | API call status (0 = success, non-zero = error) |
| body.Message           | string | Informational message                           |
| body.CacheTime         | int    | Cache validity duration (seconds)               |
| body.Data              | array  | List of holdings records                        |

### Holding Record Fields

| Property        | Type   | Description                                    |
| --------------- | ------ | ---------------------------------------------- |
| Exch            | char   | Exchange code (e.g., 'B' for BSE)              |
| ExchType        | char   | Exchange type (e.g., 'C' for Cash)             |
| NseCode         | int    | NSE instrument code                            |
| BseCode         | int    | BSE instrument code                            |
| Symbol          | string | Stock symbol                                   |
| FullName        | string | Full name of the instrument                    |
| Quantity        | long   | Quantity held                                  |
| CurrentPrice    | double | Current market price                           |
| PoolQty         | int    | Quantity in pool (if any)                      |
| DPQty           | int    | Demat Participant quantity                     |
| POASigned       | char   | POA (Power of Attorney) flag                   |
| ScripMultiplier | int    | Multiplier for the scrip                       |
| AvgRate         | double | Average rate (price) of the holding            |
| ISIN            | string | International Securities Identification Number |
| MTFPledge       | int    | Margin trading pledge quantity                 |
| MTFQty          | int    | Margin trading finance quantity                |
| MarginPledge    | int    | Quantity pledged as margin                     |
| PledgeQty       | int    | Quantity pledged                               |

---

## Error Responses

### Invalid Head Parameters

```json
{
  "head": {
    "responseCode": "5PHoldingV4",
    "status": "2",
    "statusDescription": "Invalid head parameters."
  },
  "body": null
}
```

### No Records Found

```json
{
  "head": {
    "responseCode": "5PHoldingV4",
    "status": "0",
    "statusDescription": "Success"
  },
  "body": {
    "Status": 1,
    "Message": "No record found.",
    "CacheTime": 300,
    "Data": []
  }
}
```

### Invalid Session / Authentication Failure

```json
{
  "head": {
    "responseCode": "5PHoldingV4",
    "status": "9",
    "statusDescription": "Invalid session."
  },
  "body": {
    "Status": 9,
    "Message": "Invalid session."
  }
}
```

---



## Notes

* **Authentication**: Requires valid Bearer token or session cookie.
* **ClientCode** should match the authenticated user’s client code.
* **CacheTime** indicates how long the response can be cached to optimize subsequent calls.
* This API is designed for retrieving real-time portfolio holdings and related analytics.

---

# Margin

## Overview of MarginV4 API

The `MarginV4` API provides detailed margin information for a specified client code. It retrieves equity and mutual fund margin details by querying the backend database and returning structured financial data. This API is essential for stock trading platforms needing to check real-time margin status for users.

---

## Endpoint

```
POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V4/Margin
```

---

## Authentication

- Requires Bearer Token authorization.
- Include the token in the `Authorization` header as:  
  `Authorization: Bearer <JWT_Token>`
- Cookie header may be required with valid session cookie:  
  `Cookie: 5paisacookie=<session_token>`

---

## Request Headers

| Header           | Description                 | Required |
|------------------|-----------------------------|----------|
| Content-Type     | Must be `application/json`  | Yes      |
| Authorization    | Bearer JWT token            | Yes      |
| Cookie           | Session cookie              | Optional |

---

## Request Body

```json
{
  "head": {
    "key": "dVOtp6A6mRzrBO6SdAs7Vf"
  },
  "body": {
    "ClientCode": "50"
  }
}
```

| Parameter  | Type   | Description                           | Required |
|------------|--------|-------------------------------------|----------|
| head.key   | string | API access key                      | Yes      |
| body.ClientCode | string | Unique client identifier for margin data | Yes      |

---

## cURL Example

```bash
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V4/Margin' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' \
--header 'Cookie: 5paisacookie=kmfrozkfxvest' \
--data '{
    "head": {
        "key": "dfgh34567fgh
    },
    "body": {
        "ClientCode": "456789"
    }
}'
```

---

## Response Body

```json
{
  "head": {
    "responseCode": "5PMarginV4",
    "status": 0,
    "statusDescription": "Success"
  },
  "body": {
    "ClientCode": "50",
    "Status": 0,
    "Message": "",
    "EquityMargin": [
      {
        "AdhocMargin": 0,
        "CollateralValueAfterHairCut": 0,
        "NetAvailableMargin": 0,
        "MarginUtilized": 0,
        "TotalCollateralValue": 0,
        "DerivativeMargin": 0,
        "Ledgerbalance": 0,
        "DPFreeStockValue": 0,
        "GrossHoldingValue": 0,
        "GrossHoldingValueCoverPercentage": 0,
        "HairCut": 0,
        "MarginBlockedforOpenPostion_Cash": 0,
        "MarginBlockedforOpenPostion_Collateral": 0,
        "MarginBlockedforPendingOrder_Cash": 0,
        "MarginBlockedforPendingOrder_Collateral": 0,
        "MFCollateralValueAfterHaircut": 0,
        "OptionsPremium": 0,
        "FundsWithdrawal": 0,
        "MarginBlockedForPendingOrders": 0,
        "FundsPayln": 0,
        "TodaysLoss": 0,
        "Unsettled_Credits": 0
      }
    ],
    "MFMargin": [
      {
        "MFCollateralValue": 0,
        "MFFreeStockValue": 0,
        "MFHaircutValue": 0
      }
    ],
    "TimeStamp": "2025-05-19T12:00:00"
  }
}
```

---

## Response Fields Description

| Field                             | Type     | Description                                 |
|----------------------------------|----------|---------------------------------------------|
| head.responseCode                | string   | Internal API request code                    |
| head.status                      | int      | Status code (0 = Success, others indicate error) |
| head.statusDescription           | string   | Status message                              |
| body.ClientCode                 | string   | Client code reflected back                  |
| body.Status                     | int      | Business logic status (0 = Success)          |
| body.Message                    | string   | Error or success message                     |
| body.EquityMargin               | array    | Array of equity margin details               |
| body.EquityMargin[].AdhocMargin | decimal  | Adhoc margin amount                           |
| body.EquityMargin[].CollateralValueAfterHairCut | decimal | Collateral value after haircut               |
| body.EquityMargin[].NetAvailableMargin | decimal | Net margin available                          |
| ...                            | ...      | Other margin fields as shown above           |
| body.MFMargin                  | array    | Array of mutual fund margin details          |
| body.MFMargin[].MFCollateralValue | decimal | Mutual fund collateral value                   |
| body.MFMargin[].MFFreeStockValue | decimal  | Mutual fund free stock value                   |
| body.MFMargin[].MFHaircutValue   | decimal  | Mutual fund haircut value                      |
| body.TimeStamp                 | datetime | Timestamp of the response                     |

---

## Error Handling

| Status Code | Meaning                  | Description                    |
|-------------|--------------------------|--------------------------------|
| 9           | Invalid Session          | Authentication failed or token expired |
| 2           | Input Validation Error   | Missing or invalid ClientCode or head key |
| 1           | Data Not Found           | No margin info available for the ClientCode |

---


## Additional Notes

- The API supports multi-layer validation: header key, authorization JWT, and client session cookies.
- The margin data returned is consolidated from multiple tables and stored procedures.
- This version (V4) enhances margin details compared to earlier versions, including derivatives and mutual fund margins.
- Always ensure your client code is validated to avoid unauthorized data access.

---

---
