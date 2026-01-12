# Web3 Integration

Guide to integrating with ReFlow smart contracts.

## Overview

ReFlow uses:
- **Viem** - Ethereum interaction library
- **Wagmi** - React hooks for Ethereum
- **RainbowKit** - Wallet connection UI

## Quick Start

### Install Dependencies

```bash
npm install viem wagmi @rainbow-me/rainbowkit @tanstack/react-query
```

### Configure Wagmi

```typescript
import { createConfig, http } from 'wagmi';
import { mantleSepoliaTestnet } from 'wagmi/chains';

export const config = createConfig({
  chains: [mantleSepoliaTestnet],
  transports: {
    [mantleSepoliaTestnet.id]: http()
  }
});
```

## Contract Addresses

### Testnet (Mantle Sepolia)

| Contract | Address |
|----------|---------|
| Mock USDT | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| Factory P2P | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| Factory Manager | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |

## Basic Operations

### Read Contract

```typescript
import { useReadContract } from 'wagmi';

const { data: balance } = useReadContract({
  address: tokenAddress,
  abi: erc20Abi,
  functionName: 'balanceOf',
  args: [userAddress]
});
```

### Write Contract

```typescript
import { useWriteContract } from 'wagmi';

const { writeContract } = useWriteContract();

await writeContract({
  address: loanAddress,
  abi: loanTokenAbi,
  functionName: 'invest',
  args: [amount]
});
```

### Watch Events

```typescript
import { useWatchContractEvent } from 'wagmi';

useWatchContractEvent({
  address: loanAddress,
  abi: loanTokenAbi,
  eventName: 'Investment',
  onLogs: (logs) => {
    console.log('New investment:', logs);
  }
});
```

## Detailed Guides

- [Wallet Connection](wallet-connection.md)
- [Contract ABIs](contract-abis.md)
- [Transaction Signing](transaction-signing.md)
- [Event Listening](event-listening.md)

## Common Patterns

### Investment Flow

```typescript
// 1. Check allowance
// 2. Approve if needed
// 3. Invest
```

### Claim Distribution

```typescript
// 1. Check pending
// 2. Claim if available
```

See detailed examples in subpages.
