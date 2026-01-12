# Dashboard API

API endpoints for dashboard and analytics.

## Endpoints

### Get Dashboard

Get user's dashboard data.

```
GET /dashboard
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": {
    "summary": {
      "totalValue": "12500",
      "totalReturns": "2500",
      "activeInvestments": 5,
      "pendingDistributions": "150",
      "portfolioApy": "12.5"
    },
    "performance": {
      "today": "+25",
      "week": "+150",
      "month": "+450",
      "year": "+2500"
    },
    "recentActivity": [
      {
        "type": "DISTRIBUTION",
        "amount": "50",
        "loan": "Agricultural Loan #1",
        "date": "2024-01-15T10:30:00Z"
      },
      {
        "type": "INVESTMENT",
        "amount": "1000",
        "loan": "SME Loan #5",
        "date": "2024-01-10T14:20:00Z"
      }
    ],
    "upcomingEvents": [
      {
        "type": "DISTRIBUTION",
        "loan": "Agricultural Loan #1",
        "expectedDate": "2024-02-01",
        "expectedAmount": "50"
      },
      {
        "type": "MATURITY",
        "loan": "Fisheries Loan #3",
        "date": "2024-02-15"
      }
    ]
  }
}
```

### Get Platform Stats

Get overall platform statistics.

```
GET /dashboard/stats
```

**Auth Required:** Optional

#### Response

```json
{
  "success": true,
  "data": {
    "platform": {
      "totalUsers": 1500,
      "totalVolume": "10000000",
      "activeLoans": 50,
      "completedLoans": 120,
      "totalDistributed": "2500000",
      "defaultRate": "2.5"
    },
    "averages": {
      "apy": "11.5",
      "loanSize": "15000",
      "investmentSize": "2500"
    }
  }
}
```

### Get Chart Data

Get data for dashboard charts.

```
GET /dashboard/chart
```

**Auth Required:** Yes

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| type | string | portfolio, returns, allocation |
| period | string | 7d, 30d, 90d, 1y, all |

#### Response (Portfolio Chart)

```json
{
  "success": true,
  "data": {
    "type": "portfolio",
    "period": "30d",
    "data": [
      { "date": "2024-01-01", "value": "11500" },
      { "date": "2024-01-08", "value": "11750" },
      { "date": "2024-01-15", "value": "12000" },
      { "date": "2024-01-22", "value": "12250" },
      { "date": "2024-01-29", "value": "12500" }
    ]
  }
}
```

### Get Earnings

Get earnings breakdown.

```
GET /dashboard/earnings
```

**Auth Required:** Yes

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| period | string | month, quarter, year, all |

#### Response

```json
{
  "success": true,
  "data": {
    "period": "month",
    "total": "450",
    "interest": "400",
    "tradingGains": "50",
    "fees": "-20",
    "net": "430",
    "breakdown": [
      {
        "loan": "Agricultural Loan #1",
        "amount": "150",
        "type": "INTEREST"
      },
      {
        "loan": "SME Loan #5",
        "amount": "200",
        "type": "INTEREST"
      }
    ]
  }
}
```

### Get Notifications

Get user notifications.

```
GET /dashboard/notifications
```

**Auth Required:** Yes

#### Response

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "type": "DISTRIBUTION",
      "title": "Distribution Received",
      "message": "You received $50 from Agricultural Loan #1",
      "read": false,
      "date": "2024-01-15T10:30:00Z"
    },
    {
      "id": "uuid",
      "type": "ALERT",
      "title": "Loan Maturing Soon",
      "message": "Fisheries Loan #3 matures in 30 days",
      "read": true,
      "date": "2024-01-14T09:00:00Z"
    }
  ]
}
```
