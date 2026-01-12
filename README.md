# Introduction

**Decentralized P2P Lending Platform for Real World Assets on Mantle Network**

ReFlow transforms traditional P2P lending by tokenizing loans into tradeable assets, enabling liquidity, transparency, and 24/7 access through blockchain technology.

## Problem

Traditional P2P lending platforms suffer from:

* **Illiquidity**: Funds locked for 6-12 months until loan maturity
* **Opacity**: Limited visibility into loan performance and fund usage
* **Limited Access**: Platform-dependent, office-hours operations
* **Custodial Risk**: Funds held by centralized platforms

## Solution

ReFlow addresses these issues by:

* **Tokenized Loans**: Convert P2P loans into ERC-20 tokens tradeable on secondary markets
* **On-Chain Transparency**: All transactions verifiable on Mantle Network
* **24/7 Availability**: Trade anytime via non-custodial wallets
* **Dual Investment Modes**: Choose between managed funds or self-directed lending

## Features

* **Secondary Market Trading**: Buy/sell loan tokens before maturity
* **Auto-Manage Mode**: Delegate investments to professional fund managers
* **Self-Manage Mode**: Build your own loan portfolio
* **Real-Time Portfolio Tracking**: Monitor holdings, earnings, and performance
* **KYC Integration**: Compliant onboarding process
* **Automated Repayments**: On-chain distribution of principal and interest

## Tech Stack

### Backend

* **Framework**: NestJS 10 (TypeScript)
* **Database**: PostgreSQL + Prisma ORM
* **Blockchain**: Viem for Mantle Network integration
* **Auth**: JWT
* **Docs**: Scalar UI

### Frontend

* **Framework**: Next.js 16 (App Router)
* **Styling**: Tailwind CSS 4 + shadcn/ui
* **Web3**: Wagmi + RainbowKit + Viem
* **State**: TanStack Query

### Blockchain

* **Network**: Mantle Sepolia Testnet (Chain ID: 5003)
* **Contracts**: ERC-20 based loan tokens, Factory contracts

## Smart Contract Addresses (Mantle Sepolia)

| Contract        | Address                                      |
| --------------- | -------------------------------------------- |
| Mock USDT       | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| Factory P2P     | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| Factory Manager | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |

## Getting Started

### Prerequisites

* Node.js 18+
* PostgreSQL 14+
* Git

### Backend Setup

```bash
cd Backend

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your database credentials

# Generate Prisma client
npx prisma generate

# Run database migrations
npx prisma migrate dev --name init

# Start development server (port 30000)
npm run start:dev
```

**Backend Environment Variables:**

```env
DATABASE_URL=postgresql://user:password@localhost:5432/reflow_db
PORT=30000
```

### Frontend Setup

```bash
cd Frontend

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with required keys

# Start development server (port 3000)
npm run dev
```

**Frontend Environment Variables:**

```env
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_walletconnect_project_id
NEXT_PUBLIC_USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
NEXT_PUBLIC_API_BASE_URL=http://localhost:30000/api/v1
NEXT_PUBLIC_ALCHEMY_API_KEY=your_alchemy_api_key

GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
NEXTAUTH_SECRET=your_random_secret
NEXTAUTH_URL=http://localhost:3000
```

### Production Build

```bash
# Backend
cd Backend
npm run build
npm run start:prod

# Frontend
cd Frontend
npm run build
npm run start
```

## API Documentation

Once the backend is running, access API docs at:

* Swagger UI: `http://localhost:30000/api/v1/docs`

### Key Endpoints

| Method | Endpoint                 | Description          |
| ------ | ------------------------ | -------------------- |
| POST   | `/auth/login`            | User authentication  |
| POST   | `/auth/register`         | User registration    |
| GET    | `/market/list`           | Browse loan products |
| GET    | `/market/:id`            | Product details      |
| GET    | `/portfolio`             | User holdings        |
| GET    | `/dashboard`             | Performance metrics  |
| GET    | `/investment-funds/list` | Fund managers        |
| GET    | `/history/transactions`  | Transaction history  |

## Architecture

```
reflow/
├── Backend/
│   ├── src/
│   │   ├── auth/           # JWT authentication
│   │   ├── market/         # Loan products & contracts
│   │   ├── portfolio/      # User holdings
│   │   ├── dashboard/      # Analytics
│   │   ├── auto-manage/    # Fund managers
│   │   ├── blockchain/     # Viem integration
│   │   └── prisma/         # Database client
│   └── prisma/schema.prisma
│
├── Frontend/
│   ├── src/
│   │   ├── app/            # Next.js pages
│   │   ├── components/     # React components
│   │   ├── lib/            # Utils, ABIs, config
│   │   └── hooks/          # Custom hooks
│   └── public/
│
└── Overview/               # Documentation
```

## Network Configuration

To use ReFlow, configure your wallet for Mantle Sepolia:

| Parameter    | Value                          |
| ------------ | ------------------------------ |
| Network Name | Mantle Sepolia                 |
| Chain ID     | 5003                           |
| RPC URL      | https://rpc.sepolia.mantle.xyz |
| Currency     | MNT                            |
| Explorer     | https://sepolia.mantlescan.xyz |

## Testing

```bash
# Backend tests
cd Backend
npm run test

# Frontend tests
cd Frontend
npm run test
```

## Deployment

### Backend Deployment

1. Set up PostgreSQL database (e.g., Supabase, Railway, AWS RDS)
2. Configure `DATABASE_URL` environment variable
3. Deploy to Node.js hosting (Vercel, Railway, Render, AWS)
4. Run `npx prisma migrate deploy` on first deployment

### Frontend Deployment

1. Configure environment variables in hosting platform
2. Deploy to Vercel (recommended for Next.js) or similar
3. Ensure CORS is configured on backend for frontend domain

## Partners

* **Restock.id** - P2P lending operations partner
* **Mantle Network** - Blockchain infrastructure
* **OJK** - Regulatory compliance via Restock.id

## License

MIT License

## Links

* [Mantle Network](https://www.mantle.xyz/)
* [Mantle Sepolia Faucet](https://faucet.sepolia.mantle.xyz/)
* [API Documentation](http://localhost:30000/api/v1/docs)
