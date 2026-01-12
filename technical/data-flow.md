# Data Flow

Detailed documentation of how data flows through the ReFlow platform across all components.

## High-Level Data Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│                           DATA FLOW OVERVIEW                              │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│   USER                 FRONTEND              BACKEND           BLOCKCHAIN │
│    │                      │                    │                    │    │
│    │──── Action ─────────▶│                    │                    │    │
│    │                      │──── API Call ─────▶│                    │    │
│    │                      │                    │──── Read ─────────▶│    │
│    │                      │                    │◀─── Data ──────────│    │
│    │                      │◀─── Response ──────│                    │    │
│    │                      │                    │                    │    │
│    │                      │──── Transaction ───────────────────────▶│    │
│    │◀─── Confirmation ────│◀──────────────────────────────────────│    │
│    │                      │                    │                    │    │
└──────────────────────────────────────────────────────────────────────────┘
```

## User Authentication Flow

```
┌─────────┐      ┌─────────┐      ┌─────────┐      ┌──────────────┐
│  User   │      │Frontend │      │ Backend │      │  Blockchain  │
└────┬────┘      └────┬────┘      └────┬────┘      └──────┬───────┘
     │                │                │                   │
     │ Connect Wallet │                │                   │
     │───────────────▶│                │                   │
     │                │                │                   │
     │                │ Request Nonce  │                   │
     │                │───────────────▶│                   │
     │                │                │                   │
     │                │◀──────────────│                   │
     │                │  Return Nonce  │                   │
     │                │                │                   │
     │  Sign Message  │                │                   │
     │◀───────────────│                │                   │
     │                │                │                   │
     │ Return Signature                │                   │
     │───────────────▶│                │                   │
     │                │                │                   │
     │                │ Submit Auth    │                   │
     │                │───────────────▶│                   │
     │                │                │  Verify Signature │
     │                │                │──────────────────▶│
     │                │                │◀──────────────────│
     │                │                │                   │
     │                │◀──────────────│                   │
     │                │  JWT Tokens    │                   │
     │ Authenticated  │                │                   │
     │◀───────────────│                │                   │
     │                │                │                   │
```

## Investment Flow

```
┌─────────┐      ┌─────────┐      ┌─────────┐      ┌──────────────┐
│  User   │      │Frontend │      │ Backend │      │Smart Contract│
└────┬────┘      └────┬────┘      └────┬────┘      └──────┬───────┘
     │                │                │                   │
     │ Select Loan    │                │                   │
     │───────────────▶│                │                   │
     │                │ Get Loan Data  │                   │
     │                │───────────────▶│                   │
     │                │◀──────────────│                   │
     │                │                │                   │
     │ Enter Amount   │                │                   │
     │───────────────▶│                │                   │
     │                │                │                   │
     │                │ Check Approval │                   │
     │                │──────────────────────────────────▶│
     │                │◀──────────────────────────────────│
     │                │                │                   │
     │  (If needed)   │                │                   │
     │  Approve USDT  │                │                   │
     │◀───────────────│                │                   │
     │───────────────▶│──────────────────────────────────▶│
     │                │◀──────────────────────────────────│
     │                │                │                   │
     │ Confirm Invest │                │                   │
     │◀───────────────│                │                   │
     │───────────────▶│                │                   │
     │                │ invest(amount) │                   │
     │                │──────────────────────────────────▶│
     │                │                │                   │
     │                │                │   Process Invest  │
     │                │                │  ┌───────────────┐│
     │                │                │  │Transfer USDT  ││
     │                │                │  │Mint Tokens    ││
     │                │                │  │Emit Event     ││
     │                │                │  └───────────────┘│
     │                │◀──────────────────────────────────│
     │                │   Tx Receipt   │                   │
     │                │                │                   │
     │                │ Record         │                   │
     │                │───────────────▶│                   │
     │                │                │ Save to DB       │
     │                │◀──────────────│                   │
     │  Investment    │                │                   │
     │  Confirmed     │                │                   │
     │◀───────────────│                │                   │
