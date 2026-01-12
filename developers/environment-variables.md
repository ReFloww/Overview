# Environment Variables

Complete reference for all environment variables used in ReFlow.

## Backend Variables

### Required

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/reflow_db

# Server
PORT=30000

# JWT Authentication
JWT_SECRET=your-secret-key-min-32-characters
JWT_REFRESH_SECRET=your-refresh-secret-key

# Blockchain
RPC_URL=https://rpc.sepolia.mantle.xyz
CHAIN_ID=5003
```

### Optional

```env
# Environment
NODE_ENV=development  # development | production

# JWT Expiry
JWT_EXPIRY=15m
JWT_REFRESH_EXPIRY=7d

# Contract Addresses
FACTORY_P2P_ADDRESS=0xa411df45e20d266500363c76ecbf0b8e483fd408
FACTORY_MANAGER_ADDRESS=0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235
USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10

# External Services
RESTOCK_API_URL=https://api.restock.id
KYC_PROVIDER_KEY=your_kyc_key

# Logging
LOG_LEVEL=debug  # debug | info | warn | error
```

## Frontend Variables

### Required

```env
# API
NEXT_PUBLIC_API_BASE_URL=http://localhost:30000/api/v1

# Web3
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id
```

### Optional

```env
# Contract Addresses
NEXT_PUBLIC_USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
NEXT_PUBLIC_FACTORY_ADDRESS=0xa411df45e20d266500363c76ecbf0b8e483fd408

# Blockchain
NEXT_PUBLIC_CHAIN_ID=5003
NEXT_PUBLIC_RPC_URL=https://rpc.sepolia.mantle.xyz
NEXT_PUBLIC_EXPLORER_URL=https://sepolia.mantlescan.xyz

# RPC Providers
NEXT_PUBLIC_ALCHEMY_API_KEY=your_alchemy_key

# Auth (if using NextAuth)
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
NEXTAUTH_SECRET=random_secret_string
NEXTAUTH_URL=http://localhost:3000
```

## Variable Reference

### Database

| Variable | Description | Example |
|----------|-------------|---------|
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://user:pass@host:5432/db` |

### Server

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | Backend server port | `30000` |
| `NODE_ENV` | Environment mode | `development` |

### Authentication

| Variable | Description | Notes |
|----------|-------------|-------|
| `JWT_SECRET` | Access token signing key | Min 32 characters |
| `JWT_REFRESH_SECRET` | Refresh token signing key | Different from JWT_SECRET |
| `JWT_EXPIRY` | Access token expiry | e.g., `15m`, `1h` |
| `JWT_REFRESH_EXPIRY` | Refresh token expiry | e.g., `7d`, `30d` |

### Blockchain

| Variable | Description | Testnet Value |
|----------|-------------|---------------|
| `RPC_URL` | Mantle RPC endpoint | `https://rpc.sepolia.mantle.xyz` |
| `CHAIN_ID` | Network chain ID | `5003` |

### Contracts

| Variable | Description | Testnet Address |
|----------|-------------|-----------------|
| `USDT_ADDRESS` | Stablecoin contract | `0xe01c5464...` |
| `FACTORY_P2P_ADDRESS` | Factory contract | `0xa411df45...` |
| `FACTORY_MANAGER_ADDRESS` | Fund factory | `0x4d1a3d97...` |

### WalletConnect

| Variable | Description | Get From |
|----------|-------------|----------|
| `NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID` | WalletConnect project ID | cloud.walletconnect.com |

## Environment Files

### Structure

```
reflow/
├── Backend/
│   ├── .env              # Local (gitignored)
│   └── .env.example      # Template
│
└── Frontend/
    ├── .env.local        # Local (gitignored)
    └── .env.example      # Template
```

### Sample .env Files

#### Backend/.env.example

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/reflow_db

# Server
PORT=30000
NODE_ENV=development

# JWT
JWT_SECRET=change-this-to-a-secure-secret-key
JWT_REFRESH_SECRET=change-this-to-another-secure-key

# Blockchain
RPC_URL=https://rpc.sepolia.mantle.xyz
CHAIN_ID=5003

# Contracts
FACTORY_P2P_ADDRESS=0xa411df45e20d266500363c76ecbf0b8e483fd408
USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
```

#### Frontend/.env.example

```env
# API
NEXT_PUBLIC_API_BASE_URL=http://localhost:30000/api/v1

# Web3
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id
NEXT_PUBLIC_USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
NEXT_PUBLIC_FACTORY_ADDRESS=0xa411df45e20d266500363c76ecbf0b8e483fd408
```

## Security Notes

### Never Commit

- `.env` files
- Private keys
- API secrets
- JWT secrets

### Git Ignore

Ensure `.gitignore` includes:

```
.env
.env.local
.env.*.local
```

### Production Secrets

- Use environment variables in hosting platform
- Use secret management services
- Rotate secrets regularly

## Validation

### Backend Validation

```typescript
// config.validation.ts
import * as Joi from 'joi';

export const configValidationSchema = Joi.object({
  DATABASE_URL: Joi.string().required(),
  JWT_SECRET: Joi.string().min(32).required(),
  RPC_URL: Joi.string().uri().required(),
  CHAIN_ID: Joi.number().required()
});
```

### Frontend Type Safety

```typescript
// env.d.ts
declare namespace NodeJS {
  interface ProcessEnv {
    NEXT_PUBLIC_API_BASE_URL: string;
    NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID: string;
    NEXT_PUBLIC_USDT_ADDRESS: string;
  }
}
```

## Troubleshooting

### Variables Not Loading

1. Check file name (`.env` vs `.env.local`)
2. Restart dev server
3. Check for typos

### NEXT_PUBLIC Not Working

- Must be prefixed with `NEXT_PUBLIC_`
- Only available at build time
- Restart server after changes
