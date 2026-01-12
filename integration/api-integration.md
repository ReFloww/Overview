# API Integration

Integrate ReFlow functionality into your application via REST API.

## Authentication

All API requests require authentication via JWT token.

### Get Access Token

```bash
curl -X POST https://api.reflow.io/v1/auth/token \
  -H "Content-Type: application/json" \
  -d '{
    "client_id": "your_client_id",
    "client_secret": "your_client_secret"
  }'
```

Response:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Using the Token

```bash
curl https://api.reflow.io/v1/loans \
  -H "Authorization: Bearer your_access_token"
```

## Core Endpoints

### List Available Loans

```bash
GET /v1/market/loans
```

Query parameters:
- `status` - Filter by status (active, funded, completed)
- `minRate` - Minimum interest rate
- `maxRate` - Maximum interest rate
- `page` - Page number
- `limit` - Items per page

### Get Loan Details

```bash
GET /v1/market/loans/:id
```

### Create Investment

```bash
POST /v1/investments
Content-Type: application/json

{
  "loanId": "loan_123",
  "amount": "1000000000"
}
```

### Get User Portfolio

```bash
GET /v1/portfolio
```

## Webhooks

Register webhooks to receive real-time updates. See [Webhooks Guide](webhooks.md).

## Rate Limits

| Tier | Requests/minute | Requests/day |
|------|-----------------|--------------|
| Standard | 60 | 10,000 |
| Professional | 300 | 50,000 |
| Enterprise | 1,000 | Unlimited |

## Error Handling

All errors follow standard format:

```json
{
  "error": {
    "code": "INSUFFICIENT_BALANCE",
    "message": "Insufficient USDT balance for this operation",
    "details": {
      "required": "1000000000",
      "available": "500000000"
    }
  }
}
```

### Common Error Codes

| Code | Description |
|------|-------------|
| `UNAUTHORIZED` | Invalid or expired token |
| `INSUFFICIENT_BALANCE` | Not enough funds |
| `LOAN_NOT_FOUND` | Loan doesn't exist |
| `LOAN_FULLY_FUNDED` | Loan already at capacity |
| `INVALID_AMOUNT` | Amount out of valid range |

## SDKs

Official SDKs available:

- **JavaScript/TypeScript**: `npm install @reflow/sdk`
- **Python**: `pip install reflow-sdk`

### JavaScript Example

```javascript
import { ReFlowClient } from '@reflow/sdk';

const client = new ReFlowClient({
  clientId: 'your_client_id',
  clientSecret: 'your_client_secret'
});

// Get loans
const loans = await client.market.getLoans({ status: 'active' });

// Make investment
const investment = await client.invest({
  loanId: 'loan_123',
  amount: '1000000000'
});
```

## Sandbox Environment

Test your integration in sandbox:

- Base URL: `https://sandbox.api.reflow.io/v1`
- Use testnet wallet for transactions
- No real funds required
