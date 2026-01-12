# Tech Stack

Complete reference of all technologies used in the ReFlow platform.

## Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                          REFLOW TECH STACK                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   FRONTEND            BACKEND             BLOCKCHAIN                 │
│   ─────────           ─────────           ───────────                │
│   Next.js 16          NestJS 10           Mantle Network             │
│   React 18+           TypeScript          Solidity                   │
│   Tailwind CSS 4      Prisma ORM          ERC-20                     │
│   shadcn/ui           PostgreSQL          Viem                       │
│   Wagmi               JWT/Passport                                   │
│   RainbowKit          Swagger                                        │
│   TanStack Query                                                     │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Frontend Stack

### Core Framework

| Technology | Version | Purpose |
|------------|---------|---------|
| **Next.js** | 16.x | React framework with App Router |
| **React** | 18.x | UI component library |
| **TypeScript** | 5.x | Type-safe JavaScript |

### Styling

| Technology | Version | Purpose |
|------------|---------|---------|
| **Tailwind CSS** | 4.x | Utility-first CSS framework |
| **shadcn/ui** | Latest | Pre-built component library |
| **tailwindcss-animate** | Latest | Animation utilities |

### State Management

| Technology | Version | Purpose |
|------------|---------|---------|
| **TanStack Query** | 5.x | Server state management |
| **React Context** | Built-in | Local state management |

### Web3 Libraries

| Technology | Version | Purpose |
|------------|---------|---------|
| **Wagmi** | 2.x | React hooks for Ethereum |
| **Viem** | 2.x | TypeScript Ethereum library |
| **RainbowKit** | 2.x | Wallet connection UI |

### Form Handling

| Technology | Version | Purpose |
|------------|---------|---------|
| **React Hook Form** | 7.x | Form state management |
| **Zod** | 3.x | Schema validation |

### Development Tools

| Technology | Purpose |
|------------|---------|
| **ESLint** | Code linting |
| **Prettier** | Code formatting |
| **TypeScript** | Type checking |

## Backend Stack

### Core Framework

| Technology | Version | Purpose |
|------------|---------|---------|
| **NestJS** | 10.x | Structured Node.js framework |
| **TypeScript** | 5.x | Type-safe JavaScript |
| **Node.js** | 18+ | JavaScript runtime |

### Database

| Technology | Version | Purpose |
|------------|---------|---------|
| **PostgreSQL** | 14+ | Relational database |
| **Prisma** | 5.x | ORM and migrations |

### Authentication

| Technology | Version | Purpose |
|------------|---------|---------|
| **Passport.js** | 0.6.x | Authentication middleware |
| **JWT** | - | Token-based auth |
| **bcrypt** | 5.x | Password hashing |

### API Documentation

| Technology | Version | Purpose |
|------------|---------|---------|
| **Swagger** | - | API documentation |
| **@nestjs/swagger** | 7.x | NestJS Swagger integration |

### Blockchain Integration

| Technology | Version | Purpose |
|------------|---------|---------|
| **Viem** | 2.x | Ethereum interaction |

### Validation

| Technology | Version | Purpose |
|------------|---------|---------|
| **class-validator** | 0.14.x | DTO validation |
| **class-transformer** | 0.5.x | Object transformation |

### Development Tools

| Technology | Purpose |
|------------|---------|
| **Jest** | Testing framework |
| **ESLint** | Code linting |
| **Prettier** | Code formatting |

## Blockchain Stack

### Network

| Technology | Purpose |
|------------|---------|
| **Mantle Network** | L2 blockchain |
| **Ethereum** | L1 security |

### Smart Contracts

| Technology | Version | Purpose |
|------------|---------|---------|
| **Solidity** | 0.8.x | Contract language |
| **OpenZeppelin** | 5.x | Audited contract libraries |

### Standards

| Standard | Purpose |
|----------|---------|
| **ERC-20** | Fungible token standard |

### Development Tools

| Technology | Purpose |
|------------|---------|
| **Hardhat** | Development environment |
| **Foundry** | Testing and deployment |

## Infrastructure

### Hosting (Recommended)

| Component | Recommended Service |
|-----------|-------------------|
| **Frontend** | Vercel |
| **Backend** | Railway, Render, AWS |
| **Database** | Supabase, Railway, AWS RDS |

### External Services

| Service | Purpose |
|---------|---------|
| **WalletConnect** | Wallet connection |
| **Alchemy** | RPC provider (backup) |

## Development Environment

### Required Tools

| Tool | Version | Purpose |
|------|---------|---------|
| **Node.js** | 18+ | JavaScript runtime |
| **npm/yarn** | Latest | Package manager |
| **Git** | Latest | Version control |
| **PostgreSQL** | 14+ | Local database |

### Recommended IDE

| IDE | Extensions |
|-----|------------|
| **VS Code** | ESLint, Prettier, Tailwind CSS IntelliSense |

### Environment Setup

```bash
# Required versions
node --version  # v18.0.0 or higher
npm --version   # v9.0.0 or higher
```

## Package Dependencies

### Frontend Key Packages

```json
{
  "next": "^16.0.0",
  "react": "^18.0.0",
  "tailwindcss": "^4.0.0",
  "wagmi": "^2.0.0",
  "viem": "^2.0.0",
  "@rainbow-me/rainbowkit": "^2.0.0",
  "@tanstack/react-query": "^5.0.0"
}
```

### Backend Key Packages

```json
{
  "@nestjs/core": "^10.0.0",
  "@nestjs/common": "^10.0.0",
  "@prisma/client": "^5.0.0",
  "viem": "^2.0.0",
  "passport": "^0.6.0",
  "passport-jwt": "^4.0.0"
}
```

## Version Compatibility

### Tested Combinations

| Component | Version Range |
|-----------|---------------|
| Node.js | 18.x - 20.x |
| PostgreSQL | 14.x - 16.x |
| Next.js | 14.x - 16.x |
| NestJS | 10.x |

## Security Considerations

### Frontend
- CSP headers configured
- XSS prevention
- Secure cookie handling

### Backend
- Input validation
- Rate limiting
- CORS configuration
- Helmet.js security headers

### Blockchain
- Contract audits
- Access control
- Reentrancy protection

## Related Pages

- [Architecture Overview](architecture-overview.md)
- [Backend Services](backend-services.md)
- [Frontend Application](frontend-application.md)
