# Portfolio API

API endpoints for user portfolio management.

## Endpoints

### Get Portfolio

Get user's complete portfolio.

```
GET /portfolio
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": {
    "summary": {
      "totalValue": "12500",
      "investedCapital": "10000",
      "totalReturns": "2500",
      "unrealizedGains": "500",
      "activePositions": 5,
      "portfolioApy": "12.5"
    },
    "holdings": [
      {
        "loanId": "uuid",
        "tokenAddress": "0x...",
        "name": "Agricultural Loan #1",
        "tokens": "1000",
        "principal": "1000",
        "currentValue": "1050",
        "accruedInterest": "50",
        "apy": "12",
        "daysRemaining": 180,
        "status": "ACTIVE"
      }
    ]
  }
}
```

### Get Holdings

Get list of holdings only.

```
GET /portfolio/holdings
```

**Auth Required:** Yes

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status |
| sort | string | Sort order |

#### Response

```json
{
  "success": true,
  "data": [
    {
      "loanId": "uuid",
      "tokenAddress": "0x...",
      "name": "Agricultural Loan #1",
      "tokens": "1000",
      "principal": "1000",
      "currentValue": "1050",
      "apy": "12",
      "status": "ACTIVE"
    }
  ]
}
```

### Get Portfolio Summary

Get portfolio metrics summary.

```
GET /portfolio/summary
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": {
    "totalValue": "12500",
    "investedCapital": "10000",
    "totalReturns": "2500",
    "portfolioApy": "12.5",
    "activePositions": 5,
    "managedValue": "5000",
    "selfManagedValue": "7500"
  }
}
```

### Get Performance

Get portfolio performance data.

```
GET /portfolio/performance
```

**Auth Required:** Yes

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| period | string | 7d, 30d, 90d, 1y, all |

#### Response

```json
{
  "success": true,
  "data": {
    "period": "30d",
    "startValue": "11500",
    "endValue": "12500",
    "change": "1000",
    "changePercent": "8.7",
    "chartData": [
      { "date": "2024-01-01", "value": "11500" },
      { "date": "2024-01-15", "value": "12000" },
      { "date": "2024-01-30", "value": "12500" }
    ]
  }
}
```

### Get Allocation

Get portfolio allocation breakdown.

```
GET /portfolio/allocation
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": {
    "byMode": {
      "managed": { "value": "5000", "percent": "40" },
      "selfManaged": { "value": "7500", "percent": "60" }
    },
    "byGrade": {
      "A": { "value": "2000", "percent": "16" },
      "B": { "value": "5000", "percent": "40" },
      "C": { "value": "4000", "percent": "32" },
      "D": { "value": "1500", "percent": "12" }
    },
    "bySector": {
      "Agriculture": { "value": "5000", "percent": "40" },
      "SME": { "value": "4500", "percent": "36" },
      "Fisheries": { "value": "3000", "percent": "24" }
    }
  }
}
```

## Errors

| Code | Message | Description |
|------|---------|-------------|
| 401 | Unauthorized | Not authenticated |
| 404 | Portfolio not found | User has no investments |
