# Network Configuration

Complete guide to configuring your wallet and applications for Mantle Network.

## Network Details

### Mantle Sepolia Testnet

Use this for testing and development.

| Parameter | Value |
|-----------|-------|
| **Network Name** | Mantle Sepolia Testnet |
| **Chain ID** | 5003 |
| **Currency Symbol** | MNT |
| **RPC URL** | https://rpc.sepolia.mantle.xyz |
| **Block Explorer** | https://sepolia.mantlescan.xyz |
| **Faucet** | https://faucet.sepolia.mantle.xyz |

### Mantle Mainnet

For production use.

| Parameter | Value |
|-----------|-------|
| **Network Name** | Mantle |
| **Chain ID** | 5000 |
| **Currency Symbol** | MNT |
| **RPC URL** | https://rpc.mantle.xyz |
| **Block Explorer** | https://explorer.mantle.xyz |

## Wallet Setup

### MetaMask

#### Automatic Addition

1. Visit [chainlist.org](https://chainlist.org)
2. Search for "Mantle"
3. Click "Add to MetaMask"
4. Approve in wallet

#### Manual Addition

1. Open MetaMask
2. Click network dropdown → "Add Network"
3. Click "Add a network manually"
4. Enter details:
   - Network Name: `Mantle Sepolia Testnet`
   - New RPC URL: `https://rpc.sepolia.mantle.xyz`
   - Chain ID: `5003`
   - Currency Symbol: `MNT`
   - Block Explorer: `https://sepolia.mantlescan.xyz`
5. Click "Save"

### Other Wallets

Most EVM wallets support custom networks:
- **Rainbow**: Settings → Networks → Add Custom
- **Trust Wallet**: Settings → Network → Custom
- **Coinbase Wallet**: Settings → Networks → Add Network

## RPC Endpoints

### Public RPCs

| Network | URL |
|---------|-----|
| Mainnet | https://rpc.mantle.xyz |
| Sepolia | https://rpc.sepolia.mantle.xyz |

### Alternative RPCs

For production, consider:
- **Alchemy**: Mantle support (check availability)
- **QuickNode**: Custom endpoints
- **Own Node**: Self-hosted

### RPC Configuration in Code

```typescript
// Viem configuration
import { defineChain } from 'viem';

export const mantleSepolia = defineChain({
  id: 5003,
  name: 'Mantle Sepolia',
  network: 'mantle-sepolia',
  nativeCurrency: {
    decimals: 18,
    name: 'MNT',
    symbol: 'MNT',
  },
  rpcUrls: {
    default: { http: ['https://rpc.sepolia.mantle.xyz'] },
    public: { http: ['https://rpc.sepolia.mantle.xyz'] },
  },
  blockExplorers: {
    default: {
      name: 'Mantlescan',
      url: 'https://sepolia.mantlescan.xyz',
    },
  },
  testnet: true,
});
```

## Environment Variables

### Frontend (.env)

```env
NEXT_PUBLIC_CHAIN_ID=5003
NEXT_PUBLIC_RPC_URL=https://rpc.sepolia.mantle.xyz
NEXT_PUBLIC_EXPLORER_URL=https://sepolia.mantlescan.xyz
```

### Backend (.env)

```env
CHAIN_ID=5003
RPC_URL=https://rpc.sepolia.mantle.xyz
```

## Contract Addresses

### Testnet

| Contract | Address |
|----------|---------|
| Mock USDT | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| Factory P2P | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| Factory Manager | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |

## Verifying Connection

### Check Network in Wallet

1. Open wallet
2. Verify network name shows "Mantle Sepolia"
3. Check chain ID is 5003

### Check via Code

```typescript
const chainId = await publicClient.getChainId();
if (chainId !== 5003) {
  console.error('Wrong network!');
}
```

## Troubleshooting

### "Wrong Network" Errors

1. Switch network in wallet
2. Refresh the page
3. Reconnect wallet

### RPC Issues

If RPC is unresponsive:
1. Try alternative RPC
2. Check network status
3. Wait and retry

### Transaction Failures

1. Ensure sufficient MNT for gas
2. Check contract addresses
3. Verify network connection

## Related Pages

- [Why Mantle?](why-mantle.md)
- [Getting Test MNT](getting-test-mnt.md)
- [Explorer and Tools](explorer-tools.md)
