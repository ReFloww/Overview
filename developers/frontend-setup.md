# Frontend Setup

Detailed guide for setting up the ReFlow frontend.

## Overview

The frontend is built with:
- **Next.js 16** - React framework
- **Tailwind CSS 4** - Styling
- **shadcn/ui** - Components
- **Wagmi + Viem** - Web3
- **RainbowKit** - Wallet UI
- **TanStack Query** - State

## Installation

### 1. Navigate to Frontend

```bash
cd Frontend
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
# API
NEXT_PUBLIC_API_BASE_URL=http://localhost:30000/api/v1

# Web3
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id
NEXT_PUBLIC_USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
NEXT_PUBLIC_FACTORY_ADDRESS=0xa411df45e20d266500363c76ecbf0b8e483fd408
NEXT_PUBLIC_ALCHEMY_API_KEY=your_alchemy_key

# Auth (Optional)
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
NEXTAUTH_SECRET=random_secret_string
NEXTAUTH_URL=http://localhost:3000
```

### 4. Get WalletConnect ID

1. Go to [cloud.walletconnect.com](https://cloud.walletconnect.com)
2. Create account/login
3. Create new project
4. Copy Project ID to env

### 5. Start Server

```bash
# Development
npm run dev

# Production
npm run build
npm run start
```

Access at: http://localhost:3000

## Project Structure

```
Frontend/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── layout.tsx         # Root layout
│   │   ├── page.tsx           # Home page
│   │   ├── (auth)/            # Auth routes
│   │   │   ├── login/
│   │   │   └── register/
│   │   ├── market/            # Marketplace
│   │   │   ├── page.tsx
│   │   │   └── [id]/
│   │   ├── portfolio/         # Portfolio
│   │   ├── dashboard/         # Dashboard
│   │   └── auto-manage/       # Fund managers
│   │
│   ├── components/
│   │   ├── ui/                # Base components (shadcn)
│   │   ├── layout/            # Layout components
│   │   └── features/          # Feature components
│   │
│   ├── lib/
│   │   ├── contracts/         # ABIs and addresses
│   │   ├── hooks/             # Custom hooks
│   │   ├── api/               # API client
│   │   └── utils/             # Utilities
│   │
│   ├── providers/             # React providers
│   └── styles/
│       └── globals.css
│
├── public/                    # Static assets
├── next.config.js
├── tailwind.config.js
└── package.json
```

## Key Configurations

### Wagmi Config

```typescript
// lib/config/wagmi.ts
import { createConfig, http } from 'wagmi';
import { mantleSepoliaTestnet } from 'wagmi/chains';
import { getDefaultWallets } from '@rainbow-me/rainbowkit';

const { connectors } = getDefaultWallets({
  appName: 'ReFlow',
  projectId: process.env.NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID!
});

export const config = createConfig({
  chains: [mantleSepoliaTestnet],
  connectors,
  transports: {
    [mantleSepoliaTestnet.id]: http()
  }
});
```

### TanStack Query

```typescript
// providers/query-provider.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 30000,
      retry: 1
    }
  }
});

export function QueryProvider({ children }) {
  return (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  );
}
```

### Tailwind Config

```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      // Custom theme
    }
  },
  plugins: [require('tailwindcss-animate')]
};
```

## Adding Components

### Using shadcn/ui

```bash
# Add button component
npx shadcn-ui@latest add button

# Add card component
npx shadcn-ui@latest add card

# Add form components
npx shadcn-ui@latest add form input label
```

## Development Commands

```bash
# Start dev server
npm run dev

# Build
npm run build

# Start production
npm run start

# Lint
npm run lint

# Type check
npm run type-check
```

## Web3 Development

### Connect Wallet

```tsx
import { ConnectButton } from '@rainbow-me/rainbowkit';

function Header() {
  return (
    <header>
      <ConnectButton />
    </header>
  );
}
```

### Read Contract

```tsx
import { useReadContract } from 'wagmi';

function Balance() {
  const { data: balance } = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: 'balanceOf',
    args: [userAddress]
  });

  return <div>Balance: {balance?.toString()}</div>;
}
```

### Write Contract

```tsx
import { useWriteContract } from 'wagmi';

function InvestButton() {
  const { writeContract, isPending } = useWriteContract();

  const handleInvest = () => {
    writeContract({
      address: loanAddress,
      abi: loanTokenAbi,
      functionName: 'invest',
      args: [amount]
    });
  };

  return (
    <button onClick={handleInvest} disabled={isPending}>
      {isPending ? 'Investing...' : 'Invest'}
    </button>
  );
}
```

## Troubleshooting

### Hydration Errors

Wrap Web3 components in client-only:

```tsx
'use client';

import dynamic from 'next/dynamic';

const WalletButton = dynamic(
  () => import('./WalletButton'),
  { ssr: false }
);
```

### Module Not Found

```bash
rm -rf node_modules .next
npm install
```

### Environment Variables

- `NEXT_PUBLIC_` prefix required for client-side
- Restart dev server after changes

## Next Steps

- [Environment Variables](environment-variables.md)
- [Web3 Integration](web3-integration/README.md)
- [API Integration](api-reference/README.md)
