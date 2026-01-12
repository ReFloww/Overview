# Market API

API endpoints for the loan marketplace.

## Endpoints

### List Loans

Get all available loans.

```
GET /market/list
```

**Auth Required:** No

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| page | number | Page number (default: 1) |
| limit | number | Items per page (default: 10) |
| grade | string | Filter by grade (A, B, C, D, E) |
| sector | string | Filter by sector |
| minRate | number | Minimum APY |
| maxRate | number | Maximum APY |
| status | string | Loan status |

#### Response

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "uuid",
        "tokenAddress": "0x...",
        "name": "Agricultural Loan #1",
        "symbol": "RFL001",
        "principal": "10000",
        "interestRate": "12",
        "duration": 365,
        "maturityDate": "2025-01-01T00:00:00Z",
        "riskGrade": "B",
        "sector": "Agriculture",
        "totalInvested": "7500",
        "available": "2500",
        "status": "ACTIVE"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 50,
      "pages": 5
    }
  }
}
```

### Get Loan Details

Get detailed information about a specific loan.

```
GET /market/:id
```

**Auth Required:** No

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| id | string | Loan ID or token address |

#### Response

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "tokenAddress": "0x...",
    "name": "Agricultural Loan #1",
    "symbol": "RFL001",
    "principal": "10000",
    "interestRate": "12",
    "duration": 365,
    "maturityDate": "2025-01-01T00:00:00Z",
    "startDate": "2024-01-01T00:00:00Z",
    "riskGrade": "B",
    "sector": "Agriculture",
    "collateralType": "Inventory",
    "borrowerInfo": {
      "sector": "Agriculture",
      "region": "East Java"
    },
    "totalInvested": "7500",
    "available": "2500",
    "investorCount": 15,
    "status": "ACTIVE",
    "distributions": [
      {
        "date": "2024-02-01",
        "amount": "100",
        "type": "INTEREST"
      }
    ]
  }
}
```

### Get Market Stats

Get overall market statistics.

```
GET /market/stats
```

**Auth Required:** No

#### Response

```json
{
  "success": true,
  "data": {
    "totalLoans": 50,
    "activeLoans": 35,
    "totalVolume": "5000000",
    "totalInvested": "4500000",
    "averageApy": "11.5",
    "byGrade": {
      "A": 10,
      "B": 15,
      "C": 12,
      "D": 8,
      "E": 5
    },
    "bySector": {
      "Agriculture": 20,
      "SME": 15,
      "Fisheries": 10,
      "Other": 5
    }
  }
}
```

## Filters

### Grade Filter

Filter loans by risk grade:
- `A` - Lowest risk
- `B` - Low-medium risk
- `C` - Medium risk
- `D` - Medium-high risk
- `E` - Higher risk

### Sector Filter

Available sectors:
- `Agriculture`
- `Fisheries`
- `Forestry`
- `SME`
- `Other`

### Status Filter

- `FUNDING` - Open for investment
- `ACTIVE` - Fully funded, active
- `COMPLETED` - Loan repaid
- `DEFAULTED` - Loan defaulted

## Sorting

Add `sort` parameter:
- `rate_desc` - Highest APY first
- `rate_asc` - Lowest APY first
- `newest` - Most recent first
- `ending_soon` - Nearest maturity first
