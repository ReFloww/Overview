# Tokenization Explained

Tokenization is the core innovation that makes ReFlow possible. This guide explains what tokenization is, how it works, and why it matters.

## What is Tokenization?

Tokenization converts real-world assets (in our case, P2P loans) into digital tokens on a blockchain.

```
Real World Asset          Digital Token
(P2P Loan)        →       (ERC-20 Token)
     ↓                         ↓
Paper/database record     Blockchain record
Illiquid                  Tradeable
Platform-controlled       Self-custodied
Opaque                    Transparent
```

## How Loan Tokenization Works

### The Process

```
1. LOAN APPROVED
   Restock.id approves a $10,000 loan at 12% APY for 12 months

2. SMART CONTRACT DEPLOYED
   ReFlow deploys a new ERC-20 contract for this specific loan

3. TOKENS MINTED
   10,000 tokens created (1 token = $1 of loan value)

4. TOKENS DISTRIBUTED
   Investors purchase tokens, receiving proportional ownership

5. TOKENS REPRESENT CLAIMS
   Each token = right to proportional repayments
```

### What Tokens Represent

Each loan token represents:

| Attribute | Representation |
|-----------|----------------|
| **Ownership** | Fractional share of the loan |
| **Income Rights** | Claim on interest payments |
| **Principal Rights** | Claim on principal repayment |
| **Transferability** | Can be sold or transferred |

## Token Structure

### Token Standard: ERC-20

ReFlow loan tokens use the ERC-20 standard:

- **Fungible**: All tokens for a loan are identical
- **Divisible**: Can own fractional tokens
- **Transferable**: Send to any address
- **Compatible**: Works with all ERC-20 tools

### Token Properties

| Property | Description |
|----------|-------------|
| **Name** | Identifies the loan (e.g., "ReFlow Loan #1234") |
| **Symbol** | Short code (e.g., "RFL1234") |
| **Total Supply** | Equal to loan amount |
| **Decimals** | Typically 18 (standard) |

### Token Data

Additional information stored:

```
{
  loanId: "1234",
  principal: 10000,
  interestRate: 12,
  maturityDate: "2025-06-15",
  riskGrade: "B",
  sector: "Agriculture"
}
```

## Benefits of Tokenization

### 1. Liquidity

```
Traditional:
Investment → [Locked 12 months] → Return

Tokenized:
Investment → [Trade anytime] → Return when ready
```

### 2. Fractional Ownership

```
Traditional:
Minimum investment: $1,000

Tokenized:
Buy any amount in dollar increments
```

### 3. Transparency

```
Traditional:
Trust platform reports

Tokenized:
Verify on blockchain:
- Who owns what
- When transactions happened
- Distribution history
```

### 4. Automation

```
Traditional:
Manual distribution, reconciliation

Tokenized:
Smart contract automates:
- Distribution calculations
- Proportional payments
- Record keeping
```

### 5. Composability

Tokenized assets can potentially:
- Be used as collateral
- Integrate with DeFi protocols
- Enable new financial products

## Token Lifecycle

### 1. Minting

When a loan is funded:
```
Loan Approved → Contract Deployed → Tokens Minted → Distributed to Buyers
```

### 2. Circulation

During loan term:
```
Tokens in circulation:
- Held by investors
- Traded on secondary market
- Earning distributions
```

### 3. Burning

At loan completion:
```
Final Repayment → Principal Distributed → Tokens Burned → Supply = 0
```

## How Distributions Work

### The Mechanism

```
1. Borrower repays to Restock.id
2. Funds converted to USDT
3. Smart contract receives USDT
4. Contract calculates shares:
   - Your tokens / Total tokens = Your share
5. USDT distributed to all holders
```

### Example

```
Total Supply: 10,000 tokens
Your Holdings: 500 tokens (5%)
Distribution: 1,000 USDT

Your Share: 5% × 1,000 = 50 USDT
```

## Security Considerations

### Smart Contract Security

| Measure | Purpose |
|---------|---------|
| **Audits** | Independent code review |
| **Testing** | Extensive test coverage |
| **Upgradability** | Ability to fix issues |
| **Access Control** | Protected functions |

### Token Security

| Aspect | Protection |
|--------|------------|
| **Ownership** | Wallet private keys |
| **Transfers** | Requires signature |
| **Distributions** | Automatic, trustless |

## Technical Implementation

### Smart Contract Architecture

```
Factory Contract (deploys loan contracts)
        ↓
Loan Token Contract (ERC-20 + Distribution)
        ↓
Individual token instances
```

### Key Functions

```solidity
// Standard ERC-20
balanceOf(address) → token balance
transfer(to, amount) → move tokens

// ReFlow specific
claimDistribution() → claim owed payments
getAccruedInterest() → view earnings
```

## Tokenization vs. Traditional

| Aspect | Traditional P2P | Tokenized |
|--------|-----------------|-----------|
| Ownership proof | Platform database | Blockchain |
| Transferability | None/limited | Instant |
| Divisibility | Fixed amounts | Any amount |
| Transparency | Platform reports | On-chain |
| Custody | Platform | Self |
| Automation | Manual | Smart contract |
| Composability | None | DeFi-ready |

## Understanding Token Value

### Value Components

```
Token Value = (Principal Share) + (Accrued Interest) - (Default Risk)
```

### Price Factors

| Factor | Impact |
|--------|--------|
| Time to maturity | Shorter = more certain |
| Loan performance | Current = higher value |
| Market conditions | Supply/demand |
| Interest rate | Higher rate = more value |
| Risk grade | Higher grade = lower risk premium |

## Future Possibilities

Tokenization enables:
- Cross-platform trading
- DeFi integration
- Automated portfolio management
- New derivative products
- Broader market access

## Related Pages

- [Loan Lifecycle](loan-lifecycle.md)
- [Repayments and Distributions](repayments.md)
- [Smart Contract Overview](../contracts/overview.md)
