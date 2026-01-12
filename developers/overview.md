# Developer Overview

Welcome to the ReFlow developer documentation. This guide covers everything you need to integrate with or build on ReFlow.

## Getting Started

### Quick Links

| Resource | Description |
|----------|-------------|
| [Local Setup](local-setup.md) | Set up development environment |
| [API Reference](api-reference/README.md) | REST API documentation |
| [Web3 Integration](web3-integration/README.md) | Blockchain integration |
| [Deployment Guide](deployment/README.md) | Deploy to production |

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        REFLOW STACK                              │
├─────────────────────────────────────────────────────────────────┤
│  Frontend (Next.js)  →  Backend (NestJS)  →  Blockchain (Mantle)│
│         ↓                    ↓                     ↓            │
│  React + Wagmi         REST API            Smart Contracts      │
│  TanStack Query        PostgreSQL          ERC-20 Tokens        │
└─────────────────────────────────────────────────────────────────┘
```

## Tech Stack Summary

| Layer | Technology |
|-------|------------|
| **Frontend** | Next.js 16, React, Tailwind CSS, Wagmi |
| **Backend** | NestJS 10, TypeScript, Prisma, PostgreSQL |
| **Blockchain** | Mantle Network, Solidity, Viem |

## Integration Options

### 1. API Integration

Use our REST API for:
- Reading loan data
- User authentication
- Portfolio management
- Transaction history

```typescript
// Example API call
const response = await fetch('https://api.reflow.io/v1/market/list');
const loans = await response.json();
```

### 2. Smart Contract Integration

Interact directly with contracts for:
- Investments
- Token operations
- Distributions
- Secondary market

```typescript
// Example contract read
const balance = await publicClient.readContract({
  address: loanTokenAddress,
  abi: loanTokenAbi,
  functionName: 'balanceOf',
  args: [userAddress]
});
```

### 3. Webhook Integration

Receive notifications for:
- New investments
- Distributions
- Loan status changes

## Key Resources

### Contract Addresses (Testnet)

| Contract | Address |
|----------|---------|
| Mock USDT | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| Factory P2P | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| Factory Manager | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |

### API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /market/list` | List loans |
| `GET /portfolio` | User holdings |
| `GET /dashboard` | Performance metrics |

## Development Workflow

### 1. Set Up Environment

```bash
# Clone repository
git clone https://github.com/reflow/reflow.git

# Install dependencies
cd Backend && npm install
cd ../Frontend && npm install
```

### 2. Configure Environment

```bash
# Backend
cp Backend/.env.example Backend/.env

# Frontend
cp Frontend/.env.example Frontend/.env
```

### 3. Start Development

```bash
# Backend
cd Backend && npm run start:dev

# Frontend
cd Frontend && npm run dev
```

### 4. Test Integration

- Backend API: http://localhost:30000/api/v1/docs
- Frontend: http://localhost:3000

## Code Examples

### Fetch Loans

```typescript
// Using API
const loans = await api.getLoans({ limit: 10 });

// Using contract
const loanAddresses = await publicClient.readContract({
  address: factoryAddress,
  abi: factoryP2PAbi,
  functionName: 'getAllLoans'
});
```

### Invest in Loan

```typescript
// 1. Approve USDT
await walletClient.writeContract({
  address: usdtAddress,
  abi: erc20Abi,
  functionName: 'approve',
  args: [loanAddress, amount]
});

// 2. Invest
await walletClient.writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'invest',
  args: [amount]
});
```

## Support

### Documentation
- [API Reference](api-reference/README.md)
- [Contract Documentation](../contracts/overview.md)
- [Technical Architecture](../technical/architecture-overview.md)

### Help
- GitHub Issues
- Developer Discord
- Email: dev@reflow.io

## Next Steps

1. [Set up local development](local-setup.md)
2. [Explore the API](api-reference/README.md)
3. [Learn Web3 integration](web3-integration/README.md)
