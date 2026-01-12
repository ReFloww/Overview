# Prerequisites

Requirements for developing with ReFlow.

## System Requirements

### Minimum Hardware

| Component | Requirement |
|-----------|-------------|
| CPU | 2+ cores |
| RAM | 8GB |
| Storage | 10GB free |
| Network | Stable internet |

### Recommended

| Component | Recommendation |
|-----------|----------------|
| CPU | 4+ cores |
| RAM | 16GB |
| Storage | SSD with 20GB+ free |

## Required Software

### Node.js

**Version:** 18.x or higher

**Installation:**

```bash
# Using nvm (recommended)
nvm install 18
nvm use 18

# Verify
node --version
```

Download: [nodejs.org](https://nodejs.org)

### npm

**Version:** 9.x or higher (comes with Node.js)

```bash
npm --version
```

### PostgreSQL

**Version:** 14.x or higher

**Installation:**

macOS:
```bash
brew install postgresql@14
brew services start postgresql@14
```

Ubuntu:
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

Windows:
Download from [postgresql.org](https://www.postgresql.org/download/windows/)

### Git

**Version:** Any recent

```bash
git --version
```

Download: [git-scm.com](https://git-scm.com)

## Optional Tools

### Recommended IDE

**VS Code** with extensions:
- ESLint
- Prettier
- Tailwind CSS IntelliSense
- Prisma
- TypeScript

### Database GUI

- **Prisma Studio** (built-in)
- **pgAdmin**
- **DBeaver**

### API Testing

- **Postman**
- **Insomnia**
- **HTTPie**

## Wallet Setup

For Web3 development:

### MetaMask

1. Install browser extension
2. Create or import wallet
3. Add Mantle Sepolia network
4. Get test tokens from faucet

### Network Configuration

| Parameter | Value |
|-----------|-------|
| Network | Mantle Sepolia |
| Chain ID | 5003 |
| RPC | https://rpc.sepolia.mantle.xyz |
| Symbol | MNT |

## API Keys

### Required

| Service | Purpose | Get From |
|---------|---------|----------|
| WalletConnect | Wallet connection | [cloud.walletconnect.com](https://cloud.walletconnect.com) |

### Optional

| Service | Purpose | Get From |
|---------|---------|----------|
| Alchemy | Backup RPC | [alchemy.com](https://www.alchemy.com) |
| Google OAuth | Social login | [console.cloud.google.com](https://console.cloud.google.com) |

## Verification Checklist

```bash
# Check all requirements
node --version    # v18.0.0+
npm --version     # v9.0.0+
psql --version    # 14.0+
git --version     # Any

# Optional
code --version    # VS Code
```

## Next Steps

Once prerequisites are met:
1. [Set up local development](local-setup.md)
2. [Configure backend](backend-setup.md)
3. [Configure frontend](frontend-setup.md)
