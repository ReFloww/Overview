# Deployed Contracts

Reference for all deployed smart contracts on ReFlow.

## Network Information

### Mantle Sepolia Testnet

| Parameter | Value |
|-----------|-------|
| Network Name | Mantle Sepolia Testnet |
| Chain ID | 5003 |
| RPC URL | https://rpc.sepolia.mantle.xyz |
| Block Explorer | https://sepolia.mantlescan.xyz |
| Currency | MNT |

## Core Contracts

### Mock USDT (Testnet)

| Property | Value |
|----------|-------|
| **Address** | `0xe01c5464816a544d4d0d6a336032578bd4629F10` |
| **Type** | ERC-20 Token |
| **Purpose** | Stablecoin for investments |
| **Decimals** | 6 |

**Explorer**: [View on Mantlescan](https://sepolia.mantlescan.xyz/address/0xe01c5464816a544d4d0d6a336032578bd4629F10)

### Factory P2P

| Property | Value |
|----------|-------|
| **Address** | `0xa411df45e20d266500363c76ecbf0b8e483fd408` |
| **Type** | Factory Contract |
| **Purpose** | Deploys loan token contracts |

**Explorer**: [View on Mantlescan](https://sepolia.mantlescan.xyz/address/0xa411df45e20d266500363c76ecbf0b8e483fd408)

### Factory Manager

| Property | Value |
|----------|-------|
| **Address** | `0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235` |
| **Type** | Factory Contract |
| **Purpose** | Deploys fund manager contracts |

**Explorer**: [View on Mantlescan](https://sepolia.mantlescan.xyz/address/0x4d1a3d97109b2fb7e81023cf0a97aea4277d7235)

### Swap

| Property | Value |
|----------|-------|
| **Address** | `0x5fd6dfd2a3e4fafe82ee6e9d7a228f2c395ef05e` |
| **Type** | Swap Contract |
| **Purpose** | Token swap functionality |

**Explorer**: [View on Mantlescan](https://sepolia.mantlescan.xyz/address/0x5fd6dfd2a3e4fafe82ee6e9d7a228f2c395ef05e)

## Contract Verification

### Verifying Contracts

All contracts are verified on Mantlescan. To verify:

1. Go to the contract address on Mantlescan
2. Click "Contract" tab
3. View verified source code

### Checking Contract Code

```bash
# Using cast (Foundry)
cast code 0xa411df45e20d266500363c76ecbf0b8e483fd408 --rpc-url https://rpc.sepolia.mantle.xyz
```

## Adding to Wallet

### Import Mock USDT

To add Mock USDT to your wallet:

1. Open MetaMask
2. Click "Import tokens"
3. Enter contract address: `0xe01c5464816a544d4d0d6a336032578bd4629F10`
4. Token symbol: USDT
5. Decimals: 6
6. Click "Add Custom Token"

## Contract ABIs

### Where to Find ABIs

ABIs are available in:
- Frontend: `/src/lib/contracts/`
- Documentation: [Contract ABIs](../developers/web3-integration/contract-abis.md)

### Quick Reference

```typescript
// Import in TypeScript
import { factoryP2PAbi } from '@/lib/contracts/factoryP2P';
import { loanTokenAbi } from '@/lib/contracts/loanToken';
import { erc20Abi } from 'viem';
```

## Mainnet Contracts

*Mainnet contracts will be listed here after production deployment.*

| Contract | Address |
|----------|---------|
| USDT | TBD |
| Factory P2P | TBD |
| Factory Manager | TBD |

## Contract Upgrade Policy

### Immutable Contracts

Loan token contracts are immutable once deployed:
- Cannot be modified
- Ensures trust
- State preserved forever

### Factory Updates

Factory contracts may be upgraded:
- New versions deployed
- Old versions remain functional
- Migration procedures provided

## Related Pages

- [Contract Overview](overview.md)
- [Contract Architecture](architecture.md)
- [Network Configuration](../mantle/network-config.md)