```

## Distribution Flow

```
┌──────────────┐   ┌─────────┐   ┌─────────┐   ┌──────────────┐
│ Restock.id   │   │ Backend │   │Contract │   │ Token Holders│
└──────┬───────┘   └────┬────┘   └────┬────┘   └──────┬───────┘
       │                │             │                │
       │ Repayment      │             │                │
       │ Notification   │             │                │
       │───────────────▶│             │                │
       │                │             │                │
       │                │ Transfer    │                │
       │                │ USDT        │                │
       │                │────────────▶│                │
       │                │             │                │
       │                │ distribute()│                │
       │                │────────────▶│                │
       │                │             │                │
       │                │             │  Calculate     │
       │                │             │  Per-Token     │
       │                │             │  Amount        │
       │                │             │                │
       │                │             │  Record        │
       │                │             │  Distribution  │
       │                │             │                │
       │                │◀────────────│                │
       │                │ Tx Receipt  │                │
       │                │             │                │
       │                │ Update DB   │                │
       │                │             │                │
       │                │             │                │
       │                │             │ Notify Users   │
       │                │─────────────────────────────▶│
       │                │             │                │
       │                │             │                │
       │                │             │ claimDistribution()
       │                │             │◀───────────────│
       │                │             │                │
       │                │             │ Transfer USDT  │
       │                │             │───────────────▶│
       │                │             │                │
```

## Portfolio Data Flow

```
┌─────────┐      ┌─────────┐      ┌─────────┐      ┌──────────────┐
│  User   │      │Frontend │      │ Backend │      │  Blockchain  │
└────┬────┘      └────┬────┘      └────┬────┘      └──────┬───────┘
     │                │                │                   │
     │ View Portfolio │                │                   │
     │───────────────▶│                │                   │
     │                │                │                   │
     │                │ Get Holdings   │                   │
     │                │───────────────▶│                   │
     │                │                │                   │
     │                │                │ Query Balances    │
     │                │                │──────────────────▶│
     │                │                │◀──────────────────│
     │                │                │                   │
     │                │                │ Get Loan Details  │
     │                │                │──────────────────▶│
     │                │                │◀──────────────────│
     │                │                │                   │
     │                │                │ Calculate Values  │
     │                │                │                   │
     │                │◀──────────────│                   │
     │                │  Holdings +    │                   │
     │                │  Valuations    │                   │
     │                │                │                   │
     │ Display        │                │                   │
     │ Portfolio      │                │                   │
     │◀───────────────│                │                   │
```

## Real-Time Updates

### Event Subscription

```
┌─────────┐      ┌─────────┐      ┌─────────┐      ┌──────────────┐
│Frontend │      │ Backend │      │  Event  │      │  Blockchain  │
│         │      │         │      │ Service │      │              │
└────┬────┘      └────┬────┘      └────┬────┘      └──────┬───────┘
     │                │                │                   │
     │                │                │ Subscribe Events  │
     │                │                │──────────────────▶│
     │                │                │                   │
     │                │                │                   │
     │                │                │     New Event     │
     │                │                │◀──────────────────│
     │                │                │                   │
     │                │◀──────────────│                   │
     │                │  Process Event │                   │
     │                │                │                   │
     │◀──────────────│                │                   │
     │ Push Update    │                │                   │
     │ (WebSocket)    │                │                   │
```

## Data Storage

### Database Records

```
Investment Created:
┌─────────────────────────────────────────────┐
│ investments                                  │
├─────────────────────────────────────────────┤
│ id: uuid                                    │
│ user_id: foreign key                        │
│ loan_id: foreign key                        │
│ amount: decimal                             │
│ tokens: decimal                             │
│ tx_hash: string                             │
│ created_at: timestamp                       │
└─────────────────────────────────────────────┘

Transaction Recorded:
┌─────────────────────────────────────────────┐
│ transactions                                 │
├─────────────────────────────────────────────┤
│ id: uuid                                    │
│ user_id: foreign key                        │
│ type: enum (INVEST, DISTRIBUTE, TRADE)      │
│ amount: decimal                             │
│ tx_hash: string                             │
│ status: enum (PENDING, CONFIRMED, FAILED)   │
│ created_at: timestamp                       │
└─────────────────────────────────────────────┘
```

### Blockchain State

```
On-Chain Data:
┌─────────────────────────────────────────────┐
│ Loan Token Contract                          │
├─────────────────────────────────────────────┤
│ balances: mapping(address => uint256)       │
│ totalSupply: uint256                        │
│ distributions: Distribution[]               │
│ claimed: mapping(address => uint256)        │
└─────────────────────────────────────────────┘
```

## Data Sync Strategy

### Source of Truth

| Data Type | Primary Source |
|-----------|----------------|
| Token Balances | Blockchain |
| Loan Details | Blockchain |
| User Profiles | Database |
| Transaction History | Blockchain + Database |
| Analytics | Database (cached) |

### Sync Mechanisms

```
Blockchain Events → Backend Listener → Database Update → Frontend Cache Invalidation
```

## Related Pages

- [Architecture Overview](architecture-overview.md)
- [Backend Services](backend-services.md)
- [Blockchain Layer](blockchain-layer.md)
