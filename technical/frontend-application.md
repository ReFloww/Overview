# Frontend Application

Detailed documentation of ReFlow's frontend architecture, components, and patterns.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                       Next.js Application                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │                      App Router (Pages)                          ││
│  │   / │ /market │ /portfolio │ /dashboard │ /auto-manage          ││
│  └─────────────────────────────────────────────────────────────────┘│
│                              │                                       │
│  ┌─────────────┐  ┌─────────────┐  ┌──────────────────────────────┐│
│  │  Components │  │    Hooks    │  │         Providers            ││
│  │  (UI/Logic) │  │  (Custom)   │  │  (Context/State)             ││
│  └─────────────┘  └─────────────┘  └──────────────────────────────┘│
│                              │                                       │
│  ┌─────────────────────────────────────────────────────────────────┐│
│  │                      External Libraries                          ││
│  │   Wagmi │ Viem │ RainbowKit │ TanStack Query │ Tailwind        ││
│  └─────────────────────────────────────────────────────────────────┘│
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Project Structure

```
Frontend/
├── src/
│   ├── app/                      # Next.js App Router
│   │   ├── layout.tsx           # Root layout
│   │   ├── page.tsx             # Home page
│   │   ├── (auth)/              # Auth group
│   │   │   ├── login/
│   │   │   └── register/
│   │   ├── market/              # Marketplace
│   │   │   ├── page.tsx
│   │   │   └── [id]/
│   │   ├── portfolio/           # User portfolio
│   │   ├── dashboard/           # Dashboard
│   │   └── auto-manage/         # Fund managers
│   │
│   ├── components/
│   │   ├── ui/                  # Base UI (shadcn)
│   │   │   ├── button.tsx
│   │   │   ├── card.tsx
│   │   │   ├── input.tsx
│   │   │   └── ...
│   │   ├── layout/              # Layout components
│   │   │   ├── header.tsx
│   │   │   ├── sidebar.tsx
│   │   │   └── footer.tsx
│   │   └── features/            # Feature components
│   │       ├── wallet/
│   │       ├── market/
│   │       └── portfolio/
│   │
│   ├── lib/
│   │   ├── contracts/           # Contract ABIs & addresses
│   │   ├── hooks/               # Custom hooks
│   │   ├── api/                 # API client
│   │   ├── utils/               # Utilities
│   │   └── config/              # Configuration
│   │
│   ├── providers/               # React providers
│   │   ├── wagmi-provider.tsx
│   │   └── query-provider.tsx
│   │
│   └── styles/
│       └── globals.css          # Global styles
│
├── public/                      # Static assets
├── next.config.js
├── tailwind.config.js
└── package.json
```

## Key Components

### Layout Components

#### Root Layout

```tsx
// app/layout.tsx
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <Providers>
          <Header />
          <main>{children}</main>
          <Footer />
        </Providers>
      </body>
    </html>
  );
}
```

#### Header with Wallet

```tsx
// components/layout/header.tsx
export function Header() {
  return (
    <header className="flex items-center justify-between p-4">
      <Logo />
      <Navigation />
      <ConnectButton />  {/* RainbowKit */}
    </header>
  );
}
```

### Feature Components

#### Market Card

```tsx
// components/features/market/loan-card.tsx
interface LoanCardProps {
  loan: Loan;
  onInvest: (id: string) => void;
}

export function LoanCard({ loan, onInvest }: LoanCardProps) {
  return (
    <Card>
      <CardHeader>
        <CardTitle>{loan.name}</CardTitle>
        <Badge>{loan.riskGrade}</Badge>
      </CardHeader>
      <CardContent>
        <div className="grid grid-cols-2 gap-4">
          <Stat label="APY" value={`${loan.apy}%`} />
          <Stat label="Duration" value={`${loan.duration} months`} />
          <Stat label="Amount" value={formatCurrency(loan.amount)} />
          <Stat label="Available" value={formatCurrency(loan.available)} />
        </div>
      </CardContent>
      <CardFooter>
        <Button onClick={() => onInvest(loan.id)}>Invest</Button>
      </CardFooter>
    </Card>
  );
}
```

#### Portfolio Table

```tsx
// components/features/portfolio/holdings-table.tsx
export function HoldingsTable({ holdings }: { holdings: Holding[] }) {
  const columns = [
    { header: 'Asset', accessor: 'name' },
    { header: 'Value', accessor: 'value', cell: formatCurrency },
    { header: 'APY', accessor: 'apy', cell: formatPercent },
    { header: 'Earnings', accessor: 'earnings', cell: formatCurrency }
  ];

  return (
    <DataTable columns={columns} data={holdings} />
  );
}
```

## Web3 Integration

### Wagmi Configuration

