# Backend Setup

Detailed guide for setting up the ReFlow backend.

## Overview

The backend is built with:
- **NestJS 10** - Framework
- **TypeScript** - Language
- **Prisma** - ORM
- **PostgreSQL** - Database
- **Viem** - Blockchain

## Installation

### 1. Navigate to Backend

```bash
cd Backend
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment

```bash
cp .env.example .env
```

Edit `.env`:

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/reflow_db

# Server
PORT=30000
NODE_ENV=development

# JWT Authentication
JWT_SECRET=your-secret-key-min-32-chars
JWT_REFRESH_SECRET=your-refresh-secret-key
JWT_EXPIRY=15m
JWT_REFRESH_EXPIRY=7d

# Blockchain
RPC_URL=https://rpc.sepolia.mantle.xyz
CHAIN_ID=5003

# Contracts
FACTORY_P2P_ADDRESS=0xa411df45e20d266500363c76ecbf0b8e483fd408
USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
```

### 4. Setup Database

```bash
# Generate Prisma client
npx prisma generate

# Run migrations
npx prisma migrate dev --name init

# (Optional) Seed database
npx prisma db seed
```

### 5. Start Server

```bash
# Development (with hot reload)
npm run start:dev

# Production
npm run build
npm run start:prod
```

## Project Structure

```
Backend/
├── src/
│   ├── main.ts                 # Application entry
│   ├── app.module.ts           # Root module
│   │
│   ├── auth/                   # Authentication
│   │   ├── auth.controller.ts
│   │   ├── auth.service.ts
│   │   ├── auth.module.ts
│   │   ├── dto/
│   │   ├── guards/
│   │   └── strategies/
│   │
│   ├── market/                 # Marketplace
│   │   ├── market.controller.ts
│   │   ├── market.service.ts
│   │   └── dto/
│   │
│   ├── portfolio/              # Portfolios
│   │   ├── portfolio.controller.ts
│   │   └── portfolio.service.ts
│   │
│   ├── dashboard/              # Analytics
│   │   ├── dashboard.controller.ts
│   │   └── dashboard.service.ts
│   │
│   ├── blockchain/             # Web3
│   │   ├── blockchain.service.ts
│   │   └── contracts/
│   │
│   ├── prisma/                 # Database
│   │   └── prisma.service.ts
│   │
│   └── common/                 # Shared
│       ├── decorators/
│       ├── filters/
│       ├── guards/
│       └── interceptors/
│
├── prisma/
│   ├── schema.prisma           # Database schema
│   └── migrations/
│
├── test/                       # Tests
├── .env.example
└── package.json
```

## Database Schema

Key models in `prisma/schema.prisma`:

```prisma
model User {
  id          String   @id @default(uuid())
  address     String   @unique
  email       String?
  kycStatus   KycStatus @default(PENDING)
  createdAt   DateTime @default(now())

  investments Investment[]
  transactions Transaction[]
}

model Loan {
  id            String   @id @default(uuid())
  tokenAddress  String   @unique
  name          String
  principal     Decimal
  interestRate  Decimal
  duration      Int
  status        LoanStatus @default(ACTIVE)
  createdAt     DateTime @default(now())

  investments   Investment[]
}

model Investment {
  id        String   @id @default(uuid())
  userId    String
  loanId    String
  amount    Decimal
  tokens    Decimal
  txHash    String
  createdAt DateTime @default(now())

  user      User     @relation(fields: [userId], references: [id])
  loan      Loan     @relation(fields: [loanId], references: [id])
}
```

## API Documentation

Swagger UI available at:
```
http://localhost:30000/api/v1/docs
```

### Key Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /auth/login | Authenticate user |
| GET | /market/list | List loans |
| GET | /portfolio | User holdings |
| GET | /dashboard | Analytics |

## Development Commands

```bash
# Start development
npm run start:dev

# Build
npm run build

# Start production
npm run start:prod

# Run tests
npm run test
npm run test:e2e
npm run test:cov

# Linting
npm run lint

# Prisma
npx prisma generate    # Generate client
npx prisma migrate dev # Run migrations
npx prisma studio      # GUI for database
```

## Testing

```bash
# Unit tests
npm run test

# E2E tests
npm run test:e2e

# Coverage
npm run test:cov
```

## Troubleshooting

### Database Connection

```bash
# Test connection
psql $DATABASE_URL

# Reset database
npx prisma migrate reset
```

### Port in Use

```bash
# Find process
lsof -i :30000

# Change port in .env
PORT=30001
```

### Prisma Issues

```bash
# Regenerate
npx prisma generate

# Reset and migrate
npx prisma migrate reset
```

## Next Steps

- [Frontend Setup](frontend-setup.md)
- [Environment Variables](environment-variables.md)
- [API Reference](api-reference/README.md)
