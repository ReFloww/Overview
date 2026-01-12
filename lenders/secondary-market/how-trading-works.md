# How Trading Works

Understanding the mechanics of secondary market trading helps you make better buying and selling decisions.

## Trading Flow

```
┌─────────┐    ┌──────────┐    ┌─────────────┐    ┌────────────┐
│  List   │ →  │  Match   │ →  │  Execute    │ →  │  Settle    │
│  Token  │    │  Buyer   │    │  Trade      │    │  On-Chain  │
└─────────┘    └──────────┘    └─────────────────────────────────┘
```

## Order Types

### Fixed Price Listing

Most common on ReFlow:
- Seller sets exact price
- Buyer accepts or moves on
- First buyer gets the tokens

### Characteristics

| Aspect | Fixed Price |
|--------|-------------|
| **Price Certainty** | Yes |
| **Speed** | Fast when matched |
| **Control** | Seller controls price |
| **Negotiation** | None |

## Pricing Dynamics

### What Affects Price

1. **Underlying Value**
   - Principal amount
   - Accrued interest
   - Time to maturity

2. **Risk Factors**
   - Loan performance
   - Borrower status
   - Sector conditions

3. **Market Conditions**
   - Buyer demand
   - Available supply
   - Platform activity

### Price Discovery

| Scenario | Typical Pricing |
|----------|-----------------|
| **Healthy loan, seller needs cash** | 95-99% of value |
| **Healthy loan, no rush** | 100-102% of value |
| **Near maturity** | Close to 100% |
| **Long remaining term** | Varies with demand |
| **Performance concerns** | Significant discount |

## Trading Process

### For Sellers

1. **Select Tokens**: Choose which tokens to sell
2. **Set Price**: Determine your asking price
3. **Create Listing**: Submit to marketplace
4. **Wait for Buyer**: Listing goes live
5. **Execute**: Trade completes automatically
6. **Receive Funds**: USDT in your wallet

### For Buyers

1. **Browse Listings**: Find available tokens
2. **Analyze**: Review loan details
3. **Decide**: Choose attractive opportunities
4. **Purchase**: Approve and buy
5. **Receive Tokens**: Now in your wallet
6. **Earn Returns**: Collect remaining payments

## Transaction Components

### Seller Side

| Item | Description |
|------|-------------|
| **Tokens Out** | Loan tokens transferred |
| **USDT In** | Sale proceeds received |
| **Fee** | Platform fee deducted |
| **Gas** | Transaction cost |

### Buyer Side

| Item | Description |
|------|-------------|
| **USDT Out** | Purchase price paid |
| **Tokens In** | Loan tokens received |
| **Gas** | Transaction cost |

## Fees

### Platform Fee

| Party | Fee |
|-------|-----|
| Seller | ~1% of sale price |
| Buyer | No platform fee |

### Gas Fees

- Both parties pay their own gas
- Mantle Network has low gas costs
- Varies with network congestion

## Settlement

### On-Chain Settlement

All trades settle on blockchain:
- Atomic swap (simultaneous)
- No counterparty risk
- Instant finality
- Verifiable on explorer

### What Happens

1. Smart contract receives USDT from buyer
2. Smart contract receives tokens from seller
3. Simultaneous exchange
4. Both parties receive assets

## Trading Strategies

### For Sellers

**Quick Exit**
- Price below market for fast sale
- Good when urgency > maximizing price

**Fair Value**
- Price at estimated fair value
- May take time to find buyer

**Premium Pricing**
- Price above fair value
- Works for in-demand loans

### For Buyers

**Value Hunting**
- Look for underpriced tokens
- Urgent sellers create opportunities

**Near Maturity**
- Buy close to maturity
- Lower duration risk
- Quick returns

**Yield Improvement**
- Find tokens with better rates
- Improve portfolio average yield

## Order Management

### Modifying Listings

1. Cancel existing listing
2. Create new listing with new terms
3. Cancellation may have waiting period

### Canceling Listings

1. Go to your active listings
2. Select listing to cancel
3. Confirm cancellation
4. Tokens returned to wallet

## Market Depth

### Understanding Liquidity

| Indicator | Meaning |
|-----------|---------|
| **Many listings** | Good liquidity, competitive pricing |
| **Few listings** | Lower liquidity, prices may vary |
| **Quick sales** | Active buyer demand |
| **Slow sales** | May need to adjust price |

## Best Practices

### Before Trading

1. Understand the loan details
2. Calculate fair value
3. Check loan performance
4. Review remaining term

### When Selling

1. Price realistically
2. Be patient for fair price
3. Consider market conditions
4. Monitor listing activity

### When Buying

1. Do due diligence
2. Don't overpay
3. Diversify purchases
4. Understand what you're buying

## Related Guides

- [Buying Loan Tokens](buying-tokens.md)
- [Selling Loan Tokens](selling-tokens.md)
- [Secondary Market Overview](README.md)
