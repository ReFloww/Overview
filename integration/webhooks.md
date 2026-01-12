# Webhooks

Receive real-time notifications for ReFlow events.

## Overview

Webhooks allow your application to receive push notifications when events occur in ReFlow, eliminating the need for polling.

## Setting Up Webhooks

### Register Webhook Endpoint

```bash
POST /v1/webhooks
Content-Type: application/json

{
  "url": "https://your-app.com/webhooks/reflow",
  "events": ["loan.funded", "investment.created", "repayment.received"],
  "secret": "your_webhook_secret"
}
```

Response:
```json
{
  "id": "wh_abc123",
  "url": "https://your-app.com/webhooks/reflow",
  "events": ["loan.funded", "investment.created", "repayment.received"],
  "status": "active",
  "created_at": "2025-01-15T10:00:00Z"
}
```

## Available Events

### Loan Events

| Event | Description |
|-------|-------------|
| `loan.created` | New loan listed on marketplace |
| `loan.funded` | Loan reached funding goal |
| `loan.started` | Loan term has begun |
| `loan.completed` | Loan fully repaid |
| `loan.defaulted` | Loan entered default status |

### Investment Events

| Event | Description |
|-------|-------------|
| `investment.created` | User made new investment |
| `investment.transferred` | Investment transferred to another user |
| `investment.redeemed` | Investment redeemed at maturity |

### Repayment Events

| Event | Description |
|-------|-------------|
| `repayment.received` | Repayment received from borrower |
| `repayment.distributed` | Repayment distributed to investors |

### Market Events

| Event | Description |
|-------|-------------|
| `listing.created` | New secondary market listing |
| `listing.sold` | Listing purchased |
| `listing.cancelled` | Listing cancelled |

## Webhook Payload

All webhooks follow this format:

```json
{
  "id": "evt_abc123",
  "type": "loan.funded",
  "created": "2025-01-15T10:30:00Z",
  "data": {
    "loan_id": "loan_456",
    "total_funded": "100000000000",
    "investor_count": 25
  }
}
```

## Verifying Webhooks

Verify webhook authenticity using the signature header:

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const expectedSignature = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');

  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}

// In your webhook handler
app.post('/webhooks/reflow', (req, res) => {
  const signature = req.headers['x-reflow-signature'];
  const payload = JSON.stringify(req.body);

  if (!verifyWebhook(payload, signature, process.env.WEBHOOK_SECRET)) {
    return res.status(401).send('Invalid signature');
  }

  // Process webhook
  const event = req.body;
  console.log('Received event:', event.type);

  res.status(200).send('OK');
});
```

## Retry Policy

Failed webhook deliveries are retried with exponential backoff:

| Attempt | Delay |
|---------|-------|
| 1 | Immediate |
| 2 | 1 minute |
| 3 | 5 minutes |
| 4 | 30 minutes |
| 5 | 2 hours |
| 6 | 24 hours |

After 6 failed attempts, the webhook is marked as failed and notifications are sent.

## Managing Webhooks

### List Webhooks

```bash
GET /v1/webhooks
```

### Update Webhook

```bash
PATCH /v1/webhooks/:id
Content-Type: application/json

{
  "events": ["loan.funded", "repayment.received"]
}
```

### Delete Webhook

```bash
DELETE /v1/webhooks/:id
```

## Best Practices

1. **Return 200 quickly** - Process webhooks asynchronously
2. **Handle duplicates** - Use event ID for idempotency
3. **Verify signatures** - Always validate webhook authenticity
4. **Log everything** - Keep records for debugging
5. **Set up monitoring** - Alert on webhook failures
