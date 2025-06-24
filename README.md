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

---

## 🔹 API Overview
The **NetPosition_NetWiseV3** API delivers consolidated net position data for a client across multiple exchanges and segments, helping traders compute intraday and delivery-wise positions with essential profit/loss metrics including realized PnL.

---

## 📌 Endpoint
```http
POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V3/NetPositionNetWise
```

---

## 📤 Request Format
```json
{
  "head": {
    "key": "string"
  },
  "body": {
    "ClientCode": "string"
  }
}
```

### 🔸 Request Parameters
| Field           | Type   | Description                          |
|----------------|--------|--------------------------------------|
| `head.key`     | string | Authorization token key              |
| `body.ClientCode` | string | Unique identifier for the client     |

---

## 📥 Response Format (Success)
```json
{
  "head": {
    "responseCode": "5PNPNWV3",
    "status": "0",
    "statusDescription": "Success"
  },
  "body": {
    "Status": 0,
    "Message": "",
    "NetPositionDetail": [
      {
        "Exch": "N",
        "ExchType": "D",
        "ScripCode": 62385,
        "ScripName": "NIFTY 26 Jun 2025 CE 24750.00",
        "BuyQty": 75,
        "BuyAvgRate": 316.1,
        "BuyValue": 23707.5,
        "SellQty": 75,
        "SellAvgRate": 315.95,
        "SellValue": 23696.25,
        "NetQty": 0,
        "BookedPL": -11.25,
        "LTP": 318.6,
        "OrderFor": "D",
        "BodQty": 75,
        "PreviousClose": 421.05,
        "MTOM": 0,
        "Multiplier": 1,
        "AvgRate": 0,
        "CFQty": 0,
        "AvgCFQty": 0,
        "LotSize": 75,
        "ConvertedQty": 0,
        "isPhysicalDelivery": false,
        "AvgCFPrice": 0
      }
    ]
  }
}
```

---

## ❌ Failure Response Examples
### 🔸 Invalid Session
```json
{
  "head": {
    "responseCode": "5PNPNWV3",
    "status": "0",
    "statusDescription": "Invalid Session"
  },
  "body": {
    "Status": 9,
    "Message": "Invalid Session",
    "NetPositionDetail": []
  }
}
```

### 🔸 Invalid Head Parameters
```json
{
  "head": {
    "responseCode": "5PNPNWV3",
    "status": "2",
    "statusDescription": "Invalid head parameters."
  },
  "body": null
}
```

---

## 🧾 Field Reference: NetPositionDetail
| Field              | Type    | Description                                               |
|--------------------|---------|-----------------------------------------------------------|
| `Exch`             | string  | Exchange code (e.g., N = NSE, B = BSE, M = MCX)           |
| `ExchType`         | string  | Segment code (e.g., D = Derivatives, C = Cash, U = Currency) |
| `ScripCode`        | int     | Unique code identifying the traded instrument             |
| `ScripName`        | string  | Full name of the instrument                               |
| `BuyQty`           | int     | Quantity bought                                           |
| `BuyAvgRate`       | float   | Average rate of bought quantity                           |
| `BuyValue`         | float   | Total value of buy trades                                 |
| `SellQty`          | int     | Quantity sold                                             |
| `SellAvgRate`      | float   | Average rate of sold quantity                             |
| `SellValue`        | float   | Total value of sell trades                                |
| `NetQty`           | int     | Net open position quantity                                |
| `BookedPL`         | float   | Profit/Loss realized on closed quantity      (Realized P&L)             |
| `LTP`              | float   | Last Traded Price                                         |
| `OrderFor`         | char    | Order type (D=Delivery, I=Intraday, S=BO, C=CO)           |
| `BodQty`           | int     | Quantity carried forward from previous day                |
| `PreviousClose`    | float   | Previous day's closing price                              |
| `MTOM`             | float   | Mark-to-Market P&L                                        |
| `Multiplier`       | float   | Multiplier used to scale position values                 |
| `AvgRate`          | float   | Final average holding price                               |
| `CFQty`            | float   | Carried Forward Quantity                                  |
| `AvgCFQty`         | float   | Average CF price                                          |
| `LotSize`          | float   | Lot size for futures/options                              |
| `ConvertedQty`     | int     | Quantity converted (e.g., physical delivery)              |
| `isPhysicalDelivery` | bool  | Indicates if physical delivery applies                    |
| `AvgCFPrice`       | float   | Average carried forward price                             |

---

## ✅ Status Codes
| Code | Description                         |
|------|-------------------------------------|
| 0    | Success                             |
| 1    | No record found                     |
| 2    | Invalid head/body or parameters     |
| 9    | Invalid Session                     |

---

## 🧪 Sample cURL Request
```bash
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V3/NetPositionNetWise' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <access_token>' \
--data '{
    "head": {
        "key": "<auth_key>"
    },
    "body": {
        "ClientCode": "<client_code>"
    }
}'
```

---

## 🧠 Integration Tips
- Use this API to show a snapshot of open positions and realized P&L in your trading assistant.
- Supports both equity and derivatives segments.
- Run this every few seconds (with caution to rate limits) for real-time dashboards.
- Pair with **OrderBookV2** or **HoldingsV3** for a complete client view.

---



## 📚 Notes
- LTP and MTM can be used for live P&L computation.
- Ideal for tracking both intraday and delivery positions.
- Realized P&L (`BookedPL`) is included for completed legs.
- Response includes optional field `isPhysicalDelivery` for F&O expiry settlement.

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


---
