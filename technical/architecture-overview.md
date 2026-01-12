# Architecture Overview

ReFlow's architecture combines traditional web technologies with blockchain infrastructure to deliver a decentralized P2P lending platform.

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                           USER LAYER                                 │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────┐ │
│  │   Web Browser   │  │  Mobile Device  │  │   External Apps     │ │
│  └────────┬────────┘  └────────┬────────┘  └──────────┬──────────┘ │
│           │                    │                       │            │
└───────────┼────────────────────┼───────────────────────┼────────────┘
            │                    │                       │
            ▼                    ▼                       ▼
┌─────────────────────────────────────────────────────────────────────┐
│                        APPLICATION LAYER                             │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │                     Frontend (Next.js)                          ││
│  │  • React Components  • Web3 Integration  • State Management     ││
│  └─────────────────────────────────────────────────────────────────┘│
│                              │                                       │
│                              ▼                                       │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │                     Backend (NestJS)                            ││
│  │  • REST API  • Authentication  • Business Logic  • Data Access  ││
│  └─────────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────┘
                               │
            ┌──────────────────┼──────────────────┐
            ▼                  ▼                  ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────────────────┐
│    DATABASE     │ │  BLOCKCHAIN     │ │   EXTERNAL SERVICES         │
│   PostgreSQL    │ │  Mantle Network │ │   • Restock.id API          │
│                 │ │  Smart Contracts│ │   • KYC Provider            │
└─────────────────┘ └─────────────────┘ └─────────────────────────────┘
```

## Core Components

### Frontend Application

The user-facing interface built with modern web technologies.

| Component | Technology | Purpose |
|-----------|------------|---------|
| Framework | Next.js 16 | Server-side rendering, routing |
| UI Library | React 18+ | Component-based UI |
| Styling | Tailwind CSS 4 | Utility-first CSS |
| Components | shadcn/ui | Pre-built UI components |
| State | TanStack Query | Server state management |
| Web3 | Wagmi + Viem | Blockchain interaction |
| Wallet | RainbowKit | Wallet connection |

### Backend Services

Server-side logic and API layer.

| Component | Technology | Purpose |
|-----------|------------|---------|
| Framework | NestJS 10 | Structured Node.js backend |
| Language | TypeScript | Type safety |
| ORM | Prisma | Database access |
| Auth | JWT + Passport | Authentication |
| Validation | class-validator | Input validation |
| Docs | Swagger | API documentation |
| Blockchain | Viem | Contract interaction |

### Database Layer

Persistent data storage.

| Component | Technology | Purpose |
|-----------|------------|---------|
| Primary DB | PostgreSQL 14+ | Relational data |
| ORM | Prisma | Schema management |
| Migrations | Prisma Migrate | Schema versioning |

### Blockchain Layer

On-chain operations and state.

| Component | Technology | Purpose |
|-----------|------------|---------|
| Network | Mantle Network | L2 blockchain |
| Contracts | Solidity | Smart contracts |
| Standard | ERC-20 | Token standard |
| Interaction | Viem | RPC communication |

## Data Flow

### Investment Flow

```
1. User initiates investment on Frontend
2. Frontend sends request to Backend API
3. Backend validates and prepares transaction
4. Frontend prompts user wallet signature
5. Transaction submitted to Mantle Network
6. Smart contract executes investment logic
7. Backend monitors transaction confirmation
8. Database updated with investment record
9. Frontend displays confirmation
```

### Repayment Distribution Flow

```
1. Restock.id sends repayment notification
2. Backend receives webhook/API call
3. Backend triggers distribution on smart contract
4. Smart contract calculates proportional shares
5. USDT distributed to all token holders
6. Backend records distribution
7. Users see updated balances
```

## Component Interactions

### Frontend ↔ Backend

```
Frontend                          Backend
   │                                 │
   │──── REST API Calls ──────────▶│
   │         (JSON)                  │
   │◀─── API Responses ────────────│
   │                                 │
   │──── WebSocket (optional) ────▶│
   │         (Real-time)             │
```

### Frontend ↔ Blockchain

```
Frontend                        Blockchain
   │                                │
   │──── Read Calls (Viem) ──────▶│
   │         (Contract reads)       │
   │◀─── State Data ───────────────│
   │                                │
   │──── Write Calls (Wagmi) ────▶│
   │         (Via user wallet)      │
   │◀─── Transaction Receipts ────│
```

### Backend ↔ Blockchain

```
Backend                         Blockchain
   │                                │
   │──── Event Monitoring ───────▶│
   │         (Viem)                 │
   │◀─── Events/Logs ─────────────│
   │                                │
   │──── Contract Reads ──────────▶│
   │         (State queries)        │
   │◀─── Data ─────────────────────│
```

## Key Design Principles

### Non-Custodial

```
✓ User funds stay in user wallets
✓ Platform never controls private keys
✓ Smart contracts manage fund flows
✓ Direct blockchain transactions
```

### Transparency

```
✓ All transactions on-chain
✓ Contract code verifiable
✓ State publicly readable
✓ Audit trails immutable
```

### Security

```
✓ Defense in depth
✓ Principle of least privilege
✓ Input validation everywhere
✓ Secure by default
```

### Scalability

```
✓ Stateless API design
✓ Database optimization
✓ Caching strategies
✓ L2 blockchain (low costs)
```

## Environment Architecture

### Development

```
┌──────────────────────────────────────────┐
│           Local Development              │
├──────────────────────────────────────────┤
│ Frontend: localhost:3000                 │
│ Backend: localhost:30000                 │
│ Database: Local PostgreSQL               │
│ Blockchain: Mantle Sepolia Testnet       │
└──────────────────────────────────────────┘
```

### Production

```
┌──────────────────────────────────────────┐
│             Production                   │
├──────────────────────────────────────────┤
│ Frontend: Vercel / CDN                   │
│ Backend: Cloud hosting                   │
│ Database: Managed PostgreSQL             │
│ Blockchain: Mantle Mainnet               │
└──────────────────────────────────────────┘
```

## Related Pages

- [System Components](system-components.md)
- [Tech Stack](tech-stack.md)
- [Data Flow](data-flow.md)
