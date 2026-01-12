# How Lending Works

Understanding how P2P lending works on ReFlow helps you make informed investment decisions. This guide explains the complete lending process from start to finish.

## The Lending Lifecycle

```
Borrower Request → Loan Approval → Tokenization → Investment → Repayment → Distribution
```

### 1. Loan Origination (Restock.id)

Our partner Restock.id handles all borrower-side operations:

- **Borrower Application**: Businesses apply for loans
- **Credit Assessment**: Thorough evaluation of creditworthiness
- **Collateral Verification**: Real-world assets backing the loan
- **Approval**: Only qualified borrowers are approved

### 2. Loan Tokenization (ReFlow)

Once approved, loans are tokenized on ReFlow:

- **Smart Contract Creation**: Each loan gets a unique token contract
- **Token Minting**: Loan value converted to ERC-20 tokens
- **Listing**: Tokens available for investment on the platform

### 3. Investment (You)

You participate by purchasing loan tokens:

- **Browse Market**: View available loan products
- **Due Diligence**: Review loan details, rates, and risks
- **Invest**: Purchase tokens with USDT
- **Ownership**: Tokens represent your share of the loan

### 4. Loan Servicing

While the loan is active:

- **Borrower Repayments**: Regular payments to Restock.id
- **On-Chain Recording**: All activities logged on blockchain
- **Portfolio Tracking**: Monitor your investments in real-time

### 5. Repayment Distribution

When borrowers repay:

- **Principal Return**: Original investment returned at maturity
- **Interest Payments**: Earnings distributed according to schedule
- **Automatic Distribution**: Smart contracts handle all distributions

## Understanding Loan Products

### Loan Information

Each loan product displays:

| Field | Description |
|-------|-------------|
| **Loan Amount** | Total value being funded |
| **Interest Rate** | Annual percentage yield (APY) |
| **Duration** | Loan term (e.g., 6 months, 12 months) |
| **Risk Grade** | Credit rating (A, B, C, etc.) |
| **Sector** | Industry (Agriculture, Fisheries, etc.) |
| **Collateral** | Assets backing the loan |

### Risk Grades Explained

| Grade | Risk Level | Typical APY |
|-------|------------|-------------|
| A | Lowest | 8-10% |
| B | Low-Medium | 10-12% |
| C | Medium | 12-15% |
| D | Medium-High | 15-18% |
| E | Higher | 18%+ |

*Higher risk grades offer higher potential returns but carry more default risk.*

## Token Mechanics

### What Your Tokens Represent

- **Proportional Ownership**: Your share of the loan
- **Claim on Repayments**: Right to principal and interest
- **Tradeable Asset**: Can be sold on secondary market

### Token Value

```
Token Value = (Principal Portion) + (Accrued Interest) - (Any Defaults)
```

### Example

1. You invest 1,000 USDT in a loan
2. Receive 1,000 loan tokens
3. Loan has 12% APY over 12 months
4. At maturity: 1,000 + 120 = 1,120 USDT

## Secondary Market

Don't want to wait for maturity? Trade your tokens:

- **Sell Early**: List tokens for sale anytime
- **Buy Discounted**: Purchase tokens from other lenders
- **Price Discovery**: Market determines fair value
- **Instant Settlement**: Blockchain-based trades

Learn more in [Secondary Market](secondary-market/README.md).

## Investment Modes

ReFlow offers two ways to invest:

### Managed Mode
- Fund managers handle investment decisions
- Automatic diversification
- Passive income approach
- [Learn more](investment-modes/managed-mode.md)

### Self-Managed Mode
- You choose each loan
- Full control over portfolio
- Active management required
- [Learn more](investment-modes/self-managed-mode.md)

## Key Considerations

### Returns
- Returns depend on loan performance
- Higher risk grades = higher potential returns
- Diversification reduces individual loan risk

### Risks
- Borrowers may default
- Secondary market liquidity not guaranteed
- See [Risks and Considerations](risks.md)

### Timing
- Loans have fixed durations
- Early exit only via secondary market
- Plan investments according to your needs

## Next Steps

- [Getting Started as a Lender](getting-started.md)
- [Investment Modes](investment-modes/README.md)
- [Understanding Your Portfolio](portfolio/README.md)
