# Loan Lifecycle

Understanding the complete journey of a loan on ReFlow helps you make better investment decisions. This guide covers every stage from origination to completion.

## Lifecycle Overview

```
ORIGINATION → TOKENIZATION → FUNDING → ACTIVE → REPAYMENT → COMPLETION
     ↓              ↓            ↓         ↓          ↓            ↓
 Restock.id     ReFlow       Investors   Trading   Distribution  Token Burn
```

## Stage 1: Loan Origination

### What Happens

Restock.id handles all borrower-side activities:

```
Borrower Applies → Credit Assessment → Approval → Loan Terms Set
```

### Process Details

| Step | Description | Duration |
|------|-------------|----------|
| Application | Borrower submits request | 1 day |
| Documentation | Required docs collected | 1-3 days |
| Assessment | Credit & collateral review | 2-5 days |
| Approval | Decision made | 1 day |
| Terms | Rate, amount, duration set | Same day |

### What's Evaluated

- Creditworthiness of borrower
- Business viability
- Collateral value and type
- Sector risk
- Repayment capacity

### Outcome

An approved loan with defined terms:
- Principal amount
- Interest rate (APY)
- Duration
- Repayment schedule
- Collateral details
- Risk grade assignment

## Stage 2: Tokenization

### What Happens

ReFlow converts the approved loan into blockchain tokens:

```
Approved Loan → Smart Contract Deployment → Token Minting → Market Listing
```

### Process Details

| Step | Description | Automated |
|------|-------------|-----------|
| Contract Creation | Loan-specific token contract | Yes |
| Token Minting | Create loan tokens | Yes |
| Parameter Setting | Rate, duration encoded | Yes |
| Listing | Available on marketplace | Yes |

### Token Properties Set

- Token name and symbol
- Total supply (= loan amount)
- Interest rate
- Maturity date
- Distribution schedule

## Stage 3: Funding

### What Happens

Investors purchase loan tokens:

```
Loan Listed → Investors Browse → Investment Made → Tokens Distributed
```

### Funding Period

| Scenario | Outcome |
|----------|---------|
| Fully funded | Proceeds to Active stage |
| Partially funded | May proceed or extend |
| Not funded | May be cancelled |

### What Investors See

- Loan amount and funding progress
- Interest rate offered
- Duration
- Risk grade
- Sector and collateral info
- Minimum investment

### Investment Process

1. Review loan details
2. Decide investment amount
3. Approve USDT spending
4. Execute purchase
5. Receive tokens

## Stage 4: Active Period

### What Happens

The loan is disbursed and borrower makes repayments:

```
Funds to Borrower → Borrower Uses Capital → Regular Repayments Begin
```

### During Active Period

| Activity | Description |
|----------|-------------|
| **Borrower Payments** | Per agreed schedule |
| **Interest Accrual** | Daily, based on APY |
| **Token Trading** | Available on secondary market |
| **Portfolio Tracking** | Real-time updates |

### Token Holder Rights

During this period, token holders can:
- Hold and earn distributions
- Trade on secondary market
- Monitor loan performance
- View accrued interest

### Timeline Example

```
Month 1: First repayment distributed
Month 2: Second repayment distributed
...
Month 11: Penultimate repayment
Month 12: Final repayment + principal
```

## Stage 5: Repayment Distribution

### What Happens

As borrower makes payments, token holders receive distributions:

```
Borrower Payment → Platform Receives → Smart Contract → Token Holders
```

### Distribution Process

| Step | Description |
|------|-------------|
| Payment Received | From borrower via Restock.id |
| Conversion | To USDT |
| Contract Execution | Smart contract processes |
| Distribution | Proportional to holdings |
| Notification | Token holders alerted |

### Distribution Calculation

```
Your Share = (Your Tokens / Total Tokens) × Distribution Amount
```

### Example

```
Total Tokens: 100,000
Your Tokens: 5,000 (5%)
Distribution: 2,000 USDT
Your Share: 5% × 2,000 = 100 USDT
```

## Stage 6: Completion

### Normal Completion

When the loan reaches maturity:

```
Final Repayment → Principal Returned → Tokens Burned → Loan Closed
```

### Process

| Step | Description |
|------|-------------|
| Final Payment | Last interest + principal |
| Distribution | All token holders receive |
| Token Burn | Tokens removed from supply |
| Contract Close | Loan marked complete |

### What You Receive

At completion:
- Final interest payment
- Full principal return
- Tokens removed from wallet

## Special Scenarios

### Early Repayment

If borrower repays early:
- Principal returned ahead of schedule
- May include prepayment premium
- Tokens burned early

### Late Payment

If borrower is late:
- Loan status updated
- Collection process begins
- Token holders notified
- May affect secondary market value

### Default

If borrower defaults:
- Loan marked as defaulted
- Collateral liquidation initiated
- Partial recovery possible
- Distributed to token holders

## Status Indicators

### Loan Statuses

| Status | Meaning |
|--------|---------|
| **Funding** | Open for investment |
| **Active** | Loan disbursed, repaying |
| **Current** | Payments on schedule |
| **Late** | Payment overdue |
| **Default** | Significant delinquency |
| **Completed** | Fully repaid |
| **Closed** | Finalized |

### Status Timeline

```
Funding → Active → Current → Completed → Closed
              ↓
           Late → Default (if unresolved)
              ↓
           Current (if resolved)
```

## Lifecycle Duration

### Typical Timelines

| Loan Term | Example Schedule |
|-----------|------------------|
| 3 months | Weekly/monthly repayments |
| 6 months | Monthly repayments |
| 12 months | Monthly repayments |

### From Your Perspective

```
Investment made: Day 0
First distribution: Month 1
...
Final distribution: Month N
Funds fully returned: Month N
```

## Related Pages

- [Tokenization Explained](tokenization.md)
- [Repayments and Distributions](repayments.md)
- [How Lending Works](../lenders/how-lending-works.md)
