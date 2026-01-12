# System Components

Detailed breakdown of each system component in the ReFlow architecture.

## Component Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      REFLOW SYSTEM                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────────────────┐│
│  │   Frontend   │  │   Backend    │  │   Smart Contracts      ││
│  │   (Next.js)  │  │   (NestJS)   │  │   (Solidity)           ││
│  └──────────────┘  └──────────────┘  └────────────────────────┘│
│         │                 │                      │              │
│         │                 │                      │              │
│         ▼                 ▼                      ▼              │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                   Data Layer                              │  │
│  │   PostgreSQL  │  Blockchain State  │  External Services  │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Frontend Components

### Application Structure

```
Frontend/
├── src/
│   ├── app/                 # Next.js App Router pages
│   │   ├── (auth)/         # Authentication routes
│   │   ├── dashboard/      # User dashboard
│   │   ├── market/         # Loan marketplace
│   │   ├── portfolio/      # User holdings
│   │   └── auto-manage/    # Managed investing
│   │
│   ├── components/         # React components
│   │   ├── ui/            # Base UI components
│   │   ├── layout/        # Layout components
│   │   └── features/      # Feature-specific
│   │
│   ├── lib/               # Utilities and config
│   │   ├── contracts/     # Contract ABIs
│   │   ├── hooks/         # Custom React hooks
│   │   └── utils/         # Helper functions
│   │
│   └── styles/            # Global styles
│
└── public/                # Static assets
```

### Key Frontend Features

| Feature | Component | Description |
|---------|-----------|-------------|
| Wallet Connection | RainbowKit | Multi-wallet support |
| Contract Interaction | Wagmi/Viem | Type-safe blockchain calls |
| State Management | TanStack Query | Server state caching |
| Forms | React Hook Form | Form handling |
| Tables | TanStack Table | Data tables |
| Notifications | Toast system | User feedback |

### Frontend Modules

#### Authentication Module
- Wallet connection flow
- Session management
- KYC status checking

#### Market Module
- Loan listing display
- Filtering and sorting
- Investment flow

#### Portfolio Module
- Holdings display
- Performance metrics
- Transaction history

#### Auto-Manage Module
- Fund manager listings
- Investment in funds
- Performance tracking

## Backend Components

### Application Structure

```
Backend/
├── src/
│   ├── auth/              # Authentication
│   │   ├── auth.controller.ts
│   │   ├── auth.service.ts
│   │   └── strategies/    # Passport strategies
│   │
│   ├── market/            # Loan marketplace
│   │   ├── market.controller.ts
│   │   ├── market.service.ts
│   │   └── dto/          # Data transfer objects
│   │
│   ├── portfolio/         # User portfolios
│   │   ├── portfolio.controller.ts
│   │   └── portfolio.service.ts
│   │
│   ├── dashboard/         # Analytics
│   │   ├── dashboard.controller.ts
│   │   └── dashboard.service.ts
│   │
│   ├── auto-manage/       # Fund managers
│   │   ├── auto-manage.controller.ts
│   │   └── auto-manage.service.ts
│   │
│   ├── blockchain/        # Web3 integration
│   │   ├── blockchain.service.ts
│   │   └── contracts/     # Contract interactions
│   │
│   ├── prisma/            # Database client
│   │   └── prisma.service.ts
│   │
│   └── common/            # Shared utilities
│       ├── guards/        # Auth guards
│       ├── decorators/    # Custom decorators
│       └── filters/       # Exception filters
│
└── prisma/
    ├── schema.prisma      # Database schema
    └── migrations/        # Schema migrations
```

### Backend Services

| Service | Responsibility |
|---------|----------------|
| AuthService | User authentication, JWT management |
| MarketService | Loan products, marketplace logic |
| PortfolioService | User holdings, valuations |
| DashboardService | Analytics, metrics calculation |
| AutoManageService | Fund manager operations |
| BlockchainService | Contract interaction, events |
| PrismaService | Database operations |

### API Modules

#### Auth Module
```
POST /auth/login      - Wallet authentication
POST /auth/register   - User registration
GET  /auth/profile    - Current user info
POST /auth/refresh    - Token refresh
```

#### Market Module
```
GET  /market/list     - List loan products
GET  /market/:id      - Loan details
POST /market/invest   - Create investment
```

#### Portfolio Module
```
GET  /portfolio       - User holdings
GET  /portfolio/summary - Portfolio summary
```

#### Dashboard Module
```
GET  /dashboard       - Performance metrics
GET  /dashboard/stats - Platform statistics
```

## Smart Contract Components

### Contract Structure

```
Contracts/
├── Factory/
│   ├── FactoryP2P.sol        # Creates loan tokens
│   └── FactoryManager.sol     # Creates fund contracts
│
├── Tokens/
│   └── LoanToken.sol          # ERC-20 loan tokens
│
├── Funds/
│   └── FundManager.sol        # Fund manager contracts
│
└── Interfaces/
    └── IReFlow.sol            # Interface definitions
```

### Contract Roles

| Contract | Purpose |
|----------|---------|
| **Factory P2P** | Deploys loan token contracts |
| **Factory Manager** | Deploys fund manager contracts |
| **Loan Token** | Represents loan ownership |
| **Fund Manager** | Manages pooled investments |

### Contract Interactions

```
┌─────────────────┐
│   Factory P2P   │
│   (Deployer)    │
└────────┬────────┘
         │ deploys
         ▼
┌─────────────────┐
│   Loan Token    │◀─────── Users invest
│   (Per Loan)    │──────── Distributions
└─────────────────┘
```

## Database Components

### Schema Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      DATABASE SCHEMA                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   users              loans               investments             │
│   ├── id            ├── id              ├── id                  │
│   ├── address       ├── token_address   ├── user_id             │
│   ├── email         ├── amount          ├── loan_id             │
│   ├── kyc_status    ├── rate            ├── amount              │
│   └── created_at    ├── duration        ├── tokens              │
│                     ├── status          └── created_at          │
│                     └── created_at                               │
│                                                                  │
│   transactions           fund_managers                          │
│   ├── id                ├── id                                  │
│   ├── user_id           ├── address                             │
│   ├── type              ├── name                                │
│   ├── amount            ├── aum                                 │
│   ├── tx_hash           ├── performance                         │
│   └── created_at        └── created_at                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Key Relationships

```
users ─────┬─────▶ investments
           │
           └─────▶ transactions

loans ───────────▶ investments

fund_managers ───▶ fund_investments
```

## External Integrations

### Restock.id Integration

| Integration | Purpose |
|-------------|---------|
| Loan Data API | Receive loan information |
| Repayment Webhooks | Repayment notifications |
| Status Updates | Loan status changes |

### KYC Provider

| Integration | Purpose |
|-------------|---------|
| Verification API | Identity verification |
| Status Callbacks | Verification results |
| Document Storage | Secure document handling |

### Blockchain RPC

| Provider | Purpose |
|----------|---------|
| Mantle RPC | Blockchain interaction |
| Backup RPCs | Redundancy |
| Event Indexing | Historical data |

## Related Pages

- [Backend Services](backend-services.md)
- [Frontend Application](frontend-application.md)
- [Blockchain Layer](blockchain-layer.md)
