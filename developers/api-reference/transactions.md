# Transaction History API

API endpoints for transaction history.

## Endpoints

### Get Transactions

Get user's transaction history.

```
GET /history/transactions
```

**Auth Required:** Yes

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| page | number | Page number |
| limit | number | Items per page |
| type | string | Transaction type filter |
| from | string | Start date (ISO) |
| to | string | End date (ISO) |
| asset | string | Filter by asset |

#### Response

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "uuid",
        "type": "INVESTMENT",
        "asset": "Agricultural Loan #1",
        "amount": "1000",
        "tokens": "1000",
        "txHash": "0x...",
        "status": "COMPLETED",
        "date": "2024-01-15T10:30:00Z",
        "fee": "0.5"
      },
      {
        "id": "uuid",
        "type": "DISTRIBUTION",
        "asset": "Agricultural Loan #1",
        "amount": "50",
        "txHash": "0x...",
        "status": "COMPLETED",
        "date": "2024-02-01T00:00:00Z"
      },
      {
        "id": "uuid",
        "type": "TRADE",
        "asset": "SME Loan #3",
        "amount": "500",
        "tokens": "500",
        "txHash": "0x...",
        "status": "COMPLETED",
        "date": "2024-01-20T14:20:00Z",
        "side": "BUY",
        "price": "1.00"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 45
    }
  }
}
```

### Get Transaction Details

Get details of specific transaction.

```
GET /history/transactions/:id
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "type": "INVESTMENT",
    "asset": {
      "name": "Agricultural Loan #1",
      "tokenAddress": "0x...",
      "symbol": "RFL001"
    },
    "amount": "1000",
    "tokens": "1000",
    "txHash": "0x...",
    "blockNumber": 12345678,
    "status": "COMPLETED",
    "date": "2024-01-15T10:30:00Z",
    "fee": "0.5",
    "gasUsed": "150000",
    "gasPrice": "1000000000"
  }
}
```

### Export Transactions

Export transaction history.

```
GET /history/export
```

**Auth Required:** Yes

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| format | string | csv, json, pdf |
| from | string | Start date |
| to | string | End date |
| type | string | Transaction type |

#### Response

Returns file download or:

```json
{
  "success": true,
  "data": {
    "downloadUrl": "https://...",
    "expiresAt": "2024-01-16T10:30:00Z"
  }
}
```

## Transaction Types

| Type | Description |
|------|-------------|
| `INVESTMENT` | Invested in a loan |
| `DISTRIBUTION` | Received distribution |
| `CLAIM` | Claimed distribution |
| `TRADE_BUY` | Bought tokens |
| `TRADE_SELL` | Sold tokens |
| `FUND_DEPOSIT` | Deposited to fund |
| `FUND_WITHDRAW` | Withdrew from fund |
| `APPROVAL` | Token approval |

## Transaction Status

| Status | Description |
|--------|-------------|
| `PENDING` | Processing |
| `COMPLETED` | Successfully processed |
| `FAILED` | Transaction failed |

## Date Filtering

Use ISO 8601 format:
```
from=2024-01-01T00:00:00Z
to=2024-01-31T23:59:59Z
```

## Sorting

Default: newest first

Add `sort` parameter:
- `date_desc` - Newest first
- `date_asc` - Oldest first
- `amount_desc` - Highest amount
- `amount_asc` - Lowest amount
