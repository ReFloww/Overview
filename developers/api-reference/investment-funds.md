# Investment Funds API

API endpoints for fund managers (Managed Mode).

## Endpoints

### List Funds

Get all available fund managers.

```
GET /investment-funds/list
```

**Auth Required:** No

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| page | number | Page number |
| limit | number | Items per page |
| strategy | string | Filter by strategy |
| sort | string | Sort order |

#### Response

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "uuid",
        "address": "0x...",
        "name": "Conservative Growth Fund",
        "manager": "Fund Manager A",
        "strategy": "Conservative",
        "aum": "500000",
        "performance": {
          "apy": "10.5",
          "monthlyReturn": "0.85",
          "yearlyReturn": "10.5"
        },
        "fees": {
          "management": "1.5",
          "performance": "15"
        },
        "investorCount": 150,
        "minInvestment": "100"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 15
    }
  }
}
```

### Get Fund Details

Get detailed fund information.

```
GET /investment-funds/:id
```

**Auth Required:** No

#### Response

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "address": "0x...",
    "name": "Conservative Growth Fund",
    "manager": "Fund Manager A",
    "description": "A conservative fund focusing on Grade A and B loans...",
    "strategy": "Conservative",
    "aum": "500000",
    "performance": {
      "apy": "10.5",
      "monthlyReturn": "0.85",
      "yearlyReturn": "10.5",
      "allTimeReturn": "25.5",
      "maxDrawdown": "-2.5"
    },
    "fees": {
      "management": "1.5",
      "performance": "15",
      "entry": "0",
      "exit": "0.5"
    },
    "allocation": {
      "byGrade": {
        "A": "40",
        "B": "40",
        "C": "20"
      },
      "bySector": {
        "Agriculture": "50",
        "SME": "30",
        "Fisheries": "20"
      }
    },
    "holdings": [
      {
        "loan": "Agricultural Loan #1",
        "value": "50000",
        "percent": "10"
      }
    ],
    "investorCount": 150,
    "minInvestment": "100",
    "createdAt": "2023-06-01"
  }
}
```

### Get Fund Performance

Get historical performance data.

```
GET /investment-funds/:id/performance
```

**Auth Required:** No

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| period | string | 7d, 30d, 90d, 1y, all |

#### Response

```json
{
  "success": true,
  "data": {
    "period": "1y",
    "returns": "10.5",
    "benchmark": "9.0",
    "alpha": "1.5",
    "chartData": [
      { "date": "2023-01", "value": "100" },
      { "date": "2023-06", "value": "105" },
      { "date": "2024-01", "value": "110.5" }
    ],
    "metrics": {
      "sharpeRatio": "1.8",
      "volatility": "3.2",
      "maxDrawdown": "-2.5"
    }
  }
}
```

### Get User's Fund Investments

Get user's investments in funds.

```
GET /investment-funds/my-investments
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": [
    {
      "fundId": "uuid",
      "fundName": "Conservative Growth Fund",
      "invested": "5000",
      "currentValue": "5250",
      "shares": "5000",
      "returns": "250",
      "apy": "10.5",
      "investedDate": "2023-10-01"
    }
  ]
}
```

## Strategies

Available fund strategies:
- `Conservative` - Lower risk, stable returns
- `Balanced` - Mix of risk levels
- `Growth` - Higher risk, higher potential returns
- `Sector-Specific` - Focus on specific sectors

## Sorting Options

- `apy_desc` - Highest APY first
- `aum_desc` - Largest funds first
- `newest` - Most recent first
- `popular` - Most investors first