```tsx
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

### Contract Hooks

```tsx
// lib/hooks/useInvestment.ts
export function useInvestment(loanAddress: string) {
  const { address } = useAccount();

  // Read token balance
  const { data: balance } = useReadContract({
    address: loanAddress,
    abi: loanTokenAbi,
    functionName: 'balanceOf',
    args: [address]
  });

  // Write: Invest
  const { writeContract, isPending } = useWriteContract();

  const invest = async (amount: bigint) => {
    await writeContract({
      address: loanAddress,
      abi: loanTokenAbi,
      functionName: 'invest',
      args: [amount]
    });
  };

  return { balance, invest, isPending };
}
```

### Token Approval Hook

```tsx
// lib/hooks/useTokenApproval.ts
export function useTokenApproval(tokenAddress: string, spenderAddress: string) {
  const { address } = useAccount();

  const { data: allowance } = useReadContract({
    address: tokenAddress,
    abi: erc20Abi,
    functionName: 'allowance',
    args: [address, spenderAddress]
  });

  const { writeContract: approve, isPending } = useWriteContract();

  const ensureApproval = async (amount: bigint) => {
    if (allowance && allowance >= amount) return true;

    await approve({
      address: tokenAddress,
      abi: erc20Abi,
      functionName: 'approve',
      args: [spenderAddress, amount]
    });

    return true;
  };

  return { allowance, ensureApproval, isPending };
}
```

## State Management

### TanStack Query

```tsx
// lib/api/queries.ts
export const useLoans = (filters: LoanFilters) => {
  return useQuery({
    queryKey: ['loans', filters],
    queryFn: () => api.getLoans(filters),
    staleTime: 30000 // 30 seconds
  });
};

export const usePortfolio = (userId: string) => {
  return useQuery({
    queryKey: ['portfolio', userId],
    queryFn: () => api.getPortfolio(userId),
    enabled: !!userId
  });
};
```

### API Client

```tsx
// lib/api/client.ts
const API_BASE = process.env.NEXT_PUBLIC_API_BASE_URL;

export const api = {
  // Auth
  login: (data: LoginDto) =>
    fetch(`${API_BASE}/auth/login`, {
      method: 'POST',
      body: JSON.stringify(data)
    }).then(res => res.json()),

  // Market
  getLoans: (filters: LoanFilters) =>
    fetch(`${API_BASE}/market/list?${new URLSearchParams(filters)}`)
      .then(res => res.json()),

  // Portfolio
  getPortfolio: (userId: string) =>
    fetch(`${API_BASE}/portfolio`, {
      headers: { Authorization: `Bearer ${getToken()}` }
    }).then(res => res.json())
};
```

## Pages

### Market Page

```tsx
// app/market/page.tsx
export default function MarketPage() {
  const [filters, setFilters] = useState<LoanFilters>({});
  const { data: loans, isLoading } = useLoans(filters);

  return (
    <div className="container py-8">
      <h1 className="text-3xl font-bold mb-6">Loan Marketplace</h1>

      <Filters value={filters} onChange={setFilters} />

      {isLoading ? (
        <LoadingGrid />
      ) : (
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {loans?.map(loan => (
            <LoanCard key={loan.id} loan={loan} />
          ))}
        </div>
      )}
    </div>
  );
}
```

### Dashboard Page

```tsx
// app/dashboard/page.tsx
export default function DashboardPage() {
  const { address } = useAccount();
  const { data: dashboard } = useDashboard(address);

  return (
    <div className="container py-8">
      <h1 className="text-3xl font-bold mb-6">Dashboard</h1>

      <div className="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
        <StatCard label="Total Value" value={dashboard?.totalValue} />
        <StatCard label="Total Returns" value={dashboard?.returns} />
        <StatCard label="Active Investments" value={dashboard?.activeCount} />
        <StatCard label="APY" value={dashboard?.avgApy} />
      </div>

      <PerformanceChart data={dashboard?.chartData} />

      <RecentActivity activities={dashboard?.activities} />
    </div>
  );
}
```

## Styling

### Tailwind Configuration

```js
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: { /* custom colors */ },
        secondary: { /* custom colors */ }
      }
    }
  },
  plugins: [require('tailwindcss-animate')]
};
```

### Component Styling

```tsx
// components/ui/button.tsx
const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground hover:bg-primary/90',
        outline: 'border border-input bg-background hover:bg-accent',
        ghost: 'hover:bg-accent hover:text-accent-foreground'
      },
      size: {
        default: 'h-10 px-4 py-2',
        sm: 'h-9 px-3',
        lg: 'h-11 px-8'
      }
    },
    defaultVariants: {
      variant: 'default',
      size: 'default'
    }
  }
);
```

## Environment Variables

```env
# API
NEXT_PUBLIC_API_BASE_URL=http://localhost:30000/api/v1

# Blockchain
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id
NEXT_PUBLIC_USDT_ADDRESS=0xe01c5464816a544d4d0d6a336032578bd4629F10
NEXT_PUBLIC_ALCHEMY_API_KEY=your_alchemy_key

# Auth
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
NEXTAUTH_SECRET=your_random_secret
NEXTAUTH_URL=http://localhost:3000
```

## Related Pages

- [Backend Services](backend-services.md)
- [Web3 Integration](../developers/web3-integration/README.md)
- [Tech Stack](tech-stack.md)
