# Platform Overview

ReFlow is a decentralized platform that bridges traditional P2P lending with blockchain technology. This overview explains how all the pieces work together.

## Platform Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         REFLOW PLATFORM                          │
├────────────────────┬────────────────────┬───────────────────────┤
│    LENDERS         │    PLATFORM        │    LOAN ORIGINATION   │
│    (You)           │    (ReFlow)        │    (Restock.id)       │
├────────────────────┼────────────────────┼───────────────────────┤
│ • Connect Wallet   │ • Tokenization     │ • Borrower Vetting    │
│ • Complete KYC     │ • Smart Contracts  │ • Loan Underwriting   │
│ • Invest in Loans  │ • Secondary Market │ • Collection          │
│ • Trade Tokens     │ • Portfolio Tools  │ • Regulatory Comp.    │
│ • Track Returns    │ • Analytics        │ • Asset Backing       │
└────────────────────┴────────────────────┴───────────────────────┘
                              │
                    ┌─────────▼─────────┐
                    │   MANTLE NETWORK   │
                    │   (Blockchain)     │
                    │   • Transactions   │
                    │   • Token Storage  │
                    │   • Transparency   │
                    └───────────────────┘
```

## Key Components

### 1. Lending Layer

Where you, the lender, interact:

| Component | Function |
|-----------|----------|
| **Web App** | User interface for all operations |
| **Wallet Connection** | Your gateway to the blockchain |
| **Portfolio Dashboard** | Track investments and returns |
| **Market Interface** | Browse and invest in loans |
| **Secondary Market** | Trade loan tokens |

### 2. Platform Layer

ReFlow's core infrastructure:

| Component | Function |
|-----------|----------|
| **Tokenization Engine** | Converts loans to tokens |
| **Smart Contracts** | Automates all operations |
| **Backend Services** | APIs, data, and processing |
| **Analytics Engine** | Performance metrics |
| **KYC System** | User verification |

### 3. Loan Origination Layer

Restock.id handles:

| Component | Function |
|-----------|----------|
| **Borrower Management** | Applications, vetting |
| **Credit Assessment** | Risk evaluation |
| **Loan Servicing** | Disbursement, collection |
| **Asset Management** | Collateral handling |
| **Regulatory Compliance** | OJK requirements |

### 4. Blockchain Layer

Mantle Network provides:

| Component | Function |
|-----------|----------|
| **Transaction Processing** | Fast, low-cost transactions |
| **Token Standards** | ERC-20 loan tokens |
| **Immutability** | Permanent record keeping |
| **Decentralization** | No single point of failure |

## Platform Flow

### Investment Flow

```
1. Lender → Connects wallet and deposits USDT
2. Lender → Browses available loans
3. Lender → Invests in chosen loan
4. Platform → Mints loan tokens to lender
5. Borrower → Receives funds (via Restock.id)
6. Borrower → Makes repayments
7. Platform → Distributes to token holders
8. Lender → Receives principal + interest
```

### Token Flow

```
Loan Created → Tokens Minted → Distributed to Investors
                    ↓
            [Loan Active Period]
                    ↓
Interest Paid ←→ Tokens Tradeable on Secondary Market
                    ↓
            [Loan Matures]
                    ↓
Principal Returned → Tokens Burned
```

## Platform Features

### Core Features

| Feature | Description |
|---------|-------------|
| **Tokenized Loans** | P2P loans as tradeable tokens |
| **Secondary Market** | Trade tokens before maturity |
| **Dual Modes** | Managed and self-managed investing |
| **Real-Time Tracking** | Portfolio and performance data |
| **Automated Distributions** | Smart contract-based payouts |

### Security Features

| Feature | Description |
|---------|-------------|
| **Non-Custodial** | You control your wallet |
| **KYC Verification** | Verified users only |
| **Smart Contract Audits** | Security verified |
| **On-Chain Transparency** | All actions verifiable |

### User Experience

| Feature | Description |
|---------|-------------|
| **Modern Interface** | Clean, intuitive design |
| **Mobile Responsive** | Works on all devices |
| **Wallet Integration** | Seamless Web3 connection |
| **Notification System** | Stay informed |

## User Roles

### Lenders (Investors)

You can:
- Invest in loan products
- Trade on secondary market
- Track portfolio performance
- Receive automated distributions
- Choose investment mode

### Fund Managers

Professional managers:
- Create managed funds
- Invest on behalf of lenders
- Charge management fees
- Provide diversification

### Borrowers (via Restock.id)

Borrowers:
- Apply through Restock.id
- Provide collateral
- Make repayments
- Never interact directly with ReFlow

## Technology Stack

### Frontend
- Next.js for web application
- Web3 wallet integration
- Real-time updates

### Backend
- NestJS API services
- PostgreSQL database
- Blockchain integration

### Blockchain
- Mantle Network (L2)
- ERC-20 token standard
- Smart contract automation

## Platform Benefits

### For Lenders

| Benefit | Traditional P2P | ReFlow |
|---------|-----------------|--------|
| Liquidity | Locked | Tradeable |
| Transparency | Platform reports | On-chain |
| Control | Platform-held | Self-custody |
| Access | Limited hours | 24/7 |

### For the Ecosystem

- Increased P2P lending adoption
- Better capital efficiency
- Regulatory compliance
- Financial inclusion

## Related Pages

- [The Problem We Solve](problem.md)
- [Our Solution](solution.md)
- [Loan Lifecycle](loan-lifecycle.md)
