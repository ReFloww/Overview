# Wallet Connection

Guide to implementing wallet connection with RainbowKit.

## Setup

### Install

```bash
npm install @rainbow-me/rainbowkit wagmi viem @tanstack/react-query
```

### Configure

```typescript
// config/wagmi.ts
import { getDefaultWallets } from '@rainbow-me/rainbowkit';
import { createConfig, http } from 'wagmi';
import { mantleSepoliaTestnet } from 'wagmi/chains';

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

### Provider Setup

```tsx
// providers/web3-provider.tsx
'use client';

import { RainbowKitProvider } from '@rainbow-me/rainbowkit';
import { WagmiProvider } from 'wagmi';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { config } from '@/config/wagmi';

const queryClient = new QueryClient();

export function Web3Provider({ children }: { children: React.ReactNode }) {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <RainbowKitProvider>
          {children}
        </RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

## Connect Button

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

## Using Account

```tsx
import { useAccount, useDisconnect } from 'wagmi';

function Profile() {
  const { address, isConnected, chain } = useAccount();
  const { disconnect } = useDisconnect();

  if (!isConnected) {
    return <div>Not connected</div>;
  }

  return (
    <div>
      <p>Address: {address}</p>
      <p>Chain: {chain?.name}</p>
      <button onClick={() => disconnect()}>Disconnect</button>
    </div>
  );
}
```

## Check Network

```tsx
import { useAccount, useSwitchChain } from 'wagmi';
import { mantleSepoliaTestnet } from 'wagmi/chains';

function NetworkCheck() {
  const { chain } = useAccount();
  const { switchChain } = useSwitchChain();

  if (chain?.id !== mantleSepoliaTestnet.id) {
    return (
      <button onClick={() => switchChain({ chainId: mantleSepoliaTestnet.id })}>
        Switch to Mantle Sepolia
      </button>
    );
  }

  return <div>Connected to correct network</div>;
}
```

## Custom Connect Button

```tsx
import { useAccount, useConnect, useDisconnect } from 'wagmi';

function CustomConnect() {
  const { address, isConnected } = useAccount();
  const { connect, connectors } = useConnect();
  const { disconnect } = useDisconnect();

  if (isConnected) {
    return (
      <div>
        <span>{address?.slice(0, 6)}...{address?.slice(-4)}</span>
        <button onClick={() => disconnect()}>Disconnect</button>
      </div>
    );
  }

  return (
    <div>
      {connectors.map((connector) => (
        <button key={connector.id} onClick={() => connect({ connector })}>
          Connect {connector.name}
        </button>
      ))}
    </div>
  );
}
```

## Connection State

```typescript
const {
  address,      // Connected address
  isConnected,  // Is wallet connected
  isConnecting, // Is connecting
  isDisconnected,
  connector,    // Current connector
  chain         // Current chain
} = useAccount();
```
