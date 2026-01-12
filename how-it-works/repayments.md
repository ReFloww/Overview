# Repayments and Distributions

Understanding how repayments flow from borrowers to your wallet is essential for managing your investments on ReFlow.

## How Repayments Work

### The Flow

```
Borrower → Restock.id → USDT Conversion → Smart Contract → Your Wallet
    ↓           ↓              ↓               ↓              ↓
  Repays    Collects      Converts        Distributes     You receive
```

### Detailed Process

1. **Borrower Makes Payment**
   - According to loan schedule
   - To Restock.id

2. **Restock.id Processes**
   - Receives payment
   - Converts to USDT
   - Sends to smart contract

3. **Smart Contract Distributes**
   - Calculates each holder's share
   - Executes transfers
   - Records on blockchain

4. **You Receive Funds**
   - USDT arrives in your wallet
   - Automatically, no action needed

## Distribution Types

### Interest Payments

Regular earnings during loan term:

```
Interest Distribution = Periodic Interest × (Your Tokens / Total Tokens)
```

| Schedule | Frequency | Common For |
|----------|-----------|------------|
| Monthly | Every month | Standard loans |
| Quarterly | Every 3 months | Longer-term |
| At maturity | End of term | Bullet loans |

### Principal Repayments

Return of original investment:

| Type | Description |
|------|-------------|
| **Bullet** | Full principal at maturity |
| **Amortizing** | Gradual principal return |
| **Mixed** | Combination of both |

## Distribution Calculation

### Formula

```
Your Distribution = Total Distribution × (Your Tokens / Total Token Supply)
```

### Example Calculation

```
Loan Details:
- Total Supply: 10,000 tokens
- APY: 12%
- Monthly Distribution: 100 USDT (interest)

Your Position:
- Holdings: 1,000 tokens (10%)

Your Distribution:
- 10% × 100 USDT = 10 USDT per month
```

### Maturity Example

```
At Maturity:
- Final interest: 100 USDT
- Principal return: 10,000 USDT
- Total: 10,100 USDT

Your Share (10%):
- Interest: 10 USDT
- Principal: 1,000 USDT
- Total: 1,010 USDT
```

## Receiving Distributions

### Automatic Process

```
Distribution occurs → USDT sent to your wallet → No action required
```

### What You'll See

1. **Notification**: Alert of distribution
2. **Wallet Balance**: USDT increased
3. **Transaction History**: New entry
4. **Portfolio Update**: Reflected in dashboard

### Checking Distributions

1. Go to **Portfolio** → **History**
2. Filter by "Distributions"
3. View all received payments

## Distribution Timing

### When to Expect

| Event | Timing |
|-------|--------|
| Borrower payment received | Day 0 |
| Processing | 1-2 days |
| Smart contract distribution | Day 2-3 |
| In your wallet | Immediate after contract |

### Factors Affecting Timing

- Borrower payment timing
- Processing time
- Network conditions
- Contract execution

## Tracking Distributions

### Dashboard View

See at a glance:
- Total received
- This month's distributions
- Upcoming expected
- Historical trend

### Detailed View

For each distribution:
- Date and time
- Amount received
- Source loan
- Transaction hash

### Projected Earnings

Platform shows:
- Expected next distribution
- Monthly projections
- Maturity returns

## Distribution Scenarios

### Scenario 1: Regular Payment

```
Loan performing normally:
Month 1: Interest distributed ✓
Month 2: Interest distributed ✓
...
Month 12: Final interest + principal ✓
```

### Scenario 2: Late Payment

```
Payment delayed:
Expected date: 15th
Actual payment: 20th
Your distribution: 22nd
Note: Slight delay, but received
```

### Scenario 3: Partial Default

```
Borrower defaults:
Principal: 10,000 USDT
Recovered (collateral): 7,000 USDT
Your share (10%): 700 USDT
Loss: 300 USDT (30%)
```

## Handling Defaults

### What Happens

1. Loan marked as default
2. Collection process begins
3. Collateral liquidated (if available)
4. Recovered funds distributed
5. Final settlement

### Recovery Distribution

```
Recovery Amount × (Your Tokens / Total Tokens) = Your Recovery
```

### Tax Implications

Defaults may result in:
- Realized losses (tax deductible in some jurisdictions)
- Need for documentation
- Consult tax professional

## Reinvesting Distributions

### Manual Reinvestment

1. Receive distribution
2. Go to Market
3. Invest in new loans
4. Compound your returns

### Compound Effect

```
Year 1: $10,000 × 12% = $1,200
Reinvested Year 2: $11,200 × 12% = $1,344
Reinvested Year 3: $12,544 × 12% = $1,505

Total after 3 years: ~$14,049
Without reinvestment: $13,600
```

## Distribution FAQs

### When do I receive my first distribution?

After the first borrower repayment is processed, typically within the first month of the loan.

### Why is my distribution different than expected?

Check:
- Your exact token holdings at distribution time
- Any fees deducted
- Rounding in calculations

### What if I sold some tokens?

Distributions go to whoever holds tokens at distribution time. If you sold, new owner receives that share.

### Are distributions taxable?

Interest income is typically taxable. Principal return is generally not (it's return of capital). Consult a tax professional.

## Related Pages

- [Loan Lifecycle](loan-lifecycle.md)
- [Tokenization Explained](tokenization.md)
- [Tracking Earnings](../lenders/portfolio/tracking-earnings.md)
